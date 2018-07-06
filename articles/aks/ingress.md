---
title: Azure Kubernetes Service (AKS) kümesi ile giriş yapılandırma
description: Yükleyin ve bir Azure Kubernetes Service (AKS) kümesi içinde bir NGINX giriş denetleyicisine yapılandırın.
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: bd223e9eebac495d7336c618b831528505c30959
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857404"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) HTTPS giriş

Giriş denetleyicisine ters proxy, yapılandırılabilir bir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan bir yazılım parçasıdır. Kubernetes giriş kaynakları, giriş kuralları ve rotaları için her bir Kubernetes hizmeti yapılandırmak için kullanılır. Giriş denetleyicisine ve giriş kuralları kullanarak, tek bir dış adresi bir Kubernetes kümesinde birden çok hizmet trafiği yönlendirmek için kullanılabilir.

Örnek dağıtımı bu belgesinde [NGINX giriş denetleyicisine] [ nginx-ingress] Azure Kubernetes Service (AKS) kümesi içinde. Ayrıca, [Sertifika Yöneticisi] [ cert-manager] proje otomatik olarak oluşturmak ve yapılandırmak için kullanılan [şimdi şifrelemek] [ lets-encrypt] sertifikaları. Son olarak, bazı uygulamalar, her biri tek bir adresi erişilebilir, bir AKS kümesinde çalıştırılır.

## <a name="install-an-ingress-controller"></a>Giriş denetleyicisine yükleyin

NGINX giriş denetleyicisini yüklemek için Helm kullanın. NGINX giriş denetleyicisine bkz [belgeleri] [ nginx-ingress] ayrıntılı dağıtım bilgileri için.

Bu örnek, denetleyicide yükler `kube-system` ad alanı, tercih ettiğiniz bir ad alanına değiştirilebilir. AKS kümenizi RBAC etkin değilse, ekleme `--set rbac.create=false` komutu. Daha fazla bilgi için [ngınx giriş grafik][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Yükleme sırasında bir Azure genel IP adresi için giriş denetleyicisini oluşturulur. Genel IP adresini almak için kubectl get service komutunu kullanın. Bu, hizmete atanacak IP adresi için biraz zaman alabilir.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Genel IP adresine göz atarsanız hiçbir giriş kuralları, oluşturulduğundan, NGINX giriş denetleyicileri varsayılan 404 sayfasına yönlendirilir.

![Varsayılan NGINX arka uç](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>DNS adı yapılandırma

HTTPS sertifika kullanıldığından, giriş denetleyicileri IP adresi için bir FQDN adı'nı yapılandırmanız gerekir. Bu örnekte, Azure CLI ile bir Azure FQDN oluşturulur. Betik, giriş denetleyicisine FQDN kullanmak istediğiniz adı ve IP adresiyle güncelleştirin.

```bash
#!/bin/bash

# Public IP address
IP="51.145.155.210"

# Name to associate with public IP address
DNSNAME="demo-aks-ingress"

# Get the resource-id of the public ip
PUBLICIPID=$(az network public-ip list --query "[?ipAddress!=null]|[?contains(ipAddress, '$IP')].[id]" --output tsv)

# Update public ip address with dns name
az network public-ip update --ids $PUBLICIPID --dns-name $DNSNAME
```

Giriş denetleyicisine şimdi FQDN'si erişilebilir olması gerekir.

## <a name="install-cert-manager"></a>Sertifika Yöneticisi'ni yükleyin

NGINX giriş denetleyicisine TLS sonlandırma destekler. Almak ve HTTPS için sertifikaları yapılandırmanız için birkaç yolu olsa da, bu belgeyi kullanarak gösterir [Sertifika Yöneticisi][cert-manager], otomatik sağlayan [şifrelemeksağlar] [ lets-encrypt] sertifika oluşturma ve yönetim işlevselliği.

Sertifika Yöneticisi denetleyiciyi yüklemek için aşağıdaki Helm install komutu kullanın.

```bash
helm install stable/cert-manager --set ingressShim.defaultIssuerName=letsencrypt-prod --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Kümenizi RBAC etkin değilse, bu komutu kullanın.

```bash
helm install stable/cert-manager \
  --set ingressShim.defaultIssuerName=letsencrypt-prod \
  --set ingressShim.defaultIssuerKind=ClusterIssuer \
  --set rbac.create=false \
  --set serviceAccount.create=false
```

Sertifika Yöneticisi yapılandırma hakkında daha fazla bilgi için bkz. [Sertifika Yöneticisi proje][cert-manager].

## <a name="create-ca-cluster-issuer"></a>Küme veren CA'yı oluştur

Sertifika Yöneticisi sertifika verilmeden önce gerektiren bir [veren] [ cert-manager-issuer] veya [ClusterIssuer] [ cert-manager-cluster-issuer] kaynak. Kaynakları ancak işlevselliği aynıdır `Issuer` tek bir ad alanında çalışır burada `ClusterIssuer` tüm ad alanları geneline çalışır. Daha fazla bilgi için [Sertifika Yöneticisi veren] [ cert-manager-issuer] belgeleri.

Aşağıdaki bildirimi kullanarak bir küme veren oluşturun. E-posta adresi, kuruluşunuzdaki geçerli bir adresi ile güncelleştirin.

```yaml
apiVersion: certmanager.k8s.io/v1alpha1
kind: ClusterIssuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: user@contoso.com
    privateKeySecretRef:
      name: letsencrypt-prod
    http01: {}
```

## <a name="create-certificate-object"></a>Sertifika nesnesi oluşturma

Ardından, sertifika kaynak oluşturulması gerekir. Sertifika kaynak istenen X.509 sertifikası tanımlar. Daha fazla bilgi için bkz: [Sertifika Yöneticisi sertifika][cert-manager-certificates].

Sertifika kaynak aşağıdaki bildirimi ile oluşturun.

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
    name: letsencrypt-prod
    kind: ClusterIssuer
```

## <a name="run-application"></a>Uygulamayı çalıştırın

Bu noktada, bir giriş denetleyicisine ve sertifika yönetimi çözümü yapılandırıldı. Şimdi bazı uygulamalar, AKS kümesinde çalıştırılır.

Bu örnekte, Helm, basit bir hello world uygulaması birden çok örneğini çalıştırmak için kullanılır.

Uygulamayı çalıştırmadan önce geliştirme sisteminizde Azure örnekleri Helm deposuna ekleyin.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

AKS hello world grafik ile aşağıdaki komutu çalıştırın:

```bash
helm install azure-samples/aks-helloworld
```

Şimdi ikinci bir Merhaba Dünya uygulaması örneğini yükleyin.

İki uygulama görsel olarak ayrı olmasını sağlamak için ikinci örnek, yeni bir başlık belirtin. Ayrıca, benzersiz bir hizmet adı belirtmeniz gerekir. Bu yapılandırmalar, aşağıdaki komutta görülebilir.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Giriş yolu oluştur

Her iki uygulama Kubernetes kümenizde çalışan ancak yapılandırılmış olması artık türünde bir hizmet olan `ClusterIP`. Bu nedenle, uygulamaları, internet'ten erişilemez. Kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı, iki uygulama birine trafik yönlendirme kuralları yapılandırır.

Adlı bir dosya oluşturun `hello-world-ingress.yaml` aşağıdaki YAML'ye kopyalayın.

Not, adres için trafiği `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafiği adresine `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmeti.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    certmanager.k8s.io/cluster-issuer: letsencrypt-prod
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

Giriş kaynak oluşturma `kubectl apply` komutu.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Kubernetes giriş denetleyicinizin FQDN'yi göz atın, Merhaba Dünya uygulaması görmeniz gerekir.

![Bir uygulama örneği](media/ingress/app-one.png)

Artık giriş denetleyicisini FQDN'sine göz `/hello-world-two` yolu, Merhaba Dünya uygulaması ile özel bir başlık görmeniz gerekir.

![İki uygulama örneği](media/ingress/app-two.png)

Ayrıca bağlantı şifrelenir ve şimdi şifrelemek tarafından verilen bir sertifika kullanılır dikkat edin.

![Sertifika şifreleme sağlar](media/ingress/certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede gösterilen yazılım hakkında daha fazla bilgi edinin.

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
