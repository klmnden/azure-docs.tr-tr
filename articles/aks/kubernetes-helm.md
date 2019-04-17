---
title: Azure'da kubernetes Helm ile kapsayıcıları dağıtın
description: Azure Kubernetes Service (AKS) kümesini kapsayıcıları dağıtmak için Helm paketleme Aracı'nı kullanmayı öğrenin
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 03/06/2019
ms.author: zarhoads
ms.openlocfilehash: 2fcdb72fa2717659e78e6f767bdc73b0d7be0886
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59618116"
---
# <a name="install-applications-with-helm-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) Helm ile uygulamaları yükleme

[Helm] [ helm] yükleyin ve Kubernetes uygulamaların yaşam döngüsünü yönetmenize yardımcı olan bir açık kaynak paketleme aracıdır. Linux paket yöneticileri gibi benzer *APT* ve *Yum*, Helm Kubernetes grafikleri, önceden yapılandırılmış Kubernetes kaynak paketleri yönetmek için kullanılır.

Bu makalede yapılandırma ve bir Kubernetes kümesinde AKS üzerinde Helm kullanma gösterilmektedir.

## <a name="before-you-begin"></a>Başlamadan önce

Bu makalede, var olan bir AKS kümesi olduğunu varsayar. AKS hızlı bir AKS kümesi gerekirse bkz [Azure CLI kullanarak] [ aks-quickstart-cli] veya [Azure portalını kullanarak][aks-quickstart-portal].

Helm yüklü CLI geliştirme sisteminizde çalışan ve başlatma, durdurma ve Helm ile uygulamaları yönetmenize olanak tanır istemci de gerekir. Azure Cloud Shell'i kullanırsanız, Helm CLI zaten yüklüdür. Yükleme yönergeleri, yerel platformunda görmek için [yükleme Helm][helm-install].

## <a name="create-a-service-account"></a>Bir hizmet hesabı oluşturun

Bir hizmet hesabı ve rol bağlama bir RBAC özellikli AKS kümesinde Helm dağıtabilmeniz için önce Tiller hizmeti için gereklidir. Helm güvenliğini sağlama konusunda daha fazla bilgi için / Tiller bir RBAC, etkin küme, bkz: [Tiller, ad alanları ve RBAC][tiller-rbac]. AKS kümenizi RBAC etkin değilse, bu adımı atlayın.

Adlı bir dosya oluşturun `helm-rbac.yaml` aşağıdaki YAML'ye kopyalayın:

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: tiller
  namespace: kube-system
---
apiVersion: rbac.authorization.k8s.io/v1
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

Hizmet hesabı oluşturup rolü bağlamayla `kubectl apply` komutu:

```console
kubectl apply -f helm-rbac.yaml
```

## <a name="secure-tiller-and-helm"></a>Güvenli Tiller ve Helm

Tiller hizmet ve Helm istemci kimliğini doğrulamak ve TLS/SSL aracılığıyla birbirleriyle iletişim kurar. Kubernetes kümenizin güvenliğini sağlamak için bu kimlik doğrulama yöntemini yardımcı olur ve hangi Hizmetleri dağıtılabilir. Güvenliği artırmak için kendi imzalanan sertifikalar oluşturabilir. Her bir Helm kullanıcı kendi istemci sertifikası alırsınız ve uygulanan sertifikalarla Tiller Kubernetes kümesinde başlatılması. Daha fazla bilgi için [TLS/SSL kullanarak Helm Tiller arasındaki][helm-ssl].

Bir RBAC özellikli Kubernetes kümesiyle Tiller kümeye sahip erişim düzeyini denetleyebilirsiniz. Tiller dağıtıldığı Kubernetes ad alanı tanımlayabilir ve kısıtlama ad uzaylarını Tiller ardından kaynakları dağıtabilirsiniz. Bu yaklaşım, farklı bir ad alanları ve sınırı dağıtım sınırları Tiller örnekleri oluşturma ve kullanıcıları belirli ad alanlarına Helm istemci kapsamını sağlar. Daha fazla bilgi için [Helm rol tabanlı erişim denetimlerini][helm-rbac].

## <a name="configure-helm"></a>Helm yapılandırın

Temel Tiller bir AKS kümesi dağıtmayı kullanın [helm init] [ helm-init] komutu. Kümenizi RBAC etkin değilse, kaldırma `--service-account` bağımsız değişkeni ve değer. TLS/SSL Tiller ve Helm için yapılandırdıysanız, bu temel başlatma adımı atlayın ve bunun yerine gerekli sağlamak `--tiller-tls-` sonraki örnekte gösterildiği gibi.

```console
helm init --service-account tiller
```

TLS/SSL Helm Tiller arasındaki yapılandırılıp yapılandırılmadığını sağlamak `--tiller-tls-*` parametreleri ve aşağıdaki örnekte gösterildiği gibi kendi sertifikalarınızı adları:

```console
helm init \
    --tiller-tls \
    --tiller-tls-cert tiller.cert.pem \
    --tiller-tls-key tiller.key.pem \
    --tiller-tls-verify \
    --tls-ca-cert ca.cert.pem \
    --service-account tiller
```

## <a name="find-helm-charts"></a>Helm grafikleri Bul

Helm grafikleri, uygulamalarınızı bir Kubernetes kümesi dağıtmak için kullanılır. Önceden oluşturulmuş Helm grafikleri için aranacak kullanın [helm search] [ helm-search] komutu:

```console
helm search
```

Aşağıdaki sıkıştırılmış örneğe çıktı Helm grafikleri kullanılabilir bazıları gösterilmektedir:

```
$ helm search

NAME                           CHART VERSION    APP VERSION  DESCRIPTION
stable/aerospike               0.1.7            v3.14.1.2    A Helm chart for Aerospike in Kubernetes
stable/anchore-engine          0.1.7            0.1.10       Anchore container analysis and policy evaluatio...
stable/apm-server              0.1.0            6.2.4        The server receives data from the Elastic APM a...
stable/ark                     1.0.1            0.8.2        A Helm chart for ark
stable/artifactory             7.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/artifactory-ha          0.2.1            6.0.0        Universal Repository Manager supporting all maj...
stable/auditbeat               0.1.0            6.2.4        A lightweight shipper to audit the activities o...
stable/aws-cluster-autoscaler  0.3.3                         Scales worker nodes within autoscaling groups.
stable/bitcoind                0.1.3            0.15.1       Bitcoin is an innovative payment network and a ...
stable/buildkite               0.2.3            3            Agent for Buildkite
stable/burrow                  0.4.4            0.17.1       Burrow is a permissionable smart contract machine
stable/centrifugo              2.0.1            1.7.3        Centrifugo is a real-time messaging server.
stable/cerebro                 0.1.0            0.7.3        A Helm chart for Cerebro - a web admin tool tha...
stable/cert-manager            v0.3.3           v0.3.1       A Helm chart for cert-manager
stable/chaoskube               0.7.0            0.8.0        Chaoskube periodically kills random pods in you...
stable/chartmuseum             1.5.0            0.7.0        Helm Chart Repository with support for Amazon S...
stable/chronograf              0.4.5            1.3          Open-source web application written in Go and R...
stable/cluster-autoscaler      0.6.4            1.2.2        Scales worker nodes within autoscaling groups.
stable/cockroachdb             1.1.1            2.0.0        CockroachDB is a scalable, survivable, strongly...
stable/concourse               1.10.1           3.14.1       Concourse is a simple and scalable CI system.
stable/consul                  3.2.0            1.0.0        Highly available and distributed service discov...
stable/coredns                 0.9.0            1.0.6        CoreDNS is a DNS server that chains plugins and...
stable/coscale                 0.2.1            3.9.1        CoScale Agent
stable/dask                    1.0.4            0.17.4       Distributed computation in Python with task sch...
stable/dask-distributed        2.0.2                         DEPRECATED: Distributed computation in Python
stable/datadog                 0.18.0           6.3.0        DataDog Agent
...
```

Grafikler listesini güncelleştirmek için [helm deposu güncelleştirme] [ helm-repo-update] komutu. Aşağıdaki örnek, bir başarılı depo güncelleştirme gösterir:

```console
$ helm repo update

Hold tight while we grab the latest from your chart repositories...
...Skip local chart repository
...Successfully got an update from the "stable" chart repository
Update Complete. ⎈ Happy Helming!⎈
```

## <a name="run-helm-charts"></a>Helm grafiklerini Çalıştır

İle Helm grafikleri yüklemek için kullanın [helm yükleme] [ helm-install] komut ve yüklemek için grafik adını belirtin. Bu uygulamada görmek için bir Helm grafiği kullanarak basit bir Wordpress dağıtımı şimdi yükleyin. TLS/SSL yapılandırılmış eklemeniz `--tls` Helm istemci sertifikanızın kullanılacak parametre.

```console
helm install stable/wordpress
```

Aşağıdaki sıkıştırılmış örneğe çıktı Helm grafiği tarafından oluşturulan Kubernetes kaynakları dağıtım durumunu gösterir:

```
$ helm install stable/wordpress

NAME:   wishful-mastiff
LAST DEPLOYED: Wed Mar  6 19:11:38 2019
NAMESPACE: default
STATUS: DEPLOYED

RESOURCES:
==> v1beta1/Deployment
NAME                       DESIRED  CURRENT  UP-TO-DATE  AVAILABLE  AGE
wishful-mastiff-wordpress  1        1        1           0          1s

==> v1beta1/StatefulSet
NAME                     DESIRED  CURRENT  AGE
wishful-mastiff-mariadb  1        1        1s

==> v1/Pod(related)
NAME                                        READY  STATUS   RESTARTS  AGE
wishful-mastiff-wordpress-6f96f8fdf9-q84sz  0/1    Pending  0         1s
wishful-mastiff-mariadb-0                   0/1    Pending  0         1s

==> v1/Secret
NAME                       TYPE    DATA  AGE
wishful-mastiff-mariadb    Opaque  2     2s
wishful-mastiff-wordpress  Opaque  2     2s

==> v1/ConfigMap
NAME                           DATA  AGE
wishful-mastiff-mariadb        1     2s
wishful-mastiff-mariadb-tests  1     2s

==> v1/PersistentVolumeClaim
NAME                       STATUS   VOLUME   CAPACITY  ACCESS MODES  STORAGECLASS  AGE
wishful-mastiff-wordpress  Pending  default  2s

==> v1/Service
NAME                       TYPE          CLUSTER-IP   EXTERNAL-IP  PORT(S)                     AGE
wishful-mastiff-mariadb    ClusterIP     10.1.116.54  <none>       3306/TCP                    2s
wishful-mastiff-wordpress  LoadBalancer  10.1.217.64  <pending>    80:31751/TCP,443:31264/TCP  2s
...
```

Bir veya iki için dakika sürdüğünü *EXTERNAL-IP* adresi Wordpress hizmetin doldurulması ve bir web tarayıcısına erişmek izin verir.

## <a name="list-helm-releases"></a>Helm sürümler listesi

Kümenizde yüklenmiş sürümlerin listesini görmek için [helm list komutu] [ helm-list] komutu. Aşağıdaki örnek, önceki adımda dağıtılan Wordpress yayın gösterir. TLS/SSL yapılandırılmış eklemeniz `--tls` Helm istemci sertifikanızın kullanılacak parametre.

```console
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART            APP VERSION    NAMESPACE
wishful-mastiff   1         Wed Mar  6 19:11:38 2019    DEPLOYED    wordpress-2.1.3  4.9.7          default
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kubernetes kaynak sayısı, bir Helm grafiği dağıttığınızda oluşturulur. Bu kaynaklar, pod'ları, dağıtımlar ve hizmetleri içerir. Bu kaynakları temizlemek için kullanın `helm delete` bulunan önceki sürüm adınızı belirtin ve komutu `helm list` komutu. Aşağıdaki örnekte adlı yayın siler *wishful mastiff*:

```console
$ helm delete wishful-mastiff

release "wishful-mastiff" deleted
```

## <a name="next-steps"></a>Sonraki adımlar

Helm ile Kubernetes uygulama dağıtımlarını yönetme hakkında daha fazla bilgi için Helm belgelerine bakın.

> [!div class="nextstepaction"]
> [Helm belgeleri][helm-documentation]

<!-- LINKS - external -->
[helm]: https://github.com/kubernetes/helm/
[helm-documentation]: https://docs.helm.sh/
[helm-init]: https://docs.helm.sh/helm/#helm-init
[helm-install]: https://docs.helm.sh/using_helm/#installing-helm
[helm-install-options]: https://github.com/kubernetes/helm/blob/master/docs/install.md
[helm-list]: https://docs.helm.sh/helm/#helm-list
[helm-rbac]: https://docs.helm.sh/using_helm/#role-based-access-control
[helm-repo-update]: https://docs.helm.sh/helm/#helm-repo-update
[helm-search]: https://docs.helm.sh/helm/#helm-search
[tiller-rbac]: https://docs.helm.sh/using_helm/#tiller-namespaces-and-rbac
[helm-ssl]: https://docs.helm.sh/using_helm/#using-ssl-between-helm-and-tiller

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
