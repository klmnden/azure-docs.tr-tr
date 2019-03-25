---
title: Azure Application ınsights ile ILogger .NET izleme günlüklerini inceleyin
description: Azure Application Insights ILogger uygulama ile ASP.NET Core ve konsol uygulamaları kullanma örnekleri.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 4c385d2af0d9e4e213cd690b3c30d8719588220d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2019
ms.locfileid: "58399493"
---
# <a name="ilogger"></a>ILogger

ASP.NET Core çeşitli günlük yerleşik ve üçüncü taraf sağlayıcılar ile çalışan bir günlüğe kaydetme API'si destekler. Bu makalede, günlük Application Insights ILogger uygulama hem Konsol hem de ASP.NET Core uygulamaları ile nasıl ele alınacağını gösterir. Temel ILogger günlüğü hakkında daha fazla bilgi için bkz. [bu makalede](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="console-application"></a>Konsol uygulaması

Aşağıdaki örnek, ILogger izlemeleri Application Insights'a gönderme için yapılandırılmış bir konsol uygulaması gösterir.

Yüklü paketler:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create DI container.
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
        services.AddLogging(loggingBuilder =>
        {
        // Optional: Apply filters to configure LogLevel Trace or above is sent to ApplicationInsights for all
        // categories.
        loggingBuilder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");                
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional. Epecially in case of AspNetCore request info is already
        // present in scope.
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

## <a name="aspnet-core-application"></a>ASP.NET Core uygulaması

Aşağıdaki örnek bir ASP.NET Core uygulaması ILogger izlemeleri application ınsights'a gönderme şekilde gösterir. Bu örnekte, Program.cs, Startup.cs ya da başka bir Contoller/uygulama mantığı ILogger izlemeleri göndermeyi izlenebilir.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        logger.LogInformation("From Program. Running the host now.."); // This will be picked up by AI
        host.Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(logging =>
        {
            logging.AddApplicationInsights("ikeyhere");

            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // ApplicationInsights for all categories.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);

            // Additional filtering For category starting in "Microsoft",
            // only Warning or above will be sent to Application Insights.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Warning);
        })
        .Build();
}
```

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

        // The following will be picked up by Application Insights.
        _logger.LogInformation("From ConfigureServices. Services.AddMVC invoked"); 
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

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
            _logger.LogInformation("An example of a Information trace..");
            _logger.LogWarning("An example of a Warning trace..");
            _logger.LogTrace("An example of a Trace level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

Her iki örnek, tek başına paketin üstünde Microsoft.Extensions.Logging.ApplicationInsights kullanılır. Varsayılan olarak, bu yapılandırma en az kullanır `TelemetryConfiguration` Application Insights'a veri göndermek için. En az kullanılan kanalı olması anlamına gelir; `InMemoryChannel`, hiçbir örneklemeye ve standart TelemetryInitializers yok. Bu davranış için bir konsol uygulaması aşağıdaki örnekte gösterildiği gibi geçersiz kılınabilir.

Bu paketi yükleyin:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Aşağıdaki bölümde, varsayılan geçersiz kılmak gösterilir `TelemetryConfiguration`. Bu örnekte yapılandırır `ServerTelemetryChannel`, örnekleme ve özel bir `ITelemetryInitializer`.

```csharp
    // Create DI container.
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

    // Add the logging pipelines to use. We are adding ApplicationInsights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly call Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if application terminates, telemetry is sent to the back-end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

Yukarıdaki yaklaşım de bir ASP.NET Core uygulaması kullanılabilse normal uygulama izleme (istekler, bağımlılıklar vb.) ILogger yakalama ile aşağıda gösterildiği gibi birleştirmek için daha yaygın bir yaklaşım olacaktır.

Bu paketi yükleyin:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
```

Ekleyin `ConfigureServices` yöntemi. Bu kodun normal uygulama (ServerTelemetryChannel, LiveMetrics, istek/bağımlılıkları, bağıntı vb.) varsayılan yapılandırma ile izleme etkinleştirir

```csharp
services.AddApplicationInsightsTelemetry("ikeyhere");
```

Bu örnekte, yapılandırma tarafından kullanılan `ApplicationInsightsLoggerProvider` normal uygulama izleme tarafından kullanılan aynıdır. Bu nedenle `ILogger` izlemeleri ve diğer telemetri (istekler, bağımlılıklar vb.) çalıştırıyor aynı `TelemetryInitializers`, `TelemetryProcessors`, ve `TelemetryChannel`. Bunlar bağıntılı ve örneklenen değil, aynı şekilde örneklenir.

Ancak, bu davranışı bir özel durum yoktur. Varsayılan `TelemetryConfiguration` tam olarak ne zaman ayarlama bir günlük `Program.cs` veya `Startup.cs` kendisi, bu günlükleri varsayılan yapılandırmayı kalmaması. Ancak, her diğer günlük (örneğin, günlükleri denetleyicilerinden, modelleri vb.) yapılandırmayı paylaşan.

## <a name="control-logging-level"></a>Denetim Günlük düzeyi

Yukarıdaki örneklerde gösterildiği gibi kod günlüklerini filtreleme dışında da Application Insights'ı günlüğe kaydetme düzeyini yakalayan, değiştirerek denetlemek mümkündür `appsettings.json`. [Temelleri belgeleri günlüğü ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) bunu nasıl gösterir. Application Insights için özellikle, sağlayıcı diğer adı, `ApplicationInsights`gösterildiği yapılandıran örnek aşağıda `ApplicationInsights` yalnızca günlükleri yakalamak için `Warning` ve yukarıdaki tüm kategorilerden.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

*Application Insights'ta iki kez gösterilen bazı ILogger günlükleri görüyorum?*

* Dpm'nin daha eski (artık eski) sürümüne sahip, bu mümkündür `ApplicationInsightsLoggerProvider` çağırarak etkin `AddApplicationInsights` üzerinde `ILoggerFactory`. Kontrol edin, `Configure` yöntemi aşağıdaki sahiptir ve kaldırmak.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Visual Studio'dan hata ayıklama sırasında çift günlük karşılaşıyorsanız, ardından Applicationınsights şu şekilde ayarlayarak etkinleştirmek için kullanılan kod değiştirme `EnableDebugLogger` false olarak. Bu yalnızca uygulamada hata ayıklaması yaparken geçerlidir.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```



## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Temel ILogger günlüğe kaydetme](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
- [.NET izleme günlükleri](../../azure-monitor/app/asp-net-trace-logs.md)
