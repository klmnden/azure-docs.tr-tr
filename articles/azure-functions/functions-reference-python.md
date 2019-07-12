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
ms.openlocfilehash: 14594e95efe94fe38502dc6269627158c42a04be
ms.sourcegitcommit: dda9fc615db84e6849963b20e1dce74c9fe51821
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622364"
---
# <a name="azure-functions-python-developer-guide"></a>Azure işlevleri Python Geliştirici Kılavuzu

Bu makalede, Python kullanarak Azure işlevleri geliştirmeye giriş niteliğindedir. Aşağıdaki içeriği, zaten okuduğunuz varsayılmaktadır [Azure işlevleri Geliştirici Kılavuzu](functions-reference.md).

[!INCLUDE [functions-python-preview-note](../../includes/functions-python-preview-note.md)]

## <a name="programming-model"></a>Programlama modeli

Bir Azure işlevi, giriş işleyen, çıktı üretir Python betiğinizi durum bilgisi olmayan bir yöntem olmalıdır. Varsayılan olarak, çalışma zamanı adlı bir genel yöntem uygulanması gereken yöntemini bekliyor `main()` içinde `__init__.py` dosya.

Varsayılan yapılandırma belirterek değiştirebilirsiniz `scriptFile` ve `entryPoint` özelliklerinde *function.json* dosya. Örneğin, _function.json_ aşağıda çalışma zamanı kullanmak için bildiren `customentry()` yönteminde _main.py_ dosyası, Azure işleviniz için giriş noktası olarak.

```json
{
  "scriptFile": "main.py",
  "entryPoint": "customentry",
  ...
}
```

Tetikleyiciler ve bağlamalar verilerden yöntem öznitelikleri kullanarak işleve bağlı `name` tanımlanan özellik *function.json* dosya. Örneğin, _function.json_ adlı bir HTTP isteği tarafından tetiklenen basit bir işlev aşağıda anlatılmaktadır `req`:

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

İsteğe bağlı olarak, IntelliSense ve kod düzenleyicinize tarafından sağlanan otomatik tamamlama özellikleri yararlanmak için ayrıca öznitelik türlerini bildirir ve Python tür ek açıklamaları kullanarak işlev dönüş türü. 

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
```

Var olan bir paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyası ve bağlama yapılandırma dosyası (function.json) vardır. 

Paylaşılan kod ayrı bir klasöre tutulmalıdır. Modüller SharedCode klasöründe başvurmak için aşağıdaki söz dizimini kullanabilirsiniz:

```
from __app__.SharedCode import myFirstHelperFunction
```

İşlev uygulamanızda Azure, tüm içeriğini işlev projesi dağıtılırken *FunctionApp* klasörü, paket, ancak klasörün kendisini eklenmelidir.

## <a name="triggers-and-inputs"></a>Tetikleyiciler ve girişleri

Girişler, Azure işlevleri'nde iki kategoriye ayrılmıştır: Tetikleyici girişi ve ek giriş. Farklı olsa `function.json` dosya, kullanım Python kodunda aynı.  Bağlantı dizelerini veya tetikleyici ve giriş kaynakları için gizli eşleme değerlere `local.settings.json` yerel olarak çalıştırılırken dosya ve uygulama ayarlarını Azure'da çalışırken. 

Örneğin, aşağıdaki kod, ikisi arasındaki farkı gösterir:

```json
// function.json
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

```json
// local.settings.json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "python",
    "AzureWebJobsStorage": "<azure-storage-connection-string>"
  }
}
```

```python
# __init__.py
import azure.functions as func
import logging


def main(req: func.HttpRequest,
         obj: func.InputStream):

    logging.info(f'Python HTTP triggered function processed: {obj.read()}')
```

İşlev çağrıldığında, HTTP isteği olarak işleve geçirilir `req`. Bir giriş temel Azure Blob depolama alanından alınan _kimliği_ yönlendirme URL'sindeki ve olarak kullanıma sunulan `obj` işlev gövdesindeki.  Burada depolama hesabı bağlantı dizesi bulunursa belirtilen `AzureWebJobsStorage` işlev uygulaması için kullanılan aynı depolama hesabı olduğu.


## <a name="outputs"></a>outputs

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

## <a name="logging"></a>Günlüğe Kaydetme

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

## <a name="async"></a>zaman uyumsuz

Azure işlevinizi kullanarak bir zaman uyumsuz bir eş yordam olarak yazma öneririz `async def` deyimi.

```python
# Will be run with asyncio directly


async def main():
    await some_nonblocking_socket_io_op()
```

Main() işlevi zaman uyumlu ise (hiçbir `async` niteleyicisi) size bir otomatik olarak işlevi çalıştırmak bir `asyncio` iş parçacığı havuzu.

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

## <a name="global-variables"></a>Genel değişkenler

Bunun için sonraki yürütmeleri uygulamanızın durumunu korunur garanti edilmez. Ancak, Azure işlevleri çalışma zamanı, genellikle aynı uygulamanın birden çok yürütme için aynı işlemi kullanır. Pahalı bir hesaplamanın sonuçlarını önbelleğe kaydetmek için bir genel değişken olarak bildirin. 

```python
CACHED_DATA = None


def main(req):
    global CACHED_DATA
    if CACHED_DATA is None:
        CACHED_DATA = load_json()

    # ... use CACHED_DATA in code
```

## <a name="python-version-and-package-management"></a>Python sürümü ve paket Yönetimi

Şu anda Azure işlevleri Python yalnızca destekler 3.6.x (resmi CPython dağıtım).

Visual Studio Code ve Azure işlevleri çekirdek araçları kullanarak yerel olarak geliştirirken, adlarını ve sürümleri için gerekli paketleri ekleme `requirements.txt` dosya ve bunları yüklemeniz kullanarak `pip`.

Örneğin, aşağıdaki gereksinimleri dosya ve pip komut yüklemek için kullanılabilir `requests` Pypı paketinden.

```txt
requests==2.19.1
```

```bash
pip install -r requirements.txt
```

## <a name="publishing-to-azure"></a>Azure'da yayımlamak için

Yayımlamaya hazır olduğunuzda, tüm bağımlılıklarınızı listelendiğini doğrulayın *requirements.txt* dosya, proje dizininizin kökünde bulunur. Azure'da yayımlamak için bir derleyici gerektirir ve manylinux uyumlu tekerlekleri Pypı gelen yüklenmesini desteklemiyor bir paketi kullanıyorsanız, şu hatayla başarısız olur: 

```
There was an error restoring dependencies.ERROR: cannot install <package name - version> dependency: binary dependencies without wheels are not supported.  
The terminal process terminated with exit code: 1
```

Otomatik olarak oluşturup gerekli ikili dosyaları, yapılandırma [Docker yükleme](https://docs.docker.com/install/) kullanarak yayımlamak için aşağıdaki komutu çalıştırın ve yerel makine üzerinde [Azure işlevleri çekirdek Araçları](functions-run-local.md#v2) (func). Değiştirmeyi unutmayın `<app name>` azure'daki işlev uygulamanızın adıyla. 

```bash
func azure functionapp publish <app name> --build-native-deps
```

Aslında, çalıştırmak için docker temel araçları kullanacağınız [mcr.microsoft.com/azure-functions/python](https://hub.docker.com/r/microsoft/azure-functions/) olarak yerel makinenizde kapsayıcı görüntüsü. Bu ortamı kullanarak ardından oluşturun ve bunları azure'a son dağıtım için paketleme önce kaynak dağıtım, gerekli modüllerini yükleyin.

Bağımlılıklarınızı oluşturmak ve bir sürekli teslim (CD) sistemi kullanarak yayımlamak için [Azure DevOps işlem hatları kullanın](https://docs.microsoft.com/azure/azure-functions/functions-how-to-azure-devops). 

## <a name="unit-testing"></a>Birim Testi

Python'da yazılmış işlevleri standart test çerçeveleri kullanarak başka bir Python kod gibi test edilebilir. Çoğu bağlamaları için uygun bir sınıfın bir örneğini oluşturarak sahte bir giriş nesnesi oluşturmak olası `azure.functions` paket. Bu yana [ `azure.functions` ](https://pypi.org/project/azure-functions/) paketi hemen kullanılabilir değil, ile yüklediğinizden emin olun, `requirements.txt` dosya açıklandığı [Python sürümü ve paket Yönetimi](#python-version-and-package-management) yukarıdaki bölümde.

Örneğin, sahte bir test HTTP ile tetiklenen bir işlev, aşağıda verilmiştir:

```json
{
  "scriptFile": "httpfunc.py",
  "entryPoint": "my_function",
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
    }
  ]
}
```

```python
# myapp/httpfunc.py
import azure.functions as func
import logging

def my_function(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}")
    else:
        return func.HttpResponse(
             "Please pass a name on the query string or in the request body",
             status_code=400
        )
```

```python
# myapp/test_httpfunc.py
import unittest

import azure.functions as func
from httpfunc import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock HTTP request.
        req = func.HttpRequest(
            method='GET',
            body=None,
            url='/api/HttpTrigger',
            params={'name': 'Test'})

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp.get_body(),
            b'Hello Test',
        )
```

Kuyruk ile tetiklenen bir işlev ile başka bir örnek aşağıda verilmiştir:

```python
# myapp/__init__.py
import azure.functions as func


def my_function(msg: func.QueueMessage) -> str:
    return f'msg body: {msg.get_body().decode()}'
```

```python
# myapp/test_func.py
import unittest

import azure.functions as func
from . import my_function


class TestFunction(unittest.TestCase):
    def test_my_function(self):
        # Construct a mock Queue message.
        req = func.QueueMessage(
            body=b'test')

        # Call the function.
        resp = my_function(req)

        # Check the output.
        self.assertEqual(
            resp,
            'msg body: test',
        )
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
