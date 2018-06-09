---
title: Azure faturalama ve maliyet yönetim Otomasyon senaryoları | Microsoft Docs
description: Ne kadar yaygın faturalama öğrenin ve farklı eşlenen API'leri senaryolardır yönetim maliyeti.
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
ms.date: 6/07/2018
ms.author: erikre
ms.openlocfilehash: f84071577e9636e40d621093e2c57e3e9adf4913
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35247675"
---
# <a name="billing-and-cost-management-automation-scenarios"></a>Faturalama ve maliyet yönetim Otomasyon senaryoları

Faturalama ve maliyet yönetim alanı için genel senaryolar aşağıda tanımlanır ve bu senaryolarında kullanılır farklı API'leri eşlenir. Kullanılabilir tüm API'leri ve bunlar sunar işlevselliği özetini API eşleme senaryoya altında bulunabilir. 

## <a name="common-scenarios"></a>Genel senaryolar 

İlgili sorular yönetim maliyeti yanıt maliyet ve kullanım senaryoları çeşitli API'ler ve faturalama kullanabilirsiniz.  Ana hattı yaygın senaryolar aşağıda verilmiştir.

- **Fatura uzlaştırma** -vermedi Microsoft ücret bana sağ tutar?  Faturamı nedir ve ı, kendim hesaplayabilirsiniz?

- **Ücretler arası** - göre ne kadar t, gereksinim duyan Kuruluşumdaki ödeme yapmak için ücretlendirilmeden bilebilirim?

- **En iyi duruma getirme maliyet** -bilebilirim ne kadar benden ücret alındı, ancak nasıl alabilirim ı Azure üzerinde harcama para dışında daha fazla?

- **Maliyet İzlemesi** – ne kadar ı çıkarım harcama ve zaman içinde Azure kullanarak görmek istiyorum. Eğilimleri nelerdir? Nasıl ı daha iyi yaparak?

- **Azure ay sırasında harcadığı** – my geçerli ay ne kadar tarihine harcamanız kullanıcının? My harcama ve/veya Azure kullanımını içinde ayarlamaları yapın gerekiyor mu? Ay ne zaman ı Azure en tüketen 'M?

- **Uyarıları ayarlamak** – kaynak tabanlı tüketimi veya parasal tabanlı uyarı ayarlama yapmak istiyorsunuz.

## <a name="scenario-to-api-mappings"></a>Senaryo API eşlemeleri

|         API/senaryosu        | Fatura uzlaştırma    | Ücretler arası    | Maliyet en iyi duruma getirme    | Maliyet İzlemesi    | Ayın harcamanız    | Uyarılar    |
|:---------------------------:|:-------------------------:|:----------------:|:--------------------:|:----------------:|:------------------:|:---------:|
| Bütçeler                     |                           |                  |           X          |                  |                    |     X     |
| Pazar                |             X             |         X        |           X          |         X        |          X         |     X     |
| Fiyat Listesi                 |             X             |         X        |           X          |         X        |          X         |           |
| Ayırma önerileri |                           |                  |           X          |                  |                    |           |
| Ayırma ayrıntıları         |                           |                  |           X          |         X        |                    |           |
| Ayırma özetleri       |                           |                  |           X          |         X        |                    |           |
| Kullanım Ayrıntıları               |             X             |         X        |           X          |         X        |          X         |     X     |
| Faturalama dönemleri             |             X             |         X        |           X          |         X        |                    |           |
| Faturalar                    |             X             |         X        |           X          |         X        |                    |           |
| RateCard                    |             X             |                  |           X          |         X        |          X         |           |
| Derecelendirilmemiş kullanımı               |             X             |                  |           X          |                  |          X         |           |

> [!NOTE]
> Aşağıdaki API eşlemeleri senaryoya Kurumsal tüketim API'leri içermiyor. Mümkünse, lütfen ilerleyen net yeni geliştirme senaryosu için genel tüketim API'lerini kullanır.

## <a name="api-summaries"></a>API özetleri

### <a name="consumption"></a>Tüketim
(*Web doğrudan + Kurumsal müşteriler için tüm API'leri dışındaki bu çağrılan out aşağıda*)

-   **Bütçeleri** (*yalnızca Kurumsal müşteriler*): kullanım [bütçeleri API](https://docs.microsoft.com/rest/api/consumption/budgets) maliyet veya kullanımı bütçelerini kaynaklar, kaynak grupları veya faturalama ölçümler oluşturmak için.  Bütçeleri oluşturduktan sonra uyarı kullanıcı tanımlı bütçe eşikler aşıldığında bildirmek için yapılandırılabilir. Eylemler, bütçe tutarlarının ulaşıldığında gerçekleşmesi için de yapılandırılabilir.
-   **Pazar**: kullanım [Market ücretleri API](https://docs.microsoft.com/rest/api/consumption/marketplaces) tüm Market kaynaklara ücret ve kullanım verileri almak için (Azure 3 taraf teklifleri). Bu veriler, tüm Market kaynaklar arasında maliyetleri eklemek veya maliyetleri/kullanım belirli kaynaklar üzerinde araştırmak için kullanılabilir.
-   **Fiyat listesi** (*yalnızca Kurumsal müşteriler*): Kurumsal müşteriler kullanabileceğiniz [fiyat listesi API](https://docs.microsoft.com/rest/api/consumption/pricesheet) kendi özel fiyatlandırma için tüm ölçümler alınamadı. Kuruluşların kullanım ve marketplace verilerini kullanarak maliyet hesaplamalar gerçekleştirmek için bu verileri kullanım ayrıntılarını ve Pazar kullanım bilgileri ile birlikte kullanabilirsiniz. 
-   **Ayırma önerileri**: kullanım [ayırma önerileri API](https://docs.microsoft.com/rest/api/consumption/reservationrecommendations) örnekleri ayrılmış VM satın almak için önerileri almak için. Öneriler sağlar müşterilere beklenen maliyet tasarrufu çözümlemek ve tutarlar satın almak tasarlanmıştır.
-   **Ayırma ayrıntıları**: kullanım [ayırması ayrıntıları API](https://docs.microsoft.com/rest/api/consumption/reservationsdetails) ne kadar tüketim ne kadar gerçekte kullanılıyor karşı ayrılmış gibi önceden satın alınan VM ayırmaları bilgileri görmek için. Her VM-düzeyi ayrıntı verileri görebilirsiniz.
-   **Ayırma özetleri**: kullanım [ayırma özetleri API](https://docs.microsoft.com/rest/api/consumption/reservationssummaries) ne kadar tüketim ne kadar gerçekte kullanılıyor karşı ayrılmış gibi önceden satın alınan VM ayırmaları üzerinde toplama bilgileri görmek için bir toplama. 
-   **Kullanım ayrıntılarını**: kullanım [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/consumption/usagedetails) ücretsiz ve kullanımı tüm Azure üzerinde almak için 1 taraf kaynakları. Şu anda bir kez ölçer her gün başına gösterilen kullanım ayrıntı kaydı biçiminde bilgilerdir. Bilgiler, tüm kaynaklar arasında maliyetleri eklemek veya maliyetleri/kullanım belirli kaynaklar üzerinde araştırmak için kullanılabilir.
-   **RateCard**: Web doğrudan müşteriler [RateCard API](https://msdn.microsoft.com/library/azure/mt219005.aspx) kendi ölçer oranları alınamıyor. Bunlar daha sonra döndürülen bilgilerin kaynak kullanım bilgilerini ile el ile Beklenen fatura hesaplamak için kullanabilirsiniz. 
-   **Kullanım derecelendirilmemiş**: kullanabileceğiniz [derecelendirilmemiş kullanım API'si](https://msdn.microsoft.com/library/azure/mt219003.aspx) tüm ölçüm/Azure tarafından yapılan şarj önce ham kullanım bilgileri elde edilir.

### <a name="billing"></a>Faturalandırma
-   **Faturalama nokta**: kullanım [faturalama nokta API](https://docs.microsoft.com/rest/api/billing/billingperiods) , fatura birlikte çözümlemek için bir fatura dönemi belirlemek için belirtilen belirli bir döneme ait. Fatura kimliğin aşağıdaki fatura API ile kullanılabilir. 
-   **Faturaları**: kullanım [faturalar API](https://docs.microsoft.com/rest/api/billing/invoices) PDF formunda belirli bir fatura dönemi için indirme URL'si için bir fatura almak için.

### <a name="enterprise-consumption"></a>Kurumsal tüketim
*(Tüm API'leri Kurumsal yalnızca)*

-   **Bakiye Özet**: kullanım [Bakiye Özet API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) aylık bir özetini bakiyelerini, yeni satın alma işlemleri, Azure Marketi Hizmet ücretleri, ayarlamalar ve fazla kullanım ücretleri hakkında bilgi almak için. Geçerli fatura dönemi veya geçmişte herhangi bir süre için bu bilgileri elde edebilirsiniz. Kuruluşlar bu verileri el ile hesaplanan Özet ücretleri karşılaştırmaya gerçekleştirmek için kullanabilirsiniz. Bu API, kaynak özgü bilgileri veya maliyetleri birleşik bir görünümünü sağlamaz.
-   **Kullanım ayrıntılarını**: kullanım [kullanım ayrıntılarını API'si](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) 1 taraf geçerli ay, belirli bir fatura döneminde ya da bir özel tarih dönemi için Azure kullanım ayrıntılı bilgileri almak için. Şirketler bu verileri el ile oranı ve tüketim göre fatura hesaplamak için kullanabilir ve departmanı/kuruluş bilgilerini özniteliği maliyetleri mevcut kuruluşlar arasında de kullanabilirsiniz. Veri kullanımı/maliyet kaynak özgü görünümünü sağlar.
-   **Market deposu ücret**: kullanım [Market deposu ücret API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) 3. taraf geçerli ay, belirli bir fatura döneminde ya da bir özel tarih dönemi için Azure kullanım ayrıntılı bilgileri almak için. Şirketler bu verileri el ile oranı ve tüketim göre fatura hesaplamak için kullanabilir ve departmanı/kuruluş bilgilerini özniteliği maliyetleri mevcut kuruluşlar arasında de kullanabilirsiniz. Market deposu ücret API, kullanım/maliyeti kaynak özgü görünümünü sağlar.
-   **Fiyat listesi**: [fiyat listesi API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) verilen kayıt ve Faturalama dönem için geçerli oranı her ölçer sağlar. Bu oran bilgileri kullanım ayrıntılarını ve Pazar kullanım bilgileri ile birlikte el ile Beklenen fatura hesaplamak için kullanılabilir.
-   **Faturalama nokta**: kullanım [faturalama nokta API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) nokta dört fatura bu dönem - BalanceSummary, ilgilidir Kurumsal API veri kümeleri için API yolu işaret eden bir özellik yanı sıra faturalama listesini almak için UsageDetails, Market ücretleri ve fiyat listesi.
-   **Ayrılmış örnek önerileri**: [ayrılmış örnek önerileri API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation) müşterinin 30 gün ya da sanal makine kullanımı 60 gün olan 7 günde arar ve tek ve satın alma paylaşılan öneriler sunar. Analiz etmek müşterilerin API sağlayan ayrılmış örnek maliyet tasarrufu beklenen ve satın alma tutarlar önerilir.

## <a name="next-steps"></a>Sonraki Adımlar

- Azure faturalama API'leri kullanarak program aracılığıyla Azure kullanımınızı hakkında bilgi edinme hakkında daha fazla bilgi için bkz: [Azure faturalama API genel bakış](billing-usage-rate-card-overview.md).
