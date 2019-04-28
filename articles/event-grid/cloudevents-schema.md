---
title: CloudEvents şeması olayları ile Azure Event grid'i kullanın
description: CloudEvents şeması olayları Azure Event Grid ayarlama işlemi açıklanmaktadır.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: babanisa
ms.openlocfilehash: 0195ce82396a7b05335242a38a2881e1b2d1afb3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61436626"
---
# <a name="use-cloudevents-schema-with-event-grid"></a>Event Grid ile CloudEvents şeması kullanma

Ek olarak kendi [varsayılan olay şeması](event-schema.md), Azure Event Grid, olayları yerel olarak destekleyen [CloudEvents JSON şeması](https://github.com/cloudevents/spec/blob/master/json-format.md). [CloudEvents](https://cloudevents.io/) olduğu bir [açın belirtimi](https://github.com/cloudevents/spec/blob/master/spec.md) açıklayan olay verileri.

Yayımlama için ortak bir olay şema sağlayarak birlikte çalışabilirlik CloudEvents basitleştirir ve bulut kullanan olaylara bağlı. Bu şema Tekdüzen araçları, standart yolu yönlendirme & olayları işleme ve dış olay şeması seri durumdan çıkarılırken Evrensel yollarını sağlar. Ortak bir şema ile platformlar arasında iş daha kolayca tümleştirebilirsiniz.

CloudEvents oluşturulan birkaç tarafından [ortak çalışanlar](https://github.com/cloudevents/spec/blob/master/community/contributors.md), Microsoft, dahil olmak üzere [bulut yerel bilgisayar Foundation](https://www.cncf.io/). 0.1 sürüm olarak şu anda kullanılabilir.

Bu makalede, Event Grid ile CloudEvents şeması kullanmayı açıklar.

[!INCLUDE [requires-azurerm](../../includes/requires-azurerm.md)]

## <a name="install-preview-feature"></a>Önizleme özelliğini yükle

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
| olay türü          | String   | "com.example.someevent"          | Gerçekleşen, oluşumunu türü                                   | olay türü
| eventTypeVersion   | String   | "1.0"                            | (İsteğe bağlı) eventType sürümü                            | dataVersion
| cloudEventsVersion | String   | "0.1"                            | Olay kullanır CloudEvents belirtim sürümü        | *doğrudan geçiş*
| source             | URI      | "/ mycontext"                     | Olay üretici açıklar                                       | Konu #subject
| EventID            | String   | "1234-1234-1234"                 | Etkinliğin kimliği                                                    | id
| eventTime          | Zaman damgası| "2018-04-05T17:31:00Z"           | Olay (isteğe bağlı) ne zaman oluştuğunu, zaman damgası                    | eventTime
| schemaURL          | URI      | "https:\//myschema.com"           | Veri özniteliği uyar (isteğe bağlı) şemayı Bağla | *kullanılmıyor*
| contentType        | String   | "application/json"               | Veri kodlama biçimi (isteğe bağlı) açıklayın                       | *kullanılmıyor*
| Uzantıları         | Eşleme      | {"yapıştırmadan": "vA", "extB", "vB"}  | Ek meta verileri (isteğe bağlı)                                 | *kullanılmıyor*
| veriler               | Object   | {"objA": "vA", "objB", "vB"}  | Olay Yükü (isteğe bağlı)                                       | veriler

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

Özel bir konu oluşturduğunuzda, bir özel konu için giriş şemasını ayarlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
# If you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  --name <topic_name> \
  -l westcentralus \
  -g gridResourceGroup \
  --input-schema cloudeventv01schema
```

PowerShell için şunu kullanın:

```azurepowershell-interactive
# If you have not already installed the module, do it now.
# This module is required for preview features.
Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery

New-AzureRmEventGridTopic `
  -ResourceGroupName gridResourceGroup `
  -Location westcentralus `
  -Name <topic_name> `
  -InputSchema CloudEventV01Schema
```

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. Bir konuya CloudEvent şema ile olayları yayımlamak için ayrı ayrı her bir olay yayımlayın.

### <a name="output-schema"></a>Çıkış şeması

Olay aboneliği oluşturduğunuzda, çıkış şeması ayarlayın.

Azure CLI için şunu kullanın:

```azurecli-interactive
topicID=$(az eventgrid topic show --name <topic-name> -g gridResourceGroup --query id --output tsv)

az eventgrid event-subscription create \
  --name <event_subscription_name> \
  --source-resource-id $topicID \
  --endpoint <endpoint_URL> \
  --event-delivery-schema cloudeventv01schema
```

PowerShell için şunu kullanın:
```azurepowershell-interactive
$topicid = (Get-AzureRmEventGridTopic -ResourceGroupName gridResourceGroup -Name <topic-name>).Id

New-AzureRmEventGridSubscription `
  -ResourceId $topicid `
  -EventSubscriptionName <event_subscription_name> `
  -Endpoint <endpoint_URL> `
  -DeliverySchema CloudEventV01Schema
```

Olayların toplu işleme CloudEvents geçerli sürümünü desteklemiyor. CloudEvent şema için yapılandırılmış bir olay aboneliği ayrı ayrı her bir olay alır. Şu anda CloudEvents şeması olay iletildiğinde bir Azure işlev uygulaması için bir olay Kılavuzu tetikleyicisi kullanamazsınız. HTTP tetikleyicisi kullanın. CloudEvents şeması içinde olaylarını alır bir HTTP tetikleyici uygulama örnekleri için bkz [HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma](../azure-functions/functions-bindings-event-grid.md#use-an-http-trigger-as-an-event-grid-trigger).

## <a name="next-steps"></a>Sonraki adımlar

* Olay teslimat izleme hakkında daha fazla bilgi için bkz: [İzleyici Event Grid iletiyi teslim](monitor-event-delivery.md).
* Öneririz, test etmenize, açıklama ve [katkıda](https://github.com/cloudevents/spec/blob/master/CONTRIBUTING.md) CloudEvents için.
* Azure Event Grid aboneliği oluşturma hakkında daha fazla bilgi için bkz. [Event Grid aboneliği şema](subscription-creation-schema.md).
