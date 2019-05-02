---
title: Azure işlevleri ile Azure uygulama yapılandırması için hızlı başlangıç | Microsoft Docs
description: Azure işlevleri ile Azure uygulama yapılandırması kullanmak için bir hızlı başlangıcı.
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.devlang: csharp
ms.topic: quickstart
ms.tgt_pltfrm: Azure Functions
ms.workload: tbd
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 914fe30e0b474f5d2c3ca14be6a6eb2e13cfdbf8
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64706442"
---
# <a name="quickstart-create-an-azure-function-with-app-configuration"></a>Hızlı Başlangıç: Uygulama yapılandırması ile Azure işlevi oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayın ve kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmek için kullanabilirsiniz. Bu hızlı başlangıçta hizmeti bir Azure işlev eklemesine gösterilmektedir. 

Bu hızlı başlangıçtaki adımları uygulamak için herhangi bir kod Düzenleyicisi'ni kullanabilirsiniz. [Visual Studio Code](https://code.visualstudio.com/) Windows, macOS ve Linux platformlarını mükemmel bir seçenek kullanılabilir.

![Hızlı tam yerel](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç yapmak için yükleme [Visual Studio 2017](https://visualstudio.microsoft.com/vs). Emin olun **Azure geliştirme** iş yükü de yüklenir. Ayrıca yükleme [en son Azure işlevleri Araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

6. Seçin **anahtar/değer Gezgini** > **+ Oluştur** aşağıdaki anahtar-değer çiftlerini eklemek için:

    | Anahtar | Değer |
    |---|---|
    | TestApp:Settings:Message | Azure uygulama yapılandırma verileri |

    Bırakın **etiket** ve **içerik türü** şimdilik boş.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-an-app-configuration-store"></a>Bir uygulama yapılandırma deposuna bağlanın

1. Projenize sağ tıklayıp seçin **NuGet paketlerini Yönet**. Üzerinde **Gözat** sekmesinde, arayın ve aşağıdaki NuGet paketlerini projenize ekleyin. Bunları bulamazsanız seçin **ön sürümü dahil et** onay kutusu.

    ```
    Microsoft.Extensions.Configuration.AzureAppConfiguration 1.0.0 preview or later
    ```

2. Açık *Function1.cs*, .NET Core uygulaması yapılandırma sağlayıcısı bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

3. Güncelleştirme `Run` yöntemi çağırarak uygulama yapılandırmasını kullanma `builder.AddAzureAppConfiguration()`.

    ```csharp
    public static async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req, ILogger log)
    {
        log.LogInformation("C# HTTP trigger function processed a request.");

        var builder = new ConfigurationBuilder();
        builder.AddAzureAppConfiguration(Environment.GetEnvironmentVariable("ConnectionString"));
        var config = builder.Build();
        string message = config["TestApp:Settings:Message"];
        message = message ?? req.Query["message"];

        string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
        dynamic data = JsonConvert.DeserializeObject(requestBody);
        message = message ?? data?.message;

        return message != null
            ? (ActionResult) new OkObjectResult(message)
            : new BadRequestObjectResult("Please pass a message from a configuration store, on the query string or in the request body");
    }
    ```

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString**ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanırsanız, aşağıdaki komutu çalıştırın ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "connection-string-of-your-app-configuration-store"

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu çalıştırın:

        $Env:ConnectionString = "connection-string-of-your-app-configuration-store"

    MacOS veya Linux kullanıyorsanız, aşağıdaki komutu çalıştırın:

        export ConnectionString='connection-string-of-your-app-configuration-store'

2. İşlevinizi test etmek için F5’e basın. İstenirse, indirmek ve yüklemek için Visual Studio'dan isteği kabul et **Azure işlevleri temel (CLI)** araçları. Araçlar HTTP isteklerini işleyebilmesi bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Hızlı Başlangıç VS'de hata ayıklama işlevi](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. Aşağıdaki resimde, işlevin döndürdüğü yerel GET isteğine tarayıcıda yanıt gösterilmektedir.

    ![Yerel işlev hızlı başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturuldu ve bir Azure işlevi ile kullanılır. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Yönetilen kimlik tümleştirme](./howto-integrate-azure-managed-service-identity.md)
