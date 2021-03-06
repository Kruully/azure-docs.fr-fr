---
title: "Génération en mode ligne de commande pour Azure | Microsoft Docs"
description: "Génération en ligne de commande pour Azure"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 94b35d0d-0d35-48b6-b48b-3641377867fd
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e189bf8241266443bee4dbbb5faf1cffa983bdb4


---
# <a name="command-line-build-for-azure"></a>Génération en ligne de commande pour Azure
## <a name="overview"></a>Vue d'ensemble
Vous pouvez créer un package de déploiement Azure en exécutant MSBuild à partir de l'invite de commandes. Vous pouvez configurer et définir des builds pour le débogage, la gestion intermédiaire et la production, en plus de l'automatisation d’une partie du processus de génération.

## <a name="microsoft-build-engine-msbuild"></a>Microsoft Build Engine (MSBuild)
À l'aide de Microsoft Build Engine (MSBuild), vous pouvez générer des produits dans des environnements de build lab où Visual Studio n'est pas installé. MSBuild utilise pour les fichiers projet un format XML extensible et entièrement pris en charge par Microsoft. Dans ce format de fichier, vous pouvez décrire quels éléments doivent être générés pour une ou plusieurs plateformes et configurations.

Vous pouvez également exécuter MSBuild à l’invite de commande : c’est cette approche que décrit cette rubrique. En définissant des propriétés à l’invite de commandes, vous pouvez créer des configurations propres à un projet. De même, vous pouvez également définir les cibles que la commande MSBuild va générer. Pour plus d’informations sur les paramètres de ligne de commande et MSBuild, consultez la page [Référence de la ligne de commande MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

## <a name="installation"></a>Installation
Comme le décrit la procédure suivante, vous devez installer les logiciels et les outils sur le serveur de builds pour pouvoir créer un package Azure à l’aide de MSBuild :

1. Installez .NET Framework 4 ou version ultérieure, qui inclut MSBuild.
2. Installez les [outils de création Azure](http://go.microsoft.com/fwlink/?LinkId=394615) (recherchez MicrosoftAzureAuthoringTools-x64.msi ou MicrosoftAzureAuthoringTools-x86.msi).
3. Installez les [bibliothèques Azure pour .NET](http://go.microsoft.com/fwlink/?LinkId=394616) (recherchez MicrosoftAzureLibsForNet-x64.msi ou MicrosoftAzureLibs-x86.msi).
4. Copiez le fichier Microsoft.WebApplication.targets à partir d'une installation Visual Studio vers un autre ordinateur.
   
    Le fichier se trouve dans le répertoire C:\Program Files (x86)\MSBuild\Microsoft\Visual Studio\v12.0\WebApplications (v11.0 pour Visual Studio 2012). Vous devez le copier dans le même répertoire sur le serveur de builds.
5. Installez les outils [Azure Tools for Visual Studio](http://go.microsoft.com/fwlink/?LinkId=394616).
   
    Recherchez WindowsAzureTools.vs120.exe pour générer des projets Visual Studio 2013.

## <a name="msbuild-parameters"></a>Paramètres MSBuild
La façon la plus simple de créer un package consiste à exécuter MSBuild avec l’option `/t:Publish` . Par défaut, cette commande crée un répertoire en relation avec le dossier racine du projet, tel que ProjectDir\bin\Configuration\app.publish\. Lorsque vous générez un projet Azure, vous générez deux fichiers, le fichier de package et le fichier de configuration associé :

* Project.cspkg
* ServiceConfiguration.TargetProfile.cscfg

Par défaut, tous les projets Azure comprennent un fichier de configuration de service pour les versions locales (débogage) et un autre pour les versions cloud (gestion intermédiaire ou production), mais vous pouvez ajouter ou supprimer des fichiers de configuration de service selon vos besoins. Lorsque vous générez un package dans Visual Studio, il vous est demandé quels fichiers de configuration de service vous voulez inclure avec le package. Lorsque vous générez un package avec MSBuild, le fichier de configuration de service local est inclus par défaut. Pour inclure un autre fichier de configuration de service, définissez la propriété `TargetProfile` de la commande MSBuild (`MSBuild /t:Publish /p:TargetProfile=ProfileName`).

Si vous souhaitez utiliser un autre répertoire pour les fichiers de configuration et de package stockés, définissez le chemin d'accès à l'aide de l’option `/p:PublishDir=Directory\`, en incluant la barre oblique inverse de fin comme séparateur.

## <a name="deployment"></a>Déploiement
Une fois le package créé, vous pouvez le déployer sur Azure. Pour voir un didacticiel qui illustre ce processus, consultez le site web Azure. Pour plus d’informations sur la façon d’automatiser ce processus, consultez [Livraison continue pour les services cloud dans Azure](cloud-services/cloud-services-dotnet-continuous-delivery.md).




<!--HONumber=Nov16_HO3-->


