---
title: ASP.NET Core ile Azure uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırması ile ASP.NET Core uygulamaları kullanmak için bir hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: ASP.NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: a721cc2252619923496ee5a3a8ae590a5cda3b04
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58487558"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Hızlı Başlangıç: Azure uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta hizmeti bir ASP.NET Core web uygulamanıza dahil etmek gösterilmektedir. 

ASP.NET Core, bir uygulama tarafından belirtilen bir veya daha fazla veri kaynaklarından alınan ayarları kullanarak, bir anahtar-değer tabanlı tek bir yapılandırma nesnesi oluşturur. Bu veri kaynakları olarak bilinen *yapılandırma sağlayıcıları*. İstemci uygulama Yapılandırması'nın .NET Core, bu nedenle bir sağlayıcı uygulanan olduğundan hizmet başka bir veri kaynağı gibi görünür.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Kullandığınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için. Visual Studio üzerinde .NET Core CLI kullanmanın avantajı, Windows, macOS ve Linux platformlar arasında kullanılabilir olmasıdır.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıçta adlandırın *TestAppConfig*.

2. Yeni klasörde yeni bir ASP.NET Core MVC web uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet new mvc

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Ekleme [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) projenize. Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

- *.csproj* dosyanızı açın. Ekleme bir `UserSecretsId` burada gösterildiği gibi öğesi, genellikle bir GUID olan değerini, kendi ile değiştirin. Dosyayı kaydedin.

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

1. Bir başvuru ekleyin `Microsoft.Extensions.Configuration.AzureAppConfiguration` aşağıdaki komutu çalıştırarak NuGet paketi:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration --version 1.0.0-preview-007830001

2. Projeniz için paketler geri yüklemek için aşağıdaki komutu çalıştırın:

        dotnet restore

3. Adlı bir gizli dizi eklemek *ConnectionStrings:AppConfig* gizli dizi Yöneticisi.

    Bu gizli dizi yapılandırma mağazaya erişmek için bağlantı dizesi içerir. Aşağıdaki komutta değeri, uygulama yapılandırma deponuz için bağlantı dizesiyle değiştirin.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

        dotnet user-secrets set ConnectionStrings:AppConfig "Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>"

    Gizli dizi Yöneticisi, yalnızca web uygulamasını yerel olarak test etmek için kullanılır. Ne zaman uygulama dağıtılır, örneğin, çok [Azure App Service](https://azure.microsoft.com/services/app-service/web), bir uygulama ayarı, örneğin, kullandığınız **bağlantı dizeleri** App Service'te. Gizli dizi Yöneticisi'yle bağlantı dizesini depolamak yerine, bu ayarı kullanın.

    Bu gizli dizi API configuration ile erişilir. İki nokta üst üste (:) Yapılandırma adı ' % s'yapılandırma API'si tüm desteklenen platformlarda ile çalışır. Bkz: [ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0).

4. Program.cs dosyasını açın ve güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak uygulama yapılandırmasını kullanma `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;

    ...

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(options => {
                    options.Connect(settings["ConnectionStrings:AppConfig"])
                           .SetOfflineCache(new OfflineFileCache());
                });
            })
            .UseStartup<Startup>();
    ```

5. Görünümlerde Index.cshtml açın > giriş dizini ve içeriğini aşağıdaki kodla değiştirin:

    ```html
    @using Microsoft.Extensions.Configuration
    @inject IConfiguration Configuration

    <!DOCTYPE html>
    <html lang="en">
    <style>
        body {
            background-color: @Configuration["TestApp:Settings:BackgroundColor"]
        }
        h1 {
            color: @Configuration["TestApp:Settings:FontColor"];
            font-size: @Configuration["TestApp:Settings:FontSize"];
        }
    </style>
    <head>
        <title>Index View</title>
    </head>
    <body>
        <h1>@Configuration["TestApp:Settings:Message"]</h1>
    </body>
    </html>
    ```

6. Görünümlerde _Layout.cshtml açın > Dizin paylaşılan ve içeriğini aşağıdaki kodla değiştirin:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <title>@ViewData["Title"] - hello_world</title>

        <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
        <link rel="stylesheet" href="~/css/site.css" />
    </head>
    <body>
        <div class="container body-content">
            @RenderBody()
        </div>

        <script src="~/lib/jquery/dist/jquery.js"></script>
        <script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
        <script src="~/js/site.js" asp-append-version="true"></script>

        @RenderSection("Scripts", required: false)
    </body>
    </html>
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. .NET Core CLI'yı kullanarak uygulamayı oluşturmak için komut kabuğu'nda aşağıdaki komutu çalıştırın:

        dotnet build

2. Yapılandırma başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

3. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve ASP.NET Core web uygulaması ile birlikte kullanılan [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664). Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
