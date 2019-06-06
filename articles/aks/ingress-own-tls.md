---
title: Azure Kubernetes Service (AKS) kümesini ile giriş için kendi TLS sertifikaları kullanın
description: Yükleme ve bir Azure Kubernetes Service (AKS) kümesi içinde kendi sertifikalarınızı kullanan bir NGINX giriş denetleyicisine yapılandırma hakkında bilgi edinin.
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/24/2019
ms.author: iainfou
ms.openlocfilehash: eebc484351714c7a30f65e61434076fcde175a8d
ms.sourcegitcommit: 087ee51483b7180f9e897431e83f37b08ec890ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66430946"
---
# <a name="create-an-https-ingress-controller-and-use-your-own-tls-certificates-on-azure-kubernetes-service-aks"></a>Bir HTTPS giriş denetleyicisi oluşturmak ve kendi TLS sertifikalarını Azure Kubernetes Service (AKS) kullanma

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir IP adresi için bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Bu makalede nasıl dağıtılacağı gösterilir [NGINX giriş denetleyicisine] [ nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. Kendi sertifikalarınızı oluşturmak ve bir Kubernetes giriş rota ile kullanım için gizli anahtar oluşturun. Son olarak, iki uygulama, her biri tek bir IP adresi erişilebilir, bir AKS kümesinde çalıştırılır.

Aşağıdakileri de yapabilirsiniz:

- [Dış ağ bağlantısına sahip bir temel giriş denetleyicisi oluşturun][aks-ingress-basic]
- [HTTP uygulama yönlendirme eklentiyi etkinleştir][aks-http-app-routing]
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresi ile] [ aks-ingress-tls] veya [bir statik genel IP adresi ile][aks-ingress-static-tls]

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisine ve örnek bir web uygulamasını yüklemek için Helm kullanır. AKS kümenizi içinde başlatılan ve bir hizmet hesabı için Tiller kullanarak Helm olması gerekir. Helm en son sürümünü kullandığınızdan emin olun. Yükseltme yönergeleri için bkz. [Helm yükleme docs][helm-install]. Yapılandırma ve Helm kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][use-helm].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.64 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-an-ingress-controller"></a>Bir giriş denetleyicisini oluşturma

Giriş denetleyicisine oluşturmak için kullanın `Helm` yüklemek için *ngınx giriş*. Eklenen yedeklilik için NGINX giriş denetleyicilerinin iki çoğaltma ile dağıtılan `--set controller.replicaCount` parametresi. Giriş denetleyicisine çoğaltmalarını çalışmasını tam olarak yararlanmak için AKS kümenizde birden fazla düğüm olduğundan emin olun.

Giriş denetleyicisine ayrıca Linux düğümde zamanlanması gerekir. Windows Server düğümleri (şu anda önizlemede aks'deki) giriş denetleyicisine çalıştırmamalısınız. Bir düğüm seçiciyi kullanarak belirtilen `--set nodeSelector` NGINX giriş denetleyicisine Linux tabanlı bir düğümde çalıştırılacak Kubernetes Zamanlayıcı bildirmek için parametre.

> [!TIP]
> Aşağıdaki örnek, bir Kubernetes ad alanı adlı giriş kaynakları oluşturur *giriş temel*. Bir ad alanı, kendi ortamınız için gerektiği şekilde belirtin. AKS kümenizi RBAC etkin değilse, ekleme `--set rbac.create=false` Helm komutlar.

> [!TIP]
> Etkinleştirmek istiyorsanız, [istemci kaynak IP korunması] [ client-source-ip] kümenizde kapsayıcıları istekleri için ekleme `--set controller.service.externalTrafficPolicy=Local` için Helm install komutu. IP istek üst bilgisi altında depolanan istemci kaynak *X-iletilen-için*. Giriş denetleyicisine istemci kaynak IP koruma etkin ile kullanırken SSL doğrudan çalışmaz.

```console
# Create a namespace for your ingress resources
kubectl create namespace ingress-basic

# Use Helm to deploy an NGINX ingress controller
helm install stable/nginx-ingress \
    --namespace ingress-basic \
    --set controller.replicaCount=2 \
    --set controller.nodeSelector."beta\.kubernetes\.io/os"=linux \
    --set defaultBackend.nodeSelector."beta\.kubernetes\.io/os"=linux
```

Yükleme sırasında bir Azure genel IP adresi için giriş denetleyicisini oluşturulur. Bu genel IP adresi-ömrü için giriş denetleyicisini statiktir. Giriş denetleyicisine silerseniz, genel IP adresi ataması kaybolur. Ardından bir ek giriş denetleyicisine oluşturursanız, yeni bir ortak IP adresi atanır. Genel IP adresi kullanımını korumak istiyorsanız, bunun yerine yapabilecekleriniz [giriş denetleyicisine statik bir genel IP adresiyle oluşturma][aks-ingress-static-tls].

Genel IP adresini almak için kullanın `kubectl get service` komutu. Hizmete atanan IP adresi için birkaç dakika sürer.

```
$ kubectl get service -l app=nginx-ingress --namespace ingress-basic

NAME                                          TYPE           CLUSTER-IP    EXTERNAL-IP    PORT(S)                      AGE
virulent-seal-nginx-ingress-controller        LoadBalancer   10.0.48.240   40.87.46.190   80:31159/TCP,443:30657/TCP   7m
virulent-seal-nginx-ingress-default-backend   ClusterIP      10.0.50.5     <none>         80/TCP                       7m
```

Son adımda, dağıtımı test etmek için kullanılır, bu genel IP adresini not edin.

Hiçbir giriş kuralları henüz oluşturulmamış. Genel IP adresine göz atarsanız, NGINX giriş denetleyicinin varsayılan 404 sayfası görüntülenir.

## <a name="generate-tls-certificates"></a>TLS sertifikalarını oluşturmak

Bu makale için şimdi ile otomatik olarak imzalanan bir sertifika oluşturmak `openssl`. Üretim kullanımı için bir sağlayıcı ya da kendi sertifika yetkilisinden (CA) aracılığıyla güvenilen, imzalanmış bir sertifika istemeniz gerekir. Sonraki adımda, bir Kubernetes oluşturmak *gizli* OpenSSL tarafından oluşturulan özel anahtarı ve TLS sertifikası kullanarak.

Aşağıdaki örnek, bir X 509 sertifikası geçerli adlı 365 gün için 2048 bit RSA oluşturur *aks giriş tls.crt*. Özel anahtar dosyası adlı *aks giriş tls.key*. Kubernetes TLS gizli dizi, bu dosyaların her ikisini de gerektirir.

Bu makalede çalışır *demo.azure.com* konu ortak adı ve değiştirilmesi gerekmez. Üretim kullanımı için kuruluş kendi değerlerini belirtin `-subj` parametresi:

```console
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -out aks-ingress-tls.crt \
    -keyout aks-ingress-tls.key \
    -subj "/CN=demo.azure.com/O=aks-ingress-tls"
```

## <a name="create-kubernetes-secret-for-the-tls-certificate"></a>Kubernetes için TLS sertifikası gizli dizisi oluşturma

Kubernetes TLS sertifikası ve özel anahtar için giriş denetleyicisini kullanmak izin vermek için oluşturma ve bir gizli diziyi kullanın. Gizli dizi çok kez tanımlanmış ve sertifika ve önceki adımda oluşturduğunuz anahtar dosyası kullanır. Giriş yollar tanımlarken, ardından bu gizli dizi başvurursunuz.

Aşağıdaki örnek, bir gizli dizi adı oluşturur *aks giriş tls*:

```console
kubectl create secret tls aks-ingress-tls \
    --namespace ingress-basic \
    --key aks-ingress-tls.key \
    --cert aks-ingress-tls.crt
```

## <a name="run-demo-applications"></a>Tanıtım uygulamaları

Giriş denetleyicisine ve gizli dizi, sertifika ile yapılandırıldı. Şimdi, AKS kümesinde uygulamaları çalıştırma iki deneme şimdi. Bu örnekte, iki basit bir 'Merhaba Dünya' uygulaması örneğini dağıtmak için Helm kullanılır.

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

Türünde bir hizmet yapılandırılmışsa ancak her iki uygulamaları artık Kubernetes kümenizde çalışan `ClusterIP`. Bu nedenle, uygulamaların internet'ten erişilemez. Genel olarak kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, iki uygulama birine trafik yönlendirme kuralları yapılandırır.

Aşağıdaki örnekte, adres için trafiği `https://demo.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `https://demo.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmeti. Bu makale için bu demo ana bilgisayar adlarını değiştirmeniz gerekmez. Üretim kullanımı için sertifika isteği ve oluşturma işleminin bir parçası belirtilen adları sağlar.

> [!TIP]
> Sertifika isteği işlemi sırasında CN adı belirtilen ana bilgisayar adı, giriş denetleyicisine görüntüler, giriş yolda tanımlanan konak eşleşmiyor ise bir *Kubernetes giriş denetleyicisine sahte sertifika*. Sertifika ve giriş rotanız emin olun. ana bilgisayar adları eşleşmiyor.

*Tls* bölümü adlı gizli dizi kullanmak için giriş rota söyler *aks giriş tls* konak *demo.azure.com*. Yeniden üretim sırasında kullanım için kendi ana bilgisayar adresi belirtin.

Adlı bir dosya oluşturun `hello-world-ingress.yaml` ve aşağıdaki örnekte YAML kopyalayın.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  namespace: ingress-basic
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/rewrite-target: /$1
spec:
  tls:
  - hosts:
    - demo.azure.com
    secretName: aks-ingress-tls
  rules:
  - host: demo.azure.com
    http:
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

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Bizim sahte sertifikalarla test etmek için *demo.azure.com* barındırmak, kullanın `curl` belirtin *--çözmek* parametresi. Bu parametre eşlemenize olanak tanır *demo.azure.com* giriş denetleyicinizin genel IP adresi için ad. Aşağıdaki örnekte gösterildiği gibi kendi giriş denetleyicisine genel IP adresini belirtin:

```
curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com
```

Ek yol içermeyen adresiyle, bu nedenle giriş denetleyicisine varsayılan olarak sağlandı */* rota. İlk demo uygulamasını aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi verilir:

```
$ curl -v -k --resolve demo.azure.com:443:40.87.46.190 https://demo.azure.com

[...]
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <link rel="stylesheet" type="text/css" href="/static/default.css">
    <title>Welcome to Azure Kubernetes Service (AKS)</title>
[...]
```

*- V* parametresinde bizim `curl` komut alınan TLS sertifika dahil olmak üzere ayrıntılı bilgiler çıkarır. Yarı-şekilde curl çıkışınızı aracılığıyla kendi TLS sertifikası kullanıldığını doğrulayabilirsiniz. *-K* parametresi devam otomatik olarak imzalanan bir sertifika kullanıyoruz olsa da sayfa yükleniyor. Aşağıdaki örnek, gösterir *veren: CN=Demo.Azure.com; O aks giriş tls =* sertifika kullanıldı:

```
[...]
* Server certificate:
*  subject: CN=demo.azure.com; O=aks-ingress-tls
*  start date: Oct 22 22:13:54 2018 GMT
*  expire date: Oct 22 22:13:54 2019 GMT
*  issuer: CN=demo.azure.com; O=aks-ingress-tls
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
[...]
```

Şimdi ekleyin */hello-world-two* adresi yolu gibi `https://demo.azure.com/hello-world-two`. Aşağıdaki sıkıştırılmış örneğe çıktıda gösterildiği gibi özel başlıklı ikinci demo uygulamasını döndürülür:

```
$ curl -v -k --resolve demo.azure.com:443:137.117.36.18 https://demo.azure.com/hello-world-two

[...]
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

NAME            REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
virulent-seal   1           Tue Oct 23 16:37:24 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
billowing-guppy 1           Tue Oct 23 16:41:38 2018    DEPLOYED    aks-helloworld-0.1.0                default
listless-quokka 1           Tue Oct 23 16:41:30 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

Sürümlerle Sil `helm delete` komutu. Aşağıdaki örnek, NGINX giriş dağıtım ve iki örnek AKS hello world uygulaması siler.

```
$ helm delete virulent-seal billowing-guppy listless-quokka

release "virulent-seal" deleted
release "billowing-guppy" deleted
release "listless-quokka" deleted
```

Ardından, AKS Merhaba Dünya uygulaması için Helm deposu kaldırın:

```console
helm repo remove azure-samples
```

Örnek uygulamalara yönelik trafiği yönlendiren giriş rota kaldırın:

```console
kubectl delete -f hello-world-ingress.yaml
```

Sertifika gizli anahtarı silin:

```console
kubectl delete secret aks-ingress-tls
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
- [Bir özel, iç ağ ve IP adresi kullanan bir giriş denetleyicisini oluşturma][aks-ingress-internal]
- Şimdi şifreleme TLS sertifikalarını otomatik olarak oluşturmak için kullandığı bir giriş denetleyicisine oluşturma [dinamik genel IP adresi ile] [ aks-ingress-tls] veya [bir statik genel IP adresi ile][aks-ingress-static-tls]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-show]: /cli/azure/aks#az-aks-show
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-tls]: ingress-tls.md
[client-source-ip]: concepts-network.md#ingress-controllers