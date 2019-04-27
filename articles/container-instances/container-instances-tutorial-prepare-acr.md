---
title: Öğretici - Azure Container Instances için bir container Registry'yi hazırlama
description: Azure Container Instances Öğreticisi bölüm 2 / 3 - bir Azure container Registry'yi hazırlama ve bir görüntü gönderme
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: tutorial
ms.date: 03/21/2018
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: c1a4313f9a8174b9ea6e6cff694b9a0a9cf395d1
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60685672"
---
# <a name="tutorial-deploy-an-azure-container-registry-and-push-a-container-image"></a>Öğretici: Bir Azure container Registry'yi dağıtma ve kapsayıcı görüntüsü gönderme

Bu öğretici, üç bölümden oluşan bir serinin ikinci bölümüdür. Öğreticinin [birinci bölümünde](container-instances-tutorial-prepare-app.md), Node.js web uygulaması için bir Docker kapsayıcı görüntüsü oluşturuldu. Bu öğreticide, görüntüyü Azure Container Registry’ye göndereceksiniz. Henüz kapsayıcı görüntüsünü oluşturmadıysanız [Öğretici 1 - Kapsayıcı görüntüsü oluşturma](container-instances-tutorial-prepare-app.md) bölümüne geri dönün.

Azure Container Registry sizin Azure’daki özel Docker kayıt defterinizdir. Bu öğreticide, aboneliğinizde bir Azure Container Registry örneği oluşturur ve sonra buna önceden oluşturduğunuz kapsayıcı resmini gönderirsiniz. Serinin ikinci bölümündeki bu makalede şunları yapacaksınız:

> [!div class="checklist"]
> * Azure Container Registry örneği oluşturacaksınız
> * Azure kapsayıcı kayıt defteriniz için bir kapsayıcı görüntüsü etiketleyeceksiniz
> * Görüntüyü kayıt defterinize yükleyeceksiniz

Serinin son öğreticisi olan sonraki makalede, özel kayıt defterinizdeki kapsayıcıyı Azure Container Instances’a dağıtacaksınız.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [container-instances-tutorial-prerequisites](../../includes/container-instances-tutorial-prerequisites.md)]

## <a name="create-azure-container-registry"></a>Azure kapsayıcı kayıt defteri oluşturma

Kapsayıcı kayıt defterinizi oluşturmadan önce bunun dağıtılacağı bir *kaynak grubu* gerekir. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Aşağıdaki örnekte, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturulur:

```azurecli
az group create --name myResourceGroup --location eastus
```

Kaynak grubunu oluşturduktan sonra [az acr create][az-acr-create] komutuyla bir Azure kapsayıcı kayıt defteri oluşturun. Kapsayıcı kayıt defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. `<acrName>` değerini kayıt defteriniz için benzersiz bir adla değiştirin:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic --admin-enabled true
```

Aşağıda, *mycontainerregistry082* adlı yeni bir Azure kapsayıcı kayıt defteri için çıktı örneği yer almaktadır (burada kısaltılmış şekilde gösterilmektedir):

```console
$ az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
...
{
  "adminUserEnabled": true,
  "creationDate": "2018-03-16T21:54:47.297875+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry082",
  "location": "eastus",
  "loginServer": "mycontainerregistry082.azurecr.io",
  "name": "mycontainerregistry082",
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

Bu öğreticinin geri kalan aşamalarında, bu adımda seçtiğiniz kapsayıcı kayıt defteri adı için yer tutucu olarak `<acrName>` kullanılmaktadır.

## <a name="log-in-to-container-registry"></a>Kapsayıcı kayıt defterinde oturum açma

Görüntü göndermeden önce, Azure Container Registry örneğinizde oturum açmanız gerekir. İşlemi tamamlamak için [az acr login][az-acr-login] komutunu kullanın. Oluşturduğunuzda, kapsayıcı kayıt defteri için seçtiğiniz benzersiz adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandığında `Login Succeeded` döndürülür:

```console
$ az acr login --name mycontainerregistry082
Login Succeeded
```

## <a name="tag-container-image"></a>Kapsayıcı görüntüsünü etiketleme

Azure Container Registry gibi bir özel kayıt defterine kapsayıcı görüntüsü göndermek için önce görüntüyü kayıt defterinin oturum açma sunucusunun tam adıyla etiketlemeniz gerekir.

İlk olarak Azure kapsayıcı kayıt defteriniz için tam oturum açma sunucu adını alın. Aşağıdaki [az acr show][az-acr-show] komutunu çalıştırın ve `<acrName>` değerini, yeni oluşturduğunuz kayıt defterinin adıyla değiştirin:

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Örneğin, kayıt defterinizin adı *mycontainerregistry082* ise:

```console
$ az acr show --name mycontainerregistry082 --query loginServer --output table
Result
------------------------
mycontainerregistry082.azurecr.io
```

Şimdi [docker images][docker-images] komutunu kullanarak yerel görüntülerinizin listesini görüntüleyin:

```bash
docker images
```

Makinenizdeki diğer görüntülerle birlikte, [önceki öğreticide](container-instances-tutorial-prepare-app.md) derlediğiniz *aci-tutorial-app* görüntüsünü görmeniz gerekir:

```console
$ docker images
REPOSITORY          TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app    latest    5c745774dfa9    39 minutes ago    68.1 MB
```

Kapsayıcı kayıt defterinizin loginServer’ı için *aci-tutorial-app* görüntüsünü etiketleyin. Ayrıca görüntü sürüm numarasını belirtmek için görüntü adının sonuna `:v1` etiketini ekleyin. `<acrLoginServer>` değerini, az önce çalıştırdığınız [az acr show][az-acr-show] komutunun sonucuyla değiştirin.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Etiketleme işlemini doğrulamak için tekrar `docker images` komutunu çalıştırın:

```console
$ docker images
REPOSITORY                                            TAG       IMAGE ID        CREATED           SIZE
aci-tutorial-app                                      latest    5c745774dfa9    39 minutes ago    68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app    v1        5c745774dfa9    7 minutes ago     68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

*aci-tutorial-app* görüntüsünü, özel kayıt defterinizin tam oturum açma sunucusu adıyla etiketlediğinize göre şimdi [docker push][docker-push] komutunu kullanarak bunu kayıt defterine gönderebilirsiniz. `<acrLoginServer>` değerini, önceki adımda aldığınız tam oturum açma sunucusu adıyla değiştirin.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` işlemi, internet bağlantınıza bağlı olarak birkaç saniye ile birkaç dakika arasında sürebilir ve çıktı şuna benzer:

```console
$ docker push mycontainerregistry082.azurecr.io/aci-tutorial-app:v1
The push refers to a repository [mycontainerregistry082.azurecr.io/aci-tutorial-app]
3db9cac20d49: Pushed
13f653351004: Pushed
4cd158165f4d: Pushed
d8fbd47558a8: Pushed
44ab46125c35: Pushed
5bef08742407: Pushed
v1: digest: sha256:ed67fff971da47175856505585dcd92d1270c3b37543e8afd46014d328f05715 size: 1576
```

## <a name="list-images-in-azure-container-registry"></a>Azure Container Registry’deki görüntüleri listeleme

Gönderdiğiniz görüntünün aslında Azure kapsayıcı kayıt defterinizde olduğunu doğrulamak için, [az acr repository list][az-acr-repository-list] komutunu kullanarak kayıt defterinizdeki görüntüleri listeleyin. `<acrName>` komutunu, kapsayıcı kayıt defterinizin adıyla değiştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Örneğin:

```console
$ az acr repository list --name mycontainerregistry082 --output table
Result
----------------
aci-tutorial-app
```

Belirli bir görüntünün *etiketlerini* görmek için, [az acr repository show-tags][az-acr-repository-show-tags] komutunu kullanın.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Aşağıdakine benzer bir çıktı görmeniz gerekir:

```console
$ az acr repository show-tags --name mycontainerregistry082 --repository aci-tutorial-app --output table
Result
--------
v1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container Instances ile kullanım için bir Azure kapsayıcı kayıt defteri hazırladınız ve bu kayıt defterine bir kapsayıcı görüntüsü gönderdiniz. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure Container Registry örneği dağıtıldı
> * Azure Container Registry için bir kapsayıcı görüntüsü etiketlendi
> * Azure Container Registry’ye görüntü yüklendi

Azure Container Instances kullanarak Azure’a kapsayıcıyı dağıtma hakkında bilgi edinmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Azure Container Instances‘a kapsayıcı dağıtma](container-instances-tutorial-deploy-app.md)

<!-- LINKS - External -->
[docker-build]: https://docs.docker.com/engine/reference/commandline/build/
[docker-get-started]: https://docs.docker.com/get-started/
[docker-hub-nodeimage]: https://store.docker.com/images/node
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/
[nodejs]: https://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-repository-list]: /cli/azure/acr/repository
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az-acr-repository-show-tags
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
