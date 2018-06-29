---
title: REST API ile Azure abonelik faturalama verileri gözden | Microsoft Docs
description: Abonelik faturalama ayrıntıları gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: cc29d1f613af67604d50654be794cc90080098bb
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064051"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Abonelik faturalama REST API'lerini kullanarak gözden geçirme

Azure API Yardım raporlama gözden geçirmenizi ve Azure maliyetleriniz yönetin.  

Filtreler gereksinimlerinizi karşılayacak şekilde sonuçlarını özelleştirme yardımcı olur.

Burada, belirtilen tarih aralığı için abonelik faturalama ayrıntıları döndürmek için bir REST API kullanmayı öğrenin.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Oluşturma isteği  

`{subscriptionID}` Parametresi gereklidir ve hedef abonelik tanımlar.

`{billingPeriod}` Parametresi gereklidir ve geçerli bir belirtir [fatura döneminde](https://docs.microsoft.com/rest/api/billing/billingperiods/get#billingperiod).

`${startDate}` Ve `${endDate}` Bu örnek için gerekli, ancak uç noktası için isteğe bağlı parametre.  Bunlar YYYY-AA-GG biçiminde dizeleri olarak tarih aralığını belirtin (örnek: `'20180501'` ve `'20180615'`). 

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*Content-Type:*|Gereklidir. Kümesine `application/json`.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir ayarla `Bearer` [erişim belirteci](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients). |  

## <a name="response"></a>Yanıt  

Durum kodu 200 (Tamam) hesabınız için ayrıntılı maliyetlerin bir listesini içeren başarılı bir yanıt döndürdü.

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
        "usageQuantity": ${usageQuantity},
        "billableQuantity": ${billableQuantity},
        "pretaxCost": ${cost},
        "meterId": "${meterID}",
        "meterDetails": ${meterDetails}
      }
    }
    ],
    "nextLink": "${nextLinkURL}"
} 
```  

Her öğe **değeri** hizmet kullanımına ilişkin ayrıntılar temsil eder:

|Yanıt özelliği|Açıklama|
|----------------|----------|
|**subscriptionGuid** | Abonelik için genel benzersiz kimliği. | 
|**startDate** | Başlatılan kullanım tarihi. |
|**endDate** | Sona erdi kullanım tarihi. |
|**useageQuantity** | Kullanılan miktar. | 
|**billableQuantity** | Miktar gerçekte faturalandırılır. |
|**pretaxCost** | , Geçerli vergileri önce Faturalandı maliyeti. | 
|**meterDetails** | Kullanımı hakkında ayrıntılı bilgi. |
|**nextLink**| Ne zaman ayarlama, sonraki "sayfasının" Ayrıntıları URL'sini belirtir. Sayfa sonuncu olduğunda boş. |  
||
  
Bu örnek kadar kısaltılmıştır; bkz: [listesinde kullanım ayrıntıları](https://docs.microsoft.com/rest/api/consumption/usagedetails/listbybillingperiod#usagedetailslistresult) her yanıtı alanının eksiksiz bir açıklaması. 

Diğer durum kodları hata koşulları belirtin. Bu durumlarda, isteğin neden başarısız yanıt nesnesi açıklanmaktadır.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Sonraki adımlar 
- Gözden geçirme [Kurumsal raporlamaya genel bakış](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Araştırmak [REST API faturalama Enterprise](https://docs.microsoft.com/rest/api/billing/)   
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
