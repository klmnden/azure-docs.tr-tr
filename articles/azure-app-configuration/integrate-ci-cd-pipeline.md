---
title: Azure uygulama yapılandırması kullanarak sürekli tümleştirme ve teslim işlem hattı ile tümleştirmek için öğretici | Microsoft Docs
description: Bu öğreticide, şunların nasıl sürekli tümleştirme ve teslim sırasında Azure uygulama yapılandırmasında verileri kullanarak bir yapılandırma dosyası oluşturmak için
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: tutorial
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: a92c00148d4b612a4360e3843d66503d528928cd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59700160"
---
# <a name="integrate-with-a-cicd-pipeline"></a>CI/CD işlem hattıyla tümleştirme

Uygulamanızı Azure uygulama yapılandırması ile erişmek boyutlandırılmamışsa uzak olasılığını karşı dayanıklılığı artırabilir. Bunu yapmak için başlatma sırasında yerel olarak dağıtılan uygulamayla ve yüklenen bir dosyaya geçerli yapılandırma verilerini paketi. Bu yaklaşım, uygulamanızın en az varsayılan ayarı değerlerine sahip olacağını garanti eder. Kullanılabilir olduğunda bu değerler daha yeni bir uygulama yapılandırma deposu değişiklikler tarafından üzerine yazılır.

Kullanarak [dışarı](./howto-import-export-data.md#export-data) işlevi Azure uygulama yapılandırmasına, geçerli yapılandırma verilerini tek bir dosya olarak alma işlemini otomatik hale getirebilirsiniz. Ardından bu dosya, sürekli tümleştirme ve sürekli dağıtım (CI/CD) işlem hattı bir derleme veya dağıtım adımı ekleyin.

Aşağıdaki örnek, uygulama yapılandırmalarını dahil edecek şekilde gösterilmektedir yapı olarak verileri adımı hızlı başlangıçlar, sunulan web uygulaması için. Devam etmeden önce [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) ilk.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Yerel olarak derleme yaparsanız, indirme ve yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) henüz yapmadıysanız.

Bulut derleme yapmak için Azure ile DevOps gibi emin [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yapı sisteminizde yüklü.

## <a name="export-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu dışarı aktarma

1. Açık, *.csproj* dosyasını bulun ve aşağıdaki betiği ekleyin:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    Ekleme *ConnectionString* uygulama yapılandırma deponuz olarak bir ortam değişkeni ile ilişkili.

2. Program.cs dosyasını açın ve güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak ve dışarı aktarılan JSON dosyasında kullanılacak `config.AddJsonFile()` yöntemi.

    ```csharp
    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .ConfigureAppConfiguration((hostingContext, config) =>
            {
                var directory = System.IO.Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location);
                var settings = config.Build();

                config.AddJsonFile(Path.Combine(directory, "azureappconfig.json"));
                config.AddAzureAppConfiguration(settings["ConnectionStrings:AppConfig"]);
            })
            .UseStartup<Startup>();
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString**ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. .NET Core CLI'yı kullanarak uygulamayı oluşturmak için komut kabuğu'nda aşağıdaki komutu çalıştırın:

        dotnet build

3. Yapılandırma başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

4. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarını tümleştirme için yönetilen kimlik](./howto-integrate-azure-managed-service-identity.md)
