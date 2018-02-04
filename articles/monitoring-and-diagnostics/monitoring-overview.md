---
title: "Azure uygulamaları ve kaynakları izleme | Microsoft Docs"
description: "Farklı Microsoft Hizmetleri işlevselliği ve Azure Hizmetleri ve uygulamaları için tam bir izleme stratejisi katkıda genel bakış."
author: robb
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2018
ms.author: robb,bwren
ms.openlocfilehash: 3ab7d2d5c3b95d215f3ee9eb9346e8a7895e734c
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/03/2018
---
# <a name="monitoring-azure-applications-and-resources"></a>Azure uygulamaları ve kaynakları izleme

İzleme, toplama ve performans, sistem durumu ve iş uygulamanız ve bağımlı kaynakları kullanılabilirliğini belirlemek için verileri analiz etme işlemidir. Etkili izleme stratejisi, uygulamanızın ve böylece sorun haline gelmeden önce bunları çözümleyebilirsiniz proaktif olarak, kritik sorunlar bildirerek, çalışma süresini artırmak için farklı bileşenlerin ayrıntılı çalışmasını anlamanıza yardımcı olur.

Tek tek bir spesifik rol ya da görev İzleme alanı gerçekleştirmek ve birlikte toplama, çözümleme ve uygulamanız ve temel Azure kaynaklarını telemetrisinden işlevi gören için kapsamlı bir çözüm ileten birden çok hizmet Azure içerir bunları destekleyen.  Karma bir ortamı izleme sağlamak için kritik şirket içi kaynakları izlemek için de çalışabilir.   Araçlar ve kullanılabilir verileri anlamak, uygulamanız için tam bir izleme stratejisi geliştirme ilk adımdır. 

Aşağıdaki diyagramda, Azure kaynaklarının izleme sunmak için birlikte çalışan farklı bileşenlere kavramsal bir görünüm gösterir.  Bunların her birini ayrıntılı teknik bilgi için bağlantılar ile aşağıdaki bölümlerde açıklanmıştır.

![İzlemeye genel bakış](media/monitoring-overview/overview.png)

## <a name="basic-monitoring"></a>Temel İzleme
Temel izleme temel gerekli Azure kaynaklarını izleme sağlar.  Bu hizmetler minimal yapılandırma gerektirir ve hizmetlerini izleme premium tarafından işlevden çekirdek telemetri toplar.    

### <a name="azure-monitor"></a>Azure İzleyici
[Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) temel Azure hizmeti için izlenmesi koleksiyonunu vererek etkinleştirir [ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), ve [tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).  Örneğin, etkinlik günlüğü yeni kaynaklar olduğunda oluşturulan veya değiştirilen söyler.  Ölçümleri kullanılabilir farklı kaynaklar ve hatta bir sanal makine içinde işletim sistemi performans istatistiklerini sağlayan.  Azure portalında gezginler biri ile bu verileri görüntüleme, oluşturan eğilim ve ayrıntılı çözümleme için günlük analizi için gönderme veya önceden kritik sorunlar bildiren uyarı kuralları oluşturmak.

### <a name="service-health"></a>Hizmet Durumu
Sistem, uygulamanızın bağımlı Azure hizmetlerini kullanır.  [Azure hizmet durumu](../service-health/service-health-overview.md) uygulama ve aynı zamanda tüm zamanlama bakım planı yardımcı etkileyebilecek Azure hizmetleriyle ilgili sorunları tanımlar.

### <a name="azure-advisor"></a>Azure Advisor
[Azure Danışmanı](../advisor/advisor-overview.md) en iyi uygulamalarına göre önerileri kişiselleştirilmiş sağlamak için kaynak yapılandırma ve kullanım telemetrisi sürekli olarak izler.  Bu öneriler Yardım performans, güvenlik ve uygulamalarınızı destekleyen kaynaklarda kullanılabilirliğini artırır.


## <a name="premium-monitoring-services"></a>Premium izleme Hizmetleri
Aşağıdaki Azure hizmetlerini toplama ve izleme verilerini çözümleme için zengin özellikleri sağlar.  Bunlar temel izleme ve Dengeleme ortak işlevselliği azure'da oluşturmak ve güçlü analytics, uygulamalar ve altyapı benzersiz Öngörüler vermek için toplanan verileri sağlar.  Bunlar farklı izleyiciler için hedeflenen belirli senaryolar bağlamında verileri görüntülemektedir.

### <a name="application-insights"></a>Application Insights
[Application Insights](http://azure.microsoft.com/documentation/services/application-insights) Bulut veya şirket içi barındırılan olup olmadığını İzleyici kullanılabilirliği, performansı ve kullanımı, uygulamanızın sağlar.  Application Insights ile çalışmak için uygulamanızın işaretlenerek hızla tanımlayın ve bunları raporlamak bir kullanıcı için beklemeden hataları tanılamak olanak tanıyan ayrıntılı Öngörüler elde edebilirsiniz. Topladığınız bilgileri kullanarak, uygulamanızın Bakım ve geliştirmeler hakkında bilgi sahibi seçimler yapabilirsiniz.  Topladığı veri ile etkileşim için kapsamlı araçlara ek olarak, Application Insights uyarıları, panolar ve derin çözümleme günlük analizi sorgu dili gibi paylaşılan işlevsellik yararlanmak için ortak bir depo verilerini depolar.

### <a name="log-analytics"></a>Log Analytics
[Günlük analizi](http://azure.microsoft.com/documentation/services/log-analytics) Azure nerede analiz güçlü sorgu dili ile tek bir depoda çeşitli kaynaklar veri toplayarak izleme merkezi bir rol oynar.  Application Insights ve Azure Güvenlik Merkezi günlük verileri depolamak ve analiz altyapısı yararlanan analizi verilerini depolar.  Bu yönetim çözümleri Azure İzleyicisi'nden toplanan verilerle birlikte ve bulutta veya şirket içi sanal makinelerde yüklü aracıları, tüm ortamınız eksiksiz bir görünümünü oluşturmak izin verin. 


### <a name="service-map"></a>Hizmet Eşlemesi
[Hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) kendi farklı işlemler ve diğer bilgisayarlardaki ve dış işlemlere bağımlılıkları olan sanal makineleri çözümleyerek Iaas ortamınız hakkında bilgi sağlar.  Her bilgisayar ve, ortamın geri kalanının ilişkisi bağlamında bu verileri görüntüleyebilmek olayları, performans verileri ve günlük analizi yönetim çözümlerine tümleştirir.  Hizmet eşlemesi benzer [Application Insights uygulama eşlemesinde](../application-insights/app-insights-app-map.md) ancak uygulamalarınızı destekleyen altyapı bileşenlerine odaklanır.

### <a name="network-watcher"></a>Ağ İzleyicisi
[Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md) Azure senaryolarda farklı bir ağ için senaryo tabanlı izleme ve tanılama sağlar.  Azure ölçümleri ve daha fazla çözümleme için tanılama veri depolar ve birlikte çalıştığı [günlük analizi yönetim çözümlerine](../log-analytics/log-analytics-azure-networking-analytics.md) tam ağ kaynaklarınızın izleme.


### <a name="management-solutions"></a>Yönetim çözümleri
[Yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md) belirli bir uygulama veya hizmet için bilgiler sunan paketlenmiş mantığı kümeleridir.  Bunlar, depolamak ve bunlar toplamak izleme verileri çözümlemek için günlük analizi üzerinde kullanır.  Microsoft ve izleme için çeşitli Azure ve üçüncü taraf hizmetleri sağlayan ortaklarından yönetim çözümleri kullanılabilir. Çözümlerini izleme örneği dahil [kapsayıcı izleme](../log-analytics/log-analytics-containers.md) yardımcı olan görüntülemenize ve kapsayıcı konaklarınızın yönetmenize ve [Azure SQL analizi](../log-analytics/log-analytics-azure-sql.md) toplar ve SQL için performans ölçümleri visualizes Azure veritabanları.


## <a name="shared-functionality"></a>Paylaşılan işlevi
Aşağıdaki Azure Araçları hizmetlerini izleme Premium'a kritik işlevler sağlar.  Ortak işlevsellik ve yapılandırmalar birden fazla hizmet yararlanan olanak tanıyan birden fazla hizmet tarafından paylaşılır.

### <a name="alerts"></a>Uyarılar
[Azure uyarıları](../monitoring-and-diagnostics/monitoring-overview-alerts.md) proaktif olarak kritik koşulları size bildirir ve potansiyel olarak düzeltme eylemlerini gerçekleştirin.  Uyarı kuralları ölçümleri ve günlükleri de dahil olmak üzere birden çok kaynaktan veri yararlanabilirsiniz. Kullandıkları [Eylem grupları](../monitoring-and-diagnostics/monitoring-action-groups.md) alıcıları ve yanıt olarak bir uyarı eylemleri benzersiz kümesini içerir.  Gereksinimlerinize bağlı olarak, Web kancalarını kullanarak dış eylemleri başlatın ve ITSM araçlarıyla tümleştirmenize uyarıları olabilir.

### <a name="dashboards"></a>Panolar
[Azure panolar](../azure-portal/azure-portal-dashboards.md) , Azure diğer kullanıcılarla paylaşım ve Azure Portalı'ndaki tek bir bölmesine farklı türde verileri birleştirmenize izin verir.  Örneğin, bir grafik ölçümleri, etkinlik günlükleri tablosu, Application Insights kullanım grafikten ve günlük arama çıktısını günlük analizi gösteren döşeme birleştiren bir Pano oluşturabilirsiniz.

Günlük analizi veri dışa aktarabilirsiniz [Power BI](https://docs.microsoft.com/power-bi/) ek görselleştirmeleri yararlanmak için ve ayrıca verileri başkalarına içinde ve kuruluşunuz dışında kullanılabilmesi için.

### <a name="metrics-explorer"></a>Ölçüm Gezgini
[Ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) sayısal yardımcı Azure kaynaklar tarafından üretilen değerler anlamak işlemi ve kaynak performansını şunlardır. Ölçümler için günlük analizi veri çözümleme için diğer kaynaklardan gönderebilirsiniz.



### <a name="activity-logs"></a>Etkinlik Günlükleri
[Etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) Azure kaynaklarını işlemi hakkında veriler sağlar.  Bu tür daha iyi kaynak kullanan bilgi olarak kaynak, hizmet sistem durumu olayları, öneriler yapılandırma değişikliklerini ve otomatik ölçeklendirme işlemleriyle ilgili bilgileri içerir.  Etkinlik günlüğü Explorer'da birden fazla kaynak Azure portal ya da Görünüm günlüklerinden kendi sayfasında belirli bir kaynak için günlükleri görüntüleyebilirsiniz.  Yönetim çözümleri, sanal makinelerde aracıları ve diğer kaynakları tarafından toplanan verilerle çözümlenebilir şekilde, etkinlik günlükleri için günlük analizi gönderebilirsiniz.


## <a name="example-scenarios"></a>Örnek senaryolar
Aşağıda nasıl farklı senaryolar için Azure farklı izleme araçları yararlanan gösteren yüksek düzey örnekler verilmiştir.

### <a name="monitoring-a-web-application"></a>Bir web uygulaması izleme
Uygulama Hizmetleri, Azure Storage ve SQL veritabanı kullanarak Azure'da dağıtılan bir web uygulaması göz önünde bulundurun.  Erişerek başlamasının [ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) Azure portalında kendi sayfalarında tek tek bu kaynakların her biri için.  Bu uygulama ve yapılandırma değişikliklerini tanımlamak yanı sıra ortalama yanıt süresi için istek sayısı gibi önemli bilgiler içerir.

Ölçümleri ve farklı kaynaklar için günlükleri birlikte görüntüleyebilmek için portalda İzleyicisi sonra gidebilir.  Ölçümleri standart parametrelerini belirlemek, [uyarı kuralları oluşturmak](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) ortalama yanıt süresi eşiği arttığında, örneğin, önceden size bildirilmesini.  Uygulamanızın günlük performansını hızlı bir görünümünü alabilmek için kritik KPI'ları temsil eden ölçümleri grafikleri göstermeyi Azure bir pano oluşturun.

Uygulamanızın veya daha ayrıntılı izleme gerçekleştirmek için [için Application Insights yapılandırma](../application-insights/quick-monitor-portal.md).  Artık daha fazla işlemi ve uygulamanızın performansını sağlayan ek verileri toplayabilir.  Application Insights görsel izin vermeyi, uygulamanızın bileşenleri arasındaki temel ilişkileri algılar [uygulama eşlemesi](../application-insights/app-insights-app-map.md) ile birlikte [uçtan uca izleme](../application-insights/app-insights-transaction-diagnostics.md) tanılamak için tam bileşeni, bağımlılık veya özel durum burada bir sorun oluştu.  Oluşturduğunuz [kullanılabilirlik testleri](../application-insights/app-insights-monitor-web-app-availability.md) proaktif olarak farklı bölgelerdeki uygulamanızı test etmek için.  Geliştiricilere yardımcı olmak için [profil oluşturucu etkinleştirmek](../application-insights/enable-profiler-compute.md) böylece istekleri ve belirli satırlık bir kod için özel durumlar izleyebilirsiniz.  

Daha fazla uygulamanızda kullanılan hizmetler görünürlük elde etmek için eklediğiniz [SQL analiz çözümü](../log-analytics/log-analytics-azure-sql.md) günlük analizi ek verileri toplamak için. Bir süre sonra ne zaman sitesinde performans eşiğin altına düştü dönemleri kök nedeni araştırın karar verin.  Kullanım ve performans verilerini ilişkilendirmek için günlük analizi kullanarak bir sorgu tarafından Application Insights yapılandırması ve performans ile uygulamanızı destekleyen Azure kaynakları arasında toplanan veriler yazma.


### <a name="monitoring-virtual-machines"></a>Sanal makineleri izleme
Windows ve Linux Azure üzerinde çalışan sanal makineler karışımına sahip.  Görüntülemek için Azure İzleyicisi'ni kullanın [etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve [ana bilgisayar düzeyindeki ölçümlerini](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve ardından ekleyin [Azure tanılama uzantısını](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) ölçümleri toplamak için sanal makineler Konuk işletim sisteminden.  Ardından oluşturduğunuz [uyarı kuralları](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) proaktif olarak temel olduğunda bildirmek için ölçümleri bu tür işlemci kullanımı ve bellek eşikleri arası.

Bir iş uygulaması çalıştıran sanal makineler hakkında daha fazla bilgi toplamak için [günlük analizi çalışma alanı oluşturma ve VM uzantısı etkinleştirme](../log-analytics/log-analytics-quick-collect-azurevm.md) her makinede.  Yapılandırdığınız [farklı veri kaynakları koleksiyonunu](../log-analytics/log-analytics-data-sources.md) uygulamanız için ve [görünümleri oluşturma](../log-analytics/log-analytics-view-designer.md) kendi günlük işlemi ve performans bildirmek için.  Daha sonra [uyarı kuralları oluşturmak](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) belirli hata olayları alındığında sizi bilgilendirmek üzere.  Sürekli olarak yüklü aracı durumunu izlemek için eklediğiniz [Aracısı sistem yönetimi çözümü](../operations-management-suite/oms-solution-agenthealth.md).

Daha fazla uygulama kavramanıza için [bağımlılık aracısı ekleme](../operations-management-suite/operations-management-suite-service-map-configure.md) bunlara eklemek için sanal makinelere [hizmet Haritası](../operations-management-suite/operations-management-suite-service-map.md).  Önemli işlemlerin bulur ve diğer hizmetlerle makineler arasındaki bağlantıları tanımlar.  Bildirilen bir kesinti sonra adli sorun yaşayan belirli makineler tanımlayacak gerçekleştirmek için hizmet Haritası kullanın.  Ardından oluşturduğunuz bir [günlük analizi veri sorgusu](../log-analytics/log-analytics-log-search-new.md) sorunun gelecekte tanımlamak ve koşulu algıladığında proaktif olarak bildiren bir uyarı kuralı oluşturmak için.



## <a name="next-steps"></a>Sonraki adımlar
Şu konular hakkında daha fazla bilgi edinin:

* [Azure İzleyicisi'nde Ignite 2016'den video](https://myignite.microsoft.com/videos/4977)
* [Azure İzleyicisi ile çalışmaya başlama](monitoring-get-started.md)
* [Azure tanılama](../azure-diagnostics.md) çalışıyorsanız, bulut hizmeti, sanal makine sorunlarını tanılamak sanal makine ölçeklendirme ayarlayın veya Service Fabric uygulaması.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) App Service Web uygulamanızda tanılama sorunları çalışıyorsanız.
* [Günlük analizi](https://azure.microsoft.com/documentation/services/log-analytics/) toplanan izleme verileri çözümlemek için.
