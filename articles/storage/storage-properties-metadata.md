---
title: "Configuration et récupération des propriétés et des métadonnées pour les objets dans Azure Storage | Microsoft Docs"
description: "Stockez des métadonnées personnalisées sur des objets dans Azure Storage, et définissez et récupérez les propriétés système."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 036f9006-273e-400b-844b-3329045e9e1f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: marsma
translationtype: Human Translation
ms.sourcegitcommit: 3868d36948342739eb78b013bb4b466df4381b4f
ms.openlocfilehash: 7c1ca950c3ab1b8ffb754a74597d45b82777838c


---
# <a name="set-and-retrieve-properties-and-metadata"></a>Configuration et récupération des propriétés et des métadonnées
## <a name="overview"></a>Vue d'ensemble
Objets dans les propriétés du système de support de stockage Azure et des métadonnées définies par l’utilisateur, en plus des données qu’ils contiennent :

* **Les propriétés système :** propriétés existant sur chaque ressource de stockage. Certaines d'entre elles peuvent être lues ou configurées, alors que d'autres sont en lecture seule. En arrière-plan, certaines propriétés système correspondent à certains en-têtes HTTP standard. La bibliothèque cliente de stockage Azure gère ces en-têtes pour vous.
* **Les métadonnées définies par l’utilisateur.** il s’agit de métadonnées que vous spécifiez pour une ressource donnée, sous la forme d’une paire nom-valeur. Vous pouvez utiliser les métadonnées pour stocker des valeurs supplémentaires avec une ressource de stockage. Ces valeurs sont pour votre usage personnel et n’ont aucun impact sur le comportement de la ressource.

La récupération des valeurs des propriétés et des métadonnées d’une ressource se déroule en deux étapes. Pour pouvoir lire ces valeurs, vous devez les récupérer explicitement en appelant la méthode **FetchAttributes** .

> [!IMPORTANT]
> Les valeurs des propriétés et des métadonnées d’une ressource de stockage ne sont pas renseignées, sauf si vous appelez l’une des méthodes **FetchAttributes** .
>
> Vous recevrez une erreur `400 Bad Request` si des paires nom/valeur contiennent des caractères non-ASCII. Les paires nom/valeur de métadonnées sont des en-têtes HTTP valides, et doivent donc respecter toutes les restrictions régissant les en-têtes HTTP. Par conséquent, il est recommandé d’utiliser l’encodage URL ou l’encodage Base64 pour les noms et valeurs contenant des caractères non-ASCII.
>

## <a name="setting-and-retrieving-properties"></a>Configuration et récupération des propriétés
Pour récupérer des valeurs de propriétés, appelez la méthode **FetchAttributes** sur votre objet blob ou votre conteneur pour remplir les propriétés, puis lisez les valeurs.

Pour configurer les propriétés d’un objet blob, indiquez la valeur de propriété, puis appelez la méthode **SetProperties** .

L’exemple de code suivant crée un conteneur et un objet blob, et écrit certaines des valeurs des propriétés dans une fenêtre de console :

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

//Create the service client object for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Retrieve a reference to a container.
CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

// Create the container if it does not already exist.
container.CreateIfNotExists();

// Fetch container properties and write out their values.
container.FetchAttributes();
Console.WriteLine("Properties for container {0}", container.StorageUri.PrimaryUri.ToString());
Console.WriteLine("LastModifiedUTC: {0}", container.Properties.LastModified.ToString());
Console.WriteLine("ETag: {0}", container.Properties.ETag);
Console.WriteLine();
```

## <a name="setting-and-retrieving-metadata"></a>Configuration et récupération des métadonnées
Vous pouvez indiquer des métadonnées sous la forme de paires nom-valeur sur une ressource d’objet blob ou de conteneur. Pour configurer des métadonnées, ajoutez des paires nom-valeur à la collection **Metadata** de la ressource, puis appelez la méthode **SetMetadata** pour enregistrer les valeurs sur le service.

> [!NOTE]
> Le nom de vos métadonnées doit respecter la convention d’affectation de noms pour les identificateurs C#.
>
>

L’exemple de code suivant définit les métadonnées d’un conteneur. Une valeur est définie à l’aide de la méthode **Add** de la collection. L’autre valeur est définie à l’aide d’une syntaxe implicite clé/valeur. Les deux sont valides.

```csharp
public static void AddContainerMetadata(CloudBlobContainer container)
{
    //Add some metadata to the container.
    container.Metadata.Add("docType", "textDocuments");
    container.Metadata["category"] = "guidance";

    //Set the container's metadata.
    container.SetMetadata();
}
```

Pour récupérer des métadonnées, appelez la méthode **FetchAttributes** sur votre objet blob ou votre conteneur pour remplir la collection **Metadata**, puis lisez les valeurs, comme indiqué dans l’exemple suivant.

```csharp
public static void ListContainerMetadata(CloudBlobContainer container)
{
    //Fetch container attributes in order to populate the container's properties and metadata.
    container.FetchAttributes();

    //Enumerate the container's metadata.
    Console.WriteLine("Container metadata:");
    foreach (var metadataItem in container.Metadata)
    {
        Console.WriteLine("\tKey: {0}", metadataItem.Key);
        Console.WriteLine("\tValue: {0}", metadataItem.Value);
    }
}
```

## <a name="see-also"></a>Voir aussi
* [Références sur la bibliothèque cliente Azure Storage pour .NET](http://msdn.microsoft.com/library/azure/wa_storage_30_reference_home.aspx)
* [Package de la bibliothèque cliente Azure Storage pour .NET](https://www.nuget.org/packages/WindowsAzure.Storage/)




<!--HONumber=Feb17_HO3-->


