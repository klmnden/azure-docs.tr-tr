---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API’leri
description: Azure Cosmos DB API'leri arasında tutarlılık düzeylerini anlama.
author: rimman
ms.author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/22/2019
ms.reviewer: sngun
ms.openlocfilehash: 1129152c1823fbffb3d6c9ec918d7b8cb4426bbd
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66235621"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API’leri

Azure Cosmos DB, hat üzeri için yerel destek sağlar popüler veritabanları için protokolü ile uyumlu API'leri. Bunlar, MongoDB, Apache Cassandra, Gremlin ve Azure tablo depolama alanı içerir. Bu veritabanları, tam olarak tanımlanmış tutarlılık modeli veya tutarlılık düzeyleri için SLA destekli garanti sağlamaz. Bunlar genellikle yalnızca bir alt kümesini Azure Cosmos DB tarafından sunulan beş tutarlılık modeli sunar. 

SQL API, Gremlin API ve tablo API'sini kullanarak Azure Cosmos hesabında yapılandırılmış varsayılan tutarlılık düzeyini kullanılır. 

Cassandra API veya Azure Cosmos DB'nin MongoDB kullanarak, MongoDB, Apache Cassandra ile sırasıyla sunulan tutarlılık düzeyleri, daha güçlü tutarlılık ve dayanıklılık Garantisi ile tam bir set uygulamaları alma. Bu belge, Apache Cassandra ve MongoDB tutarlılık düzeyleri için karşılık gelen Azure Cosmos DB tutarlılık düzeyleri gösterir.


## <a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

AzureCosmos DB, Apache Cassandra yerel olarak tam olarak tanımlanmış tutarlılık garantileri sağlamaz.  Bunun yerine, Apache Cassandra yazma tutarlılık düzeyi ve yüksek kullanılabilirlik, tutarlılık ve gecikme süresini bileşim etkinleştirmek için bir salt okunur tutarlılık düzeyi sağlar. Azure Cosmos DB'nin Cassandra API'si kullanıldığında: 

* Apache Cassandra yazma tutarlılık düzeyi, Azure Cosmos hesabınızda yapılandırılmış varsayılan tutarlılık düzeyini eşleştirilir. 

* Azure Cosmos DB Cassandra istemci sürücüsünü bir okuma isteği üzerinde dinamik olarak yapılandırılmış bir Azure Cosmos DB tutarlılık düzeyi tarafından belirtilen okuma tutarlılık düzeyi dinamik olarak eşler. 

Aşağıdaki tabloda, nasıl yerel Cassandra tutarlılık düzeyleri için Azure Cosmos DB tutarlılık düzeyleri Cassandra API'si kullanılırken eşlendiğine gösterilmektedir:  

[![Cassandra tutarlılık modeli eşleme](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png)](./media/consistency-levels-across-apis/consistency-model-mapping-cassandra.png#lightbox)

## <a id="mongo-mapping"></a>MongoDB ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Azure Cosmos DB, tam olarak tanımlanmış tutarlılık garantileri yerel MongoDB sağlamaz. Bunun yerine, yerel MongoDB aşağıdaki tutarlılık garantileri yapılandırmak kullanıcılara: Okuma işlemleri için birincil veya ikincil çoğaltmaları, istediğiniz tutarlılık düzeyi elde etmek için doğrudan isMaster yönergesi - yazma önemli ve okuma önemli. 

Azure Cosmos DB'nin MongoDB kullanarak, MongoDB sürücü yazma bölgenizi birincil çoğaltma olarak değerlendirir ve tüm bölgelere çoğaltma okunur. Birincil çoğaltması olarak Azure Cosmos hesabınızla ilişkili hangi bölge seçebilirsiniz. 

MongoDB için Azure Cosmos DB'nin API'sini kullanırken:

* Yazma sorunu, Azure Cosmos hesabınızda yapılandırılmış varsayılan tutarlılık düzeyini eşleştirilir.
 
* Azure Cosmos DB MongoDB istemci sürücüsü üzerinde okuma isteği dinamik olarak yapılandırılmış olan Azure Cosmos DB tutarlılık düzeyleri birine tarafından belirtilen okuma sorunu dinamik olarak eşler. 

* Belirli bir bölge yazılabilir ilk bölgeye bölgede yaparak "Ana" olarak Azure Cosmos hesabınızla ilişkili açıklama ekleyebilirsiniz. 

Aşağıdaki tabloda, nasıl yerel MongoDB yazma/kaygıları okuma gösterilmektedir. Azure Cosmos DB'nin MongoDB kullanarak, Azure Cosmos tutarlılık düzeylerine eşlenir:

[![MongoDB tutarlılık modeli eşleme](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png)](./media/consistency-levels-across-apis/consistency-model-mapping-mongodb.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Tutarlılık düzeyleri ve açık kaynak API'lerle Azure Cosmos DB API'leri arasındaki uyumluluk hakkında daha fazlasını okuyun. Aşağıdaki makalelere bakın:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [MongoDB için Azure Cosmos DB API tarafından desteklenen MongoDB özellikleri](mongodb-feature-support.md)
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)