---
title: "Azure Öğreticisi - hazırlama ACR üzerinde Kubernetes | Microsoft Docs"
description: "AKS Öğreticisi - ACR hazırlama"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1848e15a2be8d89315657a6eabdb94617bd1b5bf
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Dağıtma ve Azure kapsayıcı kayıt defteri kullanma

Azure kapsayıcı kayıt defteri (ACR) Docker kapsayıcısı görüntüleri için bir Azure tabanlı, özel kayıt defteri ' dir. Bu Eğitmeni, bölüm iki sekiz, Azure kapsayıcı kayıt defteri örneğini dağıtma ve kapsayıcı görüntü için itme kılavuzluk. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri (ACR) örneğini dağıtma
> * ACR için bir kapsayıcı görüntüsü etiketleme
> * Görüntü ACR için karşıya yükleme

Sonraki öğreticilerde bu ACR örneği AKS Kubernetes kümede tümleşiktir.

## <a name="before-you-begin"></a>Başlamadan önce

İçinde [önceki öğretici](./tutorial-kubernetes-prepare-app.md), basit bir Azure oylama uygulaması için bir kapsayıcı görüntüsü oluşturuldu. Azure oylama uygulama görüntüsü oluşturmadıysanız, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./tutorial-kubernetes-prepare-app.md).

Bu öğretici, Azure CLI Sürüm 2.0.21 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme]( /cli/azure/install-azure-cli).

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında `myResourceGroup` oluşturulan `eastus` bölge.

```azurecli
az group create --name myResourceGroup --location eastus
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. Bir kapsayıcı kayıt defteri adını **benzersiz olmalıdır**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bu öğreticinin geri kalanını, kullandığımız `<acrName>` kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

Kullanım [az acr oturum açma](https://docs.microsoft.com/cli/azure/acr#az_acr_login) ACR örneğinde oturum açma için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir ad vermeniz gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandıktan sonra 'Başarılı oturum açma' iletisi döndürür.

## <a name="tag-container-images"></a>Etiket kapsayıcı görüntüleri

Geçerli görüntüleri listesini görmek için [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) komutu.

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

Her kapsayıcı görüntü kayıt loginServer adıyla etiketlenmesi gerekiyor. Bu etiket kapsayıcı görüntüleri bir görüntü kayıt defterine Ftp'den zaman yönlendirme için kullanılır.

LoginServer adını almak için aşağıdaki komutu çalıştırın.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Şimdi, etiket `azure-vote-front` kapsayıcı kayıt defteri loginServer görüntüsüyle. Ayrıca, ekleme `:redis-v1` sonuna kadar görüntü adı. Bu etiket resim sürümünü gösterir.

```console
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Etiketli sonra [docker görüntüler] çalıştırma işlemi doğrulamak için (https://docs.docker.com/engine/reference/commandline/images/).

```console
docker images
```

Çıktı:

```
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Kayıt defteri itme görüntüleri

Anında iletme `azure-vote-front` kayıt defterine görüntü.

Aşağıdaki örneği kullanarak, ortamınızdan loginServer ACR loginServer adını değiştirin.

```console
docker push <acrLoginServer>/azure-vote-front:redis-v1
```

Bu, birkaç tamamlamak için dakika sürer.

## <a name="list-images-in-registry"></a>Kayıt defterinde listesi görüntüler

Azure kapsayıcı kaydınız gönderilen görüntüleri listesini döndürmek için kullanıcı [az acr deposu listesi](/cli/azure/acr/repository#list) komutu. Komut ACR örnek adıyla güncelleştirin.

```azurecli
az acr repository list --name <acrName> --output table
```

Çıktı:

```azurecli
Result
----------------
azure-vote-front
```

Ve ardından belirli bir resim için etiketleri görmek için [az acr deposunu Göster-etiketleri](/cli/azure/acr/repository#show-tags) komutu.

```azurecli
az acr repository show-tags --name <acrName> --repository azure-vote-front --output table
```

Çıktı:

```azurecli
Result
--------
redis-v1
```

Eğitmen tamamlandığında, kapsayıcı görüntünün bir özel Azure kapsayıcı kayıt defteri örneğinde zamandır depolanmış. Bu görüntü ACR sonraki öğreticilerde Kubernetes kümeye dağıtılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure kapsayıcı kayıt defteri AKS kümesinde kullanmak için hazırlanan. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneği dağıtılan
> * ACR için bir kapsayıcı görüntüsü etiketli
> * ACR için görüntüyü karşıya

Azure Kubernetes kümede dağıtma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes kümesi dağıtma](./tutorial-kubernetes-deploy-cluster.md)
