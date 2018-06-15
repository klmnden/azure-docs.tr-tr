---
title: Azure Service Bus uçtan uca izleme ve tanılama | Microsoft Docs
description: Hizmet veri yolu istemci tanılama ve uçtan uca izleme genel bakış
services: service-bus-messaging
documentationcenter: ''
author: lmolkova
manager: timlt
editor: ''
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 847056acd2d97391782dcac1874a2739b7f5825c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2018
ms.locfileid: "27741338"
---
# <a name="distributed-tracing-and-correlation-through-service-bus-messaging"></a>Dağıtılmış izleme ve Service Bus Mesajlaşma hizmeti aracılığıyla bağıntı

Mikro geliştirme ortak sorunların özelliği için izleme işlemi işlenirken katılan tüm hizmetleri aracılığıyla bir istemciden biridir. Hata ayıklama için Performans Analizi, A yararlıdır / B testi ve diğer genel tanılama senaryoları.
Bu sorun bir bölümünü mantıksal parçalı bir işin izlemektir. İşlem sonucu ve gecikme süresi ve dış bağımlılık çağrıları ileti içerir. Bu tanılama olayları işlem sınırları ötesinde bağıntı başka bir parçasıdır.

Bir üretici bir kuyruk aracılığıyla ileti gönderdiğinde, genellikle bazı diğer istemci veya hizmet tarafından başlatılan başka mantıksal bir işlem, kapsam içinde gerçekleşir. Bir iletiyi aldıktan sonra aynı işlemi tüketici tarafından devam ettirildiğinde. Üretici ve Tüketici (hem işlemi diğer hizmetler), büyük olasılıkla telemetri olayları izleme işlemi akış ve sonuç yayma. Bu tür olayları ve izleme işlemi uçtan uca ilişkilendirmek için her olay sahip bir izleme bağlamı damga telemetri raporları her hizmetin vardır.

Microsoft Azure Service Bus ileti üreticileri ve tüketicileri gibi izleme bağlamı geçirmek için kullanması gereken yükü özellikleri tanımlanır.
Protokol dayanır [HTTP bağıntı Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md).

| Özellik Adı        | Açıklama                                                 |
|----------------------|-------------------------------------------------------------|
|  Tanılama kimliği       | Dış bir üretici çağrısından kuyruğa benzersiz tanımlayıcısı. Başvurmak [Request-Id HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#request-id) stratejinin, ilgili önemli noktalar ve biçimi |
|  Bağıntı bağlamı | İşlem işleme katılan tüm hizmetler arasında yayılır işlem bağlamı. Daha fazla bilgi için bkz: [bağıntı-bağlamında, HTTP Protokolü](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md#correlation-context) |

## <a name="service-bus-net-client-auto-tracing"></a>Service Bus .NET istemci otomatik-izleme

3.0.0 sürümünden başlayarak [.NET için Microsoft Azure ServiceBus istemcisi](/dotnet/api/microsoft.azure.servicebus.queueclient) izleme sistemleri veya istemci kodu parçası bağlanabilir izleme araçları noktaları sağlar.
Hizmet veri yolu için tüm çağrıları izleme araçları istemci tarafı hizmetinden Mesajlaşma izin verir. İleti işleme ile yapıldığında [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler), ileti işleme de işaretlenir

### <a name="tracking-with-azure-application-insights"></a>Azure Application Insights ile izleme

[Microsoft Application Insights](https://azure.microsoft.com/services/application-insights/) zengin performans izleme kapasiteleri automagical istek ve bağımlılık izleme de dahil olmak üzere sağlar.

Proje türüne bağlı olarak, Application Insights SDK'sı yükleyin:
- [ASP.NET](../application-insights/app-insights-asp-net.md) sürüm 2.5-beta2 veya üzeri
- [ASP.NET Core](../application-insights/app-insights-asp-net-core.md) sürüm 2.2.0-beta2 ya da daha yüksek.
Bu bağlantılar, SDK'sını yükleme, kaynakları oluşturma ve (gerekirse) SDK yapılandırma hakkında ayrıntılar sağlar. ASP.NET olmayan uygulamalar için bkz [konsol uygulamaları için Azure Application Insights](../application-insights/application-insights-console.md) makalesi.

Kullanırsanız [ileti işleyicisi deseni](/dotnet/api/microsoft.azure.servicebus.queueclient.registermessagehandler) iletileri işlemek için yapılır: hizmetiniz tarafından gerçekleştirilen tüm Service Bus çağrıları otomatik olarak izlenir ve diğer telemetri öğeleri ile bağlantılı. Aksi takdirde el ile ileti işleme izleme için aşağıdaki örneğe bakın.

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

Bu örnekte, `RequestTelemetry` zaman damgası, süre ve sonucu (başarılı) sahip her işlenen ileti için bildirilir. Telemetriyi de bağıntı özellikler kümesi vardır.
İç içe geçmiş izlemeleri ve özel durum iletisi işleme sırasında bildirilen ayrıca damgalı 'alt' olarak gösteren bağıntı özellikleriyle `RequestTelemetry`.

İleti işleme sırasında desteklenen dış bileşenlere çağrı yapmak durumunda, bunlar Ayrıca izlenen ve bağıntılı automagically değildir. Başvurmak [izlemek Application Insights .NET SDK ile özel işlemler](../application-insights/application-insights-custom-operations-tracking.md) el ile izleme ve bağıntı için.

### <a name="tracking-without-tracing-system"></a>İzleme sistemi olmadan izleme
Durumunda, izleme sistem izleme automagical Service Bus çağrıları desteklemiyor bu tür destek izleme sistemine veya uygulamanıza ekleme içine aramanız. Bu bölümde, Service Bus .NET istemci tarafından gönderilen Tanılama Olayları açıklanmaktadır.  

Service Bus .NET istemci izlenmiş .NET izleme temelleri kullanarak [System.Diagnostics.Activity](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) ve [System.Diagnostics.DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).

`Activity`İzleme bağlamı sırasında görevi gören `DiagnosticSource` bir bildirim mekanizmadır. 

DiagnosticSource olayları için hiç dinleyici ise, izleme, sıfır araçları maliyetleri tutma kapalıdır. DiagnosticSource dinleyici için tüm denetim olanağı verir:
- Dinleyici, kaynakları ve dinlemek için olayları denetler
- Dinleyici denetimleri olay hızı ve örnekleme
- olayları erişebilir ve ileti nesnesi olayı sırasında değiştirmek için tüm içerik sağlayan bir yükü ile gönderilir

İle öğrenmeniz [DiagnosticSource Kullanıcı Kılavuzu](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) uygulama işlemine devam etmeden önce.

Hizmet veri yolu olaylar için bir dinleyici Microsoft.Extension.Logger günlükleriyle Yazar ASP.NET Core uygulamasında oluşturalım.
Kullandığı [System.Reactive.Core](https://www.nuget.org/packages/System.Reactive.Core) (Bu da DiagnosticSource için olmadan abone olmak kolay) DiagnosticSource abone olmak için kitaplığı

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

Bu örnekte, dinleyicisi süresi, sonuç, benzersiz tanımlayıcı ve her Service Bus işlemi için başlangıç saatini günlüğe kaydeder.

#### <a name="events"></a>Olaylar

Her işlem için iki olayları gönderilir: 'Start' ve 'Stop'. Büyük olasılıkla yalnızca 'Stop' olayları ilgilendiğiniz. Bunlar işleminin sonucu sağlamak, aynı zamanda bir etkinlik özellikleri başlangıç saati ve süreyi.

Olay yükü işlemi bağlamında olan bir dinleyicisi sağlar, API gelen parametrelerini yinelenir ve dönüş değeri. 'Start' olay tamamen görmezden 'Stop' olay yükü 'Start' olay yükünün tüm özelliklerine sahiptir.

Tüm olayları 'Varlığı' ve 'Bitiş' özellikleri de bunlar içinde tabloda göz ardı edilir
  * `string Entity`--(Kuyruk, konu, vb.) varlık adı
  * `Uri Endpoint`-Service Bus uç nokta URL'si

'Stop' her olayın `Status` özelliğiyle `TaskStatus` zaman uyumsuz işlemi tamamlandı, ayrıca kolaylık olması için aşağıdaki tablodaki atlanır.

İzleme eklenmiş operations tam listesi aşağıdadır:

| İşlem Adı | İzlenen API | Belirli yükü özellikleri|
|----------------|-------------|---------|
| Microsoft.Azure.ServiceBus.Send | [MessageSender.SendAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.sendasync) | IList<Message> gönderilen iletilerin listesini iletileri - |
| Microsoft.Azure.ServiceBus.ScheduleMessage | [MessageSender.ScheduleMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.schedulemessageasync) | İleti - işlenen ileti<br/>DateTimeOffset ScheduleEnqueueTimeUtc - zamanlanmış ileti uzaklığı<br/>uzun SequenceNumber - sıra numarası zamanlanmış iletinin (olay yükü 'Stop') |
| Microsoft.Azure.ServiceBus.Cancel | [MessageSender.CancelScheduledMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagesender.cancelscheduledmessageasync) | uzun SequenceNumber - iptal edilmesi arı ileti sıra numarası | 
| Microsoft.Azure.ServiceBus.Receive | [MessageReceiver.ReceiveAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receiveasync) |int RequestedMessageCount - en fazla alınan ileti sayısı.<br/>IList<Message> iletileri - alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.Peek | [MessageReceiver.PeekAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.peekasync) | int FromSequenceNumber - toplu iletiler göz atmak başlangıç noktası.<br/>int RequestedMessageCount - almak için ileti sayısı.<br/>IList<Message> iletileri - alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.ReceiveDeferred | [MessageReceiver.ReceiveDeferredMessageAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.receivedeferredmessageasync) | IEnumerable<long> numaraları - almak için sıra numaraları içeren liste.<br/>IList<Message> iletileri - alınan iletiler (olay yükü 'Stop') listesi |
| Microsoft.Azure.ServiceBus.Complete | [MessageReceiver.CompleteAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.completeasync) | IList<string> kilit işareti var - lock belirteçleri tamamlamak için karşılık gelen iletileri içeren liste.|
| Microsoft.Azure.ServiceBus.Abandon | [MessageReceiver.AbandonAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.abandonasync) | dize LockToken - bırakmasını karşılık gelen iletinin kilidini belirteci. |
| Microsoft.Azure.ServiceBus.Defer | [MessageReceiver.DeferAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deferasync) | dize LockToken - erteleme karşılık gelen iletinin kilidini belirteci. | 
| Microsoft.Azure.ServiceBus.DeadLetter | [MessageReceiver.DeadLetterAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.deadletterasync) | dize LockToken - sahipsiz karşılık gelen iletiye kilit simgesi. | 
| Microsoft.Azure.ServiceBus.RenewLock | [MessageReceiver.RenewLockAsync](/dotnet/api/microsoft.azure.servicebus.core.messagereceiver.renewlockasync) | dize LockToken - üzerinde kilit yenilemek için karşılık gelen iletinin kilidini belirteci.<br/>DateTime LockedUntilUtc - yeni kilit belirteç süre sonu tarihi ve saati UTC biçiminde. (Olay yükü 'Stop')|
| Microsoft.Azure.ServiceBus.Process | Sağlanan ileti işleyicisini lambda işlevi [IReceiverClient.RegisterMessageHandler](/dotnet/api/microsoft.azure.servicebus.core.ireceiverclient.registermessagehandler) | -İşlenen ileti ileti. |
| Microsoft.Azure.ServiceBus.ProcessSession | Sağlanan ileti oturum işleyici lambda işlevi [IQueueClient.RegisterSessionHandler](/dotnet/api/microsoft.azure.servicebus.iqueueclient.registersessionhandler) | -İşlenen ileti ileti.<br/>IMessageSession oturumun - işlenen oturum |
| Microsoft.Azure.ServiceBus.AddRule | [SubscriptionClient.AddRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.addruleasync) | RuleDescription kural - eklemek için kural sağlar kural açıklaması. |
| Microsoft.Azure.ServiceBus.RemoveRule | [SubscriptionClient.RemoveRuleAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.removeruleasync) | dize RuleName - kuralın adını kaldırmak için. |
| Microsoft.Azure.ServiceBus.GetRules | [SubscriptionClient.GetRulesAsync](/dotnet/api/microsoft.azure.servicebus.subscriptionclient.getrulesasync) | IEnumerable<RuleDescription> abonelikle ilişkili kuralları tüm kuralları. (Yalnızca 'Stop' yükü) |
| Microsoft.Azure.ServiceBus.AcceptMessageSession | [ISessionClient.AcceptMessageSessionAsync](/dotnet/api/microsoft.azure.servicebus.isessionclient.acceptmessagesessionasync) | SessionID - iletilerinde mevcut SessionID dizesi. |
| Microsoft.Azure.ServiceBus.GetSessionState | [IMessageSession.GetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.getstateasync) | SessionID - iletilerinde mevcut SessionID dizesi.<br/>Byte [] durumunu - oturum durumu (olay yükü 'Stop') |
| Microsoft.Azure.ServiceBus.SetSessionState | [IMessageSession.SetStateAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.setstateasync) | SessionID - iletilerinde mevcut SessionID dizesi.<br/>Byte [] durumunu - oturum durumu |
| Microsoft.Azure.ServiceBus.RenewSessionLock | [IMessageSession.RenewSessionLockAsync](/dotnet/api/microsoft.azure.servicebus.imessagesession.renewsessionlockasync) | SessionID - iletilerinde mevcut SessionID dizesi. |
| Microsoft.Azure.ServiceBus.Exception | İzleme eklenmiş herhangi bir API'yi| Özel durum özel durum oluştu - özel durumu örneği |

Her durumda, erişebilirsiniz `Activity.Current` geçerli işlem bağlamı tutar.

#### <a name="logging-additional-properties"></a>Günlüğe kaydetme ek özellikleri

`Activty.Current`Geçerli işlem ayrıntılı bağlamını ve üst öğelerinden sağlar. Daha fazla bilgi için bkz: [etkinlik belgelerine](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) daha fazla ayrıntı için.
Service Bus araçları sağlayan ek bilgileri `Activity.Current.Tags` -tuttukları `MessageId` ve `SessionId` her oldukları kullanılabilir.

'Alma' izleyen etkinlikleri 'Gözlem' ve 'ReceiveDeferred' olay de sahip olabilir `RelatedTo` etiketi. Ayrı listesini tutar `Diagnostic-Id`(s) sonucunda alınan ileti.
Bu işlemi alınabilmesi için birkaç ilgisiz iletilerine neden. Ayrıca, `Diagnostic-Id` 'Alma' işlemleri için yalnızca bu etiketi kullanarak 'İşlemi' işlemleri bağıntılı şekilde işlemi başladığında, bilinmiyor. İletisi ne kadar sürdü denetlemek için performans sorunları analiz ederken yararlı olacaktır.

Etiketler oturum için etiketler ekleme önceki örneğe benzer şekilde bunları yinelemek için etkilidir 

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

Bazı durumlarda olayları performans yükü ya da depolama kullanımını azaltmak için yalnızca bir kısmını oturum önerilir. 'Stop' olayları yalnızca (önceki örnekte olduğu gibi) veya örnek olaylarının yüzdesini oturum açabilir. 
`DiagnosticSource`ile elde etmek için yol sağlama `IsEnabled` koşulu. Daha fazla bilgi için bkz: [Bağlam tabanlı filtreleme DiagnosticSource içinde](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#context-based-filtering).

`IsEnabled`birden çok kez performans etkisini en aza indirmek tek bir işlem için çağrılabilir.

`IsEnabled`aşağıdaki sırayla çağrılır:

1. `IsEnabled(<OperationName>, string entity, null)`Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send", "MyQueue1")`. Sonunda 'Start' veya 'Stop' yoktur unutmayın. Belirli işlemleri veya sıraları filtrelemek için kullanın. Geri çağırma döndürürse `false`, işlem için olayları gönderilmez

  * 'Process' ve 'ProcessSession' işlemleri için de aldığınız `IsEnabled(<OperationName>, string entity, Activity activity)` geri çağırma. Olaylara göre filtre uygulamak için kullanmak `activity.Id` veya etiketleri özellikleri.
  
2. `IsEnabled(<OperationName>.Start)`Örneğin, `IsEnabled("Microsoft.Azure.ServiceBus.Send.Start")`. 'Start' olay harekete olup olmadığını denetler. Sonuç yalnızca 'Start' olay etkiler, ancak daha fazla izleme ona bağlı değildir.

Var olan hiçbir `IsEnabled` 'Stop' olayı için.

Özel durum, bazı işlem sonucu ise `IsEnabled("Microsoft.Azure.ServiceBus.Exception")` olarak adlandırılır. Yalnızca 'Özel' olaylarına abone olma ve Araçları'nı geri kalanının önlemek. Bu durumda, bu tür özel durumları işleme çözümlenmedi. Diğer araçları devre dışı olduğundan üretici tüketici gelen iletileri ile akışı izleme bağlamı beklememeniz gerekir.

Kullanabileceğiniz `IsEnabled` de örnekleme stratejilerini uygulamak. Örnekleme temel alarak `Activity.Id` veya `Activity.RootId` (bunu kendi kodunuzu veya izleme sistem tarafından yayılır sürece) tüm özelliklerini arasında tutarlı örnekleme sağlar.

Birden çok varlığı içinde `DiagnosticSource` dinleyicileri için aynı kaynak olduğu olay şekilde kabul etmek yalnızca tek bir dinleyici için yeterli `IsEnabled` çağrılması için kesin değildir

## <a name="next-steps"></a>Sonraki adımlar

* [Service Bus ile ilgili temel bilgiler](service-bus-fundamentals-hybrid-solutions.md)
* [Uygulama Öngörüler bağıntı](../application-insights/application-insights-correlation.md)
* [Uygulama Öngörüler İzleyici bağımlılıkları](../application-insights/app-insights-asp-net-dependencies.md) REST, SQL veya diğer dış kaynaklara, yavaşlamadan olmadığını görmek için.
* [Application Insights .NET SDK ile özel işlemler izleme](../application-insights/application-insights-custom-operations-tracking.md)
