---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API'leri | Microsoft Docs
description: Azure Cosmos DB API'leri arasında tutarlılık düzeyleri anlayın.
keywords: tutarlılık, azure cosmos db, azure, modelleri, mongodb, cassandra, grafik, tablo, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 22ddfbf86dacdd5d7f80d1fa7826d6ae11c4de45
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52680404"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API'leri

Azure Cosmos DB tarafından sunulan beş tutarlılık modeli yerel olarak Azure Cosmos DB SQL API tarafından desteklenir. Azure Cosmos DB kullandığınızda, SQL API'si varsayılandır. 

Azure Cosmos DB ayrıca kablo için yerel destek sağlar popüler veritabanları için protokolü ile uyumlu API'leri. Veritabanlarını, MongoDB, Apache Cassandra, Gremlin ve Azure tablo depolama alanı içerir. Bu veritabanları, tam olarak tanımlanmış tutarlılık modeli veya tutarlılık düzeyleri için SLA destekli garantileri sunmamaktadır. Bunlar genellikle yalnızca bir alt kümesini Azure Cosmos DB tarafından sunulan beş tutarlılık modeli sunar. SQL API, Gremlin API ve tablo API'si için Azure Cosmos DB hesabı yapılandırılmış varsayılan tutarlılık düzeyini kullanılır. 

Aşağıdaki bölümlerde OSS istemci sürücü tarafından istenen Apache Cassandra için veri tutarlılığı arasındaki eşlemeyi Göster 4.x ve MongoDB 3.4. Bu belge, ayrıca Apache Cassandra ve MongoDB için karşılık gelen Azure Cosmos DB tutarlılık düzeyleri gösterilmektedir.

## <a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Bu tablo, Azure Cosmos DB'de Apache Cassandra 4.x istemci varsayılan tutarlılık düzeyini arasında "okuma tutarlılığı" eşleme gösterir. Tabloda, çok bölgeli ve tek bölgeli dağıtımlar gösterilmiştir.

| **Apache Cassandra 4.x** | **Azure Cosmos DB (çok bölgeli)** | **Azure Cosmos DB (tek bölge)** |
| - | - | - |
| BİR İKİ ÜÇ | Tutarlı ön ek | Tutarlı ön ek |
| LOCAL_ONE | Tutarlı ön ek | Tutarlı ön ek |
| ÇEKİRDEK, TÜMÜ, SERİ | Sınırlanmış eskime durumu varsayılandır. Güçlü özel Önizleme aşamasındadır. | Güçlü |
| LOCAL_QUORUM | Sınırlanmış eskime durumu | Güçlü |
| LOCAL_SERIAL | Sınırlanmış eskime durumu | Güçlü |

## <a id="mongo-mapping"></a>MongoDB 3.4 ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Aşağıdaki tabloda, Azure Cosmos DB MongoDB 3.4 varsayılan tutarlılık düzeyini arasında "konuları okuyun" eşleme gösterilmektedir. Tabloda, çok bölgeli ve tek bölgeli dağıtımlar gösterilmiştir.

| **MongoDB 3.4** | **Azure Cosmos DB (çok bölgeli)** | **Azure Cosmos DB (tek bölge)** |
| - | - | - |
| Linearizable | Güçlü | Güçlü |
| Çoğunluk | Sınırlanmış eskime durumu | Güçlü |
| Yerel | Tutarlı ön ek | Tutarlı ön ek |

## <a name="next-steps"></a>Sonraki adımlar

Tutarlılık düzeyleri ve açık kaynak API'lerle Azure Cosmos DB API'leri arasındaki uyumluluk hakkında daha fazlasını okuyun. Aşağıdaki makalelere bakın:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Azure Cosmos DB MongoDB API'si tarafından desteklenen MongoDB özellikleri](mongodb-feature-support.md)
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)