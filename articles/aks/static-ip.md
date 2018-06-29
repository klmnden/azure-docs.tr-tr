---
title: Statik bir IP adresi olan Azure Kubernetes hizmet (AKS) yük dengeleyici kullanın
description: Statik bir IP adresi olan Azure Kubernetes hizmet (AKS) yük dengeleyici kullanın.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/21/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 2ff964e4909c288686253816bc40322b7839a2da
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37100598"
---
# <a name="use-a-static-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Statik bir IP adresi olan Azure Kubernetes hizmet (AKS) yük dengeleyici kullanın

Bazı durumlarda gibi Azure Kubernetes hizmet (AKS) yük dengeleyici yeniden veya LoadBalancer türüne sahip Kubernetes hizmetleri yeniden oluşturulur, genel IP adresini Kubernetes hizmetinin değişebilir. Bu belge Kubernetes hizmetleriniz için statik bir IP adresi yapılandırma ayrıntıları.

## <a name="create-static-ip-address"></a>Statik IP adresi oluşturun

Kubernetes hizmeti için bir statik genel IP adresi oluşturun. IP adresi AKS oluşturulmalıdır **düğümü** kaynak grubu. Kaynak grubu adı ile alma [az kaynak Göster] [ az-resource-show] komutu.

```azurecli-interactive
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Kullanım [az ağ genel IP oluşturun] [ az-network-public-ip-create] IP adresi oluşturmak için komutu.

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

 Gerekirse, adresi kullanılarak alınabilir [az ağ ortak IP listesi] [ az-network-public-ip-list] komutu.

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

Statik IP adresi oluşturulmadı veya yanlış kaynak grubunda oluşturduğunuz hizmet oluşturma başarısız olur. Sorun giderme, hizmet oluşturma olayları ile döndürmek için [kubectl açıklayan] [ kubectl-describe] komutu.

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
[az-network-public-ip-create]: /cli/azure/network/public-ip#az_network_public_ip_create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az_network_public_ip_list
[az-resource-show]: /cli/azure/resource#az-resource-show
