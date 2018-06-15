---
title: Azure veya tek başına küme .NET Service Fabric uygulamasından günlüğü olaylarını oluştur
description: Günlüğe kaydetme Azure bir küme veya tek başına küme üzerinde barındırılan .NET Service Fabric uygulamanızı ekleme hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: thraka
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/27/2018
ms.author: adegeo
ms.openlocfilehash: ed9aaf67b4f6749ea6d505a51fbc76e3d1cf0870
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34204881"
---
# <a name="add-logging-to-your-service-fabric-application"></a>Service Fabric uygulamanızı günlüğü ekleme

Uygulamanızı ortaya sorun çıktığında forensically hata ayıklamak için yeterli bilgi sağlamanız gerekir. Günlüğe kaydetme Service Fabric uygulamanızı ekleyebileceğiniz en önemli nokta biridir. Bir hata oluştuğunda, iyi günlük hataları araştırmak için bir yol verebilirsiniz. Günlük desenleri çözümleyerek, performans veya uygulamanızın tasarımını geliştirmek için yol bulabilirsiniz. Bu belge birkaç farklı günlüğe kaydetme seçeneklerini gösterir.

## <a name="eventflow"></a>EventFlow

[EventFlow Kitaplığı](https://github.com/Azure/diagnostics-eventflow) suite hangi tanılama verilerini toplamak için tanımlamak üzere uygulamalar sağlar ve burada bunlar yüzdelik için. Tanılama verilerini uygulama izlemelerini için performans sayaçlardan herhangi bir şey olabilir. İletişim ek yükü en aza şekilde uygulamayla aynı işlemde çalışır. EventFlow ve Service Fabric hakkında daha fazla bilgi için bkz: [Azure Service Fabric olay toplama EventFlow ile](service-fabric-diagnostics-event-aggregation-eventflow.md).

### <a name="using-structured-eventsource-events"></a>Yapılandırılmış EventSource olaylarını kullanma

Olayların kullanım örneği tanımlama ileti olay bağlamında olay ilişkin paket verileri imkan tanır. Filtre adları veya belirtilen olay özelliklerinin değerlerini temel ve daha kolay arayabilir. İzleme çıkışı yapar yapılandırılması okumak daha kolay ancak gerektiriyor her kullanım durumu için bir olay tanımlamak için daha fazla düşünce ve saati. 

Bazı olay tanımları tüm uygulama arasında paylaşılabilir. Örneğin, bir yöntemi Başlat veya Durdur için olay uygulama içinde birçok hizmetlerde yeniden. Bir sipariş sistemi gibi bir etki alanına özgü hizmet olabilir bir **CreateOrder** kendi benzersiz olayın olay. Bu yaklaşım birçok olayları oluşturmak ve potansiyel olarak tanımlayıcıları proje ekipleri arasında gerektiren. 

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

Belirli olayları tanımlama zor olabilir çünkü çoğu kişi genellikle dize olarak kendi bilgilerini çıktı parametreleri ortak bir dizi birkaç olaylarla tanımlayın. Yapılandırılmış en boy çoğunu kaybolur ve aramak ve sonuçları filtrelemek daha zordur. Bu yaklaşımda, genellikle günlük düzeylerini karşılık birkaç olaylar tanımlanır. Aşağıdaki kod parçacığında, bir hata ayıklama ve hata iletisi tanımlar:

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

Karma yapılandırılmış ve genel araçları kullanarak da iyi çalışabilir. Yapılandırılmış araçları hataları ve ölçümleri raporlama için kullanılır. Genel olaylar, sorun giderme için mühendisleri tarafından kullanılan ayrıntılı günlük kaydı için kullanılabilir.

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

ASP.NET oturum çekirdek ([Microsoft.Extensions.Logging NuGet paketi](https://www.nuget.org/packages/Microsoft.Extensions.Logging)) uygulamanız için bir standart günlüğü API sağlayan bir günlük çerçevedir. Diğer günlük kaydı arka uçlarını desteği ASP.NET oturum Core takılabilir. Bu, çok kodunu değiştirmek zorunda kalmadan çok çeşitli uygulamanızda oturum desteği işlenir sağlar.

1. Ekleme **Microsoft.Extensions.Logging** NuGet paketini projeye, gereç istiyorsunuz. Ayrıca, herhangi bir sağlayıcı paket ekleyin. Daha fazla bilgi için bkz: [ASP.NET çekirdek günlüğü](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Ekleme bir **kullanarak** için yönerge **Microsoft.Extensions.Logging** hizmet dosyanıza.
3. Hizmet sınıfınızı içinde özel bir değişken tanımlayın.

   ```csharp
   private ILogger _logger = null;
   ```

4. Hizmet sınıfınızı oluşturucuda bu kodu ekleyin:

   ```csharp
   _logger = new LoggerFactory().CreateLogger<Stateless>();
   ```

5. Yöntemlerinizi kodunuzda izleme başlatın. Bazı örnekler şunlardır:

   ```csharp
   _logger.LogDebug("Debug-level event from Microsoft.Logging");
   _logger.LogInformation("Informational-level event from Microsoft.Logging");

   // In this variant, we're adding structured properties RequestName and Duration, which have values MyRequest and the duration of the request.
   // Later in the article, we discuss why this step is useful.
   _logger.LogInformation("{RequestName} {Duration}", "MyRequest", requestDuration);
   ```

### <a name="using-other-logging-providers"></a>Diğer günlük sağlayıcıları kullanma

Bazı üçüncü taraf sağlayıcılar kullanımı yaklaşımı önceki bölümde açıklanan dahil olmak üzere [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), ve [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Her birinde ASP.NET oturum çekirdek içine takın veya ayrı olarak kullanabilirsiniz. Serilog Günlükçü gönderilen tüm iletiler aşağıdakilere zenginleştirir bir özelliği vardır. Bu özellik, hizmet adını, türünü ve bölüm bilgileri çıkarmak yararlı olabilir. ASP.NET Core altyapısında bu özelliği kullanmak için bu adımları uygulayın:

1. Ekleme **Serilog**, **Serilog.Extensions.Logging**, **Serilog.Sinks.Literate**, ve **Serilog.Sinks.Observable** NuGet paketleri projeye. 
2. Oluşturma bir `LoggerConfiguration` ve Günlükçü örneği.

   ```csharp
   Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
   ```

3. Ekleme bir `Serilog.ILogger` hizmet oluşturucu ve geçişi yeni oluşturulan Günlükçü bağımsız değişkeni.

   ```csharp
   ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
   ```

4. Özellik enrichers için hizmet oluşturucuda oluşturur **ServiceTypeName**, **ServiceName**, **PartitionID**, ve **InstanceId** .

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

5. ASP.NET Core Serilog olmadan kullanıyormuş gibi kodu aynı izleme.

   >[!NOTE]
   >Öneririz, *yok* statik kullanmak `Log.Logger` önceki örneğe sahip. Service Fabric aynı hizmet türünü tek bir işlem içinde birden çok örneğini barındırabilir. Statik kullanırsanız `Log.Logger`, özellik enrichers son yazan çalışan tüm örnekleri için değerleri gösterir. Bu neden _logger değişkeni bir hizmet sınıfında özel üye değişkeni nedenlerinden biridir. Ayrıca, olmalısınız `_logger` hizmetleri kullanılan ortak kod için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında daha fazla bilgi okuyun [uygulama Service Fabric izleme](service-fabric-diagnostics-event-generation-app.md).
- Günlük kaydıyla okuyun [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md).










