---
title: CloudEvents şeması olayları ile Azure Event grid'i kullanın
description: CloudEvents şeması olayları Azure Event Grid ayarlama işlemi açıklanmaktadır.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 07/13/2018
ms.author: babanisa
ms.openlocfilehash: 4f1f0e95ae74ef41ed91be55f4c964671e8f723b
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39044558"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Event Grid ile CloudEvents şeması kullanma

Ek olarak kendi [varsayılan olay şeması](event-schema.md), Azure Event Grid, olayları yerel olarak destekleyen [CloudEvents JSON şeması](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](http://cloudevents.io/) olduğu bir [standart bir belirtim açın](https://github.com/cloudevents/spec/blob/master/spec.md) olay verilerini yaygın bir şekilde tanımlamak için.

Yayımlama için ortak bir olay şema sağlayarak birlikte çalışabilirlik CloudEvents basitleştirir ve bulut kullanan olaylara bağlı. Bu şema Tekdüzen araçları, standart yolu yönlendirme & olayları işleme ve dış olay şeması seri durumdan çıkarılırken Evrensel yollarını sağlar. Ortak bir şema ile platformlar arasında iş daha kolayca tümleştirebilirsiniz.

CloudEvents birkaç yapıdan okunuyor [ortak çalışanlar](https://github.com/cloudevents/spec/blob/master/community/contributors.md), Microsoft, dahil olmak üzere [bulut yerel işlem Foundation](https://www.cncf.io/). 0.1 sürüm olarak şu anda kullanılabilir.

Bu makalede, Event Grid ile CloudEvents şeması kullanmayı açıklar.

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="cloudevent-schema"></a>CloudEvent şeması

Bir Azure Blob Depolama olayı CloudEvents biçiminde bir örnek aşağıda verilmiştir:

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

CloudEvents v0.1, aşağıdaki özelliklere sahiptir:

| CloudEvents        | Tür     | Örnek JSON değeri             | Açıklama                                                        | Event Grid eşleme
|--------------------|----------|--------------------------------|--------------------------------------------------------------------|-------------------------
| olay türü          | Dize   | "com.example.someevent"          | Gerçekleşen, oluşumunu türü                                   | olay türü
| eventTypeVersion   | Dize   | "1.0"                            | (İsteğe bağlı) eventType sürümü                            | dataVersion
| cloudEventsVersion | Dize   | "0.1"                            | Olay kullanır CloudEvents belirtim sürümü        | *doğrudan geçiş*
| source             | URI      | "/ mycontext"                     | Olay üretici açıklar                                       | Konu #subject
| EventID            | Dize   | "1234-1234-1234"                 | Etkinliğin kimliği                                                    | id
| eventTime          | Zaman damgası| "2018-04-05T17:31:00Z"           | Olay (isteğe bağlı) ne zaman oluştuğunu, zaman damgası                    | eventTime
| schemaURL          | URI      | "https://myschema.com"           | Veri özniteliği uyar (isteğe bağlı) şemayı Bağla | *kullanılmıyor*
| contentType        | Dize   | "application/json"               | Veri kodlama biçimi (isteğe bağlı) açıklayın                       | *kullanılmıyor*
| Uzantıları         | Eşleme      | {"yapıştırmadan": "vA", "extB", "vB"}  | Ek meta verileri (isteğe bağlı)                                 | *kullanılmıyor*
| veri               | Nesne   | {"objA": "vA", "objB", "vB"}  | Olay Yükü (isteğe bağlı)                                       | veri

Daha fazla bilgi için [CloudEvents spec](https://github.com/cloudevents/spec/blob/master/spec.md#context-attributes).

CloudEvents şeması ve Event Grid şema teslim edilen olaylara üstbilgi değerlerini dışında aynıdır `content-type`. CloudEvents şeması için bu üst bilgi değeri: `"content-type":"application/cloudevents+json; charset=utf-8"`. Event Grid şeması için bu üst bilgi değeri: `"content-type":"application/json; charset=utf-8"`.

## <a name="configure-event-grid-for-cloudevents"></a>Event Grid CloudEvents için yapılandırma

Event Grid, hem giriş hem de çıkış olayların CloudEvents şeması için kullanabilirsiniz. Blob Depolama olaylarını ve IOT Hub olaylarını ve özel olaylar gibi sistem olaylarını CloudEvents kullanabilirsiniz. Ayrıca, bu olaylara kablo ve geriye de dönüştürebilirsiniz.


| Giriş şeması       | Çıkış şeması
|--------------------|---------------------
| CloudEvents biçimi | CloudEvents biçimi
| Event Grid biçimi  | CloudEvents biçimi
| CloudEvents biçimi | Event Grid biçimi
| Event Grid biçimi  | Event Grid biçimi

Tüm olay şemaları için Event Grid için event grid konusu yayımlarken ve bir olay aboneliği oluşturulurken doğrulama gerektirir. Daha fazla bilgi için [Event Grid güvenliğini ve kimlik doğrulaması](security-authentication.md).

### <a name="input-schema"></a>Giriş şeması

Özel Konunuzu oluşturduğunuzda giriş şemasını CloudEvents için özel bir konuya ayarlamak için aşağıdaki parametreyi Azure CLI'yi kullanın `--input-schema cloudeventv01schema`. Özel konuya gelen olayları CloudEvents v0.1 biçiminde artık bekliyor.

Event grid konusu oluşturmak için kullanın:

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

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. Bir konuya CloudEvent şema ile olayları yayımlamak için ayrı ayrı her bir olay yayımlayın.

### <a name="output-schema"></a>Çıkış şeması

Olay aboneliği oluşturduğunuzda, çıkış şeması CloudEvents olay aboneliği ayarlamak için aşağıdaki parametreyi Azure CLI'yi kullanın `--event-delivery-schema cloudeventv01schema`. Bu olay aboneliğine olayları artık olması teslim edilir CloudEvents v0.1 biçiminde.

Bir olay aboneliği oluşturmak için kullanın:

```azurecli
az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --topic-name <topic_name> \
  -g gridResourceGroup \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. CloudEvent şema için yapılandırılmış bir olay aboneliği ayrı ayrı her bir olay alır. Şu anda CloudEvents şeması olay iletildiğinde bir Azure işlev uygulaması için bir olay Kılavuzu tetikleyicisi kullanamazsınız. HTTP tetikleyicisi kullanmanız gerekir. CloudEvents şeması içinde olaylarını alır bir HTTP tetikleyici uygulama örnekleri için bkz [HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimat izleme hakkında daha fazla bilgi için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Öneririz, test etmenize, açıklama ve [katkıda](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) CloudEvents için.
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
