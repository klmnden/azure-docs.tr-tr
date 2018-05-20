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
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: b51f7dc43f390152b2b0be223541e381bbddd3c6
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="event-analysis-and-visualization-with-log-analytics"></a>Olay çözümleme ve görselleştirme günlük analizi

Günlük analizi, OMS (Operations Management Suite) olarak da bilinen, izleme ve tanılama uygulamaları için yardımcı Yönetim Hizmetleri ve bulutta barındırılan hizmetleri koleksiyonudur. Bu makalede Öngörüler elde edin ve kümenizdeki neler olduğunu sorun giderme için günlük analizi sorguları çalıştırmak nasıl özetlenmektedir. Aşağıdaki sık sorulan ele alınmıştır:

* Sistem durumu olayları nasıl sorun giderme?
* Ne zaman bir düğüm arıza nasıl anlarım?
* My uygulamanın Hizmetleri başlatıldığı veya durdurulduğu nasıl anlarım?

## <a name="log-analytics-workspace"></a>Log Analytics çalışma alanı

Günlük analizi bir Azure depolama tablo veya bir aracı da dahil olmak üzere yönetilen kaynaklardan veri toplayan ve merkezi bir depoya tutar. Veriler, ardından analiz, uyarı ve görselleştirme için kullanılan ya da daha fazla verme olabilir. Günlük analizi olayları, performans verileri ya da herhangi bir özel veri destekler. Kullanıma [olayları tanılama uzantısını yapılandırma adımları](service-fabric-diagnostics-event-aggregation-wad.md) ve [depolama olayları okumak için bir günlük analizi çalışma alanı oluşturmak için adımları](service-fabric-diagnostics-oms-setup.md) veri günlük analizi akan emin olmak için .

Günlük analizi tarafından alınan veri sonra Azure birkaç sahip *yönetim çözümleri* birkaç senaryo için özelleştirilmiş gelen verileri izlemek için paketlenmiş çözümleri bulunur. Bunlar bir *Service Fabric Analytics* çözüm ve *kapsayıcıları* iki en uygun olanlardır tanılama ve Service Fabric kümeleri kullanırken izleme çözümü. Bu makalede, çalışma alanıyla oluşturulan Service Fabric analiz çözümü kullanmayı açıklar.

## <a name="access-the-service-fabric-analytics-solution"></a>Service Fabric analiz çözümü erişim

1. Service Fabric analiz çözümü oluşturduğunuz kaynak grubuna gidin. Kaynak Seç **ServiceFabric\<nameOfOMSWorkspace\>**  ve kendi genel bakış sayfasına gidin.

2. Genel bakış sayfasında ilk OMS Portalı'na gitmek için bağlantıya tıklayın

    ![OMS portalı bağlantı](media/service-fabric-diagnostics-event-analysis-oms/oms-portal-link.png)

3. Şimdi OMS portalında olduğunuz ve etkinleştirdiğiniz çözümleri görebilirsiniz. Service Fabric başlıklı grafiğe sağ tıklayın (aşağıdaki ilk görüntü) Service Fabric çözüme gerçekleştirilecek için (aşağıdaki ikinci görüntü)

    ![OMS BT çözümü](media/service-fabric-diagnostics-event-analysis-oms/oms-workspace-all-solutions.png)

    ![OMS BT çözümü](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-new.png)

Yukarıdaki resimde Service Fabric analiz çözümü giriş sayfasıdır. Neler olduğuna dair kümenizdeki bir anlık görüntü görünüm budur. Küme oluşturma tanılama etkinleştirilirse, olayları görebilirsiniz. 

* [İşletimsel kanal](service-fabric-diagnostics-event-generation-operational.md): Service Fabric platformundan (sistem hizmetler koleksiyonu) gerçekleştirir üst düzey işlem.
* [Reliable Actors programlama modelini olayları](service-fabric-reliable-actors-diagnostics.md)
* [Programlama modeli olaylarının güvenilir hizmetler](service-fabric-reliable-services-diagnostics.md)

>[!NOTE]
>İşletimsel kanal ek olarak, daha ayrıntılı sistem olayları tarafından toplanabilecek [tanılama uzantısını config güncelleştiriliyor](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)

### <a name="view-operational-events-including-actions-on-nodes"></a>Düğümlerde Eylemler dahil olmak üzere işletimsel olaylarını görüntüle

1. OMS portalı Service Fabric Analytics sayfasında grafik işlemsel kanal için tıklayın

    ![OMS BT çözüm işletimsel kanal](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-new-operational.png)

2. Tabloyu bir listede olayları görüntülemek için tıklatın. Bir kez Burada, toplanan tüm sistem olayları görebilirsiniz. Başvuru için bu Azure depolama hesabında WADServiceFabricSystemEventsTable arasındadır ve benzer şekilde güvenilir hizmetler ve sonraki gördüğünüz aktörler olayları ilgili bu tablolardaki.
    
    ![OMS sorgu işletimsel kanal](media/service-fabric-diagnostics-event-analysis-oms/oms-query-operational-channel.png)

Alternatif olarak soldaki Büyüteç'i tıklatın ve Kusto sorgu dili aradığınızı bulmak için kullanın. Örneğin, kümedeki düğümler üzerinde gerçekleştirilen tüm eylemler bulmak için aşağıdaki sorguyu kullanabilirsiniz. Aşağıda kullanılan olay kimlikleri bulunan [işletimsel kanal Olay Başvurusu](service-fabric-diagnostics-event-generation-operational.md)

```kusto
ServiceFabricOperationalEvent
| where EventId < 25627 and EventId > 25619 
```

Belirli düğümler (bilgisayar) sistem hizmeti (görevadı) gibi pek çok fazla alanda sorgulayabilirsiniz.

### <a name="view-service-fabric-reliable-service-and-actor-events"></a>Görünüm doku güvenilir hizmeti ve aktör olayları

1. OMS portalı Service Fabric Analytics sayfasında güvenilir hizmetler için grafiği tıklatın

    ![OMS BT çözüm güvenilir hizmetler](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics-reliable-services.png)

2. Tabloyu bir listede olayları görüntülemek için tıklatın. Burada güvenilir hizmetler olayları görebilirsiniz. Hizmet runasync başlatıldığında ve hangi dağıtım ve yükseltme işlemleri genellikle olur tamamlandı için farklı olayları görebilirsiniz. 

    ![Güvenilir hizmetler OMS sorgulama](media/service-fabric-diagnostics-event-analysis-oms/oms-query-reliable-services.png)

Güvenilir aktör olayları, benzer bir biçimde görüntülenebilir. Güvenilir aktörler için Ayrıntılı olayları yapılandırmak için değiştirmeniz gerekir `scheduledTransferKeywordFilter` (aşağıda gösterilen) tanılama uzantısı yapılandırmada. Bunlar için değerlerine ayrıntıları [güvenilir aktörler Olay Başvurusu](service-fabric-reliable-actors-diagnostics.md#keywords)

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

Kusto sorgu dili güçlüdür. Başka bir değerli sorgu çalıştırabilirsiniz, çoğu olayları hangi düğümlerin oluşturduğunu bulmaktır. Aşağıdaki ekran görüntüsünde sorgu düğümü ve belirli bir hizmeti ile bir araya getirilir güvenilir hizmetler olay gösterir

![Düğüm başına OMS sorgu olayları](media/service-fabric-diagnostics-event-analysis-oms/oms-query-events-per-node.png)

## <a name="next-steps"></a>Sonraki adımlar

* Altyapı yani performans sayaçlarını izleme etkinleştirmek için üzerinden için head [OMS aracısı ekleme](service-fabric-diagnostics-oms-agent.md). Aracı performans sayaçlarını toplar ve bunları mevcut çalışma alanına ekler.
* Şirket içi kümeleri için OMS için OMS veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [Internet erişimi olmayan bilgisayarlar için OMS OMS ağ geçidini kullanarak bağlanma](../log-analytics/log-analytics-oms-gateway.md)
* Ayarlamak için OMS yapılandırma [uyarı otomatik](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* Günlük analizi ve ne sunar daha ayrıntılı bir bakış elde, okuma [günlük analizi nedir?](../operations-management-suite/operations-management-suite-overview.md)
