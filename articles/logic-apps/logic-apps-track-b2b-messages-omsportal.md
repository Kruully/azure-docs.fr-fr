---
title: Suivre des messages B2B dans le portail OMS | Microsoft Docs
description: Suivi des messages B2B dans le portail OMS
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 53195091ac4b93ed94f432990c84c407615fc03e
ms.openlocfilehash: 9c3855c7fce5a9f38424f0bb6cd03f7a2c8d36be


---
# <a name="tracking-b2b-messages-in-oms-portal"></a>Suivi des messages B2B dans le portail OMS
La communication B2B implique des échanges de messages entre deux processus ou applications métier en cours d’exécution. Le suivi des messages B2B dans le portail OMS fournit des fonctionnalités de suivi web riches qui permettent de visualiser si les messages sont correctement traités.  Vous pouvez effectuer le suivi des éléments suivants :

* Nombre et état des messages
* État des accusés de réception
* Corrélation entre les messages et les accusés de réception
* Description détaillée des erreurs en cas échec
* Fonctionnalités de recherche

## <a name="prerequisites"></a>Composants requis
* Un compte Azure (que vous pouvez [créer gratuitement)](https://azure.microsoft.com/free)
* Un compte d’intégration (vous pouvez créer un [compte d’intégration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) et activer la journalisation en suivant les étapes présentées [ici](logic-apps-monitor-b2b-message.md))
* Une application logique (vous pouvez créer une [application logique](../logic-apps/logic-apps-create-a-logic-app.md) et activer la journalisation en suivant les étapes présentées [ici](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics-and-alerts))

## <a name="adding-logic-apps-b2b-solution-to-oms-portal"></a>Ajout d’une solution B2B Logic Apps sur le portail OMS

1. Sélectionnez **Plus de services** dans le portail, recherchez **Log Analytics**, puis sélectionnez **Log Analytics** .  
![Recherche de Log Analytics](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)  

2. Sélectionnez **Log Analytics**  
![Sélection de Log Analytics](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Sélectionnez **Portail OMS** pour ouvrir la page d’accueil du portail OMS.   
![Navigation dans le portail OMS](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Sélectionnez **Galerie de solutions**    
![Sélectionnez Galerie de solutions](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Sélectionnez **Logic Apps B2B**     
![Sélection de Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Cliquez sur **Ajouter** pour ajouter **Messages Logic Apps B2B** à la page d’accueil  
![Sélection de l’option Ajouter](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

7. Parcourez la page d’accueil pour afficher **Messages Logic Apps B2B**   
![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

## <a name="tracking-data-in-oms-portal"></a>Suivi des données dans le portail OMS

1. Processus de publication des messages ; la page d’accueil affiche le nombre de messages   
![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

2. La sélection de **Messages Logic Apps B2B** dans la page d’accueil aboutit aux états de message AS2 et X12.  Les données sont basées sur le dernier jour.
![Sélection des messages Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)



3. La sélection des messages AS2 ou X12 en fonction de leur état vous permet d’accéder à la liste des messages   
![Sélectionner l’état de message AS2](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

| Propriété | Description |
| --- | --- |
| Sender | Partenaire invité configuré dans les paramètres de réception ou partenaire hôte configuré dans les paramètres d’envoi pour un contrat AS2 |
| Receiver | Partenaire hôte configuré dans les paramètres de réception ou partenaire invité configuré dans les paramètres d’envoi pour un contrat AS2 |
| Application logique | Application logique dans laquelle les actions AS2 sont configurées |
| État | État du message AS2. Réussite = réception ou envoi d’un message AS2 valide, aucune notification d’état du message (MDN) configurée ; Réussite = réception ou envoi d’un message AS2 valide, notification d’état du message configurée, et notification d’état du message envoyée ou reçue ; Échec = Réception d’un message AS2 non valide, aucune notification d’état du message configurée ; En attente = réception ou envoi d’un message AS2 valide, notification d’état du message configurée, en attente d’un accusé de réception fonctionnel (ack) |
| Ack | État du message de notification d’état du message |
| Direction | Direction du message AS2 |
| ID de corrélation : | ID pour mettre en corrélation l’ensemble des déclencheurs et des actions au sein d’une application logique |
| ID de message |  ID du message AS2 dans les en-têtes du message AS2 |
| Timestamp | Heure à laquelle l’action AS2 traite le message |
|  |  |


![Sélectionner l’état de message X12](media/logic-apps-track-b2b-messages-omsportal/x12messagelist.png)

| Propriété | Description |
| --- | --- |
| Sender | Partenaire invité configuré dans les paramètres de réception ou partenaire hôte configuré dans les paramètres d’envoi pour un contrat AS2 |
| Receiver | Partenaire hôte configuré dans les paramètres de réception ou partenaire invité configuré dans les paramètres d’envoi pour un contrat AS2 |
| Application logique | Application logique dans laquelle les actions AS2 sont configurées |
| État | État du message X12. Réussite = réception ou envoi d’un message X12 valide, aucun accusé de réception fonctionnel (ack) configuré ; Réussite = réception ou envoi d’un message X12 valide, accusé de réception fonctionnel (ack) configuré, et accusé de réception fonctionnel envoyé ou reçu ; Échec = Réception ou envoi d’un message X12 non valide ; En attente = réception ou envoi d’un message X12 valide, accusé de réception fonctionnel (ack) configuré, en attente d’un accusé de réception fonctionnel (ack) |
| Ack | État de l’accusé de réception fonctionnel (997).  Accepté = réception ou envoi d’un accusé de réception fonctionnel (ack) positif ; Rejeté = réception ou envoi d’un accusé de réception fonctionnel négatif ; En attente = en attente d’un accusé de réception fonctionnel , aucun accusé reçu ; En attente = génération d’un accusé de réception fonctionnel, mais impossible de l’envoyer au partenaire |
| Direction | Direction du message X12 |
| ID de corrélation : | ID pour mettre en corrélation l’ensemble des déclencheurs et des actions au sein d’une application logique |
| Msg Type |  Type de message X12 EDI |
| ICN | Numéro de contrôle de l’échange du message X12 |
| TSCN | Numéro de contrôle de document automatisé du message X12 |
| Timestamp | Heure à laquelle l’action X12 traite le message |
| | |

4. Sélectionnez une ligne dans la liste de messages AS2 ou X12 pour accéder à la recherche dans les journaux.  La recherche de journal répertorie toutes les actions qui présentent le même **ID d’exécution**
![Sélectionner l’état du message](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

## <a name="queries-in-oms-portal"></a>Requêtes dans le portail OMS

Sur la page de recherche, vous pouvez créer une requête, puis lorsque vous effectuez une recherche, vous pouvez filtrer les résultats en utilisant des contrôles de facette.

### <a name="how-to-create-a-query"></a>Création d'une file d'attente

1. Dans la recherche de journaux, écrivez une requête et sélectionnez **Enregistrer**.  Vous trouverez [ici](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md) les étapes permettant d'écrire une requête ![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery.png)

2. La fenêtre **Enregistrer la recherche** s’ouvre.  Spécifiez un **nom**, une **catégorie**, puis cliquez sur **Enregistrer**   
![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery1.png)

3. Pour afficher la requête, sélectionnez **favoris**    
![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery3.png)

    ![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery4.png)

### <a name="how-to-use-a-saved-query"></a>Utilisation d'une requête enregistrée

* Dans la recherche de journaux, sélectionnez **favoris** pour afficher les requêtes enregistrées.  La sélection d'un des résultats de la requête donne ![Sélectionner la page d’accueil](media/logic-apps-track-b2b-messages-omsportal/logsearchaddquery5.png)


## <a name="next-steps"></a>Étapes suivantes
[Schéma de suivi personnalisé](logic-apps-track-integration-account-custom-tracking-schema.md "Learn about Custom Tracking Schema")   
[Schéma de suivi AS2](logic-apps-track-integration-account-as2-tracking-schemas.md "Learn about AS2 Tracking Schema")    
[Schéma de suivi X12](logic-apps-track-integration-account-x12-tracking-schema.md "Learn about X12 Tracking Schema")  
[En savoir plus sur Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Learn about Enterprise Integration Pack") 


<!--HONumber=Jan17_HO4-->


