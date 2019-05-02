---
title: Azure işlevleri C# betiği Geliştirici Başvurusu
description: C# betik kullanarak Azure işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 12/12/2017
ms.author: glenga
ms.openlocfilehash: 44a9368f82e95641d3df893ba0958c6bf8cf696f
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64724966"
---
# <a name="azure-functions-c-script-csx-developer-reference"></a>Azure işlevleri C# betiği (.csx) Geliştirici Başvurusu

<!-- When updating this article, make corresponding changes to any duplicate content in functions-dotnet-class-library.md -->

Bu makalede, C# betik kullanarak Azure işlevleri geliştirmeye giriş niteliğindedir (*.csx*).

Azure işlevleri C# ve betik C# programlama dillerini destekler. İçin yönergeler arıyorsanız [C# kullanarak bir Visual Studio sınıf kitaplığı projesinde](functions-develop-vs.md), bkz: [C# Geliştirici Başvurusu](functions-dotnet-class-library.md).

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md).

## <a name="how-csx-works"></a>.Csx nasıl çalışır?

Azure işlevleri için C# betik deneyimine dayanır [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki/Introduction). C# işleviniz yöntem bağımsız değişkenleri ile verileri akar. Bağımsız değişken adları içinde belirtilen bir `function.json` var. önceden tanımlı ve dosya adları şey erişmek için işlevi Günlükçü ve iptal belirteçlerini ister.

*.Csx* biçimi daha az "standart" yazın ve hemen bir C# işlevi yazmaya odaklanmasına olanak tanır. Her şeyi bir ad alanını ve sınıf içinde kaydırma yerine tanımlamak bir `Run` yöntemi. Tüm derleme başvurularını ve ad alanları dosyasının başında zamanki içerir.

Bir işlev uygulamasının *.csx* dosyaları örneği başlatıldığında derlenir. Bu derleme adımı hazırlıksız başlatma betiği C# sınıf kitaplıkları karşılaştırılan C# işlevlerine yönelik daha uzun sürebileceğini gibi şeyler ifade eder. Bu derleme ayrıca C# sınıfı kitaplıklar değildir; ancak C# betik işlevleri neden Azure portalında, düzenlenebilir bir adımdır.

## <a name="folder-structure"></a>klasör yapısı

Bir C# betik proje için klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.csx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

Var olan bir paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyalarını (.csx) ve bağlama yapılandırma dosyası (function.json) vardır.

Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı içinde tanımlanmıştır `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](./functions-bindings-register.md#local-development-with-azure-functions-core-tools-and-extension-bundles). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenler için bağlama

Giriş veya çıkış veri C# betik işlevi parametresi bağlı `name` özelliğinde *function.json* yapılandırma dosyası. Aşağıdaki örnekte gösterildiği bir *function.json* dosya ve *run.csx* kuyruk ile tetiklenen bir işlev için dosya. Verileri ve kuyruk iletiyi alan parametresi adlı `myQueueItem` , değeri olduğundan `name` özelliği.

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}");
}
```

`#r` Deyimi açıklanmıştır [bu makalenin ilerleyen bölümlerinde](#referencing-external-assemblies).

## <a name="supported-types-for-bindings"></a>Bağlamaları için desteklenen türler:

Her bağlama desteklenen türlerinden; yine de sahip istiyor musunuz? Örneğin, bir blob tetikleyicisi bir POCO parametresi bir dize parametresi ile kullanılabilir bir `CloudBlockBlob` parametresi ve birçok diğer hiçbirini desteklenen türleri. [Blob bağlamaları için bağlama başvurusu makalesinde](functions-bindings-storage-blob.md#trigger---usage) blob Tetikleyicileri için tüm listeleri desteklenen parametre türleri. Daha fazla bilgi için [Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md) ve [bağlama her bağlama türü için başvuru belgeleri](functions-triggers-bindings.md#next-steps).

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="referencing-custom-classes"></a>Başvuruda bulunan özel sınıflar

Özel bir düz eski CLR nesnesi (POCO) sınıfı kullanmanız gerekiyorsa, aynı dosya içinde sınıf tanımının dahil edebilir veya ayrı bir dosyada yerleştirin.

Aşağıdaki örnekte gösterildiği bir *run.csx* POCO sınıf tanımını içeren bir örnek.

```csharp
public static void Run(string myBlob, out MyClass myQueueItem)
{
    log.Verbose($"C# Blob trigger function processed: {myBlob}");
    myQueueItem = new MyClass() { Id = "myid" };
}

public class MyClass
{
    public string Id { get; set; }
}
```

Alıcı ve Ayarlayıcıyı tanımlanan her bir özellik için bir POCO sınıfı olmalıdır.

## <a name="reusing-csx-code"></a>.Csx kodu yeniden kullanma

Sınıflar ve yöntemler diğer tanımlanan kullanabileceğiniz *.csx* dosyalar, *run.csx* dosya. Bunu yapmak için kullanın `#load` yönergeleri, *run.csx* dosya. Aşağıdaki örnekte, günlüğe kaydetme yordamını adlı `MyLogger` paylaşılmış *myLogger.csx* ve içine yüklenen *run.csx* kullanarak `#load` yönergesi:

Örnek *run.csx*:

```csharp
#load "mylogger.csx"

using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Örnek *mylogger.csx*:

```csharp
public static void MyLogger(ILogger log, string logtext)
{
    log.LogInformation(logtext);
}
```

Paylaşılan kullanarak *.csx* dosyasıdır yaygın bir düzen, kesin bir POCO nesnesini kullanarak işlevleri tarafından arasında geçirilen veri türü istediğinizde. Aşağıdaki Basitleştirilmiş örnekte, bir HTTP tetikleyicisi ve kuyruk tetikleyicisi adlı bir POCO nesneyi paylaşın `Order` kesin sipariş verilerini yazmak için:

Örnek *run.csx* HTTP tetikleyicisi için:

```cs
#load "..\shared\order.csx"

using System.Net;
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, ILogger log)
{
    log.LogInformation("C# HTTP trigger function received an order.");
    log.LogInformation(req.ToString());
    log.LogInformation("Submitting to processing queue.");

    if (req.orderId == null)
    {
        return new HttpResponseMessage(HttpStatusCode.BadRequest);
    }
    else
    {
        await outputQueueItem.AddAsync(req);
        return new HttpResponseMessage(HttpStatusCode.OK);
    }
}
```

Örnek *run.csx* kuyruk tetikleyicisi için:

```cs
#load "..\shared\order.csx"

using System;
using Microsoft.Extensions.Logging;

public static void Run(Order myQueueItem, out Order outputQueueItem, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed order...");
    log.LogInformation(myQueueItem.ToString());

    outputQueueItem = myQueueItem;
}
```

Örnek *order.csx*:

```cs
public class Order
{
    public string orderId {get; set; }
    public string custName {get; set;}
    public string custAddress {get; set;}
    public string custEmail {get; set;}
    public string cartId {get; set; }

    public override String ToString()
    {
        return "\n{\n\torderId : " + orderId +
                  "\n\tcustName : " + custName +             
                  "\n\tcustAddress : " + custAddress +             
                  "\n\tcustEmail : " + custEmail +             
                  "\n\tcartId : " + cartId + "\n}";             
    }
}
```

Göreli bir yol ile kullanabileceğiniz `#load` yönergesi:

* `#load "mylogger.csx"` işlev klasöründe bir dosya yükler.
* `#load "loadedfiles\mylogger.csx"` işlev klasöründe bulunan bir dosya yükler.
* `#load "..\shared\mylogger.csx"` diğer bir deyişle, işlev klasör ile aynı düzeyde bir klasörde bir dosya yükler altında doğrudan *wwwroot*.

`#load` Yönergesi yalnızca ile birlikte çalışır *.csx* dosyaları ile değil *.cs* dosyaları.

## <a name="binding-to-method-return-value"></a>Yöntemin dönüş değeri bağlama

Adını kullanarak bir yöntemin dönüş değeri bir çıkış bağlaması için kullanabilirsiniz `$return` içinde *function.json*. Örnekler için bkz [Tetikleyicileri ve bağlamaları](./functions-bindings-return-value.md).

Yalnızca her zaman başarılı işlevi yürütme için çıktı bağlama geçirmek için bir dönüş değeri sonuçlanırsa, dönüş değeri kullanın. Aksi takdirde kullanın `ICollector` veya `IAsyncCollector`aşağıdaki bölümde gösterildiği gibi.

## <a name="writing-multiple-output-values"></a>Birden çok çıkış değerleri yazılıyor

Bir çıkış bağlaması için birden çok değer yazın ya da başarılı işlev çağrısını çıkış bağlaması için geçirilecek herhangi bir şey de sağlamayabilir kullanırsanız [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türleri. Bu yöntem tamamlandığında, çıkış bağlaması için yazılan salt yazılır koleksiyonları türleridir.

Bu örnek aynı kullanarak kuyruk birden fazla kuyruk iletileri Yazar `ICollector`:

```csharp
public static void Run(ICollector<string> myQueue, ILogger log)
{
    myQueue.Add("Hello");
    myQueue.Add("World!");
}
```

## <a name="logging"></a>Günlüğe kaydetme

Çıkış akış günlüklerinizi oturum C#, türünde bir bağımsız değişken içeren [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Bu ad öneririz `log`. Kullanmaktan kaçının `Console.Write` Azure işlevleri'nde.

```csharp
public static void Run(string myBlob, ILogger log)
{
    log.LogInformation($"C# Blob trigger function processed: {myBlob}");
}
```

> [!NOTE]
> Yerine kullanabileceğiniz daha yeni bir günlüğe kaydetme çerçevesi hakkında daha fazla bilgi için `TraceWriter`, bakın [yazma günlükleri C# işlevleri](functions-monitoring.md#write-logs-in-c-functions) içinde **İzleyici Azure işlevleri** makalesi.

## <a name="async"></a>zaman uyumsuz

Bir işlev yapmak için [zaman uyumsuz](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/), kullanın `async` anahtar sözcüğü ve dönüş bir `Task` nesne.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096);
}
```

Kullanamazsınız `out` zaman uyumsuz işlevleri parametreleri. Çıkış bağlamaları için kullanmak [işlev dönüş değeri](#binding-to-method-return-value) veya [Toplayıcı nesnesi](#writing-multiple-output-values) yerine.

## <a name="cancellation-tokens"></a>İptal belirteçleri

Bir işlev kabul edebilen bir [CancellationToken](/dotnet/api/system.threading.cancellationtoken) işlev sona erdirilecek olduğunda kodunuzu bildirmek işletim sistemi sağlayan parametresi. Bu bildirim, işlev beklenmedik bir şekilde verileri tutarsız bir durumda bırakır şekilde sonlandırmaz emin olmak için kullanabilirsiniz.

Aşağıdaki örnek, yaklaşan işlevi sonlandırma için nasıl kontrol edileceğini gösterir.

```csharp
using System;
using System.IO;
using System.Threading;

public static void Run(
    string inputText,
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
```

## <a name="importing-namespaces"></a>Ad alanlarını alma

Ad alanları almanız gerekiyorsa, bu nedenle olarak normal, ile yapabileceğiniz `using` yan tümcesi.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki ad alanlarını otomatik olarak içeri aktarılır ve bu nedenle isteğe bağlı:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Dış derlemeler başvurma

Framework derlemeleri için kullanarak başvurular ekleyin `#r "AssemblyName"` yönergesi.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
```

Aşağıdaki derlemeleri Azure işlevleri barındırma ortamı tarafından otomatik olarak eklenir:

* `mscorlib`
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`

Aşağıdaki derlemelere basit adıyla başvurulabilir (örneğin, `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Özel derlemelere başvurma

Bir özel bütünleştirilmiş kod başvurusu yapmak için ya da kullanabilirsiniz bir *paylaşılan* derleme veya *özel* derleme:

* Paylaşılan derlemeler, bir işlev uygulaması içindeki tüm işlevleri arasında paylaşılır. Bir özel bütünleştirilmiş kod başvurusu için derleme adlı klasöre yükleyin `bin` içinde [işlevi uygulama kök klasörü](functions-reference.md#folder-structure) (wwwroot).

* Özel derlemeler, verilen işlevin bağlamı'nın bir parçasıdır ve dışarıdan farklı sürümlerini destekler. Özel derlemeler yüklenmiş olması bir `bin` işlevi dizini klasöründe. Dosya adı gibi kullanarak derlemelerine `#r "MyAssembly.dll"`.

Bölümüne bakın işlevi klasörünüze dosyaları karşıya yükleme hakkında daha fazla bilgi için [paket Yönetimi](#using-nuget-packages).

### <a name="watched-directories"></a>İzlenen dizinleri

İşlev komut dosyasını içeren dizine otomatik olarak değişiklikler derlemeler için izlenen. Diğer dizinlerde derleme değişiklikleri izlemek üzere bunları Ekle `watchDirectories` listesinde [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>NuGet paketlerini kullanma
NuGet paketlerini bir 2.x kullanacak şekilde C# işlev, karşıya bir *function.proj* işlevin klasörüne işlevi uygulamanın dosya sisteminde dosya. İşte bir örnek *function.proj* bir başvuru ekler dosya *Microsoft.ProjectOxford.Face* sürüm *1.1.0*:

```xml
<Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
        <TargetFramework>netstandard2.0</TargetFramework>
    </PropertyGroup>
    
    <ItemGroup>
        <PackageReference Include="Microsoft.ProjectOxford.Face" Version="1.1.0" />
    </ItemGroup>
</Project>
```

Akışta bir özel NuGet akışı kullanmak için belirtin bir *Nuget.Config* işlev uygulaması kök dosyasında. Daha fazla bilgi için [yapılandırma NuGet davranışını](/nuget/consume-packages/configuring-nuget-behavior). 

> [!NOTE]
> 1.x içinde C# İşlevler, NuGet paketlerini başvurulan bir *project.json* yerine dosya bir *function.proj* dosya.

1.x işlevleri için bir *project.json* bunun yerine dosya. İşte bir örnek *project.json* dosyası: 

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

### <a name="using-a-functionproj-file"></a>Function.proj dosyası kullanma

1. İşlevi Azure portalında açın. Günlükleri sekmesi, paket yükleme çıkış görüntüler.
2. Karşıya yüklenecek bir *function.proj* dosya, açıklanan yöntemlerden birini kullanın [işlevi uygulama dosyalarını nasıl güncelleştireceğinizi](functions-reference.md#fileupdate) Azure işlevleri Geliştirici Başvurusu konusunda.
3. Sonra *function.proj* dosyası, işlevinizde aşağıdaki örnekte olduğu gibi çıkış günlüğü akış bakın:

```
2018-12-14T22:00:48.658 [Information] Restoring packages.
2018-12-14T22:00:48.681 [Information] Starting packages restore
2018-12-14T22:00:57.064 [Information] Restoring packages for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj...
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\function.proj...
2018-12-14T22:01:00.844 [Information] Installing Newtonsoft.Json 10.0.2.
2018-12-14T22:01:01.041 [Information] Installing Microsoft.ProjectOxford.Common.DotNetStandard 1.0.0.
2018-12-14T22:01:01.140 [Information] Installing Microsoft.ProjectOxford.Face.DotNetStandard 1.0.0.
2018-12-14T22:01:09.799 [Information] Restore completed in 5.79 sec for D:\local\Temp\9e814101-fe35-42aa-ada5-f8435253eb83\function.proj.
2018-12-14T22:01:10.905 [Information] Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri

Bir ortam değişkeni veya değeri ayarlamak uygulama almak için kullanın `System.Environment.GetEnvironmentVariable`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static void Run(TimerInfo myTimer, ILogger log)
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
```

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dilleri kullanabileceğiniz bir [kesinlik temelli](https://en.wikipedia.org/wiki/Imperative_programming) başlangıcı yerine sonundan desen, bağlama [ *bildirim temelli* ](https://en.wikipedia.org/wiki/Declarative_programming) bağlarında *function.json*. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Bu düzende, işlev kodunuzu desteklenen giriş ve çıkış bağlamaları üzerinde kolayca bağlayabilirsiniz.

Aşağıdaki şekilde bağlama bir zorunluluktur tanımlayın:

- **Sağlamadığı** bir girişe dahil *function.json* istenen kesinlik temelli bağlamalarınızı için.
- Giriş parametresi geçişinde [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlama gerçekleştirmek için aşağıdaki C# düzeni kullanın.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute` bağlamayı tanımlayan bir .NET özniteliktir ve `T` bağlama türü tarafından desteklenen bir giriş veya çıkış türü. `T` olamaz bir `out` parametre türü (gibi `out JObject`). Örneğin, bağlama destekler Mobile Apps tablo çıktısı [altı türleri çıkış](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ancak yalnızca kullanabilirsiniz [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)için `T`.

### <a name="single-attribute-example"></a>Tek öznitelik örneği

Aşağıdaki örnek kod oluşturur bir [depolama blobu çıktı bağlamasını](functions-bindings-storage-blob.md#output) blob ile çalışma zamanında tanımlanan yol sonra yazan bir dize için blob.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    using (var writer = await binder.BindAsync<TextWriter>(new BlobAttribute("samples-output/path")))
    {
        writer.Write("Hello World!!");
    }
}
```

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) tanımlar [depolama blobu](functions-bindings-storage-blob.md) giriş veya çıktı bağlaması ve [TextWriter](/dotnet/api/system.io.textwriter) desteklenen çıkış bağlama türü.

### <a name="multiple-attribute-example"></a>Birden çok öznitelik örneği

Yukarıdaki örnekte işlevi uygulamanın ana depolama hesabı bağlantı dizesi için uygulama ayarı alır (olduğu `AzureWebJobsStorage`). Ekleyerek depolama hesabı için kullanmak üzere bir özel uygulama ayarı belirtebilirsiniz [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ve öznitelik diziye geçirerek `BindAsync<T>()`. Kullanım bir `Binder` parametresi olmayan `IBinder`.  Örneğin:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host.Bindings.Runtime;

public static async Task Run(string input, Binder binder)
{
    var attributes = new Attribute[]
    {    
        new BlobAttribute("samples-output/path"),
        new StorageAccountAttribute("MyStorageAccount")
    };

    using (var writer = await binder.BindAsync<TextWriter>(attributes))
    {
        writer.Write("Hello World!");
    }
}
```

Aşağıdaki tabloda her bağlama türü ve bunların tanımlanan paketler için .NET özniteliklerini listeler.

> [!div class="mx-codeBreakAll"]
> | Bağlama | Öznitelik | Başvuru ekleme |
> |------|------|------|
> | Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.CosmosDB"` |
> | Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
> | Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
> | Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
> | Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
> | Depolama kuyruğu | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama blob'u | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Depolama tablosu | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
> | Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tetikleyiciler ve bağlamalar hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Azure işlevleri için en iyi uygulamalar hakkında daha fazla bilgi edinin](functions-best-practices.md)
