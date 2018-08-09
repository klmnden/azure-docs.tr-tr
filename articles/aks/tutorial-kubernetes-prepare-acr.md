---
title: Azure’da Kubernetes öğreticisi - ACR Hazırlama
description: AKS öğreticisi - ACR Hazırlama
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 4ad5dcb8dbb11f1d6e12e3c19eab5da68009df58
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39430765"
---
# <a name="tutorial-deploy-and-use-azure-container-registry"></a>Öğretici: Azure Container Registry’yi dağıtma ve kullanma

Azure Container Registry (ACR), Docker kapsayıcı görüntüleri için Azure tabanlı özel bir kayıt defteridir. Yedi öğreticiden oluşan bu serinin ikinci kısmında, Azure Container Registry örneği dağıtma ve bu örneğe kapsayıcı görüntüsü göndermeyle ilgili bilgi verilmektedir. Tamamlanan adımlar:

> [!div class="checklist"]
> * Azure Container Registry (ACR) örneği dağıtma
> * ACR için kapsayıcı görüntüsü etiketleme
> * Görüntüyü ACR’ye yükleme

Sonraki öğreticilerde, bu ACR örneği AKS’deki Kubernetes kümesiyle tümleştirilecektir.

## <a name="before-you-begin"></a>Başlamadan önce

[Önceki öğreticide][aks-tutorial-prepare-app], basit bir Azure Voting uygulaması için kapsayıcı görüntüsü oluşturulacaktır. Azure Voting uygulaması görüntüsünü oluşturmadıysanız [Öğretici 1 - Kapsayıcı görüntüleri oluştur][aks-tutorial-prepare-app]’a dönün.

Bu öğretici için Azure CLI 2.0.27 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="deploy-azure-container-registry"></a>Azure Container Registry’yi dağıtma

Bir Azure Container Registry dağıtırken önce bir kaynak grubuna ihtiyaç duyarsınız. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun. Bu örnekte, `eastus`bölgesinde `myResourceGroup` adlı bir kaynak grubu oluşturulur.

```azurecli
az group create --name myResourceGroup --location eastus
```

[az acr create][az-acr-create] komutuyla Azure Container kayıt defteri oluşturun. Kaynak defteri adı Azure’da benzersiz olmalı ve 5-50 arası alfasayısal karakter içermelidir.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bu öğreticinin geri kalan aşamalarında, kapsayıcı kayıt defteri adı için yer tutucu olarak `<acrName>` kullanacağız.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defterinde oturum açma

ACR örneğinde oturum açmak için [az acr login][az-acr-login] komutunu kullanın. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandığında bir “Oturum Başarıyla Açıldı” iletisi döndürür.

## <a name="tag-container-images"></a>Kapsayıcı görüntülerini etiketleme

Mevcut görüntülerin listesini görüntülemek için [docker images][docker-images] komutunu kullanın.

```console
docker images
```

Çıktı:

```
REPOSITORY                   TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front             latest              4675398c9172        13 minutes ago      694MB
redis                        latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask   flask               788ca94b2313        9 months ago        694MB
```

Her kapsayıcı görüntüsünün, kayıt defterinin loginServer adıyla etiketlenmesi gerekir. Bu etiket, görüntü kayıt defterine kapsayıcı görüntüleri gönderilirken kullanılır.

loginServer adını almak için [az acr list][az-acr-list] komutunu kullanın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Şimdi, `azure-vote-front` değerini, kapsayıcı kayıt defterinin loginServer’ıyla etiketleyin. Ayrıca, görüntü adının sonuna `:v1` ekleyin. Bu etiket, görüntü sürümünü belirtir.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:v1
```

Etiketledikten sonra, işlemi doğrulamak için [docker images][docker-images] komutunu çalıştırın.

```console
docker images
```

Çıktı:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Kayıt defterine görüntü gönderme

`azure-vote-front` görüntüsünü kayıt defterinize gönderin.

Aşağıdaki örneği kullanarak, ortamınızda, ACR loginServer adını loginServer olarak değiştirin.

```console
docker push <acrLoginServer>/azure-vote-front:v1
```

Bu işlemin tamamlanması birkaç dakika sürer.

## <a name="list-images-in-registry"></a>Kayıt defterindeki görüntüleri listeleme

Azure Container Registry’nize gönderilen görüntülerin listesini döndürmek için [az acr repository list][az-acr-repository-list] komutunu kullanın. Komutu ACR örneği adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
azure-vote-front
```

Sonra, belirli bir görüntünün etiketlerini görmek için [az acr repository show-tags][az-acr-repository-show-tags] komutunu kullanın.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Çıktı:

```azurecli
Result
--------
v1
```

Öğretici tamamlandığında, kapsayıcı görüntüsü özel bir Azure Container Registry örneğinde depolanır. Sonraki öğreticilerde, bu görüntüyü ACR’den bir Kubernetes kümesine dağıtma işlemi açıklanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, AKS kümesinde kullanılmak üzere bir Azure Container Registry hazırlanmıştır. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure Container Registry örneği dağıtıldı
> * ACR için kapsayıcı görüntüsü etiketlendi
> * Görüntü ACR’ye yüklendi

Azure’da Kubernetes kümesi dağıtmayla ilgili daha fazla bilgi edinmek için sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [Kubernetes kümesi dağıtma][aks-tutorial-deploy-cluster]

<!-- LINKS - external -->
[docker-images]: https://docs.docker.com/engine/reference/commandline/images/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#create
[az-acr-list]: /cli/azure/acr#list
[az-acr-login]: https://docs.microsoft.com/cli/azure/acr#az-acr-login
[az-acr-list]: https://docs.microsoft.com/cli/azure/acr#az-acr-list
[az-acr-repository-list]: /cli/azure/acr/repository#list
[az-acr-repository-show-tags]: /cli/azure/acr/repository#show-tags
[az-group-create]: /cli/azure/group#az-group-create
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-tutorial-deploy-cluster]: ./tutorial-kubernetes-deploy-cluster.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
