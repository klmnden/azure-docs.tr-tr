---
title: Azure Kubernetes Service (AKS) kümesini yükseltme
description: Azure Kubernetes Service (AKS) kümesini yükseltme
services: container-service
author: gabrtv
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/18/2018
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 4ff2b56afc4496b6344735b4e3c813b06cee17e3
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2018
ms.locfileid: "42057138"
---
# <a name="upgrade-an-azure-kubernetes-service-aks-cluster"></a>Azure Kubernetes Service (AKS) kümesini yükseltme

Azure Kubernetes Service (AKS), Kubernetes kümelerini yükseltme dahil ortak yönetim görevlerini gerçekleştirmeyi kolaylaştırır.

## <a name="upgrade-an-aks-cluster"></a>AKS kümesini yükseltme

Bir kümeyi yükseltmeden önce, `az aks get-upgrades` komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin.

```azurecli-interactive
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Çıktı:

```console
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  -------------------
default  mytestaks007     1.8.10           1.8.10             1.9.1, 1.9.2, 1.9.6
```

Yükseltme için kullanılabilen üç sürümü sunuyoruz: 1.9.1 1.9.2 ve 1.9.6. Kullanılabilir en son sürüme yükseltmek için `az aks upgrade` komutunu kullanabiliriz.  Yükseltme işlemi sırasında AKS yeni bir düğüm kümesine, sonra dikkatle ekleyeceğiniz [kordon altına alma ve boşaltma] [ kubernetes-drain] birer birer çalışan uygulamaların kesintiye en aza indirmek için bir düğüm.

> [!NOTE]
> AKS kümesini yükseltme yaparken, Kubernetes ikincil sürümleri atlanamaz. Örneğin, yükseltmeleri 1.8.x arasında -> 1.9.x veya 1.9.x 1.10.x -> izin verilir, ancak 1.8 1.10 -> değil. Yükseltmek için 1.8 1.10 ->, ilk sürümünden yükseltme yapmanız 1.8 -> 1.9 ve başka yapın başka 1.9 sürümüne yükseltme 1.10 ->

```azurecli-interactive
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.9.6
```

Çıktı:

```json
{
  "id": "/subscriptions/<Subscription ID>/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myAKSCluster",
  "location": "eastus",
  "name": "myAKSCluster",
  "properties": {
    "accessProfiles": {
      "clusterAdmin": {
        "kubeConfig": "..."
      },
      "clusterUser": {
        "kubeConfig": "..."
      }
    },
    "agentPoolProfiles": [
      {
        "count": 1,
        "dnsPrefix": null,
        "fqdn": null,
        "name": "myAKSCluster",
        "osDiskSizeGb": null,
        "osType": "Linux",
        "ports": null,
        "storageProfile": "ManagedDisks",
        "vmSize": "Standard_D2_v2",
        "vnetSubnetId": null
      }
    ],
    "dnsPrefix": "myK8sClust-myResourceGroup-4f48ee",
    "fqdn": "myk8sclust-myresourcegroup-4f48ee-406cc140.hcp.eastus.azmk8s.io",
    "kubernetesVersion": "1.9.6",
    "linuxProfile": {
      "adminUsername": "azureuser",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "..."
          }
        ]
      }
    },
    "provisioningState": "Succeeded",
    "servicePrincipalProfile": {
      "clientId": "e70c1c1c-0ca4-4e0a-be5e-aea5225af017",
      "keyVaultSecretRef": null,
      "secret": null
    }
  },
  "resourceGroup": "myResourceGroup",
  "tags": null,
  "type": "Microsoft.ContainerService/ManagedClusters"
}
```

`az aks show` komutuyla yükseltmenin başarılı olup olmadığını doğrulayabilirsiniz.

```azurecli-interactive
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Çıktı:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.9.6                 Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
```

## <a name="next-steps"></a>Sonraki adımlar

AKS öğreticileri ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi][aks-tutorial-prepare-app]

<!-- LINKS - external -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/

<!-- LINKS - internal -->
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
