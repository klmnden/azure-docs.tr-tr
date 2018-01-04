---
title: "Hızlı Başlangıç - Azure CLI ile azure'da özel bir Docker kayıt defteri oluşturma"
description: "Azure CLI ile özel bir Docker kapsayıcısı kayıt defteri oluşturmak hızlı bir şekilde öğrenin."
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
<<<<<<< HEAD
ms.openlocfilehash: 5cddf0ffea256ed6d1c51d48a61ac8176d08b9cc
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
=======
ms.openlocfilehash: f31f4e5e2b3fe5db85873894a7f2fa9c415392c1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Azure CLI’yı kullanarak kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Azure CLI kullanarak Azure kapsayıcı kayıt defteri örneği oluşturma bu kılavuzu ayrıntıları.

Azure CLI Sürüm 2.0.21 çalıştırıyorsanız bu hızlı başlangıç gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure CLI 2.0 yükleme][azure-cli].

Ayrıca, Docker yerel olarak yüklü olması gerekir. Docker sağlar kolayca Docker herhangi yapılandırdığınız paketler [Mac][docker-mac], [Windows][docker-windows], veya [Linux] [ docker-linux] sistem.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu hızlı başlangıç oluşturuyoruz bir *temel* kayıt defteri. Kısaca aşağıdaki tabloda açıklanan birkaç farklı SKU'ları, Azure kapsayıcı kayıt defteri mevcuttur. Her genişletilmiş Ayrıntılar için bkz [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Bir ACR örneği kullanarak oluşturduğunuz [az acr oluşturma] [ az-acr-create] komutu.

Kayıt defteri adını **benzersiz olmalıdır**. Aşağıdaki örnekte *myContainerRegistry007* kullanılır. Benzersiz bir değere güncelleştirin.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
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
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Bu hızlı başlangıç rest kullanırız `<acrName>` kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="log-in-to-acr"></a>ACR için oturum açın

Kapsayıcı görüntülerini gönderip çekmeden önce ACR örneğinde oturum açmalısınız. Bunu yapmak için kullanın [az acr oturum açma] [ az-acr-login] komutu.

```azurecli
az acr login --name <acrName>
```

Komut döndürür bir `Login Succeeded` tamamlandıktan sonra ileti.

## <a name="push-image-to-acr"></a>ACR itme görüntüye

Azure kapsayıcı kayıt defterine görüntü göndermek için ilk görüntüsü olması gerekir. Tüm yerel kapsayıcı görüntüleri henüz yoksa, varolan bir görüntü Docker hub'dan çıkarmak için aşağıdaki komutu çalıştırın.

```bash
docker pull microsoft/aci-helloworld
```

Görüntü için kayıt anında önce ACR oturum açma sunucunuzun tam adı ile etiketlemeniz gerekir. ACR örneğinin tam oturum açma sunucusu adını almak için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Kullanarak resim etiketi [docker etiketi] [ docker-tag] komutu. Değiştir `<acrLoginServer>` ACR örneğinizi oturum açma sunucusu adı.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Son olarak, [docker itme] [ docker-push] ACR örneğine görüntü göndermek için. Değiştir `<acrLoginServer>` ACR örneğinizi oturum açma sunucusu adı.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnek, bir kayıt defterinde depoları listeler:

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```bash
Result
----------------
aci-helloworld
```

Aşağıdaki örnek etiketleri listelenir **aci helloworld** deposu.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Çıktı:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olduğunda, kullanabileceğiniz [az grubu Sil] [ az-group-delete] kaynak grubu, ACR örneği ve tüm kapsayıcı görüntüleri kaldırmak için komutu.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç Azure CLI ile Azure kapsayıcı kayıt defteri oluşturuldu. Azure kapsayıcı kayıt defteri Azure kapsayıcı örnekleri ile kullanmak istiyorsanız, Azure kapsayıcı örnekleri Öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri Öğreticisi][container-instances-tutorial-prepare-app]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md