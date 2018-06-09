---
title: Azure WebJobs SDK'sını kullanma
description: Web işleri SDK'si için kod yazma hakkında daha fazla bilgi edinin. Olay kaynaklı arka plan Azure hizmetlerinin ve üçüncü taraf hizmetleri veri erişim işleme işleri oluşturun.
services: app-service\web, storage
documentationcenter: .net
author: tdykstra
manager: cfowler
editor: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/27/2018
ms.author: tdykstra
ms.openlocfilehash: 08272ba7d828f744336723f25b482bf06b9e43dc
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234659"
---
# <a name="how-to-use-the-azure-webjobs-sdk-for-event-driven-background-processing"></a>Azure WebJobs SDK olay denetimli arka plan işleme için kullanma

Bu makalede için kod yazma konusunda rehberlik sağlar [Azure WebJobs SDK](webjobs-sdk-get-started.md). Belge sürümleri için geçerlidir 2.x ve 3.x belirtilenler Aksi takdirde. 3.x tarafından sunulan ana değişiklik .NET Core .NET Framework yerine kullanılır.

>[!NOTE]
> [Azure işlevleri](../azure-functions/functions-overview.md) WebJobs SDK ve Azure işlevleri belgelere bazı konular için bu makalede bağlantıları üzerine inşa edilmiştir. İşlevler ve WebJobs SDK arasındaki aşağıdaki farkları dikkat edin:
> * Azure işlevleri sürümü 1.x Web işleri SDK'si sürüme karşılık gelen 2.x ve 2.x WebJobs SDK'sına karşılık gelen Azure işlevleri 3.x. Kaynak kodu depoları numaralandırma WebJobs SDK izleyin ve birçok v2.x dal, şu anda 3.x koduna sahip ana dala sahip.
> * Azure işlevleri C# sınıfı kitaplıklar örnek kodu Web işleri SDK'si kod gibi gerekmeyen dışında bir `FunctionName` WebJobs SDK projesi özniteliği.
> * Bazı bağlama türleri yalnızca HTTP, Web kancası ve (hangi HTTP tabanlı) olay kılavuz gibi işlevler desteklenir. 
> 
> Daha fazla bilgi için bkz: [WebJobs SDK ve Azure işlevleri](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md#compare-functions-and-webjobs). 

## <a name="prerequisites"></a>Önkoşullar

Bu makalede okuma sahip olduğunuz varsayılmaktadır [WebJobs SDK ile çalışmaya başlama](webjobs-sdk-get-started.md).

## <a name="jobhost"></a>JobHost

`JobHost` Nesnesidir işlevler için çalışma zamanı kapsayıcısı: tetikleyiciler ve çağrıları işlevleri için dinler. Oluşturduğunuz `JobHost` kod ve davranışını özelleştirmek için kod yazın.

WebJobs SDK kullanarak doğrudan ve dolaylı olarak Azure işlevlerini kullanarak kullanma arasında önemli bir fark budur. Azure işlevleri, hizmet denetimleri içinde `JobHost`, ve kod yazarak özelleştiremezsiniz. Azure işlevleri ayarlarında aracılığıyla konak davranışını özelleştirmenizi sağlar *host.json* dosya. Bu ayarlar, yapabileceğiniz özelleştirmeleri türlerini sınırlayan kod değil, dizelerdir.

### <a name="jobhost-connection-strings"></a>JobHost bağlantı dizeleri

WebJobs SDK depolama ve hizmet veri yolu bağlantı dizeleri arar *local.settings.json* çalıştırdığınızda yerel olarak veya Web işi'nin ortamda Azure'da çalıştırdığınızda. Bu bağlantı dizeleri için kendi adlarını kullanın veya başka bir yerde depolamak istiyorsanız, bunları kodda, aşağıda gösterildiği gibi ayarlayabilirsiniz:

```cs
static void Main(string[] args)
{
    var _storageConn = ConfigurationManager
        .ConnectionStrings["MyStorageConnection"].ConnectionString;

    var _dashboardConn = ConfigurationManager
        .ConnectionStrings["MyDashboardConnection"].ConnectionString;

    JobHostConfiguration config = new JobHostConfiguration();
    config.StorageConnectionString = _storageConn;
    config.DashboardConnectionString = _dashboardConn;
    JobHost host = new JobHost(config);
    host.RunAndBlock();
}
```

### <a name="jobhost-development-settings"></a>JobHost geliştirme ayarları

`JobHostConfiguration` Sınıfına sahip bir `UseDevelopmentSettings` yerel geliştirme daha verimli hale getirmek için çağırabilirsiniz yöntemi. Bu yöntem değişiklikleri ayarlardan bazıları şunlardır:

| Özellik | Geliştirme ayarı |
| ------------- | ------------- |
| `Tracing.ConsoleLevel` | `TraceLevel.Verbose` Günlük çıktısı en üst düzeye çıkarmak için. |
| `Queues.MaxPollingInterval`  | Sıra yöntemleri hemen tetiklenen emin olmak için düşük bir değer.  |
| `Singleton.ListenerLockPeriod` | 15 Hızlı yinelemeli geliştirmeye yardımcı olmak için saniye sayısı. |

Aşağıdaki örnek, geliştirme ayarları nasıl kullanılacağı gösterilir. Yapmak için `config.IsDevelopment` dönmek `true` yerel olarak çalıştırırken adlı bir yerel ortam değişkenini ayarlamak `AzureWebJobsEnv` değerle `Development`.

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

### <a name="jobhost-servicepointmanager-settings"></a>JobHost ServicePointManager ayarları

.NET Framework adlı bir API'yi içeren [ServicePointManager.DefaultConnectionLimit](https://msdn.microsoft.com/library/system.net.servicepointmanager.defaultconnectionlimit) bir ana bilgisayara eşzamanlı bağlantı sayısını denetler. Web işleri ana bilgisayarınız başlatmadan önce bu varsayılan 2 değerini artırmak öneririz.

Bir işlevden kullanarak yaptığınız tüm giden HTTP istekleri `HttpClient` akışına `ServicePointManager`. Düğmesine bastığınızda `DefaultConnectionLimit`, `ServicePointManager` çağrısı isteği göndermeden önce başlar. Varsayalım, `DefaultConnectionLimit` 2 ve, kod yapar 1.000 HTTP istekleri ayarlayın. Başlangıçta, yalnızca 2 isteklerine gerçekte aracılığıyla işletim sistemine izin verilir. Diğer 998 sıraya alınan açılıncaya kadar bunlar için. Anlamına gelir, `HttpClient` zaman aşımı nedeniyle olabilir, *düşündüğü* istek yaptı, ancak istek hedef sunucuya işletim sistemi tarafından hiçbir zaman gönderildi. Anlamlı görünmemektedir davranışı görebilirsiniz: yerel `HttpClient` bir isteği tamamlamak için 10 saniye sürüyor ancak hizmetinizi her istek 200 ms olarak döndürmektir. 

ASP.NET uygulamaları için varsayılan değer `Int32.MaxValue`, ve bu da temel ya da daha yüksek bir uygulama hizmeti planında çalışan Web işleri için çalışmak büyük olasılıkla. Web işleri genelde her zaman açık ayarı gerekir ve yalnızca temel ve daha yüksek uygulama hizmeti planları tarafından desteklenir. 

WebJob bir ücretsiz veya paylaşılan uygulama hizmeti planı çalışıyorsa, uygulamanız şu anda olan uygulama hizmeti korumalı alanı tarafından kısıtlı bir [bağlantı sınırı olan 300](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox#per-sandbox-per-appper-site-numerical-limits). İlişkisiz bağlantı sınır içinde `ServicePointManager`, büyük olasılıkla korumalı alan bağlantı Eşiğe ulaşıldığında ve site kapatılmalıdır. Bu durumda, ayarı `DefaultConnectionLimit` için bir şeyler 50 veya 100 gibi alt Bunun olmaması ve hala için yeterli işleme izin verebilirsiniz.

Tüm HTTP isteklerinin yapılma önce ayarı yapılandırılmalıdır. Bu nedenle, Web işleri ana bilgisayar ayarı deneyin döndürmemelidir otomatik olarak; ana bilgisayar başlatılır ve bu beklenmeyen bir davranışa neden olabilir oluşması HTTP isteklerini olabilir. Hemen değeri ayarlamak için en iyi yaklaşımdır, `Main` başlatma önce yöntemi `JobHost`, aşağıdaki örnekte gösterildiği gibi

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

İşlevler genel yöntemler olmalıdır ve bir tetikleyici özniteliğe sahip olması gerekir veya [NoAutomaticTrigger](#manual-trigger) özniteliği.

### <a name="automatic-trigger"></a>Otomatik tetikleme

Otomatik tetikleyiciler, bir olaya yanıt olarak bir işlevini çağırın. Sıra tetikleyici bir örnek için bkz [Get başlatılan makale](webjobs-sdk-get-started.md).

### <a name="manual-trigger"></a>El ile tetikleyici

Bir işlev el ile tetiklemek için kullanabileceğiniz `NoAutomaticTrigger` aşağıdaki örnekte gösterildiği gibi öznitelik:

```cs
static void Main(string[] args)
{
    JobHost host = new JobHost();
    host.Call(typeof(Program).GetMethod("CreateQueueMessage"), new { value = "Hello world!" });
}
```

```cs
[NoAutomaticTrigger]
public static void CreateQueueMessage(
    TextWriter logger,
    string value,
    [Queue("outputqueue")] out string message)
{
    message = value;
    logger.WriteLine("Creating queue message: ", message);
}
```

## <a name="input-and-output-bindings"></a>Giriş ve çıkış bağlamaları

Giriş bağlamaları veri Azure veya üçüncü taraf hizmetleri kodunuzu kullanılabilmesi için bildirim temelli bir yolunu sağlar. Çıktı bağlama verileri güncelleştirmek için bir yol belirtin. [Get başlatılan makale](webjobs-sdk-get-started.md) her bir örneği gösterilmektedir.

Yöntemin dönüş değerini öznitelik uygulayarak yönteminin dönüş değeri bir çıktı bağlaması için kullanabilirsiniz. Azure işlevleri örnekte bkz [Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#using-the-function-return-value) makalesi.

## <a name="binding-types"></a>Bağlama türü

Aşağıdaki tetikleyici ve bağlama türleri içinde yer alan `Microsoft.Azure.WebJobs` paketi:

* Blob depolama
* Kuyruk depolama
* Table Storage

Diğer tetikleyici ve bağlama türü kullanmak için bunları içeren NuGet paketini yükleyin ve çağrısı bir `Use<binding>` yöntemi `JobHostConfiguration` nesnesi. Örneğin Zamanlayıcı tetikleyicisi kullanmak istiyorsanız, yükleme `Microsoft.Azure.WebJobs.Extensions` ve arama `UseTimers` içinde `Main` Bu örnekte olduğu gibi yöntemi:

```cs
static void Main()
{
    config = new JobHostConfiguration();
    config.UseTimers();
    var host = new JobHost(config);
    host.RunAndBlock();
}
```

Özel bağlama türü yüklemek için paket bulabilirsiniz **paketleri** , bağlama türü bölümünü [başvurusu makalesinde](#binding-reference-information) Azure işlevleri için. Dosyaların Tetik ve Azure işlevleri tarafından desteklenmeyen bağlama (yerel dosya sistemi için), bir özel durumdur. Dosyaları kullanmak için bağlama, yükleme `Microsoft.Azure.WebJobs.Extensions` ve arama `UseFiles`.

### <a name="usecore"></a>UseCore

`Microsoft.Azure.WebJobs.Extensions` Daha önce bahsedilen paketi ayrıca çağırarak kaydedebilirsiniz özel bağlama türü sağlar `UseCore` yöntemi. Bu bağlama, tanımlamanıza olanak sağlar. bir [ExecutionContext](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Core/ExecutionContext.cs) işlevi imzanız parametresi. Context nesnesi, verilen işlev çağrısı tarafından üretilen tüm günlükleri ilişkilendirmek için kullanabileceğiniz çağırma kimliği erişmenizi sağlar. Bir örneği aşağıda verilmiştir:

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

## <a name="binding-configuration"></a>Bağlama yapılandırması

Birçok tetikleyebilir ve türleri bağlama için ilettiğiniz bir yapılandırma nesnesi özelliklerini ayarlayarak davranışlarını yapılandırma izin `JobHost`.

### <a name="queue-trigger-configuration"></a>Sıra tetikleyici yapılandırması

Depolama kuyruğu tetikleyici için yapılandırabileceğiniz ayarlar Azure işlevlerinde açıklanacak [host.json başvuru](../azure-functions/functions-host-json.md#queues). Aşağıdaki örnekte nasıl WebJobs SDK projede ayarlanacağı gösterilmiştir:

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

### <a name="configuration-for-other-bindings"></a>Diğer bağlamaları için yapılandırma

Bazı tetikleyici ve bağlama türleri kendi özel yapılandırma türünü tanımlayın. Örneğin, dosya tetikleyici izlemek için kök yolu belirtmenize olanak sağlar:

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

Öznitelik Oluşturucusu parametrelerinde değerlerine çeşitli kaynaklardan çözmek ifadeleri kullanabilirsiniz. Örneğin, aşağıdaki kodda, yolunu `BlobTrigger` özniteliği oluşturur adlı bir ifade `filename`. Çıktı bağlama için kullanıldığında `filename` tetikleme blob adını çözümler.
 
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

Bağlama ifadeleri hakkında daha fazla bilgi için bkz: [ifadeleri ve desenler bağlama](../azure-functions/functions-triggers-bindings.md#binding-expressions-and-patterns) Azure işlevleri belgelerinde.

### <a name="custom-binding-expressions"></a>Özel bağlama ifadeleri

Kuyruk adı, bir blob adı veya kapsayıcı belirtmek istediğiniz bazen veya bir tablo adı kod sabit kodlu yerine onu. Örneğin, kuyruk adı belirtmek isteyebilirsiniz `QueueTrigger` bir yapılandırma dosyası veya ortam değişkeni özniteliği.

Bunu geçirerek yapabilirsiniz bir `NameResolver` nesnesini `JobHostConfiguration` nesne. Yer tutucuları tetikleyici veya bağlama özniteliği Oluşturucusu parametreleri ekleyin ve `NameResolver` kodu bu yer tutucular yerine kullanılacak gerçek değerler sağlar. Yer tutucuları yüzde (%) işareti, çevreleyen tarafından aşağıdaki örnekte gösterildiği gibi tanımlanır:
 
```cs
public static void WriteLog([QueueTrigger("%logqueue%")] string logMessage)
{
    Console.WriteLine(logMessage);
}
```

Bu kod, test ortamında logqueuetest ve üretimde bir adlandırılmış logqueueprod adlandırılan bir kuyruğun kullanmanızı sağlar. Sabit kodlanmış kuyruk adı yerine bir girişe adını belirtin `appSettings` koleksiyonu. 

Varsayılan özel bir sağlamıyorsa etkinleşir NameResolver yoktur. Uygulama ayarları veya ortam değişkenleri, varsayılan değerleri alır.

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

Azure işlevleri uygular `INameResolver` örnekte gösterildiği gibi uygulama ayarlarından değerleri alabilirsiniz. WebJobs SDK doğrudan kullandığınızda, yer tutucu değiştirme değeri tercih ettiğiniz herhangi bir kaynaktan alır özel bir uygulama yazabilirsiniz. 

## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

Bir bağlama özniteliği gibi kullanmadan önce işlevinizi bazı iş yapmanız gerekirse `Queue`, `Blob`, veya `Table`, kullanabileceğiniz `IBinder` arabirimi.

Aşağıdaki örnek, bir giriş sırası iletisi alır ve bir çıkış sırası aynı içeriği ile yeni bir ileti oluşturur. Çıkış sırası adı işlevinin gövdesini kodda tarafından ayarlanır.

```cs
public static void CreateQueueMessage(
    [QueueTrigger("inputqueue")] string queueMessage,
    IBinder binder)
{
    string outputQueueName = "outputqueue" + DateTime.Now.Month.ToString();
    QueueAttribute queueAttribute = new QueueAttribute(outputQueueName);
    CloudQueue outputQueue = binder.Bind<CloudQueue>(queueAttribute);
    outputQueue.AddMessage(new CloudQueueMessage(queueMessage));
}
```

Daha fazla bilgi için bkz: [çalışma zamanında bağlama](../azure-functions/functions-dotnet-class-library.md#binding-at-runtime) Azure işlevleri belgelerinde.

## <a name="binding-reference-information"></a>Bağlama başvuru bilgileri

Her bağlama türü hakkında başvuru bilgileri Azure işlevleri belgelerinde sağlanır. Depolama kuyruğu bir örnek olarak kullanarak, aşağıdaki bilgileri her bağlama başvurusu makalesinde bulabilirsiniz:

* [Paketleri](../azure-functions/functions-bindings-storage-queue.md#packages---functions-1x) -WebJobs SDK projede bağlama için destek eklemek için yükleme paketini.
* [Örnekler](../azure-functions/functions-bindings-storage-queue.md#trigger---example) -C# sınıf kitaplığı örnek Web işleri SDK'si için geçerlidir; yalnızca atlayın `FunctionName` özniteliği.
* [Öznitelikleri](../azure-functions/functions-bindings-storage-queue.md#trigger---attributes) -bağlama türü için kullanılacak öznitelikleri.
* [Yapılandırma](../azure-functions/functions-bindings-storage-queue.md#trigger---configuration) -öznitelik özellikleri ve Oluşturucusu parametrelerinin açıklamaları.
* [Kullanım](../azure-functions/functions-bindings-storage-queue.md#trigger---usage) - ne türleri bağlayabilirsiniz ve bağlama hakkında bilgi çalışır. Örneğin: yoklama algoritması, sıra işleme zehirli.
  
Başvuru makaleleri bağlama bir listesi için bkz: **bağlamaları desteklenen** içinde [Tetikleyicileri ve bağlamaları](../azure-functions/functions-triggers-bindings.md#supported-bindings) makale Azure işlevleri için. Bu listede, HTTP, Web kancası ve olay kılavuz bağlamaları, yalnızca WebJobs SDK tarafından değil, Azure işlevleri tarafından desteklenir.

## <a name="disable-attribute"></a>Öznitelik devre dışı bırak 

[Devre dışı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/DisableAttribute.cs) bir işlev olup olmadığını denetlemenizi özniteliği sağlar tetiklendi. 

Aşağıdaki örnekte, uygulama ayarı `Disable_TestJob` "1" veya "True" (büyük işlevi çalışmayacak küçük harfe duyarlı), bir değer içeriyor. Bu durumda, çalışma zamanı bir günlük iletisi oluşturur *işlevi 'Functions.TestJob' devre dışı*.

```cs
[Disable("Disable_TestJob")]
public static void TestJob([QueueTrigger("testqueue2")] string message)
{
    Console.WriteLine("Function with Disable attribute executed!");
}
```

Azure portalında uygulama ayarı değerlerini değiştirdiğinizde, yeni ayarlama çekme başlatılması, Web işi olur.

Öznitelik parametre, yöntemi veya sınıf düzeyinde bildirilebilir. Ayar adı bağlama ifadeleri de içerebilir.

## <a name="timeout-attribute"></a>Zaman aşımı özniteliği

[Zaman aşımı](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/TimeoutAttribute.cs) özniteliği belirtilen bir süre içinde tamamlanmazsa iptal etmek için bir işlev neden olur. Aşağıdaki örnekte, zaman aşımı olmadan bir gün işlevi çalışır. Zaman aşımı ile işlevi 15 saniye sonra iptal edilir.

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

Sınıf veya yöntemin düzeyinde zaman aşımı özniteliği uygulayabilirsiniz ve kullanarak genel zaman aşımı belirtebilirsiniz `JobHostConfiguration.FunctionTimeout`. Sınıf veya yöntemin düzeyi zaman aşımları genel zaman aşımı geçersiz.

## <a name="singleton-attribute"></a>Singleton özniteliği

Kullanım [Singleton](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/SingletonAttribute.cs) ana web uygulamanızın birden fazla örneği bulunduğunda bir işlevi yalnızca bir örneğini çalıştırır bile emin olmak için öznitelik. Bunu uygulayarak yapar [kilitleme dağıtılmış](#viewing-lease-blobs).

Aşağıdaki örnekte, yalnızca tek bir örneğini `ProcessImage` işlevi herhangi bir zamanda çalıştırır:

```cs
[Singleton]
public static async Task ProcessImage([BlobTrigger("images")] Stream image)
{
     // Process the image
}
```

### <a name="singletonmodelistener"></a>SingletonMode.Listener

Bazı tetikleyiciler eşzamanlılık yönetimi için yerleşik destek vardır:

* **QueueTrigger** - ayarlanmış `JobHostConfiguration.Queues.BatchSize` 1.
* **ServiceBusTrigger** - ayarlanmış `ServiceBusConfiguration.MessageOptions.MaxConcurrentCalls` 1.
* **FileTrigger** - ayarlanmış `FileProcessor.MaxDegreeOfParallelism` 1.

İşlevinizi tek bir örneğinde bir singleton olarak çalıştığından emin olmak için bu ayarları kullanın. Web uygulaması için birden çok örneği çıkışı ölçeklendirir zaman işlevi yalnızca tek bir örneğini çalıştığından emin olmak için bir dinleyici düzeyi Singleton işlev kilitlemek (`[Singleton(Mode = SingletonMode.Listener)]`). Dinleyici kilitleri JobHost başlatmada alınan. Tüm üç ölçeklendirilmiş örnekleri aynı anda başlatırsanız, yalnızca bir örneği kilit alır ve yalnızca bir dinleyici başlatır.

### <a name="scope-values"></a>Kapsam değerleri

Belirleyebileceğiniz bir **kapsam ifade/değer** işlevi bu kapsamdaki tüm yürütmeleri seri hale sağlayacak Singleton üzerinde. Daha ayrıntılı bu şekilde kilitleme uygulama için belirli bir düzeyde, işlevi için paralellik gereksinimlerinize göre dikte gibi diğer çağrılarını serileştirirken izin verebilirsiniz. Örneğin, aşağıdaki örnekte kapsam ifade bağlar `Region` gelen iletinin değeri. Sıranın bölgeler "Doğu", "Doğu" ve "Batı" sırasıyla sonra "Doğu", "Batı" paralel olanla yürütülen bölge iletisiyle çalışırken seri olarak yürütülecek bölge iletileri 3 iletilerde içeriyorsa.

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

Kilit varsayılan kapsamı `SingletonScope.Function` kilit kapsamı (blob kira yolu) anlamına gelen tam işlev adını bağlı. İşlevlerinin kilitlemek için belirtmek `SingletonScope.Host` ve aynı tüm aynı anda çalıştırmak istemediğiniz işlevleri arasında olan bir kapsam kimliği adı kullanın. Aşağıdaki örnekte, yalnızca bir örneği `AddItem` veya `RemoveItem` bir seferde çalıştırır:

```charp
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

### <a name="viewing-lease-blobs"></a>Görüntüleme kira BLOB'ları

WebJobs SDK'sı [Azure blob kira](../storage/common/storage-concurrency.md#pessimistic-concurrency-for-blobs) altında dağıtılmış kilitleme uygulamak için kapsar. Singleton tarafından kullanılan kira BLOB'ları bulunabilir `azure-webjobs-host` kapsayıcısında `AzureWebJobsStorage` depolama hesabı yolu "kilitleri" altında. Örneğin, ilk kira blob yolu `ProcessImage` daha önce gösterilen örnek olabilir `locks/061851c758f04938a4426aa9ab3869c0/WebJobs.Functions.ProcessImage`. Tüm yolları Bu servis talebi 061851c758f04938a4426aa9ab3869c0 JobHost Kimliğini de içerir.

## <a name="async-functions"></a>Zaman uyumsuz işlevleri

Kod zaman uyumsuz işlevleri hakkında daha fazla bilgi için üzerinde Azure işlevleri belgelerine bakın. [zaman uyumsuz işlevleri](../azure-functions/functions-dotnet-class-library.md#async).

## <a name="cancellation-tokens"></a>İptal belirteçleri

Azure işlevleri belgelerine bakın iptal belirteçlerini nasıl ele alınacağını hakkında daha fazla bilgi için [iptal belirteçlerini ve normal şekilde kapatılmasını](../azure-functions/functions-dotnet-class-library.md#cancellation-tokens).

## <a name="multiple-instances"></a>Birden çok örneği

Web uygulamanız birden çok örneklerinde çalışır, bir sürekli Webjob'un için Tetikleyicileri dinleme ve işlevleri çağırma her örneğinde çalışır. Çeşitli tetikleyici bağlamaları verimli bir şekilde iş birliğine dayalı olarak örnekleri arasında paylaşılacak tasarlanmış böylece daha fazla örneklerine ölçeğini daha fazla yükü işlemek izin verir.

Kuyruk ve blob Tetikleyiciler otomatik olarak bir işlev bir kuyruk iletisi işlenmesini önlemek veya birden çok kez blob; İşlevler ıdempotent olması gerekmez.

Belirli bir zamanlanan saatte çalıştıran birden fazla işlev örneğini alma şekilde Zamanlayıcı tetikleyicisi otomatik olarak Zamanlayıcı çalıştığında, yalnızca bir örneğini sağlar.

Yalnızca bir örneğini emin olmak istiyorsanız bile ana web uygulamanızın birden fazla örneği bulunduğunda bir işlevi çalıştırır, kullanabileceğiniz [Singleton](#singleton) özniteliği.
    
## <a name="filters"></a>Filtreler 

İşlev filtreleri (Önizleme) kendi mantığı ile Web işleri yürütme ardışık düzeni özelleştirmek için bir yol sağlar. Filtreler benzer [ASP.NET Core filtreleri](https://docs.microsoft.com/aspnet/core/mvc/controllers/filters). İşlevler veya sınıfları için uygulanan bildirim temelli öznitelik olarak uygulanabilir. Daha fazla bilgi için bkz: [işlevi filtreleri](https://github.com/Azure/azure-webjobs-sdk/wiki/Function-Filters).

## <a name="logging-and-monitoring"></a>Günlüğe kaydetme ve izleme

ASP.NET için geliştirilmiş günlük framework öneririz ve [başlama](webjobs-sdk-get-started.md) makale nasıl kullanılacağını gösterir. 

### <a name="log-filtering"></a>Günlük filtreleme

Tarafından oluşturulan her günlük bir `ILogger` örneği olan ilişkili bir `Category` ve `Level`. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) bir numaralandırma ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|None        | 6 |

Her kategori bağımsız olarak belirli bir filtrelenebilir [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel). Örneğin, ancak yalnızca blob tetikleyici işlemek için tüm günlükleri görmek isteyebilirsiniz `Error` ve şey için daha yüksek.

Filtreleme kurallarını belirtmek kolaylaştırmak için Web işleri SDK'si sağlar `LogCategoryFilter` , geçirilebilir birçok Application Insights ve konsol dahil olmak üzere mevcut günlük sağlayıcıları.

`LogCategoryFilter` Sahip bir `Default` başlangıç değeri özelliğiyle `Information`, düzeylerine sahip herhangi bir iletisi anlamına `Information`, `Warning`, `Error`, veya `Critical` düzeylerinesahipherhangibiriletisiancakkaydedilir`Debug` veya `Trace` hemen filtrelenir.

`CategoryLevels` Özelliği günlük çıktısı ince ayar yapabilirsiniz şekilde belirli kategorileri için günlük düzeyleri belirtmenize olanak verir. İçinde hiçbir eşleşme bulunamazsa `CategoryLevels` sözlük, filtre döner geri `Default` İletiyi filtrelemekte karar verirken değeri.

Aşağıdaki örnek, varsayılan olarak tüm günlüklerine filtreleyen bir filtre oluşturur `Warning` düzeyi. Kategorileri `Function` veya `Host.Results` adresindeki filtre `Error` düzeyi. `LogCategoryFilter` Kaydedilen tüm geçerli kategoriye karşılaştırır `CategoryLevels` ve en uzun eşleşmeyi seçer. Bunun anlamı `Debug` düzeyi kayıtlı `Host.Triggers` eşleşir `Host.Triggers.Queue` veya `Host.Triggers.Blob`. Bu, her biri eklemek zorunda kalmadan daha geniş kategorilere denetlemenize olanak sağlar.

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

### <a name="custom-telemetry-for-application-insights"></a>Application Insights için özel telemetri

Dahili olarak, `TelemetryClient` WebJobs SDK'sı için Application Insights sağlayıcı tarafından oluşturulan [ServerTelemetryChannel](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/ServerTelemetryChannel/ServerTelemetryChannel.cs). Application Insights uç noktası kullanılamıyor veya azaltma gelen istekleri, bu kanal olduğunda [web uygulamanızın dosya sisteminde istekleri kaydeder ve daha sonra yeniden gönderen](http://apmtips.com/blog/2015/09/03/more-telemetry-channels).

`TelemetryClient` Uygulayan bir sınıf tarafından oluşturulan `ITelemetryClientFactory`. Varsayılan olarak, [DefaultTelemetryClientFactory](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/DefaultTelemetryClientFactory.cs).

Application Insights ardışık düzen herhangi bir kısmını değiştirmek istiyorsanız, kendi sağlayabilirsiniz `ITelemetryClientFactory`, ve ana sınıfınız oluşturmak için kullanacağı bir `TelemetryClient`. Örneğin, bu kodu geçersiz kılmaları `DefaultTelemetryClientFactory` bir özelliği değiştirmek için `ServerTelemetryChannel`:

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

SamplingPercentageEstimatorSettings nesne yapılandırır [Uyarlamalı örnekleme](https://docs.microsoft.com/azure/application-insights/app-insights-sampling#adaptive-sampling-at-your-web-server). Başka bir deyişle, belirli yüksek hacimli senaryolarda, seçili verilerin bir alt kümesini telemetri App Insights sunucusuna gönderir.

Telemetri Fabrika oluşturduktan sonra içinde için Application Insights oturum açma sağlayıcısı geçirin:

```csharp
var clientFactory = new CustomTelemetryClientFactory(instrumentationKey, filter.Filter);

config.LoggerFactory = new LoggerFactory()
    .AddApplicationInsights(clientFactory);
```

## <a id="nextsteps"></a> Sonraki adımlar

Bu kılavuz WebJobs SDK ile çalışmak için genel senaryolar nasıl ele alınacağını gösteren kod parçacıkları sağlamıştır. Tüm örnekleri için bkz: [azure webjobs sdk örnekleri](https://github.com/Azure/azure-webjobs-sdk-samples).