---
title: "Guide de la facturation et de la gestion des coûts Azure | Microsoft Docs"
description: "Découvrez les meilleures pratiques et les premières choses à faire pour optimiser votre facture"
services: 
documentationcenter: 
author: jlian
manager: mattstee
editor: 
tags: billing
ms.assetid: 482191ac-147e-4eb6-9655-c40c13846672
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/20/2016
ms.author: jlian
translationtype: Human Translation
ms.sourcegitcommit: 249d08341311e48a93db8031439f0bc35162f823
ms.openlocfilehash: 28c70685cfa3da94cc0648cebae7ec296af60986


---
# <a name="getting-started-with-azure-billing-and-cost-management"></a>Prise en main de la facturation et de la gestion des coûts Azure

Lorsque vous vous inscrivez à Azure, vous avez plusieurs choses à faire pour avoir une meilleure idée de vos dépenses. Dans le portail Azure, vous pouvez consulter la répartition de vos coûts et votre taux d’avancement actuels. Vous pouvez également télécharger vos anciennes factures et vos anciens fichiers d’utilisation détaillée. Si vous souhaitez regrouper les coûts associés à des ressources utilisées pour différents projets ou équipes, tournez-vous vers le balisage des ressources. Si votre organisation dispose d’un système de création de rapports que vous préférez utiliser, regardez du côté des API de facturation. 

Si vous êtes un client Contrat Entreprise (EA), Fournisseur de solutions cloud (CSP) ou Azure Sponsorship, un grand nombre des fonctionnalités présentées dans cet article ne vous concernent pas. Vous disposez en effet d’un autre ensemble d’outils pour la gestion des coûts. Pour plus d’informations, consultez [Ressources supplémentaires pour les offres EA, CSP et Sponsorship](#other-offers).

Si vous êtes un client de l’offre d’essai gratuit, [Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/), Azure dans Open (AIO) ou BizSpark, renseignez-vous sur la [limite de dépense](#spending-limit) afin d’éviter que votre compte ne soit désactivé. 

## <a name="before-you-add-azure-services"></a>Avant d’ajouter des services Azure

### <a name="estimate-cost-online-using-the-pricing-calculator"></a>Estimez le coût en ligne à l’aide de la calculatrice de prix

Utilisez la [calculatrice de prix](https://azure.microsoft.com/pricing/calculator/) et la [calculatrice du coût total de possession](https://aka.ms/azure-tco-calculator) pour obtenir une estimation du coût mensuel du service qui vous intéresse. Par exemple, le coût d’une machine virtuelle A1 Windows en heures de calcul est estimé à&66;,96 USD par mois si vous la laissez s’exécuter en permanence :

![Capture d’écran de la calculatrice de prix montrant que le coût d’une machine virtuelle A1 Windows est estimé à&66;,96 USD par mois](./media/billing-getting-started/pricing-calc.PNG)

Pour plus d’informations, consultez le [FAQ sur la tarification](https://azure.microsoft.com/pricing/faq/). Si vous voulez parler directement à quelqu’un de notre équipe, appelez le 1-800-867-1389.

### <a name="check-your-subscription-and-access"></a>Vérifiez votre abonnement et votre accès

L’affichage des coûts nécessite un [accès de niveau abonnement](../active-directory/role-based-access-control-configure.md), mais seul l’administrateur de compte peut accéder au [Centre des comptes](https://account.windowsazure.com/Home/Index), modifier les informations de facturation et gérer les abonnements. L’administrateur de compte est la personne qui a effectué le processus d’inscription. Pour plus d’informations, consultez [Ajout ou modification de rôles d’administrateur Azure](../billing-add-change-azure-subscription-administrator.md).

Pour savoir si vous êtes l’administrateur de compte, accédez à l’[onglet Abonnements du portail Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) et examinez la liste des abonnements auxquels vous avez accès. Regardez sous **Mon rôle**. S’il est indiqué *Administrateur de compte*, vous disposez bien de tous les droits associés. S’il est indiqué autre chose, par exemple *Propriétaire*, vous ne disposez pas de privilèges complets.

![Capture d’écran de votre rôle dans la vue Abonnements du portail Azure](./media/billing-getting-started/sub-blade-view.PNG)

Si vous n’êtes pas l’administrateur de compte, quelqu’un vous a sans doute octroyé un accès partiel via le [contrôle d’accès en fonction du rôle Azure Active Directory](../active-directory/role-based-access-control-configure.md) (RBAC). Pour gérer les abonnements et modifier les informations de facturation, [identifiez l’administrateur de compte](../billing-subscription-transfer.md#whoisaa) et demandez-lui d’effectuer les tâches souhaitées ou de [vous transférer l’abonnement](../billing-subscription-transfer.md).

Si votre administrateur de compte ne fait plus partie de votre organisation et vous avez besoin de gérer la facturation, [contactez le support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). 

### <a name="a-namespending-limita-check-if-you-have-a-spending-limit-on"></a><a name="spending-limit"></a> Vérifiez si la limite de dépense est activée

Si votre abonnement utilise des crédits, la limite de dépense est activée pour vous par défaut. De cette manière, lorsque vous dépensez tous vos crédits, votre carte bancaire n’est pas facturée. Pour plus d’informations, consultez la [liste complète des offres Azure et les informations sur la disponibilité de la limite de dépense](https://azure.microsoft.com/support/legal/offer-details/).

Toutefois, si vous atteignez votre limite de dépense, vos services sont désactivés. Cela signifie que vos machines virtuelles sont libérées. Pour éviter toute interruption de service, vous devez désactiver la limite de dépense. Tout dépassement est facturé sur la carte de crédit enregistrée. 

Pour voir si la limite de dépense est activée, accédez à la [vue Abonnements du Centre des comptes](https://account.windowsazure.com/Subscriptions). Une bannière s’affiche si votre limite de dépense est activée :

![Capture d’écran montrant l’avertissement affiché dans le Centre des comptes lorsque la limite de dépense est activée](./media/billing-getting-started/spending-limit-banner.PNG)

Cliquez sur la bannière et suivez les invites pour supprimer la limite de dépense. Si vous n’avez pas entré d’informations de carte bancaire lors de votre inscription, vous devez les saisir pour supprimer la limite de dépense. Pour plus d’informations, consultez [Limite de dépense Azure : fonctionnement et activation ou désactivation](https://azure.microsoft.com/pricing/spending-limits/).

### <a name="set-up-billing-alerts"></a>Définition des alertes de facturation

Configurez des alertes de facturation pour recevoir des e-mails lorsque les coûts d’utilisation dépassent un certain montant. Si vous disposez de crédits mensuels, configurez des alertes pour être averti lorsqu’un montant spécifique a été utilisé. Pour plus d’informations, consultez [Configurer des alertes de facturation pour vos abonnements Microsoft Azure](../billing-set-up-alerts.md).

![Capture d’écran d’un e-mail d’alerte de facturation](./media/billing-getting-started/billing-alert.png)

> [!NOTE]
> Cette fonctionnalité étant encore en version préliminaire, il est recommandé de vérifier régulièrement l’utilisation.

Pour configurer votre première alerte, vous pouvez vous baser sur l’estimation du coût obtenue à l’aide de la calculatrice de prix.

### <a name="understand-limits-and-quotas-for-your-subscription"></a>Soyez au fait des limites et des quotas de votre abonnement

Chaque abonnement fait l’objet de limites par défaut en ce qui concerne le nombre de cœurs de processeur, l’adresse IP, etc. Soyez attentif à ces limites. Pour plus d’informations, consultez [Abonnement Azure et limites, quotas et contraintes de service](../azure-subscription-service-limits.md). Vous pouvez demander une augmentation de votre limite ou de votre quota en [contactant le support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="as-you-add-services"></a>Lorsque vous ajoutez des services

### <a name="review-the-estimated-cost-in-the-portal"></a>Vérifiez l’estimation du coût dans le portail

En général, lorsque vous ajoutez un service dans le portail Azure, une vue présentant une estimation similaire du coût par mois vous est proposée. Par exemple, lorsque vous choisissez la taille de votre machine virtuelle Windows, vous pouvez voir l’estimation du coût mensuel pour les heures de calcul :

![Exemple : le coût d’une machine virtuelle A1 Windows est estimé à&66;,96 USD par mois](./media/billing-getting-started/vm-size-cost.PNG)

### <a name="a-nametagsa-add-tags-to-your-resources-to-group-your-billing-data"></a><a name="tags"></a> Ajoutez des balises à vos ressources pour regrouper vos données de facturation

Vous pouvez utiliser des balises pour regrouper les données de facturation associées aux services pris en charge. Par exemple, si vous exécutez plusieurs machines virtuelles pour différentes équipes, vous pouvez utiliser des balises pour classer les coûts par centre de coût (RH, marketing, service financier) ou par environnement (production, préproduction, test). 

![Capture d’écran illustrant la configuration de balises dans le portail](./media/billing-getting-started/tags.PNG)

Les balises apparaissent dans les différentes vues des rapports de coûts. Par exemple, elles sont visibles dans votre [vue d’analyse des coûts](#costs) immédiatement et dans le [fichier .csv d’utilisation détaillée](#invoice-and-usage) après votre première période de facturation.

Pour plus d’informations, voir [Organisation des ressources Azure à l’aide de balises](../azure-resource-manager/resource-group-using-tags.md).

### <a name="consider-enabling-cost-cutting-features-like-auto-shutdown-for-vms"></a>Envisagez d’activer les fonctionnalités de réduction des coûts telles que l’arrêt automatique pour les machines virtuelles

Selon votre scénario, vous pouvez configurer l’arrêt automatique de vos machines virtuelles dans le portail Azure. Pour en savoir plus, consultez le billet de blog [Auto-shutdown for VMs using Azure Resource Manager](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) (Arrêt automatique des machines virtuelles à l’aide d’Azure Resource Manager).

![Capture d’écran de l’option d’arrêt automatique dans le portail](./media/billing-getting-started/auto-shutdown.PNG)

L’arrêt automatique n’a pas le même effet que l’arrêt à partir des options d’alimentation d’une machine virtuelle. L’arrêt automatique met un terme au fonctionnement de vos machines virtuelles et les libère pour éviter des frais d’utilisation supplémentaires. Pour plus d’informations, consultez les informations concernant les états des machines virtuelles dans le FAQ sur la facturation pour les [machines virtuelles Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) et les [machines Windows VMs](https://azure.microsoft.com/pricing/details/virtual-machines/windows/).

Pour bénéficier d’autres fonctionnalités de réduction des coûts pour vos environnements de développement et de test, consultez [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab/).

## <a name="a-namecost-reportinga-after-using-services-view-usage"></a><a name="cost-reporting"></a> Après avoir utilisé des services, affichez l’utilisation

### <a name="a-namecostsa-regularly-check-the-portal-for-cost-breakdown-and-burn-rate"></a><a name="costs"></a> Vérifiez régulièrement la répartition des coûts et le taux d’avancement dans le portail

Une fois que vos services sont actifs, vérifiez régulièrement combien ils vous coûtent. Vous pouvez consulter les dépenses et le taux d’avancement actuels dans le portail. 

1. Accédez au [panneau Abonnements du portail Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).

2. Sélectionnez l’abonnement à afficher. Il se peut qu’il n’y en ait qu’un seul.

3. Vous devriez voir la répartition des coûts et le taux d’avancement dans le panneau contextuel qui s’affiche. Il se peut que votre offre ne donne pas accès à ces données (un avertissement est alors affiché en haut). Après avoir ajouté un service, comptez un délai de 24 heures pour que les données soient remplies.
    
    ![Capture d’écran du taux d’avancement et de la répartition des coûts dans le portail Azure](./media/billing-getting-started/burn-rate.PNG)

4. Si vous le souhaitez, vous pouvez épingler la vue à votre tableau de bord.

    ![Capture d’écran de l’épinglage d’une vue au tableau de bord](./media/billing-getting-started/pin.PNG)

5. Cliquez sur **Analyse des coûts** dans la liste figurant sur la gauche pour afficher la répartition des coûts par ressource.

    ![Capture d’écran de la vue de l’analyse des coûts dans le portail Azure](./media/billing-getting-started/cost-analysis.PNG)

6. Vous pouvez filtrer les données en fonction de différentes propriétés : [balises](#tags), groupe de ressources, intervalle de temps, etc. Cliquez sur **Appliquer** pour confirmer les filtres et sur **Télécharger** pour exporter la vue vers un fichier de valeurs séparées par des virgules (.csv).

7. Cliquez sur une ressource pour afficher l’historique des dépenses et savoir combien elle vous a coûté chaque jour.

    ![Capture d’écran de la vue de l’historique des dépenses dans le portail Azure](./media/billing-getting-started/spend-history.PNG)

Nous vous recommandons de comparer les coûts affichés avec les estimations qui vous ont été données lors de la sélection des services. Si vous constatez une différence importante, vérifiez le plan de tarification (machine virtuelle A1 ou A0, par exemple) que vous avez sélectionné pour vos ressources. 

#### <a name="view-costs-for-all-your-subscriptions-in-the-billing-blade"></a>Affichez les coûts de tous vos abonnements dans le panneau Facturation

Si vous gérez plusieurs abonnements en qualité d’administrateur de compte, vous pouvez consulter le montant global facturé et la répartition des coûts pour tous vos abonnements dans le [panneau Facturation](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade). 

<!-- Add screenshots of multiple subs each with billed usage -->

### <a name="turn-on-and-check-out-azure-advisor-recommendations"></a>Activez et consultez les recommandations d’Azure Advisor

[Azure Advisor](../advisor/advisor-overview.md) est une fonctionnalité en version préliminaire qui vous permet de réduire les coûts en identifiant les ressources peu utilisées. Activez-la dans le portail Azure :

![Capture d’écran du bouton Azure Advisor dans le portail Azure](./media/billing-getting-started/advisor-button.PNG)

Vous pourrez alors accéder à des recommandations exploitables à partir de l’onglet **Coût** du tableau de bord d’Advisor :

![Exemple de recommandation en matière de coûts d’Advisor](./media/billing-getting-started/advisor-action.PNG)

Pour plus d’informations, consultez [Recommandations du conseiller en matière de coût](../advisor/advisor-cost-recommendations.md).

### <a name="a-nameinvoice-and-usagea-download-invoice-and-detail-usage-after-your-first-billing-period"></a><a name="invoice-and-usage"></a> Téléchargez la facture et l’utilisation détaillée après votre première période de facturation

Après votre première période de facturation, vous pouvez télécharger votre facture au format PDF et votre utilisation détaillée au format CSV. Ces fichiers vous permettent de comprendre ce qui vous est finalement facturé après application des taxes, des remises et des crédits. Si vous n’avez défini aucun mode de paiement pour votre abonnement, il se peut que ces fichiers ne soient pas disponibles. Pour plus d’informations, consultez [Comment télécharger votre facture Azure et vos données d’utilisation quotidienne](../billing-download-azure-invoice-daily-usage-date.md) et [Comprendre votre facture pour Microsoft Azure](/billing-understand-your-bill.md).

![Capture d’écran d’une facture .pdf](./media/billing-getting-started/invoice.png)

Les balises que vous avez définies précédemment apparaissent dans les fichiers .csv d’utilisation détaillée :

![Capture d’écran montrant les balises dans le fichier .csv d’utilisation](./media/billing-getting-started/csv.png)

### <a name="billing-api"></a>API de facturation

Utilisez nos API de facturation pour obtenir les données d’utilisation par programmation. En associant les API RateCard et Resource Usage, vous pouvez connaître l’utilisation qui vous est facturée. Pour plus d’informations, consultez [Obtenir une vue d’ensemble de votre consommation des ressources Microsoft Azure](../billing-usage-rate-card-overview.md).

## <a name="a-nameother-offersa-additional-resources-for-ea-csp-and-sponsorship"></a><a name="other-offers"></a> Ressources supplémentaires pour les offres EA, CSP et Sponsorship

Contactez votre responsable de compte ou votre partenaire Azure pour commencer.

| Offer | les ressources |
|-------------------------------|-----------------------------------------------------------------------------------|
| Contrat Entreprise (EA) | [Portail EA](https://ea.azure.com/) et [documents d’aide](https://ea.azure.com/helpdocs) |
| Fournisseur de solutions cloud (CSP) | Contactez votre fournisseur |
| Azure Sponsorship | [Portail Sponsorship](https://www.microsoftazuresponsorships.com/) |

Si vous êtes responsable informatique d’une grande organisation, nous vous recommandons de lire l’article [Structure d’entreprise Azure](../azure-resource-manager/resource-manager-subscription-governance.md) et le document [entreprise livre blanc IT](http://download.microsoft.com/download/F/F/F/FFF60E6C-DBA1-4214-BEFD-3130C340B138/Azure_Onboarding_Guide_for_IT_Organizations_EN_US.pdf) (téléchargement au format .pdf, disponible en anglais uniquement).



<!--HONumber=Jan17_HO4-->


