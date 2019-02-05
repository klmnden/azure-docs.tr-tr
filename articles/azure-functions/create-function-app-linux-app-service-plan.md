---
title: Bir işlev uygulaması Azure App Service planı oluşturma
description: Linux üzerinde çalışan Azure CLI kullanarak bir App Service planında bir işlev uygulaması oluşturmayı öğrenin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 11/28/2018
ms.topic: conceptual
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: ec7b71c7da19ecefc14696c029e63a074b498ec8
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696762"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan-preview"></a>Bir işlev uygulaması, Linux üzerinde Azure App Service planında (Önizleme) oluşturma

Azure İşlevleri, işlevlerinizi Linux’ta varsayılan bir Azure App Service kapsayıcısında barındırmanıza olanak sağlar. Bu makale, azure'da çalışan bir Linux barındırılan bir işlev uygulaması oluşturmak için Azure CLI kullanma size bir [App Service planı](functions-scale.md#app-service-plan). Ayrıca [kendi özel kapsayıcınızı getirebilirsiniz](functions-create-function-linux-custom-image.md). Linux barındırma şu anda Önizleme aşamasındadır.

App Service planında bir işlev uygulamanızı ölçeklendirme için sorumlu olursunuz. Azure işlevleri'nin sunucusuz özelliklerinden yararlanmak için ayrıca işlevlerinizi Linux'ta barındırabilir bir [tüketim planı](functions-scale.md#consumption-plan).

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıcı tamamlamak için şunlar gerekir:

+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0.21 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>Bir Linux App Service planı oluşturma

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-a-function-app-on-linux"></a>Linux’ta işlev uygulaması oluşturma

Linux’ta işlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. Bir Linux App Service planı ile [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun.

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Ayrıca ayarlamalısınız `<language>` işlev uygulamanız için çalışma zamanı gelen `dotnet` (C#), `node` (JavaScript) veya `python`.

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --plan myAppServicePlan \
--name <app_name> --storage-account  <storage_name> --runtime <language>
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

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources-simple.md)]

## <a name="next-steps"></a>Sonraki Adımlar

Bu makalede, Azure'da barındırılan Linux işlev uygulaması oluşturma işlemini gösterir. Artık [işlev projesi dağıtma](https://docs.microsoft.com/cli/azure/functionapp/deployment/source?view=azure-cli-latest) bu işlev uygulaması. Azure işlevleri çekirdek araçları için kullanabileceğiniz [işlevler projesi oluşturma](functions-run-local.md) yerel bilgisayarınızdaki ve yeni Linux işlev uygulamanızı dağıtın.  

> [!div class="nextstepaction"] 
> [Azure İşlevleri’ni yerel olarak kodlama ve test etme](functions-run-local.md)
