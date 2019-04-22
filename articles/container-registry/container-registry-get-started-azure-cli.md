---
title: Hızlı Başlangıç - Azure - Azure CLI özel Docker kayıt defteri oluşturma
description: Azure CLI ile hızlıca özel bir Docker kapsayıcısı kayıt defteri oluşturmayı öğrenin.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 01/22/2019
ms.author: danlep
ms.custom: seodec18, H1Hack27Feb2017, mvc
ms.openlocfilehash: 24bdd52673c65d039166dc28f9f0a0a784569a1a
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678709"
---
# <a name="quickstart-create-a-private-container-registry-using-the-azure-cli"></a>Hızlı Başlangıç: Azure CLI kullanarak bir özel kapsayıcı kayıt defteri oluşturma

Azure Container Registry, özel Docker kapsayıcı görüntülerini depolamak için kullanılan bir yönetilen Docker kapsayıcı kayıt defteridir. Bu kılavuzda, Azure CLI kullanarak bir Azure Container Registry örneği oluşturma hakkındaki ayrıntılar yer alır. Sonra kayıt defterine bir kapsayıcı görüntüsü göndermeye Docker komutlarını kullanın ve son olarak çekmek ve görüntüyü kayıt defterinizden çalıştırın.

Bu hızlı başlangıçta Azure CLI'yı çalıştıran gerektirir (2.0.55 sürümü veya üzeri önerilir). Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli].

Ayrıca sisteminizde yerel olarak Docker yüklü olması gerekir. Docker [macOS][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell gerekli tüm Docker bileşenlerini (`dockerd` daemon) içermediğinden, bu hızlı başlangıçta Cloud Shell’i kullanamazsınız.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma

Bu hızlı başlangıçta oluşturduğunuz bir *temel* maliyet açısından iyileştirilmiş bir seçenek Azure Container Registry hakkında öğrenme geliştiriciler için kayıt defteri. Kullanılabilir hizmet katmanları hakkında daha fazla bilgi için bkz: [kapsayıcı kayıt defteri SKU'ları][container-registry-skus].

[az act create][az-acr-create] komutunu kullanarak bir ACR örneği oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. Aşağıdaki örnekte *myContainerRegistry007* komutu kullanılmıştır. Bunu benzersiz bir değerle güncelleştirin.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Kayıt defteri oluşturulduğunda çıkış aşağıdakilere benzer:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2019-01-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "mycontainerregistry007.azurecr.io",
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

Not `loginServer` çıktıda olduğu tam kayıt defteri adını (tamamı küçük harflerle). Bu hızlı başlangıcın geri kalanında `<acrName>`, kapsayıcı kayıt defteri adı için bir yer tutucudur.

## <a name="log-in-to-registry"></a>Kayıt defterinde oturum açma

Gönderme kapsayıcı görüntülerini gönderip çekmeden önce kayıt defterinde oturum açmalısınız. Bunu yapmak için [az acr login][az-acr-login] komutunu kullanın.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` iletisi döndürülür.

[!INCLUDE [container-registry-quickstart-docker-push](../../includes/container-registry-quickstart-docker-push.md)]

## <a name="list-container-images"></a>Kapsayıcı görüntülerini listeleme

Aşağıdaki örnek, kayıt defterindeki depoları listeler:

```azurecli
az acr repository list --name <acrName> --output table
```

Çıkış:

```
Result
----------------
hello-world
```

Aşağıdaki örnekte yer alan etiketlerin listesi **Merhaba-Dünya** depo.

```azurecli
az acr repository show-tags --name <acrName> --repository hello-world --output table
```

Çıkış:

```
Result
--------
v1
```

[!INCLUDE [container-registry-quickstart-docker-pull](../../includes/container-registry-quickstart-docker-pull.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse [az grubu Sil] [ az-group-delete] kaynak grubunu, kapsayıcı kayıt defteri ve burada depolanan kapsayıcı görüntülerini kaldırmak için komutu.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure CLI ile bir Azure Container Registry oluşturdunuz, kayıt defterine bir kapsayıcı görüntüsü gönderdiniz çekilir ve kayıt defterinden görüntü çalıştı. ACR derin göz atmak için Azure Container Registry öğreticilere geçin.

> [!div class="nextstepaction"]
> [Azure Container Registry öğreticileri][container-registry-tutorial-quick-task]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-pull]: https://docs.docker.com/engine/reference/commandline/pull/
[docker-rmi]: https://docs.docker.com/engine/reference/commandline/rmi/
[docker-run]: https://docs.docker.com/engine/reference/commandline/run/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-tutorial-quick-task]: container-registry-tutorial-quick-task.md
[container-registry-skus]: container-registry-skus.md
