---
title: "Azure kapsayıcı hizmeti Öğreticisi - ACR hazırlama"
description: "Azure kapsayıcı hizmeti Öğreticisi - ACR hazırlama"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 09/14/2017
ms.author: nepeters
ms.custom: mvc
<<<<<<< HEAD
ms.openlocfilehash: 4bca5e2522c091b5d8045fd9738b438156ff07f9
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: HT
=======
ms.openlocfilehash: c9c8ad6dfd6df0e99f9e41eaf1da12ebeb2a2da6
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="deploy-and-use-azure-container-registry"></a>Dağıtma ve Azure kapsayıcı kayıt defteri kullanma

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Azure kapsayıcı kayıt defteri (ACR) Docker kapsayıcısı görüntüleri için bir Azure tabanlı, özel kayıt defteri ' dir. Bu öğretici, bölümü yedi, iki kılavuzluk Azure kapsayıcı kayıt defteri örneğini dağıtma ve kapsayıcı görüntü için iletme. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri (ACR) örneğini dağıtma
> * ACR için bir kapsayıcı görüntüsü etiketleme
> * Görüntü ACR için karşıya yükleme

Sonraki öğreticilerde bu ACR örnek bir Azure kapsayıcı hizmeti Kubernetes kümesi ile tümleşiktir. 

## <a name="before-you-begin"></a>Başlamadan önce

İçinde [önceki öğretici](./container-service-tutorial-kubernetes-prepare-app.md), basit bir Azure oylama uygulaması için bir kapsayıcı görüntüsü oluşturuldu. Azure oylama uygulama görüntüsü oluşturmadıysanız, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./container-service-tutorial-kubernetes-prepare-app.md).

Bu öğretici, Azure CLI Sürüm 2.0.4 çalıştırmasını gerektirir veya sonraki bir sürümü. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI 2.0 yükleme]( /cli/azure/install-azure-cli). 

## <a name="deploy-azure-container-registry"></a>Azure kapsayıcı kayıt defteri dağıtın

Azure kapsayıcı kayıt defteri dağıtırken, önce bir kaynak grubu gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#create) komutuyla bir kaynak grubu oluşturun. Bu örnekte, bir kaynak grubu adında `myResourceGroup` oluşturulan `westeurope` bölge.

```azurecli
az group create --name myResourceGroup --location westeurope
```

Azure kapsayıcı kayıt defteri ile oluşturma [az acr oluşturmak](/cli/azure/acr#create) komutu. Bir kapsayıcı kayıt defteri adını **benzersiz olmalıdır**.

```azurecli
az acr create --resource-group myResourceGroup --name <acrName> --sku Basic
```

Bu öğreticinin geri kalanını, kullandığımız `<acrname>` kapsayıcı kayıt defteri adı için bir yer tutucu olarak.

## <a name="container-registry-login"></a>Kapsayıcı kayıt defteri oturum açma

Kullanım [az acr oturum açma](https://docs.microsoft.com/cli/azure/acr#az_acr_login) ACR örneğinde oturum açma için komutu. Kapsayıcı kayıt defterine oluşturulduğunda verilen benzersiz bir ad vermeniz gerekir.

```azurecli
az acr login --name <acrName>
```

Komut tamamlandıktan sonra 'Başarılı oturum açma' iletisi döndürür.

## <a name="tag-container-images"></a>Etiket kapsayıcı görüntüleri

Geçerli görüntüleri listesini görmek için [docker görüntüleri](https://docs.docker.com/engine/reference/commandline/images/) komutu.

```bash
docker images
```

Çıktı:

```bash
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

```bash
docker tag azure-vote-front <acrLoginServer>/azure-vote-front:redis-v1
```

Etiketli sonra [docker görüntüler] çalıştırma işlemi doğrulamak için (https://docs.docker.com/engine/reference/commandline/images/).

```bash
docker images
```

Çıktı:

```bash
REPOSITORY                                           TAG                 IMAGE ID            CREATED             SIZE
azure-vote-front                                     latest              eaf2b9c57e5e        8 minutes ago       716 MB
mycontainerregistry082.azurecr.io/azure-vote-front   redis-v1            eaf2b9c57e5e        8 minutes ago       716 MB
redis                                                latest              a1b99da73d05        7 days ago          106MB
tiangolo/uwsgi-nginx-flask                           flask               788ca94b2313        8 months ago        694 MB
```

## <a name="push-images-to-registry"></a>Kayıt defteri itme görüntüleri

Anında iletme `azure-vote-front` kayıt defterine görüntü. 

Aşağıdaki örneği kullanarak, ortamınızdan loginServer ACR loginServer adını değiştirin.

```bash
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

Bu öğreticide, bir ACS Kubernetes kümesinde kullanmak için bir Azure kapsayıcı kayıt defteri hazırlanan. Aşağıdaki adımlar tamamlandı:

> [!div class="checklist"]
> * Azure kapsayıcı kayıt defteri örneği dağıtılan
> * ACR için bir kapsayıcı görüntüsü etiketli
> * ACR için görüntüyü karşıya

Azure Kubernetes kümede dağıtma hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes kümesi dağıtma](./container-service-tutorial-kubernetes-deploy-cluster.md)