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
ms.openlocfilehash: b5e41b1f9ee982b8ff8c86232f715d5dab705cd6
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56962171"
---
# <a name="quickstart-create-a-net-framework-app-with-azure-app-configuration"></a>Hızlı Başlangıç: Oluşturma bir .NET Framework uygulaması ile Azure uygulama yapılandırması

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayıp kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmenize olanak tanır. Bu hızlı başlangıçta bir .NET Framework tabanlı Windows Masaüstü konsol uygulamanıza hizmet gösterilmektedir.

![Hızlı Başlangıç Yerel tamamlama](./media/quickstarts/dotnet-fx-app-run.png)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için yükleme [Visual Studio 2017](https://visualstudio.microsoft.com/vs) ve [.NET Framework 4.7.1](https://dotnet.microsoft.com/download) veya henüz yapmadıysanız sonraki bir sürümü.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-net-console-app"></a>Bir .NET konsol uygulaması oluşturma

1. Visual Studio'yu başlatın ve seçin **dosya** > **yeni** > **proje**.

2. İçinde **yeni proje** iletişim kutusunda **yüklü**, genişletme **Visual C#**   >  **Windows Masaüstü**, seçin **Konsol uygulaması (.NET Framework)**, tür a **adı** projeniz için seçtiğiniz **.NET Framework 4.7.1** veya yukarı ve **Tamam**.

## <a name="connect-to-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanın

1. Projenize sağ tıklayıp **NuGet paketlerini Yönet...** . İçinde **Gözat** sekmesinde, arayın ve NuGet paketlerini projenize ekleyin (denetleyin **ön sürümü dahil et** bunları bulamazsanız kutusunda).
    ```
    Microsoft.Configuration.ConfigurationBuilders.AzureAppConfiguration 1.0.0 preview or later
    Microsoft.Configuration.ConfigurationBuilders.Environment 2.0.0 preview or later
    ```

2. Güncelleştirme *App.config* dosya projenizin şu şekilde:

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

   Lütfen unutmayın, biz, uygulama yapılandırma deposu bağlantı dizesi ortam değişkeninden okuma şekilde `ConnectionString`, eklemek önemlidir `Environment` önce yapılandırma Oluşturucu `MyConfigStore` içinde `configBuilders` özelliği `appSettings` bölümü.

3. Açık *Program.cs* ve güncelleştirme `Main` yöntemi çağırarak uygulama yapılandırmasını kullanma `ConfigurationManager`.

    ```csharp
    static void Main(string[] args)
    {
        string message = ConfigurationManager.AppSettings["TestApp:Settings:Message"];

        Console.WriteLine(message);
    }
    ```

## <a name="build-and-run-the-app-locally"></a>Derleme ve uygulamayı yerel olarak çalıştırma

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString** bağlantı dizesini uygulama yapılandırma deponuz için. Windows Komut İstemi'ni kullanıyorsanız, aşağıdaki komutu yürütün:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu yürütün:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

2. Değişikliğin etkili olması ve ENTER tuşuna basın, Visual Studio'yu yeniden başlatmanız **Ctrl + F5 tuşlarına basarak** klavyenizde konsol uygulaması derleyebilir ve çalıştırabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve bir .NET Framework konsol uygulaması ile kullanılan. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
