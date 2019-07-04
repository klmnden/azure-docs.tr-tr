---
title: Bir Azure depolama kuyruğu bağlaması Python işlevinize ekleyin
description: Bir Azure depolama kuyruğu çıktısında işlevleri temel araçları ve Azure CLI kullanarak Python işlevinize bağlama eklemeyi öğrenin.
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
ms.openlocfilehash: c2565a5549cbca08b987883e5905f09070b5ab2c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443195"
---
# <a name="add-an-azure-storage-queue-binding-to-your-python-function"></a>Bir Azure depolama kuyruğu bağlaması Python işlevinize ekleyin

Azure işlevleri kendi tümleştirme kod yazmak zorunda kalmadan işlevleri için Azure Hizmetleri ve diğer kaynaklara bağlanmanıza olanak sağlar. Bunlar *bağlamaları*, hem giriş hem de çıktıyı temsil içine işlev tanımı bildirilir. Veri bağlamaları işlevi için parametre olarak sağlanır. Bir tetikleyici, giriş bağlaması özel türüdür. Bir işlevi yalnızca bir tetikleyiciye sahip olmakla birlikte, birden çok giriş ve çıkış bağlamaları. Daha fazla bilgi için bkz. [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

Bu makalede oluşturduğunuz işlev birleştirmek gösterilmektedir [önceki hızlı başlangıç makalesi](functions-create-first-function-python.md) bir Azure depolama kuyruğu ile. Bu işleve ekleyin çıkış bağlaması veri HTTP isteğinden kuyrukta bir ileti yazar. 

Çoğu bağlamaları işlevlerini kullanan bağlı hizmete erişmek için bir saklı bağlantı dizesi gerektirir. Bunu kolaylaştırmak için işlev uygulamanızı oluşturduğunuz depolama hesabını kullanın. Bu hesap için bir bağlantı zaten adlı ayar bir uygulamada depolanan `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Önkoşullar

Bu makalede başlamadan önce bölümündeki adımları tamamlamanız [Python hızlı başlangıç 1. bölüm](functions-create-first-function-python.md).

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirme

Önceki hızlı başlangıç makalesinde Azure gerekli depolama hesabı ile birlikte bir işlev uygulaması oluşturdunuz. Bu hesap için bağlantı dizesini uygulama ayarlarını azure'da güvenli bir şekilde depolanır. Bu makalede, aynı hesaptaki bir depolama kuyruğuna ileti yazma. İşlevi yerel olarak çalışırken, depolama hesabınıza bağlanmak için uygulama ayarları için da local.settings.json dosyasını indirmeniz gerekir. Aşağıdaki komutu çalıştırın local.settings.json dosyasına ayarları indirmek için Azure işlevleri çekirdek Araçları komut değiştirerek `<APP_NAME>` önceki makaleden işlev uygulamanızın adıyla:

```bash
func azure functionapp fetch-app-settings <APP_NAME>
```

Azure hesabınızda oturum açmak için gerekli.

> [!IMPORTANT]  
> Gizli dizileri içerdiği için hiçbir zaman yayınlanan local.settings.json dosyasında ve kaynak denetiminden hariç tutulabilmesi.

Değer ihtiyacınız `AzureWebJobsStorage`, depolama hesabı bağlantı dizesi olduğu. Çıkış bağlaması beklendiği gibi çalıştığını doğrulamak için bu bağlantıyı kullanın.

## <a name="enable-extension-bundles"></a>Uzantı paketleri etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Şimdi, ekleyebileceğiniz bir depolama çıkış bağlaması projenize.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

İşlevler, her tür bağlama gerektirir bir `direction`, `type`ve benzersiz bir `name` function.json dosyasında tanımlanmalıdır. Ek özellikler bağlama türüne bağlı olarak gerekli olabilir. [Kuyruk çıktı yapılandırma](functions-bindings-storage-queue.md#output---configuration) bir Azure depolama kuyruğu bağlama için gerekli alanlar açıklanır.

Bir bağlamayı oluşturmak için bir bağlama yapılandırma nesnesine ekleme `function.json` dosya. Bir nesneye eklemek için HttpTrigger klasörünüze function.json dosyayı düzenleyin `bindings` dizi aşağıdaki özelliklere sahiptir:

| Özellik | Değer | Açıklama |
| -------- | ----- | ----------- |
| **`name`** | `msg` | Kodunuzda başvurulan bağlama parametresi tanımlayan ad. |
| **`type`** | `queue` | Bir Azure depolama kuyruğu bağlaması bağlamadır. |
| **`direction`** | `out` | Bağlama bir çıkış bağlaması var. |
| **`queueName`** | `outqueue` | Bağlama Yazar Kuyruğun adı. Zaman *queueName* yok, ilk kullanımda bağlama oluşturur. |
| **`connection`** | `AzureWebJobsStorage` | Depolama hesabı için bağlantı dizesi içeren bir uygulama ayarı adı. `AzureWebJobsStorage` Ayarı, işlev uygulaması ile oluşturduğunuz depolama hesabının bağlantı dizesini içerir. |

Function.json dosyanız şimdi aşağıdaki örnekteki gibi görünmelidir:

```json
{
  "scriptFile": "__init__.py",
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

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Yapılandırıldıktan sonra kullanmaya başlayabilirsiniz `name` işlev imzası bir yöntem özniteliği olarak erişmek için bağlama. Aşağıdaki örnekte, `msg` örneğidir [ `azure.functions.InputStream class` ](/python/api/azure-functions/azure.functions.httprequest).

```python
import logging

import azure.functions as func


def main(req: func.HttpRequest, msg: func.Out[func.QueueMessage]) -> str:

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        msg.set(name)
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

Bir çıkış bağlaması kullanarak, Azure depolama SDK'sı kod kimlik doğrulaması için kullanılacak bir kuyruk başvurusu alma veya verileri yazma yok. İşlevler çalışma zamanı ve kuyruk çıkış bağlaması yapma bu görevleri sizin için.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Önceki örneklerde olduğu gibi yerel işlevler çalışma zamanı'nı başlatmak için aşağıdaki komutu kullanın:

```bash
func host start
```

> [!NOTE]  
> Önceki makalede host.json içinde uzantı paketleri etkinleştirmenize olduğundan [depolama bağlama uzantısı](functions-bindings-storage-blob.md#packages---functions-2x) indirildi ve diğer Microsoft bağlama uzantıları birlikte başlatılırken yüklenmiş.

Çalışma zamanı çıktısından `HttpTrigger` işlevinizin URL’sini kopyalayın ve tarayıcınızın adres çubuğuna yapıştırın. `?name=<yourname>` sorgu dizesini bu URL’ye ekleyip isteği yürütün. Önceki makalede yaptığınız gibi aynı yanıtı tarayıcıda görmeniz gerekir.

Bu kez, çıkış bağlaması da adlı bir sıra oluşturur `outqueue` , depolama hesabı ve bu aynı dize içeren bir ileti ekler.

Ardından, yeni sıranın görüntülemek ve bir ileti eklendiğini doğrulamak için Azure CLI'yı kullanın. Kuyruk kullanarak görüntüleyebilirsiniz [Microsoft Azure Depolama Gezgini][Azure Storage Explorer] veya [Azure portalında](https://portal.azure.com).

### <a name="set-the-storage-account-connection"></a>Depolama hesabı bağlantısı

Local.settings.json dosyasını açın ve değeri kopyalayın `AzureWebJobsStorage`, depolama hesabı bağlantı dizesi olduğu. Ayarlama `AZURE_STORAGE_CONNECTION_STRING` aşağıdaki Bash komutu kullanarak bağlantı dizesi ortam değişkeni:

```azurecli-interactive
export AZURE_STORAGE_CONNECTION_STRING=<STORAGE_CONNECTION_STRING>
```

Bağlantı dizesi ile kümesinde `AZURE_STORAGE_CONNECTION_STRING` ortam değişkeni, kimlik doğrulaması her zaman sağlamak zorunda kalmadan, depolama hesabınıza erişebilirsiniz.

### <a name="query-the-storage-queue"></a>Sorgu Depolama kuyruğu

Kullanabileceğiniz [ `az storage queue list` ](/cli/azure/storage/queue#az-storage-queue-list) hesabınızda aşağıdaki örnekte olduğu gibi depolama kuyrukları görmek için komutu:

```azurecli-interactive
az storage queue list --output tsv
```

Bu komutun çıktısı, adlandırılan bir kuyruğun içerir `outqueue`, işlevi çalıştırıldığında, oluşturulan kuyruk olduğu.

Ardından, [ `az storage message peek` ](/cli/azure/storage/message#az-storage-message-peek) aşağıdaki örnekte olduğu gibi bu kuyruktaki iletileri görmek için komutu.

```azurecli-interactive
echo `echo $(az storage message peek --queue-name outqueue -o tsv --query '[].{Message:content}') | base64 --decode`
```

Döndürülen dize işlevi test etmek için gönderilen ileti ile aynı olması gerekir.

> [!NOTE]  
> Önceki örnekte, döndürülen base64 dizesi kodunu çözer. Kuyruk depolama bağlamaları okunup yazılacağını Azure Depolama'dan nedeni [base64 dizeleri](functions-bindings-storage-queue.md#encoding).

Artık, Azure için güncelleştirilmiş işlevi uygulamayı yeniden yayımlaması için zamanı geldi.

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

Yeniden dağıtılan işlevi test etmek için cURL veya bir tarayıcı kullanabilirsiniz. Önceki gibi sorgu dizesini URL'ye `&name=<yourname>` aşağıdaki örnekteki gibi bir URL:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

Yapabilecekleriniz [depolama kuyruğu iletisi inceleyin](#query-the-storage-queue) çıkış yeniden bağlaması kuyrukta yeni bir ileti oluşturur doğrulayın.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bir depolama kuyruğuna veri yazmak için HTTP ile tetiklenen işlevinizi güncelleştirdik. Python kullanarak Azure işlevleri geliştirme hakkında daha fazla bilgi edinmek için [Azure işlevleri Python Geliştirici Kılavuzu](functions-reference-python.md) ve [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md).

Ardından, işlev uygulamanız için Application Insights izleme etkinleştirmeniz gerekir:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure Storage Explorer]: https://storageexplorer.com/