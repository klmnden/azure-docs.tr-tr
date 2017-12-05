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
ms.date: 11/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: a1682ef88760da39d33fac2cc9f34e0e66ea19fb
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="how-to-setup-azure-cosmos-db-global-distribution-using-the-table-api"></a>Tablo API kullanarak Azure Cosmos DB genel dağıtım ayarlama

Bu makalede aşağıdaki görevleri içerir: 

> [!div class="checklist"]
> * Azure portalını kullanarak genel dağıtım yapılandırma
> * Genel dağıtım kullanarak yapılandırma [tablo API](table-introduction.md)

[!INCLUDE [cosmos-db-tutorial-global-distribution-portal](../../includes/cosmos-db-tutorial-global-distribution-portal.md)]


## <a name="connecting-to-a-preferred-region-using-the-table-api"></a>Tablo API kullanarak bir tercih edilen bölge bağlanma

Anlamıyla yararlanabilmek için [genel dağıtım](distribute-data-globally.md), istemci uygulamaları, belge işlemlerini gerçekleştirmek için kullanılacak bölgelerin sıralı tercih listesi belirtebilirsiniz. Bu ayar yapılabilir `TablePreferredLocations` Azure Cosmos DB tablo API SDK'sı app.config yapılandırma değeri. Azure Cosmos DB tablo API SDK'sı ile iletişim kurmak için en iyi uç noktası hesabı yapılandırması, geçerli bölge kullanılabilirliği ve sağlanan tercih listesi göre seçer.

`TablePreferredLocations` Okuma için tercih edilen (çok girişli) konumları, virgülle ayrılmış listesini içermelidir. Her bir istemci örnek bir alt kümesini Bu bölgeler düşük gecikme süresi okuma tercih edilen sırayı belirtebilirsiniz. Bölgeleri kullanma şeklinde adlandırılmalıdır kendi [görünen adları](https://msdn.microsoft.com/library/azure/gg441293.aspx), örneğin, `West US`.

Tüm okuma ilk kullanılabilir bölgede gönderilen `TablePreferredLocations` listesi. İstek başarısız olursa, istemci listeyi sonraki bölgeyi başarısız ve benzeri.

SDK belirtilen bölgelerinden okumaya çalışır `TablePreferredLocations`. Böylece, örneğin, veritabanı hesabı üç bölgelerde kullanılabilir, ancak istemci yalnızca iki için yazma olmayan bölgeleri belirtir `TablePreferredLocations`, okuma, yazma bölge, yük devretme durumunda bile dışında sunulacak sonra.

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
> * Azure Cosmos DB tablo API'lerini kullanarak genel dağıtım yapılandırma

