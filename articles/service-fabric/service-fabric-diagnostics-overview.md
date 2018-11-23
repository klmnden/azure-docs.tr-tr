---
title: Azure Service Fabric izleme ve tanılamaya genel bakış | Microsoft Docs
description: Azure Service Fabric kümelerini, uygulamalar ve hizmetler için izleme ve tanılamayı öğrenin.
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
ms.date: 11/21/2018
ms.author: srrengar
ms.openlocfilehash: 82c02c0212fd79d8847d374022b6ac8f862f042a
ms.sourcegitcommit: beb4fa5b36e1529408829603f3844e433bea46fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2018
ms.locfileid: "52291122"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Azure Service Fabric için izleme ve tanılama

Bu makalede, Azure Service Fabric için izleme ve Tanılama hakkında genel bir bakış sağlar. İzleme ve tanılama geliştirme, test ve herhangi bir bulut ortamında iş yüklerini dağıtma için kritik öneme sahiptir. Örneğin, uygulamalarınızı, Service Fabric platform, performans sayaçları ile kaynak kullanımınızı ve kümenizin genel sistem tarafından gerçekleştirilen eylemler kullanımını izleyebilirsiniz. Bu bilgiler, tanılamak ve sorunları düzeltin ve gelecekte meydana gelmesini önlemek için kullanabilirsiniz. Sonraki birkaç bölümler her alanı, Service Fabric üretim iş yükleri için dikkate alınması gereken izleme kısaca açıklanmaktadır. 

## <a name="application-monitoring"></a>Uygulama izleme
Uygulama İzleme özelliklerini ve bileşenlerini, uygulamanızın nasıl kullanıldığını izler. Uygulamalarınızı etkileyen kullanıcılar yakalanan emin sorunları yapmak için izlemek istediğiniz. Uygulama izleme, iş mantığı uygulamanız için benzersiz olduğundan bir uygulama ve hizmetlerini geliştirmeye kullanıcılar sorumluluğundadır. Uygulamalarınızı izleme aşağıdaki senaryolarda yararlı olabilir:
* Uygulamamın ne kadar trafik yaşıyor? -Kullanıcı talepleri veya adresi uygulamanızdaki olası bir performans sorunu karşılamak üzere hizmetlerinizi ölçeklendirmek gereken?
* Başarılı ve izlenen hizmet çağrılarım misiniz?
* Uygulamamın kullanıcılar tarafından hangi eylemlerin yapılması? -Telemetrisini toplamaya gelecekteki özellik geliştirmeleri ve daha iyi tanılama uygulama hataları için rehberlik sağlayabilir
* Uygulamamın işlenmeyen özel durumları atma olan? 
* My kapsayıcıların içinde çalışan hizmetleri içinde ne oluyor?

Uygulama izleme hakkında harika geliştiriciler tüm araçları ve uygulamanızın bağlamında yaşadığı bu yana istediğiniz framework kullanabilir şeydir! Azure İzleyici - Application Insights ile uygulama izleme için bir Azure çözümü hakkında daha fazla bilgi [Application Insights ile olay analizi](service-fabric-diagnostics-event-analysis-appinsights.md).
İle nasıl bir öğretici de sahibiz [.NET uygulamaları için bunu](service-fabric-tutorial-monitoring-aspnet.md). Bu öğreticide, doğru Araçları ' nı yüklemek nasıl uygulama ve uygulama tanılama ve telemetri Azure portalında görüntüleme özel telemetri yazma için bir örnek üzerinden gider. 


## <a name="platform-cluster-monitoring"></a>İzleme Platformu (küme)
Bir kullanıcı, kullanıcı kodu kendisi, ancak Tanılama hakkında Service Fabric platformdan yazdığından hangi telemetrisi üzerinde uygulamadan gelen denetiminde mi? Service Fabric'in hedefleri uygulamalar donanım hatalarına dayanıklı tutmak biridir. Bu hedef platformun Sistem Hizmetleri, altyapı sorunları ve hızlı bir şekilde diğer düğümlere yük devretme iş yüklerini kümedeki algılama özelliğini aracılığıyla sağlanır. Ancak bu durumda, Sistem Hizmetleri sorunları varsa ne olur? Ya da hizmetleri yerleşimi için kuralları, dağıtmak veya bir iş yükü taşımak ise çalışılıyor, ihlal? Service Fabric, kümenizdeki gerçekleşen etkinliği hakkında bilgilendirilmesi emin olmak için daha fazla bilgi ve bunları tanılama sağlar. Küme izleme için bazı örnek senaryolar şunlardır:

* Service Fabric, uygulamalarınız yerleştirme ve kümeyi geçici Dengeleme açısından beklediğim şekilde davrandığından? 
* Kullanıcı eylemlerini onaylanır ve beklendiği gibi üzerinde yürütülen kümenizde alınır? Örneğin Ölçeklendirme, yük devretme, dağıtımları
* Hangi düğümün kümenin parçası olan izlemek ve bana bildiren bir Service Fabric olan bir sorun oluştuğunda birinde?

Sağlanan tanılama olayları hazır kapsamlı bir dizi biçiminde alır. Bunlar [Service Fabric olayları](service-fabric-diagnostics-events.md) düğümleri, uygulamalar, hizmetler, bölümler vb. gibi farklı varlıkları platformunda tarafından gerçekleştirilen eylemler gösterilmektedir. Platform aşağı gitmek için bir düğüm varsa son senaryoda yukarıdaki yayma bir `NodeDown` olay ve bildirim hemen, tercih ettiğiniz izleme aracı tarafından. Diğer ortak örnekler `ApplicationUpgradeRollbackStarted` veya `PartitionReconfigured` yük devretmesi sırasında. **Aynı olayları, hem Windows hem de Linux kümelerinde kullanılabilir.**

Olaylar, hem Windows hem de Linux standart kanallar aracılığıyla gönderilen ve bu destekleyen izleme aracı tarafından okunabilir. Log Analytics Azure İzleyici çözümüdür. Daha fazla bilgi edinin çekinmeyin bizim [Log Analytics tümleştirmesi](service-fabric-diagnostics-event-analysis-oms.md) kümenizi ve uyarılar, oluşturabileceğiniz bazı örnek sorgular için özel bir işletimsel Pano içerir. Daha fazla küme izleme kavramları kullanılabilir [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).

### <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric platform küme varlık durumu için Genişletilebilir sistem durumu raporlama sağlar. bir sistem durumu modeli içerir. Her düğüm, uygulama, hizmet, bölüm, çoğaltma veya örnek, bir sürekli olarak güncelleştirilebilir sistem durumu vardır. Sistem durumu, "Tamam", "Uyarı" veya "Error" ya da olabilir. Service Fabric olayları olarak her varlık için bir sıfat çeşitli varlıklar ve sistem durumu küme tarafından yapılan fiilleri düşünün. Belirli bir varlık durumu geçişleri, her zaman bir olay da yayılan. Bu şekilde, seçeceğiniz diğer bir olay gibi izleme araç, sorguları ve sistem durumu olayları için uyarıları ayarlayabilirsiniz. 

Ayrıca, biz bile kullanıcıların varlıklar için sistem durumu geçersiz kılma sağlar. Uygulamanızı bir yükseltme uygulanıyordur ve başarısız olan testler doğrulama varsa, uygulamanız artık sağlıklı olduğunu ve Service Fabric geri alır. otomatik olarak yükseltme belirtmek için sistem durumu API'sini kullanarak Service Fabric sistem durumu için yazabilirsiniz! Sistem durumu modeli hakkında daha fazla bilgi için kullanıma [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md)

![SFX sistem durumu Panosu](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)


### <a name="watchdogs"></a>Watchdogs
Genellikle, bir izleme sistem durumu izleme ve Hizmetleri, ping uç noktaları ve rapor sistem yükü kümedeki herhangi bir şey için ayrı bir hizmettir. Bu işlem, tek bir hizmette bir görünümü temel alarak değil algılanır hataları önlemeye yardımcı olabilir. Watchdogs de (örneğin, belirli zaman aralıklarında depolama günlük dosyalarında temizleme) kullanıcı etkileşimi olmadan düzeltici eylemler gerçekleştirir ana kod için idealdir. Bir örnek izleme hizmeti uygulaması bulabilirsiniz [burada](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="infrastructure-performance-monitoring"></a>(Performans) altyapı izleme
Uygulama ve platform tanılamayı ele aldığımız, nasıl biz donanım beklendiği gibi çalıştığını biliyor musunuz? Temel altyapınızı izleme durumunu kümenizi ve kaynak kullanımınızı anlamanın önemli bir parçasıdır. Sistem performansını ölçme bağlı olarak iş yüklerinizi öznel olabilir birçok faktöre bağlıdır. Bu etkenler, genellikle performans sayaçları aracılığıyla ölçülür. Bu performans sayaçlarını bir çeşitli işletim sistemi, .NET framework veya Service Fabric platform gibi kaynaklardan gelebilir. Bunlar yararlı olabilecek bazı senaryolar verilmiştir

* My donanım verimli bir şekilde yararlanarak? % 90 CPU veya % 10 CPU, donanım kullanmak istiyorsunuz. Bu pratik olduğunda gelen kümenizi ölçeklendirme veya uygulamanızın işlemlerinin iyileştirilmesi.
* Altyapı sorunları proaktif olarak tahmin edebilir? -birçok sorunu performansında ani değişiklikleri (düşme) tarafından öncelenen, kullanabileceğiniz şekilde ağı tahmin edip proaktif olarak sorunları tanılamak için g/ç ve CPU kullanımı gibi performans sayaçları.

Altyapı düzeyinde toplanan performans sayaçları listesini şu yolda bulunabilir: [performans ölçümlerini](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric alsp performans sayaçları için Reliable Services ve Actors programlama modeli sağlar. Bu modeller herhangi birini kullanıyorsanız, bu performans sayaçlarını, aktör yukarı ve aşağı doğru bir şekilde başlatarak olduğunu veya güvenilir hizmet isteklerinizi işlenen yeterince hızlı emin olmak için bilgileri görüntüleyebilirsiniz. Daha fazla bilgi için [güvenilir hizmet uzaktan iletişim için izleme](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) ve [Reliable Actors için performans izleme](service-fabric-reliable-actors-diagnostics.md#performance-counters). 

Bunlar toplamak için Azure İzleyici Log Analytics yalnızca platform düzeyi izleme gibi çözümüdür. Kullanmanız gereken [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) uygun performans sayaçları toplamak ve Log Analytics'te görüntülemek için.

## <a name="recommended-setup"></a>Önerilen Kurulumu
Biz her alanı izleme ve örnek senaryolar incelediğinize göre Azure izleme araçları ve yukarıdaki tüm alanları izlemek için gerekli ayarlanmış bir özeti aşağıda verilmiştir. 

* İle uygulama izlemeyi [Application Insights](service-fabric-tutorial-monitoring-aspnet.md)
* İle küme izleme [tanılama aracısını](service-fabric-diagnostics-event-aggregation-wad.md) ve [Log Analytics](service-fabric-diagnostics-oms-setup.md)
* İle altyapı izleme [Log Analytics](service-fabric-diagnostics-oms-agent.md)

Ayrıca kullanın ve bulunan örnek ARM şablonu değiştirme [burada](service-fabric-diagnostics-oms-setup.md#deploy-log-analytics-with-azure-resource-manager) tüm gerekli kaynakları ve aracıları dağıtımını otomatik hale getirmek için. 

## <a name="other-logging-solutions"></a>Diğer günlük çözümleri

İki çözümü önerilir ancak [Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) ve [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) Service Fabric ile tümleştirme, birçok olayları ETW sağlayıcıları yazılır ve olan yerleşik diğer günlük çözümlerle genişletilebilir. İçine görünmelidir [Elastic Stack](https://www.elastic.co/products) (özellikle, bir küme çevrimdışı bir ortamda çalışan düşünüyorsanız), [Dynatrace](https://www.dynatrace.com/), ya da herhangi bir platform, tercih. Tümleştirilmiş iş ortaklarının listesi sahibiz [burada](service-fabric-diagnostics-partners.md).

Seçtiğiniz önemli noktaları herhangi bir platform için nasıl, kullanıcı arabirimi, sorgulama özellikleri, özel görselleştirmeler ve Panolar ile kendinizi rahat hissedene içermelidir ve ek araçlar izleme deneyiminizi geliştirmek için sağlar. 

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı izleme ile başlamak için bkz [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md).
* Git ile uygulamanızı Application ınsights'ı ayarlamak için adımları [İzleyici Service fabric'te ASP.NET Core uygulamasını ve tanılama](service-fabric-tutorial-monitoring-aspnet.md).
* Platform ve Service Fabric sağlar için önceden olayları izleme hakkında daha fazla [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).
* Service Fabric ile Log Analytics tümleştirmesini yapılandırmak [bir kümesi için Log Analytics'i ayarlama](service-fabric-diagnostics-oms-setup.md)
* Kapsayıcıları izlemek için log Analytics'i ayarlama hakkında bilgi edinmek- [izleme ve tanılama için Windows Azure Service Fabric'teki kapsayıcıları](service-fabric-tutorial-monitoring-wincontainers.md).
* Örnek tanılama sorunlar ve çözümleri ile Service Fabric'te bkz [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Service Fabric ile tümleştirme diğer tanılama ürünleri kullanıma [Service Fabric tanılama iş ortakları](service-fabric-diagnostics-partners.md)
* Azure kaynakları - izleme genel öneriler hakkında bilgi edinin [en iyi uygulamalar - izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 