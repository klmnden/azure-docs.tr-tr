---
title: Giriş ile Azure Kubernetes hizmet (AKS) kümesi yapılandırın
description: Yükleyin ve bir Azure Kubernetes hizmet (AKS) küme NGINX giriş denetleyicisi yapılandırın.
services: container-service
author: neilpeterson
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/25/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: fcf0b6f3b7f6d75006d8c10aab041c25fc0d8c39
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751294"
---
# <a name="https-ingress-on-azure-kubernetes-service-aks"></a>HTTPS giriş Azure Kubernetes Service (AKS)

Bir giriş denetleyicisi ters proxy, yapılandırılabilir trafik yönlendirme ve TLS sonlandırma Kubernetes hizmetleri sağlayan yazılım parçasıdır. Kubernetes giriş kaynakları giriş kuralları ve tek tek Kubernetes Hizmetleri için rotalar yapılandırmak için kullanılır. Bir giriş denetleyicisi ve giriş kurallarını kullanarak, tek bir dış adresi Kubernetes kümedeki birden fazla hizmet için trafiği yönlendirmek için kullanılabilir.

Bu belgede bir örnek dağıtımında kılavuzluk etmektedir [NGINX giriş denetleyicisi] [ nginx-ingress] bir Azure Kubernetes hizmet (AKS) kümesindeki. Ayrıca, [Sertifika Yöneticisi] [ cert-manager] proje otomatik olarak oluşturmak ve yapılandırmak için kullanılan [şimdi şifrelemek] [ lets-encrypt] sertifikalar. Son olarak, bazı uygulamalar, her biri tek bir adresi erişilebilen AKS kümedeki çalıştırılır.

## <a name="install-an-ingress-controller"></a>Bir giriş denetleyicisi yükleme

Helm NGINX giriş denetleyicisi yüklemek için kullanın. Bkz. NGINX giriş controller [belgelerine] [ nginx-ingress] ayrıntılı dağıtım bilgileri için.

Bu örnek, denetleyicide yükler `kube-system` ad alanı, bu tercih ettiğiniz bir ad alanına değiştirilebilir. AKS kümenizi RBAC etkin değilse, ekleyin `--set rbac.create=false` komutu. Daha fazla bilgi için bkz: [nginx giriş grafik][nginx-ingress].

```bash
helm install stable/nginx-ingress --namespace kube-system
```

Yükleme sırasında Azure ortak IP adresi giriş denetleyici için oluşturulur. Genel IP adresi almak için kubectl get hizmet komutunu kullanın. Hizmete atanan IP adresi için biraz zaman alabilir.

```console
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                       TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
eager-crab-nginx-ingress-controller        LoadBalancer   10.0.182.160   51.145.155.210  80:30920/TCP,443:30426/TCP   20m
eager-crab-nginx-ingress-default-backend   ClusterIP      10.0.255.77    <none>          80/TCP                       20m
```

Genel IP adresine göz atarsanız hiçbir giriş kuralları, oluşturulduğundan, NGINX giriş denetleyicileri varsayılan 404 sayfasına yönlendirilir.

![Varsayılan NGINX arka uç](media/ingress/default-back-end.png)

## <a name="configure-dns-name"></a>DNS adı yapılandırma

HTTPS sertifika kullanıldığından, giriş denetleyicileri IP adresi için bir FQDN adı yapılandırmanız gerekir. Bu örnekte, bir Azure FQDN ile Azure CLI oluşturulur. Giriş denetleyicisi FQDN ile kullanmak istediğiniz adı ve IP adresiyle komut dosyasını güncelleştirin.

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

Giriş denetleyicisi artık FQDN üzerinden erişilebilir olması gerekir.

## <a name="install-cert-manager"></a>Sertifika Yöneticisi'ni yükleyin

NGINX giriş denetleyicisi TLS sonlandırma destekler. Almak ve HTTPS için sertifikaları yapılandırmak için çeşitli yollar olsa da, bu belgenin kullanımı gösterilir [Sertifika Yöneticisi][cert-manager], otomatik sağlayan [sağlar şifrelemek] [ lets-encrypt] sertifika oluşturma ve yönetim işlevselliği.

Sertifika Yöneticisi denetleyicisi yüklemek için aşağıdaki Helm yükleme komutunu kullanın.

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

Sertifika Yöneticisi yapılandırma hakkında daha fazla bilgi için bkz: [Sertifika Yöneticisi proje][cert-manager].

## <a name="create-ca-cluster-issuer"></a>CA küme veren oluşturma

Sertifika verilebilmesi için önce Sertifika Yöneticisi gerektiren bir [veren] [ cert-manager-issuer] veya [ClusterIssuer] [ cert-manager-cluster-issuer] kaynak. Kaynakları ancak işlevindeki özdeş `Issuer` tek bir ad alanında çalışır nerede `ClusterIssuer` tüm ad alanlarını çalışır. Daha fazla bilgi için bkz: [Sertifika Yöneticisi veren] [ cert-manager-issuer] belgeleri.

Aşağıdaki bildirimi kullanarak bir küme veren oluşturun. E-posta adresi geçerli bir adresi kuruluşunuzdaki ile güncelleştirin.

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

## <a name="create-certificate-object"></a>Sertifika nesnesi oluşturun

Ardından, bir sertifika kaynak oluşturulması gerekir. Sertifika kaynak istenen X.509 sertifikası tanımlar. Daha fazla bilgi için bkz: [Sertifika Yöneticisi sertifika][cert-manager-certificates].

Sertifika kaynağı ile aşağıdaki bildirimini oluşturun.

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

## <a name="run-application"></a>Uygulamayı çalıştırma

Bu noktada, bir giriş denetleyicisi ve bir sertifika yönetimi çözümü yapılandırıldı. Şimdi birkaç uygulamaları AKS kümenizdeki çalıştırın.

Bu örnekte, Helm basit hello world uygulamasının birden çok örneği çalıştırmak için kullanılır.

Uygulamayı çalıştırmadan önce geliştirme sisteminizde Azure örneklerinden Helm deposunu ekleyin.

```bash
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

AKS hello world grafik birlikte aşağıdaki komutu çalıştırın:

```bash
helm install azure-samples/aks-helloworld
```

Şimdi hello world uygulamasının ikinci bir örneğini yükleyin.

İki uygulama görsel olarak ayrı; böylece ikinci örneği için yeni bir başlık belirtin. Ayrıca, benzersiz bir hizmet adı belirtmeniz gerekir. Bu yapılandırmalar aşağıdaki komutta görülebilir.

```bash
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-ingress-route"></a>Giriş yolu oluştur

Her iki uygulamayı Kubernetes kümenizde çalışan ancak yapılandırıldı artık türünde bir hizmet olan `ClusterIP`. Bu nedenle, uygulamaları Internet'ten erişilebilir değildir. Kullanılabilir hale getirmek için bir Kubernetes giriş kaynağı oluşturun. Giriş kaynağı iki uygulamalardan biri trafiği yönlendirmek kuralları yapılandırır.

Bir dosya adı oluşturun `hello-world-ingress.yaml` ve aşağıdaki YAML kopyalayın.

Not alın adresi trafiği `https://demo-aks-ingress.eastus.cloudapp.azure.com/` adlı hizmete yönlendirilir `aks-helloworld`. Trafik adresine `https://demo-aks-ingress.eastus.cloudapp.azure.com/hello-world-two` yönlendirilir `ingress-demo` hizmet.

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

Giriş kaynakla oluşturmak `kubectl apply` komutu.

```console
kubectl apply -f hello-world-ingress.yaml
```

## <a name="test-the-ingress-configuration"></a>Giriş yapılandırmayı test etme

Kubernetes giriş denetleyicinizi FQDN'sini göz atın, hello world uygulamasının görmeniz gerekir.

![Bir uygulama örneği](media/ingress/app-one.png)

Şimdi giriş denetleyicisiyle FQDN'sini Gözat `/hello-world-two` yolu, özel başlık hello world uygulamayla görmeniz gerekir.

![İki uygulama örneği](media/ingress/app-two.png)

Ayrıca bağlantı şifrelenir ve şimdi şifrelemek tarafından verilen bir sertifika kullanılır dikkat edin.

![Sertifika şifreleme sağlar](media/ingress/certificate.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede gösterilen yazılım hakkında daha fazla bilgi edinin.

- [Helm CLI][helm-cli]
- [NGINX giriş denetleyicisi][nginx-ingress]
- [Sertifika Yöneticisi][cert-manager]

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[cert-manager]: https://github.com/jetstack/cert-manager
[cert-manager-certificates]: https://cert-manager.readthedocs.io/en/latest/reference/certificates.html
[cert-manager-cluster-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/clusterissuers.html
[cert-manager-issuer]: https://cert-manager.readthedocs.io/en/latest/reference/issuers.html
[lets-encrypt]: https://letsencrypt.org/
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx
