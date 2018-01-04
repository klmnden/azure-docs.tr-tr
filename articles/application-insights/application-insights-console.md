---
title: "Konsol uygulamaları için Azure Application Insights | Microsoft Docs"
description: "Kullanılabilirlik, performans ve kullanım için Web uygulamalarını izleyin."
services: application-insights
documentationcenter: .net
author: lmolkova
manager: bfung
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/18/2017
ms.author: lmolkova
ms.openlocfilehash: 74f3334fb5b13dccfd16eff216d370fb6a643980
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="application-insights-for-net-console-applications"></a>.NET için Application Insights konsol uygulamaları
[Application Insights](app-insights-overview.md) , web uygulamanızın kullanılabilirlik, performans ve kullanım izlemenize izin verir.

Bir aboneliğe ihtiyacınız [Microsoft Azure](http://azure.com). Windows, Xbox Live ve diğer Microsoft bulut Hizmetleri için olabilir bir Microsoft hesabıyla oturum açın. Takımınızın kurumsal bir Azure aboneliğine sahip olabilir: sahibinden Microsoft hesabınızı kullanarak eklemeli isteyin.

## <a name="getting-started"></a>Başlarken

* [Azure portalında](https://portal.azure.com) [bir Application Insights kaynağı oluşturun](app-insights-create-new-resource.md). Uygulama türü olarak ASP.NET uygulamasını seçin.
* İzleme Anahtarının bir kopyasını oluşturun. Anahtar Essentials açılan oluşturduğunuz yeni kaynak bulamaz. 
* Son yükleme [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) paket.
* Tüm telemetri izlemeden önce kodunuzda izleme anahtarını ayarlayın (veya set appınsıghts_ınstrumentatıonkey ortam değişkeni). Bundan sonra el ile telemetri izlemek ve Azure Portal'da görme gerekir

```C#
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* En son sürümünü yüklemek [Microsoft.ApplicationInsights.DependecyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) paket - HTTP, SQL veya başka bir dış bağımlılık çağrıları otomatik olarak izler.

Başlatma ve Application Insights koddan yapılandırma veya kullanarak `ApplicationInsights.config` dosya. Başlatma olabildiğince erken olur emin olun.

### <a name="using-config-file"></a>Yapılandırma dosyası kullanma

Varsayılan olarak, Application Insights SDK'sı arar `ApplicationInsights.config` çalışma dizini dosyasında zaman `TelemetryConfiguration` oluşturuluyor

```C#
TelemetryConfiguration config = TelemetryConfiguration.Active; // Read ApplicationInsights.config file if present
```

Yapılandırma dosyası yolu de belirtebilir.

```C#
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration("ApplicationInsights.config");
```

Daha fazla bilgi için bkz: [yapılandırma dosyası başvurusu](app-insights-configuration-with-applicationinsights-config.md).

En son sürümünü yükleme yapılandırma dosyasının tam bir örnek alabilirsiniz [Microsoft.ApplicationInsights.WindowsServer](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) paket. Burada **en az** kod örneğine eşdeğerdir bağımlılık koleksiyon için yapılandırma.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <TelemetryInitializers>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.HttpDependenciesParsingTelemetryInitializer, Microsoft.AI.DependencyCollector"/>
  </TelemetryInitializers>
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.DependencyCollector.DependencyTrackingTelemetryModule, Microsoft.AI.DependencyCollector">
      <ExcludeComponentCorrelationHttpHeadersOnDomains>
        <Add>core.windows.net</Add>
        <Add>core.chinacloudapi.cn</Add>
        <Add>core.cloudapi.de</Add>
        <Add>core.usgovcloudapi.net</Add>
        <Add>localhost</Add>
        <Add>127.0.0.1</Add>
      </ExcludeComponentCorrelationHttpHeadersOnDomains>
      <IncludeDiagnosticSourceActivities>
        <Add>Microsoft.Azure.ServiceBus</Add>
        <Add>Microsoft.Azure.EventHubs</Add>
      </IncludeDiagnosticSourceActivities>
    </Add>
  </TelemetryModules>
  <TelemetryChannel Type="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel.ServerTelemetryChannel, Microsoft.AI.ServerTelemetryChannel"/>
</ApplicationInsights>

```

### <a name="configuring-telemetry-collection-from-code"></a>Koddan telemetri koleksiyonunu yapılandırma

* Uygulama başlatma sırasında oluşturma ve yapılandırma `DependencyTrackingTelemetryModule` örneği - singleton olmalıdır ve uygulama etkin kalma süresi korunmalıdır.

```C#
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Sık kullanılan telemetri başlatıcıları ekleme

```C#
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* .NET Framework Windows uygulaması için aynı zamanda yükleyebilir ve performans sayacı Toplayıcı modülü açıklandığı gibi başlatma [burada](http://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Tam örnek

```C#
static void Main(string[] args)
{
    TelemetryConfiguration configuration = TelemetryConfiguration.Active;

    configuration.InstrumentationKey = "removed";
    configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
    configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

    var telemetryClient = new TelemetryClient();
    using (IntitializeDependencyTracking(configuration))
    {
        telemetryClient.TrackTrace("Hello World!");

        using (var httpClient = new HttpClient())
        {
            // Http dependency is automatically tracked!
            httpClient.GetAsync("https://microsoft.com").Wait();
        }
    }

    // run app...

    // when application stops or you are done with dependency tracking, do not forget to dispose the module
    dependencyTrackingModule.Dispose();

    telemetryClient.Flush();
}

static DependencyTrackingTelemetryModule IntitializeDependencyTracking(TelemetryConfiguration configuration)
{
    // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");    
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
    module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

    // enable known dependency tracking, note that in future versions, we will extend this list. 
    // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/NuGet/ApplicationInsights.config.install.xdt#L20
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
    module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

    // initialize the module
    module.Initialize(configuration);

    return module;
}
```

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme bağımlılıkları](app-insights-asp-net-dependencies.md) REST, SQL veya diğer dış kaynaklara, yavaşlamadan olmadığını görmek için.
* [API kullanan](app-insights-api-custom-events-metrics.md) kendi olayları ve ölçümleri, uygulamanızın performansı ve kullanımı daha ayrıntılı bir görünüm için gönderilecek.