---
title: Microsoft Müşteri sözleşmesi API - Azure için Kurumsal Anlaşmamdan geçirme | Microsoft Docs
description: Bu makale bir Microsoft Müşteri sözleşmesi için Microsoft Kurumsal Anlaşma (EA) öğesinden geçirdiğinizde API'leri etkisini anlamanıza yardımcı olur.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 03/02/2019
ms.topic: conceptual
ms.service: cost-management
manager: micflan
ms.custom: ''
ms.openlocfilehash: a12360e60026fd4251cc232caccd1bc52a34d2ff
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2019
ms.locfileid: "57344970"
---
# <a name="migrate-from-enterprise-agreement-to-microsoft-customer-agreement-apis"></a>Kurumsal Sözleşme, Microsoft Müşteri sözleşmesi API'lerine geçiş

Bu makale bir Microsoft Müşteri sözleşmesi için Microsoft Kurumsal Anlaşma (EA) öğesinden geçirdiğinizde API'leri etkisini anlamanıza yardımcı olur. Aşağıdaki yeni API'leri, maliyet ve fiyatlandırma görüntülemek için kullanılır:

- Microsoft Müşteri sözleşmesi için fiyat listesi API'si
  - Faturayla fiyat listesi
  - Faturalandırma profili tarafından fiyat listesi
  - Açık bir fatura dönemi için fiyat listesi
- Microsoft Müşteri sözleşmesi için kullanım ayrıntılarını API
  - Fatura profili faturalandırma (fatura bölümüne göre filtrele) için kullanım ayrıntıları
    - Kullanım tarihine göre kullanım ayrıntıları
    - Kullanım ayrıntıları için fatura hesabı
    - Kullanım ayrıntıları için faturalandırma profili
    - Fatura bölümü için kullanım ayrıntıları
    - Abonelik için kullanım ayrıntıları
  - Kullanım ayrıntıları için ay başından bugüne
    - Kullanım ayrıntıları için fatura hesabı
    - Kullanım ayrıntıları için faturalandırma profili
    - Fatura bölümü için kullanım ayrıntıları
    - Abonelik için kullanım ayrıntıları
  - Fatura dönemi göre kullanım ayrıntıları
- Microsoft Müşteri sözleşmesi için API kredi
  - Geçerli bakiye özeti API'si tarafından faturalandırma profili
  - Olayları faturalandırma profili tarafından API kredi
  - Çok sayıda API faturalandırma profili tarafından kredi
- Sorgu API'si
- API ücretleri
  - Fatura hesabı tarafından API ücretleri
  - Faturalandırma profili tarafından API ücretleri
  - Ücretler fatura bölümünde API
- Fatura listesi API'si
  - Faturalandırma profili tarafından
  - Fatura hesabı tarafından
- Rezervasyon API'leri
- Power BI tümleştirmesi API
- Dışarı aktarmalar API
  - Fatura hesabı tarafından
  - Faturalandırma profili tarafından
  - Fatura bölümü tarafından
- İşlem API'si
- Bütçe API
- Toplam Maliyet API

## <a name="effect-on-automation"></a>Otomasyon etkisi

Microsoft Müşteri Sözleşmesi ile kullanılan API'ler, olanları ile Kurumsal Sözleşme kullanılan farklıdır. API'leri veya Azure Resource Manager tabanlı API'ler Kurumsal kayıtları için API automation Kurumsal anahtar tabanlı kullanarak önceden ayarlamış olabilirsiniz. Bu durumda, Microsoft Müşteri Sözleşmesi ile kullanılan yeni API'ler için Otomasyon yapılandırmanızı güncelleştirin.

Microsoft Müşteri sözleşmesi tarafından desteklenmeyen eski API'ler listesini görmek için bkz: [desteklenmeyen API](#unsupported-apis).

Microsoft Müşteri sözleşmesi için API desteği yalnızca Azure Resource Manager kimlik doğrulaması — bunlar API anahtar tabanlı kimlik doğrulamasını desteklemez.

## <a name="pricesheet-apis-show-prices-for-azure-services"></a>Azure Hizmetleri için fiyatlar fiyat listesi API'leri Göster

Microsoft Müşteri sözleşmesi eklemek için fiyatlandırma bilgileri için faturalandırma, derecelendirilir tüm hizmetlerin fiyat listesi API'leri kullanım miktar temel alınarak. API yetkilendirmesini satın alma işlemleri içermez. Örneğin, Market yetkilendirmesini satın alma işlemleri, ayrılmış örnek fiyatları ve Destek teklifleri. API her hizmet belirli bir ölçüm ve ürün siparişi adı için benzersiz bir fiyat sağlar.

### <a name="view-the-pricesheet-for-azure-consumption-services-by-billing-profile"></a>Azure tüketim Hizmetleri için bir fiyat listesi profili faturalama tarafından görüntüleyin

Fiyat listesi için bir faturalandırma profili tüm birinci ve üçüncü taraf Azure tüketim hizmetlerin fiyatlarını görüntüleyebilirsiniz. Azure tüketim hizmetler fiyatlarını görüntülemek için API'yi kullanın. Fatura profiline ait tüm abonelikleri için fatura profili gösterilen fiyatlar uygulanır.

- CSV biçimi
  - YAYINLA -`https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31& format=csv`
- JSON biçimi
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billingAccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&startDate=2019-01-01&endDate=2019-01-31&format=json`

### <a name="view-current-open-billing-or-service-period-estimated-prices"></a>Mevcut açık fatura görüntüleme veya hizmet fiyatları süresi tahmini

Tahmini fiyatlar tüm geçerli açık fatura döngüsü ya da hizmet süresini görüntülemek için CSV veya JSON biçiminde Pricesheets görüntüleyebilirsiniz.

- CSV biçimi
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv`
- JSON biçimi
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/{billing AccountId}/billingProfiles/{billingProfileId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json`

Fiyat listesi geçerli hizmet boyunca her zaman gösterilen Fiyatlar, ölçüm için en son fiyatlara yansıtılmıştır. Fiyatlar, en son fiyat için ölçüm ve ürün siparişi kullanarak günlük olarak yenilenir.

Açık bir dönem içinde fiyatını temel maliyetleri gözden geçirme, maliyetleri tahmin edilir. Son kilitli fiyat açık hizmet dönemin sonunda, açık bir dönem içinde kullanım için geçerlidir.

Açık dönemi, burada kullanım henüz maliyeti ve fatura derecelendirilmiş henüz geçerli hizmet süresini belirtir.

### <a name="view-invoice-by-billing-profile-prices"></a>Profil fiyatları faturalama Faturayı görüntüle

Belirli bir fatura faturalandırma profili için faturalandırılır Azure tüketim hizmetler fiyatlarını görüntüleyebilirsiniz. Yalnızca listelenen hizmetlerin fiyatlarını faturalandırıldığınızın için API'yi kullanın. Fatura Fatura numarasıyla tanımlanır.

- CSV biçimi
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=csv`

- JSON biçimi
  - POST
    - `https://management.azure.com/providers/Microsoft.Billing/billingAccounts/2909cffc-b0a2-5de1-bb7b-5d3383764184/billingProfiles/2dcffe0c-ee92-4265-8647-515b8fe7dc78/invoices/{invoiceId}/pricesheet/default/download?api-version=2018-11-01-preview&format=json`

`{invoiceId}` Listelenen önceden aynıdır **fatura numarası** fatura özeti PDF dosyasında gösterilen. Bir örnek aşağıda verilmiştir.

![Karşılık gelen fatura numarası için Fatura kodu gösteren örnek resim](./media/migrate-cost-management-api/invoicesummary.png)

## <a name="pricesheet-api-asynchronous-response"></a>Fiyat listesi API zaman uyumsuz yanıt

API'lerini destekleyen [Azure REST zaman uyumsuz](../azure-resource-manager/resource-manager-async-operations.md) biçimi. GET kullanarak API'leri çağırmak sonra şu yanıtı görürsünüz:

```
            No Response Body
            HTTP Status 202 Accepted
```

 Aşağıdaki üst bilgileri çıkış konumu ile gönderilir.

```
Location: https://management.azure.com/providers/Microsoft.Consumption/operationresults/{operationId}?sessiontoken=XZDFSnvdkbkdsb==
Azure-AsyncOperation: https://managment.azure.com/providers/Microsoft.Consumption/operationStatus/{operationId}?sessiontoken=XZDFSnvdkbkdsb==
   Retry-After: 10
 OData-EntityId: {operationId}
```

İstemci konumu için bir GET çağrısı yapabilirsiniz. GET çağrı yanıtı işlemi tamamlandığında veya başarısız durumuna ulaşana kadar daha önce gösterilen örnek bilgiler ile aynıdır. İşlemi aynı anda yürütülürse gibi tamamlandığında yanıt konumunda bir GET çağrısı için aynı yanıtı döndürür. Bir örnek aşağıda verilmiştir.

```
HTTP Status 200
i.e.
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

Ayrıca istemci de bir GET çağrısı yapabilirsiniz `Azure-AsyncOperation`. Uç nokta işlemi durumunu döndürür. Bir örneği aşağıda verilmiştir:

```
HTTP Status 200
               {
"id": “providers/Microsoft.Consumption/operationStatus/{operationId}",
"name": "operationId",
"status" : "Succeeded | Failed | Canceled",
"startTime": "<DateLiteral per ISO8601>",
"endTime": "<DateLiteral per ISO8601>",
"percentComplete": <double between 0 and 100>
"properties": {
    /\* The resource provider can choose the values here, but it should only be
       returned on a successful operation (status being "Succeeded"). \*/
},
"error" : {
    /\* This is the OData v4 format, used by the RPC and will go into the
     v2.2 Azure REST API guidelines \*/
    "code": "BadArgument",
    "message": "The provided database &#39;foo&#39; has an invalid username."
}
}
```

### <a name="pricesheet-fields"></a>Fiyat listesi alanları

Bölümündeki bilgileri gözden geçirin [, fiyat koşullarını anlamak için bir Microsoft Müşteri sözleşmesi](../billing/billing-mca-understand-pricesheet.md) tüm fiyat listesi alanları ve onların açıklamalarını görüntülemek için.

Kullanıma göre fiyatlandırma için kullanım fiyatı süresi başlayarak hizmet süresi için kilitli. Hizmet fiyat uygulanır ve satın alma tarihi temel kilitli. Hizmet, Azure Hizmetleri için genellikle bir takvim ayı dönemdir.

Bir ürün siparişi ve ölçüm için bir Microsoft Müşteri sözleşmesi Azure tüketim hizmetinde fiyatı benzersizdir.

Azure tüketim hizmetlerine fiyatı benzersiz olarak tanımlanabilmesi için kullanabileceğiniz alanlar şunlardır:

- productOrderName
- meterId (Ayrıca meterName)

Microsoft Müşteri anlaşmalarını fiyatı kayıt hizmetleri fiyatı için benzersiz olduğu kurumsal sözleşmeler farklı şekilde tanımlanır:

- Ürün
- PartNumber
- Ölçer
- Teklif

### <a name="pricesheet-support"></a>Fiyat listesi desteği

Şu anda fiyat listesi API'si yalnızca Microsoft Müşteri sözleşmesi fiyatları birinci taraf Azure ve üçüncü taraf tüketim hizmetlerine destekler. Market Hizmetleri, ayrılmış örnekleri ve Azure hibrit avantajları (AHUB) için bunu fiyatları desteklemiyor.

API, yalnızca hizmeti karşı ücretlendirilecektir birim fiyatı sağlar. Bu fiyat, üzerinde anlaşılan fiyat'den küçük veya eşittir ' dir.

## <a name="query-for-usage-and-costs"></a>Kullanımı ve maliyetleri için sorgu

Kullanım ayrıntılarını API'si, Azure ve üçüncü taraf Market hizmet kullanımı ve maliyetleri erişim sağlar. Fiyatlar, anlaşma için üzerinde anlaşılan fiyatlar temel alır. EA müşterileri için bir müşteri sözleşmesi taşıma fatura hesap veya profili kapsam faturalandırma için kullanım ayrıntılarını kullanmanız gerekir. Sorgu aşağıdaki istisnalar dışında herhangi bir kapsamda olabilir:

|   | Müşteri Sözleşmesi | Kurumsal Anlaşma | Tek tek anlaşma (PAYG) |
| --- | --- | --- | --- |
| Fatura hesabı | Evet | Evet | Yok |
| Faturalandırma profilleri (fatura) | Evet | Yok. Fatura hesabı bakın. | Yok. Abonelikleri görürsünüz. |
| Fatura bölümleri | Hayır. Fatura profillerini sahip bir filtre kullanın. | Yok | Yok |
| EA Departmanlar | Yok | Evet | Yok |
| EA kayıt hesapları | Yok | Evet | Yok. Abonelikleri görürsünüz. |
| Yönetim grupları | Hayır. Fatura hesabı kullanın. | Evet | Evet |
| Abonelikler | Evet | Evet | Evet |
| Kaynak grupları | Hayır. Abonelikler Filtresi ile kullanın. | Hayır. Abonelikler Filtresi ile kullanın. | Hayır. Abonelikler Filtresi ile kullanın. |

Sözleşme türü UsageDetails tarafından döndürülen özellikler farklıdır. Bkz: [UsageDetails API belgeleri](/rest/api/consumption/usagedetails/) tam listesi için.

### <a name="power-bi-integration"></a>Power BI tümleştirmesi

Microsoft Müşteri anlaşması için taşıma EA müşterileri, Microsoft Azure tüketim öngörüleri EA bilgilerini kullanarak Power BI ile tüm mevcut tümleştirmesi kullanılarak durdurmanız gerekir. Bunun yerine, kullanabilirsiniz:

- Power BI Desktop – oluştur yeni Power BI ile Microsoft Customer anlaşma için raporları [Azure maliyet Yönetimi Bağlayıcısı](https://docs.microsoft.com/power-bi/desktop-connect-azure-consumption-insights).
- Power BI hizmeti – taşımak [Microsoft Azure Consumption Insights](https://docs.microsoft.com/power-bi/service-connect-to-azure-consumption-insights) içerik paketi kullanılabilir Azure maliyet Yönetimi (müşteri sözleşmesi) uygulama [AppSource](https://appsource.microsoft.com/product/power-bi/pbi_azureconsumptioninsights.pbi-azure-consumptioninsights?tab=overview).

## <a name="unsupported-apis"></a>Desteklenmeyen API'leri

Aşağıdaki Kurumsal Anlaşma API'leri, Microsoft Müşteri sözleşmesi tarafından desteklenmez. Desteklenmeyen API'lere alternatifler açıklanmaktadır.

[Fiyat listesi tarafından abonelik](/rest/api/consumption/pricesheet/get/) desteklenmiyor. Subscriptionıd kapsamını fiyatı listeler. Bunun yerine kullanabileceğiniz **faturalandırma profili tarafından Pricesheets**. Bir abonelik kapsamında bir hizmet bedelinin faturalandırma profili kapsamında fiyatı aynıdır. API çağırma kullanıcı gerekli izinlere sahip olmalıdır.

**Sayfası - fatura dönemi tarafından alma fiyat** desteklenmiyor. Subscriptionıd ve fatura dönemi tarafından fiyat listesi için bir kapsam alır. Bunun yerine kullanabileceğiniz **faturalandırma profili tarafından Pricesheets**. Hizmet fiyat abonelik kapsamda bir faturalandırma profili kapsamı hizmeti fiyatına eşittir. API çağırma kullanıcı gerekli izinlere sahip olmalıdır.

[Kayıt kurumsal müşterileri için fiyat listesi](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet/) desteklenmiyor. Ve [kayıt bir fatura dönemi için Kurumsal müşteriler için fiyat listesi](/rest/api/billing/enterprise/billing-enterprise-api-pricesheet/) desteklenmiyor. Her ikisi de, belirli kayıt ve faturalama dönemi için her bir ölçüm için geçerli hızı sunar. Bunun yerine kullanabileceğiniz **faturalandırma profili tarafından Pricesheets**. Fiyat listesi faturalandırma profili için kullanılabilir pricesheets kayıt kapsamda olur. API çağırma kullanıcı gerekli izinlere sahip olmalıdır.

## <a name="see-also"></a>Ayrıca bkz.
- Daha fazla bilgi edinin [Azure tüketim REST API'lerini](/rest/api/consumption/).
