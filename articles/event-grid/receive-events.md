---
title: "Bir HTTP uç noktası için Azure olay kılavuzdan olayları alma"
description: "Bir HTTP uç noktası doğrulamak sonra alma ve Azure olay kılavuz olaylarından seri durumdan açıklar"
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: article
ms.date: 02/16/2018
ms.author: babanisa
ms.openlocfilehash: 179f7c46186762eed2f7f8ac90620ac2fec9caf3
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="receive-events-to-an-http-endpoint"></a>HTTP uç noktasına olayları alma

Bu makalede nasıl [bir HTTP uç noktası doğrulamak](security-authentication.md#webhook-event-delivery) olayları olay abonelikten almak almak ve olayları serisini kaldırmak için. Uygulama barındırıldığı bağımsız olarak aynı kavramlar uygulanır ancak bu makale bir Azure işlevi tanıtım amacıyla kullanır.

> [!NOTE]
> Bu **kesinlikle** kullanmanız önerilir bir [olay kılavuz tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) olay kılavuz Azure işleviyle tetiklendiğinde. Genel Web kancası tetikleyici burada demonstrative kullanılır.

## <a name="prerequisites"></a>Önkoşullar

* Bir işlev uygulaması ile ihtiyacınız olacak bir [HTTP tetiklenen işlevi](../azure-functions/functions-create-generic-webhook-triggered-function.md)

## <a name="add-dependencies"></a>Bağımlılıkları ekleyin.

.Net ile geliştiriyorsanız [bir bağımlılık ekleme](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies) işlevi için için `Microsoft.Azure.EventGrid` [Nuget paketi](https://www.nuget.org/packages/Microsoft.Azure.EventGrid). Diğer dillere yönelik SDK aracılığıyla kullanılabilen [yayımlama SDK'ları](./sdk-overview.md#publish-sdks) başvuru. Bu paketleri gibi yerel olay türleri modellerini içeren `EventGridEvent`, `StorageBlobCreatedEventData`, ve `EventHubCaptureFileCreatedEventData`.

Bunu yapmak için Azure işlevinizi (sağ çoğu Azure işlevleri portalındaki bölmesinde) yer alan "Dosyaları Görüntüle" bağlantısını tıklatın ve project.json adlı bir dosya oluşturun. Aşağıdaki içerikleri ekleyin `project.json` dosya ve kaydedin:

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "1.1.0-preview"
      }
    }
   }
}

```

![Eklenen Nuget paketi](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>Bitiş noktası doğrulama

Tanıtıcı istiyoruz yapmak için ilk şey. `Microsoft.EventGrid.SubscriptionValidationEvent` olaylar. Yeni bir olay aboneliği her oluşturulduğunda olay kılavuz uç noktası için bir doğrulama olay gönderir bir `validationCode` veri yükte. Bu geri yanıt gövdesi içinde Yankı için uç nokta gereklidir [uç nokta geçerli olduğundan ve sizin tarafınızdan ait kanıtlamak](security-authentication.md#webhook-event-delivery). Kullanıyorsanız bir [olay kılavuz tetikleyicisi](../azure-functions/functions-bindings-event-grid.md) işlevi bir Web kancası tetiklenen yerine bitiş noktası doğrulama sizin için işlenir.

Abonelik doğrulama işlemek için aşağıdaki kodu kullanabilirsiniz:

```csharp

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{

    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript

var http = require('http');

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

### <a name="test-validation-response"></a>Test doğrulama yanıtı

İşlevi test alanına örnek olayı yapıştırılarak doğrulama yanıt işlevi test edin:

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

Çalıştır'ı tıklattığınızda çıktısı 200 Tamam olmalıdır ve `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}` gövdesinde:

![doğrulama yanıtı](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>BLOB Depolama olayları işlemek

Biz şimdi işlemek için işlev genişletebilirsiniz `Microsoft.Storage.BlobCreated`:

```cs

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type 
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");

            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript

var http = require('http');

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

### <a name="test-blob-created-event-handling"></a>Test Blob oluşturulan olay işleme

Koyarak işlevi yeni işlevselliğini sınamak bir [Blob Depolama olay](./event-schema-blob-storage.md#example-event) çalıştırma ve test alanı içine:

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

İşlevi günlüğünde blob URL'si çıktı görmeniz gerekir:

![Çıkış günlüğü](./media/receive-events/blob-event-response.png)

Ayrıca, bir Blob storage hesabı veya genel amaçlı V2 oluşturarak bu giden Canlı sınayabilirsiniz (GPv2) depolama hesabı [ekleme ve olay aboneliği](../storage/blobs/storage-blob-event-quickstart.md)ve uç nokta işlevi URL olarak ayarlayarak:

![İşlev URL'si](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>Özel olayları işlemek

Son olarak, böylece özel olaylar işleyebilir işlevini kez daha genişletmek olanak sağlar. Kendi olayı için bir onay eklediğimiz `Contoso.Items.ItemReceived`. Son kodunuzu aşağıdaki gibi görünmelidir:

```cs

using System.Net;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Microsoft.Azure.EventGrid.Models;

class SubscriptionValidationEventData
{
    public string ValidationCode { get; set; }
}

class SubscriptionValidationResponseData
{
    public string ValidationResponse { get; set; }
}

class ContosoItemReceivedEventData
{
    public string id { get; set; }
    public string message { get; set; }
    public string time { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;
    const string SubscriptionValidationEvent = "Microsoft.EventGrid.SubscriptionValidationEvent";
    const string StorageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    const string CustomTopicEvent = "Contoso.Items.ItemReceived";

    string requestContent = await req.Content.ReadAsStringAsync();
    EventGridEvent[] eventGridEvents = JsonConvert.DeserializeObject<EventGridEvent[]>(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        JObject dataObject = eventGridEvent.Data as JObject;

        // Deserialize the event data into the appropriate type based on event type
        if (string.Equals(eventGridEvent.EventType, SubscriptionValidationEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<SubscriptionValidationEventData>();
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response
            var responseData = new SubscriptionValidationResponseData();
            responseData.ValidationResponse = eventData.ValidationCode;
            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }

        else if (string.Equals(eventGridEvent.EventType, StorageBlobCreatedEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<StorageBlobCreatedEventData>();
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }

        else if (string.Equals(eventGridEvent.EventType, CustomTopicEvent, StringComparison.OrdinalIgnoreCase))
        {
            var eventData = dataObject.ToObject<ContosoItemReceivedEventData>();
            log.Info($"Got ContosoItemReceived event data, item URI {eventData.id}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}

```

```javascript

var http = require('http');
var t = require('tcomb');

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";
    var ContosoItemReceivedEventData = t.struct({
        id: t.Str,
        message: t.Str,
        time: t.Str,
    })

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
            var payload = new ContosoItemReceivedEventData(body.data);
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
            context.log(payload instanceof ContosoItemReceivedEventData);
        }
    }
    context.done();
};

```

### <a name="test-custom-event-handling"></a>Test özel olay işleme

Son olarak, ICB'nin işlevinizi özel olay türünüz şimdi işleyebileceğini test edin:

```json

[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Microsoft.EventGrid.CustomEventType",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "id": "831e1650-001e-001b-66ab-eeb76e069631",
            "message": "Contoso item Foo",
            "time": "2017-06-26T18:41:00.9584103Z"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]

```

Bu işlev tarafından Canlı test edebilirsiniz [portaldan CURL ile özel bir olay gönderme](./custom-event-quickstart-portal.md) veya [için özel bir konu nakil](./post-to-custom-topic.md) herhangi bir hizmet veya gibibiruçnoktaiçinNAKLEDEBİLİRSİNİZuygulamakullanarak[Postman](https://www.getpostman.com/). İşlev URL'si olarak ayarlamak uç noktası ile özel bir konu ve bir olay aboneliği oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

* Araştır [Azure olay kılavuz yönetim ve SDK'ları yayımlama](./sdk-overview.md)
* Bilgi edinmek için nasıl [post için özel bir konu](./post-to-custom-topic.md)
* Ayrıntılı olay kılavuz ve işlevleri öğreticileri birini gibi deneyin [karşıya Blob depolama alanına görüntüleri yeniden boyutlandırma](resize-images-on-storage-blob-upload-event.md)
