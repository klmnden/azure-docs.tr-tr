---
title: "Azure CLI’de ilk işlevinizi oluşturma | Microsoft Docs"
description: "Azure CLI kullanarak sunucusuz yürütme için ilk Azure İşlevinizi oluşturma hakkında bilgi edinin."
services: functions
keywords: 
author: ggailey777
ms.author: glenga
ms.assetid: 674a01a7-fd34-4775-8b69-893182742ae0
ms.date: 05/02/2017
ms.topic: hero-article
ms.service: functions
ms.custom: mvc
ms.devlang: azure-cli
manager: erikre
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: 2292b35819c5a98b690041e10f6e6d1a93fa7837
ms.contentlocale: tr-tr
ms.lasthandoff: 06/20/2017

---

<a id="create-your-first-function-using-the-azure-cli" class="xliff"></a>

# Azure CLI kullanarak ilk işlevinizi oluşturma

Bu hızlı başlangıç öğreticisi, Azure İşlevleri’ni kullanarak ilk işlevinizi oluşturma hakkında bilgi vermektedir. İşlev uygulaması oluşturmak için , işlevinizi barındıran sunucusuz bir altyapı olan Azure CLI’yi kullanın. İşlev kodu bir GitHub örnek deposundan dağıtılır.    

Mac, Windows veya Linux bilgisayar kullanarak aşağıdaki adımları izleyebilirsiniz. 

<a id="prerequisites" class="xliff"></a>

## Ön koşullar 

Bu örneği çalıştırmadan önce aşağıdakilere sahip olmanız gerekir:

+ Etkin bir [GitHub](https://github.com) hesabı. 
+ Etkin bir Azure aboneliği.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu konu başlığı için Azure CLI 2.0 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 


<a id="create-a-resource-group" class="xliff"></a>

## Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) ile bir kaynak grubu oluşturun. Azure kaynak grubu; işlev uygulamaları, veritabanları ve depolama hesapları gibi Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek `myResourceGroup` adlı bir kaynak grubu oluşturur:

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
<a id="create-an-azure-storage-account" class="xliff"></a>

## Azure Depolama hesabı oluşturma

İşlevler, işlevlerinizin durumunu ve diğer bilgilerini korumak için bir Azure Depolama hesabı kullanır. Oluşturduğunuz kaynak grubunda [az storage account create](/cli/azure/storage/account#create) komutunu kullanarak bir depolama hesabı oluşturun.

Aşağıdaki komutta kendi genel benzersiz depolama hesabı adınızı `<storage_name>` yer tutucusunu gördüğünüz yere yerleştirin. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.

```azurecli-interactive
az storage account create --name <storage_name> --location westeurope --resource-group myResourceGroup --sku Standard_LRS
```

Depolama hesabı oluşturulduktan sonra Azure CLI, aşağıdaki örneğe benzer bilgiler gösterir:

```json
{
  "creationTime": "2017-04-15T17:14:39.320307+00:00",
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myresourcegroup/...",
  "kind": "Storage",
  "location": "westeurope",
  "name": "myfunctionappstorage",
  "primaryEndpoints": {
    "blob": "https://myfunctionappstorage.blob.core.windows.net/",
    "file": "https://myfunctionappstorage.file.core.windows.net/",
    "queue": "https://myfunctionappstorage.queue.core.windows.net/",
    "table": "https://myfunctionappstorage.table.core.windows.net/"
  },
     ....
    // Remaining output has been truncated for readability.
}
```

<a id="create-a-function-app" class="xliff"></a>

## İşlev uygulaması oluşturma

İşlevlerinizin yürütülmesini barındıran bir işlev uygulamasına sahip olmanız gerekir. İşlev uygulaması, işlev kodunuzun sunucusuz yürütülmesine yönelik bir ortam sağlar. Kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır. [az functionapp create](/cli/azure/functionapp#create) komutunu kullanarak bir işlev uygulaması oluşturun. 

Aşağıdaki komutta kendi benzersiz işlev uygulamanızın adını `<app_name>` yer tutucusunun ve `<storage_name>` depolama hesabı adının yerine ekleyin. `<app_name>`, işlev uygulamasının varsayılan DNS etki alanı olarak kullanılacağı için adın Azure’daki tüm uygulamalarda benzersiz olması gerekir. 

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup --consumption-plan-location westeurope
```
Varsayılan olarak, Tüketim barındırma planı ile bir işlev uygulaması oluşturulur; bu durum, kaynakların işlevleriniz gerektirdikçe dinamik olarak eklendiği ve yalnızca kaynaklar çalışırken ücret ödediğiniz anlamına gelir. Daha fazla bilgi için bkz. [Doğru barındırma planını seçme](functions-scale.md). 

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

Artık bir işlev uygulamasına sahip olduğunuza göre, gerçek işlev kodunu GitHub örnek deposundan dağıtabilirsiniz.

<a id="deploy-your-function-code" class="xliff"></a>

## İşlev kodunuzu dağıtma  

Yeni işlev uygulamanızda işlevi kodunuzu oluşturmanın birkaç yolu vardır. Bu konu başlığında, GitHub’daki bir örnek depoya bağlanılır. Daha önce olduğu gibi, aşağıdaki kodda `<app_name>` yer tutucusunu oluşturduğunuz işlev uygulamasının adıyla değiştirin. 

```azurecli-interactive
az functionapp deployment source config --name <app_name> --resource-group myResourceGroup --repo-url https://github.com/Azure-Samples/functions-quickstart --branch master --manual-integration
```
Dağıtım kaynağı ayarlandıktan sonra, Azure CLI aşağıdaki örneğe benzer bilgiler gösterir (okunaklılığı artırmak için null değerler kaldırılmıştır):

```json
{
  "branch": "master",
  "deploymentRollbackEnabled": false,
  "id": "/subscriptions/bbbef702-e769-477b-9f16-bc4d3aa97387/resourceGroups/myResourceGroup/...",
  "isManualIntegration": true,
  "isMercurial": false,
  "location": "West Europe",
  "name": "quickstart",
  "repoUrl": "https://github.com/Azure-Samples/functions-quickstart",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.Web/sites/sourcecontrols"
}
```

<a id="test-the-function" class="xliff"></a>

## İşlevi test etme

Dağıtılan işlevi bir Mac veya Linux bilgisayarda ya da Windows üzerinde Bash kullanarak test etmek için cURL kullanın. `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirerek aşağıdaki cURL komutunu yürütün. `&name=<yourname>` sorgu dizesini URL’ye ekleyin.

```bash
curl http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
```  

![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-curl.png)  

Komut satırınızda cURL yoksa, web tarayıcınızın adres çubuğuna aynı URL'yi girmeniz yeterlidir. Burada da `<app_name>` yer tutucusunu işlev uygulamanızın adıyla değiştirin ve `&name=<yourname>` sorgu dizesini URL’ye ekleyip isteği yürütün. 

    http://<app_name>.azurewebsites.net/api/HttpTriggerJS1?name=<yourname>
   
![Tarayıcıda gösterilen işlev yanıtı.](./media/functions-create-first-azure-function-azure-cli/functions-azure-cli-function-test-browser.png)  

<a id="clean-up-resources" class="xliff"></a>

## Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, bu hızlı başlangıç ile oluşturulan tüm kaynakları silmek için aşağıdaki komutu kullanın:

```azurecli-interactive
az group delete --name myResourceGroup
```
İstendiğinde `y` yazın.

<a id="next-steps" class="xliff"></a>

## Sonraki adımlar

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

