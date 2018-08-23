---
title: Azure Event Grid olay hub'ları olay şeması
description: Azure Event Grid ile olay hub'ları olaylar için sağlanan özellikleri tanımlar
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: reference
ms.date: 08/17/2018
ms.author: tomfitz
ms.openlocfilehash: e301f3895126ed52b8d4c1f046f69dfcedb3563c
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42057439"
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Event hubs için Azure Event Grid olay şeması

Bu makale, olay hub'ları olaylar için şema ve özellikleri sağlar. Olay şemaları için bir giriş için bkz [Azure Event Grid olay şeması](event-schema.md).

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
| konu başlığı | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| Konu | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| olay türü | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| veri | object | Olay hub'ı olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesi, aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| FileURL ' | dize | Yakalama dosyası yolu. |
| fileType | dize | Yakalama dosyası dosya türü. |
| PartitionID | dize | Parça kimliği. |
| sizeInBytes | integer | Dosya boyutu. |
| eventCount | integer | Bu dosyadaki olaylar sayısı. |
| firstSequenceNumber | integer | Kuyruğu'ndan küçük sıra numarası. |
| lastSequenceNumber | integer | Kuyruğu'ndan son sıra numarası. |
| firstEnqueueTime | dize | Kuyruğu'ndan ilk kez. |
| lastEnqueueTime | dize | Kuyruğu'ndan son zaman. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
* Olay hub'ları olayları işleme hakkında daha fazla bilgi için bkz: [bir veri ambarına büyük veri Stream](event-grid-event-hubs-integration.md).