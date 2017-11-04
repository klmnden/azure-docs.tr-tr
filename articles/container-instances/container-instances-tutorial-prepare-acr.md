---
title: "Azure kapsayıcı örnekleri Öğreticisi - Azure kapsayıcı kayıt defteri hazırlama"
description: "Azure kapsayıcı örnekleri Öğreticisi - Azure kapsayıcı kayıt defteri hazırlama"
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/26/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 8cb00210ee260383d546be4faf141c133661156b
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2017
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

Bu öğretici, Azure CLI Sürüm 2.0.20 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme](/cli/azure/install-azure-cli).

Bu öğreticiyi tamamlamak için Docker geliştirme ortamı gerekir. Docker, [Mac](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/) veya [Linux](https://docs.docker.com/engine/installation/#supported-platforms)’ta Docker’ı kolayca yapılandırmanızı sağlayan paketler sağlar.

Azure bulut Kabuk her adımı tamamlamak için gereken Docker bileşenleri Bu öğretici içermez. Bu nedenle, Azure CLI ve Docker geliştirme ortamı yerel bir yüklemesini öneririz.

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Bir Azure kaynak grubu hangi Azure kaynakları dağıtılan yönetilen ve mantıksal bir koleksiyonudur.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında *myResourceGroup* oluşturulan *eastus* bölge.

```azurecli
az group create --name myResourceGroup --location eastus
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. Bir kapsayıcı kayıt defteri adını **benzersiz olmalıdır**. Aşağıdaki örnekte, kullanırız adı *mycontainerregistry082*.

```azurecli
az acr create --resource-group myResourceGroup --name mycontainerregistry082 --sku Basic --admin-enabled true
```

Bu öğreticinin geri kalanını, kullandığımız `<acrname>` seçtiğiniz kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

ACR örneğinizi görüntüleri göndermeden önce oturum gerekir. Kullanım [az acr oturum açma](/cli/azure/acr#az_acr_login) işlemi tamamlamak için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz adı sağlamanız gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandıktan sonra 'Başarılı oturum açma' iletisi döndürür.

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

LoginServer adını almak için aşağıdaki komutu çalıştırın:

```azurecli
az acr show --name <acrName> --query loginServer --output table
```

Etiket *aci öğretici uygulama* kapsayıcı kayıt defteri loginServer görüntüsüyle. Ayrıca, ekleme `:v1` sonuna kadar görüntü adı. Bu etiket görüntü sürüm numarasını gösterir.

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

Anında *aci öğretici uygulama* kayıt defterine görüntü.

Aşağıdaki örneği kullanarak, ortamınızdan loginServer kapsayıcı kayıt defteri loginServer adını değiştirin.

```bash
docker push <acrLoginServer>/aci-tutorial-app:v1
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
