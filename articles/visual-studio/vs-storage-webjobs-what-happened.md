---
title: "My Web işi projesinin ne (Visual Studio Azure depolama bağlı hizmeti)? | Microsoft Belgeleri"
description: "Visual Studio kullanarak bir depolama hesabına bağlanma Hizmetleri bağlandıktan sonra bir Azure Web işi projesi ne açıklar"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 8891685a99c5ba366b74af0a21396d4a5e499835
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>My Web işi projesinin ne (Visual Studio Azure depolama bağlı hizmeti)?
## <a name="references-added"></a>Eklenen başvuruları
Azure depolama NuGet paketini eklenecek veya Visual Studio projenizde güncelleştirildi.  
Bu paket aşağıdaki .NET başvuru ekler:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Azure eklenen depolama alanı için bağlantı dizesi
Projenizin App.config dosyasında **AzureWebJobsStorage** ve **AzureWebJobsDashboard** girişleri, seçilen depolama hesabı bağlantı dizesi ve anahtarı ile güncelleştirildi.

Daha fazla bilgi için bkz: [Azure Web işleri belge kaynakları](http://go.microsoft.com/fwlink/?linkid=390226).

