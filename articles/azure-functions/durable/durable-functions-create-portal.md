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
ms.date: 10/23/2018
ms.author: azfuncdf, glenga
ms.openlocfilehash: 3381939e296009b0fd58366f7fff410ea01d1206
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52864035"
---
# <a name="create-durable-functions-using-the-azure-portal"></a>Dayanıklı işlevler Azure portalını kullanarak oluşturma

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı Azure işlevleri için NuGet paketi sağlanan [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask). İşlev uygulamanızda bu uzantı yüklü olmalıdır. Bu makalede, Azure portalında dayanıklı işlevler geliştirilebilmesi için bu paketi yüklemek gösterilmektedir.

>[!NOTE]
>
>* Dayanıklı işlevler geliştiriyorsanız C#, bunun yerine dikkate almanız gereken [Visual Studio 2017 geliştirme](durable-functions-create-first-csharp.md).
* Dayanıklı işlevler javascript'teki geliştiriyorsanız bunun yerine dikkate almanız gereken **Visual Studio Code geliştirme**.
>
>Dayanıklı işlevler oluşturma JavaScript kullanarak henüz portalda desteklenmiyor. Visual Studio Code'u kullanın.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

Herhangi bir işlevin yürütülmesini barındıran bir işlev uygulamasına sahip olmalıdır. Bir işlev uygulaması, işlevlerinizin daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak grubu sağlar. Oluşturmalısınız bir C# işlev uygulaması, bu yana JavaScript şablonları için dayanıklı işlevler henüz desteklenmemektedir.  

[!INCLUDE [Create function app Azure portal](../../../includes/functions-create-function-app-portal.md)]

Varsayılan olarak, oluşturulan işlev uygulaması sürümünü kullanır. Azure işlevleri çalışma zamanı 2.x. Dayanıklı işlevler uzantısını her iki sürümlerinde çalışır 1.x ve 2.x'i Azure işlevleri çalışma zamanı. Ancak, şablonları yalnızca sürüm hedeflenirken kullanılabilir 2.x çalışma zamanı.

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
    curl -X POST https://{your-function-app-name}.azurewebsites.net/api/orchestrators/HelloSequence
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
> [Ortak dayanıklı işlevi desenler hakkında bilgi edinin](durable-functions-overview.md)
