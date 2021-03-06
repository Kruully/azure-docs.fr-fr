---
title: "Utilisation du portail Azure pour configurer le téléchargement du fichier | Microsoft Docs"
description: "Utilisation du portail Azure pour configurer votre IoT Hub en vue d’activer les transferts de fichiers à partir d’appareils connectés. Comprend des informations sur la configuration du compte de stockage Azure de destination."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 915f1597-272d-4fd4-8c5b-a0ccb1df0d91
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: aadbee1388e2c2aafaf0a5c50d2af195284d2bec


---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Configurer les chargements de fichiers IoT Hub à l’aide du portail Azure
## <a name="file-upload"></a>Chargement de fichiers
Pour utiliser la [fonctionnalité de chargement de fichiers dans IoT Hub][lnk-upload], vous devez d’abord associer un compte de stockage Azure à votre concentrateur. Sélectionnez les paramètres de **chargement de fichiers** pour afficher une liste de propriétés de fichier pour l’IoT Hub en cours de modification.

![][13]

**Conteneur de stockage**: utilisez le portail Azure pour sélectionner un conteneur d’objets blob dans un compte Azure Storage de votre abonnement Azure actuel à associer à votre IoT Hub. Si nécessaire, vous pouvez créer un compte Azure Storage dans le panneau **Comptes de stockage** et un conteneur d’objets blob dans le panneau **Conteneurs**. IoT Hub génère automatiquement des URI SAS avec des autorisations d’écriture pour ce conteneur d’objets blob pour les appareils à utiliser lorsqu’ils chargent des fichiers.

![][14]

**Recevoir des notifications pour les fichiers chargés**: activez ou désactivez les notifications de chargement de fichiers.

**SAS TTL**: ce paramètre est la durée de vie des URI de signature d’accès partagé renvoyés à l’appareil par IoT Hub. Il est défini par défaut sur une heure, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

**Durée de vie par défaut des paramètres de notification de fichiers**: la durée de vie d’une notification de chargement avant son expiration. Il est défini par défaut sur un jour, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

**Nombre maximal de remises de notifications de fichier**: le nombre de tentatives de remise d’une notification de chargement de fichier par l’IoT Hub. Il est défini par défaut sur 10, mais il peut être personnalisé avec d’autres valeurs à l’aide du curseur.

![][15]

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur les fonctionnalités de téléchargement de fichiers d’IoT Hub, consultez [Télécharger des fichiers à partir d’un appareil][lnk-upload] dans le guide du développeur IoT Hub.

Suivez ces liens pour en savoir plus sur la gestion de Azure IoT Hub :

* [Gérer en bloc des appareils IoT][lnk-bulk]
* [Métriques d’IoT Hub][lnk-metrics]
* [Surveillance des opérations][lnk-monitor]

Pour explorer davantage les capacités de IoT Hub, consultez :

* [Guide du développeur IoT Hub][lnk-devguide]
* [Simulation d’un appareil avec le Kit de développement logiciel (SDK) de la passerelle IoT][lnk-gateway]
* [Sécuriser votre solution IoT de bout en bout][lnk-securing]

[13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
[14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
[15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md



<!--HONumber=Dec16_HO1-->


