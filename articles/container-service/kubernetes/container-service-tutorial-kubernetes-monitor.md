---
title: "Azure kapsayıcı hizmeti Öğreticisi - İzleyici Kubernetes | Microsoft Docs"
description: "Azure kapsayıcı hizmeti Öğreticisi - İzleyici Kubernetes Microsoft Operations Management Suite (OMS)"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Mikro docker, kapsayıcıları, hizmetleri, Kubernetes, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: aurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f72944fe819a79edbafb73fba635d73642f33e4f
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="monitor-a-kubernetes-cluster-with-operations-management-suite"></a>Operations Management Suite Kubernetes kümeyle izleme

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

Özellikle birden çok uygulama ile ölçekli üretim kümesi yönetirken izleme Kubernetes küme ve kapsayıcıları, önemlidir. 

Birkaç Kubernetes izleme çözümlerinin, Microsoft veya diğer sağlayıcıları avantajından yararlanabilirsiniz. Bu öğreticide, Kubernetes kümenizi kapsayıcıları çözümde kullanarak izlemenizi [Operations Management Suite](../../operations-management-suite/operations-management-suite-overview.md), Microsoft'un bulut tabanlı BT yönetim çözümü. (OMS kapsayıcıları çözüm önizlemede değil.)

Bu öğretici, parçası yedi yedi, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * OMS çalışma ayarlarını al
> * Kubernetes düğümlerdeki OMS aracılarını ayarlama
> * İzleme bilgilerini OMS portalında veya Azure portalına erişim

## <a name="before-you-begin"></a>Başlamadan önce

Önceki eğitimlerine uygulama kapsayıcı görüntüleri, Azure kapsayıcı kayıt defterine karşıya bu görüntüler ve oluşturulan Kubernetes küme paketlenmiştir. 

Bu adımları yapmadıysanız ve izlemek istediğiniz, geri dönüp [Öğreticisi 1 – Oluştur kapsayıcı görüntüleri](./container-service-tutorial-kubernetes-prepare-app.md). 

## <a name="get-workspace-settings"></a>Çalışma alanı ayarlarını al

Ne zaman erişebilirsiniz [OMS portalı](https://mms.microsoft.com)gidin **ayarları** > **bağlı kaynakları** > **Linux sunucuları**. Burada, bulabileceğiniz *çalışma alanı kimliği* ve birincil veya ikincil *çalışma alanı anahtarı*. OMS aracıları kümedeki ayarlamak için gereken bu değerleri not edin.

## <a name="set-up-oms-agents"></a>OMS aracılarını ayarlama

Linux küme düğümlerinde OMS Aracısı ayarlamak için bir YAML dosyası aşağıda verilmiştir. Bir Kubernetes oluşturur [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/), her küme düğümünde tek aynı pod çalıştırır. DaemonSet kaynak izleme aracısını dağıtmak için idealdir. 

Aşağıdaki metni adlı bir dosyaya kaydedin `oms-daemonset.yaml`ve yer tutucu değerlerini değiştirme *myWorkspaceID* ve *myWorkspaceKey* OMS çalışma alanı kimliği ve anahtarı. (Üretimde, bu değerleri gizlilikleri olarak şifreleyebilirsiniz.)

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
    agentVersion: v1.3.4-127
    dockerProviderVersion: 10.0.0-25
  spec:
   containers:
     - name: omsagent 
       image: "microsoft/oms"
       imagePullPolicy: Always
       env:
       - name: WSID
         value: myWorkspaceID
       - name: KEY 
         value: myWorkspaceKey
       - name: DOMAIN
         value: opinsights.azure.com
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
       livenessProbe:
        exec:
         command:
         - /bin/bash
         - -c
         - ps -ef | grep omsagent | grep -v "grep"
        initialDelaySeconds: 60
        periodSeconds: 60
   volumes:
    - name: docker-sock 
      hostPath:
       path: /var/run/docker.sock
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

```azurecli-interactive
NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE-SELECTOR   AGE
omsagent   3         3         3         0            3           <none>          5m
```

Aracılar çalışan sonra alma ve verileri işlemek OMS birkaç dakika sürer.

## <a name="access-monitoring-data"></a>İzleme verilerine erişim

Görüntüleme ve OMS kapsayıcı verilerle izleme çözümleme [kapsayıcı çözüm](../../log-analytics/log-analytics-containers.md) OMS portalında veya Azure portalı. 

Kapsayıcı çözümünü kullanarak yüklemek için [OMS portalı](https://mms.microsoft.com)gidin **Çözümleri Galerisi**. Ardından ekleyin **kapsayıcı çözüm**. Alternatif olarak, kapsayıcı çözümden eklemek [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft.containersoms?tab=Overview).

OMS Portalı'nda arayın bir **kapsayıcıları** OMS Pano Özet kutucuğu. Gibi ayrıntıları içeren kutucuğa tıklayın: kapsayıcı olayları, hatalar, durumu, görüntü stok ve CPU ve bellek kullanımı. Daha ayrıntılı bilgi için herhangi bir kutucuğu satırındaki'ı tıklatın veya gerçekleştirmek bir [günlük arama](../../log-analytics/log-analytics-log-searches.md).

![OMS portalında kapsayıcıları Panosu](./media/container-service-tutorial-kubernetes-monitor/oms-containers-dashboard.png)

Azure portalında Git benzer şekilde, **günlük analizi** ve çalışma alanı adı seçin. Görmek için **kapsayıcıları** Özet kutucuğuna tıklayın **çözümleri** > **kapsayıcıları**. Ayrıntıları görmek için kutucuğa tıklayın.

Bkz: [Azure günlük analizi belgeleri](../../log-analytics/index.yml) sorgulama ve izleme verilerini analiz etme konusunda ayrıntılı yönergeler için.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, OMS Kubernetes kümenizle izlenen. Görevleri dahil ele:

> [!div class="checklist"]
> * OMS çalışma ayarlarını al
> * Kubernetes düğümlerdeki OMS aracılarını ayarlama
> * İzleme bilgilerini OMS portalında veya Azure portalına erişim


Kapsayıcı hizmeti için önceden derlenmiş kod örnekleri görmek için bu bağlantıyı izleyin.

> [!div class="nextstepaction"]
> [Azure kapsayıcı hizmeti kod örnekleri](cli-samples.md)
