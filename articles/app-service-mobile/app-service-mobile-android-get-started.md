---
title: "Créer une application Android sur Azure App Service Mobile Apps | Microsoft Docs"
description: "Suivez ce didacticiel pour commencer à utiliser des backends d’applications mobiles Azure pour le développement Android"
services: app-service\mobile
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 355f0959-aa7f-472c-a6c7-9eecea3a34a9
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: yuaxu
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e4aa29329242a7397fdb5939fa7b5b04340b53c5


---
# <a name="create-an-android-app"></a>Créer une application Android
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a>Vue d'ensemble
Ce didacticiel montre comment ajouter un backend cloud à une application mobile Android à l’aide d’un backend d’application mobile Azure.  Vous allez créer un backend d’application mobile et une simple application Android *Todo list* qui stocke les données d’application dans Azure.

Le suivi de ce didacticiel est un prérequis pour tous les autres didacticiels Android sur l’utilisation de la fonctionnalité Mobile Apps dans Azure App Service.

## <a name="prerequisites"></a>Composants requis
Pour réaliser ce didacticiel, vous avez besoin des éléments suivants :

* [Outils de développement Android](https://developer.android.com/sdk/index.html), qui incluent l’environnement de développement intégré Android Studio et la dernière plateforme Android.
* Kit de développement logiciel (SDK) Azure Mobile Android, qui est automatiquement inclus dans le projet de démarrage rapide que vous téléchargez.
* Un [compte Azure actif](https://azure.microsoft.com/pricing/free-trial/).

## <a name="create-a-new-azure-mobile-app-backend"></a>Créer un serveur principal d'applications mobiles Azure
[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

## <a name="configure-the-server-project"></a>Configurer le projet de serveur
[!INCLUDE [app-service-mobile-configure-new-backend.md](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-the-android-app"></a>Télécharger et exécuter l’application Android
[!INCLUDE [app-service-mobile-android-run-app](../../includes/app-service-mobile-android-run-app.md)]

<!-- URLs -->
[portail Azure]: https://portal.azure.com/
[Visual Studio Community 2013]: https://go.microsoft.com/fwLink/p/?LinkID=534203



<!--HONumber=Nov16_HO2-->


