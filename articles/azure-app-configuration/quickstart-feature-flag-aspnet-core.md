---
title: ASP.NET Core için özellik bayraklarını ekleme hızlı başlangıcı | Microsoft Docs
description: Özellik bayrakları için ASP.NET Core uygulamaları ekleme ve bunları Azure uygulama yapılandırmasında yönetmek için bir hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 04/19/2019
ms.author: yegu
ms.openlocfilehash: 38b404ec10fb7b66b5e276665b0c9047d0576c15
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798397"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı Başlangıç: Özellik bayrakları için ASP.NET Core uygulaması Ekle

Özellik Yönetimi ASP.NET Core uygulamanızı Azure uygulama yapılandırması bağlayarak etkinleştirebilirsiniz. Tüm özellik bayraklarını depolamak ve durumlarını merkezi olarak denetlemek için yönetilen bu hizmet kullanabilirsiniz. Bu hızlı başlangıçta bir ASP.NET Core uygulama yapılandırma birleştirmek özellik yönetimi uçtan uca uygulaması oluşturmak için web uygulaması gösterilmektedir.

.NET Core özellik yönetim kitaplıklarını framework kapsamlı özellik bayrağı desteği ile genişletin. Bu kitaplıklar, .NET Core yapılandırma sistemi üzerinde oluşturulur. Bunlar uygulama yapılandırması ile .NET Core yapılandırma sağlayıcısı sorunsuzca tümleştirin.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **özellik Yöneticisi** >  **+ Oluştur** aşağıdaki özellik bayraklarını eklemek için:

    | Anahtar | Durum |
    |---|---|
    | Beta | Kapalı |

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Kullandığınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için. Visual Studio yerine .NET Core CLI kullanmanın avantajı .NET Core CLI'yı Windows, macOS ve Linux platformlar arasında kullanılabiliyor.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıçta adlandırın *TestFeatureFlags*.

1. Yeni klasörde yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

   ```    
   dotnet new mvc
   ```

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Ekleme [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) projenize. Gizli dizi Yöneticisi aracını, proje ağacı dışında geliştirme çalışması için hassas verileri depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

1. Açık *.csproj* dosya.
1. Ekleme bir `UserSecretsId` aşağıdaki örnekte gösterildiği gibi öğesi, genellikle bir GUID olan değerini, kendi ile değiştirin:

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>netcoreapp2.1</TargetFramework>
        <UserSecretsId>79a3edd0-2092-40a2-a04d-dcb46d5ca9ed</UserSecretsId>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.AspNetCore.App" />
        <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.1.2" PrivateAssets="All" />
    </ItemGroup>

    </Project>
    ```

1. Dosyayı kaydedin.

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanma

1. Başvuru ekleme `Microsoft.Azure.AppConfiguration.AspNetCore` aşağıdaki komutu çalıştırarak NuGet paketi:

    ```
    dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-009200001-7
    ```

1. Projeniz için paketler geri yüklemek için aşağıdaki komutu çalıştırın:

    ```
    dotnet restore
    ```

1. Adlı bir gizli dizi eklemek **ConnectionStrings:AppConfig** gizli dizi Yöneticisi.

    Bu gizli dizi, uygulama yapılandırma deposuna erişmek için bağlantı dizesini içerir. Değiştirin `<your_connection_string>` bağlantı dizesini uygulama yapılandırma deponuz için aşağıdaki komutla değeri.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

    ```
    dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>
    ```

    Yalnızca web uygulamasını yerel olarak test etmek için gizli dizi Yöneticisi'ni kullanın. Bir uygulamayı [Azure App Service](https://azure.microsoft.com/services/app-service), örneğin, adlı ayar uygulama kullanmanız **bağlantı dizeleri** bağlantı dizesini depolamak için gizli dizi Yöneticisi'ni kullanmak yerine, App Service'te.

    Uygulama yapılandırma API'si ile bu gizli dizi erişebilirsiniz. İki nokta üst üste (:) Yapılandırma adı tüm desteklenen platformlarda uygulama yapılandırma API'si ile çalışır. Bkz: [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration).

1. Açık *Program.cs*, .NET Core uygulaması yapılandırma sağlayıcısı bir başvuru ekleyin:

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

1. Güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak uygulama yapılandırmasını kullanma `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .UseFeatureFlags();
                });
            })
            .UseStartup<Startup>();
    ```

1. Açık *Startup.cs*ve .NET Core özellik Yöneticisi başvuruları ekleyin:

    ```csharp
    using Microsoft.FeatureManagement.AspNetCore;
    ```

1. Güncelleştirme `ConfigureServices` özellik bayrağını destek çağırarak ekleme yöntemi `services.AddFeatureManagement()` yöntemi. İsteğe bağlı olarak, özellik bayrakları ile çağırarak kullanılmak üzere herhangi bir filtre içerebilir `services.AddFeatureFilter<FilterType>()`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

1. Güncelleştirme `Configure` web uygulamasına ASP.NET Core sırasında yinelenen aralıklarla yenilenmesi için özellik bayrağı değerleri izin vermek için bir ara yazılım ekleme yöntemi devam isteklerini almak.

    ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseAzureAppConfiguration();
        app.UseMvc();
    }
    ```

1. Ekleme bir *MyFeatureFlags.cs* dosyası:

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

1. Ekleme *BetaController.cs* için *denetleyicileri* dizini:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement;
    using Microsoft.FeatureManagement.Mvc;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [FeatureGate(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

1. Açık *_viewımports.cshtml* içinde *görünümleri* dizini ve özellik Yöneticisi etiketi Yardımcısı ekleyin:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

1. Açık *_Layout.cshtml* içinde *görünümleri*\\*paylaşılan* dizin ve Değiştir `<nav>` barkod altında `<body>`  >  `<header>` aşağıdaki kod ile:

    ```html
    <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
        <div class="container">
            <a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">TestFeatureFlags</a>
            <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
            aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
            </button>
            <div class="navbar-collapse collapse d-sm-inline-flex flex-sm-row-reverse">
                <ul class="navbar-nav flex-grow-1">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                    </li>
                    <feature name="Beta">
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Beta" asp-action="Index">Beta</a>
                    </li>
                    </feature>
                    <li class="nav-item">
                        <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
    ```

1. Oluşturma bir *Beta* altında dizin *görünümleri* ve ekleme *Index.cshtml* ona:

    ```html
    @{
        ViewData["Title"] = "Beta Home Page";
    }

    <h1>
        This is the beta website.
    </h1>
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. .NET Core CLI'yı kullanarak uygulamayı oluşturmak için komut kabuğu'nda aşağıdaki komutu çalıştırın:

    ```
    dotnet build
    ```

1. Yapılandırma başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

    ```
    dotnet run
    ```

1. Bir tarayıcı penceresi açın ve gidin `https://localhost:5001`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

1. [Azure Portal](https://portal.azure.com) oturum açın. Seçin **tüm kaynakları**, hızlı başlangıç bölümünde oluşturduğunuz uygulama yapılandırma deposu örneği seçin.

1. Seçin **özellik Yöneticisi**ve durumunu değiştirme **Beta** anahtarını **üzerinde**:

    | Anahtar | Durum |
    |---|---|
    | Beta | Açık |

1. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfayı yenileyin.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturulmuş ve ASP.NET Core web uygulaması özelliklerini yönetmek için kullanılan [özellik yönetim kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664).

- Daha fazla bilgi edinin [özellik Yönetim](./concept-feature-management.md).
- [Özellik bayraklarını Yönet](./manage-feature-flags.md).
- [Özellik bayrakları ASP.NET Core uygulaması kullanmak](./use-feature-flags-dotnet-core.md).
