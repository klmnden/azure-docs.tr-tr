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
ms.openlocfilehash: ce19041b29d567f061dde59fbe041adf61f889a0
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56961491"
---
# <a name="quickstart-create-an-aspnet-core-app-with-azure-app-configuration"></a>Hızlı Başlangıç: Azure uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayıp kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmenize olanak tanır. Bu hızlı başlangıçta hizmeti bir ASP.NET Core web uygulamanıza dahil etmek gösterilmektedir. ASP.NET Core bilinen bir veya daha fazla veri kaynaklarından alınan ayarları kullanarak bir tek anahtar-değer tabanlı yapılandırma nesnesi oluşturur *yapılandırma sağlayıcıları*, bir uygulama tarafından belirtilen. İstemci uygulama Yapılandırması'nın .NET Core, bu nedenle bir sağlayıcı uygulanan olduğundan, yalnızca başka bir veri kaynağı gibi görünür.

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-an-aspnet-core-web-app"></a>ASP.NET Core web uygulaması oluşturma

Kullanacağınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir ASP.NET Core MVC Web uygulaması projesi oluşturmak için. Visual Studio yerine .NET Core CLI kullanmanın avantajı Windows, macOS ve Linux platformlarında kullanılabilir olmasıdır.

1. Projeniz için yeni bir klasör oluşturun. Bu hızlı başlangıçta biz bunu adlandıracaktır *TestAppConfig*.

2. Yeni klasörde yeni bir ASP.NET Core MVC Web Uygulaması projesi oluşturmak için aşağıdaki komutu yürütün:

        dotnet new mvc

## <a name="add-secret-manager"></a>Gizli dizi Yöneticisi ekleme

Ekleyeceksiniz [gizli dizi Yöneticisi aracını](https://docs.microsoft.com/aspnet/core/security/app-secrets) projenize. Gizli Dizi Yöneticisi aracı, geliştirme işine yönelik hassas verileri proje ağacınızın dışında depolar. Bu yaklaşım, uygulama gizli dizilerini kaynak kodunun içinde yanlışlıkla paylaşmayı önlemeye yardımcı olur.

- *.csproj* dosyanızı açın. Ekleme bir `UserSecretsId` aşağıda gösterildiği gibi öğesi, genellikle bir GUID olan değerini, kendi ile değiştirin. Dosyayı kaydedin.

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

## <a name="connect-to-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanın

1. Aşağıdaki komutu yürüterek `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet paketine bir başvuru ekleyin:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration

2. Projeniz için paketleri geri yüklemek üzere aşağıdaki komutu yürütün.

        dotnet restore

3. Adlı bir gizli dizi eklemek *ConnectionStrings:AppConfig* gizli dizi Yöneticisi.

    Bu gizli dizi yapılandırma mağazaya erişmek için bağlantı dizesi içerir. Aşağıdaki komutta değeri, uygulama yapılandırma deponuz için bağlantı dizesiyle değiştirin.

    Bu komut, *.csproj* dosyası ile aynı dizinde yürütülmelidir.

        dotnet user-secrets set ConnectionStrings:AppConfig "Endpoint=<your_endpoint>;Id=<your_id>;Secret=<your_secret>"

    Gizli dizi Yöneticisi, yalnızca web uygulamasını yerel olarak test etmek için kullanılır. Uygulama, dağıtıldığı (örneğin, [Azure App Service](https://azure.microsoft.com/services/app-service/web)), bir uygulama ayarı kullanır (örneğin, **bağlantı dizeleri** App Service'te) yerine gizli dizi ile bağlantı dizesi Yöneticisi.

    Bu gizli diziye yapılandırma API'si ile erişilir. Desteklenen tüm platformlarda, yapılandırma API'lerinin yapılandırma adlarında iki nokta üst üste (:) işareti kullanılabilir [Ortama göre yapılandırma](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/index?tabs=basicconfiguration&view=aspnetcore-2.0#configuration-by-environment).

4. Açık *Program.cs* ve güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak uygulama yapılandırmasını kullanma `config.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var settings = config.Build();
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

5. Açık *Index.cshtml* içinde *görünümleri* > *giriş* dizini ve içeriğini aşağıdaki kodla değiştirin:

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

6. Açık *_Layout.cshtml* içinde *görünümleri* > *paylaşılan* dizini ve içeriğini aşağıdaki kodla değiştirin:

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

1. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğunda aşağıdaki komutu yürütün:

        dotnet build

2. Derleme başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu yürütün:

        dotnet run

3. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve ASP.NET Core web uygulaması ile kullanılan. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
