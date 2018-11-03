---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API'leri | Microsoft Docs
description: Azure Cosmos DB API'leri arasında tutarlılık düzeylerini anlama.
keywords: tutarlılık, azure cosmos db, azure, modelleri, mongodb, cassandra, grafik, tablo, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 974531cd5907e4f69e7d064125d3e51fa4974949
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50956392"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API'leri

Azure Cosmos DB tarafından sunulan beş tutarlılık modeli, Cosmos DB kullanıldığında varsayılan API. Cosmos DB SQL API tarafından yerel olarak desteklenir. Ek olarak SQL API'si, Cosmos DB kablo protokolünü uyumlu API'leri için yerel destek MongoDB, Apache Cassandra, Gremlin ve Azure tabloları gibi popüler veritabanları için de sağlar. Tam olarak ne teklif bu veritabanları ya da SLA destekli garantisi için tutarlılık düzeyleri tutarlılık modeli tanımlanır. Bu veritabanları genellikle yalnızca bir alt kümesini Cosmos DB tarafından sunulan beş tutarlılık modeli sağlar. SQL API, Gremlin API ve tablo API'si için Cosmos hesapta yapılandırdığınız varsayılan tutarlılık düzeyini kullanılır.

Aşağıdaki bölümlerde OSS istemci sürücü tarafından istenen Apache Cassandra için veri tutarlılığı arasındaki eşlemeyi Göster 4.x ve Cassandra API'si ve MongoDB API'si, sırasıyla kullanırken MongoDB 3.4 ve karşılık gelen Cosmos DB tutarlılık düzeyleri.

## <a id="cassandra-mapping"></a>Apache Cassandra ve Cosmos DB tutarlılık düzeyleri arasında eşleme

Aşağıdaki tabloda, çok bölgeli hem tek bölgeli dağıtımları için Cosmos DB'de Apache Cassandra 4.x istemci varsayılan tutarlılık düzeyini arasında "okuma tutarlılığı" eşleme gösterilmektedir.

| **Apache Cassandra 4.x** | **Cosmos DB (çok bölgeli)** | **Cosmos DB (tek bölge)** |
| - | - | - |
| BİR İKİ ÜÇ | Tutarlı Ön Ek | Tutarlı Ön Ek |
| LOCAL_ONE | Tutarlı Ön Ek | Tutarlı Ön Ek |
| ÇEKİRDEK, TÜMÜ, SERİ | Sınırlanmış eskime durumu (varsayılan) veya güçlü (özel önizleme içinde) | Güçlü |
| LOCAL_QUORUM | Sınırlanmış Eskime Durumu | Güçlü |
| LOCAL_SERIAL | Sınırlanmış Eskime Durumu | Güçlü |

## <a id="mongo-mapping"></a>MongoDB 3.4 ve Cosmos DB tutarlılık düzeyleri arasında eşleme

Aşağıdaki tabloda, çok bölgeli hem tek bölgeli dağıtımları için Cosmos DB MongoDB 3.4 varsayılan tutarlılık düzeyini arasında "konuları okuyun" eşleme gösterir.

| **MongoDB 3.4** | **Cosmos DB (çok bölgeli)** | **Cosmos DB (tek bölge)** |
| - | - | - |
| Linearizable | Güçlü | Güçlü |
| Çoğunluk | Sınırlanmış Eskime Durumu | Güçlü |
| Yerel | Tutarlı Ön Ek | Tutarlı Ön Ek |

## <a name="next-steps"></a>Sonraki adımlar

Tutarlılık düzeyleri ve Cosmos DB API'leri için aşağıdaki makalelere göz atın açık kaynaklı API'leri ile uyumluluğu hakkında daha fazla bilgi:

* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Cosmos DB MongoDB API'si tarafından desteklenen MongoDB özellikleri](mongodb-feature-support.md)
* [Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)