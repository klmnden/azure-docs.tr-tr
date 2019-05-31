---
title: Azure İzleyicisi'nde Insights'a genel bakış | Microsoft Docs
description: Azure İzleyici belirli uygulamalar ve hizmetler için bir özel izleme deneyimi sağlar. Bu makalede, her biri şu anda kullanılabilir olan öngörüleri kısa bir açıklamasını sağlar.
services: azure-monitor
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: bwren
ms.openlocfilehash: 8cb39a174c570b7019e872d731f49252a9505406
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247237"
---
# <a name="overview-of-insights-in-azure-monitor"></a>Azure İzleyicisi'nde Insights'a genel bakış
Belirli uygulamalar ve hizmetler için özelleştirilmiş bir izleme deneyimi sağlar. Bunlar veri deposunu [Azure İzleyici, veri platformu](../platform/data-platform.md) analiz ve Uyarılar için diğer Azure İzleyici özelliklerden yararlanın ancak ek veri toplamak ve Azure portalında bir benzersiz kullanıcı deneyimi sağlar. Erişim ınsights'tan **Insights** Azure portalında Azure İzleyici menünün bölümünde.

Aşağıdaki bölümler, Azure İzleyici'de şu anda kullanılabilir olan öngörüleri kısa bir açıklamasını sağlar. Ayrıntılar için ayrıntılı belgelere her bakın.

## <a name="application-insights"></a>Application Insights
Application Insights, farklı platformlardaki web geliştiricilerine yönelik kapsamlı bir Uygulama Performans Yönetimi (APM) hizmetidir. Canlı web uygulamanızı izlemek için kullanabilirsiniz. Uygulamalar için çok çeşitli .NET, Node.js ve Java EE dahil olmak üzere platformları üzerinde çalıştığını barındırılan şirket içi, karma veya herhangi bir genel bulut. Ayrıca DevOps işleminizle tümleştirilir ve geliştirme araçları çeşitli bağlantı noktaları vardır.

Bkz: [Application Insights nedir?](../app/app-insights-overview.md).

![Application Insights](media/insights-overview/app-insights.png)

## <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
Azure İzleyici kapsayıcıları izlemeleri için kapsayıcı iş yüklerinin performansını ya da Azure Container Instances'a dağıtılabilir veya Azure Kubernetes Service (AKS) barındırılan Kubernetes kümelerini yönetilen. Özellikle birden çok uygulama ile ölçekli olarak bir üretim kümesi çalıştırırken, kapsayıcılarınızın izlenmesi kritik önem taşır.

Bkz: [kapsayıcılara genel bakış için Azure İzleyici](../insights/container-insights-overview.md).

![Kapsayıcılar için Azure İzleyici](media/insights-overview/container-insights.png)

## <a name="azure-monitor-for-resource-groups-preview"></a>Azure İzleyici kaynak gruplarının (Önizleme)
Azure İzleyici kaynak grupları için önceliklendirme ve bir bütün olarak kaynak grubunun performans ve sistem durumu bağlam sunmaya devam ederken, kaynakların karşılaştığınız sorunların tanılanmasına yardımcı olur.

Bkz: [kaynak grupları (Önizleme) Azure İzleyici ile izleme](../insights/resource-group-insights.md).

![Azure İzleyici kaynak grupları için](media/insights-overview/resource-group-insights.png)

## <a name="azure-monitor-for-vms-preview"></a>Azure İzleyici VM'ler (Önizleme)
Azure sanal makinelerinizi (VM) sanal makineler için Azure İzleyici izler ve uygun ölçekte sanal makine ölçek kümeleri. İşlemlerini ve diğer kaynaklarla dış işlemlere olan bağımlılıklarını izleyerek Windows ve Linux VM'lerinizin performansını ve sistem durumunu analiz eder.

Bkz: [VM'ler için Azure İzleyici nedir?](vminsights-overview.md)

![VM'ler için Azure İzleyici](media/insights-overview/vm-insights.png)

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [Azure İzleyici, veri platformu](../platform/data-platform.md) ınsights tarafından kullanılabilir.
* Farklı hakkında bilgi edinin [Azure İzleyici tarafından kullanılan veri kaynaklarına](../platform/data-sources.md) ve her ınsights tarafından toplanan veriler farklı türde.
