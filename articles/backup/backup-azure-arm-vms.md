---
title: Sauvegarder des machines virtuelles Azure dans un coffre Recovery Services | Microsoft Docs
description: "Détectez, inscrivez et sauvegardez des machines virtuelles Azure dans un coffre Recovery Services."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "sauvegarde de machine virtuelle ; sauvegarder la machine virtuelle ; sauvegarde et récupération d’urgence ; sauvegarde de machine virtuelle arm"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 1/30/2017
ms.author: trinadhk;jimpark;markgal;
translationtype: Human Translation
ms.sourcegitcommit: 39147f2db1e660a21d6ed622206787ea0c569056
ms.openlocfilehash: 28a5014f7ee73b30f879d249811e7fc303b13ac6


---
# <a name="back-up-azure-vms-to-a-recovery-services-vault"></a>Sauvegarder des machines virtuelles Azure dans un coffre Recovery Services
> [!div class="op_single_selector"]
> * [Back up VMs to Recovery Services vault](backup-azure-arm-vms.md)
> * [Back up VMs to Backup vault](backup-azure-vms.md)
>
>

Cet article décrit la procédure de sauvegarde des machines virtuelles Azure (déployées à l’aide du modèle Resource Manager ou du modèle Classic) dans un coffre Recovery Services. La plupart du travail requis pour la sauvegarde des machines virtuelles repose sur la préparation. Avant de sauvegarder ou de protéger une machine virtuelle, vous devez remplir les [conditions préalables](backup-azure-arm-vms-prepare.md) pour préparer votre environnement à la protection de vos machines virtuelles. Une fois que vous avez rempli les conditions préalables, vous pouvez lancer l’opération de sauvegarde pour prendre des instantanés de votre machine virtuelle.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Pour plus d’informations, consultez les articles sur la [planification de votre infrastructure de sauvegarde des machines virtuelles dans Azure](backup-azure-vms-introduction.md) et les [machines virtuelles Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Déclenchement du travail de sauvegarde
La stratégie de sauvegarde associée au coffre Recovery Services définit la fréquence et à quel moment l’opération de sauvegarde s’exécute. Par défaut, la première sauvegarde planifiée est la sauvegarde initiale. Jusqu’à celle-ci, l’état de la dernière sauvegarde dans le panneau **Travaux de sauvegarde** est défini sur **Avertissement (sauvegarde initiale en attente)**.

![Sauvegarde en attente](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

À moins que votre sauvegarde initiale ne soit prévue prochainement, il est recommandé de sélectionner l’option **Sauvegarder maintenant**. La procédure suivante commence à partir du tableau de bord du coffre. Cette procédure est utilisée pour l’exécution du travail de sauvegarde initial une fois les conditions préalables remplies. Si le travail de sauvegarde initial a déjà été exécuté, cette procédure n’est pas disponible. La stratégie de sauvegarde associée détermine le prochain travail de sauvegarde.  

Pour exécuter le travail de sauvegarde initial :

1. Dans la mosaïque **Sauvegarde** du tableau de bord du coffre, cliquez sur **Machines virtuelles Azure**. <br/>
    ![Icône Paramètres](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Le panneau **Éléments de sauvegarde** s’ouvre.
2. Dans le panneau **Éléments de sauvegarde**, cliquez avec le bouton droit sur le coffre à sauvegarder, puis cliquez sur **Sauvegarder maintenant**.

    ![Icône Paramètres](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Le travail de sauvegarde est déclenché. <br/>

    ![Travail de sauvegarde déclenché](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. Pour vérifier si votre sauvegarde initiale est terminée, dans la vignette **Travaux de sauvegarde** du tableau de bord du coffre, cliquez sur **Machines virtuelles Azure**.

    ![Vignette Travaux de sauvegarde](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Le panneau Travaux de sauvegarde s’ouvre.
4. Le panneau **Travaux de sauvegarde** indique l’état de tous les travaux.

    ![Vignette Travaux de sauvegarde](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > Au cours de l’opération de sauvegarde, l’extension de sauvegarde de chaque machine virtuelle vide toutes les écritures et prend un instantané cohérent.
   >
   >

    Lorsque le travail de sauvegarde est terminé, l’état affiché est *Terminé*.

## <a name="troubleshooting-errors"></a>Résolution des erreurs
Si vous rencontrez des problèmes pendant la sauvegarde de votre machine virtuelle, consultez [l’article sur le dépannage des machines virtuelles](backup-azure-vms-troubleshoot.md) pour obtenir de l’aide.

## <a name="next-steps"></a>Étapes suivantes
Vous avez protégé votre machine virtuelle. Vous pouvez maintenant consulter les articles suivants pour en savoir plus sur les tâches de gestion de machine virtuelle et la restauration des machines virtuelles.

* [Gestion et surveillance de vos machines virtuelles](backup-azure-manage-vms.md)
* [Restauration des machines virtuelles](backup-azure-arm-restore-vms.md)



<!--HONumber=Jan17_HO5-->


