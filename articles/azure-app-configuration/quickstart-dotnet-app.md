---
title: .NET Framework ile Azure uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Azure uygulama yapılandırması .NET Framework uygulamaları ile kullanmak için bir hızlı başlangıç
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: .NET
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 33deea0805ffa89bcc6a64f34a97a4e080690da9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60203671"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Hızlı Başlangıç: Oluşturma bir .NET Framework uygulaması ile Azure uygulama yapılandırması

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta bir .NET Framework tabanlı Windows Masaüstü konsol uygulamanıza hizmet gösterilmektedir.

![Hızlı tam yerel](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [Visual Studio 2017](https://visualstudio.microsoft.com/vs) ve [.NET Framework 4.7.1](https://dotnet.microsoft.com/download) veya henüz yapmadıysanız sonraki bir sürümü.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **anahtar/değer Gezgini** > **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Settings:Message | Azure uygulama yapılandırma verileri |

    Bırakın **etiket** ve **içerik türü** şimdilik boş.

## <a name="create-a-net-console-app"></a>Bir .NET konsol uygulaması oluşturma

1. Visual Studio'yu başlatın ve **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje**seçin **yüklü** > **Visual C#**   >  **Windows Masaüstü**. Seçin **konsol uygulaması (.NET Framework)**, projeniz için bir ad girin. Seçin **.NET Framework 4.7.1** veya yukarı ve select **Tamam**.

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanın

1. Projenize sağ tıklayıp seçin **NuGet paketlerini Yönet**. Üzerinde **Gözat** sekmesinde, arayın ve aşağıdaki NuGet paketlerini projenize ekleyin. Bunları bulamazsanız seçin **ön sürümü dahil et** onay kutusu.

    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Güncelleştirme *App.config* projenizin aşağıdaki gibi dosya:

    ```xml
    <configSections>
        <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
    </configSections>

    <configBuilders>
        <builders>
            <add name="MyConfigStore" mode="Greedy" connectionString="${ConnectionString}" type="Microsoft.Configuration.ConfigurationBuilders.AzureAppConfigurationBuilder, Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration" />
            <add name="Environment" mode="Greedy" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
        </builders>
    </configBuilders>

    <appSettings configBuilders="Environment,MyConfigStore">
        <add key="AppName" value="Console App Demo" />
        <add key="ConnectionString" value ="Set via an environment variable - for example, dev, test, staging, or production connection string." />
    </appSettings>
    ```

   Ortam değişkenindeki bağlantı dizesini uygulama yapılandırma deposunun okunur `ConnectionString`. Ekleme `Environment` önce yapılandırma Oluşturucu `MyConfigStore` içinde `configBuilders` özelliği `appSettings` bölümü.

3. Açık *Program.cs*ve güncelleştirme `Main` yöntemi çağırarak uygulama yapılandırmasını kullanma `ConfigurationManager`.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString** bağlantı dizesini uygulama yapılandırma deponuz için. Windows Komut İstemi'ni kullanırsanız, aşağıdaki komutu çalıştırın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Değişikliğin etkili olması Visual Studio'yu yeniden başlatın. Konsol uygulaması derleyebilir ve için Ctrl + F5 tuşlarına basın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve bir .NET Framework konsol uygulaması kullanılır. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirme](./howto-integrate-azure-managed-service-identity.md)
