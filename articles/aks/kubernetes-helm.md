---
title: Azure üzerinde Kubernetes Helm ile kapsayıcıları dağıtın
description: Kapsayıcıları AKS Kubernetes kümesinde dağıtmak için Helm paketleme Aracı'nı kullanın
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 06/13/2018
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: 531e6d9368b2bf91c48fd41b1e9330879b0df49a
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37102663"
---
# <a name="use-helm-with-azure-kubernetes-service-aks"></a>Helm Azure Kubernetes hizmeti (AKS) kullanın

[Helm] [ helm] yükleme ve kullanım ömrü boyunca Kubernetes uygulamaları yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Linux paketi yöneticileri gibi benzer *APT* ve *Yum*, Helm önceden yapılandırılmış Kubernetes kaynakların paketleri Kubernetes grafikler, yönetmek için kullanılır.

Yapılandırma ve Helm AKS Kubernetes kümede kullanılarak aracılığıyla bu belge adımlar.

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğeler gereksinim duyarsanız, bkz: [AKS quickstart][aks-quickstart].

## <a name="install-helm-cli"></a>Helm CLI yükleme

Helm CLI geliştirme sisteminizde çalıştıran ve başlatmak, durdurmak ve Helm uygulamalarla yönetmenize olanak sağlayan bir istemci olur.

Azure CloudShell kullanıyorsanız, Helm CLI zaten yüklü. Bir Mac üzerinde Helm CLI yüklemek için `brew`. İçin ek yükleme seçenekleri bkz [yükleme Helm][helm-install-options].

```console
brew install kubernetes-helm
```

Çıktı:

```
==> Downloading https://homebrew.bintray.com/bottles/kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
######################################################################## 100.0%
==> Pouring kubernetes-helm-2.6.2.sierra.bottle.1.tar.gz
==> Caveats
Bash completion has been installed to:
  /usr/local/etc/bash_completion.d
==> Summary
🍺  /usr/local/Cellar/kubernetes-helm/2.6.2: 50 files, 132.4MB
```

## <a name="create-service-account"></a>Hizmet hesabı oluşturma

Bir RBAC Helm yapılandırma küme etkinleştirilmeden önce bir hizmet hesabı ve rol Tiller hizmeti için bağlama gerekir. Helm güvenliğini sağlama konusunda daha fazla bilgi için / bir RBAC Tiller etkin küme bkz [Tiller, ad alanları ve RBAC][tiller-rbac]. Not kümenizi RBAC değil, etkin, bu adımı atlayın.

Adlı bir dosya oluşturun `helm-rbac.yaml` ve aşağıdaki YAML kopyalayın.

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1beta1
kind: ClusterRoleBinding
metadata:
  name: tiller
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: tiller
    namespace: kube-system
```

Hizmet hesabı oluşturup rol bağlama ile `kubectl create` komutu.

```
kubectl create -f helm-rbac.yaml
```

Bir RBAC kullanarak küme etkinleştirildiğinde, Tiller kümeye sahip olduğu erişim düzeyine seçeneğiniz vardır. Bkz: [Helm: rol tabanlı erişim denetimlerini] [ helm-rbac] yapılandırma seçenekleri hakkında daha fazla bilgi için.

## <a name="configure-helm"></a>Helm yapılandırın

Şimdi tiller kullanarak yüklemek [helm init] [ helm-init] komutu. Kümenizi RBAC etkin değilse, kaldırma `--service-account` bağımsız değişkeni ve değeri.

```
helm init --service-account tiller
```

## <a name="find-helm-charts"></a>Helm grafikleri Bul

Helm grafikler Kubernetes kümesine uygulamaları dağıtmak için kullanılır. Önceden oluşturulmuş Helm grafiklerde aramak için kullanın [helm arama] [ helm-search] komutu.

```azurecli-interactive
helm search
```

Ancak, çoğu ile daha fazla grafikleri aşağıdakine benzer çıkış görünüyor.

```
NAME                            VERSION DESCRIPTION
stable/acs-engine-autoscaler    2.0.0   Scales worker nodes within agent pools
stable/artifactory              6.1.0   Universal Repository Manager supporting all maj...
stable/aws-cluster-autoscaler   0.3.1   Scales worker nodes within autoscaling groups.
stable/buildkite                0.2.0   Agent for Buildkite
stable/centrifugo               2.0.0   Centrifugo is a real-time messaging server.
stable/chaoskube                0.5.0   Chaoskube periodically kills random pods in you...
stable/chronograf               0.3.0   Open-source web application written in Go and R...
stable/cluster-autoscaler       0.2.0   Scales worker nodes within autoscaling groups.
stable/cockroachdb              0.5.0   CockroachDB is a scalable, survivable, strongly...
stable/concourse                0.7.0   Concourse is a simple and scalable CI system.
stable/consul                   0.4.1   Highly available and distributed service discov...
stable/coredns                  0.5.0   CoreDNS is a DNS server that chains middleware ...
stable/coscale                  0.2.0   CoScale Agent
stable/dask-distributed         2.0.0   Distributed computation in Python
stable/datadog                  0.8.0   DataDog Agent
...
```

Grafikler listesini güncelleştirmek için kullanmak [helm deposuna güncelleştirme] [ helm-repo-update] komutu.

```azurecli-interactive
helm repo update
```

Çıktı:

```
Hang tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Çalışma Helm grafikleri

Wordpress Helm grafiğini kullanarak dağıtmak için kullandığınız [helm yükleme] [ helm-install] komutu.

```azurecli-interactive
helm install stable/wordpress
```

Çıktı aşağıdakine benzer, ancak Kubernetes dağıtım kullanma hakkında yönergeler gibi ek bilgileri içerir.

```
NAME:   bilging-ibex
LAST DEPLOYED: Tue Jun  5 14:31:49 2018
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/Pod(related)
NAME                                     READY  STATUS   RESTARTS  AGE
bilging-ibex-mariadb-7557b5474-dmdxn     0/1    Pending  0         1s
bilging-ibex-wordpress-7494c545fb-tskhz  0/1    Pending  0         1s

==> v1/Secret
NAME                    TYPE    DATA  AGE
bilging-ibex-mariadb    Opaque  2     1s
bilging-ibex-wordpress  Opaque  2     1s

==> v1/ConfigMap
NAME                        DATA  AGE
bilging-ibex-mariadb        1     1s
bilging-ibex-mariadb-tests  1     1s

==> v1/PersistentVolumeClaim
NAME                    STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
bilging-ibex-mariadb    Pending  default  1s
bilging-ibex-wordpress  Pending  default  1s

==> v1/Service
NAME                    TYPE          CLUSTER-IP    EXTERNAL-IP  PORT(S)                     AGE
bilging-ibex-mariadb    ClusterIP     10.0.76.164   <none>       3306/TCP                    1s
bilging-ibex-wordpress  LoadBalancer  10.0.215.250  <pending>    80:30934/TCP,443:31134/TCP  1s

==> v1beta1/Deployment
NAME                    DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
bilging-ibex-mariadb    1        1        1           0          1s
bilging-ibex-wordpress  1        1        1           0          1s
...
```

## <a name="list-helm-releases"></a>Liste Helm serbest bırakır

Kümenizde yüklü sürümlerden listesini görmek için [helm listesi] [ helm-list] komutu.

```azurecli-interactive
helm list
```

Çıktı:

```
NAME            REVISION    UPDATED                     STATUS      CHART           NAMESPACE
bilging-ibex    1           Tue Jun  5 14:31:49 2018    DEPLOYED    wordpress-1.0.9 default
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes grafikleri yönetme hakkında daha fazla bilgi için Helm belgelerine bakın.

> [!div class="nextstepaction"]
> [Helm belgeleri][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/helm/#helm-install
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
