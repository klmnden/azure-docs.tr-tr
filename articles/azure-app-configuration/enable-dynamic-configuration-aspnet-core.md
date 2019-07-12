---
title: ASP.NET Core uygulamanızı Azure uygulama yapılandırması dinamik yapılandırma kullanmaya yönelik öğretici | Microsoft Docs
description: Bu öğreticide, ASP.NET Core uygulamaları için yapılandırma verilerini dinamik olarak güncelleştirileceğini öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 78c64786f523aa424e8a9816e42db70e2a2997c2
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798462"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulaması dinamik yapılandırması kullan

ASP.NET Core, çeşitli kaynaklardan yapılandırma verilerini okuyan bir takılabilir yapılandırma sistemi vardır. Bu değişiklikler hareket halindeyken yeniden başlatmak bir uygulama neden olmadan işleyebilir. ASP.NET Core, bağlama türü kesin belirlenmiş .NET sınıfları yapılandırma ayarlarını destekler. Bunları kodunuza çeşitli kullanarak eklediği `IOptions<T>` desenleri. Özellikle bunlardan birini desenleri `IOptionsSnapshot<T>`otomatik olarak uygulamanın yapılandırması, temel alınan veriler değiştiğinde yeniden yükler. Ekleme `IOptionsSnapshot<T>` denetleyicilere uygulamanızda Azure uygulama yapılandırmasında depolanan en son yapılandırma erişin.

Uygulama yapılandırma ASP.NET Core istemci kitaplığı kullanılarak dinamik olarak bir ara yazılım yapılandırma ayarları kümesini yenilemek için de ayarlayabilirsiniz. Web uygulama isteklerini almak devam sürece, yapılandırma ayarlarını yapılandırma deposu ile güncelleştirilmesi devam edin.

Güncelleştirilmiş ayarları tutun ve yapılandırma deposu için çok fazla sayıda çağrı önlemek için her ayar için bir önbelleği kullanılır. Önbelleğe alınan bir ayarın değerini sona erinceye kadar bile yapılandırma deposunda değeri değiştiğinde yenileme işlemi değeri güncelleştirmez. Her istek için varsayılan süre 30 saniyedir ancak gerekirse kılınabilir.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmeleri nasıl uygulayacağınıza dair gösterir. Bu hızlı başlangıçlar, sunulan web uygulaması oluşturur. Devam etmeden önce [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) ilk.

Bu öğreticideki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını kullanılabilir mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir uygulama yapılandırma deposu değişikliklere yanıt yapılandırmasını güncelleştirmek için uygulamanızı ayarlayın.
> * Uygulamanızın denetleyicileri en son yapılandırmayı yerleştirir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi uygulamak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Uygulama yapılandırma verileri yeniden yükleyin

1. Açık *Program.cs*ve güncelleştirme `CreateWebHostBuilder` ekleme yöntemi `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();

                config.AddAzureAppConfiguration(options =>
                {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .ConfigureRefresh(refresh =>
                           {
                               refresh.Register("TestApp:Settings:BackgroundColor")
                                      .Register("TestApp:Settings:FontColor")
                                      .Register("TestApp:Settings:Message")
                           });
                }
            })
            .UseStartup<Startup>();
    ```

    `ConfigureRefresh` Yöntemi, bir yenileme işlemi tetiklendiğinde, uygulama yapılandırma deposu ile yapılandırma verileri güncelleştirmek için kullanılan ayarları belirtmek için kullanılır. Aslında bir yenileme işlemi tetiklemek için bir yenileme ara yazılımı uygulamayı herhangi bir değişiklik meydana geldiğinde yapılandırma verileri yenilemek için yapılandırılması gerekir.

2. Ekleme bir *Staticcontenthosting.Web* tanımlayan ve uygulayan yeni bir dosya `Settings` sınıfı.

    ```csharp
    namespace TestAppConfig
    {
        public class Settings
        {
            public string BackgroundColor { get; set; }
            public long FontSize { get; set; }
            public string FontColor { get; set; }
            public string Message { get; set; }
        }
    }
    ```

3. Açık *Startup.cs*ve güncelleştirme `ConfigureServices` yapılandırma verilerini bağlamak için yöntemi `Settings` sınıfı.

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.Configure<Settings>(Configuration.GetSection("TestApp:Settings"));

        services.Configure<CookiePolicyOptions>(options =>
        {
            options.CheckConsentNeeded = context => true;
            options.MinimumSameSitePolicy = SameSiteMode.None;
        });

        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
    }
    ```

4. Güncelleştirme `Configure` yapılandırma ayarlarını izin vermek için bir ara yazılım ekleme yöntemi kayıtlı isteklerini almak ASP.NET Core web uygulaması devam ederken güncelleştirilmesi için yenileme.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();
        app.UseMvc();
    }
    ```
    
    Ara yazılım belirtilen yenileme yapılandırması kullanan `AddAzureAppConfiguration` yönteminde `Program.cs` ASP.NET Core web uygulaması tarafından alınan her istek için yenileme tetiklemek için. Her istek için bir yenileme işlemi tetiklenir ve önbelleğe alınan değeri kayıtlı yapılandırma ayarları için süresi dolmuş istemci kitaplığı denetler. Süresi dolan önbelleğe alınan değerleri için ayarlar için değerleri uygulama yapılandırma deposu ile güncelleştirilir ve kalan değerler değişmeden kalır.
    
    > [!NOTE]
    > Bir yapılandırma ayarı için varsayılan önbellek sona erme süresini 30 saniye, ancak çağırarak geçersiz kılınabilir `SetCacheExpiration` seçenekleri Başlatıcı yöntemine geçirilen bağımsız değişken olarak `ConfigureRefresh` yöntemi.

## <a name="use-the-latest-configuration-data"></a>En son yapılandırma verilerini kullanma

1. Açık *HomeController.cs* denetleyicileri dizinindeki bir başvuru ekleyin `Microsoft.Extensions.Options` paket.

    ```csharp
    using Microsoft.Extensions.Options;
    ```

2. Güncelleştirme `HomeController` almak için sınıf `Settings` bağımlılık ekleme ve oluşturma kendi değerlerini kullanın.

    ```csharp
    public class HomeController : Controller
    {
        private readonly Settings _settings;
        public HomeController(IOptionsSnapshot<Settings> settings)
        {
            _settings = settings.Value;
        }

        public IActionResult Index()
        {
            ViewData["BackgroundColor"] = _settings.BackgroundColor;
            ViewData["FontSize"] = _settings.FontSize;
            ViewData["FontColor"] = _settings.FontColor;
            ViewData["Message"] = _settings.Message;

            return View();
        }
    }
    ```

3. Açık *Index.cshtml* görünümlerde > giriş dizini ve içeriğini aşağıdaki betikle değiştirin:

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @ViewData["BackgroundColor"]
        }
        h1 {
            color: @ViewData["FontColor"];
            font-size: @ViewData["FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@ViewData["Message"]</h1>
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. .NET Core CLI'yı kullanarak uygulamayı oluşturmak için komut kabuğu'nda aşağıdaki komutu çalıştırın:

        dotnet build

2. Yapılandırma başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

3. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **tüm kaynakları**, hızlı başlangıç bölümünde oluşturduğunuz uygulama yapılandırma deposu örneği seçin.

5. Seçin **yapılandırması Gezgini**ve aşağıdaki anahtarlarını güncelleştirin:

    | Anahtar | Value |
    |---|---|
    | TestAppSettings:BackgroundColor | green |
    | TestAppSettings:FontColor | LightGray |
    | TestAppSettings:Message | Azure uygulama yapılandırması - verilerden canlı güncelleştirmeler ile artık! |

6. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfayı yenileyin. Tarayıcı sayfa birden fazla yenileme değişikliklerin yansıtılması için gerekli olabilir.

    ![Hızlı Başlangıç uygulaması yenileme yerel](./media/quickstarts/aspnet-core-app-launch-local-after.png)
    
    > [!NOTE]
    > Yapılandırma ayarlarını varsayılan sona erme süresi ise 30 saniyedir önbelleğe alınmış olduğundan, önbellek süresi dolduğunda, uygulama yapılandırma deposu ayarlarında yapılan değişiklikler yalnızca de web uygulamasında yansıtılır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eklediğiniz Azure yönetilen hizmet kimliği erişim uygulama yapılandırmasını kolaylaştırmaya ve uygulamanız için kimlik bilgileri yönetimi iyileştirmek için. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için Azure CLI örnekleri için devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
