---
title: "Ajouter Azure Storage à l’aide des services connectés dans Visual Studio | Microsoft Docs"
description: "Ajouter le stockage Azure à votre application à l’aide de la boîte de dialogue Ajouter des services connectés de Visual Studio"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: 521ec044-ad4b-4828-8864-01decde2e758
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 0c7ca2909de28936f17ab7ac03815d693032b04f


---
# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Ajout de stockage Azure à l’aide des services connectés de Visual Studio
## <a name="overview"></a>Vue d'ensemble
Avec Visual Studio 2015, vous pouvez connecter n’importe quel service cloud C#, service mobile principal .NET, service ou site web ASP.NET, service ASP.NET 5 ou service de tâche web Azure au stockage Azure à l’aide de la boîte de dialogue **Ajouter des services connectés** . La fonctionnalité de service connecté ajoute l’ensemble des références et du code de connexion nécessaires, et modifie vos fichiers de configuration de manière appropriée. La boîte de dialogue vous permet également d’accéder à la documentation qui vous indique quelles sont les étapes suivantes pour démarrer le stockage d’objets BLOB, les files d’attente et les tables.

## <a name="supported-project-types"></a>Types de projet pris en charge
Vous pouvez utiliser la boîte de dialogue Services connectés pour vous connecter au stockage Azure dans les types de projets suivants.

* Projets web ASP.NET
* Projets ASP.NET 5
* Projets de rôle de travail et de rôle web de service cloud .NET
* Projets de services mobiles .NET
* Projets de tâche web Azure

## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Connexion au stockage Azure à l’aide de la boîte de dialogue Services connectés
1. Assurez-vous que vous disposez d’un compte Azure. Si vous n’en avez pas, vous pouvez demander un [essai gratuit](http://go.microsoft.com/fwlink/?LinkId=518146). Une fois que vous disposez d’un compte Azure, vous pouvez créer des comptes de stockage, créer des services mobiles et configurer Azure Active Directory.
2. Ouvrez votre projet dans Visual Studio, ouvrez le menu contextuel du nœud **Références** dans l’Explorateur de solutions, puis choisissez **Ajouter un service connecté**.
   
    ![Ajout d’un service connecté](./media/vs-azure-tools-connected-services-storage/IC796702.png)
3. Dans la boîte de dialogue **Ajouter un service connecté**, choisissez **Stockage Azure**, puis le bouton **Configurer**. Si vous n’êtes pas encore connecté à Azure, vous serez invité à le faire.
   
    ![Boîte de dialogue Ajouter des services connectés - Stockage](./media/vs-azure-tools-connected-services-storage/IC796703.png)
4. Dans la boîte de dialogue **Stockage Azure**, sélectionnez un compte de stockage existant, puis sélectionnez **Ajouter**.
   
    Si vous devez créer un compte de stockage, passez à l’étape suivante. Sinon, passez à l’étape 6.
   
    ![Boîte de dialogue Stockage Azure](./media/vs-azure-tools-connected-services-storage/IC796704.png)
5. Pour créer un compte de stockage : 
   
   1. Choisissez le bouton **Créer un compte de stockage** au bas de la boîte de dialogue Stockage Azure.
   2. Remplissez la boîte de dialogue **Créer un compte de stockage**, puis choisissez le bouton **Créer**.
      
       ![Boîte de dialogue Stockage Azure](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)
      
       Quand vous revenez à la boîte de dialogue **Stockage Azure** , le nouveau stockage s’affiche dans la liste.
   3. Sélectionnez le nouveau stockage dans la liste et sélectionnez **Ajouter**.
6. Le service connecté de stockage s’affiche sous le nœud Références de service de votre projet de tâche web.
   
    ![Stockage Azure dans un projet de tâches web](./media/vs-azure-tools-connected-services-storage/IC796705.png)
7. Dans la page Prise en main qui s’affiche, examinez de quelle manière votre projet a été modifié. Cette page s’affiche dans votre navigateur quand vous ajoutez un service connecté. Vous pouvez parcourir les étapes suggérées et les exemples de code fournis, ou bien afficher la page Que s’est-il passé ? pour voir quelles références ont été ajoutées à votre projet et quelles modifications ont été apportées à vos fichiers de code et de configuration.

## <a name="how-your-project-is-modified"></a>Modifications apportées à votre projet
Quand vous avez terminé la boîte de dialogue, Visual Studio ajoute des références et modifie certains fichiers de configuration. Les modifications spécifiques varient selon le type de projet. 

* Pour les projets ASP.NET, consultez [Que s’est-il passé ? – Projets ASP.NET](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
* Pour les projets ASP.NET 5, consultez [Que s’est-il passé ? – Projets ASP.NET 5](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
* Pour les projets de service cloud (rôles web et de travail), consultez [Que s’est-il passé ? – Projets de service cloud](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
* Pour les projets de tâche web, consultez [Que s’est-il passé ? – Projets de tâche web](storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Étapes suivantes
1. En vous basant sur les exemples de code de la page Prise en main, créez le type de stockage que vous voulez, puis démarrez l’écriture de code pour accéder à votre compte de stockage !
2. Posez des questions et obtenez de l’aide
   
   * [Forum MSDN : Stockage Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)
   * [Blog de l'équipe Azure Storage](http://blogs.msdn.com/b/windowsazurestorage/)
   * [Stockage sur azure.microsoft.com](https://azure.microsoft.com/services/storage/)
   * [Documentation du stockage sur azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)




<!--HONumber=Nov16_HO3-->


