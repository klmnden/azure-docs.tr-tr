---
title: Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik IP adresi kullanın
description: Statik bir IP adresi ile Azure Kubernetes Service (AKS) yük dengeleyici kullanın.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: af1dffd681eaf7b2eb90ab4657cc25f2144a48d9
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39423632"
---
# <a name="use-a-static-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik IP adresi kullanın

Bazı durumlarda, Azure Kubernetes Service (AKS) yük dengeleyici yeniden veya Kubernetes Hizmetleri LoadBalancer türünde oluşturulur, gibi Kubernetes hizmetinin genel IP adresi değişebilir. Bu belge, Kubernetes hizmetleriniz için statik bir IP adresi yapılandırma ayrıntıları.

## <a name="create-static-ip-address"></a>Statik IP adresi oluşturma

Kubernetes hizmeti için bir statik genel IP adresi oluşturun. IP adresi, AKS içinde oluşturulması gerekiyor **düğüm** kaynak grubu. Kaynak grubu adını alın [az resource show] [ az-resource-show] komutu.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az ağ genel IP oluşturma] [ az-network-public-ip-create] IP adresi oluşturmak için komutu.

```azurecli-interactive
az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static
```

IP adresini not alın.

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    "ipConfiguration": null,
    "ipTags": [],
    "location": "eastus",
    "name": "myAKSPublicIP",
    "provisioningState": "Succeeded",
    "publicIpAddressVersion": "IPv4",
    "publicIpAllocationMethod": "Static",
    "resourceGroup": "myResourceGroup",
    "resourceGuid": "56ec8760-a3b8-4aeb-a89d-42e68d2cbc8c",
    "sku": {
      "name": "Basic"
    },
    "tags": null,
    "type": "Microsoft.Network/publicIPAddresses",
    "zones": null
  }
````

 Gerekirse adres kullanılarak alınabilir [az ağ genel IP listesi] [ az-network-public-ip-list] komutu.

```azurecli-interactive
az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv
```

```console
40.121.183.52
```

## <a name="create-service-with-ip-address"></a>IP adresi ile hizmet oluşturma

Statik IP adresi sağlandıktan sonra bir Kubernetes hizmeti ile oluşturulabilmesi için `loadBalancerIP` özelliği ve statik IP adresi değeri.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-vote-front
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-vote-front
```

## <a name="troubleshooting"></a>Sorun giderme

Statik IP adresi oluşturulmadı veya yanlış kaynak grubunda oluşturulan hizmet oluşturma başarısız olur. Sorun giderme, hizmet oluşturma olayları ile iade [kubectl açıklayan] [ kubectl-describe] komutu.

```azurecli-interactive
kubectl describe service azure-vote-front
```

```console
Name:                     azure-vote-front
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-vote-front
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-vote-front: user supplied IP Address 40.121.183.52 was not found
```

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes-v1-4.github.io/docs/user-guide/kubectl/kubectl_describe/

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-resource-show]: /cli/azure/resource#az-resource-show
