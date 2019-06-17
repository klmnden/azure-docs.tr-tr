---
title: WebJob projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)? | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlanma bağlı hizmetler sonra bir Azure WebJob proje içinde ne olduğunu açıklar.
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
ms.openlocfilehash: fa152d8b88254a35d00b91537bf1001ea1130e57
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60722634"
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

Daha fazla bilgi için [Azure WebJobs belgeleri kaynakları](https://go.microsoft.com/fwlink/?linkid=390226).

