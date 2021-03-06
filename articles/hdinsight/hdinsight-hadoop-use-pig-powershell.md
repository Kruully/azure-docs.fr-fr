---
title: Utilisation de Hadoop Pig avec PowerShell dans HDInsight | Microsoft Docs
description: "Découvrez comment soumettre des tâches Pig vers un cluster Hadoop sur HDInsight à l’aide d’Azure Powershell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/19/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 8b88db5bb2c8153953109fcd0511c22042bcf931
ms.openlocfilehash: bef30df14f6d00c4a7f5f5b3d59ec85b2e4fbe10


---
# <a name="run-pig-jobs-using-powershell"></a>Exécution de tâches Pig avec PowerShell
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ce document fournit un exemple d’utilisation d’Azure PowerShell pour soumettre des tâches Pig à un Hadoop sur un cluster HDInsight. Pig vous permet d’écrire des tâches MapReduce en utilisant un langage (Pig Latin) qui modélise les transformations de données, plutôt que de mapper et de réduire les fonctions.

> [!NOTE]
> Ce document ne fournit pas une description détaillée de ce que font les instructions Pig Latin utilisées dans les exemples. Pour plus d’informations sur le langage Pig Latin utilisé dans cet exemple, consultez la rubrique [Utilisation de Pig avec Hadoop sur HDInsight](hdinsight-use-pig.md).
> 
> 

## <a name="a-idprereqaprerequisites"></a><a id="prereq"></a>Configuration requise
Pour effectuer les étapes présentées dans cet article, vous avez besoin des éléments suivants :

* **Un cluster Azure HDInsight**

  > [!IMPORTANT]
  > Linux est le seul système d’exploitation utilisé sur HDInsight version 3.4 ou supérieure. Pour en savoir plus, consultez le paragraphe [Obsolescence de HDInsight sous Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

* **Un poste de travail sur lequel est installé Azure PowerShell**.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="a-idpowershellarun-pig-jobs-using-powershell"></a><a id="powershell"></a>Exécution de tâches Pig avec PowerShell
Azure PowerShell propose des *applets de commande* qui vous permettent d'exécuter à distance des tâches Pig sur HDInsight. En interne, cela est accompli en effectuant des appels REST à [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (anciennement nommé Templeton) exécuté sur le cluster HDInsight.

Les applets de commande suivantes sont utilisées lors de l’exécution des tâches Pig sur un cluster HDInsight à distance :

* **Login-AzureRmAccount**: authentifie Azure PowerShell sur votre abonnement Azure.
* **New-AzureRmHDInsightPigJobDefinition**: crée une *définition d’une tâche* à l’aide des instructions Pig Latin spécifiées.
* **Start-AzureRmHDInsightJob**: envoie la définition de la tâche à HDInsight, démarre la tâche et retourne un objet de *tâche* pouvant être utilisé pour vérifier le statut de la tâche.
* **Wait-AzureRmHDInsightJob**: utilise l’objet de la tâche pour vérifier le statut de la tâche. Il attend que la tâche soit terminée ou que le délai d’attente soit dépassé.
* **Get-AzureRmHDInsightJobOutput**: utilisé pour récupérer la sortie de la tâche.

Les étapes suivantes montrent comment utiliser ces cmdlets pour exécuter une tâche sur votre cluster HDInsight.

1. À l’aide d’un éditeur, enregistrez le code suivant sous **pigjob.ps1**.
   
        # Login to your Azure subscription
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Add-AzureRmAccount
        }

        # Get cluster info
        $clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
        $creds=Get-Credential -Message "Enter the login for the cluster"

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasb:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"

        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `
            -Arguments "-w"

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -HttpCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds


1. Ouvrez une invite de commandes Windows PowerShell. Accédez au répertoire du fichier **pigjob.ps1** , puis utilisez la commande suivante pour exécuter le script :
   
        .\pigjob.ps1
   
    Vous êtes d’abord invité à vous connecter à votre abonnement Azure. Ensuite, vous devez fournir le nom et le mot de passe du compte HTTPs/Admin pour le cluster HDInsight.

2. Une fois la tâche terminée, des informations similaires à celles présentées ci-dessous doivent s’afficher :
   
        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a name="a-idtroubleshootingatroubleshooting"></a><a id="troubleshooting"></a>Résolution des problèmes

Si aucune information n'est retournée lorsque la tâche est terminée, il se peut qu'une erreur soit survenue au cours du traitement. Pour afficher les informations d’erreur pour cette tâche, ajoutez la commande suivante à la fin du fichier **pigjob.ps1** , enregistrez-le et exécutez-le à nouveau.

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

Cela renvoie les informations écrites dans STDERR sur le serveur lors de l’exécution de la tâche et peut vous aider à déterminer pourquoi la tâche échoue.

## <a name="a-idsummaryasummary"></a><a id="summary"></a>Résumé
Comme vous pouvez le voir, Azure PowerShell offre un moyen facile d’exécuter des tâches Pig sur un cluster HDInsight, de surveiller l’état de la tâche et de récupérer la sortie.

## <a name="a-idnextstepsanext-steps"></a><a id="nextsteps"></a>Étapes suivantes
Pour obtenir des informations générales sur Pig dans HDInsight :

* [Utilisation de Pig avec Hadoop sur HDInsight](hdinsight-use-pig.md)

Pour plus d’informations sur d’autres méthodes de travail avec Hadoop sur HDInsight :

* [Utilisation de Hive avec Hadoop sur HDInsight](hdinsight-use-hive.md)
* [Utilisation de MapReduce avec Hadoop sur HDInsight](hdinsight-use-mapreduce.md)




<!--HONumber=Jan17_HO3-->


