---
title: "Azure Container Instances öğreticisi - Azure Container Registry’i Hazırlama"
description: "Azure Container Instances öğreticisi, 2/3. Bölüm - Azure Container Registry’i Hazırlama"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 94ecba44b8281460da4518c146aab814d2eaa850
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Azure Container Registry’yi dağıtma ve kullanma

Bu öğretici, üç bölümden oluşan bir serinin ikinci bölümüdür. [Önceki adımda](container-instances-tutorial-prepare-app.md), [Node.js][nodejs] dilinde yazılan basit bir web uygulaması için kapsayıcı görüntüsü oluşturmuştuk. Bu öğreticide, görüntüyü Azure Container Registry’e göndereceğiz. Kapsayıcı görüntüsünü oluşturmadıysanız [Öğretici 1 - Kapsayıcı görüntüsü oluştur](container-instances-tutorial-prepare-app.md)’a dönün.

Azure Container Registry, Docker kapsayıcı görüntüleri için Azure tabanlı özel bir kayıt defteridir. Bu öğreticide Azure Container Registry örneği dağıtma ve bu örneğe kapsayıcı görüntüsü göndermeyle ilgili bilgi verilmektedir.

Serinin ikinci bölümündeki bu makalede şunları yapacaksınız:

> [!div class="checklist"]
> * Azure Container Registry örneği dağıtacaksınız
> * Azure kapsayıcı kayıt defteriniz için bir kapsayıcı görüntüsü etiketleyeceksiniz
> * Görüntüyü kayıt defterinize yükleyeceksiniz

Serinin son öğreticisi olan sonraki makalede, özel kayıt defterinizdeki kapsayıcıyı Azure Container Instances’da dağıtacaksınız.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici için Azure CLI 2.0.23 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğreticiyi tamamlamak için yerel olarak yüklü bir Docker geliştirme ortamı gerekir. Docker [Mac][docker-mac], [Windows][docker-windows] veya [Linux][docker-linux]'ta Docker'ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure Cloud Shell, bu öğreticideki her adımı tamamlamak için gerekli olan Docker bileşenlerini içermez. Bu öğreticiyi tamamlamak için, yerel bilgisayarınıza Azure CLI ve Docker geliştirme ortamını yüklemeniz gerekir.

## <a name="deploy-azure-container-registry"></a>Azure Container Registry’yi dağıtma

Bir Azure Container Registry dağıtırken önce bir kaynak grubuna ihtiyaç duyarsınız. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Bu örnekte, *eastus* bölgesinde *myResourceGroup* adlı bir kaynak grubu oluşturulur.

```azurecli
az group create --name myResourceGroup --location eastus
```

[az acr create][az-acr-create] komutuyla Azure kapsayıcı kayıt defteri oluşturun. Kapsayıcı kayıt defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir. `<acrName>` değerini kayıt defteriniz için benzersiz bir adla değiştirin:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Örneğin, *mycontainerregistry082* adlı bir Azure kapsayıcı kayıt defteri oluşturmak için:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalan aşamalarında, seçtiğiniz kapsayıcı kayıt defteri adı için yer tutucu olarak `<acrName>` kullanacağız.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defterinde oturum açma

Görüntü göndermeden önce, Azure Container Registry örneğinizde oturum açmanız gerekir. İşlemi tamamlamak için [az acr login][az-acr-login] komutunu kullanın. Oluşturduğunuzda, kapsayıcı kayıt defteri için sağladığınız benzersiz adı kullanmanız gerekir.

```azurecli
az acr login --name <acrName>
```

Bu komut tamamlandığında `Login Succeeded` iletisi döndürülür.

## <a name="tag-container-image"></a>Kapsayıcı görüntüsünü etiketleme

Özel bir kayıt defterine yönelik kapsayıcı görüntüsünü dağıtmak için, görüntüyü kapsayıcının `loginServer` adıyla etiketlemelisiniz.

Mevcut görüntülerin listesini görüntülemek için [docker images][docker-images] komutunu kullanın.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

loginServer adını almak için, [az acr show][az-acr-show] komutunu çalıştırın. `<acrName>` komutunu, kapsayıcı kayıt defterinizin adıyla değiştirin.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Örnek çıktı:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Kapsayıcı kayıt defterinizin loginServer’ı için *aci-tutorial-app* görüntüsünü etiketleyin. Ayrıca, görüntü adının sonuna `:v1` ekleyin. Bu etiket, görüntü sürüm numarasını belirtir. `<acrLoginServer>` değerini, az önce çalıştırdığınız [az acr show][az-acr-show] komutunun sonucuyla değiştirin.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Etiketledikten sonra, işlemi doğrulamak için `docker images` komutunu çalıştırın.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Azure Container Registry’ye görüntü gönderme

[docker push][docker-push] komutuyla *aci-tutorial-app* görüntüsünü kayıt defterine gönderin. `<acrLoginServer>` değerini, önceki adımda öğrendiğiniz tam oturum açma sunucusu adıyla değiştirin.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` işlemi, internet bağlantınıza bağlı olarak birkaç saniye ile birkaç dakika arasında sürebilir ve çıktı şuna benzer:

```bash
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

Azure Container Registry’nize gönderilen görüntülerin listesini döndürmek için [az acr repository list][az-acr-repository-list] komutunu kullanın. Komutu, kapsayıcı kayıt defteri adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
aci-tutorial-app
```

Sonra, belirli bir görüntünün etiketlerini görmek için [az acr repository show-tags][az-acr-repository-show-tags] komutunu kullanın.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-tutorial-app --output table
```

Çıktı:

```azurecli
Result
--------
v1
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Container Instances ile kullanım için bir Azure Container Registry oluşturdunuz ve bu kayıt defterine bir kapsayıcı görüntüsü gönderdiniz. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure Container Registry örneği dağıtıldı
> * Azure Container Registry için bir kapsayıcı görüntüsü etiketlendi
> * Azure Container Registry’ye görüntü yüklendi

Azure Container Instances kullanarak Azure’da kapsayıcı dağıtma hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Container Instances’da kapsayıcı dağıtma](./container-instances-tutorial-deploy-app.md)

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
[nodejs]: http://nodejs.org

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-acr-repository-list]: /cli/azure/acr/repository#az_acr_list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#az_acr_repository_show_tags
[az-acr-show]: /cli/azure/acr#az_acr_show
[az-group-create]: /cli/azure/group#az_group_create
[azure-cli-install]: /cli/azure/install-azure-cli
