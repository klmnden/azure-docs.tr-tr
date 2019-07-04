---
title: İşlevleri Visual Studio Code kullanarak Azure Depolama'ya bağlanma
description: İşlevlerinizi Visual Studio Code'u kullanarak bir Azure depolama kuyruğuna bağlanmak için bir çıkış bağlaması eklemeyi öğrenin.
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
manager: jeconnoc
ms.openlocfilehash: b207064f691391af2c180c7a6ab03e42ed79fcb6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450999"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio-code"></a>İşlevleri Visual Studio Code kullanarak Azure Depolama'ya bağlanma

Azure işlevleri kendi tümleştirme kod yazmak zorunda kalmadan işlevleri Azure Hizmetleri ve diğer kaynaklara bağlanmanıza olanak sağlar. Bunlar *bağlamaları*, hem giriş hem de çıktıyı temsil içine işlev tanımı bildirilir. Veri bağlamaları işlevi için parametre olarak sağlanır. Bir tetikleyici, giriş bağlaması özel türüdür. Bir işlevi yalnızca bir tetikleyiciye sahip olmakla birlikte, birden çok giriş ve çıkış bağlamaları. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu makalede oluşturduğunuz işlev bağlanmak için Visual Studio Code kullanmaya gösterilmektedir [önceki hızlı başlangıç makalesi](functions-create-first-function-vs-code.md) Azure Depolama'ya. Bu işleve ekleyin çıkış bağlaması veri HTTP isteğinden Azure kuyruk depolama kuyruğuna bir ileti yazar. 

Çoğu bağlamaları işlevlerini kullanan bağlı hizmete erişmek için bir saklı bağlantı dizesi gerektirir. Bunu kolaylaştırmak için işlev uygulamanızı oluşturduğunuz depolama hesabını kullanın. Bu hesap için bir bağlantı zaten adlı ayar bir uygulamada depolanan `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Önkoşullar

Bu makalede başlamadan önce aşağıdaki gereksinimleri karşılaması gerekir:

* Yükleme [Visual Studio Code için Azure depolama uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage).
* Yükleme [Azure Depolama Gezgini](https://storageexplorer.com/). Depolama Gezgini, çıkış bağlamanızın tarafından oluşturulan kuyruk iletilerini incelemek için kullanacağınız bir araçtır. Depolama Gezgini, macOS, Windows ve Linux tabanlı işletim sistemlerinde desteklenir.
* Yükleme [.NET Core CLI Araçları](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x) (C# yalnızca projeleri).
* Bölümündeki adımları tamamlamanız [Visual Studio Code hızlı başlangıç 1. bölüm](functions-create-first-function-vs-code.md). 

Bu makale, zaten Azure aboneliğinizi Visual Studio Code'dan oturumunuz olduğunu varsayar. Çalıştırarak oturum `Azure: Sign In` komut paletinden. 

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirme

İçinde [önceki hızlı başlangıç makalesi](functions-create-first-function-vs-code.md), Azure'da bir işlev uygulaması birlikte gerekli depolama hesabı oluşturulur. Bu hesap için bağlantı dizesini uygulama ayarlarını azure'da güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna ileti yazma. İşlevi yerel olarak çalışırken, depolama hesabınıza bağlanmak için uygulama ayarları için da local.settings.json dosyasını indirmeniz gerekir. 

1. Komut paletini açın ardından aramak ve komutu çalıştırmak için F1 tuşuna basın `Azure Functions: Download Remote Settings....`. 

1. Önceki makalede oluşturduğunuz işlev uygulamasını seçin. Seçin **Tümüne Evet** mevcut yerel ayarların üzerine yazmak için. 

    > [!IMPORTANT]  
    > Gizli dizileri içerdiği için local.settings.json dosyasında hiçbir zaman yayımlanabilmesi ve kaynak denetiminden hariç tutuldu.

1. Değeri kopyalamak `AzureWebJobsStorage`, depolama hesabı bağlantı dizesi değerini anahtarı olduğu. Çıkış bağlaması beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Kuyruk depolama çıkış bağlaması kullandığınızdan, proje çalıştırmadan önce yüklü depolama bağlamaları uzantıya sahip olmalıdır. 

### <a name="javascript"></a>JavaScript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\# sınıf kitaplığı

HTTP ve Zamanlayıcı Tetikleyicileri dışında bağlamaları uzantı paketleri uygulanır. Aşağıdaki komutu çalıştırın [dotnet paketini ekleyin](/dotnet/core/tools/dotnet-add-package) depolama uzantı paketi projenize eklemek için Terminal penceresinde komutu.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

Şimdi depolama çıkış projenize bağlaması ekleyin.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

İşlevler, her tür bağlama gerektirir bir `direction`, `type`ve benzersiz bir `name` function.json dosyasında tanımlanmalıdır. Bu öznitelikler tanımladığınız şekilde işlev uygulamanızın dile bağlıdır.

### <a name="javascript"></a>JavaScript

Bağlama özniteliklerini doğrudan function.json dosyasında tanımlanır. Ek özellikler bağlama türüne bağlı olarak gerekli olabilir. [Kuyruk çıktı yapılandırma](functions-bindings-storage-queue.md#output---configuration) bir Azure depolama kuyruğu bağlama için gerekli alanlar açıklanır. Uzantı bağlamaları function.json dosyasına eklemek kolaylaştırır. 

Bir bağlamayı oluşturmak için sağ tıklatın (Ctrl + tıklama macos'ta) `function.json` seçin ve dosya HttpTrigger klasörünüzde **bağlama Ekle...** . Yeni bağlama için aşağıdaki bağlama özelliklerini tanımlamak için istemleri takip edin:

| İstem | Değer | Açıklama |
| -------- | ----- | ----------- |
| **Bağlama yönünü seçin** | `out` | Bağlama bir çıkış bağlaması var. |
| **Bağlama yönünü seçin...** | `Azure Queue Storage` | Bir Azure depolama kuyruğu bağlaması bağlamadır. |
| **Kodunuzda bu bağlamayı tanımlamak için kullanılan ad** | `msg` | Kodunuzda başvurulan bağlama parametresi tanımlayan ad. |
| **Kuyruk iletisi gönderilir** | `outqueue` | Bağlama Yazar Kuyruğun adı. Zaman *queueName* yok, ilk kullanımda bağlama oluşturur. |
| **"Local.setting.json" ayarı seçin** | `AzureWebJobsStorage` | Depolama hesabı için bağlantı dizesi içeren bir uygulama ayarı adı. `AzureWebJobsStorage` Ayarı, işlev uygulaması ile oluşturduğunuz depolama hesabının bağlantı dizesini içerir. |

Bir bağlama eklenir `bindings` aşağıdaki örnekteki gibi görünmelidir function.json dosyanızda dizisi:

```json
{
   ...

  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

### <a name="c-class-library"></a>C\# sınıf kitaplığı

İçinde bir C# sınıf kitaplığı projesi bağlamaları öznitelikleri işlev yöntemine bağlama olarak tanımlanır. Function.json sonra bu özelliklere göre otomatik olarak oluşturulan dosyasıdır.

HttpTrigger.cs proje dosyasını açın ve aşağıdakini ekleyin `using` deyimi:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Aşağıdaki parametre ekleyin `Run` yöntemi tanımı:

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
```

`msg` Parametresi bir `ICollector<T>` bağlamayı çıktısıysa işlev yazılır iletileri koleksiyonunu temsil eden tür tamamlar. Bu durumda, çıkış adlı bir depolama kuyruğuna alınır `outqueue`. Depolama hesabı için bağlantı dizesini ayarlayalım `StorageAccountAttribute`. Bu öznitelik, depolama hesabı bağlantı dizesi içeren ayarı gösterir ve sınıf, yöntem veya parametre düzeyinde uygulanabilir. Bu durumda, atlarsanız `StorageAccountAttribute` varsayılan depolama hesabı zaten kullanıldığı için.

Run yöntemi tanımı, artık aşağıdaki gibi görünmelidir:  

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
```

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Bağlama tanımlandıktan sonra kullanabileceğiniz `name` işlev imzası özniteliği olarak erişmek için bağlama. Bir çıkış bağlaması kullanarak, Azure depolama SDK'sı kod kimlik doğrulaması için kullanılacak bir kuyruk başvurusu alma veya verileri yazma yok. İşlevler çalışma zamanı ve kuyruk çıkış bağlaması yapma bu görevleri sizin için.

### <a name="javascript"></a>JavaScript

Kullanan kodu ekleyin `msg` çıkış bağlaması nesnesinde `context.bindings` kuyruk iletisi oluşturmak için. Bu kodu `context.res` deyiminden önce ekleyin.

```javascript
// Add a message to the Storage queue.
context.bindings.msg = "Name passed to the function: " + 
(req.query.name || req.body.name);
```

Bu noktada, işlevinizi şu şekilde görünmelidir:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " + 
        (req.query.name || req.body.name);
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

### <a name="c"></a>C\#

Kullanan kodu ekleyin `msg` kuyruk iletisi oluşturmak için çıktı bağlama nesnesi. Yöntem döndürmeden önce bu kodu ekleyin.

```cs
if (!string.IsNullOrEmpty(name))
{
    // Add a message to the output collection.
    msg.Add(string.Format("Name passed to the function: {0}", name));
}
```

Bu noktada, işlevinizi şu şekilde görünmelidir:

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    if (!string.IsNullOrEmpty(name))
    {
        // Add a message to the output collection.
        msg.Add(string.Format("Name passed to the function: {0}", name));
    }
    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Çıkış bağlaması ilk kez kullanıldığında, Depolama hesabınızda İşlevler çalışma zamanı tarafından **outqueue** adlı yeni bir kuyruk oluşturulur. Sıranın yanı sıra yeni iletinin oluşturulduğunu doğrulamak için Depolama Gezgini'ni kullanacaksınız.

### <a name="connect-storage-explorer-to-your-account"></a>Depolama Gezgini’ni hesabınıza bağlama

Zaten Azure Depolama Gezgini yüklü ve Azure hesabınıza bağlı bu bölümü atlayın.

1. Çalıştırma [Azure Depolama Gezgini] aracı, sol taraftaki bağlanma simgesini seçip **Hesap Ekle**.

    ![Bir Azure ekleme hesap için Microsoft Azure Depolama Gezgini](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. İçinde **Connect** iletişim kutusunda seçin **bir Azure hesabı Ekle**, seçin, **Azure ortamı**seçip **oturum açın...** . 

    ![Azure hesabınızda oturum açma](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Hesabınıza başarıyla oturum açtıktan sonra tüm hesabınızla ilişkili Azure abonelikleriyle görürsünüz.

### <a name="examine-the-output-queue"></a>Çıkış kuyruğunu inceleme

1. Visual Studio Code'da komut paletini açın ardından aramak ve komutu çalıştırmak için F1 tuşuna basın `Azure Storage: Open in Storage Explorer` ve depolama hesabınızın adını seçin. Azure depolama Gezgini'nde depolama hesabınızı açılır.  

1. **Kuyruklar** düğümünü genişletin ve sonra **outqueue** adlı kuyruğu seçin. 

   Kuyruk, HTTP ile tetiklenen işlevi çalıştırdığınızda kuyruk çıkış bağlamasının oluşturduğu iletiyi içerir. Varsayılan işlevini çağırdığınız `name` değerini *Azure*, kuyruk iletisi *işleve geçirilen ad: Azure*.

    ![Azure depolama Gezgini'nde gösterilen kuyruk iletisi](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. Çalıştırma işlevi başka bir isteği yeniden göndermek ve yeni bir iletinin kuyrukta göründüğünü göreceksiniz.  

Artık, Azure için güncelleştirilmiş işlevi uygulamayı yeniden yayımlaması için zamanı geldi.

## <a name="redeploy-and-test-the-updated-app"></a>Güncelleştirilmiş uygulamayı test etme ve yeniden dağıtma

1. Visual Studio Code'da komut paletini açın için F1 tuşuna basın. Arayın ve seçin komut Paleti'nde `Azure Functions: Deploy to function app...`.

1. İlk makalede oluşturulan işlev uygulaması seçin. Aynı uygulama projenize yeniden dağıtmaya gerek olmadığından seçin **Dağıt** dosyalarının üzerine yazarak ilgili uyarıyı yoksayın.

1. Dağıtım tamamlandıktan sonra bilgisayarına işlevi test etmek için cURL veya tarayıcı yeniden kullanabilirsiniz. Önceki örneklerde olduğu gibi sorgu dizesini URL'ye `&name=<yourname>` aşağıdaki örnekteki gibi bir URL:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. Yeniden [depolama kuyruğunda bir ileti görüntülemek](#examine-the-output-queue) çıkış yeniden bağlaması kuyrukta yeni bir ileti oluşturur doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure’da *Kaynaklar*; işlev uygulamalarını, işlevleri, depolama hesaplarını ve benzeri öğeleri ifade eder. *Kaynak grupları* halinde gruplandırılırlar ve bir grubu silerek içindeki her şeyi silebilirsiniz.

Bu hızlı başlangıçları tamamlamak için kaynaklar oluşturdunuz. [Hesap durumunuza](https://azure.microsoft.com/account/) ve [hizmet fiyatlandırmanıza](https://azure.microsoft.com/pricing/) bağlı olarak size bu kaynakların ücretleri yansıtılabilir. Kaynaklara artık ihtiyacınız yoksa, şunları yaparak silebilirsiniz:

1. Visual Studio Code'da komut paletini açın için F1 tuşuna basın. Arayın ve seçin komut Paleti'nde `Azure Functions: Open in portal`.

1. İşlev uygulamanızı seçin ve Enter tuşuna basın. İşlevi uygulaması sayfasında açılır [Azure portalında](https://portal.azure.com).

1. İçinde **genel bakış** sekmesinde, adlandırılmış altındaki bağlantıyı seçin **kaynak grubu**.

    ![İşlev uygulaması sayfasından silinecek kaynak grubunu seçin.](./media/functions-add-output-binding-storage-queue-vs-code/functions-app-delete-resource-group.png)

1. **Kaynak grubu** sayfasında, dahil edilen kaynakların listesini gözden geçirin ve silmek istediğiniz kaynakların bunlar olduğunu doğrulayın.
 
1. **Kaynak grubunu sil**’i seçin ve yönergeleri izleyin.

   Silme işlemi birkaç dakika sürebilir. İşlem tamamlandığında, birkaç saniye boyunca bir bildirim görüntülenir. Bildirimi görüntülemek için sayfanın üst kısmındaki zil simgesini de seçebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir depolama kuyruğuna veri yazmak için HTTP ile tetiklenen işlevinizi güncelleştirdik. İşlevleri geliştirme hakkında daha fazla bilgi için bkz. [geliştirme Visual Studio Code kullanarak Azure işlevleri](functions-develop-vs-code.md).

Ardından, işlev uygulamanız için Application Insights izleme etkinleştirmeniz gerekir:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Depolama Gezgini]: https://storageexplorer.com/
