---
title: Ayırma indirimi Azure Cosmos DB'ye nasıl uygulanacağını anlama | Microsoft Docs
description: Ayırma indirimi Azure Cosmos DB'de sağlanan işleme (RU/s) için nasıl uygulanacağını öğrenin.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: banders
ms.reviewer: sngun
ms.openlocfilehash: 8386d1c43761cfb27746b003d136419f72d7d4ae
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58648546"
---
# <a name="understand-how-the-reservation-discount-is-applied-to-azure-cosmos-db"></a>Ayırma indirimi Azure Cosmos DB'ye nasıl uygulanacağını anlama

Ayırma indirimi, öznitelikleri ve ayırma miktarı ile eşleşen Azure Cosmos DB kaynaklarını, otomatik olarak bir Azure Cosmos DB ayrılmış kapasite satın sonra uygulanır. Azure Cosmos DB kaynaklarını için sağlanan aktarım hızı bir ayırma kapsar. Yazılım, ağ, depolama kapsamıyordur veya kapsayıcı ücretleri önceden tanımlanmış.

## <a name="reservation-discount-applied-to-azure-cosmos-db-accounts"></a>Azure Cosmos DB hesaplarına uygulanan ayırma indirimi

Ayırma indirimi uygulanan [sağlanan aktarım hızı](../cosmos-db/request-units.md) saat saatlik olarak (RU/sn) saniye başına istek birimleri. Tam bir saat çalıştırma için Azure Cosmos DB kaynaklarını, ayırma indirimini ayırma öznitelikleri eşleşen diğer Cosmos DB kaynaklarını otomatik olarak uygulanır. İndirim, eşzamanlı olarak çalışan Azure Cosmos DB kaynaklarını uygulayabilirsiniz. Tam bir saat olarak çalıştıran ve ayırma öznitelikleri eşleşen Cosmos DB kaynaklarını yoksa, ayırma indirimini bu saat için tüm avantajlarını elde etmezsiniz.

İndirimler katmanlı halde bulunan. İstek birimi daha yüksek olan rezervasyonlar daha yüksek indirim sağlar.

Rezervasyon satın alma indirimleri tüm bölgelerde üzerine bölgesel fiyatlandırmayı eşdeğer oranı uygulanır. Her bölgede rezervasyon İndirim oranları için bkz. [bölge başına ayırma indirimi](#reservation-discount-per-region) bu makalenin.

## <a name="reservation-discount-per-region"></a>Ayırma indirimi bölge başına
Ayırma indirimi, Azure Cosmos DB performans maliyetleri saat saatlik olarak uygulanır. Tek bir abonelik veya kayıtlı hesap kapsamı uygulanır. Ayırma indirimi, aşağıdaki oranları farklı bölgelerde ölçüm kullanımı için geçerlidir:

|Ölçüm tanımı  |Bölge |Oranı  |
|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - AP Güneydoğu  |  AP Güneydoğu    |   1      |
|Azure Cosmos DB - 100 RU/s/saat - AP Doğu |   AP Doğu   |    1     |
|Azure Cosmos DB - 100 RU/s/saat - AV Kuzey|  AV Kuzey       |    1     |
|Azure Cosmos DB - 100 RU/s/saat - KR Güney|    KR Güney     |     1    |
|Azure Cosmos DB - 100 RU/s/saat - AV Batı|    AV Batı     |      1   |
|Azure Cosmos DB - 100 RU/s/saat - KR Orta|   KR Orta    |       1  |
|Azure Cosmos DB - 100 RU/s/saat - UK Güney|   BK Güney      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - UK Batı|   BK Batı      |    1     |
|Azure Cosmos DB - 100 RU/s/saat - UK Kuzey |   BK Kuzey    |     1    |
|Azure Cosmos DB - 100 RU/s/saat - UK Güney 2|   BK Güney 2      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Doğu 2|  ABD Doğu 2     |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Kuzey|   ABD Orta Kuzey      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı|   ABD Batı      |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta| ABD Orta        |     1    |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı 2|   ABD Batı 2      |      1   |
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Batı|   ABD Orta Batı      |       1  |
|Azure Cosmos DB - 100 RU/s/saat - ABD Doğu|   ABD Doğu      |  1       |
|Azure Cosmos DB - 100 RU/s/saat - SA Kuzey|     SA North    |   1      |
|Azure Cosmos DB - 100 RU/s/saat - SA Batı |    SA West      |    1     |
|Azure Cosmos DB - 100 RU/s/saat - ın Güney|    IN Güney     |    1.0375    |
|Azure Cosmos DB - 100 RU/s/saat - CA Doğu|   CA Doğu      |    1.1      |
|Azure Cosmos DB - 100 RU/s/saat - JA Doğu|   JA Doğu      |    1.125     |
|Azure Cosmos DB - 100 RU/s/saat - JA Batı|     JA Batı    |   1.125       |
|Azure Cosmos DB - 100 RU/s/saat - ın Batı|     IN Batı    |    1.1375     |
|Azure Cosmos DB - 100 RU/s/saat - ın Orta|    IN Orta     |  1.1375       |
|Azure Cosmos DB - 100 RU/s/saat - AU Doğu|     AU Doğu    |   1.15       |
|Azure Cosmos DB - 100 RU/s/saat - CA Orta|  CA Orta       |   1.2       |
|Azure Cosmos DB - 100 RU/s/saat - FR Orta|   FR Orta      |    1.25      |
|Azure Cosmos DB - 100 RU/s/saat - BR Güney|  BR Güney       |   1,5      |
|Azure Cosmos DB - 100 RU/s/saat - AU Orta|   AU Orta      |   1,5      |
|Azure Cosmos DB - 100 RU/s/saat - AU Orta 2| AU Orta 2        |    1,5     |
|Azure Cosmos DB - 100 RU/s/saat - FR Güney|    FR Güney     |    1.625     |

## <a name="scenarios-that-show-how-the-reservation-discount-is-applied"></a>Ayırma indirimi nasıl uygulandığını gösteren senaryoları

Rezervasyon için aşağıdaki gereksinimleri göz önünde bulundurun:

* Gerekli aktarım hızı: 50.000 RU/sn  
* Kullanılan bölgeler: 2

Bu durumda, toplam isteğe bağlı bu iki bölgede 100 RU/s ölçümü 500 miktarı için ücretlerdir. Toplam RU/sn tüketimini 100.000 her saattir.

**Senaryo 1**

Örneğin, ABD Orta Kuzey ve ABD Batı bölgelerinde Azure Cosmos DB dağıtımları gerektiğini varsayalım. Her bölgede 50.000 RU/sn aktarım hızı tüketiminin sahiptir. Bir rezervasyon satın alma 100.000 RU/sn tamamen isteğe bağlı bir ücret dengelemeniz.

Kapsayan bir ayırma indirimi şöyle hesaplanır: aktarım hızı tüketim * reservation_discount_ratio_for_that_region. ABD Orta Kuzey ve ABD Batı bölgeleri için ayırma indirim oranı 1'dir. Bu nedenle, toplam indirimli RU/sn, 100. 000 olan. Bu değer olarak hesaplandı: 50.000 * 1 + 50.000 * 1 = 100.000 RU/sn. Normal Kullandıkça Öde fiyatları üzerinden herhangi bir ek ücret ödemek gerekmez.

|Ölçüm tanımı | Bölge |Aktarım hızı tüketim (RU/sn) |Ayırma indirimi RU/s olarak uygulandı |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - ABD Orta Kuzey  |   ABD Orta Kuzey  | 50,000  | 50,000  |
|Azure Cosmos DB - 100 RU/s/saat - ABD Batı  |  ABD Batı   |  50,000  |  50,000 |

**Senaryo 2**

Örneğin, AU Orta 2 ve FR Güney bölgelerinde Azure Cosmos DB dağıtımları gerektiğini varsayalım. Her bölgede 50.000 RU/sn aktarım hızı tüketiminin sahiptir. 100.000 bir rezervasyon satın alma (AU Orta 2 kullanım ilk indirimli varsayılarak) RU/sn gibi geçerli olacaktır:

|Ölçüm tanımı | Bölge |Aktarım hızı tüketim (RU/sn) |Ayırma indirimi RU/s olarak uygulandı |
|---------|---------|---------|---------|
|Azure Cosmos DB - 100 RU/s/saat - AU Orta 2  |  AU Orta 2   |  50,000  |  50,000   |
|Azure Cosmos DB - 100 RU/s/saat - FR Güney  |  FR Güney   |  50,000 |  15,384  |

AU Orta 2 bölgesindeki 50.000 birimleri kullanımını 75.000 RU/s olarak Faturalandırılabilir kullanım (veya normalleştirilmiş kullanım) karşılık gelir. Bu değer olarak hesaplandı: aktarım hızı tüketim * reservation_discount_ratio_for_that_region. Hesaplama 75.000 RU/sn Faturalanabilir veya normalleştirilmiş kullanım eşittir. Bu değer olarak hesaplandı: 50.000 * 1.5 = 75.000 RU/sn.

Rezervasyon satın alma, 100.000 RU/sn AU Orta 2 75.000 RU/sn uzaklığı. Bu, FR Güney bölgesine 25.000 RU/sn bırakır. Kalan 25.000 RU/s, FR Güney bölgesine 15,384 RU/sn bir ayırma oranında indirim uygulanır. İndirim değeri olarak hesaplandı: 25.000 / 1,625 15,384 RU/sn =. Kalan 34,616 RU/sn FR Güney bölgesinde normal Kullandıkça Öde fiyatları üzerinden ücretlendirilir.

Azure fatura sistemiyle rezervasyon faturalandırma avantajından, işlenen ve ayırma yapılandırma eşleşen ilk örneğine atar. Örneğin, bu AU Orta 2 Bu durumda olur.

Anlamak ve kullanım raporları faturalama Azure ayırmalarınızın uygulamayı görüntülemek için bkz: [anlamak Azure ayırma kullanım](../billing/billing-understand-reserved-instance-usage-ea.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure için ayırmaları nelerdir](../billing/billing-save-compute-costs-reservations.md)  
* [Azure Cosmos DB ayrılmış kapasite ile Azure Cosmos DB kaynaklarını için ön ödeme](../cosmos-db/cosmos-db-reserved-capacity.md)  
* [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)  
* [Azure için ayırmaları yönetme](../billing/billing-manage-reserved-vm-instance.md)  
* [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage.md)  
* [Kurumsal kayıt için ayırma kullanımını anlama](../billing/billing-understand-reserved-instance-usage-ea.md)
* [CSP abonelikleri için ayırma kullanımını anlama](https://docs.microsoft.com/partner-center/azure-reservations)