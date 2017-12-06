---
title: "Azure Öğreticisi - İzleyici Kubernetes üzerinde Kubernetes"
description: "AKS Öğreticisi - İzleyici Kubernetes Microsoft Operations Management Suite (OMS)"
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 084c6bf3855bdc757c3f2926b35eaf7bba0ef389
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-azure-container-service-aks"></a>İzleyici Azure kapsayıcı hizmeti (AKS)

İzleme Kubernetes küme ve kapsayıcıları özellikle bir üretim kümesi ölçekli olarak birden çok uygulama ile çalışırken, önemlidir.

Bu öğreticide, AKS küme kullanarak İzlemeyi Yapılandır [günlük analizi için kapsayıcıları çözümü](../log-analytics/log-analytics-containers.md).

Bu öğretici, parçası yedi sekiz, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Kapsayıcı çözüm izleme yapılandırma
> * İzleme aracıları yapılandırma
> * Azure portalında izleme bilgilerini erişim

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine uygulama kapsayıcı görüntüleri, Azure kapsayıcı kayıt defterine karşıya bu görüntüler ve oluşturulan Kubernetes küme paketlenmiştir.

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./tutorial-kubernetes-prepare-app.md).

## <a name="configure-the-monitoring-solution"></a>İzleme çözümü yapılandırmak

Azure portalında seçin **yeni** arayın ve `Container Monitoring Solution`. Bulunduktan sonra seçin **oluşturma**.

![Çözüm Ekle](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Yeni bir OMS çalışma alanı oluşturun veya varolan bir tanesini seçin. OMS çalışma formun bu işleminde size rehberlik eder.

Çalışma alanı oluştururken, seçim **panoya Sabitle** kolay alınamayabilir.

![OMS Çalışma Alanı](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

İşiniz bittiğinde, seçin **Tamam**. Doğrulama tamamlandıktan sonra seçin **oluşturma** izleme çözümü kapsayıcısı oluşturmak için.

Çalışma alanı oluşturulduktan sonra Azure portalında sunulur.

## <a name="get-workspace-settings"></a>Çalışma alanı ayarlarını al

Günlük analizi çalışma alanı kimliği ve anahtarı Kubernetes düğümlerinde çözüm Aracısı'nı yapılandırmak için gereklidir.

Bu değerleri almaya seçin **OMS çalışma** kapsayıcı çözümleri sol taraftaki menüden. Seçin **Gelişmiş ayarları** ve not edin **çalışma alanı kimliği** ve **birincil anahtar**.

## <a name="configure-monitoring-agents"></a>İzleme aracıları yapılandırma

Aşağıdaki Kubernetes bildirim dosyası, izleme aracıları Kubernetes kümede kapsayıcı yapılandırmak için kullanılabilir. Bir Kubernetes oluşturur [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), her küme düğümünde tek pod çalıştırır.

Aşağıdaki metni adlı bir dosyaya kaydedin `oms-daemonset.yaml`ve yer tutucu değerlerini değiştirme `WSID` ve `KEY` günlük analizi çalışma alanı kimliği ve anahtarı.

```YAML
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
 name: omsagent
spec:
 template:
  metadata:
   labels:
    app: omsagent
    agentVersion: 1.4.0-12
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: <WSID>
       - name: KEY
         value: <KEY>
       securityContext:
         privileged: true
       ports:
       - containerPort: 25225
         protocol: TCP
       - containerPort: 25224
         protocol: UDP
       volumeMounts:
        - mountPath: /var/run/docker.sock
          name: docker-sock
        - mountPath: /var/opt/microsoft/omsagent/state/containerhostname
          name: container-hostname
        - mountPath: /var/log
          name: host-log
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   nodeSelector:
    beta.kubernetes.io/os: linux
   # Tolerate a NoSchedule taint on master that ACS Engine sets.
   tolerations:
    - key: "node-role.kubernetes.io/master"
      operator: "Equal"
      value: "true"
      effect: "NoSchedule"
   volumes:
    - name: docker-sock
      hostPath:
       path: /var/run/docker.sock
    - name: container-hostname
      hostPath:
       path: /etc/hostname
    - name: host-log
      hostPath:
       path: /var/log
```

DaemonSet aşağıdaki komutla oluşturun:

```azurecli-interactive
kubectl create -f oms-daemonset.yaml
```

DaemonSet oluşturduğunuz görmek için çalıştırın:

```azurecli-interactive
kubectl get daemonset
```

Çıktı aşağıdakine benzer:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Aracılar çalışan sonra alma ve verileri işlemek OMS birkaç dakika sürer.

## <a name="access-monitoring-data"></a>İzleme verilerine erişim

Azure portalında portal panosuna sabitlendi günlük analizi çalışma alanını seçin. Tıklayın **kapsayıcı izlemesi çözümü** döşeme. Burada AKS küme ve küme kapsayıcılardan hakkında bilgi bulabilirsiniz.

![Pano](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Bkz: [Azure günlük analizi belgeleri](../log-analytics/index.yml) sorgulama ve izleme verilerini analiz etme konusunda ayrıntılı yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, OMS Kubernetes kümenizle izlenen. Görevleri dahil ele:

> [!div class="checklist"]
> * Kapsayıcı çözüm izleme yapılandırma
> * İzleme aracıları yapılandırma
> * Azure portalında izleme bilgilerini erişim

Kubernetes yeni bir sürüme yükseltme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yükseltme Kubernetes](./tutorial-kubernetes-upgrade-cluster.md)