---
title: "Tablo API için Azure Cosmos DB genel dağıtım Öğreticisi | Microsoft Docs"
description: "Tablo API kullanarak Azure Cosmos DB genel dağıtım Kurulum öğrenin."
services: cosmos-db
keywords: "genel dağıtım, tablo"
documentationcenter: 
author: mimig1
manager: jhubbard
editor: cgronlun
ms.assetid: 8b815047-2868-4b10-af1d-40a1af419a70
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/10/2017
ms.author: mimig
ms.openlocfilehash: e9cb4b5b886cec46c0483287460c720855867f38
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API kullanarak Azure Cosmos DB genel dağıtım ayarlama

Bu makalede, Kurulum Azure Cosmos DB genel dağıtım için Azure Portalı'nı kullanın ve ardından tablo API (Önizleme) kullanarak bağlanmak nasıl gösterir.

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * Genel dağıtım kullanarak yapılandırma [tablo API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API kullanarak bir tercih edilen bölge bağlanma

Anlamıyla yararlanabilmek için [genel dağıtım](distribute-data-globally.md), istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin sıralı tercih listesi belirtebilirsiniz. Bu ayar yapılabilir `TablePreferredLocations` Azure depolama SDK'sını Önizleme için uygulama yapılandırması yapılandırma değeri. Azure Cosmos DB hesabı yapılandırması, geçerli bölge kullanılabilirliği ve belirtilen tercih listesi bağlı olarak, en iyi endpoint yazma gerçekleştirmek ve okuma işlemleri için Azure depolama SDK tarafından seçilir.

`TablePreferredLocations` Okuma için tercih edilen (çok girişli) konumları, virgülle ayrılmış listesini içermelidir. Her bir istemci örnek bir alt kümesini Bu bölgeler düşük gecikme süresi okuma tercih edilen sırayı belirtebilirsiniz. Bölgeleri kullanma şeklinde adlandırılmalıdır kendi [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx), örneğin, `West US`.

İlk kullanılabilir bölgede tüm okuma gönderilir `TablePreferredLocations` listesi. İstek başarısız olursa, istemci listeyi sonraki bölgeyi başarısız ve benzeri.

SDK'sı yalnızca belirtilen bölgeler okuma dener `TablePreferredLocations`. Böylece, örneğin, veritabanı hesabı üç bölgelerde kullanılabilir, ancak istemci yalnızca iki için yazma olmayan bölgeleri belirtir `TablePreferredLocations`, okuma, yazma bölge, yük devretme durumunda bile dışında sunulacak sonra.

SDK, bölge geçerli tüm yazma işlemlerini yazma otomatik olarak gönderir.

Varsa `TablePreferredLocations` özellik ayarlanmamışsa, tüm istekleri geçerli yazma bölgesinden sunulacak.

```xml
    <appSettings>
      <add key="TablePreferredLocations" value="East US, West US, North Europe"/>           
    </appSettings>
```

Bu, bu öğreticinin tamamlanan kadar. Genel olarak çoğaltılmış hesabınızı tutarlılığını okuyarak yönetmek nasıl öğrenebilirsiniz [Azure Cosmos veritabanı tutarlılık düzeylerini](consistency-levels.md). Ve Azure Cosmos DB'de nasıl genel veritabanı çoğaltma hakkında daha fazla bilgi çalıştığı için bkz: [Azure Cosmos DB genel verilerle dağıtmak](distribute-data-globally.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * DocumentDB API'lerini kullanarak genel dağıtım yapılandırma

Artık Azure Cosmos DB yerel öykünücüsü kullanarak yerel olarak geliştirme konusunda bilgi almak için sonraki öğretici devam edebilirsiniz.

> [!div class="nextstepaction"]
> [Yerel olarak öykünücü ile geliştirme](local-emulator.md)