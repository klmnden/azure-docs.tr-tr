---
title: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: 09777a9980e4576a5d00123516e33696e845dcac
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990218"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans dengeleri 

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltma kullanan dağıtılmış veritabanları ödünler yapmanız gerekir. Kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı dengelemeleri arasındadır.

Azure Cosmos DB, veri tutarlılığı seçenek yelpazesi olarak ele almaktadır. Bu yaklaşım, güçlü ve nihai tutarlılık iki uç nokta değerinden daha fazla seçenek içerir. İyi tanımlanmış beş tutarlılık spektrumu modellerde arasından seçim yapabilirsiniz. Öğesinden model güçlüden zayıfa doğru şunlardır:

- *Tanımlayıcı*
- *Sınırlanmış eskime durumu*
- *Oturumu*
- *Tutarlı ön ek*
- *Nihai*

Her model, kullanılabilirlik ve performans seçenekleri sağlar ve kapsamlı SLA'lar ile desteklenen.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

Okuma gecikme süresi tüm tutarlılık düzeyi için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır. Bu okuma gecikme süresi, SLA ile desteklenir. Okuma gecikme süresi, 50. yüzdebirlik ortalama genellikle 2 milisaniyeden kısadır veya daha az. Çeşitli bölgeleri kapsayan ve güçlü tutarlılık ile yapılandırılmış olan azure Cosmos hesapları bu garantisi için bir özel durumdur.

Yazma gecikme süresi tüm tutarlılık düzeyi için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır. Bu yazma gecikme süresi, SLA ile desteklenir. 50. yüzdebirlik ortalama yazma gecikme süresi, genellikle 5 mili saniye olan veya daha az.

Güçlü tutarlılık ile birden fazla bölge ile yapılandırılmış Azure Cosmos hesapları için yazma gecikme süresi kısa iki kez gidiş dönüş süresini (RTT) herhangi birini en uzak iki bölgeleri yanı sıra, 99. yüzdebirlik dilimde 10 mili saniye arasında garanti.

Tam RTT gecikme süresi, ışık hızı uzaklık işlevi ve Azure ağ topolojisi ' dir. Azure ağı, herhangi iki Azure bölgeleri arasında RTT için herhangi bir gecikme SLA sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri, Azure portalında görüntülenir. Azure portalını (ölçümler dikey penceresine gidin), Azure Cosmos hesabınızla ilişkili çeşitli bölgeler arasında çoğaltma gecikmeleri izlemek için kullanabilirsiniz.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve aktarım hızı

- İstek birimleri aynı sayıda için oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri güçlü ve sınırlanmış eskime durumu ile karşılaştırıldığında iki katı okuma aktarım hızı sağlar.

- Yazma işlemi, ekleme, değiştirme, upsert ve silme gibi belirli bir tür için tüm tutarlılık düzeyleri için yazma üretimi için istek birimi aynıdır.

## <a id="rto"></a>Tutarlılık düzeyleri ve veri dayanıklılığı

Bir Global olarak dağıtılmış veritabanı ortam içinde bir bölge çapında kesinti varsa tutarlılık düzeyi ve veri dayanıklılığı arasında doğrudan bir ilişki yoktur. İş sürekliliği planınızı geliştirirken, uygulamanın kesintiden sonra tamamen kurtarır önce kabul edilebilen maksimum süre anlamanız gerekir. Bir uygulamanın tamamen kurtarmak için gereken zamanı olarak bilinen **kurtarma süresi hedefi** (**RTO**). Ayrıca uygulama edilebilecek son veri güncelleştirmelerinin maksimum süreyi anlamanız gereken bir kesintiden sonra kurtarılırken. Süre kaybetmeyi göze bir güncelleştirme olarak da bilinen **kurtarma noktası hedefi** (**RPO**).

Aşağıdaki tabloda tutarlılık modeli ve veri dayanıklılığı bölge geniş kesinti varsa arasındaki ilişkiyi tanımlar. Daha güçlü tutarlılık ile dağıtılmış bir sistemde, dağıtılmış bir veritabanı bir RPO ve RTO CAP Teoremi nedeniyle sıfır olması mümkün olduğunu unutmayın. Neden hakkında daha fazla bilgi için bkz: [Azure Cosmos DB'deki tutarlılık düzeyleri](consistency-levels.md).

|**Bölgeler**|**Çoğaltma modu**|**Tutarlılık düzeyi**|**RPO**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Tek veya çok yöneticili|Herhangi bir tutarlılık düzeyi|< 240 dakika|< 1 hafta|
|>1|Tek yönetici|Oturum, tutarlı ön ek, nihai|< 15 dakika|< 15 dakika|
|>1|Tek yönetici|Sınırlanmış Eskime Durumu|*K* & *T*|< 15 dakika|
|>1|Tek yönetici|Güçlü|0|< 15 dakika|
|>1|Çok yöneticili|Oturum, tutarlı ön ek, nihai|< 15 dakika|0|
|>1|Çok yöneticili|Sınırlanmış Eskime Durumu|*K* & *T*|0|

*K* sayısı = *"K"* bir öğe (yani, güncelleştirmeleri) sürümleri.

*T* zaman aralığı = *"T"* son güncelleştirmeden bu yana.

## <a name="next-steps"></a>Sonraki adımlar

Genel dağıtım ve genel tutarlılık seçenekleri hakkında daha fazla dağıtılmış sistemlerde öğrenin. Aşağıdaki makalelere bakın:

- [Modern dağıtılmış bir veritabanı sistemleri tasarım tutarlılık seçenekleri](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Yüksek kullanılabilirlik](high-availability.md)
- [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
