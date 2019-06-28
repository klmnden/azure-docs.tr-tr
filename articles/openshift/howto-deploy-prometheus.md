---
title: Azure Red Hat OpenShift kümesinde tek başına Prometheus dağıtma | Microsoft Docs
description: Uygulamanızın ölçümleri izlemek için bir Azure Red Hat OpenShift kümesinde Prometheus örneği oluşturma işlemini aşağıda verilmiştir.
author: Makdaam
ms.author: b-lejaku
ms.service: container-service
ms.topic: conceptual
ms.date: 06/17/2019
keywords: prometheus aro, openshift, ölçümleri, red hat
ms.openlocfilehash: e66658151361edd43f61d398274c88c047928028
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67342864"
---
# <a name="deploy-a-standalone-prometheus-in-an-azure-red-hat-openshift-cluster"></a>Tek başına Prometheus Azure Red Hat OpenShift kümesinde dağıtma

Bu kılavuzda bir Azure Red Hat OpenShift kümesindeki hizmet bulma ile tek başına Prometheus yapılandırma anlatmaktadır.  "Müşteri Yönetici" erişimi kümeye gerekli değildir.

Hedef Kurulumu aşağıdaki gibidir:

- Prometheus ve Alertmanager içeren bir proje (prometheus-Proje)
- uygulamaları izlemek için içeren iki projeleri (uygulama project1 ve uygulama project2)

Bazı Prometheus yapılandırma dosyalarını yerel olarak hazırlarsınız. Bunları depolamak için yeni bir klasör oluşturun.
Gizli dizi belirteçleri bunlara daha sonra eklenen durumunda bu yapılandırma dosyalarına gizli dizi olarak kümede depolanır.

## <a name="step-1-sign-in-to-the-cluster-using-the-oc-tool"></a>1\. adım: Oturum kullanarak kümeye `oc` aracı
Bir web tarayıcısı kullanarak kümenize web konsoluna gidin (https://openshift. *rastgele bir kimlik*. *Bölge*. azmosa.io).
Azure kimlik bilgilerinizle oturum açın.
Sağ üst kullanıcı adınıza tıklayın ve "Oturum açma komutunu Kopyala"'i seçin. Kullanacağınız terminale yapıştırın.

Şu cmdlet'le kümeye doğru oturum açmadıysanız, doğrulayabilirsiniz `oc whoami -c` komutu.

## <a name="step-2-prepare-the-projects"></a>2\. adım: Projeleri hazırlama

Projeleri oluşturun.
```
oc new-project prometheus-project
oc new-project app-project1
oc new-project app-project2
```


> [!NOTE]
> Kullanabilir `-n` veya `--namespace` ile etkin bir proje seçin veya parametre `oc project` komutu

## <a name="step-3-prepare-prometheus-config"></a>3\. adım: Prometheus config hazırlama
Aşağıdaki içeriğe sahip prometheus.yml adlı bir dosya oluşturun.
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
Yapılandırma ile "prom" adlı bir gizli dizi oluşturun.
```
oc create secret generic prom --from-file=prometheus.yml -n prometheus-project
```

Yukarıda listelenen dosya yapılandırma temel Prometheus dosyadır.
Aralıkları ayarlar ve Otomatik bulma üç projenin (prometheus proje, uygulama project1, uygulama project2) yapılandırır.
Bu örnekte, uç noktaları HTTP üzerinden kimlik doğrulaması olmadan scraped otomatik bulma.
Uç noktaları değiştirilene daha fazla bilgi için bkz: https://prometheus.io/docs/prometheus/latest/configuration/configuration/#scrape_config.


## <a name="step-4-prepare-alertmanager-config"></a>4\. Adım: Alertmanager config hazırlama
Aşağıdaki içeriğe sahip alertmanager.yml adlı bir dosya oluşturun.
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
"Prom-uyarıları" yapılandırması ile adlı bir gizli dizi oluşturun.
```
oc create secret generic prom-alerts --from-file=alertmanager.yml -n prometheus-project
```

Yukarıda listelenen dosya uyarı Yöneticisi yapılandırma dosyasıdır.

> [!NOTE]
> Önceki iki adımı ile doğrulayabilirsiniz. `oc get secret -n prometheus-project`

## <a name="step-5-start-prometheus-and-alertmanager"></a>5\. Adım: Prometheus ve Alertmanager Başlat
İndirme [prometheus standalone.yaml](
https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml) şablondan [openshift/başlangıç deposunun](https://github.com/openshift/origin/tree/release-3.11/examples/prometheus) ve prometheus projede uygulayın
```
oc process -f https://raw.githubusercontent.com/openshift/origin/release-3.11/examples/prometheus/prometheus-standalone.yaml | oc apply -f - -n prometheus-project
```
Oauth proxy ve oauth proxy ile güvenliği de Alertmanager örneğini önündeki Prometheus örneği oluşturur bir OpenShift şablonu, prometheus standalone.yaml dosyasıdır.  Bu şablonda oauth proxy "" prometheus-project"ad alanını alabilirsiniz" herhangi bir kullanıcı izin verecek şekilde yapılandırılır (bkz `-openshift-sar` bayrağı).

> [!NOTE]
> "Prom" StatefulSet eşit olup olmadığını doğrulayabilirsiniz *İSTENEN* ve *geçerli* sayı Çoğaltmalarla `oc get statefulset -n prometheus-project` komutu.
> İle projedeki tüm kaynakları da kontrol edebilirsiniz `oc get all -n prometheus-project`.

## <a name="step-6-add-permissions-to-allow-service-discovery"></a>6\. Adım: Hizmet bulma izin vermek için izinleri ekleme
Aşağıdaki içerikle prometheus sdrole.yml oluşturun.
```
apiVersion: template.openshift.io/v1
kind: Template
metadata:
  name: prometheus-sdrole
  annotations:
    "openshift.io/display-name": Prometheus Service Discovery Role
    description: |
      A role and rolebinding adding permissions required to perform Service Discovery in a given project.
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
Şablon, hizmet bulma izin vermek istediğiniz tüm projeler için geçerlidir.
```
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project1
oc process -f prometheus-sdrole.yml | oc apply -f - -n app-project2
```
Çok prometheus proje izinleri uygulamak de kendisinden ölçümleri toplamak için Prometheus esnetmek istiyorsanız unutmayın.

> [!NOTE]
> Rol ve RoleBinding doğru şekilde oluşturulmuş, doğrulayabilirsiniz `oc get role` ve `oc get rolebinding` sırasıyla komutları

## <a name="optional-deploy-example-application"></a>İsteğe bağlı: Örnek uygulamayı dağıtma
Her şeyin çalıştığından, ancak ölçüm kaynağı yok. Prometheus URL'ye gidin (https://prom-prometheus-project.apps. *rastgele bir kimlik*. *Bölge*.azmosa.io/), şu komutla bulunabilir.
```
oc get route prom -n prometheus-project
```
Https:// ile ana bilgisayar adı ön eki unutmayın.

**Durumu > hizmet bulma** sayfası 0/0 etkin hedefleri gösterir.

Örnek bir uygulama dağıtmak için hangi kullanıma sunan basit python ölçümleri /metrics uç nokta altında aşağıdaki komutları çalıştırın.
```
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example1 -n app-project1
oc new-app python:3.6~https://github.com/Makdaam/prometheus-example --name=example2 -n app-project2
```
Yeni uygulamalar varsayılan olarak, dağıtımdan sonra hizmet bulma sayfası 30 saniye içinde geçerli hedef olarak görünür. Daha fazla bilgi almak **durumu > hedefleri** sayfası.

> [!NOTE]
> Başarıyla scraped her hedef için "en" ölçümü bir datapoint Prometheus ekler. Tıklayın **Prometheus** sol üst köşe ve "yukarı"'i tıklatın ve ifade girin **yürütme**.

## <a name="next-steps"></a>Sonraki adımlar

Özel Prometheus izleme uygulamalarınıza ekleyebilirsiniz.

Hazırlama Prometheus ölçümleri basitleştiren Prometheus istemci kitaplığı, farklı programlama dili için hazırdır.

 - Java https://github.com/prometheus/client_java
 - Python https://github.com/prometheus/client_python
 - Git https://github.com/prometheus/client_golang
 - Ruby https://github.com/prometheus/client_ruby
