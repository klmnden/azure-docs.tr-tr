---
title: Azure Service Fabric, bulut hizmeti ve sanal makineler .NET uygulamaları için Snapshot Debugger'ı etkinleştirme | Microsoft Docs
description: Azure Service Fabric, bulut hizmeti ve sanal makineler .NET uygulamaları için Snapshot Debugger'ı etkinleştir
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: bfung
ms.openlocfilehash: 5ac1d1339cb8a26cc86157d4d2aa664517418095
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617805"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Azure Service Fabric, bulut hizmeti ve sanal makineler .NET uygulamaları için Snapshot Debugger'ı etkinleştir

Azure App Service'te uygulama çalıştığında, ASP.NET veya ASP.NET core, yüksek oranda önerilir [Snapshot Debugger Application ınsights'ı portal sayfası aracılığıyla etkinleştirme](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json). Ancak, uygulamanız, özelleştirilmiş bir anlık görüntü hata ayıklayıcı yapılandırma veya bir önizleme sürümü, .NET core'da gerektiriyorsa, ardından bu yönerge izlenmesi gereken ***ayrıca*** yönergelerine [aracılığıyla etkinleştirme Application ınsights'ı portal sayfası](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json).

Uygulamanızı Azure Service Fabric, bulut hizmeti, sanal makineleri çalıştırır, ya da şirket içi makineleri aşağıdaki yönergeler kullanılmalıdır. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>ASP.NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. [Web uygulamanızda Application Insights'ı etkinleştirmek](../../azure-monitor/app/asp-net.md), henüz yapmadınız.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Gerekirse, eklenen Snapshot Debugger yapılandırma özelleştirilmiş [Applicationınsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). Varsayılan anlık görüntü hata ayıklayıcı yapılandırma çoğu boştur ve tüm ayarlar isteğe bağlıdır. Bir yapılandırma için varsayılan yapılandırma eşdeğer gösteren bir örnek aşağıda verilmiştir:

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bazı durumlarda (örneğin, eski sürümleri .NET platformu) için gereksinim duyabileceğiniz [özel durum koleksiyonunu yapılandırma](../../azure-monitor/app/asp-net-exceptions.md#exceptions) portalında anlık görüntülerle özel durumları görmek için.


## <a name="configure-snapshot-collection-for-applications-using-aspnet-core-20-or-above"></a>Anlık görüntü koleksiyonunu veya üzeri için ASP.NET Core 2.0 kullanarak uygulamaları yapılandırma

1. [ASP.NET Core web uygulamanızı Application Insights'ı etkinleştirme](../../azure-monitor/app/asp-net-core.md), henüz yapmadınız.

    > [!NOTE]
    > Uygulamanızı sürüm 2.1.1 başvuran emin veya Microsoft.ApplicationInsights.AspNetCore paketin daha yeni olması.

2. Dahil [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Uygulamanızın değiştirme `Startup` eklemek ve anlık görüntü toplayıcının telemetri işlemci yapılandırmak için sınıf.
    1. Varsa [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet Paket sürümü 1.3.5 veya yukarıdaki kullanılır ve ardından aşağıdaki using deyimlerini `Startup.cs`.

       ```csharp
            using Microsoft.ApplicationInsights.SnapshotCollector;
       ```

       Createservicereplicalisteners() yöntemin sonuna aşağıdakini ekleyin `Startup` sınıfını `Startup.cs`.

       ```csharp
            services.AddSnapshotCollector((configuration) =>
            {
                IConfigurationSection section = Configuration.GetSection(nameof(SnapshotCollectorConfiguration));
                if (section.Value != null)
                {
                    section.Bind(configuration);
                }
            });

       ```
    2. Varsa [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet Paket sürümü 1.3.4 veya aşağıda kullanılır ve ardından aşağıdaki using deyimlerini `Startup.cs`.

       ```csharp
       using Microsoft.ApplicationInsights.SnapshotCollector;
       using Microsoft.Extensions.Options;
       using Microsoft.ApplicationInsights.AspNetCore;
       using Microsoft.ApplicationInsights.Extensibility;
       ```
    
       Aşağıdaki `SnapshotCollectorTelemetryProcessorFactory` sınıfının `Startup` sınıfı.
    
       ```csharp
       class Startup
       {
           private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
           {
               private readonly IServiceProvider _serviceProvider;
    
               public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
                   _serviceProvider = serviceProvider;
    
               public ITelemetryProcessor Create(ITelemetryProcessor next)
               {
                   var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
                   return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
               }
           }
           ...
        ```
        Ekleme `SnapshotCollectorConfiguration` ve `SnapshotCollectorTelemetryProcessorFactory` başlangıç ardışık düzenine Hizmetleri:
    
        ```csharp
           // This method gets called by the runtime. Use this method to add services to the container.
           public void ConfigureServices(IServiceCollection services)
           {
               // Configure SnapshotCollector from application settings
               services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));
    
               // Add SnapshotCollector telemetry processor.
               services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));
    
               // TODO: Add other services your application needs here.
           }
       }
       ```

4. Gerekirse, anlık görüntü hata ayıklayıcı yapılandırma için appsettings.json SnapshotCollectorConfiguration bölüm ekleyerek özelleştirilmiş. Tüm anlık görüntü hata ayıklayıcı yapılandırma ayarlarında isteğe bağlıdır. Bir yapılandırma için varsayılan yapılandırma eşdeğer gösteren bir örnek aşağıda verilmiştir:

   ```json
   {
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

## <a name="configure-snapshot-collection-for-other-net-applications"></a>Diğer .NET uygulamaları için anlık görüntü koleksiyonunu yapılandırma

1. Uygulamanızı Application Insights ile izlenen olması değil, başlayın [Application Insights'ı etkinleştirme ve izleme anahtarı ayarını](../../azure-monitor/app/windows-desktop.md).

2. Ekleme [Microsoft.applicationınsights.snapshotcollectya da](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) uygulamanıza NuGet paketi.

3. Application ınsights'ı raporlanan özel durumların anlık görüntülerini toplanır. Bunları rapor için kodunuzu değiştirmeniz gerekebilir. Özel durum işleme kodunu uygulamanızın yapısına bağlıdır, ancak bir örnek aşağıda verilmiştir:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="next-steps"></a>Sonraki adımlar

- Bir özel durum tetikleyebileceğiniz uygulama trafiği oluşturur. Ardından, Application Insights örneğine gönderilmek için anlık görüntüleri 10-15 dakika bekleyin.
- Bkz: [anlık görüntüleri](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json#view-snapshots-in-the-portal) Azure portalında.
- Snapshot Debugger sorunlarını giderme konusunda yardım için bkz: [Snapshot Debugger sorun giderme](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).
