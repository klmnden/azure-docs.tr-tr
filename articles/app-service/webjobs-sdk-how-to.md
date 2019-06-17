---
title: Azure Web işleri SDK'sı - kullanma
description: Web işleri SDK'sı için kod yazma hakkında daha fazla bilgi edinin. Olay temelli bir arka plan Azure hizmetlerini ve üçüncü taraf hizmetleri içindeki verilere erişen işleme işleri oluşturun.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: jeconnoc
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/18/2019
ms.author: glenga
ms.openlocfilehash: 38d8bdfcba48d2080b434ebec192b41f3663ae6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60831801"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay temelli bir arka plan işlemleri için Azure Web işleri SDK'sını kullanma

Bu makalede, Azure Web işleri SDK'sı ile çalışma konusunda rehberlik sağlar. WebJobs ile hemen kullanmaya başlamak için bkz: [olay odaklı arka planda işleme için Azure Web işleri SDK'sı ile çalışmaya başlama](webjobs-sdk-get-started.md). 

## <a name="webjobs-sdk-versions"></a>WebJobs SDK sürümleri

Sürüm 3 arasındaki temel farklılıklar şunlardır. *x* ve sürüm 2. *x* Web işleri SDK'sının:

* Sürüm 3. *x* .NET Core için destek ekler.
* Sürüm 3. *x*, Web işleri SDK'sı tarafından gerekli depolama bağlama uzantısı açıkça yüklemeniz gerekir. Sürüm 2'de. *x*, depolama bağlamaları SDK'sına dahil.
* Visual Studio için .NET Core Araçları (3. *x*) projeler, .NET Framework Araçları farklıdır (2. *x*) projeleri. Daha fazla bilgi için bkz. [geliştirme ve Visual Studio - Azure App Service kullanarak Web işleri dağıtma](webjobs-dotnet-deploy-vs.md).

Mümkün olduğunda, her iki sürüm 3 için örnek verilmiştir. *x* ve sürüm 2. *x*.

> [!NOTE]
> [Azure işlevleri](../azure-functions/functions-overview.md) WebJobs SDK'da yerleşik olarak bulunur ve bu makalede Azure işlevleri belgeleri bazı konulara bağlantılar sağlar. İşlevler ve Web işleri SDK'sı arasında şu farklılıklara dikkat etmelisiniz:
> * Azure işlevleri sürüm 2. *x* karşılık gelen Web işleri SDK'sı sürüm 3. *x*ve Azure işlevleri 1. *x* WebJobs SDK 2'ye karşılık gelir. *x*. Kaynak kodu depoları numaralandırma Web işleri SDK'sını kullanın.
> * Azure işlevleri için örnek kod C# sınıf kitaplıkları, WebJobs SDK kod gibi ihtiyacınız olmayan dışında bir `FunctionName` WebJobs SDK projesindeki özniteliği.
> * Bazı bağlama türleri yalnızca HTTP (Web kancaları) ve (hangi HTTP tabanlı) bir Event Grid gibi işlevler desteklenir.
>
> Daha fazla bilgi için [Azure işlevleri ve WebJobs SDK karşılaştırması](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="webjobs-host"></a>WebJobs konak

Konak, İşlevler için bir çalışma zamanı kapsayıcıdır.  Bu, tetikleyiciler ve çağrıları işlevleri için dinler. Sürüm 3. *x*, ana öğesinin uygulanmasıdır `IHost`. Sürüm 2'de. *x*, kullandığınız `JobHost` nesne. Kodunuzda bir ana bilgisayar örneği oluşturur ve davranışını özelleştirmek için kod yazma.

WebJobs SDK kullanarak doğrudan ve dolaylı olarak Azure işlevleri aracılığıyla kullanarak arasında önemli bir fark budur. Azure işlevleri'nde hizmet konak kontrolü ve kod yazarak konak özelleştiremezsiniz. Azure işlevleri host.json dosyasındaki ayarları aracılığıyla konak davranışını özelleştirmenize olanak sağlar. Bu ayarlar dizelerdir, kod değil ve bu tür özelleştirmeleri yapmak için sınırlar.

### <a name="host-connection-strings"></a>Konak bağlantı dizeleri

WebJobs SDK, Azure'da çalıştırdığınızda, Azure depolama ve Azure Service Bus bağlantı dizeleri, yerel olarak çalıştırdığınızda local.settings.json dosyasında ya da WebJob ortamında arar. Varsayılan olarak, bir depolama bağlantı dizesi ayarı adlı `AzureWebJobsStorage` gereklidir.  

Sürüm 2. *x* Bu bağlantı dizeleri için kendi adlarınızı kullanın veya bunları başka bir yerde depolamanız SDK'sı sağlar. Kod kullanarak adları ayarlayabilirsiniz [ `JobHostConfiguration` ], burada gösterildiği gibi:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    //// Dashboard logging is deprecated; use Application Insights.
    //var _dashboardConn = ConfigurationManager
    //    .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    //config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Çünkü sürüm 3. *x* varsayılan .NET Core yapılandırma API'leri, kullandığı bağlantı dizesini adlarını değiştirmek için hiçbir API yoktur.

### <a name="host-development-settings"></a>Konak geliştirme ayarları

Konak, yerel geliştirme daha verimli hale getirmek için geliştirme modunda çalıştırabilirsiniz. Geliştirme modunda çalıştırdığınızda değişen ayarlardan bazıları şunlardır:

| Özellik | Geliştirme ayarı |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` Günlük çıktısını en üst düzeye çıkarmak için. |
| `Queues.MaxPollingInterval`  | Kuyruk yöntemleri hemen harekete emin olmak için düşük bir değer.  |
| `Singleton.ListenerLockPeriod` | 15 Hızlı yinelemeli geliştirme yardımcı olmak için saniye. |

Geliştirme modunu etkinleştirme işlemi, SDK sürümüne bağlıdır. 

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3. *x* standart ASP.NET Core API'leri kullanır. Çağrı [ `UseEnvironment` ](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) metodunda [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) örneği. Adlı bir dizeyi geçirmek `development`, bu örnekte olduğu gibi:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.UseEnvironment("development");
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

`JobHostConfiguration` Sınıfında bir `UseDevelopmentSettings` geliştirme modunu etkinleştirir yöntemi.  Aşağıdaki örnek, geliştirme ayarlarını nasıl kullanacağınız gösterilmektedir. Yapmak `config.IsDevelopment` dönüş `true` yerel olarak çalıştığında, adlı bir yerel ortam değişkenini ayarlamak `AzureWebJobsEnv` değerle `Development`.

```cs
static void Main()
{
    config = new JobHostConfiguration();

    if (config.IsDevelopment)
    {
        config.UseDevelopmentSettings();
    }

    var host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-servicepointmanager-settings"></a>Eş zamanlı bağlantıları yönetme (sürüm 2. *x*)

Sürüm 3. *x*, bağlantı sınırı varsayılan olarak sonsuz bağlantıları. Bazı nedenlerden dolayı bu sınırı değiştirmek istiyorsanız, kullanabileceğiniz [ `MaxConnectionsPerServer` ](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) özelliği [ `WinHttpHandler` ](/dotnet/api/system.net.http.winhttphandler) sınıfı.

Sürüm 2'de. *x*, kullanarak bir konak için eş zamanlı bağlantı sayısını denetleme [ServicePointManager.DefaultConnectionLimit](/dotnet/api/system.net.servicepointmanager.defaultconnectionlimit#System_Net_ServicePointManager_DefaultConnectionLimit) API. 2\. *x*, WebJobs konağınız başlatmadan önce bu varsayılan 2 değerini artırmanız gerekir.

Bir işlevden kullanarak yaptığınız tüm giden HTTP isteklerini `HttpClient` akışına `ServicePointManager`. İçinde ayarlanan değere ulaştığında `DefaultConnectionLimit`, `ServicePointManager` göndermeden önce sıraya alma isteği başlatır. Varsayalım, `DefaultConnectionLimit` 2 ve, kod yapar 1.000 HTTP isteği ayarlayın. Başlangıçta, yalnızca iki isteği için işletim sistemi aracılığıyla izin verilir. Diğer 998 kuyruğa oluncaya kadar bunları yer. Yani, `HttpClient` istek yaptığınız göründüğünden, zaman aşımı olabilir, ancak istek hedef sunucuya işletim sistemi tarafından hiçbir zaman gönderildi. Anlamlı yaramadı davranışını görebilirsiniz: yerel `HttpClient` bir isteğin tamamlanması için 10 saniye sürecek ancak hizmetinizi 200 ms içinde her istek döndürüyor. 

ASP.NET uygulamaları için varsayılan değer `Int32.MaxValue`, ve iyi bir temel veya daha yüksek App Service planı içinde çalışan WebJobs için çalışma olasılığı olan. Yalnızca temel ve daha yüksek App Service planları tarafından desteklenen ve Web işleri genellikle her zaman açık ayar gerekir.

WebJob'ınıza bir ücretsiz veya paylaşılan App Service planı içinde çalışıyorsa, uygulamanız şu anda sahip App Service korumalı alanı tarafından kısıtlı bir [bağlantı sınırı 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). İlişkisiz bağlantı sınır içinde `ServicePointManager`, korumalı alan bağlantı eşiği ulaştı ve site kapanacak daha olasıdır. Bu durumda, ayarı `DefaultConnectionLimit` şeye 50 veya 100 ' gibi daha düşük Bunun gerçekleşmesini önlemek ve yine de yeterli aktarım hızı için izin verebilirsiniz.

Ayar, herhangi bir HTTP isteğinin yapılmadan önce yapılandırılmalıdır. Bu nedenle, Web işleri ana bilgisayar ayarı olmamalıdır otomatik olarak. Beklenmeyen bir davranışa neden olabilir ana bilgisayar başlamadan önce meydana gelen HTTP isteklerini olabilir. Hemen değeri ayarlamak için en iyi yaklaşımdır, `Main` başlatma önce yöntemi `JobHost`, burada gösterildiği gibi:

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it's used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Tetikleyiciler

İşlevler genel yöntemleri olmalıdır ve bir tetikleyici özniteliği olmalıdır veya [ `NoAutomaticTrigger` ](#manual-triggers) özniteliği.

### <a name="automatic-triggers"></a>Otomatik Tetikleyiciler

Otomatik tetikleyiciler, bir olaya yanıt olarak bir işlev çağırın. Azure kuyruk depolamaya eklenen bir ileti tarafından tetiklenen bir işlev bu örneği göz önünde bulundurun. Bir blobu Azure Blob depolama alanından okunmasını tarafından yanıt verir:

```cs
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{myQueueItem}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

`QueueTrigger` Özniteliği söyleyen bir kuyruk iletisi göründüğünde işlevi çağırmak için çalışma zamanı `myqueue-items` kuyruk. `Blob` Özniteliği söyleyen bir blobun okunabilmesi için kuyruk iletisi kullanılacak çalışma zamanı *örnek-workitems* kapsayıcı. İşlev için kuyruk iletisinin içeriği geçirilen `myQueueItem` parametredir, blob adı.


### <a name="manual-triggers"></a>El ile tetikleyici

Bir işlev el ile tetiklemek için kullanmak `NoAutomaticTrigger` burada gösterildiği gibi öznitelik:

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
ILogger logger,
string value,
[Queue("outputqueue")] out string message)
{
    message = value;
    logger.LogInformation("Creating queue message: ", message);
}
```

İşlev el ile tetikleme işlemi, SDK sürümüne bağlıdır.

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage();
    });
    var host = builder.Build();
    using (host)
    {
        var jobHost = host.Services.GetService(typeof(IJobHost)) as JobHost;
        var inputs = new Dictionary<string, object>
        {
            { "value", "Hello world!" }
        };

        await host.StartAsync();
        await jobHost.CallAsync("CreateQueueMessage", inputs);
        await host.StopAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Giriş ve çıkış bağlamaları

Giriş bağlamaları, verileri Azure veya üçüncü taraf hizmetleri kodunuza kullanılabilmesi için bildirim temelli bir yöntemini sağlar. Çıkış bağlamaları, verileri güncelleştirmek için bir yol sağlar. [Başlama](webjobs-sdk-get-started.md) makalede her bir örneği gösterilmektedir.

Yöntemin dönüş değerini özniteliği uygulayarak, bir yöntemin dönüş değeri bir çıkış bağlaması için kullanabilirsiniz. Örnekte bakın [dönüş değeri Azure işlevini kullanarak](../azure-functions/functions-bindings-return-value.md).

## <a name="binding-types"></a>Bağlama türü

Yükleme ve bağlama türlerini yönetmek için bir işlem olup olmadığını, sürüm 3 kullandığınıza bağlıdır. *x* veya sürüm 2. *x* SDK. Özel bağlama türü için Azure işlevleri bağlama türün "Paketler" bölümünde yüklemek için paketi bulabilirsiniz [başvurusu makalesinde](#binding-reference-information). Dosyaları tetikleyici ve Azure işlevleri tarafından desteklenmeyen bağlama (yerel dosya sistemi) için bir özel durumdur.

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3. *x*, depolama bağlamaları dahil `Microsoft.Azure.WebJobs.Extensions.Storage` paket. Çağrı `AddAzureStorage` uzantı yönteminde `ConfigureWebJobs` burada gösterildiği gibi yöntemi:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddAzureStorage();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Diğer tetikleyici ve bağlama türlerini kullanmak için bunları içeren NuGet paketini yüklemek ve çağrı `Add<binding>` uzantı uygulanan genişletme yöntemi. Örneğin, bir Azure Cosmos DB bağlama kullanmak istiyorsanız, yükleme `Microsoft.Azure.WebJobs.Extensions.CosmosDB` ve çağrı `AddCosmosDB`, şöyle:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddCosmosDB();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

Zamanlayıcı tetikleyicisi veya dosyaları kullanmanız için bağlama, çekirdek hizmetlerin bir parçası olan, çağrı `AddTimers` veya `AddFiles` genişletme yöntemleri, sırasıyla.

#### <a name="version-2x"></a>Sürüm 2. *x*

Bu tetikleyici ve bağlama türlerini sürüm 2 dahil edilir. *x* , `Microsoft.Azure.WebJobs` paket:

* Blob depolama
* Kuyruk depolama
* Table Storage

Diğer tetikleyici ve bağlama türlerini kullanmak için bunları içeren NuGet paketini yüklemek ve çağırmak bir `Use<binding>` metodunda `JobHostConfiguration` nesne. Örneğin, bir zamanlayıcı tetikleyicisi kullanmak istiyorsanız, yükleme `Microsoft.Azure.WebJobs.Extensions` ve çağrı `UseTimers` içinde `Main` burada gösterildiği gibi yöntemi:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Dosyaları kullanmak için bağlama, yükleme `Microsoft.Azure.WebJobs.Extensions` ve çağrı `UseFiles`.

### <a name="executioncontext"></a>ExecutionContext

WebJobs bağlamanıza olanak tanır bir [ `ExecutionContext` ]. Bu bağlama ile erişebileceğiniz [ `ExecutionContext` ] işlev imzası, parametre olarak. Örneğin, aşağıdaki kod, verilen işlevi çağrısı tarafından üretilen tüm günlükler ilişkilendirmek için kullanabileceğiniz çağırma kimliği erişmek için bağlam nesnesini kullanır.  

```cs
public class Functions
{
    public static void ProcessQueueMessage([QueueTrigger("queue")] string message,
        ExecutionContext executionContext,
        ILogger logger)
    {
        logger.LogInformation($"{message}\n{executionContext.InvocationId}");
    }
}
```

Bağlama için işlem [ `ExecutionContext` ] , SDK sürümüne bağlıdır.

#### <a name="version-3x"></a>Sürüm 3. *x*

Çağrı `AddExecutionContextBinding` uzantı yönteminde `ConfigureWebJobs` burada gösterildiği gibi yöntemi:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
            {
                b.AddAzureStorageCoreServices();
                b.AddExecutionContextBinding();
            });
    var host = builder.Build();
    using (host)
    {
        host.Run();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

`Microsoft.Azure.WebJobs.Extensions` Daha önce bahsedilen paketi ayrıca çağırarak kaydedebileceğiniz bir özel bağlama türü sağlar `UseCore` yöntemi. Bu bağlama, tanımlamanıza olanak sağlar. bir [ `ExecutionContext` ] şöyle etkin olan işlev imzası, parametre:

```cs
class Program
{
    static void Main()
    {
        config = new JobHostConfiguration();
        config.UseCore();
        var host = new JobHost(config);
        host.RunAndBlock();
    }
}
```

## <a name="binding-configuration"></a>Bağlama yapılandırması

Bazı Tetikleyicileri ve bağlamaları davranışını yapılandırabilirsiniz. Bunları yapılandırma işlemi, SDK sürümüne bağlıdır.

* **Sürüm 3. *x*:** Küme yapılandırması, `Add<Binding>` yöntemi çağrıldığında `ConfigureWebJobs`.
* **Sürüm 2. *x*:** Kümesi yapılandırması için geçirdiğiniz bir yapılandırma nesnesi içinde özelliklerini ayarlayarak `JobHost`.

Bu bağlama özgü ayarları ayarlarında eşdeğer [host.json proje dosyası](../azure-functions/functions-host-json.md) Azure işlevleri'nde.

Aşağıdaki bağlamaları yapılandırabilirsiniz:

* [Azure CosmosDB tetikleyicisi](#azure-cosmosdb-trigger-configuration-version-3x)
* [Olay hub'ları tetikleme](#event-hubs-trigger-configuration-version-3x)
* Kuyruk depolama tetikleyicisi
* [SendGrid bağlama](#sendgrid-binding-configuration-version-3x)
* [Service Bus tetikleyicisi](#service-bus-trigger-configuration-version-3x)

### <a name="azure-cosmosdb-trigger-configuration-version-3x"></a>Azure CosmosDB tetikleyici yapılandırması (sürüm 3. *x*)

Bu örnek, Azure Cosmos DB tetikleyicisi yapılandırma işlemi gösterilmektedir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddCosmosDB(a =>
        {
            a.ConnectionMode = ConnectionMode.Gateway;
            a.Protocol = Protocol.Https;
            a.LeaseOptions.LeasePrefix = "prefix1";

        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Daha fazla ayrıntı için [Azure CosmosDB bağlama](../azure-functions/functions-bindings-cosmosdb-v2.md#hostjson-settings) makalesi.

### <a name="event-hubs-trigger-configuration-version-3x"></a>Olay hub'ları tetiklemek yapılandırma (sürüm 3. *x*)

Bu örnekte, Event Hubs tetikleyici yapılandırma işlemi gösterilmektedir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddEventHubs(a =>
        {
            a.BatchCheckpointFrequency = 5;
            a.EventProcessorOptions.MaxBatchSize = 256;
            a.EventProcessorOptions.PrefetchCount = 512;
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Daha fazla ayrıntı için [Event Hubs bağlama](../azure-functions/functions-bindings-event-hubs.md#hostjson-settings) makalesi.

### <a name="queue-storage-trigger-configuration"></a>Kuyruk depolama tetikleyici yapılandırması

Bu örnekler, kuyruk depolama tetikleyicisi yapılandırma işlemini gösterir:

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddAzureStorage(a => {
            a.BatchSize = 8;
            a.NewBatchThreshold = 4;
            a.MaxDequeueCount = 4;
            a.MaxPollingInterval = TimeSpan.FromSeconds(15);
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Daha fazla ayrıntı için [kuyruk depolama bağlama](../azure-functions/functions-bindings-storage-queue.md#hostjson-settings) makalesi.

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.Queues.BatchSize = 8;
    config.Queues.NewBatchThreshold = 4;
    config.Queues.MaxDequeueCount = 4;
    config.Queues.MaxPollingInterval = TimeSpan.FromSeconds(15);
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Daha fazla ayrıntı için [host.json v1.x başvurusu](../azure-functions/functions-host-json-v1.md#queues).

### <a name="sendgrid-binding-configuration-version-3x"></a>SendGrid bağlama yapılandırması (sürüm 3. *x*)

Bu örnekte, SendGrid yapılandırma işlemi gösterilmektedir çıktı bağlaması:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddSendGrid(a =>
        {
            a.FromAddress.Email = "samples@functions.com";
            a.FromAddress.Name = "Azure Functions";
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Daha fazla ayrıntı için [SendGrid bağlama](../azure-functions/functions-bindings-sendgrid.md#hostjson-settings) makalesi.

### <a name="service-bus-trigger-configuration-version-3x"></a>Service Bus tetikleyicisi yapılandırma (sürüm 3. *x*)

Bu örnekte, Service Bus tetiği yapılandırma işlemi gösterilmektedir:

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddServiceBus(sbOptions =>
        {
            sbOptions.MessageHandlerOptions.AutoComplete = true;
            sbOptions.MessageHandlerOptions.MaxConcurrentCalls = 16;
        });
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Daha fazla ayrıntı için [Service Bus bağlama](../azure-functions/functions-bindings-service-bus.md#hostjson-settings) makalesi.

### <a name="configuration-for-other-bindings"></a>Diğer bağlantılar için yapılandırma

Bazı tetikleyici ve bağlama türleri, kendi özel yapılandırma türleri tanımlar. Örneğin, dosya tetikleyici, bu örneklerde olduğu gibi izlemek için kök yolu belirtmenize olanak tanır:

#### <a name="version-3x"></a>Sürüm 3. *x*

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
        b.AddFiles(a => a.RootPath = @"c:\data\import");
    });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

```cs
static void Main()
{
    config = new JobHostConfiguration();
    var filesConfig = new FilesConfiguration
    {
        RootPath = @"c:\data\import"
    };
    config.UseFiles(filesConfig);
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

## <a name="binding-expressions"></a>Bağlama ifadeleri

Öznitelik oluşturucu parametresi, çeşitli kaynaklardan gelen değerlerine çözmek ifadeleri kullanabilirsiniz. Örneğin, aşağıdaki kodda, yolunu `BlobTrigger` öznitelik adı bir ifade oluşturur `filename`. Çıkış bağlaması için kullanıldığında `filename` tetikleme blob adı için çözümler.

```cs
public static void CreateThumbnail(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger logger)
{
    logger.Info($"Blob trigger processing: {filename}");
    // ...
}
```

Bağlama ifadeleri hakkında daha fazla bilgi için bkz: [ifadeleri ve desenleri bağlama](../azure-functions/functions-bindings-expressions-patterns.md) Azure işlevleri belgelerinde.

### <a name="custom-binding-expressions"></a>Özel bağlama ifadeleri

Bazen bir kuyruk adı, bir blob adı veya kapsayıcı ya da bir tablo adı kod yerine sabit kodlama belirtmek istersiniz. Örneğin, kuyruk adı belirtmek isteyebilirsiniz `QueueTrigger` bir yapılandırma dosyası veya ortam değişkeni özniteliği.

Geçirerek bunu yapabilirsiniz bir `NameResolver` için nesnesine `JobHostConfiguration` nesne. Tetikleyicisi veya bağlaması öznitelik oluşturucu parametresi, yer tutucu karakterleri içeren ve `NameResolver` kod, gerçek değerleri yerine bu yer tutucuları kullanılacak sağlar. Yüzde (%) ile çevreleyen tarafından yer tutucuları tanımlayın burada gösterildiği gibi işaretlere:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Bu kod adında bir kuyruk kullanmanıza olanak tanıyan `logqueuetest` test ortamı ve bir adlı `logqueueprod` üretimde. Sabit kodlanmış kuyruk adı yerine bir giriş adını belirtin `appSettings` koleksiyonu.

Bir varsayılan `NameResolver` özel bir sağlamıyorsa etkisi alan. Varsayılan değerleri uygulama ayarları veya ortam değişkenlerini alır.

`NameResolver` Sınıfı alır kuyruk adı `appSettings`, burada gösterildiği gibi:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Sürüm 3. *x*

Çözümleyici, bağımlılık ekleme kullanılarak yapılandırın. Bu örnekleri aşağıdaki gerektiren `using` deyimi:

```cs
using Microsoft.Extensions.DependencyInjection;
```

Çağırarak çözümleyici Ekle [ `ConfigureServices` ] genişletme yöntemini [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder), bu örnekte olduğu gibi:

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    var resolver = new CustomNameResolver();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureServices(s => s.AddSingleton<INameResolver>(resolver));
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Geçirmek, `NameResolver` için sınıfını `JobHost` burada gösterildiği gibi nesne:

```cs
 static void Main(string[] args)
{
    JobHostConfiguration config = new JobHostConfiguration();
    config.NameResolver = new CustomNameResolver();
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

Azure işlevleri uygulayan `INameResolver` örnekte gösterildiği gibi değerleri uygulama ayarları ' almak için. WebJobs SDK doğrudan kullandığınızda, yer tutucu değerleri tercih ettiğiniz herhangi bir kaynaktan alır. özel bir uygulama yazabilirsiniz.

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

Gibi bir bağlama özniteliği kullanmadan önce bazı çalışma işlevinizde yapmanız gerekiyorsa `Queue`, `Blob`, veya `Table`, kullanabileceğiniz `IBinder` arabirimi.

Aşağıdaki örnek, bir giriş sırası iletiyi alır ve bir çıkış sırasına aynı içeriğe sahip yeni bir ileti oluşturur. Çıkış kuyruğu adı işlevinin gövdesindeki kod tarafından ayarlanır.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessageAsync(new CloudQueueMessage(queueMessage));
}
```

Daha fazla bilgi için [çalışma zamanında bağlama](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) Azure işlevleri belgelerinde.

## <a name="binding-reference-information"></a>Bağlama başvuru bilgileri

Azure işlevleri belgelerinde her bağlama türü hakkında başvuru bilgileri sağlar. Her bağlama başvuru makalesinde, aşağıdaki bilgileri bulabilirsiniz. (Bu örnekte depolama kuyruğu üzerinde temel alınmıştır.)

* [Paketleri](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x). Paket bir Web işleri SDK'sı projesinde bağlama için destek eklemek için yüklemeniz gerekir.
* [Örnekler](../azure-functions/functions-bindings-storage-queue.md#trigger---example). Kod örnekleri. C# Sınıfı kitaplık örneği için Web işleri SDK'sı geçerlidir. Yalnızca atlamak `FunctionName` özniteliği.
* [Öznitelikleri](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes). Bağlama türü için kullanılacak öznitelikler.
* [Yapılandırma](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration). Öznitelik özelliklerini ve oluşturucu parametrelerinin açıklamaları.
* [Kullanım](../azure-functions/functions-bindings-storage-queue.md#trigger---usage). Bağlamanın nasıl çalıştığı hakkında bilgi ve türleri bağlayabilirsiniz. Örneğin: algoritma yoklama, zehirli kuyruk işleme.
  
Başvuru makalelerimize bağlama bir listesi için bkz: "Desteklenen bağlamaları" [Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#supported-bindings) makale için Azure işlevleri. Bu listede, HTTP Web kancası ve Event Grid bağlamaları, yalnızca WebJobs SDK tarafından değil, Azure işlevleri tarafından desteklenir.

## <a name="disable-attribute"></a>Öznitelik devre dışı bırak 

[ `Disable` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) Bir işlev olup olmadığını denetlemenizi öznitelik sağlar tetiklendi. 

Aşağıdaki örnekte, uygulama ayarının `Disable_TestJob` değeri `1` veya `True` işlevi (büyük/küçük harfe duyarlı) çalışmaz. Bu durumda, çalışma zamanı bir günlük iletisi oluşturur *işlevi 'Functions.TestJob' devre dışı*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Azure portalında uygulama ayar değerleri değiştirdiğinizde, webjob'ı yeni ayarlama alacak şekilde yeniden başlatır.

Öznitelik parametre, yöntemi veya sınıf düzeyinde bildirilebilir. Ayar adı bağlama ifadeleri de içerebilir.

## <a name="timeout-attribute"></a>Zaman aşımı özniteliği

[ `Timeout` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) Öznitelik, bir işlev, belirtilen bir süre içinde tamamlanmazsa iptal edilmesine neden olur. Aşağıdaki örnekte, işlev, zaman aşımı özniteliği olmadan bir gün çalışır. Zaman aşımı işlevi 15 saniye sonra iptal edilmesine neden olur.

```cs
[Timeout("00:00:15")]
public static async Task TimeoutJob(
    [QueueTrigger("testqueue2")] string message,
    CancellationToken token,
    TextWriter log)
{
    await log.WriteLineAsync("Job starting");
    await Task.Delay(TimeSpan.FromDays(1), token);
    await log.WriteLineAsync("Job completed");
}
```

Zaman aşımı özniteliğini sınıfta veya yöntemde düzeyinde uygulayabilir ve genel zaman aşımı kullanarak belirtebilirsiniz `JobHostConfiguration.FunctionTimeout`. Sınıf düzeyinde veya yöntem düzeyi zaman aşımları, genel zaman aşımı geçersiz.

## <a name="singleton-attribute"></a>Singleton özniteliği

[ `Singleton` ](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) Özniteliği çalıştığında, konak web uygulamasının birden fazla örneği bulunduğunda bile bir işlevi yalnızca bir örneğini sağlar. Bunu kullanarak yapar [kilitleme dağıtılmış](#viewing-lease-blobs).

Bu örnekte, yalnızca tek bir örneğini `ProcessImage` işlevi herhangi bir zamanda çalıştırır:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image.
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Bazı tetikleyici eşzamanlılık yönetimi için yerleşik desteği vardır:

* **QueueTrigger**. Ayarlama `JobHostConfiguration.Queues.BatchSize` için `1`.
* **ServiceBusTrigger**. Ayarlama `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` için `1`.
* **FileTrigger**. Ayarlama `FileProcessor.MaxDegreeOfParallelism` için `1`.

Bu ayarlar, işlevinizi tekil olarak tek bir örneği üzerinde çalıştığından emin olmak için kullanabilirsiniz. İşlevi yalnızca tek bir örneği için birden fazla web uygulamasını kullanıma ölçeklendirildiğinde çalıştığından emin olmak için bir dinleyici düzeyi tekil işlev kilitlemek (`[Singleton(Mode = SingletonMode.Listener)]`). JobHost başladığında dinleyici kilitler elde edilen. Tüm üç genişletilmiş örnekleri aynı anda başlatırsanız, yalnızca bir örneği kilidi alır ve yalnızca bir dinleyicisi başlatır.

### <a name="scope-values"></a>Kapsam değerleri

Belirtebileceğiniz bir *kapsam ifadesi/değer* tek üzerinde. Belirli bir kapsamda işlevinin tüm yürütmeleri serileştirilecek ifade/değeri sağlar. Daha ayrıntılı bu şekilde kilitlemek uygulama için belirli bir düzeyde işleviniz için paralellik derecesini gereksinimlerinizi tarafından belirlenen diğer çağrılarını serileştirirken izin verebilirsiniz. Örneğin, aşağıdaki kodda, kapsam ifadesi bağlar `Region` gelen iletinin değeri. Sıranın Doğu, Doğu ve Batı sırasıyla bölgede üç iletileri içerdiğinde, bölge Doğu seri olarak çalıştırılır iletileri Batı Doğu değerlerle paralel çalıştırılan ileti bölge ile çalışırken.

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item.
}

public class WorkItem
{
     public int ID { get; set; }
     public string Region { get; set; }
     public int Category { get; set; }
     public string Description { get; set; }
}
```

### <a name="singletonscopehost"></a>SingletonScope.Host

Kilidi için varsayılan kapsamı `SingletonScope.Function`, yani kilit kapsamı (blob kiralama yol) tam işlev adını bağlı. İşlevlerinin kilitlemek için belirtin `SingletonScope.Host` ve aynı anda çalıştırmak istemediğiniz tüm işlevlerinin aynı olan bir kapsam kimliği adı kullanın. Aşağıdaki örnekte, yalnızca bir örneğini `AddItem` veya `RemoveItem` teker teker çalıştırılır:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation.
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation.
}
```

### <a name="viewing-lease-blobs"></a>İzleme kira BLOB'ları

Web işleri SDK'sı kullanır [Azure blob kiralama](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) altında dağıtılmış kilitleme uygulamak için kapsar. Singleton tarafından kullanılan kira bloblar bulunabilir `azure-webjobs-host` kapsayıcısında `AzureWebJobsStorage` "kilitleri" yolunda depolama hesabı. Örneğin, ilk kira blob yolunu `ProcessImage` daha önce gösterilen örnek olabilir `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Tüm yolları Bu büyük/küçük harf 061851c758f04938a4426aa9ab3869c0 JobHost Kimliğini içerir.

## <a name="async-functions"></a>Zaman uyumsuz işlevleri

Kod zaman uyumsuz işlevleri hakkında daha fazla bilgi için bkz. [Azure işlevleri belgelerinde](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>İptal belirteçleri

İptal belirteçleri işlemek nasıl hakkında daha fazla bilgi için Azure işlevleri belgelerinde bakın [iptal belirteçleri ve normal şekilde kapatılmasını](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Birden çok örnek

Web uygulamanız birden çok örnek üzerinde çalışıyorsa, sürekli bir WebJob Tetikleyiciler için dinleme ve işlevleri çağırma her örneğinde çalışır. Çeşitli tetikleyici bağlamaları verimli bir şekilde iş örnekleri arasında işbirliği içinde paylaşmak için tasarlanmış daha fazla örnekleri için ölçek genişletme tanır; böylece, daha fazla yükü işlemek için.

Kuyruk ve blob Tetikleyicileri otomatik olarak bir işleve bir kuyruk iletisi işlenmesini önlemek veya birden çok kez blob; İşlevler, etkili olması gerekmez.

Belirli bir zamanlanan saatte çalıştıran birden fazla işlev örneği elde Zamanlayıcı tetikleyicisi otomatik olarak Zamanlayıcıyı çalıştıran, yalnızca bir örneğini sağlar.

Bu yalnızca bir örneğini sağlamak istiyorsanız bile konak web uygulamasının birden fazla örneği bulunduğunda, bir işlevi çalıştırır, kullanabileceğiniz [ `Singleton` ](#singleton-attribute) özniteliği.

## <a name="filters"></a>Filtreler

İşlev filtreleri (Önizleme), kendi mantığınızı WebJobs yürütme hattıyla özelleştirmek için bir yol sağlar. Filtreler benzer [ASP.NET Core filtreler](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). İşlevler veya sınıflar uygulanır bildirim temelli öznitelikleri uygulayabilirsiniz. Daha fazla bilgi için [işlevi filtreleri](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

ASP.NET için geliştirilmiş günlüğe kaydetme çerçevesi öneririz. [Başlama](webjobs-sdk-get-started.md) makalesi, nasıl kullanılacağını gösterir. 

### <a name="log-filtering"></a>Günlük Filtresi

Tarafından oluşturulan her günlük bir `ILogger` örneği ilişkili bir `Category` ve `Level`. [`LogLevel`](/dotnet/api/microsoft.extensions.logging.loglevel) bir sabit listesidir ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|None        | 6 |

Her kategori için belirli bir bağımsız olarak filtreleyebilirsiniz [ `LogLevel` ](/dotnet/api/microsoft.extensions.logging.loglevel). Örneğin, blob tetikleyicisi yalnızca işleme için tüm günlükleri görmek isteyebilirsiniz `Error` ve diğer her şey için daha yüksek.

#### <a name="version-3x"></a>Sürüm 3. *x*

Sürüm 3. *x* SDK'sı .NET Core yerleşik filtreleme üzerinde kullanır. `LogCategories` Sınıfı belirli İşlevler, Tetikleyiciler veya kullanıcılar için kategorileri tanımlamanıza olanak sağlar. Ayrıca belirli konak durumları için filtreler gibi tanımlar `Startup` ve `Results`. Bu, günlük çıktısı ince ayar yapmanıza olanak sağlar. İçinde tanımlanan kategorilere eşleşme bulunursa, filtre geri döner `Default` ileti filtresi karar verirken değeri.

`LogCategories` Aşağıdaki gerektirir using deyimi:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

Aşağıdaki örnek, varsayılan olarak, tüm günlükleri filtreleyen bir filtresi oluşturur `Warning` düzeyi. `Function` Ve `results` kategorileri (eşdeğer `Host.Results` sürüm 2'deki. *x*), filtre `Error` düzeyi. Geçerli kategori tüm kayıtlı düzeylerine filtresi karşılaştırır `LogCategories` örnek ve en uzun eşleşmeyi seçer. Diğer bir deyişle `Debug` düzeyi için kayıtlı `Host.Triggers` eşleşen `Host.Triggers.Queue` veya `Host.Triggers.Blob`. Bu, her birini eklemek gerek kalmadan daha geniş kategorileri denetlemenizi sağlar.

```cs
static async Task Main(string[] args)
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging(logging =>
            {
                logging.SetMinimumLevel(LogLevel.Warning);
                logging.AddFilter("Function", LogLevel.Error);
                logging.AddFilter(LogCategories.CreateFunctionCategory("MySpecificFunctionName"),
                    LogLevel.Debug);
                logging.AddFilter(LogCategories.Results, LogLevel.Error);
                logging.AddFilter("Host.Triggers", LogLevel.Debug);
            });
    var host = builder.Build();
    using (host)
    {
        await host.RunAsync();
    }
}
```

#### <a name="version-2x"></a>Sürüm 2. *x*

Sürüm 2'de. *x* kullandığınız SDK'sının `LogCategoryFilter` filtreleme denetlemek için sınıf. `LogCategoryFilter` Sahip bir `Default` başlangıç değeri özelliğiyle `Information`, herhangi bir iletisi güncelleştirmeyeceği `Information`, `Warning`, `Error`, veya `Critical` düzeyleri kaydedilir, ancak tüm iletiler `Debug` veya `Trace` düzeyleri hemen filtrelenir.

Olduğu gibi `LogCategories` sürüm 3. *x*, `CategoryLevels` özelliği, günlük çıktısı ayarlayabilmek belirli kategorileri için günlük düzeyleri belirtmenize olanak sağlar. İçinde hiçbir eşleşme bulunursa `CategoryLevels` sözlük, filtre gördükleri `Default` ileti filtresi karar verirken değeri.

Aşağıdaki örnek, varsayılan olarak tüm günlükleri filtreleyen bir filtre yapıları `Warning` düzeyi. `Function` Ve `Host.Results` kategorileri filtrelenir `Error` düzeyi. `LogCategoryFilter` Kaydedilen tüm geçerli kategoriye karşılaştırır `CategoryLevels` ve en uzun eşleşmeyi seçer. Bu nedenle `Debug` düzeyi için kayıtlı `Host.Triggers` eşleşecektir `Host.Triggers.Queue` veya `Host.Triggers.Blob`. Bu, her birini eklemek gerek kalmadan daha geniş kategorileri denetlemenizi sağlar.

```csharp
var filter = new LogCategoryFilter();
filter.DefaultLevel = LogLevel.Warning;
filter.CategoryLevels[LogCategories.Function] = LogLevel.Error;
filter.CategoryLevels[LogCategories.Results] = LogLevel.Error;
filter.CategoryLevels["Host.Triggers"] = LogLevel.Debug;

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(instrumentationKey, filter.Filter)
    .AddConsole(filter.Filter);
```

### <a name="custom-telemetry-for-application-insights"></a>Application ınsights özel telemetri

Özel telemetri için uygulama işlemi [Application Insights](../azure-monitor/app/app-insights-overview.md) SDK sürümüne bağlıdır. Application Insights yapılandırma konusunda bilgi için bkz: [Application Insights Ekle günlük](webjobs-sdk-get-started.md#add-application-insights-logging).

#### <a name="version-3x"></a>Sürüm 3. *x*

Çünkü sürüm 3. *x* genel ana bilgisayar, bir özel telemetri fabrikada sağlanan artık .NET Core üzerinde Web işleri SDK'sını kullanır. Ancak işlem hattının bağımlılık ekleme kullanılarak özel telemetri ekleyebilirsiniz. Bu bölümdeki örneklerde aşağıdaki gerektiren `using` ifadeleri:

```cs
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.ApplicationInsights.Channel;
```

Aşağıdaki özel uygulanışı [ `ITelemetryInitializer` ] kendi eklemenize olanak sağlayan [ `ITelemetry` ](/dotnet/api/microsoft.applicationinsights.channel.itelemetry) varsayılan [ `TelemetryConfiguration` ].

```cs
internal class CustomTelemetryInitializer : ITelemetryInitializer
{
    public void Initialize(ITelemetry telemetry)
    {
        // Do something with telemetry.
    }
}
```

Çağrı [ `ConfigureServices` ] özel eklemek için oluşturucu [ `ITelemetryInitializer` ] ardışık düzenine.

```cs
static void Main()
{
    var builder = new HostBuilder();
    builder.ConfigureWebJobs(b =>
    {
        b.AddAzureStorageCoreServices();
    });
    builder.ConfigureLogging((context, b) =>
    {
        // Add logging providers.
        b.AddConsole();

        // If this key exists in any config, use it to enable Application Insights.
        string appInsightsKey = context.Configuration["APPINSIGHTS_INSTRUMENTATIONKEY"];
        if (!string.IsNullOrEmpty(appInsightsKey))
        {
            // This uses the options callback to explicitly set the instrumentation key.
            b.AddApplicationInsights(o => o.InstrumentationKey = appInsightsKey);
        }
    });
    builder.ConfigureServices(services =>
        {
            services.AddSingleton<ITelemetryInitializer, CustomTelemetryInitializer>();
        });
    var host = builder.Build();
    using (host)
    {

        host.Run();
    }
}
```

Zaman [ `TelemetryConfiguration` ] oluşturulur, tüm kayıtlı türleri [ `ITelemetryInitializer` ] dahil edilir. Daha fazla bilgi için bkz. [özel olaylar ve ölçümler için Application Insights API](../azure-monitor/app/api-custom-events-metrics.md).

Sürüm 3. *x*, temizleme zorunda [ `TelemetryClient` ] konağı durduğunda. .NET Core bağımlılık ekleme sistem otomatik olarak kayıtlı atar `ApplicationInsightsLoggerProvider`, hangi Boşaltmaları [ `TelemetryClient` ].

#### <a name="version-2x"></a>Sürüm 2. *x*

Sürüm 2'de. *x*, [ `TelemetryClient` ] Web işleri SDK'sı kullanır için Application Insights sağlayıcı tarafından dahili olarak oluşturulan [ `ServerTelemetryChannel` ](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Application Insights uç noktası kullanılamıyor veya azaltma gelen istekler, bu kanal olduğunda [istekleri web uygulaması'nın dosya sisteminde kaydeder ve daha sonra yeniden gönderen](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

[ `TelemetryClient` ] Uygulayan bir sınıf tarafından oluşturulan `ITelemetryClientFactory`. Varsayılan olarak, [ `DefaultTelemetryClientFactory` ](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Application Insights işlem hattının herhangi bir bölümünü değiştirmek istiyorsanız, size kendi sağlayabilir `ITelemetryClientFactory`, ve ana sınıfınız oluşturmak için kullanacağı bir [ `TelemetryClient` ]. Örneğin, bu kodu geçersiz kılmaları `DefaultTelemetryClientFactory` özelliği değiştirilecek `ServerTelemetryChannel`:

```csharp
private class CustomTelemetryClientFactory : DefaultTelemetryClientFactory
{
    public CustomTelemetryClientFactory(string instrumentationKey, Func<string, LogLevel, bool> filter)
        : base(instrumentationKey, new SamplingPercentageEstimatorSettings(), filter)
    {
    }

    protected override ITelemetryChannel CreateTelemetryChannel()
    {
        ServerTelemetryChannel channel = new ServerTelemetryChannel();

        // Change the default from 30 seconds to 15 seconds.
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

`SamplingPercentageEstimatorSettings` Nesnesi yapılandırır [Uyarlamalı örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling). Başka bir deyişle, belirli yüksek hacimli senaryolar telemetri verilerinin seçili bir alt kümesi uygulamaları Insights sunucusuna gönderir.

Telemetri fabrikasını oluşturduktan sonra içinde Application Insights günlük sağlayıcısına geçirilen:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Sonraki adımlar

Bu makalede, Web işleri SDK'sı ile çalışmaya yönelik yaygın senaryolar nasıl ele alınacağını gösteren kod parçacıkları sağlamıştır. Tam örnekler için bkz: [azure webjobs sdk örnekleri](https://github.com/Azure/azure-webjobs-sdk-samples).

[`ExecutionContext`]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
[`TelemetryClient`]: /dotnet/api/microsoft.applicationinsights.telemetryclient
['Createservicereplicalisteners()']: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
[`ITelemetryInitializer`]: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
[`TelemetryConfiguration`]: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
[`JobHostConfiguration`]: https://github.com/Azure/azure-webjobs-sdk/blob/v2.x/src/Microsoft.Azure.WebJobs.Host/JobHostConfiguration.cs
