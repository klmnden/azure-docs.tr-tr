---
title: Günlük olaylarının bir Service Fabric .NET uygulamasından Azure veya tek başına küme oluşturma
description: .NET Service Fabric uygulamanızı Azure kümesine veya tek başına küme barındırılan günlük ekleme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/27/2018
ms.author: srrengar
ms.openlocfilehash: d1b3dc25dd9bda9d7f9d9152c2a94cea8321f5cf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60482616"
---
# <a name="add-logging-to-your-service-fabric-application"></a>Günlüğe kaydetme, Service Fabric uygulamanıza ekleyin

Uygulama, sorunları ortaya çıktığında forensically hata ayıklamak için yeterli bilgi sağlamanız gerekir. Günlüğe kaydetme, Service Fabric uygulamanızı ekleyebileceğiniz en önemli şeylerden biridir. Hata oluştuğunda, iyi günlük kaydı hataları araştırmak için bir yol verebilirsiniz. Günlük düzenleri analiz ederek performansı veya uygulamanızın tasarımını iyileştirmenin de yollarını bulabilirsiniz. Bu belge, birkaç farklı günlüğe kaydetme seçeneklerini gösterir.

## <a name="eventflow"></a>EventFlow

[EventFlow Kitaplığı](https://github.com/Azure/diagnostics-eventflow) toplanacak Tanılama verileri tanımlamak uygulamalar sağlar ve burada, yüzdelik için. Tanılama verileri herhangi bir şeyin performans sayaçları uygulamanın izlemelere olabilir. İletişim ek yükü en aza inecek şekilde, uygulamayla aynı işlemde çalıştırır. EventFlow ve Service Fabric hakkında daha fazla bilgi için bkz. [EventFlow ile Azure Service Fabric olay toplama](service-fabric-diagnostics-event-aggregation-eventflow.md).

### <a name="using-structured-eventsource-events"></a>Yapılandırılmış EventSource olaylarını kullanma

Olayların kullanım örneği tanımlama ileti olay bağlamında bir olayla ilgili paket verilere imkan tanır. Daha kolay arama ve filtre adları veya belirtilen olay özelliklerinin değerlerini göre. Yapılandırma izleme çıkış yapar, daha kolay okunur ancak gerektiriyor düşünce ve saat her kullanım örneği için bir olayı tanımlamak için. 

Bazı olay tanımlarına uygulamanın tamamında paylaşılabilir. Örnek, bir yöntem başlatma veya durdurma için olay uygulama içinde birçok hizmet arasında yeniden. Bir sipariş sistemi gibi bir etki alanına özgü hizmeti olabilir bir **CreateOrder** kendi benzersiz olay olan olay. Bu yaklaşım çok sayıda olay oluşturun ve büyük olasılıkla proje takımları arasında koordinasyon tanımlayıcıların gerektiren. 

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The instance constructor is private to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    // The ServiceTypeRegistered event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceTypeRegisteredEventId = 3;
    [Event(ServiceTypeRegisteredEventId, Level = EventLevel.Informational, Message = "Service host process {0} registered service type {1}", Keywords = Keywords.ServiceInitialization)]
    public void ServiceTypeRegistered(int hostProcessId, string serviceType)
    {
        WriteEvent(ServiceTypeRegisteredEventId, hostProcessId, serviceType);
    }

    // The ServiceHostInitializationFailed event contains a unique identifier, an event attribute that defined the event, and the code implementation of the event.
    private const int ServiceHostInitializationFailedEventId = 4;
    [Event(ServiceHostInitializationFailedEventId, Level = EventLevel.Error, Message = "Service host initialization failed", Keywords = Keywords.ServiceInitialization)]
    public void ServiceHostInitializationFailed(string exception)
    {
        WriteEvent(ServiceHostInitializationFailedEventId, exception);
    }

    ...

```

### <a name="using-eventsource-generically"></a>EventSource genel kullanma

Belirli olayları tanımlama zor olabilir çünkü çok kişi genellikle dize olarak bilgilerini çıktı parametreleri ortak bir dizi ile birkaç olaylarını tanımlayın. Yapılandırılmış en boy çoğunu kaybolur ve arama ve sonuçları filtrelemek daha zordur. Bu yaklaşımda, genellikle günlük düzeylerini için karşılık gelen bazı olayların tanımlanır. Aşağıdaki kod parçacığında, bir hata ayıklama ve hata iletisini tanımlar:

```csharp
[EventSource(Name = "MyCompany-VotingState-VotingStateService")]
internal sealed class ServiceEventSource : EventSource
{
    public static readonly ServiceEventSource Current = new ServiceEventSource();

    // The Instance constructor is private, to enforce singleton semantics.
    private ServiceEventSource() : base() { }

    ...

    private const int DebugEventId = 10;
    [Event(DebugEventId, Level = EventLevel.Verbose, Message = "{0}")]
    public void Debug(string msg)
    {
        WriteEvent(DebugEventId, msg);
    }

    private const int ErrorEventId = 11;
    [Event(ErrorEventId, Level = EventLevel.Error, Message = "Error: {0} - {1}")]
    public void Error(string error, string msg)
    {
        WriteEvent(ErrorEventId, error, msg);
    }

    ...

```

Karma izleme yapılandırılmış ve genel kullanarak da iyi çalışabilir. Yapılandırılmış araçları hataları ve ölçümleri raporlama için kullanılır. Genel olaylar, sorun giderme için mühendisleri tarafından kullanılan ayrıntılı günlük kaydı için kullanılabilir.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

Günlüğe kaydetme ASP.NET Core ([Microsoft.Extensions.Logging NuGet paketini](https://www.nuget.org/packages/Microsoft.Extensions.Logging)) uygulamanız için bir standart günlüğe kaydetme API'si sağlayan günlüğe kaydetme çerçevesi. Diğer günlük kaydı arka uçlar için destek, günlüğe kaydetme, ASP.NET Core takılabilir. Bu, kadar kodu değiştirmek zorunda kalmadan çok çeşitli uygulamanızda oturum desteğini işleneceğini sağlar.

1. Ekleme **Microsoft.Extensions.Logging** NuGet paketini projeye, gereç istiyorsunuz. Ayrıca, herhangi bir sağlayıcı paket ekleyin. Daha fazla bilgi için [ASP.NET Core günlüğü](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Ekleme bir **kullanarak** yönergesi **Microsoft.Extensions.Logging** hizmet dosyanıza.
3. Hizmet Sınıfınız içinde özel bir değişken tanımlayın.

   ```csharp
   private ILogger _logger = null;
   ```

4. Hizmet sınıfın oluşturucusunda bu kodu ekleyin:

   ```csharp
   _logger = new LoggerFactory().CreateLogger<Stateless>();
   ```

5. Yöntemlerinizi kodunuzda işaretleyerek başlayın. Bazı örnekleri şunlardır:

   ```csharp
   _logger.LogDebug("Debug-level event from Microsoft.Logging");
   _logger.LogInformation("Informational-level event from Microsoft.Logging");

   // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
   // Later in the article, we discuss why this step is useful.
   _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
   ```

### <a name="using-other-logging-providers"></a>Diğer günlük sağlayıcıları kullanma

Bazı üçüncü taraf sağlayıcılar kullanımı yaklaşımı önceki bölümde açıklanan dahil olmak üzere [Serilog](https://serilog.net/), [NLog](https://nlog-project.org/), ve [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Her birinde oturum ASP.NET Core uygulamasına takın veya ayrı olarak kullanabilirsiniz. Serilog bir günlükçüden gönderilen tüm iletilerin zenginleştirir bir özelliği vardır. Bu özellik, hizmet adı, türü ve bölüm bilgileri çıkarmak yararlı olabilir. ASP.NET Core altyapısında bu özelliğin kullanılabilmesi için bu adımları uygulayın:

1. Ekleme **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, ve **Serilog.Sinks.Observable** NuGet paketleri projeye. 
2. Oluşturma bir `LoggerConfiguration` ve Günlükçü örneği.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Ekleme bir `Serilog.ILogger` hizmet oluşturucusu ve yeni oluşturulan Günlükçü geçişi için bağımsız değişken.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. Hizmet oluşturucuda için özellik enrichers oluşturur **servicetypename birlikte belirtilemez**, **ServiceName**, **PartitionID**, ve **InstanceId** .

   ```csharp
   public Stateless(StatelessServiceContext context, Serilog.ILogger serilog)
       : base(context)
   {
       PropertyEnricher[] properties = new PropertyEnricher[]
       {
           new PropertyEnricher("ServiceTypeName", context.ServiceTypeName),
           new PropertyEnricher("ServiceName", context.ServiceName),
           new PropertyEnricher("PartitionId", context.PartitionId),
           new PropertyEnricher("InstanceId", context.ReplicaOrInstanceId),
       };

       serilog.ForContext(properties);

       _logger = new LoggerFactory().AddSerilog(serilog.ForContext(properties)).CreateLogger<Stateless>();
   }
   ```

5. ASP.NET Core Serilog olmadan kullanıyormuş gibi aynı kodu izleyin.

   >[!NOTE]
   >Olmasını öneririz, *yoksa* statik `Log.Logger` önceki örnekle. Service Fabric, tek bir işlem içinde aynı hizmet türünün birden çok örneği barındırabilir. Statik kullanırsanız `Log.Logger`, son yazıcı özelliği enrichers, çalışmakta olan tüm örnekleri için değerleri gösterecektir. Bu neden _logger değişkeni hizmet sınıfı özel üye değişkeninin nedenlerinden biridir. Ayrıca, yapmanız gereken `_logger` hizmette kullanılan ortak kod için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi okuyun [uygulama Service Fabric'te izleme](service-fabric-diagnostics-event-generation-app.md).
- Günlük kaydı ile ilgili bilgi [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [Windows Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md).










