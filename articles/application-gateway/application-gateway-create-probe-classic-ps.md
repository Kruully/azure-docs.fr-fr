---
title: "Créer une sonde personnalisée - Passerelle Azure Application Gateway - PowerShell classic | Microsoft Docs"
description: "Apprendre à créer une sonde personnalisée pour Application Gateway en utilisant PowerShell dans le modèle de déploiement classique"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 338a7be1-835c-48e9-a072-95662dc30f5e
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 4787a382b837b71a28c45211a26aa512e8fb177e


---
# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>Créer une sonde personnalisée pour Azure Application Gateway (classique) en utilisant PowerShell

> [!div class="op_single_selector"]
> * [Portail Azure](application-gateway-create-probe-portal.md)
> * [Commandes PowerShell pour Azure Resource Manager](application-gateway-create-probe-ps.md)
> * [Azure Classic PowerShell](application-gateway-create-probe-classic-ps.md)


[!INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

> [!IMPORTANT]
> Azure dispose de deux modèles de déploiement différents pour créer et utiliser des ressources : [le déploiement Resource Manager et le déploiement classique](../azure-resource-manager/resource-manager-deployment-model.md). Cet article traite du modèle de déploiement classique. Pour la plupart des nouveaux déploiements, Microsoft recommande d’utiliser le modèle Resource Manager. Découvrez comment [effectuer ces étapes à l’aide du modèle Resource Manager](application-gateway-create-probe-ps.md).

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>Créer une passerelle Application Gateway

Pour créer une passerelle d’application :

1. Créez une ressource de passerelle d’application.
2. Créez un fichier XML de configuration ou un objet de configuration.
3. Validez la configuration dans la ressource de passerelle d’application nouvellement créée.

### <a name="create-an-application-gateway-resource"></a>Créer une ressource de passerelle d’application

Pour créer la passerelle, utilisez l’applet de commande `New-AzureApplicationGateway` en remplaçant les valeurs par les vôtres. La facturation de la passerelle ne démarre pas à ce stade. La facturation commence à une étape ultérieure, lorsque la passerelle a démarré correctement.

L’exemple suivant illustre la création d’une nouvelle passerelle d’application avec un réseau virtuel appelé « testvnet1 » et un sous-réseau appelé « subnet-1 ».

```powershell
New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")
```

Pour valider la création de la passerelle, vous pouvez utiliser l’applet de commande `Get-AzureApplicationGateway`.

```powershell
Get-AzureApplicationGateway AppGwTest
```

> [!NOTE]
> La valeur par défaut pour *InstanceCount* est 2, avec une valeur maximale de 10. La valeur par défaut du paramètre *GatewaySize* est Medium. Vous avez le choix entre Small, Medium et Large.
> 
> 

Les paramètres *VirtualIPs* et *DnsName* sont sans valeur, car la passerelle n’a pas encore démarré. Ces valeurs seront créées une fois la passerelle en cours d’exécution.

## <a name="configure-an-application-gateway"></a>Configurer une passerelle d’application

Vous pouvez configurer la passerelle d’application à l’aide d’un objet de configuration ou de XML.

## <a name="configure-an-application-gateway-by-using-xml"></a>Configurer une passerelle d’application à l’aide de XML

Dans l’exemple ci-dessous, vous allez utiliser un fichier XML pour configurer tous les paramètres de la passerelle d’application et les valider dans la ressource de passerelle d’application.  

### <a name="step-1"></a>Étape 1 :

Copiez le texte suivant dans le Bloc-notes.

```xml
<ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
<FrontendIPConfigurations>
    <FrontendIPConfiguration>
        <Name>fip1</Name>
        <Type>Private</Type>
    </FrontendIPConfiguration>
</FrontendIPConfigurations>
<FrontendPorts>
    <FrontendPort>
        <Name>port1</Name>
        <Port>80</Port>
    </FrontendPort>
</FrontendPorts>
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
    </Probes>
    <BackendAddressPools>
    <BackendAddressPool>
        <Name>pool1</Name>
        <IPAddresses>
            <IPAddress>1.1.1.1</IPAddress>
            <IPAddress>2.2.2.2</IPAddress>
        </IPAddresses>
    </BackendAddressPool>
</BackendAddressPools>
<BackendHttpSettingsList>
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
</BackendHttpSettingsList>
<HttpListeners>
    <HttpListener>
        <Name>listener1</Name>
        <FrontendIP>fip1</FrontendIP>
    <FrontendPort>port1</FrontendPort>
        <Protocol>Http</Protocol>
    </HttpListener>
</HttpListeners>
<HttpLoadBalancingRules>
    <HttpLoadBalancingRule>
        <Name>lbrule1</Name>
        <Type>basic</Type>
        <BackendHttpSettings>setting1</BackendHttpSettings>
        <Listener>listener1</Listener>
        <BackendAddressPool>pool1</BackendAddressPool>
    </HttpLoadBalancingRule>
</HttpLoadBalancingRules>
</ApplicationGatewayConfiguration>
```

Modifiez les valeurs entre parenthèses pour les éléments de configuration. Enregistrez le fichier avec l’extension .xml.

L’exemple suivant montre comment utiliser un fichier de configuration pour configurer la passerelle d’application en vue d’équilibrer la charge du trafic HTTP sur le port public 80 et d’orienter le trafic réseau vers le port 80 principal entre deux adresses IP en utilisant une sonde personnalisée.

> [!IMPORTANT]
> L’élément de protocole Http ou Https est sensible à la casse.

Un nouvel élément de configuration \<Probe\> est ajouté pour configurer les sondes personnalisées.

Les paramètres de configuration sont :

* **Nom** : nom de référence de la sonde personnalisée.
* **Protocole** : protocole utilisé (les valeurs possibles sont HTTP ou HTTPS).
* **Hôte** et **Chemin** : chemin complet de l’URL qui est appelé par la passerelle d’application pour déterminer l’intégrité de l’instance. Par exemple : avec un site web http://contoso.com/, la sonde personnalisée peut être configurée pour « http://contoso.com/path/custompath.htm » afin que les contrôles de sonde renvoient une réponse HTTP réussie.
* **Intervalle** : configure les vérifications d’intervalle de sonde en secondes.
* **Délai d’expiration** : définit le délai d’expiration d’un contrôle de réponse HTTP.
* **Seuil de défaillance sur le plan de l’intégrité** : le nombre d’échecs de réponses HTTP nécessaires pour marquer l’instance de serveur principal comme *défectueuse*.

Le nom de la sonde est référencé dans la configuration \<BackendHttpSettings\> pour affecter le pool principal qui va utiliser les paramètres de sonde personnalisée.

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>Ajouter une configuration de sonde personnalisée à une passerelle d’application existante

La modification de la configuration actuelle d’une passerelle d’application se fait en trois étapes : obtenez le fichier de configuration XML actuel, modifiez-le de façon à avoir une sonde personnalisée et configurez la passerelle d’application avec les nouveaux paramètres XML.

### <a name="step-1"></a>Étape 1

Obtenir le fichier XML à l’aide de `Get-AzureApplicationGatewayConfig`. L’applet de commande exporte le fichier XML de configuration, afin d’être modifié pour y ajouter un paramètre de sonde.

```powershell
Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"
```

### <a name="step-2"></a>Étape 2 :

Ouvrez le fichier XML dans un éditeur de texte. Ajoutez une section `<probe>` après `<frontendport>`.

```xml
<Probes>
    <Probe>
        <Name>Probe01</Name>
        <Protocol>Http</Protocol>
        <Host>contoso.com</Host>
        <Path>/path/custompath.htm</Path>
        <Interval>15</Interval>
        <Timeout>15</Timeout>
        <UnhealthyThreshold>5</UnhealthyThreshold>
    </Probe>
</Probes>
```

Dans la section backendHttpSettings du fichier XML, ajoutez le nom de la sonde comme dans l’exemple suivant :

```xml
    <BackendHttpSettings>
        <Name>setting1</Name>
        <Port>80</Port>
        <Protocol>Http</Protocol>
        <CookieBasedAffinity>Enabled</CookieBasedAffinity>
        <RequestTimeout>120</RequestTimeout>
        <Probe>Probe01</Probe>
    </BackendHttpSettings>
```

Enregistrez le fichier XML.

### <a name="step-3"></a>Étape 3

Mettez à jour la configuration de la passerelle d’application avec le nouveau fichier XML avec `Set-AzureApplicationGatewayConfig`. Cette applet de commande met à jour votre passerelle d’application avec cette nouvelle configuration.

```powershell
Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"
```

## <a name="next-steps"></a>Étapes suivantes

Si vous voulez configurer le déchargement SSL (Secure Sockets Layer), consultez [Configuration d’une passerelle Application Gateway pour le déchargement SSL](application-gateway-ssl.md).

Si vous voulez configurer une passerelle Application Gateway à utiliser avec l’équilibreur de charge interne, consultez [Création d’une passerelle Application Gateway avec un équilibrage de charge interne (ILB)](application-gateway-ilb.md).




<!--HONumber=Jan17_HO4-->


