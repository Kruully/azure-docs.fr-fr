---
title: "Prise en main d’Azure IoT Hub (.NET) | Microsoft Docs"
description: "Envoi de messages appareil-vers-cloud d’un appareil vers un Azure IoT Hub à l’aide des kits de développement logiciel Azure IoT pour .NET. Vous créez une application de périphérique simulé pour envoyer des messages, une application de service pour inscrire votre appareil dans le registre des identités et une application de service pour lire les messages appareil-vers-cloud à partir du IoT Hub."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: f40604ff-8fd6-4969-9e99-8574fbcf036c
ms.service: iot-hub
ms.devlang: dotnet
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 2e4220bedcb0091342fd9386669d523d4da04d1c
ms.openlocfilehash: 90ca7089b2ce6a541f6890c6d50e912f2127ff85


---
# <a name="get-started-with-azure-iot-hub-net"></a>Mise en route d’Azure IoT Hub (.NET)
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

À la fin de ce didacticiel, vous disposerez de trois applications console .NET :

* **CreateDeviceIdentity**, qui crée une identité d’appareil et une clé de sécurité associée pour connecter votre application de périphérique simulé ;
* **ReadDeviceToCloudMessages**, qui affiche les données de télémétrie envoyées par votre application de périphérique simulé ;
* **SimulatedDevice**, qui se connecte à votre IoT hub avec l’identité d’appareil créée précédemment et envoie un message de télémétrie chaque seconde en utilisant le protocole MQTT.

> [!NOTE]
> Pour plus d’informations sur les kits de développement logiciel Azure IoT que vous pouvez utiliser pour générer les deux applications qui s’exécutent sur les appareils et sur le serveur de solution principal, voir l’article [Kits de développement logiciel (SDK) Azure IoT][lnk-hub-sdks].
> 
> 

Pour réaliser ce didacticiel, vous avez besoin des éléments suivants :

* Microsoft Visual Studio 2015.
* Un compte Azure actif. (Si vous ne possédez pas de compte, vous pouvez créer un [compte gratuit][lnk-free-trial] en quelques minutes.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Votre IoT Hub est maintenant créé et vous connaissez le nom d’hôte et la chaîne de connexion à IoT Hub dont vous avez besoin pour terminer ce qu’il reste du didacticiel.

## <a name="create-a-device-identity"></a>Création d’une identité d’appareil
Dans cette section, vous allez créer une application console .NET qui crée une identité d’appareil dans le registre d’identités de votre IoT Hub. Un appareil ne peut pas se connecter à IoT Hub, à moins de posséder une entrée dans le registre des identités. Reportez-vous à la section Registre d’identité du [Guide du développeur IoT Hub][lnk-devguide-identity] pour plus d’informations. Lorsque vous exécutez cette application de console, elle génère un ID d’appareil et une clé uniques que votre appareil peut utiliser pour s’identifier pour lorsqu’il envoie des messages appareil-à-cloud à IoT Hub.

1. Dans Visual Studio, ajoutez un projet Visual C# Bureau classique Windows à la solution actuelle en utilisant le modèle de projet **Application Console** . Assurez-vous que la version du .NET Framework est définie sur 4.5.1 ou supérieur. Nommez le projet **CreateDeviceIdentity**.
   
    ![Nouveau projet Visual C# Bureau classique Windows][10]
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **CreateDeviceIdentity**, puis cliquez sur **Gérer les packages Nuget**.
3. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**, puis recherchez **microsoft.azure.devices**. Cliquez ensuite sur **Installer** pour installer le package **Microsoft.Azure.Devices**, puis acceptez les conditions d’utilisation. Cette procédure lance le téléchargement et l’installation et ajoute une référence au [package Azure IoT Service SDK NuGet][lnk-nuget-service-sdk] et ses dépendances.
   
    ![Fenêtre du gestionnaire de package NuGet][11]
4. Ajoutez les instructions `using` suivantes en haut du fichier **Program.cs** :
   
        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;
5. Ajoutez les champs suivants à la classe **Program** . Remplacez la valeur d’espace réservé par la chaîne de connexion IoT Hub pour le hub créé dans la section précédente.
   
        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";
6. Ajoutez la méthode suivante à la classe **Program** :
   
        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }
   
    Cette méthode crée une identité d’appareil avec l’ID **myFirstDevice**. (si cet ID d’appareil existe déjà dans le registre d’identité, le code récupère simplement les informations d’appareil existantes.) L’application affiche ensuite la clé primaire pour cette identité. Vous utilisez cette clé dans l’application de périphérique simulé pour vous connecter à votre IoT Hub.
7. Enfin, ajoutez les lignes suivantes à la méthode **Main** :
   
        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();
8. Exécutez cette application et notez la clé de l’appareil.
   
    ![Clé d’appareil générée par l’application][12]

> [!NOTE]
> Le registre des identités IoT Hub stocke uniquement les identités des appareils pour permettre un accès sécurisé à IoT Hub. Il stocke les ID des appareils et les clés à utiliser comme informations d’identification de sécurité et un indicateur activé/désactivé que vous pouvez utiliser pour désactiver l’accès pour un appareil individuel. Si votre application a besoin de stocker d’autres métadonnées spécifiques aux appareils, elle doit utiliser un magasin spécifique aux applications. Pour plus d’informations, reportez-vous au [Guide du développeur IoT Hub][lnk-devguide-identity].
> 
> 

## <a name="receive-device-to-cloud-messages"></a>Recevoir des messages appareil-à-cloud
Dans cette section, vous allez créer une application console .NET qui lit les messages des appareils vers le cloud dans l’IoT Hub. Un IoT Hub expose un point de terminaison compatible avec [Azure Event Hubs][lnk-event-hubs-overview] pour vous permettre de lire les messages appareil-à-cloud. Pour simplifier les choses, ce didacticiel crée un lecteur de base qui ne convient pas dans le cas d’un déploiement à débit élevé. Pour découvrir comment traiter les messages appareil-à-cloud à grande échelle, reportez-vous au didacticiel [Traitement des messages appareil-à-cloud][lnk-process-d2c-tutorial]. Pour plus d’informations sur la façon de traiter les messages à partir des concentrateurs d’événements, reportez-vous au didacticiel [Prise en main des concentrateurs d’événements][lnk-eventhubs-tutorial]. (Ce didacticiel s’applique aux points de terminaison compatibles Event Hub IoT Hub).

> [!NOTE]
> Le point de terminaison compatible Event Hub pour lire des messages de l’appareil vers le cloud utilise toujours le protocole AMQP.
> 
> 

1. Dans Visual Studio, ajoutez un projet Visual C# Bureau classique Windows à la solution actuelle en utilisant le modèle de projet **Application Console** . Assurez-vous que la version du .NET Framework est définie sur 4.5.1 ou supérieur. Nommez le projet **ReadDeviceToCloudMessages**.
   
    ![Nouveau projet Visual C# Bureau classique Windows][10]
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **ReadDeviceToCloudMessages**, puis cliquez sur **Gérer les packages Nuget**.
3. Dans la fenêtre **Gestionnaire de package Nuget**, recherchez **WindowsAzure.ServiceBus**, cliquez sur **Installer** et acceptez les conditions d’utilisation. Cette procédure lance le téléchargement, l’installation et ajoute une référence à [Azure Service Bus][lnk-servicebus-nuget] avec toutes ses dépendances. Ce package permet à l'application de se connecter au point de terminaison compatible Event Hubs sur votre IoT hub.
4. Ajoutez les instructions `using` suivantes en haut du fichier **Program.cs** :
   
        using Microsoft.ServiceBus.Messaging;
        using System.Threading;
5. Ajoutez les champs suivants à la classe **Program** . Remplacez la valeur d’espace réservé par la chaîne de connexion IoT Hub pour le hub créé dans la section Créer un hub IoT.
   
        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;
6. Ajoutez la méthode suivante à la classe **Program** :
   
        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;
   
                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }
   
    Cette méthode utilise une instance **EventHubReceiver** pour recevoir des messages à partir de toutes les partitions de réception Appareil vers cloud IoT Hub. Notez la manière dont vous transmettez un paramètre `DateTime.Now` lorsque vous créez l’objet **EventHubReceiver** pour qu’il reçoive uniquement les messages envoyés après son démarrage. Dans un environnement de test, ce filtre permet de voir l’ensemble des messages en cours. Dans un environnement de production, votre code doit faire en sorte de traiter tous les messages. Pour plus d’informations, voir le didacticiel [Traitement des messages appareil-à-cloud IoT Hub][lnk-process-d2c-tutorial].
7. Enfin, ajoutez les lignes suivantes à la méthode **Main** :
   
        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);
   
        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;
   
        CancellationTokenSource cts = new CancellationTokenSource();
   
        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };
   
        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>Création d’une application de périphérique simulé
Dans cette section, vous allez créer une application console .NET qui simule un appareil envoyant des messages des appareils vers le cloud à un IoT Hub.

1. Dans Visual Studio, ajoutez un projet Visual C# Bureau classique Windows à la solution actuelle en utilisant le modèle de projet **Application Console** . Assurez-vous que la version du .NET Framework est définie sur 4.5.1 ou supérieur. Nommez le projet **SimulatedDevice**.
   
    ![Nouveau projet Visual C# Bureau classique Windows][10]
2. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet **SimulatedDevice**, puis cliquez sur **Gérer les packages NuGet**.
3. Dans la fenêtre **Gestionnaire de package NuGet**, cliquez sur **Parcourir**, puis recherchez **Microsoft.Azure.Devices.Client**. Cliquez ensuite sur **Installer** pour installer le package **Microsoft.Azure.Devices.Client**, puis acceptez les conditions d’utilisation. Cette procédure lance le téléchargement et l’installation et ajoute une référence au [package Azure IoT Device SDK NuGet][lnk-device-nuget] et ses dépendances.
4. Ajoutez l'instruction `using` suivante en haut du fichier **Program.cs** :
   
        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;
5. Ajoutez les champs suivants à la classe **Program** . Remplacez les valeurs des espaces réservés par le nom d’hôte IoT Hub figurant dans la section Créer un IoT Hub et par la clé d’appareil figurant dans la section Créer une identité d’appareil.
   
        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";
6. Ajoutez la méthode suivante à la classe **Program** :
   
        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();
   
            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;
   
                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));
   
                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);
   
                Task.Delay(1000).Wait();
            }
        }
   
    Cette méthode envoie un nouveau message Appareil vers cloud toutes les secondes. Le message contient un objet JSON sérialisé avec l’ID de l’appareil et un nombre généré de manière aléatoire pour simuler un anémomètre.
7. Enfin, ajoutez les lignes suivantes à la méthode **Main** :
   
        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey), TransportType.Mqtt);
   
        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();
   
   Par défaut, la méthode de création **Create** crée une instance **DeviceClient** qui utilise le protocole AMQP pour communiquer avec IoT Hub. Pour utiliser le protocole MQTT ou HTTP, remplacez la méthode **Create** afin de pouvoir spécifier le protocole. Si vous utilisez le protocole HTTP, vous devez également ajouter le package NuGet **Microsoft.AspNet.WebApi.Client** à votre projet de manière à inclure l’espace de noms **System.Net.Http.Formatting**.

Ce didacticiel vous accompagne tout au long des étapes de création d'une application d'appareil IoT Hub simulé. Vous pouvez également utiliser l’extension [Connected Service for Azure IoT Hub][lnk-connected-service] de Visual Studio pour ajouter le code nécessaire à votre application d’appareil.

> [!NOTE]
> Pour simplifier les choses, ce didacticiel n’implémente aucune stratégie de nouvelle tentative. Dans le code de production, vous devez mettre en œuvre des stratégies de nouvelle tentative (par exemple, une interruption exponentielle), comme indiqué dans l’article MSDN [Gestion des erreurs temporaires][lnk-transient-faults].
> 
> 

## <a name="run-the-apps"></a>Exécuter les applications
Vous êtes maintenant prêt à exécuter les applications.

1. Dans Visual Studio, dans l’explorateur de solutions, cliquez avec le bouton droit sur votre solution, puis sur **Définir les projets de démarrage**. Sélectionnez **Plusieurs projets de démarrage**, puis **Démarrer** en tant qu’action pour les projets **ReadDeviceToCloudMessages** et **SimulatedDevice**.
   
    ![Propriétés de démarrage de projet][41]
2. Appuyez sur **F5** pour démarrer les deux applications. La sortie de console à partir de l’application **SimulatedDevice** affiche les messages envoyés par votre application de périphérique simulé à votre IoT Hub. La sortie de console à partir de l’application **ProcessDeviceToCloudMessages** affiche les messages reçus par votre IoT Hub.
   
    ![Sortie de console à partir d’applications][42]
3. La vignette **Utilisation** du [portail Azure][lnk-portal] indique le nombre de messages envoyés au IoT Hub :
   
    ![Vignette Utilisation du portail Azure][43]

## <a name="next-steps"></a>Étapes suivantes
Dans ce didacticiel, vous avez configuré un IoT Hub dans le portail Azure, puis créé une identité d’appareil dans le registre d’identités de l’IoT Hub. Vous avez utilisé cette identité d’appareil pour permettre à l’appareil simulé d’envoyer des messages appareil-à-cloud à l’IoT Hub. Vous avez également créé une application qui affiche les messages reçus par l’IoT Hub. 

Pour continuer la prise en main de IoT Hub et explorer les autres scénarios IoT, consultez les articles suivants :

* [Connexion de votre appareil][lnk-connect-device]
* [Prise en main de la gestion d’appareils][lnk-device-management]
* [Prise en main du Kit de développement logiciel (SDK) IoT Gateway][lnk-gateway-SDK]

Pour découvrir comment étendre votre solution IoT et traiter les messages appareil-à-cloud à grande échelle, consultez le didacticiel [Traitement des messages appareil-à-cloud][lnk-process-d2c-tutorial].

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Dec16_HO3-->


