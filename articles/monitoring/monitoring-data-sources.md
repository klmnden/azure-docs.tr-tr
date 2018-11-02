---
title: Azure İzleyici'de veri kaynaklarını | Microsoft Docs
description: Sistem durumu ve Azure kaynaklarınızı ve bunlar üzerinde çalışan uygulamaların performansını izlemek için kullanılabilir veri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2018
ms.author: bwren
ms.openlocfilehash: bddef08c6a3de15c1c156b4dc020d758b7cd0fc1
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912943"
---
# <a name="sources-of-data-in-azure-monitor"></a>Azure İzleyici'de veri kaynakları
Bu makalede, durumunu ve performansını, kaynakları ve bunlar üzerinde çalışan uygulamaları izlemek için Azure İzleyici tarafından toplanan veri kaynaklarını açıklar. Bu kaynaklara başka bir bulut ya da şirket içi azure'da olabilir.  Bkz: [Azure İzleyici tarafından toplanan veriler](monitoring-data-collection.md) nasıl görüntüleyebilirsiniz ve bu verilerin nasıl depolandığı ile ilgili ayrıntılar için.

İzleme verileri azure'da bir çeşitli düzenlenebilir kaynaklardan katmanları, uygulamanızın ve tüm işletim sistemleri olan en yüksek katmanları ve Azure platformu bileşenleri olan alt katmanları sunulur. Aşağıdaki bölümlerde ayrıntılı olarak açıklanan her bir katman ile bu Aşağıdaki diyagramda gösterilmiştir.

![İzleme verilerinin katmanları](media/monitoring-data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure Kiracısı
Azure Active Directory gibi Kiracı genelinde hizmetlerden olmak üzere Azure kiracısı için ilgili telemetri toplanır.

![Azure Kiracı koleksiyonu](media/monitoring-data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory denetim günlüklerini
[Azure Active Directory raporlama](../active-directory/reports-monitoring/overview-reports.md) oturum açma etkinlik ve denetim izi belirli bir kiracıda yapılan değişikliklerin geçmişini içerir. Bu denetim günlükleri ile diğer günlük verileri çözümlemek için Log analytics'e yazılabilir.


## <a name="azure-platform"></a>Azure platformu
Telemetri, durumunu ve işlem için ilgili Azure kendi Azure aboneliğinizin yönetim ve işlem ile ilgili veriler içerir. Bu, Azure etkinlik günlüğü ve Azure Active Directory'deki denetim günlükleri depolanan hizmeti sistem durumu verilerini içerir.

![Azure aboneliği koleksiyon](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Hizmet Durumu
[Azure hizmet durumu](../monitoring-and-diagnostics/monitoring-service-notifications.md) uygulama ve kaynakları kullanır, aboneliğinizdeki Azure hizmetlerinin durumu hakkında bilgi sağlar. Uygulamanızı etkileyebilecek geçerli ve beklenen kritik sorunları size bildirilmesini sağlamak üzere uyarılar oluşturabilirsiniz. Hizmet durumu kayıtları depolanır [Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), bunları etkinlik günlüğü Gezgini'nde görüntüleyebilir ve bunları Log Analytics'e kopyalayın.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü
[Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) yapılandırma değişiklikleri Azure kaynaklarınıza kayıtları ile birlikte hizmet sistem durumu kayıtları içerir. Etkinlik günlüğü, tüm Azure kaynaklarını ve temsil kullanılabilir kendi _dış_ görünümü. Belirli tür etkinlik günlüğünde kayıt açıklanan [Azure etkinlik günlüğü olay şeması](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

Belirli bir kaynak için etkinlik günlüğü kendi içinde birden çok kaynaktan Azure portalı veya Görünüm günlükleri sayfasında görüntüleyebilirsiniz [etkinlik günlüğü Gezgini](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). İle diğer izleme verilerini birleştirmek için Log Analytics günlük girişlerini kopyalamak özellikle yararlıdır. Ayrıca bunları kullanarak diğer konumlara gönderebilirsiniz [Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).



## <a name="azure-services"></a>Azure Hizmetleri
Ölçümler ve kaynak düzeyi tanılama günlükleri hakkında bilgi sağlar _iç_ Azure kaynaklarınızın işlemi. Bunlar çoğu Azure hizmeti için kullanılabilir ve yönetim çözümleri belirli Hizmetler ek Öngörüler sağlar.

![Azure kaynak koleksiyonunu](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Ölçümler
Çoğu Azure hizmeti oluşturacağını [platform ölçümleri](monitoring-data-collection.md#metrics) performans ve işlem yansıtır. Belirli [ölçümleri, her kaynak türü için değişir](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Bunlar ölçümleri Explorer'dan erişilebilir ve Log Analytics'e eğilim ve analiz için kopyalanabilir.


### <a name="resource-diagnostic-logs"></a>Kaynak tanılama günlükleri
Etkinlik günlüğü kaynak düzeyinde bir Azure kaynak gerçekleştirilen işlemler hakkında bilgi sağlarken [tanılama günlükleri](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) kaynağının işlemi hakkında ayrıntılı bilgiler sağlar.   Bu günlükler içeriği ve yapılandırma gereksinimleri [kaynak türüne göre değişir](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

Tanılama günlüklerinin Azure portalında doğrudan görüntüleyemiyorum, ancak yapabilecekleriniz [Azure depolama, arşivleme şirketlerde](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) ve bunları dışarı aktarma [olay hub'ı](../event-hubs/event-hubs-what-is-event-hubs.md) diğer hizmetlere yönelik yeniden yönlendirme veya [günlüğüne Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) analiz. Başkalarının edilmeden önce bir depolama hesabına yazma sırasında bazı kaynaklar doğrudan Log Analytics'e yazabilirsiniz [Log Analytics'e içeri](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>İzleme Çözümleri
 [İzleme çözümleri](monitoring-solutions.md) toplama işleminin belirli bir hizmet veya uygulamanın ek Öngörüler sağlar. Bunlar burada olabilir Log Analytics'e veri toplama kullanılarak analiz [sorgu dilini](../log-analytics/log-analytics-log-search.md) veya [görünümleri](../log-analytics/log-analytics-view-designer.md) genellikle çözümüne eklenmiş.

## <a name="guest-operating-system"></a>Konuk İşletim Sistemi
Azure, diğer bulutlarda ve şirket içi bilgi işlem kaynaklarının izlemek için bir konuk işletim sistemi vardır. Bir veya daha fazla aracı yüklemesi ile aynı izleme araçları Azure Hizmetleri ile Konuk telemetri toplayabilir.

![Azure işlem kaynak koleksiyonu](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="diagnostic-extension"></a>Tanılama uzantısı
İle [Azure tanılama uzantısı](../monitoring-and-diagnostics/azure-diagnostics.md), günlükleri toplayabilir ve performans verilerini istemci işletim sisteminde Azure bilgi işlem kaynakları. Ölçümleri hem istemcilerden toplanan günlükleri yapabileceğiniz bir Azure depolama hesabında depolanır [almak için Log Analytics'i yapılandırma](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).  Ölçüm Gezgini, depolama hesabından okuma anlamakta ve diğer toplanan ölçümler ile istemci ölçümleri içerir.


### <a name="log-analytics-agent"></a>Log Analytics Aracısı
Herhangi bir Log Analytics aracısını yükleyebilirsiniz [Windows](../log-analytics/log-analytics-agent-windows.md) veya [Linux]() sanal makine veya fiziksel bilgisayar. Azure, başka bir bulut veya şirket içi sanal makinenin çalışıyor olabilir.  Aracının Log Analytics'e ya da doğrudan bağlanan aracılığıyla veya bir [bağlı System Center Operations Manager yönetim grubu](../log-analytics/log-analytics-om-agents.md) ve verileri toplamanızı sağlar [veri kaynakları](../log-analytics/log-analytics-data-sources.md) yapılandırdığınız veya [yönetim çözümleri](monitoring-solutions.md) sanal makinede çalışan uygulamaların ek Öngörüler sağlar.

### <a name="service-map"></a>Hizmet Eşlemesi
[Hizmet eşlemesi](../operations-management-suite/operations-management-suite-service-map.md) bir bağımlılık aracısını Windows ve Linux sanal makinelerinde gerektirir. Bu aracıya işlemleri dış bağımlılıkları ve sanal makine üzerinde çalışan işlemler hakkında veri toplar Log Analytics ile çalışır. Bu, bu verileri Log Analytics'te depolar ve görsel olarak Log Analytics içinde depolanan diğer veri yanı sıra topladığı verileri görüntüleyen bir konsol içerir.

## <a name="applications"></a>Uygulamalar
Uygulamanız konuk işletim sistemine yazabilirsiniz telemetri yanı sıra ayrıntılı uygulama izleme ile yapılır [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights, çok çeşitli platformlarda çalışan uygulamalardan veri toplayabilir. Uygulama, Azure, başka bir bulutta veya şirket içinde çalışıyor olabilir.

![Uygulama veri toplama](media/monitoring-data-sources/application-collection.png)


### <a name="application-data"></a>Uygulama verileri
Bir izleme paketi yükleyerek bir uygulama için Application ınsights'ı etkinleştirdiğinizde, ölçüm ve günlükleri performans ve uygulama işlemi ilgili toplar. Bu sayfa görüntülemeleri, uygulama isteklerini ve özel durumları hakkında ayrıntılı bilgi içerir. Application Insights, Azure ölçümleri ve Log Analytics topladığı verileri depolar. Bu verileri analiz etmek için kapsamlı araçlar içerir, ancak, aynı zamanda, ölçüm Gezgini ve günlük aramaları gibi araçları kullanarak diğer kaynaklardan verileri analiz edebilirsiniz.

Application ınsights'ı da kullanabilirsiniz [özel bir ölçü oluşturma](../application-insights/app-insights-api-custom-events-metrics.md).  Bu sayede bir sayısal değer hesaplamak ve ardından bu değeri Ölçüm Gezgini'nde erişilebilen ve kullanılan diğer ölçümlerle saklamak için kendi mantığını tanımlamak üzere [otomatik ölçeklendirme](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) ve ölçüm uyarıları.

### <a name="dependencies"></a>Bağımlılıklar
Bir uygulamanın farklı mantıksal işlemleri izlemeniz için yapmanız gerekenler [birden çok bileşenlerinde telemetri toplamak](../application-insights/app-insights-transaction-diagnostics.md). Application Insights'ı destekleyen [dağıtılmış telemetri bağıntısı](../application-insights/application-insights-correlation.md) birlikte analiz etmenize imkan sağlar bileşenleri arasındaki bağımlılıkları tanımlar.

### <a name="availability-tests"></a>Kullanılabilirlik testleri
[Kullanılabilirlik testleri](../application-insights/app-insights-monitor-web-app-availability.md) Application Insights'da, kullanılabilirliğini ve yanıt hızı, uygulamanızın genel Internet üzerindeki farklı konumlardan test olanak tanır. Uygulama etkin olduğunu doğrulamak için bir basit ping testi yapın veya bir kullanıcı senaryosu taklit eden bir web testi oluşturmak için Visual Studio'yu kullanın.  Kullanılabilirlik testleri, uygulamadaki tüm Araçları'nı gerektirmez.

## <a name="custom-sources"></a>Özel kaynaklar
Standart bir uygulama katmanlarına ek olarak, diğer veri kaynaklarıyla toplanan telemetri olması diğer kaynakları izlemek gerekebilir. Bu kaynaklar için bir Azure İzleyici API'sini kullanarak bu verileri yazmanız gerekir.

![Özel veri toplama](media/monitoring-data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Veri Toplayıcı API’si
Azure İzleyicisi'ni kullanarak herhangi bir REST istemcisinden günlük verilerini toplayabilir [veri toplayıcı API'sini](../log-analytics/log-analytics-data-collector-api.md). Bu, özel izleme senaryoları oluşturmanıza ve diğer kaynakları aracılığıyla telemetri sunmayın kaynaklara izleme genişletmek sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [türü izleme verilerinin Azure İzleyici tarafından toplanan](monitoring-data-collection.md) ve görüntüleme ve bu verileri analiz etme.
