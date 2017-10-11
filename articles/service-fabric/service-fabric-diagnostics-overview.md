---
title: "Azure Service Fabric izleme ve tanılama genel bakış | Microsoft Docs"
description: "İzleme ve tanılama Azure Service Fabric kümeleri, uygulamalar ve hizmetler hakkında bilgi edinin."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/17/2017
ms.author: dekapur
ms.openlocfilehash: 88f4a23f89a1c8fd88db1df3a7ff03ae5df64c0f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2017
---
# <a name="monitoring-and-diagnostics-for-azure-service-fabric"></a>İzleme ve tanılama Azure Service Fabric için

İzleme ve tanılama geliştirme, test ve uygulamaları ve Hizmetleri herhangi bir ortamda dağıtmak için kritik öneme sahiptir. Service Fabric çözümleri, planlama ve izleme uygulama Yardım tanılama uygulamaları emin olmak ve Hizmetleri yerel geliştirme ortamında veya üretim beklendiği gibi çalıştığını en iyi çalışır.

İçin izleme ve tanılama başlıca amaçları şunlardır:
* Donanım ve altyapı sorunlarını tanılamak ve Algıla
* Hizmeti kapalı kalma süresini kısaltabilir, yazılım ve uygulama sorunlarını Algıla
* Kaynak tüketimi ve Yardım sürücü işlemleri kararları anlama
* Uygulama, hizmet ve altyapı performansı en iyi duruma getirme
* İşletme öngörüleri oluşturmak ve geliştirme alanlarını tanımlayacak


Genel iş akışını izleme ve tanılama ve üç adımdan oluşur:

1. **Olay oluşturma**: Bu altyapı (küme), platform ve uygulama / hizmet düzeyinde (günlüklerini, izlemeleri, özel olaylar) olaylarını içerir
2. **Olay toplama**: oluşturulan olaylar gereken toplanacağı ve bunların görüntülenebilmesi bir araya getirilir
3. **Analiz**: olayları görselleştirilmiş ve analiz için izin vermek ve gerektiği gibi görüntülemek için bazı biçiminde erişilebilir olması gerekir

Birden çok ürün kullanılabilir, bu üç alanlarını kapsar ve her biri için farklı teknolojilerin seçmek boş. Yapıları kümeniz için uçtan uca bir izleme çözümü sunmak için birlikte çalışır olduğundan emin olmak önemlidir.

## <a name="event-generation"></a>Olay oluşturma

İlk izleme ve tanılama iş akışı oluşturma ve olayları ve günlükleri nesil adımdır. Bu olaylar, günlüklerine ve izlemelere iki düzeyde oluşturulur: (küme, makineler veya Service Fabric Eylemler dahil) platformu katmanı veya uygulama katmanı (tüm uygulamalara eklenen araçları ve kümeye dağıtılan Hizmetleri). Service Fabric bazı araçları varsayılan olarak sağlar ancak bu düzeylerin her birinde özelleştirilebilir olaylardır.

Daha fazla bilgi edinin [platform düzeyi olayları](service-fabric-diagnostics-event-generation-infra.md) ve [uygulama düzeyinde olayları](service-fabric-diagnostics-event-generation-app.md) nasıl sağlandığını ve daha fazla izleme eklemek nasıl anlamak için.

Kullanmak istediğiniz oturum açma sağlayıcısı üzerinde bir karar yaptıktan sonra günlüklerinizi olan emin olmak ihtiyacınız toplanır ve doğru bir şekilde depolanır.

## <a name="event-aggregation"></a>Olay toplama

Uygulamalarınızı ve kümeniz tarafından oluşturulan olayları ve günlükleri toplamak için genellikle kullanmanızı öneririz [Azure tanılama](service-fabric-diagnostics-event-aggregation-wad.md) (daha benzer tabanlı aracı günlük toplama) veya [EventFlow](service-fabric-diagnostics-event-aggregation-eventflow.md) () işlem içi oturum koleksiyonu).

Azure tanılama uzantısını kullanarak uygulama günlükleri toplamayı iyi Service Fabric Hizmetleri için günlük kaynakları ve hedeflere kümesini genellikle değiştirmez ve kaynakları ve hedeflerine arasında basit bir eşleme varsa bir seçenektir. Azure tanılama bu yapılandırma için yapılandırmasında yapılan önemli değişiklikler yapılmadan küme güncelleştirme/yükleyecek gerektiren şekilde nedeni Resource Manager katmanında gerçekleşir. Ayrıca, en iyi günlüklerinizi emin yaparken kullanıldığı depolanıyor burada bunlar çeşitli analiz platformlar tarafından erişilebilen öğesinden biraz daha kalıcı bir yere. Bu, EventFlow gibi bir seçenekle erişmekten daha ardışık düzen daha az verimli olması sona ereceğini anlamına gelir.

Kullanarak [EventFlow](https://github.com/Azure/diagnostics-eventflow) kendi günlüklerini çözümleme ve görselleştirme bir platform için doğrudan ve/veya depolama gönderme Hizmetleri sahip olmanızı sağlar. Diğer kitaplıkları (ILogger, Serilog, vb.) aynı amaçla kullanılan, ancak EventFlow Service Fabric Hizmetleri desteklemek için ve işlem günlük toplama için özellikle tasarlanmış avantajına sahiptir. Bu, birkaç olası avantajlara sahip olursunuz eğilimi gösterir:

* Kolay yapılandırma ve dağıtma
    * Tanılama veri toplama yalnızca yapılandırmadır hizmet yapılandırmasının bir parçası. Her zaman "eşitleme" uygulama kalanıyla korumak kolaydır.
    * Her uygulama veya hizmet başına yapılandırma kolayca elde edilebilir
    * EventFlow aracılığıyla veri hedefleri olan uygun NuGet paketi ekleme ve değiştirme yalnızca sağlasa da *eventFlowConfig.json* dosyası
* Esneklik
    * Gitmesi gereken her yerde uygulama verileri gönderebilir, bir istemci kitaplığı var olduğu sürece, hedeflenen verileri depolama sistemi destekler. Yeni hedefler eklenebilir istenen şekilde
    * Karmaşık yakalama, filtreleme ve veri toplama kuralları uygulanabilir
* İç uygulama verilerini ve içerik erişimi
    * Uygulama/hizmet işlemi içinde çalışan tanılama alt sistemi kolayca bağlamsal bilgilerle izlemeleri genişletebilirsiniz

Not etmek için bir iki Bu seçenekler birbirini dışlamaz ve benzer bir işin birini veya diğerini kullanarak ile yapılması mümkün olsa da, aynı zamanda hem ayarlamanız için anlamlı şeydir. Çoğu durumda, işlem içi koleksiyonuyla bir aracı birleştirerek daha güvenilir izleme iş akışına neden olabilir. EventFlow (işlemdeki toplama) kullanabilirken Azure tanılama uzantısını (aracı), uygulama düzeyinde günlükleri için seçilen yolunuzu platform düzeyi günlükleri için olabilir. Nelerin sizin için en iyi dahil edilir sonra onu görüntülenir ve analiz için verilerinizi nasıl istediğiniz hakkında düşünmeye saattir.

## <a name="event-analysis"></a>Olay çözümleme

Çözümleme ve görselleştirme izleme ve tanılama veri geldiğinde pazarında mevcut birkaç harika platformları vardır. Öneririz iki olan [OMS](service-fabric-diagnostics-event-analysis-oms.md) ve [Application Insights](service-fabric-diagnostics-event-analysis-appinsights.md) nedeniyle, daha iyi tümleştirme Service Fabric, ancak ayrıca içine görünmelidir [esnek yığın](https://www.elastic.co/products) () Özellikle, bir küme çevrimdışı bir ortamda çalışan düşünüyorsanız), [Splunk](https://www.splunk.com/), ya da herhangi bir platform, tercih.

Seçtiğiniz önemli noktaları herhangi bir platform için nasıl, kullanıcı arabirimi ve seçenekleri görselleştirmek ve kolay okunabilir panolar ve izlemenizi geliştirmek için sağladıkları ek araçlar oluşturma olanağı sorgulama deneyimliyseniz içermelidir, Otomatik uyarı gibi.

Seçtiğiniz platform yanı sıra bir Service Fabric kümesi bir Azure kaynağı olarak ayarladığınızda, ayrıca Azure'nın belirli bir performans için yararlı olabilecek makineler için giden kutusunu izlemek ve ölçüm izleme erişim elde edersiniz.

### <a name="azure-monitor"></a>Azure İzleyici

Kullanabileceğiniz [Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview.md) Service Fabric kümesi yerleşik Azure kaynaklarını çoğunu izlemek için. Ölçümler için bir dizi [sanal makine ölçek kümesi](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets) ve tek tek [sanal makineleri](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesetsvirtualmachines) otomatik olarak toplanır ve Azure Portalı'nda görüntülenir. Azure portalında toplanan bilgileri görüntülemek için Service Fabric kümesi içeren kaynak grubunu seçin. Ardından görüntülemek istediğiniz sanal makine ölçek kümesini seçin. İçinde **izleme** bölümünde, select **ölçümleri** grafik değerleri görüntülemek için.

![Toplanan ölçüm bilgileri Azure portal görünümü](media/service-fabric-diagnostics-overview/azure-monitoring-metrics.png)

Grafikler özelleştirmek için yönergeleri izleyin [Microsoft Azure ölçümlerini](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md). Bu ölçümleri temel uyarılar açıklandığı gibi oluşturabilirsiniz [oluşturma uyarıları Azure İzleyicisi'nde için Azure services](../monitoring-and-diagnostics/insights-alerts-portal.md). Bölümünde açıklandığı gibi web kancaları kullanarak bir bildirim hizmeti uyarıları gönderebilirsiniz [bir web kancası Azure ölçüm uyarıyı yapılandırmak](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure İzleyici yalnızca bir abonelik destekler. Birden çok abonelik izlemeniz gerekiyorsa ya da ek özellikler gerekiyorsa [günlük analizi](https://azure.microsoft.com/documentation/services/log-analytics/), kısmen Microsoft Operations Management Suite hem şirket içi ve bulut tabanlı bir bütünsel BT yönetim çözümü sunar Altyapı. Tek bir yerde, tüm ortamınız için ölçümleri ve günlükleri görebilmeniz için günlük analizi için doğrudan Azure İzleyicisi'nden veri yönlendirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

### <a name="watchdogs"></a>Watchdogs

Bir izleme, sistem durumu izleyebilir ve hizmetler ve rapor sistem yükü sistem durumu modeli hiyerarşi içinde herhangi bir şey için ayrı bir hizmettir. Bu, tek bir hizmeti görünümü temel alarak algılanmayacağı hataları önlemeye yardımcı olabilir. Watchdogs de iyi (örneğin, belirli zaman aralıklarında depolama günlük dosyalarında temizleniyor) kullanıcı etkileşimi olmadan düzeltici eylemleri gerçekleştirir konak koduna yerlerdir. Bir örnek izleme hizmeti uygulaması bulabilirsiniz [burada](https://github.com/Azure-Samples/service-fabric-watchdog-service).

Nasıl olayları ve günlükleri sırasında oluşturulan anlama ile çalışmaya başlama [platform düzeyi](service-fabric-diagnostics-event-generation-infra.md) ve [uygulama düzeyinde](service-fabric-diagnostics-event-generation-app.md).