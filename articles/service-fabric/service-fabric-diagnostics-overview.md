---
title: Azure Service Fabric izleme ve tanılama genel bakış | Microsoft Docs
description: İzleme ve tanılama Azure Service Fabric kümeleri, uygulamalar ve hizmetler hakkında bilgi edinin.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2018
ms.author: dekapur
ms.openlocfilehash: f784576547f0d85a825ad9dd107c6c84cd261092
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>İzleme ve tanılama Azure Service Fabric için

Bu makalede izleme ve tanılama Service Fabric kümeleri çalıştıran, uygulamalarınız için ayarlama genel bir bakış sağlar. İzleme ve tanılama geliştirme, test ve tüm bulut ortamınızdaki iş yükleri dağıtmak için kritik öneme sahiptir. İzleme, uygulamalarınızı nasıl kullanıldığı, kaynak kullanımı ve kümenizi genel durumunu izlemenize olanak sağlar. Tanılamak ve kümedeki tüm sorunları düzeltmek için ve sorunları gelecekte oluşmasını önlemek için bu bilgileri kullanabilirsiniz. 

İçin izleme ve tanılama başlıca amaçları şunlardır:
* Altyapı sorunlarını tanılamak ve Algıla
* Uygulamanız ile ilgili sorunları algıla
* Kaynak tüketimini anlamak
* Uygulama, hizmet ve altyapı performans izleme

Service Fabric kümesi izleme ve Tanılama verileri üç düzeylerinden gelir: uygulama, platform (küme) ve altyapı. 
* [Uygulama düzeyinde](service-fabric-diagnostics-event-generation-app.md) performansı uygulamalarınızı ve eklediğiniz tüm ek özel günlüğe kaydetme ile ilgili veriler içerir. Uygulama izleme verileri uygulama Öngörüler (AI) içinde uygulamadan elde. AI SDK'sı, EventFlow veya tercih ettiğiniz başka bir işlem hattı gelebilir.
* [Platform düzeyi](service-fabric-diagnostics-event-generation-infra.md) küme üzerinde çalışan ve uygulamaların yönetimini ilgili kümenizde gerçekleştirilmesini eylemlerine olaylarını içerir. Platform izleme verilerini OMS günlük analizi için gelen olayları görselleştirmenize yardımcı olmak için Service Fabric analiz çözümü ile gönderilmelidir. Bu veriler genellikle Windows veya Linux Azure tanılama uzantısı aracılığıyla bir depolama hesabı nerede OMS günlük analizi tarafından erişilen gönderilen. 
* Altyapı düzeyi odaklanır [performans izleme](service-fabric-diagnostics-event-generation-perf.md), görünümlü anahtar ölçümleri ve kaynak kullanımı ve yük belirlemek için performans sayaçları. Performans izleme bir aracı kullanarak elde edilebilir - böylece, bağıntılı gibi daha iyi tanılama için bir deneyim sağlayan aynı yerde platform olaylarınızı olarak performans verilerinizi sona erer (Microsoft Monitoring) OMS Aracısı kullanmanızı öneririz bir küme içinde değişir. 

![Tanılama Genel Bakış grafiği](media/service-fabric-diagnostics-overview/diagnostics-overview.png)

## <a name="monitoring-scenarios"></a>İzleme senaryoları

Bu bölümde bir Service Fabric kümesi - izleme senaryoları uygulama, küme, performans ve sistem durumu izleme anlatılmaktadır. Her senaryo için izleme hedefi ve genel yaklaşım ele alınmıştır. Bunlar ve diğer genel izleme önerileri Azure kaynakları için daha ayrıntılı bilgi şu adreste bulunabilir: [en iyi yöntemler - izleme ve tanılama](https://docs.microsoft.com/azure/architecture/best-practices/monitoring). 

Bu senaryolar da geniş izleme ve tanılama verilerinin üç düzeyi için yukarıdaki uygun şekilde kümedeki işlenecek her senaryo için yani anlatıldığı gibi eşlenen, karşılık gelen düzeyinde gelen izleme ve tanılama veri olmalıdır . Senaryo izleme sistem durumu kümeyi ve içinde çalışan her şeyi kapsayan bir özel durum taşımaktadır.

## <a name="application-monitoring"></a>Uygulama izleme
Uygulama izleme nasıl özelliklerini ve bileşenlerini hazırladığınız, bir uygulamanın kullanıldığından izler. Kullanıcıların yakalanan etkileyen emin sorunları yapmak için uygulamalarınızı izlemek istersiniz. Uygulamalarınızı izleme yararlı olabilir:
* Uygulama yük ve kullanıcı belirleme trafiği - kullanıcı taleplerini karşılamak veya uygulamanızda olası bir performans sorunu gidermek için hizmetlerinizi ölçeklendirme gerekiyor mu?
* Küme genelinde, hizmet iletişimi ve Uzaktan iletişim sorunları tanımlar
* uygulamanızla kullanıcıların ne yaptıklarını açık - uygulamalarınızı düzenleme Kılavuzu gelecekteki özelliği development ve uygulama hataları için daha iyi tanılama yardımcı olabilir

Service Fabric uygulama düzeyinde izleme için uygulama Öngörüler (AI) kullanmanızı öneririz. Visual Studio ve Azure portal ve Service Fabric Hizmet bağlamı ve AI Pano ve kapsamlı bir giden kutusu günlük baştaki uygulama eşlemesi remoting anlaşılması için araç deneyimleri Service Fabric AI'ın tümleştirmesi içerir deneyimi. Daha fazla özel günlük uygulamalarınıza eklemek ve AI içinde ne sağlanan sorunları işlemek için daha zengin bir tanılama deneyimi oluşturmak için görünür olan birçok günlükleri otomatik olarak oluşturulur ve sizin için AI kullanırken toplanan olsa öneririz Gelecekte. AI Service Fabric ile kullanma hakkında daha fazla [olay çözümleme Application Insights ile](service-fabric-diagnostics-event-analysis-appinsights.md). 

Kodunuzu uygulamalarınızı izlemek için izleme ile çalışmaya başlamak için üzerinden için head [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md). 

### <a name="monitoring-application-availability"></a>Uygulama kullanılabilirliği izleme
Kümenizdeki her şeyin beklendiği gibi çalışıyor gibi görünüyor ve sağlam olarak raporlama, ancak uygulamalarınızı erişilemiyor veya ulaşılamaz göründüğü mümkündür. Çoğu durumda bu olacağını yok ancak bunu mümkün olan en kısa sürede azaltmak için bu söz konusu olduğunda bilmeniz önemlidir. Geniş çapta'yi kullanılabilirlik izlemesi "çalışır durumda kalma süresi" sistemin genel anlamak için sisteminizin bileşenlerinin kullanılabilirliğini izleme ile ilgilidir. Bir kümede biz kullanılabilirlik açısından, uygulamanızın odaklanmak - bu uygulama yaşam döngüsü yönetimi senaryoları emin olmak için platform works kapalı kalma süresi neden olmaz. Öte yandan, uygulamanızın çalışma süresini etkileyen çeşitli sorunları kümedeki neden olabilir ve bu durumda, uygulama sahibi olarak, genellikle hemen bilmek istiyorsunuz. Diğer tüm uygulamaları yanı sıra, bir izleme kümenizdeki dağıtmanızı öneririz. Bu tür bir izleme amacı ayarlanmış bir zaman aralığından uygulamanız için uygun uç noktaları denetleyin ve yeniden erişilebilir olduklarını rapor olacaktır. Bu, uç nokta ping atar ve belirli bir zaman aralığında bir rapor döndüren bir dış hizmet kullanarak da yapabilirsiniz.

## <a name="cluster-monitoring"></a>Küme izleme
Service Fabric kümesi izleme platform ve üzerinde çalışan tüm iş yükleri beklendiği gibi çalıştığını sağlamak için önemlidir. Service Fabric'in hedeflerinden donanım hataları çalışan uygulamalar korumaktır. Bu altyapı sorunları ve hızlı bir şekilde yük devretme iş yükleri diğer düğümlere kümede algılamak için platformun sistem hizmetleri özelliği aracılığıyla sağlanır. Ancak, bu özel durumda sistem hizmetleri sorunları yoksa ne? Veya kuralları Hizmetleri yerleşimi için bir iş yükü taşınmaya çalışılırken varsa içinde ihlal? Küme izleme sorunlarını tanılama ve etkili bir şekilde giderilmesine yardımcı kümenizde gerçekleşmesini etkinliği hakkında bilgi sahibi olmak sağlar. İçin arama istediğiniz bazı önemli noktalar yer almaktadır:
* Service Fabric uygulamalarınızı yerleştirme ve küme geçici Dengeleme açısından, beklediğiniz şekilde davrandığından? 
* Eylemler, onaylanan ve beklendiği gibi çalıştırıldığı, küme yapılandırmasını değiştirmek için çıkarılmakta? Bu bir küme ölçeklendirme özellikle geçerlidir.
* Service Fabric verilerinizi ve küme içindeki hizmet-hizmet iletişimi doğru şekilde işliyor?

Service Fabric işlemsel ve veri & ileti kanalları üzerinden kutusu dışı olaylar kapsamlı bir kümesini sağlar. Tek bir ETW sağlayıcısı formunda ilgili kümesiyle bunlar Windows'ta `logLevelKeywordFilters` arasında farklı kanallar almak için kullanılır. Linux üzerinde tüm platform olaylarını LTTng gelen ve burada bunlar gerektiğinde filtrelenebilir gelen bir tabloya yerleştirin. 

Bu kanalları, küme durumunu daha iyi anlamak için kullanılan seçkin, yapılandırılmış olayları içerir. "Tanılama" ile bir Azure depolama tablosu bu kanalları olayların gelecekte sorgulamak için nereye gönderileceğini ayarladığınız küme oluşturma zamanında varsayılan olarak etkindir. Daha fazla bilgiyi konumunda kümenize izleme hakkında [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md). Kümenizi izlemek için OMS günlük analizi kullanmanızı öneririz. OMS günlük analizi Service Fabric kümeleri izleme için özel bir Pano sağlar ve kümenizin olayları sorgulamak ve uyarıları ayarlamak verir Service Fabric analizi, bir Service Fabric belirli çözüm sunar. Şu anda hakkında daha fazla bilgiyi [olay analizi OMS ile](service-fabric-diagnostics-event-analysis-oms.md). 

### <a name="watchdogs"></a>Watchdogs
Genellikle, bir izleme sistem durumu izleyebilir ve Hizmetleri, ping uç noktaları ve rapor sistem yükü kümedeki herhangi bir şey için ayrı bir hizmet içindir. Bu, tek bir hizmeti görünümü temel alarak algılanmayacağı hataları önlemeye yardımcı olabilir. Watchdogs de iyi (örneğin, belirli zaman aralıklarında depolama günlük dosyalarında temizleniyor) kullanıcı etkileşimi olmadan düzeltici eylemleri gerçekleştirir konak koduna yerlerdir. Bir örnek izleme hizmeti uygulaması bulabilirsiniz [burada](https://github.com/Azure-Samples/service-fabric-watchdog-service).

## <a name="performance-monitoring"></a>Performans izleme
Temel altyapınızı izleme durumunu kümenizi ve kaynak kullanımını anlamak, önemli bir parçasıdır. Sistem performansını ölçmek, her biri, genellikle bir ana performans göstergelerini (KPI'lar) ölçülür, birkaç etkene bağlıdır. Service Fabric ilgili KPI'ları, kümedeki düğümlerden performans sayaçları olarak toplanan ölçümleri eşlenebilir.
Bu KPI'ları ile yardımcı olabilir:
* Kaynak kullanımı ve kümenizi ölçeklendirme ya da hizmet işlemlerinizi en iyi duruma getirme amacıyla yük - anlama
* Altyapı sorunları - tahmin etmeye tahmin etmeye ve infrastructural sorunları tanılamak için g/ç ve CPU kullanımı, ağ gibi KPI'leri kullanabilmeniz için birçok sorunu ani değişiklikler (düşme) performans, öncesinde

Altyapı düzeyinde toplanan performans sayaçlarının listesi bulunabilir [performans ölçümleri](service-fabric-diagnostics-event-generation-perf.md). 

Uygulama düzeyi performansı izleme için Service Fabric Reliable Services ve Actors programlama modelleri için performans sayaçları kümesi sağlar. Bu modeller birini kullanıyorsanız, bu performans sayaçları, aktörler yukarı ve aşağı doğru dönmesini olduğunu ya da güvenilir hizmet isteklerinizi işlenmiş yeterince hızlı sağlamaya yardımcı olmak KPI'ları sağlayabilir. Bkz: [güvenilir hizmeti uzaktan iletişim için izleme](service-fabric-reliable-serviceremoting-diagnostics.md#performance-counters) ve [performans güvenilir aktörler için](service-fabric-reliable-actors-diagnostics.md#performance-counters) bunlar hakkında daha fazla bilgi için. Buna ek olarak, Application Insights ayrıca bir dizi performans ölçümleri toplar, uygulamanızla birlikte yapılandırdıysanız vardır.

Uygun performans sayaçlarını toplamak için OMS Aracısı'nı kullanın ve bu KPI'leri OMS günlük analizi görüntüleyin. Azure Tanılama Aracı Uzantısı (veya benzer herhangi bir aracı) toplamak ve analiz için ölçümler depolamak için de kullanabilirsiniz. 

## <a name="health-monitoring"></a>Sistem durumunu izleme
Service Fabric platformundan bir küme içindeki varlıkların durumunu Genişletilebilir sistem durumu raporlama sağlayan bir sistem durumu modeli içerir. Her düğüm, uygulama, hizmet, bölüm, çoğaltma veya örnek, bir sürekli olarak güncelleştirilebilir sistem durumu vardır. Sistem durumu, "Tamam", "Uyarı" veya "Error" ya da olabilir. Sistem durumu sorunları kümedeki göre her bir varlık için gösterilen durum raporu aracılığıyla değiştirilir. Varlıklarınızı sistem durumunu herhangi bir zamanda Service Fabric Explorer (SFX) aşağıda gösterildiği gibi denetlenebilir veya platformlar ait durum API üzerinden sorgulanabilir. Ayrıca, sistem durumu raporları özelleştirme ve kendi sistem durumu raporlarının ekleyerek veya sistem durumu API kullanarak bir varlık sistem durumunu değiştirebilirsiniz. Sistem durumu modeli hakkında daha fazla ayrıntı bulunabilir [Service Fabric sistem durumu izlemeye giriş](service-fabric-health-introduction.md).

![SFX sistem durumu Panosu](media/service-fabric-diagnostics-overview/sfx-healthstatus.png)

SFX son durumu raporlarına görmenin yanı sıra, her rapor bir olayı olarak da kullanılabilir. Sistem durumu olayları işletimsel kanal aracılığıyla toplanan (bakın [Azure Tanılama Olay toplama](service-fabric-diagnostics-event-aggregation-wad.md#log-collection-configurations)) ve uyarı ve gelecekte sorgulama için OMS günlük analizi depolanır. Bu nedenle uygun hatası senaryoları (OMS aracılığıyla özel uyarılar) için uyarıları ayarlama öneririz, uygulamanın kullanılabilirliğini etkileyebilecek sorunları algılamak yardımcı olur.

## <a name="monitoring-workflow"></a>İş akışı izleme 

Genel iş akışını izleme ve tanılama ve üç adımdan oluşur:

1. **Olay oluşturma**: Bu altyapı, platform (küme) ve uygulama düzeyinde (günlüklerini, izlemeleri, özel olaylar) olaylarını içerir
2. **Olay toplama**: Bu aşama, olayların Azure tanılama uzantısını veya EventFlow içeren olduğu analiz bunları, bir aracı şunun ardışık düzen etkili bir şekilde olduğu
3. **Analiz**: olayları analiz - görselleştirme, sorgulama, uyarı, vb. için izin vermek için bir aracında erişilebilir olması gerekir.

Birden çok ürün kullanılabilir, bu üç alanlarını kapsar ve her biri için farklı teknolojilerin seçmek boş. Yapıları kümeniz için uçtan uca bir izleme çözümü sunmak için birlikte çalışır olduğundan emin olmak önemlidir.

## <a name="event-generation"></a>Olay oluşturma

İlk izleme ve tanılama iş akışı oluşturma ve olay ve günlük verilerini nesil ayarlamak için adımdır. Bu olaylar, günlüklerini ve performans sayaçları tüm üç düzeylerini gösterilen: platform (olayları), Service Fabric işlemi temel alan kümesinden yayılan, uygulama düzeyinde (tüm uygulamalara eklenen araçları ve kümeye dağıtılan Hizmetleri) ve Altyapı düzeyi (performans ölçümleri her düğümden). Service Fabric bazı varsayılan araçlarını etkinleştirme karşın, bu düzeylerin her birinde toplanan tanılama verilerini özelleştirilebilir niteliktedir. 

Daha fazla bilgi edinin [platform düzeyi olayları](service-fabric-diagnostics-event-generation-infra.md) ve [uygulama düzeyinde olayları](service-fabric-diagnostics-event-generation-app.md) nasıl sağlandığını ve daha fazla izleme eklemek nasıl anlamak için. Burada yapmak zorunda ana uygulamanızı izlemek istediğiniz nasıl bir karardır. .NET uygulamaları için ILogger kullanmanızı öneririz, ancak EventSource, Serilog ve benzer diğer kitaplıkları gözden geçirebilirsiniz. Java için Log4j kullanmanızı öneririz. Bunlar, uygulamanın niteliğine göre kullanılabilecek birkaç kullanılabilen diğer seçenekler vardır. Ne belirli kullanım durumu için en iyi ve en deneyimli bir tane seçin keşfetmek çekinmeyin kullanma. 

Kullanmak istediğiniz oturum açma sağlayıcısı üzerinde bir karar yaptıktan sonra günlüklerinizi olan emin olmak ihtiyacınız toplanır ve doğru bir şekilde depolanır.

## <a name="event-aggregation"></a>Olay toplama

Uygulamalarınızı ve kümeniz tarafından oluşturulan olayları ve günlükleri toplamak için genellikle kullanmanızı öneririz [Azure tanılama uzantısını](service-fabric-diagnostics-event-aggregation-wad.md) (daha benzer tabanlı aracı günlük toplama) veya [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) (işlem içi oturum koleksiyonu). Application Insights kullanıyorsanız ve .NET veya Java geliştirme sonra EventFlow yerine Application Insights SDK'sı kullanılması önerilir.

Benzer bir işin çoğu durumda, bunlardan yalnızca birini kullanarak ile yapılması mümkün olmakla birlikte bir işlemdeki koleksiyonu sahip işlem hattı (AI SDK veya EventFlow) Azure tanılama uzantı aracısı birleştirerek daha güvenilir, kapsamlı, izleme iş akışına müşteri adayları . Uygulama düzeyi günlükleriniz için AI SDK veya EventFlow (işlemdeki toplama) kullanırken Azure tanılama uzantı aracısı yolunuzu platform düzeyi olayları için olacaktır. 

Bunlardan yalnızca birini kullanmak istediğiniz durumda da, dikkate alınması gereken bazı önemli noktalar şunlardır.
* Azure tanılama uzantısını kullanarak uygulama günlükleri toplamayı iyi Service Fabric Hizmetleri için günlük kaynakları ve hedeflere kümesini genellikle değiştirmez ve kaynakları ve hedeflerine arasında basit bir eşleme varsa bir seçenektir. Azure tanılama bu yapılandırma için yapılandırmaya önemli değişiklikler yapmak, tüm küme güncelleştirilmesini gerektirir şekilde nedeni Resource Manager katmanında gerçekleşir. Bu, genellikle AI SDK veya EventFlow erişmekten daha az verimli olması sona ereceğini anlamına gelir.
* EventFlow kullanarak kendi günlüklerini çözümleme ve görselleştirme bir platform için doğrudan ve/veya depolama gönderme Hizmetleri sahip olmanızı sağlar. Diğer kitaplıkları (ILogger, Serilog, vb.) de aynı amaçla kullanılan, ancak EventFlow Service Fabric Hizmetleri desteklemek için ve işlem günlük toplama için özellikle tasarlanmış avantajına sahiptir. Bu yapılandırma ve dağıtım kolaylığı açısından çeşitli avantajları vardır eğilimindedir ve farklı günlük kitaplıkları ve çözümleme araçları desteklemek için daha fazla esneklik sunar. 

## <a name="event-analysis"></a>Olay analizi

Çözümleme ve görselleştirme izleme ve tanılama veri geldiğinde pazarında mevcut birkaç harika platformları vardır. Öneririz iki olan [OMS](service-fabric-diagnostics-event-analysis-oms.md) ve [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) Service Fabric ile bunların tümleştirme nedeniyle. İçine görünmelidir [esnek yığın](https://www.elastic.co/products) (özellikle, bir küme çevrimdışı bir ortamda çalışan düşünüyorsanız), [Splunk](https://www.splunk.com/), ya da herhangi bir platform, tercih. 

Seçtiğiniz önemli noktaları herhangi bir platform için nasıl, kullanıcı arabirimi ve seçenekleri görselleştirmek ve kolay okunabilir panolar ve izlemenizi geliştirmek için sağladıkları ek araçlar oluşturma olanağı sorgulama deneyimliyseniz içermelidir, Otomatik uyarı gibi.

Seçtiğiniz platform yanı sıra bir Service Fabric kümesi bir Azure kaynağı olarak ayarladığınızda, ayrıca Azure'nın Giden kutusu performans makineleriniz için izleme erişim elde edersiniz.

### <a name="azure-monitor"></a>Azure İzleyici

Kullanabileceğiniz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) Service Fabric kümesi yerleşik Azure kaynaklarını çoğunu izlemek için. Ölçümler için bir dizi [sanal makine ölçek kümesi](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) ve tek tek [sanal makineleri](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) otomatik olarak toplanır ve Azure Portalı'nda görüntülenir. Azure portalında toplanan bilgileri görüntülemek için Service Fabric kümesi içeren kaynak grubunu seçin. Ardından görüntülemek istediğiniz sanal makine ölçek kümesini seçin. İçinde **izleme** bölümüne (sol gezinti), select **ölçümleri** grafik değerleri görüntülemek için.

![Toplanan ölçüm bilgileri Azure portal görünümü](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Grafikler özelleştirmek için yönergeleri izleyin [Microsoft Azure ölçümlerini](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Bu ölçümleri temel uyarılar açıklandığı gibi oluşturabilirsiniz [oluşturma uyarıları Azure İzleyicisi'nde için Azure services](../monitoring-and-diagnostics/monitoring-overview-alerts.md). 

## <a name="next-steps"></a>Sonraki adımlar

* Platform ve Service Fabric sağlar için önceden olayları izleme hakkında daha fazla bilgi [Platform düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-infra.md)
* Uygulamaları ile çalışmaya başlama için bkz: [uygulama düzeyi olay ve günlük oluşturma](service-fabric-diagnostics-event-generation-app.md)
* AI uygulamanızla birlikte ayarlamak için adımları gidin [İzleyici ve Service Fabric bir ASP.NET Core uygulama tanılama](service-fabric-tutorial-monitoring-aspnet.md)
* OMS günlük analizi kapsayıcıları izleme için ayarlama hakkında bilgi edinmek- [izleme ve tanılama için Windows kapsayıcılarında Azure Service Fabric](service-fabric-tutorial-monitoring-wincontainers.md)

