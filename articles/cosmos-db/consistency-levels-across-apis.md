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
ms.openlocfilehash: ed08b90b9e216ee8713bfe445e98144bf2ba02d4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244195"
---
# <a name="consistency-levels-and-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Cosmos DB API'leri

Beş tutarlılık modeli, varsayılan API Cosmos DB kullanıldığında SQL API'si tarafından yerel olarak desteklenir. Ek olarak SQL API'si, Cosmos DB kablo protokolünü uyumlu API'leri için yerel destek MongoDB, Apache Cassandra, Gremlin ve Azure tabloları gibi popüler veritabanları için de sağlar. Bu veritabanları, hiçbiri tam olarak tanımlanmış tutarlılık modeli ya da SLA destekli için tutarlılık düzeylerini garanti eder ve Cosmos DB sunduğu beş tutarlılık modeli yalnızca bir kısmı genellikle sağlamak sunar. SQL API, Gremlin API ve tablo API'si için Cosmos hesabında yapılandırılmış varsayılan tutarlılık düzeyini kullanılır.

Aşağıdaki tablo OSS istemci sürücü tarafından istenen Apache Cassandra için veri tutarlılığı arasındaki eşlemeyi gösterir 4.x ve Cassandra API'si ve MongoDB API'si, sırasıyla kullanırken MongoDB 3.4 ve karşılık gelen Cosmos DB tutarlılık düzeyleri.

## <a id="cassandra-mapping"></a>Apache Cassandra ve Cosmos DB tutarlılık düzeyleri eşleme

Aşağıdaki tabloda, her iki çoklu bölge ve tek bölge dağıtımı için Cosmos DB tutarlılık düzeyi "Varsayılan" ile Apache Cassandra 4.x istemci arasında okuma tutarlılığı için eşleme gösterir.

| **Apache Cassandra 4.x** | **Cosmos DB (çok bölgeli)** | **Cosmos DB (tek bölge)** |
| - | - | - |
| BİR İKİ ÜÇ | Tutarlı Ön Ek | Tutarlı Ön Ek |
| LOCAL_ONE | Tutarlı Ön Ek | Tutarlı Ön Ek |
| ÇEKİRDEK, TÜMÜ, SERİ | Sınırlanmış eskime durumu (varsayılan) veya güçlü (özel önizleme içinde) | Güçlü |
| LOCAL_QUORUM | Sınırlanmış Eskime Durumu | Güçlü |
| LOCAL_SERIAL | Sınırlanmış Eskime Durumu | Güçlü |

## <a id="mongo-mapping"></a>MongoDB 3.4 ve Cosmos DB tutarlılık düzeyleri arasında eşleme

Aşağıdaki tabloda, "Okuma" MongoDB 3.4 ve Cosmos DB "Varsayılan" tutarlılık düzeyi ile ilgili konuları hem çok bölgeli ve tek bölge dağıtımı için eşlemeyi gösterir.

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