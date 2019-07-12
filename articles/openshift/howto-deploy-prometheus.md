---
title: Azure Red Hat OpenShift kümesinde tek başına bir Prometheus örneğini dağıtma | Microsoft Docs
description: Uygulamanızın ölçümleri izlemek için bir Azure Red Hat OpenShift kümesinde Prometheus örneği oluşturun.
author: makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus aro, openshift, ölçümleri, red hat
ms.openlocfilehash: a9748932a72106413677b21fe0efd1f69fb02e47
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827009"
---
# <a name="deploy-a-standalone-prometheus-instance-in-an-azure-red-hat-openshift-cluster"></a>Azure Red Hat OpenShift kümesinde tek başına bir Prometheus örneğini dağıtma

Bu makalede bir Azure Red Hat OpenShift kümesinde hizmet bulma kullanan bir tek başına Prometheus örneği yapılandırma açıklanır.

> [!NOTE]
> Azure Red Hat OpenShift küme için müşteri yönetici erişimi gerekli değildir.

Hedef Kurulum:

- Prometheus ve Alertmanager içeren bir proje (prometheus-Proje).
- Uygulamaları izlemek için içeren iki proje (uygulama project1 ve uygulama project2).

Bazı Prometheus yapılandırma dosyaları yerel olarak hazırlarsınız. Bunları depolamak için yeni bir klasör oluşturun. Gizli dizi belirteçleri daha sonra kümeye eklenen durumunda yapılandırma dosyaları gizli dizi olarak kümede depolanır.

## <a name="sign-in-to-the-cluster-by-using-the-oc-tool"></a>Kümeye OC aracını kullanarak oturum açın

1. Bir web tarayıcısı açın ve sonra kümenizi web konsoluna gidin (https://openshift. *rastgele bir kimlik*. *Bölge*. azmosa.io).
2. Azure kimlik bilgilerinizle oturum açın.
3. Sağ üst köşeden kullanıcı adınızı seçin ve ardından **kopyalama oturum açma komutunu**.
4. Kullanıcı adınızı kullanacağınız terminale yapıştırabilirsiniz.

> [!NOTE]
> Doğru kümeye oturum açmadıysanız, görmek için şunu çalıştırın `oc whoami -c` komutu.

## <a name="prepare-the-projects"></a>Projeleri hazırlama

Projeleri oluşturmak için aşağıdaki komutları çalıştırın:
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Kullanabilir `-n` veya `--namespace` çalıştırarak etkin bir proje seçin veya parametre `oc project` komutu.

## <a name="prepare-the-prometheus-configuration-file"></a>Prometheus yapılandırma dosyasını hazırlama
Aşağıdaki içerik girerek bir prometheus.yml dosyası oluşturun:
```
global:
  scrape_interval: 30s
  evaluation_interval: 5s

scrape_configs:
    - job_name: prom-sd
      scrape_interval: 30s
      scrape_timeout: 10s
      metrics_path: /metrics
      scheme: http
      kubernetes_sd_configs:
      - api_server: null
        role: endpoints
        namespaces:
          names:
          - prometheus-project
          - app-project1
          - app-project2
```
Aşağıdaki yapılandırmayı girerek PROM adlı bir gizli dizi oluşturun:
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Bir temel Prometheus yapılandırma dosyası prometheus.yml dosyasıdır. Aralıkları ayarlar ve Otomatik bulma üç projenin (prometheus proje, uygulama project1, uygulama project2) yapılandırır. Önceki yapılandırma dosyasında, otomatik olarak keşfedilebilen uç kimlik doğrulaması olmadan HTTP üzerinden scraped.

Uç noktaları değiştirilene hakkında daha fazla bilgi için bkz. [Prometheus çıkış config](https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config).


## <a name="prepare-the-alertmanager-config-file"></a>Alertmanager yapılandırma dosyasını hazırlama
Aşağıdaki içerik girerek bir alertmanager.yml dosyası oluşturun:
```
global:
  resolve_timeout: 5m
route:
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 12h
  receiver: default
  routes:
  - match:
      alertname: DeadMansSwitch
    repeat_interval: 5m
    receiver: deadmansswitch
receivers:
- name: default
- name: deadmansswitch
```
Aşağıdaki yapılandırmayı girerek PROM uyarıları adlı bir gizli dizi oluşturun:
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Alertmanager.yml uyarı Yöneticisi yapılandırma dosyasıdır.

> [!NOTE]
> Önceki iki adımı doğrulamak için çalıştırın `oc get secret -n prometheus-project` komutu.

## <a name="start-prometheus-and-alertmanager"></a>Prometheus ve Alertmanager Başlat
Git [openshift/başlangıç deposunun](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) ve indirme [prometheus standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) şablonu. Şablonu aşağıdaki yapılandırma girerek prometheus-proje için geçerlidir:
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
OpenShift şablonu prometheus standalone.yaml dosyasıdır. Oauth yetkili ve oauth proxy ile güvenliği de Alertmanager örneğini önündeki ile Prometheus örneği oluşturun. Bu şablonda oauth proxy "prometheus proje ad alanını alabilirsiniz" herhangi bir kullanıcı izin verecek şekilde yapılandırılır (bkz `-openshift-sar` bayrağı).

> [!NOTE]
> ' % S'prom StatefulSet eşit İSTENEN ve çoğaltmaları numarası geçerli olup olmadığını doğrulamak için çalıştırın `oc get statefulset -n prometheus-project` komutu. Projedeki tüm kaynakları denetlemek için çalıştırın `oc get all -n prometheus-project` komutu.

## <a name="add-permissions-to-allow-service-discovery"></a>Hizmet bulma izin vermek için izinleri ekleme

Aşağıdaki içerik girerek bir prometheus sdrole.yml dosyası oluşturun:
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus service discovery role
    description: |
      Role and rolebinding added permissions required for service discovery in a given project.
    iconClass: fa fa-cogs
    tags: "monitoring,prometheus,alertmanager,time-series"
parameters:
- description: The project name, where a standalone Prometheus is deployed
  name: PROMETHEUS_PROJECT
  value: prometheus-project
objects:
- apiVersion: rbac.authorization.k8s.io/v1
  kind: Role
  metadata:
    name: prometheus-sd
  rules:
  - apiGroups:
    - ""
    resources:
    - services
    - endpoints
    - pods
    verbs:
    - list
    - get
    - watch
- apiVersion: rbac.authorization.k8s.io/v1
  kind: RoleBinding
  metadata:
    name: prometheus-sd
  roleRef:
    apiGroup: rbac.authorization.k8s.io
    kind: Role
    name: prometheus-sd
  subjects:
  - kind: ServiceAccount
    name: prom
    namespace: ${PROMETHEUS_PROJECT}
```
Hizmet bulma izin vermek istediğiniz tüm projeler için şablonu uygulamak için aşağıdaki komutları çalıştırın:
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Kendisinden ölçümleri toplamak için Prometheus sahip izinleri prometheus projesindeki uygulanır.

> [!NOTE]
> Rol ve RoleBinding doğru oluşturulduğunu doğrulamak için çalıştırın `oc get role` ve `oc get rolebinding` komutları.

## <a name="optional-deploy-example-application"></a>İsteğe bağlı: Örnek uygulamayı dağıtma

Her şeyin çalıştığından, ancak ölçüm kaynağı yok. Prometheus URL'ye gidin (https://prom-prometheus-project.apps. *rastgele bir kimlik*. *Bölge*.azmosa.io/). Aşağıdaki komutu kullanarak bulabilirsiniz:

```
oc get route prom -n prometheus-project
```
> [!IMPORTANT]
> Ana bilgisayar adı başlangıcına https:// ön ekini eklemeyi unutmayın.

**Durumu > hizmet bulma** sayfası 0/0 etkin hedefleri gösterir.

Temel bir Python ölçümleri /metrics uç nokta altında sunan bir örnek uygulamayı dağıtmak için aşağıdaki komutları çalıştırın:
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1

oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Yeni uygulamalar, dağıtımdan sonra 30 saniye içinde hizmet bulma sayfasında geçerli hedefleri olarak görünmelidir.

Daha fazla ayrıntı için seçin **durumu** > **hedefleri**.

> [!NOTE]
> Başarıyla scraped her hedef için bir veri noktasının Prometheus yukarı ölçümü ekler. Seçin **Prometheus** girin sol üst köşedeki **yukarı** ifade edin ve ardından **yürütme**.

## <a name="next-steps"></a>Sonraki adımlar

Özel Prometheus izleme uygulamalarınıza ekleyebilirsiniz. Prometheus ölçümleri hazırlık basitleştiren Prometheus istemci kitaplığı, farklı programlama dili için hazırdır.

Daha fazla bilgi için aşağıdaki GitHub kitaplıkları bakın:

 - [Java](https://github.com/prometheus/client_java)
 - [Python](https://github.com/prometheus/client_python)
 - [Go](https://github.com/prometheus/client_golang)
 - [Ruby](https://github.com/prometheus/client_ruby)
