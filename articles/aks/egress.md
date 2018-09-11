---
title: Azure Kubernetes Service (AKS) kümesini beyaz liste çıkış trafiği
description: Azure Kubernetes Service (AKS) kümesini beyaz liste çıkış trafiği
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: iainfou
ms.openlocfilehash: e2793a72fcbc20b79bdd564e331426fedf1ae34b
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44347809"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Azure Kubernetes Service (AKS) çıkış

Varsayılan olarak, Azure Kubernetes Service (AKS) kümesini çıkış adresinden rastgele atanır. Bu yapılandırma, dış hizmetlere erişmek için bir IP adresini tanımlamak ihtiyaç duyulduğunda ideal değildir. Bu belge, bir AKS kümesindeki bir çıkış statik olarak atanmış IP adresi oluşturup işlemi açıklanmaktadır.

## <a name="egress-overview"></a>Çıkış genel bakış

Bir AKS kümesi giden trafiği izleyen belgelenen Azure Load Balancer kuralları, [burada][outbound-connections]. İlk Kubernetes hizmeti türü önce `LoadBalancer` oluşturulur, aracı düğümleri herhangi bir Azure Load Balancer havuzunun parçası değildir. Bu yapılandırmada, bir örnek düzeyi genel IP adresi düğümlerdir. Azure, yapılandırılabilir veya belirleyici olmayan ortak kaynak IP adresine giden akış çevirir.

Bir kez türünde bir Kubernetes hizmet `LoadBalancer` oluşturulur, aracı düğümleri, bir Azure yük dengeleyici havuzuna eklenir. Giden akış için Azure, ilk genel IP adresini yük dengeleyici üzerinde yapılandırılmış çevirir.

## <a name="create-a-static-public-ip"></a>Statik genel IP oluşturma

Rastgele bir IP adresi kullanılmasını önlemek için statik bir IP adresi oluşturun ve bu adresi bir yük dengeleyici kullandığından emin olun. IP adresi, AKS içinde oluşturulması gerekiyor **düğüm** kaynak grubu.

Kaynak grubu adını alın [az resource show] [ az-resource-show] komutu. Kaynak grubu adı ve küme adını, ortamınızla eşleşecek şekilde güncelleştirin.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Ardından, [az network public-IP oluşturma] [ public-ip-create] statik genel IP adresi oluşturmak için komutu. Kaynak grubu adı son adımda ad gatherred eşleşecek şekilde güncelleştirin.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Statik IP adresiyle bir hizmet oluşturma

Bir IP adresi olduğuna göre bir Kubernetes hizmeti türüyle oluşturma `LoadBalancer` ve hizmete IP adresi atayın.

Adlı bir dosya oluşturun `egress-service.yaml` aşağıdaki YAML'ye kopyalayın. IP adresini ortamınızla eşleşecek şekilde güncelleştirin.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: aks-egress
spec:
  loadBalancerIP: 23.101.128.81
  type: LoadBalancer
  ports:
  - port: 8080
```

Hizmet oluşturup dağıtımla `kubectl apply` komutu.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Bu hizmet oluşturmak için Azure yük Dengeleyicide yeni bir ön uç IP yapılandırır. Diğer yoksa IP'ler, ardından yapılandırılmış **tüm** çıkış trafiği, bu adresi artık kullanmalıdır. Azure Load Balancer üzerinde birden çok adresi yapılandırıldığında, çıkış, yük dengeleyicide ilk IP kullanır.

## <a name="verify-egress-address"></a>Çıkış adresini doğrulayın

Genel IP adresini kullanılmakta olduğunu doğrulamak için bir hizmet gibi kullanın `checkip.dyndns.org`.

Başlatma ve bir pod için ekleme:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Gerekirse, kapsayıcıda curl yükleyin:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org`, çıkış IP adresini döndürür:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

IP adresi Azure yük dengeleyiciye bağlı statik IP adresi eşleştiğini görmeniz gerekir.

## <a name="ingress-controller"></a>Giriş denetleyicisine

Azure Load Balancer birden çok genel IP adreslerinde koruma önlemek için giriş denetleyicisini kullanmayı düşünün. Giriş denetleyicileri, Yük Dengeleme, SSL/TLS sonlandırma gibi avantajları URI taşıyabilmenizi sağlar ve Yukarı Akış SSL/TLS şifrelemesi desteği sağlar. Aks'deki giriş denetleyicileri hakkında daha fazla bilgi için bkz. [NGINX yapılandırma giriş denetleyicisine AKS kümesinde] [ ingress-aks-cluster] Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede gösterilen yazılım hakkında daha fazla bilgi edinin.

- [Helm CLI][helm-cli-install]
- [NGINX giriş denetleyicisine][nginx-ingress]
- [Azure yük dengeleyici giden bağlantılar][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress-basic.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
