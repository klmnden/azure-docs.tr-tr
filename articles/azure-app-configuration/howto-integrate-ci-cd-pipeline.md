---
title: Azure Uygulama Yapılandırması'nı kullanarak sürekli tümleştirme ve teslim işlem hattı ile tümleştirin. | Microsoft Docs
description: Azure uygulama yapılandırmasında, sürekli tümleştirme ve teslim sırasında verileri kullanarak bir yapılandırma dosyası oluşturmayı öğrenin
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 7b2e919bc46810e8478956675ffeb1cb0542da85
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957377"
---
# <a name="integrate-with-a-cicd-pipeline"></a>Bir CI/CD işlem hattı ile tümleştirin

Uygulamanızı Azure uygulama yapılandırması ile erişmek boyutlandırılmamışsa uzak olasılığını karşı dayanıklılığı artırmak için geçerli yapılandırma verilerini, başlatma sırasında dağıtılan uygulamayla ve yerel olarak yüklenen bir dosyaya paketi . Bu yaklaşım, uygulamanızın varsayılan ayar değerleri en az olacağını garanti eder. Kullanılabilir olduğunda bu değerleri bir uygulama yapılandırma deposu yeni değişiklikler yazılır.

Kullanarak [ **dışarı** ](./howto-import-export-data.md#export-data) işlevi Azure uygulama yapılandırmasına, geçerli yapılandırma verilerini tek bir dosya olarak alma işlemini otomatik hale getirebilirsiniz. Bu dosya, sürekli tümleştirme ve sürekli dağıtım işlem hattı bir derleme veya dağıtım adımda sonra ekleyebilirsiniz.

Aşağıdaki örnek, uygulama yapılandırmalarını dahil edecek şekilde gösterilmektedir yapı olarak verileri adımı hızlı başlangıçlar, sunulan web uygulaması için. Tam [uygulama yapılandırması ile bir ASP.NET Core uygulaması oluşturma](./quickstart-aspnet-core-app.md) devam etmeden önce ilk.

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

## <a name="prerequisites"></a>Önkoşullar

Yerel olarak derleme yaparsanız, indirme ve yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) henüz yapmadıysanız.

Azure DevOps ile bulut derleme yapmak istiyorsanız, emin olun. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yapı sisteminizde yüklü.

## <a name="export-app-configuration-store"></a>Uygulama yapılandırma deposu dışarı aktarma

1. Açık, *.csproj* dosya ve aşağıdakileri ekleyin:

    ```xml
    <Target Name="Export file" AfterTargets="Build">
        <Message Text="Export the configurations to a temp file. " />
        <Exec WorkingDirectory="$(MSBuildProjectDirectory)" Condition="$(ConnectionString) != ''" Command="az appconfig kv export -f $(OutDir)\azureappconfig.json --format json --separator : --connection-string $(ConnectionString)" />
    </Target>
    ```

    *ConnectionString* ilişkili ile uygulama yapılandırmanız bir ortam değişkeni deposuna eklenmelidir.

2. Açık *Program.cs* ve güncelleştirme `CreateWebHostBuilder` yöntemi çağırarak dışarı aktarılan json dosyasını kullanacak biçimde `config.AddJsonFile()` yöntemi.

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

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString** ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanıyorsanız, aşağıdaki komutu yürütün ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu yürütün:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux'ı kullanıyorsanız, aşağıdaki komutu yürütün:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. .NET Core CLI kullanarak uygulamayı derlemek için komut kabuğunda aşağıdaki komutu yürütün:

        dotnet build

3. Derleme başarıyla tamamlandıktan sonra web uygulamasını yerel olarak çalıştırmak için aşağıdaki komutu yürütün:

        dotnet run

4. Bir tarayıcı penceresi açın ve gidin `http://localhost:5000`, yerel olarak barındırılan web uygulamasının varsayılan URL'si olduğu.

    ![Yerel hızlı uygulama başlatma](./media/quickstarts/aspnet-core-app-launch-local.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
