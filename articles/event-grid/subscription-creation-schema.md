---
title: "Azure olay kılavuz abonelik şeması"
description: "Azure olay kılavuz sahip bir olay abone için özellikleri açıklar."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: babanisa
ms.openlocfilehash: a915473c67a7577582837b56d1a9ccec4d21c461
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="event-grid-subscription-schema"></a>Kılavuz abonelik şeması

Bir olay kılavuz aboneliği oluşturmak için olay oluşturma abonelik işlemi için bir istek gönderin. Aşağıdaki biçimi kullanın:

```
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Örneğin, adında bir depolama hesabı için bir olay aboneliği oluşturmak için `examplestorage` bir kaynak grubunda adlı `examplegroup`, aşağıdaki biçimi kullanın:

```
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Bu makalede özellikleri ve istek gövdesi için şema anlatılmaktadır.
 
## <a name="event-subscription-properties"></a>Olay abonelik özellikleri

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Hedef | nesne | Uç nokta tanımlayan nesnesi. |
| filtre | nesne | Olay türlerini filtrelemek için isteğe bağlı bir alan. |

### <a name="destination-object"></a>hedef nesne

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| endpointType | dize | (Web kancası/HTTP, olay hub'ı veya sıra) aboneliği için uç nokta türü. | 
| endpointUrl | dize | Bu olay aboneliği olayları için hedef URL. | 

### <a name="filter-object"></a>filtre nesnesi

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| includedEventTypes | array | Olay türü olay iletisini bu olay türü adları birine tam bir eşleşme eşleşmedir. Olay adı için olay kaynağı kayıtlı olay türü adları eşleşmediğinde bir hata oluşturur. Varsayılan, tüm olay türleri ile eşleşir. |
| subjectBeginsWith | dize | Bir önek eşleştirme Konu alanına olay iletisi filtreleyin. Varsayılan ya da boş dize tüm eşleşir. | 
| subjectEndsWith | dize | Sonek eşleşme Konu alanına olay iletisi filtreleyin. Varsayılan ya da boş dize tüm eşleşir. |
| subjectIsCaseSensitive | dize | Denetimler için filtreleri ile eşleşen büyük küçük harfe duyarlı. |


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