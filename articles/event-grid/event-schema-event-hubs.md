---
title: "Azure olay kılavuz olay hub'ları olay şeması"
description: "Olay hub'ları Azure olay kılavuz olaylarla için sağlanan özellikler açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: 9fdc8816d8db88d4f1fd7b6ce722b7d2763eeaeb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
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
        },
        "dataVersion": "",
        "metadataVersion": "1"
    }
]
```

## <a name="event-properties"></a>Olay Özellikleri

Bir olay aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. Bu değer olay kılavuz sağlar. |
| Konu | dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| eventType | dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | dize | Olay için benzersiz tanımlayıcı. |
| veriler | nesne | Olay hub'ı olay verileri. |
| dataVersion | dize | Veri nesnesi şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta veri şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| fileUrl | dize | Yakalama dosyasının yolu. |
| fileType | dize | Yakalama dosyasının dosya türü. |
| partitionId | dize | Parça kimliği. |
| sizeInBytes | integer | Dosya boyutu. |
| eventCount | integer | Dosyasındaki olay sayısıdır. |
| firstSequenceNumber | integer | En küçük sıra numarası kuyruktan. |
| lastSequenceNumber | integer | Son sıra numarası kuyruktan. |
| firstEnqueueTime | dize | Kuyruktan ilk kez. |
| lastEnqueueTime | dize | Kuyruktan son zamanı. |

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
* Olay hub'ları olayları işleme hakkında daha fazla bilgi için bkz: [veri ambarına büyük veri akışı](event-grid-event-hubs-integration.md).