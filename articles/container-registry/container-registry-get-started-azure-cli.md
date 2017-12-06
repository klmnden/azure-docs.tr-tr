---
title: "Hızlı Başlangıç - Azure CLI ile azure'da özel bir Docker kayıt defteri oluşturma"
description: "Azure CLI ile özel bir Docker kapsayıcısı kayıt defteri oluşturmak hızlı bir şekilde öğrenin."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 10/16/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 5cddf0ffea256ed6d1c51d48a61ac8176d08b9cc
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Azure CLI kullanarak Azure kapsayıcı kayıt defteri örneği oluşturma bu kılavuzu ayrıntıları.

Azure CLI Sürüm 2.0.20 çalıştırıyorsanız bu hızlı başlangıç gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Ayrıca, Docker yerel olarak yüklü olması gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu hızlı başlangıç oluşturuyoruz bir *temel* kayıt defteri. Kısaca aşağıdaki tabloda açıklanan birkaç farklı SKU'ları, Azure kapsayıcı kayıt defteri mevcuttur. Her genişletilmiş Ayrıntılar için bkz [kapsayıcı kayıt defteri SKU'ları](container-registry-skus.md).

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

[az act create](/cli/azure/acr#create) komutunu kullanarak bir ACR örneği oluşturun.

Kayıt defteri adını **benzersiz olmalıdır**. Aşağıdaki örnekte *myContainerRegistry007* kullanılır. Benzersiz bir değere güncelleştirin.

```azurecli
az acr create --name myContainerRegistry007 --resource-group myResourceGroup --sku Basic
```

Kayıt defteri oluşturulduğunda çıkış aşağıdakilere benzer:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr223140"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Bu hızlı başlangıç rest kullanırız `<acrname>` kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="log-in-to-acr"></a>ACR için oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için [az acr login](/cli/azure/acr#login) komutunu kullanın.

```azurecli
az acr login --name <acrname>
```

Komut tamamlandığında bir “Oturum Açma Başarılı” iletisi döndürür.

## <a name="push-image-to-acr"></a>ACR itme görüntüye

Azure kapsayıcı kayıt defterine görüntü göndermek için ilk görüntüsü olması gerekir. Gerekirse, önceden oluşturulmuş bir görüntü Docker hub'dan çıkarmak için aşağıdaki komutu çalıştırın.

```bash
docker pull microsoft/aci-helloworld
```

Görüntü ACR oturum açma sunucusu adı ile etiketlenmiş gerekiyor. ACR örneğinin oturum açma sunucusu adını döndürmek için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanarak resim etiketi [docker etiketi](https://docs.docker.com/engine/reference/commandline/tag/) komutu. Değiştir  *<acrLoginServer>*  ACR örneğinizi oturum açma sunucusu adı.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Son olarak, [docker itme](https://docs.docker.com/engine/reference/commandline/push/) ACR örneğine görüntü göndermek için. Değiştir  *<acrLoginServer>*  ACR örneğinizi oturum açma sunucusu adı.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnek, bir kayıt defterinde depoları listeler:

```azurecli
az acr repository list -n <acrname> -o table
```

Çıktı:

```bash
Result
----------------
aci-helloworld
```

Aşağıdaki örnek etiketleri listelenir **aci helloworld** deposu.

```azurecli
az acr repository show-tags -n <acrname> --repository aci-helloworld -o table
```

Çıktı:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil](/cli/azure/group#delete) kaynak grubu, ACR örneği ve tüm kapsayıcı görüntüleri kaldırmak için komutu.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure CLI ile Azure kapsayıcı kayıt defteri oluşturuldu. Azure kapsayıcı kayıt defteri Azure kapsayıcı örnekleri ile kullanmak istiyorsanız, Azure kapsayıcı örnekleri Öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri Öğreticisi](../container-instances/container-instances-tutorial-prepare-app.md)