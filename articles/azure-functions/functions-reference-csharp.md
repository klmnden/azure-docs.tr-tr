---
title: "Azure işlevleri C# betik Geliştirici Başvurusu | Microsoft Docs"
description: "C# kullanarak Azure işlevleri geliştirmek nasıl anlayın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: f28cda01-15f3-4047-83f3-e89d5728301c
ms.service: functions
ms.devlang: dotnet
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/07/2017
ms.author: glenga
ms.openlocfilehash: 18d3a87da8c240a3153dfa68f9b1d8bd17bbe693
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="azure-functions-c-script-developer-reference"></a>Azure işlevleri C# betik Geliştirici Başvurusu
[!INCLUDE [functions-selector-languages](../../includes/functions-selector-languages.md)]

Azure işlevleri için C# betik deneyimi üzerinde Azure WebJobs SDK'sı temel alır. C# işlevinizi yöntem bağımsız değişkenleri ile verileri akar. Bağımsız değişken adları belirtilir `function.json`, ve işlevi Günlükçü ve iptal belirteçleri gibi şeyleri erişmek için önceden tanımlanmış adları vardır.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

Sınıf kitaplıkları C# kullanma hakkında bilgi için bkz: [Azure işlevlerini kullanarak .NET sınıf kitaplıkları](functions-dotnet-class-library.md).

## <a name="how-csx-works"></a>.Csx nasıl çalışır?
`.csx` Biçimi daha az "ortak" yazın ve yalnızca bir C# işlevi yazma odaklanmanıza olanak tanır. Her zamanki gibi tüm derleme başvurularını ve dosya başına ad alanları içerir. Her şeyi bir ad alanı ve sınıf kaydırma yerine, yalnızca tanımlayan bir `Run` yöntemi. Tüm sınıflar, örneğin dahil gerekiyorsa düz eski CLR nesnesi (POCO) nesneleri tanımlamak için bir sınıf aynı dosyanın içine dahil edebilirsiniz.   

## <a name="binding-to-arguments"></a>Bağımsız değişkenler bağlama
Çeşitli bağlamaları bir C# işlevi bağlı `name` özelliğinde *function.json* yapılandırma. Her bağlama desteklenen türlerinden; yine de sahip istiyor musunuz? Örneğin, bir blob tetikleyici bir dize, bir POCO veya bir CloudBlockBlob destekleyebilir. Desteklenen türler her bağlama başvurusunu belgelenmiştir. Bir POCO nesnesi bir'Set ' yordamı her bir özellik için tanımlanmış olması gerekir.

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

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="using-method-return-value-for-output-binding"></a>Yöntemin dönüş değeri için çıktı bağlama işlemini kullanma

Adını kullanarak bir yöntemin dönüş değeri bir çıktı bağlaması için kullanabileceğiniz `$return` içinde *function.json*:

```json
{
    "type": "queue",
    "direction": "out",
    "name": "$return",
    "queueName": "outqueue",
    "connection": "MyStorageConnectionString",
}
```

```csharp
public static string Run(string input, TraceWriter log)
{
    return input;
}
```

## <a name="writing-multiple-output-values"></a>Birden çok çıktı değerleri yazılıyor

Bir çıkış bağlaması birden çok değeri yazmak için kullanın [ `ICollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [ `IAsyncCollector` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs) türleri. Bu tür yöntemi tamamlandığında, çıkış bağlama yazılan salt yazılır koleksiyonlarıdır.

Bu örnek, aynı kullanarak sıra birden çok sıraya ileti yazar `ICollector`:

```csharp
public static void Run(ICollector<string> myQueueItem, TraceWriter log)
{
    myQueueItem.Add("Hello");
    myQueueItem.Add("World!");
}
```

## <a name="logging"></a>Günlüğe kaydetme
Çıkış akış günlüklerinizi C# oturum açmak için türünde bir bağımsız değişken dahil `TraceWriter`. Bu ad öneririz `log`. Kullanmaktan kaçının `Console.Write` Azure işlevlerinde. 

`TraceWriter`tanımlanan [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/TraceWriter.cs). Günlük düzeyi için `TraceWriter` yapılandırılabilir [host.json](functions-host-json.md).

```csharp
public static void Run(string myBlob, TraceWriter log)
{
    log.Info($"C# Blob trigger function processed: {myBlob}");
}
```

## <a name="async"></a>Zaman uyumsuz
Bir işlev zaman uyumsuz hale getirmek için kullanmak `async` anahtar sözcüğü ve return bir `Task` nesnesi.

```csharp
public async static Task ProcessQueueMessageAsync(
        string blobName,
        Stream blobInput,
        Stream blobOutput)
{
    await blobInput.CopyToAsync(blobOutput, 4096, token);
}
```

## <a name="cancellation-token"></a>İptal belirteci
Bazı işlemler normal şekilde kapatılmasını gerektirir. Her zaman kilitlenen işleyebilir kod yazmak en iyi olmakla birlikte, istediğiniz durumlarda istekleri normal şekilde kapatılmasını işlemek için tanımladığınız bir [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) bağımsız değişken belirtilmiş.  A `CancellationToken` bir ana bilgisayar kapatma tetiklenir göstermek için sağlanmıştır.

```csharp
public async static Task ProcessQueueMessageAsyncCancellationToken(
        string blobName,
        Stream blobInput,
        Stream blobOutput,
        CancellationToken token)
    {
        await blobInput.CopyToAsync(blobOutput, 4096, token);
    }
```

## <a name="importing-namespaces"></a>Ad alanlarını alma
Ad alanları almanız gerekiyorsa, bu nedenle olarak normal, ile yapabileceğiniz `using` yan tümcesi.

```csharp
using System.Net;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Şu ad alanlarından otomatik olarak içe aktarılır ve bu nedenle isteğe bağlıdır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`

## <a name="referencing-external-assemblies"></a>Dış derlemelere başvurma
Kullanarak Framework derlemeler için başvurular ekleyin `#r "AssemblyName"` yönergesi.

```csharp
#r "System.Web.Http"

using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

public static Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
```

Aşağıdaki derlemeler barındırma ortamı Azure işlevleri tarafından otomatik olarak eklenir:

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

Aşağıdaki derlemeler basit adıyla başvurulabilir (örneğin, `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNet.WebHooks.Common`
* `Microsoft.Azure.NotificationHubs`

## <a name="referencing-custom-assemblies"></a>Özel derlemelere başvurma

Özel bir derlemeyi başvurmak için ya da kullanabilirsiniz bir *paylaşılan* derleme veya *özel* derleme:
- Paylaşılan derlemeler işlevi uygulamasında tüm işlevleri arasında paylaşılır. Özel bir derlemeyi başvurmak için işlevi uygulamanızı derlemeye gibi yüklemeniz bir `bin` işlevi uygulama kök klasöründe. 
- Özel derlemeler verilen işlevin bağlam parçası olan ve dışarıdan farklı sürümlerini destekler. Özel derlemeler karşıya yüklenebilir içinde bir `bin` işlevi dizin klasöründe. Dosya adı gibi kullanarak başvuru `#r "MyAssembly.dll"`. 

İşlev klasörünüze dosyaları karşıya yükleme hakkında daha fazla bilgi için paket yönetimi hakkında aşağıdaki bölümüne bakın.

### <a name="watched-directories"></a>İzlenen dizinleri

İşlev komut dosyasını içeren dizine değişiklikler derlemeler için otomatik olarak izlenen. Diğer dizinlerde derleme değişiklikleri izlemek için bunları Ekle `watchDirectories` listesinde [host.json](functions-host-json.md).

## <a name="using-nuget-packages"></a>NuGet paketlerini kullanma
C# işlevinde NuGet paketlerini kullanmak için karşıya bir *project.json* dosyasını işlevin klasöre işlevi uygulamanın dosya sistemi. İşte bir örnek *project.json* Microsoft.ProjectOxford.Face sürüm 1.1.0 bir başvuru ekler dosyası:

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

Yalnızca .NET Framework 4.6 desteklenmez, bu nedenle olduğundan emin olun, *project.json* dosyayı belirtir `net46` aşağıda gösterildiği gibi.

Karşıya yüklediğiniz zaman bir *project.json* dosya, çalışma zamanı paketleri alır ve paketi derleme başvuruları otomatik olarak ekler. Eklemeniz gerekmez `#r "AssemblyName"` yönergeleri. NuGet paketlerinde tanımlanan türlerin kullanmak için gerekli eklemek `using` deyimleri için *run.csx* dosya. 

NuGet restore işlevleri çalışma zamanı'nda çalışır karşılaştırarak `project.json` ve `project.lock.json`. Varsa dosyaların tarih ve saat Damgalar **sağlamadığı** eşleşme NuGet geri yükleme çalıştırır ve NuGet yüklemeleri paketler güncelleştirilir. Ancak, dosyaların tarih ve saat Damgalar **yapmak** eşleşme, NuGet, bir geri yükleme gerçekleştirmez. Bu nedenle, `project.lock.json` NuGet paket geri yüklemesi atlamak neden olarak kullanılmamalıdır. Kilit dosyası dağıtma önlemek için add `project.lock.json` için `.gitignore` dosya.

Akış özel bir NuGet kullanmak için akışta belirtin bir *Nuget.Config* işlev uygulaması kök dosyasında. Daha fazla bilgi için bkz: [NuGet yapılandırma davranışı](/nuget/consume-packages/configuring-nuget-behavior).

### <a name="using-a-projectjson-file"></a>Project.json dosyası kullanma
1. Azure portalında açma işlevi. Günlükleri sekmesinde paket yükleme çıktısını görüntüler.
2. Project.json dosyası karşıya yüklemek için açıklanan yöntemlerden birini kullanın [işlevi uygulama dosyaları güncelleştirmek nasıl](functions-reference.md#fileupdate) Azure işlevleri Geliştirici Başvurusu konu başlığı.
3. Sonra *project.json* dosyasının yüklendiği, işlevinizi aşağıdaki örnekte gibi bir çıktı günlük akış bakın:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri
Bir ortam değişkeni veya ayar değeri bir uygulamayı almak için `System.Environment.GetEnvironmentVariable`aşağıdaki kod örneğinde gösterildiği gibi:

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
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
```

## <a name="reusing-csx-code"></a>.Csx kodu yeniden kullanma
Sınıfları ve diğer tanımlanan yöntemler kullanabilirsiniz *.csx* dosyalar, *run.csx* dosya. Bunu yapmak için kullanmak `#load` yönergeleri, *run.csx* dosya. Aşağıdaki örnekte, bir günlük yordam adlı `MyLogger` içinde paylaşılan *myLogger.csx* ve içine yüklenen *run.csx* kullanarak `#load` yönergesi:

Örnek *run.csx*:

```csharp
#load "mylogger.csx"

public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Verbose($"Log by run.csx: {DateTime.Now}");
    MyLogger(log, $"Log by MyLogger: {DateTime.Now}");
}
```

Örnek *mylogger.csx*:

```csharp
public static void MyLogger(TraceWriter log, string logtext)
{
    log.Verbose(logtext);
}
```

Paylaşılan kullanarak *.csx* kesinlikle türü, bağımsız değişkenleri bir POCO nesnesi kullanılarak işlevleri arasındaki istediğinizde genel bir desen sağlar. Aşağıdaki basit örnekte, bir HTTP tetikleyicisi ve sıra tetikleyici adlı bir POCO nesne paylaşmak `Order` kesinlikle sipariş veri türü için:

Örnek *run.csx* HTTP tetikleyicisi için:

```cs
#load "..\shared\order.csx"

using System.Net;

public static async Task<HttpResponseMessage> Run(Order req, IAsyncCollector<Order> outputQueueItem, TraceWriter log)
{
    log.Info("C# HTTP trigger function received an order.");
    log.Info(req.ToString());
    log.Info("Submitting to processing queue.");

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

Örnek *run.csx* sıra tetikleyici için:

```cs
#load "..\shared\order.csx"

using System;

public static void Run(Order myQueueItem, out Order outputQueueItem,TraceWriter log)
{
    log.Info($"C# Queue trigger function processed order...");
    log.Info(myQueueItem.ToString());

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

* `#load "mylogger.csx"`işlev klasöründe bir dosya yükler.
* `#load "loadedfiles\mylogger.csx"`işlev klasöründe bulunan bir dosya yükler.
* `#load "..\shared\mylogger.csx"`diğer bir deyişle, işlevi klasör ile aynı düzeyde bir klasörde bulunan bir dosya yükler doğrudan altında *wwwroot*.

`#load` Yönergesi çalışır yalnızca *.csx* (C# betik) dosyaları değil *.cs* dosyaları.

<a name="imperative-bindings"></a> 

## <a name="binding-at-runtime-via-imperative-bindings"></a>Kesinlik temelli bağlamaları aracılığıyla çalışma zamanında bağlama

C# ve diğer .NET dilleri kullanabileceğiniz bir [kesinlik temelli](https://en.wikipedia.org/wiki/Imperative_programming) tersine düzeni, bağlama [ *bildirim temelli* ](https://en.wikipedia.org/wiki/Declarative_programming) bağlama *function.json*. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde kullanışlıdır. Bu desen ile desteklenen girişine bağlamak ve bağlama üzerinde-çalışma sırasında işlevi kodunuzda çıktı.

Aşağıdaki gibi bağlama kesinliği tanımlayın:

- **Sağlamadığı** bir girişe dahil *function.json* , istenen kesinlik temelli bağlamaları için.
- Giriş parametresi geçişinde [ `Binder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Host/Bindings/Runtime/Binder.cs) veya [ `IBinder binder` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IBinder.cs).
- Veri bağlama gerçekleştirmek için aşağıdaki C# düzeni kullanın.

```cs
using (var output = await binder.BindAsync<T>(new BindingTypeAttribute(...)))
{
    ...
}
```

`BindingTypeAttribute`bağlama tanımlayan bir .NET özniteliktir ve `T` , bağlama türü tarafından desteklenen giriş veya çıkış türü. `T`Ayrıca olamaz bir `out` parametre türü (gibi `out JObject`). Örneğin, Mobile Apps Tablo Bağlama destekler çıktı [altı türleri çıktı](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs#L17-L22), ancak yalnızca kullanabilirsiniz [ICollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/ICollector.cs) veya [IAsyncCollector<T> ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/IAsyncCollector.cs)için `T`.

Aşağıdaki kod örneği oluşturur bir [depolama blobu çıktı bağlama](functions-bindings-storage-blob.md#using-a-blob-output-binding) blob ile çalışma zamanında tanımlanan yol sonra Yazar bir dize için blob.

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

[BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs) tanımlar [depolama blobu](functions-bindings-storage-blob.md) giriş veya çıkış bağlamayı ve [TextWriter](https://msdn.microsoft.com/library/system.io.textwriter.aspx) desteklenen çıktı bağlama türü.
Önceki kod örneğinde, kodu işlevi uygulamanın ana depolama hesabı bağlantı dizesi için uygulama ayarı alır (olduğu `AzureWebJobsStorage`). Ekleyerek depolama hesabı için kullanılacak bir özel uygulama ayarını belirtebilirsiniz [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) ve öznitelik diziye geçirme `BindAsync<T>()`. Örneğin,

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

Aşağıdaki tabloda her bağlama türü ve tanımlanmış paketler için .NET öznitelikleri listeler.

> [!div class="mx-codeBreakAll"]
| Bağlama | Öznitelik | Başvuru ekleme |
|------|------|------|
| Cosmos DB | [`Microsoft.Azure.WebJobs.DocumentDBAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.DocumentDB"` |
| Event Hubs | [`Microsoft.Azure.WebJobs.ServiceBus.EventHubAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.Jobs.ServiceBus"` |
| Mobile Apps | [`Microsoft.Azure.WebJobs.MobileTableAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.MobileApps/MobileTableAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.MobileApps"` |
| Notification Hubs | [`Microsoft.Azure.WebJobs.NotificationHubAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.NotificationHubs/NotificationHubAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.NotificationHubs"` |
| Service Bus | [`Microsoft.Azure.WebJobs.ServiceBusAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), [`Microsoft.Azure.WebJobs.ServiceBusAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs) | `#r "Microsoft.Azure.WebJobs.ServiceBus"` |
| Depolama sırası | [`Microsoft.Azure.WebJobs.QueueAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Depolama blobu | [`Microsoft.Azure.WebJobs.BlobAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Depolama tablosu | [`Microsoft.Azure.WebJobs.TableAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TableAttribute.cs), [`Microsoft.Azure.WebJobs.StorageAccountAttribute`](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs) | |
| Twilio | [`Microsoft.Azure.WebJobs.TwilioSmsAttribute`](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Twilio/TwilioSMSAttribute.cs) | `#r "Microsoft.Azure.WebJobs.Extensions.Twilio"` |



## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
