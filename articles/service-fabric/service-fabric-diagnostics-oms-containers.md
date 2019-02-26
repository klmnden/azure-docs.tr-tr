---
title: Azure Service fabric'te Log Analytics ile kapsayıcıları izlemek | Microsoft Docs
description: Log Analytics, Azure Service Fabric kümelerinde çalışan kapsayıcıları izlemek üzere kullanın.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 2123cf0eb575d632e871e23513128e67d5433c9d
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820186"
---
# <a name="monitor-containers-with-log-analytics"></a>Log Analytics ile kapsayıcıları izleme
 
Bu makalede kapsayıcı olayları görüntülemek için Azure Log Analytics kapsayıcı izleme çözümünü için gerekli adımları ele alınmaktadır. Kapsayıcı olayları toplamak için kümenizi ayarlamak için bu bkz [adım adım öğretici](service-fabric-tutorial-monitoring-wincontainers.md). 

[!INCLUDE [log-analytics-agent-note.md](../../includes/log-analytics-agent-note.md)]

## <a name="set-up-the-container-monitoring-solution"></a>Kapsayıcı izleme çözümünü ayarlama

> [!NOTE]
> Yanı sıra kümeniz için Log Analytics ayarlanmış sahip Log Analytics aracısını düğümlerinizi üzerinde dağıtılmış olması gerekir. Adımları izlerseniz yoksa [Log Analytics'i ayarlama](service-fabric-diagnostics-oms-setup.md) ve [Log Analytics aracısını bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) ilk.

1. Log Analytics aracısını Log Analytics ile kümenizi ayarlandıktan kapsayıcılarınızı dağıtın. Sonraki adıma geçmeden önce dağıtılacak kapsayıcılarınızı bekleyin.

2. Azure Market'te arama *kapsayıcı izleme çözümü* tıklayın **kapsayıcı izleme çözümü** izleme + Yönetim altında gösterilir kaynak kategorisi.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Küme için önceden oluşturulmuş çalışma alanı içindeki çözümü oluşturun. Bu değişiklik, kapsayıcılara göre docker verilerini toplamaya başlamak için aracının otomatik olarak tetikler. Yaklaşık 15 dakika veya bunu, aşağıdaki resimde gösterildiği gibi gelen günlüklerinin ve istatistiklerinin, ile açık yedekleme çözümü görmeniz gerekir.

    ![Temel Log Analytics Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Log Analytics'te sorgulanan veya performans göstergelerini görselleştirmek için kullanılan birden fazla kapsayıcı özgü günlük koleksiyonu aracı sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumunu ve adını görüntüleri ile ilgili bilgileri gösterir.
* ContainerImageInventory: bilgi kimliği veya boyutları da dahil olmak üzere dağıtılan görüntüler hakkında
* ContainerLog: özel hata günlükleri, docker günlüklerini (stdout, vb.) ve diğer girişler
* ContainerServiceLog: çalıştırılmış docker daemon komutları
* Performans: kapsayıcı dahil olmak üzere performans sayaçları cpu, bellek, ağ trafiğini, disk g/ç ve konak makinelerden özel ölçümler



## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Log Analytics kapsayıcılar çözümü](../azure-monitor/insights/containers.md).
* -Service fabric'te kapsayıcı düzenleme hakkında daha fazla bilgiyi [Service Fabric ve kapsayıcılar](service-fabric-containers-overview.md)
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri Log Analytics kapsamında sunulan
* Log Analytics'i yapılandırma [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olmak için kurallar