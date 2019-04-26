---
title: Azure Kubernetes Service'i (AKS) Istio yükleyin
description: Bir Azure Kubernetes Service (AKS) kümesi içinde bir hizmet kafes oluşturmak için Istio yükleyip kullanmayı öğrenin
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 12/3/2018
ms.author: pabouwer
ms.openlocfilehash: d85b830b63e2d52f3eeb5df8645edccfccf43c76
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60465351"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Yükleme ve Istio Azure Kubernetes Service (AKS) kullanma

[Istio] [ istio-github] Kubernetes kümesindeki mikro hizmetler arasında önemli bir dizi işlev sağlayan bir açık kaynak hizmeti kafes olduğu. Bu özellikler, trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlaması ve observability içerir. Resmi Istio hakkında daha fazla bilgi için bkz. [Istio nedir?] [ istio-docs-concepts] belgeleri.

Bu makalede nasıl Istio yükleneceği gösterilmektedir. Istio `istioctl` istemci ikili İstemci makinenizde yüklü ve AKS bir Kubernetes kümesinde Istio bileşenleri yüklenir. Bu yönergeler Istio sürümü başvuru *1.0.4*. Ek Istio sürümlerin bulabilirsiniz [GitHub - Istio yayınlar][istio-github-releases].

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Istio indirin
> * İkili Istio istemcisini yükleme
> * Istio Kubernetes bileşenlerini yükleme
> * Yüklemeyi doğrulama

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede ayrıntılı adımlarda bir AKS kümesi oluşturduğunuz varsayılır (Kubernetes 1.10 ve yukarıdaki RBAC ile etkin) ve yerleşik olduğu bir `kubectl` kümeyle bağlantı. Bu öğelerden herhangi birinin yardıma ihtiyacınız varsa bkz [AKS hızlı başlangıçları][aks-quickstart].

Istio yüklemek için ihtiyacınız [Helm] [ helm] sürüm *2.10.0* veya daha sonra doğru yüklendiğinden ve kümenizde yapılandırılmış. Helm yükleme konusunda Yardım gerekiyorsa bkz [AKS Helm yükleme yönergeleri][helm-install]. En az yoksa sürüm *2.10.0* Helm yüklenen, yükseltme veya bakın [Istio - Helm Kılavuzu yüklemesiyle] [ istio-install-helm] diğer yükleme seçenekleri için.

Bu makalede, çeşitli ayrı adımlara Istio yükleme yönergeleri ayırır. Istio yüklemeyi öğrenin ve Istio Kubernetes ile nasıl çalıştığı hakkında bilgi için bu adımların her biri açıklanmaktadır. Sonuç resmi Istio yükleme yapısındaki aynıdır [Kılavuzu][istio-install-k8s-quickstart].

## <a name="download-istio"></a>Istio indirin

İlk olarak, indirin ve en son sürüm Istio ayıklayın. Adımlar, bir bash kabuğunda MacOS, Linux veya bir PowerShell kabuk ve Linux için Windows alt sistemi için biraz farklıdır. Tercih ettiğiniz ortam için adımlar aşağıdaki yükleme birini seçin:

* [Linux için MacOS, Linux veya Windows alt sistemi bash](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

Macos'ta, `curl` Istio en son sürümü indirin ve ardından ile ayıklamak için `tar` gibi:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Linux veya Linux için Windows alt sistemi, `curl` Istio en son sürümü indirin ve ardından ile ayıklamak için `tar` gibi:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.0.4

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

### <a name="powershell"></a>PowerShell

PowerShell'de kullanın [Invoke-WebRequest] [ Invoke-WebRequest] Istio en son sürümü indirin ve ardından ile ayıklamak için [genişletme arşiv] [ Expand-Archive] olarak aşağıdaki gibidir:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.0.4"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

## <a name="install-the-istio-client-binary"></a>İkili Istio istemcisini yükleme

`istioctl` İstemci ikili İstemci makinenizde çalıştırır ve Istio yönlendirme kuralları ve ilkeleri yönetmenize olanak sağlar. Yeniden yükleme adımları, istemci işletim sistemleri arasında biraz farklılık gösterir. Tercih ettiğiniz ortam için adımlar aşağıdaki yükleme birini seçin.

> [!IMPORTANT]
> Yüklediğiniz ve açtığınız Istio sürüm üst düzey klasöründen adımları Bu bölümde, çalıştırdığınızdan emin olun.

### <a name="macos"></a>macOS

Istio yüklemek için `istioctl` MacOS üzerinde bash tabanlı bir kabuk istemci ikili aşağıdaki komutları kullanın. Bu komutlar kopyalama `istioctl` istemci standart kullanıcı programı konumu için ikili, `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Komut satırı tamamlanmasını Istio isterseniz `istioctl` istemci ikili, ardından bunu gibi:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Şimdi bölümüne üzerine Taşı [Istio Kubernetes bileşenleri yüklemek](#install-the-istio-kubernetes-components).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux veya Linux için Windows alt sistemi

Istio yüklemek için aşağıdaki komutları kullanın `istioctl` istemci ikili Linux'ta bash tabanlı bir kabuk içinde veya [Linux için Windows alt sistemi][install-wsl]. Bu komutlar kopyalama `istioctl` istemci standart kullanıcı programı konumu için ikili, `PATH`.

```bash
cd istio-$ISTIO_VERSION
chmod +x ./bin/istioctl
sudo mv ./bin/istioctl /usr/local/bin/istioctl
```

Komut satırı tamamlanmasını Istio isterseniz `istioctl` istemci ikili, ardından bunu gibi:

```bash
# Generate the bash completion file and source it in your current shell
mkdir -p ~/completions && istioctl collateral --bash -o ~/completions
source ~/completions/istioctl.bash

# Source the bash completion file in your .bashrc so that the command-line completions
# are permanently available in your shell
echo "source ~/completions/istioctl.bash" >> ~/.bashrc
```

Şimdi bölümüne üzerine Taşı [Istio Kubernetes bileşenleri yüklemek](#install-the-istio-kubernetes-components).

### <a name="windows"></a>Windows

Istio yüklemek için `istioctl` , Windows Powershell tabanlı bir kabuk istemci ikili aşağıdaki komutları kullanın. Bu komutlar kopyalama `istioctl` istemci ikili yeni bir kullanıcıya program konumu ve aracılığıyla kullanılabilir hale getirmek, `PATH`.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:/Program Files/Istio"
mv ./bin/istioctl.exe "C:/Program Files/Istio/"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Program Files\Istio\", "User")
```

## <a name="install-the-istio-kubernetes-components"></a>Istio Kubernetes bileşenlerini yükleme

> [!IMPORTANT]
> Yüklediğiniz ve açtığınız Istio sürüm üst düzey klasöründen adımları Bu bölümde, çalıştırdığınızdan emin olun.

> [!NOTE]
> Sürüm `1.0.6` ve Istio Helm grafiğinin yeni değişiklikler vardır. Bu sürümü yüklemeyi seçerseniz, artık bir gizli dizi Kiali için el ile oluşturmanız gerekir. Gizli dizi ayarladıysanız Grafana için el ile oluşturmanız gerekecektir `grafana.security.enabled=true`. Istio Helm grafiği görmek [README.md](https://github.com/istio/istio/tree/master/install/kubernetes/helm/istio#installing-the-chart) bu gizli anahtarları oluşturma hakkında daha fazla bilgi için.

AKS kümenizi Istio bileşenleri yüklemek için Helm kullanın. Istio kaynakları yükleme `istio-system` ad alanını ve güvenlik ve izleme gibi ek seçenekleri etkinleştirebilirsiniz:

```azurecli
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set grafana.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

Helm grafiği, çok sayıda nesneleri dağıtır. Dağıtımın tamamlanması küme ortamına bağlı olarak 2 ila 3 dakika sürebilir.

## <a name="validate-the-installation"></a>Yüklemeyi doğrulama

Istio'nın başarılı dağıtımı sahip olduğunuzdan emin olmak için şimdi yüklemeyi doğrulayın.

İlk beklenen Hizmetleri oluşturulmuş olduğunu doğrulayın. Kullanım [kubectl alma svc] [ kubectl-get] çalışmakta olan hizmetlerin görmek için komutu. Sorgu *istio sistem* burada Istio ve eklenti bileşeni yüklenmedi Helm grafiği ad alanı:

```console
kubectl get svc --namespace istio-system --output wide
```

Aşağıdaki örnek çıktıda, artık çalıştırılması gerektiği hizmetler gösterilmektedir:

- *istio -** Hizmetleri
- * jaeger-**, *izleme*, ve *zipkin* eklenti izleme Hizmetleri
- *prometheus* eklenti ölçümleri hizmeti
- *grafana* eklenti analiz ve izleme panosu hizmeti
- *kiali* eklenti hizmeti kafes Panosu

Varsa `istio-ingressgateway` bir dış IP gösterir `<pending>`, Azure ağı ile bir IP adresi atanmış olan kadar birkaç dakika bekleyin.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                                                                                                                   AGE       SELECTOR
grafana                  ClusterIP      10.0.26.60     <none>           3000/TCP                                                                                                                  3m        app=grafana
istio-citadel            ClusterIP      10.0.88.87     <none>           8060/TCP,9093/TCP                                                                                                         3m        istio=citadel
istio-egressgateway      ClusterIP      10.0.115.115   <none>           80/TCP,443/TCP                                                                                                            3m        app=istio-egressgateway,istio=egressgateway
istio-galley             ClusterIP      10.0.104.183   <none>           443/TCP,9093/TCP                                                                                                          3m        istio=galley
istio-ingressgateway     LoadBalancer   10.0.12.216    52.187.250.239   80:31380/TCP,443:31390/TCP,31400:31400/TCP,15011:30469/TCP,8060:31999/TCP,853:31235/TCP,15030:32000/TCP,15031:30293/TCP   3m        app=istio-ingressgateway,istio=ingressgateway
istio-pilot              ClusterIP      10.0.38.134    <none>           15010/TCP,15011/TCP,8080/TCP,9093/TCP                                                                                     3m        istio=pilot
istio-policy             ClusterIP      10.0.253.81    <none>           9091/TCP,15004/TCP,9093/TCP                                                                                               3m        istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.181.186   <none>           443/TCP                                                                                                                   3m        istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.177.113   <none>           9091/TCP,15004/TCP,9093/TCP,42422/TCP                                                                                     3m        istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>           5775/UDP,6831/UDP,6832/UDP                                                                                                3m        app=jaeger
jaeger-collector         ClusterIP      10.0.112.81    <none>           14267/TCP,14268/TCP                                                                                                       3m        app=jaeger
jaeger-query             ClusterIP      10.0.179.193   <none>           16686/TCP                                                                                                                 3m        app=jaeger
kiali                    ClusterIP      10.0.211.63    <none>           20001/TCP                                                                                                                 3m        app=kiali
prometheus               ClusterIP      10.0.70.113    <none>           9090/TCP                                                                                                                  3m        app=prometheus
tracing                  ClusterIP      10.0.139.121   <none>           80/TCP                                                                                                                    3m        app=jaeger
zipkin                   ClusterIP      10.0.60.155    <none>           9411/TCP                                                                                                                  3m        app=jaeger
```

Ardından, gerekli pod'ların oluşturulduğunu onaylayın. Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu ve yeniden sorgula *istio sistem* ad alanı:

```console
kubectl get pods --namespace istio-system
```

Aşağıdaki örnek çıktı, çalıştırılan pod'ları gösterir:

- *istio -** pod'ları
- *prometheus -** eklenti ölçümleri pod
- *grafana -** eklenti analiz ve izleme Pano podunun
- *kiali* eklenti hizmeti kafes Pano podunun

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-59b787b9b-cr6d7                  1/1       Running     0          6m
istio-citadel-78df8c67d9-9lfpf           1/1       Running     0          6m
istio-egressgateway-6b96cd7f5-k848h      1/1       Running     0          6m
istio-galley-58f566cb66-8mhbv            1/1       Running     0          6m
istio-ingressgateway-6cbbf596f6-6jz8g    1/1       Running     0          6m
istio-pilot-8449d555fc-sl6kp             2/2       Running     0          6m
istio-policy-6b99d88bc5-55s52            2/2       Running     0          6m
istio-sidecar-injector-b88dfb954-8m86s   1/1       Running     0          6m
istio-telemetry-675cb4cb9d-8s7wd         2/2       Running     0          6m
istio-tracing-7596597bd7-sbnt9           1/1       Running     0          6m
kiali-5fbd6ffb-4qcxw                     1/1       Running     0          6m
prometheus-76db5fddd5-2tkxs              1/1       Running     0          6m
```

Tüm pod'ların durumunu göster `Running`. Bu durumlar podlarınız yoksa, bunu bir veya iki dakika bekleyin. Tüm pod'ların bir sorunu bildirmek kullanırsanız [kubectl açıklayan pod] [ kubectl-describe] , çıkış ve durumunu gözden geçirmek için komut.

## <a name="accessing-the-add-ons"></a>Eklentileri erişme

Eklenti sayısına yüklenen Istio ek işlevsellik sağlayan bizim kurulumunda yukarıdaki. Eklentiler için kullanıcı arabirimleri dış IP adresi genel olarak açık değildir. Eklenti kullanıcı arabirimleri erişmek için [kubectl bağlantı noktası-İleri] [ kubectl-port-forward] komutu. Bu komut, AKS kümenizin İstemci makinenizde yerel bağlantı noktası ve ilgili pod arasında güvenli bir bağlantı oluşturur.

### <a name="grafana"></a>Grafana

Analiz ve izleme panoları Istio tarafından sağlanan için [Grafana][grafana]. Yerel bağlantı noktası iletme *3000* bağlantı noktasına İstemci makinenizde *3000* Grafana AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

Grafana için yapılandırılan bağlantı noktası-İleri aşağıdaki örnek çıktı gösterilmektedir:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

İstemci makinenizde - artık Grafana şu URL'den ulaşabilirsiniz [ http://localhost:3000 ](http://localhost:3000).

### <a name="prometheus"></a>Prometheus

Ölçümler için ifade tarayıcı tarafından sağlanan [Prometheus][prometheus]. Yerel bağlantı noktası iletme *9090* bağlantı noktasına İstemci makinenizde *9090* Prometheus AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=prometheus -o jsonpath='{.items[0].metadata.name}') 9090:9090
```

Aşağıdaki örnek çıktıda Prometheus için yapılandırılan bağlantı noktası-İleri gösterir:

```console
Forwarding from 127.0.0.1:9090 -> 9090
Forwarding from [::1]:9090 -> 9090
```

İstemci makinenizde - artık Prometheus ifade tarayıcı şu URL'den ulaşabilirsiniz [ http://localhost:9090 ](http://localhost:9090).

### <a name="jaeger"></a>Jaeger

Kullanıcı arabirimi izleme tarafından sağlanır [Jaeger][jaeger]. Yerel bağlantı noktası iletme *16686* bağlantı noktasına İstemci makinenizde *16686* Jaeger AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=jaeger -o jsonpath='{.items[0].metadata.name}') 16686:16686
```

Aşağıdaki örnek çıktıda Jaeger için yapılandırılan bağlantı noktası-İleri gösterir:

```console
Forwarding from 127.0.0.1:16686 -> 16686
Forwarding from [::1]:16686 -> 16686
```

İstemci makinenizde - artık Jaeger izleme kullanıcı arabirimi şu URL'den ulaşabilirsiniz [ http://localhost:16686 ](http://localhost:16686).

### <a name="kiali"></a>Kiali

Hizmet kafes observability Panosu tarafından sağlanan [Kiali][kiali]. Yerel bağlantı noktası iletme *20001* bağlantı noktasına İstemci makinenizde *20001* Kiali AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

Aşağıdaki örnek çıktıda Kiali için yapılandırılan bağlantı noktası-İleri gösterir:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

İstemci makinenizde - artık Kiali hizmet kafes observability Panosu şu URL'den ulaşabilirsiniz [ http://localhost:20001 ](http://localhost:20001).

Varsayılan kullanıcı adı ve parola Kiali Pano *username:admin / parola: admin*. Bu kimlik bilgileri aracılığıyla yapılandırılabilir *kiali.dashboard.username* ve *kiali.dashboard.passphrase* Helm değerleri.

## <a name="next-steps"></a>Sonraki adımlar

Uygulamanızı birden çok sürümleri arasında akıllı yönlendirme sağlamak ve vamp alma için Istio nasıl kullanabileceğinizi görmek için aşağıdaki belgelere bakın:

> [!div class="nextstepaction"]
> [AKS Istio akıllı yönlendirme senaryosu][istio-scenario-routing]

Daha fazla yükleme ve yapılandırma seçeneklerini Istio için keşfetmek için resmi Istio aşağıdaki makalelere bakın:

- [Istio - Kubernetes yükleme hızlı başlangıç][istio-install-k8s-quickstart]
- [Istio - Helm Yükleme Kılavuzu][istio-install-helm]
- [Istio - Helm yükleme seçenekleri][istio-install-helm-options]

Ayrıca ek senaryoları kullanarak izleyebileceğiniz [Istio Bookinfo uygulama örneği][istio-bookinfo-example].

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh
[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-k8s-quickstart]: https://istio.io/docs/setup/kubernetes/quick-start/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/helm-install/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10
[kubernetes-crd]: https://kubernetes.io/docs/tasks/access-kubernetes-api/custom-resources/custom-resource-definitions/
[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md
[Invoke-WebRequest]: /powershell/module/microsoft.powershell.utility/invoke-webrequest
[Expand-Archive]: /powershell/module/Microsoft.PowerShell.Archive/Expand-Archive
