---
title: "Özel Docker kapsayıcısı kayıt defteri oluşturma - Azure CLI| Microsoft Docs"
description: "Azure CLI 2.0 ile Docker kapsayıcısı kayıt defterleri oluşturmaya ve yönetmeye başlayın"
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: c7cdb1b13bf32388d18c2a25af28337a81861c1e
ms.contentlocale: tr-tr
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-cli"></a>Azure CLI’yı kullanarak yönetilen bir kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda, Azure CLI kullanarak yönetilen bir Azure Container Registry örneği oluşturma hakkındaki ayrıntılar yer alır.

Yönetilen Azure kapsayıcı kayıt defterleri önizleme aşamasındadır ve tüm bölgelerde kullanılamaz.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yükleyip kullanmayı seçerseniz bu hızlı başlangıç için Azure CLI 2.0.4 veya sonraki bir sürümünü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *westcentralus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

[az act create](/cli/azure/acr#create) komutunu kullanarak bir ACR örneği oluşturun.

> [!NOTE]
> Bir kayıt defteri oluştururken yalnızca harf ve sayı içeren genel olarak benzersiz bir üst düzey etki alanı adı sağlayın.

 Örnekteki kayıt defterinin adını (*myContainerRegistry1*), kendi bulduğunuz benzersiz bir adla değiştirin.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

Kayıt defteri oluşturulduğunda çıkış aşağıdakilere benzer:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-to-acr-instance"></a>ACR örneğinde oturum açma

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için [az acr login](/cli/azure/acr#login) komutunu kullanın.

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

Komut tamamlandığında bir “Oturum Açma Başarılı” iletisi döndürür.

## <a name="use-azure-container-registry"></a>Azure Container Registry’yi kullanma

### <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Bir depodaki görüntüleri ve etiketleri sorgulamak için `az acr` CLI komutlarını kullanın.

> [!NOTE]
> Container Kayıt Defteri şu anda görüntü ve etiket sorgulamak için `docker search` komutunu desteklememektedir.

### <a name="list-repositories"></a>Depoları listeleme

Aşağıdaki örnekte, bir kayıt defterindeki depolar JSON (JavaScript Nesne Gösterimi) biçiminde listelenmiştir:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Etiketleri listeleme

Aşağıdaki örnekte, **samples/nginx** deposundaki etiketler JSON biçiminde listelenmiştir:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI kullanarak bir yönetilen Azure Container Registry örneği oluşturdunuz.

> [!div class="nextstepaction"]
> [Docker CLI’yı kullanarak ilk görüntünüzü itme](container-registry-get-started-docker-cli.md)

