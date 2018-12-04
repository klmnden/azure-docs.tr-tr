---
title: Azure Cosmos DB kullanan uygulamanız için doğru tutarlılık düzeyi seçme
description: Azure Cosmos DB'de uygulamanız için doğru tutarlılık düzeyi seçme.
keywords: tutarlılık, performans, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/24/2018
ms.author: mjbrown
ms.openlocfilehash: a1c7d750bcd0c3f37d2269aee299e0ccd8c4ef4a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849301"
---
# <a name="choose-the-right-consistency-level-for-your-application"></a>Uygulamanız için doğru tutarlılık düzeyi seçme

Çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı olan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Çoğu ticari olarak satışta dağıtılmış veritabanları iki aşırı tutarlılık modeller arasında seçim geliştiricilerin isteyin: güçlü tutarlılık ve nihai tutarlılık. Azure Cosmos DB, geliştiricilerin beş iyi tanımlanmış tutarlılık modeli arasında seçin'ı izin verir: güçlü, sınırlanmış eskime, oturum, tutarlı ön ek ve nihai. Bu tutarlılık modellerinin her biri, iyi tanımlanmış, sezgisel ve belirli gerçek dünya senaryoları için kullanılabilir. Her beş tutarlılık modeli sağlayan [kullanılabilirliğini ve performansını ödünler](consistency-levels-tradeoffs.md) ve kapsamlı SLA'lar ile desteklenen. Aşağıdaki basit konuları birçok yaygın senaryoda doğru seçim yapmanıza yardımcı olur.

## <a name="sql-api-and-table-api"></a>SQL API ve tablo API'si

Cosmos DB SQL API veya tablo API'sini kullanarak uygulamanızı oluşturdunuz, aşağıdaki noktaları göz önünde bulundurun.

- Pek çok gerçek dünya senaryoları için oturum tutarlılığı en iyi olduğunu ve önerilen seçenektir. Daha fazla bilgi için bkz: [nasıl yapılır yönetme, uygulamanız için oturum belirteci](how-to-manage-consistency.md#utilize-session-tokens).

- Uygulamanızın güçlü tutarlılık gerektiriyorsa, sınırlanmış eskime durumu tutarlılık düzeyi kullanmanız önerilir.

- Oturum tutarlılığı ve yazma işlemleri için Tek haneli milisaniye gecikme süresi, kullanmanız önerilir sağlanan olanları eskime durumu tutarlılık düzeyi sınırlanmış daha katı gerekiyorsa tutarlılığını garanti eder.  

- Uygulamanızın son tutarlılık gerektiriyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.

- Oturum tutarlılığı tarafından sağlanan yapılandırılanlardan daha az katı tutarlılık garantisi gerekiyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.

- En düşük gecikme süresi ve yüksek kullanılabilirlik gerekiyorsa, nihai tutarlılık düzeyi'ni kullanın.

## <a name="cassandra-mongodb-and-gremlin-api"></a>Cassandra, MongoDB ve Gremlin API

- Apache Cassandra ve Cosmos DB tutarlılık düzeyi "Okuma tutarlılık düzeyi" sunulan arasında eşleme ayrıntıları görmek için [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#cassandra-mapping).

- MongoDB ve Azure Cosmos DB tutarlılık düzeyi "okuma sorunu" arasında eşleme hakkında daha fazla bilgi için bkz: [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#mongo-mapping).

## <a name="consistency-guarantees-in-practice"></a>Uygulamada tutarlılık garantisi

Uygulamada güçlü tutarlılık garantisi elde edebilirsiniz. Tutarlılık garantisi okuma işlemi için karşılık gelen, için yenilik ve sıralama istediğiniz veritabanının durumu. Okuma tutarlılığı, sıralama ve yazma/güncelleştirme işlemleri yayılma bağlıdır.  

* Tutarlılık düzeyi ayarlandığında **sınırlanmış eskime durumu**, Cosmos DB, istemcileri her zaman değeri, önceki yazma ile sınırlanmış eskime durumu pencere tarafından görüntülenmesine okumanızı garanti eder.

* Tutarlılık düzeyi ayarlandığında **güçlü**eskime durumu penceresi Sıfıra eşdeğer olan ve yazma işlemi ' son işlenen değerini okumak için istemcilerini garanti edilir.

* Kalan üç tutarlılık düzeylerini eskime durumu penceresi, iş yüküne büyük ölçüde büyük/küçük harf bağlıdır. Örneğin, bir okuma işlemi ile veritabanında yazma işlemi varsa **nihai**, **oturumu**, veya **tutarlı ön ek** tutarlılık düzeyleri getirebilir güçlü tutarlılık düzeyi okuma işlemi olarak aynı sonuçları.

Cosmos DB hesabınızı güçlü tutarlılık dışındaki bir tutarlılık düzeyi ile yapılandırılmışsa, istemcilerinize güçlü alma olasılığını ve iş yükleriniz için tutarlı okumaların olasılığa dayalı sınırlanmış eskime durumu (PBS konumunda) bakarak bulabilirsiniz Ölçüm. Bu ölçüm, Azure portalında sunulur, daha fazla bilgi için bkz. [İzleyici Probabilistically sınırlanmış eskime durumu (PBS) ölçüm](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Olasılığa dayalı sınırlanmış eskime durumu, nihai tutarlılık öğelerinizi nasıl nihai olduğunu gösterir. Bu ölçüm, Cosmos DB hesabınızda şu anda yapılandırılmış tutarlılık düzeyi daha güçlü tutarlılık elde edebilirsiniz ne sıklıkta Öngörüler sağlar. Diğer bir deyişle, yazma birleşimi için kesinlikle tutarlı okumaların alma olasılığı (milisaniye cinsinden ölçülür) görebilir ve okuma bölgeleri.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde tutarlılık düzeyleri hakkında daha fazlasını okuyun:

* [Cosmos DB API'ları arasında tutarlılık düzeyi eşleme](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Uygulamanız için oturum belirteci yönetme](how-to-manage-consistency.md#utilize-session-tokens)
* [İzleyici Probabilistically sınırlanmış eskime durumu (PBS) ölçüm](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
