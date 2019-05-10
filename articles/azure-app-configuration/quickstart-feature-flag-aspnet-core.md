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
ms.openlocfilehash: 95f702b1d85dc8fe22b1800df3f7b0ebc987bee5
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65412388"
---
# <a name="quickstart-add-feature-flags-to-an-aspnet-core-app"></a>Hızlı Başlangıç: Özellik bayrakları için ASP.NET Core uygulaması Ekle

Özellik Yönetimi ASP.NET Core uygulamanızı Azure uygulama yapılandırması bağlama tarafından etkinleştirilebilir. Tüm özellik bayraklarını depolamak ve durumlarını merkezi olarak denetlemek için yönetilen bu hizmet kullanabilirsiniz. Bu hızlı başlangıçta hizmeti özellik yönetimi uçtan uca uygulaması oluşturmak için bir ASP.NET Core web uygulamanıza dahil etmek gösterilmektedir.

.NET Core özellik yönetim kitaplıklarını framework kapsamlı özellik bayrağı desteği ile genişletin. Bunlar, .NET Core yapılandırma sistemi üzerinde oluşturulur. Bunlar uygulama yapılandırması ile .NET Core yapılandırma sağlayıcısı sorunsuzca tümleştirin.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **özellik Yöneticisi** > **+ Oluştur** aşağıdaki özellik bayraklarını eklemek için:

    | Anahtar | Eyalet |
    |---|---|
    | Beta | Kapalı |

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Kullandığınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için. Visual Studio üzerinde .NET Core CLI kullanmanın avantajı, Windows, macOS ve Linux platformlar arasında kullanılabilir olmasıdır.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıçta adlandırın *TestFeatureFlags*.

2. Yeni klasörde yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet new mvc

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Ekleme [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) projenize. Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

- Açık *.csproj* dosya. Ekleme bir `UserSecretsId` burada gösterildiği gibi öğesi, genellikle bir GUID olan değerini, kendi ile değiştirin. Dosyayı kaydedin.

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

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanın

1. Başvuruları Ekle `Microsoft.Extensions.Configuration.AzureAppConfiguration` ve `Microsoft.FeatureManagement` aşağıdaki komutları çalıştırarak NuGet paketleri:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-008520001

        dotnet add package Microsoft.FeatureManagement.AspNetCore --version 1.0.0-preview-008560001-910

2. Projeniz için paketler geri yüklemek için aşağıdaki komutu çalıştırın:

        dotnet restore

3. Adlı bir gizli dizi eklemek *ConnectionStrings:AppConfig* gizli dizi Yöneticisi.

    Bu gizli dizi yapılandırma mağazaya erişmek için bağlantı dizesi içerir. Aşağıdaki komutta değeri, uygulama yapılandırma deponuz için bağlantı dizesiyle değiştirin.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

        dotnet user-secrets set ConnectionStrings:AppConfig <your_connection_string>

    Gizli dizi Yöneticisi, yalnızca web uygulamasını yerel olarak test etmek için kullanılır. Ne zaman uygulamanın dağıtıldığı [Azure App Service](https://azure.microsoft.com/services/app-service/web), örneğin, bir uygulama ayarı kullanmanızı **bağlantı dizeleri** App Service'te bağlantı dizesini depolamak için Yöneticisi ile gizli dizi yerine.

    Bu gizli dizi API configuration ile erişilir. İki nokta üst üste (:) Yapılandırma adı ' % s'yapılandırma API'si tüm desteklenen platformlarda ile çalışır. Bkz: [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Açık *Program.cs*, .NET Core uygulaması yapılandırma sağlayıcısı bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

5. Güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak uygulama yapılandırmasını kullanma `config.AddAzureAppConfiguration()` yöntemi.

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

6. Açık *Startup.cs*ve .NET Core özellik Yöneticisi başvuruları ekleyin.

    ```csharp
    using Microsoft.FeatureManagement.AspNetCore;
    ```

7. Güncelleştirme `ConfigureServices` özellik bayrağını destek çağırarak ekleme yöntemi `services.AddFeatureManagement()` yöntemi ve isteğe bağlı olarak çağırarak özellik bayrakları ile kullanılacak herhangi bir filtre içeren `services.AddFeatureFilter<FilterType>()`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddFeatureManagement();
    }
    ```

8. Ekleme bir *MyFeatureFlags.cs* dosya.

    ```csharp
    namespace TestFeatureFlags
    {
        public enum MyFeatureFlags
        {
            Beta
        }
    }
    ```

9. Ekleme *BetaController.cs* denetleyicileri dizini:

    ```csharp
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.FeatureManagement.AspNetCore;

    namespace TestFeatureFlags.Controllers
    {
        public class BetaController: Controller
        {
            private readonly IFeatureManager _featureManager;

            public BetaController(IFeatureManagerSnapshot featureManager)
            {
                _featureManager = featureManager;
            }

            [Feature(MyFeatureFlags.Beta)]
            public IActionResult Index()
            {
                return View();
            }
        }
    }
    ```

10. Açık *_viewımports.cshtml* görünümleri dizinde ve özellik Yöneticisi etiketi Yardımcısı ekleyin:

    ```html
    @addTagHelper *, Microsoft.FeatureManagement.AspNetCore
    ```

11. Açık *_Layout.cshtml* görünümlerde > paylaşılan dizin ve değiştirme için `<nav>` altında çubuk `<body>`  >  `<header>` aşağıdaki kod ile:

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

12. Görünümleri altında bir Beta dizin oluşturma ve ekleme *Index.cshtml* ona:

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

        dotnet build

2. Yapılandırma başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

3. Bir tarayıcı penceresi açın ve gidin `https://localhost:5001`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-feature-flag-local-before.png)

4. [Azure Portal](https://aka.ms/azconfig/portal) oturum açın. Seçin **tüm kaynakları**, hızlı başlangıç bölümünde oluşturduğunuz uygulama yapılandırma deposu örneği seçin.

5. Seçin **özellik Yöneticisi**, değerini değiştirip *Beta* için *üzerinde*:

    | Anahtar | Eyalet |
    |---|---|
    | Beta | Açık |

6. Yeni yapılandırma ayarlarını görmek için tarayıcı sayfayı yenileyin.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-feature-flag-local-after.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturulmuş ve ASP.NET Core web uygulaması özelliklerini yönetmek için kullanılan [özellik yönetim kitaplıkları](https://go.microsoft.com/fwlink/?linkid=2074664).

* Daha fazla bilgi edinin [yönetim özelliği](./concept-feature-management.md)
* [Özellik bayraklarını yönetme](./manage-feature-flags.md)
* [Özellik bayrakları ASP.NET Core uygulaması kullanın.](./use-feature-flags-dotnet-core.md)
