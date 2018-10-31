---
title: Uygulamanız için doğru tutarlılık düzeyi seçme | Microsoft Docs
description: Azure Cosmos DB'de uygulamanız için doğru tutarlılık düzeyi seçme.
keywords: tutarlılık, performans, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: mjbrown
ms.openlocfilehash: 4b438320ac75918d10880a4bcdccadca4eebec32
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244188"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Uygulamanız için doğru tutarlılık düzeyi seçme

Çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı olan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Çoğu ticari olarak satışta dağıtılmış veritabanları iki aşırı tutarlılık modeller arasında seçim geliştiricilerin isteyin: güçlü tutarlılık ve nihai tutarlılık. Azure Cosmos DB, geliştiricilerin beş iyi tanımlanmış tutarlılık modeli seçin'ı izin verir: güçlü, sınırlanmış eskime, oturum, tutarlı ön ek ve nihai. Bu tutarlılık modellerinin her biri, iyi tanımlanmış, sezgisel ve belirli gerçek dünya senaryoları için kullanılabilir. Her beş tutarlılık modeli Temizle kullanılabilirliğini ve performansını ödünler sağlar ve kapsamlı SLA'lar ile desteklenen. Aşağıdaki basit konuları birçok yaygın senaryoda doğru seçim yapmanıza yardımcı olur.

## <a name="sql-api-or-table-api"></a>SQL API veya tablo API'si

- Pek çok gerçek dünya senaryoları için oturum tutarlılığı en iyi olduğunu ve önerilen seçenektir. Daha fazla bilgi edinmek, [nasıl yapılır yönetme, uygulamanız için oturum belirteci](how-to-manage-consistency.md#utilize-session-tokens).
- Uygulamanızın gerektirdiği güçlü tutarlılık, sınırlanmış eskime durumu tutarlılık düzeyi kullanmanız önerilir.
- Oturum tutarlılığı hem tek basamaklı milisaniyelik gecikme tarafından yazma işlemleri için sağlanan yapılandırılanlardan katı tutarlılık garantisi gerekiyorsa, sınırlanmış eskime durumu tutarlılık düzeyi kullanmanız önerilir.  
- Uygulamanızın son tutarlılık gerektiriyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.
- Oturum tutarlılığı tarafından sağlanan daha az katı tutarlılık garantisi gerekiyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.
- En düşük gecikme süresi ve yüksek kullanılabilirlik gerekiyorsa, nihai tutarlılık düzeyi'ni kullanın.

## <a name="cassandra-mongodb-or-gremlin-api"></a>Cassandra, MongoDB veya Gremlin API

- Apache Cassandra ve Cosmos DB tutarlılık düzeyi "okuma tutarlılık düzeyi" arasında eşleme hakkında daha fazla bilgi için bkz: [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#cassandra-mapping).
- MongoDB ve Azure Cosmos DB tutarlılık düzeyi "okuma sorunu" arasında eşleme hakkında daha fazla bilgi için bkz: [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#mongo-mapping).

## <a name="you-may-get-stronger-consistency-guarantees-in-practice"></a>Uygulamada güçlü tutarlılık garantisi elde edebilirsiniz

Okuma işlemi için tutarlılık garantisi, yenilik ve istenen veritabanının durumunu sıralama karşılık gelir. Okuma tutarlılığı, sıralama ve yazma (güncelleştirme) işlemleri yayılma bağlıdır.  

Tutarlılık düzeyi ayarlandığında **sınırlanmış eskime durumu**, Cosmos DB, istemcileri her zaman değeri, önceki yazma ile sınırlanmış eskime durumu pencere tarafından görüntülenmesine okumanızı garanti eder.

Tutarlılık düzeyi ayarlandığında **güçlü**eskime durumu penceresi Sıfıra eşdeğer olan ve istemcilerin en son kaydedilen değerini yazma okumak için garanti edilir.

Kalan üç tutarlılık düzeylerini eskime durumu penceresi, iş yüküne büyük ölçüde büyük/küçük harf bağlıdır. Örneğin, hiçbir yazma varsa veritabanında okuma işlemi ile oluşmasını **nihai**, **oturumu**, veya **tutarlı ön ek** tutarlılık düzeyleri getirebilir güçlü tutarlılık düzeyi okuma işlemi olarak aynı sonuçları.

Cosmos DB hesabınızı güçlü tutarlılık dışındaki herhangi bir tutarlılık düzeyi ile yapılandırılmışsa, istemcilerinizin olasılığa dayalı sınırlanmış bakarak, workload(s) için (linearizable) kesinlikle tutarlı okumaların alma olasılığı bulabilirsiniz Azure portalında kullanıma sunulan eskime durumu (PBS) ölçüm [PBS ölçüm kullanmayı için burada gördüğünüz](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric). Olasılığa dayalı sınırlanmış eskime durumu, nihai tutarlılık öğelerinizi nasıl nihai olduğunu gösterir. Bu ölçüm, Cosmos DB hesabınızda yapılandırdığınız tutarlılık düzeyi daha güçlü tutarlılık elde ne sıklıkta Öngörüler sağlar. Diğer bir deyişle, yazma ve okuma bölgeleri için kesinlikle tutarlı okumaların alma olasılığı (içinde gösterilen milisaniye cinsinden) görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde tutarlılık düzeyleri hakkında daha fazlasını okuyun:

* [Cosmos DB API'ları arasında tutarlılık düzeyi eşleme](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Uygulamanız için oturum belirteci yönetme](how-to-manage-consistency.md#utilize-session-tokens)
* [Probabilistically sınırlanmış eskime durumu (PBS) ölçüm izleme](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)