---
title: "FAQ de l’Azure Security Center | Microsoft Docs"
description: "Ce forum aux questions concerne le Centre de sécurité Azure."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/09/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: 0135732e95279f2e717334d3dd39902b56b0aa90
ms.openlocfilehash: 30a327f59b8149f41c3b5206e0b0c2fc859934a0


---
# <a name="azure-security-center-frequently-asked-questions-faq"></a>FAQ du Centre de sécurité Azure
Cette FAQ répond aux questions concernant Azure Security Center, qui vous aide à prévenir, détecter et résoudre les menaces grâce à une meilleure visibilité et à un meilleur contrôle de la sécurité de vos ressources Microsoft Azure.

## <a name="general-questions"></a>Questions générales
### <a name="what-is-azure-security-center"></a>Qu’est-ce que le Centre de sécurité Azure ?
Le Centre de sécurité Azure vous aide à prévenir, détecter et résoudre les menaces grâce à une visibilité et un contrôle accrus de la sécurité de vos ressources Azure. Il fournit une surveillance de la sécurité et une gestion des stratégies intégrées pour l’ensemble de vos abonnements, vous aidant ainsi à détecter les menaces qui pourraient passer inaperçues. De plus, il est compatible avec un vaste écosystème de solutions de sécurité.

### <a name="how-do-i-get-azure-security-center"></a>Comment obtenir le Centre de sécurité Azure ?
Azure Security Center est disponible avec votre abonnement Microsoft Azure et accessible à partir du [Portail Azure](https://azure.microsoft.com/features/azure-portal/). ([Connectez-vous au portail](https://portal.azure.com), sélectionnez **Parcourir**, puis faites défiler jusqu’à **Centre de sécurité**).  

## <a name="billing"></a>Facturation
### <a name="how-does-billing-work-for-azure-security-center"></a>Comment fonctionne la facturation pour le Centre de sécurité Azure ?
Security Center est proposé en deux niveaux : Gratuit et Standard.

Le niveau Gratuit vous permet de définir des stratégies de sécurité et de recevoir des alertes de sécurité, des incidents et des recommandations qui vous guident tout au long du processus de configuration des contrôles nécessaires. Avec le niveau Gratuit, vous pouvez également surveiller l’état de la sécurité de vos ressources Azure et des solutions de partenaires intégrées à votre abonnement Azure.

Le niveau Standard fournit les fonctionnalités du niveau Gratuit, ainsi que des fonctions de détection plus avancées : informations sur les menaces, analyse comportementale, analyse des incidents et détection des anomalies. Une évaluation gratuite de 90 jours du niveau Standard est disponible. Pour mettre à niveau, sélectionnez Niveau tarifaire sous [Stratégie de sécurité](security-center-policies.md#set-security-policies-for-subscriptions). Pour en savoir plus, consultez la page [Tarification de Security Center](security-center-pricing.md).

## <a name="permissions"></a>Autorisations
Azure Security Center utilise le [contrôle d’accès en fonction du rôle (RBAC)](../active-directory/role-based-access-control-configure.md) qui fournit des [rôles intégrés](../active-directory/role-based-access-built-in-roles.md) susceptibles d’être affectés à des utilisateurs, des groupes et des services dans Azure.

Security Center évalue la configuration de vos ressources pour identifier les vulnérabilités et les problèmes de sécurité. Dans Security Center, vous ne voyez les informations relatives à une ressource que lorsque vous avez reçu le rôle de propriétaire, de collaborateur ou de lecteur pour l’abonnement ou le groupe de ressources auquel appartient la ressource.

Pour plus d’informations sur les rôles et les actions autorisées dans Security Center, consultez l’article [Permissions in Azure Security Center (Autorisations dans Azure Security Center)](security-center-permissions.md).

## <a name="data-collection"></a>Collecte des données
Security Center collecte les données de vos machines virtuelles afin d’évaluer l’état de leur sécurité, de fournir des recommandations en matière de sécurité et de vous avertir des menaces. Lorsque vous accédez au Centre de sécurité pour la première fois, la collecte de données est activée sur toutes les machines virtuelles de votre abonnement. La collecte des données est recommandée, mais vous pouvez refuser cette fonctionnalité en la [désactivant](#how-do-i-disable-data-collection) dans la stratégie de Security Center.

### <a name="how-do-i-disable-data-collection"></a>Comment désactiver la collecte des données ?
Vous pouvez désactiver la **collecte des données** pour un abonnement dans la stratégie de sécurité à tout moment. ([Connectez-vous au Portail Azure](https://portal.azure.com), sélectionnez **Parcourir**, **Centre de sécurité**, puis **Stratégie**.)  Quand vous sélectionnez un abonnement, un nouveau panneau s’ouvre et affiche une option permettant de désactiver la **collecte des données**. Pour supprimer des agents des machines virtuelles existantes, sélectionnez l’option **Supprimer des agents** dans le ruban supérieur.

> [!NOTE]
> Vous pouvez définir les stratégies de sécurité au niveau du groupe de ressources et de l’abonnement Azure, mais vous devez sélectionner un abonnement pour désactiver la collecte des données.
>
>

### <a name="how-do-i-enable-data-collection"></a>Comment activer la collecte des données ?
Vous pouvez activer la collecte des données pour votre abonnement Azure dans la stratégie de sécurité. Pour activer la collecte des données, [connectez-vous au Portail Azure](https://portal.azure.com), sélectionnez **Parcourir**, **Centre de sécurité**, puis **Stratégie**. Définissez **Collecte des données** sur **Activé**, puis configurez les comptes de stockage dans lesquels vous voulez stocker les données collectées (voir la question « [Où sont stockées mes données ? ](#where-is-my-data-stored)»). Quand **Collecte des données** est activé, les informations liées à la configuration et aux événements de sécurité sont automatiquement collectées sur l’ensemble des machines virtuelles prises en charge par l’abonnement.

> [!NOTE]
> Bien que vous puissiez définir les stratégies de sécurité au niveau du groupe de ressources et de l’abonnement Azure, la configuration de la collecte des données intervient uniquement au niveau de l’abonnement.
>
>

### <a name="what-happens-when-data-collection-is-enabled"></a>Que se passe-t-il quand la collecte des données est activée ?
La collecte des données peut être activée via l’agent de surveillance Azure et via l’extension Surveillance de la sécurité Azure. L’extension Surveillance de la sécurité Azure analyse différentes configurations de sécurité et les envoie sous forme de traces de [Suivi d’événements pour Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW). En outre, le système d’exploitation crée des entrées de journal des événements.  L’agent de surveillance Azure lit les entrées du journal des événements et les traces ETW, puis les copie dans votre compte de stockage pour les analyser.  Le compte de stockage en question est celui que vous avez configuré dans la stratégie de sécurité. Pour plus d’informations sur les comptes de stockage, reportez-vous à la question «[Où sont stockées mes données ?](#where-is-my-data-stored)».

### <a name="does-the-monitoring-agent-or-security-monitoring-extension-impact-the-performance-of-my-servers"></a>L’agent de surveillance et l’extension Surveillance de la sécurité ont-ils un impact sur les performances de mon serveur ?
L’agent et l’extension utilisent une quantité minime de ressources système et n’ont donc qu’un faible impact sur les performances. Pour en savoir plus sur l’impact sur les performances, l’agent et l’extension, consultez le [Guide de planification et de fonctionnement](security-center-planning-and-operations-guide.md#data-collection-and-storage).

### <a name="where-is-my-data-stored"></a>Où sont stockées mes données ?
Pour chaque région où s’exécutent des machines virtuelles, vous devez choisir le compte de stockage où doivent être stockées les données collectées à partir de ces machines virtuelles. Ainsi, vous pouvez stocker les données dans une même région pour garantir la confidentialité et la souveraineté des données. Vous devez sélectionner le compte de stockage d’un abonnement dans la stratégie de sécurité. ([Connectez-vous au Portail Azure](https://portal.azure.com), sélectionnez **Parcourir**, **Centre de sécurité**, puis **Stratégie**.) Quand vous sélectionnez un abonnement, un nouveau panneau s’ouvre. Pour sélectionner une région, sélectionnez **Choisir des comptes de stockage**.

> [!NOTE]
> Bien que vous puissiez définir les stratégies de sécurité au niveau du groupe de ressources et de l’abonnement Azure, la sélection d’une région pour votre compte de stockage intervient uniquement au niveau de l’abonnement.
>
>

Pour plus d’informations sur Azure Storage et les comptes de stockage, consultez la [Documentation Azure Storage](https://azure.microsoft.com/documentation/services/storage/) et [À propos des comptes de stockage Azure](../storage/storage-create-storage-account.md).

## <a name="using-azure-security-center"></a>Utilisation du Centre de sécurité Azure
### <a name="what-is-a-security-policy"></a>Qu’est-ce qu’une stratégie de sécurité ?
Une stratégie de sécurité définit l’ensemble des contrôles recommandés pour les ressources d’un abonnement ou groupe de ressources spécifique. Dans Azure Security Center, vous définissez les stratégies de vos abonnements et groupes de ressources Azure en fonction des exigences de sécurité de votre société et du type d’applications ou du niveau de confidentialité des données de chaque abonnement.

Par exemple, les ressources utilisées pour le développement ou le test peuvent avoir des exigences de sécurité différentes de celles utilisées pour les applications de production. De même, les applications dont les données sont réglementées (par exemple, les informations d’identification personnelle (PII)) peuvent nécessiter un niveau de sécurité plus élevé. Les stratégies de sécurité activées dans Azure Security Center déterminent les recommandations et la surveillance liées à la sécurité. Pour plus d’informations sur les stratégies de sécurité, consultez [Surveillance de l’intégrité de la sécurité dans le Centre de sécurité Azure](security-center-monitoring.md).

> [!NOTE]
> Si un conflit se produit entre la stratégie de niveau abonnement et la stratégie de niveau groupe de ressources, la stratégie de niveau groupe de ressources est prioritaire.
>
>

### <a name="who-can-modify-a-security-policy"></a>Qui peut modifier une stratégie de sécurité ?
Vous pouvez configurer des stratégies de sécurité pour chaque abonnement ou groupe de ressources. Pour modifier une stratégie de sécurité au niveau de l’abonnement ou du groupe de ressources, vous devez avoir le rôle de propriétaire ou de collaborateur pour l’abonnement concerné.

Pour savoir comment configurer une stratégie de sécurité, consultez [Définition de stratégies de sécurité dans le Centre de sécurité Azure](security-center-policies.md).

### <a name="what-is-a-security-recommendation"></a>Qu’est-ce qu’une recommandation de sécurité ?
Le Centre de sécurité Azure analyse l’état de sécurité de vos ressources Azure. Quand des failles de sécurité potentielles sont identifiées, des recommandations sont créées. Les recommandations vous guident tout au long du processus de configuration du contrôle. Voici quelques exemples :

* Approvisionnement d’un logiciel anti-programme malveillant pour identifier et supprimer les programmes malveillants
* Configuration de [groupes de sécurité réseau](../virtual-network/virtual-networks-nsg.md) et de règles pour contrôler le trafic vers les machines virtuelles
* Approvisionnement d’un pare-feu d’applications web pour se défendre des attaques ciblant vos applications web
* Déploiement de mises à jour système manquantes
* Correction des configurations de système d’exploitation qui ne suivent pas les recommandations

Seules les recommandations qui sont activées dans les stratégies de sécurité sont affichées ici.

### <a name="how-can-i-see-the-current-security-state-of-my-azure-resources"></a>Comment connaître l’état de sécurité actuel de mes ressources Azure ?
La mosaïque **Intégrité des ressources** du panneau **Centre de sécurité** affiche la posture de sécurité globale de votre environnement, avec le détail de vos machines virtuelles, applications web et autres ressources. Chaque ressource possède un indicateur montrant la présence éventuelle de failles de sécurité. En cliquant sur la mosaïque Intégrité des ressources, vous affichez vos ressources et les problèmes éventuels les concernant.

### <a name="what-triggers-a-security-alert"></a>Qu’est-ce qui déclenche une alerte de sécurité ?
Azure Security Center collecte, analyse et fusionne automatiquement les données du journal à partir de vos ressources Azure, du réseau et des solutions partenaires telles que les logiciels anti-programme malveillant et les pare-feu. Quand des menaces sont détectées, une alerte de sécurité est créée. Voici quelques exemples de détections :

* Des machines virtuelles compromises qui communiquent avec des adresses IP connues comme étant malveillantes
* Des programmes malveillants avancés qui sont détectés à l’aide du rapport d’erreurs Windows
* Des attaques par force brute contre des machines virtuelles
* Des alertes de sécurité émises par des solutions de sécurité partenaires intégrées, telles que des logiciels anti-programme malveillant ou des pare-feu d’applications web

### <a name="whats-the-difference-between-threats-detected-and-alerted-on-by-microsoft-security-response-center-versus-azure-security-center"></a>Quelle est la différence entre les menaces détectées et faisant l’objet d’une alerte par Microsoft Security Response Center et par Azure Security Center ?
Microsoft Security Response Center (MSRC) effectue certaines analyses de sécurité sur l'infrastructure et le réseau Azure et reçoit des informations sur les menaces et des plaintes pour mauvaise utilisation provenant de tiers. Lorsque MSRC constate que les données client ont été utilisées par un tiers illégal ou non autorisé ou que l'utilisation d’Azure par le client ne respecte pas les conditions de bon usage, un gestionnaire des incidents de sécurité informe le client. La notification correspond généralement à un courrier électronique envoyé aux contacts de sécurité spécifiés dans Azure Security Center ou au propriétaire de l’abonnement Azure si aucun contact de sécurité n’est spécifié.

Security Center est un service Azure qui surveille en continu l'environnement Azure du client et applique des analyses de façon à détecter automatiquement un large éventail d'activités potentiellement malveillantes. Ces détections sont signalées en tant qu'alertes de sécurité dans le tableau de bord du Security Center.

### <a name="which-azure-resources-are-monitored-by-azure-security-center"></a>Quelles ressources Azure sont surveillées par Azure Security Center ?
Azure Security Center surveille les ressources Azure suivantes :

* Machines virtuelles (VM) (y compris [Cloud Services](../cloud-services/cloud-services-choose-me.md))
* Réseaux virtuels Azure
* Service SQL Azure
* Solutions de partenaires intégrées à votre abonnement Azure, par exemple pare-feu d’applications web sur les machines virtuelles et sur [App Service Environment](../app-service/app-service-app-service-environments-readme.md)

## <a name="virtual-machines"></a>Machines virtuelles
### <a name="what-types-of-virtual-machines-are-supported"></a>Quels sont les types de machines virtuelles pris en charge ?
La surveillance de l’intégrité de la sécurité et les recommandations sont disponibles pour les machines virtuelles créées à l’aide des [modèles de déploiement Classic et Resource Manager](../azure-classic-rm.md).

Les machines virtuelles Windows prises en charge sont les suivantes :

* Windows Server 2008 R2
* Windows Server 2012
* Windows Server 2012 R2

Les machines virtuelles Linux prises en charge sont les suivantes :

* Ubuntu versions 12.04, 14.04, 16.04, 16.10
* Debian versions 7 et 8
* CentOS versions 6.\* et 7.*
* Red Hat Enterprise Linux (RHEL) versions 6.\* et 7.*
* SUSE Linux Enterprise Server (SLES) versions 11.\* et 12.*
* Oracle Linux versions 6.\*, 7.*

Les machines virtuelles en cours d’exécution dans un service cloud sont également prises en charge. Seuls les rôles de travail et web des services cloud en cours d’exécution dans des emplacements de production sont surveillés. Pour en savoir plus sur le service cloud, consultez [Vue d’ensemble de Cloud Services](../cloud-services/cloud-services-choose-me.md).

### <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Pourquoi Azure Security Center ne reconnaît-il pas la solution anti-programme malveillant de ma machine virtuelle Azure ?
Azure Security Center n’a de visibilité que sur les logiciels anti-programme malveillant installés par le biais des extensions Azure. Par exemple, Security Center n’est pas en mesure de détecter les logiciels anti-programme malveillant préinstallés sur une image que vous avez fournie ou installés sur vos machines virtuelles à l’aide de votre propre processus (notamment les systèmes de gestion de la configuration).

### <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Pourquoi reçois-je le message « Données d’analyse manquantes » pour ma machine virtuelle ?
Le remplissage des données d’analyse peut prendre un certain temps (inférieur à une heure) une fois la collecte de données activée dans Azure Security Center. Le remplissage des analyses n’est pas effectué pour les machines virtuelles à l’état Arrêté.

### <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Pourquoi reçois-je le message « Agent de machine virtuelle manquant » ?
L’agent de machine virtuelle doit être installé sur les machines virtuelles pour activer la collecte des données. L’agent de machine virtuelle est installé par défaut sur les machines virtuelles déployées depuis Azure Marketplace. Pour plus d’informations sur l’installation de l’agent de machine virtuelle sur d’autres machines virtuelles, consultez l’article de blog [Agent de machine virtuelle et extensions](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).



<!--HONumber=Dec16_HO2-->


