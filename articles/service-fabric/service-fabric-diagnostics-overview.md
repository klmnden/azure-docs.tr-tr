---
title: Azure Service Fabric izleme ve tanılama genel bakış | Microsoft Docs
description: İzleme ve tanılama Azure Service Fabric kümeleri, uygulamalar ve hizmetler hakkında bilgi edinin.
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
ms.date: 04/25/2018
ms.author: srrengar
ms.openlocfilehash: f7fe07500f877cf34626e53361c9c68dd459a5e4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643184"
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>İzleme ve tanılama Azure Service Fabric için

Bu makalede, Azure Service Fabric için izleme ve Tanılama hakkında genel bir bakış sağlar. İzleme ve tanılama geliştirme, test ve tüm bulut ortamınızdaki iş yükleri dağıtmak için kritik öneme sahiptir. İzleme, uygulamalarınızı nasıl kullanıldığı, kaynak kullanımı ve kümenizi genel durumunu izlemenize olanak sağlar. Tanılamak ve sorunları düzeltmek için bu bilgileri kullanın ve sorunları gelecekte oluşmasını önlemek. 

## <a name="application-monitoring"></a>Uygulama izleme
Uygulama İzleme özelliklerini ve bileşenlerini, uygulamanızın nasıl kullanıldığını izler. Kullanıcıların yakalanan etkileyen emin sorunları yapmak için uygulamalarınızı izlemek istersiniz. Uygulamalarınızı izleme aşağıdaki senaryolarda yararlı olabilir:
* Uygulama yük ve kullanıcı belirleme trafiği - kullanıcı taleplerini karşılamak veya uygulamanızda olası bir performans sorunu gidermek için hizmetlerinizi ölçeklendirme gerekiyor mu?
* Küme genelinde, hizmet iletişimi ve Uzaktan iletişim sorunları tanımlar
* Uygulamanızla kullanıcıların ne yaptıklarını açık - telemetri uygulamalarınızda toplama Kılavuzu gelecekteki özelliği development ve uygulama hataları için daha iyi tanılama yardımcı olabilir
* Çalışan kapsayıcılarda neler olduğunu izleme

Service Fabric uygulaması kodunuzu uygun izlemeleri ve telemetri ile izleme için birçok seçenek destekler. Uygulama Öngörüler (AI) kullanmanızı öneririz. Service Fabric AI'ın tümleştirmesi araç deneyimleri Service Fabric belirli ölçümleri yanı sıra, Visual Studio ve Azure portal için kapsamlı Giden kutusu günlük deneyimi sağlayan içerir. Birçok günlükleri otomatik olarak oluşturulan ve sizin için AI ile toplanan olsa, özel günlük daha fazla daha zengin bir tanılama deneyimi oluşturmak için uygulamalarınızı eklemenizi öneririz. Service Fabric ile Application Insights ile çalışmaya başlama hakkında daha fazla [olay çözümleme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md).

## <a name="platform-cluster-monitoring"></a>Platform (küme) izleme
Service Fabric kümesi izleme platform ve tüm iş yükleri beklendiği gibi çalıştığını sağlamak için önemlidir. Service Fabric'in hedeflerinden uygulamaları dayanıklı donanım hataları korumaktır. Bu hedef, kümedeki altyapı sorunları ve hızlı bir şekilde yük devretme iş yükleri diğer düğümlere algılamak için platformun sistem hizmetleri özelliği aracılığıyla elde edilir. Ancak, bu özel durumda sistem hizmetleri sorunları yoksa ne? Veya kuralları Hizmetleri yerleşimi için bir iş yükü taşınmaya çalışılırken varsa içinde ihlal? Küme izleme sorunlarını tanılama ve etkili bir şekilde giderilmesine yardımcı kümenizde gerçekleşmesini etkinliği hakkında bilgi sahibi olmak sağlar. İçin arama istediğiniz bazı önemli noktalar yer almaktadır:
* Service Fabric uygulamalarınızı yerleştirme ve küme geçici Dengeleme açısından, beklediğiniz şekilde davrandığından? 
* Kullanıcı işlemler onaylanır ve üzerinde beklendiği gibi yürütülen kümenizde uygulanır? Bu bir küme ölçeklendirme özellikle geçerlidir.
* Service Fabric verilerinizi ve küme içindeki hizmet-hizmet iletişimi doğru şekilde işliyor?

Service Fabric kutusu dışı olaylar kapsamlı bir kümesini sağlar. Bunlar [Service Fabric olayları](service-fabric-diagnostics-events.md) EventStore API'leri veya işletimsel kanalı (olay kanalı platform tarafından gösterilen) üzerinden erişilebilir. 
* EventStore - EventStore (Windows'ta sürümlerde kullanılabilir 6.2 ve sonraki, Linux hala devam ettiğinden bu makalenin son güncelleştirilme tarihi itibariyle), bu olayları bir dizi API (REST uç noktaları yoluyla veya istemci kitaplığı üzerinden erişilebilen) aracılığıyla kullanıma sunar. EventStore hakkında daha fazla bilgiyi [EventStore genel bakış](service-fabric-diagnostics-eventstore.md).
* Service Fabric olay kanalları - Windows, Service Fabric olaylar, ilgili kümesi ile tek bir ETW sağlayıcıdan kullanılabilir `logLevelKeywordFilters` işlemsel ve veri & ileti kanalları arasında - seçmek için kullanılan bu biz ayrı giden çıkışı yoludur Yapı olayları gerektiğinde üzerinde filtre uygulanacak hizmet. Linux üzerinde Service Fabric olayları LTTng gelir ve bir depolama tabloya burada bunlar gerektiğinde filtrelenebilir gelen yerleştirilir. Bu kanalları, küme durumunu daha iyi anlamak için kullanılan seçkin, yapılandırılmış olayları içerir. Tanılama varsayılan olarak bu kanalları olayların gelecekte sorgulamak için gönderildiği bir Azure depolama tablosu oluşturduğunuz küme oluşturma sırasında etkinleştirilir. 

Biz EventStore hızlı analiz ve kümenizi nasıl çalıştığını bir anlık görüntü fikir almak için kullanılması önerilir ve olarak gerçekleşmesi durumunda bekleniyordu. Küme tarafından oluşturulan olayları ve günlükleri toplamak için genellikle kullanmanızı öneririz [Azure tanılama uzantısını](service-fabric-diagnostics-event-aggregation-wad.md). Service Fabric analizi, Service Fabric kümeleri izleme için özel bir Pano sağlar ve kümenizin olayları sorgulamak ve uyarıları ayarlamak verir OMS günlük analizi Service Fabric belirli çözümü ile iyi entegre olur. Şu anda hakkında daha fazla bilgiyi [olay analizi OMS ile](service-fabric-diagnostics-event-analysis-oms.md). 

 Daha fazla bilgiyi konumunda kümenize izleme hakkında [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).

## <a name="performance-monitoring"></a>Performans izleme
Temel altyapınızı izleme durumunu kümenizi ve kaynak kullanımını anlamak, önemli bir parçasıdır. Sistem performansını ölçmek, her biri, genellikle bir ana performans göstergelerini (KPI'lar) ölçülür, birkaç etkene bağlıdır. Service Fabric ilgili KPI'ları, kümedeki düğümlerden performans sayaçları olarak toplanan ölçümleri eşlenebilir.
Bu KPI'ları ile yardımcı olabilir:
* Kaynak kullanımı ve kümenizi ölçeklendirme ya da hizmet işlemlerinizi en iyi duruma getirme amacıyla yük - anlama.
* Altyapı sorunları - tahmin etmeye infrastructural sorunlarını tanılamak ve tahmin etmek için ağ g/ç ve CPU kullanımı gibi KPI'leri kullanabilmeniz için birçok sorunu performans, ani değişiklikler (düşme) tarafından öncesinde olur.

Altyapı düzeyinde toplanan performans sayaçlarının listesi bulunabilir [performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md). 

Service Fabric Reliable Services ve Actors programlama modelleri için performans sayaçları kümesi sağlar. Bu modeller birini kullanıyorsanız, bu performans sayaçları, aktörler yukarı ve aşağı doğru dönmesini olduğunu ya da güvenilir hizmet isteklerinizi işlenmiş yeterince hızlı sağlamaya yardımcı olmak KPI'ları sağlayabilir. Daha fazla bilgi için bkz: [güvenilir hizmeti uzaktan iletişim için izleme](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) ve [performans güvenilir aktörler için](service-fabric-reliable-actors-diagnostics.md#performance-counters). Buna ek olarak, Application Insights ayrıca bir dizi performans ölçümleri toplar, uygulamanızla birlikte yapılandırdıysanız vardır.

Kullanım [OMS Aracısı](service-fabric-diagnostics-oms-agent.md) uygun performans sayaçlarını toplamak ve bu KPI'leri OMS günlük analizleri görüntülemek için.

![Tanılama Genel Bakış grafiği](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric platformundan bir küme içindeki varlıkların durumunu Genişletilebilir sistem durumu raporlama sağlayan bir sistem durumu modeli içerir. Her düğüm, uygulama, hizmet, bölüm, çoğaltma veya örnek, bir sürekli olarak güncelleştirilebilir sistem durumu vardır. Sistem durumu, "Tamam", "Uyarı" veya "Error" ya da olabilir. Sistem durumu sorunları kümedeki göre her bir varlık için gösterilen durum raporu aracılığıyla değiştirilir. Varlıklarınızı sistem durumunu herhangi bir zamanda Service Fabric Explorer (SFX) aşağıda gösterildiği gibi denetlenebilir veya platformlar ait durum API üzerinden sorgulanabilir. Ayrıca, sistem durumu raporları özelleştirme ve kendi sistem durumu raporlarının ekleyerek veya sistem durumu API kullanarak bir varlık sistem durumunu değiştirebilirsiniz. Sistem durumu modeli hakkında daha fazla ayrıntı bulunabilir [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md).

![SFX sistem durumu Panosu](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

SFX son durumu raporlarına görmenin yanı sıra, her rapor bir olayı olarak da kullanılabilir. Sistem durumu olayları işletimsel kanal aracılığıyla toplanan (bakın [Azure Tanılama Olay toplama](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) ve uyarı ve gelecekte sorgulama için günlük analizi depolanır. Bu nedenle uygun hatası senaryoları (günlük analizi ile özel uyarı) için uyarıları ayarlama öneririz, uygulamanın kullanılabilirliğini etkileyebilecek sorunları algılamak yardımcı olur.

## <a name="other-logging-solutions"></a>Diğer günlük çözümleri

İki çözümleri önerilir ancak [OMS](service-fabric-diagnostics-event-analysis-oms.md) ve [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) yerleşik Service Fabric ile tümleştirme, birçok olayları etw sağlayıcılar ile yazılmış ve diğer Genişletilebilir günlüğe kaydetme çözümleri. İçine görünmelidir [esnek yığın](https://www.elastic.co/products) (özellikle, bir küme çevrimdışı bir ortamda çalışan düşünüyorsanız), [Splunk](https://www.splunk.com/), [Dynatrace](https://www.dynatrace.com/), ya da diğer tercihinize Platform. 

Seçtiğiniz önemli noktaları herhangi bir platform için nasıl, kullanıcı arabirimi ve seçenekleri görselleştirmek ve kolay okunabilir panolar ve izlemenizi geliştirmek için sağladıkları ek araçlar oluşturma olanağı sorgulama deneyimliyseniz içermelidir, Otomatik uyarı gibi.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamaları ile çalışmaya başlama için bkz: [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md).
* Platform ve Service Fabric sağlar için önceden olayları izleme hakkında daha fazla bilgi [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md).
* AI uygulamanızla birlikte ayarlamak için adımları gidin [İzleyici ve Service Fabric bir ASP.NET Core uygulama tanılama](service-fabric-tutorial-monitoring-aspnet.md).
* OMS günlük analizi kapsayıcıları izleme için ayarlama hakkında bilgi edinmek- [izleme ve tanılama için Windows kapsayıcılarında Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md).
* Azure kaynaklarını - yönelik genel izleme öneriler hakkında bilgi edinin [en iyi yöntemler - izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 
