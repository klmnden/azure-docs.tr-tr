---
title: Python için Azure işlevleri Geliştirici Başvurusu
description: Python ile işlevleri Geliştirme işlemini anlama
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
keywords: Azure işlevleri, İşlevler, olay işleme, dinamik işlem, sunucusuz mimari, python
ms.service: azure-functions
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/16/2018
ms.author: glenga
ms.openlocfilehash: 28f2b395c7f9be1b194b500ef20456be8ff405b0
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58438706"
---
# <a name="azure-functions-python-developer-guide"></a>Azure işlevleri Python Geliştirici Kılavuzu

Bu makalede, Python kullanarak Azure işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içeriği, zaten okuduğunuz varsayılmaktadır [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Programlama modeli

Bir Azure işlevi, giriş işleyen, çıktı üretir Python betiğinizi durum bilgisi olmayan bir yöntem olmalıdır. Varsayılan olarak, çalışma zamanı adlı bir genel yöntem uygulanması için bekliyor `main()` içinde `__init__.py` dosya.

Varsayılan yapılandırma belirterek değiştirebilirsiniz `scriptFile` ve `entryPoint` özelliklerinde `function.json` dosya. Örneğin, _function.json_ aşağıda çalışma zamanı kullanmak için bildiren _customentry()_ yönteminde _main.py_ dosyası, Azure işleviniz için giriş noktası olarak.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Tetikleyiciler ve bağlamalar verilerden yöntem öznitelikleri kullanarak işleve bağlı `name` tanımlanan özellik `function.json` yapılandırma dosyası. Örneğin, _function.json_ adlı bir HTTP isteği tarafından tetiklenen basit bir işlev aşağıda anlatılmaktadır `req`:

```json
{
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

`__init__.py` Dosyası aşağıdaki işlev kodu içerir:

```python
def main(req):
    user = req.params.get('user')
    return f'Hello, {user}!'
```

İsteğe bağlı olarak ayrıca parametre türleri bildirme ve Python tür ek açıklamaları kullanarak işlev dönüş türü. Örneğin, aynı işlevi kullanılarak aşağıdaki gibi ek açıklamaları, yazılabilir:

```python
import azure.functions

def main(req: azure.functions.HttpRequest) -> str:
    user = req.params.get('user')
    return f'Hello, {user}!'
```  

Dahil Python ek açıklamalarını kullanma [azure.functions.*](/python/api/azure-functions/azure.functions?view=azure-python) giriş ve çıkışları için yöntemlerinizi bağlamak için paket. 

## <a name="folder-structure"></a>klasör yapısı

Bir Python işlevleri proje için klasör yapısı aşağıdaki gibi görünür:

```
 FunctionApp
 | - MyFirstFunction
 | | - __init__.py
 | | - function.json
 | - MySecondFunction
 | | - __init__.py
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.py
 | | - mySecondHelperFunction.py
 | - host.json
 | - requirements.txt
 | - extensions.csproj
 | - bin
```

Var olan bir paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyası ve bağlama yapılandırma dosyası (function.json) vardır. 

Paylaşılan kod ayrı bir klasöre tutulmalıdır. Modüller SharedCode klasöründe başvurmak için aşağıdaki söz dizimini kullanabilirsiniz:

```
from ..SharedCode import myFirstHelperFunction
```

İşlevler çalışma zamanı tarafından kullanılan bağlama uzantıları içinde tanımlanmış `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](./functions-bindings-register.md#local-development-azure-functions-core-tools) Azure işlevleri çekirdek araçları kullanarak. 

İşlev uygulamanızda Azure işlevleri projesi dağıtırken, paket, ancak klasörün kendisini FunctionApp klasörün tüm içeriğini eklenmelidir.

## <a name="inputs"></a>Girişler

Girişler, Azure işlevleri'nde iki kategoriye ayrılmıştır: Tetikleyici girişi ve ek giriş. Farklı olsa `function.json`, kullanım Python kodunda aynıdır. Aşağıdaki kod parçacığını bir örnek olarak alalım:

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous",
      "route": "items/{id}"
    },
    {
      "name": "obj",
      "direction": "in",
      "type": "blob",
      "path": "samples/{id}",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

```python
import azure.functions as func
import logging

def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

İşlev çağrıldığında, HTTP isteği olarak işleve geçirilir `req`. Bir giriş temel Azure Blob depolama alanından alınan _kimliği_ yönlendirme URL'sindeki ve olarak kullanıma sunulan `obj` işlev gövdesindeki.

## <a name="outputs"></a>Çıkışlar

Çıkış, dönüş değeri ve çıkış parametresi belirtilebilir. Yalnızca bir çıkış varsa, dönüş değeri kullanmanızı öneririz. Birden çok çıkış için çıktı parametreleri kullanmak zorunda kalırsınız.

Bir işlevin dönüş değeri bir çıkış bağlaması değeri olarak kullanılacak `name` bağlama özelliği ayarlanmalıdır `$return` içinde `function.json`.

Birden çok çıktı üretmek için kullanmak `set()` yöntemi tarafından sağlanan `azure.functions.Out` bağlama için bir değer atamak için arabirim. Örneğin, aşağıdaki işlev bir kuyruğa bir ileti gönderin ve ayrıca bir HTTP yanıtı döndürür.

```json
{
  "scriptFile": "__init__.py",
  "bindings": [
    {
      "name": "req",
      "direction": "in",
      "type": "httpTrigger",
      "authLevel": "anonymous"
    },
    {
      "name": "msg",
      "direction": "out",
      "type": "queue",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "direction": "out",
      "type": "http"
    }
  ]
}
```

```python
import azure.functions as func

def main(req: func.HttpRequest,
         msg: func.Out[func.QueueMessage]) -> str:

    message = req.params.get('body')
    msg.set(message)
    return message
```

## <a name="logging"></a>Günlüğe kaydetme

Azure işlevleri çalışma zamanı Günlükçü erişimi bir kök kullanılabilir [ `logging` ](https://docs.python.org/3/library/logging.html#module-logging) işlev uygulamanızı işleyici. Bu Günlükçü Application Insights'a bağlanır ve bayrağını uyarılar ve hatalarla işlev yürütülürken karşılaşıldı olanak tanır.

Bir HTTP tetikleyici işlevi çağrıldığında, aşağıdaki örnek bir bilgi iletisi günlüğe kaydeder.

```python
import logging

def main(req):
    logging.info('Python HTTP trigger function processed a request.')
```

Ek günlük yöntemlerini kullanılabilir farklı izleme düzeylerini konsola yazma olanak veren:

| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| günlüğe kaydetme. **kritik (_ileti_)**   | Üzerinde kök Günlükçü düzeyi kritik içeren bir ileti yazar.  |
| günlüğe kaydetme. **hata (_ileti_)**   | Üzerinde kök Günlükçü düzeyi hata içeren bir ileti yazar.    |
| günlüğe kaydetme. **uyarı (_ileti_)**    | Kök Günlükçü üzerinde uyarı düzeyine sahip bir ileti yazar.  |
| günlüğe kaydetme. **bilgisi (_ileti_)**    | Üzerinde kök Günlükçü düzeyi bilgileri içeren bir ileti yazar.  |
| günlüğe kaydetme. **hata ayıklama (_ileti_)** | Üzerinde kök Günlükçü düzeyinde hata ayıklama içeren bir ileti yazar.  |

## <a name="importing-shared-code-into-a-function-module"></a>Paylaşılan kod bir işlev modülü içe aktarma

Göreli alma söz dizimini kullanarak Python modüllerini işlevi modülleri ile birlikte yayımlanan aktarılması gerekir:

```python
from . import helpers  # Use more dots to navigate up the folder structure.
def main(req: func.HttpRequest):
    helpers.process_http_request(req)
```

Alternatif olarak, paylaşılan kod bir tek başına paketine koyabilir, genel veya özel Pypı örneği için yayımlama ve normal bir bağımlılık olarak belirtin.

## <a name="async"></a>zaman uyumsuz

İşlev uygulaması yalnızca bir Python işlemi bulunabilir gerektiğinden, bir Azure işlevi kullanarak bir zaman uyumsuz bir eş yordam olarak uygulamak için önerilir `async def` deyimi.

```python
# Will be run with asyncio directly
async def main():
    await some_nonblocking_socket_io_op()
```

Main() işlevi zaman uyumlu ise (hiçbir `async` niteleyicisi) otomatik olarak çalıştıralım bir `asyncio` iş parçacığı havuzu.

```python
# Would be run in an asyncio thread-pool
def main():
    some_blocking_socket_io()
```

## <a name="context"></a>Bağlam

Yürütme sırasında bir işlev çağırma bağlamı almak için dahil `context` imzası bağımsız değişkeni. 

Örneğin:

```python
import azure.functions

def main(req: azure.functions.HttpRequest,
            context: azure.functions.Context) -> str:
    return f'{context.invocation_id}'
```

**Bağlam** sınıf aşağıdaki yöntemleri vardır:

`function_directory`  
İşlev çalıştığı dizin.

`function_name`  
İşlevin adı.

`invocation_id`  
Geçerli işlev çağırma kimliği.

## <a name="python-version-and-package-management"></a>Python sürümü ve paket Yönetimi

Şu anda Azure işlevleri Python yalnızca destekler 3.6.x (resmi CPython dağıtım).

Visual Studio Code ve Azure işlevleri çekirdek araçları kullanarak yerel olarak geliştirirken, adlarını ve sürümleri için gerekli paketleri ekleme `requirements.txt` dosya ve bunları yüklemeniz kullanarak `pip`.

Örneğin, aşağıdaki gereksinimleri dosya ve pip komut yüklemek için kullanılabilir `requests` Pypı paketinden.

```bash
pip install requests
```

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

Yayımlanmaya hazır olduğunuzda, tüm bağımlılıklarınızı listelendiğini doğrulayın `requirements.txt` dosyası, proje dizininizin kökünde bulunur. Başarıyla, Azure işlevleri'ni çalıştırmak için gereksinimleri dosya en az şu paketleri içermelidir:

```txt
azure-functions
azure-functions-worker
grpcio==1.14.1
grpcio-tools==1.14.1
protobuf==3.6.1
six==1.11.0
```

## <a name="publishing-to-azure"></a>Azure'da yayımlamak için

Azure'da yayımlamak için bir derleyici gerektirir ve manylinux uyumlu tekerlekleri Pypı gelen yüklenmesini desteklemiyor bir paketi kullanıyorsanız, şu hatayla başarısız olur: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Otomatik olarak oluşturup gerekli ikili dosyaları, yapılandırma [Docker yükleme](https://docs.docker.com/install/) kullanarak yayımlamak için aşağıdaki komutu çalıştırın ve yerel makine üzerinde [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2) (func). Değiştirmeyi unutmayın `<app name>` azure'daki işlev uygulamanızın adıyla. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Aslında, çalıştırmak için docker temel araçları kullanacağınız [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) olarak yerel makinenizde kapsayıcı görüntüsü. Bu ortamı kullanarak ardından oluşturun ve bunları azure'a son dağıtım için paketleme önce kaynak dağıtım, gerekli modüllerini yükleyin.

> [!NOTE]
> Temel Araçları (func) PyInstaller program kullanıcı kodunu ve Azure'da çalıştırmak için tek tek başına yürütülebilir dosya bağımlılıklarınızı dondurmak için kullanır. Bu işlevsellik şu anda Önizleme aşamasındadır ve Python paketlerini tüm türlerini genişletmek de değil. Modülleri içeri aktarmak zamanınız yoksa kullanarak yeniden yayımlamayı deneyin `--no-bundler` seçeneği. 
> ```
> func azure functionapp publish <app_name> --build-native-deps --no-bundler
> ```
> Sorun yaşamaya devam ederseniz, lütfen tarafından bize [açılırken bir sorun](https://github.com/Azure/azure-functions-core-tools/issues/new) ve sorunun açıklamasını dahil. 


Bağımlılıklarınızı oluşturmak ve sürekli tümleştirme (CI) ve sürekli teslim (CD) sistemi kullanarak yayımlamak için kullanabileceğiniz bir [Azure işlem hattı](https://docs.microsoft.com/azure/devops/pipelines/get-started-yaml?view=vsts) veya [Travis CI özel betik](https://docs.travis-ci.com/user/deployment/script/). 

Bir örnek aşağıdadır `azure-pipelines.yml` derleme ve yayımlama işlemi için komut dosyası.
```yml
pool:
  vmImage: 'Ubuntu 16.04'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '8.x'

- script: |
    set -e
    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
    curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
    sudo apt-get install -y apt-transport-https
    echo "install Azure CLI..."
    sudo apt-get update && sudo apt-get install -y azure-cli
    npm i -g azure-functions-core-tools --unsafe-perm true
    echo "installing dotnet core"
    curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin --channel 2.0
- script: |
    set -e
    az login --service-principal --username "$(APP_ID)" --password "$(PASSWORD)" --tenant "$(TENANT_ID)" 
    func settings add FUNCTIONS_WORKER_RUNTIME python
    func extensions install
    func azure functionapp publish $(APP_NAME) --build-native-deps
```

Bir örnek aşağıdadır `.travis.yaml` derleme ve yayımlama işlemi için komut dosyası.

```yml
sudo: required

language: node_js

node_js:
  - "8"

services:
  - docker

before_install:
  - echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | sudo tee /etc/apt/sources.list.d/azure-cli.list
  - curl -L https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
  - sudo apt-get install -y apt-transport-https
  - sudo apt-get update && sudo apt-get install -y azure-cli
  - npm i -g azure-functions-core-tools --unsafe-perm true


script:
  - az login --service-principal --username "$APP_ID" --password "$PASSWORD" --tenant "$TENANT_ID"
  - az account get-access-token --query "accessToken" | func azure functionapp publish $APP_NAME --build-native-deps

```

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Tüm bilinen sorunları ve özellik istekleri kullanılarak izlenir [GitHub sorunları](https://github.com/Azure/azure-functions-python-worker/issues) listesi. Bir sorunla karşılaşırsanız ve sorunu Github'da bulunamıyor, yeni bir sorun açın ve sorunun ayrıntılı bir açıklama ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [BLOB Depolama bağlamaları](functions-bindings-storage-blob.md)
* [HTTP ve Web kancası bağlamaları](functions-bindings-http-webhook.md)
* [Kuyruk depolama bağlamaları](functions-bindings-storage-queue.md)
* [Zamanlayıcı tetikleyicisi](functions-bindings-timer.md)
