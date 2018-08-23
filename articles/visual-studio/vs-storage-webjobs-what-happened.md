---
title: WebJob projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)? | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlanma bağlı hizmetler sonra bir Azure WebJob proje ne olduğunu açıklar.
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: cf8b7849a603d5c1304846ab243478bb6afd5c18
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057445"
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>WebJob projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)?
## <a name="references-added"></a>Eklenen başvuruları
Azure depolama NuGet paketi için eklendi veya Visual Studio projenize güncelleştirildi.  
Bu paket, aşağıdaki .NET başvuruları ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Eklenen Azure depolama bağlantı dizesi
Projenizin App.config dosyasında **AzureWebJobsStorage** ve **AzureWebJobsDashboard** girişleri, seçili depolama hesabının bağlantı dizesini ve anahtarı ile güncelleştirildi.

Daha fazla bilgi için [Azure WebJobs belgeleri kaynakları](http://go.microsoft.com/fwlink/?linkid=390226).

