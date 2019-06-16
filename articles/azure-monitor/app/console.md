---
title: Konsol uygulamaları için Azure Application Insights | Microsoft Docs
description: Web uygulamalarının kullanılabilirliğini, performansını ve kullanımını izleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 01/30/2019
ms.reviewer: lmolkova
ms.author: mbullwin
ms.openlocfilehash: 602cd9696271931babad9aa962638c5b646c80ac
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60901861"
---
# <a name="application-insights-for-net-console-applications"></a>.NET için Application Insights konsol uygulamaları
[Application Insights](../../azure-monitor/app/app-insights-overview.md) web uygulamanızın kullanılabilirliğini, performansını ve kullanımını izlemenize olanak tanır.

Bir aboneliğe ihtiyacınız [Microsoft Azure](https://azure.com). Windows, Xbox Live veya diğer Microsoft bulut Hizmetleri için olabilir bir Microsoft hesabıyla oturum açın. Takımınızın kurumsal bir Azure aboneliğine sahip olabilir: sahibinden Microsoft hesabınızı kullanarak eklemeli isteyin.

## <a name="getting-started"></a>Başlarken

* [Azure portalında](https://portal.azure.com) [bir Application Insights kaynağı oluşturun](../../azure-monitor/app/create-new-resource.md ). Uygulama türü olarak seçin **genel**.
* İzleme Anahtarının bir kopyasını oluşturun. Anahtarı bulma **Essentials** oluşturduğunuz yeni kaynağın açılır. 
* Son yükleme [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights) paket.
* Hiç telemetri izlemeden önce izleme anahtarını kodunuzda ayarlayın (veya set appınsıghts_ınstrumentatıonkey ortam değişkeni). Bundan sonra el ile telemetri izlemek ve Azure portalında görme olanağına olmalıdır

```csharp
TelemetryConfiguration.Active.InstrumentationKey = " *your key* ";
var telemetryClient = new TelemetryClient();
telemetryClient.TrackTrace("Hello World!");
```

* En son sürümünü yükleyin [Microsoft.ApplicationInsights.DependencyCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector) package - HTTP, SQL veya başka bir dış bağımlılık çağrıları otomatik olarak izler.

Başlat ve koddan konfigurovat Application Insights veya bu adı kullanıyor `ApplicationInsights.config` dosya. Mümkün olduğunca erken başlatma gerçekleşir emin olun. 

> [!NOTE]
> Başvuru yönergeleri **Applicationınsights.config** .NET Framework hedefleme ve .NET Core uygulamaları için geçerli değildir uygulamalar yalnızca geçerlidir.

### <a name="using-config-file"></a>Yapılandırma dosyası kullanma

Varsayılan olarak, Application Insights SDK'sı arar `ApplicationInsights.config` çalışma dizininde dosya olduğunda `TelemetryConfiguration` oluşturuluyor

```csharp
TelemetryConfiguration config = TelemetryConfiguration.Active; // Reads ApplicationInsights.config file if present
```

Yapılandırma dosyasının yolunu da belirtebilirsiniz.

```csharp
using System.IO;
TelemetryConfiguration configuration = TelemetryConfiguration.CreateFromConfiguration(File.ReadAllText("C:\\ApplicationInsights.config"));
var telemetryClient = new TelemetryClient(configuration);
```

Daha fazla bilgi için [yapılandırma dosyası başvurusu](configuration-with-applicationinsights-config.md).

En son sürümünü yükleyerek, yapılandırma dosyasının tam bir örnek alabilirsiniz [Microsoft.applicationınsights.windowsserver](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer) paket. İşte **minimal** yapılandırması için örnek kod eşdeğerdir bağımlılık toplama.

```XML
<?xml version="1.0" encoding="utf-8"?>
<ApplicationInsights xmlns="http://schemas.microsoft.com/ApplicationInsights/2013/Settings">
  <InstrumentationKey>Your Key</InstrumentationKey>
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

### <a name="configuring-telemetry-collection-from-code"></a>Koddan telemetri toplamayı yapılandırma

* Uygulama başlatma sırasında oluşturma ve yapılandırma `DependencyTrackingTelemetryModule` örneği - tekil olmalıdır ve uygulama ömrü boyunca korunmalıdır.

```csharp
var module = new DependencyTrackingTelemetryModule();

// prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
//...

// enable known dependency tracking, note that in future versions, we will extend this list. 
// please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");
//....

// initialize the module
module.Initialize(configuration);
```

* Sık kullanılan telemetri başlatıcılar Ekle

```csharp
// stamps telemetry with correlation identifiers
TelemetryConfiguration.Active.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());

// ensures proper DependencyTelemetry.Type is set for Azure RESTful API calls
TelemetryConfiguration.Active.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());
```

* .NET Framework Windows uygulaması için de yükleyebilir ve performans sayacı Toplayıcı modülü açıklandığı Başlat [burada](https://apmtips.com/blog/2017/02/13/enable-application-insights-live-metrics-from-code/)

#### <a name="full-example"></a>Tam örnek

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility;
using System.Net.Http;
using System.Threading.Tasks;

namespace ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            TelemetryConfiguration configuration = TelemetryConfiguration.Active;

            configuration.InstrumentationKey = "removed";
            configuration.TelemetryInitializers.Add(new OperationCorrelationTelemetryInitializer());
            configuration.TelemetryInitializers.Add(new HttpDependenciesParsingTelemetryInitializer());

            var telemetryClient = new TelemetryClient();
            using (InitializeDependencyTracking(configuration))
            {
                // run app...

                telemetryClient.TrackTrace("Hello World!");

                using (var httpClient = new HttpClient())
                {
                    // Http dependency is automatically tracked!
                    httpClient.GetAsync("https://microsoft.com").Wait();
                }

            }

            // before exit, flush the remaining data
            telemetryClient.Flush();

            // flush is not blocking so wait a bit
            Task.Delay(5000).Wait();

        }

        static DependencyTrackingTelemetryModule InitializeDependencyTracking(TelemetryConfiguration configuration)
        {
            var module = new DependencyTrackingTelemetryModule();

            // prevent Correlation Id to be sent to certain endpoints. You may add other domains as needed.
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.windows.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.chinacloudapi.cn");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.cloudapi.de");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("core.usgovcloudapi.net");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("localhost");
            module.ExcludeComponentCorrelationHttpHeadersOnDomains.Add("127.0.0.1");

            // enable known dependency tracking, note that in future versions, we will extend this list. 
            // please check default settings in https://github.com/Microsoft/ApplicationInsights-dotnet-server/blob/develop/Src/DependencyCollector/DependencyCollector/ApplicationInsights.config.install.xdt

            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.ServiceBus");
            module.IncludeDiagnosticSourceActivities.Add("Microsoft.Azure.EventHubs");

            // initialize the module
            module.Initialize(configuration);

            return module;
        }
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar
* [İzleme bağımlılıkları](../../azure-monitor/app/asp-net-dependencies.md) REST, SQL ve diğer dış kaynaklara, yavaşlatmadan olmadığını görmek için.
* [API'yi kullanmak](../../azure-monitor/app/api-custom-events-metrics.md) kendi olayları ve ölçümleri daha ayrıntılı bir görünüm, uygulamanızın performans ve kullanım için gönderilecek.
