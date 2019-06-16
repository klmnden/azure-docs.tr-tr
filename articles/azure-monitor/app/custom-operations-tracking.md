---
title: Azure Application Insights .NET SDK ile özel işlemleri izleme | Microsoft Docs
description: Azure Application Insights .NET SDK ile özel işlemleri izleme
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/30/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: ae6e0e186f5cc0c9e3f0cd02d45d57c079eb3539
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60900898"
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Application Insights .NET SDK ile özel işlemleri izleme

Azure Application Insights SDK'ları, gelen HTTP isteklerini ve HTTP isteklerini ve SQL sorguları gibi bağımlı hizmetler çağrıları otomatik olarak izler. İzleme ve bağıntı istekleri ve bağımlılıkları birleştiren bu uygulamayı tüm mikro hizmetler arasında tüm uygulamanın yanıt verme hızını ve güvenilirliğini görünürlük sağlar. 

Genel olarak desteklenen uygulama desenleri sınıfının yoktur. Uygun tür desenlerini izlemek için el ile kod araçları gerekir. Bu makale, işleme ve uzun süre çalışan arka plan görevleri çalıştırmanın özel kuyruk gibi el ile izleme gerektirebilecek birkaç desen kapsar.

Bu belge, Application Insights SDK'sı ile özel işlemleri izleme konusunda rehberlik sağlar. Bu belge için geçerlidir:

- .NET (Temel SDK olarak da bilinir) sürüm 2.4 + için Application Insights.
- 2\.4 + web uygulamaları (ASP.NET çalışan) sürümü için Application Insights.
- ASP.NET Core 2.1 + sürümü için Application Insights.

## <a name="overview"></a>Genel Bakış
Bir uygulama tarafından çalıştırılan mantıksal bir parça sahip çalışma bir işlemdir. Bu, başlangıç saati, süre, sonuç ve sonuç kullanıcı adı ve özellikler gibi yürütme bağlamında, bir adı vardır. İşlem A, B işlemi tarafından başlatıldı. ardından işlemi B A. için üst öğe olarak ayarlanır Bir işlem yalnızca bir üste sahip olabilir, ancak çok sayıda alt işlemi olabilir. İşlemler ve telemetri bağıntısı hakkında daha fazla bilgi için bkz. [Azure Application Insights telemetri bağıntısı](correlation.md).

Application Insights .NET SDK işlemi soyut sınıfı tarafından açıklanan [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Extensibility/Implementation/OperationTelemetry.cs) ve alt öğelerini [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/RequestTelemetry.cs) ve [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Gelen işlem izleme 
Application Insights web SDK'sı, HTTP istekleri bir IIS işlem hattında çalıştırılan ASP.NET uygulamaları ve tüm ASP.NET Core uygulamaları için otomatik olarak toplar. Diğer platformlar ve çerçeveler için topluluk tarafından desteklenen çözümü vardır. Uygulama herhangi bir standart veya topluluk tarafından desteklenen çözümler tarafından desteklenmiyorsa, ancak, bunu el ile işaretleyebilir.

Özel İzleme gerektiren başka bir kuyruktan öğeleri alır bir çalışan örnektir. Bazı sıralar için bu kuyruğa bir ileti ekleyin çağrısı bağımlılık olarak izlenir. Ancak, ileti işleme açıklayan yüksek düzeyli işlem otomatik olarak toplanmaz.

Bu işlemleri nasıl izlenebilir görelim.

Yüksek bir düzeyde, görev oluşturmaktır `RequestTelemetry` ve bilinen özellikleri ayarlayın. İşlem tamamlandıktan sonra telemetri izleyin. Aşağıdaki örnek, bu görevi gösterir.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP isteğinde Owın şirket içinde barındırılan uygulama
Bu örnekte, şunlara göre izleme bağlamı yayılır [HTTP protokolü için bağıntı](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Burada açıklanan üst bilgileri almak beklemeniz gerekir.

```csharp
public class ApplicationInsightsMiddleware : OwinMiddleware
{
    private readonly TelemetryClient telemetryClient = new TelemetryClient(TelemetryConfiguration.Active);
    
    public ApplicationInsightsMiddleware(OwinMiddleware next) : base(next) {}

    public override async Task Invoke(IOwinContext context)
    {
        // Let's create and start RequestTelemetry.
        var requestTelemetry = new RequestTelemetry
        {
            Name = $"{context.Request.Method} {context.Request.Uri.GetLeftPart(UriPartial.Path)}"
        };

        // If there is a Request-Id received from the upstream service, set the telemetry context accordingly.
        if (context.Request.Headers.ContainsKey("Request-Id"))
        {
            var requestId = context.Request.Headers.Get("Request-Id");
            // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
            requestTelemetry.Context.Operation.Id = GetOperationId(requestId);
            requestTelemetry.Context.Operation.ParentId = requestId;
        }

        // StartOperation is a helper method that allows correlation of 
        // current operations with nested operations/telemetry
        // and initializes start time and duration on telemetry items.
        var operation = telemetryClient.StartOperation(requestTelemetry);

        // Process the request.
        try
        {
            await Next.Invoke(context);
        }
        catch (Exception e)
        {
            requestTelemetry.Success = false;
            telemetryClient.TrackException(e);
            throw;
        }
        finally
        {
            // Update status code and success as appropriate.
            if (context.Response != null)
            {
                requestTelemetry.ResponseCode = context.Response.StatusCode.ToString();
                requestTelemetry.Success = context.Response.StatusCode >= 200 && context.Response.StatusCode <= 299;
            }
            else
            {
                requestTelemetry.Success = false;
            }

            // Now it's time to stop the operation (and track telemetry).
            telemetryClient.StopOperation(operation);
        }
    }
    
    public static string GetOperationId(string id)
    {
        // Returns the root ID from the '|' to the first '.' if any.
        int rootEnd = id.IndexOf('.');
        if (rootEnd < 0)
            rootEnd = id.Length;

        int rootStart = id[0] == '|' ? 1 : 0;
        return id.Substring(rootStart, rootEnd - rootStart);
    }
}
```

Bağıntı için HTTP protokolü ayrıca bildirir `Correlation-Context` başlığı. Ancak, kolaylık olması için burada atlanmıştır.

## <a name="queue-instrumentation"></a>Kuyruk izleme
Varken [HTTP protokolü için bağıntı](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) HTTP isteği ile bağıntı ayrıntıları geçirmek için kuyruk iletisi aynı ayrıntılarını nasıl geçirilir tanımlamak her kuyruk Protokolü vardır. İleti yükü kodlanacak bağlamı geçirme ek meta veriler ve bazıları (gibi Azure depolama kuyruğu) gerektiren bazı kuyruk protokolündeki (AMQP) sağlar.

### <a name="service-bus-queue"></a>Service Bus kuyruğu
Application Insights, Service Bus Mesajlaşması çağrıları yeni izler [.NET için Microsoft Azure Service Bus istemci](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/) sürüm 3.0.0 ve daha yüksek.
Kullanırsanız [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) iletileri işlemek üzere işiniz: hizmetiniz tarafından yapılan tüm Service Bus çağrıları otomatik olarak izlenir ve diğer telemetriyi öğeleriyle ilişkili. Başvurmak [Microsoft Application Insights ile izleme Service Bus istemci](../../service-bus-messaging/service-bus-end-to-end-tracing.md) el ile iletilerinin işlenme.

Kullanırsanız [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) örnek, daha fazla - okuma paket izlemek (ve ilişkilendirmenize olanak) nasıl göstermek için Service Bus Service Bus kuyruğu AMQP protokolünü kullanır ve Application Insights'ı değil çağırır Kuyruk işlemleri otomatik olarak izler.
Bağıntı tanımlayıcılar içinde ileti özellikleri geçirilir.

#### <a name="enqueue"></a>Sıraya alma

```csharp
public async Task Enqueue(string payload)
{
    // StartOperation is a helper method that initializes the telemetry item
    // and allows correlation of this operation with its parent and children.
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queueName);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queueName;

    var message = new BrokeredMessage(payload);
    // Service Bus queue allows the property bag to pass along with the message.
    // We will use them to pass our correlation identifiers (and other context)
    // to the consumer.
    message.Properties.Add("ParentId", operation.Telemetry.Id);
    message.Properties.Add("RootId", operation.Telemetry.Context.Operation.Id);

    try
    {
        await queue.SendAsync(message);
        
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = true;
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Set operation.Telemetry Success and ResponseCode here.
        operation.Telemetry.Success = false;
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

#### <a name="process"></a>Process
```csharp
public async Task Process(BrokeredMessage message)
{
    // After the message is taken from the queue, create RequestTelemetry to track its processing.
    // It might also make sense to get the name from the message.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };

    var rootId = message.Properties["RootId"].ToString();
    var parentId = message.Properties["ParentId"].ToString();
    // Get the operation ID from the Request-Id (if you follow the HTTP Protocol for Correlation).
    requestTelemetry.Context.Operation.Id = rootId;
    requestTelemetry.Context.Operation.ParentId = parentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

### <a name="azure-storage-queue"></a>Azure depolama kuyruğu
Aşağıdaki örnek nasıl izleneceğini gösteren [Azure depolama kuyruğu](../../storage/queues/storage-dotnet-how-to-use-queues.md) işlemler ve üretici, tüketici ve Azure depolama arasındaki performanstaki telemetri. 

Depolama kuyruğu bir HTTP API'SİNİN vardır. Kuyruğa tüm çağrıları HTTP istekleri için Application Insights bağımlılık toplayıcı tarafından izlenir.
Olduğundan emin olun `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` içinde `applicationInsights.config`. Sahip değilseniz, programlı olarak açıklandığı gibi ekleyin [filtreleme ve Azure Application Insights SDK'da önişleme](../../azure-monitor/app/api-filtering-sampling.md).

Application Insights'ı el ile yapılandırın, oluşturma ve başlatma emin `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` benzer şekilde:
 
```csharp
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

Application Insights işlem kimliği depolama istek kimliği ile ilişkilendirmek isteyebilirsiniz Ayarlama ve depolama istek istemci ve sunucu istek kimliği alma hakkında daha fazla bilgi için bkz. [izleme, tanılama ve Azure depolama sorunlarını giderme](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Sıraya alma
Depolama kuyrukları, HTTP API desteklediğinden, sıra ile yapılan tüm işlemlerde Application Insights tarafından otomatik olarak izlenir. Çoğu durumda, bu araçları yeterli olmalıdır. Ancak, tüketici tarafında izlemeleri üretici izlemeleri ile ilişkilendirmek için bazı bağıntı bağlam benzer şekilde nasıl bağıntı için HTTP protokolünde yaparız için geçirmeniz gerekir. 

Bu örnek nasıl izleneceğini gösteren `Enqueue` işlemi. Şunları yapabilirsiniz:

 - **Yeniden deneme (varsa) bağıntısını**: Bunların tümü, üst bir ortak sahip olan `Enqueue` işlemi. Aksi takdirde, gelen istek alt öğe olarak izlenen. Birden çok mantıksal istekler kuyruğa varsa, yeniden denemeler arama sonuçlandı bulmak zor olabilir.
 - **Depolama günlüklerini (varsa ve gerektiğinde) bağıntısını**: Bunlar, Application Insights telemetri ile bağıntılı.

`Enqueue` Alt öğesi üst işlemi (örneğin, gelen HTTP isteği) bir işlemdir. HTTP bağımlılık çağrısı alt öğesi olan `Enqueue` işlemi ve gelen isteğin en alt:

```csharp
public async Task Enqueue(CloudQueue queue, string message)
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>("enqueue " + queue.Name);
    operation.Telemetry.Type = "Queue";
    operation.Telemetry.Data = "Enqueue " + queue.Name;

    // MessagePayload represents your custom message and also serializes correlation identifiers into payload.
    // For example, if you choose to pass payload serialized to JSON, it might look like
    // {'RootId' : 'some-id', 'ParentId' : '|some-id.1.2.3.', 'message' : 'your message to process'}
    var jsonPayload = JsonConvert.SerializeObject(new MessagePayload
    {
        RootId = operation.Telemetry.Context.Operation.Id,
        ParentId = operation.Telemetry.Id,
        Payload = message
    });
    
    CloudQueueMessage queueMessage = new CloudQueueMessage(jsonPayload);

    // Add operation.Telemetry.Id to the OperationContext to correlate Storage logs and Application Insights telemetry.
    OperationContext context = new OperationContext { ClientRequestID = operation.Telemetry.Id};

    try
    {
        await queue.AddMessageAsync(queueMessage, null, null, new QueueRequestOptions(), context);
    }
    catch (StorageException e)
    {
        operation.Telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        operation.Telemetry.Success = false;
        operation.Telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}  
```

Uygulamanızın telemetri miktarını azaltmak için raporlar veya izlemek istemiyorsanız `Enqueue` işlem başka amaçlarla kullanmak için `Activity` doğrudan API:

- (Oluşturup başlatın) yeni bir `Activity` yerine Application Insights işlemi başlatılıyor. Bunu *değil* işlem adı dışında bulunan herhangi bir özelliği atamanız gerekir.
- Seri hale getirme `yourActivity.Id` yerine iletisi yüküne `operation.Telemetry.Id`. Ayrıca `Activity.Current.Id`.


#### <a name="dequeue"></a>Sıradan Çıkarma
Benzer şekilde `Enqueue`, depolama kuyruğu için gerçek bir HTTP isteği, Application Insights tarafından otomatik olarak izlenir. Ancak, `Enqueue` işlemi üst bağlamında, gelen bir istek bağlamı gibi büyük olasılıkla olur. Application Insights SDK'ları otomatik olarak bu tür bir işlem (ve kendi HTTP bölümü) üst istek ve aynı kapsamda bildirilen diğer telemetri ile ilişkilendirin.

`Dequeue` İşlemdir zor. Application Insights SDK'sı, HTTP isteklerini otomatik olarak izler. Bununla birlikte, iletinin ayrıştırılır kadar bağıntı bağlam bilmez. Telemetri geri kalanı ile ileti almak için HTTP isteği ilişkilendirmek mümkün değildir.

Çoğu durumda, HTTP isteği kuyruğa diğer izlemeleri ile ilişkilendirmek yararlı olabilir. Aşağıdaki örnek nasıl yapılacağını göstermektedir:

```csharp
public async Task<MessagePayload> Dequeue(CloudQueue queue)
{
    var telemetry = new DependencyTelemetry
    {
        Type = "Queue",
        Name = "Dequeue " + queue.Name
    };

    telemetry.Start();

    try
    {
        var message = await queue.GetMessageAsync();

        if (message != null)
        {
            var payload = JsonConvert.DeserializeObject<MessagePayload>(message.AsString);

            // If there is a message, we want to correlate the Dequeue operation with processing.
            // However, we will only know what correlation ID to use after we get it from the message,
            // so we will report telemetry after we know the IDs.
            telemetry.Context.Operation.Id = payload.RootId;
            telemetry.Context.Operation.ParentId = payload.ParentId;

            // Delete the message.
            return payload;
        }
    }
    catch (StorageException e)
    {
        telemetry.Properties.Add("AzureServiceRequestID", e.RequestInformation.ServiceRequestID);
        telemetry.Success = false;
        telemetry.ResultCode = e.RequestInformation.HttpStatusCode.ToString();
        telemetryClient.TrackException(e);
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetry.Stop();
        telemetryClient.Track(telemetry);
    }

    return null;
}
```

#### <a name="process"></a>Process

Aşağıdaki örnekte, bir gelen iletiyi bir şekilde benzer şekilde gelen HTTP isteği izlenir:

```csharp
public async Task Process(MessagePayload message)
{
    // After the message is dequeued from the queue, create RequestTelemetry to track its processing.
    RequestTelemetry requestTelemetry = new RequestTelemetry { Name = "Dequeue " + queueName };
    // It might also make sense to get the name from the message.
    requestTelemetry.Context.Operation.Id = message.RootId;
    requestTelemetry.Context.Operation.ParentId = message.ParentId;

    var operation = telemetryClient.StartOperation(requestTelemetry);

    try
    {
        await ProcessMessage();
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        throw;
    }
    finally
    {
        // Update status code and success as appropriate.
        telemetryClient.StopOperation(operation);
    }
}
```

Benzer şekilde, diğer kuyruk işlemleri izleme eklenmiş. Benzer şekilde bir göz atma işlemi bir sıradan çıkarma işlemi işaretlenmiş durumda. Sıra yönetim işlemleri izleme gerekli değildir. Application Insights HTTP gibi işlemleri izler ve çoğu durumda yeterlidir.

İleti silme izleme, işlem (bağıntı) tanımlayıcıları emin olun. Alternatif olarak, `Activity` API. Ardından Application Insights SDK'sı, sizin için gerçekleştirdiğinden, telemetri öğelerde işlemi tanımlayıcıları ayarlamanız gerekmez:

- Yeni bir `Activity` sonra sıradan bir öğe aradığınızı bulacaksınız.
- Kullanım `Activity.SetParentId(message.ParentId)` tüketici ve üretici günlüklerini ilişkilendirmek için.
- Başlangıç `Activity`.
- İzleme sıradan çıkarma işlemi ve silme işlemleri kullanarak `Start/StopOperation` Yardımcıları. Aynı zaman uyumsuz denetim akışı (yürütme bağlamı) yapın. Bu şekilde, bunlar düzgün bağıntılı.
- Durdur `Activity`.
- Kullanım `Start/StopOperation`, veya çağrı `Track` telemetri el ile.

### <a name="batch-processing"></a>Toplu işlem
Bazı kuyruklar, bir istek ile birden çok iletiyi sıradan çıkarma. Bu tür iletileri işleme, büyük olasılıkla bağımsızdır ve farklı mantıksal işlemlerine ait. Bu durumda, ilişkilendirmek mümkün değildir `Dequeue` belirli ileti işleme işlemi.

Her ileti kendi zaman uyumsuz denetim akışında işlenmelidir. Daha fazla bilgi için [izleme giden bağımlılıklar](#outgoing-dependencies-tracking) bölümü.

## <a name="long-running-background-tasks"></a>Uzun süre çalışan arka plan görevleri

Bazı uygulamalar tarafından kullanıcı isteklerini kaynaklanabilir uzun süre çalışan işlemleri başlatın. İzleme/izleme açısından bakıldığında, istek veya bağımlılık İzleme'den farklı değildir: 

```csharp
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<DependencyTelemetry>(taskName);
    operation.Telemetry.Type = "Background";
    try
    {
        int progress = 0;
        while (progress < 100)
        {
            // Process the task.
            telemetryClient.TrackTrace($"done {progress++}%");
        }
        // Update status code and success as appropriate.
    }
    catch (Exception e)
    {
        telemetryClient.TrackException(e);
        // Update status code and success as appropriate.
        throw;
    }
    finally
    {
        telemetryClient.StopOperation(operation);
    }
}
```

Bu örnekte, `telemetryClient.StartOperation` oluşturur `DependencyTelemetry` ve bağıntı bağlam doldurur. Sahip olduğunuz işlemi zamanlanan gelen istekler tarafından oluşturulan bir üst işlem varsayalım. Sürece `BackgroundTask` aynı zaman uyumsuz olarak başlatır denetim olarak gelen bir istek akışı, bu üst işlem ile ilişkilendirilir. `BackgroundTask` ve tüm iç içe geçmiş telemetri öğelerinin bile isteği sona erdikten sonra neden olan istek ile otomatik olarak ilişkilendirilir.

Görev, herhangi bir işlem yok arka plan iş parçacığından başladığında (`Activity`) ilişkili `BackgroundTask` herhangi bir üst sahip değil. Ancak, bu işlem iç içe geçmiş. Görev bildirilen tüm telemetri öğelerinin bağıntılı olan `DependencyTelemetry` oluşturulan `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>İzleme giden bağımlılıklar
Kendi bağımlılık türü veya Application Insights tarafından desteklenmeyen bir işlem izleyebilirsiniz.

`Enqueue` Yöntemi Service Bus kuyruğu veya depolama kuyruğu gibi özel izleme için örnek olarak hizmet verebilir.

Özel bağımlılık izleme için genel yaklaşım olmaktır:

- Çağrı `TelemetryClient.StartOperation` dolduran (uzantı) metodu `DependencyTelemetry` bağıntı ve diğer bazı özellikler için gerekli olan özellikleri (başlangıç zamanı damgası, süre).
- Özel diğer özellikleri ayarlamak `DependencyTelemetry`adı ve gereksinim duyduğunuz diğer bağlam gibi.
- Bağımlılık çağrısı ve bekleyin olun.
- İşlemi durdurmak `StopOperation` , tamamlandığında.
- Özel durumları işler.

```csharp
public async Task RunMyTaskAsync()
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>("task 1"))
    {
        try 
        {
            var myTask = await StartMyTaskAsync();
            // Update status code and success as appropriate.
        }
        catch(...) 
        {
            // Update status code and success as appropriate.
        }
    }
}
```

İşlem disposing neden işlemi durdurulacak çağırmak yerine yapabilirsiniz şekilde `StopOperation`.

*Uyarı*: Bazı durumlarda unhanded özel durum olabilir [önlemek](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-finally) `finally` işlemleri izlenmez için çağrılabilir.

### <a name="parallel-operations-processing-and-tracking"></a>İşleme paralel işlemler ve izleme

`StopOperation` yalnızca başlatılan işlemi durdurur. Geçerli çalışan işlemi durdurmak istediğiniz bir eşleşmiyorsa `StopOperation` hiçbir şey yapmaz. Paralel aynı yürütme bağlamında birden çok işlem başlamadan bu durum meydana gelebilir:

```csharp
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
var firstTask = RunMyTaskAsync();

var secondOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 2");
var secondTask = RunMyTaskAsync();

await firstTask;

// FAILURE!!! This will do nothing and will not report telemetry for the first operation
// as currently secondOperation is active.
telemetryClient.StopOperation(firstOperation); 

await secondTask;
```

Arama her zaman emin olun `StartOperation` ve işlem aynı işlemde **zaman uyumsuz** paralel olarak çalışan işlemleri yalıtmak için yöntemi. İşlem zaman uyumlu değilse (veya zaman uyumsuz değil) ile izlemek ve sarmalama işlemi `Task.Run`:

```csharp
public void RunMyTask(string name)
{
    using (var operation = telemetryClient.StartOperation<DependencyTelemetry>(name))
    {
        Process();
        // Update status code and success as appropriate.
    }
}

public async Task RunAllTasks()
{
    var task1 = Task.Run(() => RunMyTask("task 1"));
    var task2 = Task.Run(() => RunMyTask("task 2"));
    
    await Task.WhenAll(task1, task2);
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Temel bilgileri öğrenmek [telemetri bağıntısı](correlation.md) Application ınsights.
- Bkz: [veri modeli](../../azure-monitor/app/data-model.md) için Application Insights türleri ve veri modeli.
- Özel rapor [olaylar ve ölçümler](../../azure-monitor/app/api-custom-events-metrics.md) Application ınsights.
- Standart denetleyin [yapılandırma](configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) bağlam özellikleri koleksiyonu.
- Denetleme [System.Diagnostics.Activity Kullanıcı Kılavuzu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) nasıl biz telemetrinin bağıntısını görmek için.
