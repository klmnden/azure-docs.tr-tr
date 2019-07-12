---
title: Azure Kubernetes Service (AKS) çıkış trafiği için statik IP adresi
description: Azure Kubernetes Service (AKS) kümesini çıkış trafiği için bir statik genel IP adresi oluşturulacağı ve kullanılacağı hakkında bilgi edinin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 03/04/2019
ms.author: mlearned
ms.openlocfilehash: 094a696a12025dcfd575ce3f035b12b4a04aba10
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615577"
---
# <a name="use-a-static-public-ip-address-for-egress-traffic-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) çıkış trafiği için bir statik genel IP adresi kullanın

Varsayılan olarak, Azure Kubernetes Service (AKS) kümesini çıkış IP adresinden rastgele atanır. Örneğin dış hizmetlere erişim için bir IP adresi tanımlamak, ihtiyacınız olduğunda bu yapılandırma ideal değildir. Bunun yerine, hizmet erişim için izin verilenler listesinde olabilir statik bir IP adresi atamanız gerekebilir.

Bu makalede oluşturma ve çıkış trafiği bir AKS kümesindeki ile kullanmak için bir statik genel IP adresi kullanma gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak][aks-quickstart-cli] or [using the Azure portal][aks-quickstart-portal].

Ayrıca Azure CLI Sürüm 2.0.59 gerekir veya daha sonra yüklü ve yapılandırılmış. Çalıştırma `az --version` sürümü bulmak için. Gerekirse yüklemek veya yükseltmek bkz [Azure CLI yükleme][install-azure-cli].

## <a name="egress-traffic-overview"></a>Çıkış trafiği genel bakış

Bir AKS kümesi giden trafiği izleyen [Azure Load Balancer kuralları][outbound-connections]. İlk Kubernetes hizmeti türü önce `LoadBalancer` oluşturulur, aracı düğümleri bir AKS kümesindeki herhangi bir Azure Load Balancer havuzunun parçası değildir. Bu yapılandırmada hiçbir örnek düzeyi genel IP adresi düğümler var. Azure, yapılandırılabilir veya belirleyici olmayan ortak kaynak IP adresine giden akış çevirir.

Bir kez türünde bir Kubernetes hizmet `LoadBalancer` oluşturulur, aracı düğümleri, bir Azure yük dengeleyici havuzuna eklenir. Giden akış için Azure, ilk genel IP adresini yük dengeleyici üzerinde yapılandırılmış çevirir. Bu genel IP adresi, yalnızca o kaynak kullanım ömrü için geçerlidir. Kubernetes LoadBalancer hizmet silerseniz, IP adresi ve ilişkili yük dengeleyici da silinir. Belirli bir IP adresi atamak veya bilgisayarına Kubernetes Hizmetleri için bir IP adresi korumak istiyorsanız, oluşturun ve bir statik genel IP adresini kullanın.

## <a name="create-a-static-public-ip"></a>Statik genel IP oluşturma

AKS ile kullanım için bir statik genel IP adresi oluşturduğunuzda, IP adresi kaynağı oluşturulmalıdır **düğüm** kaynak grubu. Kaynak grubu adını alın [az aks show][az-aks-show] komut ve ekleme `--query nodeResourceGroup` sorgu parametresi. Aşağıdaki örnek, düğüm kaynak grubu için AKS kümesinin adını alır. *myAKSCluster* kaynak grubu adında *myResourceGroup*:

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

Aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi IP adresi gösterilir:

```json
{
  "publicIp": {
    "dnsSettings": null,
    "etag": "W/\"6b6fb15c-5281-4f64-b332-8f68f46e1358\"",
    "id": "/subscriptions/<SubscriptionID>/resourceGroups/MC_myResourceGroup_myAKSCluster_eastus/providers/Microsoft.Network/publicIPAddresses/myAKSPublicIP",
    "idleTimeoutInMinutes": 4,
    "ipAddress": "40.121.183.52",
    [..]
  }
```

Genel IP adresini kullanarak daha sonra alabilirsiniz [az ağ genel IP listesi][az-network-public-ip-list] komutu. Düğüm kaynak grubunun adını belirtin ve ardından sorgu *IPADDRESS* aşağıdaki örnekte gösterildiği gibi:

```azurecli-interactive
$ az network public-ip list --resource-group MC_myResourceGroup_myAKSCluster_eastus --query [0].ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-with-the-static-ip"></a>Statik IP adresiyle bir hizmet oluşturma

Statik genel IP adresiyle bir hizmet oluşturmak için Ekle `loadBalancerIP` özelliği ve değerini bir statik genel IP adresi için YAML bildirimi. Adlı bir dosya oluşturun `egress-service.yaml` aşağıdaki YAML'ye kopyalayın. Önceki adımda oluşturulan kendi genel IP adresi sağlayın.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: azure-egress
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
```

Hizmet oluşturup dağıtımla `kubectl apply` komutu.

```console
kubectl apply -f egress-service.yaml
```

Bu hizmet, Azure yük Dengeleyicide yeni bir ön uç IP yapılandırır. Diğer yoksa IP'ler, ardından yapılandırılmış **tüm** çıkış trafiği, bu adresi artık kullanmalıdır. Azure Load Balancer üzerinde birden çok adresi yapılandırıldığında, çıkış, yük dengeleyicide ilk IP kullanır.

## <a name="verify-egress-address"></a>Çıkış adresini doğrulayın

Statik genel IP adresi kullanılır, DNS Arama hizmeti gibi kullanabileceğinizi doğrulamak için `checkip.dyndns.org`.

Başlatma ve ekleme için temel bir *Debian* pod:

```console
kubectl run -it --rm aks-ip --image=debian --generator=run-pod/v1
```

Kapsayıcı içindeki bir web sitesinden erişmek için `apt-get` yüklemek için `curl` kapsayıcıya alın.

```console
apt-get update && apt-get install curl -y
```

Artık erişmek için curl kullanın *checkip.dyndns.org* site. Aşağıdaki örnek çıktıda gösterildiği çıkış IP adresi gösterilir. Bu IP adresi ile eşleşen oluşturulur ve yük dengeleyici hizmet için tanımlanan statik genel IP adresi:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 40.121.183.52</body></html>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure Load Balancer birden çok genel IP adreslerinde koruma önlemek için bunun yerine bir giriş denetleyicisini kullanabilirsiniz. Giriş denetleyicileri, URI taşıyabilmenizi sağlar ve Yukarı Akış SSL/TLS şifreleme için SSL/TLS sonlandırma, destek gibi ek avantajlar sunacak. Daha fazla bilgi için [AKS içinde bir temel giriş denetleyicisine oluşturma][ingress-aks-cluster].

<!-- LINKS - internal -->
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[azure-cli-install]: /cli/azure/install-azure-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli