---
title: Azure işlevleri C# Geliştirici Başvurusu
description: C# kullanarak Azure işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 09/12/2018
ms.author: glenga
ms.openlocfilehash: 2a6d670ba9f2f496cc94d2790eb6f66d46305746
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872794"
---
# <a name="azure-functions-c-developer-reference"></a>Azure işlevleri C# Geliştirici Başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-reference-csharp.md -->

Bu makalede, C# .NET sınıf kitaplıkları kullanarak Azure işlevleri geliştirmeye giriş niteliğindedir.

Azure işlevleri C# ve betik C# programlama dillerini destekler. İçin yönergeler arıyorsanız [C# kullanarak Azure portalında](functions-create-function-app-portal.md), bkz: [C# betiği (.csx) Geliştirici Başvurusu](functions-reference-csharp.md).

Bu makalede, aşağıdaki makalelere zaten konusunu okuduğunuz varsayılır:

* [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md)
* [Azure işlevleri Visual Studio 2019 araçları](functions-develop-vs.md)

## <a name="functions-class-library-project"></a>İşlevleri sınıf kitaplığı projesi

Visual Studio'da **Azure işlevleri** proje şablonu, aşağıdaki dosyaları içeren bir C# sınıf kitaplığı projesi oluşturur:

* [Host.JSON](functions-host-json.md) -proje uygulamasında tüm işlevleri yerel olarak veya azure'da çalıştırırken etkileyen yapılandırma ayarları depolar.
* [Local.Settings.JSON](functions-run-local.md#local-settings-file) -uygulama ayarları ve yerel olarak çalıştırırken kullanılan bağlantı dizeleri depolar. Bu dosya, gizli dizileri içerir ve işlev uygulamanızı azure'da yayımlanan değil. Bunun yerine, [uygulama ayarlarını işlev uygulamanıza ekleme](functions-develop-vs.md#function-app-settings).

Aşağıdaki örnek, yapı çıktı dizininde oluşturulmuş gibi görünen bir klasör yapısı proje oluşturduğunuzda:

```
<framework.version>
 | - bin
 | - MyFirstFunction
 | | - function.json
 | - MySecondFunction
 | | - function.json
 | - host.json
```

Bu dizin ne işlev uygulamanızı azure'da dağıtılan olur. Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı olan [projeye NuGet paketleri olarak eklenen](./functions-bindings-register.md#c-class-library-with-visual-studio-2019).

> [!IMPORTANT]
> Derleme işlemi oluşturur bir *function.json* her işlev için dosya. Bu *function.json* dosyasının doğrudan düzenlenmesi değil yöneliktir. Bağlama yapılandırması değiştiremez veya bu dosyayı düzenleyerek işlevi devre dışı bırakın. Bir işlev devre dışı bırakma hakkında bilgi edinmek için [işlevler devre dışı bırakma](disable-function.md#functions-2x---c-class-libraries).

## <a name="methods-recognized-as-functions"></a>İşlevleri kabul edilen yöntemleri

Bir sınıf kitaplığında bir işlev ile statik bir yöntemi olan bir `FunctionName` ve aşağıdaki örnekte gösterildiği gibi bir tetikleyici özniteliği:

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

`FunctionName` Özniteliği yöntemi bir işlev giriş noktası olarak işaretler. Adı bir projesi içinde benzersiz olmalı, bir harf ile başlamalı ve yalnızca harf, sayı içeren `_`, ve `-`, en çok 127 karakter uzunluğunda. Proje şablonları, genellikle adlı bir yöntem oluşturma `Run`, ancak yöntem adı geçerli bir C# yöntem adı olabilir.

Tetikleyici özniteliğini tetikleyici türü belirtir ve giriş verileri için bir yöntem parametresi bağlar. Örnek işlevi bir kuyruk iletisi tarafından tetiklenir ve kuyruk iletisini yöntemine geçirilen `myQueueItem` parametresi.

## <a name="method-signature-parameters"></a>Yöntem imzası parametreleri

Yöntem imzası tetikleyici özniteliğiyle kullanılan farklı parametreler içerebilir. Ekleyebileceğiniz ek parametreler bazıları şunlardır:

* [Giriş ve çıkış bağlamaları](functions-triggers-bindings.md) özniteliklerle tasarlayarak işaretlenen.  
* Bir `ILogger` veya `TraceWriter` ([yalnızca 1.x sürümü](functions-versions.md#creating-1x-apps)) parametresi için [günlüğü](#logging).
* A `CancellationToken` parametresi için [kapatılmasını](#cancellation-tokens).
* [İfadeleri bağlama](./functions-bindings-expressions-patterns.md) meta verileri için parametreleri tetikleyin.

İşlev imzası parametrelerinde sırası önemli değildir. Örneğin, önce veya sonra diğer bağlamalar tetikleyici parametreleri yerleştirebilir ve Günlükçü parametrenin öncesinde veya sonrasında tetikleyicisi veya bağlaması parametreleri koyabilirsiniz.

### <a name="output-binding-example"></a>Çıkış bağlaması örneği

Aşağıdaki örnek, önceki bir kuyruk çıkış bağlaması ekleyerek değiştirir. İşlev için farklı bir sırada yeni bir kuyruk iletisi işlevi tetikleyen bir kuyruk iletisi yazar.

```csharp
public static class SimpleExampleWithOutput
{
    [FunctionName("CopyQueueMessage")]
    public static void Run(
        [QueueTrigger("myqueue-items-source")] string myQueueItem, 
        [Queue("myqueue-items-destination")] out string myQueueItemCopy,
        ILogger log)
    {
        log.LogInformation($"CopyQueueMessage function processed: {myQueueItem}");
        myQueueItemCopy = myQueueItem;
    }
}
```

Bağlama başvuru makalelerimize ([depolama kuyrukları](functions-bindings-storage-queue.md), örneğin) tetikleyici, giriş veya çıkış öznitelikleri bağlaması ile kullanabileceğiniz hangi parametre türleri açıklanmaktadır.

### <a name="binding-expressions-example"></a>Bağlama ifadeleri örneği

Aşağıdaki kod, bir uygulama ayarı izlemek için kuyruk adını alır ve kuyruk iletisi oluşturulma zamanını alır `insertionTime` parametresi.

```csharp
public static class BindingExpressionsExample
{
    [FunctionName("LogQueueMessage")]
    public static void Run(
        [QueueTrigger("%queueappsetting%")] string myQueueItem,
        DateTimeOffset insertionTime,
        ILogger log)
    {
        log.LogInformation($"Message content: {myQueueItem}");
        log.LogInformation($"Created at: {insertionTime}");
    }
}
```

## <a name="autogenerated-functionjson"></a>Otomatik olarak oluşturulan function.json

Derleme işlemi oluşturur bir *function.json* derleme klasörü işlevi klasöründe bir dosya. Daha önce belirtildiği gibi bu dosyayı doğrudan düzenlenmesi için tasarlanmamıştır. Bağlama yapılandırması değiştiremez veya bu dosyayı düzenleyerek işlevi devre dışı bırakın. 

Bu dosyanın amacı için kullanmak üzere ölçeği denetleyicisini bilgileri sağlamaktır [kararları tüketim planında ölçeklendirme](functions-scale.md#how-the-consumption-and-premium-plans-work). Bu nedenle, dosya yalnızca Tetikleyici bilgileri, giriş veya çıktı bağlaması var.

Oluşturulan *function.json* dosya içeren bir `configurationSource` çalışma zamanı bağlamaları için .NET öznitelikleri kullanmak için bildiren özelliği yerine *function.json* yapılandırma. Bir örneği aşağıda verilmiştir:

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

## <a name="microsoftnetsdkfunctions"></a>Microsoft.NET.Sdk.Functions

*Function.json* dosya oluşturma, NuGet paketi tarafından gerçekleştirilir [Microsoft\.NET\.Sdk\.işlevleri](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions). 

Her iki sürümü için kullanılan aynı paket 1.x ve 2.x'i işlevler çalışma zamanı. Hedef Framework'ü ne 1.x proje 2.x projeden ayırır ' dir. İlgili bölümleri şunlardır *.csproj* farklı gösteren dosyalar, hedef çerçeveleri ve aynı `Sdk` paket:

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
  <TargetFramework>netcoreapp2.1</TargetFramework>
  <AzureFunctionsVersion>v2</AzureFunctionsVersion>
</PropertyGroup>
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Sdk.Functions" Version="1.0.8" />
</ItemGroup>
```

Arasında `Sdk` paket bağımlılıkları, Tetikleyicileri ve bağlamaları alınır. .NET Core 2.x Tetikleyicileri ve bağlamaları hedef sırada bu Tetikleyicileri ve bağlamaları .NET Framework'ü hedefleyen çünkü 1.x Tetikleyicileri ve bağlamaları için bir 1.x proje ifade eder.

`Sdk` Paket de bağlıdır [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json)ve dolaylı olarak [WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage). Bu bağımlılıkları projenize işlevler çalışma zamanı sürümüyle çalışan bu paketleri sürümlerini kullandığından emin olun, proje. Örneğin, `Newtonsoft.Json` sürüm 11 için .NET Framework 4.6.1 var, ancak .NET Framework 4.6.1'i hedefleyen işlevler çalışma zamanı yalnızca uyumlu `Newtonsoft.Json` 9.0.1. Bu projedeki işlev kodunuzu kullanmak de sahiptir. `Newtonsoft.Json` 9.0.1.

Kaynak kodu `Microsoft.NET.Sdk.Functions` GitHub deposunda kullanılabilir [azure\-işlevleri\-vs\-derleme\-sdk](https://github.com/Azure/azure-functions-vs-build-sdk).

## <a name="runtime-version"></a>Çalışma zamanı sürümü

Visual Studio kullanan [Azure işlevleri çekirdek Araçları](functions-run-local.md#install-the-azure-functions-core-tools) işlevleri projelerini çalıştırmak için. Temel araçları, İşlevler çalışma zamanı için bir komut satırı arabirimidir.

Npm kullanarak Core araçlarını yüklerseniz, Visual Studio tarafından kullanılan temel araçları sürümü etkilemez. İşlevler çalışma zamanı sürümü için 1.x, Visual Studio temel araçları sürümlerinde depolar *%USERPROFILE%\AppData\Local\Azure.Functions.Cli* ve burada depolanan en son sürümünü kullanır. İçin 2.x İşlevler, temel Araçlar dahil **Azure işlevleri ve Web işleri Araçları** uzantısı. 1.x ve 2.x'i için İşlevler projesi çalıştırdığınızda, konsol çıkışında hangi sürümü kullanılıyor görebilirsiniz:

```terminal
[3/1/2018 9:59:53 AM] Starting Host (HostId=contoso2-1518597420, Version=2.0.11353.0, ProcessId=22020, Debug=False, Attempt=0, FunctionsExtensionVersion=)
```

## <a name="supported-types-for-bindings"></a>Bağlamaları için desteklenen türler:

Her bağlama desteklenen türlerinden; yine de sahip istiyor musunuz? Örneğin, bir blob tetikleyici özniteliği bir POCO parametresi bir dize parametresi için uygulanabilir bir `CloudBlockBlob` parametresi ve birçok diğer hiçbirini desteklenen türleri. [Blob bağlamaları için bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger---usage) listeleri tüm desteklenen parametre türleri. Daha fazla bilgi için [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) ve [bağlama her bağlama türü için başvuru belgeleri](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="binding-to-method-return-value"></a>Yöntemin dönüş değeri bağlama

Yöntemin dönüş değerini özniteliği uygulayarak, bir yöntemin dönüş değeri bir çıkış bağlaması için kullanabilirsiniz. Örnekler için bkz [Tetikleyicileri ve bağlamaları](./functions-bindings-return-value.md). 

Yalnızca her zaman başarılı işlevi yürütme için çıktı bağlama geçirmek için bir dönüş değeri sonuçlanırsa, dönüş değeri kullanın. Aksi takdirde kullanın `ICollector` veya `IAsyncCollector`aşağıdaki bölümde gösterildiği gibi.

## <a name="writing-multiple-output-values"></a>Birden çok çıkış değerleri yazılıyor

Bir çıkış bağlaması için birden çok değer yazın ya da başarılı işlev çağrısını çıkış bağlaması için geçirilecek herhangi bir şey de sağlamayabilir kullanırsanız [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türleri. Bu yöntem tamamlandığında, çıkış bağlaması için yazılan salt yazılır koleksiyonları türleridir.

Bu örnek aynı kullanarak kuyruk birden fazla kuyruk iletileri Yazar `ICollector`:

```csharp
public static class ICollectorExample
{
    [FunctionName("CopyQueueMessageICollector")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-3")] string myQueueItem,
        [Queue("myqueue-items-destination")] ICollector<string> myDestinationQueue,
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
        myDestinationQueue.Add($"Copy 1: {myQueueItem}");
        myDestinationQueue.Add($"Copy 2: {myQueueItem}");
    }
}
```

## <a name="logging"></a>Günlüğe kaydetme

Çıkış akış günlüklerinizi oturum C#, türünde bir bağımsız değişken içeren [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Bu ad öneririz `log`, aşağıdaki örnekte olduğu gibi:  

```csharp
public static class SimpleExample
{
    [FunctionName("QueueTrigger")]
    public static void Run(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
} 
```

Kullanmaktan kaçının `Console.Write` Azure işlevleri'nde. Daha fazla bilgi için bkz. [yazma oturum açtığında C# işlevleri](functions-monitoring.md#write-logs-in-c-functions) içinde **İzleyici Azure işlevleri** makalesi.

## <a name="async"></a>zaman uyumsuz

Bir işlev yapmak için [zaman uyumsuz](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), kullanın `async` anahtar sözcüğü ve dönüş bir `Task` nesne.

```csharp
public static class AsyncExample
{
    [FunctionName("BlobCopy")]
    public static async Task RunAsync(
        [BlobTrigger("sample-images/{blobName}")] Stream blobInput,
        [Blob("sample-images-copies/{blobName}", FileAccess.Write)] Stream blobOutput,
        CancellationToken token,
        ILogger log)
    {
        log.LogInformation($"BlobCopy function processed.");
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
}
```

Kullanamazsınız `out` zaman uyumsuz işlevleri parametreleri. Çıkış bağlamaları için kullanmak [işlev dönüş değeri](#binding-to-method-return-value) veya [Toplayıcı nesnesi](#writing-multiple-output-values) yerine.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev kabul edebilen bir [CancellationToken](/dotnet/api/system.threading.cancellationtoken) işlev sona erdirilecek olduğunda kodunuzu bildirmek işletim sistemi sağlayan parametresi. Bu bildirim, işlev beklenmedik bir şekilde verileri tutarsız bir durumda bırakır şekilde sonlandırmaz emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlevi sonlandırma için nasıl kontrol edileceğini gösterir.

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

Bir ortam değişkeni veya değeri ayarlamak uygulama almak için kullanın `System.Environment.GetEnvironmentVariable`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static class EnvironmentVariablesExample
{
    [FunctionName("GetEnvironmentVariables")]
    public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, ILogger log)
    {
        log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
        log.LogInformation(GetEnvironmentVariable("AzureWebJobsStorage"));
        log.LogInformation(GetEnvironmentVariable("WEBSITE_SITE_NAME"));
    }

    public static string GetEnvironmentVariable(string name)
    {
        return name + ": " +
            System.Environment.GetEnvironmentVariable(name, EnvironmentVariableTarget.Process);
    }
}
```

Uygulama ayarları, yerel olarak geliştirirken hem Azure'da çalıştırırken ortam değişkenlerinden okunabilir. Uygulama ayarları yerel olarak geliştirirken, gelir `Values` koleksiyonda *local.settings.json* dosya. Her iki ortamda yerel ve Azure, `GetEnvironmentVariable("<app setting name>")` adlandırılmış uygulama ayarının değerini alır. Yerel olarak çalıştırırken, örneğin, "Site adı", döndürülecek olan, *local.settings.json* dosyasını içeren `{ "Values": { "WEBSITE_SITE_NAME": "My Site Name" } }`.

[System.Configuration.ConfigurationManager.AppSettings](https://docs.microsoft.com/dotnet/api/system.configuration.configurationmanager.appsettings) özelliği, uygulama ayar değerleri almak için alternatif bir API olduğu halde kullanmanızı öneririz `GetEnvironmentVariable` burada gösterildiği gibi.

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dilleri kullanabilirsiniz bir [kesinlik temelli](https://en.wikipedia.org/wiki/Imperative_programming) başlangıcı yerine sonundan desen, bağlama [ *bildirim temelli* ](https://en.wikipedia.org/wiki/Declarative_programming) bağlamaları öznitelikler. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Bu düzende, işlev kodunuzu desteklenen giriş ve çıkış bağlamaları üzerinde kolayca bağlayabilirsiniz.

Aşağıdaki şekilde bağlama bir zorunluluktur tanımlayın:

- **Sağlamadığı** istenen kesinlik temelli bağlamalarınızı için işlev imzası bir özniteliğini ekleyin.
- Giriş parametresi geçişinde [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlama gerçekleştirmek için aşağıdaki C# düzeni kullanın.

  ```cs
  using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
  {
      ...
  }
  ```

  `BindingTypeAttribute` bağlamayı tanımlayan bir .NET özniteliktir ve `T` bağlama türü tarafından desteklenen bir giriş veya çıkış türü. `T` olamaz bir `out` parametre türü (gibi `out JObject`). Örneğin, bağlama destekler Mobile Apps tablo çıktısı [altı türleri çıkış](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ancak yalnızca kullanabilirsiniz [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)kesinlik temelli bağlamaya sahip.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod oluşturur bir [depolama blobu çıktı bağlamasını](functions-bindings-storage-blob.md#output) blob ile çalışma zamanında tanımlanan yol sonra yazan bir dize için blob.

```cs
public static class IBinderExample
{
    [FunctionName("CreateBlobUsingBinder")]
    public static void Run(
        [QueueTrigger("myqueue-items-source-4")] string myQueueItem,
        IBinder binder,
        ILogger log)
    {
        log.LogInformation($"CreateBlobUsingBinder function processed: {myQueueItem}");
        using (var writer = binder.Bind<TextWriter>(new BlobAttribute(
                    $"samples-output/{myQueueItem}", FileAccess.Write)))
        {
            writer.Write("Hello World!");
        };
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) tanımlar [depolama blobu](functions-bindings-storage-blob.md) giriş veya çıktı bağlaması ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen çıkış bağlama türü.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Yukarıdaki örnekte işlevi uygulamanın ana depolama hesabı bağlantı dizesi için uygulama ayarı alır (olduğu `AzureWebJobsStorage`). Ekleyerek depolama hesabı için kullanmak üzere bir özel uygulama ayarı belirtebilirsiniz [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ve öznitelik diziye geçirerek `BindAsync<T>()`. Kullanım bir `Binder` parametresi olmayan `IBinder`.  Örneğin:

```cs
public static class IBinderExampleMultipleAttributes
{
    [FunctionName("CreateBlobInDifferentStorageAccount")]
    public async static Task RunAsync(
            [QueueTrigger("myqueue-items-source-binder2")] string myQueueItem,
            Binder binder,
            ILogger log)
    {
        log.LogInformation($"CreateBlobInDifferentStorageAccount function processed: {myQueueItem}");
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

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
