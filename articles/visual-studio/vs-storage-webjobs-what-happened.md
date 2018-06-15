---
title: My Web işi projesinin ne (Visual Studio Azure depolama bağlı hizmeti)? | Microsoft Docs
description: Visual Studio kullanarak bir depolama hesabına bağlanma Hizmetleri bağlandıktan sonra bir Azure Web işi projesi ne açıklar
services: storage
author: ghogen
manager: douge
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 12/02/2016
ms.author: ghogen
ms.openlocfilehash: 056ccd572a04a436ff53dda6ae967711c9f9903d
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
ms.locfileid: "31790808"
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

