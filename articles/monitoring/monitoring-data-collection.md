---
title: Azure'da veri toplama izleme | Microsoft Docs
description: Uygulamaları ve Hizmetleri Azure ve analiz için kullanılan araçları toplanan izleme verilerinin genel bakış.
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
ms.openlocfilehash: 35580d71aa2592fa94f42cfdbad3c192acc303c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363994"
---
# <a name="collect-monitoring-data-in-azure"></a>Azure izleme verilerini toplama
Bu makalede, uygulamalardan ve Azure hizmetlerinde toplanan izleme verilerinin genel bir bakış sağlar. Ayrıca, verileri analiz etmek için kullanabileceğiniz araçları açıklar. 

## <a name="types-of-monitoring-data"></a>İzleme türleri
Tüm izleme verilerini birinin iki temel türler, Ölçümler ve günlükleri içine sığar. Her tür farklı özelliklere sahiptir ve belirli senaryolar için idealdir.

### <a name="metrics"></a>Ölçümler
Sayısal değerleri, belirli bir zamanda bir sistem bazı yönlerini açıklamak ölçümleridir. Bunlar:

* Değerin kendisi de dahil olmak üzere benzersiz veri.
* Değer toplandığı zaman.
* Değeri temsil eden ölçü türü.
* Değer ilişkili olduğu kaynak. 

Ölçümler, değeri değiştirir olup olmadığını düzenli aralıklarla toplanır. Örneğin, işlemci kullanımı dakikada bir sanal makineden toplayabilir veya kullanıcı sayısını her 10 dakikada uygulamanıza oturum.

Ölçümler, basit ve gerçek zamanlı senaryoları destekleme yeteneği. Ölçümler, sık olarak örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete çünkü uyarmak için yararlı oldukları. Örneğin, bir ölçüm eşiği değeri aştığında bir uyarı harekete. Ya da iki ölçüm arasındaki fark, belirli bir değere ulaştığında bir uyarı yangın.

Tek tek ölçüler genellikle küçük hakkında fikir, kendi sağlar. Basit eşik karşılaştırması dışında herhangi bir bağlam olmadan tek bir değer sağlarlar. Bunlar, modelleri ve eğilimlerini belirlemek için diğer ölçümleri ile birleştirildiğinde veya belirli değerleri etrafında bağlam sağlamak günlükleri ile birleştirildiğinde değerli. 

Örneğin, uygulamanız bir anda kullanıcıların belirli bir sayıda uygulama durumunu ilgili çok az söyleyebilir. Ancak kullanıcılar, aynı ölçümü birden çok değeri tarafından belirtilen bir ani düşme bir sorun olduğunu gösteriyor olabilir. Uygulama tarafından oluşturulan ve ayrı bir ölçüme göre belirtilen aşırı özel durumlar açılır neden olan bir uygulama sorunu tespit edebilecek. Uygulama bileşenlerinden hataları belirlemek için oluşturduğu olayları kök nedeni belirlemenize yardımcı olabilir.

Günlüklerine göre uyarı ölçümlerine bağlı uyarılar olarak duyarlı değildir ancak daha karmaşık mantık dahil edebilirsiniz. Birden çok kaynaktan veri üzerinde karmaşık bir analiz gerçekleştirir herhangi bir sorgunun sonuçlarına dayalı bir uyarı oluşturabilirsiniz.

### <a name="logs"></a>Günlükler
Farklı türlerde veri kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve günlükleri içerir. Günlükleri ölçümler gibi sayısal değerleri içeren ancak genellikle ayrıntılı açıklamalar içeren metin verileri içerir. Bunlar daha fazla ölçüm arasından kendi yapısında farklılık, farklı ve düzenli aralıklarla toplanır değildir.

Ortak tür günlük girdisini bir olaydır. Olayları tutularak toplanır. Bunlar, bir uygulama veya hizmet tarafından oluşturulan ve genellikle kendi başlarına tüm bağlam sağlamak için yeterli bilgi içerir. Örneğin, bir olay belirli bir kaynağa oluşturulmuş veya değiştirilmiş, artan trafiğe yanıt olarak başlatılan yeni bir ana bilgisayar veya bir uygulamada bir hata algılandı olduğunu gösterebilir.

Günlükleri, zaman içinde eğilim ve kaynakları, karmaşık analiz için çeşitli verilerin birleştirilmesi için özellikle yararlıdır. Veri biçimi farklılık gösterebileceğinden, ihtiyaç duydukları yapısını kullanarak özel günlükleri uygulamalar oluşturabilirsiniz. Ölçümler, eğilimleri belirlemek için diğer izleme verilerinin ve diğer veri analizi ile birleştirmek için günlüklerinde bile çoğaltılabilir.


## <a name="monitoring-tools-in-azure"></a>Azure'da izleme araçları
İzleme verileri azure'da toplanır ve aşağıdaki kaynaklar aracılığıyla analiz edilir.

### <a name="azure-monitor"></a>Azure İzleyici
Azure İzleyici ile Azure kaynaklarını ve uygulamaların ölçümleri toplanır. Ölçüm verilerini Azure kaynakları için Azure portalında sayfalarıyla tümleşiktir. Sanal makineler için seçilen makine için CPU ve ağ kullanımını olarak gibi ölçüm grafikleri görünür. 

Kullanarak verileri çözümleyebilirsiniz [ölçüm Gezgini](../monitoring-and-diagnostics/monitoring-metric-charts.md), zaman içinde değerleri birden çok ölçüm grafikleri. Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. Ölçümleri kullanarak da alabilirsiniz [Azure REST API izleme](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Çeşitli Azure kaynaklarını toplamak ölçüm verileri hakkında daha fazla bilgi için bkz. [azure'da veri izleme kaynakları](monitoring-data-sources.md). 

![Ölçüm Gezgini](media/monitoring-data-collection/metrics-explorer.png)


### <a name="activity-log"></a>Etkinlik Günlüğü 
[Azure etkinlik günlüğü](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) günlükleri yapılandırması ve Azure hizmetlerinin durumunu depolar. Azure portalında Bu günlükleri görüntülemek üzere etkinlik günlüğü Gezgini'ni kullanabilirsiniz, ancak genellikle oldukları [Azure Log Analytics'e kopyalanan](../log-analytics/log-analytics-activity.md) diğer günlük verilerinizi Analiz edilecek.

Belirli ölçütlerle eşleşmesi için filtre Etkinlik günlüğünü görüntülemek üzere etkinlik günlüğü Gezgini'ni kullanabilirsiniz. En fazla kaynak de bir **etkinlik günlüğü** Azure portalında kendi menüsündeki seçeneği. Etkinlik günlüğü bu kaynak için filtre Gezgini görüntülenir. Etkinlik günlüklerini kullanarak da alabilirsiniz [Azure izleme REST API](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Etkinlik günlüğü Gezgini](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics, Azure yönetimi için ortak bir veri platformu sağlar. Bu depolama ve Azure günlük analizi için kullanılan birincil bir hizmettir. Aracılarda Azure kaynaklarını sanal makineleri ve yönetim çözümleri gibi kaynakları, çeşitli veri toplar. Ölçüm ve izleme verilerinin tam bir merkezi depo oluşturmak için etkinlik günlüğüne dahil olmak üzere diğer kaynaklardan veri kopyalayabilirsiniz.

Log Analytics, toplanan verileri analiz etmek için zengin bir sorgu dilini sahiptir. Kullanabileceğiniz [günlük araması portalları](../log-analytics/log-analytics-log-search-portals.md) etkileşimli olarak yazılması ve sorguları test etme ve sonuçları analiz etme. Ayrıca [görünümleri oluşturma](../log-analytics/log-analytics-view-designer.md) sonuçları günlük aramalarınızı görselleştirebilir veya bir Azure panosunu için doğrudan bir sorgunun sonuçlarını yapıştırın.  

Yönetim çözümleri günlük aramaları ve görünümleri Log Analytics'te, topladığınız verileri çözümlemek için içerir. Azure Application Insights gibi diğer hizmetlerle veri Log Analytics'te depolar ve analiz için ek araçlar sağlar.  

![Günlükler](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights, farklı platformlarda üzerinde yüklü web uygulamaları için telemetri toplar. Azure İzleyici ve Log Analytics verilerini depolar. Ve analiz etme ve verilerini görselleştirme için kapsamlı birtakım araçlar sağlar. Bu özellikler, uyarılar, günlük aramaları ve diğer izleme için kullanmayı panolar gibi hizmetleri ortak bir dizi kullanmanıza olanak sağlar.


![Application Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Hizmet Eşlemesi
Hizmet eşlemesi, sanal makineler işlemleri ve bağımlılıkları görsel bir gösterimini sağlar. Diğer yönetim verilerinizi analiz edebilmesi çoğu bu veriler, Log Analytics'te depolar. Hizmet eşlemesi Konsolu da analiz edilen sanal makine bağlamında sunmak için Log Analytics veri alır.

![Hizmet Eşlemesi](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>İzleme verilerini aktarma

### <a name="metrics-to-logs"></a>Günlükler için ölçümleri
Ölçümler, zengin bir sorgu dilini kullanarak diğer veri türlerine sahip karmaşık bir analiz gerçekleştirmek için Log analytics'te çoğaltabilirsiniz. Ayrıca, zaman içinde eğilim gerçekleştirmenize olanak tanıyan ölçümleri daha uzun süre günlük verileri koruyabilirsiniz. Zaman ölçümleri veya diğer herhangi bir performans verilerini verileri bir günlük olarak davranan Log analytics'te depolanır. Neredeyse gerçek zamanlı analiz ve eğilim ve analiz diğer verilerle için günlükleri kullanırken uyarı desteklemek için ölçümleri kullanın.

Azure kaynaklardan ölçümleri toplamaya yönelik rehberlik alabilirsiniz [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](../log-analytics/log-analytics-azure-storage.md). Azure PaaS kaynakları'ndan kaynakları ölçümleri toplamaya ilişkin yönergeler almak [Log Analytics ile Azure PaaS kaynak ölçümleri toplamayı yapılandırmak](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Ölçümler için günlükleri
Bu nedenle daha düşük gecikme süresi ve daha düşük bir maliyetle uyarılar oluşturabilirsiniz daha önce açıklandığı gibi günlükleri, daha hızlı yanıt ölçümleridir. Log Analytics önemli ölçüde ölçümler için uygun olabilir, ancak Azure İzleyici'de depolanan değil sayısal veri toplar. 

Aracılar ve yönetim çözümlerinden toplanan performans verilerini buna yaygın bir örnektir. Bu değerlerden bazıları, ölçüm Gezgini ile analizi ve Uyarılar için kullanılabilir olduğu Azure İzleyici halinde kopyalanabilir.

Bu özellik açıklaması kullanılabilir [daha hızlı ölçüm uyarıları artık sınırlı genel Önizleme günlükler için](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). Değerleri destek listesi kullanılabilir [desteklenen Ölçümler ve oluşturma yöntemleri için yeni ölçüm uyarıları](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hubs"></a>Event Hubs
İzleme verilerini analiz etmek için Azure'da araçlarını kullanabilmenin yanı sıra güvenlik bilgileri ve Olay yönetimi (SIEM) ürün gibi bir dış araç iletmek isteyebilirsiniz. Genellikle bu iletme aracılığıyla gerçekleştirilir [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

Veri izleme farklı türleri için rehberlik alma [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [izleme verilerini kullanılabilir](monitoring-data-sources.md) azure'daki farklı kaynakları. 