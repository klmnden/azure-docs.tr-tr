---
title: Dayanıklı işlevler Azure portalını kullanarak oluşturma
description: Dayanıklı işlevler uzantısını portal geliştirme için Azure işlevleri için yüklemeyi öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 10/23/2018
ms.date: 03/25/2019
ms.author: v-junlch
ms.openlocfilehash: 1c60bd4dae6c279ccff637ff0aa798c48ebec6f1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60710957"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Dayanıklı işlevler Azure portalını kullanarak oluşturma

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı Azure işlevleri için NuGet paketi sağlanan [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). İşlev uygulamanızda bu uzantı yüklü olmalıdır. Bu makalede, Azure portalında dayanıklı işlevler geliştirilebilmesi için bu paketi yüklemek gösterilmektedir.

> [!NOTE]
> 
> * Dayanıklı işlevler geliştiriyorsanız C#, bunun yerine dikkate almanız gereken [Visual Studio 2017 geliştirme](durable-functions-create-first-csharp.md).
> * Dayanıklı işlevler javascript'teki geliştiriyorsanız bunun yerine dikkate almanız gereken [Visual Studio Code geliştirme](./quickstart-js-vscode.md).

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Herhangi bir işlevin yürütülmesini barındıran bir işlev uygulamasına sahip olmalıdır. Bir işlev uygulaması, işlevlerinizin daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak grubu sağlar. Bir .NET veya JavaScript uygulaması oluşturabilirsiniz.

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Varsayılan olarak, oluşturulan işlev uygulaması sürümünü kullanır. Azure işlevleri çalışma zamanı 2.x. Dayanıklı işlevler uzantısını her iki sürümlerinde çalışır 1.x ve 2.x'i Azure işlevleri çalışma zamanı içinde C#ve sürüm 2.x JavaScript içinde. Ancak, şablonları yalnızca sürüm hedeflenirken kullanılabilir 2.x çalışma zamanı seçtiğiniz dili ne olursa olsun.

## <a name="install-the-durable-functions-npm-package-javascript-only"></a>Dayanıklı işlevler npm paketini (yalnızca JavaScript) yükleme

Dayanıklı işlevler JavaScript oluşturuyorsanız yüklemeniz gerekir [ `durable-functions` npm paket](https://www.npmjs.com/package/durable-functions).

1. Ardından, işlev uygulamanızın adını seçin **Platform özellikleri**, ardından **Gelişmiş araçlar (Kudu)**.

   ![Kudu işlevleri platform özellikleri seçin](./media/durable-functions-create-portal/function-app-platform-features-choose-kudu.png)

2. Kudu konsolu içinde seçin **hata ayıklama konsoluna** ardından **CMD**.

   ![Kudu hata ayıklama konsoluna](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

3. İşlev uygulamanızın dosya dizin yapısının görüntülemelidir. `site/wwwroot` klasörüne gidin. Burada, yüklediğiniz bir `package.json` dosya dizini penceresine bırakarak dosya. Bir örnek `package.json` aşağıda verilmiştir:

    ```json
    {
      "dependencies": {
        "durable-functions": "^1.1.2"
      }
    }
    ```

   ![Kudu package.json karşıya yükleme](./media/durable-functions-create-portal/kudu-choose-debug-console.png)

4. Bir kez, `package.json` yüklenir, çalıştırma `npm install` Kudu uzak yürütme konsolundan komutu.

   ![Kudu npm yükleme çalıştırma](./media/durable-functions-create-portal/kudu-npm-install.png)

## <a name="create-an-orchestrator-function"></a>Bir düzenleyici işlevi oluşturma

1. İşlev uygulamanızı genişletin ve **İşlevler**'in yanındaki **+** düğmesine tıklayın. Bu, işlev uygulamanızdaki ilk işlevse **Portalda**'yı ve ardından **Devam**'ı seçin. Aksi takdirde üçüncü adıma geçin.

   ![Azure portalındaki İşlevler hızlı başlangıç sayfası](./media/durable-functions-create-portal/function-app-quickstart-choose-portal.png)

1. **Diğer şablonlar**'ı ve ardından **Sonlandır ve şablonları görüntüle**'yi seçin.

    ![İşlevler hızlı başlangıcı diğer şablonlar](./media/durable-functions-create-portal/add-first-function.png)

1. Arama alanına yazın `durable` seçip **dayanıklı işlevler HTTP Başlatıcısı** şablonu.

1. Sorulduğunda, **yükleme** Azure DurableTask uzantısı işlev uygulamasına tüm bağımlılıkları yüklemek üzere. Yalnızca uzantısı verin işlev uygulaması için bir kez yüklemeniz gerekir. Yükleme başarılı olduktan sonra **Devam**'ı seçin.

    ![Bağlama uzantılarını yükleme](./media/durable-functions-create-portal/install-durabletask-extension.png)

1. Yükleme tamamlandıktan sonra yeni işlev adı `HttpStart` ve **Oluştur**. Oluşturulan işleve, orchestration başlatmak için kullanılır.

1. Başka bir işlevi kullanarak şu işlev uygulamasında oluşturma **dayanıklı işlevler Düzenleyicisi** şablonu. Yeni düzenleme işlevinizi adlandırın `HelloSequence`.

1. Adlı üçüncü bir işlev oluşturma `Hello` kullanarak **dayanıklı işlevler etkinliği** şablonu.

## <a name="test-the-durable-function-orchestration"></a>Test dayanıklı işlevi düzenleme

1. Geri Git **HttpStart** işlev öğesini **<> / işlev URL'sini Al** ve **kopyalama** URL. Başlamak için bu URL'yi kullanın **HelloSequence** işlevi.

1. Kopyaladığınız URL'ye bir POST isteği göndermek için bir Postman veya cURL gibi HTTP aracını kullanın. Aşağıdaki örnek, bir POST isteği gönderir dayanıklı işleve bir cURL komutu:

    ```bash
    curl -X POST https://{your-function-app-name}.chinacloudsites.cn/api/orchestrators/HelloSequence
    ```

    Bu örnekte, `{your-function-app-name}` , işlev uygulamanızın adı olduğu etki alanıdır. Yanıt iletisi, aşağıdaki örnekteki gibi görünür yürütme yönetmek ve izlemek için kullanabileceğiniz bir URI uç noktaları kümesini içerir:

    ```json
    {  
       "id":"10585834a930427195479de25e0b952d",
       "statusQueryGetUri":"https://...",
       "sendEventPostUri":"https://...",
       "terminatePostUri":"https://...",
       "rewindPostUri":"https://..."
    }
    ```

1. Çağrı `statusQueryGetUri` uç noktası URI'si ve bakın şu örnekteki gibi görünebilir dayanıklı işlev geçerli durumu:

    ```json
        {
            "runtimeStatus": "Running",
            "input": null,
            "output": null,
            "createdTime": "2017-12-01T05:37:33Z",
            "lastUpdatedTime": "2017-12-01T05:37:36Z"
        }
    ```

1. Arama devam `statusQueryGetUri` durum olana kadar uç nokta **tamamlandı**, aşağıdaki örneğe benzer bir yanıt görürsünüz:

    ```json
    {
            "runtimeStatus": "Completed",
            "input": null,
            "output": [
                "Hello Tokyo!",
                "Hello Seattle!",
                "Hello London!"
            ],
            "createdTime": "2017-12-01T05:38:22Z",
            "lastUpdatedTime": "2017-12-01T05:38:28Z"
        }
    ```

Dayanıklı ilk işlevinizi Azure'da artık hazır ve çalışır olduğu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ortak dayanıklı işlevi desenler hakkında bilgi edinin](durable-functions-concepts.md)

<!-- Update_Description: wording update -->
