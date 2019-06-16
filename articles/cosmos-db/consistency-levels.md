---
title: Azure Cosmos DB'deki tutarlılık düzeyleri
description: Azure Cosmos DB Bakiye nihai tutarlılık, kullanılabilirlik ve gecikme süresi dengelemeler yardımcı olmak üzere beş tutarlılık düzeyi vardır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/20/2019
ms.openlocfilehash: dcd51756a9c5a5a24a082862bb911cc2d2605d61
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65954366"
---
# <a name="consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'deki tutarlılık düzeyleri

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de çoğaltma kullanan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Çoğu ticari olarak satışta dağıtılmış veritabanları iki aşırı tutarlılık modeller arasında seçim geliştiricilerin isteyin: *güçlü* tutarlılık ve *nihai* tutarlılık.  [Doğrusallaştırılabilirlik](https://cs.brown.edu/~mph/HerlihyW90/p463-herlihy.pdf) veya güçlü tutarlılık modeli veri programlama, altın standardıdır. Ancak daha yüksek gecikme süresine (durağan) fiyatı ekler ve kullanılabilirlik (hatalarda) azaltıldı. Öte yandan, nihai tutarlılık, daha yüksek kullanılabilirlik ve daha iyi performans sunar, ancak program uygulamaları zorlaştırır. 

Azure Cosmos DB olarak içeren iki uç nokta yerine seçimleri, veri tutarlılığını yaklaşıyor. Güçlü tutarlılık ve nihai tutarlılık spektrumun bir ucunda, ancak spektrumun boyunca birçok tutarlılık seçeneği vardır. Geliştiriciler, kesin seçenekleri ve yüksek kullanılabilirlik ve performans ile ilgili ayrıntılı ödünler yapmak için bu seçenekleri kullanabilirsiniz. 

Azure Cosmos DB ile geliştiriciler beş iyi tanımlanmış tutarlılık modeli üzerinde tutarlılık spektrumu arasından seçim yapabilirsiniz. Öğesinden daha rahat güçlüden modelleridir *güçlü*, *sınırlanmış eskime durumu*, *oturumu*, *tutarlı ön ek*ve *nihai* tutarlılık. Modelleri, iyi tanımlanmış ve sezgisel ve belirli gerçek dünya senaryoları için kullanılabilir. Her model sağlayan [kullanılabilirliğini ve performansını ödünler](consistency-levels-tradeoffs.md) ve SLA'lar ile desteklenen. Aşağıdaki görüntüde bir yelpaze farklı tutarlılık düzeyleri gösterilmektedir.

![Tutarlılık spektrumu olarak](./media/consistency-levels/five-consistency-levels.png)

Tutarlılık düzeyleri bölge bağımsızdır ve kendisinden okuma ve yazma işlemleri sunulan, Azure Cosmos hesabınızla ilişkili bölge sayısı uygulama bölgesine bakılmaksızın tüm işlemler için garanti veya tek bir hesabınız olup olmadığını yapılandırıldı veya birden çok yazma bölgeleri.

## <a name="scope-of-the-read-consistency"></a>Okuma tutarlılığı kapsamı

Bölüm anahtar aralığı veya bir mantıksal bölüm içinde kapsamlı bir tek okuma işlemi okuma tutarlılığı uygular. Okuma işlemi, uzak bir istemci veya saklı yordam tarafından verilebilir.

## <a name="configure-the-default-consistency-level"></a>Varsayılan tutarlılık düzeyini yapılandırma

İstediğiniz zaman Azure Cosmos hesabınızdaki varsayılan tutarlılık düzeyini yapılandırabilirsiniz. Tüm Azure Cosmos veritabanı ve kapsayıcıları söz konusu hesap altında yapılandırılmış hesabınızdaki varsayılan tutarlılık düzeyi uygular. Tüm okuma ve verilen bir kapsayıcı veya bir veritabanına karşı sorgular belirtilen tutarlılık düzeyi varsayılan olarak kullanır. Daha fazla bilgi için bkz. nasıl [varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level).

## <a name="guarantees-associated-with-consistency-levels"></a>Tutarlılık düzeyleri ile ilişkili garanti eder

Yüzde 100 okuma isteklerinin seçtiğiniz herhangi bir tutarlılık düzeyi için tutarlılık garantisini karşılayan Azure Cosmos DB garantisi tarafından sağlanan kapsamlı SLA'lar. Tutarlılık düzeyi ile ilişkili tüm tutarlılık garantileri sağlanırsa Okuma isteği tutarlılık SLA karşılar. Azure Cosmos DB kullanarak beş tutarlılık düzeyi kesin tanımlarını [TLA + belirtim dili](https://lamport.azurewebsites.net/tla/tla.html) sağlanan [azure cosmos tla](https://github.com/Azure/azure-cosmos-tla) GitHub deposu. 

Beş tutarlılık düzeyi semantiği aşağıda açıklanmıştır:

- **Güçlü**: Güçlü tutarlılık sunan bir [doğrusallaştırılabilirlik](https://aphyr.com/posts/313-strong-consistency-models) garanti. Okuma işlemleri, bir öğe işlenen en son sürümünü döndürmek için garanti edilir. Bir istemci hiçbir zaman işlenmemiş ya da kısmi bir yazma görür. Kullanıcıların her zaman en son kabul edilen yazma okumak için garanti edilir.

- **Sınırlanmış eskime durumu**: Okuma işlemleri, tutarlı ön ek garantisi uymanız garanti edilir. Okuma göre yazma işlemlerinin arkasındaki en fazla lag *"K"* (yani, "güncelleştirmeler") bir öğe veya tarafından sürümleri *"T"* zaman aralığı. Diğer bir deyişle, sınırlanmış eskime durumu seçtiğinizde, "eskime" iki şekilde yapılandırılabilir: 

  * Sürüm sayısı (*K*) öğesi
  * Zaman aralığı (*T*) tarafından okuma işlemleri yazma lag 

  Sınırlanmış eskime durumu teklifler toplam genel sıra dışında "eskime durumu penceresi içinde." Monoton okuma garantisi, hem Azure içindeki hem eskime durumu penceresi dışında bir bölge içinde mevcut. Güçlü tutarlılık, sınırlanmış eskime durumu tarafından sunulan biri ile aynı semantiğe sahip. Eskime durumu penceresi sıfıra eşittir. Sınırlanmış eskime durumu, doğrusallaştırılabilme zaman Gecikmeli da bilinir. Bir istemci, okuma işlemleri yazma kabul eden bir bölge içinde gerçekleştirdiğinde, sınırlanmış eskime durumu tutarlılık tarafından sağlanan garantileri güçlü tutarlılık tarafından bu garantileri aynıdır.

- **Oturum**: Okuma tutarlı (çoklu "yazan" oturumu varsayılarak) ön ek, monoton okuma, monoton yazma, okuma your-yazma ve yazma yazdıklarınızı okuma garanti etmenin garanti edilir. Oturum tutarlılığı için bir istemci oturumundan kapsamlıdır.

- **Tutarlı ön ek**: Tüm güncelleştirmeleri herhangi bir boşluk ile bazı ön döndürülen güncelleştirmeleri içerir. Tutarlı ön ek tutarlılık düzeyi okuma hiçbir sırası yazma gördüğünüzü garanti eder.

- **Nihai**: Okuma için sıralama garantisi yoktur. Başka yazma işlemlerinin olmaması durumunda, yineleme sonunda birbirine yaklaşır.

## <a name="consistency-levels-explained-through-baseball"></a>Beyzbol örneği ile açıklanan tutarlılık düzeyleri

Beyzbol oyun senaryoya örnek olarak alalım. Beyzbol oyunu baştan puanı temsil eden bir yazma dizisi düşünün. İnning tarafından inning satır puanı açıklanan [Beyzbol aracılığıyla veri tutarlılığı çoğaltılan](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf) kağıt. Şu anda yedinci inning ortasında bu kuramsal Beyzbol oyundur. Bu, yedinci – inning esnetme olur. Ziyaretçi arkasında aşağıda gösterildiği gibi bir 2 ila 5 puanı ile şunlardır:

| | **1** | **2** | **3** | **4** | **5** | **6** | **7** | **8** | **9** | **Çalıştırmalar** |
| - | - | - | - | - | - | - | - | - | - | - |
| **Ziyaretçiler** | 0 | 0 | 1\. | 0 | 1\. | 0 | 0 |  |  | 2 |
| **Giriş** | 1 | 0 | 1\. | 1 | 0 | 2 |  |  |  | 5 |

Bir Azure Cosmos kapsayıcı giriş takımlar ve ziyaretçiler için çalıştırma toplam tutar. Garanti farklı puanları okuma istemcilerle sonuçlanabilir oyun devam ederken, farklı okuyun. Aşağıdaki tabloda her beş tutarlılık garantileri giriş puanları ve ziyaretçilerinizin okuyarak döndürülebilir puanları tam kümesini listeler. Ziyaretçilerinizin puanı önce listelenir. Farklı olası dönüş değerleri virgülle ayrılır.

| **Tutarlılık düzeyi** | **Puanlar (ziyaretçilerin, giriş)** |
| - | - |
| **Tanımlayıcı** | 2-5 |
| **Sınırlanmış eskime durumu** | En fazla bir inning güncel olan Puanları: 2-3, 2-4, 2-5 |
| **Oturumu** | <ul><li>Yazıcı için: 2-5</li><li> Herkes için yazıcı dışında: 0-0, 0-1, 0-2, 0-3, 4 0, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5</li><li>1-3 okuduktan sonra: 1-3, 1-4, 1-5, 2-3, 2-4, 2-5</li> |
| **Tutarlı ön ek** | 0-0, 0-1, 1-1, 1-2, 1-3, 2-3, 2-4, 2-5 |
| **Nihai** | 0-0, 0-1, 0-2, 0-3, 4 0, 0-5, 1-0, 1-1, 1-2, 1-3, 1-4, 1-5, 2-0, 2-1, 2-2, 2-3, 2-4, 2-5 |

## <a name="additional-reading"></a>Ek okuma

Tutarlılık kavramları hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

- [Azure Cosmos DB tarafından sunulan beş tutarlılık düzeyi için üst düzey TLA + belirtimleri](https://github.com/Azure/azure-cosmos-tla)
- [Çoğaltılan verilerin tutarlılık açıklandığı aracılığıyla Beyzbol (video) Doug Terry tarafından](https://www.youtube.com/watch?v=gluIh8zd26I)
- [Çoğaltılan verilerin tutarlılık açıklandığı aracılığıyla Beyzbol (Teknik İnceleme) Doug Terry tarafından](https://www.microsoft.com/en-us/research/publication/replicated-data-consistency-explained-through-baseball/?from=http%3A%2F%2Fresearch.microsoft.com%2Fpubs%2F157411%2Fconsistencyandbaseballreport.pdf)
- [Oturum, tutarlı zayıf çoğaltılan veriler için garanti eder](https://dl.acm.org/citation.cfm?id=383631)
- [Veritabanı sistemleri tasarımı tutarlılık Ödünler Modern, dağıtılmış: CAP hikayeyi yalnızca bir parçası olan](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Olasılığa dayalı sınırlanmış eskime durumu (PBS) için pratik kısmi çekirdekleri](https://vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf)
- [Sonunda tutarlı - Revisited](https://www.allthingsdistributed.com/2008/12/eventually_consistent.html)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'deki tutarlılık düzeyleri hakkında daha fazla bilgi edinmek için bu makaleleri okuyun:

* [Uygulamanız için doğru tutarlılık düzeyi seçme](consistency-levels-choosing.md)
* [Azure Cosmos DB API'leri arasında tutarlılık düzeyleri](consistency-levels-across-apis.md)
* [Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri](consistency-levels-tradeoffs.md)
* [Varsayılan tutarlılık düzeyini yapılandırma](how-to-manage-consistency.md#configure-the-default-consistency-level)
* [Varsayılan tutarlılık düzeyi geçersiz kıl](how-to-manage-consistency.md#override-the-default-consistency-level)

