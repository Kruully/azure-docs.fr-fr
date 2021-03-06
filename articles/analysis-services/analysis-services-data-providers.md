---
title: "Fournisseurs de données requis pour les connexions clientes à Azure Analysis Services | Microsoft Docs"
description: "Décrit les fournisseurs de données requis pour les clients qui se connectent à Azure Analysis Services"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/20/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: f24024821f522bf89c0d9baf32a427af4636c890
ms.openlocfilehash: 41e2556437004699991363096147379d0ffa967c


---

# <a name="data-providers-for-connecting-to-azure-analysis-services"></a>Fournisseurs de données pour la connexion à Azure Analysis Services

Les fournisseurs de données, ou bibliothèques clientes, sont nécessaires pour que les applications clientes se connectent aux serveurs Analysis Services. 

Analysis Services utilise trois fournisseurs de données. ADOMD.NET et Analysis Services Management Objects (AMO) sont des fournisseurs de données managés. Le fournisseur OLE DB Analysis Services (MSOLAP DLL) est un fournisseur de données en mode natif. En général, les trois fournisseurs sont installés en même temps. Azure Analysis Services nécessite les dernières versions des fournisseurs de données. 

Les applications clientes Microsoft telles que Power BI Desktop et Excel installent les trois fournisseurs de données. Toutefois, selon la version d’Excel, ou selon que des versions plus récentes d’Excel et de Power BI Desktop soient ou non mises à jour tous les mois, il se peut que les fournisseurs installés ne puissent pas être mis à jour vers les dernières versions requises par Azure Analysis Service. Il en va de même pour les applications personnalisées ou d’autres interfaces telles que AsCmd, TOM, ADOMD.NET. Ces applications nécessitent l’installation manuelle des fournisseurs. Les fournisseurs de données pour l’installation manuelle sont inclus dans les packs de fonctionnalités SQL Server sous forme de packages distribuables. Toutefois, comme ils sont liés à la version de SQL Server, il se peut qu’ils ne soient pas à jour.  

Les fournisseurs de données pour les connexions clientes sont différents des fournisseurs de données requis pour connecter un serveur Azure Analysis Services à une source de données. Pour plus d’informations sur les connexions aux sources de données, consultez [Connexions de source de données](analysis-services-datasource.md).
 
## <a name="download-the-latest-data-providers"></a>Télécharger les dernières versions des fournisseurs de données  

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Étapes suivantes
Les données les plus récentes sont installées et votre application cliente est prête à se connecter à un serveur. Pour en savoir plus sur la connexion, consultez la page sur l’[obtention des données à partir d’Azure Analysis Services](analysis-services-connect.md).



<!--HONumber=Feb17_HO1-->


