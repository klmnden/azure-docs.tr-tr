---
title: "Azure işlevleriyle .NET sınıf kitaplıkları kullanarak | Microsoft Docs"
description: "Azure işlevleri ile kullanmak için .NET sınıf kitaplıkları Yazar öğrenin"
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: 9f5db0c2-a88e-4fa8-9b59-37a7096fc828
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/10/2017
ms.author: glenga
ms.openlocfilehash: e55af617236f3c36da161158a10b26f2f8f30224
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="using-net-class-libraries-with-azure-functions"></a>Azure işlevleriyle .NET sınıf kitaplıkları kullanma

Komut dosyaları ek olarak, bir veya daha fazla işlev uygulaması olarak bir sınıf kitaplığı yayımlama Azure işlevlerini destekler. Kullanmanızı öneririz [Azure işlevleri Visual Studio 2017 Araçları](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/).

## <a name="prerequisites"></a>Ön koşullar 

Bu makalede aşağıdaki önkoşullar vardır:

- [Visual Studio 2017 sürüm 15.3](https://www.visualstudio.com/vs/), veya sonraki bir sürümü.
- Yükleme **Azure geliştirme** iş yükü.

## <a name="functions-class-library-project"></a>İşlevler sınıf kitaplığı proje

Visual Studio'da yeni bir Azure işlevleri projesi oluşturun. Yeni Proje şablonu dosyaları oluşturur *host.json* ve *local.settings.json*. Yapabilecekleriniz [host.json Azure işlevleri çalışma zamanı ayarlarınızı özelleştirme](functions-host-json.md). 

Dosya *local.settings.json* uygulama ayarları, bağlantı dizeleri ve Azure işlevleri çekirdek araçları ayarlarını saklar. Yapısı hakkında daha fazla bilgi için bkz: [kod ve yerel olarak Azure işlevlerini test](functions-run-local.md#local-settings).

### <a name="functionname-attribute"></a>FunctionName özniteliği

Öznitelik [ `FunctionNameAttribute` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/FunctionNameAttribute.cs) yöntemi bir işlev giriş noktası olarak işaretler. Tam olarak bir tetikleyiciyle kullanılmalıdır ve 0 veya daha fazla giriş ve bağlamaları çıkış.

### <a name="conversion-to-functionjson"></a>Function.json dönüştürme

Bir Azure işlevleri projeyi derlerken bir *function.json* dosya işlevin dizinde oluşturulur. Dizin adı işlevi ile aynıdır, ad `[FunctionName]` özniteliği belirtir. *Function.json* Tetikleyicileri ve bağlamaları ve proje derleme dosyası noktalarına dosyası içerir.

Bu dönüştürme NuGet paketi tarafından gerçekleştirilen [Microsoft\.NET\.Sdk\.işlevler](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). GitHub depo kaynağı kullanılabilir [azure\-işlevleri\-vs\-yapı\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar 

Tetikleyiciler ve bir Azure işlevleri sınıf kitaplığında kullanılabilir bağlamaları aşağıdaki tabloda listelenmektedir. Ad alanındaki tüm öznitelikleridir `Microsoft.Azure.WebJobs`.

| Bağlama | Öznitelik | NuGet paketi |
|------   | ------    | ------        |
| [BLOB Depolama tetikleyici, giriş, çıkış](#blob-storage) | [BlobAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | [Blob depolama] |
| [Cosmos DB tetikleyici](#cosmos-db) | [CosmosDBTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] | 
| [Cosmos DB giriş ve çıkış](#cosmos-db) | [DocumentDBAttribute] | [Microsoft.Azure.WebJobs.Extensions.DocumentDB] |
| [Olay hub'ları tetikleyici ve çıkış](#event-hub) | [EventHubTriggerAttribute], [EventHubAttribute] | [Microsoft.Azure.WebJobs.ServiceBus] |
| [Dış dosya giriş ve çıkış](#api-hub) | [ApiHubFileAttribute] | [Microsoft.Azure.WebJobs.Extensions.ApiHub] |
| [HTTP ve Web kancası tetikleyici](#http) | [HttpTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions.Http] |
| [Mobile Apps giriş ve çıkış](#mobile-apps) | [MobileTableAttribute] | [Microsoft.Azure.WebJobs.Extensions.MobileApps] | 
| [Bildirim hub'ları çıktı](#nh) | [NotificationHubAttribute] | [Microsoft.Azure.WebJobs.Extensions.NotificationHubs] | 
| [Kuyruk depolama tetikleyici ve çıkış](#queue) | [QueueAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [SendGrid çıkış](#sendgrid) | [SendGridAttribute] | [Microsoft.Azure.WebJobs.Extensions.SendGrid] | 
| [Hizmet veri yolu tetikleyici ve çıkış](#service-bus) | [ServiceBusAttribute], [ServiceBusAccountAttribute] | [Microsoft.Azure.WebJobs.ServiceBus]
| [Tablo depolama giriş ve çıkış](#table) | [TableAttribute], [StorageAccountAttribute] | [Microsoft.Azure.WebJobs] | 
| [Zamanlayıcı tetikleyicisi](#timer) | [TimerTriggerAttribute] | [Microsoft.Azure.WebJobs.Extensions] | 
| [Twilio çıkış](#twilio) | [TwilioSmsAttribute] | [Microsoft.Azure.WebJobs.Extensions.Twilio] | 

<a name="blob-storage"></a>

### <a name="blob-storage-trigger-input-bindings-and-output-bindings"></a>BLOB Depolama tetikleyici bağlamaları giriş ve bağlamaları çıkış

Azure işlevleri destekler tetikleyici, giriş ve Azure Blob Depolama bağlantılarında çıkış. Bağlama ifadeleri ve meta verileri hakkında daha fazla bilgi için bkz: [Azure işlevleri Blob Depolama bağlamaları](functions-bindings-storage-blob.md).

Bir blob tetikleyici ile tanımlanmış `[BlobTrigger]` özniteliği. Öznitelik kullanabilirsiniz `[StorageAccount]` bir tüm işlev veya sınıf tarafından kullanılan depolama hesabı bağlantı dizesi içeren uygulama ayarı adı tanımlamak için.

```csharp
[StorageAccount("AzureWebJobsStorage")]
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Blob giriş ve çıkış kullanılarak tanımlanır `[Blob]` , bunların ile öznitelik bir `FileAccess` parametresi belirten okuma veya yazma. Aşağıdaki örnek, bir blob kullanır tetikleyici ve blob çıkış bağlama.

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image, 
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall, 
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```        

<a name="cosmos-db"></a>

### <a name="cosmos-db-trigger-input-bindings-and-output-bindings"></a>Cosmos DB tetikleyici bağlamaları giriş ve bağlamaları çıkış

Azure işlevleri Tetikleyicileri destekler ve giriş ve Cosmos DB bağlantılarında çıkış. Cosmos DB bağlama özellikleri hakkında daha fazla bilgi için bkz: [Azure işlevleri Cosmos DB bağlamaları](functions-bindings-documentdb.md).

Cosmos DB belgeden tetiklemek için öznitelik kullanmak `[CosmosDBTrigger]` NuGet paketi [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Belirli bir aşağıdaki örnek tetikleyicilerden `database` ve `collection`. Ayar `myCosmosDB` Cosmos DB örneğine bağlantı içerir. 

```csharp
[FunctionName("DocumentUpdates")]
public static void Run(
    [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
IReadOnlyList<Document> documents, TraceWriter log)
{
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
}
```

Cosmos DB belgeye bağlamak için öznitelik kullanmak `[DocumentDB]` NuGet paketi [Microsoft.Azure.WebJobs.Extensions.DocumentDB]. Aşağıdaki örnek, bir sıra tetikleyici ve bir DocumentDB bağlama çıktısı API vardır.

```csharp
[FunctionName("QueueToDocDB")]        
public static void Run(
    [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, 
    [DocumentDB("ToDoList", "Items", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
{
    document = new { Text = myQueueItem, id = Guid.NewGuid() };
}

```

<a name="event-hub"></a>

### <a name="event-hubs-trigger-and-output"></a>Olay hub'ları tetikleyici ve çıkış

Tetikler ve olay hub'ları için bağlamaları çıktı Azure işlevleri destekler. Daha fazla bilgi için bkz: [Azure işlevleri olay hub'ı bağlamaları](functions-bindings-event-hubs.md).

Türleri `[Microsoft.Azure.WebJobs.ServiceBus.EventHubTriggerAttribute]` ve `[Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.ServiceBus]. 

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi kullanır:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Aşağıdaki örnek, bir olay çıkışı, çıkış olarak yöntemi dönüş değerini kullanarak Hub sahiptir:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

<a name="api-hub"></a>

### <a name="external-file-input-and-output"></a>Dış dosya giriş ve çıkış

Azure işlevlerini destekleyen tetikleyici, giriş ve çıkış bağlamaları Google sürücü, Dropbox ve OneDrive gibi harici dosyalar için. Daha fazla bilgi için bkz: [Azure işlevleri dış dosya bağlamalarını](functions-bindings-external-file.md). Öznitelikleri `[ExternalFileTrigger]` ve `[ExternalFile]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.Extensions.ApiHub].

Aşağıdaki C# örnek dış dosyası giriş ve çıkış bağlama gösterir. Kod giriş dosyası çıktı dosyasına kopyalar.

```csharp
[StorageAccount("MyStorageConnection")]
[FunctionName("ExternalFile")]
[return: ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}-Copy", FileAccess.Write)]
public static string Run([QueueTrigger("myqueue-items")] string myQueueItem, 
    [ApiHubFile("MyFileConnection", "samples-workitems/{queueTrigger}", FileAccess.Read)] string myInputFile, 
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    return myInputFile;
}
```

<a name="http"></a>

### <a name="http-and-webhooks"></a>HTTP ve web kancaları

Kullanım `HttpTrigger` bir HTTP tetikleyicisi veya Web kancası tanımlamak için öznitelik. Bu öznitelik NuGet paketi tanımlanan [Microsoft.Azure.WebJobs.Extensions.Http]. Yetki düzeyini, Web kancası türü, yol ve yöntemleri özelleştirebilirsiniz. Aşağıdaki örnek, bir HTTP tetikleyicisi anonim kimlik doğrulaması ile tanımlar ve _genericJson_ Web kancası türü.

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

<a name="mobile-apps"></a>

### <a name="mobile-apps-input-and-output"></a>Mobile Apps giriş ve çıkış

Giriş ve Mobile Apps için bağlamaları çıktı Azure işlevleri destekler. Daha fazla bilgi için bkz: [Azure işlevleri Mobile Apps bağlamaları](functions-bindings-mobile-apps.md). Öznitelik `[MobileTable]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.Extensions.MobileApps].

Aşağıdaki örnek, Mobile Apps gösterir bağlama çıktı:

```csharp
[FunctionName("MobileAppsOutput")]        
[return: MobileTable(ApiKeySetting = "MyMobileAppKey", TableName = "MyTable", MobileAppUriSetting = "MyMobileAppUri")]
public static object Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem, TraceWriter log)
{
    return new { Text = $"I'm running in a C# function! {myQueueItem}" };
}
```

<a name="nh"></a>

### <a name="notification-hubs-output"></a>Bildirim hub'ları çıktı

Azure işlevleri, bildirim hub'ları için bir çıktı bağlama destekler. Daha fazla bilgi için bkz: [Azure işlevleri bildirim hub'ı çıktı bağlama](functions-bindings-notification-hubs.md). Öznitelik `[NotificationHub]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.Extensions.NotificationHubs].

<a name="queue"></a>

### <a name="queue-storage-trigger-and-output"></a>Kuyruk depolama tetikleyici ve çıkış

Tetikler ve Azure sıralar bağlamaları çıktı Azure işlevleri destekler. Daha fazla bilgi için bkz: [Azure işlevleri kuyruk depolama bağlamaları](functions-bindings-storage-queue.md).

Aşağıdaki örnek bağlama, kullanarak bir sıra çıkışıyla işlev dönüş türü kullanmayı gösterir `[Queue]` özniteliği. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // HTTP trigger with queue output binding
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  TraceWriter log)
    {
        log.Info($"C# function processed: {input.Text}");
        return input.Text;
    }
}

```

Bir kuyruk tetikleyici tanımlamak için `[QueueTrigger]` özniteliği.
```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    // Queue trigger
    [FunctionName("QueueTrigger")]
    [StorageAccount("AzureWebJobsStorage")]
    public static void QueueTrigger([QueueTrigger("myqueue-items")] string myQueueItem, TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
}

```


<a name="sendgrid"></a>

### <a name="sendgrid-output"></a>SendGrid çıkış

Azure işlevleri program aracılığıyla e-posta göndermek için bağlama SendGrid çıkış destekler. Daha fazla bilgi için bkz: [Azure işlevleri SendGrid bağlamaları](functions-bindings-sendgrid.md).

Öznitelik `[SendGrid]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.Extensions.SendGrid]. SendGrid bağlama adlandırılmış ayarlama uygulama gerektirir `AzureWebJobsSendGridApiKey`, SendGrid API anahtarınızı içerir. Bu varsayılan ayarı SendGrid API anahtarınıza ilişkin adıdır. Anahtar veya farklı ayar adı seçin birden fazla SendGrid değiştirilmesi gerekiyorsa, bu adı kullanarak ayarlayabilirsiniz `ApiKey` özelliği `SendGrid` aşağıdaki örnekteki gibi bağlama özniteliği:

    [SendGrid(ApiKey = "MyCustomSendGridKeyName")]

Aşağıdaki hizmet veri yolu kuyruğu tetikleyici kullanılarak ve SendGrid çıktı kullanarak bir bağlama örneğidir `SendGridMessage`:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```
Bu örnek, depolama adlandırılmış ayarını bir uygulamada olmasını SendGrid API anahtarı gerektirir Not `AzureWebJobsSendGridApiKey`.

<a name="service-bus"></a>

### <a name="service-bus-trigger-and-output"></a>Hizmet veri yolu tetikleyici ve çıkış

Tetikler ve Service Bus kuyrukları ve konuları için bağlamaları çıktı Azure işlevleri destekler. Bağlamaları yapılandırma hakkında daha fazla bilgi için bkz: [Azure işlevleri Service Bus bağlamaları](functions-bindings-service-bus.md).

Öznitelikleri `[ServiceBusTrigger]` ve `[ServiceBus]` NuGet paketinde tanımlanan [Microsoft.Azure.WebJobs.ServiceBus]. 

Hizmet veri yolu kuyruğu tetikleyici bir örnek verilmiştir:

```csharp
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run([ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

Bağlama, yöntemin dönüş türü çıkış olarak kullanan bir hizmet veri yolu çıktısı örneği verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```        

<a name="table"></a>

### <a name="table-storage-input-and-output"></a>Tablo depolama giriş ve çıkış

Giriş ve Azure tablo depolaması için bağlamaları çıktı Azure işlevleri destekler. Daha fazla bilgi için bkz: [Azure işlevleri tablo depolama bağlamaları](functions-bindings-storage-table.md).

Aşağıdaki örnekte, iki işlevlerle tablo depolama çıkış ve giriş bağlamaları tanıtmak için kullanılan bir sınıftır. 

```csharp
[StorageAccount("AzureWebJobsStorage")]
public class TableStorage
{
    public class MyPoco
    {
        public string PartitionKey { get; set; }
        public string RowKey { get; set; }
        public string Text { get; set; }
    }

    [FunctionName("TableOutput")]
    [return: Table("MyTable")]
    public static MyPoco TableOutput([HttpTrigger] dynamic input, TraceWriter log)
    {
        log.Info($"C# http trigger function processed: {input.Text}");
        return new MyPoco { PartitionKey = "Http", RowKey = Guid.NewGuid().ToString(), Text = input.Text };
    }

    // use the metadata parameter "queueTrigger" to bind the queue payload
    [FunctionName("TableInput")]
    public static void TableInput([QueueTrigger("table-items")] string input, [Table("MyTable", "Http", "{queueTrigger}")] MyPoco poco, TraceWriter log)
    {
        log.Info($"C# function processed: {poco.Text}");
    }
}

```

<a name="timer"></a>

### <a name="timer-trigger"></a>Zamanlayıcı tetikleyicisi

Azure işlevleri tanımlanmış bir zamanlamaya göre işlevi kodunuzu çalıştırmak olanak sağlayan zamanlayıcı tetikleyicisi bağlama sahiptir. Bağlama özellikleri hakkında daha fazla bilgi için bkz: [zamanlama kod yürütmeyi Azure işlevleriyle](functions-bindings-timer.md).

Tüketim plan üzerinde zamanlamalarla tanımlayabilirsiniz bir [CRON ifade](http://en.wikipedia.org/wiki/Cron#CRON_expression). Bir uygulama hizmeti planı kullanıyorsanız, bir TimeSpan dize de kullanabilirsiniz. 

Aşağıdaki örnek, her beş dakikada bir zamanlayıcı tetikleyicisi tanımlar:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

<a name="twilio"></a>

### <a name="twilio-output"></a>Twilio çıkış

Azure işlevleri destekler Twilio SMS iletileri göndermek işlevlerinizi etkinleştirmek için bağlamaları çıktı. Daha fazla bilgi için bkz: [Twilio kullanarak Azure işlevlerden SMS gönder İleti çıkışı bağlama](functions-bindings-twilio.md). 

Öznitelik `[TwilioSms]` paketinde tanımlanan [Microsoft.Azure.WebJobs.Extensions.Twilio].

Aşağıdaki C# örnek bir sıra kullandığı tetikleyici ve bir Twilio çıkış bağlama:

```csharp
[FunctionName("QueueTwilio")]
[return: TwilioSms(AccountSidSetting = "TwilioAccountSid", AuthTokenSetting = "TwilioAuthToken", From = "+1425XXXXXXX" )]
public static SMSMessage Run([QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] JObject order, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {order}");

    var message = new SMSMessage()
    {
        Body = $"Hello {order["name"]}, thanks for your order!",
        To = order["mobileNumber"].ToString()
    };

    return message;
}
```

## <a name="next-steps"></a>Sonraki adımlar

C# betik yazımı Azure işlevleri kullanma hakkında daha fazla bilgi için bkz: [Azure işlevleri C\# komut Geliştirici Başvurusu](functions-reference-csharp.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]


<!-- NuGet packages --> 
[Microsoft.Azure.WebJobs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.DocumentDB]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB/1.1.0-beta4
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.MobileApps]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.MobileApps/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.NotificationHubs]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.NotificationHubs/1.1.0-beta1
[Microsoft.Azure.WebJobs.ServiceBus]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.SendGrid]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid/2.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions.Http]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http/1.0.0-beta1
[Microsoft.Azure.WebJobs.Extensions.BotFramework]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.BotFramework/1.0.15-beta
[Microsoft.Azure.WebJobs.Extensions.ApiHub]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ApiHub/1.0.0-beta4
[Microsoft.Azure.WebJobs.Extensions.Twilio]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Twilio/1.1.0-beta1
[Microsoft.Azure.WebJobs.Extensions]: http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions/2.1.0-beta1


<!-- Links to source --> 
[DocumentDBAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs
[CosmosDBTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs
[EventHubAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs
[EventHubTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs
[MobileTableAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs
[NotificationHubAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs 
[ServiceBusAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs
[ServiceBusAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs
[QueueAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs
[StorageAccountAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs
[BlobAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs
[TableAttribute]: https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs
[TwilioSmsAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs
[SendGridAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs
[HttpTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs
[ApiHubFileAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.ApiHub/ApiHubFileAttribute.cs
[TimerTriggerAttribute]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs
