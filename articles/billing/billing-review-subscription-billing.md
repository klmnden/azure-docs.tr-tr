---
title: REST API'si ile Azure abonelik faturalama verileri gözden geçirin | Microsoft Docs
description: Abonelik fatura ayrıntılarını gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: erikre
ms.openlocfilehash: 0a73462b7fdbaf6386a3051a72da755f31ff8dd2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65192115"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Abonelik faturalama REST API'lerini kullanarak gözden geçirin

Gözden geçirin ve Azure maliyetlerinizi yönetin, azure API'leri raporlama Yardımı.

Filtre sonuçlarını gereksinimlerinizi karşılayacak şekilde özelleştirmek yardımcı olur.

Burada, aboneliğe ait fatura ayrıntılarını belirli bir tarih aralığındaki için döndürülecek bir REST API'sini kullanmayı öğrenin.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>Derleme isteği

`{subscriptionID}` Parametresi gereklidir ve hedef abonelik tanımlar.

`{billingPeriod}` Parametresi gereklidir ve bir geçerli belirtir [fatura döneminde](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods).

`${startDate}` Ve `${endDate}` parametreleri, bu örnek için gerekli, ancak uç noktası için isteğe bağlıdır. Bunlar YYYY-AA-GG biçiminde bir dize olarak tarih aralığını belirtin (örnek: `'20180501'` ve `'20180615'`).

Aşağıdaki üst bilgiler gereklidir:

|İstek üstbilgisi|Açıklama|
|--------------------|-----------------|
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|
|*Authorization:*|Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |

## <a name="response"></a>Yanıt

Durum kodu 200 (Tamam) ayrıntılı maliyetlerin hesabınız için bir listesini içeren başarılı bir yanıt döndürdü.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": "${usageQuantity}",
        "billableQuantity": "${billableQuantity}",
        "pretaxCost": "${cost}",
        "meterId": "${meterID}",
        "meterDetails": "${meterDetails}"
      }
    }
  ],
  "nextLink": "${nextLinkURL}"
}
```

Her öğe **değer** hizmet kullanımına ilişkin bir ayrıntı temsil eder:

|Yanıt özelliği|Açıklama|
|----------------|----------|
|**subscriptionGuid** | Abonelik için genel olarak benzersiz kimliği. |
|**startDate** | Başlatılan kullanımı tarih. |
|**endDate** | Sona erdi kullanımı tarih. |
|**useageQuantity** | Kullanılan miktar. |
|**billableQuantity** | Miktar gerçekten faturalandırılır. |
|**pretaxCost** | , Vergileri önce Faturalanan Maliyet. |
|**meterDetails** | Kullanımı hakkında ayrıntılı bilgi sağlar. |
|**nextLink**| Ne zaman ayarlama, sonraki "sayfasının" Ayrıntıları URL'sini belirtir. Sonuncu sayfanın olduğunda boş. |

Bu örnekte kadar kısaltılmıştır; bkz: [listesinde kullanım ayrıntıları](https://docs.microsoft.com/rest/api/consumption/usagedetails/list#usagedetailslistforbillingperiod) her yanıt alan eksiksiz bir açıklaması.

Diğer durum kodları hata koşulları belirtin. Bu gibi durumlarda, isteğin neden başarısız yanıt nesnesini açıklar.

``` json
{
  "error": [
    {
      "code": "Error type.",
      "message": "Error response describing why the operation failed."
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
- Gözden geçirme [Kurumsal raporlama genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Araştırma [Kurumsal faturalama REST API'si](https://docs.microsoft.com/rest/api/billing/)
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)
