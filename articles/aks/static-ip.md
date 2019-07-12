---
title: Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik IP adresi kullanın
description: Oluşturma ve statik IP adresi Azure Kubernetes Service (AKS) yük dengeleyiciyle kullanma hakkında bilgi edinin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: mlearned
ms.openlocfilehash: 9e32715766734bcbb150d70aeed2dc5b06a4bcbb
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67614463"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Azure Kubernetes Service (AKS) yük dengeleyiciyle bir statik genel IP adresi kullanın

Varsayılan olarak, bir AKS kümesi tarafından oluşturulan bir yük dengeleyici kaynağına atanmış genel IP adresi yalnızca o kaynak kullanım ömrü için geçerli olur. Kubernetes hizmeti silerseniz, IP adresi ve ilişkili yük dengeleyici da silinir. Belirli bir IP adresi atamak veya bilgisayarına Kubernetes Hizmetleri için bir IP adresi korumak istiyorsanız, oluşturun ve bir statik genel IP adresini kullanın.

Bu makalede bir statik genel IP adresi oluşturun ve Kubernetes hizmetinizi atayın gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

Şu anda yalnızca *temel IP SKU*desteklenir. İş devam ederken desteklemek için *standart IP* kaynak SKU. Daha fazla bilgi için [IP adresi türleri ve ayırma yöntemleri azure'da][ip-sku].

## <a name="create-a-static-ip-address"></a>Statik bir IP adresi oluşturma

AKS ile kullanım için bir statik genel IP adresi oluşturduğunuzda, IP adresi kaynağı oluşturulması gereken **düğüm** kaynak grubu. Kaynakları ayırmak istiyorsanız aşağıdaki bölüme bakın [düğüm kaynak grubu dışında bir statik IP adresi kullanacak](#use-a-static-ip-address-outside-of-the-node-resource-group).

İlk olarak, düğüm kaynak grubu adını alın [az aks show][az-aks-show] komut ve ekleme `--query nodeResourceGroup` sorgu parametresi. Aşağıdaki örnek, düğüm kaynak grubu için AKS kümesinin adını alır. *myAKSCluster* kaynak grubu adında *myResourceGroup*:

```azurecli-interactive
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Şimdi bir statik genel IP adresiyle oluşturmak [az ağ genel IP oluşturma][az-network-public-ip-create] komutu. Düğüm kaynak grubu adı, önceki komutta alınan ve ardından bir adı IP adresi kaynağı, gibi belirtmek *myAKSPublicIP*:

```azurecli-interactive
az network public-ip create \
    --resource-group MC_myResourceGroup_myAKSCluster_eastus \
    --name myAKSPublicIP \
    --allocation-method static
```

IP adresi aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi görüntülenir:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [...]
  }
}
```

Genel IP adresini kullanarak daha sonra alabilirsiniz [az ağ genel IP listesi][az-network-public-ip-list] komutu. Kaynak grubu düğümü, oluşturduğunuz genel IP adresi ve sorgu için bir ad belirtin *IPADDRESS* aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
$ az network public-ip show --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Statik IP adresini kullanarak bir hizmet oluşturma

Statik genel IP adresiyle bir hizmet oluşturmak için Ekle `loadBalancerIP` özelliği ve değerini bir statik genel IP adresi için YAML bildirimi. Adlı bir dosya oluşturun `load-balancer-service.yaml` aşağıdaki YAML'ye kopyalayın. Önceki adımda oluşturulan kendi genel IP adresi sağlayın.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Hizmet oluşturup dağıtımla `kubectl apply` komutu.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="use-a-static-ip-address-outside-of-the-node-resource-group"></a>Düğüm kaynak grubu dışında bir statik IP adresi kullanın

Kubernetes 1.10 veya sonrası, düğüm kaynak grubu dışında oluşturulan statik bir IP adresi kullanabilirsiniz. AKS kümesi tarafından kullanılan hizmet sorumlusunun izinlerini diğer kaynak grubuna aşağıdaki örnekte gösterildiği gibi yetkilerine sahip olmanız gerekir:

```azurecli-interactive
az role assignment create\
    --assignee <SP Client ID> \
    --role "Network Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Düğüm kaynak grubu dışında bir IP adresi kullanmak için hizmet tanımı için bir ek açıklama ekleyin. Aşağıdaki örnekte adlı bir kaynak grubu için ek açıklama ayarlar *myResourceGroup*. Kendi kaynak grubu adı girin:

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

## <a name="troubleshoot"></a>Sorun giderme

Statik IP adresi olarak tanımlanmışsa *loadBalancerIP* Kubernetes hizmet bildiriminin özelliği mevcut olmadığından veya oluşturulmamış düğüm kaynak grubunu ve yapılandırılan ek temsilci yok, Yük Dengeleyici Hizmeti oluşturma başarısız olur. Sorun gidermek için ile hizmet oluşturma olayları gözden geçirme [kubectl açıklayan][kubectl-describe] komutu. YAML bildiriminde belirtilen hizmet adı, aşağıdaki örnekte gösterildiği gibi sağlayın:

```console
kubectl describe service azure-load-balancer
```

Kubernetes Hizmet kaynağı hakkında bilgiler görüntülenir. *Olayları* bildiren aşağıdaki örnek çıktıda sonunda *sağlanan IP adresi bulunamadı kullanıcı*. Bu senaryolarda, düğüm kaynak grubunda oluşturulan statik genel IP adresini ve Kubernetes hizmet bildiriminde belirtilen IP adresinin doğru olduğundan emin olun.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
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
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulamalarınıza ağ trafiği üzerinde ek denetim için bunun yerine isteyebileceğiniz [giriş denetleyicisine oluşturma][aks-ingress-basic]. You can also [create an ingress controller with a static public IP address][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
