---
title: ASP.NET Core uygulamaları için Azure Application Insights | Microsoft Docs
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
ms.openlocfilehash: 5ea7ec41ccc721e8eafda56aa7463505ba089845
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827800"
---
# <a name="application-insights-for-aspnet-core-applications"></a>ASP.NET Core uygulamaları için Application Insights

Bu makalede için Application Insights'ı etkinleştirmek nasıl bir [ASP.NET Core](https://docs.microsoft.com/aspnet/core) uygulama. Bu makaledeki yönergeleri tamamladıktan sonra Application Insights ASP.NET Core uygulamanızı istekler, bağımlılıklar, özel durumlar, performans sayaçları, sinyal ve günlükleri toplayın. 

Kullanacağız burada örnek bir [MVC uygulaması](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) hedefleyen `netcoreapp2.2`. Tüm ASP.NET Core uygulamaları için bu yönergeleri uygulayabilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

[ASP.NET Core için Application Insights SDK'sı](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) olduğu ya da nasıl çalıştıkları fark etmeksizin uygulamalarınızı izleyebilirsiniz. Uygulamanızı çalıştıran ve Azure ağ bağlantısı olan, telemetri toplanabilir. Application Insights izleme .NET Core desteklenen her yerde desteklenir. Destek kapsar:
* **İşletim sistemi**: Windows, Linux veya Mac bilgisayar
* **Yöntem barındırma**: İşlem veya işlemi. 
* **Dağıtım yöntemi**: Framework bağımlı veya kendi içinde.
* **Web sunucusu**: IIS (Internet Information Server) veya Kestrel. 
* **Barındırma platformu**: Web Apps özelliği, Azure App Service, Azure VM, Docker, Azure Kubernetes Service (AKS) ve benzeri.
* **IDE**: Visual Studio, VS Code veya komut satırı.

## <a name="prerequisites"></a>Önkoşullar

- Çalışan bir ASP.NET Core uygulaması. Bir ASP.NET Core uygulaması oluşturmak ihtiyacınız varsa, bunu izleyin [ASP.NET Core Öğreticisi](https://docs.microsoft.com/aspnet/core/getting-started/).
- Geçerli bir Application Insights izleme anahtarı. Application ınsights'ı hiç telemetri göndermek için bu anahtar gereklidir. Bir izleme anahtarı, bkz: almak için yeni bir Application Insights kaynağı oluşturmak ihtiyacınız varsa [Application Insights kaynağı oluşturma](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Application Insights sunucu tarafı telemetri (Visual Studio) etkinleştir

1. Projenizi Visual Studio'da açın.

    > [!TIP]
    > İsterseniz, Application Insights yaptığı tüm değişiklikler izleyebilmek projeniz için kaynak denetimini ayarlayabilirsiniz. Kaynak denetimi etkinleştirmek için seçin **dosya** > **kaynak denetimine Ekle**.

2. Seçin **proje** > **Application Insights Telemetrisi Ekle**.

3. Seçin **başlama**. Bu seçimin metin, Visual Studio sürümünüze bağlı olarak değişebilir. Bazı eski sürümlerini kullanan bir **ücretsiz Başlat** yerine düğme.

4. Aboneliğinizi seçin. Ardından **kaynak** > **kaydetme**.

5. Projenize Application Insights ekledikten sonra SDK'sı en son kararlı sürümünü kullandığınızı onaylayın kontrol edin. Git **proje** > **NuGet paketlerini Yönet** > **Microsoft.ApplicationInsights.AspNetCore**. Gerekirse, seçin **güncelleştirme**.

     ![Güncelleştirme için Application Insights paketini seçmek nereye gösteren ekran görüntüsü](./media/asp-net-core/update-nuget-package.png)

6. İsteğe bağlı ipucu izleyen ve projeniz için kaynak denetimi eklendi, Git **görünümü** > **Takım Gezgini** > **değişiklikleri**. Ardından Application Insights telemetri tarafından yapılan değişiklikler fark görünümünü görmek için her bir dosyayı seçin.

## <a name="enable-application-insights-server-side-telemetry-no-visual-studio"></a>Application Insights sunucu tarafı telemetri (Visual Studio yok) etkinleştir

1. Yükleme [ASP.NET Core için Application Insights SDK'sı NuGet paketi](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Her zaman en son kararlı sürümünü kullanmanızı öneririz. Tam sürüm notları için SDK'sını bulmak [açık kaynaklı GitHub deposunu](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    Aşağıdaki kod örneği, projenizin eklenmesi yapılan değişiklikleri gösterir `.csproj` dosya.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.7.0" />
        </ItemGroup>
    ```

2. Ekleme `services.AddApplicationInsightsTelemetry();` için `ConfigureServices()` yönteminde, `Startup` sınıfı, bu örnekte olduğu gibi:

    ```csharp
        // This method gets called by the runtime. Use this method to add services to the container.
        public void ConfigureServices(IServiceCollection services)
        {
            // The following line enables Application Insights telemetry collection.
            services.AddApplicationInsightsTelemetry();
    
            // This code adds other services for your application.
            services.AddMvc();
        }
    ```

3. İzleme anahtarını ayarlayın.

    İzleme anahtarı bağımsız değişkeni olarak sağlamasına karşın `AddApplicationInsightsTelemetry`, izleme anahtarını yapılandırmasında belirtmenizi öneririz. Aşağıdaki kod örneği, bir izleme anahtarını belirtmek gösterilmektedir `appsettings.json`. Emin `appsettings.json` yayımlama sırasında uygulama kök klasörüne kopyalanır.

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

    Alternatif olarak, izleme anahtarını aşağıdaki ortam değişkenlerini birini belirtin:

    * `APPINSIGHTS_INSTRUMENTATIONKEY`

    * `ApplicationInsights:InstrumentationKey`

    Örneğin:

    * `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    * `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    Genellikle, `APPINSIGHTS_INSTRUMENTATIONKEY` Web Apps'e dağıtılan uygulamaları için izleme anahtarını belirtir.

    > [!NOTE]
    > Kod WINS ortam değişkeni içinde belirtilen bir izleme anahtarı `APPINSIGHTS_INSTRUMENTATIONKEY`, diğer seçeneklerine kıyasla daha fazla WINS.

## <a name="run-your-application"></a>Uygulamanızı çalıştırma

Uygulamanızı çalıştırın ve istekleri yapın. Telemetri, Application ınsights'ı artık akış. Application Insights SDK'sı, aşağıdaki telemetriyi otomatik olarak toplar.

|İstekleri/bağımlılıkları |Ayrıntılar|
|---------------|-------|
|İstekler | Uygulamanıza gelen web isteklerini. |
|HTTP veya HTTPS | İle yapılan çağrılar `HttpClient`. |
|SQL | İle yapılan çağrılar `SqlClient`. |
|[Azure Depolama](https://www.nuget.org/packages/WindowsAzure.Storage/) | Azure depolama istemci ile yapılan çağrılar. |
|[EventHubs istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | 1\.1.0 sürümü ve üzeri. |
|[Service Bus istemci SDK'sı](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Sürüm 3.0.0 ve daha sonra. |
|Azure Cosmos DB | Yalnızca HTTP/HTTPS kullanılıyorsa otomatik olarak izlenir. Application Insights TCP modunu yakalama değil. |

### <a name="performance-counters"></a>Performans sayaçları

Destek [performans sayaçları](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) ASP.NET Core sınırlıdır:

   * SDK sürümleri 2.4.1 ve Web uygulamaları (Windows) uygulama çalışıyorsa daha sonra Topla performans sayacı.
   * SDK sürümleri 2.7.0-beta3 ve sonraki toplama performans sayaçları, Windows ve hedefleri uygulama çalışıyorsa `NETSTANDARD2.0` veya üzeri.
   * .NET Framework'ü hedefleyen uygulamalar için performans sayaçları SDK'ın tüm sürümleri destekler.
 
Linux performans sayacı destek eklendiğinde bu makale güncelleştirilecektir.

### <a name="ilogger-logs"></a>ILogger günlükleri

[ILogger günlükleri](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) önem derecesi `Warning` veya üzeri sürümleri 2.7.0-beta3 SDK ve daha sonra otomatik olarak yakalanır.

### <a name="live-metrics"></a>Canlı ölçümleri

Bu telemetri portalda görünmeye başlaması birkaç dakika sürebilir. Hızlı bir şekilde her şeyin çalıştığından emin olmak için bunu kullanmak en iyisidir [Canlı ölçümleri](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream) çalışan uygulamaya istekleri yaptığınızda.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Web uygulamaları için istemci tarafı telemetri etkinleştirme

Önceki adımlar yeterli, sunucu tarafı telemetri toplamaya başlamak yardımcı olmak için yer almaktadır. Uygulamanızın istemci tarafı bileşenler varsa toplamaya başlamak için sonraki adımları [kullanım telemetri](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview).

1. İçinde `_ViewImports.cshtml`, ekleme ekleyin:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. İçinde `_Layout.cshtml`, Ekle `HtmlHelper` sonunda `<head>` bölümü, ancak önce başka bir komut dosyası. Rapor sayfasındaki tüm özel JavaScript telemetri istiyorsanız, sonra bu kod parçacığı Ekle:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

`.cshtml` Olan varsayılan MVC uygulama şablondan daha önce başvurulan dosya adları. Doğru uygulamanız için istemci tarafı izlemeyi etkinleştirmek istiyorsanız, sonuç olarak, JavaScript kod parçacığını görünmelidir `<head>` uygulamanızın izlemek istediğiniz her sayfanın bölümü. JavaScript kod parçacığını ekleyerek bu uygulama şablonu için bu hedefe ulaşmak `_Layout.cshtml`. 

Projenizi içermiyorsa `_Layout.cshtml`, eklemeye devam edebilirsiniz [istemci-tarafı izleme](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Bunu denetleyen bir eşdeğer dosyasına JavaScript kod parçacığını ekleyerek yapabilirsiniz `<head>` uygulamanızdaki tüm sayfa. Veya birden çok sayfa için kod parçacığı ekleyebilirsiniz, ancak bu çözüm elde etmek zordur ve genellikle önerilmez.

## <a name="configure-the-application-insights-sdk"></a>Application Insights SDK yapılandırma

Varsayılan yapılandırmayı değiştirmek ASP.NET Core için Application Insights SDK özelleştirebilirsiniz. Kullanıcıları, Application Insights ASP.NET SDK'sını kullanarak yapılandırmasını değiştirme ile ilgili bilgi sahibi olabilir `ApplicationInsights.config` veya değiştirerek `TelemetryConfiguration.Active`. ASP.NET Core için farklı şekilde yapılandırmasını değiştirin. ASP.NET Core SDK'sını uygulamanıza eklemek ve yerleşik ASP.NET Core kullanarak yapılandırma [bağımlılık ekleme](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Neredeyse tüm yapılandırma değişiklikleri yapmak `ConfigureServices()` yöntemi, `Startup.cs` sınıf sürece, aksi takdirde yönlendirilirsiniz. Aşağıdaki bölümlerde daha fazla bilgi sunar.

> [!NOTE]
> ASP.NET Core uygulamalarında değiştirerek yapılandırmasını değiştirme `TelemetryConfiguration.Active` desteklenmiyor.

### <a name="using-applicationinsightsserviceoptions"></a>Using ApplicationInsightsServiceOptions

Birkaç ortak ayarları geçirerek değiştirebileceğiniz `ApplicationInsightsServiceOptions` için `AddApplicationInsightsTelemetry`, bu örnekte olduğu gibi:

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

Daha fazla bilgi için [yapılandırılabilir ayarlarında `ApplicationInsightsServiceOptions` ](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>Örnekleme

ASP.NET Core için Application Insights SDK'sı hem sabit fiyat hem de Uyarlamalı örnekleme destekler. Uyarlamalı örnekleme, varsayılan olarak etkindir. 

Daha fazla bilgi için [Uyarlamalı örnekleme ASP.NET Core uygulamaları için yapılandırma](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications).

### <a name="adding-telemetryinitializers"></a>TelemetryInitializers ekleme

Kullanım [telemetri başlatıcılar](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) tüm telemetri ile gönderilen genel özellikleri tanımlamak istediğinizde.

Herhangi bir yeni Ekle `TelemetryInitializer` için `DependencyInjection` aşağıdaki kodda gösterildiği gibi bir kapsayıcı. SDK otomatik olarak tüm seçer `TelemetryInitializer` eklenen `DependencyInjection` kapsayıcı.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>TelemetryInitializers kaldırılıyor

Telemetri başlatıcılar, varsayılan olarak mevcuttur. Tümünü kaldırmak için veya belirli bir telemetri başlatıcılar, aşağıdaki örnek kod *sonra* çağırmanızı `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in telemetry initializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace by using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetry-processors"></a>Telemetri işlemciler ekleme

Özel telemetri işlemcilere ekleyebilirsiniz `TelemetryConfiguration` genişletme yöntemini kullanarak `AddApplicationInsightsTelemetryProcessor` üzerinde `IServiceCollection`. Telemetri işlemciler kullanan [Gelişmiş senaryoları filtreleme](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) ne dahil veya Application Insights hizmetine gönderdiğiniz telemetriyi dışında tutulan daha doğrudan denetime izin verecek şekilde. Aşağıdaki örneği kullanın.

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

Application Insights telemetri modüllerini kullanan [yararlı bilgiler otomatik olarak toplamak](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) hakkında ek yapılandırma gerektirmeden belirli iş yükleri.

Aşağıdaki koleksiyon otomatik modüller, varsayılan olarak etkindir. Bu modüller, telemetri toplanması için otomatik olarak sorumludur. Devre dışı bırakın veya bunları kendi varsayılan davranışı değiştirmek için yapılandırın.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Herhangi bir varsayılan yapılandırmak için `TelemetryModule`, genişletme yöntemini kullanmak `ConfigureTelemetryModule<T>` üzerinde `IServiceCollection`aşağıdaki örnekte gösterildiği gibi.

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

### <a name="configuring-a-telemetry-channel"></a>Telemetri kanalı yapılandırma

Varsayılan kanal `ServerTelemetryChannel`. Aşağıdaki örnekte gösterildiği gibi kılabilirsiniz.

```csharp
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // Use the following to replace the default channel with InMemoryChannel.
        // This can also be applied to ServerTelemetryChannel.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

### <a name="disable-telemetry-dynamically"></a>Telemetri dinamik olarak devre dışı bırak

Telemetri koşullu ve dinamik olarak devre dışı bırakmak istiyorsanız, size çözebilir `TelemetryConfiguration` örneği ile ASP.NET Core bağımlılık ekleme kapsayıcısını kodunuzdaki herhangi bir yere ve ayarlama `DisableTelemetry` bu bayrağı.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, TelemetryConfiguration configuration)
    {
        configuration.DisableTelemetry = true;
        ...
    }
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="how-can-i-track-telemetry-thats-not-automatically-collected"></a>Otomatik olarak toplanan telemetri nasıl izleyebilir miyim?

Bir kopyasını almak `TelemetryClient` göre kullanarak Oluşturucu ekleme ve gerekli çağrı `TrackXXX()` yöntemini. Yeni oluşturma önermemekteyiz `TelemetryClient` bir ASP.NET Core uygulaması örnekleri. Tekil örneğini `TelemetryClient` zaten kaydedilmiştir `DependencyInjection` paylaşır kapsayıcı `TelemetryConfiguration` rest telemetri ile. Yeni bir oluşturma `TelemetryClient` örneği yalnızca önerilen telemetri geri kalanından ayrı bir yapılandırma gerekiyorsa. 

Aşağıdaki örnek, bir denetleyiciden ek telemetri izlemek gösterilmektedir.

```csharp
using Microsoft.ApplicationInsights;

public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // Use constructor injection to get a TelemetryClient instance.
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // Call the required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

Özel veri Application Insights'ta raporlama hakkında daha fazla bilgi için bkz: [Application Insights özel ölçümler API Başvurusu](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/).

### <a name="some-visual-studio-templates-used-the-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Bazı Visual Studio şablonları UseApplicationInsights() uzantısı IWebHostBuilder üzerinde Application Insights'ı etkinleştirmek için kullanılan yöntem. Bu kullanım hala geçerli mi?

Evet, bu yöntem ile Application Insights'ı etkinleştirme geçerli değil. Bu teknik, Visual Studio ekleme ve Web Apps uzantıları kullanılır. Ancak, kullanmanızı öneririz `services.AddApplicationInsightsTelemetry()` çünkü bazı yapılandırma denetlemek için aşırı yüklemeler sağlar. Özel yapılandırma uygulamak gerekmiyorsa, her iki yöntem çağırması iki yöntem de aynı şeyi dahili olarak yapın.

### <a name="im-deploying-my-aspnet-core-application-to-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Ben Web Apps'e my ASP.NET Core uygulaması dağıtma. Web uygulamalarından Application Insights uzantısını etkinleştirmeniz gerekir?

SDK'ın oluşturma zamanında yüklendiğinden, bu makalede gösterilen şekilde, App Service Portalı'ndan Application Insights uzantısını etkinleştirmeniz gerekmez. Uzantı yüklü olsa bile SDK'sı zaten uygulamaya eklendiğini algıladığında kapalı yedekler. Application Insights uzantısını etkinleştirme, yükleme ve SDK'sı güncelleştirme gerekmez. Ancak bu makaledeki yönergeleri izleyerek Application Insights'ı etkinleştirirseniz, daha fazla esneklik için:

   * Application Insights telemetrisi çalışmaya devam:
       * Windows, Linux ve Mac gibi tüm işletim sistemleri
       * Tümünü Yayımla modları, kendi başına da dahil olmak üzere veya framework bağımlı.
       * Tam .NET Framework dahil olmak üzere tüm hedef çerçeve.
       * Web Apps, VM'ler, Linux, kapsayıcılar, Azure Kubernetes hizmeti ve Azure olmayan barındırma da dahil olmak üzere tüm barındırma seçenekleri.
   * Visual Studio'dan hata ayıklama sırasında telemetri yerel olarak görebilirsiniz.
   * Kullanarak ek özel telemetri izleyebilirsiniz `TrackXXX()` API.
   * Yapılandırmanız üzerinde tam denetim sahibi.

### <a name="can-i-enable-application-insights-monitoring-by-using-tools-like-status-monitor"></a>Application Insights Durum İzleyicisi gibi araçları kullanarak izleme etkinleştirebilir miyim?

Hayır. [Durum İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) ve [Durum İzleyicisi v2](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) şu anda ASP.NET desteği yalnızca 4.x.

### <a name="is-application-insights-automatically-enabled-for-my-aspnet-core-20-application"></a>Application Insights, ASP.NET Core 2.0 Uygulamam için otomatik olarak etkin mi?

`Microsoft.AspNetCore.All` 2.0 metapackage dahil Application Insights SDK'sını (sürüm 2.1.0). Uygulamayı Visual Studio hata ayıklayıcısı altında çalıştırırsanız, Visual Studio Application Insights'ı etkinleştirir ve telemetri IDE içinde yerel olarak gösterir. Bir izleme anahtarı belirtilmediyse Application Insights hizmetine telemetri gönderilmedi. Hatta 2.0 için Application ınsights'ı etkinleştirmek için bu makaledeki yönergeleri izleyerek öneririz uygulamalar.

### <a name="if-i-run-my-application-in-linux-are-all-features-supported"></a>Linux'ta Uygulamam çalıştırırsanız, tüm özellikler desteklenir?

Evet. Özellik desteği SDK'sı aşağıdaki istisnalar dışında tüm platformlarda aynıdır:

* Performans sayaçları, yalnızca Windows içinde desteklenir.
* Olsa da `ServerTelemetryChannel` etkin uygulama Linux veya MacOS çalıştırıyorsa, varsayılan olarak, kanal otomatik olarak ağ sorunları varsa telemetri geçici olarak saklamak için bir yerel depolama klasör oluşturmaz. Bu sınırlama nedeniyle, geçici ağ veya sunucu sorunları telemetri kaybolur. Bu sorunu geçici olarak çözmek için kanal için bir yerel klasör yapılandırın:

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure the channel to use the given folder to temporarily
        // store telemetry items during network or Application Insights server issues.
        // User should ensure that the given folder already exists
        // and that the application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="open-source-sdk"></a>Açık kaynak SDK'sı

[Okuma ve koda katkıda](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Video

- Bu dış adım adım videoya göz atın [Application Insights, Visual Studio ve .NET Core ile yapılandırma](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) sıfırdan.
- Bu dış adım adım videoya göz atın [.NET Core ve Visual Studio Code ile Application Insights'ı yapılandırın](https://youtu.be/ygGt84GDync) sıfırdan.

## <a name="next-steps"></a>Sonraki adımlar

* [Kullanıcı akışları keşfedin](../../azure-monitor/app/usage-flows.md) için kullanıcıların uygulamanızda nasıl gezindiğini anlayın.
* [Anlık görüntü koleksiyonunu yapılandırma](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) bir özel durum şu anda kaynak kodu ve değişkenleri durumunu görmek için.
* [API'yi kullanmak](../../azure-monitor/app/api-custom-events-metrics.md) kendi olaylarını ve metriklerini uygulamanızın performansına ve kullanımına ait ayrıntılı bir görünüm için gönderilecek.
* Kullanım [kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md) uygulamanızdan sürekli olarak dünyanın dört bir yanındaki denetlemek için.
* [ASP.NET core'da bağımlılık ekleme](https://docs.microsoft.com/aspnet/fundamentals/dependency-injection)
