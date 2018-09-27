---
title: Azure'da faturalandırma ve maliyet Yönetimi Otomasyon senaryoları | Microsoft Docs
description: Ne kadar yaygın faturalandırma öğrenin ve farklı API'leri maliyet Yönetimi senaryolar verilmiştir.
services: billing
documentationcenter: ''
author: Erikre
manager: dougeby
editor: ''
tags: billing
ms.assetid: 204b15b2-6667-4b6c-8ea4-f32c06f287fd
ms.service: billing
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 6/13/2018
ms.author: erikre
ms.openlocfilehash: 97f6339e5c9532250245a97a81dade12db139e98
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227376"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Faturalandırma ve maliyet Yönetimi Otomasyon senaryoları

Faturalandırma ve maliyet Yönetimi alanı için genel senaryolar aşağıda tanımlanan ve bu senaryolarda kullanılabilecek farklı API'lere eşlenmiş. Kullanılabilir tüm API'leri ve sundukları işlevi özetini API eşlemesi senaryoya altında bulunabilir. 

## <a name="common-scenarios"></a>Genel senaryolar

Faturalandırma kullanabilirsiniz ve maliyet Yönetimi API'leri yanıt maliyet ve kullanım senaryolarına çeşitli ziyaret edin.  Anahat yaygın senaryolar aşağıda verilmiştir.

- **Fatura uzlaştırma** -Microsoft mı ücret bana doğru miktarı?  Faturamı ve kendim ben bunu hesaplar nedir?

- **Ücretleri çapraz** - göre ne kadar Kuruluşumda kimlerin ödemesi gerekir ücretlendirilirim bilebilirim?

- **En iyi duruma getirme maliyeti** -biliyorum oluşturursam ne kadar ücret, ancak nasıl alabilirim miyim Azure'da harcama para dışında daha fazla?

- **Maliyet İzlemesi** – ne kadar ı harcama ve zaman içinde Azure kullanarak görmek istiyorum. Eğilimleri nelerdir? Nasıl daha iyi yapıyor?

- **Azure harcamalarınızı ay boyunca** – ne kadar my geçerli ay için tarih harcama kullanıcının? My harcama ve/veya Azure kullanımını içinde ayarlamaları yapın gerekiyor mu? Ay içinde ne zaman miyim en çok kullanan Azure miyim?

- **Uyarıları Ayarlama** – kaynak tabanlı tüketim veya parasal tabanlı uyarı ayarlamak istiyorsunuz.

## <a name="scenario-to-api-mappings"></a>Senaryo API eşlemeleri

|         API/senaryo        | Fatura uzlaştırma    | Çapraz ücretleri    | Maliyet iyileştirme    | Maliyet İzlemesi    | Ay harcama    | Uyarılar    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Bütçeler                     |                           |                  |           X          |                  |                    |     X     |
| Pazar                |             X             |         X        |           X          |         X        |          X         |     X     |
| Fiyat Listesi                 |             X             |         X        |           X          |         X        |          X         |           |
| Ayırma önerileri |                           |                  |           X          |                  |                    |           |
| Rezervasyon ayrıntıları         |                           |                  |           X          |         X        |                    |           |
| Ayırma özetleri       |                           |                  |           X          |         X        |                    |           |
| Kullanım ayrıntıları               |             X             |         X        |           X          |         X        |          X         |     X     |
| Faturalama dönemleri             |             X             |         X        |           X          |         X        |                    |           |
| Faturalar                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Derecelendirilmemiş kullanımı               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Aşağıdaki API eşlemeleri senaryoya Kurumsal tüketim API'leri dahil etmeyin. Mümkünse, lütfen ilerletme net yeni geliştirme senaryoları için genel tüketim API'leri kullanarak.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
(*Web Direct + Kurumsal müşteriler için tüm API'leri hariç bu çağrılan out aşağıda*)

-   **Bütçe** (*yalnızca Kurumsal müşteriler*): kullanım [bütçelerini API](https://docs.microsoft.com/rest/api/consumption/budgets) kaynakları, kaynak grupları ve faturalandırma ölçümlerinde maliyet ya da kullanım bütçeleri oluşturmak için.  Bütçe oluşturduktan sonra uyarı kullanıcı tanımlı bütçe eşikleri aşıldığında bildirmek için yapılandırılabilir. Eylemler, bütçe tutarları ulaşıldığında gerçekleşecek şekilde de yapılandırılabilir.
-   **Pazar**: kullanım [Market ücretleri API](https://docs.microsoft.com/rest/api/consumption/marketplaces) ücret ve kullanım verileri tüm Market kaynakları almak için (Azure 3. taraf teklifleri). Bu veriler, tüm Market kaynaklar arasında maliyetleri eklemek veya maliyetleri/kullanım belirli kaynaklar üzerinde araştırmak için kullanılabilir.
-   **Fiyat listesi** (*yalnızca Kurumsal müşteriler*): Kurumsal müşteriler [fiyat listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) kendi özel fiyatlandırma için tüm ölçümleri almak için. Kuruluşlar veri kullanımı ve Market kullanarak maliyet hesaplamaları gerçekleştirmek için bu verileri kullanım ayrıntılarını ve marketlerden kullanım bilgileri ile birlikte kullanabilirsiniz. 
-   **Ayırma önerileri**: kullanım [ayırma öneriler API'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) ayrılmış VM örnekleri satın alma önerileri almak için. Öneriler değeri, müşterilerin beklenen maliyet tasarrufu analiz edin ve tutarları satın tanır şekilde tasarlanmıştır.
-   **Rezervasyon ayrıntıları**: kullanım [rezervasyon ayrıntıları API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) ne kadar tüketimi ne kadar gerçekten kullanılan ve ayrılmış gibi önceden satın alınan VM rezervasyonlar bilgileri görmek için. Her VM-düzey ayrıntı verileri görebilirsiniz.
-   **Ayırma özetleri**: kullanım [ayırma özetleri API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) ne kadar tüketimi ne kadar gerçekten kullanılan ve ayrılmış gibi önceden satın alınan VM ayırmalar, toplam bilgileri görmek için toplama. 
-   **Kullanım ayrıntıları**: kullanım [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) tüm Azure'da ücretsiz olarak ve kullanım almak için 1. taraf kaynaklar. Şu anda bir kez günde ölçüm başına yayılan kullanım ayrıntı kaydı biçiminde bilgilerdir. Bilgiler, tüm kaynaklar arasında maliyetleri eklemek veya maliyetleri/kullanım belirli kaynaklar üzerinde araştırmak için kullanılabilir.
-   **RateCard**: Web Direct müşterilerine kullanabileceğiniz [RateCard API'si](https://msdn.microsoft.com/library/azure/mt219005.aspx) kendi ölçüm fiyatları almak için. Ardından döndürülen bilgi ile kendi kaynak kullanım bilgilerini el ile beklenen faturanıza yansıyan tutarı hesaplamak için kullanabilirler. 
-   **Kullanım derecelendirilmemiş**: kullanabileceğiniz [derecelendirilmemiş kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003.aspx) tüm ölçüm/Azure tarafından yapılan şarj önce ham kullanım bilgileri edinmek için.

### <a name="billing"></a>Faturalandırma
-   **Faturalama dönemleri**: kullanım [faturalandırma dönemlerini API'si](https://docs.microsoft.com/rest/api/billing/billingperiods) çözümlemek, fatura yanı sıra için bir faturalandırma döneminde belirlemek için bu dönem için kimliğin. Fatura kimliğin aşağıdaki fatura API ile kullanılabilir. 
-   **Faturalar**: kullanım [faturaları API](https://docs.microsoft.com/rest/api/billing/invoices) PDF formunda belirli bir fatura dönemi için fatura için indirme URL'sini almak için.

### <a name="enterprise-consumption"></a>Kurumsal tüketim
*(Tüm API'leri kuruluş yalnızca)*

-   **Bakiye özeti**: kullanım [bakiye özeti API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) aylık özeti bakiyelerini, yeni satın alma işlemleri, Azure Market hizmet ücretlerini, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi almak için. Geçerli fatura dönemi veya geçmişte herhangi bir nokta için bu bilgileri elde edebilirsiniz. Kuruluşlar, bu verileri el ile hesaplanan Özet Ücretlerle bir karşılaştırma gerçekleştirmek için kullanabilirsiniz. Bu API, kaynağa özgü bilgileri veya maliyetleri birleşik bir görünümünü sağlamaz.
-   **Kullanım ayrıntılarını**: kullanım [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) 1. taraf Azure kullanım geçerli ay, belirli bir fatura dönemi veya bir özel bir tarih dönemi için ayrıntılı bilgileri almak için. Kurumlar, bu verileri el ile fatura oranı ve tüketim göre hesaplamak için kullanabilirsiniz ve bölüm/kuruluş bilgileri özniteliği maliyetleri mevcut kuruluşlar arasında da kullanabilirsiniz. Veri kullanım/maliyeti kaynağa özgü bir görünümünü sağlar.
-   **Market depolama ücreti**: kullanım [Market Store ücret API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) 3. taraf Azure kullanım geçerli ay, belirli bir fatura dönemi veya bir özel bir tarih dönemi için ayrıntılı bilgileri almak için. Kurumlar, bu verileri el ile fatura oranı ve tüketim göre hesaplamak için kullanabilirsiniz ve bölüm/kuruluş bilgileri özniteliği maliyetleri mevcut kuruluşlar arasında da kullanabilirsiniz. Market depolama ücreti API kullanım/maliyeti kaynağa özgü bir görünümünü sağlar.
-   **Fiyat listesi**: [fiyat listesi API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) verilen kayıt ve faturalama dönemi için geçerli hızı için her bir ölçüm sağlar. Bu fiyat bilgileri birlikte kullanım ayrıntılarını ve marketlerden kullanım bilgilerini el ile beklenen faturanıza yansıyan tutarı hesaplamak için kullanılabilir.
-   **Faturalama dönemleri**: kullanım [faturalandırma dönemlerini API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) fatura dönemleri dört söz konusu fatura dönemindeki - BalanceSummary, ilgili Kurumsal API'si veri kümelerini API yolunu işaret eden bir özellik ile birlikte bir listesini almak için UsageDetails, Market ücretlerini ve fiyat listesi.
-   **Azure ayırma önerileri**: [ayrılmış örnek öneriler API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) müşterinin 30 gün içinde veya sanal makine kullanımına ait 60 gün olan 7 günde arar ve tek ve paylaşılan satın alma önerileri sunar. Ayrılmış örnek API'analiz etmek müşteriler sağlar, beklenen maliyet tasarrufu ve satın alma tutarları önerilir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-is-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Kurumsal raporlama API'lerini ve tüketim API'ları arasındaki fark nedir? Her ne zaman kullanmalıyım?
Bu API'ler, benzer bir işlev kümesi olması ve aynı sayıda faturalandırma ve maliyet Yönetimi alanındaki sorular yanıt verebilirsiniz. Ancak, her API, farklı Hedef Kitleleri hedefler: 

- **Kurumsal raporlama API'lerini**: Bu API üzerinde anlaşılan parasal taahhüdüyle ve özel fiyatlandırma erişim verir Microsoft ile Kurumsal Sözleşme imzalayan müşteriler tarafından kullanılabilir. API'ler aracılığıyla edinilen kullanmak için anahtar gerektiren [Enterprise Portal](https://ea.azure.com). Bu API'ler ile ilgili açıklama için bkz: [genel bakış, raporlama API'lerini Kurumsal müşteriler için](billing-enterprise-api.md).

- **Tüketim API'leri**: Bu API'ler birkaç istisna dışında tüm müşteriler tarafından kullanılabilir. Daha fazla bilgi için [Azure kullanım API'si genel bakış](billing-consumption-api-overview.md) ve [Azure tüketim API Başvurusu](https://docs.microsoft.com/rest/api/consumption/). Sağlanan API'leri, en son geliştirme senaryoları için önerilen çözümdür. 

### <a name="what-is-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Kullanım ayrıntılarını API'si ve kullanım API'si arasındaki fark nedir?
Bu API'ler, tamamen farklı veri sağlayın:

- **Kullanım ayrıntılarını**: [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) sağlayan Azure ölçüm örnek başına kullanım ve maliyet bilgileri. Sağlanan verileri zaten kadar Azure'un ücretsiz sistem ölçümünü geçirilen ve vardı yanı sıra diğer olası değişikliklerin uygulanmış Maliyet:

    - Ön ödemeli parasal taahhüt kullanımı için hesap değişiklikleri
    - Azure tarafından bulunan kullanım farklılıklarını hesaba değişiklikleri

- **Kullanım**: [kullanım API'si](https://msdn.microsoft.com/library/Mt219003.aspx) maliyet Ölçüm sistemi Azure geçirmeden önce ham Azure kullanım bilgileri sağlar. Bu veriler, Azure Ücretli sonra Ölçüm sistemi görülür kullanım ve/veya ücret tutarı ile herhangi bir ilişki olmayabilir.

### <a name="what-is-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Fatura API kullanım ayrıntılarını API'si arasındaki fark nedir?
Bu API'leri, aynı verileri farklı bir görünümünü sağlar. [Fatura API](https://docs.microsoft.com/rest/api/billing/invoices) yalnızca Web Direct müşterilerine yöneliktir ve faturanızda toplam ücretleri her bir ölçüm türü temel yukarı aylık bir dökümünü sağlar. [Kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) her gün için kullanım/maliyeti kayıtları ayrıntılı bir görünümünü sağlar ve hem kurumsal hem de doğrudan Web müşterileri tarafından kullanılabilir.

### <a name="what-is-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Fiyat listesi API'si ve RateCard API'si arasındaki fark nedir?
Bu API'ler benzer veri kümesi sağlar, ancak farklı bir hedef kitlelere sahip. Aşağıdaki bilgiler.

- Fiyat listesi API'si: [fiyat listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) özel fiyatlandırma, kurumsal bir müşteri için anlaşma sağlar.
- RateCard API'si: [RateCard API'si](https://msdn.microsoft.com/library/mt219005.aspx) sağlar, fiyatlandırma, genel kullanıma Web Direct müşterileri için geçerlidir.

## <a name="next-steps"></a>Sonraki Adımlar

- Program aracılığıyla Azure kullanımınızı öngörü almak için Azure API'leri kullanma hakkında daha fazla bilgi için bkz: [Azure tüketim API'sine genel bakış](billing-consumption-api-overview.md) ve [Azure faturalama API'sine genel bakış](billing-usage-rate-card-overview.md).
- Ayrıntılı günlük kullanım dosyası ve Azure portalında maliyet Yönetimi raporlarını faturanızı karşılaştırmak için bkz [Microsoft Azure için faturanızı anlayın](billing-understand-your-bill.md)
- Hala başka sorularınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
