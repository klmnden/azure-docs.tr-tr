---
title: Toplama izleme verilerini azure'da | Microsoft Docs
description: Uygulama ve hizmetlerden Azure ve Araçları'ndaki toplanan izleme verilerinin genel bakış, analiz etmek için kullanılır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: bwren
ms.openlocfilehash: d3ebd512f8244de74c009ac8a2936ed8e817dad9
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38991512"
---
# <a name="collecting-monitoring-data-in-azure"></a>Azure izleme verilerini toplama
Bu makale, uygulama ve Hizmetleri Azure ve Araçları'ndaki toplanan izleme verilerinin özetini incelemek için kullanılan sağlar. 

## <a name="types-of-monitoring-data"></a>İzleme türleri
Tüm izleme verilerini birinin iki temel türler, Ölçümler ve günlükleri içine sığar. Her tür farklı özelliklere sahiptir ve aşağıda açıklandığı gibi belirli senaryolar için idealdir.

### <a name="metrics"></a>Ölçümler
Zaman içinde belirli bir noktada bir sistem bazı yönlerini açıklayan bir sayısal değerler ölçümleridir. Bunlar ayrı veri değerinin kendisi, değer toplandığı zaman dahil olmak üzere, ölçüm değeri temsil eder ve değeri ilişkili olduğu belirli bir kaynak türünü içerir. Ölçümler, değeri değiştirir olup olmadığını düzenli aralıklarla toplanır. Örneğin, her dakika ya da kullanıcı uygulamanıza her 10 dakikada oturum açmış bir sanal makineden işlemci kullanımı topla.

Ölçümler, basit ve gerçek zamanlı senaryoları destekleme yeteneği. Bunlar, ölçümler, sık olarak örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete uyarmak için özellikle yararlı olur. Örneğin, bir ölçüm eşiği değeri aştığında bir uyarı yangın veya iki ölçüm değeri arasındaki farkı belirli bir değere ulaştığında bir uyarı yangın.

Tek tek ölçüler genellikle küçük hakkında fikir, kendi sağlar. Basit eşik karşılaştırması dışında herhangi bir bağlam olmadan tek bir değer sağlarlar. Bunlar yine de modelleri ve eğilimlerini belirlemek için diğer ölçümleri ile birleştirildiğinde veya belirli değerleri etrafında bağlam sağlamak günlükleri ile birlikte kullanıldığında değerlidir. Örneğin, uygulamanız bir anda kullanıcıların belirli bir sayıda uygulama durumunu ilgili çok az söyleyebilirsiniz. Kullanıcılar yine de aynı Ölçüm birden çok değeri tarafından belirtilen bir ani düşme bir sorunu gösterebilir. Uygulama tarafından oluşturulan ve ayrı bir ölçüme göre belirtilen aşırı özel durumlar açılır neden olan bir uygulama sorunu tespit edebilecek. Uygulamanın bileşenleri hatası özellikle tanımlayan uygulama tarafından oluşturulan olayları kökenini tanımlanmasına yardımcı olabilir.

Günlüklerine göre uyarı ölçümlerine bağlı uyarılar olarak duyarlı değildir ancak daha karmaşık mantık dahil edebilirsiniz. Birden çok kaynaktan veri üzerinde karmaşık bir analiz gerçekleştirir herhangi bir sorgunun sonuçlarına dayalı bir uyarı oluşturabilirsiniz.

### <a name="logs"></a>Günlükler
Farklı türlerde veri kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve günlükleri içerir. Günlükleri ölçümler gibi sayısal değerler içermesi ancak genellikle ayrıntılı açıklamalar içeren metin verileri içerir. Bunlar daha fazla ölçüm arasından kendi yapısında farklılık, farklı ve düzenli aralıklarla toplanır değildir.

Ortak tür günlük girdisini bir olaydır. Olaylar, bir uygulama veya hizmet tarafından oluşturulan tutularak toplanır ve genellikle kendi başlarına tüm bağlam sağlamak için yeterli bilgi içerir.  Örneğin, bir olayı, belirli bir kaynak oluşturulduğunda veya değiştirildiğinde, yeni bir ana bilgisayar başlatma artan trafiğe yanıt olarak veya bir uygulamada bir hata algılandı olduğunu gösteriyor olabilir.

Günlükleri, zaman içinde eğilim ve kaynakları karmaşık bir analiz için çeşitli verilerin birleştirilmesi için özellikle yararlıdır. Veri biçimi farklılık gösterebileceğinden, ihtiyaç duydukları yapı kullanarak özel günlükleri uygulamalar oluşturabilirsiniz. Ölçümler, eğilimleri belirlemek için diğer izleme verilerinin ve diğer veri analizi ile birleştirilecek günlüklerinde bile çoğaltılabilir.


## <a name="monitoring-tools-in-azure"></a>Azure'da izleme araçları
İzleme verileri azure'da toplanır ve aşağıdaki bölümlerde açıklanan birden çok kaynakları kullanarak Analiz.

### <a name="azure-metrics"></a>Azure ölçümleri
Azure kaynaklarını ve uygulamaların ölçümleri Azure ölçümleri toplanır. Ölçüm verilerini olarak seçilen makine için CPU ve ağ kullanımı gibi ölçümlerinin grafiklerini içeren sanal makineler gibi belirli Azure kaynakları için Azure portalında sayfalarıyla tümleşiktir. İle de çözümlenebilir [ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md) olduğu grafik birden çok ölçüm değerleri zaman içinde.  Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. İle ölçümleri de alabilirsiniz [Azure REST API izleme](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Çeşitli Azure kaynakları tarafından toplanan bir ölçüm verileri üzerinde ayrıntılı alabilirsiniz [azure'da veri izleme kaynakları](monitoring-data-sources.md). 

![Ölçüm Gezgini](media/monitoring-data-collection/metrics-explorer.png)


### <a name="azure-activity-log"></a>Azure etkinlik günlüğü 
[Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) günlükleri yapılandırması ve Azure hizmetlerinin durumunu depolar. Etkinlik günlüğü Gezgini, Azure portalında Bu günlükleri görüntülemek için kullanabilirsiniz, ancak genellikle oldukları [Log Analytics'e kopyalanan](../log-analytics/log-analytics-activity.md) diğer günlük verilerinizi Analiz edilecek.

Etkinlik günlüğü Gezgini belirli ölçütlerle eşleşmesi için filtre etkinlik günlüğü görüntülemek için kullanabilirsiniz.  En fazla kaynak, ayrıca Azure portalında etkinlik günlüğü Gezgini görüntüler kendi menüsündeki seçeneği, bu kaynak için filtrelenmiş bir etkinlik günlüğü sahip olacaksınız. Etkinlik günlükleri ile de alabilirsiniz [Azure REST API izleme](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Etkinlik günlüğü Gezgini](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics, Azure yönetimi için ortak bir veri platformu sağlar. Bu, depolama ve Azure, veri toplama aracıları sanal makineler, yönetim çözümlerine ve farklı Azure kaynakları gibi kaynakları çeşitli günlükler analiz için kullanılan birincil hizmettir. Ölçümler ve etkinlik günlüğüne dahil olmak üzere diğer kaynaklardan verileri Log Analytics'e izleme verilerinin tam bir merkezi depo oluşturmak için kopyalanabilir.

Log Analytics topladığı verileri analiz etmek için zengin bir sorgu dilini sahiptir.  Kullanabileceğiniz [günlük araması portalları](../log-analytics/log-analytics-log-search-portals.md) etkileşimli olarak yazılması ve sorguları test etme ve sonuçları analiz etme. Ayrıca [görünümleri oluşturma](../log-analytics/log-analytics-view-designer.md) sonuçları günlük aramalarınızı görselleştirebilir veya bir Azure panosunu için doğrudan bir sorgunun sonuçlarını yapıştırın.  Yönetim çözümleri günlük aramaları ve görünümleri Log Analytics'te bunlar topladığı verileri çözümlemek için içerir. Application Insights gibi başka hizmetler verilerini Log Analytics'te depolar ve analiz için ek araçlar sağlar.  

![Günlükler](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights, farklı platformlarda üzerinde yüklü web uygulamaları için telemetri toplar. Bu, Azure ölçümleri ve Log Analytics verilerini depolayan ve kapsamlı bir analiz etme ve verilerini görselleştirme için bu verileri çözümlemek için mevcut araçları üzerine zengin araçlar kümesi sağlar. Bu, uyarılar, günlük aramaları ve diğer izleme için kullanmayı panolar gibi hizmetleri ortak bir dizi yararlanmanıza olanak sağlar.


![App Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Hizmet Eşlemesi
Hizmet eşlemesi, sanal makineler işlemleri ve bağımlılıkları görsel bir gösterimini sağlar. Diğer yönetim verilerinizi analiz edebilmesi çoğu bu veriler, Log Analytics'te depolar. Hizmet eşlemesi konsolu ayrıca Log Analytics, çözümlenen sanal makine bağlamında sunmak için verileri alır.

![Hizmet Eşlemesi](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>İzleme verilerini aktarma

### <a name="metrics-to-logs"></a>Günlükler için ölçümleri
Ölçümler, diğer veri türleri, zengin bir sorgu dilini kullanarak karmaşık bir analiz gerçekleştirmek için Log Analytics içine de çoğaltılabilir. Ayrıca, zaman içinde eğilim gerçekleştirmenize olanak tanıyan ölçümleri, daha uzun süre günlük verileri koruyabilirsiniz. Zaman ölçümleri veya diğer herhangi bir performans verilerini verileri bir günlük olarak davranan Log analytics'te depolanır. Neredeyse gerçek zamanlı analiz ve eğilim ve analiz diğer verilerle için günlükleri kullanırken uyarı desteklemek için ölçümleri kullanın.

Azure kaynaklardan ölçümleri toplamaya yönelik rehberlik alabilirsiniz [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](../log-analytics/log-analytics-azure-storage.md). Azure PaaS kaynakları'ndan kaynakları ölçümleri toplamaya ilişkin yönergeler almak [Log Analytics ile Azure PaaS kaynak ölçümleri toplamayı yapılandırmak](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Ölçümler için günlükleri
Yukarıda açıklandığı gibi daha düşük gecikme süresi ve daha düşük bir maliyetle uyarılar oluşturmanızı sağlayan bir günlük daha hızlı yanıt ölçümleridir. Log Analytics önemli ölçüde ölçümler için uygun olabilir, ancak Azure ölçümlerde saklanmaz sayısal veri toplar. Aracılar ve yönetim çözümlerinden toplanan performans verilerini buna yaygın bir örnektir. Bu değerlerden bazıları, ölçüm Gezgini ile analizi ve Uyarılar için kullanılabilir olduğu Azure ölçümleri halinde kopyalanabilir.

Bu özellik açıklaması kullanılabilir [daha hızlı ölçüm uyarıları artık sınırlı genel Önizleme günlükler için](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). Değerleri destek listesi kullanılabilir [desteklenen Ölçümler ve oluşturma yöntemleri için yeni ölçüm uyarıları](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hub"></a>Olay Hub'ı
İzleme verilerini analiz etmek için Azure'da araçlarını kullanabilmenin yanı sıra, bir dış aracı gibi bir güvenlik bilgileri ve Olay yönetimi (SIEM) ürün iletmek isteyebilirsiniz. Bu genellikle yapılır kullanarak [Azure olay hub'ı](https://docs.microsoft.com/azure/event-hubs/). 

Veri izleme farklı türleri için rehberlik alma [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [izleme verilerini kullanılabilir](monitoring-data-sources.md) azure'daki farklı kaynakları. 