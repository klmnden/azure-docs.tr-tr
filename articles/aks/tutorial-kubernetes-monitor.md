---
title: Azure’da Kubernetes öğreticisi - Kubernetes’i izleme
description: AKS öğreticisi - Microsoft Operations Management Suite (OMS) ile Kubernetes’i izleme
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: tutorial
ms.date: 02/22/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 227601858dbe07e6cb774a2d24878ddca05aaf56
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="monitor-azure-container-service-aks"></a>Azure Container Service’i (AKS) izleme

Kubernetes kümenizin ve kapsayıcılarınızın izlenmesi, özellikle de birden fazla uygulama ile ölçekli olarak bir üretim kümesi çalıştırılırken kritik önem taşır.

Bu öğreticide, [Log Analytics için kapsayıcı çözümü][log-analytics-containers] bölümünü kullanarak AKS kümenizin izlemesini yapılandırırsınız.

Sekizinci bölümün yedinci kısmını oluşturan bu öğretici aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Kapsayıcı izleme çözümünü yapılandırma
> * İzleme aracılarını yapılandırma
> * Azure portalında izleme bilgilerine erişme

## <a name="before-you-begin"></a>Başlamadan önce

Önceki öğreticilerde, bir uygulama kapsayıcı görüntülerine paketlendi, bu görüntüler Azure Container Registry’ye yüklendi ve bir Kubernetes kümesi oluşturuldu.

Bu adımları tamamlamadıysanız ve takip etmek istiyorsanız, [Öğretici 1 – Kapsayıcı görüntüleri oluşturma][aks-tutorial-prepare-app] konusuna dönün.

## <a name="configure-the-monitoring-solution"></a>İzleme çözümünü yapılandırma

Azure portalında **Kaynak oluştur**’u seçin ve `Container Monitoring Solution` araması yapın. Bulunduktan sonra **Oluştur**’u seçin.

![Çözüm ekleme](./media/container-service-tutorial-kubernetes-monitor/add-solution.png)

Yeni bir OMS çalışma alanı oluşturun veya mevcut bir çalışma alanını seçin. OMS Çalışma Alanı formu, bu işlem boyunca size yol gösterir.

Çalışma alanını oluştururken kolayca almak için **Panoya sabitle**’yi seçin.

![OMS Çalışma Alanı](./media/container-service-tutorial-kubernetes-monitor/oms-workspace.png)

İşiniz bittiğinde **Tamam**’ı seçin. Doğrulama tamamlandıktan sonra **Oluştur**’u seçerek kapsama izleme çözümünü oluşturun.

Çalışma alanı oluşturulduktan sonra Azure portalında size sunulur.

## <a name="get-workspace-settings"></a>Çalışma Alanı ayarlarını alma

Kubernetes düğümlerinde çözüm aracısını yapılandırmak için Log Analytics Çalışma Alanı Kimliği ve Anahtarı gereklidir.

Bu değerleri almak için kapsayıcı çözümlerinin sol tarafındaki menüden **OMS Çalışma Alanı**’nı seçin. **Gelişmiş ayarlar**’ı seçin ve **ÇALIŞMA ALANI KİMLİĞİ** ve **BİRİNCİL ANAHTAR** bilgilerini not alın.

## <a name="create-kubernetes-secret"></a>Kubernetes gizli dizisi oluşturma

[kubectl create secret][kubectl-create-secret] komutunu kullanarak OMS çalışma alanı ayarlarını `omsagent-secret` adlı Kubernetes gizli dizisinde depolayın. `WORKSPACE_ID` değerini OMS çalışma alanı kimliği, `WORKSPACE_KEY` değerini ise çalışma alanı anahtarı ile güncelleştirin.

```console
kubectl create secret generic omsagent-secret --from-literal=WSID=WORKSPACE_ID --from-literal=KEY=WORKSPACE_KEY
```

## <a name="configure-monitoring-agents"></a>İzleme aracılarını yapılandırma

Aşağıdaki Kubernetes bildirim dosyası, bir Kubernetes kümesindeki kapsayıcı izleme aracılarını yapılandırmak için kullanılabilir. Her küme düğümünde tek pod çalıştıran bir Kubernetes [DaemonSet][kubernetes-daemonset] oluşturur.

Aşağıdaki metni `oms-daemonset.yaml` adlı bir dosyaya kaydedin.

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
    agentVersion: 1.4.3-174
    dockerProviderVersion: 1.0.0-30
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
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
        - mountPath: /var/log 
          name: host-log
        - mountPath: /etc/omsagent-secret
          name: omsagent-secret
          readOnly: true
        - mountPath: /var/lib/docker/containers 
          name: containerlog-path  
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
    - name: host-log
      hostPath:
       path: /var/log 
    - name: omsagent-secret
      secret:
       secretName: omsagent-secret
    - name: containerlog-path
      hostPath:
       path: /var/lib/docker/containers    
```

Aşağıdaki komut ile DaemonSet oluşturun:

```azurecli
kubectl create -f oms-daemonset.yaml
```

DaemonSet’in oluşturulduğunu görmek için şunu çalıştırın:

```azurecli
kubectl get daemonset
```

Çıktı aşağıdakine benzer:

```
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR                 AGE
omsagent   3         3         3         3            3           beta.kubernetes.io/os=linux   8m
```

Aracılar çalıştırıldıktan sonra OMS’nin verileri alıp işlemesi birkaç dakika sürer.

## <a name="access-monitoring-data"></a>İzleme verilerine erişme

Azure portalında, portal panosuna sabitlenen Log Analytics çalışma alanını seçin. **Kapsayıcı İzleme Çözümü** kutucuğuna tıklayın. Burada, AKS kümesi ve kümedeki kapsayıcılar hakkında bilgi bulabilirsiniz.

![Pano](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Verileri sorgulamaya ve analiz etmeye ilişkin ayrıntılı yönergeler için [Azure Log Analytics belgelerine][log-analytics-docs] bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, OMS ile Kubernetes kümenizi izlediniz. Dahil edilen görevler:

> [!div class="checklist"]
> * Kapsayıcı izleme çözümünü yapılandırma
> * İzleme aracılarını yapılandırma
> * Azure portalında izleme bilgilerine erişme

Kubernetes’i yeni bir sürüme yükseltme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Kubernetes’i yükseltme][aks-tutorial-upgrade]

<!-- LINKS - external -->
[kubectl-create-secret]: https://kubernetes.io/docs/concepts/configuration/secret/
[kubernetes-daemonset]: https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/

<!-- LINKS - internal -->
[aks-tutorial-deploy-app]: ./tutorial-kubernetes-deploy-application.md
[aks-tutorial-prepare-app]: ./tutorial-kubernetes-prepare-app.md
[aks-tutorial-upgrade]: ./tutorial-kubernetes-upgrade-cluster.md
[log-analytics-containers]: ../log-analytics/log-analytics-containers.md
[log-analytics-docs]: ../log-analytics/index.yml
