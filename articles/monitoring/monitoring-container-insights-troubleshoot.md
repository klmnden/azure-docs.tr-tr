---
title: Kapsayıcılar için Azure İzleyici gidermek için nasıl | Microsoft Docs
description: Bu makalede, sorun giderme ve kapsayıcılar için Azure İzleyici ile ilgili sorunları giderme nasıl açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2018
ms.author: magoedte
ms.openlocfilehash: de7ae5788224b83105e4dc9a24aea35c8b841c88
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46986736"
---
# <a name="troubleshooting-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici sorunlarını giderme

Azure Kubernetes Service (AKS) kümenizi kapsayıcılar için Azure İzleyici ile izleme yapılandırdığınızda veri toplama işlemini engelliyor veya durum raporlama bir sorunla karşılaşabilirsiniz. Bu makalede bazı yaygın sorunlar ve sorun giderme adımları ayrıntılı olarak açıklanmaktadır.

## <a name="azure-monitor-for-containers-is-enabled-but-not-reporting-any-information"></a>Kapsayıcılar için Azure İzleyici herhangi bir bilgi bildirmeyen ancak etkin
Kapsayıcılar için Azure İzleyici başarıyla etkinleştirildikten ve yapılandırıldıktan sonra ancak durum bilgilerini görüntüleyemezsiniz veya bir Log Analytics günlük sorgudan döndürülen sonuç, aşağıdaki adımları izleyerek sorunu tanılamak: 

1. Komutunu çalıştırarak aracının durumunu denetleyin: 

    `kubectl get ds omsagent --namespace=kube-system`

    Doğru şekilde dağıtıldığını gösteren aşağıdaki çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get ds omsagent --namespace=kube-system 
    NAME       DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR                 AGE
    omsagent   2         2         2         2            2           beta.kubernetes.io/os=linux   1d
    ```  
2. Aracı sürümü ile dağıtım durumunu denetlemek *06072018* veya daha sonra komutunu kullanarak:

    `kubectl get deployment omsagent-rs -n=kube-system`

    Doğru şekilde dağıtıldığını gösterir aşağıdaki örnek çıktıya benzemelidir:

    ```
    User@aksuser:~$ kubectl get deployment omsagent-rs -n=kube-system 
    NAME       DESIRED   CURRENT   UP-TO-DATE   AVAILABLE    AGE
    omsagent   1         1         1            1            3h
    ```

3. Komutunu kullanarak çalıştığını doğrulamak için pod durumunu kontrol edin: `kubectl get pods --namespace=kube-system`

    Çıkış durumu aşağıdaki örneğe benzemelidir *çalıştıran* omsagent için:

    ```
    User@aksuser:~$ kubectl get pods --namespace=kube-system 
    NAME                                READY     STATUS    RESTARTS   AGE 
    aks-ssh-139866255-5n7k5             1/1       Running   0          8d 
    azure-vote-back-4149398501-7skz0    1/1       Running   0          22d 
    azure-vote-front-3826909965-30n62   1/1       Running   0          22d 
    omsagent-484hw                      1/1       Running   0          1d 
    omsagent-fkq7g                      1/1       Running   0          1d 
    ```

4. Aracı günlüklerini denetleyin. Kapsayıcı Aracısı dağıtıldığında OMI komutları çalıştırarak hızlı bir denetim çalıştırır ve sağlayıcı ve aracı sürümünü gösterir. 

5. Aracıyı eklenen başarıyla sağlandığını doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

    Durum, aşağıdaki örneğe benzemelidir:

    ```
    User@aksuser:~$ kubectl logs omsagent-484hw --namespace=kube-system
    :
    :
    instance of Container_HostInventory
    {
        [Key] InstanceID=3a4407a5-d840-4c59-b2f0-8d42e07298c2
        Computer=aks-nodepool1-39773055-0
        DockerVersion=1.13.1
        OperatingSystem=Ubuntu 16.04.3 LTS
        Volume=local
        Network=bridge host macvlan null overlay
        NodeRole=Not Orchestrated
        OrchestratorType=Kubernetes
    }
    Primary Workspace: b438b4f6-912a-46d5-9cb1-b44069212abc    Status: Onboarded(OMSAgent Running)
    omi 1.4.2.2
    omsagent 1.6.0.23
    docker-cimprov 1.0.0.31
    ```

## <a name="next-steps"></a>Sonraki adımlar
AKS kümesi düğümlerin ve pod'ları için sistem durumu ölçümleri yakalamak için etkinleştirilmiş izleme ile bu sistem durumu ölçümleri Azure portalında kullanılabilir. Kapsayıcılar için Azure İzleyicisi'ni kullanma konusunda bilgi almak için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](monitoring-container-insights-analyze.md).