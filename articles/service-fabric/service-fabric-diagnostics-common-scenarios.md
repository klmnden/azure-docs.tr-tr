---
title: Azure Service Fabric'e tanılama senaryoları | Microsoft Docs
description: Yaygın senaryoları Azure Service Fabric ile ilgili sorunları giderme hakkında bilgi edinin
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
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 265aea1b8873d812859b39175c732c3e7118cbb5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394257"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Yaygın senaryolar ile Service Fabric'i tanılama

Bu makalede izleme ve Tanılama ile Service Fabric'i alanında kullanıcıların karşılaştığı yaygın senaryolar gösterilmektedir. Service fabric'in tüm 3 katmanları sunulan senaryoları kapsar: Uygulama, küme ve altyapı. Her çözüm her senaryonun tamamlanması için izleme araçları, Azure Application Insights ve Azure İzleyici günlüklerine kullanır. Her çözüm adımları kullanıcılar Application Insights'ı kullanma hakkında giriş bilgileri sağlar ve Azure İzleyici, Service Fabric bağlamında günlüğe kaydeder.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites-and-recommendations"></a>Önkoşullar ve öneriler

Bu makalede çözümleri aşağıdaki araçları kullanır. Bu küme up ve yapılandırılmış olması önerilir:

* [Application Insights ile Service Fabric](service-fabric-tutorial-monitoring-aspnet.md)
* [Kümenizde Azure tanılamayı etkinleştirme](service-fabric-diagnostics-event-aggregation-wad.md)
* [Bir Log Analytics çalışma alanını ayarlama](service-fabric-diagnostics-oms-setup.md)
* [Analytics aracısını performans sayaçlarını izlemek için oturum açın](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>İşlenmeyen özel durumları uygulamamda nasıl görebilirim?

1. Uygulamanız ile yapılandırılmış Application Insights kaynağınıza gidin.
2. Tıklayarak *arama* sol üstteki. Ardından sonraki panelinde filtreye tıklayın.

    ![Yapay ZEKA genel bakış](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Çok sayıda olay (izlemeleri, istekler, özel olaylar) türleri görürsünüz. Filtreniz olarak "özel durum" seçin.

    ![Yapay ZEKA filtresi listesi](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Bir özel durum listesinde tıklayarak, Service Fabric Application Insights SDK kullanıyorsanız Hizmet bağlamı da dahil olmak üzere daha fazla bilgi bakabilirsiniz.

    ![Yapay ZEKA özel durumu](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Nasıl HTTP görüntüleyebilirim çağrıları Hizmetlerim içinde kullanılır?

1. Aynı Application Insights kaynağını "isteklerinde" yerine özel durumları filtrelemek ve yapılan tüm istekleri görüntüle
2. Service Fabric Application Insights SDK kullanıyorsanız, birbirine bağlı hizmetlerinizi görsel bir temsilini görebilir ve başarılı ve başarısız istek sayısı. Sol taraftaki "Uygulama Haritası"'a tıklayın.

    ![Yapay ZEKA Uygulama Haritası dikey](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI Uygulama Haritası](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Uygulama Haritası hakkında daha fazla bilgi için ziyaret [Uygulama Haritası belgeleri](../azure-monitor/app/app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Bir düğümü arızalandığında uyarı nasıl oluşturabilirim

1. Düğüm olayları, Service Fabric kümeniz tarafından izlenir. Adlı Service Fabric analizi çözümü kaynağına gidin **ServiceFabric(NameofResourceGroup)**
2. Graf üzerinde "Özet" başlıklı dikey pencerenin altında tıklayın

    ![Azure İzleyici çözüm günlüğe kaydeder.](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Burada, birçok grafikleri ve çeşitli ölçümleri görüntüleme kutucukları vardır. Grafikler birine tıklayın ve günlük araması için sizi yönlendirir. Burada herhangi bir küme olayları veya performans sayaçları için sorgulayabilirsiniz.
4. Aşağıdaki sorguyu girin. Bu olay kimlikleri bulunan [düğümü etkinlik başvurusu](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID >= 25622 and EventID <= 25626
    ```

5. Üst kısmında "Yeni uyarı kuralı" tıklayın ve bu sorgu tabanlı bir olay ulaşan her zaman, artık bir uyarı iletişim seçtiğiniz yöntemi elde edersiniz.

    ![Azure İzleyici yeni uyarı günlüğe kaydeder](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Nasıl miyim uygulama yükseltme düzeyine uyarısını alabilir?

1. Önce aynı günlük araması penceresini yükseltme geri alma işlemleri için aşağıdaki sorguyu girin. Bu olay kimlikleri altında bulunan [uygulama olayları başvurusu](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventID == 29623 or EventID == 29624
    ```

2. Üst kısmında "Yeni uyarı kuralı" tıklayın ve artık bu sorgu tabanlı bir olay ulaşan her zaman bir uyarı alırsınız.

## <a name="how-do-i-see-container-metrics"></a>Kapsayıcı ölçümleri nasıl görebilirim?

Tüm grafikler ile aynı görünümde kapsayıcılarınızı performansı için bazı kutucuklar görürsünüz. Log Analytics aracısını gerekir ve [kapsayıcı izleme çözümü](service-fabric-diagnostics-oms-containers.md) doldurmak bu kutucukları için.

![Log Analytics kapsayıcı ölçümleri](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Aleti telemetriye **içinde** eklemeniz gerekir, kapsayıcınızın [kapsayıcılar için Application Insights nuget paketi](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>Performans sayaçlarını nasıl izleyebilirim?

1. Kümeniz için Log Analytics aracısını ekledikten sonra izlemek istediğiniz belirli bir performans sayaçları eklemeniz gerekir. Çalışma alanı sekmesindeki sol taraftaki menüde olduğu çözümün sayfasından – portalda Log Analytics çalışma alanınızın sayfasına gidin.

    ![Log Analytics çalışma alanı sekmesi](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Çalışma alanınızın sayfasında başladığınızda, aynı sol menüdeki "Gelişmiş ayarları"'a tıklayın.

    ![Log Analytics Gelişmiş ayarlar](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Veri tıklayın > Windows performans sayaçları (veri > Linux makineler için Linux performans sayaçlarıyla) belirli sayaçları Log Analytics aracısını, düğümlerden toplamaya başlamak için. Sayaçlar eklemek için biçim örnekleri aşağıda verilmiştir

   * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
   * `Processor(_Total)\\% Processor Time`

     Bu sayaçlardan izleme şöyle görünmelidir hızlı başlangıçta, VotingData ve VotingWeb kullanıldığında, işlem adları olduğundan

   * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
   * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

     ![Log Analytics performans sayaçları](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Bu iş yüklerinizi altyapınızı nasıl işlediğinin görmenize olanak veren ve kaynak kullanımı temelinde ilgili uyarılar ayarlayın. Örneğin: toplam işlemci kullanımı % 90 üzerinde veya %5 altında olursa uyarı ayarlamak isteyebilirsiniz. "% İşlemci süresi", bunun için kullanacağınız sayaç adı olan Aşağıdaki sorgu için bir uyarı kuralı oluşturarak bunu yapabilirsiniz:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Reliable Services ve aktörler performansını nasıl izleyebilir?

Uygulamalarınızda Reliable Services veya aktörler performansını izlemek için de Service Fabric aktör, aktör yöntemi, hizmet ve hizmet yöntemi sayaçları toplamanız gerekir. Toplanacak güvenilir hizmeti ve aktör performans sayaçları örnekleri aşağıda verilmiştir

>[!NOTE]
>Service Fabric performans sayaçları Log Analytics aracısı tarafından şu anda toplanamıyor ancak tarafından toplanan [tanılama diğer çözümleri](service-fabric-diagnostics-partners.md)

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Güvenilir performans sayaçlarını tam listesi için bu bağlantıları kontrol [Hizmetleri](service-fabric-reliable-serviceremoting-diagnostics.md) ve [aktörler](service-fabric-reliable-actors-diagnostics.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Yapay ZEKA uyarıları ayarlama](../azure-monitor/app/alerts.md) performans ya da kullanım değişiklikler hakkında bildirim almak için
* [Akıllı algılama Application ınsights'ta](../azure-monitor/app/proactive-diagnostics.md) yapay ZEKA, olası performans sorunları sizi uyarabilmek için gönderilen telemetri bir öngörülü analiz gerçekleştirir
* Azure İzleyici günlüklerine hakkında daha fazla bilgi [uyarı](../log-analytics/log-analytics-alerts.md) algılama ve tanılama konusunda yardımcı olacak.
* Azure İzleyici günlüklerine, şirket içi kümeleri için Azure İzleyici günlüklerine veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla [Internet erişimi olmayan bilgisayarları Log Analytics ağ geçidi'ni kullanarak Azure İzleyici günlüklerine bağlanma](../azure-monitor/platform/gateway.md)
* Analytics'in [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) özellikleri, Azure İzleyici günlüklerine bir parçası olarak sunulan
* Azure İzleyici günlüklerine ve hangi sunduğu daha ayrıntılı bir genel bakış edinme, okuma [Azure İzleyici günlüklerine nedir?](../operations-management-suite/operations-management-suite-overview.md)
