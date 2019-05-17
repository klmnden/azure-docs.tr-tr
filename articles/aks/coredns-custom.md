---
title: Azure Kubernetes Service'i (AKS) için CoreDNS özelleştirme
description: Azure Kubernetes Service (AKS) kullanarak özel DNS uç noktaları alt etki alanlarını ekleyin veya CoreDNS özelleştirmeyi öğrenin
services: container-service
author: jnoller
ms.service: container-service
ms.topic: article
ms.date: 03/15/2019
ms.author: jnoller
ms.openlocfilehash: 9c67902f8a6c10552ea60fed145afc24f82b01a1
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65594298"
---
# <a name="customize-coredns-with-azure-kubernetes-service"></a>Azure Kubernetes hizmeti ile CoreDNS özelleştirme

Azure Kubernetes Service (AKS) kullanan [CoreDNS] [ coredns] küme DNS Yönetimi ve tüm proje *1.12.x* ve daha yüksek kümeleri. Daha önce kube-dns proje kullanıldı. Bu dns kube proje artık kullanım dışı bırakılmıştır. CoreDNS özelleştirme ve Kubernetes hakkında daha fazla bilgi için bkz. [Yukarı Akış resmi belgelerine][corednsk8s].

AKS, yönetilen bir hizmet olduğundan, CoreDNS ana yapılandırması değiştirilemiyor. (bir *CoreFile*). Bunun yerine, bir Kubernetes kullandığınız *ConfigMap* varsayılan ayarları geçersiz kılmak için. ' % S'varsayılan AKS CoreDNS ConfigMaps görmek için `kubectl get configmaps coredns -o yaml` komutu.

Bu makalede aks'deki CoreDNS temel özelleştirme seçenekleri için ConfigMaps kullanmayı gösterir.

> [!NOTE]
> `kube-dns` farklı sunulan [özelleştirme seçenekleri] [ kubednsblog] Kubernetes yapılandırma eşlemesi aracılığıyla. CoreDNS olduğu **değil** kube-dns geriye dönük uyumludur. Daha önce kullandığınız tüm özelleştirmeler CoreDNS ile kullanılmak üzere güncelleştirilmelidir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. [Azure CLI kullanarak] AKS hızlı başlangıç, bir AKS kümesi gerekirse bkz. [aks-hızlı başlangıç-cli] veya [Azure portalını kullanarak] [aks-hızlı başlangıç-portal].

## <a name="what-is-supportedunsupported"></a>Desteklenen/desteklenmeyen nedir

Tüm yerleşik CoreDNS eklentileri desteklenir. Add-on/üçüncü taraf eklenti desteklenir.

## <a name="rewrite-dns"></a>DNS yeniden yazma

Sahip olduğunuz bir senaryo üzerinde halindeyken DNS adı yeniden gerçekleştirmektir. Aşağıdaki örnekte, değiştirin `<domain to be written>` kendi tam etki alanı adına sahip. Adlı bir dosya oluşturun `corednsms.json` örnek aşağıdaki yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    <domain to be rewritten>.com:53 {
        errors
        cache 30
        rewrite name substring <domain to be rewritten>.com default.svc.cluster.local
        proxy .  /etc/resolv.conf # you can redirect this to a specific DNS server such as 10.0.0.10
    }
```

ConfigMap kullanarak oluşturduğunuz [kubectl uygulamak configmap] [ kubectl-apply] komut ve YAML bildiriminizi adını belirtin:

```console
kubectl apply -f corednsms.json
```

Özelleştirmeleri uygulanıp uygulanmadığını doğrulamak için [kubectl alma configmaps] [ kubectl-get] ve belirtin, *coredns özel* ConfigMap:

```
kubectl get configmaps --namespace=kube-system coredns-custom -o yaml
```

Artık CoreDNS ConfigMap yeniden yüklemek için zorla. [Kubectl Sil pod] [ kubectl delete] komut bozucu değildir ve süresini neden olmaz. `kube-dns` Pod'ların silinir ve Kubernetes Zamanlayıcı bunları yeniden oluşturur. Bu yeni pod'ların TTL değerindeki içerir.

```console
kubectl delete pod --namespace kube-system -l k8s-app=kube-dns
```

> [!Note]
> Yukarıdaki komutu büyük/küçük harf doğrudur. Değiştirmekte olduğunuz sırada `coredns`, dağıtım altındadır **kube-dns** adı.

## <a name="custom-proxy-server"></a>Özel bir proxy sunucusu

Ağ trafiğiniz için proxy sunucusunu belirtmek gerekirse, DNS özelleştirmek için bir ConfigMap oluşturabilirsiniz. Aşağıdaki örnekte, güncelleştirme `proxy` ad ve adres için kendi ortamınızdaki değerlerle. Adlı bir dosya oluşturun `corednsms.json` örnek aşağıdaki yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: | # you may select any name here, but it must end with the .server file extension
    <domain to be rewritten>.com:53 {
        proxy foo.com 1.1.1.1
    }
```

Önceki örneklerde olduğu gibi ConfigMap kullanarak oluşturma [kubectl uygulamak configmap] [ kubectl-apply] YAML bildiriminizi adını belirtin ve komutu. Ardından, CoreDNS ConfigMap kullanarak yeniden yüklemek için zorlama [kubectl pod Sil] [ kubectl delete] Kubernetes Zamanlayıcı bunları yeniden oluşturmak için:

```console
kubectl apply -f corednsms.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="use-custom-domains"></a>Özel etki alanları kullanın

Yalnızca dahili olarak çözülebilir özel etki alanlarını yapılandırmak isteyebilirsiniz. Örneğin, özel etki alanını çözümlemek isteyebilirsiniz *puglife.local*, geçerli bir üst düzey etki alanı değil. Özel bir etki alanı ConfigMap, AKS kümesi adresi çözümlenemiyor.

Aşağıdaki örnekte, özel etki alanı ve IP adresi için trafiği için kendi ortamınızdaki değerlerle güncelleştirin. Adlı bir dosya oluşturun `corednsms.json` örnek aşağıdaki yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  puglife.server: |
    puglife.local:53 {
        errors
        cache 30
        proxy . 192.11.0.1  # this is my test/dev DNS server
    }
```

Önceki örneklerde olduğu gibi ConfigMap kullanarak oluşturma [kubectl uygulamak configmap] [ kubectl-apply] YAML bildiriminizi adını belirtin ve komutu. Ardından, CoreDNS ConfigMap kullanarak yeniden yüklemek için zorlama [kubectl pod Sil] [ kubectl delete] Kubernetes Zamanlayıcı bunları yeniden oluşturmak için:

```console
kubectl apply -f corednsms.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="stub-domains"></a>Saplama etki alanları

CoreDNS saplama etki alanlarını yapılandırmak için de kullanılabilir. Aşağıdaki örnekte, özel etki alanları ve IP adresleri için kendi ortamınızdaki değerlerle güncelleştirin. Adlı bir dosya oluşturun `corednsms.json` örnek aşağıdaki yapılandırmayı yapıştırın:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  test.server: |
    abc.com:53 {
        errors
        cache 30
        proxy . 1.2.3.4
    }
    my.cluster.local:53 {
        errors
        cache 30
        proxy . 2.3.4.5
    }

```

Önceki örneklerde olduğu gibi ConfigMap kullanarak oluşturma [kubectl uygulamak configmap] [ kubectl-apply] YAML bildiriminizi adını belirtin ve komutu. Ardından, CoreDNS ConfigMap kullanarak yeniden yüklemek için zorlama [kubectl pod Sil] [ kubectl delete] Kubernetes Zamanlayıcı bunları yeniden oluşturmak için:

```console
kubectl apply -f corednsms.json
kubectl delete pod --namespace kube-system --label k8s-app=kube-dns
```

## <a name="hosts-plugin"></a>Konaklar eklentisi

Tüm yerleşik eklentileri desteklenen CoreDNS buna [konakları] [ coredns hosts] eklentidir de özelleştirmek kullanılabilir:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom # this is the name of the configmap you can overwrite with your changes
  namespace: kube-system
data:
    test.override: |
          hosts example.hosts example.org { # example.hosts must be a file
              10.0.0.1 example.org
              fallthrough
          }
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede CoreDNS özelleştirme için bazı örnek senaryolar gösterilmiştir. CoreDNS proje hakkında daha fazla bilgi için bkz: [CoreDNS Yukarı Akış proje sayfası][coredns].

Çekirdek Ağ kavramları hakkında daha fazla bilgi edinmek için [kavramları aks'deki uygulamalar için ağ][concepts-network].

<!-- LINKS - external -->
[kubednsblog]: https://www.danielstechblog.io/using-custom-dns-server-for-domain-specific-name-resolution-with-azure-kubernetes-service/
[coredns]: https://coredns.io/
[corednsk8s]: https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/#coredns
[dnscache]: https://coredns.io/plugins/cache/
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[coredns hosts]: https://coredns.io/plugins/hosts/

<!-- LINKS - external -->
[concepts-network]: concepts-network.md
