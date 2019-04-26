---
title: (KULLANIM DIŞI) Azure Container Service Öğreticisi - uygulamayı dağıtma
description: Azure Container Service öğreticisi - Uygulama Dağıtma
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 02/26/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: dafbb8d1221d5e9c6194611ad338b3714a089cea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60309241"
---
# <a name="deprecated-run-applications-in-kubernetes"></a>(KULLANIM DIŞI) Kubernetes'te uygulamaları çalıştırma

> [!TIP]
> Azure Kubernetes hizmeti kullanan Bu öğretici için güncelleştirilmiş sürümü görmek [Öğreticisi: Azure Kubernetes Service (AKS) uygulamaları çalıştırma](../../aks/tutorial-kubernetes-deploy-application.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

Yedi bölümün dördüncüsü olan bu öğreticide Kubernetes kümesine örnek bir uygulama dağıtılır. Tamamlanan adımlar:

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını güncelleştirme
> * Kubernetes'te uygulama çalıştırma
> * Uygulamayı test etme

Sonraki öğreticilerde bu uygulamanın ölçeği genişletilir, uygulama güncelleştirilir ve Log Analytics, Kubernetes kümesini izlemek için yapılandırılır.

Bu öğreticide temel Kubernetes kavramlarını bildiğiniz varsayılmıştır. Kubernetes hakkında ayrıntılı bilgi için bkz. [Kubernetes belgeleri](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. 

Bu öğreticiyi tamamlamak için önceden oluşturulmuş `azure-vote-all-in-one-redis.yml` Kubernetes bildirim dosyasına ihtiyaç duyarsınız. Bu dosya, önceki öğreticide uygulama kaynak koduyla indirildi. Deponun bir kopyasını oluşturduğunuzu ve dizinleri kopyalanmış depoya göre değiştirdiğinizi doğrulayın.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma](./container-service-tutorial-kubernetes-prepare-app.md) konusuna dönün. 

## <a name="update-manifest-file"></a>Bildirim dosyasını güncelleştirme

Bu öğreticide kapsayıcı görüntüsü depolamak için Azure Container Registry (ACR) kullanılmıştır. Uygulamayı çalıştırmadan önce, ACR oturum açma sunucusu adının Kubernetes bildirim dosyasında güncelleştirilmesi gerekir.

[az acr list](/cli/azure/acr#az-acr-list) komutuyla ACR oturum açma sunucusu adını alın.

```azurecli-interactive
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Bildirim dosyası, `microsoft` oturum açma sunucusu adıyla önceden oluşturulmuştur. Dosyayı herhangi bir metin düzenleyici ile açın. Bu örnekte, dosya `vi` ile açılır.

```bash
vi azure-vote-all-in-one-redis.yml
```

ACR oturum açma sunucu adını `microsoft` ile değiştirin. Bu değer, bildirim dosyasının **47**. satırında bulunur.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:v1
```

Dosyayı kaydedin ve kapatın.

## <a name="deploy-application"></a>Uygulamayı dağıtma

Uygulamayı çalıştırmak için [kubectl create](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create) komutunu kullanın. Bu komut, bildirim dosyasını ayrıştırır ve tanımlanmış Kubernetes nesnelerini oluşturur.

```azurecli-interactive
kubectl create -f azure-vote-all-in-one-redis.yml
```

Çıkış:

```bash
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Uygulamayı test etme

Uygulamayı internette kullanıma sunan [Kubernetes hizmeti](https://kubernetes.io/docs/concepts/services-networking/service/) oluşuturulur. Bu işlem birkaç dakika sürebilir. 

İlerleme durumunu izlemek için [kubectl get service](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli-interactive
kubectl get service azure-vote-front --watch
```

Başlangıçta `azure-vote-front` için **EXTERNAL-IP** durumu `pending` olarak görünür. EXTERNAL-IP adresi `pending` durumundan `IP address` değerine değiştiğinde kubectl izleme işlemini durdurmak için `CTRL-C` komutunu kullanın.

```bash
NAME               CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
azure-vote-front   10.0.42.158   <pending>     80:31873/TCP   1m
azure-vote-front   10.0.42.158   52.179.23.131 80:31873/TCP   2m
```

Uygulamayı görmek için dış IP adresine gözatın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Vote uygulaması, bir Azure Container Service Kubernetes kümesine dağıtılmıştır. Tamamlanan görevler şunları içerir:  

> [!div class="checklist"]
> * Kubernetes bildirim dosyalarını indirme
> * Uygulamayı Kubernetes'te çalıştırma
> * Uygulamayı test etme

Hem bir Kubernetes uygulamasını hem de temel alınan Kubernetes altyapısını ölçeklendirme hakkında daha fazla bilgi için sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Kubernetes uygulamasını ve altyapısını ölçeklendirme](./container-service-tutorial-kubernetes-scale.md)