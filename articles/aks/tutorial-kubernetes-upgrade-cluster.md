---
title: Azure’da Kubernetes öğreticisi - kümeyi güncelleştirme
description: Azure’da Kubernetes öğreticisi - kümeyi güncelleştirme
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 04/05/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 8340ca9d95e7600e2500755de252e7f6c6de9343
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="tutorial-upgrade-kubernetes-in-azure-container-service-aks"></a>Öğretici: Azure Container Service’te (AKS) Kubernetes’i yükseltme

Azure Container Service (AKS) kümesi, Azure CLI kullanılarak yükseltilebilir. Yükseltme işlemi sırasında, çalışan uygulamaların kesintiye uğramasını azaltmak için Kubernetes düğümleri dikkatli bir şekilde [kordonlanır ve boşaltılır][kubernetes-drain].

Sekiz parçalık bu öğreticinin sekizinci kısmında, bir Kubernetes kümesi yükseltilir. Tamamladığınız görevler şunları içerir:

> [!div class="checklist"]
> * Geçerli ve kullanılabilir Kubernetes sürümlerini tanımlama
> * Kubernetes düğümlerini yükseltme
> * Başarılı bir yükseltmeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama bir kapsayıcı görüntüsüne paketlendi, bu görüntü Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu. Ardından uygulama Kubernetes kümesinde çalıştırıldı.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.


## <a name="get-cluster-versions"></a>Küme sürümlerini edinme

Bir kümeyi yükseltmeden önce, `az aks get-upgrades` komutunu kullanarak hangi Kubernetes sürümlerinin yükseltme için kullanılabilir olduğunu öğrenin.

```azurecli
az aks get-upgrades --name myAKSCluster --resource-group myResourceGroup --output table
```

Bu örnekte, geçerli düğüm sürümünün `1.7.9` ve kullanılabilir yükseltme sürümlerinin yükseltme sütununun altında olduğunu görebilirsiniz.

```
Name     ResourceGroup    MasterVersion    NodePoolVersion    Upgrades
-------  ---------------  ---------------  -----------------  ----------------------------------
default  myResourceGroup  1.7.9            1.7.9              1.7.12, 1.8.1, 1.8.2, 1.8.6, 1.8.7
```

## <a name="upgrade-cluster"></a>Kümeyi yükseltme

Küme düğümlerini yükseltmek için `az aks upgrade` komutunu kullanın. Aşağıdaki örnekler kümeyi `1.8.2` sürümüne güncelleştirir.

```azurecli
az aks upgrade --name myAKSCluster --resource-group myResourceGroup --kubernetes-version 1.8.2
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
    "kubernetesVersion": "1.8.2",
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

## <a name="validate-upgrade"></a>Yükseltmeyi doğrulama

`az aks show` komutuyla yükseltmenin başarılı olup olmadığını doğrulayabilirsiniz.

```azurecli
az aks show --name myAKSCluster --resource-group myResourceGroup --output table
```

Çıktı:

```json
Name          Location    ResourceGroup    KubernetesVersion    ProvisioningState    Fqdn
------------  ----------  ---------------  -------------------  -------------------  ----------------------------------------------------------------
myAKSCluster  eastus     myResourceGroup  1.8.2                Succeeded            myk8sclust-myresourcegroup-3762d8-2f6ca801.hcp.eastus.azmk8s.io
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