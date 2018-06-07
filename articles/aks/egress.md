---
title: Beyaz liste çıkış trafiği Azure Kubernetes hizmet (AKS) küme
description: Beyaz liste çıkış trafiği Azure Kubernetes hizmet (AKS) bir kümenin
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/23/2018
ms.author: nepeters
ms.openlocfilehash: 0bafb62fcdc8a24084cf7170a0e0f72bfd7824b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655357"
---
# <a name="azure-kubernetes-service-aks-egress"></a>Azure Kubernetes hizmet (AKS) çıkış

Varsayılan olarak, bir Azure Kubernetes hizmet (AKS) küme çıkış adresinden rastgele atanır. Bu yapılandırma, harici hizmetlerine erişmek için bir IP adresi tanımlamak ihtiyacı olduğunda ideal değildir. Bu belgeyi oluşturmak ve bir AKS küme bir statik olarak atanmış çıkış IP adresini korumak nasıl ayrıntılarını verir.

## <a name="egress-overview"></a>Çıkış genel bakış

AKS bir kümenin giden trafiği izleyen belgelenen Azure yük dengeleyici kuralları [burada][outbound-connections]. İlk Kubernetes hizmet türü önce `LoadBalancer` oluşturulur, aracı düğümleri tüm Azure yük dengeleyici havuzu parçası değildir. Bu yapılandırmada, bir örnek düzeyinde ortak IP adresi düğümler var. Azure yapılandırılabilir veya belirleyici olmayan ortak kaynak IP adresine giden akış çevirir.

Bir kez Kubernetes hizmet türü `LoadBalancer` oluşturulur Aracısı düğümleri, Azure yük dengeleyici havuzuna eklenir. Giden akış için Azure, yük dengeleyicide yapılandırılmış ilk genel IP adresine çevirir.

## <a name="create-a-static-public-ip"></a>Statik genel IP oluşturma

Rastgele IP adreslerini kullanılmasını önlemek için statik bir IP adresi oluşturun ve bu adresi yük dengeleyici kullandığından emin olun. IP adresi AKS oluşturulmalıdır **düğümü** kaynak grubu.

Kaynak grubu adı ile alma [az kaynak Göster] [ az-resource-show] komutu. Kaynak grubu adı ve ortamınıza uyum sağlaması için küme adını güncelleştirin.

```
$ az resource show --resource-group myResourceGroup --name myAKSCluster --resource-type Microsoft.ContainerService/managedClusters --query properties.nodeResourceGroup -o tsv

MC_myResourceGroup_myAKSCluster_eastus
```

Ardından, kullanın [az ağ genel IP oluşturun] [ public-ip-create] bir statik genel IP adresi oluşturmak için komutu. Kaynak grubu adı son adımda adı gatherred eşleşecek şekilde güncelleştirin.

```console
$ az network public-ip create --resource-group MC_myResourceGroup_myAKSCluster_eastus --name myAKSPublicIP --allocation-method static --query publicIp.ipAddress -o table

Result
-------------
23.101.128.81
```

## <a name="create-a-service-with-the-static-ip"></a>Statik IP ile hizmet oluşturma

Bir IP adresine sahip olduğunuza göre bir Kubernetes hizmet türü ile oluşturma `LoadBalancer` ve hizmet için bir IP adresi atayın.

Adlı bir dosya oluşturun `egress-service.yaml` ve aşağıdaki YAML kopyalayın. Ortamınıza uyum sağlaması için IP adresini güncelleştirin.

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

Hizmet oluşturup dağıtımı ile `kubectl apply` komutu.

```console
$ kubectl apply -f egress-service.yaml

service "aks-egress" created
```

Bu hizmet oluşturma Azure yük dengeleyici ile ilgili yeni bir ön uç IP yapılandırır. Diğer yoksa IP'leri, ardından yapılandırılmış **tüm** çıkış trafiği şimdi bu adresi kullanmalıdır. Birden çok adresi Azure yük dengeleyici üzerinde yapılandırıldığında, çıkış ilk IP üzerinde bu yük dengeleyici kullanır.

## <a name="verify-egress-address"></a>Çıkış adresini doğrulayın

Genel IP adresi kullanılmakta olduğunu doğrulamak için bir hizmet gibi kullandığınız `checkip.dyndns.org`.

Başlatma ve bir pod ekleme:

```console
$ kubectl run -it --rm aks-ip --image=debian
```

Gerekirse, curl kapsayıcısında yükleyin:

```console
$ apt-get update && apt-get install curl -y
```

Curl `checkip.dyndns.org`, çıkış IP adresi döndürür:

```console
$ curl -s checkip.dyndns.org

<html><head><title>Current IP Check</title></head><body>Current IP Address: 23.101.128.81</body></html>
```

IP adresi Azure yük dengeleyiciye bağlı statik IP adresi eşleşen görmeniz gerekir.

## <a name="ingress-controller"></a>Giriş denetleyicisi

Azure yük dengeleyici üzerinde birden çok ortak IP adresleri koruma önlemek için bir giriş denetleyicisi kullanmayı düşünün. Giriş denetleyicileri Yük Dengeleme, SSL/TLS sonlandırma gibi fayda URI yeniden yazdırmaya ve Yukarı Akış SSL/TLS şifrelemesi için destek sağlar. AKS giriş denetleyiciler hakkında daha fazla bilgi için bkz: [yapılandırma NGINX giriş denetleyicisi AKS kümedeki] [ ingress-aks-cluster] Kılavuzu.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede gösterilen yazılım hakkında daha fazla bilgi edinin.

- [Helm CLI][helm-cli-install]
- [NGINX giriş denetleyicisi][nginx-ingress]
- [Azure yük dengeleyici giden bağlantılar][outbound-connections]

<!-- LINKS - internal -->
[az-resource-show]: /cli/azure/resource#az-resource-show
[azure-cli-install]: /cli/azure/install-azure-cli
[azure-cloud-shell]: ../cloud-shell/overview.md
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[create-aks-cluster]: ./kubernetes-walkthrough.md
[helm-cli-install]: ./kubernetes-helm.md#install-helm-cli
[ingress-aks-cluster]: ./ingress.md
[outbound-connections]: ../load-balancer/load-balancer-outbound-connections.md#scenarios
[public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create

<!-- LINKS - external -->
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
