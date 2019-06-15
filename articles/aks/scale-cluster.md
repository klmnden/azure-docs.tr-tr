---
title: Azure Kubernetes Service (AKS) kümesini ölçeklendirme
description: Bir Azure Kubernetes Service (AKS) kümedeki düğüm sayısını ölçeklendirmeyi öğrenin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: iainfoulds
ms.openlocfilehash: de3f8613c93715aecf7e9e066a8ad1d82e4379e3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66475138"
---
# <a name="scale-the-node-count-in-an-azure-kubernetes-service-aks-cluster"></a>Bir Azure Kubernetes Service (AKS) kümedeki düğüm sayısını ölçekleyin

Kaynak, gerekiyorsa uygulamalarınızı değiştirmek için farklı sayıda düğüm çalıştırmak için bir AKS kümesi elle ölçeklendirebilirsiniz. Ölçeği Azalt düğümleri dikkatli bir şekilde konusunda [kordonlanır ve boşaltılır] [ kubernetes-drain] çalışan uygulamaların kesintiye en aza indirmek için. Ölçeği artırma, AKS düğümleri işaretlenene kadar bekler `Ready` pod'ları üzerinde zamanlanmış önce Kubernetes kümesi tarafından.

## <a name="scale-the-cluster-nodes"></a>Küme düğümlerini ölçeklendirme

İlk olarak, alın *adı* düğüm havuzu kullanmanın [az aks show] [ az-aks-show] komutu. Aşağıdaki örnekte adlı Küme için düğüm havuzu adı alır *myAKSCluster* içinde *myResourceGroup* kaynak grubu:

```azurecli-interactive
az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles
```

Aşağıdaki örnek çıktı gösterilmektedir *adı* olduğu *nodepool1*:

```console
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query agentPoolProfiles

[
  {
    "count": 1,
    "maxPods": 110,
    "name": "nodepool1",
    "osDiskSizeGb": 30,
    "osType": "Linux",
    "storageProfile": "ManagedDisks",
    "vmSize": "Standard_DS2_v2"
  }
]
```

Kullanım [az aks ölçek] [ az-aks-scale] küme düğümlerini ölçeklendirmek için komutu. Aşağıdaki örnekte adlı bir küme ölçeklendirir *myAKSCluster* tek bir düğüm. Kendi sağlamak *--nodepool adı* önceki komuttan gibi *nodepool1*:

```azurecli-interactive
az aks scale --resource-group myResourceGroup --name myAKSCluster --node-count 1 --nodepool-name <your node pool name>
```

Aşağıdaki örnek çıktıda kümenin bir düğümü için başarıyla Ölçeklendirildi gösterildiği gösterir *agentPoolProfiles* bölümü:

```json
{
  "aadProfile": null,
  "addonProfiles": null,
  "agentPoolProfiles": [
    {
      "count": 1,
      "maxPods": 110,
      "name": "nodepool1",
      "osDiskSizeGb": 30,
      "osType": "Linux",
      "storageProfile": "ManagedDisks",
      "vmSize": "Standard_DS2_v2",
      "vnetSubnetId": null
    }
  ],
  [...]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, el ile bir AKS kümesi artırmak veya düğüm sayısını azaltmak için ölçeği. Ayrıca [ölçeklendiriciyi küme] [ cluster-autoscaler] (şu anda önizlemede AKS) kümenizi otomatik olarak ölçeklendirmek için.

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial]: ./tutorial-kubernetes-prepare-app.md
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-aks-scale]: /cli/azure/aks#az-aks-scale
[cluster-autoscaler]: cluster-autoscaler.md
