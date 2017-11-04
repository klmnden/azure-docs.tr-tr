---
title: "Azure Öğreticisi - dağıtmak uygulama üzerinde Kubernetes | Microsoft Docs"
description: "AKS Öğreticisi - uygulama dağıtma"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Docker, Container’lar, Mikro hizmetler, Kumernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 73b943709fb3846058d4b1c09bc76b09460855ed
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2017
---
# <a name="run-applications-in-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) uygulamaları çalıştırma

Bu öğreticide sekiz dördünü kısım, örnek bir uygulama Kubernetes kümesine dağıtılır. Tamamlanan adımları içerir:

> [!div class="checklist"]
> * Güncelleştirme Kubernetes bildirim dosyaları
> * Kubernetes Çalıştırma uygulaması
> * Uygulamayı test etme

Sonraki öğreticilerde, bu uygulama, güncelleştirilmiş, ölçeklenir ve Kubernetes küme izlemek için Operations Management Suite yapılandırılır.

Bu öğretici Kubernetes kavramlar, Kubernetes bakın hakkında ayrıntılı bilgi için temel bir anlayış varsayar [Kubernetes belgelerine](https://kubernetes.io/docs/home/).

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticileri, bir uygulama bir kapsayıcı görüntüsüne paketlenmiş ve bu görüntüyü Azure kapsayıcı kayıt defterine karşıya Kubernetes küme oluşturuldu. 

Bu öğreticiyi tamamlamak için önceden oluşturulmuş gerekir `azure-vote-all-in-one-redis.yml` Kubernetes bildirim dosyası. Bu dosya uygulama kaynak koduna bir önceki öğreticide yüklendi. Depoyu kopyalanmış ve kopyalanan deposu dizinleri değiştirilmediğini doğrulayın.

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./tutorial-kubernetes-prepare-app.md). 

## <a name="update-manifest-file"></a>Güncelleştirme bildirim dosyası

Bu öğreticide, Azure kapsayıcı kayıt defteri (ACR) bir kapsayıcı görüntüsü depolamak için kullanılmış. Uygulamayı çalıştırmadan önce ACR oturum açma sunucusu adı Kubernetes bildirim dosyasında güncelleştirilmesi gerekiyor.

ACR oturum açma sunucu adıyla alma [az acr listesi](/cli/azure/acr#list) komutu.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Bildirim dosyası bir oturum açma sunucusu adı ile önceden oluşturulmuş `microsoft`. Dosyayı herhangi bir metin düzenleyicisinde açın. Bu örnekte, dosya içeren açıldıktan `vi`.

```console
vi azure-vote-all-in-one-redis.yml
```

Değiştir `microsoft` ACR oturum açma sunucu adına sahip. Bu değer satırına bulundu **47** bildirim dosyasının.

```yaml
containers:
- name: azure-vote-front
  image: microsoft/azure-vote-front:redis-v1
```

Dosyasını kaydedin ve kapatın.

## <a name="deploy-application"></a>Uygulama dağıtma

Uygulamayı çalıştırmak için [kubectl create](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#create) komutunu kullanın. Bu komut, bildirim dosyası ayrıştırır ve tanımlanmış Kubernetes nesneleri oluşturma.

```azurecli
kubectl create -f azure-vote-all-in-one-redis.yml
```

Çıktı:

```
deployment "azure-vote-back" created
service "azure-vote-back" created
deployment "azure-vote-front" created
service "azure-vote-front" created
```

## <a name="test-application"></a>Uygulamayı test etme

A [Kubernetes hizmet](https://kubernetes.io/docs/concepts/services-networking/service/) hangi uygulamanın Internet'e gösterir oluşturulur. Bu işlem birkaç dakika sürebilir. 

İlerleme durumunu izlemek için [kubectl get service](https://kubernetes.io/docs/user-guide/kubectl/v1.7/#get) komutunu `--watch` bağımsız değişkeniyle birlikte kullanın.

```azurecli
kubectl get service azure-vote-front --watch
```

Başlangıçta *azure-vote-front* için *EXTERNAL-IP* durumu *pending* olarak görünür.
  
```
azure-vote-front   10.0.34.242   <pending>     80:30676/TCP   7s
```

Bir kez *dış IP* adresi değiştiğini *bekleyen* için bir *IP adresi*, kullanın `CTRL-C` kubectl izleme işlemi durdurmak için. 

```
azure-vote-front   10.0.34.242   52.179.23.131   80:30676/TCP   2m
```

Uygulama görmek için dış IP adresine göz atın.

![Azure’da Kubernetes kümesinin görüntüsü](media/container-service-kubernetes-tutorials/azure-vote.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure oy uygulama AKS Kubernetes kümede için dağıtıldı. Tamamlanan görevler aşağıdakileri içerir:  

> [!div class="checklist"]
> * Kubernetes bildirim dosyaları indirme
> * İçinde Kubernetes uygulamayı çalıştırın
> * Uygulamayı test

Kubernetes uygulama ve Kubernetes altyapının ölçeklendirme hakkında bilgi edinmek için sonraki öğretici ilerleyin. 

> [!div class="nextstepaction"]
> [Ölçek Kubernetes uygulama ve altyapı](./tutorial-kubernetes-scale.md)
