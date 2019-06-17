---
title: Azure Kubernetes Service'i (AKS) Istio yükleyin
description: Bir Azure Kubernetes Service (AKS) kümesi içinde bir hizmet kafes oluşturmak için Istio yükleyip kullanmayı öğrenin
services: container-service
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 04/19/2019
ms.author: pabouwer
ms.openlocfilehash: 33d86ab8c88b45c7787620773f0df6e7fe888cf3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65850408"
---
# <a name="install-and-use-istio-in-azure-kubernetes-service-aks"></a>Yükleme ve Istio Azure Kubernetes Service (AKS) kullanma

[Istio] [ istio-github] Kubernetes kümesindeki mikro hizmetler arasında önemli bir dizi işlev sağlayan bir açık kaynak hizmeti kafes olduğu. Bu özellikler, trafik yönetimi, hizmet kimliği ve güvenlik, ilke zorlaması ve observability içerir. Resmi Istio hakkında daha fazla bilgi için bkz. [Istio nedir?] [ istio-docs-concepts] belgeleri.

Bu makalede nasıl Istio yükleneceği gösterilmektedir. Istio `istioctl` istemci ikili İstemci makinenizde yüklü olduğundan ve AKS bir Kubernetes kümesinde Istio bileşenleri yüklenir.

> [!NOTE]
> Bu yönergeler Istio sürümü başvuru `1.1.3`.
>
> Istio `1.1.x` sürümleri, Kubernetes sürümlerini karşı Istio ekibi tarafından sınanmıştır `1.11`, `1.12`, `1.13`. Ek Istio sürümlerin bulabilirsiniz [GitHub - Istio sürümleri] [ istio-github-releases] ve her biri sürümleri hakkında bilgi [Istio - sürüm notları] [ istio-release-notes].

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Istio indirin
> * İkili Istio istioctl istemcisini yükleme
> * AKS Istio CRD'ler yükleyin
> * AKS Istio bileşenlerini yükleme
> * Istio yüklemeyi doğrulama
> * Eklentileri erişme
> * Istio AKS kaldırın

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede ayrıntılı adımlarda bir AKS kümesi oluşturduğunuz varsayılır (Kubernetes `1.11` ve yukarıdaki RBAC ile etkin) ve yerleşik olduğu bir `kubectl` kümeyle bağlantı. Bu öğelerden herhangi birinin yardıma ihtiyacınız varsa bkz [AKS hızlı başlangıçları][aks-quickstart].

İhtiyacınız olacak [Helm] [ helm] Istio yükleyip bu yönergeleri izleyin. Sürümüne sahip önerilir `2.12.2` veya daha sonra doğru yüklendiğinden ve kümenizde yapılandırılmış. Helm yükleme yardıma ihtiyacınız varsa bkz [AKS Helm yükleme yönergeleri][helm-install]. Tüm Istio pod'ları, ayrıca Linux düğümleri üzerinde çalışmak için zamanlanmalıdır.

Bu makalede, çeşitli ayrı adımlara Istio yükleme yönergeleri ayırır. Sonuç resmi Istio yükleme yapısındaki aynıdır [Kılavuzu][istio-install-helm].

## <a name="download-istio"></a>Istio indirin

İlk olarak, indirin ve en son sürüm Istio ayıklayın. Adımlar, bir bash kabuğunda MacOS, Linux veya bir PowerShell kabuk ve Linux için Windows alt sistemi için biraz farklıdır. Tercih ettiğiniz ortam eşleşen yükleme aşağıdakilerden birini seçin:

* [Linux için MacOS, Linux veya Windows alt sistemi bash](#bash)
* [PowerShell](#powershell)

### <a name="bash"></a>Bash

Macos'ta, `curl` Istio en son sürümü indirin ve ardından ile ayıklamak için `tar` gibi:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

# MacOS
curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-osx.tar.gz" | tar xz
```

Linux veya Linux için Windows alt sistemi, `curl` Istio en son sürümü indirin ve ardından ile ayıklamak için `tar` gibi:

```bash
# Specify the Istio version that will be leveraged throughout these instructions
ISTIO_VERSION=1.1.3

curl -sL "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-linux.tar.gz" | tar xz
```

Şimdi bölümüne üzerine Taşı [Istio istioctl istemci ikili yükleme](#install-the-istio-istioctl-client-binary).

### <a name="powershell"></a>PowerShell

PowerShell'de kullanın `Invoke-WebRequest` Istio en son sürümü indirin ve ardından ile ayıklamak için `Expand-Archive` gibi:

```powershell
# Specify the Istio version that will be leveraged throughout these instructions
$ISTIO_VERSION="1.1.3"

# Windows
$ProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -URI "https://github.com/istio/istio/releases/download/$ISTIO_VERSION/istio-$ISTIO_VERSION-win.zip" -OutFile "istio-$ISTIO_VERSION.zip"
Expand-Archive -Path "istio-$ISTIO_VERSION.zip" -DestinationPath .
```

Şimdi bölümüne üzerine Taşı [Istio istioctl istemci ikili yükleme](#install-the-istio-istioctl-client-binary).

## <a name="install-the-istio-istioctl-client-binary"></a>İkili Istio istioctl istemcisini yükleme

> [!IMPORTANT]
> Yüklediğiniz ve açtığınız Istio sürüm üst düzey klasöründen adımları Bu bölümde, çalıştırdığınızdan emin olun.

`istioctl` İstemci ikili İstemci makinenizde çalıştırır ve Istio hizmet ağı ile etkileşimde bulunmanızı sağlar. Yükleme adımları, istemci işletim sistemleri arasında biraz farklılık gösterir. Tercih ettiğiniz ortam eşleşen yükleme aşağıdakilerden birini seçin:

* [MacOS](#macos)
* [Linux veya Linux için Windows alt sistemi](#linux-or-windows-subsystem-for-linux)
* [Windows](#windows)

### <a name="macos"></a>macOS

Istio yüklemek için `istioctl` MacOS üzerinde bash tabanlı bir kabuk istemci ikili aşağıdaki komutları kullanın. Bu komutlar kopyalama `istioctl` istemci standart kullanıcı programı konumu için ikili, `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
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

Artık sonraki bölüme Taşı [AKS Istio CRD'ler yükleme](#install-the-istio-crds-on-aks).

### <a name="linux-or-windows-subsystem-for-linux"></a>Linux veya Linux için Windows alt sistemi

Istio yüklemek için aşağıdaki komutları kullanın `istioctl` istemci ikili Linux'ta bash tabanlı bir kabuk içinde veya [Linux için Windows alt sistemi][install-wsl]. Bu komutlar kopyalama `istioctl` istemci standart kullanıcı programı konumu için ikili, `PATH`.

```bash
cd istio-$ISTIO_VERSION
sudo cp ./bin/istioctl /usr/local/bin/istioctl
sudo chmod +x /usr/local/bin/istioctl
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

Artık sonraki bölüme Taşı [AKS Istio CRD'ler yükleme](#install-the-istio-crds-on-aks).

### <a name="windows"></a>Windows

Istio yüklemek için `istioctl` istemci, ikili bir **Powershell**-Windows, temel Kabuk aşağıdaki komutları kullanın. Bu komutlar kopyalama `istioctl` istemci Istio klasörüne bir ikili ve aracılığıyla kalıcı olarak kullanılabilir hale getirmek, `PATH`. Bu komutları çalıştırmak için yükseltilmiş (Yönetici) ayrıcalıklarına gerekmez.

```powershell
cd istio-$ISTIO_VERSION
New-Item -ItemType Directory -Force -Path "C:\Istio"
Copy-Item -Path .\bin\istioctl.exe -Destination "C:\Istio\"
$PATH = [environment]::GetEnvironmentVariable("PATH", "User")
[environment]::SetEnvironmentVariable("PATH", $PATH + "; C:\Istio\", "User")
```

Artık sonraki bölüme Taşı [AKS Istio CRD'ler yükleme](#install-the-istio-crds-on-aks).

## <a name="install-the-istio-crds-on-aks"></a>AKS Istio CRD'ler yükleyin

> [!IMPORTANT]
> Yüklediğiniz ve açtığınız Istio sürüm üst düzey klasöründen adımları Bu bölümde, çalıştırdığınızdan emin olun.

Istio kullanan [özel kaynak tanımları (CRD'ler)] [ kubernetes-crd] çalışma zamanı yapılandırmasını yönetmek için. Istio bileşenleri bunlar üzerinde bir bağımlılığı olduğundan Istio CRD'ler yüklemeniz gerekir. Helm kullanın ve `istio-init` Istio CRD'ler içine yüklemek için grafik `istio-system` AKS kümenizde ad alanı:

```azurecli
helm install install/kubernetes/helm/istio-init --name istio-init --namespace istio-system
```

[İşleri] [ kubernetes-jobs] parçası olarak dağıtılan `istio-init` CRD'ler yüklemek için Helm grafiği. Bu işleri, küme ortamınıza bağlı olarak tamamlanması 1-2 dakika arasında sürer. İşleri gibi tamamladınız doğrulayabilirsiniz:

```azurecli
kubectl get jobs -n istio-system
```

Aşağıdaki örnek çıktıda başarıyla tamamlanmış işleri gösterir.

```console
NAME                COMPLETIONS   DURATION   AGE
istio-init-crd-10   1/1           16s        18s
istio-init-crd-11   1/1           15s        18s
```

Şimdi biz işleri başarılı olarak tamamlanmasına onayladıktan sonra biz Istio yüklü CRD'ler doğru sayıda sahip olduğunuzu doğrulayın. Ortamınız için uygun komutu çalıştırarak tüm 53 Istio CRD'ler yüklendiğini doğrulayabilirsiniz. Komut sayısı döndürmelidir `53`.

Bash

```bash
kubectl get crds | grep 'istio.io' | wc -l
```

PowerShell

```powershell
(kubectl get crds | Select-String -Pattern 'istio.io').Count
```

Bu noktada kendinizi, ardından Istio CRD'ler başarıyla yüklediniz anlamına gelir. Artık sonraki bölüme Taşı [AKS Istio bileşenleri yüklemek](#install-the-istio-components-on-aks).

## <a name="install-the-istio-components-on-aks"></a>AKS Istio bileşenlerini yükleme

> [!IMPORTANT]
> Yüklediğiniz ve açtığınız Istio sürüm üst düzey klasöründen adımları Bu bölümde, çalıştırdığınızdan emin olun.

Biz yüklenmesi [Grafana] [ grafana] ve [Kiali] [ kiali] bizim Istio yüklemesinin bir parçası olarak. Grafana analiz ve izleme panoları ve hizmet kafes observability Panosu Kiali sağlar. Bizim Kurulum, bu bileşenlerin her birinin olarak belirtilmelidir, kimlik bilgileri gerektiren bir [gizli][kubernetes-secrets].

Biz Istio bileşenlerini yüklemeden önce biz Grafana ve Kiali için gizli dizileri oluşturmalısınız. Bu gizli dizileri, ortamınız için uygun komutları çalıştırarak oluşturun.

### <a name="add-grafana-secret"></a>Grafana gizli dizi eklemek

Değiştirin `REPLACE_WITH_YOUR_SECURE_PASSWORD` belirteç parolanızla ve aşağıdaki komutları çalıştırın:

#### <a name="macos-linux"></a>macOS, Linux

```bash
GRAFANA_USERNAME=$(echo -n "grafana" | base64)
GRAFANA_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$GRAFANA_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("grafana"))
$GRAFANA_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: grafana
  namespace: istio-system
  labels:
    app: grafana
type: Opaque
data:
  username: $GRAFANA_USERNAME
  passphrase: $GRAFANA_PASSPHRASE" | kubectl apply -f -
```

### <a name="add-kiali-secret"></a>Kiali gizli dizi eklemek

Değiştirin `REPLACE_WITH_YOUR_SECURE_PASSWORD` belirteç parolanızla ve aşağıdaki komutları çalıştırın:

#### <a name="macos-linux"></a>macOS, Linux

```bash
KIALI_USERNAME=$(echo -n "kiali" | base64)
KIALI_PASSPHRASE=$(echo -n "REPLACE_WITH_YOUR_SECURE_PASSWORD" | base64)

cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE
EOF
```

#### <a name="windows"></a>Windows

```powershell
$KIALI_USERNAME=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("kiali"))
$KIALI_PASSPHRASE=[Convert]::ToBase64String([System.Text.Encoding]::UTF8.GetBytes("REPLACE_WITH_YOUR_SECURE_PASSWORD"))

"apiVersion: v1
kind: Secret
metadata:
  name: kiali
  namespace: istio-system
  labels:
    app: kiali
type: Opaque
data:
  username: $KIALI_USERNAME
  passphrase: $KIALI_PASSPHRASE" | kubectl apply -f -
```

### <a name="install-istio-components"></a>Istio bileşenlerini yükleme

Başarıyla Grafana ve Kiali gizli dizileri bizim AKS kümesinde oluşturduk, Istio bileşenleri yüklemek için zaman var. Helm kullanın ve `istio` Istio bileşenlerine yüklemek için grafik `istio-system` AKS kümenizde ad alanı. Ortamınız için uygun komutları kullanın.

> [!NOTE]
> Bizim yüklemesinin bir parçası aşağıdaki seçenekleri kullanıyoruz:
> - `global.controlPlaneSecurityEnabled=true` -Karşılıklı TLS için Denetim düzlemi etkin
> - `mixer.adapters.useAdapterCRDs=false` -kullanım dışı için kullanıcılar ve bu performansı iyileştirir gözcüler mixer bağdaştırıcısında CRD'ler kaldırın
> - `grafana.enabled=true` -Grafana dağıtım analizi için etkinleştirme ve izleme panoları
> - `grafana.security.enabled=true` -Grafana için kimlik doğrulamasını etkinleştirme
> - `tracing.enabled=true` -İzleme Jaeger dağıtımı etkinleştir
> - `kiali.enabled=true` -Hizmet kafes observability panosu için Kiali dağıtımı etkinleştir

Bash

```bash
helm install install/kubernetes/helm/istio --name istio --namespace istio-system \
  --set global.controlPlaneSecurityEnabled=true \
  --set mixer.adapters.useAdapterCRDs=false \
  --set grafana.enabled=true --set grafana.security.enabled=true \
  --set tracing.enabled=true \
  --set kiali.enabled=true
```

PowerShell

```powershell
helm install install/kubernetes/helm/istio --name istio --namespace istio-system `
  --set global.controlPlaneSecurityEnabled=true `
  --set mixer.adapters.useAdapterCRDs=false `
  --set grafana.enabled=true --set grafana.security.enabled=true `
  --set tracing.enabled=true `
  --set kiali.enabled=true
```

`istio` Helm grafiği, çok sayıda nesneleri dağıtır. Listeden çıktısını görebilirsiniz, `helm install` yukarıdaki komutu. Istio bileşenlerin dağıtımına ilişkin küme ortamınıza bağlı olarak tamamlanması 4 ila 5 dakika sürebilir.

> [!NOTE]
> Tüm Istio pod'ları, Linux düğümleri üzerinde çalışmak için zamanlanmalıdır. Kümenizde Linux düğüm havuzları yanı sıra Windows Server düğüm havuzları varsa, tüm Istio pod'ların Linux düğümleri üzerinde çalışmak üzere zamanlanmış olduğunu doğrulayın.

Bu noktada, AKS kümenizi Istio dağıttınız. Biz Istio dağıtımının başarılı olmasını sağlamak için için sonraki bölüme geçelim [Istio yüklemesini doğrulamak](#validate-the-istio-installation).

## <a name="validate-the-istio-installation"></a>Istio yüklemeyi doğrulama

İlk beklenen Hizmetleri oluşturulmuş olduğunu doğrulayın. Kullanım [kubectl alma svc] [ kubectl-get] çalışmakta olan hizmetlerin görmek için komutu. Sorgu `istio-system` ad alanı, burada Istio ve eklenti bileşeni yüklenmedi tarafından `istio` Helm grafiği:

```console
kubectl get svc --namespace istio-system --output wide
```

Aşağıdaki örnek çıktıda, artık çalıştırılması gerektiği hizmetler gösterilmektedir:

- `istio-*` Hizmetleri
- `jaeger-*`, `tracing`, ve `zipkin` eklenti izleme Hizmetleri
- `prometheus` Eklenti ölçümleri hizmeti
- `grafana` Eklenti analiz ve izleme panosu hizmeti
- `kiali` Eklenti hizmeti kafes Panosu

Varsa `istio-ingressgateway` bir dış IP gösterir `<pending>`, Azure ağı ile bir IP adresi atanmış olan kadar birkaç dakika bekleyin.

```console
NAME                     TYPE           CLUSTER-IP     EXTERNAL-IP     PORT(S)                                                                                                                                      AGE       SELECTOR
grafana                  ClusterIP      10.0.81.182    <none>          3000/TCP                                                                                                                                     119s      app=grafana
istio-citadel            ClusterIP      10.0.96.33     <none>          8060/TCP,15014/TCP                                                                                                                           119s      istio=citadel
istio-galley             ClusterIP      10.0.237.158   <none>          443/TCP,15014/TCP,9901/TCP                                                                                                                   119s      istio=galley
istio-ingressgateway     LoadBalancer   10.0.154.12    20.188.211.19   15020:30603/TCP,80:31380/TCP,443:31390/TCP,31400:31400/TCP,15029:31198/TCP,15030:30610/TCP,15031:30937/TCP,15032:31344/TCP,15443:31499/TCP   119s      app=istio-ingressgateway,istio=ingressgateway,release=istio
istio-pilot              ClusterIP      10.0.178.56    <none>          15010/TCP,15011/TCP,8080/TCP,15014/TCP                                                                                                       119s      istio=pilot
istio-policy             ClusterIP      10.0.116.118   <none>          9091/TCP,15004/TCP,15014/TCP                                                                                                                 119s      istio-mixer-type=policy,istio=mixer
istio-sidecar-injector   ClusterIP      10.0.31.160    <none>          443/TCP                                                                                                                                      119s      istio=sidecar-injector
istio-telemetry          ClusterIP      10.0.187.246   <none>          9091/TCP,15004/TCP,15014/TCP,42422/TCP                                                                                                       119s      istio-mixer-type=telemetry,istio=mixer
jaeger-agent             ClusterIP      None           <none>          5775/UDP,6831/UDP,6832/UDP                                                                                                                   119s      app=jaeger
jaeger-collector         ClusterIP      10.0.116.63    <none>          14267/TCP,14268/TCP                                                                                                                          119s      app=jaeger
jaeger-query             ClusterIP      10.0.22.108    <none>          16686/TCP                                                                                                                                    119s      app=jaeger
kiali                    ClusterIP      10.0.142.50    <none>          20001/TCP                                                                                                                                    119s      app=kiali
prometheus               ClusterIP      10.0.138.134   <none>          9090/TCP                                                                                                                                     119s      app=prometheus
tracing                  ClusterIP      10.0.165.210   <none>          80/TCP                                                                                                                                       118s      app=jaeger
zipkin                   ClusterIP      10.0.126.211   <none>          9411/TCP                                                                                                                                     118s      app=jaeger
```

Ardından, gerekli pod'ların oluşturulduğunu onaylayın. Kullanım [kubectl pod'ları alma] [ kubectl-get] komutunu ve yeniden sorgula `istio-system` ad alanı:

```console
kubectl get pods --namespace istio-system
```

Aşağıdaki örnek çıktı, çalıştırılan pod'ları gösterir:

- `istio-*` pod'ları
- `prometheus-*` eklenti ölçümleri pod
- `grafana-*` eklenti analiz ve izleme Pano podunun
- `kiali` eklenti hizmeti kafes Pano podunun

```console
NAME                                     READY     STATUS      RESTARTS   AGE
grafana-88779954d-nzpm7                  1/1       Running     0          6m26s
istio-citadel-7f699dc8c8-n7q8g           1/1       Running     0          6m26s
istio-galley-649bc8cd97-wfjzm            1/1       Running     0          6m26s
istio-ingressgateway-65dfbd566-42wkn     1/1       Running     0          6m26s
istio-init-crd-10-tmtw5                  0/1       Completed   0          20m38s
istio-init-crd-11-ql25l                  0/1       Completed   0          20m38s
istio-pilot-958dd8cc4-4ckf9              2/2       Running     0          6m26s
istio-policy-86b4b7cf9-zf7v7             2/2       Running     4          6m26s
istio-sidecar-injector-d48786c5c-pmrj9   1/1       Running     0          6m26s
istio-telemetry-7f6996fdcc-84w94         2/2       Running     3          6m26s
istio-tracing-79db5954f-h7hmz            1/1       Running     0          6m26s
kiali-5c4cdbb869-s28dv                   1/1       Running     0          6m26s
prometheus-67599bf55b-pgxd8              1/1       Running     0          6m26s
```

Bulunmamalıdır iki `istio-init-crd-*` pod'ları ile bir `Completed` durumu. Bu pod'ların CRD'ler daha önceki bir adımda oluşturulan işleri çalıştırmaktan sorumludur. Tüm diğer pod durumunu göstermesi gerekir `Running`. Bu durumlar podlarınız yoksa, bunu bir veya iki dakika bekleyin. Tüm pod'ların bir sorunu bildirmek kullanırsanız [kubectl açıklayan pod] [ kubectl-describe] , çıkış ve durumunu gözden geçirmek için komut.

## <a name="accessing-the-add-ons"></a>Eklentileri erişme

Eklenti sayısına yüklenen Istio ek işlevsellik sağlayan bizim kurulumunda yukarıdaki. Eklentiler için kullanıcı arabirimleri dış IP adresi genel olarak açık değildir. Eklenti kullanıcı arabirimleri erişmek için [kubectl bağlantı noktası-İleri] [ kubectl-port-forward] komutu. Bu komut, AKS kümenizin İstemci makinenizde ve ilgili pod arasında güvenli bir bağlantı oluşturur.

Ek bir güvenlik katmanı Grafana ve Kiali için kimlik bilgileri için bu makalenin önceki bölümlerinde belirterek ekledik.

### <a name="grafana"></a>Grafana

Analiz ve izleme panoları için Istio tarafından sağlanan [Grafana][grafana]. Yerel bağlantı noktası iletme `3000` bağlantı noktasına İstemci makinenizde `3000` Grafana AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=grafana -o jsonpath='{.items[0].metadata.name}') 3000:3000
```

Grafana için yapılandırılan bağlantı noktası-İleri aşağıdaki örnek çıktı gösterilmektedir:

```console
Forwarding from 127.0.0.1:3000 -> 3000
Forwarding from [::1]:3000 -> 3000
```

İstemci makinenizde - artık Grafana şu URL'den ulaşabilirsiniz [ http://localhost:3000 ](http://localhost:3000). Grafana daha önce istendiğinde gizli oluşturduğunuz kimlik bilgilerini kullandığınızdan emin olun.

### <a name="prometheus"></a>Prometheus

Ölçümleri Istio için sağladığı [Prometheus][prometheus]. Yerel bağlantı noktası iletme `9090` bağlantı noktasına İstemci makinenizde `9090` Prometheus AKS kümenizde çalışan bir pod üzerinde:

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

Tarafından sağlanan izleme Istio içinde [Jaeger][jaeger]. Yerel bağlantı noktası iletme `16686` bağlantı noktasına İstemci makinenizde `16686` Jaeger AKS kümenizde çalışan bir pod üzerinde:

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

Hizmet kafes observability Panosu tarafından sağlanan [Kiali][kiali]. Yerel bağlantı noktası iletme `20001` bağlantı noktasına İstemci makinenizde `20001` Kiali AKS kümenizde çalışan bir pod üzerinde:

```console
kubectl port-forward -n istio-system $(kubectl get pod -n istio-system -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```

Aşağıdaki örnek çıktıda Kiali için yapılandırılan bağlantı noktası-İleri gösterir:

```console
Forwarding from 127.0.0.1:20001 -> 20001
Forwarding from [::1]:20001 -> 20001
```

İstemci makinenizde - artık Kiali hizmet kafes observability Panosu şu URL'den ulaşabilirsiniz [ http://localhost:20001/kiali/console/ ](http://localhost:20001/kiali/console/). Kiali istendiğinde gizli oluşturduğunuz kimlik bilgilerini kullandığınızdan emin olun.

## <a name="uninstall-istio-from-aks"></a>Istio AKS kaldırın

> [!WARNING]
> Istio çalışan bir sistemden silinmesi trafiği neden olabilir ilgili sorunları, hizmetler arasında. Hükümler sisteminizin hala Istio devam etmeden önce düzgün çalışması yapılan sağlayın.

### <a name="remove-istio-components-and-namespace"></a>Istio bileşenlerini kaldır ve ad alanı

AKS kümenizi Istio kaldırmak için aşağıdaki komutları kullanın. `helm delete` Komutları kaldırılır `istio` ve `istio-init` grafikler ve `kubectl delete ns` komutu kaldırır `istio-system` ad alanı.

```azurecli
helm delete --purge istio
helm delete --purge istio-init
kubectl delete ns istio-system
```

### <a name="remove-istio-crds"></a>Istio CRD'ler Kaldır

Yukarıdaki komutlar Istio bileşenleri ve ad alanını silin, ancak biz yine de ile Istio CRD'ler bırakılır. CRD'ler silmek için bir aşağıdaki yaklaşımlardan kullanabilirsiniz.

Yaklaşım #1 - Bu komut, bu adım en üst düzey klasör Istio ile yüklemek için kullanılan Istio indirilen ve ayıklanan sürümünün çalıştığını varsayar.

```azure-cli
kubectl delete -f install/kubernetes/helm/istio-init/files
```

Yaklaşım Istio ile yüklemek için kullanılan Istio indirilen ve ayıklanan sürümünde artık erişiminiz varsa, bu komutların #2 - kullanın. Bu komut, biraz daha uzun - sürer tamamlanması birkaç dakikayı beklediğiniz.

Bash
```bash
kubectl get crds -o name | grep 'istio.io' | xargs -n1 kubectl delete
```

PowerShell
```powershell
kubectl get crds -o name | Select-String -Pattern 'istio.io' |% { kubectl delete $_ }
```

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki belgeler, vamp alma için akıllı yönlendirme sağlamak üzere Istio nasıl kullanabileceğinizi açıklar:

> [!div class="nextstepaction"]
> [AKS Istio akıllı yönlendirme senaryosu][istio-scenario-routing]

Daha fazla yükleme ve yapılandırma seçeneklerini Istio için keşfetmek için resmi Istio aşağıdaki makalelere bakın:

- [Istio - Helm Yükleme Kılavuzu][istio-install-helm]
- [Istio - Helm yükleme seçenekleri][istio-install-helm-options]

Ayrıca ek senaryoları kullanarak izleyebileceğiniz [Istio Bookinfo uygulama örneği][istio-bookinfo-example].

Application Insights ile Istio AKS uygulamanızı izlemek öğrenmek için aşağıdaki Azure İzleyici belgelere bakın:
- [Barındırılan uygulamalarını sıfır izleme uygulama için Kubernetes izleme][app-insights]

<!-- LINKS - external -->
[istio]: https://istio.io
[helm]: https://helm.sh

[istio-docs-concepts]: https://istio.io/docs/concepts/what-is-istio/
[istio-github]: https://github.com/istio/istio
[istio-github-releases]: https://github.com/istio/istio/releases
[istio-release-notes]: https://istio.io/about/notes/
[istio-install-download]: https://istio.io/docs/setup/kubernetes/download-release/
[istio-install-helm]: https://istio.io/docs/setup/kubernetes/install/helm/
[istio-install-helm-options]: https://istio.io/docs/reference/config/installation-options/
[istio-bookinfo-example]: https://istio.io/docs/examples/bookinfo/
[install-wsl]: https://docs.microsoft.com/windows/wsl/install-win10

[kubernetes-crd]: https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions
[kubernetes-jobs]: https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/
[kubernetes-secrets]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-port-forward]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#port-forward

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/
[jaeger]: https://www.jaegertracing.io/
[kiali]: https://www.kiali.io/

[app-insights]: https://docs.microsoft.com/azure/azure-monitor/app/kubernetes

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[istio-scenario-routing]: ./istio-scenario-routing.md
[helm-install]: ./kubernetes-helm.md