---
title: Akıllı Yönlendirme ve Istio Azure Kubernetes Service (AKS) ile kanarya sürümleri
description: Akıllı yönlendirme sağlamak ve kanarya sürümlerde Azure Kubernetes Service (AKS) kümesini dağıtma Istio kullanmayı öğrenin
services: container-service
author: rockboyfor
ms.service: container-service
ms.topic: article
origin.date: 12/03/2018
ms.date: 03/04/2019
ms.author: v-yeche
ms.openlocfilehash: 0a4e5e7e310a9949ee59291c2032eafda46955a9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465941"
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

Bu makalede ayrıntılı adımlarda bir AKS kümesi oluşturduğunuz varsayılır (Kubernetes 1.10 ve yukarıdaki RBAC ile etkin) ve yerleşik olduğu bir `kubectl` kümeyle bağlantı. Ayrıca, kümenizin yüklü Istio gerekir.

Bu öğelerden herhangi birinin yardıma ihtiyacınız varsa bkz [AKS hızlı başlangıçları] [ aks-quickstart] ve [AKS Istio yükleme][istio-install].

## <a name="about-this-application-scenario"></a>Bu uygulama senaryosuna hakkında

Örnek AKS oylama uygulaması, kullanıcıların iki oylama seçeneklerini (kediler veya köpekler) sağlar. Her seçeneği için oy sayısı kalıcı depolama bileşeni yoktur. Ayrıca, her bir seçenekte cast oyları ayrıntılarla sağlayan bir analytics bileşeni yoktur.

Bu makalede, sürüm dağıtarak Başlat *1.0* sürümü ve oylama uygulamasına *1.0* analytics bileşen. Analytics bileşeni için oy sayısı basit sayımları sağlar. Analytics bileşeni ve oylama uygulamasına sürümüyle etkileşim *1.0* Redis tarafından desteklenen depolama bileşen.

Analytics bileşeni sürümüne yükseltme *1.1*, hangi sayımları sağlar ve artık toplar ve yüzde.

Bir alt kümesini kullanıcılar test sürüm *2.0* bir vamp aracılığıyla uygulama. Bu yeni sürümün bir MySQL veritabanı tarafından desteklenen bir depolama bileşeni kullanır.

Emin olduğunuzda bu sürümü *2.0* alt kullanıcılar kümeniz üzerinde beklendiği gibi çalıştığını sürümü alma *2.0* tüm kullanıcılarınız için.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Azure Kubernetes Service (AKS) kümenizi uygulamasına dağıtarak başlayalım. Bu bölümde - sonunda çalıştırılanlar Aşağıdaki diyagramda gösterilmiştir sürüm *1.0* Istio giriş ağ geçidi aracılığıyla hizmet gelen istekleri içeren tüm bileşenlerinin:

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-01.png)

Bu makaleyi izlemek için gereken yapıtları kullanılabilir [Azure-Samples/aks-oy-app] [ github-azure-sample] GitHub deposu. Yapıtları indirmeyi veya deposunu şu şekilde kopyalayın:

```console
git clone https://github.com/Azure-Samples/aks-voting-app.git
```

İndirilen / kopyalanan deponun aşağıdaki klasöre geçin ve sonraki tüm adımları bu klasörden çalıştır:

```console
cd scenarios/intelligent-routing-with-istio
```

İlk olarak, AKS kümenizde adlı örnek AKS oylama uygulaması için bir ad alanı oluşturma *oylama* gibi:

```console
kubectl create namespace voting
```

Ad alanı ile etiket `istio-injection=enabled`. Bu etiketi otomatik olarak istio proxy'leri podlarınız bu ad alanındaki tüm sepetler eklenmek üzere Istio bildirir.

```console
kubectl label namespace voting istio-injection=enabled
```

Şimdi bileşenler için AKS oylama uygulaması oluşturalım. Bu bileşenlerde oluşturma *oylama* bir önceki adımda oluşturduğunuz ad alanı.

```console
kubectl apply -f kubernetes/step-1-create-voting-app.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, kaynakları başarıyla oluşturulup oluşturulmadığını gösterir:

```
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

```console
kubectl get pods -n voting
```

Aşağıdaki örnek çıktı, üç örneği olmadığını gösteren *voting uygulamasını* pod ve her ikisi de tek bir örneğini *oylama analiz* ve *oylama depolama* pod'ları. Pod'ların her iki kapsayıcı vardır. Bu kapsayıcıların bir bileşenidir ve diğeri *istio proxy*:

```
NAME                                    READY     STATUS    RESTARTS   AGE
voting-analytics-1-0-669f99dcc8-lzh7k   2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-bdmld         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-gcrng         2/2       Running   0          1m
voting-app-1-0-6c65c4bdd4-strzc         2/2       Running   0          1m
voting-storage-1-0-7954799d96-5fv9r     2/2       Running   0          1m
```

Pod hakkında bilgi için kullanın [kubectl açıklayan pod][kubectl-describe]. Pod önceki çıktısından kendi AKS kümesinde bir pod adıyla değiştirin:

```console
kubectl describe pod voting-app-1-0-6c65c4bdd4-bdmld --namespace voting
```

*İstio proxy* kapsayıcı oluşturdukça otomatik tarafından Istio bileşenlerinizin gelen ve giden ağ trafiğini yönetmek için aşağıdaki örnek çıktıda gösterildiği gibi:

```
[...]
Containers:
  voting-app:
    Image:         mcr.microsoft.com/aks/samples/voting/app:1.0
    ...
  istio-proxy:
    Image:         dockerhub.azk8s.cn/istio/proxyv2:1.0.4
[...]
```

Istio oluşturana kadar oylama uygulamasına bağlanılamıyor [ağ geçidi] [ istio-reference-gateway] ve [sanal hizmet][istio-reference-virtualservice]. Bu Istio kaynakları uygulamamız varsayılan Istio giriş ağ geçidine gelen trafiği yönlendirin.

> [!NOTE]
> A *ağ geçidi* gelen veya giden HTTP ve TCP trafiği alan hizmet kafes kenarındaki bir bileşendir.
>
> A *sanal hizmet* bir veya daha fazla hedef hizmet için yönlendirme kuralları kümesi tanımlar.

Kullanım `istioctl` ağ geçidi ve sanal hizmet yaml dağıtmak için istemci ikili. Olduğu gibi `kubectl apply` komutu, bu kaynakları içine dağıtılır ad alanı belirtmeyi unutmayın.

```console
istioctl create -f istio/step-1-create-voting-app-gateway.yaml --namespace voting
```

Istio giriş aşağıdaki komutu kullanarak ağ geçidi IP adresini alın:

```console
kubectl get service istio-ingressgateway --namespace istio-system -o jsonpath='{.status.loadBalancer.ingress[0].ip}'
```

Aşağıdaki örnek çıktıda, giriş ağ geçidi IP adresini gösterir:

```
52.187.250.239
```

Bir tarayıcı açın ve IP adresini yapıştırın. Örnek AKS oylama uygulaması görüntülenir.

![AKS kümesi bizim Istio çalışan AKS oylama uygulamasını etkin.](media/istio/deploy-app-01.png)

Uygulama sürümünü kullanır, ekranın alt kısmındaki bilgileri gösterir *1.0* , *voting uygulamasını* ve sürüm *1.0* (depolama seçeneği olarak Redis).

## <a name="update-the-application"></a>Uygulamayı güncelleştirme

Biz analytics bileşenin yeni bir sürümünü dağıtalım. Bu yeni sürümün *1.1* toplamları ve yüzde sayısı her kategori için ek olarak görüntüler.

Bu bölümde - yalnızca sürüm sonunda çalıştırılanlar Aşağıdaki diyagramda gösterilmiştir *1.1* , bizim *oylama analytics* bileşeniyse gelen yönlendirilen trafiği *voting uygulamasını* bileşeni. Olsa da sürüm *1.0* , bizim *oylama analytics* bileşeni çalışmaya devam eder ve tarafından başvurulan *oylama analytics* hizmet Istio proxy trafiğine izin verme Bu gelen ve giden.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-02.png)

Sürüm dağıtalım *1.1* , *oylama analytics* bileşeni. Bu bileşeni oluşturma *oylama* ad alanı:

```console
kubectl apply -f kubernetes/step-2-update-voting-analytics-to-1.1.yaml --namespace voting
```

Önceki adımda elde edilen AKS oylama uygulamasına Istio giriş ağ geçidi IP adresini kullanarak bir tarayıcıda yeniden örneği açın.

Tarayıcınız, aşağıda gösterilen iki görünüm arasında geçiş yapıyor. Bir Kubernetes kullandığından [hizmet] [ kubernetes-service] için *oylama analytics* yalnızca bir tek etiketli Seçici ile bileşen (`app: voting-analytics`), Kubernetes varsayılan kullanır hepsini seçicinin eşleşen pod'ları arasındaki davranış. Bu durumda, her iki sürüm olduğu *1.0* ve *1.1* , sizin *oylama analytics* pod'ları.

![AKS oylama uygulamamızı çalıştıran analytics bileşenin 1.0 sürümü.](media/istio/deploy-app-01.png)

![Sürüm 1.1 AKS oylama uygulamamızı çalıştıran analytics bileşeni olması gerekir.](media/istio/update-app-01.png)

Bu iki sürümleri arasında geçiş görselleştirebilirsiniz *oylama analytics* bileşeni aşağıdaki gibi. Kendi Istio giriş ağ geçidi IP adresini kullandığınızdan emin olun.

```console
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
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

Artık yalnızca sürüm trafiği şimdi kilitleme *1.1* , *oylama analytics* bileşen ve sürüm *1.0* , *oylama depolama* bileşeni. Ardından tüm diğer bileşenleri için yönlendirme kuralları tanımlayın.

> * A *sanal hizmet* bir veya daha fazla hedef hizmet için yönlendirme kuralları kümesi tanımlar.
> * A *hedef kuralı* trafik kuralları ve sürüm belirli ilkeleri tanımlar.
> * A *ilke* hangi kimlik doğrulama yöntemleri üzerinde workload(s) kabul edilebilir tanımlar.

Kullandığınız `istioctl` üzerinde sanal hizmet tanımı değiştirmek için istemci ikili, *voting uygulamasını* ve ekleme [hedef kuralları] [ istio-reference-destinationrule] ve [ Sanal Hizmetleri] [ istio-reference-virtualservice] diğer bileşenler için.

Ayrıca eklediğiniz bir [ilke] [ istio-reference-policy] için *oylama* tüm hizmetleri arasında iletişim sağlamak için ad alanı, karşılıklı TLS ve istemci sertifikaları kullanmak güvenlidir.

İçin mevcut bir sanal hizmet tanımı olmadığından *voting uygulamasını* değiştirin, kullanın `istioctl replace` komutuyla şu şekilde:

```console
istioctl replace -f istio/step-2a-update-voting-app-virtualservice.yaml --namespace voting
```

Aşağıdaki örnek çıktıda Istio sanal hizmet başarıyla güncelleştirildi gösterir:

```
Updated config virtual-service/voting/voting-app to revision 141902
```

Ardından, `istioctl create` yeni ilke ve ayrıca yeni hedef kurallar ve diğer bileşenlere yönelik sanal Hizmetleri eklemek için komutu.

* İlkesi `peers.mtls.mode` kümesine `STRICT` hizmetlerinizi içinde arasında karşılıklı TLS uygulandığını emin olmak için *oylama* ad alanı.
* Ayrıca `trafficPolicy.tls.mode` için `ISTIO_MUTUAL` bizim hedef kuralları. Istio karşılıklı TLS ve Istio şeffaf bir şekilde yönetir istemci sertifikaları kullanan hizmetler arasındaki iletişimin güvenliğini sağlar ve daha güçlü kimliklerle hizmetleri sağlar.

```console
istioctl create -f istio/step-2b-add-routing-for-all-components.yaml --namespace voting
```

Yeni ilke, hedef kuralları, aşağıdaki örnek çıktıda gösterilir ve sanal Hizmetleri başarıyla oluşturuldu:

```
Created config policy/voting/default to revision 142118
Created config destination-rule/voting/voting-app at revision 142119
Created config destination-rule/voting/voting-analytics at revision 142120
Created config virtual-service/voting/voting-analytics at revision 142121
Created config destination-rule/voting/voting-storage at revision 142122
Created config virtual-service/voting/voting-storage at revision 142123
```

AKS oylama uygulamasına bir tarayıcıdan yeniden, yalnızca yeni sürüm açarsanız *1.1* , *oylama analytics* bileşeni tarafından kullanılan *voting uygulamasını* bileşeni.

![Sürüm 1.1 AKS oylama uygulamamızı çalıştıran analytics bileşeni olması gerekir.](media/istio/update-app-01.png)

Biz artık yalnızca sürüm yönlendirildiğinden daha kolay görselleştirebilirsiniz *1.1* , *oylama analytics* aşağıdaki gibi bileşeni. Istio giriş ağ geçidi IP adresini kullandığınızdan emin olun.

Artık yalnızca sürüm yönlendirildiğinden görselleştirebilirsiniz *1.1* , *oylama analytics* aşağıdaki gibi bileşeni. Kendi Istio giriş ağ geçidinin IP adresi kullanmayı unutmayın:

```azurecli
INGRESS_IP=52.187.250.239
for i in {1..5}; do curl -si $INGRESS_IP | grep results; done
```

Döndürülen web sitesi ilgili bölümü aşağıdaki örnek çıktı gösterilmektedir:

```
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
  <div id="results"> Cats: 2/6 (33%) | Dogs: 4/6 (67%) </div>
```

Hizmetlerimizin her biri arasında güvenli iletişim için karşılıklı TLS Istio kullandığını onaylayın. Aşağıdaki komutları her biri için TLS ayarları kontrol *voting uygulamasını* Hizmetleri:

```console
istioctl authn tls-check voting-app.voting.svc.cluster.local
istioctl authn tls-check voting-analytics.voting.svc.cluster.local
istioctl authn tls-check voting-storage.voting.svc.cluster.local
```

Aşağıdaki örnek çıktıda, hedef kuralları ve ilke aracılığıyla hizmetlerinin her biri için TLS karşılıklı uygulandığını gösterir:

```
HOST:PORT                                    STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-app.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-app/voting

HOST:PORT                                          STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-analytics.voting.svc.cluster.local:8080     OK         mTLS       mTLS       default/voting     voting-analytics/voting

HOST:PORT                                        STATUS     SERVER     CLIENT     AUTHN POLICY       DESTINATION RULE
voting-storage.voting.svc.cluster.local:6379     OK         mTLS       mTLS       default/voting     voting-storage/voting
```

## <a name="roll-out-a-canary-release-of-the-application"></a>Uygulamanın bir vamp Aktar

Şimdi yeni bir sürüm dağıtalım *2.0* , *voting uygulamasını*, *oylama analytics*, ve *oylama depolama* bileşenleri. Yeni *oylama depolama* bileşeni MySQL, Redis yerine kullanın ve *voting uygulamasını* ve *oylama analytics* bu yeni kullanmalarınaizinvermekiçingüncelleştirilmişbileşenleri*oylama depolama* bileşeni.

*Voting uygulamasını* bileşen artık özellik bayrağı işlevselliği destekler. Bu özellik bayrağını Istio vamp yeteneğini kullanıcıların bir alt kümesi için test etmenizi sağlar.

Aşağıdaki diyagramda, bu bölümün sonunda çalıştırılanlar gösterilmektedir.

* Sürüm *1.0* , *voting uygulamasını* bileşeni, sürüm *1.1* , *oylama analytics* bileşen ve sürüm  *1.0* , *oylama depolama* bileşen birbiriyle iletişim kurabilir.
* Sürüm *2.0* , *voting uygulamasını* bileşeni, sürüm *2.0* , *oylama analytics* bileşen ve sürüm  *2.0* , *oylama depolama* bileşen birbiriyle iletişim kurabilir.
* Sürüm *2.0* , *voting uygulamasını* bileşenidir yalnızca belirli özellik bayrağı ayarlanmış olan kullanıcılar için erişilebilir. Bu değişiklik, bir tanımlama bilgisi aracılığıyla bir özellik bayrağı kullanılarak yönetilir.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-03.png)

İlk olarak, bu yeni bileşenler için uygun Istio hedef kuralları ve sanal Hizmetleri güncelleştirin. Bu güncelleştirmeler, trafiği yanlış yeni bileşenler için yol yok ve kullanıcılar, beklenmeyen erişim elde etmezsiniz emin olun:

```console
istioctl replace -f istio/step-3-add-routing-for-2.0-components.yaml --namespace voting
```

Aşağıdaki örnek çıktı, sanal Hizmetleri ve hedef kuralları başarıyla güncelleştirildi gösterir:

```
Updated config destination-rule/voting/voting-app to revision 150930
Updated config virtual-service/voting/voting-app to revision 150931
Updated config destination-rule/voting/voting-analytics to revision 150937
Updated config virtual-service/voting/voting-analytics to revision 150939
Updated config destination-rule/voting/voting-storage to revision 150940
Updated config virtual-service/voting/voting-storage to revision 150941
```

Ardından, yeni sürüm için Kubernetes nesneleri ekleyelim *2.0* bileşenleri. Ayrıca güncelleştirme *oylama depolama* içerecek şekilde hizmet *3306* MySQL için bağlantı noktası:

```console
kubectl apply -f kubernetes/step-3-update-voting-app-with-new-storage.yaml --namespace voting
```

Aşağıdaki örnek çıktıda, Kubernetes nesneler başarıyla oluşturulan veya güncelleştirilen gösterir:

```
service/voting-storage configured
secret/voting-storage-secret created
deployment.apps/voting-storage-2-0 created
persistentvolumeclaim/mysql-pv-claim created
deployment.apps/voting-analytics-2-0 created
deployment.apps/voting-app-2-0 created
```

Sürüme kadar bekleyin *2.0* pod'ların çalışıyor. Kullanım [kubectl pod'ları alma] [ kubectl-get] tüm pod'ların görüntülemek için komut *oylama* ad alanı:

```azurecli
kubectl get pods --namespace voting
```

Artık sürümleri arasında geçiş yapabilirsiniz olmalıdır *1.0* ve sürüm *2.0* oylama uygulamasının (kanarya). Özellik bayrağı aç/kapa ekranın alt kısmındaki bir tanımlama bilgisi ayarlar. Bu tanımlama bilgisi tarafından kullanılan *voting uygulamasını* rota kullanıcılara yeni sürüme sanal hizmet *2.0*.

![Sürüm 1.0 AKS Voting uygulamasını - özellik bayrağı değil olarak ayarlanmış olması gerekir.](media/istio/canary-release-01.png)

![Sürüm 2.0 AKS Voting uygulamasını - özellik bayrağı olduğu ayarlanmış olması gerekir.](media/istio/canary-release-02.png)

Oy Sayısı, uygulama sürümleri arasında farklılık gösterir. Bu fark, iki farklı depolama arka uçları kullanarak vurgular.

## <a name="finalize-the-rollout"></a>Piyasaya çıkma Sonlandır

Vamp başarıyla test ettikten sonra güncelleştirme *voting uygulamasını* sürümüne tüm trafiği yönlendirmek için sanal hizmet *2.0* , *voting uygulamasını* bileşeni. Tüm kullanıcılar sürüm bkz *2.0* özellik bayrağının ayarlanmasına bağımsız olarak uygulamanın:

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-04.png)

Tüm hedef bileşenlerin sürümlerini kaldırmak için artık etkin istediğiniz kurallarını güncelleştirin. Ardından, tüm sanal sürümler başvuran durdurmak için Hizmetleri güncelleştirin.

Artık olduğundan bileşenlerin eski sürümlerinin herhangi birinin tüm trafiği, bu bileşenler için tüm dağıtımlar artık güvenli bir şekilde silebilirsiniz.

![Oylama uygulaması bileşenleri ve yönlendirme AKS.](media/istio/components-and-routing-05.png)

Artık başarıyla AKS Voting uygulamasını yeni bir sürümü alındı.

## <a name="next-steps"></a>Sonraki adımlar

Kullanan ek senaryoları inceleyebilirsiniz [Istio Bookinfo uygulama örneği][istio-bookinfo-example].

<!-- LINKS - external -->
[github-azure-sample]: https://github.com/Azure-Samples/aks-voting-app
[istio]: https://istio.io
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-requirements-pods-and-services]: https://istio.io/docs/setup/kubernetes/spec-requirements/
[istio-reference-gateway]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#Gateway
[istio-reference-policy]: https://istio.io/docs/reference/config/istio.authentication.v1alpha1/#Policy
[istio-reference-virtualservice]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#VirtualService
[istio-reference-destinationrule]: https://istio.io/docs/reference/config/istio.networking.v1alpha3/#DestinationRule
[kubernetes-service]: https://kubernetes.io/docs/concepts/services-networking/service/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-install]: ./istio-install.md