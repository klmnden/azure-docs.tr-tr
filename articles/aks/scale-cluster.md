---
title: "Bir Azure kapsayıcı hizmeti (AKS) küme ölçeklendirin | Microsoft Docs"
description: "Bir Azure kapsayıcı hizmeti (AKS) küme ölçeklendirme."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: "Kubernetes, Docker, Kapsayıcılar, Mikro Hizmetler, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 3c05ccc6d1e304b7c511015b916425347bc4d995
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2017
---
# <a name="scale-an-azure-container-service-aks-cluster"></a>Bir Azure kapsayıcı hizmeti (AKS) küme ölçeklendirme

Düğümlerin farklı sayıda AKS kümeye ölçeklendirme kolaydır.  İstenilen düğüm sayısına seçin ve Çalıştır `az aks scale` komutu.  Ölçeklendirme, düğümleri dikkatle olacak [cordoned ve boşaltmış](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/) çalışan uygulamalar engellemeyi en aza indirmek için.  Ölçekleme sırasında `az` komutu düğümleri işaretlenmiş kadar bekler `Ready` Kubernetes küme tarafından.

## <a name="scale-the-cluster-nodes"></a>Küme düğümleri ölçeklendirme

Kullanım `az aks scale` küme düğümleri ölçeklendirmek için komutu. Aşağıdaki örnek adlı bir küme ölçeklendirir *myK8SCluster* tek bir düğüme.

```azurecli-interactive
az aks scale --name myK8sCluster --resource-group myResourceGroup --agent-count 1
```

Çıktı:

```json
{
  "id": "/subscriptions/4f48eeae-9347-40c5-897b-46af1b8811ec/resourcegroups/myResourceGroup/providers/Microsoft.ContainerService/managedClusters/myK8sCluster",
  "location": "eastus",
  "name": "myK8sCluster",
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
        "name": "myK8sCluster",
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
    "kubernetesVersion": "1.7.7",
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

## <a name="next-steps"></a>Sonraki adımlar

Dağıtma ve AKS AKS öğreticileri ile yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS Öğreticisi](./tutorial-kubernetes-prepare-app.md)
