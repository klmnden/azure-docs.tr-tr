---
title: Azure Kubernetes Service (AKS) temel giriş denetleyicisini oluşturma
description: Yükleme ve bir Azure Kubernetes Service (AKS) kümesi içinde temel bir NGINX giriş denetleyicisine yapılandırma hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 03/27/2019
ms.author: iainfou
ms.openlocfilehash: fd9695698f90a1efebb71a2b24a196dd8c911081
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466723"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) giriş denetleyicisini oluşturma

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir IP adresi için bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Bu makalede nasıl dağıtılacağı gösterilir [NGINX giriş denetleyicisine] [ nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. İki uygulama, ardından her biri tek bir IP adresi erişilebilir, bir AKS kümesinde çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
- Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresi ile] [ aks-ingress-tls] veya [bir statik genel IP adresi ile][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisine, Sertifika Yöneticisi ve örnek bir web uygulamasını yüklemek için Helm kullanır. AKS kümenizi içinde başlatılan ve bir hizmet hesabı için Tiller kullanarak Helm olması gerekir. Yapılandırma ve Helm kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][use-helm].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.61 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Bir giriş denetleyicisini oluşturma

Giriş denetleyicisine oluşturmak için kullanın `Helm` yüklemek için *ngınx giriş*. Eklenen yedeklilik için NGINX giriş denetleyicilerinin iki çoğaltma ile dağıtılan `--set controller.replicaCount` parametresi. Giriş denetleyicisine çoğaltmalarını çalışmasını tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

> [!TIP]
> Aşağıdaki örnek, bir Kubernetes ad alanı adlı giriş kaynakları oluşturur *giriş temel*. Bir ad alanı, kendi ortamınız için gerektiği şekilde belirtin. AKS kümenizi RBAC etkin değilse, ekleme `--set rbac.create=false` Helm komutlar.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress --namespace ingress-basic --set controller.replicaCount=2
```

NGINX giriş denetleyici için Kubernetes Yük Dengeleyici Hizmeti oluşturulduğunda, dinamik genel IP adresi, aşağıdaki örnek çıktıda gösterildiği gibi atanır:

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)                      AGE
aspiring-labradoodle-nginx-ingress-controller        LoadBalancer   10.0.61.144    40.117.74.8   80:30386/TCP,443:32276/TCP   6m2s
aspiring-labradoodle-nginx-ingress-default-backend   ClusterIP      10.0.192.145   <none>        80/TCP                       6m2s
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

Aşağıdaki örnekte, adres için trafiği `http://40.117.74.8/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `http://40.117.74.8/hello-world-two` yönlendirilir `ingress-demo` hizmeti.

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
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

Giriş kullanarak kaynak oluşturma `kubectl apply -f hello-world-ingress.yaml` komutu.

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>Giriş denetleyicisine test

Yollar için giriş denetleyicisini test etmek için iki uygulamalarına göz atın. NGINX giriş denetleyicinizin IP adresini bir web tarayıcısı gibi açın *http://40.117.74.8*. İlk demo uygulamayı web tarayıcısında aşağıdaki örnekte gösterildiği gibi görüntülenir:

![İlk uygulamanızı giriş denetleyicisine çalıştırma](media/ingress-basic/app-one.png)

Şimdi ekleyin */hello-world-two* gibi yolu IP adresi *http://40.117.74.8/hello-world-two*. Özel başlıklı ikinci demo uygulamasını görüntülenir:

![Giriş denetleyicisine çalışan ikinci uygulama](media/ingress-basic/app-two.png)

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

NAME                    REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
aspiring-labradoodle    1           Wed Mar 27 19:55:37 2019    DEPLOYED    nginx-ingress-1.3.1     0.22.0      ingress-basic
esteemed-koala          1           Wed Mar 27 19:59:18 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
wonderful-puma          1           Wed Mar 27 19:59:07 2019    DEPLOYED    aks-helloworld-0.1.0                ingress-basic
```

Sürümlerle Sil `helm delete` komutu. Aşağıdaki örnek, NGINX giriş dağıtım ve iki örnek AKS hello world uygulaması siler.

```
$ helm delete aspiring-labradoodle esteemed-koala wonderful-puma

release "aspiring-labradoodle" deleted
release "esteemed-koala" deleted
release "wonderful-puma" deleted
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

- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- [Kendi TLS sertifikalarını kullanan bir giriş denetleyicisini oluşturma][aks-ingress-own-tls]
- Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresi ile] [ aks-ingress-tls] veya [bir statik genel IP adresi ile][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
