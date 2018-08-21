---
title: Azure hizmeti REST API'si ile kaynak kullanımını gözden geçirin | Microsoft Docs
description: Azure hizmet kaynak kullanımı gözden geçirmek için Azure REST API'lerini kullanmayı öğrenin.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2018
ms.author: alleonar
ms.openlocfilehash: 1b7b1455413fb4886b317d468e6d278111c094b1
ms.sourcegitcommit: 974c478174f14f8e4361a1af6656e9362a30f515
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2018
ms.locfileid: "40226025"
---
# <a name="review-azure-resource-usage-using-the-rest-api"></a>REST API kullanarak Azure kaynak kullanımını gözden geçirin


Azure [tüketim API'leri](https://docs.microsoft.com/rest/api/consumption/) Yardım, Azure kaynaklarınızın maliyet ve kullanım verileri inceleyin.

Bu sonuçları filtrelemek için temel olarak nasıl bu makalede, almak ve bir Azure kaynak grubundaki kaynaklar için kaynak kullanım bilgilerini toplamak de öğrenirsiniz [Azure resource manager etiketleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags).

## <a name="get-usage-for-a-resource-group"></a>Kullanım için bir kaynak grubu Al

İşlem, veritabanı ve diğer kaynakları bir kaynak grubu, kaynak kullanımını almak için kullanın `usageDetails` REST işlemini ve kaynak grubuna göre sonuçları filtreleyin.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Consumption/usageDetails?api-version=2018-06-30&filter=properties/resourceGroup eq '{resource-group}]
Content-Type: application/json   
Authorization: Bearer
```

`{subscription-id}` Parametresi gereklidir ve okuyucu rolüne sahip {kaynak-grubu} kaynak grubunda erişebilen bir abonelik kimliği içermelidir. 

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik türü:*| Gereklidir. Kümesine `application/json`. |  
|*Yetkilendirme:*| Gereklidir. Geçerli bir kümesi `Bearer` belirteci. |

### <a name="response"></a>Yanıt  

200 (Tamam) durum kodunu subscriptipon Kimliğine sahip kaynak grubunda her Azure kaynağı için kullanım istatistiklerini listesini içeren başarılı bir yanıt için döndürülen `00000000-0000-0000-0000-000000000000`.

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resource-group/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    } ] }
```

## <a name="get-usage-for-tagged-resources"></a>Kullanım için etiketli kaynakları alma

Etiketlere göre düzenlenmiş kaynaklarını kullanmak için kaynak kullanımını almak için `usageDetails` REST işlemini ve etiket adını kullanarak sonuçları filtreleyin `$filter` sorgu parametresi.

```http
https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Consumption/usageDetails?$filter=tags eq 'tag1'&api-version=2018-06-30
Content-Type: application/json   
Authorization: Bearer
```

`{subscription-id}` Parametresi gereklidir ve etiketli kaynaklara erişebilen bir abonelik kimliği içermelidir.


### <a name="response"></a>Yanıt  

200 (Tamam) durum kodunu subscriptipon Kimliğine sahip kaynak grubunda her Azure kaynağı için kullanım istatistiklerini listesini içeren başarılı bir yanıt için döndürülen `00000000-0000-0000-0000-000000000000` ve etiket adı anahtar kasası çifti `dev` ve `tools`. 

Örnek yanıt:

```json
{
  "value": [
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/usageDetailsId1",
      "name": "usageDetailsId1",
      "type": "Microsoft.Consumption/usageDetails",
      "tags": {
        "dev": "tools"
      },
      "properties": {
        "billingPeriodId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/billingPeriods/201702",
        "invoiceId": "/subscriptions/00000000-0000-0000-0000-000000000000/providers/Microsoft.Billing/invoices/201703-123456789",
        "usageStart": "2017-02-13T00:00:00Z",
        "usageEnd": "2017-02-13T23:59:59Z",
        "instanceName": "shared1",
        "instanceId": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-Web-eastasia/providers/Microsoft.Web/sites/shared1",
        "instanceLocation": "eastasia",
        "currency": "USD",
        "usageQuantity": 0.00328,
        "billableQuantity": 0.00328,
        "pretaxCost": 0.67,
        "isEstimated": false,
        "meterId": "00000000-0000-0000-0000-000000000000",
        "partNumber": "Part Number 1",
        "resourceGuid": "00000000-0000-0000-0000-000000000000",
        "offerId": "Offer Id 1",
        "chargesBilledSeparately": true,
        "location": "EU West"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Sonraki adımlar
- [Azure REST API'si ile çalışmaya başlama](https://docs.microsoft.com/rest/api/azure/)   
