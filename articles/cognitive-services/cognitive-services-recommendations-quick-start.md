---
title: "Guide de démarrage rapide : API Machine Learning Recommendations | Microsoft Docs"
description: "Azure Machine Learning Recommendations - Guide de démarrage rapide"
services: cognitive-services
documentationcenter: 
author: luiscabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 7f90fe5e-c0bf-4c16-9624-49118adf9069
ms.service: cognitive-services
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: luisca
translationtype: Human Translation
ms.sourcegitcommit: 2acb7a96b04ba457fefe811765b17ee869de52bc
ms.openlocfilehash: 86d297fd902eb1eb8af159bc684ed119feef2b30


---
# <a name="quick-start-guide-for-the-cognitive-services-recommendations-api"></a>Guide de démarrage rapide pour l'API de recommandations Cognitive Services
Ce document décrit comment intégrer votre service ou application pour utiliser les [API de recommandations](http://go.microsoft.com/fwlink/?LinkId=759710).
Vous trouverez [ici](http://go.microsoft.com/fwlink/?LinkId=759709)plus d'informations sur l'API de recommandations et d’autres Cognitive Services. Tout au long de ce guide, la [Référence de l’API de recommandations](http://go.microsoft.com/fwlink/?LinkId=759348) et le [Guide de démarrage rapide de l’interface utilisateur des recommandations](cognitive-services-recommendations-ui-intro.md) pourront également vous être utiles.

<a name="Overview"></a>

### <a name="general-overview"></a>Présentation générale
Ce document est un guide pas à pas. Notre objectif est de vous guider à travers les étapes nécessaires pour effectuer l’apprentissage d’un modèle et de vous indiquer les ressources qui vous permettront d’utiliser le modèle à partir de votre environnement de production.

Cet exercice prend environ 30 minutes.

Pour utiliser l’ [API de recommandations](http://go.microsoft.com/fwlink/?LinkId=759710), procédez comme suit :

1. Créez un modèle : le modèle est le conteneur de vos données d’utilisation, des données de catalogue et du modèle de recommandation.
2. Importez les données du catalogue : un catalogue contient des informations de métadonnées sur les éléments.
3. Importez des données d'utilisation : les données d'utilisation peuvent être téléchargées de deux manières :
   * En téléchargeant un fichier qui contient les données d'utilisation.
   * Envoyez des événements d’acquisition de données.
     Généralement, vous téléchargez un fichier d’utilisation pour pouvoir créer un modèle de recommandation initial (démarrage) et l’utiliser jusqu’à ce que le système rassemble suffisamment de données en utilisant le format d’acquisition de données.
4. Générez un modèle de recommandation : il s’agit d’une opération asynchrone dans laquelle le système de recommandation prend toutes les données d’utilisation et crée un modèle de recommandation. Cette opération peut prendre plusieurs minutes, voire plusieurs heures, selon la taille des données et les paramètres de configuration de génération. Lors du déclenchement de la build, vous obtenez un ID de build. Utilisez-le pour vérifier à quel moment s’est terminé le processus de génération et ce, avant de commencer à utiliser les recommandations.
5. Utilisez les recommandations : obtenez des recommandations pour un élément spécifique ou pour une liste d’éléments.

<a name="GetStarted"></a>

### <a name="lets-get-started"></a>C’est parti !
Vous allez commencer la génération d'un modèle de recommandations. Puis nous vous expliquerons comment utiliser les résultats générés par le modèle pour alimenter des recommandations sur un site de commerce électronique.

<a name="Ex1Task1"></a>

#### <a name="task-1---signing-up-for-the-recommendations-api"></a>Tâche 1: Inscription à l'API de recommandations
Au cours de cette tâche, vous allez vous inscrire au service de l’API de recommandations et créer un modèle de recommandations.

1. Sur le **portail Azure** , accédez à [http://portal.azure.com/](http://portal.azure.com/) et connectez-vous avec votre compte Azure.
2. Cliquez sur **+ Nouveau**.
3. Sélectionnez l’option **Intelligence** .
4. Sélectionnez le produit **API Cognitive Services** .
   Ce produit vous permettra de commencer un abonnement à l’une des API Cognitive Services (Face, Text Analytics, Computer Vision, etc.). Aujourd'hui, nous nous concentrerons sur l'API de recommandations.
5. Sur la page d’accueil de l’API Cognitive Services, entrez le **Nom de compte** de votre abonnement Recommandations. (Par exemple : « MyRecommendations »). Ce nom ne doit pas contenir d'espaces.
6. Pour **Type d’API**, sélectionnez **Recommandations**.
7. Pour **Niveau tarifaire**, vous pouvez sélectionner un plan. Vous pouvez sélectionner le niveau **Gratuit** pour 10 000 transactions/mois. Il s'agit d'un plan gratuit : c’est donc un bon moyen d’essayer le système. Une fois en production, nous vous recommandons de prendre en compte votre volume de demandes et de modifier votre type de plan en conséquence. (Remarque : vous ne pouvez avoir qu’un seul abonnement de niveau Gratuit à la fois)
8. Sélectionnez un **Groupe de ressources**, ou créez-en un nouveau si vous n'en avez pas.
9. Vous pouvez modifier d'autres éléments dans la boîte de dialogue Créer. À noter que le fournisseur de ressources est à ce jour uniquement pris en charge à partir des centres de données des États-Unis.
10. Une fois les sélections terminées, cliquez sur **Créer**.
11. Attendez quelques minutes que la ressource se déploie.
    Une fois qu’elle est déployée, vous pouvez accéder à la section **Clés** dans le panneau **Paramètres** où vous seront fournies une clé primaire et une clé secondaire pour utiliser l’API.  Copiez la clé primaire, car vous en aurez besoin lors de la création de votre premier modèle.

<a name="Ex1Task2"></a>

#### <a name="task-2---did-you-bring-your-data"></a>Tâche 2 - Avez-vous emporté des données ?
L'API de recommandations apprendra à partir de votre catalogue et de vos opérations afin de fournir de bonnes recommandations de produit. Cela signifie que devez lui fournir de bonnes données sur vos produits (ce que nous appelons fichier de **catalogue**) et un ensemble de transactions suffisamment grand pour qu’elle y trouve des modèles de consommation intéressants (ce que nous appelons **utilisation**).

1. Généralement, vous récupérez ces éléments d'information en interrogeant votre base de données de transactions.
   Au cas où vous n’en auriez pas à disposition, nous vous proposons des exemples de données fondés sur les données de transactions Microsoft Store.
   
   Vous pouvez télécharger les données [ici](http://aka.ms/RecoSampleData). Copiez et décompressez l’archive MsStoreData.Zip dans un dossier sur votre ordinateur local.
   
   > [!NOTE]
   > L’exemple de code que vous allez télécharger et exécuter dans la tâche 3 comporte des exemples de données déjà intégrés. Cette tâche est donc facultative.  Cela dit, elle vous permettra de télécharger des jeux de données plus réalistes et de mieux comprendre les entrées dans l'API de recommandations.
   > 
   > 
2. Examinons maintenant le fichier de catalogue. Naviguez jusqu'à l'emplacement où vous avez copié les données.
   Ouvrez le fichier de catalogue dans le **bloc-notes**.
   
   Vous remarquerez que le fichier de catalogue est assez simple. Il a le format suivant `<itemid>,<item name>,<product category>`
   
   > AAA-04294,OfficeLangPack 2013 32/64 E34 Online DwnLd,Office  <br>
   > AAA-04303,OfficeLangPack 2013 32/64 ET Online DwnLd,Office  <br>
   >  C9F-00168,KRUSELL Kiruna Flip Cover for Nokia Lumia 635 - Camel,Accessories
   > 
   > 
   
   À noter qu’un fichier de catalogue peut être beaucoup plus riche. Par exemple, vous pouvez ajouter des métadonnées sur les produits (ce que nous appelons *caractéristiques d’éléments*). Consultez la section relative au [format de catalogue](http://go.microsoft.com/fwlink/?LinkID=760716) dans la Référence de l’API pour plus d’informations sur le format de catalogue.
3. Faisons de même avec les données d'utilisation. Vous remarquerez que les données d’utilisation sont au format `<User Id>,<Item Id>,<Time Stamp>,<Event>`.
   
   > 00037FFEA61FCA16,288186200,2015/08/04T11:02:52,Purchase 0003BFFDD4C2148C,297833400,2015/08/04T11:02:50,Purchase 0003BFFDD4C2118D,297833300,2015/08/04T11:02:40,Purchase 00030000D16C4237,297833300,2015/08/04T11:02:37,Purchase 0003BFFDD4C20B63,297833400,2015/08/04T11:02:12,Purchase 00037FFEC8567FB8,297833400,2015/08/04T11:02:04,Purchase
   > 
   > 

Notez que les trois premiers éléments sont obligatoires. Le type d’événement est facultatif. Vous pouvez consulter le [format d’utilisation](http://go.microsoft.com/fwlink/?LinkID=760712) pour plus d’informations sur cette rubrique.

> **De quelle quantité de données avez-vous besoin ?**
> 
> <p>
> Cela dépend vraiment des données d'utilisation. Le système apprend lorsque les utilisateurs achètent différents éléments. Pour certaines versions comme FBT, il est important de savoir quels éléments sont achetés dans une même transaction. (C’est ce que nous appelons *co-occurrences*.) Si vous avez 10 000 éléments dans votre catalogue, nous recommandons donc de disposer d'au moins 20 fois ce nombre de transactions, soit environ 200 000 transactions.
> Une fois encore, il s'agit d'une règle empirique. Vous devez faire des essais avec vos données.
> </p>
> 
> 

<a name="Ex1Task3"></a>

#### <a name="task-3---creating-a-recommendations-model"></a>Tâche 3 : Création d'un modèle de recommandations
Maintenant que vous avez un compte et que vous disposez de données, créons votre premier modèle.

Dans cette tâche, vous allez utiliser l'exemple d'application pour générer votre premier modèle.

1. Avant tout, vous devez connaître la [Référence de l’API de recommandations](http://go.microsoft.com/fwlink/?LinkId=759348).
2. Téléchargez l’ [Exemple d’application](http://go.microsoft.com/fwlink/?LinkID=759344) dans un dossier local.
3. Dans Visual Studio, ouvrez la solution **RecommendationsSample.sln** située dans le dossier **C#**.
4. Ouvrez le fichier **SampleApp.cs** . Notez les étapes suivantes dans le fichier :
   
   * Création d'un modèle
   * Ajout d’un fichier de catalogue
   * Ajout d’un fichier d'utilisation
   * Déclenchement d’une génération du modèle
   * Récupération d’une recommandation basée sur une paire d'éléments
     <p></p>
5. Remplacez la valeur du champ **AccountKey** par la clé de la tâche 1.
6. Parcourez la solution : vous verrez comment se crée un modèle.
7. Essayez de remplacer les fichiers de catalogue et d'utilisation que vous venez de télécharger pour créer un nouveau modèle pour Microsoft Store ou pour les recommandations de livres. Vous devez modifier le nom du modèle et les éléments pour lesquels vous demandez des recommandations.
8. Lorsque le modèle est créé, notez l’ **ID de modèle** car vous en aurez besoin lors de la demande de recommandations dans votre environnement de production.

> Apprenez-en plus sur les types de build et la manière d’évaluer la qualité des builds [ici](cognitive-services-recommendations-buildtypes.md).
> 

<a name="Ex1Task4"></a>

### <a name="putting-your-model-in-production"></a>Placement de votre modèle en production !
Maintenant que vous savez comment créer un modèle et utiliser des recommandations, l'étape suivante est la mise en production sur votre site web ou votre application mobile ou l’intégration à votre système CRM ou ERP.
Évidemment, chacune de ces implémentations est différente. Étant donné que l'API de recommandations est interrogée en tant que service web, vous devriez pouvoir l'appeler facilement à partir de ces différents environnements.

>  [!IMPORTANT]
>  Si vous souhaitez afficher les recommandations à partir d’un client public (par exemple, votre site de commerce électronique), vous devez créer un serveur proxy pour fournir les recommandations. Ceci est important pour que votre clé d'API ne soit pas exposée à des entités externes (potentiellement non fiables).

Voici quelques idées d’emplacements où vous pouvez utiliser les recommandations :

### <a name="product-page"></a>Page produits
**Recommandations d’articles**

<p>Si l’apprentissage du modèle a été effectué sur des données d’achat, il permet à votre client de *découvrir les produits qui sont susceptibles d’intéresser les personnes ayant acheté l’élément source*.</p>
<p>Si l’apprentissage du modèle a été effectué sur des données de clic, il permet à votre client de *découvrir les produits qui sont susceptibles d’être consultés par les personnes ayant consulté l’élément source*. Ce type de modèle peut retourner des articles similaires.</p>

**Recommandations d’articles fréquemment achetés ensemble**

<p>Une build d’articles fréquemment achetés ensemble peut être entraînée, afin d'obtenir des jeux d’articles susceptibles d'être achetés avec cet article.</p>

### <a name="check-out-page"></a>Consultez la page
**Recommandations d’articles**

<p>Un modèle de recommandations peut prendre en entrée une liste d’articles. Par conséquent, vous pouvez transmettre tous les éléments du panier comme entrée pour obtenir des recommandations.
Dans ce cas, le modèle fournit des recommandations intéressantes compte tenu de tous les articles dans le panier.
</p>

### <a name="landing-page"></a>Page d’accueil
**Recommandations utilisateur**

<p>
Un modèle de recommandation peut prendre en entrée l'identifiant utilisateur.  Il utilise l'historique des transactions de l'utilisateur pour fournir des recommandations personnalisées à l'utilisateur spécifié.
</p>

Consultez la [documentation Obtenir des recommandations d’articles](http://go.microsoft.com/fwlink/?LinkID=760719).

<a name="Ex1Task6"></a>

### <a name="whats-next"></a>Et ensuite ?
Félicitations si vous êtes arrivé jusqu’ici ! Pour en savoir plus, vous pouvez consulter la [Référence de l’API de recommandations](http://go.microsoft.com/fwlink/?LinkId=759348). Si vous avez des questions, n’hésitez pas à nous contacter à l’adresse mlapi@microsoft.com




<!--HONumber=Nov16_HO5-->


