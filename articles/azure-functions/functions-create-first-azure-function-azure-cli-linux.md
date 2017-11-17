---
title: "Linux Azure clı'dan (Önizleme) ilk işlevinizi oluşturma | Microsoft Docs"
description: "İlk Azure Azure CLI kullanarak bir varsayılan Linux görüntüsü üzerinde çalışan işlevinizi oluşturmayı öğrenin."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.date: 11/15/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: d04e2000f2043e8bb11e15f6b9d7fd06ef5b9da3
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="create-your-first-function-running-on-linux-using-the-azure-cli-preview"></a>Azure CLI (Önizleme) kullanarak Linux üzerinde çalışan ilk işlevinizi oluşturma

Azure işlevleri, bir varsayılan Azure App Service kapsayıcıda Linux'ta işlevlerinizi barındırmak olanak sağlar. Bu işlevsellik şu anda önizlemede değil. Ayrıca [kendi özel kapsayıcı Getir](functions-create-function-linux-custom-image.md). 

Bu hızlı başlangıç konu, Azure işlevlerinin Azure CLI ile uygulama hizmeti kapsayıcı varsayılan olarak barındırılan Linux üzerinde ilk işlevi uygulamanızı oluşturmak için nasıl kullanılacağı açıklanmaktadır. İşlev kodu, GitHub örnek depodan görüntüye dağıtılır.    

Aşağıdaki adımlar, Mac, Windows veya Linux bilgisayarı üzerinde desteklenir. 

## <a name="prerequisites"></a>Ön koşullar 

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Yüklemek ve CLI yerel olarak kullanmak seçerseniz, bu konu Azure CLI Sürüm 2.0.21 gerektirir veya sonraki bir sürümü. Çalıştırma `az --version` sürüm bulunamadı. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Linux uygulama hizmeti planı oluştur

Linux barındırma işlevleri için şu anda yalnızca bir uygulama hizmeti plan üzerinde desteklenir. Tüketim barındırma planı henüz desteklenmiyor. Barındırma hakkında daha fazla bilgi için bkz: [Azure işlevleri barındırma planları karşılaştırma](functions-scale.md). 

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Linux üzerinde bir işlev uygulaması oluşturma

Linux üzerinde işlevlerinizin yürütülmesini barındırmak için bir işlev uygulaması olması gerekir. İşlev uygulaması işlevi kodunuzu yürütülmesi için bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. Kullanarak bir işlev uygulaması oluşturma [az functionapp oluşturma](/cli/azure/functionapp#create) bir Linux uygulama hizmeti planıyla komutu. 

Aşağıdaki komutta, gördüğünüz benzersiz işlev uygulama adı yerine `<app_name>` yer tutucu ve depolama hesabı adı için `<storage_name>`. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. _Dağıtım kaynağı URL'si_ parametresi "Merhaba tetiklenen World" HTTP işlevi içeriyor github'da örnek deposudur.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-source-url https://github.com/Azure-Samples/functions-quickstart-linux
```
İşlev sonra uygulama oluşturulduğundan ve dağıtılan, Azure CLI bilgileri aşağıdaki örneğe benzer şekilde gösterir:

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

Çünkü `myAppServicePlan` Linux planı yerleşik docker görüntüsü işlev uygulaması Linux üzerinde çalışan kapsayıcısı oluşturmak için kullanılır. 

>[!NOTE]  
>Örnek depo şu anda iki komut dosyalarını içeren [deploy.sh](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/deploy.sh) ve [.deployment](https://github.com/Azure-Samples/functions-quickstart-linux/blob/master/.deployment). .Deployment dosya deploy.sh olarak kullanılacak dağıtım işlemi söyler [özel dağıtım komut dosyası](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script). Geçerli Önizleme sürümü, komut dosyaları Linux görüntüsüne işlevi uygulamasını dağıtmak için gereklidir.  

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]
