---
title: Azure olay kılavuz abonelik şeması
description: Azure olay kılavuz sahip bir olay abone için özellikleri açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 05/02/2018
ms.author: babanisa
ms.openlocfilehash: cfb4dabea12f2988108d24b025e324cf05afb325
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="event-grid-subscription-schema"></a>Kılavuz abonelik şeması

Bir olay kılavuz aboneliği oluşturmak için olay oluşturma abonelik işlemi için bir istek gönderin. Aşağıdaki biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/{group-name}/providers/{resource-provider}/{resource-type}/{resource-name}/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Örneğin, adında bir depolama hesabı için bir olay aboneliği oluşturmak için `examplestorage` bir kaynak grubunda adlı `examplegroup`, aşağıdaki biçimi kullanın:

```HTTP
PUT /subscriptions/{subscription-id}/resourceGroups/examplegroup/providers/Microsoft.Storage/storageaccounts/examplestorage/Microsoft.EventGrid/eventSubscriptions/{event-type-definitions}?api-version=2018-01-01
``` 

Bu makalede özellikleri ve istek gövdesi için şema anlatılmaktadır.
 
## <a name="event-subscription-properties"></a>Olay abonelik özellikleri

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Hedef | object | Uç nokta tanımlayan nesnesi. |
| filtre | object | Olay türlerini filtrelemek için isteğe bağlı bir alan. |

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
| isSubjectCaseSensitive | dize | Denetimler için filtreleri ile eşleşen büyük küçük harfe duyarlı. |


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
      "includedEventTypes": [ "Microsoft.Storage.BlobCreated", "Microsoft.Storage.BlobDeleted" ],
      "subjectBeginsWith": "blobServices/default/containers/mycontainer/log",
      "subjectEndsWith": ".jpg",
      "isSubjectCaseSensitive ": "true"
    }
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

* Olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)