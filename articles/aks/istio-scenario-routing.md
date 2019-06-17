---
title: Akıllı Yönlendirme ve Istio Azure Kubernetes Service (AKS) ile kanarya sürümleri
description: Akıllı yönlendirme sağlamak ve kanarya sürümlerde Azure Kubernetes Service (AKS) kümesini dağıtma Istio kullanmayı öğrenin
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: bd660a2b6ffb96478c3170cc7013ff22518b758f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64702216"
---
# <a name="use-intelligent-routing-and-canary-releases-with-istio-in-azure-kubernetes-service-aks"></a>Istio Azure Kubernetes Service (AKS) ile akıllı Yönlendirme ve kanarya sürümleri kullanın

[Istio] [ istio-github] Kubernetes kümesindeki mikro hizmetler arasında önemli bir dizi işlev sağlayan bir açık kaynak hizmeti kafes olduğu. Bu özellikler, trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlaması ve observability içerir. Resmi Istio hakkında daha fazla bilgi için bkz. [Istio nedir?] [ istio-docs-concepts] belgeleri.

Bu makalede Istio trafik yönetimi işlevlerini kullanma işlemini gösterir. Bir örnek AKS oylama uygulaması, akıllı Yönlendirme ve kanarya sürümleri keşfetmek için kullanılır.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Uygulamayı dağıtma
> * Uygulamayı güncelleştirme
> * Uygulamanın bir vamp Aktar
> * Piyasaya çıkma Sonlandır

## <a name="before-you-begin"></a>Başlamadan önce

> [!NOTE]
> Bu senaryo Istio sürüm karşı test edilmiştir `1.1.3`.

Bu makalede ayrıntılı adımlarda bir AKS kümesi oluşturduğunuz varsayılır (Kubernetes `1.11` ve yukarıdaki RBAC ile etkin) ve yerleşik olduğu bir `kubectl` kümeyle bağlantı. Ayrıca, kümenizi yüklü Istio gerekir.

Bu öğelerden herhangi birinin yardıma ihtiyacınız varsa bkz [AKS hızlı başlangıçları] [ aks-quickstart] ve [yükleme Istio aks'deki] [ istio-install] Kılavuzu.

## <a name="about-this-application-scenario"></a>Bu uygulama senaryosuna hakkında

Örnek AKS oylama uygulaması iki oylama seçeneklerini sağlar (**kediler** veya **köpekler**) kullanıcılar. Her seçeneği için oy sayısı kalıcı depolama bileşeni yoktur. Ayrıca, her bir seçenekte cast oyları ayrıntılarla sağlayan bir analytics bileşeni yoktur.

Bu uygulama senaryosunda sürüm dağıtarak Başlat `1.0` sürümü ve oylama uygulamasına `1.0` analytics bileşen. Analytics bileşeni için oy sayısı basit sayımları sağlar. Analytics bileşeni ve oylama uygulamasına sürümüyle etkileşim `1.0` Redis tarafından desteklenen depolama bileşen.

Analytics bileşeni sürümüne yükseltme `1.1`, hangi sayımları sağlar ve artık toplar ve yüzde.

Bir alt kümesini kullanıcılar test sürüm `2.0` bir vamp aracılığıyla uygulama. Bu yeni sürümün bir MySQL veritabanı tarafından desteklenen bir depolama bileşeni kullanır.

Emin olduğunuzda bu sürümü `2.0` alt kullanıcılar kümeniz üzerinde beklendiği gibi çalıştığını sürümü alma `2.0` tüm kullanıcılarınız için.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Azure Kubernetes Service (AKS) kümenizi uygulamasına dağıtarak başlayalım. Bu bölümde - sonunda çalıştırılanlar Aşağıdaki diyagramda gösterilmiştir sürüm `1.0` Istio giriş ağ geçidi aracılığıyla hizmet gelen istekleri içeren tüm bileşenlerinin:

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-01.png)

Bu makaleyi izlemek için gereken yapıtları kullanılabilir [Azure-Samples/aks-oy-app] [ github-azure-sample] GitHub deposu. Yapıtları indirmeyi veya deposunu şu şekilde kopyalayın:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

İndirilen / kopyalanan deponun aşağıdaki klasöre geçin ve sonraki tüm adımları bu klasörden çalıştır:

```console
cd scenarios/intelligent-routing-with-istio
```

İlk olarak, AKS kümenizde adlı örnek AKS oylama uygulaması için bir ad alanı oluşturma `voting` gibi:

```azurecli
kubectl create namespace voting
```

Ad alanı ile etiket `istio-injection=enabled`. Bu etiketi otomatik olarak istio proxy'leri podlarınız bu ad alanındaki tüm sepetler eklenmek üzere Istio bildirir.

```azurecli
kubectl label namespace voting istio-injection=enabled
```

Şimdi bileşenler için AKS oylama uygulaması oluşturalım. Bu bileşenlerde oluşturma `voting` bir önceki adımda oluşturduğunuz ad alanı.

```azurecli
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, oluşturulan kaynaklarını gösterir:

```console
deployment.apps/voting-storage-1-0 created
service/voting-storage created
deployment.apps/voting-analytics-1-0 created
service/voting-analytics created
deployment.apps/voting-app-1-0 created
service/voting-app created
```

> [!NOTE]
> Istio, pod'ların ve Hizmetleri ile ilgili belirli bazı gereksinimler vardır. Daha fazla bilgi için [pod'ların ve Hizmetleri belgeleri Istio gereksinimleri][istio-requirements-pods-and-services].

Oluşturulmuş pod'ların görmek için [kubectl pod'ları alma] [ kubectl-get] komutuyla şu şekilde:

```azurecli
kubectl get pods -n voting
```

Aşağıdaki örnek çıktı, üç örneği olmadığını gösteren `voting-app` pod ve her ikisi de tek bir örneğini `voting-analytics` ve `voting-storage` pod'ları. Pod'ların her iki kapsayıcı vardır. Bu kapsayıcıların bir bileşenidir ve diğeri `istio-proxy`:

```console
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-57c7fccb44-ng7dl   2/2       Running   0          39s
voting-app-1-0-956756fd-d5w7z           2/2       Running   0          39s
voting-app-1-0-956756fd-f6h69           2/2       Running   0          39s
voting-app-1-0-956756fd-wsxvt           2/2       Running   0          39s
voting-storage-1-0-5d8fcc89c4-2jhms     2/2       Running   0          39s
```

Pod hakkında bilgi için kullanın [kubectl açıklayan pod][kubectl-describe]. Pod önceki çıktısından kendi AKS kümesinde bir pod adıyla değiştirin:

```azurecli
kubectl describe pod voting-app-1-0-956756fd-d5w7z --namespace voting
```

`istio-proxy` Kapsayıcı oluşturdukça otomatik tarafından Istio bileşenlerinizin gelen ve giden ağ trafiğini yönetmek için aşağıdaki örnek çıktıda gösterildiği gibi:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         docker.io/istio/proxyv2:1.1.3
[...]
```

Istio oluşturana kadar oylama uygulamasına bağlanılamıyor [ağ geçidi] [ istio-reference-gateway] ve [sanal hizmet][istio-reference-virtualservice]. Bu Istio kaynakları uygulamamız varsayılan Istio giriş ağ geçidine gelen trafiği yönlendirin.

> [!NOTE]
> A **ağ geçidi** gelen veya giden HTTP ve TCP trafiği alan hizmet kafes kenarındaki bir bileşendir.
> 
> A **sanal hizmet** bir veya daha fazla hedef hizmet için yönlendirme kuralları kümesi tanımlar.

Kullanım `kubectl apply` ağ geçidi ve sanal hizmet yaml dağıtmak için komutu. Bu kaynakları içine dağıtılır ad alanı belirtmeyi unutmayın.

```azurecli
kubectl apply -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, yeni ağ geçidi ve sanal oluşturulan hizmet gösterir:

```console
virtualservice.networking.istio.io/voting-app created
gateway.networking.istio.io/voting-app-gateway created
```

Istio giriş aşağıdaki komutu kullanarak ağ geçidi IP adresini alın:

```azurecli
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Aşağıdaki örnek çıktıda, giriş ağ geçidi IP adresini gösterir:

```
20.188.211.19
```

Bir tarayıcı açın ve IP adresini yapıştırın. Örnek AKS oylama uygulaması görüntülenir.

![AKS kümesi bizim Istio çalışan AKS oylama uygulamasını etkin.](media/istio/deploy-app-01.png)

Uygulama sürümünü kullanır, ekranın alt kısmındaki bilgileri gösterir `1.0` , `voting-app` ve sürüm `1.0` , `voting-storage` (Redis).

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Biz analytics bileşenin yeni bir sürümünü dağıtalım. Bu yeni sürümün `1.1` toplamları ve yüzde sayısı her kategori için ek olarak görüntüler.

Bu bölümde - yalnızca sürüm sonunda çalışacağı Aşağıdaki diyagramda gösterilmiştir `1.1` , bizim `voting-analytics` bileşeniyse gelen yönlendirilen trafiği `voting-app` bileşeni. Olsa da sürüm `1.0` , bizim `voting-analytics` bileşeni çalışmaya devam eder ve tarafından başvurulan `voting-analytics` hizmet Istio proxy'leri, gelen ve giden trafiği izin vermeyin.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-02.png)

Sürüm dağıtalım `1.1` , `voting-analytics` bileşeni. Bu bileşeni oluşturma `voting` ad alanı:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, oluşturulan kaynaklarını gösterir:

```console
deployment.apps/voting-analytics-1-1 created
```

Önceki adımda elde edilen AKS oylama uygulamasına Istio giriş ağ geçidi IP adresini kullanarak bir tarayıcıda yeniden örneği açın.

Tarayıcınız, aşağıda gösterilen iki görünüm arasında geçiş yapıyor. Bir Kubernetes kullandığından [hizmet] [ kubernetes-service] için `voting-analytics` yalnızca bir tek etiketli Seçici ile bileşen (`app: voting-analytics`), Kubernetes hepsini arasında varsayılan davranışını kullanır seçicinin eşleşen pod'ları. Bu durumda, her iki sürüm olduğu `1.0` ve `1.1` , uygulamanızın `voting-analytics` pod'ları.

![AKS oylama uygulamamızı çalıştıran analytics bileşenin 1.0 sürümü.](media/istio/deploy-app-01.png)

![Sürüm 1.1 AKS oylama uygulamamızı çalıştıran analytics bileşeni olması gerekir.](media/istio/update-app-01.png)

Bu iki sürümleri arasında geçiş görselleştirebilirsiniz `voting-analytics` aşağıdaki gibi bileşeni. Kendi Istio giriş ağ geçidi IP adresini kullandığınızdan emin olun.

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

Aşağıdaki örnek çıktıda, sürümleri arasında site anahtarlar olarak döndürülen web sitesi ilgili bölümü gösterilmektedir:

```
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2 | Dogs: 4 </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

### <a name="lock-down-traffic-to-version-11-of-the-application"></a>Uygulamanın sürüm 1.1 trafiği kilitleme

Artık yalnızca sürüm trafiği şimdi kilitleme `1.1` , `voting-analytics` bileşen ve sürüm `1.0` , `voting-storage` bileşeni. Ardından tüm diğer bileşenleri için yönlendirme kuralları tanımlayın.

> * A **sanal hizmet** bir veya daha fazla hedef hizmet için yönlendirme kuralları kümesi tanımlar.
> * A **hedef kuralı** trafik kuralları ve sürüm belirli ilkeleri tanımlar.
> * A **ilke** hangi kimlik doğrulama yöntemleri üzerinde workload(s) kabul edilebilir tanımlar.

Kullanım `kubectl apply` üzerinde sanal hizmet tanımı değiştirmek için komut, `voting-app` ve ekleme [hedef kuralları] [ istio-reference-destinationrule] ve [sanal Hizmetleri] [ istio-reference-virtualservice] diğer bileşenler için. Ekleyeceksiniz bir [ilke] [ istio-reference-policy] için `voting` tüm hizmetleri arasında iletişim sağlamak için ad alanı, karşılıklı TLS ve istemci sertifikaları kullanmak güvenlidir.

* İlkesi `peers.mtls.mode` kümesine `STRICT` hizmetlerinizi içinde arasında karşılıklı TLS uygulandığını emin olmak için `voting` ad alanı.
* Ayrıca `trafficPolicy.tls.mode` için `ISTIO_MUTUAL` bizim hedef kuralları. Istio karşılıklı TLS ve Istio şeffaf bir şekilde yönetir istemci sertifikaları kullanan hizmetler arasındaki iletişimin güvenliğini sağlar ve daha güçlü kimliklerle hizmetleri sağlar.

```azurecli
kubectl apply -f istio/step-2-update-and-add-routing-for-all-components.yaml --namespace voting
```

Yeni ilke, hedef kuralları ve sanal güncelleştirilen ve oluşturulan olan hizmetleri aşağıdaki örnek çıktı gösterilmektedir:

```console
virtualservice.networking.istio.io/voting-app configured
policy.authentication.istio.io/default created
destinationrule.networking.istio.io/voting-app created
destinationrule.networking.istio.io/voting-analytics created
virtualservice.networking.istio.io/voting-analytics created
destinationrule.networking.istio.io/voting-storage created
virtualservice.networking.istio.io/voting-storage created
```

AKS Voting uygulamasını bir tarayıcıda yeniden, yalnızca yeni sürüm açarsanız `1.1` , `voting-analytics` bileşeni tarafından kullanılan `voting-app` bileşeni.

![Sürüm 1.1 AKS oylama uygulamamızı çalıştıran analytics bileşeni olması gerekir.](media/istio/update-app-01.png)

Artık yalnızca sürüm yönlendirildiğinden görselleştirebilirsiniz `1.1` , sizin `voting-analytics` aşağıdaki gibi bileşeni. Kendi Istio giriş ağ geçidinin IP adresi kullanmayı unutmayın:

Bash 

```bash
INGRESS_IP=20.188.211.19
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

PowerShell

```powershell
$INGRESS_IP="20.188.211.19"
(1..5) |% { (Invoke-WebRequest -Uri $INGRESS_IP).Content.Split("`n") | Select-String -Pattern "results" }
```

Döndürülen web sitesi ilgili bölümü aşağıdaki örnek çıktı gösterilmektedir:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Şimdi Istio hizmetlerimizin her biri arasındaki iletişimin güvenliğini sağlamak için TLS karşılıklı kullanarak artık onaylayın. Bunun için kullanacağız [authn tls denetimi] [ istioctl-authn-tls-check] komutunu `istioctl` aşağıdaki biçimdedir ikili istemcisi.

```console
istioctl authn tls-check <pod-name[.namespace]> [<service>]
```

Bu komut kümesini bir ad alanındaysa ve etiketleri kümesiyle eşleşen tüm pod belirtilen Hizmetleri, erişim hakkında bilgi sağlar:

Bash

```bash
# mTLS configuration between each of the istio ingress pods and the voting-app service
kubectl get pod -n istio-system -l app=istio-ingressgateway | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.istio-system voting-app.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-analytics service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-analytics.voting.svc.cluster.local

# mTLS configuration between each of the voting-app pods and the voting-storage service
kubectl get pod -n voting -l app=voting-app | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
kubectl get pod -n voting -l app=voting-analytics,version=1.1 | grep Running | cut -d ' ' -f1 | xargs -n1 -I{} istioctl authn tls-check {}.voting voting-storage.voting.svc.cluster.local
```

PowerShell

```powershell
# mTLS configuration between each of the istio ingress pods and the voting-app service
(kubectl get pod -n istio-system -l app=istio-ingressgateway | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".istio-system") voting-app.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-analytics service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-analytics.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-app pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-app | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
(kubectl get pod -n voting -l app=voting-analytics,version=1.1 | Select-String -Pattern "Running").Line |% { $_.Split()[0] |% { istioctl authn tls-check $($_ + ".voting") voting-storage.voting.svc.cluster.local } }
```

Aşağıdaki örnek çıktıda, karşılıklı TLS her yukarıdaki bizim sorgular için uygulandığını gösterir. Çıktı, ayrıca ilke ve hedef karşılıklı TLS zorlayan kurallarını gösterir:

```console
# mTLS configuration between istio ingress pods and the voting-app service
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

# mTLS configuration between each of the voting-app pods and the voting-analytics service
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting
HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

# mTLS configuration between each of the voting-app pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting

# mTLS configuration between each of the voting-analytics version 1.1 pods and the voting-storage service
HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Uygulamanın bir vamp Aktar

Şimdi yeni bir sürüm dağıtalım `2.0` , `voting-app`, `voting-analytics`, ve `voting-storage` bileşenleri. Yeni `voting-storage` bileşeni MySQL, Redis yerine kullanın ve `voting-app` ve `voting-analytics` bu yeni kullanmalarına izin vermek için güncelleştirilmiş bileşenleri `voting-storage` bileşeni.

`voting-app` Bileşen artık özellik bayrağı işlevselliği destekler. Bu özellik bayrağını Istio vamp yeteneğini kullanıcıların bir alt kümesi için test etmenizi sağlar.

Sahip olacaktır, aşağıdaki diyagramda gösterilmiştir bu bölümün sonunda çalışıyor.

* Sürüm `1.0` , `voting-app` bileşeni, sürüm `1.1` , `voting-analytics` bileşen ve sürüm `1.0` , `voting-storage` bileşen birbiriyle iletişim kurabilir.
* Sürüm `2.0` , `voting-app` bileşeni, sürüm `2.0` , `voting-analytics` bileşen ve sürüm `2.0` , `voting-storage` bileşen birbiriyle iletişim kurabilir.
* Sürüm `2.0` , `voting-app` bileşenidir yalnızca belirli özellik bayrağı ayarlanmış olan kullanıcılar için erişilebilir. Bu değişiklik, bir tanımlama bilgisi aracılığıyla bir özellik bayrağı kullanılarak yönetilir.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-03.png)

İlk olarak, bu yeni bileşenler için uygun Istio hedef kuralları ve sanal Hizmetleri güncelleştirin. Bu güncelleştirmeler, trafiği yanlış yeni bileşenler için yol yok ve kullanıcılar, beklenmeyen erişim elde etmezsiniz emin olun:

```azurecli
kubectl apply -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Aşağıdaki örnek çıktı, sanal güncelleştirilmesini Hizmetleri ve hedef kuralları gösterir:

```console
destinationrule.networking.istio.io/voting-app configured
virtualservice.networking.istio.io/voting-app configured
destinationrule.networking.istio.io/voting-analytics configured
virtualservice.networking.istio.io/voting-analytics configured
destinationrule.networking.istio.io/voting-storage configured
virtualservice.networking.istio.io/voting-storage configured
```

Ardından, yeni sürüm için Kubernetes nesneleri ekleyelim `2.0` bileşenleri. Ayrıca güncelleştirme `voting-storage` içerecek şekilde hizmet `3306` MySQL için bağlantı noktası:

```azurecli
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, Kubernetes nesneler başarıyla oluşturulan veya güncelleştirilen gösterir:

```console
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Sürüme kadar bekleyin `2.0` pod'ların çalışıyor. Kullanım [kubectl pod'ları alma] [ kubectl-get] tüm pod'ların görüntülemek için komut `voting` ad alanı:

```azurecli
kubectl get pods --namespace voting
```

Artık sürümleri arasında geçiş yapabilirsiniz olmalıdır `1.0` ve sürüm `2.0` oylama uygulamasının (kanarya). Özellik bayrağı aç/kapa ekranın alt kısmındaki bir tanımlama bilgisi ayarlar. Bu tanımlama bilgisi tarafından kullanılan `voting-app` rota kullanıcılara yeni sürüme sanal hizmet `2.0`.

![Sürüm 1.0 AKS Voting uygulamasını - özellik bayrağı değil olarak ayarlanmış olması gerekir.](media/istio/canary-release-01.png)

![Sürüm 2.0 AKS Voting uygulamasını - özellik bayrağı olduğu ayarlanmış olması gerekir.](media/istio/canary-release-02.png)

Oy Sayısı, uygulama sürümleri arasında farklılık gösterir. Bu fark, iki farklı depolama arka uçları kullanarak vurgular.

## <a name="finalize-the-rollout"></a>Piyasaya çıkma Sonlandır

Vamp başarıyla test ettikten sonra güncelleştirme `voting-app` sürümüne tüm trafiği yönlendirmek için sanal hizmet `2.0` , `voting-app` bileşeni. Tüm kullanıcılar sürüm bkz `2.0` özellik bayrağının ayarlanmasına bağımsız olarak uygulamanın:

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-04.png)

Tüm hedef bileşenlerin sürümlerini kaldırmak için artık etkin istediğiniz kurallarını güncelleştirin. Ardından, tüm sanal sürümler başvuran durdurmak için Hizmetleri güncelleştirin.

Artık olduğundan bileşenlerin eski sürümlerinin herhangi birinin tüm trafiği, bu bileşenler için tüm dağıtımlar artık güvenli bir şekilde silebilirsiniz.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-05.png)

Artık başarıyla AKS Voting uygulamasını yeni bir sürümü alındı.

## <a name="clean-up"></a>Temizleme 

Silerek AKS kümenizi Bu senaryoda kullandık AKS oylama uygulamasına kaldırabilirsiniz `voting` gösterildiği gibi ad alanı:

```azurecli
kubectl delete namespace voting
```

Aşağıdaki örnek çıktıda, AKS oylama uygulamasının tüm bileşenleri AKS kümenizi kaldırılmış gösterir.

```console
namespace "voting" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

Kullanan ek senaryoları inceleyebilirsiniz [Istio Bookinfo uygulama örneği][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio-github]: https://github.com/istio/istio

[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/prepare/requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/networking/v1alpha3/gateway/
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[istioctl-authn-tls-check]: https://istio.io/docs/reference/commands/istioctl/#istioctl-authn-tls-check

[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md
