---
title: Ayırma indirimi Azure Cosmos DB'ye nasıl uygulanacağını anlama | Microsoft Docs
description: Ayırma indirimi Azure Cosmos DB'de sağlanan işleme (RU/s) için nasıl uygulanacağını öğrenin.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: cwatson
ms.reviewer: sngun
ms.openlocfilehash: adcd91a8f1b3368d03f4b634e7aef40104d953e3
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393648"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Ayırma indirimi Azure Cosmos DB'ye nasıl uygulanacağını anlama

Ayırma indirimi, öznitelikleri ve ayırma miktarı ile eşleşen Azure Cosmos DB kaynaklarını, otomatik olarak bir Azure Cosmos DB ayrılmış kapasite satın sonra uygulanır. Azure Cosmos DB kaynaklarını için sağlanan aktarım hızı bir ayırma kapsar ve yazılım, ağ, depolama kapsamıyordur veya kapsayıcı ücretleri önceden tanımlanmış.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>Azure Cosmos DB hesaplarına uygulanan ayırma indirimi

Ayırma indirimi uygulanan [sağlanan aktarım hızı](../cosmos-db/request-units.md) saat saatlik olarak second(RU/s) başına istek birimleri. Tam bir saat çalıştırma için Azure Cosmos DB kaynaklarını, ayırma indirimini ayırma öznitelikleri eşleşen diğer Cosmos DB kaynaklarını otomatik olarak uygulanır. İndirim, eşzamanlı olarak çalışan Azure Cosmos DB kaynaklarını uygulayabilirsiniz. Tam bir saat olarak çalıştıran ve ayırma öznitelikleri eşleşen Cosmos DB kaynaklarını yoksa, ayırma indirimini bu saat için tüm avantajlarını elde etmezsiniz.

* İndirimler daha yüksek İndirim oranları yüksek istek birimleri ile ayırmaları sağlamak anlamına katmanlı halde bulunan.  
* Rezervasyon satın alma indirimleri tüm bölgelerde üzerine bölgesel fiyatlandırmayı eşdeğer oranı uygulanır. Her bölgede İndirim oranları için ayırma bkz [bölge başına ayırma indirimi](#reservation-discount-per-region) bu makalenin.

## <a name="reservation-discount-per-region"></a>Ayırma indirimi bölge başına
Ayırma indirimi, Azure Cosmos DB aktarım hızı maliyetleri tek bir abonelik veya kayıtlı hesap kapsamı saat saatlik olarak uygulanır. Ayırma indirimi, aşağıdaki oranı farklı bölgelerde ölçüm kullanımı için geçerlidir:

|Ölçüm tanımı  |Bölge |Oranı  |
|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - AP Güneydoğu  |  AP Güneydoğu    |   1      |
|Azure Cosmos DB - 100 RU/s/saat - AP Doğu |   AP Doğu   |    1     |
|Azure Cosmos DB - 100 RU/s/saat - AV Kuzey|  AV Kuzey       |    1     |
|Azure Cosmos DB - 100 RU/s/saat - KR Güney|    KR Güney     |     1    |
|Azure Cosmos DB - 100 RU/s/saat - AV Batı|    AV Batı     |      1   |
|Azure Cosmos DB - 100 RU/s/saat - KR Orta|   KR Orta    |       1  |
|Azure Cosmos DB - 100 RU/s/saat - UK Güney|   Birleşik Krallık Güney      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - UK Batı|   Birleşik Krallık Batı      |    1     |
|Azure Cosmos DB - 100 RU/s/saat - UK Kuzey |   UK Kuzey    |     1    |
|Azure Cosmos DB - 100 RU/s/saat - UK Güney 2|   UK Güney 2      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Doğu 2|  ABD Doğu 2     |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Kuzey|   ABD Orta Kuzey      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı|   ABD Batı      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta| ABD Orta        |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı 2|   ABD Batı 2      |      1   |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Batı|   ABD Orta Batı      |       1  |
|Azure Cosmos DB - 100 RU/s/saat - ABD Doğu|   ABD Doğu      |  1       |
|Azure Cosmos DB - 100 RU/s/saat - SA Kuzey|     SA Kuzey    |   1      |
|Azure Cosmos DB - 100 RU/s/saat - SA Batı |    SA Batı      |    1     |
|Azure Cosmos DB - 100 RU/s/saat - ın Güney|    IN Güney     |    1.0375    |
|Azure Cosmos DB - 100 RU/s/saat - CA Doğu|   CA Doğu      |    1.1      |
|Azure Cosmos DB - 100 RU/s/saat - JA Doğu|   JA Doğu      |    1,125     |
|Azure Cosmos DB - 100 RU/s/saat - JA Batı|     JA Batı    |   1,125       |
|Azure Cosmos DB - 100 RU/s/saat - ın Batı|     IN Batı    |    1.1375     |
|Azure Cosmos DB - 100 RU/s/saat - ın Orta|    IN Orta     |  1.1375       |
|Azure Cosmos DB - 100 RU/s/saat - AU Doğu|     AU Doğu    |   1.15       |
|Azure Cosmos DB - 100 RU/s/saat - CA Orta|  CA Orta       |   1.2       |
|Azure Cosmos DB - 100 RU/s/saat - FR Orta|   FR Orta      |    1,25      |
|Azure Cosmos DB - 100 RU/s/saat - BR Güney|  BR Güney       |   1,5      |
|Azure Cosmos DB - 100 RU/s/saat - AU Orta|   AU Orta      |   1,5      |
|Azure Cosmos DB - 100 RU/s/saat - AU Orta 2| AU Orta 2        |    1,5     |
|Azure Cosmos DB - 100 RU/s/saat - FR Güney|    FR Güney     |    1.625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Ayırma indirimi nasıl uygulandığını gösteren senaryoları

Rezervasyon için aşağıdaki gereksinimleri göz önünde bulundurun:

* Gerekli aktarım hızı: 50.000 RU/sn  
* Kullanılan bölgeler: 2. 

Bu durumda, toplam isteğe bağlı bu iki bölgeleri 100 RU/sn ölçer toplam RU/sn tüketimi saatte 100.000 500 miktarı için ücretlerdir. 

**Senaryo 1**

Örneğin, "Kuzey Orta ABD" ve "ABD Batı" bölgelerdeki Azure Cosmos DB dağıtımın ihtiyacınız varsa ve her bölgede 50.000 RU/sn aktarım hızı tüketiminin varsa. Bir rezervasyon satın alma 100.000 RU/sn tamamen isteğe bağlı bir ücret dengelemeniz.

Ayırma tarafından kapsanan indirim olarak hesaplanır (aktarım hızı tüketim * reservation_discount_ratio_for_that_region). "Kuzey Orta ABD" ve "ABD Batı" bölgeleri için ayırma indirim oranı "1" dır. Bu nedenle, 100.000 RU/sn olan toplam indirimli RU/sn (Bu değer olarak hesaplandı: 50.000 * 1 + 50.000 * 1 = 100.000 RU/sn), ve normal Kullandıkça Öde fiyatları üzerinden herhangi bir ek ücret ödemek gerekmez. 

|Ölçüm tanımı | Bölge |Aktarım hızı tüketim (RU/sn) |Ayırma indirimi RU/s olarak uygulandı |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Kuzey  |   ABD Orta Kuzey  | 50,000  | 50,000  |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı  |  ABD Batı   |  50,000  |  50,000 |

**Senaryo 2**

Örneğin, "AU Orta 2" ve "FR Güney" bölgelerde Azure Cosmos DB dağıtımları ihtiyacınız varsa ve her bölgede 50.000 RU/sn aktarım hızı tüketiminin varsa. 100.000 bir rezervasyon satın alma (AU Orta 2 kullanım ilk indirimli varsayılarak) RU/sn gibi geçerli olacaktır:

|Ölçüm tanımı | Bölge |Aktarım hızı tüketim (RU/sn) |Ayırma indirimi RU/s olarak uygulandı |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - AU Orta 2  |  AU Orta 2   |  50,000  |  50,000   |
|Azure Cosmos DB - 100 RU/s/saat - FR Güney  |  FR Güney   |  50,000 |  15,384  |

50.000 birimleri kullanımı "AU Orta 2" bölgesinde 75.000 RU/s olarak Faturalandırılabilir kullanım (veya normalleştirilmiş kullanım) karşılık gelir. Bu değer hesaplanır (aktarım hızı tüketim * reservation_discount_ratio_for_that_region) 75.000 RU/s değerine eşittir (Bu değer olarak hesaplandı: 50.000 * 1.5 = 75.000 RU/sn) Faturalanabilir veya normalleştirilmiş kullanımı. 

Rezervasyon satın alma, 100.000 RU/sn, "AU Orta 2" 75.000 RU/sn uzaklığı ve 25.000 RU/sn "FR Güney" bölgeye bırakır. Kalan 25.000 RU/s, bir ayırma indirimi 15,384 RU/sn (Bu değer olarak hesaplandı: 25.000 / 1,625 = 15,384 RU/s) "FR Güney" bölgesi için uygulanır. Kalan 34,616 RU/sn "FR Güney" bölgedeki normal Kullandıkça Öde fiyatları üzerinden ücretlendirilir. 

Azure fatura sistemiyle rezervasyon faturalandırma avantajından ayırma yapılandırma (örneğin, AU Orta 2 Bu durumda) ile eşleşen işlenen ilk örneğine atar.

Anlamak ve kullanım raporları faturalama Azure ayırmalarınızın uygulamayı görüntülemek için bkz: [anlamak Azure ayırma kullanım](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure ayırmaları nelerdir?](../billing/billing-save-compute-costs-reservations.md)  
* [Azure Cosmos DB ayrılmış kapasite ile Azure Cosmos DB kaynaklarını için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Azure SQL veritabanı'nın ayrılmış kapasite ile SQL veritabanı bilgi işlem kaynakları için ön ödeme](../sql-database/sql-database-reserved-capacity.md)  
* [Azure Ayırmalarını yönetme](../billing/billing-manage-reserved-vm-instance.md)  
* [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage.md)  
* [Kurumsal kayıt için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md)  
* [CSP abonelikleri için ayırma kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.

