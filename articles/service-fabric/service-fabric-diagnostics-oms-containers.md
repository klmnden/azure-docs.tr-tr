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
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 79d30a47b017379107b63b0006a35534f68c43b9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
ms.locfileid: "34210785"
---
# <a name="monitor-containers-with-log-analytics"></a>Günlük analizi ile İzleyici kapsayıcıları
 
Bu makalede, kapsayıcı olayları görüntülemek için izleme çözümü OMS günlük analizi kapsayıcısı ayarlama ayarlamak için gerekli adımları yer almaktadır. Kapsayıcı olayları toplamak için kümenizi ayarlamak için bu bkz [adım adım öğretici](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>İzleme çözümü kapsayıcısı ayarlama ayarlayın

> [!NOTE]
> Yanı sıra, kümeniz için günlük analizi ayarlanmış sahip düğümler üzerinde dağıtılan OMS Aracısı olması gerekir. ' Ndaki adımları izlerseniz değilse [günlük analizi ayarlamak](service-fabric-diagnostics-oms-setup.md) ve [OMS Aracısı bir kümeye ekleme](service-fabric-diagnostics-oms-agent.md) ilk.

1. Kümeniz günlük analizi ve OMS Aracısı ile ayarlandıktan sonra kapsayıcıları dağıtın. Sonraki adıma geçmeden önce dağıtılacak kapsayıcılarınızı bekleyin.

2. Azure Marketi'nde aramak *kapsayıcı izlemesi çözümü* ve tıklayın **kapsayıcı izlemesi çözümü** izleme + Yönetimi altında görüntülenir kaynak kategorisi.

    ![Kapsayıcılar çözümü ekleme](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Çözüm küme için oluşturulan aynı çalışma alanı içinde oluşturun. Bu değişiklik kapsayıcılarında docker verilerini toplamaya başlamak için aracının otomatik olarak tetikler. Yaklaşık 15 dakika veya bunu, gelen günlükleri ve istatistikleri açık yukarı çözüm görmeniz gerekir. Aşağıdaki resimde gösterildiği gibi.

    ![Temel OMS Panosu](./media/service-fabric-diagnostics-event-analysis-oms/oms-containers-dashboard.png)

Aracı OMS sorgulanan veya görselleştirilmiş performans göstergeleri için kullanılan birkaç kapsayıcı özgü günlükleri koleksiyonunu sağlar. Toplanan günlük türleri şunlardır:

* ContainerInventory: kapsayıcı konumunu, adı ve görüntüleri hakkında bilgi gösterir
* ContainerImageInventory: bilgi kimlikleri veya boyutları dahil olmak üzere, dağıtılan görüntüler hakkında
* ContainerLog: belirli hata günlüklerini, docker günlükleri (stdout, vb.) ve diğer girişleri
* ContainerServiceLog: çalıştırılmış docker arka plan programı komutları
* Perf: kapsayıcı dahil olmak üzere performans sayaçlarını cpu, bellek, ağ trafiğini, disk g/ç ve ana bilgisayar makinelerden özel ölçümleri



## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [OMS'ın kapsayıcıları çözüm](../log-analytics/log-analytics-containers.md).
* Service Fabric üzerinde-kapsayıcı düzenleme hakkında daha fazla bilgiyi [Service Fabric ve kapsayıcıları](service-fabric-containers-overview.md)
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* Ayarlamak için günlük analizi yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için kurallar