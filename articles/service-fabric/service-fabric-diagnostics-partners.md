---
title: Azure Service Fabric'e izleme iş ortakları | Microsoft Docs
description: Azure Service Fabric iş ortağı çözümlerini izleme ile izleme hakkında bilgi edinin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/16/2018
ms.author: srrengar
ms.openlocfilehash: e9bfb835a1dea240573c21ccbbe13088561851ab
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58661420"
---
# <a name="azure-service-fabric-monitoring-partners"></a>Azure Service Fabric izleme iş ortakları

Bu makalede, nasıl bir, Service Fabric uygulamaları, kümeler ve altyapı iş ortağı çözümlerini bir küçük izleyebilirsiniz gösterilmektedir. Her bir Service Fabric için tümleşik teklifleri oluşturmak için aşağıdaki ortakları ile çalıştı.

## <a name="dynatrace"></a>Dynatrace

Dynatrace ile bizim tümleştirme çok Service Fabric kümeleriniz izlemek için kutusunu özellikleri dışında sağlar. Dynatrace OneAgent VMSS örneklerinizin yükleme uygulama düzeye, performans sayaçları ve Service Fabric dağıtımınızın bir topoloji sunar. Dynatrace, şirket içinde izleme için ideal bir tercih de var. Daha fazla listelenen özellikleri kullanıma [duyuru](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) ve [yönergeleri](https://www.dynatrace.com/news/blog/automatic-end-to-end-service-fabric-monitoring-with-dynatrace/) Dynatrace kümenizde etkinleştirmek için. 

## <a name="datadog"></a>Datadog

Datadog, hem Windows hem de Linux örnekleri için VMSS için uzantısına sahiptir. Datadog kullanarak Windows olay günlüklerini toplar ve dolayısıyla Windows üzerinde Service Fabric platform olaylarını topla. Datadog için tanılama verilerinizi göndermek yönergeler kullanıma [burada](https://www.datadoghq.com/blog/azure-monitoring-enhancements/#integrate-with-azure-service-fabric).

## <a name="appdynamics"></a>AppDynamics

AppDynamics ile Service Fabric uygulama düzeyinde tümleştirmedir. Ortam değişkenlerini güncelleştirme ve uygulama Dynamics Nuget'i kullanma için AppDynamics uygulama telemetri gönderebilir. Başvurun [yönergeleri](https://docs.appdynamics.com/display/AZURE/Install+AppDynamics+for+Azure+Service+Fabric) nasıl AppDynamics ile .NET Service Fabric uygulamalarınızı tümleştirin.

## <a name="new-relic"></a>New Relic

Yeni Relic da Service Fabric uygulamaları ile tümleşen başka bir uygulama performansı Yönetimi aracıdır. Yeni Relic NuGet paketlerini yükleme ve için New Relic uygulama telemetrinizi göndermek için bildirim dosyalarınızın belirli ortam değişkenlerini ekleyin. Bu kontrol [yönergeleri](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/install-net-agent-azure-service-fabric) .NET Service Fabric uygulamalarınızı için New Relic telemetri etkinleştirmek için.

## <a name="elk"></a>ELK 

ELK yığını, açık kaynaklı teknolojiler topluluğudur: Elasticsearch, Logstash ve kibana'yı. Bunları birlikte kullanarak toplamak, depolamak ve Service Fabric izleme ve tanılama verilerini analiz edin. Bir öğretici için Service Fabric yerel Java uygulamalarıyla bunun nasıl yapılacağını sahibiz [burada](service-fabric-tutorial-java-elk.md). 


## <a name="next-steps"></a>Sonraki adımlar

* Alma bir [izleme ve tanılama genel bakış](service-fabric-diagnostics-overview.md) Service fabric'te
* Bilgi nasıl [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md) birinci taraf araçlarımız ile
