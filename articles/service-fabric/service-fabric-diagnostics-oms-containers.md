---
title: İzleme günlük analizi ile Azure Service Fabric kapsayıcılarında | Microsoft Docs
description: Günlük analizi Azure Service Fabric kümelerde çalışan kapsayıcılar izlemek için kullanın.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 7a775b6d23c144c81650bb3608ee6a117475a9ba
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="monitor-containers-with-log-analytics"></a>Günlük analizi ile İzleyici kapsayıcıları
 
Bu makalede, kümeniz için kapsayıcı izleme ayarlamak için gerekli adımlar kapsanmaktadır. Bunun hakkında daha fazla bilgi için bkz: [Service Fabric izleme kapsayıcılarında](service-fabric-diagnostics-event-analysis-oms.md#monitoring-containers). Bu adım adım öğretici görmek için de izleyebilirsiniz [Service Fabric İzleyici Windows kapsayıcılarında](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>İzleme çözümü kapsayıcısı ayarlama ayarlayın

> [!NOTE]
> Yanı sıra, kümeniz için günlük analizi ayarlanmış sahip düğümler üzerinde dağıtılan OMS Aracısı olması gerekir. ' Ndaki adımları izlerseniz değilse [günlük analizi ayarlamak](service-fabric-diagnostics-oms-setup.md) ve [OMS Aracısı bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) ilk.

1. Kümeniz günlük analizi ve OMS Aracısı ile ayarlandıktan sonra kapsayıcıları dağıtın. Sonraki adıma geçmeden önce dağıtılacak kapsayıcılarınızı bekleyin.

2. Azure Marketi'nde aramak *kapsayıcı izlemesi çözümü* ve tıklayın **kapsayıcı izlemesi çözümü** izleme + Yönetimi altında görüntülenir kaynak kategorisi.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Çözüm küme için oluşturulan aynı çalışma alanı içinde oluşturun. Bu değişiklik kapsayıcılarında docker verilerini toplamaya başlamak için aracının otomatik olarak tetikler. Yaklaşık 15 dakika veya bunu, gelen günlükleri ve istatistikleri açık yukarı çözüm görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar
* Service Fabric üzerinde-kapsayıcı düzenleme hakkında daha fazla bilgiyi [Service Fabric ve kapsayıcıları](service-fabric-containers-overview.md)
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için kurallar