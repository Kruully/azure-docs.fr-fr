---
title: "Vue d’ensemble des disques gérés Azure Premium et Standard"
description: "Vue d’ensemble d’Azure Managed Disks, qui prend en charge les comptes de stockage lorsque vous utilisez des machines virtuelles Azure"
services: storage
documentationcenter: na
author: ramankumarlive
manager: tadb
editor: tysonn
ms.assetid: 272250b3-fd4e-41d2-8e34-fd8cc341ec87
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: ramankum
translationtype: Human Translation
ms.sourcegitcommit: 58c395a45115c9db0027cffe96d20863c928a63d
ms.openlocfilehash: 74ec73388af06dbf5682c5aa1c84b153dfc4a744


---

# <a name="azure-managed-disks-overview"></a>Vue d’ensemble d’Azure Managed Disks

Azure Managed Disks simplifie la gestion des disques des machines virtuelles Azure IaaS, en gérant les [comptes de stockage](storage-introduction.md) associés aux disques de machines virtuelles. Vous spécifiez simplement le type ([Premium](storage-premium-storage.md) ou [Standard](storage-standard-storage.md)) et la taille du disque dont vous avez besoin, et Azure crée et gère le disque pour vous.

>[!NOTE]
> Managed Disks nécessite la disponibilité du port 8443. Si vous voulez bloquer ce port, il vous faut alors utiliser des disques non gérés.
>

## <a name="benefits-of-managed-disks"></a>Avantages des disques gérés

Examinons quelques-uns des avantages procurés par l’utilisation des disques gérés.

### <a name="simple-and-scalable-vm-deployment"></a>Déploiement de machines virtuelles simple et évolutif

Les disques gérés gèrent le stockage pour vous en arrière-plan. Auparavant, vous deviez créer des comptes de stockage dédiés à la prise en charge des disques (fichiers de disques durs virtuels) de vos machines virtuelles Azure. Lors d’une montée en puissance, il était nécessaire de vous assurer d’avoir créé la quantité suffisante de comptes de stockage supplémentaires, ceci pour ne pas dépasser la limite d’E/S associée au stockage de vos disques. Maintenant que votre stockage est géré par Managed Disks, vous n’êtes plus restreint par les limites des comptes de stockage (comme 20 000 E/S par seconde et par compte). Vous n’avez plus à copier vos images personnalisées (fichiers de disques durs virtuels) sur plusieurs comptes de stockage. Vous pouvez les gérer à partir d’un emplacement centralisé (un compte de stockage par région Azure) et les utiliser pour créer des centaines de machines virtuelles dans un abonnement.

Managed Disks vous permet de créer jusqu’à 10 000 **disques** de machines virtuelles dans un abonnement. Ainsi, vous êtes en mesure de mettre en œuvre des milliers de **machines virtuelles** dans un seul abonnement. Par ailleurs, cette fonctionnalité optimise l’évolutivité des [groupes de machines virtuelles identiques](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) (VMSS, Virtual Machine Scale Sets), en vous donnant les moyens de créer jusqu’à&1; millier de machines virtuelles dans une instance VMSS à l’aide d’une image de Marketplace.

### <a name="better-reliability-for-availability-sets"></a>Fiabilité supérieure des groupes à haute disponibilité

Managed Disks accroît la fiabilité des groupes à haute disponibilité en garantissant que les disques des machines virtuelles d’un groupe sont suffisamment isolés l’un de l’autre, ceci pour éviter les points de défaillance uniques. Comment le service procède-t-il ? Il place automatiquement les disques dans différentes unités d’échelle de stockage (horodatages). Si un horodatage est mis en échec en raison d’une défaillance matérielle ou logicielle, seules les instances de machine virtuelle possédant des disques sur ces horodatages sont mises en échec. Par exemple, supposons qu’une de vos applications est exécutée sur&5; machines virtuelles, qui sont hébergées dans un groupe à haute disponibilité. Les disques de ces machines virtuelles ne seront pas stockés dans le même horodatage. Par conséquent, si un horodatage est mis en échec, les autres instances de l’application continuent de s’exécuter.

### <a name="better-security"></a>Sécurité optimisée

Utilisez le [contrôle d’accès en fonction du rôle Azure](../active-directory/role-based-access-control-what-is.md) afin d’affecter à un ou plusieurs utilisateurs des autorisations spécifiques d’accès à un disque géré. Managed Disks expose différentes opérations, notamment la lecture, l’écriture (création/mise à jour), la suppression, l’exportation et la récupération d’un [URI de signature d’accès partagé](storage-dotnet-shared-access-signature-part-1.md) pour le disque. N’accordez l’accès qu’aux opérations dont une personne a besoin pour exécuter son travail. Par exemple, si vous voulez empêcher un utilisateur de copier un disque géré sur un compte de stockage, vous pouvez décider de lui interdire l’accès à l’action d’exportation sur ce disque géré. De la même manière, si vous voulez empêcher un utilisateur d’utiliser un URI de signature d’accès partagé pour copier un disque géré, vous pouvez décider de ne pas lui octroyer l’autorisation d’accès à ce disque géré.

## <a name="pricing-and-billing"></a>Tarification et facturation 

Lorsque vous utilisez Managed Disks, les considérations de facturation suivantes s’appliquent :

* Type de stockage

* Taille du disque

* Nombre de transactions

* Transferts de données sortantes

* Captures instantanées des disques gérés (copie intégrale du disque)

Examinons ces éléments de plus près.

**Type de stockage :** Managed Disks propose 2 niveaux de performance : [Premium](storage-premium-storage.md) (disques SSD) et [Standard](storage-standard-storage.md) (disque dur). La facturation d’un disque géré dépend du type de stockage sélectionné.


**Taille de disque** : la facturation des disques gérés dépend de leur taille configurée. Azure mappe la taille configurée des disques (arrondie à la valeur supérieure) sur l’option Managed Disks la plus proche, tel qu’indiqué dans les tableaux ci-dessous. Chaque disque géré mappe sur l’une des tailles configurées prises en charge et est facturé en conséquence. Par exemple, si vous créez un disque géré standard et définissez une taille configurée de 200 Go, vous êtes facturé selon la tarification du disque S20.

Voici les tailles de disque disponible pour un disque géré Premium :

| **Disques gérés<br> Premium**  | **P10** | **P20** | **P30**        |
|------------------|---------|---------|----------------|
| Taille du disque        | 128 Go  | 512 Go  | 1024 Go (1 To) |

Voici les tailles de disque disponible pour un disque géré Standard : 

| **Disques gérés<br> Standard** | **S4**  | **S6**  | **S10**        | **S20** | **S30**        |
|------------------|---------|---------|----------------|---------|----------------|
| Taille du disque        | 32 Go   | 64 Go   | 128 Go  | 512 Go  | 1024 Go (1 To) |

**Nombre de transactions** : vous êtes facturé pour le nombre de transactions effectuées sur un disque géré standard. Les transactions associées aux disques gérés Premium ne sont pas facturées.

**Transferts de données sortantes**: les [transferts de données sortantes](https://azure.microsoft.com/pricing/details/data-transfers/) (données sortant des centres de données Azure) sont facturés en fonction de la bande passante utilisée.

**Captures instantanées des disques gérés (copie de disque intégrale)** : une copie instantanée d’un disque géré est une copie en lecture seule d’un disque géré qui est stocké en tant que disque géré standard. Avec des captures instantanées, vous pouvez sauvegarder vos disques gérés à tout moment dans le temps. Ces captures instantanées existent indépendamment du disque source et peuvent être utilisées pour créer des disques gérés par la suite. Le coût d’une capture instantanée gérée est le même que celui d’un disque géré standard. Par exemple, si vous avez une capture instantanée d’un disque géré Premium de 128 Go, le coût de la capture instantanée gérée équivaut à celui d’un disque géré Standard de 128 Go.

Pour le moment, les [captures instantanées incrémentielles](storage-incremental-snapshots.md) ne sont pas prises en charge pour les disques gérés, mais elles le seront dans le futur.

Pour en savoir plus sur la création de captures instantanées avec Managed Disks, consultez ces ressources :

* [Créer une copie d’un disque dur virtuel stocké en tant que disque géré à l’aide de la fonction Instantanés dans Windows](../virtual-machines/virtual-machines-windows-snapshot-copy-managed-disk.md)
* [Créer une copie d’un disque dur virtuel stocké en tant que disque géré à l’aide de la fonction Instantanés dans Linux](../virtual-machines/linux/virtual-machines-linux-snapshot-copy-managed-disk.md)


Pour plus d’informations sur la tarification d’Azure Managed Disks, consultez la page [Managed Disks Tarification](https://azure.microsoft.com/pricing/details/managed-disks).

## <a name="images"></a>Images

Managed Disks prend également en charge la création d’une image personnalisée gérée. Vous pouvez créer une image à partir de votre disque dur virtuel personnalisé dans un compte de stockage ou directement à partir d’une machine virtuelle en cours d’exécution. L’ensemble des disques gérés associés à une machine virtuelle en cours d’exécution sont capturés dans une seule image. Il s’agit notamment des disques de données et du système d’exploitation. Dès lors, vous pouvez créer des centaines de machines virtuelles à l’aide de votre image personnalisée, sans qu’il ne soit nécessaire de copier ou de gérer aucun compte de stockage.

Pour plus d’informations sur la création des images, consultez les articles suivants :
* [Procédure de capture d’une image managée d’une machine virtuelle généralisée dans Azure](../virtual-machines/virtual-machines-windows-capture-image-resource.md)
* [Généraliser et capturer une machine virtuelle Linux à l’aide d’Azure CLI 2.0 (version préliminaire)](../virtual-machines/virtual-machines-linux-capture-image.md)

## <a name="images-versus-snapshots"></a>Images et captures instantanées

Souvent, le mot « image » est associé aux machines virtuelles. C’est désormais aussi le cas du terme « captures instantanées ». Il est important de bien saisir la différence entre les&2;. Managed Disks vous permet de saisir une image d’une machine virtuelle généralisée qui a été libérée. Cette image comprend l’ensemble des disques attachés à la machine virtuelle. Vous pouvez l’utiliser pour créer une autre machine virtuelle, qui le cas échéant intégrera l’ensemble des disques.

Une capture instantanée est une copie d’un disque à un instant t. Elle s’applique uniquement à un disque. Si vous possédez une machine virtuelle qui présente uniquement un disque (le système d’exploitation), vous pouvez en saisir une capture instantanée ou une image, à partir desquelles vous créez une machine virtuelle.

Que se passe-t-il si vous possédez une machine virtuelle qui a cinq disques agrégés par bande ? Vous pouvez saisir une capture instantanée de chacun des disques, mais l’état des disques n’est pas connu au sein de la machine virtuelle. Les captures ne sont relatives qu’à leur disque respectif. Dans ce scénario, il faudrait coordonner les captures instantanées ; cette fonctionnalité n’est pas actuellement prise en charge. Ici donc, si vous souhaitez créer une copie de votre machine virtuelle, il vous faudra créer une image. Par défaut, l’image comprend une copie coordonnée des&5; disques.

## <a name="azure-backup-service-support"></a>Prise en charge du service Sauvegarde Azure 

Les machines virtuelles avec disques non gérés peuvent être sauvegardées à l’aide de Sauvegarde Azure. [Détails supplémentaires](../backup/backup-azure-vms-first-look-arm.md).

Vous pouvez également utiliser le service Sauvegarde Azure avec Managed Disks pour créer une tâche de sauvegarde avec des sauvegardes périodiques, une restauration facile des machines virtuelles et des stratégies de rétention de sauvegarde. Vous pouvez en savoir plus sur ce point dans [Using Azure Backup service for VMs with Managed Disks](../backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup) (Utilisation du service Sauvegarde Azure pour les machines virtuelles avec Managed Disks). 

## <a name="managed-disks-and-storage-service-encryption-sse"></a>Managed Disks et Storage Service Encryption (SSE)

Le stockage Azure prend automatiquement en charge le chiffrement des données écrites sur le compte de stockage. Pour plus d’informations, consultez la page [Azure Storage Service Encryption pour les données au repos](storage-service-encryption.md). Qu’en est-il des données sur vos disques gérés ? Actuellement, il n’est pas possible d’activer Storage Service Encryption pour Managed Disks ; vous le pourrez à l’avenir. En attendant vous devez vous familiariser avec l’utilisation d’un fichier de disque dur virtuel chiffré hébergé dans un compte de stockage chiffré. 

SSE chiffre les données durant leur écriture sur le compte de stockage. Si vous possédez un fichier de disque dur virtuel qui n’a jamais été chiffré avec SSE, vous ne pouvez pas l’utiliser pour créer une machine virtuelle utilisant Managed Disks. Il n’est pas non plus possible de convertir un disque non géré chiffré en disque géré. Enfin, précisons que si vous désactivez le chiffrement sur ce compte de stockage, l’opération n’est pas inversée ; le fichier de disque dur virtuel n’est pas déchiffré. 

Pour utiliser un disque déchiffré, vous devez dans un premier temps copier le fichier de disque dur virtuel sur un compte de stockage qui n’a jamais été chiffré. Vous pouvez ensuite créer une machine virtuelle avec Managed Disks et spécifier ce disque dur virtuel durant la création, ou attacher le fichier de disque dur virtuel copié sur une machine virtuelle en cours d’exécution avec Managed Disks. 

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur Managed Disks, consultez les articles suivants.

### <a name="get-started-with-managed-disks"></a>Bien démarrer avec les disques gérés 

* [Créer une machine virtuelle à l’aide de Resource Manager et de PowerShell](../virtual-machines/virtual-machines-windows-ps-create.md)

* [Créer une machine virtuelle Linux à l’aide d’Azure CLI 2.0 (version préliminaire)](../virtual-machines/virtual-machines-linux-quick-create-cli.md)

* [Attach a managed data disk to a Windows VM using PowerShell](../virtual-machines/virtual-machines-windows-attach-disk-ps.md) (Attacher un disque de données géré à une machine virtuelle Windows à l’aide de PowerShell)

* [Ajouter un disque géré à une machine virtuelle Linux](../virtual-machines/virtual-machines-linux-quick-create-cli.md)

### <a name="compare-managed-disks-storage-options"></a>Comparer les options de stockage Managed Disks 

* [Premium Storage : stockage hautes performances pour les charges de travail des machines virtuelles Azure](storage-premium-storage.md)

* [Cost-effective Standard Storage and unmanaged and managed Azure VM disks](storage-standard-storage.md) (Standard Storage : stockage économique pour les disques de machines virtuelles Azure gérées et non gérées)

### <a name="operational-guidance"></a>Instructions d’utilisation

* [Migrate from AWS and other platforms to Managed Disks in Azure](../virtual-machines/virtual-machines-windows-on-prem-to-azure.md) (Migrer de AWS et d’autres plates-formes vers Managed Disks dans Azure)

* [Migrate Azure VMs to Managed Disks in Azure](../virtual-machines/virtual-machines-windows-migrate-to-managed-disks.md) (Migrer des machines virtuelles Azure vers Managed Disks dans Azure)



<!--HONumber=Feb17_HO2-->


