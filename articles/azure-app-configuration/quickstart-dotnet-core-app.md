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
ms.openlocfilehash: c42066b3ac02264ee357bac7ed3fdd360e9d5d4a
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67798407"
---
# <a name="quickstart-create-a-net-core-app-with-app-configuration"></a>Hızlı Başlangıç: Oluşturma bir .NET Core uygulaması ile uygulama yapılandırması

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta bir .NET Core konsol uygulamanıza hizmet gösterilmektedir.

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

![Hızlı Başlangıç uygulama çalıştırma](./media/quickstarts/dotnet-core-app-run.png)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [.NET Core SDK'sı](https://dotnet.microsoft.com/download).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **yapılandırması Gezgini** >  **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Value |
    |---|---|
    | TestApp:Settings:Message | Azure uygulama yapılandırma verileri |

    Bırakın **etiket** ve **içerik türü** şimdilik boş.

## <a name="create-a-net-core-console-app"></a>.NET Core konsol uygulaması oluşturma

Kullandığınız [.NET Core komut satırı arabirimi (CLI)](https://docs.microsoft.com/dotnet/core/tools/) yeni bir .NET Core konsol uygulaması projesi oluşturmak için. Visual Studio üzerinde .NET Core CLI kullanmanın avantajı, Windows, macOS ve Linux platformlar arasında kullanılabilir olmasıdır.

1. Projeniz için yeni bir klasör oluşturun.

2. Yeni klasörde yeni bir ASP.NET Core konsol uygulaması projesi oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet new console

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanın

1. Bir başvuru ekleyin `Microsoft.Azure.AppConfiguration.AspNetCore` aşağıdaki komutu çalıştırarak NuGet paketi:

        dotnet add package Microsoft.Azure.AppConfiguration.AspNetCore --version 2.0.0-preview-009200001-7

2. Projeniz için paketler geri yüklemek için aşağıdaki komutu çalıştırın:

        dotnet restore

3. Açık *Program.cs*, .NET Core uygulaması yapılandırma sağlayıcısı bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

4. Güncelleştirme `Main` yöntemi çağırarak uygulama yapılandırmasını kullanma `builder.AddAzureAppConfiguration()` yöntemi.

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

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString**ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. Konsol uygulaması oluşturmak için aşağıdaki komutu çalıştırın:

        dotnet build

3. Yapılandırma başarıyla tamamlandıktan sonra uygulamayı yerel olarak çalıştırmak için aşağıdaki komutu çalıştırın:

        dotnet run

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve bir .NET Core konsol uygulaması ile birlikte kullanılan [uygulama yapılandırma sağlayıcısı](https://go.microsoft.com/fwlink/?linkid=2074664). Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirme](./howto-integrate-azure-managed-service-identity.md)
