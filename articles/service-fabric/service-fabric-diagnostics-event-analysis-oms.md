---
title: Günlük analizi ile Azure Service Fabric olay çözümleme | Microsoft Docs
description: Görselleştirme ve izleme ve tanılama Azure Service Fabric kümeleri için günlük analizi kullanarak olayları analiz etme hakkında bilgi edinin.
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
ms.openlocfilehash: 49d9b5306a0fcf51cc0de036c725fca8345cd0ec
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36302191"
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Olay çözümleme ve görselleştirme günlük analizi
Günlük analizi toplar ve uygulamalardan ve bulutta barındırılan hizmetleri telemetri analiz eder ve bunların kullanılabilirliğini ve performansını en üst düzeye çıkarmanıza yardımcı olmak için analiz araçları sağlar. Bu makalede Öngörüler elde edin ve kümenizdeki neler olduğunu sorun giderme için günlük analizi sorguları çalıştırmak nasıl özetlenmektedir. Aşağıdaki sık sorulan ele alınmıştır:

* Sistem durumu olayları nasıl sorun giderme?
* Ne zaman bir düğüm arıza nasıl anlarım?
* My uygulamanın Hizmetleri başlatıldığı veya durdurulduğu nasıl anlarım?

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı

Günlük analizi bir Azure depolama tablo veya bir aracı da dahil olmak üzere yönetilen kaynaklardan veri toplayan ve merkezi bir depoya tutar. Veriler, ardından analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla verme olabilir. Günlük analizi olayları, performans verileri ya da herhangi bir özel veri destekler. Kullanıma [olayları tanılama uzantısını yapılandırma adımları](service-fabric-diagnostics-event-aggregation-wad.md) ve [depolama olayları okumak için bir günlük analizi çalışma alanı oluşturmak için adımları](service-fabric-diagnostics-oms-setup.md) veri günlük analizi akan emin olmak için .

Günlük analizi tarafından alınan veri sonra Azure birkaç sahip *yönetim çözümleri* birkaç senaryo için özelleştirilmiş gelen verileri izlemek için paketlenmiş çözümleri bulunur. Bunlar bir *Service Fabric Analytics* çözüm ve *kapsayıcıları* iki en uygun olanlardır tanılama ve Service Fabric kümeleri kullanırken izleme çözümü. Bu makalede, çalışma alanıyla oluşturulan Service Fabric analiz çözümü kullanmayı açıklar.

## <a name="access-the-service-fabric-analytics-solution"></a>Service Fabric analiz çözümü erişim

1. Azure Portalı'nda, Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin.

2. Kaynak Seç **ServiceFabric\<nameOfOMSWorkspace\>**.

2. Özet olarak, her biri için Service Fabric dahil olmak üzere etkin çözümleri için bir grafik biçiminde kutucuklar görürsünüz. Tıklatın **Service Fabric** Service Fabric analiz çözümü devam etmek için (aşağıdaki ilk görüntü) grafik (aşağıdaki ikinci görüntü).

    ![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_summary.PNG)

    ![Service Fabric çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_solution.PNG)

Yukarıdaki resimde Service Fabric analiz çözümü giriş sayfasıdır. Neler olduğuna dair kümenizdeki bir anlık görüntü görünüm budur. Küme oluşturma tanılama etkinleştirilirse, olayları görebilirsiniz. 

* [İşletimsel kanal](service-fabric-diagnostics-event-generation-operational.md): Service Fabric platformundan (sistem hizmetler koleksiyonu) gerçekleştirir üst düzey işlem.
* [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
* [Programlama modeli olaylarının güvenilir hizmetler](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>İşletimsel kanal ek olarak, daha ayrıntılı sistem olayları tarafından toplanabilecek [tanılama uzantısını config güncelleştirme](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations).

### <a name="view-service-fabric-events-including-actions-on-nodes"></a>Service Fabric düğümlerinde Eylemler dahil olmak üzere olaylarını görüntüle

1. Grafik için Service Fabric Analytics sayfasında tıklayın **Service Fabric olayları**.

    ![Service Fabric çözüm işletimsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events_selection.png)

2. Tıklatın **listesi** olayları bir listesini görüntülemek için. Bir kez Burada, toplanan tüm sistem olayları görebilirsiniz. Başvuru için bu Azure depolama hesabında WADServiceFabricSystemEventsTable arasındadır ve benzer şekilde güvenilir hizmetler ve sonraki gördüğünüz aktörler olayları ilgili bu tablolardaki.
    
    ![Sorgu işletimsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms_service_fabric_events.png)

Alternatif olarak soldaki Büyüteç'i tıklatın ve Kusto sorgu dili aradığınızı bulmak için kullanın. Örneğin, kümedeki düğümler üzerinde gerçekleştirilen tüm eylemler bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay kimlikleri bulunan [işletimsel kanal olay başvurusu](service-fabric-diagnostics-event-generation-operational.md).

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Belirli düğümler (bilgisayar) sistem hizmeti (görevadı) gibi pek çok fazla alanda sorgulayabilirsiniz.

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Görünüm doku güvenilir hizmeti ve aktör olayları

1. Grafik için Service Fabric Analytics sayfasında, tıklatın **Reliable Services**.

    ![Service Fabric çözüm güvenilir hizmetler](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_services_events_selection.png)

2. Tıklatın **listesi** olayları bir listesini görüntülemek için. Burada güvenilir hizmetler olayları görebilirsiniz. Hizmet runasync başlatıldığında ve hangi dağıtım ve yükseltme işlemleri genellikle olur tamamlandı için farklı olayları görebilirsiniz. 

    ![Sorgu güvenilir hizmetler](media/service-fabric-diagnostics-event-analysis-oms/oms_reliable_service_events.png)

Güvenilir aktör olayları, benzer bir biçimde görüntülenebilir. Güvenilir aktörler için Ayrıntılı olayları yapılandırmak için değiştirmeniz gerekir `scheduledTransferKeywordFilter` (aşağıda gösterilen) tanılama uzantısı yapılandırmada. Bunlar için değerlerine ayrıntıları [güvenilir aktörler olay başvurusu](service-fabric-reliable-actors-diagnostics.md#keywords).

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

Kusto sorgu dili güçlüdür. Başka bir değerli sorgu çalıştırabilirsiniz, çoğu olayları hangi düğümlerin oluşturduğunu bulmaktır. Aşağıdaki ekran görüntüsünde sorgu düğümü ve belirli bir hizmeti ile bir araya getirilir Service Fabric çalışma olaylarını gösterir.

![Düğüm başına sorgu olayları](media/service-fabric-diagnostics-event-analysis-oms/oms_kusto_query.png)

## <a name="next-steps"></a>Sonraki adımlar

* Altyapı yani performans sayaçlarını izleme etkinleştirmek için üzerinden için head [günlük analizi aracısı ekleme](service-fabric-diagnostics-oms-agent.md). Aracı performans sayaçlarını toplar ve bunları mevcut çalışma alanına ekler.
* Şirket içi kümeleri için günlük analizi için günlük analizi veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [günlük OMS ağ geçidini kullanma analizi için Internet erişimi olmayan bilgisayarları bağlama](../log-analytics/log-analytics-oms-gateway.md)
* Yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* Günlük analizi ve ne sunar daha ayrıntılı bir bakış elde, okuma [günlük analizi nedir?](../operations-management-suite/operations-management-suite-overview.md)
