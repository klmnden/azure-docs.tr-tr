---
title: Azure Service Fabric iyi izleme | Microsoft Docs
description: Service Fabric kümeleri ve uygulamaları izlemek için en iyi yöntemler.
services: service-fabric
documentationcenter: .net
author: peterpogorski
manager: chackdan
editor: ''
ms.assetid: 19ca51e8-69b9-4952-b4b5-4bf04cded217
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: d90daaf18e5161053e00671b7667d05ec8e5db76
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60533818"
---
# <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

[İzleme ve tanılama](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-overview) geliştirme, test ve herhangi bir bulut ortamında iş yüklerini dağıtma için kritik öneme sahiptir. Örneğin, uygulamalarınızı, Service Fabric platform, performans sayaçları ile kaynak kullanımınızı ve kümenizin genel sistem tarafından gerçekleştirilen eylemler kullanımını izleyebilirsiniz. Bu bilgiler, tanılamak ve sorunları düzeltin ve gelecekte meydana gelmesini önlemek için kullanabilirsiniz.

## <a name="application-monitoring"></a>Uygulama izleme

Uygulama İzleme özelliklerini ve bileşenlerini, uygulamanızın nasıl kullanıldığını izler. Sorunları kullanıcılarınız etkilemeden yakalanan emin olmak için uygulamalarınızı izleyin. Uygulama izleme, bu uygulamanızın iş mantığına benzersiz olduğu için uygulama ve hizmetlerini geliştirmeye sorumluluğundadır. İle uygulama izleme işlevini ayarlama önerilir [Application Insights](https://docs.microsoft.com/azure/service-fabric/service-fabric-tutorial-monitoring-aspnet), Azure'nın uygulama izleme aracı.

## <a name="cluster-monitoring"></a>Küme izleme

Service Fabric'in hedeflerinden uygulamalar donanım hatalarına dayanıklı olmasını sağlamaktır. Bu hedef platformun Sistem Hizmetleri, altyapı sorunları ve hızlı bir şekilde diğer düğümlere yük devretme iş yüklerini kümedeki algılama özelliğini aracılığıyla sağlanır. Ancak, Sistem Hizmetleri sorunları varsa ne olur? Ya da hizmetleri yerleşimi için kuralları, dağıtmak veya bir iş yükü taşımak ise çalışılıyor, ihlal? Service Fabric, bunlar için tanılama ve nasıl Service Fabric platform, uygulamaları, hizmetleri, kapsayıcılar ve düğümleri ile etkileşime giren hakkında bilgilendirilmesi emin olmak için diğer sorunlara da sağlar.

Windows kümeleri için ile küme izleme işlevini ayarlama önerilir [tanılama aracısını](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-event-aggregation-wad) ve [Azure İzleyicisi](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-setup).

Linux kümeleri için Azure İzleyici günlüklerine var. Ayrıca Azure platformu ve altyapı izleme için önerilen araç Linux platformu tanılama içinde belirtilenlerle farklı yapılandırma gerektiren [Service Fabric Linux kümesi olayları Syslog](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-syslog).

## <a name="infrastructure-monitoring"></a>Altyapı izleme

[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-diagnostics-oms-agent) küme düzeyindeki olayları izleme için önerilir. Log Analytics aracısını çalışma alanınızla önceki bağlantısında açıklandığı gibi yapılandırdıktan sonra CPU kullanımı gibi performans ölçümlerini işlem düzeyi CPU kullanımı, Service Fabric performans gibi .NET performans sayaçları toplamak mümkün olmayacak bir güvenilir hizmet ve kapsayıcı ölçümleri CPU kullanımı gibi özel durumların sayısı gibi sayaçları.  Azure İzleyici günlüklerine kullanılabilir olmaları için stdout veya stderr kapsayıcı günlüklerini yazma izni gerekir.

## <a name="watchdogs"></a>Watchdogs

Genellikle, bir izleme sistem durumu ve yük hizmetlerinde izleyen, uç noktaları ping atar ve küme beklenmeyen bir sistem durumu olaylarını raporlar ayrı bir hizmettir. Bu, yalnızca tek bir hizmet performansını temel olabilir hatalarını engellemeye yardımcı olabilir. Watchdogs Ayrıca belirli zaman aralıklarında depolama günlük dosyalarında temizleme gibi kullanıcı etkileşimi gerektirmeyen düzeltici eylem gerçekleştiren ana kod için idealdir. Bir örnek izleme hizmeti uygulamasında görmek [Service Fabric Linux kümesi olayları Syslog](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı izleme kullanmaya başlayın: [Uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md).
* Git ile uygulamanızı Application ınsights'ı ayarlamak için adımları [İzleyici Service fabric'te ASP.NET Core uygulamasını ve tanılama](service-fabric-tutorial-monitoring-aspnet.md).
* Platform ve Service Fabric için sunar olayları izleme hakkında daha fazla bilgi edinin: [Platform düzeyinde olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).
* Azure İzleyici logs tümleştirmesi, Service Fabric ile yapılandırın: [Azure İzleyici günlüklerine kümesi için ayarlayın](service-fabric-diagnostics-oms-setup.md)
* Kapsayıcıları izlemek için Azure İzleyici günlüklerine ayarlama işlemleri gerçekleştirmeyi öğreneceksiniz: [Azure'da Windows kapsayıcıları için izleme ve tanılama Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Örnek tanılama sorunlar ve çözümleri ile Service Fabric'i bakın: [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Azure kaynakları için izleme genel öneriler hakkında bilgi edinin: [En iyi uygulamalar - izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).