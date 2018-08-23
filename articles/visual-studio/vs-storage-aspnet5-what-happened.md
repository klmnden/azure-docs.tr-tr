---
title: ASP.NET 5 projeme ne oldu (Visual Studio bağlı hizmetler) | Microsoft Docs
description: Visual Studio kullanarak bir ASP.NET 5 içeren Visual Studio projesi içinde bir Azure depolama hesabına bağlanma bağlı hizmetler sonra ne olacağı açıklanır
services: storage
author: ghogen
manager: douge
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 0e38e570b5ecb2a19af943e0dde96e2ed66f3507
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42056181"
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>ASP.NET 5 projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)?
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

Ayrıca, NuGet paketi **Microsoft.Framework.Configuration.Json** eklendi.

## <a name="connection-string-for-azure-storage-added"></a>Eklenen Azure depolama bağlantı dizesi
Projenizin config.json dosyası bir öğenin seçili depolama hesabının bağlantı dizesini ve anahtarı ile oluşturuldu.

Daha fazla bilgi için [ASP.NET 5](http://www.asp.net/vnext).

