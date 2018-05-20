---
title: Azure olay kılavuz olay şeması
description: Azure olay kılavuz olan olaylar için sağlanan özellikler açıklar
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 04/17/2018
ms.author: babanisa
ms.openlocfilehash: 3e0b7fd825b8e985cea2c32301986b3a7f8bb619
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-event-grid-event-schema"></a>Azure olay kılavuz olay şeması

Bu makalede, tüm olaylar için mevcut şema ve özellikler açıklanmaktadır. Olayları beş gerekli dize özellikleri ve gerekli veri nesnesi kümesinden oluşur. Tüm olayları için herhangi bir yayımcıdan yaygın özelliklerdir. Veri nesnesi her yayımcı için özel özellikleri içerir. Sistem konuları için bu özellikler Azure Storage veya Azure Event Hubs gibi kaynak sağlayıcısı özgüdür.

Olay kaynakları olayları Azure olay kılavuza birden çok olay nesneleri içeren bir dizide gönderin. Bir olay kılavuz konusu olaylarına nakil sırasında dizi toplam boyutu en fazla 1 MB olabilir. Her olay dizisindeki 64 KB ile sınırlıdır. Bir olay ya da dizi boyutu sınırları aşarsa, yanıtı alırsınız **413 yükü çok büyük**.

Olay kılavuz, tek bir olay içeren bir dizi aboneler için olaylar gönderir. Bu davranış gelecekte değişebilir.

Olay kılavuz olay ve her Azure yayımcının veri yükü için JSON şeması bulabilirsiniz [olay şema deposuna](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

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
    },
    "dataVersion": string,
    "metadataVersion": string
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
    },
    "dataVersion": "",
    "metadataVersion": "1"
  }
]
```

## <a name="event-properties"></a>Olay Özellikleri

Tüm olaylar aynı aşağıdaki üst düzey veri içerir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| Konu | dize | Olay kaynağı tam kaynak yolu. Bu alan yazılabilir değil. Bu değer olay kılavuz sağlar. |
| Konu | dize | Olay konu yayımcı tarafından tanımlanan yolu. |
| Olay türü | dize | Bu olay kaynağı için kayıtlı olay türünden biri. |
| EventTime | dize | Olayı oluşturan zaman sağlayıcının UTC zamanı temel alınarak. |
| id | dize | Olay için benzersiz tanımlayıcı. |
| veriler | object | Olay verileri kaynak sağlayıcıya özel. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Olay kılavuz, şemanın en üst düzey özellikleri tanımlar. Bu değer olay kılavuz sağlar. |

Veri nesnesi özellikleri hakkında bilgi edinmek için olay kaynağı bakın:

* [Azure abonelikleri (yönetim işlemlerini)](event-schema-subscriptions.md)
* [Blob depolama](event-schema-blob-storage.md)
* [Event Hubs](event-schema-event-hubs.md)
* [Service Bus](event-schema-service-bus.md)
* [IoT Hub’ı](event-schema-iot-hub.md)
* [Kaynak grupları (yönetim işlemlerini)](event-schema-resource-groups.md)

Özel konular için olay yayımcısı veri nesnesi belirler. Üst düzey veri standart kaynak tarafından tanımlanan olayları aynı alanları içermelidir.

Olaylar için özel konular yayımlarken konularla ilgili olayda ilgilenen olup olmadıklarını öğrenmek aboneleri için kolaylaştıran olaylarınızı oluşturun. Aboneler konu filtre ve rota olaylar için kullanın. Böylece aboneleri yol kesimleri göre filtre uygulayabilirsiniz olay yapıldığı için yolun belirtmeyi deneyin. Yolun dar veya geniş çapta olayları filtrelemek aboneleri sağlar. Örneğin, bir üç segment yolu gibi sağlarsanız, `/A/B/C` Bu konu, aboneler ilk segmente göre filtreleyebilirsiniz `/A` olayların geniş kapsamlı bir kümesini almak için. Bu aboneleri konularıyla gibi olayları Al `/A/B/C` veya `/A/D/E`. Diğer aboneleri göre filtre uygulayabilirsiniz `/A/B` olayları daha dar bir dizi alınamıyor.

Bazen Konunuzu ne hakkında daha fazla ayrıntı gerekir. Örneğin, **depolama hesapları** publisher sağlar konu `/blobServices/default/containers/<container-name>/blobs/<file>` bir dosya için bir kapsayıcı eklendiğinde. Bir abonenin yoluyla filtre uygulayabilirsiniz `/blobServices/default/containers/testcontainer` bu kapsayıcı, ancak diğer kapsayıcılar değil depolama hesabındaki tüm olayları alınamıyor. Bir abonenin de filtre uygulayabilirsiniz veya rota soneki tarafından `.txt` yalnızca metin dosyalarıyla çalışmak üzere.

## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
