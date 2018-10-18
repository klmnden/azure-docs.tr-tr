---
title: Azure CLI kullanarak ilk işlevinizi oluşturma
description: Azure CLI ve Azure Functions Core Tools kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 09/10/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: ef5459b2b31b67afe187612ffc1ab079a5045a8c
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114919"
---
# <a name="create-your-first-function-from-the-command-line"></a>Komut satırından ilk işlevinizi oluşturma

Bu hızlı başlangıç konusu, komut satırından veya terminalden ilk işlevinizi oluşturma hakkında bilgi vermektedir. İşlev uygulaması oluşturmak için, işlevinizi barındıran [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir altyapı olan Azure CLI’yi kullanın. İşlev kodu projesi, bir uygulamadan, işlev uygulaması projesini Azure’ye dağıtmak için de kullanılan [Azure Functions Core Tools](functions-run-local.md) kullanılarak bir şablondan oluşturulur.

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ [Azure Core Tools sürüm 2.x](functions-run-local.md#v2) yükleme.

+ [Azure CLI]( /cli/azure/install-azure-cli)’yi yükleyin. Bu makale, Azure CLI 2.0 veya sonraki bir sürümü gerektirir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. [Azure Cloud Shell](https://shell.azure.com/bash)’i de kullanabilirsiniz.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>Yerel işlev uygulaması projesi oluşturma

Geçerli yerel dizinin `MyFunctionProj` klasöründe bir işlev uygulaması projesi oluşturmak için komut satırından aşağıdaki komutu çalıştırın. `MyFunctionProj`’de bir GitHub deposu da oluşturulur.

```bash
func init MyFunctionProj
```

İstendiğinde, aşağıdaki dil seçimlerinden bir worker çalışma zamanı seçmek için ok tuşlarını kullanın:

+ `dotnet`: yeni bir .NET Core sınıf kitaplığı projesi (.csproj) oluşturur.
+ `node`: bir JavaScript projesi oluşturur.

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

## <a name="create-a-function"></a>İşlev oluşturma

Aşağıdaki komut yeni projeye gider ve HTTP ile tetiklenen `MyHtpTrigger` adlı bir işlev oluşturur.

```bash
cd MyFunctionProj
func new --name MyHttpTrigger --template "HttpTrigger"
```

Komut yürütüldüğünde, aşağıdaki çıktıya benzer, JavaScript işlevi olan bir şey görürsünüz:

```output
Writing C:\functions\MyFunctionProj\MyHttpTrigger\index.js
Writing C:\functions\MyFunctionProj\MyHttpTrigger\sample.dat
Writing C:\functions\MyFunctionProj\MyHttpTrigger\function.json
```

## <a name="edit-the-function"></a>İşlevi düzenle

Varsayılan olarak şablon, istekte bulunurken bir işlev anahtarı gerektiren bir işlev oluşturur. Azure’da işlevi test etmeyi kolaylaştırmak üzere, işlevi anonim erişime izin vermek için güncelleştirmeniz gerekir. Bu değişikliği yapmanızın yolu işlev projenizin diline bağlıdır.

### <a name="c"></a>C\#

Yeni işleviniz olan MyHttpTrigger.cs kod dosyasını açın ve işlev tanımındaki **AuthorizationLevel** özniteliğini `anonymous` değerine güncelleştirin ve değişikliklerinizi kaydedin.

```csharp
[FunctionName("MyHttpTrigger")]
        public static IActionResult Run([HttpTrigger(AuthorizationLevel.Anonymous, 
            "get", "post", Route = null)]HttpRequest req, ILogger log)
```

### <a name="javascript"></a>JavaScript

Yeni işlevinizin function.json dosyasını açın, dosyayı bir metin düzenleyicide açın, **bindings.httpTrigger**’daki **authLevel** özelliğini `anonymous`’e güncelleştirin ve değişikliklerinizi kaydedin.

```json
  "bindings": [
    {
      "authLevel": "anonymous",
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
    }
  ]
```

Artık işlev anahtarını sağlamanıza gerek kalmadan işlevi Azure’da çağırabilirsiniz. İşlevi anahtarı, yerel olarak çalıştırılırken hiçbir zaman gerekli değildir.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Aşağıdaki komut işlev uygulamasını başlatır. Uygulama, Azure’daki Azure İşlevleri çalışma zamanını kullanarak çalışır.

```bash
func host start --build
```

C# projeleri derlemek için `--build` seçeneği gereklidir. Bu JavaScript projesi seçeneğine ihtiyacınız yoktur.

İşlevler ana bilgisayarı başlatıldığında, kolay okunması için kırpılmış olan aşağıdaki çıktıya benzer bir şey yazar:

```output

                  %%%%%%
                 %%%%%%
            @   %%%%%%    @
          @@   %%%%%%      @@
       @@@    %%%%%%%%%%%    @@@
     @@      %%%%%%%%%%        @@
       @@         %%%%       @@
         @@      %%%       @@
           @@    %%      @@
                %%
                %

...

Content root path: C:\functions\MyFunctionProj
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.

...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Çalışma zamanı çıktısından `HTTPTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Yerel işlevin döndürdüğü GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

![Tarayıcıda yerel olarak test etme](./media/functions-create-first-azure-function-azure-cli/functions-test-local-browser.png)

İşlevinizi yerel olarak çalıştırdığınıza göre, işlev uygulamasını ve gerekli diğer kaynakları Azure’da oluşturabilirsiniz.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. _deployment-source-url_ parametresi GitHub’da bir "Merhaba Dünya" HTTP ile tetiklenen işlevi içeren örnek bir depodur.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name>  
```

_Consumption-plan-location_ parametresini ayarlamak, işlev uygulamasının bir Tüketim barındırma planında barındırıldığı anlamına gelir. Bu sunucusuz planda, işlevleriniz gerektirdikçe kaynaklar dinamik olarak eklenir ve yalnızca işlevler çalışırken ücret ödersiniz. Daha fazla bilgi için bkz. [Doğru barındırma planını seçme](functions-scale.md).

İşlev uygulaması oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

Core Tools sürüm 2.x, Azure İşlevleri 2.x çalışma zamanının şablonlarını kullanarak projeler oluşturur. Bu yüzden, 2.x çalışma zamanının Azure’da kullanıldığından emin olmalısınız. `FUNCTIONS_WORKER_RUNTIME` uygulama ayarını `~2` olarak ayarladığınızda, işlev uygulaması en son 2.x sürümüne sabitlenir. [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) komutuyla uygulama ayarlarını belirleyin.

Aşağıdaki Azure CLI komutunda <app_name>, işlev uygulamanızın adıdır.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~2
```

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
