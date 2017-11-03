---
title: "Azure olay kılavuz abonelik şeması"
description: "Azure olay kılavuz sahip bir olay abone için özellikleri açıklar."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 08/17/2017
ms.author: babanisa
ms.openlocfilehash: eff2352066a76010d6d882a7b7e1961870cd2d46
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="event-grid-subscription-schema"></a>Kılavuz abonelik şeması

Bir olay kılavuz aboneliği oluşturmak için olay oluşturma abonelik işlemi için bir istek gönderin. Aşağıdaki biçimi kullanın:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Örneğin, adında bir depolama hesabı için bir olay aboneliği oluşturmak için `examplestorage` bir kaynak grubunda adlı `examplegroup`, aşağıdaki biçimi kullanın:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2017-06-15-preview
``` 

Bu makalede özellikleri ve istek gövdesi için şema anlatılmaktadır.
 
## <a name="event-subscription-properties"></a>Olay abonelik özellikleri

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Hedef | Nesne | Uç nokta tanımlayan nesnesi. |
| Filtre | Nesne | Olay türlerini filtrelemek için isteğe bağlı bir alan. |

### <a name="destination-object"></a>hedef nesne

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| endpointType | Dize | (Web kancası/HTTP, olay hub'ı veya sıra) aboneliği için uç nokta türü. | 
| endpointUrl | Dize |  | 

### <a name="filter-object"></a>filtre nesnesi

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| includedEventTypes | Dizi | Olay türü olay iletisini bu olay türü adları birine tam bir eşleşme eşleşmedir. Olay adı için olay kaynağı kayıtlı olay türü adları eşleşmediğinde bir hata oluşturur. Varsayılan, tüm olay türleri ile eşleşir. |
| subjectBeginsWith | Dize | Bir önek eşleştirme Konu alanına olay iletisi filtreleyin. Varsayılan ya da boş dize tüm eşleşir. | 
| subjectEndsWith | Dize | Sonek eşleşme Konu alanına olay iletisi filtreleyin. Varsayılan ya da boş dize tüm eşleşir. |
| subjectIsCaseSensitive | Dize | Denetimler için filtreleri ile eşleşen büyük küçük harfe duyarlı. |


## <a name="example-subscription-schema"></a>Abonelik şema örneği

```json
{
  "properties": {
    "destination": {
      "endpointType": "webhook",
      "properties": {
          "endpointUrl": "https://example.azurewebsites.net/api/HttpTriggerCSharp1?code=VXbGWce53l48Mt8wuotr0GPmyJ/nDT4hgdFj9DpBiRt38qqnnm5OFg=="
      }
    },
    "filter": {
      "includedEventTypes": [ "blobCreated", "blobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "subjectIsCaseSensitive": "true"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)