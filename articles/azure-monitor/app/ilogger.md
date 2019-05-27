---
title: Azure Application ınsights ile ILogger .NET izleme günlüklerini inceleyin
description: Azure Application Insights ILogger sağlayıcısı ile ASP.NET Core ve konsol uygulamaları kullanma örnekleri.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: fd5a16334fff0319d7993fb2403a48d1777f6bce
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65955331"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>.NET Core ILogger günlükleri ApplicationInsightsLoggerProvider

ASP.NET Core günlük yerleşik ve üçüncü taraf sağlayıcılar farklı türde çalışır bir günlüğe kaydetme API'si destekler. Günlüğe kaydetme, arama yoluyla yapılır **Log()** veya türevleri üzerinde *ILogger* örnekleri. Bu makalede nasıl yapılacağı açıklanır *ApplicationInsightsLoggerProvider* konsolu ve ASP.NET Core uygulamaları ILogger günlükleri tutmak için. Bu makalede ayrıca ApplicationInsightsLoggerProvider diğer Application Insights telemetri ile nasıl tümleştirildiğini açıklar.
Daha fazla bilgi için bkz. [ASP.NET Core günlüğü](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

ApplicationInsightsLoggerProvider, varsayılan olarak etkindir [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.0-beta3 (ve sonraki sürümler) ne zaman kapatma normal Application Insights ya da standart ile izleme hakkında yöntemleri:
- Çağırarak **UseApplicationInsights** IWebHostBuilder üzerinde genişletme yöntemi 
- Çağırarak **AddApplicationInsightsTelemetry** IServiceCollection üzerinde genişletme yöntemi

Toplanan herhangi bir telemetri aynı yapılandırmaya ApplicationInsightsLoggerProvider yakalar ILogger günlükleri tabidir. Bunlar aynı dizi TelemetryInitializers ve TelemetryProcessors sahip, aynı TelemetryChannel kullanın ve bağıntılı olan ve aynı şekilde diğer telemetri örneklenir. Sürüm 2.7.0-beta3 kullanın veya daha sonra herhangi bir eylemi ILogger günlükleri tutmak için gerekli olur.

Yalnızca *uyarı* ya da daha yüksek ILogger günlükleri (tüm kategorilerden), varsayılan olarak Application Insights'a gönderilir. Ancak [bu davranışı değiştirmek için filtreler uygulayabilirler](#control-logging-level). ILogger günlüklerinden yakalamak için gerekli ek adımlar **Program.cs** veya **Startup.cs**. (Bkz [yakalama ILogger, ASP.NET Core uygulamalarında Startup.cs ve Program.cs günlükleri](#capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps).)

Microsoft.ApplicationInsights.AspNet SDK'ın önceki bir sürümü kullanılıyor veya yalnızca ApplicationInsightsLoggerProvider herhangi diğer Application Insights izleme olmadan kullanmak istiyorsanız, aşağıdaki yordamı kullanın:

1. NuGet paketini yükleyin:

   ```xml
       <ItemGroup>
         <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
       </ItemGroup>
   ```

1. Değiştirme **Program.cs** burada gösterildiği gibi:

   ```csharp
   using Microsoft.AspNetCore;
   using Microsoft.AspNetCore.Hosting;
   using Microsoft.Extensions.Logging;

   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
         WebHost.CreateDefaultBuilder(args)
           .UseStartup<Startup>()
         .ConfigureLogging(
               builder =>
               {
                   // Providing an instrumentation key here is required if you're using
                   // standalone package Microsoft.Extensions.Logging.ApplicationInsights
                   // or if you want to capture logs from early in the application startup
                   // pipeline from Startup.cs or Program.cs itself.
                   builder.AddApplicationInsights("ikey");

                   // Optional: Apply filters to control what logs are sent to Application Insights.
                   // The following configures LogLevel Information or above to be sent to
                   // Application Insights for all categories.
                   builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                    ("", LogLevel.Information);
               }
           );
   }
   ```

2. adımda kod yapılandırır `ApplicationInsightsLoggerProvider`. Aşağıdaki kod örneği kullanan denetleyici sınıfı gösterir `ILogger` günlükleri göndermek için. Günlükler, Application Insights tarafından yakalanır.

```csharp
public class ValuesController : ControllerBase
{
    private readonly `ILogger` _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all of them will have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
                _logger.LogWarning("An example of a Warning trace..");
                _logger.LogError("An example of an Error level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

### <a name="capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps"></a>Startup.cs ve Program.cs içinde ASP.NET Core uygulamaları ILogger günlükleri yakalama

Yeni ApplicationInsightsLoggerProvider günlüklerinden uygulama başlatma ardışık düzende erkenden yakalayabilir. ApplicationInsightsLoggerProvider Application ınsights'ı (sürüm 2.7.0-beta3 ile başlayarak) otomatik olarak etkin olsa da, bu kadar yukarı daha sonra işlem hattı ayarlayın. bir izleme anahtarı gerekli değildir. Bu nedenle, yalnızca oturumu **denetleyicisi**/ diğer sınıflar yakalanır. İle başlayarak her günlüğünü kaydetme amacıyla **Program.cs** ve **Startup.cs** kendisi, açıkça bir izleme anahtarı ApplicationInsightsLoggerProvider için etkinleştirmeniz gerekir. Ayrıca, *TelemetryConfiguration* 'oturumu tam olarak ayarlanmadı **Program.cs** veya **Startup.cs** kendisi. Bu nedenle bu günlükleri InMemoryChannel hiçbir örneklemeye ve hiçbir standart telemetri başlatıcılar veya işlemciler kullanan en az bir yapılandırma gerekir.

Bu özellik ile aşağıdaki örnekler göstermektedir **Program.cs** ve **Startup.cs**.

#### <a name="example-programcs"></a>Örnek Program.cs

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        // This will be picked up by AI
        logger.LogInformation("From Program. Running the host now..");
        host.Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
        builder =>
            {
            // Providing an instrumentation key here is required if you're using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Program).FullName, LogLevel.Trace);

            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Startup).FullName, LogLevel.Trace);
            }
        );
}
```

#### <a name="example-startupcs"></a>Örnek Startup.cs

```csharp
public class Startup
{
    private readonly `ILogger` _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following will be picked up by Application Insights.
        _logger.LogInformation("Logging from ConfigureServices.");
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

## <a name="migrate-from-the-old-applicationinsightsloggerprovider"></a>Eski ApplicationInsightsLoggerProvider geçirme

Artık bir oturum açma sağlayıcısı 2.7.0-Beta2 desteklenen önce Microsoft.ApplicationInsights.AspNet SDK sürümleri Kullanımdan kalktı. Bu sağlayıcı ile etkinleştirilen **AddApplicationInsights()** Iloggerfactory genişletme yönteminin. İki adımdan oluşur yeni sağlayıcısına geçiş yapma öneririz:

1. Kaldırma *ILoggerFactory.AddApplicationInsights()* çağırmanıza **Startup.Configure()** çift günlük kaydını önlemek için yöntemi.
2. Bunlar yeni sağlayıcı tarafından da dikkate değil çünkü kodda, hiçbir filtreleme kurallarını yeniden uygulayın. Overloads biri *ILoggerFactory.AddApplicationInsights()* minimum LogLevel veya filtre işlevleri sürdü. Yeni sağlayıcı ile filtreleme günlüğe kaydetme çerçevesinin bir parçasıdır. Application Insights sağlayıcı tarafından yapılmaz. Üzerinden sağlanan kadar herhangi bir filtre *ILoggerFactory.AddApplicationInsights()* aşırı kaldırılmalıdır. Ve filtreleme kuralları sağlanmalıdır izleyerek [günlük düzeyi denetim](#control-logging-level) yönergeleri. Kullanırsanız *appsettings.json* her ikisi de aynı sağlayıcı diğer kullandığından günlüğünü filtrelemek için bu yeni sağlayıcı ile çalışmaya devam edecektir *Applicationınsights*.

Eski sağlayıcısını kullanmaya devam edebilirsiniz. (Bu yalnızca bir ana sürüm değişikliği 3 kaldırılacak. *xx*.) Ancak, aşağıdaki nedenlerle yeni sağlayıcısına geçiş yapma öneririz:

- Önceki sağlayıcısını dönülemiyor [oturum kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). Yeni sağlayıcısında kapsam özellikleri özel özellikler toplanan telemetriyi otomatik olarak eklenir.
- Günlükleri artık çok daha önce uygulama başlangıç ardışık düzeninizde yakalanabilir. Oturumu **Program** ve **başlangıç** sınıfları artık tutulabilir.
- Yeni sağlayıcı ile filtre framework düzeyinde kendisini gerçekleştirilir. Konsolunda, hata ayıklama gibi yerleşik sağlayıcılar dahil olmak üzere diğer sağlayıcılar, olduğu gibi Application Insights sağlayıcı günlüklerine filtreleyebilir ve benzeri. Birden çok sağlayıcı için aynı filtreler de uygulayabilirsiniz.
- ASP.NET Core (2.0 ve üzeri) için önerilen yöntem, [günlük sağlayıcıları etkinleştir](https://github.com/aspnet/Announcements/issues/255) içinde ILoggingBuilder üzerinde genişletme yöntemleri kullanılarak **Program.cs** kendisi.

> [!Note]
> Yeni sağlayıcıyı NETSTANDARD2.0 hedefleyen uygulamalar için veya sonraki kullanılabilir. Uygulamanız .NET Core 1.1 gibi eski .NET Core sürümlerini hedefliyorsa veya .NET Framework hedefliyorsa, eski sağlayıcısını kullanmaya devam edin.

## <a name="console-application"></a>Konsol uygulaması

Aşağıdaki kod, örnek bir konsol uygulaması ILogger izlemeleri Application Insights'a göndermek için yapılandırıldığı gösterir.

Yüklü paketler:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />  
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create the DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(builder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // Application Insights for all categories.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("", LogLevel.Trace);
            builder.AddApplicationInsights("--YourAIKeyHere--");
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

Bu örnekte tek başına paketin `Microsoft.Extensions.Logging.ApplicationInsights`. Varsayılan olarak, bu yapılandırma "çıplak minimum" kullanır. TelemetryConfiguration Application Insights'a veri göndermek için. En az InMemoryChannel kullanılan kanal anlamına gelir. Hiçbir örnekleme ve Hayır standart TelemetryInitializers yoktur. Aşağıdaki örnekte gösterildiği gibi bir konsol uygulaması için bu davranışı geçersiz kılınabilir.

Bu paketi yükleyin:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Aşağıdaki bölümde kullanarak TelemetryConfiguration varsayılan geçersiz kılmak gösterilir **Hizmetleri. Yapılandırma<TelemetryConfiguration>()** yöntemi. Bu örnekte ayarlar `ServerTelemetryChannel` ve örnekleme. Özel ITelemetryInitializer TelemetryConfiguration için ekler.

```csharp
    // Create the DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding Application Insights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly calling Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if the application terminates, telemetry is sent to the back end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Denetim Günlük düzeyi

ASP.NET Core *ILogger* altyapısı uygulamak için yerleşik bir mekanizma vardır [günlük filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering). Bu Application Insights sağlayıcısı dahil olmak üzere her kayıtlı sağlayıcısına gönderilen günlükler denetlemenize olanak tanır. Filtrelemeyi yapılandırma ya da yapılabilir (kullanarak genellikle bir *appsettings.json* dosyası) veya kod. Bu özellik, framework tarafından sağlanır. Application Insights sağlayıcıya özgü değildir.

Aşağıdaki örnekler için ApplicationInsightsLoggerProvider filtre kuralları geçerlidir.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Filtre kurallarını appsettings.json yapılandırmasıyla oluşturun

Sağlayıcı diğer ApplicationInsightsLoggerProvider için olan `ApplicationInsights`. Aşağıdaki bölümde, *appsettings.json* günlükleri için yapılandırır *uyarı* ve yukarıdaki tüm kategorilerden ve *hata* ve yukarıdaki kategorilerden başlayın " Gönderileceği Microsoft" `ApplicationInsightsLoggerProvider`.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning",
        "Microsoft": "Error"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="create-filter-rules-in-code"></a>Kod içinde filtre kuralları oluşturma

Aşağıdaki kod parçacığı için günlükleri yapılandırır *uyarı* ve yukarıdaki tüm kategorilerden ve için *hata* ve yukarıdaki kategorilerden gönderilmesini "Microsoft" ile başlayan `ApplicationInsightsLoggerProvider`. Bu yapılandırma önceki bölümde gösterildiği gibi aynıdır *appsettings.json*.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-are-the-old-and-new-versions-of-applicationinsightsloggerprovider"></a>ApplicationInsightsLoggerProvider eski ve yeni sürümlerini nelerdir?

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) aracılığıyla etkin bir yerleşik ApplicationInsightsLoggerProvider (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider) dahil  **System.Diagnostics.tracesorce** genişletme yöntemleri. Bu sağlayıcı sürümü 2.7.0-beta2 ' eski olarak işaretlendi. Bir sonraki ana sürüm değişiklik tamamen kaldırılacaktır. [Microsoft.ApplicationInsights.AspNetCore 2.6.1](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) paket kendisini değil, artık kullanılmıyor. İstekler, bağımlılıklar ve benzeri izlemeyi etkinleştirmek için zorunludur.

Yeni tek başına paketin önerilen alternatiftir [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), geliştirilmiş bir ApplicationInsightsLoggerProvider (içerir Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) ve bunu etkinleştirmek için ILoggerBuilder üzerinde genişletme yöntemleri.

[Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.0-beta3 yeni paketi bir bağımlılık alır ve ILogger yakalama otomatik olarak etkinleştirir.

### <a name="why-are-some-ilogger-logs-shown-twice-in-application-insights"></a>Neden bazı ILogger günlükleri Application Insights'ta iki kez görüntülenir?

Çoğaltma etkin çağırarak ApplicationInsightsLoggerProvider (artık kullanılmayan) eski sürümü varsa meydana gelebilir `AddApplicationInsights` üzerinde `ILoggerFactory`. Kontrol edin, **yapılandırma** yöntemi aşağıdaki vardır ve bunu kaldırın:

```csharp
 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
 {
     loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
     // ..other code.
 }
```

Visual Studio'dan hata ayıklama sırasında çift günlüğe kaydetme deneyimi verilirse `EnableDebugLogger` için *false* kodda gibi Application ınsights'ı etkinleştirir. Bu çoğaltma ve düzeltme, yalnızca ilgili uygulama hata ayıklama işlemi yaparken.

```csharp
 public void ConfigureServices(IServiceCollection services)
 {
     ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
     options.EnableDebugLogger = false;
     services.AddApplicationInsightsTelemetry(options);
     // ..other code.
 }
```

### <a name="i-updated-to-microsoftapplicationinsightsaspnet-sdkhttpswwwnugetorgpackagesmicrosoftapplicationinsightsaspnetcore-version-270-beta3-and-logs-from-ilogger-are-captured-automatically-how-do-i-turn-off-this-feature-completely"></a>Ben için güncelleştirilmiş [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.0-beta3 ve ILogger günlüklerinden yakalanır otomatik olarak. Bu özelliği tamamen nasıl kapatırım?

Bkz: [günlük düzeyi denetim](../../azure-monitor/app/ilogger.md#control-logging-level) bölümü nasıl filtreleme yapılacağını görmek için genel olarak günlüğe kaydeder. Denetlediğinde ApplicationInsightsLoggerProvider için kullanmak `LogLevel.None`:

**Kod:**

```csharp
    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                      ("", LogLevel.None);
```

**Yapılandırmada:**

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "None"
      }
}
```

### <a name="why-do-some-ilogger-logs-not-have-the-same-properties-as-others"></a>Neden bazı ILogger günlükleri başkalarının aynı özelliklere sahip değil?

Application Insights yakalar ve diğer her telemetri için kullanılan aynı TelemetryConfiguration kullanarak ILogger günlükleri gönderir. Ancak bir özel durum söz konusudur. Gelen oturum açtığınızda varsayılan olarak, TelemetryConfiguration tam olarak ayarlanmadı **Program.cs** veya **Startup.cs**. Tüm TelemetryInitializers ve TelemetryProcessors çalışıyor olurlarsa olmaz için varsayılan yapılandırma günlükleri şu konumlardan olmaz.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-i-want-to-log-some-additional-custom-telemetry-manually-how-should-i-do-that"></a>Tek başına paketin Microsoft.Extensions.Logging.ApplicationInsights kullanıyorum ve bazı ek özel telemetri el ile oturum istiyorum. Bu ne yapmalıyım?

Tek başına paketin kullandığınızda `TelemetryClient` , yeni bir örneğini oluşturmak için gereken şekilde DI kapsayıcıya eklenen değil `TelemetryClient` ve Günlükçü sağlayıcısı kullandıkça aşağıdaki kodun gösterdiği olarak aynı yapılandırmayı kullan. Bu, aynı yapılandırmayı ILogger alınan telemetri yanı sıra tüm özel telemetri için kullanılmasını sağlar.

```csharp
public class MyController : ApiController
{
   // This telemtryclient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Application Insights'ı etkinleştirmek için Microsoft.ApplicationInsights.AspNetCore paket kullanırsanız, almak için bu kodu Değiştir `TelemetryClient` doğrudan oluşturucuda. Bir örnek için bkz. [bu SSS](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core#frequently-asked-questions).


### <a name="what-application-insights-telemetry-type-is-produced-from-ilogger-logs-or-where-can-i-see-ilogger-logs-in-application-insights"></a>Application Insights telemetri türüne ILogger günlükleri üretilir? Veya burada ILogger Application Insights'ta günlüklerini görebilir miyim?

ApplicationInsightsLoggerProvider ILogger günlükleri yakalar ve bunları TraceTelemetry oluşturur. Bir özel durum nesnesi iletilmezse **Log()** ILogger, metodunda *ExceptionTelemetry* TraceTelemetry yerine oluşturulur. Bu telemetri öğelerinin portal, analytics veya Visual Studio yerel hata ayıklayıcı gibi Application Insights için aynı yerde herhangi bir TraceTelemetry veya ExceptionTelemetry olarak bulunabilir.

Her zaman TraceTelemetry göndermek isterseniz, bu kod parçacığı kullanın: ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```

### <a name="i-dont-have-the-sdk-installed-and-i-use-the-azure-web-apps-extension-to-enable-application-insights-for-my-aspnet-core-applications-how-do-i-use-the-new-provider"></a>SDK'ın yüklü yok mu ve my ASP.NET Core uygulamaları için Application Insights'ı etkinleştirmek için Azure Web Apps uzantısı kullanabilirim. Yeni sağlayıcıyı nasıl kullanabilirim? 

Application Insights uzantısını Azure Web apps'te eski sağlayıcısı kullanır. Filtreleme kuralları değiştirebilirsiniz *appsettings.json* uygulamanız için dosya. Yeni sağlayıcının yararlanmak için üzerinde SDK'sı bir NuGet bağımlılık yararlanarak derleme Araçları'nı kullanın. Yeni sağlayıcıyı kullanmak için uzantı geçiş yaptığında Bu makale güncelleştirilecektir.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-enabling-application-insights-provider-by-calling-builderaddapplicationinsightsikey-is-there-an-option-to-get-an-instrumentation-key-from-configuration"></a>Tek başına paketin Microsoft.Extensions.Logging.ApplicationInsights kullanarak ve Application Insights sağlayıcısı çağırarak etkinleştirme **Oluşturucusu. AddApplicationInsights("ikey")**. Yapılandırmasından bir izleme anahtarı almak için bir seçenek var mı?


Program.cs ve appsettings.json aşağıdaki gibi değiştirin:

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateWebHostBuilder(args).Build().Run();
       }

       public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
           WebHost.CreateDefaultBuilder(args)
               .UseStartup<Startup>()
               .ConfigureLogging((hostingContext, logging) =>
               {
                   // hostingContext.HostingEnvironment can be used to determine environments as well.
                   var appInsightKey = hostingContext.Configuration["myikeyfromconfig"];
                   logging.AddApplicationInsights(appInsightKey);
               });
   }
   ```

   İlgili bölümünden `appsettings.json`:

   ```json
   {
     "myikeyfromconfig": "putrealikeyhere"
   }
   ```

Bu kod, yalnızca bir tek başına günlük sağlayıcısını kullandığınızda gereklidir. Normal Application Insights izleme için izleme anahtarını otomatik olarak yapılandırma yolundan yüklenir *Applicationınsights: Instrumentationkey*. AppSettings.JSON şöyle görünmelidir:

   ```json
   {
     "ApplicationInsights":
       {
           "Instrumentationkey":"putrealikeyhere"
       }
   }
   ```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi:

* [ASP.NET core'da günlüğe kaydetme](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [Application Insights'ta .NET izleme günlükleri](../../azure-monitor/app/asp-net-trace-logs.md)
