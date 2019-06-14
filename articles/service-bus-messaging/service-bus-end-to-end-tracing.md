---
title: Azure Service Bus uçtan uca izleme ve tanılama | Microsoft Docs
description: Service Bus istemci tanıları ve uçtan uca izleme genel bakış
services: service-bus-messaging
documentationcenter: ''
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 6e5895392db1d75a985674bf2f878a84bc8dd926
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60311021"
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Dağıtılmış izleme ve Service Bus mesajlaşması ile bağıntı

Mikro hizmetler geliştirmede karşılaşılan izleme işlemi için işleme katılan tüm hizmetleri aracılığıyla bir istemciden becerisidir. Hata ayıklama için Performans Analizi, A yararlıdır / B testi ve diğer tanılama tipik senaryolar.
Bu sorunun bir bölümü mantıksal parçalara işin izlemektir. Bu iletiyi işlemeyi sonucu, gecikme süresi ve dış bağımlılık çağrılarını içerir. Bu işlem sınırları dışında tanılamayı olayların bağıntı başka bir parçasıdır.

Bir üretici bir kuyruk aracılığıyla ileti gönderdiğinde, genellikle bazı diğer istemci veya hizmet tarafından başlatılan bazı diğer mantıksal işlemi, kapsam içinde gerçekleşir. Bir ileti aldıktan sonra aynı işlemi tüketici tarafından devam. Üretici ve Tüketici (hem işlemi diğer hizmetler), büyük olasılıkla sonuç ve işlem akışı izlemek için telemetri olayları Yayımla. Bu tür olayları ve izleme işlemi uçtan uca bağıntısı kurmak için her olay bir izleme bağlamı ile damgalamak telemetri rapor her hizmet içerir.

Microsoft Azure Service Bus Mesajlaşma üreticilerin ve tüketicilerin gibi izleme bağlamı geçirmek kullanması gereken yükü özellikleri tanımladı.
Protokol dayanır [HTTP bağıntı Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).

| Özellik Adı        | Açıklama                                                 |
|----------------------|-------------------------------------------------------------|
|  Tanılama kimliği       | Dış bir üretici çağrısından kuyruğa benzersiz tanımlayıcısı. Başvurmak [Request-Id HTTP protokolünde](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) stratejinin, konuları ve biçimi |
|  Bağıntı bağlamı | İşlemi bağlam işlemi işleme dahil tüm hizmetlerde yayılır. Daha fazla bilgi için [bağıntı-bağlamında, HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-auto-tracing"></a>Hizmet veri yolu .NET istemci otomatik izleme

Sürüm 3.0.0 ile başlayan [.NET için Microsoft Azure Service Bus istemci](/dotnet/api/microsoft.azure.servicebus.queueclient) izleme sistemlerini veya istemci kod parçasını bağlanabilir izleme ölçümlü izleme noktalarına sağlar.
İzleme istemci tarafı hizmetinden Mesajlaşma Service Bus tüm çağrıları izleme sağlar. İleti işleme ile yapıldıysa [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler), ileti işleme da izleniyor

### <a name="tracking-with-azure-application-insights"></a>Azure Application Insights ile izleme

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) zengin performans izleme özelliğini automagical istek ve bağımlılık izleme gibi özellikleri sağlar.

Proje türüne bağlı olarak, Application Insights SDK'sını yükleyin:
- [ASP.NET](../azure-monitor/app/asp-net.md) -sürüm 2.5-beta2 yükleyin veya üzeri
- [ASP.NET Core](../azure-monitor/app/asp-net-core.md) -sürüm 2.2.0-beta2 yükleyin veya üzeri.
Bu bağlantılar, SDK'sını yükleme, kaynakları oluşturma ve (gerekirse) SDK'sını yapılandırma ayrıntıları sağlar. ASP.NET olmayan uygulamalar için başvurmak [konsol uygulamaları için Azure Application Insights](../azure-monitor/app/console.md) makalesi.

Kullanırsanız [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) iletileri işlemek üzere işiniz: hizmetiniz tarafından yapılan tüm Service Bus çağrıları otomatik olarak izlenir ve diğer telemetriyi öğeleriyle ilişkili. Aksi takdirde, el ile ileti işleme izleme için aşağıdaki örneğe bakın.

#### <a name="trace-message-processing"></a>İzleme ileti işleme

```csharp
private readonly TelemetryClient telemetryClient;

async Task ProcessAsync(Message message)
{
    var activity = message.ExtractActivity();
    
    // If you are using Microsoft.ApplicationInsights package version 2.6-beta or higher, you should call StartOperation<RequestTelemetry>(activity) instead
    using (var operation = telemetryClient.StartOperation<RequestTelemetry>("Process", activity.RootId, activity.ParentId))
    {
        telemetryClient.TrackTrace("Received message");
        try 
        {
           // process message
        }
        catch (Exception ex)
        {
             telemetryClient.TrackException(ex);
             operation.Telemetry.Success = false;
             throw;
        }

        telemetryClient.TrackTrace("Done");
   }
}
```

Bu örnekte, `RequestTelemetry` bir zaman damgası, süresi ve sonucu (başarılı) işlenen her ileti için bildirilir. Telemetriyi de bağıntı özellikler kümesi vardır.
İç içe geçmiş izlemeleri ve özel durum iletisi işleme sırasında bildirilen ayrıca damgalı bunları 'alt' temsil eden bağıntı özellikleriyle `RequestTelemetry`.

İleti işleme sırasında desteklenen dış bileşenler çağrı yapmak durumunda, bunlar da otomatik olarak bağıntılı ve izlenmesi. Başvurmak [Application Insights .NET SDK ile özel işlemleri izleme](../azure-monitor/app/custom-operations-tracking.md) el ile izleme ve bağıntı için.

### <a name="tracking-without-tracing-system"></a>İzleme sistemi olmadan izleme
Durumda, izleme sistemi otomatik Service Bus çağrıları izleme desteklemiyor, bu tür destek bir izleme sistemine veya uygulamanıza ekleme içine arıyor olabilirsiniz. Bu bölümde, Service Bus .NET istemci tarafından gönderilen Tanılama Olayları açıklanmaktadır.  

Hizmet veri yolu .NET istemci .NET izleme temelleri kullanarak izleniyor [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).

`Activity` bir izleme bağlamı sırasında görevi gören `DiagnosticSource` bir bildirim mekanizmasıdır. 

DiagnosticSource olaylar için bir dinleyici yok ise, izleme, sıfır izleme maliyetleri kapalıdır. DiagnosticSource dinleyici için tüm denetim verir:
- Dinleyici, kaynakları ve olayları dinleyecek şekilde denetler.
- Dinleyici denetimleri olayı oranı ve örnekleme
- olayları tam bağlam erişmek ve iletiyi nesnede değişiklik olayı sırasında sağlayan bir yükü ile gönderilir

İle kendinizi alıştırın [DiagnosticSource Kullanıcı Kılavuzu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) uygulama devam etmeden önce.

Service Bus olayları için bir dinleyici Microsoft.Extension.Logger günlükleriyle Yazar ASP.NET Core uygulamasında oluşturalım.
Kullandığı [System.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) (olduğu da DiagnosticSource için abone olmak kolay) DiagnosticSource abone olmak için kitaplığı

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory factory, IApplicationLifetime applicationLifetime)
{
    // configuration...

    var serviceBusLogger = factory.CreateLogger("Microsoft.Azure.ServiceBus");

    IDisposable innerSubscription = null;
    IDisposable outerSubscription = DiagnosticListener.AllListeners.Subscribe(delegate (DiagnosticListener listener)
    {
        // subscribe to the Service Bus DiagnosticSource
        if (listener.Name == "Microsoft.Azure.ServiceBus")
        {
            // receive event from Service Bus DiagnosticSource
            innerSubscription = listener.Subscribe(delegate (KeyValuePair<string, object> evnt)
            {
                // Log operation details once it's done
                if (evnt.Key.EndsWith("Stop"))
                {
                    Activity currentActivity = Activity.Current;
                    TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");
                    serviceBusLogger.LogInformation($"Operation {currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}");
                }
            });
        }
    });

    applicationLifetime.ApplicationStopping.Register(() =>
    {
        outerSubscription?.Dispose();
        innerSubscription?.Dispose();
    });
}
```

Bu örnekte, dinleyici süresi, sonuç, benzersiz tanımlayıcı ve her Service Bus işlemi için başlangıç saati günlüğe kaydeder.

#### <a name="events"></a>Events

Her işlem için iki olay gönderilir: 'Start' ve 'Stop'. En büyük olasılıkla, yalnızca 'Stop' olayları ilgilendiğiniz. Bunlar işleminin sonucu sağlayın, hem de bir etkinlik özellikleri başlangıç saati ve süreyi.

Bir işlem bağlamı dinleyicisi olay yükü sağlar, API gelen parametreleri çoğaltır ve dönüş değeri. Olay yükü 'Stop' olay yükü 'Start', tüm özelliklerini bulunduğundan, 'Start' olay tamamen yoksayabilirsiniz.

Tüm olaylar 'Entity' ve 'Uç noktası' özellikleri de vardır. Bunlar, aşağıdaki tabloda göz ardı edilir
  * `string Entity` --(Kuyruk, konu, vb.) varlık adı
  * `Uri Endpoint` -Service Bus uç noktası URL'si

Her 'Stop' olayının `Status` özelliğiyle `TaskStatus` zaman uyumsuz işlemi tamamlandı, kolaylık olması için aşağıdaki tablodaki atlanır.

İzleme eklenmiş operations tam listesi aşağıda verilmiştir:

| İşlem adı | İzlenen API | Belirli yükü özellikleri|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Send | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | `IList<Message> Messages` -Gönderilen iletilerin listesi |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | `Message Message` -İşlenen ileti<br/>`DateTimeOffset ScheduleEnqueueTimeUtc` -Zamanlanmış iletileri uzaklığı<br/>`long SequenceNumber` -Zamanlanmış iletileri (olay yükü 'Stop') seri numarası |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | `long SequenceNumber` -İptal edilecek metin iletisinin sıra numarası | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) | `int RequestedMessageCount` -En fazla alınan ileti sayısı.<br/>`IList<Message> Messages` -Alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | `int FromSequenceNumber` -Başlangıç noktası toplu iletiler göz atın.<br/>`int RequestedMessageCount` -Alınacak ileti sayısı.<br/>`IList<Message> Messages` -Alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.ReceiveDeferred | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | `IEnumerable<long> SequenceNumbers` -Almak için sıra numaralarını içeren listesi.<br/>`IList<Message> Messages` -Alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | `IList<string> LockTokens` -Kilit belirteçleri tamamlamak için karşılık gelen iletilerin içeren listesi.|
| Microsoft.Azure.ServiceBus.Abandon | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | `string LockToken` -Karşılık gelen bırakılacak iletinin kilit belirteci. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | `string LockToken` -Karşılık gelen ertelenecek iletinin kilit belirteci. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | `string LockToken` -Edilemeyen karşılık gelen iletinin kilit belirteci. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | `string LockToken` -Karşılık gelen iletinin kilidini Yenile kilit belirteci.<br/>`DateTime LockedUntilUtc` -Belirteç sona erme tarihi ve saati UTC biçiminde yeni kilidi. (Olay yükü 'Stop')|
| Microsoft.Azure.ServiceBus.Process | İleti işleyici lambda işlevi içinde sağlanan [IReceiverClient.RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) | `Message Message` -İşlenen ileti. |
| Microsoft.Azure.ServiceBus.ProcessSession | İleti oturumu işleyici lambda işlevi içinde sağlanan [IQueueClient.RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) | `Message Message` -İşlenen ileti.<br/>`IMessageSession Session` -İşlenen oturum |
| Microsoft.Azure.ServiceBus.AddRule | [SubscriptionClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | `RuleDescription Rule` -Kural eklemek için sağlar kural açıklaması. |
| Microsoft.Azure.ServiceBus.RemoveRule | [SubscriptionClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | `string RuleName` -Kaldırmak için kuralın adı. |
| Microsoft.Azure.ServiceBus.GetRules | [SubscriptionClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | `IEnumerable<RuleDescription> Rules` -Abonelikle ilişkili tüm kurallar. (Yalnızca 'Stop' yükü) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | `string SessionId` -SessionID iletileri yok. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | `string SessionId` -SessionID iletileri yok.<br/>`byte [] State` -Oturum durumu (olay yükü 'Stop') |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | `string SessionId` -SessionID iletileri yok.<br/>`byte [] State` -Oturum durumu |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | `string SessionId` -SessionID iletileri yok. |
| Microsoft.Azure.ServiceBus.Exception | İzleme eklenmiş tüm API'leri| `Exception Exception` -Özel durum örneği |

Her durumda, erişebileceğiniz `Activity.Current` , geçerli işlem bağlamı içerir.

#### <a name="logging-additional-properties"></a>Günlük ek özellikleri

`Activity.Current` ayrıntılı bağlamında geçerli işlem ve onun ana öğelerinden sağlar. Daha fazla bilgi için [etkinlik belgeleri](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) daha fazla ayrıntı için.
Service Bus izleme, ek bilgi sağlar `Activity.Current.Tags` -tuttukları `MessageId` ve `SessionId` olduğunda bunlar kullanılabilir.

'Receive' izleyen etkinlikleri 'Özet' ve 'ReceiveDeferred' olay de olabilir `RelatedTo` etiketi. Farklı bir listesini tutar `Diagnostic-Id`(s) sonucunda alınan ileti.
Bu işlemi alınabilmesi için birkaç ilgisiz iletilerine neden. Ayrıca, `Diagnostic-Id` işlemi başladığında, bu etiketi yalnızca 'Process' işlemleri 'Receive' operations bağıntılı böylece bilinmiyor. İleti almak için ne kadar sürdüğünü denetlemek için performans sorunlarını analiz edilirken yararlı olacaktır.

Etiketleri açmak için etiketler ekleme önceki örneğe benzer şekilde, bunlar üzerinde yinelemek için etkilidir 

```csharp
Activity currentActivity = Activity.Current;
TaskStatus status = (TaskStatus)evnt.Value.GetProperty("Status");

var tagsList = new StringBuilder();
foreach (var tags in currentActivity.Tags)
{
    tagsList.Append($", "{tags.Key}={tags.Value}");
}

serviceBusLogger.LogInformation($"{currentActivity.OperationName} is finished, Duration={currentActivity.Duration}, Status={status}, Id={currentActivity.Id}, StartTime={currentActivity.StartTimeUtc}{tagsList}");
```

#### <a name="filtering-and-sampling"></a>Filtreleme ve örnekleme

Bazı durumlarda, performans ek yük ya da depolama tüketimini azaltmak için olayları yalnızca bir kısmını oturum tercih edilir. 'Stop' olaylar yalnızca (önceki örnekte olduğu gibi) ya da örnek olaylarının yüzdesini oturum açılamadı. 
`DiagnosticSource` ile bunu yapmanın bir yolu sağlamak `IsEnabled` koşul. Daha fazla bilgi için [DiagnosticSource Bağlam tabanlı filtreleme](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled` performans etkisini en aza indirmek tek bir işlem için birden çok kez çağrılabilir.

`IsEnabled` aşağıdaki sırayla çağrılır:

1. `IsEnabled(<OperationName>, string entity, null)` Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. 'Start' veya 'Durdur' sonunda dikkat edin. Belirli işlemleri veya Kuyrukları filtrelemek için kullanın. Geri çağırma döndürürse `false`, işlemi için olayları gönderilemedi

   * 'Process' ve 'ProcessSession' işlemleri için de alırsınız `IsEnabled(<OperationName>, string entity, Activity activity)` geri çağırma. Dayalı olarak olayları filtrelemek için kullanmak `activity.Id` veya etiketleri özellikleri.
  
2. `IsEnabled(<OperationName>.Start)` Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. 'Start' olay harekete olup olmadığını denetler. Sonuç, yalnızca 'Start' olay etkiler, ancak daha fazla izleme bağımlı değil.

Var olan hiçbir `IsEnabled` 'Stop' olayı için.

Özel durum, bazı işlem sonucu olup olmadığını `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` çağrılır. Yalnızca 'Özel durum' olaylarına abone olma ve izleme geri kalanını engellemek. Bu durumda, yine de böyle özel durumları işlemek zorunda. Diğer Araçları'nı devre dışı olduğundan, tüketicinin kuyruktan üretici için flow'u için izleme bağlamı beklememeniz gerekir.

Kullanabileceğiniz `IsEnabled` de örnekleme stratejisi uygulayın. Örnekleme temel alarak `Activity.Id` veya `Activity.RootId` (bunu kendi kodunuzu veya izleme sistemi tarafından yayılır sürece) tüm Lastikler arasında tutarlı örnekleme sağlar.

Birden çok varlığı içinde `DiagnosticSource` dinleyicileri aynı kaynak için olduğu kadar olay kabul etmek yalnızca bir dinleyicisi için yeterli `IsEnabled` çağrılacak, garanti edilmez

## <a name="next-steps"></a>Sonraki adımlar

* [Application Insights bağıntı](../azure-monitor/app/correlation.md)
* [Application Insights bağımlılıklarını İzle](../azure-monitor/app/asp-net-dependencies.md) REST, SQL ve diğer dış kaynaklara, yavaşlatmadan olmadığını görmek için.
* [Application Insights .NET SDK ile özel işlemleri izleme](../azure-monitor/app/custom-operations-tracking.md)
