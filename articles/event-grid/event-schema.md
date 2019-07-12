---
title: Azure Event Grid olay şeması
description: Olayları Azure Event Grid ile sağlanan özellikleri tanımlar
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 01/20/2019
ms.author: babanisa
ms.openlocfilehash: 8a8193d21bbc1d0af933657705e605ce31589cbf
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67785859"
---
# <a name="azure-event-grid-event-schema"></a>Azure Event Grid olay şeması

Bu makalede, tüm olaylar için mevcut olan şema ve özellikleri açıklar. Olayları beş gereken dize özellikleri ve gerekli veri nesnesi kümesinden oluşur. Tüm olaylara herhangi bir yayımcıdan yaygın özelliklerdir. Veri nesnesi, her yayımcı için özel özellikleri vardır. Sistem konuları için bu özellikleri Azure depolama veya Azure Event Hubs gibi bir kaynak sağlayıcısı özgüdür.

Olay kaynakları, çeşitli olay nesneleri olan bir dizi içinde Azure Event Grid için olayları gönderirsiniz. Event grid konusu olayları nakil sırasında dizinin toplam boyutu 1 MB'a kadar olabilir. Dizideki her olay, 64 KB'lık (Genel kullanım) veya 1 MB (Önizleme) ile sınırlıdır. Bir olay ya da dizi boyutu sınırları büyükse yanıt almanız **413 yükü çok büyük**.

> [!NOTE]
> Bir olay boyutu en fazla 64 KB, genel kullanılabilirlik (GA) hizmet düzeyi sözleşmesi (SLA) tarafından alınmıştır. Bir olay boyutu en fazla desteği 1 MB şu anda Önizleme aşamasındadır. Olaylar üzerinde 64 KB, 64 KB'lık artışlarla ücretlendirilir. 

Event Grid, tek bir olayda bir dizideki abonelere olayları gönderir. Bu davranışı gelecekte değişebilir.

Event Grid olay ve her Azure yayımcının veri yükteki için JSON şemasını bulabilirsiniz [olay şeması depolaması](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/eventgrid/data-plane).

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

Tüm olaylar aynı aşağıdaki üst düzey veri sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| topic | dize | Olay kaynağı tam kaynak yolu. Bu alan, yazılabilir değil. Event Grid, bu değeri sağlar. |
| subject | dize | Yayımcı tarafından tanımlanan olay konu yolu. |
| eventType | dize | Bu olay kaynağı için kayıtlı olay türlerinden biri. |
| eventTime | dize | Olayın oluşturulduğu zamandan, sağlayıcının UTC saatini temel alan. |
| id | dize | Olayın benzersiz tanımlayıcısı. |
| data | object | Kaynak sağlayıcıya özel olay verileri. |
| dataVersion | dize | Veri nesnesinin şema sürümü. Yayımcı, şema sürümü tanımlar. |
| metadataVersion | dize | Olay meta verilerinin şema sürümü. Event Grid, şemanın en üst düzey özellikleri tanımlar. Event Grid, bu değeri sağlar. |

Veri nesnesinin özellikleri hakkında bilgi edinmek için bkz: olay kaynağı:

* [Azure abonelikleri (yönetim işlemleri)](event-schema-subscriptions.md)
* [Container Registry](event-schema-container-registry.md)
* [Blob depolama](event-schema-blob-storage.md)
* [Event Hubs](event-schema-event-hubs.md)
* [IoT Hub’ı](event-schema-iot-hub.md)
* [Media Services](../media-services/latest/media-services-event-schemas.md?toc=%2fazure%2fevent-grid%2ftoc.json)
* [Kaynak grupları (yönetim işlemleri)](event-schema-resource-groups.md)
* [Service Bus](event-schema-service-bus.md)
* [Azure SignalR](event-schema-azure-signalr.md)

Özel konu için olay yayımcısı veri nesnesi belirler. Üst düzey veri kaynağı tarafından tanımlanan standart olayları aynı alanları olması gerekir.

Olayları özel konular yayımlarken konularla ilgili olay ilgilenen olup olmadığını bilmek aboneleri için kolaylaştıran olaylarınızı oluşturun. Aboneleri konu filtresi ve rota olaylar için kullanın. Olayın gerçekleştiği için yolun yol kesimleri tarafından aboneleri süzebilirsiniz sağlamayı göz önüne alın. Aboneler, sayısı azalacağından veya geniş çapta olayları filtrelemek yol sağlar. Örneğin, bir üç segment yolu gibi sağlarsanız `/A/B/C` Bu konu, abonelerin ilk segmente göre filtreleyebilirsiniz `/A` çok sayıda olayları almak için. Bu olaylar gibi konular ile aboneleri `/A/B/C` veya `/A/D/E`. Diğer aboneler tarafından filtreleyebilirsiniz `/A/B` daha dar bir etkinlik kümesi alınamıyor.

Bazı durumlarda, konu ne hakkında daha fazla ayrıntı gerekir. Örneğin, **depolama hesapları** yayımcı sağlar konu `/blobServices/default/containers/<container-name>/blobs/<file>` bir kapsayıcıya bir dosya eklendiğinde. Abone yoluna göre filtrelemeyi `/blobServices/default/containers/testcontainer` bu kapsayıcı, ancak diğer kapsayıcı olmayan depolama hesabındaki tüm olayları almak için. Abone de filtre uygulayabilirsiniz veya rota soneki tarafından `.txt` yalnızca metin dosyaları ile çalışmak için.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Event grid'e giriş için bkz [Event Grid nedir?](overview.md)
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
