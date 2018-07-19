---
title: Azure Kubernetes Service (AKS) kümesi ile giriş yapılandırma
description: Yükleme ve şimdi şifrelemek için Azure Kubernetes Service (AKS) kümesini otomatik SSL sertifikası oluşturma kullanan bir NGINX giriş denetleyicisine yapılandırma hakkında bilgi edinin.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 07/17/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d31a3e62aaabf7a865078aa2e7c6d1585466b379
ms.sourcegitcommit: b9786bd755c68d602525f75109bbe6521ee06587
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2018
ms.locfileid: "39126686"
---
# <a name="deploy-an-https-ingress-controller-on-azure-kubernetes-service-aks"></a>HTTPS giriş denetleyicisine Azure Kubernetes Service'teki (AKS) dağıtma

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir dış adresi bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Bu makalede nasıl dağıtılacağı gösterilir [NGINX giriş denetleyicisine] [ nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. [Sertifika Yöneticisi] [ cert-manager] proje otomatik olarak oluşturmak ve yapılandırmak için kullanılan [şimdi şifrelemek] [ lets-encrypt] sertifikaları. Son olarak, bazı uygulamalar, her biri tek bir adresi erişilebilir, bir AKS kümesinde çalıştırılır.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, NGINX giriş denetleyicisine, Sertifika Yöneticisi ve örnek bir web uygulamasını yüklemek için Helm kullanır. AKS kümenizi içinde başlatılan ve bir hizmet hesabı için Tiller kullanarak Helm olması gerekir. Yapılandırma ve Helm kullanma hakkında daha fazla bilgi için bkz. [Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme][use-helm].

Bu makalede, ayrıca Azure CLI Sürüm 2.0.41 çalıştırdığınız gerektirir veya üzeri. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="install-an-ingress-controller"></a>Giriş denetleyicisine yükleyin

NGINX giriş denetleyicisini yüklemek için Helm kullanın. Ayrıntılı dağıtım için bilgi [NGINX giriş denetleyicisine belgeleri][nginx-ingress].

Aşağıdaki örnek, denetleyicide yükler `kube-system` ad alanı. Farklı bir ad alanı için ortamınızda belirtebilirsiniz. AKS kümenizi RBAC etkin değilse, ekleme `--set rbac.create=false` komutu.

```console
helm install stable/nginx-ingress --namespace kube-system
```

Yükleme sırasında bir Azure genel IP adresi için giriş denetleyicisini oluşturulur. Genel IP adresini almak için kullanın `kubectl get service` komutu. Hizmete atanan IP adresi için birkaç dakika sürer.

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Hiçbir giriş kuralları henüz oluşturulmamış. Genel IP adresine göz atarsanız, NGINX giriş denetleyicinin varsayılan 404 sayfa, aşağıdaki örnekte gösterildiği gibi görüntülenir:

![Varsayılan NGINX arka uç](media/ingress/default-back-end.png)

## <a name="configure-a-dns-name"></a>Bir DNS adı yapılandırma

HTTPS sertifikaları düzgün çalışması bir FQDN giriş denetleyicisine IP adresi için yapılandırın. Aşağıdaki komut dosyası, giriş denetleyicisine FQDN için kullanmak istediğiniz benzersiz bir ad ve IP adresiyle güncelleştirin:

```console
#!/bin/bash

# Public IP address of your ingress controller
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with DNS name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Giriş denetleyicisine şimdi FQDN'si erişilebilir.

## <a name="install-cert-manager"></a>Sertifika Yöneticisi'ni yükleyin

NGINX giriş denetleyicisine TLS sonlandırma destekler. Almak ve HTTPS için sertifikaları yapılandırmanız için birkaç yolu vardır. Bu makalede kullanmayı gösterir [Sertifika Yöneticisi][cert-manager], otomatik sağlayan [sağlar şifrelemek] [ lets-encrypt] sertifika oluşturma ve Yönetim işlevselliği.

> [!NOTE]
> Bu makalede `staging` şimdi şifrelemek için ortamı. Üretim dağıtımında kullanmak `letsencrypt-prod` ve `https://acme-v02.api.letsencrypt.org/directory` kaynak tanımlarında ve Helm grafiği yüklerken.

Sertifika Yöneticisi denetleyicisi RBAC özellikli bir kümede yüklemek için aşağıdakileri kullanın `helm install` komutu:

```console
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-staging --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Bunun yerine, kümenizin RBAC etkin değilse, aşağıdaki komutu kullanın:

```console
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-staging \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Sertifika Yöneticisi yapılandırma hakkında daha fazla bilgi için bkz. [Sertifika Yöneticisi proje][cert-manager].

## <a name="create-a-ca-cluster-issuer"></a>Veren CA küme oluşturma

Sertifika Yöneticisi sertifika verilmeden önce gerektiren bir [veren] [ cert-manager-issuer] veya [ClusterIssuer] [ cert-manager-cluster-issuer] kaynak. Bu Kubernetes kaynakları işlevselliği, ancak özdeş `Issuer` tek bir ad alanında çalışır ve `ClusterIssuer` tüm ad alanları geneline çalışır. Daha fazla bilgi için [Sertifika Yöneticisi veren] [ cert-manager-issuer] belgeleri.

Bir küme veren gibi oluşturma `cluster-issuer.yaml`, aşağıdaki örnekte bildirimi kullanarak. Kuruluşunuzun geçerli bir adresten e-posta adresiyle güncelleştirme:

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-staging
spec:
  acme:
    server: https://acme-staging-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-staging
    http01: {}
```

Verici oluşturmak için kullanın `kubectl create -f cluster-issuer.yaml` komutu.

```
$ kubectl create -f cluster-issuer.yaml

clusterissuer.certmanager.k8s.io/letsencrypt-prod created
```

## <a name="create-a-certificate-object"></a>Bir sertifika nesnesi oluşturun

Ardından, sertifika kaynak oluşturulması gerekir. Sertifika kaynak istenen X.509 sertifikası tanımlar. Daha fazla bilgi için [Sertifika Yöneticisi sertifika][cert-manager-certificates].

Sertifika kaynak gibi oluşturma `certificates.yaml`, aşağıdaki örnekte bildirime sahip. Güncelleştirme *dnsNames* ve *etki alanları* bir önceki adımda oluşturduğunuz DNS adı.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: Certificate
metadata:
  name: tls-secret
spec:
  secretName: tls-secret
  dnsNames:
  - demo-aks-ingress.eastus.cloudapp.azure.com
  acme:
    config:
    - http01:
        ingressClass: nginx
      domains:
      - demo-aks-ingress.eastus.cloudapp.azure.com
  issuerRef:
    name: letsencrypt-staging
    kind: ClusterIssuer
```

Sertifika kaynak oluşturmak için kullanın `kubectl create -f certificates.yaml` komutu.

```
$ kubectl create -f certificates.yaml

certificate.certmanager.k8s.io/tls-secret created
```

## <a name="run-demo-applications"></a>Tanıtım uygulamaları

Giriş denetleyicisine ve sertifika yönetimi çözümü yapılandırıldı. Şimdi, AKS kümesinde uygulamaları çalıştırma iki deneme şimdi. Bu örnekte, iki basit bir 'Merhaba Dünya' uygulaması örneğini dağıtmak için Helm kullanılır.

Örnek Helm grafikleri yükleyebilmek için önce Azure örnekleri deposu Helm ortamınıza aşağıdaki şekilde ekleyin:

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

Aşağıdaki komutla bir Helm grafiği ilk demo uygulamasını oluşturun:

```console
helm install azure-samples/aks-helloworld
```

Şimdi ikinci bir örneğini demo uygulamasını yükleyin. İkinci örnek için iki uygulama görsel olarak benzersiz olacak şekilde yeni bir başlık belirtin. Ayrıca bir benzersiz bir hizmet ad belirtin:

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>Bir giriş yol oluşturma

Türünde bir hizmet yapılandırılmışsa ancak her iki uygulamaları artık Kubernetes kümenizde çalışan `ClusterIP`. Bu nedenle, uygulamaların internet'ten erişilemez. Genel olarak kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, iki uygulama birine trafik yönlendirme kuralları yapılandırır.

Aşağıdaki örnekte, adres için trafiği `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmeti. Güncelleştirme *konakları* ve *konak* bir önceki adımda oluşturduğunuz DNS adı.

Adlı bir dosya oluşturun `hello-world-ingress.yaml` ve aşağıdaki örnekte YAML kopyalayın:

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-staging
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  tls:
  - hosts:
    - demo-aks-ingress.eastus.cloudapp.azure.com
    secretName: tls-secret
  rules:
  - host: demo-aks-ingress.eastus.cloudapp.azure.com
    http:
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

Giriş kullanarak kaynak oluşturma `kubectl create -f hello-world-ingress.yaml` komutu.

```
$ kubectl create -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Kubernetes giriş denetleyicinizin FQDN'si için bir web tarayıcısı gibi açın *https://demo-aks-ingress.eastus.cloudapp.azure.com*.

Bu örneklerde gibi `letsencrypt-staging`, tarayıcı tarafından verilen bir SSL sertifikası güvenilir değil. Uygulamanıza devam etmek için herhangi bir uyarıyı kabul edin. Bu sertifika bilgilerini gösterir *sahte LE Ara X1* şimdi şifrelemek tarafından sertifikanın verildiği. Bu sahte bir sertifika gösterir `cert-manager` istek doğru şekilde işlenir ve bir sertifika sağlayıcıdan alınan:

![Şimdi hazırlama sertifika şifreleme](media/ingress/staging-certificate.png)

Şimdi kullanmak için şifrelemek değiştirdiğinizde `prod` yerine `staging`, aşağıdaki örnekte gösterildiği gibi şimdi şifrelemek tarafından verilen, güvenilen bir sertifika kullanılır:

![Şimdi sertifika şifreleme](media/ingress/certificate.png)

Demo uygulamayı web tarayıcısında gösterilmektedir:

![Bir uygulama örneği](media/ingress/app-one.png)

Şimdi ekleyin */hello-world-two* FQDN yolu gibi *https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two*. Özel başlıklı ikinci tanıtım uygulaması gösterilmiştir:

![İki uygulama örneği](media/ingress/app-two.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, AKS dış bazı bileşenleri dahil. Bu bileşenler hakkında daha fazla bilgi edinmek için şu proje sayfalara bakın:

- [Helm CLI][helm-cli]
- [NGINX giriş denetleyicisine][nginx-ingress]
- [Sertifika Yöneticisi][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli