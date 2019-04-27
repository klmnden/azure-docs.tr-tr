---
title: Azure İzleyiciye Genel Bakış | Microsoft Docs
description: Azure hizmetleriniz ve uygulamalarınız için eksiksiz bir izleme stratejisine katkıda bulunan Microsoft hizmetleri ve işlevlerine genel bakış.
author: bwren
manager: carmonm
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: azure-monitor
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/26/2019
ms.author: bwren
ms.openlocfilehash: 0485f8e3b377ce94ec23a4a1a94eb7e189b0232b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60787232"
---
# <a name="azure-monitor-overview"></a>Azure İzleyiciye Genel Bakış

Azure İzleyici bulut alınan telemetri üzerinde çalışan toplama ve analiz için kapsamlı bir çözüm sunarak kullanılabilirlik ve uygulamalarınızın performansını en üst düzeye çıkarır ve şirket içi Ortamlarınızdaki. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur.

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagram, Azure İzleyici üst düzey bir görünümünü sağlar. Diyagram merkezde, ölçüm ve günlükleri, Azure İzleyici tarafından veri kullanımı iki temel türler için veri depolarıdır. Solda [veri izleme kaynakları](platform/data-sources.md) , doldurmak bu [veri depoları](platform/data-platform.md). Sağ tarafta, uyarı ve dış sisteme akışı toplanan verileri analiz gibi Azure İzleyici gerçekleştiren farklı işlevlerdir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

![Azure İzleyiciye Genel Bakış](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>İzleme veri platformu
Azure İzleyici tarafından toplanan tüm verileri iki temel türlerinden birine uyan [ölçümlerini ve günlüklerini](platform/data-platform.md). [Ölçümleri](platform/data-platform-metrics.md) zaman içinde belirli bir noktada bir sistem bazı yönlerini açıklayan bir sayısal değerler. Bunlar, basit ve gerçek zamanlı senaryoları destekleme yeteneği. [Günlükleri](platform/data-platform-logs.md) farklı türde kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve verileri içerir. Olaylarla ve izlemelerle gibi telemetri depolanır günlükleri olarak ayrıca performans verilerini ve böylece tüm analiz için birleştirilebilir.

Birçok Azure kaynağı için kendi genel bakış sayfasında Azure portalında Azure İzleyicisi'ni sağ tarafından toplanan verileri görürsünüz. Herhangi bir sanal makineye bir göz gibi sahip ve performans ölçümlerini görüntüleme, birden fazla grafik görürsünüz. Tüm Grafik verileri açmak için tıklayın [ölçüm Gezgini](platform/metrics-charts.md) Azure portalında, zaman içinde birden çok ölçüm değerleri grafik olanak tanıyan.  Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin.

![Ölçümler](media/overview/metrics.png)

Azure İzleyici tarafından toplanan günlük verilerini analiz ile [sorguları](log-query/log-query-overview.md) hızlı bir şekilde almak, birleştirmek ve toplanan verileri çözümlemek için.  Oluşturma ve test sorguları kullanarak [Log Analytics](log-query/portals.md) Azure portalında ve ardından ya da doğrudan bu araçları kullanarak verileri analiz etmek veya ile kullanmak için sorguları Kaydet [görselleştirmeler](visualizations.md) veya [Uyarısı kuralları](platform/alerts-overview.md).

Azure İzleyici, bir sürümünü kullanan [Kusto sorgu dili](/azure/kusto/query/) basit günlük sorgular ancak ayrıca toplamalar, birleştirmeler ve akıllı analiz gibi gelişmiş işlevleri içerir, uygun olan Azure Veri Gezgini tarafından kullanılır. Sorgu dilini kullanarak hızla edinebilirsiniz [birden çok dersleri](log-query/get-started-queries.md).  [SQL](log-query/sql-cheatsheet.md) ve [Splunk](log-query/splunk-cheatsheet.md)’u önceden bilen kullanıcılara belirli yönergeler sağlanır.

![Günlükler](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Azure İzleyici hangi verileri toplar?
Azure İzleyici, çeşitli kaynaklardan veri toplayabilir. Uygulamanız, herhangi bir işletim sistemi ve platform aşağı, bağımlı hizmetler arasında değişen katmanlarındaki uygulamalarınız için veri izleme düşünebilirsiniz. Azure İzleyici her aşağıdaki katmanları alanından veri toplar:

- **Uygulama izleme verilerini**: Performansı ve işlevselliği, platforma bakılmaksızın yazdığınız kodun ilgili veriler.
- **Konuk işletim sistemi izleme verileri**: Uygulamanızın üzerinde çalıştığı işletim sistemiyle ilgili veriler. Bu Azure, başka bir bulutta veya şirket içinde çalışıyor olabilir. 
- **Azure kaynak verilerini izleme**: Bir Azure kaynağının çalışması hakkında veriler.
- **İzleme verileri bir azure aboneliği**: Azure işlem ve sistem durumu hakkında veriler yanı sıra, işlem ve bir Azure aboneliğinin yönetim verileri kendisini. 
- **İzleme verileri bir azure kiracısı**: Azure Active Directory gibi Azure hizmetlerinin Kiracı düzeyinde çalışması hakkında veriler.

Bir Azure aboneliği ve sanal makineler ve web uygulamaları gibi kaynakları eklemeye başlayın oluşturduğunuz hemen sonra Azure İzleyici, veri toplamaya başlar.  [Etkinlik günlükleri](platform/activity-logs-overview.md) kaynakları, oluşturulacak veya değiştirilecek kayıt. [Ölçümleri](platform/data-platform.md) kaynak nasıl performans gösterdiğini ve onu kullanan kaynakları söyleyin. 

Gerçek işlem kaynakları tarafından içine toplama verileri genişletmek [tanılamayı etkinleştirme](platform/diagnostic-logs-overview.md) ve [aracı ekleme](platform/agent-windows.md) işlem kaynakları için. Bu kaynağın iç işlem için telemetri toplar ve yapılandırmak farklı izin [veri kaynakları](platform/agent-data-sources.md) Windows ve Linux konuk işletim sisteminden günlükleri ve ölçümleri toplamak için. 

[Bir izleme paketi uygulamanıza eklemek](app/azure-web-apps.md), sayfa görüntülemeleri, uygulama isteklerini ve özel durumlar dahil olmak üzere, uygulamanız hakkında ayrıntılı bilgi toplamak Application ınsights'ı etkinleştirmek için. Daha fazla yapılandırarak uygulamanızın kullanılabilirliğini doğrulayın bir [kullanılabilirlik testi](app/monitor-web-app-availability.md) kullanıcı trafiğinin benzetimini yapmak için.

### <a name="custom-sources"></a>Özel kaynaklar
Azure İzleyicisi'ni kullanarak herhangi bir REST istemcisinden günlük verilerini toplayabilir [veri toplayıcı API'sini](platform/data-collector-api.md). Bu, özel izleme senaryoları oluşturmanıza ve diğer kaynakları aracılığıyla telemetri sunmayın kaynaklara izleme genişletmek sağlar.



## <a name="insights"></a>Insights
İzleme verileri yalnızca, bilgi işlem ortamınızın işlemi görünürlük artırabilirsiniz yararlıdır. Azure İzleyici, çeşitli özellikler ve uygulamalarınızın ve bağımlı oldukları diğer kaynakları değerli Öngörüler sağlayan araçları içerir. [İzleme çözümleri](insights/solutions.md) ve gibi özellikleri [Application Insights](app/app-insights-overview.md) ve [kapsayıcılar için Azure İzleyici](insights/container-insights-overview.md) uygulama ve belirli bir Azure farklı yönlerini derin Öngörüler sağlayın Hizmetler. 

### <a name="application-insights"></a>Application Insights
[Application Insights](app/app-insights-overview.md) kullanılabilirliğine, performansına ve kullanımına web uygulamalarınızı bulutta veya şirket içinde barındırılan olup olmadığını izler. Bu, güçlü veri analizi platformu, uygulamanızın işlem derin Öngörüler sağlar ve bunları rapor bir kullanıcının bildirmesini beklemeden hataları tanılamak için Azure İzleyici'de yararlanır. Application Insights, geliştirme araçları çeşitli bağlantı noktaları içerir ve DevOps işlemlerinizi desteklemek için Visual Studio ile tümleşir.

![App Insights](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
[Kapsayıcılar için Azure İzleyici](insights/container-insights-overview.md) dağıtılan Azure Kubernetes Service (AKS) barındırılan yönetilen Kubernetes kümeleri için kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir. Bunu, toplama bellek ve işlemci ölçümleri performans görünürlük denetleyicileri, düğümleri ve Kubernetes ölçümleri API'si aracılığıyla kullanılabilen kapsayıcılar sunar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerdeki izleme etkinleştirdikten sonra bu ölçüm ve günlükleri otomatik olarak sizin için Linux için Log Analytics aracısını kapsayıcı bir sürümü aracılığıyla toplanır.

![Kapsayıcı durumu](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
[VM'ler için Azure İzleyici](insights/vminsights-overview.md) Azure sanal makinelerinizi (VM) Windows ve Linux Vm'leri, farklı işlemler ve diğer kaynakları ve dış birbirine bağımlılıkları da dahil olmak üzere, sistem durumu ve performansı çözümleyerek ölçekli olarak izler. işler. Uygulama bağımlılıkları VM'ler için şirket içi veya başka bir bulut sağlayıcısı barındırılan ve çözüm, performans izleme için destek içerir.  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>İzleme çözümleri
[İzleme çözümleri](insights/solutions.md) Azure İzleyici'de belirli bir uygulama veya hizmet için Öngörüler sağlayan paketlenmiş mantık kümeleridir. Mantıksal uygulama veya hizmetine ilişkin izleme verilerini toplamak için içerirler [sorguları](log-query/log-query-overview.md) bu verileri analiz etmek ve [görünümleri](../log-analytics/log-analytics-view-designer.md) görselleştirme için. İzleme çözümleri [Microsoft'tan kullanılabilir](insights/solutions-inventory.md) ve iş ortakları, çeşitli Azure Hizmetleri ve diğer uygulamalar için izleme sağlamak için.

![İzleme çözümleri](media/overview/solutions-overview.png)

## <a name="responding-to-critical-situations"></a>Kritik durumlar için yanıtlama
İzleme verileri etkileşimli olarak çözümlemek için yayımlamanızı etkili bir izleme çözümü topladığı verileri tanımlanan kritik koşulları proaktif olarak yanıt mümkün olması gerekir. Bu bir metin veya posta yönetici olarak bir sorunu araştırmaya sorumlu gönderemiyor. Veya bir hata koşulu gidermek için deneyen otomatik bir işlem başlatabilir.


### <a name="alerts"></a>Uyarılar
[Azure İzleyici'de uyarılar](platform/alerts-overview.md) kritik koşulları proaktif olarak bildiren ve olası düzeltici dener. Neredeyse gerçek zamanlı uyarı kuralları günlüklerine göre birden çok kaynaktan veri üzerinde karmaşık mantık için izin verirken, sayısal değerlerine göre ölçümlere göre uyarı kuralları sağlar.

Uyarı kuralları Azure İzleyici kullanımda [Eylem grupları](platform/action-groups.md), benzersiz alıcı ve birden çok kural arasında paylaşılabilir Eylemler kümesi bulunur. Gereksinimlerinize göre Eylem grupları olarak uyarıları dış eylemleri başlatmak veya ITSM araçlarınıza ile tümleştirmek için Web kancalarını kullanma gibi işlemleri gerçekleştirebilirsiniz.

![Uyarılar](media/overview/alerts.png)

### <a name="autoscale"></a>Otomatik Ölçeklendirme
Otomatik ölçeklendirme, uygulamanızın üzerindeki yükü işlemek için çalışan kaynakları doğru miktarda sahip olmanızı sağlar. Boşta zaman otomatik olarak yük artışlarını işlemek ve da oturan kaynakları kaldırarak tasarruf etmek için kaynak ekleme belirlemek için Azure İzleyici tarafından toplanan ölçümleri kullanan kuralları oluşturmanıza olanak sağlar. Örnekler ve ne zaman artırın veya azaltın kaynakları için mantıksal minimum ve maksimum sayısını belirtin.

![Otomatik Ölçeklendirme](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>İzleme verilerini Görselleştirme
[Görselleştirmeler](visualizations.md) grafikleri ve tabloları özetlemeye ve çoğaltmaya verilerin izlenmesi için farklı Hedef Kitleleri sunmak için etkili Araçlar gibi. Azure İzleyici, izleme verilerini görselleştirmek için kendi özellikleri vardır ve diğer Azure Hizmetleri için farklı Hedef Kitleleri yayımlama yararlanır.

### <a name="dashboards"></a>Panolar
[Azure panoları](../azure-portal/azure-portal-dashboards.md) farklı türlerde veri, birleştirmek izin hem ölçümlerini ve günlüklerini tek bir bölmede içine dahil olmak üzere [Azure portalında](https://portal.azure.com). İsteğe bağlı olarak, Azure diğer kullanıcılarla Pano paylaşabilir. Azure İzleyici boyunca öğeleri herhangi bir günlük sorgusu veya ölçümleri grafiğe çıktısını yanı sıra bir Azure panosunu eklenebilir. Örneğin, ölçümler, etkinlik günlükleri tablosu, Application ınsights'tan bir kullanım grafiği ve bir günlük sorgusu çıkışını bir grafiğini göster kutucukları birleştiren bir Pano oluşturabilirsiniz.

![Pano](media/overview/dashboard.png)

### <a name="views"></a>Görünümler
[Görünümleri](../log-analytics/log-analytics-view-designer.md) günlük verilerini Azure İzleyici'de görsel olarak sunar.  Her görünüm, çubuk ve çizgi kritik verileri özetleyen listelerin yanı sıra grafikler gibi görselleştirmelerin birleşimini aşağı ayrıntılı açıklanmıştır tek bir kutucuk içerir.  İzleme çözümleri, belirli bir uygulama için verileri özetleyen görünümleri içerir ve herhangi bir günlük sorgu verileri sunmak için kendi görünümlerinizi oluşturabilirsiniz. Azure İzleyicisi'nde diğer unsurları gibi görünümler, Azure panolara eklenebilir.

![Görünüm](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) çok çeşitli veri kaynakları üzerinde etkileşimli görselleştirmeler sağlayan bir İş analizi hizmetidir ve veri başkalarına kullanılabilir içinde ve kuruluşunuz dışındaki hale getirme etkili bir yoludur. Power BI için yapılandırabileceğiniz [günlük verilerini Azure İzleyici'den otomatik olarak içeri](../log-analytics/log-analytics-powerbi.md) bu ek görselleştirmeler yararlanmak için.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Tümleştirme ve veri dışarı aktarma
Genellikle, Azure İzleyici diğer sistemlerle tümleştirmek için ve izleme verilerinizi kullanan özel çözümler oluşturmak için gereksinim sahip olacaksınız. Bu tümleştirme sağlamak için Azure İzleyici ile diğer Azure hizmetleriyle çalışır.

### <a name="event-hub"></a>Olay Hub'ı
[Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs) , dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işleme/depolama bağdaştırıcısı kullanarak verileri depolama bir akış platformu ve olay alma hizmetidir. Event Hubs kullanan [Azure İzleyici, veri akışı](platform/stream-monitoring-data-event-hubs.md) ortak SIEM ve izleme araçları için.


### <a name="logic-apps"></a>Logic Apps
[Logic Apps](https://azure.microsoft.com/services/logic-apps) , görevleri ve farklı sistemlerde ve hizmetlerde ile tümleştirilen iş akışlarını kullanarak iş süreçlerini otomatik hale getirmenizi sağlayan bir hizmettir. Etkinlikler kullanılabilir okuma ve çeşitli diğer sistemleri ile tümleştirme iş akışları oluşturmanıza olanak sağlayan Azure İzleyici'de, ölçüm ve günlükleri yazma.


### <a name="api"></a>API
Birden çok API'ları okuyup ölçüm ve günlükleri için ve Azure İzleyici'den oluşturulan uyarılar erişmenin yanı sıra kullanılabilir. Ayrıca, yapılandırabilir ve uyarılar almak. Bu, Azure İzleyici ile tümleştirilebilen özel çözümler oluşturmak için temel olarak sınırsız olasılıklar sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakiler hakkında daha fazla bilgi edinin:

* [Ölçüm ve günlükleri](platform/data-platform.md) Azure İzleyici tarafından toplanan veriler için.
* [Veri kaynakları](platform/data-sources.md) nasıl uygulamanızın farklı bileşenlerini telemetri göndermek için.
* [Oturum sorguları](log-query/log-query-overview.md) toplanan verileri analiz etmek için.