---
title: HTTP uygulama yönlendirme eklenti Azure Kubernetes Service'teki (AKS)
description: HTTP uygulama yönlendirme eklentisi, Azure Kubernetes Service (AKS) kullanın.
services: container-service
author: lachie83
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 04/25/2018
ms.author: laevenso
ms.openlocfilehash: d6e1cc033416c90e27b5caf4bba310400e55b3a5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60466329"
---
# <a name="http-application-routing"></a>HTTP uygulaması yönlendirme

HTTP uygulama yönlendirme çözümü, Azure Kubernetes Service (AKS) kümenize dağıtılan uygulamalara erişmek kolaylaştırır. Çözümün etkin olduğunda, bir giriş denetleyicisine AKS kümenizin yapılandırır. Çözüm, ayrıca dağıtılan uygulamalar gibi uygulama uç noktaları için ortak olarak erişilebilen DNS adları oluşturur.

Eklenti etkinleştirildiğinde, aboneliğinizde bir DNS bölgesi oluşturur. DNS maliyet hakkında daha fazla bilgi için bkz: [DNS fiyatlandırma][dns-pricing].

> [!CAUTION]
> HTTP uygulama yönlendirme eklenti giriş denetleyicisine hızla oluşturun ve uygulamalarınızın erişim sağlamak için tasarlanmıştır. Bu eklenti, üretim kullanımı için önerilmez. Birden çok çoğaltmalar ve TLS içeren üretime hazır giriş dağıtımlarını desteklemek için bkz: [bir HTTPS giriş denetleyicisine oluşturma](https://docs.microsoft.com/azure/aks/ingress-tls).

## <a name="http-routing-solution-overview"></a>HTTP yönlendirme çözümüne genel bakış

Eklenti iki bileşenlerini dağıtır: bir [Kubernetes giriş denetleyicisine] [ ingress] ve [dış DNS] [ external-dns] denetleyicisi.

- **Giriş denetleyicisine**: Giriş denetleyicisine LoadBalancer türünde bir Kubernetes hizmetini kullanarak internet'e açıktır. Giriş denetleyicisine izler ve uygulayan [Kubernetes giriş kaynakları][ingress-resource], uygulama uç noktaları için rotalar oluşturur.
- **Dış DNS denetleyicisi**: Kubernetes giriş kaynakları izler ve kümeye özgü DNS bölgesinde DNS A kayıtlarını da oluşturur.

## <a name="deploy-http-routing-cli"></a>HTTP yönlendirme dağıtın: CLI

HTTP uygulama yönlendirme eklentisi, Azure CLI ile bir AKS kümesi dağıtırken etkinleştirilebilir. Bunu yapmak için [az aks oluşturma] [ az-aks-create] komutunu `--enable-addons` bağımsız değişken.

```azurecli
az aks create --resource-group myResourceGroup --name myAKSCluster --enable-addons http_application_routing
```

> [!TIP]
> Birden çok eklenti etkinleştirmek istiyorsanız, bunları bir virgülle ayrılmış liste olarak belirtin. Örneğin, HTTP uygulama Yönlendirme ve izlemeyi etkinleştirmek için biçimini kullanın. `--enable-addons http_application_routing,monitoring`.

HTTP kullanarak varolan bir AKS kümesi üzerinde yönlendirme de etkinleştirebilirsiniz [az aks enable-addons] [ az-aks-enable-addons] komutu. Var olan bir kümede HTTP yönlendirme etkinleştirmek için eklemeniz `--addons` parametresi belirtin *http_application_routing* aşağıdaki örnekte gösterildiği gibi:

```azurecli
az aks enable-addons --resource-group myResourceGroup --name myAKSCluster --addons http_application_routing
```

Kümeye dağıtılan veya güncelleştirildikten sonra kullanın [az aks show] [ az-aks-show] DNS bölge adını almak için komutu. Bu ad, AKS kümesi uygulama dağıtmak için gereklidir.

```azurecli
$ az aks show --resource-group myResourceGroup --name myAKSCluster --query addonProfiles.httpApplicationRouting.config.HTTPApplicationRoutingZoneName -o table

Result
-----------------------------------------------------
9f9c1fe7-21a1-416d-99cd-3543bb92e4c3.eastus.aksapp.io
```

## <a name="deploy-http-routing-portal"></a>HTTP yönlendirme dağıtın: Portal

HTTP uygulama yönlendirme eklentisi, bir AKS kümesi dağıtırken Azure portalı üzerinden etkinleştirilebilir.

![HTTP yönlendirme özelliğini etkinleştir](media/http-routing/create.png)

Küme dağıtıldıktan sonra otomatik olarak oluşturulan bir AKS kaynak grubuna gidin ve DNS bölgesini seçin. DNS bölge adını not alın. Bu ad, AKS kümesi uygulama dağıtmak için gereklidir.

![DNS bölge adını Al](media/http-routing/dns.png)

## <a name="use-http-routing"></a>HTTP yönlendirme kullanın

HTTP uygulama yönlendirme çözümü gibi ek açıklamalı giriş kaynaklar üzerinde yalnızca tetiklenebilir:

```yaml
annotations:
  kubernetes.io/ingress.class: addon-http-application-routing
```

Adlı bir dosya oluşturun **örnekleri-http-uygulaması-routing.yaml** aşağıdaki YAML'ye kopyalayın. 43 satırında güncelleştirme `<CLUSTER_SPECIFIC_DNS_ZONE>` DNS bölge adı ile bu makalenin önceki adımda toplanan.


```yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: party-clippy
spec:
  template:
    metadata:
      labels:
        app: party-clippy
    spec:
      containers:
      - image: r.j3ss.co/party-clippy
        name: party-clippy
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 250m
            memory: 256Mi
        tty: true
        command: ["party-clippy"]
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: party-clippy
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 8080
  selector:
    app: party-clippy
  type: ClusterIP
---
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: party-clippy
  annotations:
    kubernetes.io/ingress.class: addon-http-application-routing
spec:
  rules:
  - host: party-clippy.<CLUSTER_SPECIFIC_DNS_ZONE>
    http:
      paths:
      - backend:
          serviceName: party-clippy
          servicePort: 80
        path: /
```

Kullanım [kubectl uygulamak] [ kubectl-apply] kaynakları oluşturmak için komutu.

```bash
$ kubectl apply -f samples-http-application-routing.yaml

deployment "party-clippy" created
service "party-clippy" created
ingress "party-clippy" created
```

Http uygulama routing.yaml örnekleri dosyasının ana bölümünde belirtilen ana bilgisayar adı gitmek için cURL veya bir tarayıcı kullanın. Uygulama, internet üzerinden kullanılabilir olmadan önce en fazla bir dakika sürebilir.

```bash
$ curl party-clippy.471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io

 _________________________________
/ It looks like you're building a \
\ microservice.                   /
 ---------------------------------
 \
  \
     __
    /  \
    |  |
    @  @
    |  |
    || |/
    || ||
    |\_/|
    \___/

```

## <a name="remove-http-routing"></a>HTTP yönlendirme Kaldır

HTTP yönlendirme çözümü Azure CLI kullanarak kaldırabilirsiniz. Bunu yapmak için aşağıdaki komutu çalıştırın, değiştirme, AKS kümesi ve kaynak grubu adı.

```azurecli
az aks disable-addons --addons http_application_routing --name myAKSCluster --resource-group myResourceGroup --no-wait
```

HTTP uygulama yönlendirme eklentiyi devre dışı bırakıldığında, kümedeki bazı Kubernetes kaynakları kalabilir. Bu kaynaklar içerir *configMaps* ve *gizli dizileri*ve oluşturulan *kube-system* ad alanı. Temiz bir küme korumak için bu kaynakları kaldırmak isteyebilirsiniz.

Aranan *eklentisi-http-uygulaması-yönlendirme* aşağıdakileri kullanarak kaynakları [kubectl alma] [ kubectl-get] komutları:

```console
kubectl get deployments --namespace kube-system
kubectl get services --namespace kube-system
kubectl get configmaps --namespace kube-system
kubectl get secrets --namespace kube-system
```

Silinmesi gereken configMaps aşağıdaki örnek çıktı gösterilmektedir:

```
$ kubectl get configmaps --namespace kube-system

NAMESPACE     NAME                                                       DATA   AGE
kube-system   addon-http-application-routing-nginx-configuration         0      9m7s
kube-system   addon-http-application-routing-tcp-services                0      9m7s
kube-system   addon-http-application-routing-udp-services                0      9m7s
```

Kaynakları silmek için kullanın [kubectl Sil] [ kubectl-delete] komutu. Kaynak türü, kaynak adı ve ad alanı belirtin. Aşağıdaki örnek, bir önceki configmaps siler:

```console
kubectl delete configmaps addon-http-application-routing-nginx-configuration --namespace kube-system
```

Önceki yineleyin `kubectl delete` adım için tüm *eklentisi-http-uygulaması-yönlendirme* kümenizde kalan kaynaklar.

## <a name="troubleshoot"></a>Sorun giderme

Kullanım [kubectl günlükleri] [ kubectl-logs] dış DNS uygulama için uygulama günlüklerini görmek için komutu. Günlükler, bir A ve TXT DNS kaydı, başarıyla oluşturuldu onaylamalıdır.

```
$ kubectl logs -f deploy/addon-http-application-routing-external-dns -n kube-system

time="2018-04-26T20:36:19Z" level=info msg="Updating A record named 'party-clippy' to '52.242.28.189' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
time="2018-04-26T20:36:21Z" level=info msg="Updating TXT record named 'party-clippy' to '"heritage=external-dns,external-dns/owner=default"' for Azure DNS zone '471756a6-e744-4aa0-aa01-89c4d162a7a7.canadaeast.aksapp.io'."
```

Bu kayıtlar, ayrıca Azure portalında DNS bölgesi kaynağı üzerinde görülebilir.

![DNS kayıtlarını alma](media/http-routing/clippy.png)

Kullanım [kubectl günlükleri] [ kubectl-logs] Ngınx giriş denetleyicisine için uygulama günlüklerini görmek için komutu. Günlükleri onaylamalıdır `CREATE` bir giriş kaynağı ve denetleyicisinin yeniden yükle. Tüm HTTP etkinlik günlüğe kaydedilir.

```bash
$ kubectl logs -f deploy/addon-http-application-routing-nginx-ingress-controller -n kube-system

-------------------------------------------------------------------------------
NGINX Ingress controller
  Release:    0.13.0
  Build:      git-4bc943a
  Repository: https://github.com/kubernetes/ingress-nginx
-------------------------------------------------------------------------------

I0426 20:30:12.212936       9 flags.go:162] Watching for ingress class: addon-http-application-routing
W0426 20:30:12.213041       9 flags.go:165] only Ingress with class "addon-http-application-routing" will be processed by this ingress controller
W0426 20:30:12.213505       9 client_config.go:533] Neither --kubeconfig nor --master was specified.  Using the inClusterConfig.  This might not work.
I0426 20:30:12.213752       9 main.go:181] Creating API client for https://10.0.0.1:443
I0426 20:30:12.287928       9 main.go:225] Running in Kubernetes Cluster version v1.8 (v1.8.11) - git (clean) commit 1df6a8381669a6c753f79cb31ca2e3d57ee7c8a3 - platform linux/amd64
I0426 20:30:12.290988       9 main.go:84] validated kube-system/addon-http-application-routing-default-http-backend as the default backend
I0426 20:30:12.294314       9 main.go:105] service kube-system/addon-http-application-routing-nginx-ingress validated as source of Ingress status
I0426 20:30:12.426443       9 stat_collector.go:77] starting new nginx stats collector for Ingress controller running in namespace  (class addon-http-application-routing)
I0426 20:30:12.426509       9 stat_collector.go:78] collector extracting information from port 18080
I0426 20:30:12.448779       9 nginx.go:281] starting Ingress controller
I0426 20:30:12.463585       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-nginx-configuration", UID:"2588536c-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"559", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-nginx-configuration
I0426 20:30:12.466945       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-tcp-services", UID:"258ca065-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"561", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-tcp-services
I0426 20:30:12.467053       9 event.go:218] Event(v1.ObjectReference{Kind:"ConfigMap", Namespace:"kube-system", Name:"addon-http-application-routing-udp-services", UID:"259023bc-4990-11e8-a5e1-0a58ac1f0ef2", APIVersion:"v1", ResourceVersion:"562", FieldPath:""}): type: 'Normal' reason: 'CREATE' ConfigMap kube-system/addon-http-application-routing-udp-services
I0426 20:30:13.649195       9 nginx.go:302] starting NGINX process...
I0426 20:30:13.649347       9 leaderelection.go:175] attempting to acquire leader lease  kube-system/ingress-controller-leader-addon-http-application-routing...
I0426 20:30:13.649776       9 controller.go:170] backend reload required
I0426 20:30:13.649800       9 stat_collector.go:34] changing prometheus collector from  to default
I0426 20:30:13.662191       9 leaderelection.go:184] successfully acquired lease kube-system/ingress-controller-leader-addon-http-application-routing
I0426 20:30:13.662292       9 status.go:196] new leader elected: addon-http-application-routing-nginx-ingress-controller-5cxntd6
I0426 20:30:13.763362       9 controller.go:179] ingress backend successfully reloaded...
I0426 21:51:55.249327       9 event.go:218] Event(v1.ObjectReference{Kind:"Ingress", Namespace:"default", Name:"party-clippy", UID:"092c9599-499c-11e8-a5e1-0a58ac1f0ef2", APIVersion:"extensions", ResourceVersion:"7346", FieldPath:""}): type: 'Normal' reason: 'CREATE' Ingress default/party-clippy
W0426 21:51:57.908771       9 controller.go:775] service default/party-clippy does not have any active endpoints
I0426 21:51:57.908951       9 controller.go:170] backend reload required
I0426 21:51:58.042932       9 controller.go:179] ingress backend successfully reloaded...
167.220.24.46 - [167.220.24.46] - - [26/Apr/2018:21:53:20 +0000] "GET / HTTP/1.1" 200 234 "" "Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Trident/5.0)" 197 0.001 [default-party-clippy-80] 10.244.0.13:8080 234 0.004 200
```

## <a name="clean-up"></a>Temizleme

Bu makalede oluşturulan ilişkili Kubernetes nesnelerini kaldırın.

```bash
$ kubectl delete -f samples-http-application-routing.yaml

deployment "party-clippy" deleted
service "party-clippy" deleted
ingress "party-clippy" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

AKS içinde bir HTTPS güvenli giriş denetleyicisine yükleme hakkında daha fazla bilgi için bkz: [HTTPS giriş Azure Kubernetes Service'teki (AKS)][ingress-https].

<!-- LINKS - internal -->
[az-aks-create]: /cli/azure/aks?view=azure-cli-latest#az-aks-create
[az-aks-show]: /cli/azure/aks?view=azure-cli-latest#az-aks-show
[ingress-https]: ./ingress-tls.md
[az-aks-enable-addons]: /cli/azure/aks#az-aks-enable-addons


<!-- LINKS - external -->
[dns-pricing]: https://azure.microsoft.com/pricing/details/dns/
[external-dns]: https://github.com/kubernetes-incubator/external-dns
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs
[ingress]: https://kubernetes.io/docs/concepts/services-networking/ingress/
[ingress-resource]: https://kubernetes.io/docs/concepts/services-networking/ingress/#the-ingress-resource
