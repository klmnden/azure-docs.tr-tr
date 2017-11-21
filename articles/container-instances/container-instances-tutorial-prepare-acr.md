---
title: "Azure kapsayıcı örnekleri Öğreticisi - Azure kapsayıcı kayıt defteri hazırlama"
description: "Azure kapsayıcı örnekleri Öğreticisi - Azure kapsayıcı kayıt defteri hazırlama"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: mmacy
tags: acs, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 0beadcca2247fb64c03835c4cb1e1e4fb9426d6f
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Dağıtma ve Azure kapsayıcı kayıt defteri kullanma

Bu iki üç bölümlü öğretici parçasıdır. İçinde [önceki adımda](container-instances-tutorial-prepare-app.md), yazılan basit bir web uygulaması için bir kapsayıcı görüntüsü oluşturuldu [Node.js](http://nodejs.org). Bu öğreticide, Azure kapsayıcı kayıt defterine görüntü iletin. Kapsayıcı görüntü oluşturmadıysanız, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntü](container-instances-tutorial-prepare-app.md).

Azure kapsayıcı kayıt defteri Docker kapsayıcısı görüntüleri için bir Azure tabanlı, özel kayıt defteri ' dir. Bu öğreticide Azure kapsayıcı kayıt defteri örneğini dağıtma ve kapsayıcı görüntü için itme açıklanmaktadır. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneğini dağıtma
> * Azure kapsayıcı kayıt defteri için etiketleme kapsayıcı görüntüsü
> * Azure kapsayıcı kayıt defterine görüntüyü karşıya yükleme

Sonraki öğreticilerde, kapsayıcı Azure kapsayıcı örnekleri özel kayıt defterinden dağıtın.

## <a name="before-you-begin"></a>Başlamadan önce

Bu öğretici, Azure CLI Sürüm 2.0.21 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamı yerel bir yüklemesini öneririz.

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Bir Azure kaynak grubu hangi Azure kaynakları dağıtılan yönetilen ve mantıksal bir koleksiyonudur.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *eastus* bölge.

```azurecli
az group create --name myResourceGroup --location eastus
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. Kapsayıcı kayıt defteri adı **benzersiz olmalıdır** , azure'daki ve 5-50 alfasayısal karakterler içermelidir. Değiştir `<acrName>` kayıt için benzersiz bir ad ile:

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Örneğin, bir Azure kapsayıcı kayıt oluşturmak üzere adlı *mycontainerregistry082*:

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalanını, kullandığımız `<acrName>` seçtiğiniz kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

ACR örneğinizi görüntüleri göndermeden önce oturum gerekir. Kullanım [az acr oturum açma](/cli/azure/acr#az_acr_login) işlemi tamamlamak için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut döndürür bir `Login Succeeded` tamamlandıktan sonra ileti.

## <a name="tag-container-image"></a>Etiket kapsayıcı görüntüsü

Özel bir kayıt defteri kapsayıcı görüntü dağıtmak için görüntü ile etiketlenmesi gereken `loginServer` kayıt adı.

Geçerli görüntüleri listesini görmek için `docker images` komutu.

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                   TAG                 IMAGE ID            CREATED              SIZE
aci-tutorial-app             latest              5c745774dfa9        39 seconds ago       68.1 MB
```

LoginServer adını almak için aşağıdaki komutu çalıştırın. Değiştir `<acrName>` kapsayıcı kayıt adı.

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Örnek çıktı:

```
Result
------------------------
mycontainerregistry082.azurecr.io
```

Etiket *aci öğretici uygulama* kapsayıcı kaydınız loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket görüntü sürüm numarasını gösterir. Değiştir `<acrLoginServer>` sonucu ile `az acr show` yalnızca yürütülen komutu.

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

Anında *aci öğretici uygulama* kayıt defteri ile görüntüye `docker push` komutu. Değiştir `<acrLoginServer>` tam oturum açma sunucu adıyla önceki adımda elde.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
```

`push` İşlemi birkaç saniye Internet bağlantınızı bağlı olarak birkaç dakika ile almalıdır ve çıktı aşağıdakine benzer:

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

Azure kapsayıcı kaydınız gönderilen görüntüleri listesini döndürmek için kullanın [az acr deposu listesi](/cli/azure/acr/repository#list) komutu. Komut kapsayıcı kayıt defteri adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
aci-tutorial-app
```

Ve ardından belirli bir resim için etiketleri görmek için [az acr deposunu Göster-etiketleri](/cli/azure/acr/repository#show-tags) komutu.

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

Bu öğreticide Azure kapsayıcı örnekleri ile kullanmak için bir Azure kapsayıcı kayıt defteri hazırlanan ve kapsayıcı görüntü gönderilir. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneği dağıtılan
> * Azure kapsayıcı kayıt için bir kapsayıcı görüntüsü etiketli
> * Azure kapsayıcı kayıt defterine görüntüyü karşıya

Azure kapsayıcı örneği kullanarak Azure kapsayıcı dağıtma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı örnekleri kapsayıcıları dağıtın](./container-instances-tutorial-deploy-app.md)
