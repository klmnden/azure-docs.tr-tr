---
title: Visual Studio olmadan ASP.NET Core için Azure Application Insights | Microsoft Docs
description: Kullanılabilirlik, performans ve kullanım için ASP.NET Core web uygulamalarını izleyin.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: cithomas
ms.openlocfilehash: 8243523887ec9861459b2d196126237cf89bad97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60691351"
---
# <a name="application-insights-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için Application Insights

Bu makalede için Application Insights'ı etkinleştirme adımları bir [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2) Visual Studio IDE'yi kullanmaya gerek kalmadan uygulama. Ardından, yüklü Visual Studio IDE varsa [bu](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) belge Visual Studio özel yönergeler içermektedir. Bu makalede bulunan yönergeleri izleyerek, Application Insights, ASP.NET Core uygulamanızı istekler, bağımlılıklar, özel durumlar, performans sayaçları, sinyal ve günlükleri toplama başlar. Kullanılan örnek uygulama bir [MVC uygulaması](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-2.2) hedefleyen `netcoreapp2.2`, ancak buradaki yönergeleri tüm ASP.NET Core uygulamaları için uygulanabilir. Özel durumların uygun olduğunda çağrılır.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

ASP.NET Core için Application Insights SDK (Yazılım Geliştirme Seti) burada veya nasıl bir uygulama çalıştırılır bağımsız olarak uygulamalarınızı izleyebilir. Uygulamanızı çalıştıran ve Application Insights hizmetinin ağ bağlantısı olan, telemetri toplanması beklenir. Bu destek içerir, ancak hiçbir işletim sistemi (Windows, Linux, Mac), barındırma yöntemini (işlem içi ve dışı işlem), dağıtım yöntemi (framework bağımlı vs müstakil), Web sunucusu (IIS, Kestrel), platform (Azure Web Apps, Azure VM, sınırlı değildir Docker, AKS vb.) veya IDE (Visual Studio, VS Code, komut satırı.)

## <a name="prerequisites"></a>Önkoşullar

- Çalışan bir ASP.NET Core uygulaması. İzleyin [bu](https://docs.microsoft.com/aspnet/core/getting-started/) gerekirse bir ASP.NET Core uygulaması oluşturmak için Kılavuzu.
- Application Insights hizmetine hiç telemetri göndermek için gerekli olan geçerli bir Application Insights izleme anahtarı. İzleyin [bunlar](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) gerekirse yeni bir Application Insights kaynağı oluşturun ve bir izleme anahtarı almak için yönergeler.

## <a name="enabling-application-insights-server-side-telemetry"></a>Application Insights sunucu tarafı Telemetri etkinleştirme

1. Normal bir NuGet paketi ASP.NET Core için Application Insights SDK'sını yükleyin. Her zaman en son kararlı sürümünü kullanmak için önerilir. Bu makalede açıklanan işlevler bir bölümü eski sürümlerde kullanılabilir olmayabilir. SDK'sı tam yayın notlarını bulunabilir [burada](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases). 

Şu proje için .csproj dosyasını eklenecek değişiklikleri gösterir.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
```

2. Ekleme `services.AddApplicationInsightsTelemetry();` için `ConfigureServices()` yönteminde, `Startup` sınıfı. Aşağıdaki örnekte tam.

```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // The following line enables Application Insights telemetry collection.
        services.AddApplicationInsightsTelemetry();

        // code adding other services for your application
        services.AddMvc();
    }
```

3. İzleme anahtarını ayarlayın.

   İzleme anahtarı bağımsız değişkeni olarak sağlanması mümkün olmakla birlikte `services.AddApplicationInsightsTelemetry("putinstrumentationkeyhere");`, yapılandırmada izleme anahtarını belirtmek için önerilir. Aşağıdaki bir izleme anahtarını belirteceğiniz gösterilmektedir `appsettings.json`. Emin `appsettings.json` yayımlanırken uygulama kök klasörüne kopyalanır.

```json
    {
      "ApplicationInsights": {
        "InstrumentationKey": "putinstrumentationkeyhere"
      },
      "Logging": {
        "LogLevel": {
          "Default": "Warning"
        }
      }
    }
```

    Alternately, the instrumentation key can also be specified in either of the following environment variables.
    APPINSIGHTS_INSTRUMENTATIONKEY
    ApplicationInsights:InstrumentationKey

    Example:
    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

`APPINSIGHTS_INSTRUMENTATIONKEY` genellikle Azure Web Apps'e dağıtılan uygulamaları için izleme anahtarını belirtmek için kullanılır.

> [!NOTE]
> Kod WINS'de ortam değişkeni içinde belirtilen bir izleme anahtarı `APPINSIGHTS_INSTRUMENTATIONKEY`, diğer seçeneklerine kıyasla daha fazla WINS.

4. Uygulamanızı çalıştırın ve istekleri olun. Telemetri, Application Insights'a akan artık başlamanız gerekir. Aşağıdaki telemetriyi Application Insights SDK'sı tarafından otomatik olarak toplanır.

    1. İstekleri - Gelen web uygulamanıza.
    1. Bağımlılıklar
        1. İle yapılan http/Https çağrıları `HttpClient`
        1. Yapılan SQL çağrıları `SqlClient`
        1. Azure depolama çağrıları ile yapılan [Azure depolama istemcisi](https://www.nuget.org/packages/WindowsAzure.Storage/)*
        1. [EventHub istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) 1.1.0 sürümü ve üzeri
        1. [Service Bus istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) sürüm 3.0.0 ve üzeri

             * Azure Cosmos DB, HTTP/HTTPS kullanılıyorsa otomatik olarak izlenir. TCP modu, Application Insights tarafından yakalanan olmaz.


    1. [Performans sayaçları](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)
        1. ASP.NET Core performans sayaçları için destek aşağıdaki sınırlıdır
            1. SDK'sı sürüm 2.4.1'i ve yukarıda uygulamayı Azure Web uygulaması (Windows) olarak çalışıyorsa performans sayaçlarını toplar
            1. SDK sürümü 2.7.0-beta3 ve yukarıdaki uygulama içinde Windows çalıştıran ve hedefleme performans sayaçlarını toplar `NETSTANDARD2.0` veya üzeri.
            1. .NET Framework'ün tamamını hedefleyen uygulamalarda, performans sayaçları, SDK'ın tüm sürümlerinde desteklenir.

            Linux performans sayacı destek eklendiğinde, bu belge güncelleştirilecektir.
    1. [Canlı Ölçümler](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)
    1. `ILogger` önem derecesi günlükleri `Warning` veya üzeri otomatik olarak yakalanan SDK sürümü 2.7.0-beta3 gelen veya üzeri. Daha fazla bilgi edinin [burada](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

Bu portalda görünmeye başlatmak telemetri birkaç dakika sürebilir. Her şeyin çalıştığından, kullanmak en iyisidir hemen kontrol etmek için [Canlı ölçümleri](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), çalışan bir uygulamaya yapma ister.

## <a name="send-ilogger-logs-to-application-insights"></a>Application Insights'a ILogger günlükleri gönderme

Application Insights ILogger gönderilen yakalama günlükleri destekler. Tam belgeleri okuyun [burada](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Web uygulamaları için istemci tarafı Telemetri etkinleştirme

Sunucu tarafı telemetri toplamaya başlamak yukarıdaki adımları yeterlidir. İstemci tarafı bileşenlerini uygulamanız varsa toplamaya başlamak için aşağıdaki adımları takip edin [kullanım telemetri](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) buradan.

1. Ekleme _viewımports.cshtml içinde ekleyin:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. _Layout.cshtml içinde HtmlHelper sonuna Ekle `<head>` bölümü, ancak önce başka bir komut dosyası. Sonra bu kod parçacığı sayfasından raporlamak istediğiniz tüm özel JavaScript telemetri eklenmesi:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Dosya adlarını asıl uygulamanız göre değiştirin. Yukarıda kullanılan varsayılan MVC uygulaması şablondan adlarıdır.

## <a name="configuring-application-insights-sdk"></a>Application Insights SDK yapılandırma

ASP.NET Core için Application Insights SDK'sı, varsayılan yapılandırmayı değiştirmek için özelleştirilebilir. Application Insights ASP.NET SDK kullanıcıları yapılandırma kullanımıyla ilgili bilgi sahibi olabilir `ApplicationInsights.config`, veya değiştirerek `TelemetryConfiguration.Active`. ASP.NET Core için yapılandırma farklı şekilde yapılır. ASP.NET Core SDK'sını kullanarak ASP.NET Core'nın yerleşik uygulamaya eklenen [DependencyInjection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) mekanizması ve aynı yapılandırma ayrıca kullanıyor DependencyInjection. Neredeyse tüm yapılandırma değişiklikleri Bitti `ConfigureServices()` yöntemi, `Startup.cs` aksi belirtilmedikçe, sınıf. Daha fazla bilgi için aşağıdaki bölümlerde izleyin.

> [!NOTE]
> Bu değiştirme yapılandırmasını değiştirerek dikkate almak önemlidir `TelemetryConfiguration.Active` ASP.NET Core uygulamalarında önerilmez.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>ApplicationInsightsServiceOptions kullanarak yapılandırma

Geçirerek birkaç ortak ayarları değiştirmek mümkündür `ApplicationInsightsServiceOptions` için `services.AddApplicationInsightsTelemetry();`. Bir örnek aşağıda gösterilmiştir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

Yapılandırılabilir ayarlarında tam listesini `ApplicationInsightsServiceOptions` bulunabilir [burada](https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/f0b8631e482d25982eb52092103b34e3ff6e5fef/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Örnekleme

ASP.NET Core için Application Insights SDK'sı FixedRate hem Uyarlamalı örnekleme destekler. Uyarlamalı örnekleme, varsayılan olarak etkindir. İzleyin [bu](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications) belge örnekleme ASP.NET Core uygulamaları için yapılandırma hakkında bilgi edinin.

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers ekleme

Yeni bir `TelemetryInitializer`, aşağıda gösterildiği gibi DependencyInjection kapsayıcının içine ekleyin. `TelemetryInitializer`s DependencyInjection kapsayıcısına eklenmiş SDK'sı tarafından otomatik olarak seçilir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>TelemetryInitializers kaldırılıyor

Tümünü kaldırmak için veya varsayılan olarak mevcut olan belirli TelemetryInitializers aşağıdaki örnek kod **sonra** çağırma `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>TelemetryProcessors ekleme

Özel telemetri işlemci eklenebilir `TelemetryConfiguration` genişletme yöntemini kullanarak `AddApplicationInsightsTelemetryProcessor` üzerinde `IServiceCollection`. Aşağıdaki örneği kullanın.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Yapılandırma veya varsayılan TelemetryModules kaldırılıyor

Aşağıdaki otomatik toplama modülleri, varsayılan olarak etkindir ve otomatik telemetri toplanması için sorumludur. Edilebilmeleri devre dışı bırakıldı ve varsayılan davranışı değiştirmek için yapılandırılmış.

1. `RequestTrackingTelemetryModule`
1. `DependencyTrackingTelemetryModule`
1. `PerformanceCollectorModule`
1. `QuickPulseTelemetryModule`
1. `AppServicesHeartbeatTelemetryModule`
1. `AzureInstanceMetadataTelemetryModule`

Herhangi bir varsayılan yapılandırmak için `TelemetryModule`, genişletme yöntemini kullanmak `ConfigureTelemetryModule<T>` üzerinde `IServiceCollection` aşağıdaki örnekte gösterildiği gibi.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-telemetry-channel"></a>Telemetri kanalı yapılandırma

Kullanılan varsayılan kanal `ServerTelemetryChannel`. Aşağıdaki örnek aşağıdaki kılınabilir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

*1. Bazı ek telemetri otomatik toplanan telemetriyi dışında izlemek istiyorsunuz. Bunu nasıl yapmalıyım?*

* Bir örneği elde `TelemetryClient` göre kullanarak Oluşturucu ekleme ve gerekli çağrı `TrackXXX()` yöntemini. Yeni önerilmez `TelemetryClient` tekil örneğini olarak ASP.NET Core uygulaması örnekleri `TelemetryClient` paylaşır DI kapsayıcısında zaten kayıtlı `TelemetryConfiguration` rest telemetri ile. Yeni bir oluşturma `TelemetryClient` örneği yalnızca telemetri geri kalanından ayrı bir yapılandırma için varsa önerilir. Aşağıdaki örnek, bir denetleyiciden ek telemetri izlemek gösterilmektedir.

```csharp
public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 Başvurmak [Application Insights özel ölçümler API Başvurusu](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) özel veri Application Insights'ta raporlama açıklaması.

*2. Bazı Visual Studio şablonları UseApplicationInsights() uzantısı IWebHostBuilder üzerinde Application Insights'ı etkinleştirmek için kullanılan yöntem. Bu kullanım hala geçerli mi?*

* Bu yöntem ile Application Insights'ı etkinleştirme geçerliyse ve kolaylaşmasına Visual Studio ve Azure Web uygulaması uzantıları da kullanılır. Ancak, kullanılacak önerilir `services.AddApplicationInsightsTelemery()` gibi bazı yapılandırma denetlemek için aşırı yüklemeler sağlar. Hem yöntemi dahili olarak aynı şeyi yapar, uygulanacak hiçbir özel yapılandırma varsa bunu, çağıran ya da ince ayar yapma.

*3. Ben my ASP.NET Core uygulamasını Azure Web Apps'e dağıtma. Web uygulamalarından Application Insights uzantısını etkinleştirmeniz gerekir?*

* SDK derleme sırasında bu makalede gösterilen şekilde yüklüyse, Web Apps portalından Application Insights uzantısını etkinleştirmek için gerek yoktur. Uzantı yüklü olsa bile, geri alma, SDK zaten uygulamaya eklendiğini algıladığında. Application Insights uzantısını etkinleştirme, yükleme ve SDK'sını uygulamanıza güncelleştirme serbest bırakır. Ancak, bu makalede Application Insights'ı etkinleştirme aşağıdaki nedenlerle daha esnektir.
    1. Application Insights Telemetrisi çalışmaya devam
        1. Tüm işletim sistemleri - Windows, Linux, Mac
        1. Tümünü Yayımla modu - müstakil ya da Framework bağımlı.
        1. Tam .NET Framework dahil olmak üzere tüm hedef çerçeve.
        1. Tüm barındırma seçenekleri - Azure Web uygulaması, VM'ler, Linux, kapsayıcılar, AKS, Azure dışı.
    1. Visual Studio'dan hata ayıklama sırasında telemetri yerel olarak görülebilir.
    1. İzleme kullanarak ek özel telemetri `TrackXXX()` API.
    1. Yapılandırmanız üzerinde tam denetime sahiptir.

*4. Application Insights Durum İzleyicisi gibi araçları kullanarak izleme etkinleştirebilir miyim?*

* Hayır. [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) ve yaklaşan değişimi [IISConfigurator](https://github.com/Microsoft/ApplicationInsights-Announcements/issues/21) şu anda ASP.NET yalnızca destekler. Belge güncelleştirilir ASP.NET Core uygulaması için destek.

*5. Bir ASP.NET Core 2.0 uygulamasını sahibim? Application ınsights'ı otomatik olarak bunlar için bana hiçbir şey etkin değil mi?*

* `Microsoft.AspNetCore.All` Application Insights SDK'sını (sürüm 2.1.0) 2.0 metapackage dahil ve uygulamayı Visual Studio hata ayıklayıcısı altında çalıştırırsanız, Visual Studio application ınsights'ı etkinleştirir ve telemetri IDE içinde yerel olarak gösterir. Bir izleme anahtarı açıkça belirtilmediği sürece Application Insights hizmetine telemetri gönderilmedi. Hatta 2.0 için Application ınsights'ı etkinleştirmek için bu makaledeki yönergeleri izlemeniz önerilir uygulamalar.

*6. Linux'ta Uygulamam çalıştırabilir. Tüm özellikler, Linux'ta de destekleniyor mu?*

* Evet. Özellik desteği SDK'sı aşağıdaki istisnalar dışında tüm platformlarda aynıdır:
    1. Performans sayaçları, Non-Windows henüz desteklenmemektedir. Linux desteği eklendiğinde, bu belge güncelleştirilecektir.
    1. Olsa da `ServerTelemetryChannel` etkin olmayan-windows, uygulama çalışıyorsa, varsayılan olarak, kanal otomatik olarak ağ sorunları varsa telemetri geçici olarak saklamak için bir yerel depolama klasör oluşturmaz. Bu sınırlama, telemetri geçici ağ veya sunucu sorunları varsa kaybolmasına neden olur. Bu sorunun geçici çözümü kullanıcının kanal için bir yerel klasör yapılandırmasını aşağıda gösterildiği gibi içindir.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```
