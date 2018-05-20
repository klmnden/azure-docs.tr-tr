---
title: CloudEvents şemasında olaylarla Azure olay kılavuzunu kullanın
description: Azure olay kılavuzunda CloudEvents şema olayları için ayarlamak üzere açıklar.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: babanisa
ms.openlocfilehash: 23187fbc230e384984085d330bfbfbc90cc9f945
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Olay kılavuzla CloudEvents şeması kullanma

Ek olarak kendi [varsayılan olay şema](event-schema.md), Azure olay kılavuz yerel olarak destekleyen olaylarda [CloudEvents JSON şeması](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) olan bir [standart belirtimi açmak](https://github.com/cloudevents/spec/blob/master/spec.md) olay verilerini yaygın bir şekilde tanımlamak için.

Yayımlama için ortak bir olay şema sağlayarak birlikte çalışabilirlik CloudEvents basitleştirir ve bulut kullanan tabanlı olaylar. Bu şemayı Tekdüzen araçları, yönlendirme & olayları işleme standart yolları ve dış olay şema seri durumdan Evrensel yolu için izin verir. Ortak bir şema ile iş platformları arasında daha kolay tümleştirebilirsiniz.

CloudEvents yapı birkaç tarafından olan [ortak](https://github.com/cloudevents/spec/blob/master/community/contributors.md), Microsoft, aracılığıyla dahil [bulut yerel işlem Foundation](https://www.cncf.io/). Sürüm 0.1 şu anda kullanılabilir değil.

Bu makalede, olay kılavuzla CloudEvents şema kullanmayı açıklar.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>CloudEvent şeması

Bir Azure Blob Storage olay CloudEvents biçiminde bir örneği burada verilmiştir:

``` JSON
{
    "cloudEventsVersion" : "0.1",
    "eventType" : "Microsoft.Storage.BlobCreated",
    "eventTypeVersion" : "",
    "source" : "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}#blobServices/default/containers/{storage-container}/blobs/{new-file}",
    "eventID" : "173d9985-401e-0075-2497-de268c06ff25",
    "eventTime" : "2018-04-28T02:18:47.1281675Z",
    "data" : {
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
```

CloudEvents v0.1 kullanılabilir aşağıdaki özelliklere sahiptir:

| CloudEvents        | Tür     | Örnek JSON değer             | Açıklama                                                        | Olay kılavuz eşleme
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| Olay türü          | Dize   | "com.example.someevent"          | Oldu oluşumu türü                                   | Olay türü
| eventTypeVersion   | Dize   | "1.0"                            | (İsteğe bağlı) eventType sürümü                            | dataVersion
| cloudEventsVersion | Dize   | "0,1"                            | Olay kullanan CloudEvents belirtiminin sürümü        | *Geçirilecek*
| source             | URI      | "/ mycontext"                     | Olay üretici açıklar                                       | Konu #subject
| Olay Kimliği            | Dize   | "1234-1234-1234"                 | Olay Kimliği                                                    | id
| EventTime          | Zaman damgası| "2018-04-05T17:31:00Z"           | (İsteğe bağlı) olay zaman olmadan, zaman damgası                    | EventTime
| schemaURL          | URI      | "https://myschema.com"           | (İsteğe bağlı) veri özniteliği aynılarını şema Bağla | *kullanılmıyor*
| ContentType        | Dize   | "application/json"               | Veri kodlama biçimi (isteğe bağlı) açıklanır                       | *kullanılmıyor*
| Uzantıları         | Eşleme      | {"extA": "vA", "extB", "vB"}  | Tüm ek meta veriler (isteğe bağlı)                                 | *kullanılmıyor*
| veriler               | Nesne   | {"objA": "vA", "objB", "vB"}  | Olay Yükü (isteğe bağlı)                                       | veriler

Daha fazla bilgi için bkz: [CloudEvents spec](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

## <a name="configure-event-grid-for-cloudevents"></a>Olay kılavuz CloudEvents için yapılandırma

Şu anda Azure olay kılavuz desteklemek için CloudEvents JSON biçimi giriş ve çıkış içinde önizleme sahip **Batı Orta ABD**, **Orta ABD**, ve **Kuzey Avrupa**.

Giriş ve çıkış CloudEvents şemasında olayların olay kılavuzunu kullanabilirsiniz. Blob Storage olayları ve IOT hub'ı olayları ve özel olaylar gibi sistem olayları için CloudEvents kullanabilirsiniz. Ayrıca, olaylar ve geriye hattaki de dönüştürebilirsiniz.


| Giriş şemasını       | Çıkış şeması
|--------------------|---------------------
| CloudEvents biçimi | CloudEvents biçimi
| Olay kılavuz biçimi  | CloudEvents biçimi
| CloudEvents biçimi | Olay kılavuz biçimi
| Olay kılavuz biçimi  | Olay kılavuz biçimi

Tüm olay şemaları için bir olay kılavuz konusu yayımlarken ve bir olay abonelik oluştururken doğrulama olay kılavuz gerektirir. Daha fazla bilgi için bkz: [olay kılavuz güvenlik ve kimlik doğrulama](security-authentication.md).

### <a name="input-schema"></a>Giriş şemasını

Konunuzu oluşturduğunuzda CloudEvents özel bir konuya giriş şemasını ayarlamak için aşağıdaki parametresini Azure CLI kullanın `--input-schema cloudeventv01schema`. Özel Konu gelen olayları CloudEvents v0.1 biçiminde şimdi bekliyor.

Bir olay kılavuz konusu oluşturmak için kullanın:

```azurecli
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. Bir konuya CloudEvent şemasıyla olayları yayımlamak için her olay ayrı ayrı yayımlayın.

### <a name="output-schema"></a>Çıkış şeması

Olay aboneliğinizi oluşturduğunuzda, çıkış şeması CloudEvents için bir olay aboneliği ayarlamak için aşağıdaki parametresini Azure CLI kullanın `--event-delivery-schema cloudeventv01schema`. Bu olay aboneliği için olayları artık olması teslim edilir CloudEvents v0.1 biçiminde.

Bir olay aboneliği oluşturmak için kullanın:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. CloudEvent şeması için yapılandırılmış bir olay aboneliği ayrı ayrı her olay alır.

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimler izleme hakkında daha fazla bilgi için bkz: [İzleyicisi olay kılavuz ileti teslimi](monitor-event-delivery.md).
* Test, yorum öneririz ve [katkıda](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) CloudEvents için.
* Bir Azure olay kılavuz abonelik oluşturma hakkında daha fazla bilgi için bkz: [olay kılavuz abonelik şema](subscription-creation-schema.md).
