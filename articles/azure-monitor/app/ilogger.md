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
ms.openlocfilehash: 615eaa3df7cabad72ac321978eb01d93a7bfa988
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608294"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>.NET Core ILogger günlükleri ApplicationInsightsLoggerProvider

ASP.NET Core günlük yerleşik ve üçüncü taraf sağlayıcılar farklı türde çalışır bir günlüğe kaydetme API'si destekler. Günlüğe kaydetme, Log() veya bir değişken üzerinde çağrılarak gerçekleştirilir `ILogger` örnekleri. Bu makalede nasıl kullanılacağını gösterir `ApplicationInsightsLoggerProvider` yakalamak için `ILogger` hem Konsol hem de ASP.NET Core uygulamaları günlüğe kaydeder. Bu makalede ayrıca nasıl `ApplicationInsightsLoggerProvider` diğer Application Insights telemetri ile tümleşiktir.
Asp.Net Core daha fazla günlük bilgi edinmek için [bu makalede](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>ASP.NET Core uygulamaları

İle başlayarak [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) veya sonraki sürümleri, sürüm 2.7.0-beta3 `ApplicationInsightsLoggerProvider` normal Application Insights kullanarak standart yöntemlerin - göre izleme etkinleştirilirken varsayılan olarak etkindir Çağırma `UseApplicationInsights` IWebHostBuilder genişletme yöntemini veya `AddApplicationInsightsTelemetry` IServiceCollection genişletme yöntemini. `ILogger` Günlükleri yakalanan tarafından `ApplicationInsightsLoggerProvider` herhangi diğer telemetri toplanan aynı yapılandırmaya tabidir. yani kümesinin aynısına sahip oldukları `TelemetryInitializer`s, `TelemetryProcessor`s, kullandığı aynı `TelemetryChannel`ve bağıntılı ve diğer her telemetri aynı şekilde örneklenir.  Bu sürüm SDK'sının veya üzeri olan sonra herhangi bir eylemi yakalamak için gerekli `ILogger` günlükleri.

Varsayılan olarak yalnızca `ILogger` , günlükleri `Warning` veya yukarıda (tüm kategorilerden), Application Insights'a gönderilir. Bu davranış, gösterildiği gibi filtreler uygulayarak değiştirilebilir [burada](#control-logging-level). Ayrıca ek adımlar, gerekli `ILogger` oturumu `Program.cs` veya `Startup.cs` gösterildiği yakalanıp üzeresiniz [burada](#capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications).

Microsoft.ApplicationInsights.AspNet SDK'ın önceki bir sürümünü kullanıyorsanız veya yalnızca ApplicationInsightsLoggerProvider, kullanmak istediğiniz tüm diğer Application Insights izleme olmadan, aşağıdaki adımları izleyin.

1. nuget paketini yükleyin.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
    </ItemGroup>
```

2 değiştirme `Program.cs` olarak aşağıda

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
                // Providing an instrumentation key here is required if you are using
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

Yukarıdaki kod yapılandıracak `ApplicationInsightsLoggerProvider`. Aşağıdaki gösteren bir örnek kullanan denetleyici sınıfı `ILogger` göre Applicationınsights yakalanan günlükleri göndermek için.

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

### <a name="capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications"></a>Startup.cs, Program.cs içinde Asp.Net Core uygulamaları ILogger günlüklerinden yakalama

Yeni ApplicationInsightsLoggerProvider ile günlüklerinden uygulama başlangıç ardışık düzende erken yakalamak mümkündür. Hatta ancak ApplicationInsightsLoggerProvider olduğundan otomatik olarak Application Insights (ve sonraki sürümlerde 2.7.0-beta3) etkin, onu yok izleme anahtarı kurulumu daha sonra işlem hattındaki kadar bu nedenle yalnızca günlükleri denetleyicisinden / diğer sınıflar yakalanan. İle başlayarak her günlükleri tutmak için `Program.cs` ve `Startup.cs` kendisini ihtiyacım ApplicationInsightsLoggerProvider bir izleme anahtarı ile açıkça etkinleştirmek. Dikkat etmeniz önemlidir `TelemetryConfiguration` tam olarak ne zaman ayarlama bir günlük `Program.cs` veya `Startup.cs` kendisi, bu günlükleri InMemoryChannel, hiçbir örneklemeye ve hiçbir standart telemetri kullanan tam bir en düşük yapılandırma kullanmasını sağlayın başlatıcılar veya işlemci.

Aşağıdaki örnekler gösterilmektedir `Program.cs` ve `Startup.cs` bu özelliği kullanarak.

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
            // providing an instrumentation key here is required if you are using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Program.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Program", LogLevel.Trace);
            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Startup.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Startup", LogLevel.Trace);
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

## <a name="migrating-from-old-applicationinsightsloggerprovider"></a>Eski ApplicationInsightsLoggerProvider geçiş

2.7.0-beta2 önce Microsoft.ApplicationInsights.AspNet SDK sürümleri artık kullanımdan kalkmıştır bir oturum açma sağlayıcısı desteklenmiyor. Bu sağlayıcı ile etkinleştirildi `AddApplicationInsights()` genişletme yönteminin `ILoggerFactory`. Bu sağlayıcı artık kullanılmıyor ve kullanıcıların yeni sağlayıcıyı geçirmek için önerilir. Geçiş iki adımdan oluşur.

1. ' Dan ILoggerFactory.AddApplicationInsights() çağrısını kaldırın `Startup.Configure()` çift günlük kaydını önlemek için yöntemi.
2. Bunlar yeni sağlayıcı tarafından da dikkate değil olarak kodda herhangi bir filtreleme kurallarını yeniden uygulayın. En düşük LogLevel veya filtre işlevleri ILoggerFactory.AddApplicationInsights() aşırı sürdü. Yeni sağlayıcı ile filtreleme günlüğe kaydetme çerçevesi kendisini parçası olan ve Application Insights sağlayıcısı tarafından yapılmadı. Bu nedenle herhangi bir filtre aracılığıyla sağlanan `ILoggerFactory.AddApplicationInsights()` aşırı kaldırılmalıdır ve filtreleme kuralları sağlanmalıdır aşağıdaki [bu](#control-logging-level) yönergeleri. Kullanırsanız `appsettings.json` günlüğünü filtrelemek için her ikisi de aynı sağlayıcı diğer - kullandıkça yeni sağlayıcı ile çalışmayı sürdürecektir **Applicationınsights**.

Eski sağlayıcısı halen kullanılabilse (artık kullanımdan kalkmıştır ve yalnızca ana sürüm değişiklik 3.xx kaldırılacak), yeni sağlayıcısına geçiş aşağıdaki sebeplerden dolayı özellikle önerilir.

1. Önceki sağlayıcı desteği koduk [kapsamları](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). Yeni sağlayıcısında kapsam özellikleri özel özellikler toplanan telemetriyi otomatik olarak eklenir.
2. Günlükleri artık çok daha önce uygulama başlangıç ardışık düzeninizde yakalanabilir. yani Program ve başlangıç sınıflardan günlükleri artık yakalanabilir.
3. Yeni sağlayıcı ile filtre framework düzeyinde kendisini gerçekleştirilir. Günlüklerini filtreleme Application Insights sağlayıcısına konsol, hata ayıklama gibi yerleşik sağlayıcılar dahil olmak üzere, diğer sağlayıcılar için tam aynı şekilde yapılabilir ve benzeri. Birden çok sağlayıcı için aynı filtre uygulamak da mümkündür.
4. [Önerilen](https://github.com/aspnet/Announcements/issues/255) günlük sağlayıcıları etkinleştirmek için ASP.NET Core (2.0 veya sonraki sürümleri) de yoludur ILoggingBuilder genişletme yöntemlerini kullanarak `Program.cs` kendisi.

> [!Note]
> Yeni sağlayıcıyı hedefleyen uygulamalar için kullanılabilir `NETSTANDARD2.0` veya üzeri. .NET Core 1.1 gibi eski .NET Core sürümleri, uygulamanızın hedeflediği veya .NET Framework hedeflemesi gerekiyorsa, eski sağlayıcısını kullanmaya devam edin.

## <a name="console-application"></a>Konsol uygulaması

Aşağıdaki kod göndermek üzere yapılandırılmış bir konsol uygulaması gösterir `ILogger` Application ınsights izlemelerini.

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

Yukarıdaki örnekte, tek başına paketin `Microsoft.Extensions.Logging.ApplicationInsights` kullanılır. Varsayılan olarak, bu yapılandırma en az kullanır `TelemetryConfiguration` Application Insights'a veri göndermek için. En az kullanılan kanalı olması anlamına gelir; `InMemoryChannel`, hiçbir örneklemeye ve standart TelemetryInitializers yok. Bu davranış için bir konsol uygulaması aşağıdaki örnekte gösterildiği gibi geçersiz kılınabilir.

Bu paketi yükleyin:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

Aşağıdaki bölümde, varsayılan geçersiz kılmak gösterilir `TelemetryConfiguration` kullanarak `services.Configure<TelemetryConfiguration>()` yöntemi. Bu örnekte ayarlar `ServerTelemetryChannel`, örnekleme ve özel bir ekler `ITelemetryInitializer` için `TelemetryConfiguration`.

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

    // Add the logging pipelines to use. We are adding Application Insights only.
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

## <a name="control-logging-level"></a>Denetim Günlük düzeyi

Asp.Net Core `ILogger` altyapısı uygulamak için yerleşik bir mekanizma vardır [filtreleme](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) Application Insights sağlayıcısı dahil olmak üzere her kayıtlı sağlayıcılardan gönderilen günlükleri kontrol olanağı tanıyan günlüklerinin. Bu filtreleme ya da yapılandırma yapılabilir (genellikle kullanarak `appsettings.json` dosyası) veya kod. Bu özellik, framework tarafından sağlanan ve Application Insights sağlayıcıya özgü değildir.

Filtre kuralları için ApplicationInsightsLoggerProvider uygulama örnekleri aşağıda verilmiştir.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Appsettings.json ile yapılandırma içinde filtre kuralları oluşturma

Sağlayıcı diğer ApplicationInsightsLoggerProvider için olan `ApplicationInsights`. Aşağıda gösterilen bölümde `appsettings.json` günlükleri yapılandırır `Warning` ve yukarıdaki tüm kategorilerden `Error` ve yukarıdaki kategorilerden gönderilmesini "Microsoft" ile başlayan `ApplicationInsightsLoggerProvider`.

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

Aşağıdaki kod parçacığı günlükleri yapılandırır `Warning` ve yukarıdaki tüm kategorilerden `Error` ve yukarıdaki kategorilerden gönderilmesini 'Microsoft' ile başlayan `ApplicationInsightsLoggerProvider`. Bu yapılandırma gerçekleştirilen Yukarıdaki yapılandırma aynıdır `appsettings.json`.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Sık Sorulan Sorular

*1. Eski ve yeni ApplicationInsightsLoggerProvider nedir?*

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sevk Iloggerfactory kullanarak etkin bir yerleşik ApplicationInsightsLoggerProvider ile (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider) genişletme yöntemleri. Bu sağlayıcı ve sonraki sürümlerde 2.7.0-beta2 kullanılmıyor biçiminde işaretlendiğinden ve sonraki sürüm değişiklik tamamen kaldırılacaktır. [Bu](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) kendisi değil eski paketi ve bağımlılıkları vb. isteklerden izlemeyi etkinleştirmek için gereklidir.

* Yeni tek başına paketin önerilen alternatiftir [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), içeren geliştirilmiş bir ApplicationInsightsLoggerProvider ( Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) ve bunu etkinleştirmek için uzantı yöntemleri ILoggerBuilder.

* [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) sürüm 2.7.0-beta3 ve sonraki sürümlerde bir bağımlılık yukarıdaki paketi sürer ve sağlayan `ILogger` otomatik olarak yakalayın.

*2. Bazı görüyorum `ILogger` günlükleri, Application Insights'ta iki kez gösterilir?*

* (Artık kullanılmayan) eski sürümü varsa, bu çoğaltma mümkündür `ApplicationInsightsLoggerProvider` çağırarak etkin `AddApplicationInsights` üzerinde `ILoggerFactory`. Kontrol edin, `Configure` yöntemi aşağıdaki sahiptir ve kaldırmak.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Visual Studio'dan hata ayıklama sırasında çift günlük karşılaşıyorsanız, ardından ayarlayarak gibi Application ınsights'ı etkinleştirmek için kullanılan kod değiştirme `EnableDebugLogger` false olarak. Bu çoğaltma sorunu düzeltmenin olup yalnızca ilgili uygulamada hata ayıklaması yaparken.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```

*3. Ben için güncelleştirilmiş [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) ve sürüm 2.7.0-beta3 ve şimdi görmenizi, günlükleri `ILogger` otomatik olarak yakalanır. Nasıl bu özelliği tamamen kapatabilir miyim?*

* Bkz: [bu](../../azure-monitor/app/ilogger.md#control-logging-level) bölümü nasıl filtreleme yapılacağını öğrenmek için genel olarak günlüğe kaydeder. Kullanmak için denetlediğinde ApplicationInsightsLoggerProvider `LogLevel.None` de.

  Kodda

    ```csharp
        builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                          ("", LogLevel.None);
    ```

  Yapılandırması

    ```json
    {
      "Logging": {
        "ApplicationInsights": {
          "LogLevel": {
            "Default": "None"
          }
    }
    ```

*4. Bazı görüyorum `ILogger` günlükleri, başkalarının aynı özelliklere sahip değil?*

* Application Insights yakalamalarına ve gönderir `ILogger` aynı kullanarak günlükleri `TelemetryConfiguration` için her bir telemetri kullanılır. Bu kural için bir özel durum yoktur. Varsayılan `TelemetryConfiguration` tam olarak ne zaman ayarlama bir günlük `Program.cs` veya `Startup.cs` kendisi, günlükleri şu konumlardan varsayılan yapılandırması olmaz ve bu nedenle tüm çalışmıyor olacaktır `TelemetryInitializer`s ve `TelemetryProcessor`s.

*5. Tek başına paketin Microsoft.Extensions.Logging.ApplicationInsights kullanıyorum ve bazı ek özel telemetri el ile oturum istiyorum. Bu ne yapmalıyım?*

* Tek başına paketin kullanırken `TelemetryClient` kullanıcıların yeni bir örneğini oluşturmak için beklenen şekilde DI kapsayıcıya eklenen değil `TelemetryClient` aşağıda gösterildiği gibi Günlükçü sağlayıcısı tarafından kullanılan aynı yapılandırmayı kullanarak. Bu, tüm özel telemetri, aynı zamanda ILogger yakalanan için kullanılan aynı yapılandırmayı sağlar.

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
> Application Insights'ı etkinleştirmek için paketini Microsoft.ApplicationInsights.AspNetCore kullandıysanız, ardından Yukarıdaki örnek almak için değiştirilmesi gereken, lütfen unutmayın `TelemetryClient` doğrudan oluşturucuda. Bkz: [bu](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core-no-visualstudio#frequently-asked-questions) tam bir örnek için.


*6. Application Insights telemetri türüne öğesinden üretilen `ILogger` günlükleri? veya nerede görebilirim `ILogger` uygulama anlayışları'nda oturum?*

* ApplicationInsightsLoggerProvider yakalar `ILogger` oluşturur ve günlükleri `TraceTelemetry` almaktır. Bir özel durum nesnesi Log() yönteme ILogger üzerinde ardından yerine iletilmezse `TraceTelemetry`e `ExceptionTelemetry` oluşturulur. Bu telemetri öğelerinin diğer olarak aynı yerde bulunabilir `TraceTelemetry` veya `ExceptionTelemetry` Application ınsights, portal, analytics veya Visual Studio yerel hata ayıklayıcı gibi.
Her zaman Gönder tercih ederseniz `TraceTelemetry`, daha sonra kod parçacığını kullanın ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```.

*7. SDK yüklü olmayan ve my Asp.Net Core uygulamaları için Application Insights'ı etkinleştirmek için Azure Web uygulaması uzantısı kullanabilirim. Yeni sağlayıcıyı nasıl kullanabilirim?*

* Application Insights uzantısını Azure Web uygulamasında eski sağlayıcısı kullanır. Filtreleme kuralları değiştirilebilir `appsettings.json` uygulamanız için. Yeni sağlayıcıyı yararlanmak isterseniz, üzerinde SDK'sı nuget bağımlılık yararlanarak derleme Araçları'nı kullanın. Yeni sağlayıcıyı kullanmak için uzantı geçiş yaptığında bu belge güncelleştirilecektir.

*8. I Microsoft.Extensions.Logging.ApplicationInsights tek başına paketin kullanarak ve Application Insights sağlayıcısı çağıran oluşturucusu tarafından etkinleştirme. AddApplicationInsights("ikey"). Yapılandırmasından izleme anahtarını almak için bir seçenek var mı?*


* Değiştirme `Program.cs` ve `appsettings.json` aşağıda gösterildiği gibi.

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

İlgili bölümünden `appsettings.json`

```json
{
  "myikeyfromconfig": "putrealikeyhere"
}
```

Yukarıdaki kod, yalnızca tek başına oturum açma sağlayıcısı kullanılırken gereklidir. Normal Application Insights izleme için izleme anahtarını otomatik olarak yapılandırma yolundan yüklenir `ApplicationInsights:Instrumentationkey` ve `appsettings.json` aşağıdaki gibi görünmelidir.

```json
{
  "ApplicationInsights":
    {
        "Instrumentationkey":"putrealikeyhere"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler hakkında daha fazla bilgi edinin:

* [Asp.Net core'da günlüğe kaydetme](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [Application Insights'ta .NET izleme günlükleri](../../azure-monitor/app/asp-net-trace-logs.md)
