---
title: "Azure üzerinde Kubernetes Helm ile kapsayıcıları dağıtın"
description: "Kapsayıcıları AKS Kubernetes kümesinde dağıtmak için Helm paketleme Aracı'nı kullanın"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 002c4376c91f4a7176cc9b00a4d6ba275f87dadb
ms.sourcegitcommit: 094061b19b0a707eace42ae47f39d7a666364d58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2017
---
# <a name="use-helm-with-azure-container-service-aks"></a>Azure kapsayıcı hizmeti (AKS) Helm kullanın

[Helm](https://github.com/kubernetes/helm/) yükleyip Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Linux paketi yöneticileri gibi benzer *APT* ve *Yum*, Helm önceden yapılandırılmış Kubernetes kaynakların paketleri Kubernetes grafikler, yönetmek için kullanılır.

Yapılandırma ve Helm AKS Kubernetes kümede kullanılarak aracılığıyla bu belge adımlar.

## <a name="before-you-begin"></a>Başlamadan önce

Bu belgedeki adımlarda bir AKS kümesi oluşturduğunuz ve kümeyle bir kubectl bağlantısı kurduğunuz kabul edilmektedir. Bu öğelere gereksiniminiz varsa bkz. [AKS hızlı başlangıç](./kubernetes-walkthrough.md).

## <a name="install-helm-cli"></a>Helm CLI yükleme

Helm CLI geliştirme sisteminizde çalıştıran ve başlatma, durdurma ve Helm grafiklerle uygulamaları yönetmenize olanak sağlayan bir istemci olur.

Azure CloudShell kullanıyorsanız, Helm CLI zaten yüklü. Bir Mac üzerinde Helm CLI yüklemek için `brew`. İçin ek yükleme seçenekleri bkz [yükleme Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md).

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

## <a name="configure-helm"></a>Helm yapılandırın

[Helm init](https://docs.helm.sh/helm/#helm-init) komutu Kubernetes kümede Helm bileşenlerini yükleyin ve istemci tarafı yapılandırmaları yapmak için kullanılır. Helm AKS kümenizde yükleyip Helm istemcisini yapılandırmak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
helm init
```

Çıktı:

```
$HELM_HOME has been configured at /Users/user/.helm.
Not installing Tiller due to 'client-only' flag having been set
Happy Helming!
```

## <a name="find-helm-charts"></a>Helm grafikleri Bul

Helm grafikler Kubernetes kümesine uygulamaları dağıtmak için kullanılır. Önceden oluşturulmuş Helm grafiklerde aramak için kullanın [helm arama](https://docs.helm.sh/helm/#helm-search) komutu.

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

Grafikler listesini güncelleştirmek için kullanmak [helm deposuna güncelleştirme](https://docs.helm.sh/helm/#helm-repo-update) komutu.

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

Bir NGINX giriş denetleyicisi dağıtmak için kullandığınız [helm yükleme](https://docs.helm.sh/helm/#helm-install) komutu.

```azurecli-interactive
helm install stable/nginx-ingress
```

Çıktı aşağıdakine benzer, ancak Kubernetes dağıtım kullanma hakkında yönergeler gibi ek bilgileri içerir.

```
NAME:   tufted-ocelot
LAST DEPLOYED: Thu Oct  5 00:48:04 2017
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1/ConfigMap
NAME                                    DATA  AGE
tufted-ocelot-nginx-ingress-controller  1     5s

==> v1/Service
NAME                                         CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
tufted-ocelot-nginx-ingress-controller       10.0.140.10  <pending>    80:30486/TCP,443:31358/TCP  5s
tufted-ocelot-nginx-ingress-default-backend  10.0.34.132  <none>       80/TCP                      5s

==> v1beta1/Deployment
NAME                                         DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
tufted-ocelot-nginx-ingress-controller       1        1        1           0          5s
tufted-ocelot-nginx-ingress-default-backend  1        1        1           1          5s
...
```

NGINX giriş denetleyicisi Kubernetes ile kullanma hakkında daha fazla bilgi için bkz: [NGINX giriş denetleyicisi](https://github.com/kubernetes/ingress/tree/master/controllers/nginx).

## <a name="list-helm-charts"></a>Liste Helm grafikleri

Grafikler, kümeye yüklü listesini görmek için [helm listesi](https://docs.helm.sh/helm/#helm-list) komutu.

```azurecli-interactive
helm list
```

Çıktı:

```
NAME            REVISION    UPDATED                     STATUS      CHART               NAMESPACE
bilging-ant     1           Thu Oct  5 00:11:11 2017    DEPLOYED    nginx-ingress-0.8.7 default
```

## <a name="next-steps"></a>Sonraki adımlar

Kubernetes grafikleri yönetme hakkında daha fazla bilgi için Helm belgelerine bakın.

> [!div class="nextstepaction"]
> [Helm belgeleri](https://github.com/kubernetes/helm/blob/master/docs/index.md)