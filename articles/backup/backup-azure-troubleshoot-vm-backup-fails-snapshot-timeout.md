---
title: "Résoudre les échecs d’Azure Backup : La sous-tâche de capture de la machine virtuelle a expiré | Microsoft Docs"
description: "Symptômes, causes et résolutions d’échecs d’Azure Backup relatifs à l’erreur : Impossible de communiquer avec l’agent de machine virtuelle pour l’état de l’instantané. La sous-tâche de capture de la machine virtuelle a expiré"
services: backup
documentationcenter: 
author: genlin
manager: cshepard
editor: 
ms.assetid: 4b02ffa4-c48e-45f6-8363-73d536be4639
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: genli;markgal;
translationtype: Human Translation
ms.sourcegitcommit: 26ea5c6f867165a25dd5aecb01d0a0ce3b213a51
ms.openlocfilehash: 707d666eb6c23fb926c31711daddfb22979513bc

---

# <a name="troubleshoot-azure-backup-failure-snapshot-vm-sub-task-timed-out"></a>Résoudre les échecs d’Azure Backup : La sous-tâche de capture de la machine virtuelle a expiré
## <a name="summary"></a>Résumé
Après avoir enregistré et planifié une machine virtuelle pour le service Azure Backup , ce dernier lance le travail en communiquant avec l’extension de sauvegarde de la machine virtuelle pour prendre un instantané à un moment donné. Une des quatre conditions peut empêcher le déclenchement de l’instantané, qui à son tour risque de faire échouer la sauvegarde. Cet article détaille les étapes nécessaires au dépannage qui vont vous permettre de résoudre les problèmes liés aux échecs de sauvegarde relatifs à une erreur de délai d’expiration d’un instantané.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Symptôme
Azure Backup pour une machine virtuelle IaaS échoue en renvoyant le message d’erreur suivant dans les informations relatives à l’erreur du travail dans le [portail Azure](https://portal.azure.com/) : « Impossible de communiquer avec l’agent de machine virtuelle pour l’état de l’instantané. La sous-tâche de capture de la machine virtuelle a expiré. ».

## <a name="cause-1-the-vm-has-no-internet-access"></a>Cause 1 : la machine virtuelle ne possède aucun accès à Internet
Selon la configuration requise pour le déploiement, la machine virtuelle n’a pas accès à Internet, ou a des restrictions en place qui empêchent l’accès à l’infrastructure Azure.

Pour fonctionner correctement, l’extension de sauvegarde nécessite une connectivité vers les adresses IP publiques Azure. L’extension envoie des commandes vers un point de terminaison Azure Storage (HTTP URL) pour gérer les instantanés de la machine virtuelle. Si l’extension n’a pas accès à Internet public, la sauvegarde échoue.

### <a name="solution"></a>Solution
Pour résoudre le problème, essayez l’une des méthodes répertoriées ci-dessous :
#### <a name="allow-access-to-the-azure-datacenter-ip-ranges"></a>Autorisez l’accès aux plages IP du centre de données Azure.

1. Obtenez la [liste des adresses IP de centres de données Azure](https://www.microsoft.com/download/details.aspx?id=41653) pour y autoriser l’accès.
2. Débloquez les adresses IP en exécutant l’applet de commande **New-NetRoute** de la machine virtuelle Azure dans une fenêtre PowerShell avec élévation de privilèges. Exécutez l’applet de commande en tant qu’administrateur.
3. Ajoutez des règles au groupe de sécurité réseau (le cas échéant) pour autoriser l’accès aux adresses IP.

#### <a name="create-a-path-for-http-traffic-to-flow"></a>Créez un chemin d'accès pour le trafic HTTP

1. Si des restrictions réseau ont été mises en place (un groupe de sécurité réseau, par exemple), déployez un serveur proxy HTTP pour acheminer le trafic.
2. Ajoutez des règles au groupe de sécurité réseau (le cas échéant) pour autoriser l’accès à Internet à partir du serveur proxy HTTP.

Pour savoir comment configurer un proxy HTTP pour les sauvegardes de machines virtuelles, consultez [Préparer votre environnement pour la sauvegarde des machines virtuelles Azure](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Cause 2 : l’agent installé dans la machine virtuelle est obsolète (pour les machines virtuelles Linux)

### <a name="solution"></a>Solution
La plupart des échecs des machines virtuelles Linux liés aux agents ou aux extensions sont causés par des problèmes qui affectent un agent obsolète de machine virtuelle. Pour résoudre ce problème, suivez ces recommandations générales :

1. Suivez les instructions fournies pour la [mise à jour d’un agent de machine virtuelle Linux](../virtual-machines/virtual-machines-linux-update-agent.md).

 >[!NOTE]
 >Nous *recommandons vivement* la mise à jour de l’agent uniquement par le biais de référentiel de distribution. Nous ne recommandons pas de télécharger le code de l’agent à partir de GitHub directement et d’effectuer la mise à jour. Si le dernier agent n’est pas disponible pour la distribution, contactez le support de distribution pour obtenir des instructions sur l’installation de celui-ci. Pour rechercher l’agent le plus récent, accédez à la page relative à l’[agent Windows Azure Linux](https://github.com/Azure/WALinuxAgent/releases) du référentiel GitHub.

2. Assurez-vous que l’agent Azure s’exécute sur la machine virtuelle en exécutant la commande suivante : `ps -e`

 Si le processus ne s’exécute pas, redémarrez-le à l’aide des commandes suivantes :

 * Pour Ubuntu : `service walinuxagent start`
 * Pour les autres distributions : `service waagent start`

3. [Configurez l’agent de redémarrage automatique](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Exécutez une nouvelle sauvegarde de test. Si le problème persiste, veuillez collecter les journaux suivants à partir de la machine virtuelle du client :

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Si nous exigeons une journalisation détaillée pour waagent, procédez comme suit :

1. Dans le fichier /etc/waagent.conf, recherchez la ligne suivante : **Activer l’enregistrement des informations détaillées (o|n)**
2. Passez la valeur **Logs.Verbose** de *N* à *O*.
3. Enregistrez cette modification, puis redémarrez waagent en suivant les étapes précédentes de cette section.

## <a name="cause-3-the-backup-extension-fails-to-update-or-load"></a>Cause 3 : l’extension de sauvegarde ne peut être mise à jour ou chargée.
Si les extensions ne peuvent pas être chargées, la sauvegarde échoue en raison de l’impossibilité de capturer un instantané.

### <a name="solution"></a>Solution

Pour les invités Windows :

Vérifiez que le service iaasvmprovider est activé et qu’il a un type de démarrage *automatique*. Si ce n’est pas le cas, activez le service pour déterminer si la sauvegarde suivante réussit.

Pour les hôtes Linux :

La dernière version de VMSnapshot pour Linux (extension utilisée par la sauvegarde) est 1.0.91.0

Si l’extension de sauvegarde rencontre toujours un échec lors de la mise à jour ou du chargement, vous pouvez forcer le rechargement de l’extension VMSnapshot en désinstallant l’extension. La prochaine tentative de sauvegarde rechargera l’extension.

Pour désinstaller l’extension, procédez comme suit :

1. Accédez au [portail Azure](https://portal.azure.com/).
2. Localisez la machine virtuelle qui rencontre des problèmes de sauvegarde.
3. Cliquez sur **Paramètres**.
4. Cliquez sur **Extensions**.
5. Cliquez sur **Extension Vmsnapshot**.
6. Cliquer sur **Désinstaller**.  

Cette procédure réinstalle l’extension lors de la sauvegarde suivante.

## <a name="cause-4-the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>Cause 4 : Impossible de récupérer l’état de l’instantané ou de capturer un instantané
La sauvegarde de machine virtuelle émet une commande de capture instantanée à destination du stockage sous-jacent. La sauvegarde peut échouer, car elle n’a pas accès au compte de stockage ou parce que l’exécution de la tâche d’instantané est différée.

### <a name="solution"></a>Solution
Voici les causes possibles de l’échec d’une tâche de capture instantanée :

| Cause : | Solution |
| --- | --- |
| La sauvegarde SQL Server a été configurée que la machine virtuelle. | Par défaut, la sauvegarde de machine virtuelle exécute une sauvegarde complète VSS sur les machines virtuelles Windows. Sur les machines virtuelles exécutant des serveurs basés sur SQL Server et sur lesquelles la sauvegarde SQL Server est configurée, des retards d’exécution des captures instantanées peuvent survenir.<br><br>Définissez la clé de Registre suivante si vous rencontrez des échecs de sauvegarde en raison de problèmes de capture instantanée.<br><br>**[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE"** |
| L’état de la machine virtuelle est rapporté de manière incorrecte en raison de l’arrêt de la machine virtuelle dans RDP. | Si vous avez arrêté la machine virtuelle dans Remote Desktop Protocol (RDP), retournez sur le portail pour vérifier que l’état de la machine virtuelle est correct. Si ce n’est pas le cas, arrêtez la machine virtuelle dans le portail à l’aide de l’option **Arrêter** dans le tableau de bord de la machine virtuelle. |
| Plusieurs machines virtuelles du même service cloud sont configurées pour exécuter la sauvegarde en même temps. | Il est recommandé d’étaler les planifications de sauvegarde entre les différentes machines virtuelles d’un même service cloud. |
| Forte sollicitation du processeur ou de la mémoire sur la machine virtuelle. | Si la machine virtuelle sollicite fortement le processeur (plus de 90 pour cent) ou la mémoire, la tâche de capture instantanée est mise en file d’attente et retardée, puis finit par expirer. En pareille situation, essayez de procéder à une sauvegarde à la demande. |
| La machine virtuelle ne peut pas obtenir l’adresse d’hôte/de structure du protocole DHCP. | Le protocole DHCP doit être activé dans l’invité pour que la sauvegarde de la machine virtuelle IaaS fonctionne.  Si la machine virtuelle ne peut pas bénéficier de l’adresse d’hôte/de structure de la réponse DHCP 245, elle ne peut ni télécharger, ni exécuter d’extension. Si vous avez besoin d’une adresse IP privée statique, vous devez la configurer via la plateforme. L’option DHCP à l’intérieur de la machine virtuelle doit être laissée désactivée. Pour en savoir plus, consultez la [définition d’une adresse IP privée interne statique](../virtual-network/virtual-networks-reserved-private-ip.md). |



<!--HONumber=Jan17_HO4-->


