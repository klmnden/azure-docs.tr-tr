---
title: Azure uygulamalarını ve kaynaklarını izleme
description: Azure hizmetleriniz ve uygulamalarınız için eksiksiz bir izleme stratejisine katkıda bulunan Microsoft hizmetleri ve işlevlerine genel bakış.
author: rboucher
services: azure-monitor
ms.service: azure-monitor
ms.topic: overview
ms.date: 03/05/2018
ms.author: robb,bwren
ms.component: ''
ms.openlocfilehash: e6adcc136c273210cc40d23ed2cb177287654005
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2018
ms.locfileid: "35265141"
---
# <a name="monitoring-azure-applications-and-resources"></a>Azure uygulamalarını ve kaynaklarını izleme

İzleme, iş uygulamalarınızın ve bağımlı oldukları kaynakların performansını, sistem durumunu ve kullanılabilirliğini belirlemeye yönelik verileri toplayıp analiz etme eylemidir. Etkili bir izleme stratejisi, uygulamanız için bileşenlerin ayrıntılı işlemini anlamanıza yardımcı olur. Ayrıca sorunları büyümeden çözümleyebilmeniz için kritik sorunları proaktif şekilde size bildirerek çalışma sürenizi artırmanıza da yardımcı olur.

Azure, izleme alanında belirli bir rolü veya görevi tek tek gerçekleştiren birden çok hizmet içerir. Bu hizmetler birlikte, uygulamanız için telemetri verilerini toplama, analiz etme ve bu verilere göre eylemde bulunmaya ilişkin kapsamlı bir çözüm ve bunları destekleyen Azure kaynaklarını sunar. Karma bir izleme ortamı sağlamak için kritik şirket içi kaynakları izlemek için de çalışır. Kullanılabilir olan araç ve verilerin anlaşılması, uygulamanız için eksiksiz bir izleme stratejisi geliştirmenin ilk adımıdır.

Aşağıdaki diyagramda, Azure kaynaklarının izlenmesini sağlamak için birlikte çalışan bileşenlerin kavramsal bir görünümü gösterilmektedir. Aşağıdaki bölümlerde bu bileşenler açıklanmakta ve ayrıntılı teknik bilgilerin bağlantıları sağlanmaktadır.

![İzlemeye genel bakış](media/monitoring-overview/monitoring-products-overview.png)


## <a name="shared-capabilities"></a>Paylaşılan Özellikler
Temel ve ayrıntılı izleme hizmeti, aşağıdaki özellikleri sağlayan işlevleri paylaşır.

### <a name="alerts"></a>Uyarılar
[Azure uyarıları](../monitoring-and-diagnostics/monitoring-overview-alerts.md) size proaktif olarak kritik koşulları bildirir ve düzeltici eylemler uygular. Uyarı kuralları, ölçümler ve günlükler de dahil olmak üzere birden çok kaynaktan verileri kullanabilir. Bir uyarıya yanıt olarak benzersiz alıcıları ve eylemleri içeren [eylem gruplarını](../monitoring-and-diagnostics/monitoring-action-groups.md) kullanır. Gereksinimlerinize bağlı olarak, web kancalarını kullanıp uyarıların dış eylemler başlatmasını ve ITSM araçlarınızla tümleştirilmesini sağlayabilirsiniz.

### <a name="dashboards"></a>Panolar
Farklı türde verileri [Azure portalında](../azure-portal/azure-portal-dashboards.md) tek bir bölgede birleştirmek için [Azure panolarını](https://portal.azure.com) kullanabilirsiniz. Daha sonra panoyu diğer Azure kullanıcılarıyla paylaşabilirsiniz.

Örneğin, şunları birleştiren bir pano oluşturabilirsiniz:
- Ölçümlerin grafiğini gösteren kutucuklar
- Etkinlik günlüklerinin tablosu
- Application Insights’ın kullanım grafiği
- Log Analytics’te günlük araması çıktısı

Log Analytics verilerini [Power BI](https://docs.microsoft.com/power-bi/)’a da dışarı aktarabilirsiniz. Burada, ek görselleştirmelerden yararlanabilirsiniz. Ayrıca verileri kuruluşunuzun içindeki ve dışındaki başka kişilerin de kullanımına sunabilirsiniz.

### <a name="metrics-explorer"></a>Ölçüm Gezgini
[Ölçümler](../monitoring-and-diagnostics/monitoring-overview-metrics.md), kaynağın işlemini ve performansını anlamanıza yardımcı olması için bir Azure kaynağı tarafından oluşturulan sayısal değerlerdir. Ölçüm Gezgini’ni kullanarak, diğer kaynaklardan gelen verilerin analizi için Log Analytics’e ölçümler gönderebilirsiniz.


## <a name="core-monitoring"></a>Temel izleme
Temel izleme, Azure kaynakları arasında temel ve gerekli izleme deneyimini sağlar. Bu hizmetler minimum yapılandırma gerektirir ve premium izleme hizmetlerinin kullandığı temel telemetri verilerini toplar.    

### <a name="azure-monitor"></a>Azure İzleyici
[Azure İzleyici](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md), [ölçümlerin](../monitoring-and-diagnostics/monitoring-overview-metrics.md), [etkinlik günlüklerinin](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve [tanılama günlüklerinin](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) toplanmasına olanak sağlayarak Azure hizmetleri için temel izleme deneyimi sağlar. Örneğin, etkinlik günlüğü size yeni kaynakların ne zaman oluşturulduğunu veya değiştirildiğini bildirir.

Farklı kaynaklar için ve hatta bir sanal makinenin içindeki işletim sistemi için bile performans istatistikleri sağlayan ölçümler kullanılabilir. Azure portalında gezginlerden biriyle bu verileri görüntüleyebilir ve bu ölçümlere dayalı uyarılar oluşturabilirsiniz. Azure İzleyici, en hızlı ölçüm işlem hattını (5 dakika ila 1 dakika) sağlar, bu nedenle zaman açısından kritik uyarılar ve bildirimler için bunu kullanmalısınız.

Ayrıca eğilim analizi ve ayrıntılı analiz için bu ölçümleri ve günlükleri Azure Log Analytics’e gönderebilir veya bu analizin sonucunda kritik sorunları proaktif olarak size bildirecek ek uyarı kuralları oluşturabilirsiniz.  

> [!NOTE]
> Çok boyutlu ölçümlerin tanılama ayarları aracılığıyla Log Analytics’e gönderilmesi şu anda desteklenmemektedir. Boyutlu ölçümler, boyut değerlerinin toplamı alınarak düzleştirilmiş tek yönlü ölçümler olarak dışarı aktarılır.
>
> *Örneğin*: Bir Olay Hub'ındaki 'Gelen İletiler' ölçümü, kuyruk düzeyi temelinde araştırılıp grafiği oluşturulabilir. Ancak, Log Analytics’e dışarı aktarılan ölçüm, Event Hub’daki tüm kuyruklarda tüm gelen iletiler olarak ifade edilir.
>
>

### <a name="azure-advisor"></a>Azure Advisor
[Azure Danışmanı](../advisor/advisor-overview.md) sürekli olarak kaynak yapılandırma ve kullanımına ilişkin telemetri verilerinizi izler. Daha sonra size en iyi uygulamalara dayalı kişiselleştirilmiş öneriler sunar. Bu önerileri izleyerek, uygulamalarınızı destekleyen kaynakların performansının, güvenliğinin ve kullanılabilirliğinin artırılmasına yardımcı olabilirsiniz.

### <a name="service-health"></a>Hizmet Durumu
Uygulamanızın durumu, bağımlı olduğu Azure hizmetlerine bağlıdır. [Azure Hizmet Durumu](../service-health/service-health-overview.md), Azure hizmetleriyle ilgili uygulamanızı etkileyebilecek tüm sorunları tanımlar. Hizmet Durumu, zamanlanan bakım için plan yapmanıza da yardımcı olur.

### <a name="activity-log"></a>Etkinlik Günlüğü
[Etkinlik Günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), bir Azure kaynağının işlemi hakkında veriler sağlar. Bu bilgiler arasında şunlar yer alır:
- Kaynak üzerindeki yapılandırma değişiklikleri.
- Hizmet durumu olayları.
- Kaynağın daha iyi kullanımına ilişkin öneriler.
- Otomatik ölçeklendirme işlemleriyle ilgili bilgiler.

Belirli bir kaynağın günlüklerini Azure portalında kendi sayfasında görüntüleyebilirsiniz. Veya Etkinlik Günlüğü Gezgini’nde birden fazla kaynaktan günlükleri görüntüleyebilirsiniz.

Etkinlik günlüğü girişlerini Log Analytics’e de gönderebilirsiniz. Burada, yönetim çözümleri, sanal makinelerdeki aracılar ve diğer kaynaklardan toplanan verileri kullanarak günlükleri analiz edebilirsiniz.

## <a name="deep-monitoring-services"></a>Ayrıntılı izleme hizmetleri
Aşağıdaki Azure hizmetleri, izleme verilerini daha ayrıntılı düzeyde toplama ve analiz etmeye ilişkin zengin özellikler sağlar. Bu hizmetler, temel izleme temelinde oluşturulmuş olup Azure’daki genel işlevlerden yararlanır. Size, uygulamalarınıza ve altyapınıza yönelik benzersiz içgörü sunmak için toplanan verilerle güçlü analiz sağlar. Farklı kitleleri hedefleyen senaryolar bağlamında veri sunar.

## <a name="deep-application-monitoring"></a>Ayrıntılı uygulama izleme
### <a name="application-insights"></a>Application Insights
İster bulutta isterse şirket içinde barındırılıyor olsun, uygulamanızın kullanılabilirliğini, performansını ve kullanımını izlemek için [Azure Application Insights](http://azure.microsoft.com/documentation/services/application-insights) kullanabilirsiniz.

Application Insights ile çalışmak için uygulamanızı düzenleyerek ayrıntılı içgörüler elde edebilir ve DevOps senaryoları uygulayabilirsiniz. Bir kullanıcının bildirmesini beklemeden hataları hızlıca tanımlayıp tespit edebilirsiniz. Topladığınız bilgilerle, uygulamanızın bakımı ve iyileştirmeleri ile ilgili bilinçli kararlar alabilirsiniz.

Application Insights, topladığı verilerle etkileşim kurmak için kapsamlı araçlara sahiptir. Application Insights, verilerini genel bir depoda saklar. Log Analytics sorgu diliyle ayrıntılı analiz, uyarılar ve panolar gibi paylaşılan işlevlerden yararlanabilir.

## <a name="deep-infrastructure-monitoring"></a>Ayrıntılı altyapı izleme
### <a name="log-analytics"></a>Log Analytics
[Log Analytics](http://azure.microsoft.com/documentation/services/log-analytics), çeşitli kaynaklardan (Microsoft olmayan araçlar dahil olmak üzere) verileri tek bir depoda toplayarak Azure izlemede merkezi bir rol oynar. Burada, güçlü bir sorgu dili kullanarak verileri analiz edebilirsiniz.

Application Insights ve Azure Güvenlik Merkezi, verilerini Log Analytics veri deposunda saklar ve analiz altyapısını kullanır. Azure İzleyici, yönetim çözümleri ve buluttaki veya şirket içi sanal makinelerde yüklü olan aracılardan da veri toplanır. Bu paylaşılan işlevsellik, ortamınızın eksiksiz bir resmini oluşturmanıza yardımcı olur.

### <a name="management-solutions"></a>Yönetim çözümleri
[Yönetim çözümleri](../log-analytics/log-analytics-add-solutions.md), belirli bir uygulama veya hizmet için içgörüler sağlayan paketlenmiş mantık kümeleridir. Topladıkları izleme verilerini depolamak ve analiz etmek için Log Analytics’i kullanır.

Çeşitli Azure hizmetlerine ve üçüncü taraf hizmetlere yönelik izleme sağlamak için Microsoft ve iş ortaklarının yönetim çözümleri mevcuttur. İzleme çözümü örnekleri arasında şunlar yer alır:
* Kapsayıcı ana bilgisayarlarınızı görüntülemenize ve yönetmenize yardımcı olan [Kapsayıcı İzleme](../log-analytics/log-analytics-containers.md).
* Azure SQL veritabanları için performans ölçümlerini toplayan ve görselleştiren [Azure SQL Analytics](../log-analytics/log-analytics-azure-sql.md).

Azure Portal’da *İzleyici* ekranında tüm kullanılabilir yönetim çözümlerini görüntüleyebilirsiniz.

### <a name="network-monitoring"></a>Ağ İzleme
Azure veya şirket içi ağınızda, ağınızın çeşitli yönlerini izlemek için birlikte çalışan birçok araç vardır.  

[Ağ İzleyicisi](../network-watcher/network-watcher-monitoring-overview.md), Azure’da farklı ağ senaryoları için senaryoya dayalı izleme ve tanılama sağlar. Daha fazla analiz için Azure ölçümleri ve tanılamalarında verileri depolar. Ağınızın çeşitli yönlerini izlemek için aşağıdaki çözümlerle birlikte çalışır.

[Ağ Performans İzleyicisi (NPM)](../log-analytics/log-analytics-network-performance-monitor.md), genel bulutlar, veri merkezleri ve şirket içi ortamlar arasında bağlantıyı izleyen, bulut tabanlı bir ağ izleme çözümüdür.

[ExpressRoute İzleyicisi](../expressroute/how-to-npm.md), Azure ExpressRoute devrelerindeki uçtan uca bağlantı ve performansı izleyen bir NPM özelliğidir.

[DNS Analizi](../log-analytics/log-analytics-dns.md), DNS sunucularınıza dayalı olarak güvenlik, performans ve işlemlerle ilgili içgörüler sağlayan bir çözümdür.

[Hizmet Uç Noktası İzleyicisi](../networking/network-monitoring-overview.md), şirket içinde, taşıyıcı ağlarında ve bulut/özel veri merkezlerinde uygulamaların ulaşılabilirliğini test eder ve performans sorunlarını algılar.


### <a name="service-map"></a>Hizmet Eşlemesi
[Hizmet Eşlemesi](../operations-management-suite/operations-management-suite-service-map.md), diğer bilgisayarlardaki ve dış süreçlerdeki farklı süreçleri ve bağımlılıkları ile sanal makineleri analiz ederek IaaS ortamınıza yönelik içgörü sağlar. Log Analytics’te olayları, performans verilerini ve yönetim çözümlerini tümleştirir. Daha sonra her bir bilgisayar bağlamında bu verileri ve ortamınızın geri kalanıyla ilişkisini görüntüleyebilirsiniz.

Hizmet Eşlemesi, [Application Insights’taki Uygulama Eşlemesine](../application-insights/app-insights-app-map.md) benzer. Uygulamalarınızı destekleyen altyapı bileşenlerine odaklanır.


## <a name="example-scenarios"></a>Örnek senaryolar
Aşağıda, farklı senaryolar için Azure’da farklı izleme araçlarını nasıl kullanabileceğinizi gösteren yüksek düzey örnekler verilmiştir.

### <a name="monitoring-a-web-application"></a>Web uygulamasını izleme
Azure App Service, Azure Depolama ve SQL veritabanı aracılığıyla Azure’da dağıtılan bir web uygulamasını düşünün. Bu kaynakların Azure portalındaki sayfalarından bu kaynaklar için [ölçümlere](../monitoring-and-diagnostics/monitoring-overview-metrics.md) ve [etkinlik günlüklerine](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) erişerek başlarsınız. Uygulamaya yönelik istek sayısı ve ortalama yanıt süresi gibi kritik bilgileri ararsınız. Ayrıca varsa, yapılandırma değişikliklerini de belirlersiniz.

Daha sonra farklı kaynaklar için ölçümleri ve günlükleri birlikte görüntülemek için portalda İzleyici’ye gidersiniz. Ölçümler için standart parametreleri belirlerken [uyarı kuralları oluşturursunuz](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md). Bu kurallar, örneğin, ortalama yanıt süresi bir eşiği aştığında proaktif olarak size bildirim gönderir. Uygulamanızın günlük performansının hızlı bir görünümünü almak için bir Azure panosu oluşturarak kritik KPI’leri temsil eden metriklerin grafiklerini gösterirsiniz.

Uygulamanızı daha ayrıntılı izlemek için [Application Insights için uygulamanızı yapılandırırsınız](../application-insights/quick-monitor-portal.md). Artık, uygulamanızın işlem ve performansına yönelik daha fazla içgörü sağlayan ek veriler toplayabilirsiniz. Application Insights, uygulamanızın bileşenleri arasındaki temel ilişkileri algılar. Bir sorunun oluştuğu tam bileşeni, bağımlılığı veya istisnayı tespit etmek için [uçtan uca izleme](../application-insights/app-insights-transaction-diagnostics.md) ile birlikte [Uygulama Eşlemesi](../application-insights/app-insights-app-map.md) aracılığıyla görsel temsile olanak sağlar.

Farklı bölgelerden uygulamanızı proaktif şekilde test etmek için [Uygulanabilirlik testleri](../application-insights/app-insights-monitor-web-app-availability.md) oluşturursunuz. Geliştiricilerinize yardımcı olmak için, [Profil Oluşturucuyu etkinleştirerek](../application-insights/enable-profiler-compute.md) istekleri ve istisnaları belirli bir kod satırına kadar izleyebilirsiniz. Uygulamanızda kullanılan hizmetlere yönelik daha fazla görünürlük elde etmek için, [SQL Analytics çözümünü](../log-analytics/log-analytics-azure-sql.md) ekleyerek Log Analytics’te ek veriler toplarsınız.

Bir süre sonra, sitedeki performansın bir eşiğin altına düştüğü zamanların kök nedenini araştırmaya karar verirsiniz. Log Analytics kullanarak bir sorgu yazarsınız. Application Insights tarafından toplanan kullanım ve performans verilerini, uygulamanızı destekleyen Azure kaynaklarındaki yapılandırma ve performans verileriyle ilişkilendirmenize yardımcı olur.


### <a name="monitoring-virtual-machines"></a>Sanal makineleri izleme
Azure’da çalışan hem Windows hem de Linux sanal makineleriniz vardır. [Etkinlik günlüklerini](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) ve [ana bilgisayar düzeyinde ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) görüntülemek için Azure İzleyici’yi kullanırsınız. Konuk işletim sisteminden ölçümleri toplamak için sanal makinelere [Azure Tanılama uzantısını](../virtual-machines/linux/tutorial-monitoring.md#install-diagnostics-extension) eklersiniz. Daha sonra, işlemci kullanımı ve belleği gibi temel ölçümler eşikleri geçtiğinde proaktif olarak size bildirim göndermesi için [uyarı kuralları](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) oluşturursunuz.

Bir iş uygulaması çalıştıran sanal makineler hakkında daha fazla ayrıntı toplamak için, her makinede [bir Log Analytics çalışma alanı oluşturur ve sanal makine uzantısını etkinleştirirsiniz](../log-analytics/log-analytics-quick-collect-azurevm.md). Uygulamanız için [farklı veri kaynakları koleksiyonunu](../log-analytics/log-analytics-data-sources.md) yapılandırır ve bunun günlük işlem ve performansını bildirmek için [görünümler oluşturursunuz](../log-analytics/log-analytics-view-designer.md). Daha sonra belirli hata olayları alındığında size bildirim göndermesi için [uyarı kuralları oluşturursunuz](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md).

Yüklü aracının durumunu sürekli olarak izlemek için [Aracı Durumu yönetim çözümünü](../operations-management-suite/oms-solution-agenthealth.md) eklersiniz. Uygulamaya ilişkin daha fazla içgörü elde etmek için, sanal makinelere [bağımlılık aracısını ekleyerek](../operations-management-suite/operations-management-suite-service-map-configure.md) bu makineleri [Hizmet Eşlemesi](../operations-management-suite/operations-management-suite-service-map.md)’ne eklersiniz. Hizmet Eşlemesi, kritik işlemleri keşfeder ve makineler ile diğer hizmetler arasındaki bağlantıları tanımlar.

Bildirilen bir kesintiden sonra Hizmet Eşlemesini kullanarak, sorunu yaşayan makineyi belirlemek amacıyla adli araştırma gerçekleştirirsiniz. Ardından, gelecekte sorunu tanımlamak için [Log Analytics verilerinde bir sorgu](../log-analytics/log-analytics-log-search-new.md) oluşturursunuz. Ayrıca koşul algılandığında proaktif olarak size bildirim göndermesi için bir uyarı kuralı oluşturursunuz.



## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakiler hakkında daha fazla bilgi edinin:

* Temel izleme ölçümlerini ve uyarılarını kullanmaya başlamak için [Azure İzleyici](https://azure.microsoft.com/services/monitor/).
* App Service web uygulamanızda sorunları tanılamaya çalışıyorsanız [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/).
* Toplanan izleme verilerini ve günlüklerini analiz etmek için [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/).
