---
title: Azure İzleyici (Önizleme) kapsayıcı Aracısı yükseltme | Microsoft Docs
description: Bu makalede, Azure İzleyici tarafından kullanılan kapsayıcıları için Log Analytics aracısını nasıl yükseltme açıklanır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/28/2018
ms.author: magoedte
ms.openlocfilehash: 026bdd6a59dc84220e7e52707cee3c1971fba838
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104331"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Azure İzleyici (Önizleme) kapsayıcı Aracısı yükseltme
Kapsayıcılar için Azure İzleyici, Linux için Log Analytics aracısını kapsayıcı bir sürümünü kullanır. Aracıyı yeni bir sürümü yayımlandığında, aracıyı Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri üzerinde otomatik olarak yükseltilir.  

Bu makalede, aracı yükseltme başarısız olursa, el ile aracı yükseltme işlemi açıklanmaktadır. Yayımlanan sürümleri takip etmek için bkz: [Aracı sürüm duyuruları](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).   

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>İzlenen bir Kubernetes kümesinde aracı yükseltme
Aracı yükseltme işlemi, iki anlaşılır adımdan oluşur. İlk adım, Azure İzleyici ile Azure CLI kullanarak kapsayıcıları için izleme devre dışı bırakmaktır.  İçinde açıklanan adımları izleyin [izlemeyi devre dışı](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli) makalesi. Azure CLI kullanarak çözüm ve çalışma alanında depolanan karşılık gelen verileri etkilemeden Kümedeki düğümlerden aracıyı kaldırmak sağlıyor. 

>[!NOTE]
>Bu bakım etkinliği gerçekleştiriyorsanız, kümedeki düğümler, toplanan verileri ilettiğiniz değil ve performans görünümlerine veri gösterilmez arasındaki zaman aracıyı kaldırın ve ardından yeni sürümü yükleyin. 
>

Aracısı'nın yeni sürümünü yüklemek için açıklanan adımları izleyin. [yerleşik izleme](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) bu işlemi tamamlamak için Azure CLI kullanarak makalesi.  

İzleme yeniden etkinleştirdikten sonra bu küme için güncelleştirilmiş sistem durumu ölçümleri görmeden önce yaklaşık 15 dakika sürebilir. Aracı başarıyla yükseltildi doğrulamak için komutu çalıştırın: `kubectl logs omsagent-484hw --namespace=kube-system`

Durum, aşağıdaki örneğe benzemelidir burada değeri *OMI* ve *omsagent* belirtilen en son sürümü ile eşleşmelidir [Aracı sürüm geçmişi](https://github.com/microsoft/docker-provider/tree/ci_feature_prod).  

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
    omi 1.4.2.5
    omsagent 1.6.0-163
    docker-cimprov 1.0.0.31

## <a name="next-steps"></a>Sonraki adımlar
Aracı yükseltme sırasında sorunlarla karşılaşırsanız, gözden [sorun giderme kılavuzu](container-insights-troubleshoot.md) desteği.