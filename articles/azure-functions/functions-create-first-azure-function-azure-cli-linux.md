---
title: Linux’ta Azure CLI’de ilk işlevinizi oluşturma (önizleme) | Microsoft Docs
description: Azure CLI kullanarak varsayılan bir Linux görüntüsü üzerinde çalışan ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 1cf20a4a93ef1b5bfb9c7818f35be5e75e45a3d2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901099"
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Azure CLI kullanarak Linux’ta çalışan ilk işlevinizi oluşturma (önizleme)

Azure İşlevleri, işlevlerinizi Linux’ta varsayılan bir Azure App Service kapsayıcısında barındırmanıza olanak sağlar. Ayrıca [kendi özel kapsayıcınızı getirebilirsiniz](functions-create-function-linux-custom-image.md). Bu işlevsellik şu anda önizleme aşamasındadır ve [İşlevler 2.0 çalışma zamanını](functions-versions.md) gerektirir.

Bu hızlı başlangıç konusunda, Linux’ta varsayılan App Service kapsayıcısında barındırılan ilk işlev uygulamanızı oluşturmak için Azure CLI ile Azure İşlevleri’ni nasıl kullanacağınız gösterilmektedir. İşlev kodu bir GitHub örnek deposundan görüntüye dağıtılır.    

Aşağıdaki adımlar Mac, Windows veya Linux bilgisayarlarda desteklenir. 

## <a name="prerequisites"></a>Ön koşullar 

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0.21 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

Linux İşlev barındırma şu anda yalnızca App Service planı üzerinde desteklenmektedir. Tüketim planı barındırma henüz desteklenmemektedir. Barındırma hakkında daha fazla bilgi edinmek için, bkz. [Azure İşlevleri barındırma planları karşılaştırması](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Linux’ta işlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. Bir Linux App Service planı ile [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. _deployment-source-url_ parametresi GitHub’da bir "Merhaba Dünya" HTTP ile tetiklenen işlevi içeren örnek bir depodur.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
İşlev uygulaması oluşturulduktan ve dağıtıldıktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
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

`myAppServicePlan` bir Linux planı olduğu için, Linux üzerinde işlev uygulamasını çalıştıran kapsayıcıyı oluşturmak için yerleşik docker görüntüsü kullanılır. 

>[!NOTE]  
>Örnek depo şu anda iki betik oluşturma dosyası içerir, [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) ve [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). .deployment dosyası, dağıtım işlemine [özel dağıtım betiği](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) olarak deploy.sh dosyasının kullanılacağını bildirir. Geçerli önizleme sürümünde, işlev uygulamasını bir Linux görüntüsüne dağıtmak için betikler gerekir.  

## <a name="configure-the-function-app"></a>İşlev uygulamasını yapılandırma

GitHub deposundaki proje için İşlevler çalışma zamanının 1.x sürümü gerekir. `FUNCTIONS_WORKER_RUNTIME` uygulama ayarını `~1` olarak ayarladığınızda, işlev uygulaması en son 1.x sürümüne sabitlenir. [az functionapp config appsettings set](https://docs.microsoft.com/cli/azure/functionapp/config/appsettings#set) komutuyla uygulama ayarlarını belirleyin.

Aşağıdaki Azure CLI komutunda <app_name>, işlev uygulamanızın adıdır.

```azurecli-interactive
az functionapp config appsettings set --name <app_name> \
--resource-group myResourceGroup \
--settings FUNCTIONS_WORKER_RUNTIME=~1
```

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
