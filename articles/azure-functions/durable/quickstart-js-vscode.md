---
title: JavaScript kullanarak Azure'da dayanıklı ilk işlevinizi oluşturma
description: Oluşturup Azure sürekli Visual Studio Code kullanarak işlevi yayımladıktan.
services: functions
documentationcenter: na
author: ColbyTresness
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: quickstart
ms.date: 11/07/2018
ms.author: azfuncdf, cotresne, glenga
ms.openlocfilehash: 114278b2d53cab8dd3302a8a2c2c074b8a5b24fa
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52638582"
---
# <a name="create-your-first-durable-function-in-javascript"></a>JavaScript'te dayanıklı ilk işlevinizi oluşturma

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](../functions-overview.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı durumu ve kontrol noktaları yeniden sizin yerinize yönetir.

Bu makalede, yerel olarak oluşturma ve dayanıklı bir "Merhaba Dünya" işlevi test etmek için Visual Studio kod Azure işlevleri uzantısını kullanmayı öğrenin.  Bu işlev, düzenlemek ve diğer işlevlere yapılan çağrıları zincir. Ardından işlev kodunu Azure’da yayımlayacaksınız.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* Yükleme [Visual Studio Code'u](https://code.visualstudio.com/download).

* [En son Azure İşlevleri araçlarınızın](../functions-develop-vs.md#check-your-tools-version) olduğundan emin olun.

* Bir Windows bilgisayarda doğrulayın [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) yüklü ve çalışır. Bir Mac veya Linux bilgisayarda gerçek bir Azure depolama hesabı kullanmanız gerekir.

* Sürüm 8.0 veya sonraki bir sürüme sahip olduğunuzdan emin olun [Node.js](https://nodejs.org/) yüklü.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-create-function-app-vs-code.md)]

## <a name="create-a-starter-function"></a>Başlatıcı bir işlev oluşturma

1. **Azure: İşlevler** seçeneğinden İşlev Oluştur simgesini seçin.

    ![İşlev oluşturma](../media/functions-create-first-function-vs-code/create-function.png)

1. İşlev uygulaması projenizin yer aldığı klasörü seçin ve **HTTP tetikleyicisi** işlev şablonunu seçin.

    ![HTTP tetikleyicisi şablonunu seçin](../media/functions-create-first-function-vs-code/create-function-choose-template.png)

1. İşlev adı için `HttpStart` yazın ve Enter tuşuna basın ve **Anonim** kimlik doğrulamasını seçin.

    ![Anonim kimlik doğrulamasını seçin](../media/functions-create-first-function-vs-code/create-function-anonymous-auth.png)

    HTTP ile tetiklenen işlevin şablonu kullanılarak, seçtiğiniz dilde bir işlev oluşturulur.

    ![Visual Studio Code’daki HTTP ile tetiklenen işlev şablonu](../media/functions-create-first-function-vs-code/new-function-full.png)

1. İle index.js değiştirin JavaScript aşağıda:

    ```javascript
    const df = require("durable-functions");
    
    module.exports = async function (context, req) {
        const client = df.getClient(context);
        const instanceId = await client.startNew(req.params.functionName, undefined, req.body);
    
        context.log(`Started orchestration with ID = '${instanceId}'.`);
    
        return client.createCheckStatusResponse(context.bindingData.req, instanceId);
    };
    ```

1. İle Function.JSON değiştirin JSON aşağıda:

    ```JSON
    {
      "bindings": [
        {
          "authLevel": "anonymous",
          "name": "req",
          "type": "httpTrigger",
          "direction": "in",
          "route": "orchestrators/{functionName}",
          "methods": ["post"]
        },
        {
          "name": "$return",
          "type": "http",
          "direction": "out"
        },
        {
          "name": "starter",
          "type": "orchestrationClient",
          "direction": "in"
        }
      ],
      "disabled": false
    }
    ```

Bir giriş noktası bizim dayanıklı işleve oluşturduk. Bir orchestrator ekleyelim.

## <a name="create-an-orchestrator-function"></a>Bir düzenleyici işlevi oluşturma

1. **Azure: İşlevler** seçeneğinden İşlev Oluştur simgesini seçin.

    ![İşlev oluşturma](../media/functions-create-first-function-vs-code/create-function.png)

1. İşlev uygulaması projenizin yer aldığı klasörü seçin ve **HTTP tetikleyicisi** işlev şablonunu seçin. Bu kod değiştiriliyor olduğundan, tetikleyici önemli değildir.

    ![HTTP tetikleyicisi şablonunu seçin](../media/functions-create-first-function-vs-code/create-function-choose-template.png)

1. İşlev adı için `OrchestratorFunction` yazın ve Enter tuşuna basın ve **Anonim** kimlik doğrulamasını seçin.

    ![Anonim kimlik doğrulamasını seçin](../media/functions-create-first-function-vs-code/create-function-anonymous-auth.png)

1. İle index.js değiştirin JavaScript aşağıda:

    [!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/index.js)]

1. İle Function.JSON değiştirin JSON aşağıda:

    [!code-json[Main](~/samples-durable-functions/samples/javascript/E1_HelloSequence/function.json)]

Etkinlik işlevlerini koordine etmek için bir düzenleyici ekledik. Artık başvurulan etkinlik işlevi ekleyelim.

## <a name="create-an-activity-function"></a>Bir etkinlik işlevi oluşturma

1. **Azure: İşlevler** seçeneğinden İşlev Oluştur simgesini seçin.

    ![İşlev oluşturma](../media/functions-create-first-function-vs-code/create-function.png)

1. İşlev uygulaması projenizin yer aldığı klasörü seçin ve **HTTP tetikleyicisi** işlev şablonunu seçin. Bu kod değiştiriliyor olduğundan, tetikleyici önemli değildir.

    ![HTTP tetikleyicisi şablonunu seçin](../media/functions-create-first-function-vs-code/create-function-choose-template.png)

1. İşlev adı için `SayHello` yazın ve Enter tuşuna basın ve **Anonim** kimlik doğrulamasını seçin.

    ![Anonim kimlik doğrulamasını seçin](../media/functions-create-first-function-vs-code/create-function-anonymous-auth.png)

1. İle index.js değiştirin JavaScript aşağıda:

    [!code-javascript[Main](~/samples-durable-functions/samples/javascript/E1_SayHello/index.js)]

1. İle Function.JSON değiştirin JSON aşağıda:

    [!code-json[Main](~/samples-durable-functions/samples/csx/E1_SayHello/function.json)]

Şimdi, düzenleme ve zinciri birlikte etkinlik işlevlerini devre dışı başlatmak için gerekli tüm bileşenler ekledik.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.  

1. Dayanıklı işlevler npm paket çalıştırarak yükleyin `npm install durable-functions` işlev uygulamasının kök dizininde.

1. Bir Windows bilgisayarda, Azure depolama öykünücüsü'nü başlatmak ve emin olun **AzureWebJobsStorage** local.settings.json özelliği ayarlandığında `UseDevelopmentStorage=true`. Bir Mac veya Linux bilgisayarda ayarlamalısınız **AzureWebJobsStorage** özelliğini mevcut bir Azure depolama hesabı bağlantı dizesi. Bu makalenin sonraki bölümlerinde'de bir depolama hesabı oluşturun.

1. İşlevinizi test etmek için işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir. Dayanıklı İşlevler, ilk kez kullanıyorsanız, dayanıklı işlevler uzantısını yüklenir ve yapı birkaç saniye sürebilir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](../media/functions-create-first-function-vs-code/functions-vscode-f5.png)

1. HTTP isteğinin URL'sini tarayıcınızın adres çubuğuna yapıştırın ve, orchestration durumunu görürsünüz.

1. Hata ayıklamayı durdurmak için Shift + F1 tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/api/<functionname>

1. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Önce aynı durum yanıt olarak yayımlanan uygulama kullanılırken almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve JavaScript dayanıklı bir işlev uygulaması yayımlamak için Visual Studio Code kullandığınızı.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlevi desenler hakkında bilgi edinin](durable-functions-overview.md)