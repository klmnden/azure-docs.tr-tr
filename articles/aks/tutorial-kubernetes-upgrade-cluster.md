---
title: Azure’da Kubernetes öğreticisi - Kümeyi yükseltme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde var olan bir AKS kümesini en son Kubernetes sürümüne yükseltmeyi öğreneceksiniz.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: tutorial
ms.date: 12/19/2018
ms.author: mlearned
ms.custom: mvc
ms.openlocfilehash: 90c5a4e18f72d9a8b048ef0f40a5c0b405a584f2
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614157"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Service'te (AKS) Kubernetes'i yükseltme

Uygulama ve küme yaşam döngüsünün bir parçası olarak Kubernetes'in son sürümüne yükselterek yeni özelliklerden faydalanmak isteyebilirsiniz. Azure Kubernetes Hizmeti (AKS) kümesi, Azure CLI kullanılarak yükseltilebilir.

Yedi parçalık bu öğreticinin yedinci kısmında, bir Kubernetes kümesi yükseltilir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi. Bu görüntü Azure Container Registry'ye yüklendi ve bir AKS kümesi oluşturulmakta. Uygulama ardından için AKS kümesi dağıtıldı. Bu adımları tamamlamadıysanız ve takip etmek istediğiniz, başlayan [öğretici 1 – kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app].

Bu öğreticide, Azure CLI Sürüm 2.0.53 gerekir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Kullanılabilir küme sürümlerini alma

Bir kümeyi yükseltmeden önce, [az aks get-upgrades][] komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnekte, geçerli sürümüdür *1.9.11*, ve kullanılabilir sürümler altında gösterilen *yükseltmeleri* sütun.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  --------------
default  myResourceGroup  1.9.11           1.9.11             1.10.8, 1.10.9
```

## <a name="upgrade-a-cluster"></a>Kümeyi yükseltme

Çalışan uygulamaların kesintiye en aza indirmek için AKS düğümleri dikkatli bir şekilde kordonlanır boşaltılır ve. Aşağıdaki adımlar bu işlemde gerçekleştirilir:

1. Kubernetes Zamanlayıcı yükseltilecek olan düğümün üzerinde zamanlanmasını ek pod'lar engeller.
1. Düğümde çalışan pod'ları, kümedeki diğer düğümlere zamanlanır.
1. En son Kubernetes bileşenlerini çalıştıran bir düğüm oluşturulur.
1. Yeni düğümü hazır ve kümeye birleştirilmiş olduğunda, Kubernetes Zamanlayıcı pod'ları üzerinde çalışmaya başlar.
1. Eski düğümü silinir ve kümedeki sonraki düğüme cordon ve boşaltma işlemi başlar.

AKS kümesini yükseltmek için [az aks upgrade][] komutunu kullanın. Aşağıdaki örnekte, kümeyi Kubernetes sürümüne yükseltir. *1.10.9*.

> [!NOTE]
> Aynı anda yalnızca bir ikincil sürüm yükseltmesi yapabilirsiniz. Örneğin, sürümünden yükseltme yapabilirsiniz *1.9.11* için *1.10.9*, ancak yükseltme yapamazsınız *1.9.6* için *1.11.x* doğrudan. Yükseltmenin uygulanacağı *1.9.11* için *1.11.x*, ilk sürümünden yükseltme *1.9.11* için *1.10.x*, ardından başkabiryükseltmegerçekleştirmek*1.10.x* için *1.11.x*.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.9
```

Aşağıdaki sıkıştırılmış örneğe çıktısı bunu gösterir *kubernetesVersion* artık raporlar *1.10.9*:

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
  "kubernetesVersion": "1.10.9",
  "location": "eastus",
  "name": "myAKSCluster",
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

## <a name="validate-an-upgrade"></a>Yükseltmeyi doğrulama

[az aks show][] komutunu aşağıdaki şekilde kullanarak yükseltmenin başarılı olup olmadığını doğrulayabilirsiniz:

```azurecli
az aks show --resource-group myResourceGroup --name myAKSCluster --output table
```

AKS kümesi çalıştırır, aşağıdaki örnek çıktı gösterilmektedir *KubernetesVersion 1.10.9*:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.9               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Küme silme

Bu öğretici serisinin son bölümünde olduğundan, AKS kümeyi silmek isteyebilirsiniz. Kubernetes düğümleri Azure sanal makinelerinde (VM) çalıştığından kümeyi kullanmasanız dahi ücret tahsil edilmeye devam eder. Kullanım [az grubu Sil][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini kaldırmak için ve tüm ilgili kaynakları.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz: [AKS hizmet sorumlusu hakkında önemli noktalar ve silme][sp-delete].

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir AKS kümesinde Kubernetes’i yükselttiniz. Şunları öğrendiniz:

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
[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-delete]: /cli/azure/group#az-group-delete
[sp-delete]: kubernetes-service-principal.md#additional-considerations
