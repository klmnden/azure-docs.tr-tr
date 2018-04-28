---
title: Azure Application Insights .NET SDK ile özel işlemler izlemek | Microsoft Docs
description: Azure Application Insights .NET SDK ile özel işlemler izleme
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/30/2017
ms.author: sergkanz
ms.openlocfilehash: 94424a3d8aad56cf4504cccd8adb1a45523d95e0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="track-custom-operations-with-application-insights-net-sdk"></a>Application Insights .NET SDK ile özel işlemler izleme

Azure uygulama Insights SDK'ları, gelen HTTP isteklerini ve HTTP istekleri ve SQL sorguları gibi bağımlı hizmetleri çağrıları otomatik olarak izler. İzleme ve isteklerinin ve bağımlılıkları bağıntı bu uygulamayı birleştirmek tüm mikro hizmetler arasında tüm uygulama yanıt hızını ve güvenilirlik görünürlük sağlar. 

Genel olarak desteklenen uygulama düzenleri sınıfının yoktur. Bu tür desenlerini izlemenin düzgün yapılabilmesi el ile kod araçları gerektirir. Bu makalede işleme ve uzun süre çalışan arka plan görevleri çalıştıran özel sıra gibi el ile araçları gerektirebilir birkaç desenleri kapsar.

Bu belgede, Application Insights SDK'sı ile özel işlemleri izlemek nasıl Rehber sağlanır. Bu belge için geçerlidir:

- .NET (Temel SDK olarak da bilinir) sürüm 2.4 + için Application Insights.
- Web uygulamaları (ASP.NET çalıştıran) sürüm 2.4 + için Application Insights.
- ASP.NET Core sürüm 2.1 + için Application Insights.

## <a name="overview"></a>Genel Bakış
Bir mantıksal parça sahip bir uygulama tarafından çalıştırılan bir çalışma bir işlemdir. Başlangıç saati, süresi, sonuç ve kullanıcı adı, özellikler ve sonuç gibi yürütme bağlamı bir adı vardır. İşlem A B işlemi tarafından başlatıldı, sonra işlemi B A. için üst öğe olarak ayarlanır Bir işlemi yalnızca bir üst olabilir, ancak birçok alt işlemleri olabilir. İşlemler ve telemetri bağıntı hakkında daha fazla bilgi için bkz: [Azure Application Insights telemetri bağıntı](application-insights-correlation.md).

Application Insights .NET SDK işlemi soyut sınıf tarafından açıklanan [OperationTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/Extensibility/Implementation/OperationTelemetry.cs) ve alt öğeleri [RequestTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/RequestTelemetry.cs) ve [DependencyTelemetry](https://github.com/Microsoft/ApplicationInsights-dotnet/blob/develop/src/Microsoft.ApplicationInsights/DataContracts/DependencyTelemetry.cs).

## <a name="incoming-operations-tracking"></a>Gelen işlemlerini izleme 
Application Insights web SDK IIS ardışık düzeninde çalışan ASP.NET uygulamaları ve tüm ASP.NET Core uygulamaları HTTP isteklerini otomatik olarak toplar. Diğer platformlar ve altyapıları için topluluk tarafından desteklenen çözümleri vardır. Uygulama herhangi bir standart veya topluluk tarafından desteklenen çözümleri tarafından desteklenmiyorsa, ancak, siz onu el ile işaretleyebilir.

Özel İzleme gerektiren başka bir öğeleri kuyruktan alır çalışan bir örnektir. Bazı sıraları, bu sıraya ileti eklemek için arama bağımlılık olarak izlenir. Ancak, ileti işleme açıklayan yüksek düzeyli işlemi otomatik olarak toplanmaz.

Bu tür işlemler nasıl izleneceğini görelim.

Yüksek bir düzeyde, görev oluşturmaktır `RequestTelemetry` ve bilinen özelliklerini ayarlama. İşlemi tamamlandıktan sonra telemetri izler. Aşağıdaki örnekte bu görevin nasıl yapılacağı gösterilmektedir.

### <a name="http-request-in-owin-self-hosted-app"></a>HTTP isteği Owın kendini barındıran uygulama
Bu örnekte, izleme bağlamı göre yayılır [bağıntı için HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Var. açıklanan üstbilgileri almak beklemeniz gerekir.

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

Bağıntı için HTTP protokolü ayrıca bildirir `Correlation-Context` üstbilgi. Ancak, kolaylık sağlamak için burada atlanır.

## <a name="queue-instrumentation"></a>Sıra Araçları
Varken [bağıntı için HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md) HTTP isteği bağıntı ayrıntılarla geçirmek için aynı ayrıntıları kuyruk iletisini nasıl geçtiğini tanımlamak her sıra Protokolü sahiptir. Bazı sıra protokolleri (örneğin, AMQP) geçirme ek meta veri ve diğerlerinin (gibi Azure depolama kuyruğu) ileti yükü kodlanacak bağlam gerektiren izin verir.

### <a name="service-bus-queue"></a>Service Bus Kuyruğu
Application Insights izler Service Bus Mesajlaşma hizmeti çağrıları yeni [.NET için Microsoft Azure ServiceBus istemcisi](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus/) sürüm 3.0.0 ve daha yüksek.
Kullanırsanız [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) iletileri işlemek için yapılır: hizmetiniz tarafından gerçekleştirilen tüm Service Bus çağrıları otomatik olarak izlenir ve diğer telemetri öğeleri ile bağlantılı. Başvurmak [Microsoft Application Insights ile izleme Service Bus istemci](../service-bus-messaging/service-bus-end-to-end-tracing.md) el ile iletilerinin işlenme.

Kullanırsanız [WindowsAzure.ServiceBus](https://www.nuget.org/packages/WindowsAzure.ServiceBus/) örnek, daha fazla - okumadan paket izlemek (ve ilişkilendirmek) nasıl göstermek için hizmet veri yolu hizmet veri yolu kuyruğu AMQP protokolünü kullanır ve Application Insights değil olarak çağırır otomatik olarak kuyruk işlemlerini izler.
Bağıntı tanımlayıcıları ileti özelliklerinde geçirilir.

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

#### <a name="process"></a>İşlem
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
Aşağıdaki örnekte nasıl izleneceğini gösterir [Azure depolama kuyruğu](../storage/queues/storage-dotnet-how-to-use-queues.md) işlemler ve üretici, tüketici ve Azure Storage arasında correlate telemetri. 

Depolama sırası bir HTTP API vardır. Sıranın tüm çağrıları, HTTP isteklerini uygulama Öngörüler bağımlılık toplayıcısı tarafından izlenir.
Olduğundan emin olun `Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer` içinde `applicationInsights.config`. Yoksa, program aracılığıyla açıklanan ekleme [filtreleme ve Azure Application Insights SDK ön işleme](app-insights-api-filtering-sampling.md).

Application Insights el ile yapılandırırsanız, oluşturma ve başlatma emin olun `Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule` benzer şekilde:
 
```csharp
DependencyTrackingTelemetryModule module = new DependencyTrackingTelemetryModule();

// You can prevent correlation header injection to some domains by adding it to the excluded list.
// Make sure you add a Storage endpoint. Otherwise, you might experience request signature validation issues on the Storage service side.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
module.Initialize(TelemetryConfiguration.Active);

// Do not forget to dispose of the module during application shutdown.
```

Application Insights işlem kimliği depolama istek kimliği ile ilişkilendirmek isteyebilirsiniz Ayarlama ve depolama isteği istemci ve sunucu istek kimliği alma hakkında daha fazla bilgi için bkz: [izleme, tanılama ve Azure Storage sorun giderme](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#end-to-end-tracing).

#### <a name="enqueue"></a>Sıraya alma
Depolama kuyrukları HTTP API desteklediğinden, sıranın sahip tüm işlemlerin Application Insights tarafından otomatik olarak izlenir. Çoğu durumda, bu araçları yeterli olacaktır. Ancak, tüketici tarafında izlemeleri üretici izlemeleri ile ilişkilendirmek için bazı bağıntı bağlam benzer şekilde nasıl biz bağıntı için HTTP protokolü bunu için geçmesi gerekir. 

Bu örnek nasıl izleneceğini gösterir `Enqueue` işlemi. Şunları yapabilirsiniz:

 - **(Varsa) yeniden deneme ilişkilendirmek**: hepsi, üst bir ortak sahip olan `Enqueue` işlemi. Aksi durumda, gelen istek alt olarak izlenen. Sıra birden fazla mantıksal isteği varsa, yeniden deneme içinde hangi çağrısı sonuçlandı bulmak zor olabilir.
 - **Depolama günlükleri (varsa ve gerektiğinde) ilişkilendirmek**: Application Insights telemetri ile bağıntılı.

`Enqueue` Üst işlemi (örneğin, bir gelen HTTP istek) alt bir işlemdir. HTTP bağımlılık çağrısı alt öğesi olan `Enqueue` işlemi ve gelen isteğin en alt:

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

Uygulamanızı telemetri miktarını azaltmak için raporlar veya izlemek istemiyorsanız `Enqueue` başka nedenlerle kullanım işlemi `Activity` doğrudan API:

- (Oluşturup başlatın) yeni bir `Activity` yerine Application Insights işlemi başlatılıyor. Bunu *değil* işlem adı dışında bulunan herhangi bir özellik atamanız gerekir.
- Seri hale `yourActivity.Id` yerine ileti yükü olarak `operation.Telemetry.Id`. Aynı zamanda `Activity.Current.Id`.


#### <a name="dequeue"></a>Dequeue
Benzer şekilde `Enqueue`, depolama kuyruğu gerçek bir HTTP isteğine Application Insights tarafından otomatik olarak izlenir. Ancak, `Enqueue` işlemi büyük olasılıkla olur üst bağlamda gelen bir istek bağlamı gibi. Uygulama Insights SDK'ları otomatik olarak bu tür bir işlemi (ve HTTP bölümü) üst isteğin ve aynı kapsamda bildirilen diğer telemetri ile ilişkilendirmek.

`Dequeue` İşlemdir hassas. Application Insights SDK'sı, HTTP isteklerini otomatik olarak izler. Ancak, ileti ayrıştırılır kadar bağıntı bağlam bilmiyor. Telemetri kalanıyla iletisini almak için HTTP isteği ilişkilendirmek mümkün değildir.

Çoğu durumda, sıranın HTTP isteği diğer izlemeleri ile ilişkilendirmek yararlı olabilir. Aşağıdaki örnekte nasıl yapılacağını gösterir:

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

#### <a name="process"></a>İşlem

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

Benzer şekilde, diğer kuyruk işlemlerini izleme eklenmiş. Peek işlemi benzer şekilde dequeue işlem olarak işaretlenir. Kuyruk yönetim işlemlerini izleme gerekli değildir. Application Insights HTTP gibi işlemleri izler ve çoğu durumda yeterlidir.

İleti silinmesini izleme zaman işlemi (bağıntı) tanımlayıcıları ayarladığınızdan emin olun. Alternatif olarak, kullanabileceğiniz `Activity` API. Ardından Application Insights SDK'sı, sizin yerinize yaptığından telemetri öğeler üzerinde işlem tanımlayıcıları ayarlamanız gerekmez:

- Yeni bir `Activity` sıradan bir öğe kurduktan sonra.
- Kullanım `Activity.SetParentId(message.ParentId)` tüketici ve üretici günlükleriyle ilişkilendirmek için.
- Başlat `Activity`.
- İzleme dequeue, işlem ve silme işlemleri kullanarak `Start/StopOperation` Yardımcıları. Aynı zaman uyumsuz denetim akışı (yürütme bağlamı) yapın. Bu şekilde, bunların düzgün şekilde bağıntılı.
- Durdur `Activity`.
- Kullanım `Start/StopOperation`, veya arama `Track` telemetri el ile.

### <a name="batch-processing"></a>Toplu işlem
Bazı kuyruklar, bir istek ile birden fazla ileti dequeue. Bu tür iletileri işleme büyük olasılıkla bağımsızdır ve farklı mantıksal işlemlerini ait. Bu durumda, ilişkilendirmek olası değil `Dequeue` belirli ileti işleme işlemi.

Her ileti kendi zaman uyumsuz denetim akışı işlenmesi. Daha fazla bilgi için bkz: [giden bağımlılıkları izleme](#outgoing-dependencies-tracking) bölümü.

## <a name="long-running-background-tasks"></a>Uzun süre çalışan arka plan görevleri
Bazı uygulamalar kullanıcı isteklerinden kaynaklanabilir uzun süre çalışan işlemlerini başlatın. İzleme/Araçları açısından bakıldığında, istek veya bağımlılık izleme farklı değil: 

```csharp
async Task BackgroundTask()
{
    var operation = telemetryClient.StartOperation<RequestTelemetry>(taskName);
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

Bu örnekte, `telemetryClient.StartOperation` oluşturur `RequestTelemetry` ve bağıntı bağlam doldurur. İşlemi zamanlanan gelen istekler tarafından oluşturulan bir üst işlemi sahip varsayalım. Sürece `BackgroundTask` başlatır aynı zaman uyumsuz olarak gelen bir istek akışı denetle, o üst işlemi ile ilişkilendirilir. `BackgroundTask` ve tüm iç içe geçmiş telemetri öğeler otomatik olarak bile isteği sona erdikten sonra neden isteği ile ilişkili.

Görev, herhangi bir işlem yok arka plan iş parçacığından başladığında (`Activity`) ile ilişkili `BackgroundTask` herhangi bir üst sahip değil. Ancak, bu işlemleri iç içe. Görevden bildirilen tüm telemetri öğeleri bağıntılı olan `RequestTelemetry` oluşturulan `BackgroundTask`.

## <a name="outgoing-dependencies-tracking"></a>Giden bağımlılıkları izleme
Kendi bağımlılık türü veya Application Insights tarafından desteklenmeyen bir işlem izleyebilirsiniz.

`Enqueue` Yöntemi hizmet veri yolu kuyruğu ya da depolama kuyruğu gibi özel izleme için örnek olarak hizmet verebilir.

İçin özel bağımlılık izleme için genel yaklaşım şöyledir:

- Çağrı `TelemetryClient.StartOperation` doldurur (uzantısı) yöntemi `DependencyTelemetry` bağıntı ve bazı diğer özellikler için gerekli olan özellikleri (başlangıç saati Damga, süre).
- Diğer özel özellikleri Ayarla `DependencyTelemetry`adı ve gereksinim duyduğunuz diğer bağlamı gibi.
- Arama ve bekleyin bir bağımlılık olun.
- İşlemi durdurmak `StopOperation` bu tamamlandığında.
- Özel durumları işleme.

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

İşlemi atma neden durdurulacak işlemi yerine arama yapabilirsiniz şekilde `StopOperation`.

*Uyarı*: Bazı durumlarda unhanded özel durum olabilir [engellemek](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-finally) `finally` operations izlenmez şekilde çağrılabilir.

### <a name="parallel-operations-processing-and-tracking"></a>Paralel işlemler işleme ve izleme

`StopOperation` yalnızca başlatıldığından işlemi durdurur. Geçerli çalışan işlemin durdurmak istediğiniz bir eşleşmiyorsa `StopOperation` hiçbir şey yapmaz. Paralel aynı yürütme bağlamı olarak birden çok işlemi başlatırsanız bu durum gerçekleşebilir:

```csharp
var firstOperation = telemetryClient.StartOperation<DependencyTelemetry>("task 1");
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

Her zaman çağrı emin olun `StartOperation` ve işlem aynı işlemde **zaman uyumsuz** yöntemi paralel olarak çalışan işlemlerini yalıtır. İşlem zaman uyumlu ise (veya zaman uyumsuz değil) işlemi kaydırma ve ile izleme `Task.Run`:

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

- Temel bilgileri öğrenmek [telemetri bağıntı](application-insights-correlation.md) Application ınsights'ta.
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Özel rapor [olayları ve ölçümleri](app-insights-api-custom-events-metrics.md) Application Insights için.
- Standart denetleyin [yapılandırma](app-insights-configuration-with-applicationinsights-config.md#telemetry-initializers-aspnet) bağlam özellikleri koleksiyonu için.
- Denetleme [System.Diagnostics.Activity Kullanıcı Kılavuzu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) nasıl biz telemetrinin bağıntısını görmek için.
