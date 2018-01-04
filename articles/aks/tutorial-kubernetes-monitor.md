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
ms.openlocfilehash: b01aa01df198ce75b2f8b66d28a2db68b1c30b87
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="monitor-azure-container-service-aks"></a>İzleyici Azure kapsayıcı hizmeti (AKS)

İzleme Kubernetes küme ve kapsayıcıları özellikle bir üretim kümesi ölçekli olarak birden çok uygulama ile çalışırken, önemlidir.

Bu öğreticide, AKS küme kullanarak İzlemeyi Yapılandır [kapsayıcıları çözüm günlük analizi için][log-analytics-containers].

Bu öğretici, parçası yedi sekiz, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Kapsayıcı çözüm izleme yapılandırma
> * İzleme aracıları yapılandırma
> * Azure portalında izleme bilgilerini erişim

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine uygulama kapsayıcı görüntüleri, Azure kapsayıcı kayıt defterine karşıya bu görüntüler ve oluşturulan Kubernetes küme paketlenmiştir.

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri][aks-tutorial-prepare-app].

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

Aşağıdaki Kubernetes bildirim dosyası, izleme aracıları Kubernetes kümede kapsayıcı yapılandırmak için kullanılabilir. Bir Kubernetes oluşturur [DaemonSet][kubernetes-daemonset], her küme düğümünde tek pod çalıştırır.

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
        - mountPath: /var/lib/docker/containers/
          name: container-log
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
    - name: container-log
      hostPath:
       path: /var/lib/docker/containers/
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

Bkz: [Azure günlük analizi belgeleri] [ log-analytics-docs] sorgulama ve izleme verilerini analiz etme konusunda ayrıntılı yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, OMS Kubernetes kümenizle izlenen. Görevleri dahil ele:

> [!div class="checklist"]
> * Kapsayıcı çözüm izleme yapılandırma
> * İzleme aracıları yapılandırma
> * Azure portalında izleme bilgilerini erişim

Kubernetes yeni bir sürüme yükseltme hakkında bilgi edinmek için sonraki öğretici ilerleyin.

> [!div class="nextstepaction"]
> [Yükseltme Kubernetes][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
