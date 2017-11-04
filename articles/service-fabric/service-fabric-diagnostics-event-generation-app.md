---
title: "Azure Service Fabric uygulama düzeyinde izleme | Microsoft Docs"
description: "Uygulama ve hizmet düzeyi olayları ve günlükleri izlemek ve Azure Service Fabric kümeleri tanılamak için kullanılan hakkında bilgi edinin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 9b07c7cd256cea31c5654f0c3325e9fb675b39f8
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
---
# <a name="application-and-service-level-event-and-log-generation"></a>Uygulama ve hizmet düzeyi olay ve günlük oluşturma

## <a name="instrumenting-the-code-with-custom-events"></a>Özel olaylar koduyla düzenleme

Kod işaretleme çoğu diğer yönlerini hizmetlerinizi izleme temelidir. İzleme bildiğiniz bir şeylerin yanlış olduğunu tek yoludur ve neyi düzeltilmesi gerektiğini tanılamak için. Teknik olarak, bir hata ayıklayıcısı bir üretim hizmetine bağlanmak mümkün olsa da, ortak bir uygulama değildir. Bu nedenle, izleme verileri ayrıntılı önemlidir.

Bazı ürünler kodunuzu otomatik olarak işaretleme. Bu çözüm de olsa da, el ile araçları neredeyse her zaman gereklidir. Sonunda forensically uygulamada hata ayıklama için yeterli bilgiye sahip olmalıdır. Bu belgede, kodunuz ve ne zaman bir yaklaşım başka bir ad seçin düzenleme için farklı yaklaşımlara açıklanmaktadır.

## <a name="eventsource"></a>EventSource
Visual Studio'da bir şablondan bir Service Fabric çözüm oluşturduğunuzda bir **EventSource**-türetilen sınıfı (**ServiceEventSource** veya **ActorEventSource**) oluşturulur. Bir şablon oluşturduğunuzu, uygulama veya hizmet için olayları ekleyebilirsiniz. **EventSource** adı **gerekir** benzersiz olmalı ve varsayılan şablon dizeden Şirketim - adlandırılmamalıdır&lt;çözüm&gt;-&lt;proje&gt;. Birden çok sahip **EventSource** aynı adı kullanan tanımları çalışma zamanında bir sorunu neden olur. Her tanımlanan olay benzersiz bir tanımlayıcı olması gerekir. Tanımlayıcı benzersiz değilse, bir çalışma zamanı hatası oluşur. Bazı kuruluşlar ayrı geliştirme ekipleri arasındaki çakışmaları önleme tanımlayıcıları için değerlerin aralıkları preassign. Daha fazla bilgi için bkz: [Vance'nın blogu](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/) veya [MSDN belgelerine](https://msdn.microsoft.com/library/dn774985(v=pandp.20).aspx).

### <a name="using-structured-eventsource-events"></a>Yapılandırılmış EventSource olaylarını kullanma

Bu bölümdeki kod örnekleri, olayların her biri tanımlanmış bir belirli durumlarda, örneğin, bir hizmet türünün kaydedildiğinde. Kullanım örneği tarafından iletileri tanımlamak, veri hata metni ile paketlenmesi ve daha yapabilecekleriniz kolayca arama ve filtre adları veya belirtilen özelliklerin değerlerine göre. İzleme çıkışı yapar yapılandırılması kullanmak, daha kolay ancak gerektiriyor her kullanım durumu için yeni bir olayı tanımlamak için daha fazla düşünce ve saati. Bazı olay tanımları tüm uygulama arasında paylaşılabilir. Örneğin, bir yöntemi Başlat veya Durdur için olay uygulama içinde birçok hizmetlerde yeniden. Bir sipariş sistemi gibi bir etki alanına özgü hizmet olabilir bir **CreateOrder** kendi benzersiz olayın olay. Bu yaklaşım birçok olayları oluşturmak ve potansiyel olarak tanımlayıcıları proje ekipleri arasında gerektiren. 

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
```

Karma yapılandırılmış ve genel araçları kullanarak da iyi çalışabilir. Yapılandırılmış araçları hataları ve ölçümleri raporlama için kullanılır. Genel olaylar, sorun giderme için mühendisleri tarafından kullanılan ayrıntılı günlük kaydı için kullanılabilir.

## <a name="aspnet-core-logging"></a>ASP.NET oturum çekirdek

Kodunuzu nasıl izleme dikkatle planlamanız önemlidir. Sağ araçları planı büyük olasılıkla kod temeliniz destabilizing ve kod reinstrument gerek önlemenize yardımcı olabilir. Riskini azaltmak için bir araç kitaplığı gibi seçebilirsiniz [Microsoft.Extensions.Logging](https://www.nuget.org/packages/Microsoft.Extensions.Logging/), Microsoft ASP.NET Core parçası olduğu. ASP.NET Core sahip bir [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) var olan kodu üzerindeki etkisini en aza indirerek tercih ettiğiniz sağlayıcısı ile birlikte kullanabileceğiniz arabirimi. Windows ve Linux üzerinde ASP.NET Core kodu kullanabilirsiniz ve tam .NET Framework, bu nedenle araçları kodunuzu standartlaştırılmıştır. Bu daha aşağıda incelediniz:

### <a name="using-microsoftextensionslogging-in-service-fabric"></a>Service Fabric Microsoft.Extensions.Logging kullanma

1. İstediğiniz projesine Microsoft.Extensions.Logging NuGet paketi ekleme aracı. Ayrıca, herhangi bir sağlayıcı paket ekleme (üçüncü taraf paketi için aşağıdaki örneğe bakın). Daha fazla bilgi için bkz: [ASP.NET çekirdek günlüğü](https://docs.microsoft.com/aspnet/core/fundamentals/logging).
2. Ekleme bir **kullanarak** hizmet dosyanıza Microsoft.Extensions.Logging için yönerge.
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

## <a name="using-other-logging-providers"></a>Diğer günlük sağlayıcıları kullanma

Bazı üçüncü taraf sağlayıcılar kullanımı yaklaşımı önceki bölümde açıklanan dahil olmak üzere [Serilog](https://serilog.net/), [NLog](http://nlog-project.org/), ve [Loggr](https://github.com/imobile3/Loggr.Extensions.Logging). Her birinde ASP.NET oturum çekirdek içine takın veya ayrı olarak kullanabilirsiniz. Serilog Günlükçü gönderilen tüm iletiler aşağıdakilere zenginleştirir bir özelliği vardır. Bu özellik, hizmet adını, türünü ve bölüm bilgileri çıkarmak yararlı olabilir. ASP.NET Core altyapısında bu özelliği kullanmak için bu adımları uygulayın:

1. Serilog, Serilog.Extensions.Logging ve Serilog.Sinks.Observable NuGet paketlerini projenize ekleyin. Sonraki örneğin Serilog.Sinks.Literate de ekleyin. Daha iyi bir yaklaşım, bu makalenin sonraki bölümlerinde gösterilir.
2. Serilog içinde bir LoggerConfiguration ve Günlükçü örneği oluşturun.

  ```csharp
  Log.Logger = new LoggerConfiguration().WriteTo.LiterateConsole().CreateLogger();
  ```

3. Hizmet oluşturucuya Serilog.ILogger bağımsız değişkeni eklemek ve yeni oluşturulan Günlükçü geçirin.

  ```csharp
  ServiceRuntime.RegisterServiceAsync("StatelessType", context => new Stateless(context, Log.Logger)).GetAwaiter().GetResult();
  ```

4. Hizmet oluşturucuda için özellik enrichers oluşturur aşağıdaki kodu ekleyin **ServiceTypeName**, **ServiceName**, **PartitionID**, ve **InstanceId** hizmetinin özelliklerini. Kodunuzda Microsoft.Extensions.Logging.ILogger kullanabilmeniz için ASP.NET Core günlük Fabrika için de bir özellik enricher ekler.

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
  >Önceki örnekle statik Log.Logger kullanmamanızı öneririz. Service Fabric aynı hizmet türünü tek bir işlem içinde birden çok örneğini barındırabilir. Statik Log.Logger kullanırsanız, özellik enrichers son yazan çalışan tüm örnekleri için değerleri gösterir. Bu neden _logger değişkeni bir hizmet sınıfında özel üye değişkeni nedenlerinden biridir. Ayrıca, _logger hizmetleri kullanılan ortak kodun kullanımına yapmanız gerekir.

## <a name="choosing-a-logging-provider"></a>Oturum açma sağlayıcısı seçme

Uygulamanızı yüksek performans üzerinde dayalıysa **EventSource** genellikle iyi bir yaklaşımdır. **EventSource** *genellikle* daha az kaynak kullanır ve ASP.NET Core günlüğü ya da herhangi bir kullanılabilir üçüncü taraf çözümleri daha iyi gerçekleştirir.  Bu hizmet performans kullanarak dayalı olup olmadığını ancak birçok Hizmetleri için bir sorun değildir **EventSource** daha iyi bir seçim olabilir. Ancak, bu yararları almak için günlüğe kaydetme, yapılandırılmış **EventSource** mühendislik ekibi daha büyük bir yatırım gerektirir. Mümkünse, birkaç günlüğe kaydetme seçeneklerini hızlı prototipi yapın ve ardından ihtiyaçlarınıza en uygun olanı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalar ve hizmetler işaretlemesini, oturum açma sağlayıcısı seçtikten sonra herhangi bir çözümleme platform gönderilmeden önce toplanacak günlüklerini ve olayları gerekir. Hakkında bilgi edinin [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) ve [WAD](service-fabric-diagnostics-event-aggregation-wad.md) önerilen seçeneklerden bazıları daha iyi anlamak için.
