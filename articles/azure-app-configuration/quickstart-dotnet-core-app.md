---
title: .NET Core ile Azure uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırması ile .NET Core uygulamaları kullanmak için bir hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET Core
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: cd4115aaeec15d14d48dcb71cbdc75212c6dc2db
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960691"
---
# <a name="quickstart-create-an-net-core-app-with-app-configuration"></a>Hızlı Başlangıç: Oluşturma bir .NET Core uygulaması ile uygulama yapılandırması

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayıp kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmenize olanak tanır. Bu hızlı başlangıçta bir .NET Core konsol uygulamanıza hizmet gösterilmektedir.

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-core-console-app"></a>.NET Core konsol uygulaması oluşturma

Kullanacağınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir .NET Core konsol uygulaması projesi oluşturmak için. Visual Studio yerine .NET Core CLI kullanmanın avantajı Windows, macOS ve Linux platformlarında kullanılabilir olmasıdır.

1. Projeniz için yeni bir klasör oluşturun.

2. Yeni klasörde yeni bir ASP.NET Core MVC Web Uygulaması projesi oluşturmak için aşağıdaki komutu yürütün:

        dotnet new console

## <a name="connect-to-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanın

1. Aşağıdaki komutu yürüterek `Microsoft.Extensions.Configuration.AzureAppConfiguration` NuGet paketine bir başvuru ekleyin:

        dotnet add package Microsoft.Extensions.Configuration.AzureAppConfiguration

2. Projeniz için paketleri geri yüklemek üzere aşağıdaki komutu yürütün.

        dotnet restore

3. Açık *Program.cs* ve güncelleştirme `Main` yöntemi çağırarak uygulama yapılandırmasını kullanma `builder.AddAzureAppConfiguration()` yöntemi.

    ```csharp
    static void Main(string[] args)
    {
        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));

        var config = builder.Build();
        Console.WriteLine(config["TestApp:Settings:Message"] ?? "Hello world!");
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString** ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanıyorsanız, aşağıdaki komutu yürütün ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu yürütün:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux'ı kullanıyorsanız, aşağıdaki komutu yürütün:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Konsol uygulaması oluşturmak için aşağıdaki komutu yürütün:

        dotnet build

3. Yapılandırma başarıyla tamamlandıktan sonra uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu yürütün:

        dotnet run

    ![Hızlı Başlangıç uygulama çalıştırma](./media/quickstarts/dotnet-core-app-run.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve bir .NET Core konsol uygulaması ile kullanılan. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
