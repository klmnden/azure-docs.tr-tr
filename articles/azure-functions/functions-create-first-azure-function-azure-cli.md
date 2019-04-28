---
title: Azure CLI kullanarak ilk işlevinizi oluşturma
description: Azure CLI ve Azure Functions Core Tools kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin.
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/13/2018
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: 7ff57519cfbb99fa705aff6c970951730c501f3e
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104433"
---
# <a name="create-your-first-function-from-the-command-line"></a>Komut satırından ilk işlevinizi oluşturma

Bu hızlı başlangıç konusu, komut satırından veya terminalden ilk işlevinizi oluşturma hakkında bilgi vermektedir. İşlev uygulaması oluşturmak için, işlevinizi barındıran [sunucusuz](https://azure.microsoft.com/solutions/serverless/) bir altyapı olan Azure CLI’yi kullanın. İşlev kodu projesi, bir uygulamadan, işlev uygulaması projesini Azure’ye dağıtmak için de kullanılan [Azure Functions Core Tools](functions-run-local.md) kullanılarak bir şablondan oluşturulur.

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

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

İstendiğinde, aşağıdaki dil seçimlerinden bir worker çalışma zamanı seçin:

+ `dotnet`: yeni bir .NET Core sınıf kitaplığı projesi (.csproj) oluşturur.
+ `node`: bir JavaScript projesi oluşturur.

Komut yürütüldüğünde, aşağıdaki çıktıya benzer bir şey görürsünüz:

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Initialized empty Git repository in C:/functions/MyFunctionProj/.git/
```

Yeni `MyFunctionProj` proje klasörüne gitmek için aşağıdaki komutu kullanın.

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#az-functionapp-create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. Ayrıca ayarlamalısınız `<language>` işlev uygulamanız için çalışma zamanı gelen `dotnet` (C#) veya `node` (JavaScript).

```azurecli-interactive
az functionapp create --resource-group myResourceGroup --consumption-plan-location westeurope \
--name <app_name> --storage-account  <storage_name> --runtime <language> 
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

[!INCLUDE [functions-publish-project](../../includes/functions-publish-project.md)]

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]

