---
title: Azure İzleyici'de veri kaynaklarını | Microsoft Docs
description: Sistem durumu ve Azure kaynaklarınızı ve bunlar üzerinde çalışan uygulamaların performansını izlemek için kullanılabilir veri açıklar.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: bwren
ms.openlocfilehash: 6d03c219025c8cd39214bd8ab6807125709f9742
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59790885"
---
# <a name="sources-of-data-in-azure-monitor"></a>Azure İzleyici'de veri kaynakları
Bu makalede, durumunu ve performansını, kaynakları ve bunlar üzerinde çalışan uygulamaları izlemek için Azure İzleyici tarafından toplanan veri kaynaklarını açıklar. Bu kaynaklara başka bir bulut ya da şirket içi azure'da olabilir.  Bkz: [Azure İzleyici tarafından toplanan veriler](data-platform.md) nasıl görüntüleyebilirsiniz ve bu verilerin nasıl depolandığı ile ilgili ayrıntılar için.

İzleme verileri azure'da bir çeşitli düzenlenebilir kaynaklardan katmanları, uygulamanızın ve tüm işletim sistemleri olan en yüksek katmanları ve Azure platformu bileşenleri olan alt katmanları sunulur. Aşağıdaki bölümlerde ayrıntılı olarak açıklanan her bir katman ile bu Aşağıdaki diyagramda gösterilmiştir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

![İzleme verilerinin katmanları](media/data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure Kiracısı
Azure Active Directory gibi Kiracı genelinde hizmetlerden olmak üzere Azure kiracısı için ilgili telemetri toplanır.

![Azure Kiracı koleksiyonu](media/data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Azure Active Directory denetim günlüklerini
[Azure Active Directory raporlama](../../active-directory/reports-monitoring/overview-reports.md) oturum açma etkinlik ve denetim izi belirli bir kiracıda yapılan değişikliklerin geçmişini içerir. Bu denetim günlükleri ile diğer günlük verileri çözümlemek için Azure İzleyici günlüklerine yazılır.


## <a name="azure-platform"></a>Azure platformu
Telemetri, durumunu ve işlem için ilgili Azure kendi Azure aboneliğinizin yönetim ve işlem ile ilgili veriler içerir. Bu, Azure etkinlik günlüğü ve Azure Active Directory'deki denetim günlükleri depolanan hizmeti sistem durumu verilerini içerir.

![Azure aboneliği koleksiyon](media/data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Hizmet Durumu
[Azure hizmet durumu](service-notifications.md) uygulama ve kaynakları kullanır, aboneliğinizdeki Azure hizmetlerinin durumu hakkında bilgi sağlar. Uygulamanızı etkileyebilecek geçerli ve beklenen kritik sorunları size bildirilmesini sağlamak üzere uyarılar oluşturabilirsiniz. Hizmet durumu kayıtları depolanır [Azure etkinlik günlüğü](activity-logs-overview.md), bunları etkinlik günlüğü Gezgini'nde görüntüleyebilir ve bunları Azure İzleyici günlüklerine kopyalayın.

### <a name="azure-activity-log"></a>Azure etkinlik günlüğü
[Azure etkinlik günlüğü](activity-logs-overview.md) yapılandırma değişiklikleri Azure kaynaklarınıza kayıtları ile birlikte hizmet sistem durumu kayıtları içerir. Etkinlik günlüğü, tüm Azure kaynaklarını ve temsil kullanılabilir kendi _dış_ görünümü. Belirli tür etkinlik günlüğünde kayıt açıklanan [Azure etkinlik günlüğü olay şeması](activity-log-schema.md).

Belirli bir kaynak için etkinlik günlüğü kendi içinde birden çok kaynaktan Azure portalı veya Görünüm günlükleri sayfasında görüntüleyebilirsiniz [etkinlik günlüğü Gezgini](activity-logs-overview.md). Azure İzleyici ile diğer izleme verilerini birleştirmek için günlük girişlerini kopyalamak özellikle yararlıdır. Ayrıca bunları kullanarak diğer konumlara gönderebilirsiniz [Event Hubs](activity-logs-stream-event-hubs.md).



## <a name="azure-services"></a>Azure Hizmetleri
Ölçümler ve kaynak düzeyi tanılama günlükleri hakkında bilgi sağlar _iç_ Azure kaynaklarınızın işlemi. Bunlar çoğu Azure hizmeti için kullanılabilir ve yönetim çözümleri belirli Hizmetler ek Öngörüler sağlar.

![Azure kaynak koleksiyonunu](media/data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Ölçümler
Çoğu Azure hizmeti oluşturacağını [platform ölçümleri](data-platform-metrics.md) performans ve işlem yansıtır. Belirli [ölçümleri, her kaynak türü için değişir](metrics-supported.md).  Bunlar, ölçümleri analiz erişilebilir ve Log Analytics kullanarak popüler ve çözümleme günlüklerini kopyalanabilir.


### <a name="resource-diagnostic-logs"></a>Kaynak tanılama günlükleri
Etkinlik günlüğü kaynak düzeyinde bir Azure kaynak gerçekleştirilen işlemler hakkında bilgi sağlarken [tanılama günlükleri](diagnostic-logs-overview.md) kaynağının işlemi hakkında ayrıntılı bilgiler sağlar.   Bu günlükler içeriği ve yapılandırma gereksinimleri [kaynak türüne göre değişir](diagnostic-logs-schema.md).

Tanılama günlüklerinin Azure portalında doğrudan görüntüleyemiyorum, ancak yapabilecekleriniz [Azure depolama, arşivleme şirketlerde](archive-diagnostic-logs.md) ve bunları dışarı aktarma [olay hub'ı](../../event-hubs/event-hubs-about.md) diğer hizmetlere yönelik yeniden yönlendirme veya [için Azure İzleyici](diagnostic-logs-stream-log-store.md) analiz. Başkalarının edilmeden önce bir depolama hesabına yazma sırasında bazı kaynaklar doğrudan Azure İzleyici yazabilirsiniz [Log Analytics'e içeri](azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>İzleme Çözümleri
 [İzleme çözümleri](../../azure-monitor/insights/solutions.md) toplama işleminin belirli bir hizmet veya uygulamanın ek Öngörüler sağlar. Bunlar Azure İzleyici günlüklerine burada olabilir toplamak kullanılarak analiz [sorgu dilini](../../azure-monitor/log-query/log-query-overview.md) veya [görünümleri](view-designer.md) genellikle çözümüne eklenmiş.


## <a name="guest-operating-system"></a>Konuk işletim sistemi
Azure, diğer bulutlarda ve şirket içi bilgi işlem kaynaklarının izlemek için bir konuk işletim sistemi vardır. Bir veya daha fazla aracı yüklemesi ile aynı izleme araçları Azure Hizmetleri ile Konuk telemetri toplayabilir.

![Azure işlem kaynak koleksiyonu](media/data-sources/compute-resource-collection.png)

### <a name="azure-diagnostic-extension"></a>Azure tanılama uzantısı
Azure tanılama uzantısı ile günlükleri toplayarak izleme temel bir düzeyde sağlar ve performans verilerini istemci işletim sisteminde Azure işlem kaynakları.   

### <a name="log-analytics-agent"></a>Log Analytics Aracısı
Kapsamlı izleme ve yönetim, Windows veya Linux sanal makineleri veya fiziksel bilgisayar ile Log Analytics aracısını teslim. Sanal makineyi Azure, başka bir bulutta veya şirket içinde çalışan ve aracıyı Azure'a bağlar, verileri toplamanıza olanak tanır ve doğrudan ya da System Center Operations Manager ile izleme [veri kaynakları](agent-data-sources.md) , yapılandırma veya [izleme çözümleri](../../azure-monitor/insights/solutions.md) sanal makinede çalışan uygulamaların ek Öngörüler sağlar.


### <a name="dependency-agent"></a>Bağımlılık aracısı
[Hizmet eşlemesi](../insights/service-map.md) ve [VM'ler için Azure İzleyici](../../azure-monitor/insights/vminsights-overview.md) bir bağımlılık aracısını Windows ve Linux sanal makinelerinde gerektirir. Bu topladığı için Log Analytics aracısını dış işlem bağımlılıklarını ve sanal makine üzerinde çalışan işlemler hakkında bulunan verileri tümleşir. Azure İzleyici'de bu verileri depolar ve bulunan birbirine bağlı bileşenleri görselleştirir.  

Ek aracılar ve izleme gereksinimlerinize bağlı olarak kullanmak istediğiniz arasındaki farkları anlamak için bkz [izleme aracıları görünümü](agents-overview.md).

## <a name="applications"></a>Uygulamalar
Uygulamanız konuk işletim sistemine yazabilirsiniz telemetri yanı sıra ayrıntılı uygulama izleme ile yapılır [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights, çok çeşitli platformlarda çalışan uygulamalardan veri toplayabilir. Uygulama, Azure, başka bir bulutta veya şirket içinde çalışıyor olabilir.

![Uygulama veri toplama](media/data-sources/application-collection.png)


### <a name="application-data"></a>Uygulama verileri
Bir izleme paketi yükleyerek bir uygulama için Application ınsights'ı etkinleştirdiğinizde, ölçüm ve günlükleri performans ve uygulama işlemi ilgili toplar. Bu sayfa görüntülemeleri, uygulama isteklerini ve özel durumları hakkında ayrıntılı bilgi içerir. Application Insights, Azure İzleyici'de topladığı verileri depolar. Bu verileri analiz etmek için kapsamlı araçlar içerir, ancak, de, ölçüm analytics ve log analytics gibi araçları kullanarak diğer kaynaklardan verileri analiz edebilirsiniz.

Application ınsights'ı da kullanabilirsiniz [özel bir ölçü oluşturma](../../application-insights/app-insights-api-custom-events-metrics.md).  Bu sayede bir sayısal değer hesaplamak ve ardından bu değeri Ölçüm analytics'ten erişilebilen ve kullanılan diğer ölçümlerle saklamak için kendi mantığını tanımlamak üzere [otomatik ölçeklendirme](autoscale-custom-metric.md) ve ölçüm uyarıları.


### <a name="dependencies"></a>Bağımlılıklar
Bir uygulamanın farklı mantıksal işlemleri izlemeniz için yapmanız gerekenler [birden çok bileşenlerinde telemetri toplamak](../../application-insights/app-insights-transaction-diagnostics.md). Application Insights'ı destekleyen [dağıtılmış telemetri bağıntısı](../../application-insights/application-insights-correlation.md) birlikte analiz etmenize imkan sağlar bileşenleri arasındaki bağımlılıkları tanımlar.

### <a name="availability-tests"></a>Kullanılabilirlik testleri
[Kullanılabilirlik testleri](../../application-insights/app-insights-monitor-web-app-availability.md) Application Insights'da, kullanılabilirliğini ve yanıt hızı, uygulamanızın genel Internet üzerindeki farklı konumlardan test olanak tanır. Uygulama etkin olduğunu doğrulamak için bir basit ping testi yapın veya bir kullanıcı senaryosu taklit eden bir web testi oluşturmak için Visual Studio'yu kullanın.  Kullanılabilirlik testleri, uygulamadaki tüm Araçları'nı gerektirmez.

## <a name="custom-sources"></a>Özel kaynaklar
Standart bir uygulama katmanlarına ek olarak, diğer veri kaynaklarıyla toplanan telemetri olması diğer kaynakları izlemek gerekebilir. Bu kaynaklar için bir Azure İzleyici API'sini kullanarak bu verileri yazmanız gerekir.

![Özel veri toplama](media/data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Veri Toplayıcı API’si
Azure İzleyicisi'ni kullanarak herhangi bir REST istemcisinden günlük verilerini toplayabilir [veri toplayıcı API'sini](../../azure-monitor/platform/data-collector-api.md). Bu, özel izleme senaryoları oluşturmanıza ve diğer kaynakları aracılığıyla telemetri sunmayın kaynaklara izleme genişletmek sağlar.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [türü izleme verilerinin Azure İzleyici tarafından toplanan](data-platform.md) ve görüntüleme ve bu verileri analiz etme.
