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
ms.openlocfilehash: 9cbdfe957587977b01bc46b46818856f789f46d8
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393626"
---
# <a name="tutorial-use-dynamic-configuration-in-an-aspnet-core-app"></a>Öğretici: ASP.NET Core uygulaması dinamik yapılandırması kullan

ASP.NET Core, çeşitli kaynaklardan yapılandırma verilerini okuyan bir takılabilir yapılandırma sistemi vardır. Bu değişiklikler hareket halindeyken yeniden başlatmak bir uygulama neden olmadan işleyebilir. ASP.NET Core, bağlama türü kesin belirlenmiş .NET sınıfları yapılandırma ayarlarını destekler. Bunları kodunuza çeşitli kullanarak eklediği `IOptions<T>` desenleri. Özellikle bunlardan birini desenleri `IOptionsSnapshot<T>`otomatik olarak uygulamanın yapılandırması, temel alınan veriler değiştiğinde yeniden yükler.

Ekleme `IOptionsSnapshot<T>` denetleyicilere uygulamanızda Azure uygulama yapılandırmasında depolanan en son yapılandırma erişin. Ayrıca, sürekli olarak izlemek ve bir uygulama yapılandırma deposundaki herhangi bir değişiklik almak için uygulama yapılandırma ASP.NET Core istemci kitaplığı da ayarlayabilirsiniz. Düzenli aralıklarla yoklama aralığını tanımlarsınız.

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

1. Açık *Program.cs*ve güncelleştirme `CreateWebHostBuilder` ekleyerek yöntemi `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options =>
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .Watch("TestApp:Settings:BackgroundColor")
                           .Watch("TestApp:Settings:FontColor")
                           .Watch("TestApp:Settings:Message"));
            })
            .UseStartup<Startup>();
    ```

    İkinci parametre `.Watch` yöntemidir yoklama aralığı için ASP.NET istemci kitaplığı bir uygulama yapılandırma deposu sorgular. İstemci Kitaplığı herhangi bir değişiklik oluşup olmadığını görmek için belirli yapılandırma ayarı denetler.
    
    > [!NOTE]
    > İçin varsayılan yoklama aralığını `Watch` belirtilmediyse genişletme yöntemi: 30 saniye.

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

    | Anahtar | Değer |
    |---|---|
    | TestAppSettings:BackgroundColor | green |
    | TestAppSettings:FontColor | LightGray |
    | TestAppSettings:Message | Azure uygulama yapılandırması - verilerden canlı güncelleştirmeler ile artık! |

6. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfayı yenileyin.

    ![Hızlı Başlangıç uygulaması yenileme yerel](./media/quickstarts/aspnet-core-app-launch-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, eklediğiniz Azure yönetilen hizmet kimliği erişim uygulama yapılandırmasını kolaylaştırmaya ve uygulamanız için kimlik bilgileri yönetimi iyileştirmek için. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için Azure CLI örnekleri için devam edin.

> [!div class="nextstepaction"]
> [CLI örnekleri](./cli-samples.md)
