---
title: My ASP.NET 5 Proje ne (Visual Studio bağlı Hizmetleri) | Microsoft Docs
description: Visual Studio kullanarak bir Visual Studio ASP.NET 5 Proje Azure depolama hesabında bağlanma Hizmetleri bağlandıktan sonra ne olacağını açıklar
services: storage
author: ghogen
manager: douge
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: f99ba4b6c954ae9faa87b9604c06e94c56e4f631
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>My ASP.NET 5 Proje ne (Visual Studio Azure depolama bağlı Hizmetleri)?
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

Ayrıca, NuGet paketi **Microsoft.Framework.Configuration.Json** eklendi.

## <a name="connection-string-for-azure-storage-added"></a>Azure eklenen depolama alanı için bağlantı dizesi
Projeniz config.json dosyasında bir öğeyi seçilen depolama hesabı bağlantı dizesi ve anahtarı ile oluşturuldu.

Daha fazla bilgi için bkz: [ASP.NET 5](http://www.asp.net/vnext).

