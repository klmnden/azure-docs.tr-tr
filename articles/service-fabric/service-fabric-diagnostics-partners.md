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
ms.openlocfilehash: c2f953c98e41291951f07556bd0cd441d2793d1d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59796257"
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

## <a name="humio"></a>Humio

Humio uygulamalardan ve Service Fabric bulutta veya şirket içi gerçek zamanlı olaylardan günlükleri toplayabilir, bir günlük koleksiyonu hizmetidir. Canlı observability yanı sıra Humio ınsights Tanılama'ya toplamak ve görüntülemek için teknoloji analiz ve görselleştirme özellikleri sunar. Humio uygun maliyetli fiyatlandırma planları sahip ve için oluşturulmuştur, korurken ölçek hızlı gölge. Ayrıca Service Fabric platform olaylarına ve uygulama telemetrisini ile doğrudan tümleşir. Daha fazla bilgi edinebilirsiniz Humio ve Service Fabric tümleştirmesi hakkında [burada](https://github.com/humio/service-fabric-humio).

## <a name="next-steps"></a>Sonraki adımlar

* Alma bir [izleme ve tanılama genel bakış](service-fabric-diagnostics-overview.md) Service fabric'te
* Bilgi nasıl [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md) birinci taraf araçlarımız ile
