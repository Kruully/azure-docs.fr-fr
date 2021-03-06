---
title: "Création d&quot;un espace de travail Machine Learning | Microsoft Docs"
description: "Création d’un espace de travail pour Microsoft Azure Machine Learning Studio."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2016
ms.author: garye;bradsev;ahgyger
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 647398c1a0994da04845672e89d9f82d5e8b6d21


---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Créer et partager un espace de travail Azure Machine Learning
Ce menu pointe vers des rubriques qui décrivent comment configurer les différents environnements de science de données utilisés par le processus d’analyse Cortana (CAP).

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Pour utiliser Azure Machine Learning Studio, vous devez disposer d’un espace de travail Machine Learning. Cet espace de travail contient les outils dont vous avez besoin pour créer, gérer et publier des expériences.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="to-create-a-workspace"></a>Pour créer un espace de travail
1. Connectez-vous au [portail Microsoft Azure Classic].

> [!NOTE]
> Pour vous connecter, vous devez être un administrateur d’abonnement Azure. Le propriétaire d’un espace de travail Machine Learning ne vous donne pas accès au [portail Microsoft Azure Classic]. Consultez la page [Privileges of Azure subscription administrator and workspace owner (Privilèges de l’administrateur d’abonnement Azure et du propriétaire de l’espace de travail)](#subscriptionvsworkspace) pour plus de détails.
> 
> 

1. Dans le volet des services Microsoft Azure, cliquez sur **MACHINE LEARNING**.
   
    ![Service Machine Learning][1]
2. Cliquez sur **+NOUVEAU** en bas de la fenêtre.
3. Cliquez sur **SERVICES DE DONNÉES**, puis **APPRENTISSAGE AUTOMATIQUE**, puis **CRÉATION RAPIDE**.
   
    ![Création rapide du nouvel espace de travail][3]
4. Entrez un **NOM D’ESPACE DE TRAVAIL** pour votre espace de travail.
5. Spécifiez l’**EMPLACEMENT** Azure, puis entrez un **COMPTE DE STOCKAGE** Azure existant ou sélectionnez **Créer un compte de stockage** pour en créer un.
6. Cliquez sur **CRÉER UN ESPACE DE TRAVAIL ML**.

Une fois votre espace de travail Machine Learning créé, vous voyez son nom apparaître sur la page **Machine Learning** .

## <a name="sharing-an-azure-machine-learning-workspace"></a>Partage d’un espace de travail Azure Machine Learning
Une fois l’espace de travail Machine Learning créé, vous pouvez y inviter les utilisateurs et partager son accès et toutes ses expériences. Nous prenons en charge les deux rôles d’utilisateurs suivants :

* **Utilisateur** : un utilisateur de l’espace de travail peut créer, ouvrir, modifier et supprimer des groupes de données, des expériences et des services web dans l’espace de travail.
* **Propriétaire** : un propriétaire peut inviter, supprimer et répertorier les utilisateurs ayant accès à l’espace de travail et les actions qu’ils sont autorisés à effectuer. Il a également accès aux ordinateurs blocs-notes.

### <a name="to-share-a-workspace"></a>Pour partager un espace de travail
1. Connectez-vous à [Azure Machine Learning Studio]
2. Dans le panneau Machine Learning Studio, cliquez sur **PARAMÈTRES**
3. Cliquez sur **UTILISATEURS**
4. Cliquez sur **INVITE MORE USERS**
   
    ![INVITE MORE USERS][4]
5. Entrez une ou plusieurs adresses e-mail. L’utilisateur a seulement besoin d’un compte Microsoft valide (par exemple, name@outlook.com)) ou d’un compte professionnel (issu d’Azure Active Directory).
6. Cliquez sur le bouton représentant une coche.

Chaque utilisateur ajouté recevra un e-mail contenant des instructions de connexion à l’espace de travail partagé.

Pour plus d’informations sur la gestion de votre espace de travail, consultez [Gestion d’un espace de travail Azure Machine Learning].
En cas de problème lors de la création de votre espace de travail, consultez [Guide de résolution des problèmes : création et connexion à un espace de travail Machine Learning].

## <a name="a-namesubscriptionvsworkspaceaprivileges-of-azure-subscription-administrator-and-of-workspace-owner"></a><a name="subscriptionvsworkspace"></a>Privileges of Azure subscription administrator and workspace owner (Privilèges de l’administrateur d’abonnement Azure et du propriétaire de l’espace de travail)
Voici une table permettant de clarifier la différence entre un administrateur d’abonnement Azure et un propriétaire d’espace de travail.

| Actions | Administrateur d’abonnement Azure | Propriétaire de l'espace de travail |
| --- |:---:|:---:|
| Accéder au [portail Microsoft Azure Classic] |Oui |Non |
| Créer un espace de travail |Oui |Non |
| Supprimer un espace de travail |Oui |Non |
| Ajouter un point de terminaison à un service web |Oui |Non |
| Supprimer un point de terminaison d’un service web |Oui |Non |
| Modifier l’accès concurrentiel pour un service web |Oui |Non |
| Accéder à [Azure Machine Learning Studio] |Non * |Oui |

> [!NOTE]
> * Un administrateur d’abonnement Azure est automatiquement ajouté à l’espace de travail qu’il crée en tant que propriétaire de l’espace de travail. Toutefois, le simple fait d’être administrateur d’abonnement Azure ne lui accorde pas l’accès à tous les espaces de travail sous cet abonnement.
> 
> 

<!-- ![List of Machine Learning workspaces][2] -->

<!--Anchors-->
[Pour créer un espace de travail]: #createworkspace

<!--Image references-->
[1]: media/machine-learning-create-workspace/cw1.png
[2]: media/machine-learning-create-workspace/cw2.png
[3]: media/machine-learning-create-workspace/cw4.png
[4]: media/machine-learning-create-workspace/cw5.png


<!--Link references-->
[Gestion d’un espace de travail Azure Machine Learning]: machine-learning-manage-workspace.md
[Guide de résolution des problèmes : création et connexion à un espace de travail Machine Learning]: machine-learning-troubleshooting-creating-ml-workspace.md
[Azure Machine Learning Studio]: https://studio.azureml.net/  
[portail Microsoft Azure Classic]: https://manage.windowsazure.com/



<!--HONumber=Nov16_HO3-->


