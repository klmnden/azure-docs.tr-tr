---
title: "Azure olay kılavuz olay şeması"
description: "Azure olay kılavuz olan olaylar için sağlanan özellikler açıklar"
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/07/2017
ms.author: babanisa
ms.openlocfilehash: caa709fdc2a59472ee812bde91f7300396aa5755
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema"></a>Azure olay kılavuz olay şeması

Bu makalede, tüm olaylar için mevcut şema ve özellikler açıklanmaktadır. Olayları beş gerekli dize özellikleri ve gerekli veri nesnesi kümesinden oluşur. Tüm olayları için herhangi bir yayımcıdan yaygın özelliklerdir. Veri nesnesi her yayımcı için özel özellikleri içerir. Sistem konuları için bu özellikler Azure Storage veya Azure Event Hubs gibi kaynak sağlayıcısı özgüdür.

Olayları Azure olay kılavuza birden çok olay nesneleri içeren bir dizide gönderilir. Yalnızca tek bir olay ise, dizi 1 uzunluğuna sahip. Dizi toplam boyutu en fazla 1 MB olabilir. Her olay dizisindeki 64 KB ile sınırlıdır.

## <a name="event-schema"></a>Olay şeması

Aşağıdaki örnek, tüm olay yayımcıları tarafından kullanılan özellikleri gösterir:

```json
[
  {
    "topic": string,
    "subject": string,    
    "id": string,
    "eventType": string,
    "eventTime": string,
    "data":{
      object-unique-to-each-publisher
    }
  }
]
```

Örneğin, bir Azure Blob Depolama olayı yayımlanan şema şöyledir:

```json
[
  {
    "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
    "subject": "/blobServices/default/containers/oc2d2817345i200097container/blobs/oc2d2817345i20002296blob",
    "eventType": "Microsoft.Storage.BlobCreated",
    "eventTime": "2017-06-26T18:41:00.9584103Z",
    "id": "831e1650-001e-001b-66ab-eeb76e069631",
    "data": {
      "api": "PutBlockList",
      "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
      "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
      "eTag": "0x8D4BCC2E4835CD0",
      "contentType": "application/octet-stream",
      "contentLength": 524288,
      "blobType": "BlockBlob",
      "url": "https://oc2d2817345i60006.blob.core.windows.net/oc2d2817345i200097container/oc2d2817345i20002296blob",
      "sequencer": "00000000000004420000000000028963",
      "storageDiagnostics": {
        "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
      }
    }
  }
]
```
 
## <a name="event-properties"></a>Olay Özellikleri

Tüm olaylar aynı aşağıdaki üst düzey veri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | Dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. |
| Konu | Dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| Olay türü | Dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | Dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | Dize | Olay için benzersiz tanımlayıcı. |
| Veri | Nesne | Olay verileri kaynak sağlayıcıya özel. |

Veri nesnesi özellikleri hakkında bilgi edinmek için olay kaynağı bakın:

* [Azure abonelikleri (yönetim işlemlerini)](event-schema-subscriptions.md)
* [Blob depolama](event-schema-blob-storage.md)
* [Olay hub'ları](event-schema-event-hubs.md)
* [Kaynak grupları (yönetim işlemlerini)](event-schema-resource-groups.md)

Özel konular için olay yayımcısı veri nesnesi belirler. Üst düzey veri standart kaynak tarafından tanımlanan olayları aynı alanları içermelidir. Olaylar için özel konular yayımlarken, Yönlendirme ve filtreleme yardımcı olmak üzere, olayların konu modelleme düşünmelisiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
