---
title: Azure'da faturalandırma ve maliyet yönetimi için Otomasyon senaryoları | Microsoft Docs
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
ms.openlocfilehash: cb7a13d9abcf7c677d51f03df002ea06b543014e
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232470"
---
# <a name="automation-scenarios-for-billing-and-cost-management"></a>Faturalandırma ve maliyet yönetimi için Otomasyon senaryoları

Bu makalede, Azure'da faturalandırma ve maliyet yönetimi için genel senaryolar listelenmektedir. Bu senaryolar için kullanabileceğiniz API'ler eşler. Her senaryo için API eşlemesi altında API'leri ve sundukları işlevselliği özeti bulabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Faturalandırma ve maliyet Yönetimi API'leri çeşitli senaryolarda maliyet ve kullanım ilgili soruları yanıtlamak için kullanabilirsiniz. Anahat yaygın senaryolar aşağıda verilmiştir:

- **Fatura uzlaştırma**: Microsoft benim doğru miktarı ücret?  Faturamı nedir ve ben bunu kendim hesaplayabilirsiniz?

- **Arası ücretleri**: Ne kadar Kuruluşumda kimlerin ödemesi gerekir ücretlendirilirim biliyorum göre?

- **En iyi duruma getirme maliyeti**: Oluşturursam ne kadar ücret biliyorum. Nasıl Azure'da geçiriyorum para dışında daha fazla bilgi edinebilirim?

- **Maliyet İzlemesi**: Ne kadar ı harcama ve zaman içinde Azure kullanarak görmek istiyorsunuz. Eğilimleri nelerdir? Nasıl daha iyi yapabilirim?

- **Azure ay boyunca harcama**: Ne kadar my geçerli ayın tarih için harcıyorsa? My harcama ve/veya Azure kullanımını değişiklik yapmak gerekiyor mu? Ay içinde ne zaman miyim en çok kullanan Azure miyim?

- **Uyarılar**: Kaynak tabanlı tüketim veya parasal tabanlı uyarılar ayarlama nasıl ayarlayabilirim?

## <a name="scenario-to-api-mapping"></a>Senaryo API eşlemesi

|         API        | Fatura uzlaştırma    | Arası ücretleri    | Maliyet iyileştirmesi    | Maliyet İzlemesi    | Midmonth harcama    | Uyarılar    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Bütçeler                     |                           |                  |           X          |                  |                    |     X     |
| Market Ücretleri                |             X             |         X        |           X          |         X        |          X         |     X     |
| Fiyat Listesi                 |             X             |         X        |           X          |         X        |          X         |           |
| Rezervasyon Önerileri |                           |                  |           X          |                  |                    |           |
| Rezervasyon Ayrıntıları         |                           |                  |           X          |         X        |                    |           |
| Rezervasyon Özetleri       |                           |                  |           X          |         X        |                    |           |
| Kullanım Ayrıntıları               |             X             |         X        |           X          |         X        |          X         |     X     |
| Faturalama dönemleri             |             X             |         X        |           X          |         X        |                    |           |
| Faturalar                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Derecelendirilmemiş kullanımı               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Senaryo için API eşlemesi Kurumsal tüketim API'leri içermez. Mümkünse, yeni geliştirme senaryoları için genel tüketim API'leri kullanın.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
Doğrudan Web ve kurumsal müşterilere belirtilenler dışında tüm aşağıdaki API'leri kullanabilirsiniz:

-   [Bütçe API](https://docs.microsoft.com/rest/api/consumption/budgets) (*yalnızca Kurumsal müşteriler*): Kaynaklar, kaynak grupları veya faturalandırma ölçümlerinde maliyet ya da kullanım bütçeleri oluşturun. Bütçelerini oluşturduğunuz aşıldığında bütçe eşikleri tanımladığınız bilgilendirmek üzere uyarılar yapılandırabilirsiniz. Bütçe tutarları ulaştınız meydana gelmesini eylemleri de yapılandırabilirsiniz.

-   [Market ücretleri API](https://docs.microsoft.com/rest/api/consumption/marketplaces): Tüm Azure Market kaynakları (Azure iş ortağı teklifleri) ücret ve kullanım verileri alın. Tüm Market kaynaklar arasında maliyetleri eklemek veya belirli kaynaklarla ilgili maliyetleri/kullanım araştırmak için bu verileri kullanabilirsiniz.

-   [Fiyat listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) (*yalnızca Kurumsal müşteriler*): Özel fiyatlandırma için tüm ölçümleri alın. Kuruluşlar bu veriler kullanım ayrıntıları ve Market kullanım bilgileri ile birlikte kullanım ve Market verileri kullanarak maliyetleri hesaplamak için kullanabilirsiniz. 

-   [Ayırma öneriler API'si](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations): Ayrılmış VM örnekleri satın almak için öneriler alın. Öneriler beklenen maliyet tasarrufu çözümlemenize ve tutarları satın yardımcı olur. Daha fazla bilgi için [Otomasyon Azure ayırma için API'leri](billing-reservation-apis.md).

-   [Rezervasyon ayrıntıları API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails): Ne kadar kullanıldığını ve ne kadar tüketimi ayrılmış gibi önceden satın alınan VM ayırmaları bilgilere bakın. Her VM-düzey ayrıntı verileri görebilirsiniz. Daha fazla bilgi için [Otomasyon Azure ayırma için API'leri](billing-reservation-apis.md).

-   [Ayırma özetleri API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries): Ne kadar tüketimi ne kadar toplama kullanılır ve ayrılmış gibi kuruluşunuzun satın VM ayırmaları toplanan bilgilere bakın. Daha fazla bilgi için [Otomasyon Azure ayırma için API'leri](billing-reservation-apis.md).

-   [Kullanım ayrıntılarını API](https://docs.microsoft.com/rest/api/consumption/usagedetails): Tüm Azure kaynaklarına gider ve kullanım bilgilerini Microsoft'tan alın. Şu anda bir kez günde ölçüm başına yayılan kullanım ayrıntı kaydı biçiminde bilgilerdir. Tüm kaynaklar arasında maliyetleri eklemek veya belirli kaynaklarla ilgili maliyetleri/kullanım araştırmak için bilgileri kullanabilirsiniz.

-   [RateCard API'si](/previous-versions/azure/reference/mt219005(v=azure.100)): Web Direct müşterisiyseniz ölçüm fiyatlar alabilirsiniz. Ardından kaynak kullanım bilgilerinizi döndürülen bilgileri el ile beklenen faturanıza yansıyan tutarı hesaplamak için kullanabilirsiniz. 

-   [Kullanım API'si derecelendirilmemiş](/previous-versions/azure/reference/mt219003(v=azure.100)): Azure, tüm ölçüm/şarj yapmadan önce ham kullanım bilgilerini alın.

### <a name="billing"></a>Faturalama
-   [Faturalama dönemleri API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): ' % S'fatura kimlikleri bu süre için birlikte analiz etmek için bir faturalandırma döneminde belirleyin. Fatura kimlikleri faturaları API ile kullanabilirsiniz.

-   [Faturaları API](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices): İndirme URL'si için fatura PDF formundaki bir fatura dönemi için alın.

### <a name="enterprise-consumption"></a>Kurumsal tüketim
Aşağıdaki API'leri kuruluş için yalnızca şunlardır:

-   [Bakiye özeti API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary): Aylık özeti bakiyelerini, yeni satın alma işlemleri, Azure Market hizmet ücretlerini, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi alın. Geçerli fatura dönemi veya geçmişte herhangi bir nokta için bu bilgileri elde edebilirsiniz. Kuruluşlar, el ile hesaplanan Özet ücretleri ile karşılaştırmak için bu verileri kullanabilirsiniz. Bu API, kaynağa özgü bilgileri veya maliyetleri birleşik bir görünümünü sağlamaz.

-   [Kullanım ayrıntılarını API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail): Geçerli ay, belirli bir fatura dönemi veya bir özel bir tarih dönemi için (tekliflerin Microsoft), Azure kullanımı hakkında bilgi alın. Kuruluşlar, bu verileri el ile fatura oranı ve tüketim göre hesaplamak için kullanabilirsiniz. Kuruluşlar, kuruluşlar arasında öznitelik maliyetleri bölüm/kuruluş bilgilerini de kullanabilirsiniz. Veri kullanım/maliyeti kaynağa özgü bir görünümünü sağlar.

-   [Market Store ücret API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge): Geçerli ay, belirli bir fatura dönemi veya bir özel bir tarih dönemi için (iş ortağı teklifleri), Azure kullanımı hakkında bilgi alın. Kuruluşlar, bu verileri el ile fatura oranı ve tüketim göre hesaplamak için kullanabilirsiniz. Kuruluşlar, kuruluşlar arasında öznitelik maliyetleri bölüm/kuruluş bilgilerini de kullanabilirsiniz. Bu API kullanım/maliyeti kaynağa özgü bir görünümünü sağlar.

-   [Fiyat listesi API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet): Her bir ölçüm için verilen kayıt ve fatura dönemi için geçerli oranı alın. El ile beklenen faturanıza yansıyan tutarı hesaplamak için bu kullanım ayrıntıları ile birlikte oranı bilgileri ve Market kullanım bilgileri kullanabilirsiniz.

-   [Faturalama dönemleri API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods): Fatura dönemleri listesini alın. API, API rota dört bu fatura dönemi için ilgili Kurumsal API veri kümesi için işaret eden bir özellik sağlar: BalanceSummary, UsageDetails, Market ücretlerini ve fiyat listesi.

-   [Ayrılmış örnek öneriler API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation): 7 günde 30 gün içinde veya sanal makine kullanımı 60 günün bulun ve tek ve paylaşılan satın alma önerileri alın. Önerilen satın alma tutarlarının ve beklenen maliyet tasarrufu analiz etmek için bu API'yi kullanabilirsiniz. Daha fazla bilgi için [Otomasyon Azure ayırma için API'leri](billing-reservation-apis.md).

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="whats-the-difference-between-the-enterprise-reporting-apis-and-the-consumption-apis-when-should-i-use-each"></a>Kurumsal raporlama API'lerini ve tüketim API'ları arasındaki fark nedir? Her ne zaman kullanmalıyım?
Bu API'ler, benzer bir işlev kümesi olması ve aynı sayıda faturalandırma ve maliyet Yönetimi alanındaki sorular yanıt verebilirsiniz. Ancak bunlar farklı Hedef Kitleleri belirlemenizi: 

- Kurumsal raporlama API'lerini, bunları anlaşma parasal taahhüdüyle ve özel fiyatlandırma erişim sunan Microsoft ile Kurumsal Sözleşme imzalayan müşteriler tarafından kullanılabilir. Sayfasından edinebilirsiniz bir anahtar API'leri gerektiren [Enterprise Portal](https://ea.azure.com). Bu API'ler ile ilgili açıklama için bkz: [genel bakış, raporlama API'lerini Kurumsal müşteriler için](billing-enterprise-api.md).

- Tüketim API'leri, birkaç özel durum, tüm müşteriler için kullanılabilir. Daha fazla bilgi için [Azure kullanım API'si genel bakış](billing-consumption-api-overview.md) ve [Azure tüketim API Başvurusu](https://docs.microsoft.com/rest/api/consumption/). En son geliştirme senaryoları için çözümü olarak sağlanan API'leri öneririz. 

### <a name="whats-the-difference-between-the-usage-details-api-and-the-usage-api"></a>Kullanım ayrıntılarını API'si ve kullanım API'si arasındaki fark nedir?
Bu API'ler, tamamen farklı veri sağlayın:

- [Kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) sağlayan Azure ölçüm örnek başına kullanım ve maliyet bilgileri. Sağlanan veri zaten azure'da sistem ölçümünü maliyet geçtiğini ve vardı, diğer olası değişikliklerle birlikte uygulanan maliyet:

   - Ön ödemeli parasal taahhüt kullanımı için hesap değişiklikleri
   - Azure tarafından bulunan kullanım farklılıklarını hesaba değişiklikleri

- [Kullanım API'si](/previous-versions/azure/reference/mt219003(v=azure.100)) aracılığıyla azure'da sistem ölçümünü maliyet geçirmeden önce ham Azure kullanım bilgileri sağlar. Bu veriler, Azure Ücretli sonra Ölçüm sistemi görülür kullanımı veya ücret tutarı ile herhangi bir ilişki olmayabilir.

### <a name="whats-the-difference-between-the-invoice-api-and-the-usage-details-api"></a>Fatura API kullanım ayrıntılarını API'si arasındaki fark nedir?
Bu API'leri, aynı verileri farklı bir görünümünü sağlar:

- [Fatura API](https://docs.microsoft.com/rest/api/billing/2018-11-01-preview/invoices) yalnızca Web Direct müşterilerine yöneliktir. Bu aylık faturanızda toplam ücretleri her bir ölçüm türü temel dökümünü sağlar. 

- [Kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) her gün için kullanım/maliyeti kayıtları ayrıntılı bir görünümünü sağlar. Hem kurumsal hem de Web Direct müşterilerine onu kullanabilirsiniz.

### <a name="whats-the-difference-between-the-price-sheet-api-and-the-ratecard-api"></a>Fiyat listesi API'si ve RateCard API'si arasındaki fark nedir?
Benzer veri kümeleri bu API'ler sağlar, ancak farklı Hedef Kitleleri vardır:

- [Fiyat listesi API'si](https://docs.microsoft.com/rest/api/consumption/pricesheet) özel fiyatlandırma için kurumsal bir müşterinin anlaşması sağlar.

- [RateCard API'si](/previous-versions/azure/reference/mt219005(v=azure.100)) genel kullanıma yönelik fiyatlandırma geçerlidir Web Direct müşterilerine sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Program aracılığıyla Azure kullanımınızı öngörü almak için Azure API'leri kullanma hakkında daha fazla bilgi için bkz: [Azure tüketim API'sine genel bakış](billing-consumption-api-overview.md) ve [Azure faturalandırma API'si genel bakış](billing-usage-rate-card-overview.md).

- Ayrıntılı günlük kullanım dosyası ve Azure portalında maliyet Yönetimi raporlarını faturanızı karşılaştırmak için bkz [Microsoft Azure için faturanızı anlayın bölümü](billing-understand-your-bill.md).

- Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://go.microsoft.com/fwlink/?linkid=2083458).
