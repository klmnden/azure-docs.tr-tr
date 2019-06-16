---
title: Azure işlevleri ile ilk PowerShell işlevinizi oluşturma
description: Visual Studio Code kullanarak Azure'da ilk PowerShell işlevinizi oluşturmayı öğrenin.
services: functions
keywords: ''
author: joeyaiello
manager: jeconnoc
ms.author: jaiello, glenga
ms.date: 04/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.devlang: powershell
ms.openlocfilehash: 1fc541f1236d17e1c2ffbf64ddb0340dcf5c0b9a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063471"
---
# <a name="create-your-first-powershell-function-in-azure-preview"></a>(Önizleme) ile Azure PowerShell'i ilk işlevinizi oluşturma

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

Bu hızlı başlangıç makalesi ilk kez oluşturma hakkında bilgi vermektedir [sunucusuz](https://azure.com/serverless) Visual Studio Code kullanarak PowerShell işlevi.

![Visual Studio kod projesinde Azure işlevleri kodu](./media/functions-create-first-function-powershell/powershell-project-first-function.png)

Kullandığınız [Visual Studio Code için Azure işlevleri uzantısı] yerel olarak bir PowerShell işlevi oluşturmak için ve azure'da yeni bir işlev uygulaması dağıttınız. Uzantı şu an önizleme aşamasındadır. Daha fazla bilgi edinmek için [Visual Studio Code için Azure İşlevleri uzantısı] sayfasına bakın.

> [!NOTE]  
> PowerShell desteği [Azure işlevleri uzantısı][Visual Studio Code için Azure işlevleri uzantısı] şu anda varsayılan olarak devre dışıdır. PowerShell desteğini etkinleştirme, bu makaledeki adımlarda biridir.

Aşağıdaki adımlar, macOS, Windows ve Linux tabanlı işletim sistemlerinde desteklenir.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için:

* Yükleme [PowerShell Core](/powershell/scripting/install/installing-powershell-core-on-windows)

* [Desteklenen platformlardan](https://code.visualstudio.com/docs/supporting/requirements#_platforms) birinde [Visual Studio Code](https://code.visualstudio.com/)’u yükleyin. 

* Yükleme [Visual Studio Code için PowerShell uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell).

* Yükleme [.NET Core SDK'sını 2.2 +](https://www.microsoft.com/net/download) (Azure işlevleri çekirdek araçları gerektirdiği ve tüm desteklenen platformlarda kullanılabilir).

* Sürümünü yüklemek 2.x [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2).

* Ayrıca bir etkin Azure aboneliği gerekir.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)] 

## <a name="create-a-function-app-project"></a>İşlev uygulaması projesi oluşturma

Visual Studio Code'daki Azure İşlevleri proje şablonu, Azure'daki bir işlev uygulamasında yayımlanabilen bir proje oluşturur. İşlev uygulaması, kaynakların yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. 

1. Visual Studio Code'da Azure logosu görüntülemek için seçin **Azure: İşlevleri** alan ve yeni proje Oluştur simgesini seçin.

    ![İşlev uygulaması projesi oluşturma](./media/functions-create-first-function-powershell/create-function-app-project.png)

1. İşlevler projesi çalışma alanınız için bir konum seçin ve **seçin**.

    > [!NOTE]
    > Bu makale, bir çalışma alanının dışında tamamlanacak şekilde tasarlanmıştır. Bu örnekte, bir çalışma alanının parçası olan bir proje klasörünü seçmeyin.

1. Seçin **Powershell (Önizleme)** işlev uygulaması projenizi dili olarak ve ardından **Azure işlevler v2**.

1. Seçin **HTTP tetikleyicisi** ilk işlevinizi şablonu olarak kullanmak `HTTPTrigger` işlevi olarak adlandırın ve bir yetkilendirme düzeyini seçin **işlevi**.

    > [!NOTE]
    > **İşlevi** yetki düzeyi gerekmektedir bir [işlev anahtarı](functions-bindings-http-webhook.md#authorization-keys) işlev uç noktası Azure'da çağırırken değeri. Bu, yalnızca herkesin işlevinizi çağırmak zorlaştırır.

1. İstendiğinde **Çalışma alanına ekle**’yi seçin.

Visual Studio Code yeni bir çalışma alanında PowerShell işlev uygulaması projesi oluşturur. Bu projeyi içeren [host.json](functions-host-json.md) ve [local.settings.json](functions-run-local.md#local-settings-file) yapılandırma dosyaları, projedeki tüm işlevi uygulanır. Bu [PowerShell projesine](functions-reference-powershell.md#folder-structure) Azure'da çalışan bir işlev uygulaması ile aynıdır.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Azure işlevleri temel araçları, Visual Studio, çalıştırın ve Azure işlevleri projenizi yerel olarak hata ayıklama izin vermek için kod ile tümleştirilir.  

1. İşlevinizin hatalarını ayıklamak için bir çağrı ekleyin [ `Wait-Debugger` ] hata ayıklayıcıyı iliştirmek istediğiniz önce işlev kodunu cmdlet'te, işlev uygulaması projesi başlatmak ve hata ayıklayıcıyı iliştirmek için F5 tuşuna basın. Temel Araçlar’daki çıktı, **Terminal** panelinde görüntülenir.

1. **Terminal** panelinde, HTTP ile tetiklenen işlevinizin URL uç noktasını kopyalayın.

    ![Azure yerel çıktısı](./media/functions-create-first-function-powershell/functions-vscode-f5.png)

1. Sorgu dizesini URL'ye `?name=<yourname>` bu URL ve ardından `Invoke-RestMethod` isteği şu şekilde yürütmek için:

    ```powershell
    PS > Invoke-RestMethod -Method Get -Uri http://localhost:7071/api/HttpTrigger?name=PowerShell
    Hello PowerShell
    ```

    Ayrıca, bir tarayıcıdan GET isteği yürütebilir.

    HttpTrigger uç noktası olmadığında geçirme çağırdığınızda bir `name` sorgu parametresi olarak veya gövde parametresi, işlev, 500 hata döndürür. Run.ps1 kodu gözden geçirirken, bu hata, tasarım gereği oluşur bakın.

1. Hata ayıklamayı durdurmak için Shift + F5 tuşuna basın.

İşlevin yerel bilgisayarınızda düzgün çalıştığını doğruladıktan sonra, projeyi Azure'da yayımlamanın zamanı gelmiştir.

> [!NOTE]
> Çağrıları kaldırmayı unutmayın `Wait-Debugger` Azure'da işlevlerinizin yayımlamadan önce. 

> [!NOTE]
> Azure'da bir işlev uygulaması oluşturma, işlev uygulaması adı için yalnızca ister. AzureFunctions.advancedCreation diğer tüm değerler için size sorulması için true olarak ayarlayın.

[!INCLUDE [functions-publish-project-vscode](../../includes/functions-publish-project-vscode.md)]

## <a name="test"></a>İşlev Azure'da çalıştırın

Azure'da yayımlanan işlevinizin çalıştığını doğrulamak için aşağıdaki PowerShell komutunu yürütün değiştirerek `Uri` önceki adımdan HTTPTrigger işlevin URL'si ile parametre. Önceki örneklerde olduğu gibi sorgu dizesini URL'ye `&name=<yourname>` aşağıdaki örnekteki gibi bir URL:

```powershell
PS > Invoke-WebRequest -Method Get -Uri "https://glengatest-vscode-powershell.azurewebsites.net/api/HttpTrigger?code=nrY05eZutfPqLo0som...&name=PowerShell"

StatusCode        : 200
StatusDescription : OK
Content           : Hello PowerShell
RawContent        : HTTP/1.1 200 OK
                    Content-Length: 16
                    Content-Type: text/plain; charset=utf-8
                    Date: Thu, 25 Apr 2019 16:01:22 GMT

                    Hello PowerShell
Forms             : {}
Headers           : {[Content-Length, 16], [Content-Type, text/plain; charset=utf-8], [Date, Thu, 25 Apr 2019 16:01:22 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 16
```

## <a name="next-steps"></a>Sonraki adımlar

Basit bir HTTP ile tetiklenen işlevi ile bir PowerShell işlev uygulaması oluşturmak için Visual Studio Code kullandığınızı. Daha fazla bilgi edinin isteyebilirsiniz [PowerShell işlevi yerel olarak hata ayıklama](functions-debug-powershell-local.md) Azure işlevleri çekirdek araçları kullanarak. Kullanıma [Azure işlevleri PowerShell Geliştirici kılavuzunda](functions-reference-powershell.md).

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure portal]: https://portal.azure.com
[Azure Functions Core Tools]: functions-run-local.md
[Visual Studio Code için Azure İşlevleri uzantısı]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions
['Wait-Hata Ayıklayıcı']: /powershell/module/microsoft.powershell.utility/wait-debugger?view=powershell-6
