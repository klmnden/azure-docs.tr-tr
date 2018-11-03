---
title: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri | Microsoft Docs
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri.
keywords: tutarlılık, performans, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 8f36026c7e5802994b8cf22d60c6ecea052e6382
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50963056"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels-in-azure-cosmos-db"></a>Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri

Çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı olan dağıtılmış veritabanları kullanılabilirlik, gecikme süresi ve aktarım hızı ve okuma tutarlılığı temel etmekten olun. Azure Cosmos DB içeren, güçlü ve nihai tutarlılık iki uç nokta yerine seçenek olarak veri tutarlılığını yaklaşıyor. Cosmos DB, geliştiriciler (zayıf için en güçlü) – tutarlılık yelpazesinden arasında beş iyi tanımlanmış tutarlılık modeli seçmek için güçlendirir **güçlü**, **sınırlanmış eskime durumu**, **oturumu** , **tutarlı ön ek**, ve **nihai**. Her beş tutarlılık modeli, kullanılabilirlik ve performans seçenekleri sağlar ve kapsamlı SLA'lar ile desteklenen.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

- **Okuma gecikme süresi** tüm tutarlılık düzeyleri için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır ve SLA ile desteklenir. Gecikme süresi, genellikle 2 milisaniye cinsinden ortalama (50. yüzdebirlik) en okuyun veya daha az.

- Çeşitli bölgeleri kapsayan ve güçlü tutarlılık ile yapılandırılan Cosmos hesapları dışında **yazma gecikme süresi** için kalan tutarlılık düzeyleri her zaman garantisi 99th en az 10 milisaniyeden olması yüzdebirlik. Bu yazma gecikme süresi, SLA ile desteklenir. (50. yüzdebirlik), ortalama yazma gecikme süresi, genellikle 5 mili saniye olan veya daha az.

- Çeşitli bölgeleri (şu anda önizlemede), güçlü tutarlılık ile yapılandırılmış olan Cosmos hesaplarıyla için **yazma gecikme süresi** olması garanti küçüktür < (2 * gidiş dönüş süresi/RTT) + 99. yüzdebirlik dilimde 10 mili saniye. Cosmos hesabınızla ilişkili iki en uzak bölgelerden arasında RTT. Tam RTT gecikme süresi, hız, ışık uzaklığı ve tam Azure ağ topolojisi için kullanılan bir işlevdir. Azure ağı, herhangi iki Azure bölgeleri arasında RTT için herhangi bir gecikme SLA sağlamaz. Cosmos DB çoğaltma gecikmeleri, Cosmos hesabınızla ilişkili çeşitli bölgeler arasında çoğaltma gecikmeleri izlemenize olanak tanıyan Cosmos hesabınız için Azure portalında görüntülenir.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve aktarım hızı

- Aynı istek birimleri sayısı için güçlü ve sınırlanmış eskime durumu ile karşılaştırıldığında aktarım hızı yaklaşık 2 X okuma oturum, tutarlı ön ek ve nihai tutarlılık düzeyleri sağlar.

- Yazma işlemi ekleme, değiştirme, upsert, silme, vb. gibi belirli bir tür için tüm tutarlılık düzeyleri için yazma üretimi için istek birimi aynıdır.

## <a name="consistency-levels-and-durability"></a>Tutarlılık düzeyleri ve dayanıklılık

Bir yazma işlemi, istemci için Onaylandı önce veri yazma işlemleri kabul eden bir bölgedeki bir çekirdeği tarafından depolanmasına kararlıdır. Ayrıca, kapsayıcı ile tutarlı dizin oluşturma ilkesi yapılandırdıysanız, dizini ayrıca eş zamanlı olarak güncelleştirilen çoğaltılan ve yazma işlemini onaylanmasını istemciye gönderilmeden önce çekirdeği dizinlendiğini.

Aşağıdaki tabloda, çeşitli bölgeleri kapsayan Cosmos hesapları için bölgesel bir olağanüstü durum olması halinde olası veri kaybı penceresini özetlenmektedir.

| **Tutarlılık düzeyi** | **Bölgesel bir olağanüstü durum olması halinde olası veri kaybı penceresini** |
| - | - |
| Güçlü | Sıfır |
| Sınırlanmış Eskime Durumu | "Cosmos hesapta yapılandırma eskime penceresine" sınırlı. |
| Oturum | En fazla 5 saniye |
| Tutarlı Ön Ek | En fazla 5 saniye |
| Nihai | En fazla 5 saniye |

## <a name="next-steps"></a>Sonraki adımlar

Sonraki daha fazla genel dağıtım ve genel tutarlılık seçenekleri hakkında aşağıdaki makaleleri kullanarak dağıtılmış sistemlerde bilgi edinebilirsiniz:

* [Modern dağıtılmış bir veritabanı sistemleri tasarım tutarlılık seçenekleri](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Yüksek kullanılabilirlik](high-availability.md)
* [Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
