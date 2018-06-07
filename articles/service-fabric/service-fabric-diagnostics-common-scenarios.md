---
title: Azure Service Fabric tanılamak yaygın senaryolar | Microsoft Docs
description: Yaygın senaryolar Azure Service Fabric ile ilgili sorunları giderme öğrenin
services: service-fabric
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/16/2018
ms.author: srrengar
ms.openlocfilehash: bd7a7e0288ced0219a0600034b273d1acba6b09b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655821"
---
# <a name="diagnose-common-scenarios-with-service-fabric"></a>Service Fabric ile yaygın senaryolar tanılama

Bu makalede izleme ve tanılama Service Fabric ile alanı kullanıcılar karşılaşmış yaygın senaryolar gösterilmektedir. Service fabric tüm 3 katmanlı sunulan senaryolarını kapsamak: uygulama, küme ve altyapı. Her bir çözüm, her senaryonun tamamlanması için Application Insights ve günlük analizi, Azure izleme araçları kullanır. Her çözüm adımları kullanıcılar Application Insights ve günlük analizi Service Fabric bağlamında kullanma hakkında giriş bilgileri sağlar.

## <a name="prerequisites-and-recommendations"></a>Önkoşullar ve öneriler

Bu makalede çözümleri aşağıdaki araçları kullanır. Bu ayarla yukarı ve yapılandırılmış olması önerilir:

* [Service Fabric Application Insights'a](service-fabric-tutorial-monitoring-aspnet.md)
* [Azure tanılama kümenizde etkinleştir](service-fabric-diagnostics-event-aggregation-wad.md)
* [Bir OMS günlük analizi çalışma alanı ayarlayıp](service-fabric-diagnostics-oms-setup.md)
* [Performans sayaçlarını izlemek için OMS Aracısı](service-fabric-diagnostics-oms-agent.md)

## <a name="how-can-i-see-unhandled-exceptions-in-my-application"></a>İşlenmeyen özel durumlar uygulamamda nasıl görebilirim?

1. Uygulamanız ile yapılandırılmış Application Insights kaynağınıza gidin.
2. Tıklayın *arama* üst sol. Filtre sonraki paneldeki'ye tıklayın.

    ![AI genel bakış](media/service-fabric-diagnostics-common-scenarios/ai-search-filter.png)

3. Çok sayıda olay (izlemeler, istekleri, özel olaylar) türleri görürsünüz. Filtre olarak "özel durum"'i seçin.

    ![AI filtresi listesi](media/service-fabric-diagnostics-common-scenarios/ai-filter-list.png)

    Bir özel durum listesinde tıklatarak Service Fabric Application Insights SDK kullanıyorsanız, hizmet bağlamı da dahil olmak üzere daha fazla bilgi görünebilir.

    ![AI özel durumu](media/service-fabric-diagnostics-common-scenarios/ai-exception.png)

## <a name="how-do-i-view-which-http-calls-are-used-in-my-services"></a>Ne ı görüntülemek HTTP çağrıları my hizmetlerinde kullanılır?

1. Aynı Application Insights kaynağını yerine özel durumlar "isteklerinde" filtre ve yapılan tüm istekleri görüntüleyin
2. Service Fabric Application Insights SDK kullanıyorsanız, birbirine bağlı hizmetlerinizi görsel gösterimi görebilir ve başarılı ve başarısız istek sayısı. Sol taraftaki "uygulama" Haritası

    ![AI uygulama harita dikey](media/service-fabric-diagnostics-common-scenarios/app-map-blade.png) ![AI uygulama eşleme](media/service-fabric-diagnostics-common-scenarios/app-map-new.png)

    Uygulama eşlemesi hakkında daha fazla bilgi için ziyaret [uygulama eşlemesi belgeleri](../application-insights/app-insights-app-map.md)

## <a name="how-do-i-create-an-alert-when-a-node-goes-down"></a>Bir düğüm azaldığında nasıl bir uyarı oluşturulur

1. Düğümü olaylarını Service Fabric kümesi tarafından izlenir. Adlı Service Fabric analiz çözümü kaynağa gidin **ServiceFabric(NameofResourceGroup)**
2. "Özet" başlıklı dikey pencerenin altındaki grafikte tıklayın

    ![OMS Çözümü](media/service-fabric-diagnostics-common-scenarios/oms-solution-azure-portal.png)

3. Burada, birçok grafikleri ve çeşitli ölçümleri görüntüleme döşeme vardır. Aşağıdakilerden grafikleri tıklatın ve işlemin günlük arama sürecek. Burada herhangi bir küme olayları veya performans sayaçları için sorgulayabilirsiniz.
4. Aşağıdaki sorgu girin. Bu olay kimlikleri bulunan [düğümü Olay Başvurusu](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId >= 25623 or EventId <= 25626
    ```

5. En üstünde "Yeni uyarı kuralı"'i tıklatın ve bir olay bu sorgu temel alınarak ulaşan verdiğinizde, artık bir uyarı iletişim seçtiğiniz yöntemi alırsınız.

    ![OMS yeni uyarı](media/service-fabric-diagnostics-common-scenarios/oms-create-alert.png)

## <a name="how-can-i-be-alerted-of-application-upgrade-rollbacks"></a>Nasıl t uygulama yükseltme düzeyine uyarısını alabilir?

1. Önce aynı günlük arama penceresinde yükseltme geri alma için aşağıdaki sorguyu girin. Bu olay kimlikleri altında bulunan [uygulama olayları başvurusu](service-fabric-diagnostics-event-generation-operational.md#application-events)

    ```kusto
    ServiceFabricOperationalEvent
    | where EventId == 29623 or EventId == 29624
    ```

2. En üstünde "Yeni uyarı kuralı"'i tıklatın ve bir olay bu sorgu temel alınarak ulaşan verdiğinizde, artık bir uyarı alırsınız.

## <a name="how-do-i-see-container-metrics"></a>Kapsayıcı ölçümleri nasıl görürüm?

Tüm grafik ile aynı görünümünde kapsayıcılarınızı performansını için bazı kutucuklar görürsünüz. OMS Aracısı gerekir ve [kapsayıcı izlemesi çözümü](service-fabric-diagnostics-oms-containers.md) doldurmak bu kutucuklar için.

![OMS kapsayıcı ölçümleri](media/service-fabric-diagnostics-common-scenarios/containermetrics.png)

>[!NOTE]
>Aracı telemetrisinden için **içinde** eklemeniz gerekiyor, kapsayıcı [kapsayıcıları için Application Insights nuget paketi](https://github.com/Microsoft/ApplicationInsights-servicefabric#microsoftapplicationinsightsservicefabric--for-service-fabric-lift-and-shift-scenarios).

## <a name="how-can-i-monitor-performance-counters"></a>Performans sayaçlarını nasıl izleyebilir mi?

1. Kümeniz için OMS aracısının ekledikten sonra izlemek istediğiniz belirli performans sayaçlarını eklemeniz gerekir. Çalışma alanı sekmesini sol menüde yer çözümün sayfasından portalında – OMS Workspace'in sayfaya gidin.

    ![OMS çalışma sekmesi](media/service-fabric-diagnostics-common-scenarios/workspacetab.png)

2. Çalışma Alanı'nın sayfasında olduğunuzda, aynı soldaki menüde "Gelişmiş ayarları" tıklayın.

    ![OMS Gelişmiş ayarlar](media/service-fabric-diagnostics-common-scenarios/advancedsettingsoms.png)

3. Verileri tıklayın > Windows performans sayaçlarını (veri > Linux makineler için Linux performans sayaçları) özel sayaçlar, düğümlerden OMS Aracısı üzerinden toplamaya başlamak için. Örnekler eklemek sayaçları için biçimi

    * `.NET CLR Memory(<ProcessNameHere>)\\# Total committed Bytes`
    * `Processor(_Total)\\% Processor Time`
    * `Service Fabric Service(*)\\Average milliseconds per request`

    Bu sayaçlar izleme gibi görünür şekilde başlangıcın VotingData ve VotingWeb kullanıldığında, işlem adlarıdır

    * `.NET CLR Memory(VotingData)\\# Total committed Bytes`
    * `.NET CLR Memory(VotingWeb)\\# Total committed Bytes`

    ![OMS performans sayaçları](media/service-fabric-diagnostics-common-scenarios/omsperfcounters.png)

4. Bu altyapınızı iş yüklerinizi nasıl işlediğinin görmenize olanak veren ve kaynak kullanımı temelinde ilgili uyarıları ayarlayın. Örneğin – toplam işlemci kullanımı % 90 üstüne veya altına %5 kalırsa bir uyarı ayarlamak isteyebilirsiniz. Bunun için kullanacağınız sayaç "% işlemci zamanı" adıdır Aşağıdaki sorgu için bir uyarı kuralı oluşturarak bunu yapabilirsiniz:

    ```kusto
    Perf | where CounterName == "% Processor Time" and InstanceName == "_Total" | where CounterValue >= 90 or CounterValue <= 5.
    ```

## <a name="how-do-i-track-performance-of-my-reliable-services-and-actors"></a>Güvenilir hizmetler ve aktörler performansını izlemek nasıl?

Uygulamalarınızda güvenilir hizmetler veya aktörler performansını izlemek için Service Fabric aktör, aktör yöntemi, hizmet ve hizmet yöntemi sayaçları de eklemeniz gerekir. Bu sayaçlar senaryo yukarıdaki benzer bir biçimde ekleyebilirsiniz, örnekler OMS eklemek için güvenilir hizmeti ve aktör performans sayaçları

* `Service Fabric Service(*)\\Average milliseconds per request`
* `Service Fabric Service Method(*)\\Invocations/Sec`
* `Service Fabric Actor(*)\\Average milliseconds per request`
* `Service Fabric Actor Method(*)\\Invocations/Sec`

Güvenilir performans sayaçlarını tam listesi için bu bağlantıları kontrol [Hizmetleri](service-fabric-reliable-serviceremoting-diagnostics.md) ve [aktörler](service-fabric-reliable-actors-diagnostics.md)

## <a name="next-steps"></a>Sonraki adımlar

* [AI uyarıları ayarlama](../application-insights/app-insights-alerts.md) performans veya kullanımı değişiklikler hakkında bildirim almak için
* [Akıllı algılama Application ınsights'ta](../application-insights/app-insights-proactive-diagnostics.md) AI için olası performans sorunları sizi uyaracak şekilde telemetri gönderimini öngörülü bir analizini gerçekleştirir
* OMS günlük analizi hakkında daha fazla bilgi [uyarı](../log-analytics/log-analytics-alerts.md) algılama ve tanılama yardımcı olmak için.
* Şirket içi kümeleri için OMS için OMS veri göndermek için kullanılan bir ağ geçidi (HTTP İleri Proxy) sunar. Uygulamasında hakkında daha fazla bilgi [Internet erişimi olmayan bilgisayarlar için OMS OMS ağ geçidini kullanarak bağlanma](../log-analytics/log-analytics-oms-gateway.md)
* İle familiarized [günlük arama ve sorgulama](../log-analytics/log-analytics-log-searches.md) günlük analizi bir parçası olarak sunulan özellikler
* Günlük analizi ve ne sunar daha ayrıntılı bir bakış elde, okuma [günlük analizi nedir?](../operations-management-suite/operations-management-suite-overview.md)
