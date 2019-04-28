---
title: Azure'da ilk Python uygulamanızı oluşturma
description: Azure işlevleri çekirdek araçları ve Azure CLI kullanarak Azure'da ilk Python uygulamanızı oluşturma konusunda bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 08/29/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: af684a4fcc3a70326c1a57cb10a39204b4fd12dc
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103941"
---
# <a name="create-your-first-python-function-in-azure-preview"></a>(Önizleme) Azure'da ilk Python uygulamanızı oluşturma

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Bu hızlı başlangıç makalesi ilk oluşturmak için Azure CLI kullanma hakkında bilgi vermektedir [sunucusuz](https://azure.com/serverless) Python işlev Linux üzerinde çalışan uygulama. İşlev kodu yerel ortamda oluşturulur ve ardından [Azure Functions Core Tools](functions-run-local.md) ile Azure'a dağıtılır. Linux üzerinde işlev uygulamalarınızı çalıştırmak için Önizleme konuları hakkında daha fazla bilgi için bkz: [bu makalede Linux işlevleri](https://aka.ms/funclinux).

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir.

## <a name="prerequisites"></a>Önkoşullar

Derleme ve yerel olarak test etmek için ihtiyacınız olacak:

+ Yükleme [Python 3.6](https://www.python.org/downloads/).

+ Yükleme [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2) 2.2.70 sürümü veya üzeri (.NET Core gerektirir 2.x SDK'sı).

Yayımlama ve Azure'da çalıştırmak için:

+ Yükleme [Azure CLI]( /cli/azure/install-azure-cli) sürüm 2.x veya üzeri.

+ Etkin bir Azure aboneliğine ihtiyacınız var.
  [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Oluşturma ve bir sanal ortam etkinleştirin

İşlevler projesi oluşturmak için gerekli olduğu bir Python 3.6 sanal ortamda çalışan. Oluşturma ve adlı bir sanal ortam etkinleştirmek için aşağıdaki komutları çalıştırın `.env`.

```bash
# In Bash
python3.6 -m venv .env
source .env/bin/activate

# In PowerShell
py -3.6 -m venv .env
.env\scripts\activate
```

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

Artık bir yerel işlevler projesi oluşturabilirsiniz. Bu dizinde bir Azure işlev uygulamasında eşdeğerdir. Bu, aynı yerel ve barındırma yapılandırmayı paylaşan birden fazla işlev içerebilir.

Terminal penceresinde ya da bir komut isteminden aşağıdaki komutu çalıştırın:

```bash
func init MyFunctionProj
```

Çekme **python** istenen çalışma zamanı.

```output
Select a worker runtime:
1. dotnet
2. node
3. python
```

Aşağıdaki çıktı benzer bir şey görürsünüz.

```output
Installing wheel package
Installing azure-functions package
Installing azure-functions-worker package
Running pip freeze
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing /MyFunctionProj/.vscode/extensions.json
```

Adlı yeni bir klasör _MyFunctionProj_ oluşturulur. Devam etmek için bu klasörüne dizin değiştirin.

```bash
cd MyFunctionProj
```

## <a name="create-a-function"></a>İşlev oluşturma

Bir işlev oluşturmak için aşağıdaki komutu çalıştırın:

```bash
func new
```

Seçin `HTTP Trigger` şablonu olarak ve bir **işlev adı** , `HttpTrigger`.

```output
Select a template:
1. Blob trigger
2. Cosmos DB trigger
3. Event Grid trigger
4. Event Hub trigger
5. HTTP trigger
6. Queue trigger
7. Service Bus Queue trigger
8. Service Bus Topic trigger
9. Timer trigger

Choose option: 5
Function name: HttpTrigger
```

Aşağıdaki çıktı benzer bir şey görürsünüz.

```output
Writing /MyFunctionProj/HttpTrigger/sample.dat
Writing /MyFunctionProj/HttpTrigger/__init__.py
Writing /MyFunctionProj/HttpTrigger/function.json
The function "HttpTrigger" was created successfully from the "HTTP trigger" template.
```

Bir alt klasör _HttpTrigger_ oluşturulur. Bu içeren `__init__.py` birincil komut dosyası olduğu ve `function.json` tetikleyicisini ve bağlamalarını işlev tarafından kullanılan tanımlayan dosya. Programlama modeli hakkında daha fazla bilgi için başvurabilirsiniz [Azure işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md).

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

İşlevleri konağa yerel olarak çalıştırmak için aşağıdaki komutu kullanın.

```bash
func host start
```

İşlevleri ana bilgisayar başladığında, HTTP ile tetiklenen işlevinizin URL'sini çıkarır. (Tüm çıktıyı okunabilirliği artırmak için kesilmiştir olduğunu unutmayın.)

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
Now listening on: http://0.0.0.0:7071
Application started. Press Ctrl+C to shut down.
...

Http Functions:

        HttpTrigger: http://localhost:7071/api/HttpTrigger
```

Çıktısından işlevinizin URL'sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün.

    http://localhost:7071/api/HttpTrigger?name=<yourname>

Tarayıcıdan tetiklendiğinde aşağıdaki ekran görüntüsünde, işlevden alınan yanıta gösterir:

![test](./media/functions-create-first-function-python/function-test-local-browser.png)

Artık bir işlev uygulaması ve Azure'da yayımlamak için gerekli diğer kaynakları oluşturmaya hazırsınız.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-function-app-in-azure"></a>Azure'da bir Linux işlev uygulaması oluşturma

İşlev uygulaması, işlev kodunuzu yürütmeye yönelik bir ortam sağlar. Bu sayede daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak gruplandırmanıza işlevleri. Oluşturma bir **Python işlev uygulamasını Linux üzerinde çalışan** kullanarak [az functionapp oluşturma](/cli/azure/functionapp) komutu.

Bir yerine benzersiz işlev uygulamanızın adını kullanarak aşağıdaki komutu çalıştırarak `<app_name>` yer tutucu ve depolama hesabı adı için `<storage_name>`. `<app_name>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <app_name> --storage-account  <storage_name>
```

> [!NOTE]
> Linux dışında App Service uygulamalarının bulunduğu `myResourceGroup` adlı bir kaynak grubunuz varsa farklı bir kaynak grubu kullanmanız gerekir. Windows ve Linux uygulamalarını aynı kaynak grubunda barındıramazsınız.  

İşlev uygulaması oluşturulduktan sonra aşağıdaki iletiyi görürsünüz:

```output
Your serverless Linux function app 'myfunctionapp' has been successfully created.
To active this function app, publish your app content using Azure Functions Core Tools or the Azure portal.
```

Azure işlev uygulaması için yerel işlevler projenizi yayımlamak artık hazırsınız.

## <a name="deploy-the-function-app-project-to-azure"></a>İşlev uygulaması projesini Azure'a dağıtma

Azure işlevleri çekirdek araçları kullanarak, aşağıdaki komutu çalıştırın. Değiştirin `<app_name>` uygulamanızı önceki adımdan adı.

```bash
func azure functionapp publish <app_name>
```

Okunabilirliği artırmak için kesilmiştir aşağıdaki çıktıyı benzer bir şey görürsünüz.

```output
Getting site publishing info...

...

Preparing archive...
Uploading content...
Upload completed successfully.
Deployment completed successfully.
Syncing triggers...
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python kullanarak Azure işlevleri geliştirme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Azure işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md)
> [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
