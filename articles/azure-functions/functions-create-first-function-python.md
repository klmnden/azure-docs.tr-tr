---
title: Azure'da bir HTTP ile tetiklenen işlev oluşturma
description: Azure işlevleri çekirdek araçları ve Azure CLI kullanarak Azure'da ilk Python uygulamanızı oluşturma konusunda bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 04/24/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: python
manager: jeconnoc
ms.openlocfilehash: 9bfe3cf78c7b8ac30088ffe6a5baa0d460d37607
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64867307"
---
# <a name="create-an-http-triggered-function-in-azure"></a>Azure'da bir HTTP ile tetiklenen işlev oluşturma

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

Bu makalede Azure işlevleri'nde çalışan bir Python projesi oluşturmak için komut satırı araçlarını kullanmayı gösterir. Oluşturduğunuz işlevi, HTTP isteklerinden tetiklenir. Son olarak çalışacak şekilde projenizi yayımlayabilmeniz bir [sunucusuz işlev](functions-scale.md#consumption-plan) azure'da.

Bu makalede Azure işlevleri için iki Hızlı başlangıçlar, davranıştır. Bu makaleyi tamamladıktan sonra [ekleme bir Azure depolama kuyruğu çıktı bağlaması](functions-add-output-binding-storage-queue-python.md) işlevinize.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdakilere sahip olmanız gerekir:

+ Yükleme [Python 3.6](https://www.python.org/downloads/).

+ Yükleme [Azure işlevleri çekirdek Araçları](./functions-run-local.md#v2) 2.6.666 sürümü veya üzeri.

+ Yükleme [Azure CLI](/cli/azure/install-azure-cli) sürüm 2.x veya üzeri.

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-and-activate-a-virtual-environment"></a>Oluşturma ve bir sanal ortam etkinleştirin

Yerel olarak geliştirip test Python işlevleri için bir Python 3.6 ortamında çalışmalıdır. Oluşturma ve adlı bir sanal ortam etkinleştirmek için aşağıdaki komutları çalıştırın `.env`.

### <a name="bash-or-a-terminal-window"></a>Bash veya bir terminal penceresi:

```bash
python3.6 -m venv .env
source .env/bin/activate
```

### <a name="powershell-or-a-windows-command-prompt"></a>PowerShell veya bir Windows komut istemi:

```powershell
py -3.6 -m venv .env
.env\scripts\activate
```

Sanal ortamın içinde kalan komutlar çalıştırılır.

## <a name="create-a-local-functions-project"></a>Bir yerel işlevler projesi oluşturma

İşlevler projesini bir Azure işlev uygulamasında eşdeğerdir. Tümü aynı yerel ve barındırma yapılandırmalarına paylaşan birden çok işlevleri sağlayabilirsiniz.

Sanal ortamda aşağıdaki komutu çalıştırın, seçme komutunu **python** , alt çalışma zamanı.

```command
func init MyFunctionProj
```

Adlı bir klasör _MyFunctionProj_ oluşturulur, aşağıdaki üç dosyaları içerir:

* `local.settings.json` yerel olarak çalıştırılırken uygulama ayarlarının ve bağlantı dizeleri depolamak için kullanılır. Bu dosya, Azure'da yayımlanan değil.
* `requirements.txt` Azure'da yayımlamak için yüklenecek paketlerin listesini içerir.
* `host.json` bir işlev uygulamasında tüm işlevleri etkiler genel yapılandırma seçenekleri içerir. Bu dosya, Azure'da yayımlanan.

Yeni MyFunctionProj klasöre gidin:

```command
cd MyFunctionProj
```

Ardından, uzantı paketleri etkinleştirmek için host.json dosyasını güncelleştirin.  

## <a name="reference-bindings"></a>Başvuru bağlamaları

Uzantı paketleri kolaylaştırır yol aşağı bağlama uzantıları. Ayrıca .NET Core yükleme gereksinimini kaldırır 2.x SDK. Uzantı paketleri, temel Araçlar 2.6.1071 sürümünü veya sonraki bir sürümünü gerektirir. 

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık, bir işlev projenize ekleyebilirsiniz.

## <a name="create-a-function"></a>İşlev oluşturma

Projenize bir işlev eklemek için aşağıdaki komutu çalıştırın:

```command
func new
```

Seçin **HTTP tetikleyicisi** şablonu, türü `HttpTrigger` işlevi için ad Enter tuşuna basın.

Adlı bir alt klasör _HttpTrigger_ oluşturulur, aşağıdaki dosyaları içerir:

* **Function.JSON**: işlev, tetikleyici ve diğer bağlamalar tanımlayan yapılandırma dosyası. Bu dosyayı gözden geçirmesine ve görüp değeri `scriptFile` çağırma tetikleyicisini ve bağlamalarını tanımlanan sırasında işlev içeren dosyayı işaret `bindings` dizisi.

  Her bağlama, bir yönü, türü ve benzersiz bir ad gerektirir. HTTP tetikleyicisi türünde bir giriş bağlaması olan [ `httpTrigger` ](functions-bindings-http-webhook.md#trigger) ve çıktı bağlaması türü [ `http` ](functions-bindings-http-webhook.md#output).

* **__init__.py**: HTTP betik dosyasının tetiklenen işlevi. Bu komut dosyasını gözden geçirin ve varsayılan içerip içermediğini `main()`. HTTP tetikleyicisi verileri kullanarak bu işlevi geçirilir `req` bağlama parametresinin adı. Function.JSON içinde tanımlanan `req` örneğidir [azure.functions.HttpRequest sınıfı](/python/api/azure-functions/azure.functions.httprequest). 

    Tanımlanan dönüş nesnesi `$return` function.json içinde örneğidir [azure.functions.HttpResponse sınıfı](/python/api/azure-functions/azure.functions.httpresponse). Daha fazla bilgi için bkz. [Azure işlevleri HTTP Tetikleyicileri ve bağlamaları](functions-bindings-http-webhook.md).

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Aşağıdaki komut, Azure üzerinde aynı Azure işlevleri çalışma zamanı kullanılarak yerel olarak çalışan işlev uygulamasını başlatır.

```bash
func host start
```

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

        HttpTrigger: http://localhost:7071/api/MyHttpTrigger

[8/27/2018 10:38:27 PM] Host started (29486ms)
[8/27/2018 10:38:27 PM] Job host started
```

Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Yerel işlevin döndürdüğü GET isteğine tarayıcıda verilen yanıt aşağıda gösterilmiştir:

![Tarayıcıda yerel olarak test etme](./media/functions-create-first-function-python/function-test-local-browser.png)

İşlevinizi yerel olarak çalıştırdığınıza göre, işlev uygulamasını ve gerekli diğer kaynakları Azure’da oluşturabilirsiniz.

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app-in-azure"></a>Azure'da bir işlev uygulaması oluşturma

Bir işlev uygulaması, işlev kodunuzu yürütmeye yönelik bir ortam sağlar. Bu sayede daha kolay yönetilmesi, dağıtım ve kaynakların paylaşımı için bir mantıksal birim olarak gruplandırmanıza işlevleri.

Bir yerine benzersiz işlev uygulamanızın adını kullanarak aşağıdaki komutu çalıştırarak `<APP_NAME>` yer tutucu ve depolama hesabı adı için `<STORAGE_NAME>`. `<APP_NAME>` aynı zamanda işlev uygulamasının varsayılan DNS etki alanıdır. Bu ad Azure'daki tüm uygulamalar arasında benzersiz olmalıdır.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --os-type Linux \
--consumption-plan-location westeurope  --runtime python \
--name <APP_NAME> --storage-account  <STORAGE_NAME>
```

> [!NOTE]
> Linux ve Windows uygulamaları, aynı kaynak grubunda barındırılamaz. Adlı bir kaynak grubu varsa `myResourceGroup` Windows işlev uygulaması veya web uygulaması ile farklı bir kaynak grubu kullanmanız gerekir.

Azure işlev uygulaması için yerel işlevler projenizi yayımlamak artık hazırsınız.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

## <a name="next-steps"></a>Sonraki adımlar

Python işlevleri projenizi yerel makinenizde çalıştırmak, bir HTTP ile tetiklenen işlev ile oluşturduğunuz ve Azure'a dağıtılabilir. Şimdi, işleviniz tarafından genişletme...

> [!div class="nextstepaction"]
> [Bir Azure depolama ekleme kuyruk çıktı bağlaması](functions-add-output-binding-storage-queue-python.md)