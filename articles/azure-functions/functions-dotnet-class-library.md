---
title: "Azure işlevleri C# Geliştirici Başvurusu"
description: "C# kullanarak Azure işlevleri geliştirmek nasıl anlayın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 57edd392d25be20c237185d6780335e4ca9ba3e4
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2018
---
# <a name="azure-functions-c-developer-reference"></a>Azure işlevleri C# Geliştirici Başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Bu makalede, C# .NET sınıf kitaplıkları kullanarak Azure işlevleri geliştirmek için giriş ' dir.

Azure işlevleri, C# ve programlama dilleri C# betik destekler. Yönergeler için arıyorsanız [C# kullanarak Azure portalında](functions-create-function-app-portal.md), bkz: [C# betik (.csx) Geliştirici Başvurusu](functions-reference-csharp.md).

Bu makalede, aşağıdaki makaleler zaten okuduğunuz varsayılır:

* [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md)
* [Visual Studio 2017 araçları Azure işlevleri](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>İşlevler sınıf kitaplığı proje

Visual Studio'da **Azure işlevleri** proje şablonu aşağıdaki dosyaları içeren bir C# sınıf kitaplığı projesi oluşturur:

* [Host.JSON](functions-host-json.md) -yerel olarak veya Azure çalıştırırken projedeki tüm işlevleri etkileyen yapılandırma ayarlarını saklar.
* [Local.Settings.JSON](functions-run-local.md#local-settings-file) -uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri depolar.

### <a name="functionname-and-trigger-attributes"></a>FunctionName ve tetikleyici öznitelikleri

Bir sınıf kitaplığı'nda, statik bir yöntemle işlevidir bir `FunctionName` ve aşağıdaki örnekte gösterildiği gibi bir tetikleyici özniteliği:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

`FunctionName` Özniteliği yöntemi bir işlev giriş noktası olarak işaretler. Adı bir proje içinde benzersiz olmalıdır.

Tetikleyici özniteliği tetikleme türünü belirtir ve bir yöntem parametresi için giriş verileri bağlar. Örnek işlevi bir kuyruk iletisi tarafından tetiklenir ve kuyruk iletisini yöntemine geçirilen `myQueueItem` parametresi.

### <a name="additional-binding-attributes"></a>Ek bağlama öznitelikleri

Ek giriş ve çıkış öznitelikleri bağlama kullanılabilir. Aşağıdaki örnek bir çıktı sırası bağlama ekleyerek önceki bir değiştirir. İşlev giriş sırası ileti farklı bir sırada yeni bir kuyruk iletisi yazar.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

### <a name="conversion-to-functionjson"></a>Function.json dönüştürme

Derleme işlemi oluşturur bir *function.json* yapı klasöründeki işlevi klasöründe dosya. Bu dosyayı doğrudan düzenlenmesi için tasarlanmamıştır. Bağlama yapılandırmasını değiştirmek veya bu dosyasını düzenleyerek işlevi devre dışı bırakın. 

Bu dosyanın amacı için kullanmak üzere ölçeği denetleyicisini bilgileri sağlamaktır [tüketim planla ilgili kararları ölçeklendirme](functions-scale.md#how-the-consumption-plan-works). Bu nedenle, dosya, yalnızca giriş veya çıkış bağlamaları Tetikleyici bilgileri, vardır.

Oluşturulan *function.json* dosya içeren bir `configurationSource` .NET öznitelikleri bağlamaları için çalışma zamanı söyler özelliği yerine *function.json* yapılandırma. Örnek aşağıda verilmiştir:

```json
{
  "generatedBy": "Microsoft.NET.Sdk.Functions-1.0.0.0",
  "configurationSource": "attributes",
  "bindings": [
    {
      "type": "queueTrigger",
      "queueName": "%input-queue-name%",
      "name": "myQueueItem"
    }
  ],
  "disabled": false,
  "scriptFile": "..\\bin\\FunctionApp1.dll",
  "entryPoint": "FunctionApp1.QueueTrigger.Run"
}
```

*Function.json* dosyası oluşturma NuGet paketi tarafından gerçekleştirilen [Microsoft\.NET\.Sdk\.işlevler](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). Kaynak kodu GitHub deposuna kullanılabilir [azure\-işlevleri\-vs\-yapı\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="supported-types-for-bindings"></a>Bağlamaları için desteklenen türler

Her bağlama desteklenen türlerinden; yine de sahip istiyor musunuz? örneği için bir POCO parametresi bir dize parametresi için bir blob tetikleyici özniteliği uygulanabilir bir `CloudBlockBlob` parametresi ya da birkaç diğer hiçbirini desteklenen türler. [Blob bağlamaları için bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger---usage) desteklenen tüm listeleri parametre türleri. Daha fazla bilgi için bkz: [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) ve [bağlama başvuru belgeleri her bağlama türü için](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Bağlama yöntemini dönüş değeri

Aşağıdaki örnekte gösterildiği gibi bir çıktı bağlama için bir yöntem dönüş değeri kullanabilirsiniz:

```csharp
public static class ReturnValueOutputBinding
{
    [FunctionName("CopyQueueMessageUsingReturnValue")]
    [return: Queue("myqueue-items-destination")]
    public static string Run(
        [QueueTrigger("myqueue-items-source-2")] string myQueueItem,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        return myQueueItem;
    }
}
```

## <a name="writing-multiple-output-values"></a>Birden çok çıktı değerleri yazılıyor

Bir çıkış bağlaması birden çok değeri yazmak için kullanın [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türleri. Bu tür yöntemi tamamlandığında, çıkış bağlama yazılan salt yazılır koleksiyonlarıdır.

Bu örnek, aynı kullanarak sıra birden çok sıraya ileti yazar `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myQueueItemCopy,
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 1: {myQueueItem}");
        myQueueItemCopy.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Günlüğe kaydetme

Çıkış akış günlüklerinizi C# oturum açmak için türünde bir bağımsız değişken dahil `TraceWriter`. Bu ad öneririz `log`. Kullanmaktan kaçının `Console.Write` Azure işlevlerinde. 

`TraceWriter`tanımlanan [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Günlük düzeyi için `TraceWriter` yapılandırılabilir [host.json](functions-host-json.md).

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        TraceWriter log)
    {
        log.Info($"C# function processed: {myQueueItem}");
    }
} 
```

> [!NOTE]
> Yerine kullanabileceğiniz yeni bir günlük framework hakkında bilgi için `TraceWriter`, bkz: [yazma günlüklerini C# işlevlerde](functions-monitoring.md#write-logs-in-c-functions) içinde **İzleyici Azure işlevleri** makale.

## <a name="async"></a>Zaman uyumsuz

Bir işlev zaman uyumsuz hale getirmek için kullanmak `async` anahtar sözcüğü ve return bir `Task` nesnesi.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        TraceWriter log)
    {
        log.Info($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bazı işlemler normal şekilde kapatılmasını gerektirir. Her zaman kilitlenen işleyebilir kod yazmak en iyi olmakla birlikte, kapatma isteklerini işlemek istediğiniz durumlarda tanımlayan bir [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) bağımsız değişken belirtilmiş.  A `CancellationToken` bir ana bilgisayar kapatma tetiklenir göstermek için sağlanmıştır.

```csharp
public static class CancellationTokenExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

## <a name="environment-variables"></a>Ortam değişkenleri

Bir ortam değişkeni veya ayar değeri bir uygulamayı almak için `System.Environment.GetEnvironmentVariable`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
    {
        log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
        log.Info(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.Info(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dilleri kullanabileceğiniz bir [kesinlik temelli](https://en.wikipedia.org/wiki/Imperative_programming) tersine düzeni, bağlama [ *bildirim temelli* ](https://en.wikipedia.org/wiki/Declarative_programming) bağlamaları öznitelikler. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde kullanışlıdır. Bu desen ile desteklenen giriş ve çıkış bağlamaları üzerinde-çalışma sırasında işlevi kodunuzda bağlayabilirsiniz.

Aşağıdaki gibi bağlama kesinliği tanımlayın:

- **Sağlamadığı** özniteliği, istenen kesinlik temelli bağlamaları için işlev imzasına dahil edin.
- Giriş parametresi geçişinde [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlama gerçekleştirmek için aşağıdaki C# düzeni kullanın.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute`bağlama tanımlayan bir .NET özniteliktir ve `T` , bağlama türü tarafından desteklenen bir giriş veya çıkış türü. `T`olamaz bir `out` parametre türü (gibi `out JObject`). Örneğin, Mobile Apps Tablo Bağlama destekler çıktı [altı türleri çıktı](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ancak yalnızca kullanabilirsiniz [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)kesinlik temelli bağlamaya sahip.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki kod örneği oluşturur bir [depolama blobu çıktı bağlama](functions-bindings-storage-blob.md#output) blob ile çalışma zamanında tanımlanan yol sonra Yazar bir dize için blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        TraceWriter log)
    {
        log.Info($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) tanımlar [depolama blobu](functions-bindings-storage-blob.md) giriş veya çıkış bağlamayı ve [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) desteklenen çıktı bağlama türü.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Önceki örnekte işlevi uygulamanın ana depolama hesabı bağlantı dizesi için uygulama ayarı alır (olduğu `AzureWebJobsStorage`). Ekleyerek depolama hesabı için kullanılacak bir özel uygulama ayarını belirtebilirsiniz [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ve öznitelik diziye geçirme `BindAsync<T>()`. Kullanım bir `Binder` parametresi olmayan `IBinder`.  Örneğin:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            TraceWriter log)
    {
        log.Info($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
        var attributes = new Attribute[]
        {
        new BlobAttribute($"samples-output/{myQueueItem}", FileAccess.Write),
        new StorageAccountAttribute("MyStorageAccount")
        };
        using (var writer = await binder.BindAsync<TextWriter>(attributes))
        {
            await writer.WriteAsync("Hello World!!");
        }
    }
}
```

## <a name="triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar 

Aşağıdaki tabloda tetikleyici ve bir Azure işlevleri sınıf kitaplığında kullanılabilir bağlama öznitelikleri listeler. Ad alanındaki tüm öznitelikleridir `Microsoft.Azure.WebJobs`.

| Tetikleyici | Girdi | Çıktı|
|------   | ------    | ------  |
| [BlobTrigger](functions-bindings-storage-blob.md#trigger---attributes)| [BLOB](functions-bindings-storage-blob.md#input---attributes)| [BLOB](functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](functions-bindings-documentdb.md#trigger---attributes)| [DocumentDB](functions-bindings-documentdb.md#input---attributes)| [DocumentDB](functions-bindings-documentdb.md#output---attributes) |
| [EventHubTrigger](functions-bindings-event-hubs.md#trigger---attributes)|| [EventHub](functions-bindings-event-hubs.md#output---attributes) |
| [HTTPTrigger](functions-bindings-http-webhook.md#trigger---attributes)|||
| [QueueTrigger](functions-bindings-storage-queue.md#trigger---attributes)|| [Sırası](functions-bindings-storage-queue.md#output---attributes) |
| [ServiceBusTrigger](functions-bindings-service-bus.md#trigger---attributes)|| [ServiceBus](functions-bindings-service-bus.md#output---attributes) |
| [TimerTrigger](functions-bindings-timer.md#attributes) | ||
| |[ApiHubFile](functions-bindings-external-file.md)| [ApiHubFile](functions-bindings-external-file.md)|
| |[MobileTable](functions-bindings-mobile-apps.md#input---attributes)| [MobileTable](functions-bindings-mobile-apps.md#output---attributes) | 
| |[Tablo](functions-bindings-storage-table.md#input---attributes)| [Tablo](functions-bindings-storage-table.md#output---attributes)  | 
| ||[NotificationHub](functions-bindings-notification-hubs.md#attributes) |
| ||[SendGrid](functions-bindings-sendgrid.md#attributes) |
| ||[Twilio](functions-bindings-twilio.md#attributes)| 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
