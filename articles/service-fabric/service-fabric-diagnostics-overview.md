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
ms.date: 10/18/2018
ms.author: srrengar
ms.openlocfilehash: 5fc2674a145be99fb8867c5cf1b1f65ba860db80
ms.sourcegitcommit: 668b486f3d07562b614de91451e50296be3c2e1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49457842"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>Azure Service Fabric için izleme ve tanılama

Bu makalede, Azure Service Fabric için izleme ve Tanılama hakkında genel bir bakış sağlar. İzleme ve tanılama geliştirme, test ve herhangi bir bulut ortamında iş yüklerini dağıtma için kritik öneme sahiptir. İzleme, uygulamalarınızı nasıl kullanıldığı, kaynak kullanımınızı ve kümenize genel durumunu izlemenize olanak sağlar. Tanılama ve sorunları gidermek için bu bilgileri kullanın ve sorunları gelecekte meydana gelmesini önlemek. 

## <a name="application-monitoring"></a>Uygulama izleme
Uygulama İzleme özelliklerini ve bileşenlerini, uygulamanızın nasıl kullanıldığını izler. Uygulamalarınızı etkileyen kullanıcılar yakalanan emin sorunları yapmak için izlemek istediğiniz. Uygulamalarınızı izleme aşağıdaki senaryolarda yararlı olabilir:
* Uygulama yükleme ve kullanıcı belirleme trafiği - kullanıcı taleplerine uygun veya uygulamanızdaki olası bir performans sorunu gidermek için hizmetlerinizi ölçeklendirmek gerekiyor mu?
* Sorunları uzaktan hizmet iletişimi ile küme genelinde tanımlama
* Uygulamanızla kullanıcılarınızın neler yaptığını anlamak - uygulamalarınızda telemetrisini toplamaya Kılavuzu gelecekteki özellik geliştirmeleri ve daha iyi tanılama için uygulama hataları yardımcı olabilir
* Çalışan kapsayıcıları içinde neler olduğunu izleme

Service Fabric uygulama kodunuzu uygun izleme ve telemetri ile izleme için birçok seçenek destekler. Application Insights (AI) kullanmanızı öneririz. Service Fabric ile yapay ZEKA'ın tümleştirme araç deneyimlerinden Service Fabric belirli ölçümleri yanı sıra, Visual Studio ve Azure portalı için kapsamlı kullanıma hazır günlüğe kaydetme deneyimi sağlama çalışmaları içerir. Birçok günlükleri otomatik olarak oluşturulur ve sizin için yapay ZEKA ile toplanan rağmen özel günlük daha fazla daha zengin bir tanılama deneyimi oluşturmak için uygulamalarınıza eklemenizi öneririz. Application Insights ile Service Fabric ile çalışmaya başlama hakkında daha fazla bilgi bkz [Application Insights ile olay analizi](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>İzleme Platformu (küme)
Service Fabric kümenizi izleme, platformlar ve tüm iş yükleri beklendiği gibi çalıştığını sağlamak önemlidir. Service Fabric'in hedefleri uygulamalar donanım hatalarına dayanıklı tutmak biridir. Bu hedef platformun Sistem Hizmetleri, altyapı sorunları ve hızlı bir şekilde diğer düğümlere yük devretme iş yüklerini kümedeki algılama özelliğini aracılığıyla sağlanır. Ancak bu durumda, Sistem Hizmetleri sorunları varsa ne olur? Veya kuralları Hizmetleri yerleşimi için bir iş yükü taşınmaya çalışılırken varsa içinde ihlal? Küme izleme sorunları tanılama ve etkili bir şekilde giderilmesine yardımcı olur, kümenizdeki gerçekleşen etkinliği hakkında bilgi edinmenizi sağlar. İçin arama istediğiniz bazı önemli noktalar şunlardır:
* Service Fabric uygulamalarınızı yerleştirme ve kümeyi geçici Dengeleme açısından, beklediğiniz şekilde davrandığından? 
* Kullanıcı eylemlerini onaylanır ve beklendiği gibi üzerinde yürütülen kümenizde alınır? Bu bir küme ölçeklendirme özellikle geçerlidir.
* Service Fabric, verilerinizi ve küme içinde service hizmeti iletişiminizin doğru işleyen?

Service Fabric olayları hazır kapsamlı bir dizi sağlar. Bunlar [Service Fabric olayları](service-fabric-diagnostics-events.md) EventStore API'leri veya işlevsel kanal (olay kanalı platform tarafından sunulan) aracılığıyla erişilebilir. 
* Eventstore'a - Eventstore'a (Windows sürümlerde kullanılabilir 6.2 ve sonraki sürümlerinde, Linux hala devam ederken bu makalenin son güncelleştirme tarihi itibariyle), bu olayları bir dizi API'si (REST uç noktaları aracılığıyla veya istemci kitaplığı aracılığıyla erişilebilir) aracılığıyla kullanıma sunar. Eventstore'a hakkında daha fazla bilgiyi [Eventstore'a genel bakış](service-fabric-diagnostics-eventstore.md).
* Service Fabric olay kanalları - üzerinde Windows, Service Fabric olayları ile ilgili bir dizi tek bir ETW sağlayıcısından kullanılabilir `logLevelKeywordFilters` arasında işletimsel ve veri ve ileti kanalı - almak için kullanılır, ayırabiliriz dışarı giden yolu budur Fabric olayları gerektiğinde üzerinde filtre uygulanacak hizmeti. Linux üzerinde Service Fabric olayları LTTng gelir ve bir depolama tablosuna nerede bunlar gerektiği şekilde filtrelenebilen gelen yerleştirilir. Bu Kanallar, küme durumunu daha iyi anlamak için kullanılan seçkin, yapılandırılmış olayları içerir. Tanılama varsayılan olarak bu kanallar olayları gelecekte sorgulamak için nereye gönderileceğini bir Azure depolama tablosuna oluşturduğunuz küme oluşturma sırasında etkinleştirilir. 

Biz Eventstore'a hızlı analiz ve kümenizin nasıl çalıştığını bir anlık görüntü fikir almak için kullanılması önerilir ve olarak şeyler varsa bekleniyor. Kümeniz tarafından oluşturulan olayları ve günlükleri toplamak için genellikle kullanmanızı öneririz [Azure tanılama uzantısını](service-fabric-diagnostics-event-aggregation-wad.md). Bu, Service Fabric analizi, Service Fabric kümeleri izleme için özel bir Pano sağlayan ve sorgu kümenizin olayları ve uyarıları ayarlama sağlar Log Analytics Service Fabric belirli çözümü ile tümleştirilir. Şu anda hakkında daha fazla bilgiyi [Log Analytics ile olay analizi](service-fabric-diagnostics-event-analysis-oms.md). 

 Daha fazla bilgi edinebilirsiniz konumunda kümenize izleme hakkında [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).

## <a name="performance-monitoring"></a>Performans izleme
Temel altyapınızı izleme durumunu kümenizi ve kaynak kullanımınızı anlamanın önemli bir parçasıdır. Sistem performansı ölçmek, her biri genellikle bir ana performans göstergeleri (KPI) ölçülür birkaç faktöre bağlıdır. Service Fabric, kümenizdeki düğümleri olarak performans sayaçları toplandığı ölçümlerine ilgili KPI'leri eşlenebilir.
Bu KPI'ları ile yardımcı olabilir:
* Kaynak kullanımı ve kümenizi ölçeklendirme ve hizmet işlemlerinizi en iyi duruma getirme amacıyla yükleme - anlama.
* Altyapı sorunları - tahmin tahmin edin ve altyapı sorunları tanılamak için ağ g/ç ve CPU kullanımı gibi KPI'leri kullanabilmeniz için çok sayıda sorunu performans, ani değişiklikleri (düşme) tarafından öncesinde olur.

Altyapı düzeyinde toplanan performans sayaçları listesini şu yolda bulunabilir: [performans ölçümlerini](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric Reliable Services ve Actors programlama modelleri için performans sayaçları kümesini sağlar. Bu modeller herhangi birini kullanıyorsanız, bu performans sayaçlarını, aktör yukarı ve aşağı doğru bir şekilde başlatarak olduğunu veya güvenilir hizmet isteklerinizi işlenen yeterince hızlı sağlamak KPI'ları sağlayabilir. Daha fazla bilgi için [güvenilir hizmet uzaktan iletişim için izleme](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) ve [Reliable Actors için performans izleme](service-fabric-reliable-actors-diagnostics.md#performance-counters). Buna ek olarak, Application Insights de bir dizi performans ölçümleri toplar, uygulamanızla yapılandırdıysanız vardır.

Kullanım [Log Analytics aracısını](service-fabric-diagnostics-oms-agent.md) uygun performans sayaçlarını toplayan ve Azure Log Analytics'te bu KPI'leri görüntüleyin.

![Genel Bakış grafik tanılama](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric platform küme varlık durumu için Genişletilebilir sistem durumu raporlama sağlar. bir sistem durumu modeli içerir. Her düğüm, uygulama, hizmet, bölüm, çoğaltma veya örnek, bir sürekli olarak güncelleştirilebilir sistem durumu vardır. Sistem durumu, "Tamam", "Uyarı" veya "Error" ya da olabilir. Sistem durumu sorunları kümedeki göre her varlık için gösterilen sistem durumu raporlarının aracılığıyla değiştirilir. Varlıklarınızın durumunu Service Fabric Explorer (SFX) dilediğiniz zaman aşağıda gösterildiği gibi denetlenebilir veya platformlar'ın sistem durumu API aracılığıyla sorgulanabilir. Ayrıca, sistem durumu raporları özelleştirme ve kendi sistem durumu raporlarının ekleme veya sistem durumu API'si kullanarak bir varlığın sistem durumunu değiştirebilirsiniz. Sistem durumu modeli hakkında daha fazla ayrıntı şu adreste bulunabilir: [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md).

![SFX sistem durumu Panosu](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

SFX en son durumu raporlarına görmenin yanı sıra, her rapor bir olay olarak da sunulur. Sistem durumu olaylarını işlevsel kanal toplanan (bkz [Azure Tanılama ile olay toplama](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) ve uyarı verme ve gelecekte sorgulama için Log Analytics'te depolanır. Bu, uygun hata senaryoları (Log Analytics aracılığıyla özel uyarılar) için uyarıları ayarlama öneririz, böylece, uygulama kullanılabilirliği etkileyebilecek sorunları belirlemenize yardımcı olur.

## <a name="other-logging-solutions"></a>Diğer günlük çözümleri

İki çözümü önerilir ancak [Azure Log Analytics](service-fabric-diagnostics-event-analysis-oms.md) ve [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) Service Fabric ile tümleştirme, birçok olayları ETW sağlayıcıları yazılır ve olan yerleşik diğer günlük çözümlerle genişletilebilir. İçine görünmelidir [Elastic Stack](https://www.elastic.co/products) (özellikle, bir küme çevrimdışı bir ortamda çalışan düşünüyorsanız), [Dynatrace](https://www.dynatrace.com/), ya da herhangi bir platform, tercih. Tümleştirilmiş iş ortaklarının listesi sahibiz [burada](service-fabric-diagnostics-partners.md).

Seçtiğiniz önemli noktaları herhangi bir platform için nasıl, kullanıcı arabirimi ve verileri görselleştirmek ve kolay okunabilir panolar ve izleme işleminiz geliştirmek için sağladıkları ek araçlar oluşturma olanağı seçenekleri sorgulama kendinizi rahat hissedene içermelidir, Otomatik uyarı verme gibi.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamalarınızı izleme ile başlamak için bkz [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md).
* Platform ve Service Fabric sağlar için önceden olayları izleme hakkında daha fazla [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).
* Uygulamanız için yapay ZEKA ' ayarlamak için adımları tamamlayın [İzleyici Service fabric'te ASP.NET Core uygulamasını ve tanılama](service-fabric-tutorial-monitoring-aspnet.md).
* Kapsayıcıları izlemek üzere OMS Log Analytics'i ayarlama hakkında bilgi edinmek- [izleme ve tanılama için Windows Azure Service Fabric'teki kapsayıcıları](service-fabric-tutorial-monitoring-wincontainers.md).
* Örnek tanılama sorunlar ve çözümleri ile Service Fabric'te bkz [yaygın senaryoları tanılama](service-fabric-diagnostics-common-scenarios.md)
* Service Fabric ile tümleştirme diğer tanılama ürünleri kullanıma [Service Fabric tanılama iş ortakları](service-fabric-diagnostics-partners.md)
* Azure kaynakları - izleme genel öneriler hakkında bilgi edinin [en iyi uygulamalar - izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
