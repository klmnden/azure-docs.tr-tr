---
title: "Azure kapsayıcı örnekleri Öğreticisi - Azure kapsayıcı kayıt defteri hazırlama"
description: "Azure kapsayıcı örnekleri öğretici Kısım 2 / 3 - Azure kapsayıcı kayıt defteri hazırlama"
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0aad1f9bbaac9a456b34f75633faba92f57f498
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="deploy-and-use-azure-container-registry"></a>Dağıtma ve Azure kapsayıcı kayıt defteri kullanma

Bu iki üç bölümlü öğretici parçasıdır. İçinde [önceki adımda](container-instances-tutorial-prepare-app.md), yazılan basit bir web uygulaması için bir kapsayıcı görüntüsü oluşturuldu [Node.js][nodejs]. Bu öğreticide, Azure kapsayıcı kayıt defterine görüntü iletin. Kapsayıcı görüntü oluşturmadıysanız, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntü](container-instances-tutorial-prepare-app.md).

Azure kapsayıcı kayıt defteri Azure tabanlı, özel bir kayıt defteri Docker kapsayıcısı görüntüleri ' dir. Bu öğreticide, Azure kapsayıcı kayıt defteri örneğini dağıtma ve kapsayıcı görüntü için itme aracılığıyla açıklanmaktadır.

Bu makalede, iki serinin Kısım:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneğini dağıtma
> * Azure kapsayıcı kayıt için bir kapsayıcı görüntü etiketi
> * Kayıt defterine görüntüyü karşıya yükleme

Sonraki makalede, serideki son Öğreticisi, kapsayıcı özel kayıt defterinden Azure kapsayıcı örnekleri dağıtın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.23 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Gerekirse yükleyin veya yükseltin, bakın [Azure CLI 2.0 yükleme][azure-cli-install].

Bu öğreticiyi tamamlamak için yerel olarak yüklenmiş bir Docker geliştirme ortamı gerekir. Docker sağlar kolayca Docker herhangi yapılandırdığınız paketler [Mac][docker-mac], [Windows][docker-windows], veya [Linux] [ docker-linux] sistem.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu öğreticiyi tamamlamak için yerel bilgisayarınızda Azure CLI ve Docker geliştirme ortamı yüklemeniz gerekir.

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Bir Azure kaynak grubu hangi Azure kaynakları dağıtılan yönetilen ve mantıksal bir koleksiyonudur.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *eastus* bölge.

```azurecli
az group create --name myResourceGroup --location eastus
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturma] [ az-acr-create] komutu. Kapsayıcı kayıt defteri adı **benzersiz olmalıdır** , azure'daki ve 5-50 alfasayısal karakterler içermelidir. Değiştir `<acrName>` kayıt için benzersiz bir ad ile:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Örneğin, bir Azure kapsayıcı kayıt oluşturmak üzere adlı *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalanını, kullandığımız `<acrName>` seçtiğiniz kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

Azure kapsayıcı kayıt defteri örneğinizi görüntüleri göndermeden önce oturum gerekir. Kullanım [az acr oturum açma] [ az-acr-login] işlemi tamamlamak için komutu. Oluşturduğunuz sırada kapsayıcı kayıt defteri sağlanan benzersiz adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut döndürür bir `Login Succeeded` tamamlandıktan sonra ileti.

## <a name="tag-container-image"></a>Etiket kapsayıcı görüntüsü

Özel bir kayıt defteri kapsayıcı görüntü dağıtmak için olan görüntü etiketi `loginServer` kayıt adı.

Geçerli görüntüleri listesini görmek için [docker görüntüleri] [ docker-images] komutu.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

LoginServer adını almak için Çalıştır [az acr Göster] [ az-acr-show] komutu. Değiştir `<acrName>` kapsayıcı kayıt adı.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Örnek çıktı:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Etiket *aci öğretici uygulama* kapsayıcı kaydınız loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket görüntü sürüm numarasını gösterir. Değiştir `<acrLoginServer>` sonucu ile [az acr Göster] [ az-acr-show] yalnızca yürütülen komutu.

```bash
docker tag aci-tutorial-app <acrLoginServer>/aci-tutorial-app:v1
```

Etiketlenmiş bir kez çalıştır `docker images` işlemi doğrulamak için.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                                TAG                 IMAGE ID            CREATED             SIZE
aci-tutorial-app                                          latest              5c745774dfa9        39 seconds ago      68.1 MB
mycontainerregistry082.azurecr.io/aci-tutorial-app        v1                  a9dace4e1a17        7 minutes ago       68.1 MB
```

## <a name="push-image-to-azure-container-registry"></a>Azure kapsayıcı kayıt defterine görüntü gönderme

Anında iletme *aci öğretici uygulama* kayıt defteri ile görüntüye [docker itme] [ docker-push] komutu. Değiştir `<acrLoginServer>` tam oturum açma sunucu adıyla önceki adımda elde.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` İşlemi birkaç saniye internet bağlantınızı bağlı olarak birkaç dakika ile almalıdır ve çıktı aşağıdakine benzer:

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

## <a name="list-images-in-azure-container-registry"></a>Azure kapsayıcı kayıt defterinde listesi görüntüler

Azure kapsayıcı kaydınız gönderilen görüntüleri listesini döndürmek için kullanın [az acr deposu listesi] [ az-acr-repository-list] komutu. Komut kapsayıcı kayıt defteri adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
aci-tutorial-app
```

Ve ardından belirli bir resim için etiketleri görmek için [az acr deposunu Göster-etiketleri] [ az-acr-repository-show-tags] komutu.

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

Bu öğreticide Azure kapsayıcı örnekleri ile kullanmak için bir Azure kapsayıcı kayıt defteri hazır ve kayıt defterine bir kapsayıcı görüntüsü gönderilir. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneği dağıtılan
> * Azure kapsayıcı kayıt için bir kapsayıcı görüntüsü etiketli
> * Azure kapsayıcı kayıt defterine görüntüyü karşıya

Azure kapsayıcı örneği kullanarak Azure kapsayıcı dağıtma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri kapsayıcıları dağıtın](./container-instances-tutorial-deploy-app.md)

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
