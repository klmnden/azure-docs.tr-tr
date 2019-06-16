---
title: Azure Kubernetes Service (AKS) kümesini yükseltme
description: Azure Kubernetes Service (AKS) kümesini yükseltme hakkında bilgi edinin
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfou
ms.openlocfilehash: 2cadd4b33cb52307599ce1e83eee8370ef9850fe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66692784"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesini yükseltme

Bir AKS kümesi yaşam döngüsü bir parçası olarak, genellikle en son Kubernetes sürümüne yükseltmeniz gerekir. En son yayımlanan Kubernetes güvenlik uygulamak veya en son özellikleri almak için yükseltmek önemlidir. Bu makalede ana bileşenleri veya tek bir varsayılan bir AKS kümesindeki düğüm havuzu sürümüne yükseltme yapmayı gösterir.

AKS için bkz: birden çok düğüm havuzları veya (hem de AKS şu anda önizlemede), Windows Server düğümleri kullanan kümeleri [aks'deki bir düğüm havuzunu yükseltme][nodepool-upgrade].

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, Azure CLI Sürüm 2.0.65 gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="check-for-available-aks-cluster-upgrades"></a>Kullanılabilir AKS küme güncelleştirmelerini denetle

Kümeniz için hangi Kubernetes sürümlerinin kullanılabilir denetlemek için kullanmak [az aks get-yükseltmeleri] [ az-aks-get-upgrades] komutu. Aşağıdaki örnekte adlı Küme için kullanılabilen yükseltmeler denetler *myAKSCluster* adlı kaynak grubunda *myResourceGroup*:

```azurecli-interactive
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

> [!NOTE]
> Bir AKS kümesi yükselttiğinizde, ikincil Kubernetes sürümleri atlanamaz. Örneğin, arasında yükseltme *1.11.x* -> *1.12.x* veya *1.12.x* -> *1.13.x* , ancak izin verilir *1.11.x* -> *1.13.x* değil.
>
> Yükseltmek için gelen *1.11.x* -> *1.13.x*, ilk sürümünden yükseltme *1.11.x* -> *1.12.x*, ardından yükseltme gelen *1.12.x* -> *1.13.x*.

Aşağıdaki örnek çıktıda kümenin sürümüne yükseltilebilir gösterir *1.12.7* veya *1.12.8*:

```console
Name     ResourceGroup    MasterVersion  NodePoolVersion  Upgrades
-------  ---------------  -------------  ---------------  --------------
default  myResourceGroup  1.11.9         1.11.9           1.12.7, 1.12.8
```

## <a name="upgrade-an-aks-cluster"></a>AKS kümesini yükseltme

AKS kümenizin kullanılabilir sürümlerin listesini ile [az aks yükseltme] [ az-aks-upgrade] yükseltmek komutu. Yükseltme işlemi sırasında AKS yeni bir düğüm belirtilen Kubernetes sürümü, ardından dikkatli bir şekilde çalışan kümeye ekler [cordon ve azaldıkça] [ kubernetes-drain] çalışan kesintisini en aza indirmek için eski düğümlerinden biri uygulamalar. Yeni düğüm uygulama pod'ların çalıştığı onaylandıktan eski düğümü silinir. Kümedeki tüm düğümlerin yükseltilene dek bu işlemi yineler.

Aşağıdaki örnek, bir küme sürüme yükseltme *1.12.8*:

```azurecli-interactive
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.12.8
```

Sahip olduğunuz kaç düğümleri bağlı olarak bir küme yükseltmesi birkaç dakika sürer.

Yükseltmenin başarılı olduğunu doğrulamak için şunu kullanın [az aks show] [ az-aks-show] komutu:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

Küme artık çalışan aşağıdaki örnek çıktı gösterilmektedir *1.12.8*:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ---------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.12.8               Succeeded            myaksclust-myresourcegroup-19da35-90efab95.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, var olan bir AKS kümesini yükseltme nasıl oluşturulacağını gösterir. AKS kümesi yönetme ve dağıtma hakkında daha fazla bilgi edinmek için öğreticileri kümesini bakın.

> [!div class="nextstepaction"]
> [AKS öğreticileri][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-upgrades]: /cli/azure/aks#az-aks-get-upgrades
[az-aks-upgrade]: /cli/azure/aks#az-aks-upgrade
[az-aks-show]: /cli/azure/aks#az-aks-show
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
