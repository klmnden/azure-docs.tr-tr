---
title: ASP.NET projeme ne oldu? | Microsoft Docs
description: Bağlı hizmetler Azure Storage Visual Studio kullanarak ASP.NET projeye ekledikten sonra ne olacağı açıklanır
services: storage
author: ghogen
manager: douge
ms.assetid: e1fe1b6d-4e3d-476d-8b2f-f7ade050515e
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: a6e05d706d54d63695861b03cd9de0e65ebdd8bb
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2018
ms.locfileid: "42061113"
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>ASP.NET projeme ne oldu (Visual Studio Azure Depolama'ya bağlı hizmet)?
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
Projenizin web.config dosyasında, bir öğenin seçili depolama hesabının bağlantı dizesini ve anahtarı ile oluşturuldu.

Daha fazla bilgi için [ASP.NET](http://www.asp.net).

