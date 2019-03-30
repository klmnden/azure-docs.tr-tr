---
title: Azure İzleyici ile Azure Service Fabric olay analizi günlükleri | Microsoft Docs
description: İzleme ve Tanılama, Azure Service Fabric kümeleri için Azure İzleyici günlüklerine kullanarak olayları analiz ve görselleştirme hakkında bilgi edinin.
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
ms.date: 02/21/2019
ms.author: srrengar
ms.openlocfilehash: ba4923edbc59f0e6650fda1a71e1c4f79b884cf2
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662168"
---
# <a name="event-analysis-and-visualization-with-azure-monitor-logs"></a>Olay çözümleme ve görselleştirme ile Azure izleme günlükleri
 Azure izleme günlükleri toplar ve uygulamalardan ve bulut üzerinde barındırılan hizmetlerden daha fazla telemetri analiz eder ve kullanılabilirliği ve performansı en üst düzeye çıkarmanıza yardımcı olması için analiz araçları sağlar. Bu makalede, Azure İzleyici günlüklerine Öngörüler edinin ve neler kümenizde sorun giderme için sorguları çalıştırmak nasıl özetlenmektedir. Aşağıdaki yaygın sorular ele alınmıştır:

* Sistem durumu olaylarını sorunlarını nasıl giderebilirim?
* Ne zaman bir düğüm arıza olduğunu nasıl öğrenebilirim?
* My uygulamanın hizmetlerine başlatıldığı veya durdurulduğu nasıl anlarım?

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="overview-of-the-log-analytics-workspace"></a>Log Analytics çalışma alanına genel bakış

>[!NOTE] 
>Tanılama depolama, küme oluşturma sırasında varsayılan olarak etkindir, ancak tanılama depolama alanından okuma yine de Log Analytics çalışma alanını ayarlamanız gerekir.

Azure İzleyici, bir Azure depolama tablosu veya bir aracı gibi yönetilen kaynaklardan toplanan veri günlükleri ve merkezi bir depoya tutar. Veriler daha sonra analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla dışa aktarma olabilir. Azure İzleyicisi'ni destekleyen olayları, performans verisi veya herhangi bir özel veri kaydeder. Kullanıma [olayları tanılama uzantısını yapılandırma adımları](service-fabric-diagnostics-event-aggregation-wad.md) ve [depolama olayları okumak için bir Log Analytics çalışma alanı oluşturmak için adımları](service-fabric-diagnostics-oms-setup.md) veri Azure İzleyici ile akan emin olmak için günlüğe kaydeder.

Azure İzleyici günlüklerine tarafından verilerin alındıktan sonra Azure vardır *izleme çözümleri* olan önceden paketlenmiş bir çözüm ya da birkaç senaryo için özelleştirilmiş, gelen verileri izlemek için işletimsel panolar. Bunlar bir *Service Fabric analizi* çözüm ve *kapsayıcıları* tanılama ve Service Fabric kümeleri kullanılırken izleme iki en uygun ayarlara olan çözüm. Bu makalede, çalışma alanı ile oluşturulmuş Service Fabric analizi çözümü kullanmayı açıklar.

## <a name="access-the-service-fabric-analytics-solution"></a>Service Fabric analizi çözümü erişim

İçinde [Azure portalı](https://portal.azure.com), Service Fabric analizi çözümü oluşturduğunuz kaynak grubuna gidin.

Kaynak seçin **ServiceFabric\<nameOfOMSWorkspace\>**.

İçinde `Summary`, her bir Service Fabric için de dahil olmak üzere etkin çözüm için bir grafik biçiminde kutucuklar görürsünüz. Tıklayın **Service Fabric** Service Fabric analizi çözümü devam etmek için bir grafik.

![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

Service Fabric analizi çözümü giriş sayfası aşağıdaki resimde gösterilmektedir. Bu giriş sayfası, kümenizin neler olduğunu bir anlık görüntü görünümü sağlar.

![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

 Küme oluşturma sırasında tanılama etkinleştirilirse, olayları görebilirsiniz. 

* [Service Fabric küme olayları](service-fabric-diagnostics-event-generation-operational.md)
* [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programlama modeline olayları](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>Ayrıntılı sistem olaylarını yanı sıra Service Fabric olayları hazır tarafından toplanabilir [tanılama uzantınızın yapılandırmayı güncelleştirerek](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

## <a name="view-service-fabric-events-including-actions-on-nodes"></a>Düğümlerde Eylemler dahil olmak üzere görünümü Service Fabric olayları

Grafik için Service Fabric analizi sayfasında tıklayarak **Service Fabric olayları**.

![Service Fabric çözüm işlevsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

Tıklayın **listesi** olayları bir listesini görüntülemek için. Bir kez toplanan tüm sistem olaylarını burada görürsünüz. Bunlar, başvuru için gelen **WADServiceFabricSystemEventsTable** Azure depolama hesabı ve benzer şekilde sonraki gördüğünüz güvenilir hizmetler ve actors olayları ilgili bu tablolardaki'dır.
    
![Sorgu işlevsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Alternatif olarak soldaki büyütece tıklayın ve aradığınız ne öğrenmek Kusto sorgu dili kullanın. Örneğin, kümedeki düğümler üzerinde gerçekleştirilen tüm eylemler bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay kimlikleri bulunan [işlevsel kanal etkinlik başvurusu](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Belirli düğümler (bilgisayar) sistem hizmeti (TaskName) gibi birçok diğer alanlar üzerinde sorgulama yapabilirsiniz.

## <a name="view-service-fabric-reliable-service-and-actor-events"></a>Görünüm Service Fabric güvenilir hizmeti ve aktör olayları

Service Fabric analizi sayfasında için grafiğe tıklayın **Reliable Services**.

![Service Fabric güvenilir hizmetler çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

Tıklayın **listesi** olayları bir listesini görüntülemek için. Burada, güvenilir hizmetler olayları görebilirsiniz. Hizmet runasync başlatıldığında ve hangi dağıtımları ve yükseltmeleri genellikle gerçekleşir tamamlandı farklı olaylara görebilirsiniz. 

![Sorgu güvenilir hizmetler](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Güvenilir aktör olayları, benzer bir biçimde görüntülenebilir. Reliable actors için Ayrıntılı olayları yapılandırmak için değiştirmeniz gerekir `scheduledTransferKeywordFilter` yapılandırmasını tanılama uzantısı (aşağıda gösterilmiştir). Değerleri içinde bunlar için ayrıntıları [reliable actors olayları başvurusu](service-fabric-reliable-actors-diagnostics.md#keywords).

```json
"EtwEventSourceProviderConfiguration": [
                {
                    "provider": "Microsoft-ServiceFabric-Actors",
                    "scheduledTransferKeywordFilter": "1",
                    "scheduledTransferPeriod": "PT5M",
                    "DefaultEvents": {
                    "eventDestination": "ServiceFabricReliableActorEventTable"
                    }
                },
```

Kusto sorgu güçlü bir dildir. Başka bir değerli sorgu çalıştırabilir, en çok olayın hangi düğümleri ürettiğini bulmaktır. Aşağıdaki ekran görüntüsünde sorgu düğümü ve belirli bir hizmeti ile toplanan Service Fabric işletimsel olayları gösterir.

![Düğüm başına sorgu olayları](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Sonraki adımlar

* Yani performans sayaçlarını izleme altyapı etkinleştirmek için attıktan [Log Analytics aracısını ekleme](service-fabric-diagnostics-oms-agent.md). Aracı, performans sayaçlarını toplar ve bunları, mevcut bir çalışma alanına ekler.
* Azure İzleyici günlüklerine, şirket içi kümeleri için Azure İzleyici günlüklerine veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla [Internet erişimi olmayan bilgisayarları Log Analytics ağ geçidi'ni kullanarak Azure İzleyici günlüklerine bağlanma](../azure-monitor/platform/gateway.md).
* Yapılandırma [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olacak.
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri Azure İzleyici günlüklerine bir parçası olarak sunulmaktadır.
* Azure İzleyici günlüklerine ve hangi sunduğu daha ayrıntılı bir genel bakış edinme, okuma [Azure İzleyici günlüklerine nedir?](../operations-management-suite/operations-management-suite-overview.md).
