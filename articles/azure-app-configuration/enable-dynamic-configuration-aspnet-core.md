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
ms.devlang: na
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 44ae922b182874eef378d4868fb278c3c76252db
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884793"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulaması dinamik yapılandırması kullan

ASP.NET Core, çeşitli kaynaklardan yapılandırma verilerini okurken olarak yeniden başlatmak bir uygulama neden olmadan hareket halindeyken değişiklikleri işleme yeteneğine sahip bir takılabilir yapılandırma sistemi vardır. Kesin türü belirtilmiş .NET sınıfları yapılandırma ayarlarını, bağlama ve bunları kullanarak çeşitli kodunuza ekleme destekler `IOptions<T>` desenleri. Özellikle bunlardan birini desenleri `IOptionsSnapshot<T>`, temel alınan verileri değiştirdiğinizde uygulama yapılandırmasını otomatik yeniden yüklemeyi sağlar. Ekleme `IOptionsSnapshot<T>` denetleyicilere uygulamanızda Azure uygulama yapılandırmasında depolanan en son yapılandırma erişin. Ayrıca, sürekli olarak izlemek ve tanımladığınız düzenli aralıklarla yoklama aracılığıyla bir uygulama yapılandırma deposundaki herhangi bir değişiklik almak için uygulama yapılandırma ASP.NET Core istemci kitaplığı ayarlayabilirsiniz.

Bu öğretici, kodunuzda dinamik yapılandırma güncelleştirmeleri nasıl uygulayacağınıza dair gösterir. Bu hızlı başlangıçlar, sunulan web uygulaması oluşturur. Tam [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) devam etmeden önce ilk.

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Bir uygulama yapılandırma deposu değişikliklere yanıt yapılandırmasını güncelleştirmek için uygulamanızı ayarlayın
> * En son yapılandırmayı, uygulamanızın denetleyicileri ekleme

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="reload-data-from-app-configuration"></a>Uygulama yapılandırma verileri yeniden yükleyin

1. Açık *Program.cs* ve güncelleştirme `CreateWebHostBuilder` ekleyerek yöntemi `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(o => o.Connect(settings["ConnectionStrings:AppConfig"])
                    .Watch("TestApp:Settings:BackgroundColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:FontColor", TimeSpan.FromSeconds(1))
                    .Watch("TestApp:Settings:Message", TimeSpan.FromSeconds(1)));
            })
            .UseStartup<Startup>();
    ```

    İkinci parametre `.Watch` yöntemidir yoklama aralığı için ASP.NET istemci kitaplığı oluştu, belirli bir yapılandırma ayarı için herhangi bir değişiklik görmek için bir uygulama yapılandırma deposu sorgular.

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

3. Açık *Startup.cs* ve güncelleştirme `ConfigureServices` yapılandırma verilerini bağlamak için yöntemi `Settings` sınıfı.

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

## <a name="use-the-latest-configuration-data"></a>En son yapılandırma verilerini kullanma

1. Açık *HomeController.cs* içinde *denetleyicileri* dizini, güncelleştirme `HomeController` almak için sınıf `Settings` yapma ve bağımlılık ekleme ile kendi değerlerini kullanın.

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

2. Açık *Index.cshtml* içinde *görünümleri* > *giriş* dizini ve içeriğini aşağıdakilerle değiştirin:

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

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğunda aşağıdaki komutu yürütün:

        dotnet build

2. Derleme başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu yürütün:

        dotnet run

3. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local-before.png)

4. Oturum [Azure portalında](https://aka.ms/azconfig/portal), tıklayın **tüm kaynakları** ve hızlı başlangıçta oluşturduğunuz uygulama yapılandırma deposu örneği.

5. Tıklayın **anahtar/değer Gezgini** ve aşağıdaki anahtarlarını güncelleştirin:

    | Anahtar | Değer |
    |---|---|
    | TestAppSettings:BackgroundColor | Mavi |
    | TestAppSettings:FontColor | LightGray |
    | TestAppSettings:Message | Azure uygulama yapılandırması - verilerden canlı güncelleştirmeler ile artık! |

6. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfayı yenileyin.

    ![Hızlı Başlangıç uygulaması yenileme yerel](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eklediğiniz Azure yönetilen hizmet kimliği erişim uygulama yapılandırmasını kolaylaştırmaya ve uygulamanız için kimlik bilgileri yönetimi iyileştirmek için. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için Azure CLI örnekleri için devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
