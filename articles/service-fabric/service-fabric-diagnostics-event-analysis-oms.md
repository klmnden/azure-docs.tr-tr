---
title: Log Analytics ile Azure Service Fabric olay analizi | Microsoft Docs
description: Log Analytics, izleme ve Tanılama, Azure Service Fabric kümeleri kullanarak olayları analiz ve görselleştirme hakkında bilgi edinin.
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
ms.date: 05/29/2018
ms.author: srrengar
ms.openlocfilehash: 6dee895ba9fc024baac0500619b7d6cc62167b6d
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49404486"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Olay çözümleme ve görselleştirme Log Analytics ile
Log Analytics toplar ve uygulamalardan ve bulut üzerinde barındırılan hizmetlerden daha fazla telemetri analiz eder ve kullanılabilirliği ve performansı en üst düzeye çıkarmanıza yardımcı olması için analiz araçları sağlar. Bu makalede, Öngörüler ve neler kümenizde sorun giderme için Log analytics'te sorgu çalıştırma açıklanmaktadır. Aşağıdaki yaygın sorular ele alınmıştır:

* Sistem durumu olaylarını sorunlarını nasıl giderebilirim?
* Ne zaman bir düğüm arıza olduğunu nasıl öğrenebilirim?
* My uygulamanın hizmetlerine başlatıldığı veya durdurulduğu nasıl anlarım?

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı

Log Analytics, Azure depolama tablosu veya bir aracı gibi yönetilen kaynaklardan veri toplar ve merkezi bir depoya tutar. Veriler daha sonra analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla dışa aktarma olabilir. Log Analytics, olaylar, performans verisi veya herhangi bir özel veri destekler. Kullanıma [olayları tanılama uzantısını yapılandırma adımları](service-fabric-diagnostics-event-aggregation-wad.md) ve [depolama olayları okumak için bir Log Analytics çalışma alanı oluşturmak için adımları](service-fabric-diagnostics-oms-setup.md) verileri Log Analytics'e akan emin olmak için .

Verilerin Log Analytics tarafından alındıktan sonra Azure vardır *yönetim çözümleri* birkaç senaryo için özelleştirilmiş, gelen verileri izlemek için önceden paketlenmiş çözümleri bulunmaktadır. Bunlar bir *Service Fabric analizi* çözüm ve *kapsayıcıları* tanılama ve Service Fabric kümeleri kullanılırken izleme iki en uygun ayarlara olan çözüm. Bu makalede, çalışma alanı ile oluşturulmuş Service Fabric analizi çözümü kullanmayı açıklar.

## <a name="access-the-service-fabric-analytics-solution"></a>Service Fabric analizi çözümü erişim

1. Azure Portalı'nda, Service Fabric analizi çözümü oluşturduğunuz kaynak grubuna gidin.

2. Kaynak seçin **ServiceFabric\<nameOfOMSWorkspace\>**.

2. Özet olarak, her bir Service Fabric için de dahil olmak üzere etkin çözüm için bir grafik biçiminde kutucuklar görürsünüz. Tıklayın **Service Fabric** graf (aşağıdaki ilk görüntü) Service Fabric analizi çözümü devam etmek için (aşağıdaki ikinci resim).

    ![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

Yukarıdaki resimde Service Fabric analizi çözümü, giriş sayfasıdır. Kümenizde neler olduğunu bir anlık görüntü görünümü budur. Küme oluşturma sırasında tanılama etkinleştirilirse, olayları görebilirsiniz. 

* [İşlevsel kanal](service-fabric-diagnostics-event-generation-operational.md): Service Fabric platform (Sistem Hizmetleri koleksiyonunu) gerçekleştirir daha üst düzey işlem.
* [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
* [Reliable Services programlama modeline olayları](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>İşlevsel kanal yanı sıra ayrıntılı sistem olaylarını tarafından toplanabilir [tanılama uzantınızın yapılandırmayı güncelleştirerek](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Görünüm Service Fabric düğümlerinde Eylemler dahil olmak üzere olayları

1. Grafik için Service Fabric analizi sayfasında tıklayarak **Service Fabric olayları**.

    ![Service Fabric çözüm işlevsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Tıklayın **listesi** olayları bir listesini görüntülemek için. Bir kez toplanan tüm sistem olaylarını burada görürsünüz. Başvuru için bu Azure depolama hesabındaki WADServiceFabricSystemEventsTable arasındadır ve benzer şekilde güvenilir hizmetler ve ardından gördüğünüz actors olayları ilgili bu tablolardaki.
    
    ![Sorgu işlevsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Alternatif olarak soldaki büyütece tıklayın ve aradığınız ne öğrenmek Kusto sorgu dili kullanın. Örneğin, kümedeki düğümler üzerinde gerçekleştirilen tüm eylemler bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay kimlikleri bulunan [işlevsel kanal etkinlik başvurusu](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Belirli düğümler (bilgisayar) sistem hizmeti (TaskName) gibi birçok diğer alanlar üzerinde sorgulama yapabilirsiniz.

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Görünüm Service Fabric güvenilir hizmeti ve aktör olayları

1. Service Fabric analizi sayfasında için grafiğe tıklayın **Reliable Services**.

    ![Service Fabric güvenilir hizmetler çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Tıklayın **listesi** olayları bir listesini görüntülemek için. Burada, güvenilir hizmetler olayları görebilirsiniz. Hizmet runasync başlatıldığında ve hangi dağıtımları ve yükseltmeleri genellikle gerçekleşir tamamlandı farklı olaylara görebilirsiniz. 

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
* Şirket içi kümeleri için Log Analytics verilerini Log Analytics'e göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla [Internet erişimi olmayan bilgisayarları Log Analytics ağ geçidini kullanarak Log Analytics'e bağlanma](../log-analytics/log-analytics-oms-gateway.md).
* Yapılandırma [otomatik uyarı verme](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olacak.
* Log Analytics’in bir parçası olarak sunulan [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri hakkında bilgi sahibi olun.
* Log Analytics ve hangi sunduğu daha ayrıntılı bir genel bakış edinme, okuma [Log Analytics nedir?](../operations-management-suite/operations-management-suite-overview.md).
