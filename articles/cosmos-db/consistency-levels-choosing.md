---
title: Azure Cosmos DB kullanan uygulamanız için doğru tutarlılık düzeyi seçme
description: Azure Cosmos DB'de uygulamanız için doğru tutarlılık düzeyi seçme.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/22/2019
ms.reviewer: sngun
ms.openlocfilehash: b08f9a85b8c9f52724e2cd08eaf13eb1faae0977
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243554"
---
# <a name="choose-the-right-consistency-level"></a>Doğru tutarlılık düzeyini seçme 

Çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı olan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Çoğu ticari olarak satışta dağıtılmış veritabanları iki aşırı tutarlılık modeller arasında seçim geliştiricilerin isteyin: *güçlü* tutarlılık ve *nihai* tutarlılık. Azure Cosmos DB, geliştiricilerin beş iyi tanımlanmış tutarlılık modeli arasında seçin'ı izin verir: *güçlü*, *sınırlanmış eskime durumu*, *oturumu*, *tutarlı önek* ve *nihai*. Bu tutarlılık modellerinin her biri, iyi tanımlanmış, sezgisel ve belirli gerçek dünya senaryoları için kullanılabilir. Her beş tutarlılık modeli sağlayan kesin [kullanılabilirliğini ve performansını ödünler](consistency-levels-tradeoffs.md) ve kapsamlı SLA'lar ile desteklenen. Aşağıdaki basit konuları birçok yaygın senaryoda doğru seçim yapmanıza yardımcı olur.

## <a name="sql-api-and-table-api"></a>SQL API ve tablo API'si

SQL API veya tablo API'sini kullanarak uygulamanızı oluşturdunuz, aşağıdaki noktaları göz önünde bulundurun:

- Pek çok gerçek dünya senaryoları için oturum tutarlılığı en iyi olduğunu ve önerilen seçenektir. Daha fazla bilgi için bkz: [nasıl yapılır yönetme, uygulamanız için oturum belirteci](how-to-manage-consistency.md#utilize-session-tokens).

- Uygulamanızın güçlü tutarlılık gerektiriyorsa, sınırlanmış eskime durumu tutarlılık düzeyi kullanmanız önerilir.

- Oturum tutarlılığı ve yazma işlemleri için Tek haneli milisaniye gecikme süresi, kullanmanız önerilir sağlanan olanları eskime durumu tutarlılık düzeyi sınırlanmış daha katı gerekiyorsa tutarlılığını garanti eder.  

- Uygulamanızın son tutarlılık gerektiriyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.

- Oturum tutarlılığı tarafından sağlanan yapılandırılanlardan daha az katı tutarlılık garantisi gerekiyorsa, tutarlı ön ek tutarlılık düzeyi kullanmanız önerilir.

- En yüksek kullanılabilirlik ve düşük gecikme süresi gerekiyorsa, nihai tutarlılık düzeyi'ni kullanın.

- Daha yüksek veri dayanıklılık performanstan ödün vermeden ihtiyacınız varsa, bir özel tutarlılık düzeyi uygulama katmanında oluşturabilirsiniz. Daha fazla bilgi edinmek, [nasıl yapılır, uygulamalarınızda özel eşitlemesi uygulama](how-to-custom-synchronization.md).

## <a name="cassandra-mongodb-and-gremlin-apis"></a>Cassandra, MongoDB ve Gremlin API'leri

- Apache Cassandra ve Cosmos DB tutarlılık düzeyi "Okuma tutarlılık düzeyi" sunulan arasında eşleme ayrıntıları görmek için [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#cassandra-mapping).

- MongoDB ve Azure Cosmos DB tutarlılık düzeyi "okuma sorunu" arasında eşleme hakkında daha fazla bilgi için bkz: [tutarlılık düzeyleri ve Cosmos DB API'leri](consistency-levels-across-apis.md#mongo-mapping).

## <a name="consistency-guarantees-in-practice"></a>Uygulamada tutarlılık garantisi

Uygulamada, genellikle daha güçlü tutarlılık garantisi elde edebilirsiniz. Tutarlılık garantisi okuma işlemi için karşılık gelen, için yenilik ve sıralama istediğiniz veritabanının durumu. Okuma tutarlılığı, sıralama ve yazma/güncelleştirme işlemleri yayılma bağlıdır.  

* Tutarlılık düzeyi ayarlandığında **sınırlanmış eskime durumu**, Cosmos DB, istemcileri her zaman değeri, önceki yazma ile sınırlanmış eskime durumu pencere tarafından görüntülenmesine okumanızı garanti eder.

* Tutarlılık düzeyi ayarlandığında **güçlü**eskime durumu penceresi Sıfıra eşdeğer olan ve yazma işlemi ' son işlenen değerini okumak için istemcilerini garanti edilir.

* Kalan üç tutarlılık düzeylerini eskime durumu penceresi, iş yüküne büyük ölçüde büyük/küçük harf bağlıdır. Örneğin, bir okuma işlemi ile veritabanında yazma işlemi varsa **nihai**, **oturumu**, veya **tutarlı ön ek** tutarlılık düzeyleri getirebilir güçlü tutarlılık düzeyi okuma işlemi olarak aynı sonuçları.

Azure Cosmos hesabınızı güçlü tutarlılık dışındaki bir tutarlılık düzeyi ile yapılandırılmışsa, istemcilerinize güçlü alma olasılığını ve iş yükleriniz için tutarlı okumaların bakarak bulabilirsiniz *Probabilistically Sınırlanmış eskime durumu* (PBS) ölçümü. Bu ölçüm, Azure portalında sunulur, daha fazla bilgi için bkz. [İzleyici Probabilistically sınırlanmış eskime durumu (PBS) ölçüm](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric).

Olasılığa dayalı sınırlanmış eskime durumu, nihai tutarlılık öğelerinizi nasıl nihai olduğunu gösterir. Bu ölçüm, Azure Cosmos hesabınızda şu anda yapılandırılmış tutarlılık düzeyi daha güçlü tutarlılık elde edebilirsiniz ne sıklıkta Öngörüler sağlar. Diğer bir deyişle, yazma birleşimi için kesinlikle tutarlı okumaların alma olasılığı (milisaniye cinsinden ölçülür) görebilir ve okuma bölgeleri.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde tutarlılık düzeyleri hakkında daha fazlasını okuyun:

* [Cosmos DB API'ları arasında tutarlılık düzeyi eşleme](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Uygulamanız için oturum belirteci yönetme](how-to-manage-consistency.md#utilize-session-tokens)
* [İzleyici Probabilistically sınırlanmış eskime durumu (PBS) ölçüm](how-to-manage-consistency.md#monitor-probabilistically-bounded-staleness-pbs-metric)
