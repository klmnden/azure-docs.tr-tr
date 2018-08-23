---
title: Bulut hizmeti projeme ne oldu? | Microsoft Docs
description: Visual Studio kullanarak bir Azure depolama hesabına bağlanma bağlı hizmetler sonra cloud services projesinde ne olacağı açıklanır
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e823bc16c500dfee44312a774d5e3bd6622e5fae
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42057142"
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Bulut Hizmetleri projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)?
## <a name="references-added"></a>Eklenen başvuruları
Azure depolama NuGet paketini Visual Studio projenize eklendi.  
Bu paket, aşağıdaki .NET başvuruları ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Eklenen Azure depolama bağlantı dizesi
Öğeleri seçili depolama hesabının bağlantı dizesini ve anahtarı ile oluşturulur. Aşağıdaki dosyalar için yapılan değişiklikler:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

