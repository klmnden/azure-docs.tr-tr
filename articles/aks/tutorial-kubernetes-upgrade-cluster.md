---
title: Azure’da Kubernetes öğreticisi - Kümeyi yükseltme
description: Bu Azure Kubernetes Service (AKS) öğreticisinde var olan bir AKS kümesini en son Kubernetes sürümüne yükseltmeyi öğreneceksiniz.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 1c0710be11b95b66d16661b5aff9cbf739ccda92
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48901955"
---
# <a name="tutorial-upgrade-kubernetes-in-azure-kubernetes-service-aks"></a>Öğretici: Azure Kubernetes Hizmeti’nde (AKS) Kubernetes’i yükseltme

Uygulama ve küme yaşam döngüsünün bir parçası olarak Kubernetes'in son sürümüne yükselterek yeni özelliklerden faydalanmak isteyebilirsiniz. Azure Kubernetes Hizmeti (AKS) kümesi, Azure CLI kullanılarak yükseltilebilir. Çalışan uygulamaların kesintiye uğramasını azaltmak için yükseltme işlemi sırasında Kubernetes düğümleri dikkatli bir şekilde [kordonlanır ve boşaltılır][kubernetes-drain].

Yedi parçalık bu öğreticinin yedinci kısmında, bir Kubernetes kümesi yükseltilir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı. Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

Bu öğretici için Azure CLI 2.0.44 veya sonraki bir sürümü kullanmanız gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="get-available-cluster-versions"></a>Kullanılabilir küme sürümlerini alma

Bir kümeyi yükseltmeden önce, [az aks get-upgrades][] komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin:

```azurecli
az aks get-upgrades --resource-group myResourceGroup --name myAKSCluster --output table
```

Aşağıdaki örnekte, geçerli sürümün *1.9.6* olduğu ve kullanılabilir yükseltme sürümlerinin *Yükseltmeler* altında olduğunu görebilirsiniz.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------
default  myResourceGroup  1.9.9            1.9.9              1.10.3, 1.10.5, 1.10.6
```

## <a name="upgrade-a-cluster"></a>Kümeyi yükseltme

AKS kümesini yükseltmek için [az aks upgrade][] komutunu kullanın. Aşağıdaki örnek kümeyi Kubernetes'in *1.10.6* sürümüne yükseltir.

> [!NOTE]
> Aynı anda yalnızca bir ikincil sürüm yükseltmesi yapabilirsiniz. Örneğin *1.9.6* sürümünden *1.10.3* sürümüne yükseltebilirsiniz ancak *1.9.6* sürümünden doğrudan *1.11.x* sürümüne yükseltemezsiniz. *1.9.6* sürümünden *1.11.x* sürümüne yükseltmek için öncelikle *1.9.6* sürümünden *1.10.3* sürümüne, ardından *1.10.3* sürümünden *1.11.x* sürümüne yükseltmeniz gerekir.

```azurecli
az aks upgrade --resource-group myResourceGroup --name myAKSCluster --kubernetes-version 1.10.6
```

Aşağıdaki kısaltılmış çıkış *kubernetesVersion* için şimdi *1.10.6* sürümünü gösterir:

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
  "kubernetesVersion": "1.10.6",
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

Aşağıdaki örnekte AKS kümesinde *KubernetesVersion 1.10.6* sürümünün yüklü olduğu gösterilmektedir:

```
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus      myResourceGroup  1.10.6               Succeeded            myaksclust-myresourcegroup-19da35-bd54a4be.hcp.eastus.azmk8s.io
```

## <a name="delete-the-cluster"></a>Küme silme

Öğretici serisinin son bölümüne geldiğiniz için AKS kümesini silmek isteyebilirsiniz. Kubernetes düğümleri Azure sanal makinelerinde (VM) çalıştığından kümeyi kullanmasanız dahi ücret tahsil edilmeye devam eder. [az group delete][az-group-delete] komutunu kullanarak kaynak grubunu, kapsayıcı hizmetini ve ilgili tüm kaynakları kaldırabilirsiniz.

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

> [!NOTE]
> Kümeyi sildiğinizde, AKS kümesi tarafından kullanılan Azure Active Directory hizmet sorumlusu kaldırılmaz. Hizmet sorumlusunu kaldırma adımları için bkz. [AKS hizmet sorumlusuyla ilgili önemli noktalar ve silme][sp-delete].

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
