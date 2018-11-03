---
title: Azure Cosmos DB'deki tutarlılık düzeyleri | Microsoft Docs
description: Azure Cosmos DB Bakiye nihai tutarlılık, kullanılabilirlik ve gecikme süresi dengelemeler yardımcı olmak üzere beş tutarlılık düzeyi vardır.
keywords: Nihai tutarlılık, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2611c25764503551c4da918d06bcaabe315cbf7c
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963090"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'deki tutarlılık düzeyleri

Çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı olan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Çoğu ticari olarak satışta dağıtılmış veritabanları iki aşırı tutarlılık modeller arasında seçim geliştiricilerin isteyin: güçlü tutarlılık ve nihai tutarlılık. Sırada [doğrusallaştırılabilirlik](http://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) veya güçlü tutarlılık modeli için veri programlama, altın standarttır, daha yüksek gecikme süresine (durağan) seyretmez fiyatı ekler ve kullanılabilirlik sınırlı (hataları sırasında). Öte yandan, nihai tutarlılık, daha yüksek kullanılabilirlik ve daha iyi performans sunar, ancak program uygulamaları zordur.

Cosmos DB olarak içeren iki uç nokta yerine seçimleri, veri tutarlılığını yaklaşıyor. Güçlü tutarlılık ve nihai tutarlılık spektrumu iki ucunu olsa da, spektrumun boyunca birçok tutarlılık seçeneği vardır. Bu tutarlılık seçenekleri, geliştiricilerin kesin seçenekleri ve yüksek kullanılabilirlik veya performans ile ilgili ayrıntılı ödünler olanak sağlar. Cosmos DB, geliştiriciler (zayıf için en güçlü) – tutarlılık yelpazesinden arasında beş iyi tanımlanmış tutarlılık modeli seçmek etkin **güçlü**, **sınırlanmış eskime durumu**, **oturumu** , **tutarlı ön ek**, ve **nihai**. Bu tutarlılık modellerinin her biri, iyi tanımlanmış, sezgisel ve belirli gerçek dünya senaryoları için kullanılabilir. Her beş tutarlılık modeli sağlayan [kullanılabilirliğini ve performansını ödünler](consistency-levels-tradeoffs.md) ve kapsamlı SLA'lar ile desteklenen.

![Tutarlılık spektrumu olarak](./media/consistency-levels/five-consistency-levels.png)

Tutarlılık düzeyleri bölge bağımsızdır. Cosmos DB hesabınızın tutarlılık düzeyi aşağıdaki özellikleri bağımsız olarak tüm okuma işlemleri için garantili:

- İçinden okuma ve yazma işlemleri hizmet bölgesi
- Cosmos hesabınızla ilişkili bölge sayısı
- Hesabınız tek bir veya birden çok yazma bölgeleri ile yapılandırıldı

## <a name="scope-of-the-read-consistency"></a>Okuma tutarlılığı kapsamı

Okuma işlemi (yani bir mantıksal bölüm) bir bölüm anahtar aralığı içinde kapsamlı bir tek bir okuma tutarlılığı uygular. Okuma işlemi, uzak bir istemci veya saklı yordam tarafından verilebilir.

## <a name="configuring-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

Yapılandırabileceğiniz **varsayılan tutarlılık düzeyi** Cosmos DB hesabınızda herhangi bir zamanda. Yapılandırılmış hesabınızdaki varsayılan tutarlılık düzeyini, bu hesap altında tüm Cosmos veritabanları (ve kapsayıcılar) uygular. Tüm okuma ve verilen bir kapsayıcı veya bir veritabanına karşı sorgular belirtilen tutarlılık düzeyi varsayılan olarak kullanır. Daha fazla bilgi için bkz. nasıl [varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level) makalesi.

## <a name="guarantees-associated-with-consistency-levels"></a>Tutarlılık düzeyleri ile ilişkili garanti eder

Okuma isteklerinin % 100 seçtiğiniz herhangi bir tutarlılık düzeyi için tutarlılık garantisini karşılar Azure Cosmos DB garantisi tarafından sağlanan kapsamlı SLA'lar. Tutarlılık düzeyi ile ilişkili tüm tutarlılık garantileri sağlanırsa tutarlılık SLA'sı karşılamak için Okuma isteği kabul edilir. Kullanarak Cosmos DB'de beş tutarlılık düzeyi kesin tanımlarını [TLA + belirtim dili](http://lamport.azurewebsites.net/tla/tla.html) sağlanan [azure cosmos tla](https://github.com/Azure/azure-cosmos-tla) GitHub deposu. Beş tutarlılık düzeyi semantiği aşağıda açıklanmıştır:

- **Tutarlılık düzeyi = "strong"**: güçlü tutarlılık sunan bir [doğrusallaştırılabilirlik](https://aphyr.com/posts/313-strong-consistency-models) garanti, bir öğenin işlenmiş en son sürüm dönmesi garanti okur ile. Bir istemci hiçbir zaman işlenmemiş ya da kısmi bir yazma görürsünüz. Kullanıcıların her zaman en son kabul edilen yazma okumak için garanti edilir.

- **Tutarlılık düzeyi = "sınırlanmış eskime durumu"**: Okuma, tutarlı ön ek garantisi uymanız garanti edilir. En fazla (yani öğenin "güncelleştirmeler") K sürümleri veya 't okuma yazma lag ' zaman aralığı. Seçme sınırlanmış eskime durumu, "eskime" iki şekilde yapılandırılabilir: 

  * Sürüm (K) öğe sayısı veya
  * Zaman aralığı (t) okuma yazma lag tarafından. 

  Sınırlanmış eskime durumu teklifler toplam genel sıra dışında "eskime durumu penceresi içinde." Monoton okuma garantisi mevcut bir bölgede hem iç hem de dış "eskime durumu pencere." Tarafından sunulan sınırlanmış eskime durumu ve bir "eskime penceresiyle" sıfıra eşit gibi güçlü tutarlılık ile aynı semantiğe sahip. Sınırlanmış eskime durumu da denir **zaman Gecikmeli doğrusallaştırılabilirlik**. Bir istemci, okuma işlemleri yazma kabul eden bir bölge içinde gerçekleştirdiğinde, sınırlanmış eskime durumu tutarlılık tarafından sağlanan garantileri güçlü tutarlılık ile aynı olan.

- **Tutarlılık düzeyi = "oturum"**: Okuma tutarlı ön ek, monoton okuma, monoton yazma etmenin garanti edilir, okuma-your-yazma işlemleri, yazma yazdıklarınızı okuma garanti eder. Oturum tutarlılığı için bir istemci oturumundan kapsamlıdır.

- **Tutarlılık düzeyi = "tutarlı ön ek"**: döndürülen güncelleştirmeleri, tüm güncelleştirmeleri herhangi bir boşluk ile bazı ön içerir. Tutarlı ön ek garantisi okuma hiçbir zaman sıralamaya yazma bakın.

- **Tutarlılık düzeyi = "son"**: Okuma için sıralama garantisi yoktur. Başka yazma işlemlerinin olmaması durumunda, yineleme sonunda birbirine yaklaşır.

## <a name="consistency-levels-explained-through-baseball"></a>Beyzbol örneği ile açıklanan tutarlılık düzeyleri

Şimdi Beyzbol oyun senaryoya örnek olarak almak, bir dizi yazma Imagine puanı inning tarafından inning satır puanı Beyzbol oyun açıklandığı temsil eden [Beyzbol aracılığıyla veri tutarlılığı çoğaltılan](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) Kağıt. Şu anda yedinci inning ortasında bu kuramsal Beyzbol oyunudur (proverbial yedinci – inning esnetme), ve takım giriş, 2-5 kazanma.

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Çalıştırmalar** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Ziyaretçiler** | 0 | 0 | 1 | 0 | 1 | 0 | 0 |  |  | 2 |
| **Giriş** | 1 | 0 | 1 | 1 | 0 | 2 |  |  |  | 5 |

Bir Cosmos DB kapsayıcısında ziyaretçileri ve giriş takımın çalışma toplam tutar. İstemciler farklı puanları okuma garantisi neden olabilir oyun devam ederken, farklı okuyun. Aşağıdaki tabloda her beş tutarlılık garantileri giriş puanları ve Ziyaretçiler okuyarak döndürülebilen puanları tam kümesini listeler. Ziyaretçilerinizin puanı listede ilk sıradaysa ve farklı olası dönüş değerleri virgülle ayrılır.

| **Tutarlılık düzeyi** | **Puanları** |
| - | - |
| **Tanımlayıcı** | 2-5 |
| **Sınırlanmış eskime durumu** | puanları çoğu bir inning güncel olmayan"2-3, 2-4, 2-5 |
| **Oturumu** | <ul><li>Yazıcı için"2-5</li><li> Yazıcı dışındaki için: 0-0, 0-1, 0-2, 0-3, 4 0, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>1-3 okuma sonra: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Tutarlı ön ek** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Nihai** | 0-0, 0-1, 0-2, 0-3, 4 0, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Ek okuma

Tutarlılık kavramları hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

- [Azure Cosmos DB tarafından sunulan beş tutarlılık düzeyi için üst düzey TLA + belirtimleri](https://github.com/Azure/azure-cosmos-tla)
- [Veri tutarlılığı açıklanan Doug Terry tarafından Beyzbol (video) üzerinden çoğaltılır](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Veri tutarlılığı açıklanan Doug Terry tarafından Beyzbol (Teknik İnceleme) üzerinden çoğaltılır](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Zayıf sahipli tutarlı çoğaltılan veriler için oturumu garanti eder](https://dl.acm.org/citation.cfm?id=383631)
- [Modern dağıtılmış veritabanı sistemleri tasarım tutarlılık seçenekleri: UÇ hikayeyi yalnızca bir parçası değildir](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
- [Olasılığa dayalı sınırlanmış eskime durumu (PBS) için pratik kısmi çekirdekleri](http://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Sonunda tutarlı - Revisited](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB'deki tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

* [Uygulamanız için doğru tutarlılık düzeyi seçme](consistency-levels-choosing.md)
* [Cosmos DB API'ları arasında tutarlılık düzeyleri](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Varsayılan tutarlılık düzeyini geçersiz kılma](how-to-manage-consistency.md#override-the-default-consistency-level)

