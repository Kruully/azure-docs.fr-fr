---
title: "Connexion à une machine virtuelle Windows Server | Microsoft Docs"
description: "Découvrez comment vous connecter à une machine virtuelle Windows à l’aide du portail Azure et du modèle de déploiement Resource Manager."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: ef62b02e-bf35-468d-b4c3-71b63fe7f409
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/28/2016
ms.author: cynthn
translationtype: Human Translation
ms.sourcegitcommit: 5919c477502767a32c535ace4ae4e9dffae4f44b
ms.openlocfilehash: 9077746d8ffe968504f1dde90ed5f76dd1facc19


---
# <a name="how-to-connect-and-log-on-to-an-azure-virtual-machine-running-windows"></a>Connexion à une machine virtuelle Azure exécutant Windows
Vous utilisez le bouton **Connecter** dans le portail Azure pour démarrer une session Bureau à distance (RDP). Tout d’abord, connectez-vous à la machine virtuelle, puis ouvrez une session.

## <a name="connect-to-the-virtual-machine"></a>Connectez-vous à la machine virtuelle.
1. Si ce n’est pas déjà fait, connectez-vous au [portail Azure](https://portal.azure.com/).
2. Dans le menu hub, cliquez sur **Machines virtuelles**.
3. Sélectionnez la machine virtuelle dans la liste.
4. Dans le panneau de la machine virtuelle, cliquez sur **Se connecter**.
   
    ![Capture d'écran du portail Azure montrant comment se connecter à votre machine virtuelle.](./media/virtual-machines-windows-connect-logon/connect.png)
   
   > [!TIP]
   > Si le bouton **Connecter** du portail est grisé et si vous n’êtes pas connecté à Azure avec une connexion [Express Route](../expressroute/expressroute-introduction.md) ou [réseau privé virtuel de site à site](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md), vous devez créer votre machine virtuelle et lui attribuer une adresse IP publique pour pouvoir utiliser le protocole RDP. Pour en savoir plus sur les adresses IP publiques dans Azure, consultez [cet article](../virtual-network/virtual-network-ip-addresses-overview-arm.md).
   > 
   > 

## <a name="log-on-to-the-virtual-machine"></a>Connexion à la machine virtuelle
[!INCLUDE [virtual-machines-log-on-win-server](../../includes/virtual-machines-log-on-win-server.md)]

## <a name="next-steps"></a>Étapes suivantes
En cas de problème de connexion, consultez [Résolution des problèmes de connexion Bureau à distance](virtual-machines-windows-troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Cet article vous guide tout au long des opérations de diagnostic et de résolution des problèmes courants.




<!--HONumber=Nov16_HO2-->


