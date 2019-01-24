---
title: Tutarlılık düzeyleri ve Azure Cosmos DB API’leri
description: Azure Cosmos DB API'leri arasında tutarlılık düzeylerini anlama.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: 4d2994ea6ab6d6472ec56f0f2e378062590c8920
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54807006"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Tutarlılık düzeyleri ve Azure Cosmos DB API’leri

Azure Cosmos DB tarafından sunulan beş tutarlılık modeli yerel olarak Azure Cosmos DB SQL API tarafından desteklenir. Azure Cosmos DB kullandığınızda, SQL API'si varsayılandır. 

Azure Cosmos DB ayrıca kablo için yerel destek sağlar popüler veritabanları için protokolü ile uyumlu API'leri. Veritabanlarını, MongoDB, Apache Cassandra, Gremlin ve Azure tablo depolama alanı içerir. Bu veritabanları, tam olarak tanımlanmış tutarlılık modeli veya tutarlılık düzeyleri için SLA destekli garantileri sunmamaktadır. Bunlar genellikle yalnızca bir alt kümesini Azure Cosmos DB tarafından sunulan beş tutarlılık modeli sunar. SQL API, Gremlin API ve tablo API'si için Azure Cosmos DB hesabı yapılandırılmış varsayılan tutarlılık düzeyini kullanılır. 

Aşağıdaki bölümlerde OSS istemci sürücü tarafından istenen Apache Cassandra için veri tutarlılığı arasındaki eşlemeyi Göster 4.x ve MongoDB 3.4. Bu belge, ayrıca Apache Cassandra ve MongoDB için karşılık gelen Azure Cosmos DB tutarlılık düzeyleri gösterilmektedir.

## <a id="cassandra-mapping"></a>Apache Cassandra ve Azure Cosmos DB tutarlılık düzeyleri arasında eşleme

Bu tablo, Azure Cosmos DB'de Apache Cassandra ve tutarlılık düzeyleri tutarlılık eşlemesini gösterir. Her Cassandra okuma ve yazma tutarlılık düzeyleri için karşılık gelen bir Cosmos DB tutarlılık düzeyi daha güçlü sağlayan başka bir deyişle, katı garanti eder.

Aşağıdaki tabloda **tutarlılık eşleme yazma** Azure Cosmos DB ile Cassandra arasında:

| Cassandra | Azure Cosmos DB | Garantisi |
| - | - | - |
|TÜMÜ|Güçlü  | Doğrusallaştırma |
| EACH_QUORUM   | Güçlü    | Doğrusallaştırma | 
| ÇEKİRDEK SERİ |  Güçlü |    Doğrusallaştırma |
| LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, TÜM | Tutarlı Ön Ek |Genel tutarlı ön ek |
| EACH_QUORUM   | Güçlü    | Doğrusallaştırma |
| ÇEKİRDEK SERİ |  Güçlü |    Doğrusallaştırma |
| LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, TÜM | Tutarlı Ön Ek | Genel tutarlı ön ek |
| ÇEKİRDEK SERİ | Güçlü   | Doğrusallaştırma |
| LOCAL_QUORUM, ÜÇ, İKİ, BİRİ, LOCAL_ONE, TÜM | Tutarlı Ön Ek | Genel tutarlı ön ek |
| LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ    | Sınırlanmış Eskime Durumu | <ul><li>Sınırlanmış eskime durumu.</li><li>En fazla K sürümleri veya t saatten.</li><li>Son yürütülen değere bölgede okuyun.</li></ul> |
| BİR LOCAL_ONE, TÜM   | Tutarlı Ön Ek | Bölge başına tutarlı ön ek |

Aşağıdaki tabloda **tutarlılık eşleme okuma** Azure Cosmos DB ile Cassandra arasında:

| Cassandra | Azure Cosmos DB | Garantisi |
| - | - | - |
| TÜM ÇEKİRDEK, SERİ, LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ, BİRİ, LOCAL_ONE | Güçlü  | Doğrusallaştırma|
| TÜM ÇEKİRDEK, SERİ LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ   |Güçlü |   Doğrusallaştırma |
|LOCAL_ONE, ONE | Tutarlı Ön Ek | Genel tutarlı ön ek |
| TÜMÜ, ÇEKİRDEK, SERİ   | Güçlü    | Doğrusallaştırma |
| LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ |  Tutarlı Ön Ek   | Genel tutarlı ön ek |
| LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK |    Tutarlı Ön Ek   | Genel tutarlı ön ek |
| TÜM ÇEKİRDEK, SERİ LOCAL_QUORUM, LOCAL_SERIAL, ÜÇ, İKİ   |Güçlü |   Doğrusallaştırma |
| LOCAL_ONE, ONE    | Tutarlı Ön Ek | Genel tutarlı ön ek|
| TÜM çekirdek, seri güçlü Doğrusallaştırılabilirlik
LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ  |Tutarlı Ön Ek  | Genel tutarlı ön ek |
|TÜMÜ    |Güçlü |Doğrusallaştırma |
| LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK  |Tutarlı Ön Ek  |Genel tutarlı ön ek|
|TÜM çekirdek, seri güçlü Doğrusallaştırılabilirlik
LOCAL_ONE, BİRİ, LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ  |Tutarlı Ön Ek  |Genel tutarlı ön ek |
|TÜMÜ    |Güçlü | Doğrusallaştırma |
| LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK  | Tutarlı Ön Ek | Genel tutarlı ön ek |
| ÇEKİRDEK LOCAL_QUORUM, LOCAL_SERIAL, İKİ, ÜÇ |  Sınırlanmış Eskime Durumu   | <ul><li>Sınırlanmış eskime durumu.</li><li>En fazla K sürümleri veya t saatten. </li><li>Son yürütülen değere bölgede okuyun.</li></ul>
| LOCAL_ONE, ONE |Tutarlı Ön Ek | Bölge başına tutarlı ön ek |
| LOCAL_ONE, BİR, İKİ, ÜÇ, LOCAL_QUORUM, ÇEKİRDEK  | Tutarlı Ön Ek | Bölge başına tutarlı ön ek |


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
* [MongoDB için Azure Cosmos DB API tarafından desteklenen MongoDB özellikleri](mongodb-feature-support.md)
* [Azure Cosmos DB Cassandra API'si tarafından desteklenen Apache Cassandra özellikleri](cassandra-support.md)