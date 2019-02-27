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
ms.openlocfilehash: a6ed644334977f13107b12f955b1fbf015f731d1
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884889"
---
# <a name="quickstart-create-an-azure-function-with-app-configuration"></a>Hızlı Başlangıç: Uygulama yapılandırması ile Azure işlevi oluşturma

Azure uygulama yapılandırması, azure'da yönetilen yapılandırma hizmetidir. Kolayca depolayıp kodunuzdan tüm uygulama ayarlarınızı ayrılmış tek bir yerden yönetmenize olanak tanır. Bu hızlı başlangıçta bir Azure işlevine hizmeti eklemesine gösterilmektedir. 

Bu hızlı başlangıçtaki adımları tamamlamak için herhangi bir kod düzenleyicisini kullanabilirsiniz. Ancak, Windows, macOS ve Linux platformlarında sağlanan [Visual Studio Code](https://code.visualstudio.com/) mükemmel bir seçenektir.

![Hızlı Başlangıç Yerel tamamlama](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için yükleme [Visual Studio 2017](https://visualstudio.microsoft.com/vs) (olduğundan emin olun **Azure geliştirme** iş yükü yüklenmiş) ve [en son Azure işlevleri Araçları](../azure-functions/functions-develop-vs.md#check-your-tools-version).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-app-configuration-store"></a>Bir uygulama yapılandırma deposu oluşturma

[!INCLUDE [azure-app-configuration-create](../../includes/azure-app-configuration-create.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [Create a project using the Azure Functions template](../../includes/functions-vstools-create.md)]

## <a name="connect-to-app-configuration-store"></a>Uygulama yapılandırma deposuna bağlanın

1. Açık *Function1.cs* ve uygulama yapılandırma .NET Core yapılandırma sağlayıcısı bir başvuru ekleyin.

    ```csharp
    using Microsoft.Extensions.Configuration.AzureAppConfiguration;
    ```

2. Güncelleştirme `Run` yöntemi çağırarak uygulama yapılandırmasını kullanma `builder.AddAzureAppConfiguration()`.

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

1. Adlı bir ortam değişkenini ayarlamak **ConnectionString** ve uygulama yapılandırma deponuz için erişim anahtarı ayarlayın. Windows Komut İstemi'ni kullanıyorsanız, aşağıdaki komutu yürütün ve değişikliğin etkili olması için izin vermek için komut istemine yeniden başlatın:

        setx ConnectionString "Endpoint=<service_endpoint>;Id=<store_id>;Secret=<secret_key>="

    Windows PowerShell kullanıyorsanız, aşağıdaki komutu yürütün:

        $Env:ConnectionString = "Endpoint=<service_endpoint>;Id=<store_id>;Secret=<secret_key>="

    MacOS veya Linux'ı kullanıyorsanız, aşağıdaki komutu yürütün:

        export ConnectionString='Endpoint=<service_endpoint>;Id=<store_id>;Secret=<secret_key>='

2. İşlevinizi test etmek için basın **F5**. İstenirse, indirmek ve yüklemek için Visual Studio'dan isteği kabul et **Azure işlevleri temel (CLI)** araçları. Aracın HTTP isteklerini işleyebilmesi için bir güvenlik duvarı özel durumu etkinleştirmeniz de gerekebilir.

3. Azure İşlevleri çalışma zamanı çıktısından işlevinizin URL'sini kopyalayın.

    ![Hızlı Başlangıç VS'de hata ayıklama işlevi](./media/quickstarts/function-visual-studio-debugging.png)

4. HTTP isteğinin URL’sini tarayıcınızın adres çubuğuna yapıştırın. İşlevin döndürdüğü yerel GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

    ![Yerel işlev hızlı başlatma](./media/quickstarts/dotnet-core-function-launch-local.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [azure-app-configuration-cleanup](../../includes/azure-app-configuration-cleanup.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir uygulama yapılandırma deposu oluşturulur ve bir Azure işlevi ile kullanılan. Uygulama yapılandırmasını kullanma hakkında daha fazla bilgi edinmek için kimlik doğrulaması gösteren bir sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure kaynaklarını tümleştirme için yönetilen kimlik](./integrate-azure-managed-service-identity.md)
