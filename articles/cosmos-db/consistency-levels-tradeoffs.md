---
title: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: f56b1b06e8be46c1a111e920cfb98bc58d4f6636
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55466872"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Tutarlılık, kullanılabilirlik ve performans seçenekleri 

Yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi için çoğaltma kullanan dağıtılmış veritabanları ödünler yapmanız gerekir. Kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı dengelemeleri arasındadır. 

Azure Cosmos DB, veri tutarlılığı seçenek yelpazesi olarak ele almaktadır. Bu yaklaşım, güçlü ve nihai tutarlılık iki uç nokta değerinden daha fazla seçenek içerir. İyi tanımlanmış beş tutarlılık spektrumu modellerde arasından seçim yapabilirsiniz. Öğesinden model güçlüden zayıfa doğru şunlardır:

- Güçlü 
- Sınırlanmış eskime durumu 
- Oturum 
- Tutarlı ön ek 
- Nihai 

Her model, kullanılabilirlik ve performans seçenekleri sağlar ve kapsamlı bir SLA ile desteklenir.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

- Okuma gecikme süresi tüm tutarlılık düzeyi için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır. Bu okuma gecikme süresi, SLA ile desteklenir. Okuma gecikme süresi, 50. yüzdebirlik ortalama genellikle 2 milisaniyeden kısadır veya daha az. Çeşitli bölgeleri kapsayan ve güçlü tutarlılık ile yapılandırılmış olan azure Cosmos hesapları bu garantisi için bir özel durumdur.

-  Yazma gecikme süresi kalan tutarlılık düzeyleri için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır. Bu yazma gecikme süresi, SLA ile desteklenir. 50. yüzdebirlik ortalama yazma gecikme süresi, genellikle 5 mili saniye olan veya daha az.

Bazı Azure Cosmos hesapları çeşitli bölgeleri güçlü tutarlılık ile yapılandırılmış olabilir. Bu durumda, yazma gecikme süresi yanı sıra, 99. yüzdebirlik dilimde 10 milisaniyeden kısa iki kez gidiş dönüş süresini (RTT) olması sağlanır. RTT herhangi birini en uzak iki bölgeleri arasında Azure Cosmos hesabınızla ilişkilendirilmiş olması gerekir. Herhangi bir Azure Cosmos hesabınızla ilişkili en uzak iki bölgeleri arasında RTT eşittir. Bu seçenek şu anda Önizleme aşamasındadır. 

Tam RTT gecikme süresi, ışık hızı uzaklık işlevi ve Azure ağ topolojisi ' dir. Azure ağı, herhangi iki Azure bölgeleri arasında RTT için herhangi bir gecikme SLA sağlamaz. Azure Cosmos hesabınız için çoğaltma gecikmeleri, Azure portalında görüntülenir. Hesabınızla ilişkili çeşitli bölgeler arasında çoğaltma gecikmeleri izlemek için Azure portalını kullanabilirsiniz.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve aktarım hızı

- İstek birimleri aynı sayıda için oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri güçlü ve sınırlanmış eskime durumu ile karşılaştırıldığında iki katı okuma aktarım hızı sağlar.

- Yazma işlemi, ekleme, değiştirme, upsert ve silme gibi belirli bir tür için tüm tutarlılık düzeyleri için yazma üretimi için istek birimi aynıdır.

## <a name="next-steps"></a>Sonraki adımlar

Genel dağıtım ve genel tutarlılık seçenekleri hakkında daha fazla dağıtılmış sistemlerde öğrenin. Aşağıdaki makalelere bakın:

* [Modern dağıtılmış bir veritabanı sistemleri tasarım tutarlılık seçenekleri](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Yüksek kullanılabilirlik](high-availability.md)
* [Azure Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
