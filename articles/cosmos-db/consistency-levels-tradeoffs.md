---
title: Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri | Microsoft Docs
description: Azure Cosmos DB'de çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri.
keywords: tutarlılık, performans, azure cosmos db, azure, Microsoft azure
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/20/2018
ms.author: mjbrown
ms.openlocfilehash: 061a7c223d0e1c6fa7384d7defe9f84f470236ce
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244076"
---
# <a name="availability-and-performance-tradeoffs-for-various-consistency-levels"></a>Çeşitli tutarlılık düzeyleri için kullanılabilirlik ve performans seçenekleri

Veritabanlarını çoğaltma için yüksek kullanılabilirlik, düşük gecikme süresi veya her ikisi de bağlı okuma tutarlılığı availability1 latency2 ve aktarım hızı ve temel etmekten olun. Azure Cosmos DB içeren, güçlü ve nihai tutarlılık iki uç nokta yerine seçenek olarak veri tutarlılığını yaklaşıyor. Cosmos DB, geliştiricilerin (zayıf için en güçlü) – tutarlılık spektrumu beş iyi tanımlanmış tutarlılık modellerinden arasında seçim güçlendirir **güçlü**, **sınırlanmış eskime durumu**, **oturumu** , **tutarlı ön ek**, ve **nihai**. Her beş tutarlılık modeli Temizle kullanılabilirliğini ve performansını ödünler sağlar ve kapsamlı SLA'lar ile desteklenen.

## <a name="consistency-levels-and-latency"></a>Tutarlılık düzeyleri ve gecikme süresi

- **Okuma gecikme süresi** tüm tutarlılık düzeyleri için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır ve SLA ile desteklenir. Gecikme süresi, genellikle 2 milisaniye cinsinden ortalama (50. yüzdebirlik) en okuyun veya daha az.
- Dışında çeşitli bölgeleri kapsayan Cosmos hesapları ve güçlü tutarlılık ile yapılandırılmış **yazma gecikme süresi** tüm tutarlılık düzeyleri için her zaman 10 milisaniyeden kısa 99. yüzdebirlik olması sağlanır ve tarafından desteklenir SLA. (50. yüzdebirlik), ortalama yazma gecikme süresi, genellikle 5 mili saniye olan veya daha az.
- (Şu anda önizlemede), güçlü tutarlılık ile yapılandırılmış birden fazla bölge ile Cosmos hesapları için **yazma gecikme süresi** olması garanti küçüktür < (2 * RTT) + 99. yüzdebirlik dilimde 10 mili saniye. RTT en uzak herhangi birinin Cosmos hesabınızla ilişkili iki bölgeleri arasında. Tam RTT gecikme süresi, hız, ışık uzaklığı ve tam Azure ağ topolojisi için kullanılan bir işlevdir. Azure ağı, herhangi iki Azure bölgeleri arasında RTT için herhangi bir gecikme SLA sağlamaz. Cosmos DB çoğaltma gecikmeleri, Cosmos hesabınızla ilişkili çeşitli bölgeler arasında çoğaltma gecikmeleri izlemenize olanak tanıyan Cosmos hesabınız için Azure portalında görüntülenir.

## <a name="consistency-levels-and-throughput"></a>Tutarlılık düzeyleri ve aktarım hızı

- Aynı RU miktarı için aktarım hızı için güçlü ve sınırlanmış eskime durumu kıyasla yaklaşık 2 X okuma oturum, tutarlı ön ek ve nihai tutarlılık sağlar.
- Yazma işlemi (örneğin, ekleme, değiştirme, Upsert, silme, vb.) verilen tür için RUS yazma üretimi tüm tutarlılık düzeyleri için aynıdır.

## <a name="consistency-levels-and-durability"></a>Tutarlılık düzeyleri ve dayanıklılık

Bir yazma işlemi, istemci için Onaylandı önce veriler üzerinde yazma kabul bölge içindeki bir çekirdeği tarafından dizinlendiğini. Ayrıca, kapsayıcı ile tutarlı dizin oluşturma ilkesi yapılandırdıysanız, dizin de zaman uyumlu olarak güncelleştirilir, çoğaltılan olup yazma istemciye onaylanır önce çekirdeği dizinlendiğini. 

Aşağıdaki tabloda, çeşitli bölgeleri kapsayan Cosmos hesapları için bölgesel bir olağanüstü durum olması halinde olası veri kaybı penceresini özetlenmektedir.

| **Tutarlılık düzeyi** | **Bölgesel bir olağanüstü durum olması halinde olası veri kaybı penceresini** |
| - | - |
| Güçlü | Sıfır |
| Sınırlanmış Eskime Durumu | "Cosmos hesabındaki geliştirici tarafından yapılandırılan eskime penceresine" sınırlı |
| Oturum | En fazla 5 saniye |
| Tutarlı Ön Ek | En fazla 5 saniye |
| Nihai | En fazla 5 saniye |

## <a name="next-steps"></a>Sonraki adımlar

Sonraki daha fazla genel dağıtım ve genel tutarlılık seçenekleri hakkında aşağıdaki makaleleri kullanarak dağıtılmış sistemlerde bilgi edinebilirsiniz:

* [Modern dağıtılmış bir veritabanı sistemleri tasarım tutarlılık seçenekleri](https://www.computer.org/web/csdl/index/-/csdl/mags/co/2012/02/mco2012020037-abs.html)
* [Yüksek kullanılabilirlik](high-availability.md)
* [Cosmos DB SLA'sı](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
