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
ms.openlocfilehash: 70c4d6276970a781517fe49ec47e9b2ddb884c78
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
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

> [!IMPORTANT]
> Derleme işlemi oluşturur bir *function.json* her işlev için dosya. Bu *function.json* dosyasını doğrudan düzenlenmesine izin verilmiyor yöneliktir. Bağlama yapılandırmasını değiştirmek veya bu dosyasını düzenleyerek işlevi devre dışı bırakın. Bir işlev devre dışı bırakmak için [devre dışı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) özniteliği. Örneğin, MY_TIMER_DISABLED ayarı Boolean bir uygulama ekleme ve geçerli `[Disable("MY_TIMER_DISABLED")]` işlevinizi için. Sonra etkinleştirme ve uygulama ayarı değiştirerek devre dışı.

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

### <a name="order-of-parameters"></a>Parametreler sırası

İşlev imzası parametrelerinde sırası önemli değildir. Örneğin, önce veya sonra diğer bağlamaları trigger parametreleri koyabilirsiniz ve Günlükçü parametre önce veya sonra tetikleyici veya bağlama parametrelerini koyabilirsiniz.

### <a name="binding-expressions"></a>Bağlama ifadeleri

Bağlama ifadeleri özniteliği Oluşturucu parametreleri ve işlev parametrelerini kullanabilirsiniz. Örneğin, aşağıdaki kod bir uygulama ayarı izlemek için sıra adını alır ve kuyruk iletisi oluşturma süresi alır `insertionTime` parametresi.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        TraceWriter log)
    {
        log.Info($"Message content: {myQueueItem}");
        log.Info($"Created at: {insertionTime}");
    }
}
```

Daha fazla bilgi için bkz: **ifadeleri ve desenler bağlama** içinde [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md#binding-expressions-and-patterns).

### <a name="conversion-to-functionjson"></a>Function.json dönüştürme

Derleme işlemi oluşturur bir *function.json* yapı klasöründeki işlevi klasöründe dosya. Daha önce belirtildiği gibi bu dosyayı doğrudan düzenlenmesi için tasarlanmamıştır. Bağlama yapılandırmasını değiştirmek veya bu dosyasını düzenleyerek işlevi devre dışı bırakın. 

Bu dosyanın amacı için kullanmak üzere ölçeği denetleyicisini bilgileri sağlamaktır [tüketim planla ilgili kararları ölçeklendirme](functions-scale.md#how-the-consumption-plan-works). Bu nedenle, dosya, yalnızca giriş veya çıkış bağlamaları Tetikleyici bilgileri, vardır.

Oluşturulan *function.json* dosya içeren bir `configurationSource` .NET öznitelikleri bağlamaları için çalışma zamanı söyler özelliği yerine *function.json* yapılandırma. Bir örneği aşağıda verilmiştir:

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

### <a name="microsoftnetsdkfunctions-nuget-package"></a>Microsoft.NET.Sdk.Functions NuGet paketi

*Function.json* dosyası oluşturma NuGet paketi tarafından gerçekleştirilen [Microsoft\.NET\.Sdk\.işlevler](http://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

Aynı paketin her iki sürümü için kullanılan 1.x ve 2.x işlevleri çalışma zamanı. Hedef Framework'ü ne 1.x proje 2.x projeden ayıran ' dir. İlgili bölümleri şunlardır *.csproj* hedef çerçeveler ve aynı dosyaları, farklı gösteren `Sdk` paketi:

**1.x işlevleri**

```xml
<PropertyGroup>
  <TargetFramework>net461</TargetFramework>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

**2.x işlevleri**

```xml
<PropertyGroup>
  <TargetFramework>netstandard2.0</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

Arasında `Sdk` paket bağımlılıkları olan Tetikleyicileri ve bağlamaları. .NET Core hedef 2.x Tetikleyicileri ve bağlamaları sırada bu .NET Framework hedeflemediği 1.x proje 1.x Tetikleyicileri ve bağlamaları başvuruyor.

`Sdk` Paketi de bağımlı [Newtonsoft.Json](http://www.nuget.org/packages/Newtonsoft.Json)ve dolaylı olarak [WindowsAzure.Storage](http://www.nuget.org/packages/WindowsAzure.Storage). Bu bağımlılıklar, projenizin işlevleri çalışma zamanı sürümü ile çalışma bu paketleri sürümlerini kullandığından emin olun, Proje hedefleri. Örneğin, `Newtonsoft.Json` sürüm 11 .NET Framework 4.6.1 olsa da, .NET Framework 4.6.1 hedefleyen işlevleri çalışma zamanı yalnızca uyumlu `Newtonsoft.Json` 9.0.1. Proje işlevi kodunuzda da kullanması gereken şekilde `Newtonsoft.Json` 9.0.1.

Kaynak kodu `Microsoft.NET.Sdk.Functions` GitHub depo kullanılabilir [azure\-işlevleri\-vs\-yapı\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

### <a name="runtime-version"></a>Çalışma zamanı sürümü

Visual Studio kullanan [Azure işlevleri çekirdek Araçları](functions-run-local.md#install-the-azure-functions-core-tools) işlevleri projelerini çalıştırmak için. Çekirdek araçları işlevleri çalışma zamanı için bir komut satırı arabirimidir.

Çekirdek araçları npm kullanarak yüklerseniz, Visual Studio tarafından kullanılan çekirdek Araçları sürüm etkilemez. İşlevler çalışma zamanı sürümü için 1.x, Visual Studio depolayan çekirdek araçları sürümlerinde *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* ve depolanan en son sürümü var. kullanır. İçin 2.x İşlevler, çekirdek araçları içinde yer alan **Azure işlevleri ve Web işleri Araçları** uzantısı. 1.x ve 2.x için işlevleri Projeyi çalıştırdığınızda konsol çıktısı hangi sürümünün kullanıldığını görmek:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Bağlamaları için desteklenen türler

Her bağlama desteklenen türlerinden; yine de sahip istiyor musunuz? örneği için bir POCO parametresi bir dize parametresi için bir blob tetikleyici özniteliği uygulanabilir bir `CloudBlockBlob` parametresi ya da birkaç diğer hiçbirini desteklenen türler. [Blob bağlamaları için bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger---usage) desteklenen tüm listeleri parametre türleri. Daha fazla bilgi için bkz: [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) ve [bağlama başvuru belgeleri her bağlama türü için](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Bağlama yöntemini dönüş değeri

Yöntemin dönüş değerini öznitelik uygulayarak yönteminin dönüş değeri bir çıktı bağlaması için kullanabilirsiniz. Örnekler için bkz: [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md#using-the-function-return-value).

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

`TraceWriter` tanımlanan [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Günlük düzeyi için `TraceWriter` yapılandırılabilir [host.json](functions-host-json.md).

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

## <a name="async"></a>Async

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

Bir işlev kabul edebileceği bir [CancellationToken](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) işlevi hakkında sonlandırılacak olduğunda kodunuzu bildirmek işletim sisteminin sağlar parametresi. Bu bildirim, beklenmedik bir şekilde veri tutarsız bir durumda bırakır şekilde sonlandırma işlevi değil emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlevi sonlandırma için denetlenecek gösterilmiştir.

```csharp
public static class CancellationTokenExample
{
    public static void Run(
        [QueueTrigger("inputqueue")] string inputText,
        TextWriter logger,
        CancellationToken token)
    {
        for (int i = 0; i < 100; i++)
        {
            if (token.IsCancellationRequested)
            {
                logger.WriteLine("Function was cancelled at iteration {0}", i);
                break;
            }
            Thread.Sleep(5000);
            logger.WriteLine("Normal processing for queue message={0}", inputText);
        }
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

  `BindingTypeAttribute` bağlama tanımlayan bir .NET özniteliktir ve `T` , bağlama türü tarafından desteklenen bir giriş veya çıkış türü. `T` olamaz bir `out` parametre türü (gibi `out JObject`). Örneğin, Mobile Apps Tablo Bağlama destekler çıktı [altı türleri çıktı](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ancak yalnızca kullanabilirsiniz [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)kesinlik temelli bağlamaya sahip.

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
| [BlobTrigger](functions-bindings-storage-blob.md#trigger---attributes)| [Blob](functions-bindings-storage-blob.md#input---attributes)| [Blob](functions-bindings-storage-blob.md#output---attributes)|
| [CosmosDBTrigger](functions-bindings-cosmosdb.md#trigger---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#input---attributes)| [DocumentDB](functions-bindings-cosmosdb.md#output---attributes) |
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
