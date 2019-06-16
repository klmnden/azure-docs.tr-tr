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
ms.openlocfilehash: 91b61e88d876f481e74b8f2295b6fffced3f7902
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065565"
---
# <a name="create-your-first-durable-function-in-javascript"></a>JavaScript'te dayanıklı ilk işlevinizi oluşturma

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](../functions-overview.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir.

Bu makalede, yerel olarak oluşturma ve dayanıklı bir "Merhaba Dünya" işlevi test etmek için Visual Studio kod Azure işlevleri uzantısını kullanmayı öğrenin.  Bu işlev, düzenlemek ve diğer işlevlere yapılan çağrıları zincir. Ardından işlev kodunu Azure’da yayımlayacaksınız.

![Dayanıklı işlevi Azure'da çalışan](./media/quickstart-js-vscode/functions-vs-code-complete.png)

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için:

* Yükleme [Visual Studio Code'u](https://code.visualstudio.com/download).

* En son sürümüne sahip olduğunuzdan emin olun [Azure işlevleri çekirdek Araçları](../functions-run-local.md).

* Bir Windows bilgisayarda doğrulayın [Azure Storage öykünücüsü](../../storage/common/storage-use-emulator.md) yüklü ve çalışır. Bir Mac veya Linux bilgisayarda gerçek bir Azure depolama hesabı kullanmanız gerekir.

* Sürüm 8.0 veya sonraki bir sürüme sahip olduğunuzdan emin olun [Node.js](https://nodejs.org/) yüklü.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../../includes/functions-install-vs-code-extension.md)]

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-create-function-app-vs-code.md)]

## <a name="install-the-durable-functions-npm-package"></a>Dayanıklı işlevler npm paketini yükleme

1. Yükleme `durable-functions` çalıştırarak npm paketini `npm install durable-functions` işlev uygulamasının kök dizininde.

## <a name="creating-your-functions"></a>İşlevlerinizi oluşturma

Artık dayanıklı işlevlerini kullanmaya başlamak için üç işlev oluşturacağız: HTTP başlatıcısı, bir orchestrator ve bir etkinlik işlevi. Tüm çözümünüz HTTP Başlatıcısı başlatır ve orchestrator çeşitli etkinlik işlevleri işi gönderir.

### <a name="http-starter"></a>HTTP Başlatıcısı

İlk olarak dayanıklı işlevi düzenleme başlatan bir HTTP ile tetiklenen işlevi oluşturun.

1. Gelen *Azure: İşlevleri*, seçin **Create FUNCTION** simgesi.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizi içeren klasöre seçip **dayanıklı işlevler HTTP Başlatıcısı** işlev şablonu.

    ![HTTP Başlangıç şablonu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

3. Olarak varsayılan adı bırakın `DurableFunctionsHttpStart` basın *** Enter ** ardından **anonim** kimlik doğrulaması.

    ![Anonim kimlik doğrulamasını seçin](./media/quickstart-js-vscode/create-function-anonymous-auth.png)

Bir giriş noktası bizim dayanıklı işleve oluşturduk. Bir orchestrator ekleyelim.

### <a name="orchestrator"></a>Orchestrator

Artık, etkinlik işlevlerini koordine etmek için bir orchestrator oluşturacağız.

1. Gelen *Azure: İşlevleri*, seçin **Create FUNCTION** simgesi.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizi içeren klasöre seçip **dayanıklı işlevler Düzenleyicisi** işlev şablonu. Adı "DurableFunctionsOrchestrator" varsayılan olarak bırakın.

    ![Orchestrator şablonu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

Etkinlik işlevlerini koordine etmek için bir düzenleyici ekledik. Artık başvurulan etkinlik işlevi ekleyelim.

### <a name="activity"></a>Etkinlik

Aslında çözümün gerçekleştirmesi için bir etkinlik işlevi artık oluşturacağız.

1. Gelen *Azure: İşlevleri*, seçin **Create FUNCTION** simgesi.

    ![İşlev oluşturma](./media/quickstart-js-vscode/create-function.png)

2. İşlev uygulaması projenizi içeren klasöre seçip **dayanıklı işlevler etkinliği** işlev şablonu. Adı "Hello" varsayılan olarak bırakın.

    ![Etkinlik şablonu seçin](./media/quickstart-js-vscode/create-function-choose-template.png)

Artık bir düzenleme ve zincirini birlikte etkinlik işlevleri devre dışı bırakmak başlatmak için gereken tüm bileşenler ekledik.

## <a name="test-the-function-locally"></a>İşlevi yerel olarak test etme

Azure İşlevleri Temel Araçları, Azure İşlevleri projenizi yerel geliştirme bilgisayarınızda çalıştırmanıza olanak sağlar. Visual Studio Code'da ilk kez bir işlev başlattığınızda bu araçları yüklemeniz istenir.

1. Bir Windows bilgisayarda, Azure depolama öykünücüsü'nü başlatmak ve emin olun **AzureWebJobsStorage** özelliği *local.settings.json* ayarlanır `UseDevelopmentStorage=true`.

    Emin olmak için depolama öykünücüsü 5.8 **AzureWebJobsSecretStorageType** local.settings.json özelliği ayarlandığında `files`. Bir Mac veya Linux bilgisayarda ayarlamalısınız **AzureWebJobsStorage** özelliğini mevcut bir Azure depolama hesabı bağlantı dizesi. Bu makalenin sonraki bölümlerinde'de bir depolama hesabı oluşturun.

2. İşlevinizi test etmek için işlev kodunda bir kesme noktası ayarlayın ve işlev uygulaması projesini başlatmak için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir. Dayanıklı İşlevler, ilk kez kullanıyorsanız, dayanıklı işlevler uzantısını yüklenir ve yapı birkaç saniye sürebilir.

    > [!NOTE]
    > JavaScript dayanıklı işlevler sürümü gerektiren **1.7.0** veya büyük **Microsoft.Azure.WebJobs.Extensions.DurableTask** uzantısı. Dayanıklı işlevler uzantısını yüklemek için Azure işlevleri uygulamanızın kök klasöründen aşağıdaki komutu çalıştırın `func extensions install -p Microsoft.Azure.WebJobs.Extensions.DurableTask -v 1.7.0`

3. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](../media/functions-create-first-function-vs-code/functions-vscode-f5.png)

4. `{functionName}` yerine `DurableFunctionsOrchestrator` yazın.

5. Bir aracı gibi kullanarak [Postman](https://www.getpostman.com/) veya [cURL](https://curl.haxx.se/), URL uç noktasına bir HTTP POST isteği gönderin.

   Yanıt, dayanıklı düzenleme bize bildirdiğiniz HTTP işlevi ilk sonuç başarıyla başlatıldı. Bunu henüz düzenleme nihai sonucu değil. Yanıt birkaç faydalı URL'leri içeriyor. Şimdilik, şimdi orchestration durumunu sorgulayın.

6. URL değerini kopyalayın `statusQueryGetUri` tarayıcının adres çubuğuna yapıştırın ve isteği yürütün. Alternatif olarak da GET isteği için Postman'ı kullanmaya devam edebilirsiniz.

   İstek orchestration örneği durumu için sorgular. Bize örneği tamamlandı ve çıktılar veya dayanıklı işlevinin sonuçlarını içeren gösterir nihai bir yanıt almanız gerekir. Bunu şu şekilde görünür: 

    ```json
    {
        "instanceId": "d495cb0ac10d4e13b22729c37e335190",
        "runtimeStatus": "Completed",
        "input": null,
        "customStatus": null,
        "output": [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ],
        "createdTime": "2018-11-08T07:07:40Z",
        "lastUpdatedTime": "2018-11-08T07:07:52Z"
    }
    ```

7. Hata ayıklamayı durdurmak için basın **Shift + F5 tuşlarına basarak** VS code'da.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

[!INCLUDE [functions-create-function-app-vs-code](../../../includes/functions-sign-in-vs-code.md)]

[!INCLUDE [functions-publish-project-vscode](../../../includes/functions-publish-project-vscode.md)]

## <a name="test-your-function-in-azure"></a>Azure'da işlevinizi test etme

1. **Çıktı** panelinden HTTP tetikleyicisinin URL’sini kopyalayın. HTTP tarafından tetiklenen işlevinizi çağıran URL aşağıdaki biçimde olmalıdır:

        http://<functionappname>.azurewebsites.net/orchestrators/<functionname>

2. HTTP isteğinin yeni URL’sini tarayıcınızın adres çubuğuna yapıştırın. Önce aynı durum yanıt olarak yayımlanan uygulama kullanılırken almanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Oluşturma ve JavaScript dayanıklı bir işlev uygulaması yayımlamak için Visual Studio Code kullandığınızı.

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlevi desenler hakkında bilgi edinin](durable-functions-concepts.md)
