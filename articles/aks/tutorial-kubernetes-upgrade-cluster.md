---
title: Azure’da Kubernetes öğreticisi - kümeyi güncelleştirme
description: Azure’da Kubernetes öğreticisi - kümeyi güncelleştirme
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d66197b69a0804a49fabb72e9b97c77e000bdf88
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37131652"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Hizmeti’nde (AKS) Kubernetes’i yükseltme

Azure Kubernetes Hizmeti (AKS) kümesi, Azure CLI kullanılarak yükseltilebilir. Çalışan uygulamaların kesintiye uğramasını azaltmak için yükseltme işlemi sırasında Kubernetes düğümleri dikkatli bir şekilde [kordonlanır ve boşaltılır][kubernetes-drain].

Yedi parçalık bu öğreticinin yedinci kısmında, bir Kubernetes kümesi yükseltilir. Tamamladığınız görevler şunları içerir:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="get-cluster-versions"></a>Küme sürümlerini edinme

Bir kümeyi yükseltmeden önce, [az aks get-upgrades][] komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin.

```azurecli
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Aşağıdaki örnekte, geçerli düğüm sürümünün *1.9.6* olduğu ve kullanılabilir yükseltme sürümlerinin *Yükseltmeler* altında olduğunu görebilirsiniz.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------
default  myResourceGroup  1.9.6            1.9.6              1.10.3
```

## <a name="upgrade-cluster"></a>Kümeyi yükseltme

Küme düğümlerini yükseltmek için [az aks upgrade][] komutunu kullanın. Aşağıdaki örnek kümeyi *1.10.3* sürümüne güncelleştirir.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.10.3
```

Aşağıdaki kısaltılmış çıkış *kubernetesVersion* için şimdi *1.10.3* sürümünü gösterir:

```json
{
  "agentPoolProfiles": [
    {
      "count": 3,
      "maxPods": 110,
      "name": "nodepool1",
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS1_v2",
    }
  ],
  "dnsPrefix": "myAKSClust-myResourceGroup-19da35",
  "enableRbac": false,
  "fqdn": "myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io",
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "kubernetesVersion": "1.10.3",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-upgrade"></a>Yükseltmeyi doğrulama

[az aks show][] komutuyla yükseltmenin başarılı olup olmadığını doğrulayabilirsiniz.

```azurecli
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Çıktı:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.3               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir AKS kümesinde Kubernetes’i yükselttiniz. Şu görevler tamamlandı:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

AKS hakkında daha fazla bilgi için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [AKS'ye genel bakış][aks-intro]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-intro]: ./intro-kubernetes.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[az aks show]: /cli/azure/aks#az-aks-show
[az aks get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az aks upgrade]: /cli/azure/aks#az-aks-upgrade