---
title: "Azure olay kılavuz olay hub'ları olay şeması"
description: "Olay hub'ları Azure olay kılavuz olaylarla için sağlanan özellikler açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: tomfitz
ms.openlocfilehash: 80959ee589a1cfcf317a98c3bafd7f92c796fc2d
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-event-hubs"></a>Olay hub'ları için Azure olay kılavuz şeması

Bu makale, olay hub'ları olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md).

### <a name="available-event-types"></a>Kullanılabilir olay türleri

Olay hub'ları yayar **Microsoft.EventHub.CaptureFileCreated** yakalama dosyası oluşturulurken olay türü.

## <a name="example-event"></a>Örnek olayı

Bu örnek olay yakalama özelliği bir dosya depoladığında gerçekleşen bir olay hub'ları olay şeması gösterir: 

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
        }
    }
]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | Dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. |
| Konu | Dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| Olay türü | Dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | Dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | Dize | Olay için benzersiz tanımlayıcı. |
| Veri | Nesne | Olay hub'ı olay verileri. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| FileURL ' | Dize | Yakalama dosyasının yolu. |
| Dosya türü | Dize | Yakalama dosyasının dosya türü. |
| PartitionID | Dize | Parça kimliği. |
| sizeInBytes | tamsayı | Dosya boyutu. |
| eventCount | tamsayı | Dosyasındaki olay sayısıdır. |
| firstSequenceNumber | tamsayı | En küçük sıra numarası kuyruktan. |
| lastSequenceNumber | tamsayı | Son sıra numarası kuyruktan. |
| firstEnqueueTime | Dize | Kuyruktan ilk kez. |
| lastEnqueueTime | Dize | Kuyruktan son zamanı. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
* Olay hub'ları olayları işleme hakkında daha fazla bilgi için bkz: [veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md).