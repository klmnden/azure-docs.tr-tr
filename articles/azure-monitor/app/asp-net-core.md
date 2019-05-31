---
title: ASP.NET Core için Azure Application Insights | Microsoft Docs
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
ms.date: 05/22/2019
ms.author: mbullwin
ms.openlocfilehash: cb7ace20fd0a59dafff3d7f8240f54c3c8e12492
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66226409"
---
# <a name="application-insights-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için Application Insights

Bu makalede için Application Insights'ı etkinleştirmek nasıl bir [ASP.NET Core](https://docs.microsoft.com/aspnet/core) uygulama. Bu makaledeki yönergeleri tamamladıktan sonra Application Insights ASP.NET Core uygulamanızı istekler, bağımlılıklar, özel durumlar, performans sayaçları, sinyal ve günlükleri toplamaya başlar. Örnek uygulama bir [MVC uygulaması](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) hedefleyen `netcoreapp2.2`, ancak bu yönergeler, tüm ASP.NET Core uygulamaları için geçerlidir.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

[ASP.NET Core için Application Insights SDK (Yazılım Geliştirme Seti)](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) nereye veya nasıl bir uygulama çalıştırılır bağımsız olarak uygulamalarınızı izleyebilir. Uygulamanızı çalıştıran ve Azure ağ bağlantısı olan, telemetri toplanabilir. Bu, Application Insights izleme .NET Core desteklenen her yerde desteklendiği anlamına gelir. Bu destek, içeren herhangi bir işletim sistemi (Windows, Linux, Mac), barındırma yöntemini (işlem içi ve dışı işlem), dağıtım yöntemi (framework bağımlı vs müstakil), Web sunucusu (IIS, Kestrel), barındırma Platformu (Azure Web Apps, Azure VM, Docker, Azure Kubernetes Service'i (AKS) ve bu şekilde devam eder.) veya IDE (Visual Studio, VS Code, komut satırı.)

## <a name="prerequisites"></a>Önkoşullar

- Çalışan bir ASP.NET Core uygulaması. İzleyin [alma ASP.NET Core ile çalışmaya başlama Kılavuzu](https://docs.microsoft.com/aspnet/core/getting-started/) gerekirse bir ASP.NET Core uygulaması oluşturmak için.
- Application Insights hizmetine hiç telemetri göndermek için gerekli olan geçerli bir Application Insights izleme anahtarı. İzleyin [yönergeleri kaynak Oluştur](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) gerekirse yeni bir Application Insights kaynağı oluşturun ve bir izleme anahtarı edinmek için.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Application Insights sunucu tarafı telemetri (Visual Studio) etkinleştir

1. Projenizi Visual Studio'da açın.

    > [!TIP]
    > Değil zorunlu bir adım sırasında projeniz için kaynak denetimi, Application Insights tarafından yapılan tüm değişiklikleri izleyebilirsiniz şekilde ayarlamak yararlı olabilir. Kaynak Denetimi Seç etkinleştirmek için **dosya** > **kaynak denetimine Ekle**.

2. Seçin **proje** > **Application Insights Telemetrisi Ekle**.

3. Seçin **başlama**. (Visual Studio sürümüne bağlı olarak, metin biraz farklı olabilir. Bazı daha önceki sürümlerde bir **ücretsiz Başlat** yerine düğme.)

4. Aboneliğinizi seçin ve ardından **kaynak** > **kaydetme**.

5. Projenize Application Insights ekledikten sonra SDK'sı en son kararlı sürümünü kullandığınızı onaylayın kontrol edin. Git **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore** > seçin,gerekirse**Güncelleştirme**.

     ![Ekran görüntüsü, Application Insights paketini güncelleştirmek için seçilen ile NuGet paketini ekran yönetme](./media/asp-net-core/update-nuget-package.png)

6. İsteğe bağlı ipucu izleyen ve proje kaynak denetimine eklediğiniz ardından gidebilirsiniz **görünümü** > **Takım Gezgini** > **değişiklikleri** ve Application Insights telemetri eklemesini yapılan değişiklikler fark görünümü her bir dosya seçin.

## <a name="enable-application-insights-server-side-telemetry-without-visual-studio"></a>Application Insights sunucu tarafı telemetri (Visual Studio) olmadan etkinleştir

1. Yükleme [ASP.NET Core için Application Insights SDK'sı NuGet paketi](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Her zaman en son kararlı sürümünü kullanmanızı öneririz. SDK'sı tam yayın notlarını bulunabilir [kaynak GitHub deposunu açmak](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    Aşağıdaki kod parçacığı, projenizin eklenmesi yapılan değişiklikleri gösterir `.csproj` dosya.

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

    İzleme anahtarı bağımsız değişkeni olarak sağlanması mümkün olmakla birlikte `AddApplicationInsightsTelemetry`, izleme anahtarını yapılandırmasında belirtmenizi öneririz. Aşağıdaki bir izleme anahtarını belirteceğiniz gösterilmektedir `appsettings.json`. Emin `appsettings.json` yayımlanırken uygulama kök klasörüne kopyalanır.

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

    Alternatif olarak, izleme anahtarını da aşağıdaki ortam değişkenlerini birini belirtilebilir.

    APPINSIGHTS_INSTRUMENTATIONKEY

    ApplicationInsights:InstrumentationKey

    Örnek:

    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    `APPINSIGHTS_INSTRUMENTATIONKEY` genellikle Azure Web Apps'e dağıtılan uygulamaları için izleme anahtarını belirtmek için kullanılır.

    > [!NOTE]
    > Kod WINS'de ortam değişkeni içinde belirtilen bir izleme anahtarı `APPINSIGHTS_INSTRUMENTATIONKEY`, diğer seçeneklerine kıyasla daha fazla WINS.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

 Uygulamanızı çalıştırın ve istekleri olun. Telemetri, Application Insights'a akan artık başlamanız gerekir. Aşağıdaki telemetriyi Application Insights SDK'sı tarafından otomatik olarak toplanır.

|İstekleri/bağımlılıkları |Ayrıntılar|
|---------------|-------|
|İstekler | Uygulamanıza gelen web isteklerini. |
|HTTP/Https | İle yapılan çağrılar `HttpClient`. |
|SQL | İle yapılan çağrılar `SqlClient`. |
|[Azure depolama alanı](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure depolama istemci ile yapılan çağrılar. |
|[EventHub istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1.1.0 sürümü ve üzeri. |
|[Service Bus istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve üstü. |
|Azure Cosmos DB | HTTP/HTTPS kullanılıyorsa yalnızca otomatik olarak izlenir. TCP modu, Application Insights tarafından yakalanan olmaz. |

### <a name="performance-counters"></a>Performans Sayaçları

Destek [performans sayaçları](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) ASP.NET Core aşağıdaki sınırlıdır

   * SDK'sı sürüm 2.4.1'i ve yukarıda uygulamayı Azure Web uygulaması (Windows) olarak çalışıyorsa performans sayaçlarını toplar
   * SDK sürümü 2.7.0-beta3 ve yukarıdaki uygulama içinde Windows çalıştıran ve hedefleme performans sayaçlarını toplar `NETSTANDARD2.0` veya üzeri.
   * .NET Framework'ü hedefleyen uygulamalar için performans sayaçları, SDK'ın tüm sürümlerinde desteklenir.
   * Linux performans sayacı destek eklendiğinde bu makale güncelleştirilecektir.

### <a name="ilogger-logs"></a>ILogger günlükleri

[ILogger günlükleri](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) önem derecesi `Warning` veya üzeri otomatik olarak yakalanan SDK sürümü 2.7.0-beta3 gelen veya üzeri.

### <a name="live-metrics"></a>Canlı ölçümleri

Bu portalda görünmeye başlatmak telemetri birkaç dakika sürebilir. Her şeyin çalıştığından, kullanmak en iyisidir hemen kontrol etmek için [Canlı ölçümleri](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), çalışan bir uygulamaya yapma ister.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Web uygulamaları için istemci tarafı Telemetri etkinleştirme

Sunucu tarafı telemetri toplamaya başlamak yukarıdaki adımları yeterlidir. Uygulamanızın istemci tarafı bileşenler varsa toplamaya başlamak için aşağıdaki adımları takip edin [kullanım telemetri](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) buradan.

1. İçinde `_ViewImports.cshtml`, ekleme ekleyin:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. İçinde `_Layout.cshtml`, HtmlHelper sonuna Ekle `<head>` bölümü, ancak önce başka bir komut dosyası. Sonra bu kod parçacığı sayfasından raporlamak istediğiniz tüm özel JavaScript telemetri eklenmesi:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

`.cshtml` Yukarıda anılan dosya adı, varsayılan MVC uygulama şablondan olur. Sonuç olarak, düzgün bir şekilde uygulamanız için istemci tarafı izlemeyi etkinleştirmek için bulunması için JavaScript kod parçacığını ihtiyacınız `<head>` uygulamanızın izlemek istediğiniz her sayfanın bölümü. Bu uygulama şablonu, Javascript kod parçacığını eklemek için `_Layout.cshtml` bu amacı gerçekleştirmenize etkili. Projenizi bu dosya yoksa eklemeye devam edebilirsiniz [istemci-tarafı izleme](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Ya da JavaScript denetleyen bir eşdeğer dosyasına eklemeniz yeterlidir `<head>` birden çok kişi kod parçacığını bu bakımını yapmak zor olacaktır ancak sayfaları ve genel olarak değil, uygulamanızı veya alternatif olarak, içindeki tüm sayfa sonuna ekleyebilirsiniz önerilir.

## <a name="configuring-application-insights-sdk"></a>Application Insights SDK yapılandırma

ASP.NET Core için Application Insights SDK'sı, varsayılan yapılandırmayı değiştirmek için özelleştirilebilir. Application Insights ASP.NET SDK kullanıcıları yapılandırma kullanımıyla ilgili bilgi sahibi olabilir `ApplicationInsights.config`, veya değiştirerek `TelemetryConfiguration.Active`. ASP.NET Core için yapılandırma farklı şekilde yapılır. ASP.NET Core SDK'sını uygulamaya eklenir ve ASP.NET Core'nın yerleşik kullanılarak yapılandırılan [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Neredeyse tüm yapılandırma değişiklikleri Bitti `ConfigureServices()` yöntemi, `Startup.cs` aksi belirtilmedikçe, sınıf. Daha fazla bilgi için aşağıdaki bölümlerde izleyin.

> [!NOTE]
>  Değiştirerek yapılandırması değiştiriliyor `TelemetryConfiguration.Active` ASP.NET Core uygulamalarında önerilmez.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>ApplicationInsightsServiceOptions kullanarak yapılandırma

Geçirerek birkaç ortak ayarları değiştirmek mümkündür `ApplicationInsightsServiceOptions` için `AddApplicationInsightsTelemetry`. Bir örnek aşağıda gösterilmiştir.

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

Yapılandırılabilir ayarlarında tam listesini `ApplicationInsightsServiceOptions` bulunabilir [burada](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Örnekleme

ASP.NET Core için Application Insights SDK'sı FixedRate hem Uyarlamalı örnekleme destekler. Uyarlamalı örnekleme, varsayılan olarak etkindir. İzleyin bizim [Uyarlamalı örnekleme yönergeler](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications), örnekleme ASP.NET Core uygulamaları için yapılandırma hakkında bilgi edinin.

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers ekleme

[Telemetri başlatıcılar](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) tüm telemetri ile gönderilen genel özellikleri tanımlamak istediğinizde kullanılır.

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

Özel telemetri işlemci eklenebilir `TelemetryConfiguration` genişletme yöntemini kullanarak `AddApplicationInsightsTelemetryProcessor` üzerinde `IServiceCollection`. Telemetri işlemcileri kullanıldığı [Gelişmiş senaryoları filtreleme](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) ne dahil veya Application Insights hizmetine gönderdiğiniz telemetriyi dışında tutulan daha doğrudan denetime izin verecek şekilde. Aşağıdaki örneği kullanın.

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

Application Insights telemetri modülleri bir yolu olarak kullandığı [otomatik olarak toplamak yararlı bilgiler](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) hakkında ek yapılandırma gerektirmeden belirli iş yükleri.

Aşağıdaki otomatik toplama modülleri, varsayılan olarak etkindir ve otomatik telemetri toplanması için sorumludur. Edilebilmeleri devre dışı bırakıldı ve varsayılan davranışı değiştirmek için yapılandırılmış.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Herhangi bir varsayılan yapılandırmak için `TelemetryModule`, genişletme yöntemini kullanmak `ConfigureTelemetryModule<T>` üzerinde `IServiceCollection` aşağıdaki örnekte gösterildiği gibi.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

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
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="i-want-to-track-additional-telemetry-other-than-the-auto-collected-telemetry-how-do-i-do-it"></a>Otomatik toplanan telemetriyi dışında ek telemetri izlemek istiyorsunuz. Bunu nasıl yapmalıyım?

Bir örneği elde `TelemetryClient` göre kullanarak Oluşturucu ekleme ve gerekli çağrı `TrackXXX()` yöntemini. Yeni önerilmez `TelemetryClient` tekil örneğini olarak ASP.NET Core uygulaması örnekleri `TelemetryClient` paylaşır DI kapsayıcısında zaten kayıtlı `TelemetryConfiguration` rest telemetri ile. Yeni bir oluşturma `TelemetryClient` örneği yalnızca telemetri geri kalanından ayrı bir yapılandırma için varsa önerilir. Aşağıdaki örnek, bir denetleyiciden ek telemetri izlemek gösterilmektedir.

```csharp
using Microsoft.ApplicationInsights;

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

### <a name="some-visual-studio-templates-used-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Bazı Visual Studio şablonları UseApplicationInsights() uzantısı IWebHostBuilder üzerinde Application Insights'ı etkinleştirmek için kullanılan yöntem. Bu kullanım hala geçerli mi?

Bu yöntem ile Application Insights'ı etkinleştirme geçerliyse ve kolaylaşmasına Visual Studio ve Azure Web uygulaması uzantıları da kullanılır. Ancak, kullanılacak önerilir `services.AddApplicationInsightsTelemetry()` gibi bazı yapılandırma denetlemek için aşırı yüklemeler sağlar. Hem yöntemleri dahili olarak aynı şeyi yapmak, uygulanacak hiçbir özel yapılandırma varsa bunu, çağıran ya da ince ayar yapma.

### <a name="i-am-deploying-my-aspnet-core-application-to-azure-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ben my ASP.NET Core uygulamasını Azure Web Apps'e dağıtma. Web uygulamalarından Application Insights uzantısını etkinleştirmeniz gerekir?

SDK'ın oluşturma zamanında yüklendiğinden, bu makalede gösterilen şekilde, App Service Portalı'ndan Application Insights uzantısını etkinleştirmek için gerek yoktur. Uzantı yüklü olsa bile SDK'sı zaten uygulamaya eklendiğini algıladığında kapalı yedekler. Application Insights uzantısını etkinleştirme, yükleme ve SDK'sı güncelleştirme serbest bırakır. Ancak, bu makalede Application Insights'ı etkinleştirme aşağıdaki nedenlerle daha esnektir.
   * Application Insights Telemetrisi çalışmaya devam:
       * Tüm işletim sistemleri - Windows, Linux, Mac
       * Tümünü Yayımla modları - müstakil ya da Framework bağımlı.
       * Tam .NET Framework dahil olmak üzere tüm hedef çerçeve.
       * Tüm barındırma seçenekleri - Azure Web uygulaması, VM'ler, Linux, kapsayıcılar, AKS, Azure dışı.
   * Visual Studio'dan hata ayıklama sırasında telemetri yerel olarak görülebilir.
   * İzleme kullanarak ek özel telemetri `TrackXXX()` API.
   * Yapılandırmanız üzerinde tam denetim sahibi.

### <a name="can-i-enable-application-insights-monitoring-using-tools-like-status-monitor"></a>Application Insights Durum İzleyicisi gibi araçları kullanarak izleme etkinleştirebilir miyim?

Hayır. [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) ve yaklaşan değişimi [Durum İzleyicisi v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) şu anda ASP.NET destekler yalnızca 4.x.

### <a name="i-have-an-aspnet-core-20-application-isnt-application-insights-automatically-enabled-without-me-doing-anything"></a>Bir ASP.NET Core 2.0 Uygulamam var. Application Insights bana hiçbir şey otomatik olarak etkin değil mi?

`Microsoft.AspNetCore.All` Application Insights SDK'sını (sürüm 2.1.0) 2.0 metapackage dahil ve uygulamayı Visual Studio hata ayıklayıcısı altında çalıştırırsanız, Visual Studio Application Insights'ı etkinleştirir ve telemetri IDE içinde yerel olarak gösterir. Bir izleme anahtarı açıkça belirtilmediği sürece Application Insights hizmetine telemetri gönderilmedi. Hatta 2.0 için Application ınsights'ı etkinleştirmek için bu makaledeki yönergeleri izleyerek öneririz uygulamalar.

### <a name="i-run-my-application-in-linux-are-all-features-supported-in-linux-as-well"></a>Linux'ta Uygulamam çalıştırabilir. Tüm özellikler, Linux'ta de destekleniyor mu?

* Evet. Özellik desteği SDK'sı aşağıdaki istisnalar dışında tüm platformlarda aynıdır:

    * Performans sayaçları, Non-Windows henüz desteklenmemektedir.
    * Olsa da `ServerTelemetryChannel` etkin uygulama Linux veya MacOS çalıştırıyorsa, varsayılan olarak, kanal otomatik olarak ağ sorunları varsa telemetri geçici olarak saklamak için bir yerel depolama klasör oluşturmaz. Bu sınırlama, telemetri geçici ağ veya sunucu sorunları varsa kaybolmasına neden olur. Bu sorunun geçici çözümü kullanıcının kanal için bir yerel klasör yapılandırmasını aşağıda gösterildiği gibi içindir.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

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

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı
[Okuma ve koda katkıda](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- İlgili dış adım adım video [.NET Core ve Visual Studio ile Application Insights yapılandırma](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) sıfırdan.
- İlgili dış adım adım video [.NET Core ve Visual Studio Code ile Application Insights yapılandırma](https://youtu.be/ygGt84GDync) sıfırdan.

## <a name="next-steps"></a>Sonraki adımlar
* [Kullanıcı akışları keşfedin](../../azure-monitor/app/usage-flows.md) için kullanıcıların uygulamanızda nasıl gezindiğini anlayın.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API'yi kullanmak](../../azure-monitor/app/api-custom-events-metrics.md) kendi olayları ve ölçümleri daha ayrıntılı bir görünüm, uygulamanızın performans ve kullanım için gönderilecek.
* Kullanım [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) uygulamanızdan sürekli olarak dünyanın dört bir yanındaki denetlemek için.
