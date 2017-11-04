---
title: My bulut hizmeti projesini ne oldu? | Microsoft Belgeleri
description: "Visual Studio kullanarak bir Azure depolama hesabına bağlanma Hizmetleri bağlandıktan sonra bulut Hizmetleri projede ne olacağını açıklar"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 4c9de56f6daf07097c0f593db37d14dce3bce05f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-happened-to-my-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Bulut Hizmetleri proje için ne (Visual Studio Azure depolama bağlı hizmeti)?
## <a name="references-added"></a>Eklenen başvuruları
Azure depolama NuGet paketini Visual Studio projenizi eklendi.  
Bu paket aşağıdaki .NET başvuru ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Azure eklenen depolama alanı için bağlantı dizesi
Öğeleri seçilen depolama hesabı bağlantı dizesi ve anahtar ile oluşturulmuş. Aşağıdaki dosyalar için yapılan değişiklikler:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

