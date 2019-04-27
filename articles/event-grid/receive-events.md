---
title: Olayları Azure Event Grid'den gelen bir HTTP uç noktasına alma
description: Bir HTTP uç noktasını doğrulamak sonra alır ve Azure Event grid'den olayların seri durumdan açıklar
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: conceptual
ms.date: 01/01/2019
ms.author: babanisa
ms.openlocfilehash: cb38fd17c0c1bfbe3e5957d8f432f0a43b285c93
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60803767"
---
# <a name="receive-events-to-an-http-endpoint"></a>HTTP uç noktasına olayları alma

Bu makalede nasıl [bir HTTP uç noktasını doğrulamak](security-authentication.md#webhook-event-delivery) olayları bir Event abonelikten almak almak ve olayları seri durumdan çıkarmak için. Bu makalede, uygulamanın barındırıldığı bağımsız olarak aynı kavramlar geçerlidir ancak tanıtım amacıyla bir Azure işlevi kullanır.

> [!NOTE]
> Bu **kesin** kullanmanız önerilir bir [olay Kılavuzu tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) Event Grid ile bir Azure işlevi tetiklerken. Burada Genel Web kancası tetikleyici kullanımını gösterim amaçlıdır.

## <a name="prerequisites"></a>Önkoşullar

Bir işlev uygulaması ile bir HTTP ile tetiklenen işlev ihtiyacınız vardır.

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin

.NET içinde geliştiriyorsanız [bağımlılık ekleme](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) için işlevinize `Microsoft.Azure.EventGrid` [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.EventGrid). Bu makaledeki örneklerde sürümü 1.4.0 gerekir veya üzeri.

Diğer dillerine yönelik SDK'lar aracılığıyla kullanılabilen [yayımlama SDK'ları](./sdk-overview.md#data-plane-sdks) başvuru. Bu paketleri gibi yerel olay türü için modelleri sahip `EventGridEvent`, `StorageBlobCreatedEventData`, ve `EventHubCaptureFileCreatedEventData`.

Azure işlevinizde (Azure işlevleri portalındaki doğru çoğu bölmesinde) "Dosyaları Görüntüle" bağlantısına tıklayın ve project.json adlı bir dosya oluşturun. Aşağıdaki içeriği ekleyin `project.json` dosya ve kaydedin:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "2.0.0"
      }
    }
   }
}
```

![Eklenen NuGet paketi](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Uç nokta doğrulaması

Yapmak isteyeceğiniz ilk şey gerçekleştirilir `Microsoft.EventGrid.SubscriptionValidationEvent` olayları. Her zaman biri bir olaya abone olur, Event Grid doğrulama olayı ile uç noktasına gönderir. bir `validationCode` veri yükünde. Bu geri için yanıt gövdesindeki echo için uç nokta gereklidir [uç noktası, geçerli ve size ait olduğunu kanıtlamak](security-authentication.md#webhook-event-delivery). Kullanıyorsanız, bir [olay Kılavuzu tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) bir Web kancası ile tetiklenen işlev yerine, uç nokta doğrulaması sizin yerinize gerçekleştirilir. Bir üçüncü taraf API hizmetini kullanıyorsanız (gibi [Zapier](https://zapier.com) veya [IFTTT](https://ifttt.com/)), programlama yoluyla bir doğrulama kodu echo mümkün olmayabilir. Bu hizmetler için abonelik gönderilen abonelik doğrulama olayı doğrulama URL'yi kullanarak el ile doğrulayabilirsiniz. Bu URL'yi kopyalayın `validationUrl` özelliği ve gönderme bir GET isteği, bir REST istemcisi ya da web tarayıcınız aracılığıyla.

C# ' ta, `DeserializeEventGridEvents()` işlevi, Event Grid olayları seri durumdan çıkarır. Olay verileri StorageBlobCreatedEventData gibi uygun bir türle içine çıkarır. Kullanım `Microsoft.Azure.EventGrid.EventTypes` desteklenen olay türleri ve adları almak için sınıf.

Programlama yoluyla bir doğrulama kodu echo için aşağıdaki kodu kullanın. En ilgili örnekleri bulabilirsiniz [Event Grid tüketici örneği](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridConsumer).

```csharp
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
};
```

### <a name="test-validation-response"></a>Doğrulama yanıtı test etme

Test alanı işlevi için örnek olay yapıştırarak doğrulama yanıt işlevi test:

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

Çalıştır'a tıklayın, çıkış 200 Tamam olmalıdır ve `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` gövdesinde:

![doğrulama yanıtı](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>BLOB Depolama olaylarını işleme

Şimdi, şimdi işlemek için işlev genişletme `Microsoft.Storage.BlobCreated`:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};

```

### <a name="test-blob-created-event-handling"></a>BLOB oluşturulan olay işlemeyi test edin

Koyarak işlevin yeni işlevleri test bir [Blob Depolama olayı](./event-schema-blob-storage.md#example-event) test alan ve çalıştırma:

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
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Bir blob URL çıktı işlevi günlüğünde görmeniz gerekir:

![Çıkış günlüğü](./media/receive-events/blob-event-response.png)

Blob Depolama hesabı veya genel amaçlı V2 oluşturarak ayrıca test edebilirsiniz (GPv2) depolama hesabı [ekleme ve olay aboneliği](../storage/blobs/storage-blob-event-quickstart.md)ve uç nokta için işlev URL'si:

![İşlev URL'si](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Özel olaylarını işleme

Son olarak, özel olaylar işleyebilmesi işlevini bir kez daha genişletmek olanak tanır. 

C# ' ta SDK olay veri türü için bir olay türü adını eşleme destekler. Kullanım `AddOrUpdateCustomEventMapping()` özel olay eşlemek için işlev.

Olay için bir denetimi Ekle `Contoso.Items.ItemReceived`. Son kod gibi görünmelidir:

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

class ContosoItemReceivedEventData
{
    [JsonProperty(PropertyName = "itemSku")]
    public string ItemSku { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();
    eventGridSubscriber.AddOrUpdateCustomEventMapping("Contoso.Items.ItemReceived", typeof(ContosoItemReceivedEventData));
    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
        else if (eventGridEvent.Data is ContosoItemReceivedEventData)
        {
            var eventData = (ContosoItemReceivedEventData)eventGridEvent.Data;
            log.Info($"Got ContosoItemReceived event data, item SKU {eventData.ItemSku}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = body.data;
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
        }
    }
    context.done();
};
```

### <a name="test-custom-event-handling"></a>Test özel olay işleme

Son olarak, işlevinizi artık, özel olay türünü işleyebilen test edin:

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Contoso.Items.ItemReceived",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "itemSku": "Standard"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

Ayrıca bu işlev tarafından Canlı test [portaldan CURL ile özel bir olay gönderme](./custom-event-quickstart-portal.md) ya da [özel bir konu başlığına ileti gönderme](./post-to-custom-topic.md) herhangi bir hizmet veya gibibiruçnoktasınaGÖNDEREBİLİRuygulamakullanarak[Postman](https://www.getpostman.com/). İşlev URL'si ayarlayın. uç noktası ile özel konu ve bir olay aboneliği oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Keşfedin [Azure Event Grid yönetim ve SDK'ları yayımlama](./sdk-overview.md)
* Bilgi edinmek için nasıl [gönderi için özel bir konu](./post-to-custom-topic.md)
* Event Grid ve işlevleri detaylı eğitimler birini gibi deneyin [Blob depolama alanına yüklenen görüntüleri yeniden boyutlandırma](resize-images-on-storage-blob-upload-event.md)
