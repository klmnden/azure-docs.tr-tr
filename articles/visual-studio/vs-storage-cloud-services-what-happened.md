---
title: My bulut hizmeti projesini ne oldu? | Microsoft Docs
description: Visual Studio kullanarak bir Azure depolama hesabına bağlanma Hizmetleri bağlandıktan sonra bulut Hizmetleri projede ne olacağını açıklar
services: storage
author: ghogen
manager: douge
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: e88e41edbd062f89e24915889f5b74660ec45513
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790687"
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

