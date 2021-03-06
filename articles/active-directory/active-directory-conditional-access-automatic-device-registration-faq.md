---
title: "FAQ sur l’inscription d’appareils automatique | Microsoft Docs"
description: "FAQ sur l’inscription d’appareils automatique avec Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: cdc25576-37f2-4afb-a786-f59ba4c284c2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 918c2b096b9b6d935c8d5bc2588a978cdf8878cf
ms.openlocfilehash: 0c4f1489893f464eee1f76e94f2b35650f06e44e


---
# <a name="automatic-device-registration-faq"></a>FAQ sur l’inscription d’appareils automatique

**Q : J’ai enregistré récemment l’appareil. Pourquoi ne puis-je pas voir l’appareil sous mes informations d’utilisateur dans le portail Azure ?**

**R :** Les appareils Windows 10 qui sont joints à un domaine avec l’inscription d’appareils automatique ne s’affichent pas sous les informations UTILISATEUR.
Vous devez utiliser PowerShell pour afficher tous les appareils. 

Seuls les appareils suivants sont répertoriés sous les informations UTILISATEUR :

- Tous les appareils personnels qui ne sont pas joints à une entreprise 
- Tous les appareils non Windows 10 / Windows Server 2016 
- Tous les appareils non Windows 

---

**Q : Pourquoi ne puis-je pas voir tous les appareils inscrits auprès d’Azure Active Directory dans le portail Azure ?** 

**R :** Actuellement, il n’existe aucun moyen d’afficher tous les appareils inscrits dans le portail Azure. Vous pouvez utiliser Azure PowerShell pour rechercher tous les appareils. Pour plus d’informations, consultez l’applet de commande [Get-MsolDevice](https://docs.microsoft.com/en-us/powershell/msonline/v1/get-msoldevice).

--- 

**Q : Comment puis-je connaître l’état de l’inscription d’appareils du client ?**

**R :** L’état de l’inscription d’appareils dépend des facteurs suivants :

- du type d’appareil concerné ;
- de la manière dont il a été inscrit ; 
- de tous les détails qui lui sont associés. 
 

---

**Q : Pourquoi un appareil que j’ai supprimé dans le portail Azure ou à l’aide de Windows PowerShell est-il toujours répertorié comme inscrit ?**

**R :** Il s’agit du comportement par défaut. L’appareil n’aura pas accès aux ressources dans le cloud. Si vous souhaitez réinscrire l’appareil, vous devez effectuer une action manuelle sur celui-ci. 

Pour Windows 10 et Windows Server 2016 sur site AD et joints à un domaine :

1.  Ouvrez une invite de commandes en tant qu’administrateur.

2.  Entrez **dsregcmd.exe /leave**.

3.  Entrez **dsregcmd.exe**.

Pour les autres plateformes Windows sur site AD et jointes à un domaine :

1.  Ouvrez une invite de commandes en tant qu’administrateur.
2.  Saisissez `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /l"`.
3.  Saisissez `"%programFiles%\Microsoft Workplace Join\autoworkplace.exe /j"`.

---

**Q : Pourquoi le portail Azure affiche-t-il des entrées d’appareils dupliquées ?**

**R :**

-   Pour Windows 10 et Windows Server 2016, en cas de tentatives répétées visant à disjoindre et à joindre à nouveau le même appareil, des entrées dupliquées peuvent s’afficher. 

-   Si vous avez utilisé « Ajouter un compte professionnel ou scolaire », chaque utilisateur Windows qui utilise « Ajouter un compte professionnel ou scolaire » créera un nouvel enregistrement d’appareil avec le même nom d’appareil.

-   Les autres plateformes Windows sur site AD jointes à un domaine à l’aide de l’inscription automatique créeront un nouvel enregistrement d’appareil avec le même nom d’appareil pour chaque utilisateur du domaine qui se connecte à l’appareil. 

-   Une machine AADJ qui a été réinitialisée, réinstallée et jointe à nouveau avec le même nom s’affichera en tant que nouvel enregistrement avec le même nom d’appareil.

---

**Q : Pourquoi un utilisateur peut-il toujours accéder aux ressources à partir d’un appareil que j’ai désactivé dans le portail Azure ?**

**R :** Une opération de révocation peut prendre jusqu’à une heure pour être entièrement appliquée.

>[!Note] 
>Pour les appareils perdus, nous vous recommandons de réinitialiser l’appareil pour vous assurer que les utilisateurs ne puissent pas y accéder. Pour plus d’informations, consultez [Inscrire des appareils pour la gestion dans Intune](https://docs.microsoft.com/intune/deploy-use/enroll-devices-in-microsoft-intune). 


---

**Q : Pourquoi mes utilisateurs voient-ils s’afficher « Vous ne pouvez pas accéder à cet emplacement à partir d’ici » ?**

**R :** Si vous avez configuré certaines règles d’accès conditionnel pour exiger un état d’appareil spécifique et que l’appareil ne respecte pas les critères, les utilisateurs sont bloqués et ce message s’affiche. Veuillez évaluer les règles et vous assurer que l’appareil est en mesure de respecter les critères pour éviter l’affichage de ce message.

---


**Q : Je vois l’enregistrement d’appareil sous les informations UTILISATEUR dans le portail Azure, ainsi que l’état en tant qu’inscrit sur le client. Ma configuration est-elle correcte pour l’utilisation de l’accès conditionnel ?**

**R :** L’enregistrement d’appareil (deviceID) et l’état sur le portail Azure doivent correspondre au client et respecter les critères d’évaluation de l’accès conditionnel. Pour plus d’informations, consultez [Bien démarrer avec le service Azure Active Directory Device Registration](active-directory-conditional-access-device-registration-overview.md).

---

**Q : Pourquoi est-ce que je reçois le message « nom d’utilisateur ou mot de passe incorrect » pour un appareil que je viens juste de joindre à Azure AD ?**

**R :** Les raisons les plus courantes sont les suivantes :

1.  Vos informations d’identification ne sont plus valides.

2.  Votre ordinateur n’est pas en mesure de communiquer avec Azure Active Directory. Vérifiez les éventuels problèmes de connectivité réseau.

3.  Les conditions préalables pour la jonction à Azure AD n’ont pas été respectées. Veuillez vous assurer que vous avez respecté les étapes de la section [Extension des fonctionnalités du cloud aux appareils Windows 10 via Azure Active Directory Join](active-directory-azureadjoin-overview.md).  

4.  Les connexions fédérées nécessitent que votre serveur de fédération prenne en charge un point de terminaison WS-Trust actif. 

---

**Q : Pourquoi la boîte de dialogue « Désolé... une erreur s’est produite ! » s’affiche-t-elle lorsque j’essaye d’inscrire mon ordinateur ?**

**R :** Cela résulte de la configuration de l’inscription Azure Active Directory avec Intune. Pour plus d’informations, consultez [Configurer la gestion des appareils Windows](https://docs.microsoft.com/intune/deploy-use/set-up-windows-device-management-with-microsoft-intune#azure-active-directory-enrollment).  

---

**Q : Pourquoi ma tentative d’inscription d’un ordinateur a-t-elle échoué alors que je n’ai reçu aucune information d’erreur ?**

**R :** Une cause possible est que l’utilisateur est connecté à l’appareil à l’aide du compte administrateur intégré. Créez un compte local distinct avant d’utiliser Azure Active Directory Join pour terminer la configuration. 

---

**Q : Où puis-je trouver des informations de résolution des problèmes concernant l’inscription d’appareils automatique ?**

**R :** Pour obtenir des informations de résolution des problèmes, consultez :

1. [Résolution des problèmes de l’inscription automatique des ordinateurs joints au domaine Azure Active Directory pour Windows 10 et Windows Server 2016](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows.md)

2. [Résolution des problèmes de l’inscription automatique des ordinateurs joints au domaine Azure Active Directory pour les clients de bas niveau Windows](active-directory-conditional-access-automatic-device-registration-troubleshoot-windows-legacy.md)
 
---




<!--HONumber=Feb17_HO1-->


