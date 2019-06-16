---
title: İzleme günlüklerini Azure İzleyici ile Azure Service Fabric kapsayıcıları | Microsoft Docs
description: Azure İzleyici günlüklerine, Azure Service Fabric kümelerinde çalışan kapsayıcıları izlemek üzere kullanın.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: d03d68560502821b9c343be983d9f7b5a83ed977
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60321915"
---
# <a name="monitor-containers-with-azure-monitor-logs"></a>Azure İzleyici günlüklerine ile kapsayıcıları izleme
 
Bu makalede kapsayıcı olayları görüntülemek için Azure İzleyici günlüklerine kapsayıcı izleme çözümünü için gerekli adımları ele alınmaktadır. Kapsayıcı olayları toplamak için kümenizi ayarlamak için bu bkz [adım adım öğretici](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Kapsayıcı izleme çözümünü ayarlama

> [!NOTE]
> Azure İzleyicisi'ni günlükleri kümeniz için ayarlanmış yanı sıra, düğümlerde dağıtılan Log Analytics aracısını sahip olması gerekir. Adımları izlerseniz yoksa [Azure İzleyici günlüklerini ayarlamak](service-fabric-diagnostics-oms-setup.md) ve [Log Analytics aracısını bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) ilk.

1. Azure İzleyici günlüklerine ve Log Analytics aracısını ile kümenizi ayarlandıktan kapsayıcılarınızı dağıtın. Sonraki adıma geçmeden önce dağıtılacak kapsayıcılarınızı bekleyin.

2. Azure Market'te arama *kapsayıcı izleme çözümü* tıklayın **kapsayıcı izleme çözümü** izleme + Yönetim altında gösterilir kaynak kategorisi.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Küme için önceden oluşturulmuş çalışma alanı içindeki çözümü oluşturun. Bu değişiklik, kapsayıcılara göre docker verilerini toplamaya başlamak için aracının otomatik olarak tetikler. Yaklaşık 15 dakika veya bunu, aşağıdaki resimde gösterildiği gibi gelen günlüklerinin ve istatistiklerinin, ile açık yedekleme çözümü görmeniz gerekir.

    ![Temel Log Analytics Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Azure İzleyici günlüklerine sorgulanan veya performans göstergelerini görselleştirmek için kullanılan birden fazla kapsayıcı özgü günlük koleksiyonu aracı sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumunu ve adını görüntüleri ile ilgili bilgileri gösterir.
* ContainerImageInventory: bilgi kimliği veya boyutları da dahil olmak üzere dağıtılan görüntüler hakkında
* ContainerLog: özel hata günlükleri, docker günlüklerini (stdout, vb.) ve diğer girişler
* ContainerServiceLog: çalıştırılmış docker daemon komutları
* Performans: kapsayıcı dahil olmak üzere performans sayaçları cpu, bellek, ağ trafiğini, disk g/ç ve konak makinelerden özel ölçümler



## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Azure İzleyici günlükleri kapsayıcılar çözümü](../azure-monitor/insights/containers.md).
* -Service fabric'te kapsayıcı düzenleme hakkında daha fazla bilgiyi [Service Fabric ve kapsayıcılar](service-fabric-containers-overview.md)
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri, Azure İzleyici günlüklerine bir parçası olarak sunulan
* Azure İzleyici günlüklerine ayarlamak için yapılandırma [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olmak için kurallar