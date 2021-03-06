---
title: "Options de résolution de noms DNS pour les machines virtuelles Linux dans Azure"
description: "Scénarios concernant la résolution de noms pour les machines virtuelles Linux dans Azure IaaS, y compris les services DNS fournis, le DNS externe hybride et l’apport de son propre serveur DNS."
services: virtual-machines
documentationcenter: na
author: RicksterCDN
manager: timlt
editor: tysonn
ms.assetid: 787a1e04-cebf-4122-a1b4-1fcf0a2bbf5f
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/19/2016
ms.author: rclaus
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3c4bd01781b256542650add05faf5ec508be61ef


---
# <a name="dns-name-resolution-options-for-linux-vms-in-azure"></a>Options de résolution de noms DNS pour les machines virtuelles Linux dans Azure
Azure fournit la résolution de noms DNS par défaut pour toutes les machines virtuelles d’un même réseau virtuel. Vous pouvez implémenter votre propre solution de résolution de noms DNS en configurant vos propres services DNS sur vos machines virtuelles hébergées sur Azure. Les scénarios suivants vous aideront à choisir la solution qui convient le mieux à votre situation.

* [Résolution de noms dans Azure](#azure-provided-name-resolution)
* [Résolution de noms à l’aide de votre propre serveur DNS](#name-resolution-using-your-own-dns-server) 

Le type de résolution de noms que vous utilisez dépend de la manière dont vos machines virtuelles et vos instances de rôle doivent communiquer entre elles.

**Le tableau suivant illustre des scénarios et des solutions correspondantes de résolution de noms :**

| **Scénario** | **Solution** | **Suffixe** |
| --- | --- | --- |
| Résolution de noms entre des instances de rôle ou des machines virtuelles situées dans le même réseau virtuel |[Résolution de noms dans Azure](#azure-provided-name-resolution) |Nom d’hôte ou nom de domaine complet |
| Résolution de noms entre des instances de rôles ou des machines virtuelles situées dans différents réseaux virtuels |Serveurs DNS gérés par le client qui redirigent les requêtes entre les réseaux virtuels en vue de la résolution par Azure (proxy DNS).  Consultez [Résolution de noms à l’aide de votre propre serveur DNS](#name-resolution-using-your-own-dns-server) |Nom de domaine complet uniquement |
| Résolution des noms de service et d’ordinateur locaux à partir des instances de rôle ou des machines virtuelles dans Azure |Serveurs DNS gérés par le client (par exemple, contrôleur de domaine local, contrôleur de domaine en lecture seule local ou serveur DNS secondaire synchronisé à l’aide de transferts de zone).  See [Résolution de noms à l’aide de votre propre serveur DNS](#name-resolution-using-your-own-dns-server) |Nom de domaine complet uniquement |
| Résolution de noms d’hôtes Azure à partir d’ordinateurs locaux |Rediriger les requêtes vers un serveur proxy DNS géré par le client dans le réseau virtuel correspondant ; le serveur proxy redirige les requêtes vers Azure en vue de la résolution. See [Résolution de noms à l’aide de votre propre serveur DNS](#name-resolution-using-your-own-dns-server) |Nom de domaine complet uniquement |
| DNS inversé pour les adresses IP internes |[Résolution de noms à l’aide de votre propre serveur DNS](#name-resolution-using-your-own-dns-server) |n/a |

## <a name="azure-provided-name-resolution"></a>Résolution de noms dans Azure
Avec la résolution de noms DNS publics, Azure fournit la résolution de noms interne pour les machines virtuelles et les instances de rôle qui résident dans le même réseau virtuel.  Dans les réseaux virtuels ARM, le suffixe DNS est cohérent sur le réseau virtuel (le nom de domaine complet n’est donc pas nécessaire) et les noms DNS peuvent être affectés à la fois aux cartes réseau et aux machines virtuelles. Bien que la résolution de noms fournie par Azure ne nécessite aucune configuration, elle ne convient pas à l’ensemble des scénarios de déploiement, comme illustré dans le tableau précédent.

### <a name="features-and-considerations"></a>Fonctionnalités et considérations
**Fonctionnalités :**

* Simplicité d’utilisation : aucune configuration n’est requise pour utiliser la résolution de noms dans Azure.
* Le service de résolution de noms dans Azure est hautement disponible, vous évitant d’avoir à créer et à gérer des clusters de vos propres serveurs DNS.
* Peut être utilisé avec vos propres serveurs DNS pour résoudre les noms d’hôte locaux et Azure.
* Une résolution de noms est effectuée entre les machines virtuelles des réseaux virtuels sans qu’un nom de domaine complet soit nécessaire. 
* Vous pouvez utiliser des noms d’hôtes qui décrivent vos déploiements de manière plus appropriée, plutôt que des noms générés automatiquement.

**Considérations :**

* Le suffixe DNS créé par Azure ne peut pas être modifié.
* Vous ne pouvez pas enregistrer manuellement vos propres enregistrements.
* WINS et NetBIOS ne sont pas pris en charge.
* Les noms d’hôte doivent être compatibles DNS (ils doivent comporter uniquement les caractères 0-9, a-z et « - » et ne peuvent pas démarrer ou se terminer par un « - ». Voir la section 2 de RFC 3696.)
* Le trafic de requêtes DNS est limité pour chaque machine virtuelle. Cela ne devrait pas avoir d’incidence sur la plupart des applications.  Si la limitation de requêtes est respectée, assurez-vous que la mise en cache côté client est activée.  Pour plus d’informations, consultez [Tirer le meilleur parti de la résolution de noms dans Azure](#Getting-the-most-from-Azure-provided-name-resolution).

### <a name="getting-the-most-from-azure-provided-name-resolution"></a>Tirer le meilleur parti de la résolution de noms dans Azure
**Mise en cache côté client :**

Les requêtes DNS ne sont pas toutes envoyées sur le réseau.  La mise en cache côté client permet de réduire la latence et d’améliorer la résilience aux spots réseau en résolvant les requêtes DNS récurrentes à partir d’un cache local.  Les enregistrements DNS ont une durée de vie qui permet au cache de les stocker aussi longtemps que possible sans affecter leur fraîcheur.  Pour cette raison, la mise en cache côté client convient donc à la plupart des situations.

Certaines distributions Linux n’incluent pas la mise en cache par défaut.  Il est recommandé d’en ajouter une à chaque machine virtuelle Linux (après avoir vérifié qu’il n’existe pas déjà un cache local).

Différents packages de mise en cache DNS sont disponibles, par exemple, dnsmasq. Voici les étapes pour installer dnsmasq sur les distributions les plus courantes :

* **Ubuntu (utilise resolvconf)**:
  * installez le package dnsmasq (« sudo apt-get install dnsmasq »).
* **SUSE (utilise netconf)**:
  * installez le package dnsmasq (« sudo zypper install dnsmasq ») 
  * activez le service dnsmasq (« systemctl enable dnsmasq.service ») 
  * démarrez le service dnsmasq (« systemctl start dnsmasq.service ») 
  * modifiez « /etc/sysconfig/network/config » et remplacez NETCONFIG_DNS_FORWARDER="" par « dnsmasq »
  * mettez à jour resolv.conf (netconfig update) pour définir le cache en tant que programme de résolution DNS local
* **OpenLogic (utilise NetworkManager)**:
  * installez le package dnsmasq (« sudo yum install dnsmasq »)
  * activez le service dnsmasq (« systemctl enable dnsmasq.service »)
  * démarrez le service dnsmasq (« systemctl start dnsmasq.service »)
  * ajoutez « prepend domain-name-servers 127.0.0.1; » à « /etc/dhclient-eth0.conf »
  * redémarrez le service réseau (« service network restart ») pour définir le cache en tant que programme de résolution DNS local

> [!NOTE]
>  : le package 'dnsmasq' constitue l’un des nombreux caches DNS disponibles pour Linux.  Avant de l’utiliser, vérifiez son adéquation à vos besoins et assurez-vous qu’aucun autre cache n’est installé.
> 
> 

**Nouvelles tentatives côté client :**

DNS est principalement un protocole UDP.  Comme le protocole UDP ne garantit pas la remise des messages, la logique de nouvelle tentative est gérée dans le protocole DNS lui-même.  Chaque client DNS (système d’exploitation) peut afficher une logique de nouvelle tentative différente selon la préférence de son auteur :

* Les systèmes d’exploitation Windows effectuent de nouvelles tentatives après une seconde, puis à nouveau après deux secondes, quatre secondes et encore quatre secondes. 
* La configuration Linux par défaut effectue une nouvelle tentative après cinq secondes.  Vous devez modifier ce paramètre pour effectuer cinq nouvelles tentatives à des intervalles d’une seconde.  

Pour vérifier les paramètres actuels sur une machine virtuelle Linux, accédez à « cat /etc/resolv.conf » et consultez la ligne « options », par exemple :

    options timeout:1 attempts:5

Le fichier resolv.conf est généré automatiquement et ne doit pas être modifié.  Les étapes spécifiques pour l’ajout de la ligne 'options' varient selon la distribution :

* **Ubuntu** (utilise resolvconf) :
  * ajoutez la ligne 'options' à '/etc/resolveconf/resolv.conf.d/head' 
  * exécutez 'resolvconf -u' pour mettre à jour
* **SUSE** (utilise netconf) :
  * ajoutez 'timeout:1 attempts:5' au paramètre NETCONFIG_DNS_RESOLVER_OPTIONS="" dans '/etc/sysconfig/network/config' 
  * exécutez 'netconfig update' pour mettre à jour
* **OpenLogic** (utilise NetworkManager) :
  * ajoutez 'echo "options timeout:1 attempts:5"' à '/etc/NetworkManager/dispatcher.d/11-dhclient' 
  * exécutez 'service network restart' pour mettre à jour

## <a name="name-resolution-using-your-own-dns-server"></a>Résolution de noms à l’aide de votre propre serveur DNS
Dans certaines situations, les fonctionnalités fournies par Azure peuvent ne pas couvrir vos besoins en matière de résolution de noms, par exemple si vous devez recourir à une résolution DNS entre des réseaux virtuels.  Dans le cadre de ce scénario, Azure vous permet d’utiliser vos propres serveurs DNS.  

Les serveurs DNS dans un réseau virtuel peuvent rediriger les requêtes DNS vers les programmes de résolution récursifs d’Azure en vue de la résolution des noms d’hôtes au sein de ce réseau virtuel.  Par exemple, un serveur DNS en cours d’exécution dans Azure peut répondre aux requêtes DNS concernant ses propres fichiers de zone DNS et rediriger toutes les autres requêtes vers Azure.  Ainsi, les machines virtuelles peuvent voir vos entrées dans vos fichiers de zone et les noms d’hôtes fournis par Azure (par le biais du redirecteur).  Les programmes de résolution récursifs d’Azure sont accessibles via l’adresse IP virtuelle 168.63.129.16.

En outre, grâce à la redirection DNS, la résolution DNS entre réseaux virtuels est possible, et vos ordinateurs locaux peuvent résoudre les noms d’hôtes fournis par Azure.  Pour résoudre le nom d’hôte d’une machine virtuelle, la machine virtuelle du serveur DNS doit résider dans le même réseau virtuel et être configurée pour rediriger les requêtes de nom d’hôte vers Azure.  Comme le suffixe DNS est différent dans chaque réseau virtuel, vous pouvez utiliser des règles de redirection conditionnelles pour envoyer les requêtes DNS au réseau virtuel approprié en vue de la résolution.  L’illustration suivante montre deux réseaux virtuels et un réseau local effectuant une résolution DNS entre réseaux virtuels à l’aide de cette méthode :

![DNS entre réseaux virtuels](./media/virtual-machines-linux-azure-dns/inter-vnet-dns.png)

Quand vous utilisez la résolution de noms dans Azure, le suffixe DNS interne est fourni à chaque machine virtuelle à l’aide de DHCP.  Quand vous utilisez votre propre solution de résolution de noms, ce suffixe n’est pas fourni aux machines virtuelles, car il interfère avec d’autres architectures DNS.  Pour faire référence à des ordinateurs par nom de domaine complet, ou pour configurer le suffixe sur vos machines virtuelles, vous pouvez déterminer celui-ci à l’aide de PowerShell ou de l’API :

* Pour les réseaux virtuels gérés dans Azure Resource Management, le suffixe est disponible par le biais de la ressource de [carte d’interface réseau](https://msdn.microsoft.com/library/azure/mt163668.aspx). Vous pouvez également exécuter la commande `azure network public-ip show <resource group> <pip name>` pour afficher les détails de votre adresse IP publique, y compris le nom de domaine complet de la carte réseau.    

Si la redirection des requêtes vers Azure ne suffit pas, vous devez fournir votre propre solution DNS.  Votre solution DNS doit :

* Fournir la résolution de nom d’hôte approprié, par exemple par le biais de [DDNS](../virtual-network/virtual-networks-name-resolution-ddns.md).  Si vous utilisez DNS dynamique, vous pouvez être amené à désactiver le nettoyage des enregistrements DNS, car les baux DHCP d’Azure sont très longs et le nettoyage risque de supprimer des enregistrements DNS prématurément. 
* Fournir la résolution récursive appropriée pour permettre la résolution de noms de domaines externes.
* Être accessible (TCP et UDP sur le port 53) à partir des clients qu’elle dessert et être en mesure d’accéder à internet.
* Être protégée contre tout accès à partir d’Internet, pour atténuer les menaces posées par les agents externes.

> [!NOTE]
> Pour de meilleures performances, lors de l’utilisation de machines virtuelles Azure en tant que serveurs DNS, le protocole IPv6 doit être désactivé et une [adresse IP publique de niveau d’instance](../virtual-network/virtual-networks-instance-level-public-ip.md) doit être affectée à chaque machine virtuelle du serveur DNS.  
> 
> 




<!--HONumber=Nov16_HO3-->


