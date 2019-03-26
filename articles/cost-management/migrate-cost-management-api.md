---
title: Microsoft Müşteri sözleşmesi API - Azure için Kurumsal Anlaşmamdan geçirme | Microsoft Docs
description: Bu makale bir Microsoft Müşteri sözleşmesi için Microsoft Kurumsal Anlaşma (EA) geçiş işleminin sonuçlarını anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/20/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: 283808c0bd3f5297011b25619d6f978c99d4dc32
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439235"
---
# <a name="migrate-from-enterprise-agreement-to-microsoft-customer-agreement-apis"></a>Kurumsal Sözleşme, Microsoft Müşteri sözleşmesi API'lerine geçiş

Bu makalede veri yapısı, API ve diğer Kurumsal Anlaşma (EA) ve Microsoft Müşteri Sözleşmesi (MCA) hesapları sistem tümleştirme farklılıklardan anlamanıza yardımcı olur. Azure maliyet yönetimi, API'leri her iki hesap türlerini destekler. Gözden geçirme [kurulum için Faturalama hesabı](../billing/billing-mca-setup-account.md) devam etmeden önce Microsoft Müşteri sözleşmesi makalesi.

Mevcut bir EA hesap bir kuruluşlarıyla MCA hesabınızı ayarlama ile birlikte bu makalede gözden geçirmeniz gerekir. Daha önce bir EA hesap yenileniyor, eski bir kayıt işlemini yeni bir dala taşımak için bazı en az iş gereklidir. Ancak, geçiş MCA hesabınız için ek çaba gerektirir. Temel fatura alt sistemdeki tüm maliyet ile ilgili API'ler ve hizmet teklifleri etkileyen değişiklikler nedeniyle ek çalışmasıdır.

## <a name="mca-apis-and-integration"></a>MCA API'ler ve tümleştirme

MCA API'leri ve yeni bir tümleştirme sağlar:

- Yerel Azure API'leri aracılığıyla tam API kullanılabilirlik vardır.
- Birden çok faturası tek bir fatura hesap yapılandırın.
- Market satın alımları Azure hizmet kullanımı ve üçüncü taraf Market kullanım ile birleştirilmiş bir API erişin.
- Görüntüleme maliyetleri profilleri (aynı kayıtlar) faturalama arasında Azure maliyet Yönetimi'ni kullanma.
- Maliyetleri gösterme, bildirim maliyetlerini önceden tanımlanmış eşiklerini aşan ve otomatik olarak ham verileri dışarı aktarma alın. yeni API'ler erişim.

## <a name="migration-checklist"></a>Geçiş denetim listesi

Aşağıdaki öğeler MCA API'lerine geçiş Yardımı.

- Yeni bilgilenmeli [Microsoft Müşteri faturalama hesabı sözleşmesi](../billing/billing-mca-overview.md).
- Hangi API'leri kullanın ve hangilerinin aşağıdaki bölümde değiştirilir bkz belirleyin.
- İle kendinizi alıştırın [Azure Resource Manager REST API'leri](/rest/api/azure).
- Azure Resource Manager API'leri, zaten kullanıyorsanız, [istemci uygulamanızı Azure AD'ye kaydetme](/rest/api/azure/#register-your-client-application-with-azure-ad).
- Herhangi bir programlama kod güncelleştirme [kullanımı Azure AD kimlik doğrulaması](/rest/api/azure/#create-the-request).
- EA API çağrıları MCA API çağrısı ile değiştirmek için programlama kodu güncelleştirin.
- Yeni hata kodları kullanmak için hata işleme güncelleştirin.
- Cloudyn ve diğer Power BI eylem gerektiği gibi ek tümleştirme teklifleri inceleyin.

## <a name="ea-apis-replaced-with-mca-apis"></a>EA API MCA API'leri ile değiştirildi

EA API, API anahtarı kimlik doğrulaması ve yetkilendirme için kullanın. Azure AD kimlik doğrulaması MCA API'lerini kullanın.

| Amaç | EA API | MCA API |
| --- | --- | --- |
| Bakiye ve krediler | [/balancesummary](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) | Microsoft.Billing/billingAccounts/billingProfiles/availableBalanceussae |
| Kullanım (JSON) | [/UsageDetails](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)[/usagedetailsbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format) | [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Kullanım (CSV) | [/ usagedetails/indirme](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format) [ /usagedetails/gönderme](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Market kullanım (CSV) | [/marketplacecharges](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)[/marketplacechargesbycustomdate](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge) | [Microsoft.Consumption/usageDetails/download](/rest/api/consumption/usagedetails)<sup>1</sup> |
| Faturalama dönemleri | [/billingperiods](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Fiyat listesi | [/pricesheet](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) | Microsoft.Billing/billingAccounts/billingProfiles/pricesheet/default/download biçimi json =|CSV Microsoft.Billing/billingAccounts/.../billingProfiles/.../invoices/... /pricesheet/default/download biçimi json =|CSV Microsoft.Billing/billingAccounts/... / billingProfiles /... /providers/Microsoft.Consumption/pricesheets/download  |
| Rezervasyon satın alma | [/reservationcharges](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges) | Microsoft.Billing/billingAccounts/billingProfiles/transactions |
| Ayırma önerileri | [/ SharedReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)[/](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)[SingleReservationRecommendations](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations) | [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) |
| Ayırma kullanımı | [/reservationdetails](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)[/reservationsummaries](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary) | [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails)[Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) |

<sup>1</sup> azure hizmet ve üçüncü taraf Market kullanım bulunan [kullanım ayrıntılarını API'si](/rest/api/consumption/usagedetails).

Aşağıdaki API'leri MCA fatura hesapları için kullanılabilir:

| Amaç | Microsoft Müşteri Sözleşmesi (MCA) API'si |
| --- | --- |
| Faturalama hesaplarının<sup>2</sup> | Microsoft.Billing/billingAccounts |
| Faturalandırma profilleri<sup>2</sup> | Microsoft.Billing/billingAccounts/billingProfiles |
| Fatura bölümleri<sup>2</sup> | Microsoft.Billing/billingAccounts/invoiceSections |
| Faturalar | Microsoft.Billing/billingAccounts/billingProfiles/invoices |
| Faturalandırma abonelikler | {kapsamı} / billingSubscriptions |

<sup>2</sup> API'leri burada maliyet yönetimi, Azure portalında deneyimleri ve API'leri çalışması kapsamları nesnelerinin listesini döndürür. Maliyet Yönetimi kapsamları hakkında daha fazla bilgi için bkz. [anlayın ve kapsamlı iş](understand-work-scopes.md).

Mevcut bir EA API kullanıyorsanız, bunları MCA fatura hesapları desteklemek için güncelleştirmeniz gerekir. Aşağıdaki tablo, başka bir tümleştirme değişiklik gösterir:

| Amaç | Eski teklifi | Yeni Teklif |
| --- | --- | --- |
| Cloudyn | [Cloudyn.com](https://www.cloudyn.com) | [Azure Maliyet Yönetimi](https://azure.microsoft.com/services/cost-management/) |
| Power BI | [Microsoft kullanım öngörüleri](/power-bi/desktop-connect-azure-consumption-insights) İçerik Paketi ve bağlayıcı | [Microsoft Azure tüketim öngörüleri Power BI uygulamasında](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview) ve [ Azure tüketim öngörüleri Bağlayıcısı](/power-bi/desktop-connect-azure-consumption-insights) |

## <a name="apis-to-get-balance-and-credits"></a>Bakiye ve kredi almak için API'ler

[Bakiye özeti alma](/rest/api/billing/enterprise/billing-enterprise-api-balance-summary) API'si aylık bir özetini sunar:

- Bakiyeler
- Yeni satın almalar
- Azure Market hizmeti ücreti
- Düzeltmeler
- Hizmet fazla kullanım ücretleri

Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request).

Bakiye özeti alma API Microsoft.Billing/billingAccounts/billingProfiles/availableBalance API'si tarafından değiştirilir.

Kullanılabilir Bakiye API'si ile kullanılabilir bakiyeleri almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/availableBalances?api-version=2018-11-01-preview` |

## <a name="apis-to-get-cost-and-usage"></a>API'ları, maliyet ve kullanım bilgilerini alma

Azure hizmet kullanımı, üçüncü taraf Market kullanım ve aşağıdaki API'leri ile diğer Market alışverişleri maliyetlerin günlük dökümünü alın. Aşağıdaki ayrı API'leri, Azure Hizmetleri ve üçüncü taraf Market kullanım için birleştirilmiştir. Eski API'ler değiştirilir [Microsoft.Consumption/usageDetails](/rest/api/consumption/usagedetails) API. Daha önce yalnızca bakiyesi tarihe Özet gösterilen Market satın alımları ekler.

- [Kullanım ayrıntısı/download Al](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Kullanım ayrıntısı/gönderme Al](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#csv-format)
- [Kullanım ayrıntısı/usagedetails Al](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Kullanım ayrıntısı/usagedetailsbycustomdate Al](/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#json-format)
- [Market deposu ücret/marketplacecharges Al](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)
- [Market deposu ücret/marketplacechargesbycustomdate Al](/rest/api/billing/enterprise/billing-enterprise-api-marketplace-storecharge)

Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request).

Önceki tüm API'leri tüketim/kullanım ayrıntılarını API'si tarafından değiştirilir.

Kullanım ayrıntılarını API'si ile kullanım ayrıntılarını almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/{scope}/providers/Microsoft.Consumption/usageDetails?api-version=2019-01-01` |

Kullanım ayrıntılarını API'si tüm maliyet Yönetimi API'leri ile gibi birden çok kapsamların kullanılabilir. Geleneksel olarak, bir kayıt düzeyinde alacaksınız Faturalanan maliyetleri için faturalandırma profili kapsamı kullanın.  Maliyet Yönetimi kapsamları hakkında daha fazla bilgi için bkz. [anlayın ve kapsamlı iş](understand-work-scopes.md).

| Type | Kimliği biçimi |
| --- | --- |
| Fatura hesabı | `/Microsoft.Billing/billingAccounts/{billingAccountId}` |
| Faturalama profili | `/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}` |
| Abonelik | `/subscriptions/{subscriptionId}` |
| Kaynak grubu | `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}` |

Herhangi bir programlama kod güncelleştirmek için aşağıdaki querystring parametreleri kullanın.

| Eski parametreleri | Yeni parametreleri |
| --- | --- |
| `billingPeriod={billingPeriod}` | Desteklenmiyor |
| `endTime=yyyy-MM-dd` | `endDate=yyyy-MM-dd` |
| `startTime=yyyy-MM-dd` | `startDate=yyyy-MM-dd` |

Yanıt gövdesi de değiştirildi.

Eski yanıt gövdesi:

```
{
  "id": "string",
  "data": [{...}, ...],
  "nextLink": "string"
}
```

Yeni yanıt gövdesi:

```
{
  "value": [{
    "id": "{scope}/providers/Microsoft.Consumption/usageDetails/###",
    "name": "###",
    "type": "Microsoft.Consumption/usageDetails",
    "tags": {...},
    "properties": [{...}, ...],
    "nextLink": "string"
  }, ...]
}
```

Veri kullanım kayıtlarından oluşan bir diziyi içeren özellik adı değiştirildi _değerleri_. Düz liste ayrıntılı özellikler sağlamak için kullanılan her bir kaydı. Bununla birlikte, artık tüm ayrıntıları artık adlı iç içe özellik her kayıt _özellikleri_, etiketler dışında. Yeni yapı, diğer Azure API'leri ile tutarlıdır. Bazı özellik adları değiştirilmiştir. Aşağıdaki tabloda, ilgili özellikleri gösterir.

| Eski özelliği | Yeni özellik | Notlar |
| --- | --- | --- |
| Hesap kimliği | Yok | Aboneliği Oluşturucusu izlenen değil. İnvoiceSectionId (departmentId ile aynı) kullanın. |
| AccountNameAccountOwnerId ve AccountOwnerEmail | Yok | Aboneliği Oluşturucusu izlenen değil. İnvoiceSectionName (departmentName ile aynı) kullanın. |
| Ek bilgi | additionalInfo | &nbsp;  |
| ChargesBilledSeparately | isAzureCreditEligible | Bu özellikler birbirinin zıttı olduğunu unutmayın. İsAzureCreditEnabled true ise, ChargesBilledSeparately false olur. |
| Tüketilen miktar | quantity | &nbsp; |
| Tüketilen hizmet | consumedService | Tam dize değerlerini gösterebilir. |
| Tüketilen hizmet kimliği | None | &nbsp; |
| Maliyet merkezi | costCenter | &nbsp; |
| Tarih ve usageStartDate | date | &nbsp;  |
| Gün | None | Başlangıç tarihi gün ayrıştırır. |
| Bölüm kimliği | invoiceSectionId | Tam değerleri farklı. |
| Bölüm adı | invoiceSectionName | Tam dize değerlerini gösterebilir. Departmanlar, eşleştirilecek fatura bölümlerde gerekli olup olmadığını yapılandırın. |
| ExtendedCost ve maliyet | costInBillingCurrency | &nbsp;  |
| Örnek kimliği | resourceId | &nbsp;  |
| Yinelenen Ücretlendirme | None | &nbsp;  |
| Konum | location | &nbsp;  |
| Ölçüm kategorisi | meterCategory | Tam dize değerlerini gösterebilir. |
| Ölçüm kimliği | meterId | Tam dize değerleri farklı. |
| Ölçüm adı | meterName | Tam dize değerlerini gösterebilir. |
| Ölçüm bölgesi | meterRegion | Tam dize değerlerini gösterebilir. |
| Ölçüm alt kategorisi | meterSubCategory | Tam dize değerlerini gösterebilir. |
| Ay | None | Tarihten itibaren ay ayrıştırır. |
| Teklif Adı | None | PublisherName ve productOrderName kullanın. |
| OfferId | None | &nbsp;  |
| Sipariş numarası | None | &nbsp;  |
| PartNumber | None | Fiyatlar benzersiz olarak tanımlanabilmesi için meterId ve productOrderName kullanın. |
| Plan Adı | productOrderName | &nbsp;  |
| Ürün | Ürün |   |
| Ürün kimliği | productId | Tam dize değerleri farklı. |
| Yayımcı Adı | publisherName | &nbsp;  |
| ResourceGroup | resourceGroupName | &nbsp;  |
| Kaynak Guid'si | meterId | Tam dize değerleri farklı. |
| Kaynak konumu | resourceLocation | &nbsp;  |
| Kaynak konumu kimliği | None | &nbsp;  |
| Kaynak fiyatı | effectivePrice | &nbsp;  |
| Hizmet yöneticisi kimliği | Yok | &nbsp;  |
| Hizmet bilgisi 1 | serviceInfo1 | &nbsp;  |
| Hizmet bilgisi 2 | serviceInfo2 | &nbsp;  |
| ServiceName | meterCategory | Tam dize değerlerini gösterebilir. |
| ServiceTier | meterSubCategory | Tam dize değerlerini gösterebilir. |
| Depolama hizmeti tanımlayıcısı | Yok | &nbsp;  |
| Abonelik guid'i | subscriptionId | &nbsp;  |
| SubscriptionId | subscriptionId | &nbsp;  |
| Abonelik adı | subscriptionName | &nbsp;  |
| Etiketler | etiketler | Etiketler özelliği kök nesnesi, iç içe özellikler özelliğine uygulanır. |
| Ölçü birimi | unitOfMeasure | Tam dize değerleri farklı. |
| usageEndDate | date | &nbsp;  |
| Yıl | None | Tarihinden itibaren bir yıl ayrıştırır. |
| (yeni) | billingCurrency | Para birimi ücreti. |
| (yeni) | billingProfileId | Faturalandırma profili (kayıt ile aynı) benzersiz kimliği. |
| (yeni) | billingProfileName | Faturalandırma profili (kayıt ile aynı) adı. |
| (yeni) | chargeType | Azure hizmet kullanımı, Market kullanım ve satın alma işlemleri ayırt etmek için kullanın. |
| (yeni) | invoiceId | Fatura benzersiz kimliği. Geçerli ve açık ay için boş. |
| (yeni) | publisherType | Satın alma işlemleri için yayımcı türü. Kullanım için boş. |
| (yeni) | serviceFamily | Satın alma türü. Kullanım için boş. |
| (yeni) | servicePeriodEndDate | Satın alınan hizmet bitiş tarihi. |
| (yeni) | servicePeriodStartDate | Satın alınan hizmet için başlangıç tarihi. |

## <a name="billing-periods-api-replaced-by-invoices-api"></a>Faturalama dönemleri API faturaları API'sı tarafından değiştirildi

Faturalama hesaplarının MCA faturalandırma dönemleri kullanmayın. Bunun yerine, belirli bir fatura dönemlerine kapsam maliyetleri faturalara kullanın. [Faturalandırma dönemlerini API'si](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) faturalar API'si tarafından değiştirilir. Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request).

Faturalar faturaları API ile almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/invoices?api-version=2018-11-01-preview` |

## <a name="price-sheet-apis"></a>Fiyat listesi API'leri

Bu bölümde, var olan fiyat listesi API'leri açıklar ve fiyat sayfası API'sine için Microsoft Müşteri anlaşmalarını taşımak için öneriler sağlar. Ayrıca, fiyat listesi API'si için Microsoft Müşteri anlaşmalarını açıklanır ve fiyat listeleri alanları açıklar. [Kurumsal elde fiyat](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet) ve [fatura dönemleri Kurumsal elde](/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) API'leri için Microsoft Müşteri anlaşmalarını (Microsoft.Billing/billingAccounts/billingProfiles fiyat listesi API'si tarafından değiştirilir / Fiyat listesi). Yeni API hem JSON hem de CSV biçimleri, zaman uyumsuz REST biçimlerde destekler. Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request).

### <a name="billing-enterprise-apis"></a>Faturalama Kurumsal API'leri

Faturalama Kurumsal API'leri ile Kurumsal kayıtlarını fiyat ve faturalandırma dönemi bilgileri almak için kullanılır. Kimlik doğrulama ve yetkilendirme, Azure Active Directory web belirteçleri kullanılır.

Faturalandırma dönemi API'lerini ve fiyat listesi ile belirtilen Kurumsal kayıt için uygun fiyatları almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet` |
| GET | `https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet` |

### <a name="price-sheet-api-for-microsoft-customer-agreements"></a>Microsoft Müşteri sözleşmesi için fiyat sayfası API'si

Tüm Azure tüketim ve Market tüketim hizmetlerin fiyatlarını görüntülemek için Microsoft Müşteri sözleşmesi fiyat listesi API'si kullanın. Fatura profiline ait tüm abonelikleri için fatura profili gösterilen fiyatlar uygulanır.

Fiyat listesi API'si, tüm Azure tüketim Hizmetleri fiyat listesi verilerini CSV biçiminde görüntülemek için kullanın:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=csv` |

Fiyat listesi API'si, JSON biçiminde tüm Azure tüketim Hizmetleri fiyat listesi verilerini görüntülemek için kullanın:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=json` |

API'yi kullanarak tüm hesap için fiyat listesini döndürür. Ancak, PDF biçiminde fiyat daraltılmış bir sürümünü de alabilirsiniz. Özet için belirli bir fatura faturalandırılır Azure tüketim ve Market tüketim hizmetleri içerir. Fatura {Fatura kodu tarafından}, tanımlanmış aynı olduğu **fatura numarası** fatura özeti PDF dosyaları gösterilir. Bir örnek aşağıda verilmiştir.

![Karşılık gelen fatura numarası için Fatura kodu gösteren örnek resim](./media/migrate-cost-management-api/invoicesummary.png)

CSV biçiminde fiyat listesi API'si ile fatura bilgilerini görüntülemek için:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

JSON biçiminde fiyat listesi API'si ile fatura bilgilerini görüntülemek için:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

Azure tüketim veya Market herhangi bir tüketim hizmeti için tahmini Fiyatlar, geçerli fatura dönemi açık ya da hizmet süresini de görebilirsiniz.

Tahmini fiyatlar fiyat listesi API'si ile kullanım hizmeti için CSV biçiminde görüntülemek için:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv` |

Tahmini fiyatlar fiyat listesi API'si ile kullanım hizmeti için JSON biçiminde görüntülemek için:

| Yöntem | İstek URI'si |
| --- | --- |
| POST | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json` |

Microsoft Müşteri sözleşmesi fiyat sayfası API'leri *zaman uyumsuz REST API'leri*. API'ler için yanıtlar daha eski zaman uyumlu API'lerinden değiştirildi. API yanıt gövdesinin de değiştirildi.

#### <a name="old-response-body"></a>Eski yanıt gövdesi

REST API yanıt zaman uyumlu bir örnek aşağıda verilmiştir:

```
[
        {
            "id": "enrollments/573549891/billingperiods/2016011/products/343/pricesheets",
            "billingPeriodId": "201704",
            "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
            "meterName": "A1 VM",
            "unitOfMeasure": "100 Hours",
            "includedQuantity": 0,
            "partNumber": "N7H-00015",
            "unitPrice": 0.00,
            "currencyCode": "USD"
        },
        {
    ]
```

#### <a name="new-response-body"></a>Yeni yanıt gövdesi

API'lerini destekleyen [Azure REST zaman uyumsuz](../azure-resource-manager/resource-manager-async-operations.md) biçimi. GET kullanarak API çağırın ve şu yanıtı alırsınız:

```
No Response Body

HTTP Status 202 Accepted
```

Aşağıdaki üst bilgiler, çıkış konumunu gönderilir:

```
Location:https://management.azure.com/providers/Microsoft.Consumption/operationresults/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Azure-AsyncOperation:https://managment.azure.com/providers/Microsoft.Consumption/operationStatus/{operationId}?sessiontoken=XZDFSnvdkbkdsb==

Retry-After: 10

OData-EntityId: {operationId}

```

Başka bir GET çağrısı konumuna olun. İşlem tamamlandığında veya başarısız durumuna ulaşana kadar yanıt alma çağrısı için aynıdır. Tamamlandığında, yanıt bir GET çağrısı konuma indirme URL'sini döndürür. İşlem aynı anda yalnızca yürütülmesi alacağı. Bir örneği aşağıda verilmiştir:

```
HTTP Status 200

                                    {
                            “id”: “providers/Microsoft.Consumption/operationresults/{operationId}”,
                            “name”: {operationId},
                           “type”: “Microsoft.Consumption/operationResults”,
                           “properties” : {
                                  “downloadUrl”: {urltoblob},
                                  “vaildTill”: “Date”
}
                     }
```

İstemci bir GET çağrısı de yapmak `Azure-AsyncOperation`. Uç nokta işlemi durumunu döndürür.

Aşağıdaki tabloda, eski Kurumsal elde fiyat sayfası API'SİNDE alanları gösterir. Yeni fiyat listesinde karşılık gelen alanları için Microsoft Müşteri anlaşmalarını içerir:

| Eski özelliği | Yeni özellik | Notlar |
| --- | --- | --- |
| billingPeriodId  | _Uygulanamaz_ | Geçerli değildir. Microsoft Müşteri sözleşmelerini billingPeriodId kavramı fatura ve ilişkili fiyat değiştirildi. |
| meterId  | meterId | &nbsp;  |
| unitOfMeasure  | unitOfMeasure | Tam dize değerlerini gösterebilir. |
| includedQuantity  | includedQuantity | Microsoft Müşteri anlaşmalarını hizmetler için geçerli değildir. |
| PartNumber  | _Uygulanamaz_ | Bunun yerine, productOrderName (OfferId ile aynı) ve meterid bileşimini kullanın. |
| UnitPrice  | UnitPrice | Birim fiyatı, Microsoft Müşteri sözleşmelerde kullanılan hizmetler için geçerlidir. |
| currencyCode  | pricingCurrency | Microsoft Müşteri sözleşmesi, fiyatlandırma para birimi ve Fatura para birimi fiyat temsilleri olabilir. Microsoft Müşteri anlaşmalarla pricingCurrency currencyCode karşılık gelir. |
| OfferId | productOrderName | OfferId, yerine productOrderName kullanabilirsiniz ancak OfferId olarak aynı değildir. Ancak, productOrderName ve ölçüm Microsoft Müşteri sözleşmelerde fiyatlandırma meterId ve OfferId ilgili eski kayıtları belirleyin. |

## <a name="consumption-price-sheet-api-operations"></a>Tüketim fiyatı sayfası API işlemleri

Kurumsal anlaşmalar için tüketim fiyatı sayfası API kullanılan [alma](/rest/api/consumption/pricesheet/get) ve [fatura dönemi tarafından alma](/rest/api/consumption/pricesheet/getbybillingperiod) bir kapsam Subscriptionıd ya da bir fatura dönemi için operations. API, Azure kaynak yönetimi kimlik doğrulaması kullanır.

Fiyat listesi API'si ile bir kapsam için fiyat bilgileri almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

Faturalama dönemi fiyat listesi API'si ile tarafından fiyat bilgileri almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Billing/billingPeriods/{billingPeriodName}/providers/Microsoft.Consumption/pricesheets/default?api-version=2018-10-01` |

Yukarıdaki API uç noktaları yerine Microsoft Müşteri sözleşmeleri için aşağıdaki sorguyu kullanın:

**Fiyat listesi API için Microsoft Müşteri anlaşmalarını (zaman uyumsuz REST API'si)**

Bu API için Microsoft Müşteri anlaşmalarını olan ve ek öznitelikler sağlar.

**Fiyat listesi için bir faturalandırma profili kapsamında bir faturalama hesabı**

Bu API var olan bir API'dir. Fiyat listesi bir faturalama hesabı faturalama profilde sağlamak için güncelleştirildi.

## <a name="price-sheet-for-a-scope-by-billing-account"></a>Fatura hesabı tarafından bir kapsam için fiyat listesi

Fatura hesabındaki kayıt kapsamda fiyat listesini aldığınızda, azure Resource Manager kimlik doğrulaması kullanılır.

Bir faturalama hesabı kayıt hesabında, fiyat listesini almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/65085863/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

Microsoft Müşteri sözleşmesi için aşağıdaki bölümde bilgileri kullanın. Bu, Microsoft Customer anlaşmalar için kullanılan alanı özellikleri sağlar.

### <a name="price-sheet-for-a-billing-profile-scope-in-a-billing-account"></a>Fiyat listesi için bir faturalandırma profili kapsamındaki bir faturalama hesabı

Fatura hesabı API tarafından güncelleştirilmiş fiyat fiyat CSV biçiminde alır. Bir MCA faturalandırma profili kapsamda fiyat listesini almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `/providers/Microsoft.Billing/billingAccounts/28ae4b7f-41bb-581e-9fa4-8270c857aa5f/billingProfiles/ef37facb-cd6f-437a-9261-65df15b673f9/providers/Microsoft.Consumption/pricesheets/download?api-version=2019-01-01` |

EA'ın kayıt kapsamında, API yanıt ve özellikleri aynıdır. Özellikleri aynı MCA özelliklerine karşılık gelir.

Eski özelliklerini [Azure Resource Manager fiyat sayfası API'leri](/rest/api/consumption/pricesheet) ve aynı yeni özellikler aşağıdaki tabloda yer alan.

| Eski Azure Resource Manager fiyat sayfası API özelliği  | Yeni Microsoft Müşteri sözleşmesi fiyat listesi API'si özelliği   | Açıklama |
| --- | --- | --- |
| Ölçüm Kimliği | _meterId_ | Ölçüm için benzersiz tanımlayıcı. MeterId ile aynıdır. |
| Ölçüm adı | meterName | Ölçüm adı. Ölçüm, Azure hizmet dağıtılabilir kaynağa temsil eder. |
| Ölçüm kategorisi  | hizmet | Ölçüm için sınıflandırma kategorisi adı. Microsoft Müşteri sözleşmesi fiyat hizmette ile aynıdır. Tam dize değerleri farklı. |
| Ölçüm alt kategorisi | meterSubCategory | Ölçüm subclassification kategorisi adı. Üst düzey özellik kümesi ayrıştırması hizmetinde sınıflandırmasını temel. Örneğin, temel SQL veritabanı ile standart SQL veritabanı. |
| Ölçüm bölgesi | meterRegion | &nbsp;  |
| Birim | _Uygulanamaz_ | UnitOfMeasure ayrıştırılamaz. |
| Ölçü birimi | unitOfMeasure | &nbsp;  |
| Parça numarası | _Uygulanamaz_ | PartNumber yerine productOrderName ve MeterId fiyat faturalandırma profili için benzersiz olarak tanımlanabilmesi için kullanın. Alanları MCA faturalar içinde partNumber yerine MCA fatura listelenir. |
| Birim fiyatı | UnitPrice | Microsoft Müşteri sözleşmesi birim fiyatı. |
| Para birimi kodu | pricingCurrency | Microsoft Müşteri sözleşmeleri, fiyatları para birimi fiyatlandırma ve Fatura para birimi temsil eder. Para birimi kodu, Microsoft Müşteri anlaşmalarla pricingCurrency ile aynıdır. |
| Dahil edilen miktar | includedQuantity | Microsoft Müşteri anlaşmalarını Hizmetleri için geçerli değildir. İle sıfır değerleri gösterir. |
|  Teklif Kimliği  | productOrderName | ProductOrderName OfferId yerine kullanın. Aynı OfferId, ancak Microsoft Müşteri sözleşmelerde fiyatlandırma productOrderName ve ölçüm belirleyebilir. Eski kayıtları içinde meterId ve OfferId ilgili. |

Microsoft Müşteri anlaşmalarını fiyatı Kurumsal sözleşmeler farklı tanımlanır. Kurumsal kayıt hizmetleri için fiyat, ürün, PartNumber, ölçüm ve teklif için benzersizdir. Microsoft Müşteri sözleşmelerde PartNumber kullanılmaz.

Bir Microsoft Müşteri sözleşmesinin bir parçası olan Azure tüketim hizmeti fiyatı productOrderName ve meterId için benzersizdir. Bunlar, hizmet ölçer ve ürün planı temsil eder.

Fiyat listesi ve kullanım ayrıntılarını API'si kullanımı arasında mutabık kılınacak productOrderName ve meterId kullanabilirsiniz.

Faturalandırma Profil sahibi, katkıda bulunan, okuyucu ve Fatura Yöneticisi haklarına sahip kullanıcılar, fiyat listesini indirebilirsiniz.

Fiyat, fiyat, kullanıma bağlı hizmetler fiyatları içerir. Hizmetleri, Azure tüketim ve Market tüketimini içerir. Her hizmet dönemin sonunda en son fiyat kilitlidir ve tek hizmet nokta ile kullanım için uygulanır. Azure tüketim hizmetler için genellikle bir takvim ayı hizmet dönemdir.

### <a name="retired-price-sheet-api-fields"></a>Devre dışı bırakılan fiyat listesi API'si alanları

Aşağıdaki alanları olan Microsoft Müşteri sözleşmesi fiyat sayfası API'leri bulunan veya aynı alanlara sahip.

|Devre dışı bırakılan alan| Açıklama|
|---|---|
| billingPeriodId | Hayır, uygulanabilir. Fatura için kodu için MCA karşılık gelir. |
| OfferId | Geçerli değildir. ProductOrderName MCA içinde karşılık gelir. |
| meterCategory  | Geçerli değildir. MCA hizmetinde karşılık gelir. |
| birim | Geçerli değildir. UnitOfMeasure ayrıştırılamaz. |
| currencyCode | PricingCurrency MCA içinde aynıdır. |
| meterLocation | MeterRegion MCA içinde aynıdır. |
| partNumber partnumber | Parça numarası MCA faturaları listede olduğundan geçerli değil. PartNumber yerine meterId ve productOrderName birleşimi fiyatları benzersiz olarak tanımlanabilmesi için kullanın. |
| totalIncludedQuantity | Geçerli değildir. |
| pretaxStandardRate  | Geçerli değildir. |

## <a name="reservation-instance-charge-api-replaced"></a>Ayırma örneği ücret API değiştirildi

İşlemleri rezervasyon satın alma işlemleri için fatura almak [ayrılmış örnek ücret API](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges). Yeni API üçüncü taraf Market teklifleri de dahil olmak üzere tüm satın alımlar içerir. Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request). Ayrılmış örnek ücret API işlem API'si tarafından değiştirilir.

Rezervasyon satın alma işlemleri işlem API'si ile almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/transactions?api-version=2018-11-01-preview` |

## <a name="recommendations-apis-replaced"></a>Öneriler API'lerini değiştirildi

Ayrılmış örnek satın alma önerileri API'leri son 7, 30 ve 60 gün içinde sanal makine kullanımı sağlar. API'leri, rezervasyon satın alma önerileri de sağlar. Bunlara aşağıdakiler dahildir:

- [Paylaşılan ayrılmış örnek öneri API'si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-shared-reserved-instance-recommendations)
- [Tek bir ayrılmış örnek öneriler API'si](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation#request-for-single-reserved-instance-recommendations)

Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request). API'leri daha önce listelenen ayırma önerileri değiştirilir [Microsoft.Consumption/reservationRecommendations](/rest/api/consumption/reservationrecommendations/list) API.

Ayırma öneriler API'si ile ayırma öneriler almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationRecommendations?api-version=2019-01-01` |

## <a name="reservation-usage-apis-replaced"></a>Ayırma kullanım değiştirilen API'ler

Ayrılmış örnek kullanımı API'si ile bir kayıt rezervasyon kullanım elde edebilirsiniz. Bir kayıt birden fazla ayrılmış örnek varsa, ayrıca kullanım, tüm bu API'yi kullanarak ayrılmış örnek satın alabilirsiniz.

Bunlara aşağıdakiler dahildir:

- [Ayrılmış örnek kullanım ayrıntıları](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-details)
- [Ayrılmış örnek Kullanım Özeti](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage#request-for--reserved-instance-usage-summary)

Tüm tüketim API'leri, Azure AD kimlik doğrulama ve yetkilendirme için kullanın. yerel Azure API'leri ile değiştirilir. Azure REST API'lerini çağırmayla ilgili daha fazla bilgi için bkz. [REST ile çalışmaya başlama](/rest/api/azure/#create-the-request). API'leri daha önce listelenen ayırma önerileri değiştirilir [Microsoft.Consumption/reservationDetails](/rest/api/consumption/reservationsdetails) ve [Microsoft.Consumption/reservationSummaries](/rest/api/consumption/reservationssummaries) API'leri.

Rezervasyon ayrıntıları API'siyle rezervasyon ayrıntıları almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationDetails?api-version=2019-01-01` |

Ayırma özetleri ayırma özetleri API'si ile almak için:

| Yöntem | İstek URI'si |
| --- | --- |
| GET | `https://management.azure.com/providers/Microsoft.Consumption/reservationSummaries?api-version=2019-01-01` |



## <a name="move-from-cloudyn-to-cost-management"></a>Maliyet Yönetimi Cloudyn'den Taşı

Kullanan kuruluşlar [Cloudyn](https://cloudyn.com) kullanarak başlamalıdır [Azure maliyet Yönetimi](https://azure.microsoft.com/services/cost-management/) herhangi bir maliyet Yönetimi gereksinimleri için. Maliyet yönetimi, hiçbir ekleme ve bir sekiz saatlik gecikme süresine sahip Azure portalında kullanılabilir. Daha fazla bilgi için [maliyet Yönetimi belgeleri](index.yml).

Azure maliyet yönetimi ile şunları yapabilirsiniz:

- Önceden tanımlanmış bir bütçeyle karşılaştırmalı bir zaman içinde maliyet görüntüleyin. Günlük maliyet desenleri tanımlamak ve anomalileri harcama durdurmak için analiz edin. Maliyetlerin etiketler, kaynak grubu, hizmet ve konuma göre bölümlere ayırmak.
- Bütçe sınırlarını kullanım ve maliyetlerinizi ve önemli eşikleri yaklaşıldığında bildirim alın. oluşturun. Otomasyon ile özel olaylar tetikleyin ve sizin şartlarınıza göre sabit sınırlara zorlamak için eylem gruplarını ayarlayın.
- Maliyet ve kullanım önerileri Azure Danışmanı ile iyileştirin. Rezervasyon satın alma iyileştirmeler keşfedin, downsize kapatacağı sanal makineler ve bütçelerini içinde kalmak için kullanılmayan kaynakları silin.
- Günlük depolama hesabınıza bir CSV dosyası yayımlanacak bir maliyet ve kullanım verileri dışarı aktarma zamanlayın. Faturalama verileri eşitlenmiş durumda ve güncel tutmak için dış sistemlerle tümleştirme otomatikleştirin.

## <a name="power-bi-integration"></a>Power BI tümleştirmesi

Maliyet raporlama için Power BI kullanıyorsanız, geçiş için aşağıdakiler gerekir:

- Microsoft Azure tüketim öngörüleri Power BI uygulaması
- Azure tüketim öngörüleri Masaüstü Bağlayıcısı


Bağlayıcı en üst düzeyde esneklik isteyen kuruluşlar için tavsiye edilir. Ancak, Power BI uygulaması da hızlı kurulum için kullanılabilir durumdadır.

- Yükleme [Microsoft Azure tüketim öngörüleri Power BI uygulaması](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview)
- [Azure tüketim öngörüleri Bağlayıcısı ile bağlanma](/power-bi/desktop-connect-azure-consumption-insights)

Eski Consumption Insights İçerik Paketi ve bağlayıcı bir kayıt düzeyinde çalışmıştır. Bu, en azından okuma erişimi gereklidir. Yeni tüketim öngörüleri Power BI uygulamasını ve yeni Azure tüketim öngörüleri Bağlayıcısı faturalandırma profili kullanıcılar için kullanılabilir. Ek seçenekler maliyetleri gözden geçirme veya faturalama profilleri arasında maliyetleri görüntülemek üzere takımlar kullanmalıdır [maliyet analizi](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/costanalysis) Azure portalı.

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [maliyet Yönetimi belgeleri](index.yml) izlemek ve Azure harcama denetlemek nasıl öğrenmek için. Veya, maliyet yönetimi ile kaynak kullanımını en iyi duruma getirmek istiyorsanız.
