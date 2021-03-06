---
title: "Didacticiel : Intégration d’Azure Active Directory avec Huddle | Microsoft Docs"
description: "Apprenez à utiliser Huddle avec Azure Active Directory pour activer l’authentification unique, l’approvisionnement automatique et bien plus encore."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8389ba4c-f5f8-4ede-b2f4-32eae844ceb0
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/29/2016
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 85d7789599cfb39ce0de5d52e0fe057482a49039


---
# <a name="tutorial-azure-active-directory-integration-with-huddle"></a>Didacticiel : Intégration d’Azure Active Directory avec Huddle
L’objectif de ce didacticiel est de montrer comment intégrer Azure et Huddle.  
Le scénario décrit dans ce didacticiel part du principe que vous disposez des éléments suivants :

* Un abonnement Azure valide
* Un abonnement Huddle pour lequel l’authentification unique est activée

À l’issue de ce didacticiel, les utilisateurs d’Azure AD que vous avez affectés à Huddle pourront s’authentifier de manière unique dans l’application sur votre site d’entreprise Huddle (connexion initiée par le fournisseur du service) ou en s’aidant de la [Présentation du volet d’accès](active-directory-saas-access-panel-introduction.md).

Le scénario décrit dans ce didacticiel se compose des blocs de construction suivants :

1. Activation de l’intégration d’application pour Huddle
2. Configuration de l'authentification unique
3. Configuration de l'approvisionnement des utilisateurs
4. Affectation d’utilisateurs

![Configurer l’authentification unique](./media/active-directory-saas-huddle-tutorial/IC787830.png "Configure Single Sign-On")

## <a name="enabling-the-application-integration-for-huddle"></a>Activation de l’intégration d’application pour Huddle
Cette section décrit l’activation de l’intégration d’application pour Huddle.

### <a name="to-enable-the-application-integration-for-huddle-perform-the-following-steps"></a>Pour activer l’intégration d’application pour Huddle, procédez comme suit :
1. Dans le volet de navigation gauche du portail Azure Classic, cliquez sur **Active Directory**.
   
   ![Active Directory](./media/active-directory-saas-huddle-tutorial/IC700993.png "Active Directory")
2. Dans la liste **Annuaire** , sélectionnez l'annuaire pour lequel vous voulez activer l'intégration d'annuaire.
3. Pour ouvrir la vue des applications, dans la vue d'annuaire, cliquez sur **Applications** dans le menu du haut.
   
   ![Applications](./media/active-directory-saas-huddle-tutorial/IC700994.png "Applications")
4. Cliquez sur **Ajouter** en bas de la page.
   
   ![Ajouter une application](./media/active-directory-saas-huddle-tutorial/IC749321.png "Add application")
5. Dans la boîte de dialogue **Que voulez-vous faire ?**, cliquez sur **Ajouter une application à partir de la galerie**.
   
   ![Ajouter une application à partir de la galerie](./media/active-directory-saas-huddle-tutorial/IC749322.png "Add an application from gallerry")
6. Dans la **zone de recherche**, entrez **Huddle**.
   
   ![Galerie d’applications](./media/active-directory-saas-huddle-tutorial/IC787831.png "Application Gallery")
7. Dans le volet des résultats, sélectionnez **Huddle**, puis cliquez sur **Terminer** pour ajouter l’application.
   
   ![Huddle](./media/active-directory-saas-huddle-tutorial/IC787832.png "Huddle")
   
   ## <a name="configuring-single-sign-on"></a>Configuration de l'authentification unique

Cette section explique comment permettre aux utilisateurs de s’authentifier sur Huddle avec leur compte Azure AD en utilisant la fédération basée sur le protocole SAML.

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a>Pour configurer l’authentification unique, procédez comme suit :
1. Sur la page d’intégration d’applications **Huddle** du portail Azure Classic, cliquez sur **Configurer l’authentification unique** pour ouvrir la boîte de dialogue **Configurer l’authentification unique**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-huddle-tutorial/IC787833.png "Configure Single Sign-On")
2. Dans la page **How would you like users to sign on to Huddle** (Comment voulez-vous que les utilisateurs se connectent à Huddle), sélectionnez **Authentification unique avec Microsoft Azure AD**, puis cliquez sur **Suivant**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-huddle-tutorial/IC787834.png "Configure Single Sign-On")
3. Dans la page **Configurer l’URL de l’application**, dans la zone de texte **URL de connexion de Huddle**, tapez l’URL de votre locataire Huddle selon le modèle suivant « *http://company.huddle.com* », puis cliquez sur **Suivant**.
   
   ![Configurer l’URL de l’application](./media/active-directory-saas-huddle-tutorial/IC787835.png "Configure App URL")
4. Dans la page **Configurer l’authentification unique sur Huddle** , procédez comme suit :
   
   ![Configurer l’authentification unique](./media/active-directory-saas-huddle-tutorial/IC787836.png "Configure Single Sign-On")
   
   1. Cliquez sur **Télécharger le certificat**, puis enregistrez le certificat sur votre ordinateur.
   2. Copiez la valeur **URL de l’émetteur**, la valeur **URL SSO SAML** et le certificat téléchargé, puis envoyez-les à l’équipe du support technique Huddle.
   
   > [!NOTE]
   > L’authentification unique doit être activée par l’équipe du support technique Huddle.
   > Vous recevrez une notification dès la configuration terminée.
   > 
   > 
5. Dans le portail Azure Classic, sélectionnez la confirmation de la configuration de l’authentification unique, puis cliquez sur **Terminer** pour fermer la boîte de dialogue **Configurer l’authentification unique**.
   
   ![Configurer l’authentification unique](./media/active-directory-saas-huddle-tutorial/IC787837.png "Configure Single Sign-On")
   
   ## <a name="configuring-user-provisioning"></a>Configuration de l'approvisionnement des utilisateurs

Pour se connecter à Huddle, les utilisateurs d’Azure AD doivent être approvisionnés dans Huddle.  
Dans le cas de Huddle, l’approvisionnement est une tâche manuelle.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Pour configurer l'approvisionnement des utilisateurs, procédez comme suit :
1. Connectez-vous à votre site d’entreprise **Huddle** en tant qu’administrateur.
2. Cliquez sur **Workspace**.
3. Cliquez sur **People \> Invite People**.
   
   ![Personnes](./media/active-directory-saas-huddle-tutorial/IC787838.png "People")
4. Dans la section **Create a new invitation** , procédez comme suit :
   
   ![Nouvelle invitation](./media/active-directory-saas-huddle-tutorial/IC787839.png "New Invitation")
   
   1. Dans la liste **Choose a team to invite people to join**, sélectionnez **team**.
   2. Indiquez le l’ **adresse de messagerie** d’un compte Azure Active Directory valide que vous souhaitez approvisionner dans la zone de texte correspondante.
   3. Cliquez sur **Invite**.
   
   > [!NOTE]
   > Le titulaire du compte Azure AD reçoit alors un message électronique contenant un lien pour confirmer le compte avant qu’il ne soit activé.
   > 
   > 

> [!NOTE]
> Vous pouvez utiliser n’importe quel autre outil ou API de création de compte d’utilisateur Huddle fourni par ce site pour approvisionner des comptes d’utilisateurs AAD.
> 
> 

## <a name="assigning-users"></a>Affectation d’utilisateurs
Pour tester votre configuration, vous devez autoriser les utilisateurs d’Azure AD concernés à accéder à votre application.

### <a name="to-assign-users-to-huddle-perform-the-following-steps"></a>Pour affecter des utilisateurs à Huddle, procédez comme suit :
1. Dans le portail Azure Classic, créez un compte de test.
2. Dans la page d’intégration d’application **Huddle**, cliquez sur **Affecter des utilisateurs**.
   
   ![Affecter des utilisateurs](./media/active-directory-saas-huddle-tutorial/IC787840.png "Assign Users")
3. Sélectionnez votre utilisateur de test, cliquez sur **Affecter**, puis sur **Oui** pour confirmer votre affectation.
   
   ![Oui](./media/active-directory-saas-huddle-tutorial/IC767830.png "Yes")

Si vous souhaitez tester vos paramètres d’authentification unique, ouvrez le volet d’accès. Pour plus d'informations sur le panneau d'accès, consultez [Présentation du panneau d’accès](active-directory-saas-access-panel-introduction.md).




<!--HONumber=Nov16_HO3-->


