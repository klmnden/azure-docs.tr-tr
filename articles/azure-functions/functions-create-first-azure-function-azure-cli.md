---
title: "Azure CLI’de ilk işlevinizi oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 11/08/2017
ms.topic: quickstart
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: cfowler
ms.openlocfilehash: 4356d00b2694224f52a9359cd4a57d3a70a34d18
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="create-your-first-function-using-the-azure-cli"></a>Azure CLI kullanarak ilk işlevinizi oluşturma

Bu hızlı başlangıç konusu, Azure İşlevleri’ni kullanarak ilk işlevinizi oluşturma hakkında bilgi vermektedir. İşlev uygulaması oluşturmak için, işlevinizi barındıran [sunucusuz](https://azure.microsoft.com/overview/serverless-computing/) bir altyapı olan Azure CLI’yi kullanın. İşlev kodu bir GitHub örnek deposundan dağıtılır.    

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar 

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin bir [GitHub](https://github.com) hesabı. 
+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Kullandığınız sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta benzersiz bir işlev uygulamasının adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. _deployment-source-url_ parametresi GitHub’da bir "Merhaba Dünya" HTTP ile tetiklenen işlevi içeren örnek bir depodur.

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--consumption-plan-location westeurope --deployment-source-url https://github.com/Azure-Samples/functions-quickstart
```
_Consumption-plan-location_ parametresini ayarlamak, işlev uygulamasının bir Tüketim barındırma planında barındırıldığı anlamına gelir. Bu planda, işlevleriniz tarafından gerektirildikçe kaynaklar dinamik olarak eklenir ve yalnızca işlevler çalışırken ücret ödersiniz. Daha fazla bilgi için bkz. [Doğru barındırma planını seçme](functions-scale.md). 

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

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

[!INCLUDE [functions-quickstart-next-steps-cli](../../includes/functions-quickstart-next-steps-cli.md)]