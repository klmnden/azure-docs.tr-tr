---
title: Azure Kubernetes Service (AKS) bir iç ağ için bir giriş denetleyicisini oluşturma
description: Yükleme ve bir NGINX giriş denetleyicisine iç, özel bir ağ için bir Azure Kubernetes Service (AKS) kümesi içinde yapılandırma hakkında bilgi edinin.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: mlearned
ms.openlocfilehash: 935b96bd553c9ae73b55086483baa0ea7c4aeaa4
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67615461"
---
# <a name="create-an-ingress-controller-to-an-internal-virtual-network-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) bir giriş denetleyicisine bir iç sanal ağ oluşturma

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir IP adresi için bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Bu makalede nasıl dağıtılacağı gösterilir [NGINX giriş denetleyicisine][nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. Giriş denetleyicisine, bir özel, iç sanal ağ ve IP adresi üzerinde yapılandırılır. Hiçbir dış erişim izni verilir. İki uygulama, ardından her biri tek bir IP adresi erişilebilir, bir AKS kümesinde çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
- Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresine sahip][aks-ingress-tls] or [with a static public IP address][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisine, Sertifika Yöneticisi ve örnek bir web uygulamasını yüklemek için Helm kullanır. AKS kümenizi içinde başlatılan ve bir hizmet hesabı için Tiller kullanarak Helm olması gerekir. Yapılandırma ve Helm kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][use-helm].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.64 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Bir giriş denetleyicisini oluşturma

Varsayılan olarak, bir NGINX giriş denetleyicisine bir dinamik genel IP adresi ataması ile oluşturulur. Bir özel, iç ağ ve IP adresi, ortak bir yapılandırma gereksinimi kullanmaktır. Bu yaklaşım, iç kullanıcılar, dış erişim olmaksızın hizmetlerinize erişimi sınırlamak sağlar.

Adlı bir dosya oluşturun *ingress.yaml iç* aşağıdaki örnek bildirim dosyası kullanma. Bu örnekte atar *10.240.0.42* için *loadBalancerIP* kaynak. Giriş denetleyicisine ile kullanmak için kendi iç IP adresi sağlayın. Bu IP adresi sanal ağınız içinde kullanılıyor değil emin olun.

```yaml
controller:
  service:
    loadBalancerIP: 10.240.0.42
    annotations:
      service.beta.kubernetes.io/azure-load-balancer-internal: "true"
```

Şimdi Dağıt *ngınx giriş* Helm grafiği. Önceki adımda oluşturduğunuz bildirim dosyası kullanmak için ekleme `-f internal-ingress.yaml` parametresi. Eklenen yedeklilik için NGINX giriş denetleyicilerinin iki çoğaltma ile dağıtılan `--set controller.replicaCount` parametresi. Giriş denetleyicisine çoğaltmalarını çalışmasını tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Giriş denetleyicisine ayrıca Linux düğümde zamanlanması gerekir. Windows Server düğümleri (şu anda önizlemede aks'deki) giriş denetleyicisine çalıştırmamalısınız. Bir düğüm seçiciyi kullanarak belirtilen `--set nodeSelector` NGINX giriş denetleyicisine Linux tabanlı bir düğümde çalıştırılacak Kubernetes Zamanlayıcı bildirmek için parametre.

> [!TIP]
> Aşağıdaki örnek, bir Kubernetes ad alanı adlı giriş kaynakları oluşturur *giriş temel*. Bir ad alanı, kendi ortamınız için gerektiği şekilde belirtin. AKS kümenizi RBAC etkin değilse, ekleme `--set rbac.create=false` Helm komutlar.

> [!TIP]
> Etkinleştirmek istiyorsanız, [istemci kaynak IP korunması][client-source-ip] kümenizde kapsayıcıları istekleri için ekleme `--set controller.service.externalTrafficPolicy=Local` için Helm install komutu. IP istek üst bilgisi altında depolanan istemci kaynak *X-iletilen-için*. Giriş denetleyicisine istemci kaynak IP koruma etkin ile kullanırken SSL doğrudan çalışmaz.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    -f internal-ingress.yaml \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

NGINX giriş denetleyici için Kubernetes Yük Dengeleyici Hizmeti oluşturulduğunda, iç IP adresiniz, aşağıdaki örnek çıktıda gösterildiği gibi atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                              TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
alternating-coral-nginx-ingress-controller        LoadBalancer   10.0.97.109   10.240.0.42   80:31507/TCP,443:30707/TCP   1m
alternating-coral-nginx-ingress-default-backend   ClusterIP      10.0.134.66   <none>        80/TCP                       1m
```

İç IP adresine göz atın NGINX giriş denetleyicisinin varsayılan 404 sayfa görüntülenir bu nedenle hiçbir giriş kuralları henüz oluşturulmadı. Aşağıdaki adımlarda yapılandırılmış giriş kural.

## <a name="run-demo-applications"></a>Tanıtım uygulamaları

Giriş denetleyicisine iş başında görmek için iki demo uygulamayı AKS kümenizin çalıştıralım. Bu örnekte, iki basit bir 'Merhaba Dünya' uygulaması örneğini dağıtmak için Helm kullanılır.

Örnek Helm grafikleri yükleyebilmek için önce Azure örnekleri deposu Helm ortamınıza aşağıdaki şekilde ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Aşağıdaki komutla bir Helm grafiği ilk demo uygulamasını oluşturun:

```console
helm install azure-samples/aks-helloworld --namespace ingress-basic
```

Şimdi ikinci bir örneğini demo uygulamasını yükleyin. İkinci örnek için iki uygulama görsel olarak benzersiz olacak şekilde yeni bir başlık belirtin. Ayrıca bir benzersiz bir hizmet ad belirtin:

```console
helm install azure-samples/aks-helloworld \
    --namespace ingress-basic \
    --set title="AKS Ingress Demo" \
    --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Bir giriş yol oluşturma

Her iki uygulama Kubernetes kümeniz artık çalışıyor. Her uygulama için trafiği yönlendirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, iki uygulama birine trafik yönlendirme kuralları yapılandırır.

Aşağıdaki örnekte, adres için trafiği `http://10.240.0.42/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `http://10.240.0.42/hello-world-two` yönlendirilir `ingress-demo` hizmeti.

Adlı bir dosya oluşturun `hello-world-ingress.yaml` ve aşağıdaki örnekte YAML kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  rules:
  - http:
      paths:
      - backend:
          serviceName: aks-helloworld
          servicePort: 80
        path: /(.*)
      - backend:
          serviceName: ingress-demo
          servicePort: 80
        path: /hello-world-two(/|$)(.*)
```

Giriş kullanarak kaynak oluşturma `kubectl apply -f hello-world-ingress.yaml` komutu.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Giriş denetleyicisine test

Yollar için giriş denetleyicisini test etmek için bir web istemcisi ile iki uygulamalara göz atın. Gerekirse, bu yalnızca dahili işlevinden bir AKS kümesi pod hızlıca test edebilirsiniz. Bir test pod oluşturun ve terminal oturumu ekler:

```console
kubectl run -it --rm aks-ingress-test --image=debian --namespace ingress-basic
```

Yükleme `curl` pod kullanarak `apt-get`:

```console
apt-get update && apt-get install -y curl
```

Şimdi, Kubernetes giriş denetleyicisini kullanarak adresi erişim `curl`, gibi *http://10.240.0.42* . Bu makalede, ilk adımda giriş denetleyicisine dağıttığınızda kendi iç IP adresi belirtilen sağlar.

```console
curl -L http://10.240.0.42
```

Ek yol içermeyen adresiyle, bu nedenle giriş denetleyicisine varsayılan olarak sağlandı */* rota. İlk demo uygulamasını aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi verilir:

```
$ curl -L 10.240.0.42

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

Şimdi ekleyin */hello-world-two* adresi yolu gibi *http://10.240.0.42/hello-world-two* . Aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi özel başlıklı ikinci demo uygulamasını döndürülür:

```
$ curl -L -k http://10.240.0.42/hello-world-two

<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>AKS Ingress Demo</title>
[...]
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu makalede, örnek uygulamaları ve giriş bileşenleri yüklemek için Helm kullanılır. Kubernetes kaynak sayısı, bir Helm grafiği dağıttığınızda oluşturulur. Bu kaynaklar, pod'ları, dağıtımlar ve hizmetleri içerir. Bu kaynakları temizlemek için ya da tüm örnek ad alanı veya tek tek kaynakları silebilirsiniz.

### <a name="delete-the-sample-namespace-and-all-resources"></a>Örnek ad alanı ve tüm kaynakları silme

Tüm örnek ad alanı silmek için kullanın `kubectl delete` komut ve ad alanı adınızı belirtin. Ad alanındaki tüm kaynaklar silinir.

```console
kubectl delete namespace ingress-basic
```

Ardından, AKS Merhaba Dünya uygulaması için Helm deposu kaldırın:

```console
helm repo remove azure-samples
```

### <a name="delete-resources-individually"></a>Tek tek kaynakları silme

Alternatif olarak, daha ayrıntılı bir yaklaşım oluşturulan kaynakların silmektir. Helm sürümleri ile liste `helm list` komutu. Adlı grafiklerde Ara *ngınx giriş* ve *aks-helloworld*aşağıdaki örnek çıktıda gösterildiği gibi:

```
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
kissing-ferret      1           Tue Oct 16 17:13:39 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
intended-lemur      1           Tue Oct 16 17:20:59 2018    DEPLOYED    aks-helloworld-0.1.0                default
pioneering-wombat   1           Tue Oct 16 17:21:05 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Sürümlerle Sil `helm delete` komutu. Aşağıdaki örnek, NGINX giriş dağıtım ve iki örnek AKS hello world uygulaması siler.

```
$ helm delete kissing-ferret intended-lemur pioneering-wombat

release "kissing-ferret" deleted
release "intended-lemur" deleted
release "pioneering-wombat" deleted
```

Ardından, AKS Merhaba Dünya uygulaması için Helm deposu kaldırın:

```console
helm repo remove azure-samples
```

Örnek uygulamalara yönelik trafiği yönlendiren giriş rota kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
```

Son olarak, kendi ad alanı silebilirsiniz. Kullanım `kubectl delete` komut ve ad alanı adınızı belirtin:

```console
kubectl delete namespace ingress-basic
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS dış bazı bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için şu proje sayfalara bakın:

- [Helm CLI][helm-cli]
- [NGINX giriş denetleyicisine][nginx-ingress]

Aşağıdakileri de yapabilirsiniz:

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Dinamik genel IP ile bir giriş denetleyicisi oluşturmak ve TLS sertifikalarını otomatik olarak oluşturmak için şimdi şifreleme yapılandırma][aks-ingress-tls]
- [Bir statik genel IP adresiyle bir giriş denetleyicisi oluşturmak ve TLS sertifikalarını otomatik olarak oluşturmak için şimdi şifreleme yapılandırma][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers