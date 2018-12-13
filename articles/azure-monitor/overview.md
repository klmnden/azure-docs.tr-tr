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
ms.date: 09/26/2018
ms.author: bwren
ms.openlocfilehash: 2d1f96359512a3c2135909ebf69ec9ec3b801d61
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53190569"
---
# <a name="azure-monitor-overview"></a>Azure İzleyiciye Genel Bakış

Azure İzleyici bulut alınan telemetri üzerinde çalışan toplama ve analiz için kapsamlı bir çözüm sunarak kullanılabilirlik ve uygulamalarınızın performansını en üst düzeye çıkarır ve şirket içi Ortamlarınızdaki. Uygulamalarınızın performansını anlamanıza ve uygulamalarla bağlı oldukları kaynakları etkileyen sorunları önceden tespit etmenize yardımcı olur.

> [!VIDEO https://www.youtube.com/embed/_hGff5bVtkM]

## <a name="overview"></a>Genel Bakış
Aşağıdaki diyagram, Azure İzleyici üst düzey bir görünümünü sağlar. Diyagram merkezde, Ölçümler ve veri kullanımı Azure İzleyici tarafından iki temel türü olan günlükler için veri depolarıdır. Solda [telemetri farklı toplama kaynakları izlenen kaynakları](../azure-monitor/platform/data-sources.md) ve doldurma [veri depoları](../azure-monitor/platform/data-collection.md). Sağ tarafta, uyarı ve dış sisteme akışı toplanan verileri analiz gibi Azure İzleyici gerçekleştiren farklı işlevlerdir.


![Azure İzleyiciye Genel Bakış](media/overview/overview.png)


## <a name="monitoring-data-platform"></a>İzleme veri platformu
Azure İzleyici tarafından toplanan tüm verileri iki temel türlerinden birine uyan [ölçümlerini ve günlüklerini](../azure-monitor/platform/data-collection.md). [Ölçümleri](../azure-monitor/platform/data-collection.md#metrics) zaman içinde belirli bir noktada bir sistem bazı yönlerini açıklayan bir sayısal değerler. Bunlar, basit ve gerçek zamanlı senaryoları destekleme yeteneği. [Günlükleri](../azure-monitor/platform/data-collection.md#logs) farklı türde kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve verileri içerir. Olaylarla ve izlemelerle gibi telemetri depolanır günlükleri olarak ayrıca performans verilerini ve böylece tüm analiz için birleştirilebilir.

Birçok Azure kaynağı için kendi genel bakış sayfasında Azure portalında Azure İzleyicisi'ni sağ tarafından toplanan verileri görürsünüz. Herhangi bir sanal makineye bir göz gibi sahip ve performans ölçümlerini görüntüleme, birden fazla grafik görürsünüz. Tüm Grafik verileri açmak için tıklayın [ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md) Azure portalında, zaman içinde birden çok ölçüm değerleri grafik olanak tanıyan.  Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin.

![Ölçümler](media/overview/metrics.png)

Azure İzleyici tarafından toplanan günlük verilerini içeren Log Analytics'e depolanan bir [zengin sorgu dili](../azure-monitor/log-query/log-query-overview.md) hızlı bir şekilde almak, birleştirmek ve toplanan verileri çözümlemek için.  Oluşturma ve test sorguları kullanarak [Log Analytics sayfa](../azure-monitor/log-query/portals.md) ile kullanılmak üzere sorguları kaydedebilir ya da doğrudan Azure portalını sonra da bu araçları kullanarak verileri analiz ederek [görselleştirmeler](visualizations.md) veya [ Uyarı kuralları](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

Log Analytics sorgu dili basit günlük sorguları için uygundur, ancak ayrıca toplamalar, birleştirmeler ve akıllı analiz gibi gelişmiş işlevleri içerir. Sorgu dilini kullanarak hızla edinebilirsiniz [birden çok dersleri](../azure-monitor/log-query/get-started-queries.md) kullanılabilir.  [SQL](../azure-monitor/log-query/sql-cheatsheet.md) ve [Splunk](../azure-monitor/log-query/splunk-cheatsheet.md)’u önceden bilen kullanıcılara belirli yönergeler sağlanır.

![Günlükler](media/overview/logs.png)

## <a name="what-data-does-azure-monitor-collect"></a>Azure İzleyici hangi verileri toplar?
Azure İzleyici, çeşitli kaynaklardan veri toplayabilir. Uygulamanız, herhangi bir işletim sistemi ve platform aşağı, bağımlı hizmetler arasında değişen katmanlarındaki uygulamalarınız için veri izleme düşünebilirsiniz. Azure İzleyici her aşağıdaki katmanları alanından veri toplar:

- **Uygulama izleme verilerini**: Performansı ve işlevselliği, platforma bakılmaksızın yazdığınız kodun ilgili veriler.
- **Konuk işletim sistemi izleme verileri**: Uygulamanızın üzerinde çalıştığı işletim sistemiyle ilgili veriler. Bu Azure, başka bir bulutta veya şirket içinde çalışıyor olabilir. 
- **Azure kaynak verilerini izleme**: Bir Azure kaynağının çalışması hakkında veriler.
- **İzleme verileri bir azure aboneliği**: Azure işlem ve sistem durumu hakkında veriler yanı sıra, işlem ve bir Azure aboneliğinin yönetim verileri kendisini. 
- **İzleme verileri bir azure kiracısı**: Azure Active Directory gibi Azure hizmetlerinin Kiracı düzeyinde çalışması hakkında veriler.

Bir Azure aboneliği ve sanal makineler ve web uygulamaları gibi kaynakları eklemeye başlayın oluşturduğunuz hemen sonra Azure İzleyici, veri toplamaya başlar.  [Etkinlik günlükleri](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) kaynakları, oluşturulacak veya değiştirilecek kayıt. [Ölçümleri](../monitoring-and-diagnostics/monitoring-overview-metrics.md) kaynak nasıl performans gösterdiğini ve onu kullanan kaynakları söyleyin. 

Gerçek işlem kaynakları tarafından içine toplama verileri genişletmek [tanılamayı etkinleştirme](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) ve [aracı ekleme](../azure-monitor/platform/agent-windows.md) işlem kaynakları için. Bu kaynağın iç işlem için telemetri toplar ve yapılandırmak farklı izin [veri kaynakları](../azure-monitor/platform/agent-data-sources.md) Windows ve Linux konuk işletim sisteminden günlükleri ve ölçümleri toplamak için. 

[Bir izleme paketi uygulamanıza eklemek](../application-insights/app-insights-azure-web-apps.md), sayfa görüntülemeleri, uygulama isteklerini ve özel durumlar dahil olmak üzere, uygulamanız hakkında ayrıntılı bilgi toplamak Application ınsights'ı etkinleştirmek için. Daha fazla yapılandırarak uygulamanızın kullanılabilirliğini doğrulayın bir [kullanılabilirlik testi](../application-insights/app-insights-monitor-web-app-availability.md) kullanıcı trafiğinin benzetimini yapmak için.

### <a name="custom-sources"></a>Özel kaynaklar
Azure İzleyicisi'ni kullanarak herhangi bir REST istemcisinden günlük verilerini toplayabilir [veri toplayıcı API'sini](../azure-monitor/platform/data-collector-api.md). Bu, özel izleme senaryoları oluşturmanıza ve diğer kaynakları aracılığıyla telemetri sunmayın kaynaklara izleme genişletmek sağlar.



## <a name="insights"></a>Insights
İzleme verileri yalnızca, bilgi işlem ortamınızın işlemi görünürlük artırabilirsiniz yararlıdır. Azure İzleyici, çeşitli özellikler ve uygulamalarınızın ve bağımlı oldukları diğer kaynakları değerli Öngörüler sağlayan araçları içerir. [İzleme çözümleri](../azure-monitor/insights/solutions.md) ve gibi özellikleri [Application Insights](../application-insights/app-insights-overview.md) ve kapsayıcı öngörüleri, uygulama ve belirli Azure hizmetlerinin farklı yönlerini derin Öngörüler sağlayın. 

### <a name="application-insights"></a>Application Insights
[Application Insights](../application-insights/app-insights-overview.md) kullanılabilirliğine, performansına ve kullanımına web uygulamalarınızı bulutta veya şirket içinde barındırılan olup olmadığını izler. Bu, uygulamanızın işlem derin Öngörüler sağlar ve bunları rapor bir kullanıcının bildirmesini beklemeden hataları tanılamak için Log analytics'te güçlü veri analizi platformu yararlanır. Application Insights, geliştirme araçları çeşitli bağlantı noktaları içerir ve DevOps işlemlerinizi desteklemek için Visual Studio ile tümleşir.

![App Insights](media/overview/app-insights.png)

### <a name="azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici
Kapsayıcılar için Azure İzleyici, yönetilen Azure Kubernetes Service (AKS) barındırılan Kubernetes kümelerini dağıtılan kapsayıcı iş yüklerinin performansını izlemek için tasarlanmış bir özelliktir. Bunu, toplama bellek ve işlemci ölçümleri performans görünürlük denetleyicileri, düğümleri ve Kubernetes ölçümleri API'si aracılığıyla kullanılabilen kapsayıcılar sunar. Kapsayıcı günlükleri de toplanır.  Kubernetes kümelerdeki izleme etkinleştirdikten sonra bu ölçüm ve günlükleri otomatik olarak sizin için Linux için Log Analytics aracısını kapsayıcı bir sürümü aracılığıyla toplanan ve Log Analytics'te depolanır.

![Kapsayıcı durumu](media/overview/container-insights.png)

### <a name="azure-monitor-for-vms"></a>VM'ler için Azure İzleyici
Azure İzleyici VM içgörüler izler, Azure sanal makineleri (VM) uygun ölçekte Windows ve Linux Vm'leri, farklı işlemler ve diğer kaynakları ve dış işlemlere birbirine bağımlılıkları da dahil olmak üzere, sistem durumu ve performansı çözümleyerek. Uygulama bağımlılıkları VM'ler için şirket içi veya başka bir bulut sağlayıcısı barındırılan ve çözüm, performans izleme için destek içerir.  


![VM Insights](media/overview/vm-insights.png)

### <a name="monitoring-solutions"></a>İzleme çözümleri
[İzleme çözümleri](../azure-monitor/insights/solutions.md) Azure İzleyici'de belirli bir uygulama veya hizmet için Öngörüler sağlayan paketlenmiş mantık kümeleridir. Bunlar verileri Log Analytics'e kullanarak diğer izleme verilerinin yanı sıra toplama [sorguları](../azure-monitor/log-query/log-query-overview.md) analiz ve [görünümleri](../azure-monitor/platform/view-designer.md) görselleştirme için. İzleme çözümleri [Microsoft'tan kullanılabilir](../azure-monitor/insights/solutions-inventory.md) ve iş ortakları, çeşitli Azure Hizmetleri ve diğer uygulamalar için izleme sağlamak için.

![İzleme çözümleri](media/overview/solutions-overview.png)

## <a name="responding-to-critical-situations"></a>Kritik durumlar için yanıtlama
İzleme verileri etkileşimli olarak çözümlemek için yayımlamanızı etkili bir izleme çözümü topladığı verileri tanımlanan kritik koşulları proaktif olarak yanıt mümkün olması gerekir. Bu bir metin veya posta yönetici olarak bir sorunu araştırmaya sorumlu gönderemiyor. Veya bir hata koşulu gidermek için deneyen otomatik bir işlem başlatabilir.


### <a name="alerts"></a>Uyarılar
[Azure İzleyici'de uyarılar](../monitoring-and-diagnostics/monitoring-overview-alerts.md) kritik koşulları proaktif olarak bildiren ve olası düzeltici dener. Neredeyse gerçek zamanlı uyarı kuralları günlüklerine göre birden çok kaynaktan veri üzerinde karmaşık mantık için izin verirken, sayısal değerlerine göre ölçümlere göre uyarı kuralları sağlar.

Uyarı kuralları Azure İzleyici kullanımda [Eylem grupları](../azure-monitor/platform/action-groups.md), benzersiz alıcı ve birden çok kural arasında paylaşılabilir Eylemler kümesi bulunur. Gereksinimlerinize göre Eylem grupları olarak uyarıları dış eylemleri başlatmak veya ITSM araçlarınıza ile tümleştirmek için Web kancalarını kullanma gibi işlemleri gerçekleştirebilirsiniz.

![Uyarılar](media/overview/alerts.png)

### <a name="autoscale"></a>Otomatik Ölçeklendirme
Otomatik ölçeklendirme, uygulamanızın üzerindeki yükü işlemek için çalışan kaynakları doğru miktarda sahip olmanızı sağlar. Boşta zaman otomatik olarak yük artışlarını işlemek ve da oturan kaynakları kaldırarak tasarruf etmek için kaynak ekleme belirlemek için Azure İzleyici tarafından toplanan ölçümleri kullanan kuralları oluşturmanıza olanak sağlar. Örnekler ve ne zaman artırın veya azaltın kaynakları için mantıksal minimum ve maksimum sayısını belirtin.

![Otomatik Ölçeklendirme](media/overview/autoscale.png)

## <a name="visualizing-monitoring-data"></a>İzleme verilerini Görselleştirme
[Görselleştirmeler](visualizations.md) grafikleri ve tabloları özetlemeye ve çoğaltmaya verilerin izlenmesi için farklı Hedef Kitleleri sunmak için etkili Araçlar gibi. Azure İzleyici, izleme verilerini görselleştirmek için kendi özellikleri vardır ve diğer Azure Hizmetleri için farklı Hedef Kitleleri yayımlama yararlanır.

### <a name="dashboards"></a>Panolar
[Azure panoları](../azure-portal/azure-portal-dashboards.md) farklı türlerde veri, birleştirmek izin hem ölçümlerini ve günlüklerini tek bir bölmede içine dahil olmak üzere [Azure portalında](https://portal.azure.com). İsteğe bağlı olarak, Azure diğer kullanıcılarla Pano paylaşabilir. Azure İzleyici boyunca öğeleri herhangi bir günlük sorgusu veya ölçümleri grafiğe çıktısını yanı sıra bir Azure panosunu eklenebilir. Örneğin, Log Analytics'ten ölçümler, etkinlik günlükleri tablosu, Application ınsights'tan bir kullanım grafiği ve bir sorgu çıktısı bir grafiğini göster kutucukları birleştiren bir Pano oluşturabilirsiniz.

![Pano](media/overview/dashboard.png)

### <a name="views"></a>Görünümler
[Azure İzleyici görünümlerde](../azure-monitor/platform/view-designer.md) günlük verileri Log Analytics'e görsel olarak sunar.  Her görünüm, çubuk ve çizgi kritik verileri özetleyen listelerin yanı sıra grafikler gibi görselleştirmelerin birleşimini aşağı ayrıntılı açıklanmıştır tek bir kutucuk içerir.  İzleme çözümleri, belirli bir uygulama için verileri özetleyen görünümleri içerir ve bir Log Analytics günlük araması verileri sunmak için kendi görünümlerinizi oluşturabilirsiniz. Azure İzleyicisi'nde diğer unsurları gibi görünümler, Azure panolara eklenebilir.

![Log Analytics Görünümü](media/overview/view.png)

### <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com) çok çeşitli veri kaynakları üzerinde etkileşimli görselleştirmeler sağlayan bir İş analizi hizmetidir ve veri başkalarına kullanılabilir içinde ve kuruluşunuz dışındaki hale getirme etkili bir yoludur. Power BI için yapılandırabileceğiniz [günlük verilerini Azure İzleyici'den otomatik olarak içeri](../azure-monitor/platform/powerbi.md) bu ek görselleştirmeler yararlanmak için.


![Power BI](media/overview/power-bi.png)


## <a name="integrate-and-export-data"></a>Tümleştirme ve veri dışarı aktarma
Genellikle, Azure İzleyici diğer sistemlerle tümleştirmek için ve izleme verilerinizi kullanan özel çözümler oluşturmak için gereksinim sahip olacaksınız. Bu tümleştirme sağlamak için Azure İzleyici ile diğer Azure hizmetleriyle çalışır.

### <a name="event-hub"></a>Olay Hub'ı
[Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs) , dönüştürme ve herhangi bir gerçek zamanlı analiz sağlayıcısı veya toplu işleme/depolama bağdaştırıcısı kullanarak verileri depolama bir akış platformu ve olay alma hizmetidir. Event Hubs kullanan [akış günlük verilerini Azure İzleyici](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) ortak SIEM ve izleme araçları için.

> [!VIDEO https://www.youtube.com/embed/SPHxCgbcvSw]

### <a name="logic-apps"></a>Logic Apps
[Logic Apps](https://azure.microsoft.com/services/logic-apps) , görevleri ve farklı sistemlerde ve hizmetlerde ile tümleştirilen iş akışlarını kullanarak iş süreçlerini otomatik hale getirmenizi sağlayan bir hizmettir. Etkinlikler kullanılabilir okuma ve çeşitli diğer sistemleri ile tümleştirme iş akışları oluşturmanıza olanak sağlayan Azure İzleyici'de, ölçüm ve günlükleri yazma.

![Logic App](platform/media/collect-activity-logs-subscriptions/log-analytics-logic-apps-activity-log-overview.png)

### <a name="api"></a>API
Birden çok API'ları okuyup ölçüm ve günlükleri için ve Azure İzleyici'den oluşturulan uyarılar erişmenin yanı sıra kullanılabilir. Ayrıca, yapılandırabilir ve uyarılar almak. Bu, Azure İzleyici ile tümleştirilebilen özel çözümler oluşturmak için temel olarak sınırsız olasılıklar sağlar.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdakiler hakkında daha fazla bilgi edinin:

* [Ölçüm ve günlükleri](../azure-monitor/platform/data-collection.md) Azure İzleyici tarafından toplanan veriler için.
* [Veri kaynakları](../azure-monitor/platform/data-sources.md) nasıl uygulamanızın farklı bileşenlerini telemetri göndermek için.
* [Log Analytics](../azure-monitor/log-query/log-query-overview.md) toplanan verileri analiz etmek için.
