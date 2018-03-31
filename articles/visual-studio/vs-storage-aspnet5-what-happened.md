---
title: My ASP.NET 5 Proje ne (Visual Studio bağlı Hizmetleri) | Microsoft Docs
description: Visual Studio kullanarak bir Visual Studio ASP.NET 5 Proje Azure depolama hesabında bağlanma Hizmetleri bağlandıktan sonra ne olacağını açıklar
services: storage
documentationcenter: ''
author: ghogen
manager: douge
editor: ''
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 16da7e7a2d61cffb95d22a85669c8f91f28ae476
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
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

