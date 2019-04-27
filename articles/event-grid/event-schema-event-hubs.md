---
title: Azure Event Grid olay hub'ları olay şeması
description: Azure Event Grid ile olay hub'ları olaylar için sağlanan özellikleri tanımlar
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: reference
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 9c0113687d27bf43375f298057129a5594ec0a06
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60561837"
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Event hubs için Azure Event Grid olay şeması

Bu makale, olay hub'ları olaylar için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

Örnek betikler ve öğreticiler listesi için bkz: [Event Hubs olay kaynağı](event-sources.md#event-hubs).

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Olay hub'ları yayan **Microsoft.EventHub.CaptureFileCreated** yakalama dosyası oluşturulduğunda olay türü.

## <a name="example-event"></a>Örnek olay

Bu örnek olay yakalama özelliği, bir dosyayı depolarıyla çalışırken gerçekleşen bir olay hub'ları olay şeması gösterir: 

```json
[
    {
        "topic": "/subscriptions/<guid>/resourcegroups/rgDataMigrationSample/providers/Microsoft.EventHub/namespaces/tfdatamigratens",
        "subject": "eventhubs/hubdatamigration",
        "eventType": "Microsoft.EventHub.CaptureFileCreated",
        "eventTime": "2017-08-31T19:12:46.0498024Z",
        "id": "14e87d03-6fbf-4bb2-9a21-92bd1281f247",
        "data": {
            "fileUrl": "https://tf0831datamigrate.blob.core.windows.net/windturbinecapture/tfdatamigratens/hubdatamigration/1/2017/08/31/19/11/45.avro",
            "fileType": "AzureBlockBlob",
            "partitionId": "1",
            "sizeInBytes": 249168,
            "eventCount": 1500,
            "firstSequenceNumber": 2400,
            "lastSequenceNumber": 3899,
            "firstEnqueueTime": "2017-08-31T19:12:14.674Z",
            "lastEnqueueTime": "2017-08-31T19:12:44.309Z"
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri vardır:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| konu başlığı | string | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| konu | string | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | string | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | string | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | string | Olayın benzersiz tanımlayıcısı. |
| veriler | object | Olay hub'ı olay verileri. |
| dataVersion | string | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | string | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| FileURL ' | string | Yakalama dosyası yolu. |
| fileType | string | Yakalama dosyası dosya türü. |
| PartitionID | string | Parça kimliği. |
| sizeInBytes | integer | Dosya boyutu. |
| eventCount | integer | Bu dosyadaki olaylar sayısı. |
| firstSequenceNumber | integer | Kuyruğu'ndan küçük sıra numarası. |
| lastSequenceNumber | integer | Kuyruğu'ndan son sıra numarası. |
| firstEnqueueTime | string | Kuyruğu'ndan ilk kez. |
| lastEnqueueTime | string | Kuyruğu'ndan son zaman. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
* Olay hub'ları olayları işleme hakkında daha fazla bilgi için bkz: [bir veri ambarına büyük veri Stream](event-grid-event-hubs-integration.md).