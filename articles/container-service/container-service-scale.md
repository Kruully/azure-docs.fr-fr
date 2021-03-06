---
title: "Mise à l’échelle d’un cluster Azure Container Service | Microsoft Docs"
description: "Procédure de mise à l’échelle de votre cluster Azure Container Service à l’aide de l’interface de ligne de commande Azure ou du portail Azure."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: Docker, conteneurs, micro-services, Mesos, Azure
ms.assetid: 4a567474-f9a2-4172-bf86-7522aa4d4d80
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/10/2017
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: cb3fd28659eb09dfb74496d2aa526736d223631a
ms.openlocfilehash: d1571aa6191111c46c43b3a424cea415091adfc9


---
# <a name="scale-an-azure-container-service-cluster"></a>Mise à l’échelle d’un cluster Azure Container Service
Après le [déploiement d’un cluster Azure Container Service](container-service-deployment.md), vous devez peut-être modifier le nombre de nœuds de l’agent. Vous risquez par exemple d’avoir besoin de davantage d’agents afin de pouvoir exécuter davantage d’instances ou d’applications de conteneur. 

Vous pouvez modifier le nombre de nœuds de l’agent dans le cluster à l’aide du portail Azure ou d’Azure CLI 2.0 (version préliminaire). La CLI Azure 2.0 (version préliminaire) est la [CLI nouvelle génération](/cli/azure/old-and-new-clis) pour le modèle de déploiement Resource Manager.

> [!NOTE]
> Actuellement, la mise à l’échelle de nœuds d’agent dans un cluster Kubernetes du service de conteneur n’est pas prise en charge.


## <a name="scale-with-the-azure-portal"></a>Mise à l’échelle avec le portail Azure

1. Dans le [portail Azure](https://portal.azure.com), recherchez les **services de conteneur**, puis cliquez sur le service de conteneur que vous souhaitez modifier.
2. Dans le panneau **Service de conteneur**, cliquez sur **Agents**.
3. Dans **Nombre de machines virtuelles**, entrez le nombre souhaité de nœuds d’agent.

    ![Mise à l’échelle d’un pool dans le portail](./media/container-service-scale/container-service-scale-portal.png)

4. Pour enregistrer la configuration, cliquez sur **Enregistrer**.



## <a name="scale-with-the-azure-cli-20-preview"></a>Mise à l’échelle avec la CLI Azure 2.0 (version préliminaire)

Assurez-vous d’avoir [installé](/cli/azure/install-az-cli2) la dernière version de la CLI Azure 2.0 (version préliminaire) et d’être connecté à un compte Azure (`az login`).


### <a name="see-the-current-agent-count"></a>Affichage du nombre actuel d’agents
Pour afficher le nombre d’agents actuellement dans le cluster, exécutez la commande `az acs show`. Elle permet d’afficher la configuration du cluster. Par exemple, la commande suivante affiche la configuration du service de conteneur nommé `containerservice-myACSName` dans le groupe de ressources `myResourceGroup` :

```azurecli
az acs show -g myResourceGroup -n containerservice-myACSName
```

La commande renvoie le nombre d’agents dans la valeur `Count` sous `AgentPoolProfiles`.


### <a name="use-the-az-acs-scale-command"></a>Utilisation de la commande az acs scale
Pour modifier le nombre de nœuds de l’agent, exécutez la commande `az acs scale` et indiquez le **groupe de ressources**, le **nom du service de conteneur** et le **nombre de nouveaux agents**. Si vous utilisez un nombre inférieur ou supérieur, vous descendez ou montez en puissance, respectivement.

Par exemple, pour modifier le nombre d’agents dans le précédent cluster en 10, tapez la commande suivante :

```azurecli
azure acs scale -g myResourceGroup -n containerservice-myACSName --new-agent-count 10
```

La CLI Azure 2.0 (version préliminaire) renvoie une chaîne JSON représentant la nouvelle configuration du service de conteneur, avec notamment le nouveau nombre d’agents.

Pour plus d’options de commande, exécutez `az acs scale --help`.


## <a name="scaling-considerations"></a>Mise à l'échelle - Éléments à prendre en compte


* Le nombre de nœuds de l’agent doit être compris entre 1 et 100 (inclus). 

* Votre quota de cœurs peut limiter le nombre de nœuds de l’agent dans un cluster.

* Les opérations de mise à l’échelle de nœud de l’agent sont appliquées à un groupe de machines virtuelles Azure identiques qui contient le pool de l’agent. Dans un cluster de contrôleur de domaine/système d’exploitation, seuls les nœuds de l’agent dans le pool privé sont mis à l’échelle par les opérations indiquées dans cet article.

* Selon l’orchestrateur que vous déployez dans votre cluster, vous pouvez mettre à l’échelle séparément le nombre d’instances d’un conteneur en cours d’exécution sur le cluster. Par exemple, dans un cluster de contrôleur de domaine/système d’exploitation, utilisez l’[interface utilisateur Marathon](container-service-mesos-marathon-ui.md) pour modifier le nombre d’instances d’une application conteneur.

* Actuellement, la mise à l’échelle automatique de nœuds d’agent dans un cluster du service de conteneur n’est pas prise en charge.





## <a name="next-steps"></a>Étapes suivantes
* Consultez d’[autres exemples](container-service-create-acs-cluster-cli.md) d’utilisation des commandes de la CLI Azure 2.0 (version préliminaire) avec Azure Container Service.
* En savoir plus sur les [pools d’agents contrôleur de domaine/système d’exploitation](container-service-dcos-agents.md) d’Azure Container Service.




<!--HONumber=Jan17_HO2-->


