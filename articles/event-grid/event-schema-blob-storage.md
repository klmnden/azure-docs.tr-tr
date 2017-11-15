---
title: "Azure olay kılavuz blob depolama olay şeması"
description: "Blob depolama Azure olay kılavuz olaylarla için sağlanan özellikler açıklar"
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 11/08/2017
ms.author: tomfitz
ms.openlocfilehash: bdc64733b75fd809cf0245986aa96370343c1a34
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/11/2017
---
# <a name="azure-event-grid-event-schema-for-blob-storage"></a>Blob Depolama için Azure olay kılavuz olay şeması

Bu makale, blob depolama olaylar için şema ve özellikleri sağlar. Olay şemaları giriş için bkz: [Azure olay kılavuz olay şema](event-schema.md).

## <a name="available-event-types"></a>Kullanılabilir olay türleri

BLOB Depolama aşağıdaki olay türlerini gösterir:

| Olay türü | Açıklama |
| ---------- | ----------- |
| Microsoft.Storage.BlobCreated | Bir blob oluşturulduğunda oluşturulur. |
| Microsoft.Storage.BlobDeleted | Bir blob silindiğinde oluşturuldu. |

## <a name="example-event"></a>Örnek olayı

Aşağıdaki örnek, olay oluşturulmuş bir blob şeması gösterir: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
```

Bir blob silinmiş olayı için şemayı benzer: 

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobDeleted",
  "eventTime": "2017-11-07T20:09:22.5674003Z",
  "id": "4c2359fe-001e-00ba-0e04-58586806d298",
  "data": {
    "api": "DeleteBlob",
    "requestId": "4c2359fe-001e-00ba-0e04-585868000000",
    "contentType": "text/plain",
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "0000000000000281000000000002F5CA",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  }
}]
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
| Veri | Nesne | BLOB Depolama olay verileri. |

Veri nesnesi aşağıdaki özelliklere sahiptir:

| Özellik | Tür | Açıklama |
| -------- | ---- | ----------- |
| api | Dize | Olayı tetikleyen işlemi. |
| ClientRequestId | Dize | Bir istemci tarafından oluşturulan, donuk değerle bir 1 KB karakter sınırı. Depolama çözümlemeleri günlük kaydı etkinleştirildiğinde, analytics günlüklerine kaydedilir. |
| RequestId | Dize | İstek için benzersiz tanımlayıcı. İstek sorun giderme için kullanın. |
| ETag | Dize | Koşullu işlemleri gerçekleştirmek için kullanabileceğiniz değeri. |
| ContentType | Dize | İçerik türü için blob belirtildi. |
| contentLength | tamsayı | Blob bayt cinsinden boyutu. |
| BlobType | Dize | Blob türü. |
| URL | Dize | Blob yolu. |
| Sıralayıcı | Dize | İstekleri izlemek için kullanabileceğiniz bir kullanıcı tarafından denetlenen bir değer. |
| storageDiagnostics | Nesne | Depolama Tanılama hakkında bilgi sağlar. |
 
## <a name="next-steps"></a>Sonraki adımlar

* Azure olay kılavuz giriş için bkz: [olay kılavuz nedir?](overview.md)
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
* Blob depolama olaylarla çalışma giriş için bkz: [rota Blob Depolama olayları - Azure CLI](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json). 
