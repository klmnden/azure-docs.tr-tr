---
title: Azure Web işleri SDK'sı - kullanma
description: Web işleri SDK'sı için kod yazma hakkında daha fazla bilgi edinin. Olay temelli bir arka plan Azure hizmetlerini ve üçüncü taraf hizmetleri içindeki verilere erişen işleme işleri oluşturun.
services: app-service\web, storage
documentationcenter: .net
author: ggailey777
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/19/2019
ms.author: glenga
ms.openlocfilehash: ab502c25a632977065e55d2eeafd684203636b14
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109920"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Olay temelli bir arka plan işlemleri için Azure Web işleri SDK'sını kullanma

Bu makalede için kod yazmaya yönelik rehberlik sağlanır [Azure WebJobs SDK](webjobs-sdk-get-started.md). Belge her iki sürümü için geçerlidir 3.x ve WebJobs SDK 2.x. API farklılıklar da mevcut olduğunda, her iki örnek verilmiştir. Tanıtılan ana değişiklik sürümü tarafından 3.x .NET Core, .NET Framework yerine kullanılır.

>[!NOTE]
> [Azure işlevleri](../azure-functions/functions-overview.md) Web işleri SDK'sı ve bazı konular için Azure işlevleri belgelerinde Bu makale bağlantıları temel alır. İşlevler ve Web işleri SDK'sı arasında aşağıdaki farklılıklara dikkat edin:
> * Azure işlevleri sürüm 2.x WebJobs SDK sürümüne karşılık gelen 3.x ve Azure işlevleri 1.x karşılık gelen WebJobs SDK 2.x. Kaynak kodu depoları numaralandırma Web işleri SDK'yı izleyin.
> * Azure işlevleri C# sınıf kitaplıkları için örnek kod, Web işleri SDK'sı kod gibi gerekmez dışında olan bir `FunctionName` WebJobs SDK projesindeki özniteliği.
> * Bazı bağlama türleri yalnızca, HTTP Web kancası ve Event Grid (hangi HTTP göre) gibi işlevler de desteklenir.
> 
> Daha fazla bilgi için [Azure işlevleri ve WebJobs SDK karşılaştırması](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs).

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi okuyun ve görevleri tamamlandı varsayar [WebJobs SDK ile çalışmaya başlama](webjobs-sdk-get-started.md).

## <a name="webjobs-host"></a>WebJobs konak

Konak, İşlevler için bir çalışma zamanı kapsayıcıdır.  Bu, tetikleyiciler ve çağrıları işlevleri için dinler. Sürümünde 3.x konak uygulaması olduğu `IHost`ve sürüm 2.x kullanmanız `JobHost` nesne. Kodunuzda bir ana bilgisayar örneği oluşturur ve davranışını özelleştirmek için kod yazma.

WebJobs SDK kullanarak doğrudan ve dolaylı olarak Azure işlevleri'ni kullanarak kullanarak arasında önemli bir fark budur. Azure işlevleri'nde hizmet konak kontrolü ve kod yazarak özelleştiremezsiniz. Azure işlevleri aracılığıyla ayarlarında konak davranışını özelleştirmenize olanak sağlar *host.json* dosya. Bu ayarlar, yapabileceğiniz özelleştirmeleri türlerini sınırlayan kodu değil, dizelerdir.

### <a name="host-connection-strings"></a>Konak bağlantı dizeleri 

WebJobs SDK, Azure depolama ve Azure Service Bus bağlantı dizeleri arar *local.settings.json* Azure'da çalıştırdığınızda, yerel olarak veya WebJob'ın ortamındaki çalıştırdığınızda dosya. Varsayılan olarak, bir depolama bağlantı dizesi ayarı adlı `AzureWebJobsStorage` gereklidir.  

Sürüm 2.x SDK'sının sağlar, bu bağlantı dizeleri için kendi adlarınızı kullanın veya bunları başka bir yerde depolayın. Bunları kod içinde burada gösterildiği gibi ayarlayabilirsiniz:

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

Varsayılan .NET Core yapılandırma API'leri kullanan olduğundan hiçbir API sürümünde 3.x bağlantı dizesi adlarını değiştirmek için.

### <a name="host-development-settings"></a>Konak geliştirme ayarları

Konak, yerel geliştirme daha verimli hale getirmek için geliştirme modunda çalıştırabilirsiniz. Geliştirme modunda çalışırken değiştirildi ayarlardan bazıları şunlardır:

| Özellik | Geliştirme ayarı |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` Günlük çıktısını en üst düzeye çıkarmak için. |
| `Queues.MaxPollingInterval`  | Kuyruk yöntemleri hemen harekete emin olmak için düşük bir değer.  |
| `Singleton.ListenerLockPeriod` | 15 Hızlı yinelemeli geliştirme yardımcı olmak için saniye. |

Geliştirme modunu etkinleştirmek şekilde SDK sürümüne bağlıdır. 

#### <a name="version-3x"></a>Sürüm 3.x

Standart ASP.NET Core API sürümü 3.x kullanır. Çağrı [UseEnvironment](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useenvironment) metodunda [ `HostBuilder` ](/dotnet/api/microsoft.extensions.hosting.hostbuilder) örneği. Adlı bir dizeyi geçirmek `development`, aşağıdaki örnekte olduğu gibi:

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

#### <a name="version-2x"></a>Sürüm 2.x

`JobHostConfiguration` Sınıfında bir `UseDevelopmentSettings` geliştirme modunu etkinleştirir yöntemi.  Aşağıdaki örnek, geliştirme ayarlarını nasıl kullanacağınız gösterilmektedir. Yapmak `config.IsDevelopment` dönüş `true` yerel olarak çalıştırılırken adlı bir yerel ortam değişkenini ayarlamak `AzureWebJobsEnv` değerle `Development`.

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

### <a name="jobhost-servicepointmanager-settings"></a>Eşzamanlı bağlantı sayısı ('ın v2.x) yönetme

Sürümünde 3.x bağlantı sınırı varsayılan olarak sonsuz bağlantıları. Bazı nedenlerden dolayı bu sınırı değiştirmek istiyorsanız, kullanabileceğiniz [MaxConnectionsPerServer](/dotnet/api/system.net.http.winhttphandler.maxconnectionsperserver) özelliği [WinHttpHander](/dotnet/api/system.net.http.winhttphandler) sınıfı.

İçin sürüm 2.x kullanarak bir konak için eş zamanlı bağlantı sayısını denetleme [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) API. 2.x içinde WebJobs konağınız başlatmadan önce bu varsayılan 2 değerini artırmanız gerekir.

Bir işlevden kullanarak yaptığınız tüm giden HTTP isteklerini `HttpClient` akışına `ServicePointManager`. Yaklaştığınızda `DefaultConnectionLimit`, `ServicePointManager` göndermeden önce sıraya alma isteği başlatır. Varsayalım, `DefaultConnectionLimit` 2 ve, kod yapar 1.000 HTTP isteği ayarlayın. Başlangıçta, yalnızca iki isteği için işletim sistemi aracılığıyla izin verilir. Diğer 998 kuyruğa oluncaya kadar bunları yer. Anlamına gelir, `HttpClient` zaman aşımı nedeniyle olabilir. Bunu *gördüğü* istek yaptı, ancak istek hedef sunucuya işletim sistemi tarafından hiçbir zaman gönderildi. Anlamlı yaramadı davranışını görebilirsiniz: yerel `HttpClient` bir isteğin tamamlanması için 10 saniye sürecek ancak hizmetinizi 200 ms içinde her istek döndürüyor. 

ASP.NET uygulamaları için varsayılan değer `Int32.MaxValue`, ve da temel veya daha yüksek bir App Service planında çalıştırmayı WebJobs için çalışma olasılığı olmasıdır. Yalnızca temel ve daha yüksek App Service planları tarafından desteklenen ve Web işleri genellikle her zaman açık ayar gerekir. 

WebJob'ınıza bir ücretsiz veya paylaşılan App Service planı içinde çalışıyorsa, uygulamanız şu anda sahip App Service korumalı alanı tarafından kısıtlı bir [bağlantı sınırı 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). İlişkisiz bağlantı sınır içinde `ServicePointManager`, korumalı alan bağlantı eşiği ulaştı ve site kapatma daha olasıdır. Bu durumda, ayarı `DefaultConnectionLimit` şeye 50 veya 100 ' gibi daha düşük Bunun gerçekleşmesini önlemek ve yine de yeterli aktarım hızı için izin verebilirsiniz.

Ayar, herhangi bir HTTP isteğinin yapılmadan önce yapılandırılmalıdır. Bu nedenle, Web işleri ana bilgisayar ayarı çalışmaması gerekir otomatik olarak; Bu beklenmeyen davranışa neden olabilir ve konak başlatıldığında önce meydana gelen HTTP isteklerini olabilir. Hemen değeri ayarlamak için en iyi yaklaşımdır, `Main` başlatma önce yöntemi `JobHost`aşağıdaki örnekte gösterildiği gibi

```csharp
static void Main(string[] args)
{
    // Set this immediately so that it is used by all requests.
    ServicePointManager.DefaultConnectionLimit = Int32.MaxValue;

    var host = new JobHost();
    host.RunAndBlock();
}
```

## <a name="triggers"></a>Tetikleyiciler

İşlevler genel yöntemleri olmalıdır ve bir tetikleyici özniteliği olmalıdır veya [NoAutomaticTrigger](#manual-trigger) özniteliği.

### <a name="automatic-trigger"></a>Otomatik tetikleyici

Otomatik tetikleyiciler, bir olaya yanıt olarak bir işlev çağırın. Kuyruk tetikleyicisi örneği için bkz. [Get başlatılan makale](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>El ile tetikleme

Bir işlev el ile tetiklemek için kullanmak `NoAutomaticTrigger` aşağıdaki örnekte gösterildiği gibi öznitelik:

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

İşlev el ile tetikleme yolu SDK'sı sürümüne bağlıdır.

#### <a name="version-3x"></a>Sürüm 3.x

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

#### <a name="version-2x"></a>Sürüm 2.x

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

## <a name="input-and-output-bindings"></a>Giriş ve çıkış bağlamaları

Giriş bağlamaları, verileri Azure veya üçüncü taraf hizmetleri kodunuza kullanılabilmesi için bildirim temelli bir yöntemini sağlar. Çıkış bağlamaları, verileri güncelleştirmek için bir yol sağlar. [Get başlangıç makalesi](webjobs-sdk-get-started.md) her bir örneği gösterilmektedir.

Yöntemin dönüş değerini özniteliği uygulayarak, bir yöntemin dönüş değeri bir çıkış bağlaması için kullanabilirsiniz. Azure işlevleri'nde örneğe bakın [Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) makalesi.

## <a name="binding-types"></a>Bağlama türü

Bağlama türleri yüklü ve yönetilen yol farklı sürümleri arasında 3.x ve 2.x SDK. Bir özel bağlama türü için yüklemek için paketi bulabilirsiniz **paketleri** bağlama türün bölümünü [başvurusu makalesinde](#binding-reference-information) Azure işlevleri için. Dosyaları tetikleyici ve bağlama (yerel dosya sistemi) için Azure işlevleri tarafından desteklenmeyen bir özel durumdur.

#### <a name="version-3x"></a>Sürüm 3.x

Sürümünde 3.x depolama bağlamaları dahil edilecek `Microsoft.Azure.WebJobs.Extensions.Storage` paket. Çağrı `AddAzureStorage` uzantı yönteminde `ConfigureWebJobs` aşağıdaki örnekte gösterildiği gibi yöntemi:

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

Diğer tetikleyici ve bağlama türlerini kullanmak için bunları içeren NuGet paketini yüklemek ve çağrı `Add<binding>` uzantı uygulanan genişletme yöntemi. Örneğin, bir Azure Cosmos DB bağlama kullanmak istiyorsanız, yükleme `Microsoft.Azure.WebJobs.Extensions.CosmosDB` ve çağrı `AddCosmosDB`, aşağıdaki örnekte olduğu gibi:

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

#### <a name="version-2x"></a>Sürüm 2.x

Aşağıdaki tetikleyici ve bağlama türlerini sürümünde bulunan 2.x `Microsoft.Azure.WebJobs` paket:

* Blob depolama
* Kuyruk depolama
* Table Storage

Diğer tetikleyici ve bağlama türlerini kullanmak için bunları içeren NuGet paketini yüklemek ve çağırmak bir `Use<binding>` metodunda `JobHostConfiguration` nesne. Örneğin, bir zamanlayıcı tetikleyicisi kullanmak istiyorsanız, yükleme `Microsoft.Azure.WebJobs.Extensions` ve çağrı `UseTimers` içinde `Main` yöntemi, bu örnekte olduğu gibi:

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

Adlarınıza şekilde [ `ExecutionContext` ] , SDK sürümüne bağlıdır.

#### <a name="version-3x"></a>Sürüm 3.x

Çağrı `AddExecutionContextBinding` uzantı yönteminde `ConfigureWebJobs` aşağıdaki örnekte gösterildiği gibi yöntemi:

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

#### <a name="version-2x"></a>Sürüm 2.x

`Microsoft.Azure.WebJobs.Extensions` Daha önce bahsedilen paketi ayrıca çağırarak kaydedebileceğiniz bir özel bağlama türü sağlar `UseCore` yöntemi. Bu bağlama, tanımlamanıza olanak sağlar. bir [ `ExecutionContext` ] parametresini aşağıdaki gibi etkinleştirilir, işlev imzası:

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

Bazı tetikleyicisini ve bağlamalarını davranışlarını yapılandırmanıza olanak sağlar. Bunları yapılandırma şekliniz SDK sürümüne bağlıdır.

* **Sürüm 3.x:** Yapılandırması ne zaman ayarlanır `Add<Binding>` yöntemi çağrıldığında `ConfigureWebJobs`.
* **Sürüm 2.x:** Özellikleri bir yapılandırma nesnesi, ayarlayarak, için geçirdiğiniz `JobHost`.

### <a name="queue-trigger-configuration"></a>Kuyruk tetikleyicisi yapılandırma

Depolama kuyruğu tetikleyici için yapılandırabileceğiniz ayarlar, Azure işlevleri'nde açıklanmaktadır [host.json başvurusu](../azure-functions/functions-host-json.md#queues). Aşağıdaki örnekler, yapılandırmanızda ayarlamaya gösterilmektedir:

#### <a name="version-3x"></a>Sürüm 3.x

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

#### <a name="version-2x"></a>Sürüm 2.x

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

### <a name="configuration-for-other-bindings"></a>Diğer bağlantılar için yapılandırma

Bazı tetikleyici ve bağlama türleri, kendi özel yapılandırma türü tanımlar. Örneğin, dosya tetikleyici, aşağıdaki örneklerde olduğu gibi izlemek için kök yolu belirtmenize olanak tanır:

#### <a name="version-3x"></a>Sürüm 3.x

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

#### <a name="version-2x"></a>Sürüm 2.x

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

Bağlama ifadeleri hakkında daha fazla bilgi için bkz: [ifadeleri ve desenleri bağlama](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) Azure işlevleri belgelerinde.

### <a name="custom-binding-expressions"></a>Özel bağlama ifadeleri

Bazı durumlarda bir kuyruk adı, bir blob adı veya kapsayıcı belirtmek istediğiniz veya bir tablo adı: sabit kodlamak yerine kod bu. Örneğin, kuyruk adı belirtmek isteyebilirsiniz `QueueTrigger` bir yapılandırma dosyası veya ortam değişkeni özniteliği.

Geçirerek bunu yapabilirsiniz bir `NameResolver` nesnesini `JobHostConfiguration` nesnesi. Tetikleyicisi veya bağlaması öznitelik oluşturucu parametresi, yer tutucu karakterleri içeren ve `NameResolver` kod, gerçek değerleri yerine bu yer tutucuları kullanılacak sağlar. Yer tutucuları, yüzde (%) işareti içine alarak aşağıdaki örnekte gösterildiği gibi tanımlanır:

```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Bu kod adında bir kuyruk kullanmanıza olanak tanıyan `logqueuetest` test ortamı ve bir adlı `logqueueprod` üretimde. Sabit kodlanmış kuyruk adı yerine bir giriş adını belirtin `appSettings` koleksiyonu.

Özel bir sağlamıyorsa etkinleşir NameResolver varsayılan yoktur. Varsayılan değerleri uygulama ayarları veya ortam değişkenlerini alır.

`NameResolver` Sınıfı alır kuyruk adı `appSettings` aşağıdaki örnekte gösterildiği gibi:

```cs
public class CustomNameResolver : INameResolver
{
    public string Resolve(string name)
    {
        return ConfigurationManager.AppSettings[name].ToString();
    }
}
```

#### <a name="version-3x"></a>Sürüm 3.x

Çözümleyici, bağımlılık ekleme kullanılarak yapılandırılır. Bu örnekleri aşağıdaki gerektiren `using` deyimi:

```cs
using Microsoft.Extensions.DependencyInjection;
```

Çözümleyici çağrılarak eklenir [ `ConfigureServices` ] genişletme yöntemini [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder), aşağıdaki örnekte olduğu gibi:

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

#### <a name="version-2x"></a>Sürüm 2.x

Geçirmek, `NameResolver` için sınıfını `JobHost` nesne aşağıdaki örnekte gösterildiği gibi:

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

Bağlamanın öznitelik gibi kullanmadan önce bazı çalışma işlevinizde yapmanız gerekiyorsa `Queue`, `Blob`, veya `Table`, kullanabileceğiniz `IBinder` arabirimi.

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

Azure işlevleri belgelerinde her bağlama türü hakkında başvuru bilgileri sağlanır. Depolama kuyruğu, örnek olarak kullanıldığında, aşağıdaki bilgileri her bağlama başvuru makalesinde bulabilirsiniz:

* [Paketleri](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -ne destek bağlama için bir Web işleri SDK'sı projeye dahil etmek için yüklemek için paketi.
* [Örnekler](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -C# sınıf kitaplığı örneği için Web işleri SDK'sı geçerlidir; yalnızca atlamak `FunctionName` özniteliği.
* [Öznitelikleri](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -bağlama türü için kullanılacak öznitelikleri.
* [Yapılandırma](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -öznitelik özellikleri ve oluşturucu parametrelerinin açıklamaları.
* [Kullanım](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) - ne türleri adlarınıza bağlayabileceğiniz ve bağlama hakkında bilgi çalışır. Örneğin: algoritma yoklama, zehirli kuyruk işleme.
  
Makaleler başvuru bağlamasının bir listesi için bkz **bağlamaları desteklenen** içinde [Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#supported-bindings) makale için Azure işlevleri. Bu listede, HTTP Web kancası ve Event Grid bağlamaları, yalnızca WebJobs SDK tarafından değil, Azure işlevleri tarafından desteklenir.

## <a name="disable-attribute"></a>Öznitelik devre dışı bırak 

[Devre dışı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) bir işlev olup olmadığını denetlemenizi öznitelik sağlar tetiklendi. 

Aşağıdaki örnekte, uygulama ayarının `Disable_TestJob` "1" veya "True" (büyük/küçük harf işlevi çalışmayacak küçük harfe duyarlı) değerine sahiptir. Bu durumda, çalışma zamanı bir günlük iletisi oluşturur *işlevi 'Functions.TestJob' devre dışı*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Azure portalında uygulama ayar değerleri değiştiğinde, webjob'ı yeniden başlatılması, yeni ayarlama çekme olur.

Öznitelik parametre, yöntemi veya sınıf düzeyinde bildirilebilir. Ayar adı bağlama ifadeleri de içerebilir.

## <a name="timeout-attribute"></a>Zaman aşımı özniteliği

[Zaman aşımı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) öznitelik, bir işlev, belirtilen bir süre içinde tamamlanmazsa iptal edilmesine neden olur. Aşağıdaki örnekte, işlev, zaman aşımı olmadan bir gün çalışır. Zaman aşımı ile işlev 15 saniye sonra iptal edildi.

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

Zaman aşımı özniteliğini sınıfta veya yöntemde düzeyinde uygulayabilir ve genel zaman aşımı kullanarak belirtebilirsiniz `JobHostConfiguration.FunctionTimeout`. Sınıfta veya yöntemde düzeyi zaman aşımları, genel zaman aşımı geçersiz.

## <a name="singleton-attribute"></a>Singleton özniteliği

Kullanım [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) konak web uygulamasının birden fazla örneği bulunduğunda bile çalışır bir işlevi yalnızca bir örneğini sağlamak için özniteliği. Bunu uygulayarak yapar [kilitleme dağıtılmış](#viewing-lease-blobs).

Aşağıdaki örnekte, yalnızca tek bir örneğini `ProcessImage` işlevi herhangi bir zamanda çalıştırır:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Bazı tetikleyici eşzamanlılık yönetimi için yerleşik desteği vardır:

* **QueueTrigger** - ayarlanmış `JobHostConfiguration.Queues.BatchSize` 1.
* **ServiceBusTrigger** - ayarlanmış `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` 1.
* **FileTrigger** - ayarlanmış `FileProcessor.MaxDegreeOfParallelism` 1.

Bu ayarlar, işlevinizi tekil olarak tek bir örneği üzerinde çalıştığından emin olmak için kullanabilirsiniz. İşlevi yalnızca tek bir örneği için birden fazla web uygulamasını kullanıma ölçeklendirildiğinde çalıştığından emin olmak için bir dinleyici düzeyi tekil işlev kilitlemek (`[Singleton(Mode = SingletonMode.Listener)]`). Dinleyici kilitleri JobHost başlatmada elde edilen. Tüm üç genişletilmiş örnekleri aynı anda başlatırsanız, yalnızca bir örneği kilidi alır ve yalnızca bir dinleyicisi başlatır.

### <a name="scope-values"></a>Kapsam değerleri

Belirtebileceğiniz bir **kapsam ifadesi/değer** işlev bu kapsamda tüm yürütmeleri serileştirilecek sağlar tekli üzerinde. Daha ayrıntılı bu şekilde kilitlemek uygulama için belirli bir düzeyde, işleviniz için paralellik derecesini gereksinimlerinizi tarafından belirlenen diğer çağrılarını serileştirirken izin verebilirsiniz. Örneğin, aşağıdaki örnekte kapsam ifadesi bağlar `Region` gelen iletinin değeri. Sıra bölgeler "Doğu", "Doğu" ve "Batı" sırasıyla "Doğu" ileti "Batı" içinde "Doğu." ile paralel çalıştırılan bölge ile çalışırken seri olarak yürütülen bölgesi olan iletiler üç iletileri içerdiğinde

```csharp
[Singleton("{Region}")]
public static async Task ProcessWorkItem([QueueTrigger("workitems")] WorkItem workItem)
{
     // Process the work item
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

Kilidi için varsayılan kapsamı `SingletonScope.Function` anlamı kilit kapsamı (blob kiralama yol) tam işlev adını bağlı. İşlevlerinin kilitlemek için belirtin `SingletonScope.Host` ve tümü aynı anda çalıştırmak istemediğiniz işlevleri aynı olan bir kapsam kimliği adı kullanın. Aşağıdaki örnekte, yalnızca bir örneğini `AddItem` veya `RemoveItem` teker teker çalıştırılır:

```csharp
[Singleton("ItemsLock", SingletonScope.Host)]
public static void AddItem([QueueTrigger("add-item")] string message)
{
     // Perform the add operation
}

[Singleton("ItemsLock", SingletonScope.Host)]
public static void RemoveItem([QueueTrigger("remove-item")] string message)
{
     // Perform the remove operation
}
```

### <a name="viewing-lease-blobs"></a>İzleme kira BLOB'ları

Web işleri SDK'sı kullanır [Azure blob kiralama](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) altında dağıtılmış kilitleme uygulamak için kapsar. Singleton tarafından kullanılan kira bloblar bulunabilir `azure-webjobs-host` kapsayıcısında `AzureWebJobsStorage` "kilitleri" yolunda depolama hesabı. Örneğin, ilk kira blob yolunu `ProcessImage` daha önce gösterilen örnek olabilir `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Tüm yolları Bu büyük/küçük harf 061851c758f04938a4426aa9ab3869c0 JobHost Kimliğini içerir.

## <a name="async-functions"></a>Zaman uyumsuz işlevleri

Kod zaman uyumsuz işlevleri hakkında daha fazla bilgi için Azure işlevleri belgelerinde bakın [zaman uyumsuz işlevleri](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>İptal belirteçleri

İptal belirteçleri işlemek nasıl hakkında daha fazla bilgi için Azure işlevleri belgelerinde bakın [iptal belirteçleri ve normal şekilde kapatılmasını](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Birden çok örneği

Web uygulamanız birden çok örnek üzerinde çalışıyorsa, sürekli bir WebJob Tetikleyiciler için dinleme ve işlevleri çağırma her örneğinde çalışır. Çeşitli tetikleyici bağlamaları verimli bir şekilde iş örnekleri arasında işbirliği içinde paylaşmak için tasarlanmış daha fazla örnekleri için ölçek genişletme tanır; böylece, daha fazla yükü işlemek için.

Kuyruk ve blob Tetikleyicileri otomatik olarak bir işleve bir kuyruk iletisi işlenmesini önlemek veya birden çok kez blob; işlevleri kez etkili olması gerekmez.

Belirli bir zamanlanan saatte çalıştıran birden fazla işlev örneği elde Zamanlayıcı tetikleyicisi otomatik olarak Zamanlayıcıyı çalıştıran, yalnızca bir örneğini sağlar.

Bu yalnızca bir örneğini sağlamak istiyorsanız bile konak web uygulamasının birden fazla örneği bulunduğunda, bir işlevi çalıştırır, kullanabileceğiniz [tekil özniteliği](#singleton-attribute).

## <a name="filters"></a>Filtreler

İşlev filtreleri (Önizleme), kendi mantığınızı WebJobs yürütme hattıyla özelleştirmek için bir yol sağlar. Filtreler benzer [ASP.NET Core filtreleri](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). İşlevler veya sınıflar uygulanır bildirim temelli öznitelikleri olarak uygulanabilir. Daha fazla bilgi için [işlevi filtreleri](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

ASP.NET için geliştirilmiş günlüğe kaydetme çerçevesi öneririz ve [başlama](webjobs-sdk-get-started.md) makalesi, nasıl kullanılacağını gösterir. 

### <a name="log-filtering"></a>Günlük Filtresi

Tarafından oluşturulan her günlük bir `ILogger` örneği ilişkili bir `Category` ve `Level`. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) bir sabit listesidir ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|None        | 6 |

Her kategori için belirli bir bağımsız olarak filtrelenebilir [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel). Örneğin, blob tetikleyicisi yalnızca işleme için tüm günlükleri görmek isteyebilirsiniz `Error` ve diğer her şey için daha yüksek.

#### <a name="version-3x"></a>Sürüm 3.x

Sürüm 3.x SDK'sının .NET Core ile yapılandırılan filtrelemeyi kullanır. `LogCategories` Sınıfı belirli İşlevler, Tetikleyiciler veya kullanıcılar için kategorileri tanımlamanıza olanak sağlar. Ayrıca belirli konak durumları için filtreler gibi tanımlar `Startup` ve `Results`. Bu şekilde, günlük çıktısı hassas ayarlamalar yapabilirsiniz. İçinde tanımlanan kategorilere eşleşme bulunursa, filtre geri döner `Default` ileti filtresi karar verirken değeri.

`LogCategories` Aşağıdaki gerektirir using deyimi:

```cs
using Microsoft.Azure.WebJobs.Logging; 
```

Aşağıdaki örnek, varsayılan olarak tüm günlükleri filtreleyen bir filtre yapıları `Warning` düzeyi. Kategorileri `Function` veya `results` (eşdeğer `Host.Results` sürüm 2.x), filtre `Error` düzeyi. Geçerli kategori tüm kayıtlı düzeylerine filtresi karşılaştırır `LogCategories` örnek ve en uzun eşleşmeyi seçer. Diğer bir deyişle `Debug` düzeyi için kayıtlı `Host.Triggers` eşleşecektir `Host.Triggers.Queue` veya `Host.Triggers.Blob`. Bu, her birini eklemek gerek kalmadan daha geniş kategorileri denetlemenizi sağlar.

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

#### <a name="version-2x"></a>Sürüm 2.x

Sürüm 2.x SDK ' nın `LogCategoryFilter` sınıfı filtreleme denetlemek için kullanılır. `LogCategoryFilter` Sahip bir `Default` başlangıç değeri özelliğiyle `Information`, düzeylerine sahip herhangi bir iletisi güncelleştirmeyeceği `Information`, `Warning`, `Error`, veya `Critical` düzeylerinesahipherhangibiriletisiancakkaydedilir`Debug` veya `Trace` hemen filtrelenir.

Olduğu gibi `LogCategories` sürümünde 23.x, `CategoryLevels` özelliği, günlük çıktısı ayarlayabilmek belirli kategorileri için günlük düzeyleri belirtmenize olanak sağlar. İçinde hiçbir eşleşme bulunursa `CategoryLevels` sözlük, filtre gördükleri `Default` ileti filtresi karar verirken değeri.

Aşağıdaki örnek, varsayılan olarak tüm günlükleri filtreleyen bir filtre yapıları `Warning` düzeyi. Kategorileri `Function` veya `Host.Results` , filtrelenmiş `Error` düzeyi. `LogCategoryFilter` Kaydedilen tüm geçerli kategoriye karşılaştırır `CategoryLevels` ve en uzun eşleşmeyi seçer. Diğer bir deyişle `Debug` düzeyi için kayıtlı `Host.Triggers` eşleşecektir `Host.Triggers.Queue` veya `Host.Triggers.Blob`. Bu, her birini eklemek gerek kalmadan daha geniş kategorileri denetlemenizi sağlar.

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

Özel telemetri için uygulayan bir şekilde [Application Insights](../azure-monitor/app/app-insights-overview.md) kullanmakta olduğunuz SDK'sı sürümüne bağlıdır. Application Insights yapılandırma konusunda bilgi için bkz: [Application Insights Ekle günlük](webjobs-sdk-get-started.md#add-application-insights-logging).

#### <a name="version-3x"></a>Sürüm 3.x

Sürümünden 3.x Web işleri SDK'sı, .NET Core genel konakta kullanır, artık özel telemetri fabrikada sağlanan. Ancak işlem hattının bağımlılık ekleme kullanılarak özel telemetri ekleyebilirsiniz. Bu bölümdeki örneklerde aşağıdaki gerektiren `using` ifadeleri:

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
        // Add Logging Providers
        b.AddConsole();

        // If this key exists in any config, use it to enable App Insights
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

Zaman [ `TelemetryConfiguration` ] oluşturulur, tüm kayıtlı türleri [ `ITelemetryInitializer` ] dahil edilir. Bkz: çalışma hakkında daha fazla bilgi edinmek için [özel olaylar ve ölçümler için Application Insights API](../azure-monitor/app/api-custom-events-metrics.md).

Sürümünde 3.x artık zorunda boşaltma [ `TelemetryClient` ] konağı durduğunda. .NET Core bağımlılık ekleme sistem otomatik olarak kayıtlı atar `ApplicationInsightsLoggerProvider`, hangi Boşaltmaları [ `TelemetryClient` ].

#### <a name="version-2x"></a>Sürüm 2.x

Sürüm 2.x [ `TelemetryClient` ] Web işleri SDK'sı kullanır için Application Insights sağlayıcı tarafından dahili olarak oluşturulan [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Application Insights uç noktası kullanılamıyor veya azaltma gelen istekler, bu kanal olduğunda [istekleri web uygulaması'nın dosya sisteminde kaydeder ve daha sonra yeniden gönderen](https://apmtips.com/blog/2015/09/03/more-telemetry-channels).

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

        // change the default from 30 seconds to 15 seconds
        channel.MaxTelemetryBufferDelay = TimeSpan.FromSeconds(15);

        return channel;
    }
}
```

SamplingPercentageEstimatorSettings nesnesi yapılandırır [Uyarlamalı örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Başka bir deyişle, yüksek hacimli belirli senaryolarda, seçilen verilerin bir alt kümesini telemetri App Insights sunucusuna gönderir.

Telemetri Fabrika oluşturduktan sonra içinde Application Insights günlük sağlayıcısına geçirilen:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Sonraki adımlar

Bu kılavuz, WebJobs SDK ile çalışmaya yönelik yaygın senaryolar nasıl ele alınacağını gösteren kod parçacıkları sağlamıştır. Tam örnekler için bkz: [azure webjobs sdk örnekleri](https://github.com/Azure/azure-webjobs-sdk-samples).

[`ExecutionContext`]: https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs
[`TelemetryClient`]: /dotnet/api/microsoft.applicationinsights.telemetryclient
['Createservicereplicalisteners()']: /dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices
[`ITelemetryInitializer`]: /dotnet/api/microsoft.applicationinsights.extensibility.itelemetryinitializer
[`TelemetryConfiguration`]: /dotnet/api/microsoft.applicationinsights.extensibility.telemetryconfiguration
