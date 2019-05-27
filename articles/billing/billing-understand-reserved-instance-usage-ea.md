---
title: Kurumsal anlaşmalar için Azure ayırmaları kullanımını anlama
description: Kurumsal kayıt için Azure ayırma nasıl uygulanacağını anlamak için kullanım okumayı öğrenin.
author: bandersmsft
manager: yashar
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/07/2019
ms.author: banders
ms.openlocfilehash: b2452580eaecc0ab922f8e7db48676f70831a8ca
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66126855"
---
# <a name="get-enterprise-agreement-reservation-costs-and-usage"></a>Kurumsal Anlaşma ayırma maliyetleri ve kullanım bilgilerini alma

Maliyet ayırma ve kullanım verileri, Kurumsal Anlaşma müşterileri için Azure portalı ve REST API'ler kullanılabilir. Bu makale size yardımcı olur:

- Rezervasyon satın alma veri alma
- Ayırma eksik kullanım verileri alın
- Ayırma maliyetleri İtfası
- Geri ödeme ayırma kullanımı
- Ayırma tasarruf hesaplayın

Market ücretlerini, Kullanım verilerinin birleştirilir. Tek bir veri kaynağı gerçekleştirilen birinci taraf kullanım ve Market kullanım ücretleri görebilirsiniz.

## <a name="reservation-charges-in-azure-usage-data"></a>Azure kullanım verilerindeki rezervasyon ücreti

Verileri iki ayrı veri kümeleri halinde ayrılmıştır: _Gerçek maliyet_ ve _amorti edilmiş maliyet_. Bu iki veri kümesi farkı:

**Gerçek maliyet** -ile aylık faturanızı mutabık kılmak için veri sağlar. Bu veriler, rezervasyon satın almak maliyet vardır. Ayırma indirimi alınan kullanım için sıfır EffectivePrice var.

**Amorti edilmiş maliyet** -kaynak ayırma indirimi alır EffectiveCost ayrılmış örnek saatlere eşit olarak dağıtılmış maliyetidir. Veri kümesi de sahip kullanılmamış ayırma maliyetlerini. Toplam Maliyet ayırma ve kullanılmamış ayırma, ayırma günlük amorti edilmiş maliyetini sağlar.

İki veri kümesi karşılaştırması:

| Veriler | Gerçek maliyet veri kümesi | Amorti edilmiş maliyet veri kümesi |
| --- | --- | --- |
| Rezervasyon satın alma | Bu görünümde kullanılabilir.<br><br>  Bu veri filtresi ChargeType üzerinde almak için = &quot;satın alma&quot;. <br><br> Reservationıd veya ReservationName ücretlendirme yapılır hangi ayırma bilmesi için bakın.  | Bu görünüm için geçerli değildir. <br><br> Satın alma maliyetleri amorti edilmiş veri sağlanmayan. |
| effectivePrice | Ayırma indirimi alır kullanım için sıfır değerdir. | Değer, rezervasyon ayırma indirimi olan kullanımlar için saatlik günlere eşit olarak dağıtılır maliyetidir. |
| Kullanılmamış ayırma (ayırma bir gün içinde kullanılan değildi saat sayısı ve atık parasal değerini sağlar) | Bu görünümde geçerli değildir. | Bu görünümde kullanılabilir.<br><br> Bu verileri almak için filtre ChargeType üzerinde = &quot;UnusedReservation&quot;.<br><br>  Hangi ayırma potansiyelinden az kullanılmasına neden bilmek Reservationıd veya ReservationName başvurun. Rezervasyon ne kadar günün boşa budur.  |
| UnitPrice (kaynak, fiyat fiyatı) | Kullanılabilir | Kullanılabilir |

Azure kullanım verilerinde başka bilgilerine değişmiştir:

- Daha önce yaptığınız gibi Azure Reservationıd ve ReservationName, ürün ve ölçüm bilgilerini - kaldırmayacağına başlangıçta tüketilen ölçer.
- Reservationıd ve ReservationName - kendi veri alanları oldukları. Daha önce yalnızca Additionalınfo altında kullanılabilir olması için kullanılır.
- Kendi alan olarak eklenen ProductOrderId - rezervasyon sipariş kimliği.
- ProductOrderName - satın alınan ayırmanın ürün adı.
- Terim - 12 ay içinde veya 36 ay.
- RINormalizationRatio - Additionalınfo altında kullanılabilir. Bu rezervasyon kullanım kaydı uygulandığı oranıdır. Örneği boyutu esnekliği, ayırma için etkin değilse, diğer boyutları için uygulayabilirsiniz. Değer, ayırma için kullanım kaydı uygulandığından oranını gösterir.

## <a name="get-azure-consumption-and-reservation-usage-data-using-api"></a>API kullanarak Azure tüketim ve ayırma kullanım veri alma

API'sini kullanarak veri alma veya Azure portalından indirin.

Çağırmanızı [kullanım ayrıntılarını API'si](/rest/api/consumption/usagedetails/list) API sürümüyle &quot;2019-04-01-preview&quot; yeni verileri almak için. Terimler hakkında daha fazla ayrıntı için bkz: [kullanım koşulları](billing-understand-your-usage.md). Çağıran, Kurumsal Sözleşme kullanan bir kuruluş yöneticisi olmalıdır [EA portal](https://ea.azure.com). Kuruluş Yöneticileri salt okunur veri de alabilirsiniz.

Veriler kullanılabilir değil [Kurumsal müşteriler - kullanım ayrıntıları için raporlama API'lerini](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail).

Bir örnek araması API'sine şu şekildedir:

```
https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{enrollmentId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodId}/providers/Microsoft.Consumption/usagedetails?metric={metric}&amp;api-version=2019-04-01-preview&amp;$filter={filter}
```

Daha fazla bilgi {Enrollmentıd} ve {billingPeriodId} için bakın [kullanım ayrıntıları – liste](https://docs.microsoft.com/rest/api/consumption/usagedetails/list) API makalesi.

Ölçüm ve filtre hakkında bilgi aşağıdaki tabloda, için ortak bir ayırma sorunlarını çözmenize yardımcı olabilir.

| **API veri türü** | Eylem API çağrısı |
| --- | --- |
| **Tüm ücretler (kullanım ve satın alma)** | {Ölçümü} ActualCost ile değiştirin. |
| **Ayırma indirimi alındı kullanımı** | {Ölçümü} ActualCost ile değiştirin.<br><br>{Filter} değiştirin: properties/reservationId%20ne%20 |
| **Ayırma indirimi almadınız kullanımı** | {Ölçümü} ActualCost ile değiştirin.<br><br>{Filter} değiştirin: properties/reservationId%20eq%20 |
| **Amorti edilmiş ücretleri (kullanım ve satın alma)** | {Ölçümü} AmortizedCost ile değiştirin. |
| **Kullanılmamış ayırma raporu** | {Ölçümü} AmortizedCost ile değiştirin.<br><br>{Filter} değiştirin: properties/ChargeType%20eq%20'UnusedReservation' |
| **Rezervasyon satın alma** | {Ölçümü} ActualCost ile değiştirin.<br><br>{Filter} değiştirin: properties/ChargeType%20eq%20'Purchase'  |
| **Mahsup işlemleri** | {Ölçümü} ActualCost ile değiştirin.<br><br>{Filter} değiştirin: properties/ChargeType%20eq%20'Refund' |

## <a name="download-the-usage-csv-file-with-new-data"></a>Yeni verilerle kullanım CSV dosyalarını indirme

EA yöneticisinin, Azure portalından yeni kullanım verilerini içeren CSV dosyası indirebilirsiniz. Bu veriler kullanılabilir değil [EA portal](https://ea.azure.com).

Azure portalında gidin [maliyet Yönetimi + faturalandırma](https://portal.azure.com/#blade/Microsoft_Azure_Billing/ModernBillingMenuBlade/BillingAccounts).

1. Fatura hesabı seçin.
2. Tıklayın **kullanım ve Ücret**.
3. **İndir**'e tıklayın.  
![Azure portalında CSV kullanım verileri dosyasını indirmek nereye gösteren örnek](./media/billing-understand-reserved-instance-usage-ea/portal-download-csv.png)
4. İçinde **kullanımı indir + ücretleri** altında **kullanım ayrıntılarını sürüm 2** seçin **(kullanım ve satın alma) tüm ücretleri** ve indir'e tıklayın. Yineleme için **ücretleri (kullanım ve satın alma) amorti edilmiş**.

İndirdiğiniz CSV dosyaları fiili maliyet ve amorti edilmiş maliyet içerir.

## <a name="common-cost-and-usage-tasks"></a>Maliyet ve kullanım ortak görevler

Aşağıdaki bölümlerde ayırma maliyet ve kullanım verilerini görüntülemek için çoğu kişi kullanan ortak görevlerdir.

### <a name="get-reservation-purchase-costs"></a>Rezervasyon satın alma maliyetleri alın

Gerçek maliyet verileri, rezervasyon satın alma maliyetleri kullanılabilir. Filtre _ChargeType satın alma =_. Satın alma için hangi rezervasyon siparişi belirlemek için ProductOrderID bakın.

### <a name="get-underutilized-reservation-quantity-and-costs"></a>Az kullanılan ayırma miktarını ve maliyetleri alın

Amorti edilmiş maliyet veri almak ve filtre _ChargeType_ _UnusedReservation =_. Günlük kullanılmamış ayırma miktarını ve maliyet olursunuz. Verileri için bir ayırma veya rezervasyon siparişi kullanarak filtreleyebilirsiniz _Reservationıd_ ve _ProductOrderId_ alanlar, sırasıyla. Rezervasyon kullanılan % 100 idiyse, kaydı bir miktar 0 ' var.

### <a name="amortize-reservation-costs"></a>Ayırma maliyetleri İtfası

Amorti edilmiş maliyet verilerini ve kullanarak bir rezervasyon siparişi için filtre _ProductOrderID_ günlük amorti edilmiş maliyet almak için bir ayırma için.

### <a name="chargeback-for-a-reservation"></a>Geri ödeme için bir ayırma

Geri ödeme ayırma diğer kuruluşlar için abonelik, kaynak grupları veya etiketleri kullanabilirsiniz. Amorti edilmiş maliyet verilerini parasal değerini bir ayırma'nın kullanımı, aşağıdaki veri türlerini sağlar:

- Kaynakları (örneğin, bir VM)
- Kaynak grubu
- Tags
- Abonelik

### <a name="get-the-blended-rate-for-chargeback"></a>Harmanlanmış oranı geri ödemeye Al

Harmanlanmış oranını belirlemek için amorti edilmiş maliyet verileri alın ve toplam maliyeti toplama. VM'ler için Additionalınfo JSON verileri MeterName ya da ServiceType bilgiler kullanabilirsiniz. Toplam maliyeti Harmanlanmış oranı almak için kullanılan miktar bölün.

### <a name="audit-optimum-reservation-use-for-instance-size-flexibility"></a>Denetim optimum ayırma kullanma örneği için boyutu esneklik

Birden fazla miktarla _RINormalizationRatio_, Additionalınfo öğesinden. Kaç saat ayırma kullanımı, kullanım kaydı uygulandığı sonuçları gösterir.

### <a name="determine-reservation-savings"></a>Ayırma tasarruf belirleme

Amortized maliyetleri veri almak ve ayrılmış örnek için verileri filtreleyin. Daha sonra:

1. Kullandıkça Öde tahmini maliyetleri alın. Çarp _UnitPrice_ değerini _miktar_ değerleri almak için tahmini Kullandıkça Öde maliyetleri ayırma indirimi kullanım için geçerliyse kaydetmedi.
2. Ayırma maliyetleri alın. Sum _maliyet_ ayrılmış örnek için ücretli parasal değerini almak için değerler. Bu rezervasyon kullanılan ve kullanılmayan maliyetlerini içerir.
3. Tahmini tasarruf almak için Kullandıkça Öde Tahmini maliyetler ayırma maliyetlerden çıkarın.

## <a name="reservation-purchases-and-amortization-in-azure-cost-analysis"></a>Rezervasyon satın alma ve Azure maliyet analizi itfa

Ayrılmış örnek maliyeti kullanılabilir [Azure maliyet analizi Önizleme modunu](https://preview.portal.azure.com/?feature.canmodifystamps=true&amp;microsoft_azure_costmanagement=stage2&amp;Microsoft_Azure_CostManagement_arm_canary=true&amp;Microsoft_Azure_CostManagement_apiversion=2019-04-01-preview&amp;Microsoft_Azure_CostManagement_amortizedCost=true#blade/Microsoft_Azure_CostManagement/Menu/costanalysis). Varsayılan olarak, maliyet veri görünümü için gerçek maliyetidir. Amorti edilmiş maliyet geçiş yapabilirsiniz. Bir örnek aşağıda verilmiştir.

![Amorti edilmiş maliyet maliyet analizi seçmek nereye gösteren örnek](./media/billing-understand-reserved-instance-usage-ea/portal-cost-analysis-amortized-view.png)

Bir ayırma veya ücret türüne göre ücretlerinizi görmek için filtre uygulayın. Ayırma adı ayırmaları tarafından ayrılmış maliyetleri görmek üzere gruplandırma.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun.

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Sonraki adımlar

Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
- [Azure Ayrılmış VM Örnekleri ile Sanal Makinelere ön ödeme yapma](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Azure SQL Veritabanı ayrılmış kapasitesi ile SQL Veritabanı işlem kaynakları için ön ödeme yapma](../sql-database/sql-database-reserved-capacity.md)
- [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
- [Ayırma indirimi nasıl uygulanacağını anlama](billing-understand-vm-reservation-charges.md)
- [Kullandıkça Öde aboneliğinizi için ayırma kullanımını anlama](billing-understand-reserved-instance-usage.md)
- [Windows yazılım maliyetleri ile ayırmaları dahil değil](billing-reserved-instance-windows-software-costs.md)
