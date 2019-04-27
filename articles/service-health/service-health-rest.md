---
title: Azure kaynak durumu olayları REST API kullanarak alın | Microsoft Docs
description: Azure kaynaklarınız için sistem durumu olaylarını almak için Azure REST API'lerini kullanın.
author: stephbaron
ms.author: stbaron
ms.service: service-health
ms.custom: REST
ms.topic: article
ms.date: 06/06/2017
ms.openlocfilehash: 6d83aed6910127ceb34b9a694f48ca9c19ab6d18
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60790921"
---
# <a name="get-resource-health-using-the-rest-api"></a>REST API kullanarak kaynak durumu alma 

Bu örnek makale sistem durumu olaylarını kullanarak Azure kaynakları için bir listesini almak nasıl [Azure REST API'si](/rest/api/azure/).

Eksiksiz başvuru belgeleri ve REST API için ek örneklerini bulunan [Azure İzleyici REST başvurusu](/rest/api/monitor). 

## <a name="build-the-request"></a>Derleme isteği

Aşağıdaki `GET` arasındaki zaman aralığı için aboneliğiniz için sistem durumu olaylarını listelemek için HTTP isteği `2018-05-16` ve `2018-06-20`.

```http
https://management.azure.com/subscriptions/{subscription-id}/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&%24filter=eventTimestamp%20ge%20'2018-05-16T04%3A36%3A37.6407898Z'%20and%20eventTimestamp%20le%20'2018-06-20T04%3A36%3A37.6407898Z'
```

### <a name="request-headers"></a>İstek üst bilgileri

Aşağıdaki üst bilgiler gereklidir: 

|İstek üstbilgisi|Açıklama|  
|--------------------|-----------------|  
|*İçerik türü:*|Gereklidir. Kümesine `application/json`.|  
|*Yetkilendirme:*|Gereklidir. Geçerli bir kümesi `Bearer` [erişim belirteci](/rest/api/azure/#authorization-code-grant-interactive-clients). |  

### <a name="uri-parameters"></a>URI parametreleri

| Ad | Açıklama |
| :--- | :---------- |
| subscriptionId | Bir Azure aboneliği tanımlayan abonelik kimliği. Birden fazla aboneliğiniz varsa, bkz. [birden çok abonelik ile çalışma](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest). |
| API sürümü | İstek için kullanılacak API sürümü.<br /><br /> Api sürümü bu belgede ele alınmaktadır `2015-04-01`, yukarıdaki URL'deki yer.  |
| $filter | Döndürülen sonuç kümesini azaltmak için filtre seçeneği. Bu parametre için izin verilen desenler kullanılabilir [başvurusu için etkinlik günlüklerini işlemi](/rest/api/monitor/activitylogs/list#uri-parameters). Gösterilen örnek 2018-05-16 ve 2018-06-20 arasında bir zaman aralığındaki tüm olayları yakalar. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>İstek gövdesi

Hiçbir istek gövdesi, bu işlem için gereklidir.

## <a name="handle-the-response"></a>Yanıtı işleme

200 durum kodu ile birlikte filtre parametresi için karşılık gelen sistem durumu olayı değer listesi ile döndürülen bir `nextlink` sonraki sonuç sayfasını alacak için URI.

## <a name="example-response"></a>Örnek yanıt 

```json
{
  "value": [
    {
      "correlationId": "1e121103-0ba6-4300-ac9d-952bb5d0c80f",
      "eventName": {
        "value": "EndRequest",
        "localizedValue": "End request"
      },
      "id": "/subscriptions/{subscription-id}/resourceGroups/MSSupportGroup/providers/microsoft.support/supporttickets/115012112305841/events/44ade6b4-3813-45e6-ae27-7420a95fa2f8/ticks/635574752669792776",
      "resourceGroupName": "MSSupportGroup",
      "resourceProviderName": {
        "value": "microsoft.support",
        "localizedValue": "microsoft.support"
      },
      "operationName": {
        "value": "microsoft.support/supporttickets/write",
        "localizedValue": "microsoft.support/supporttickets/write"
      },
      "status": {
        "value": "Succeeded",
        "localizedValue": "Succeeded"
      },
      "eventTimestamp": "2015-01-21T22:14:26.9792776Z",
      "submissionTimestamp": "2015-01-21T22:14:39.9936304Z",
      "level": "Informational"
    }
  ],
  "nextLink": "https://management.azure.com/########-####-####-####-############$skiptoken=######"
}
```
