---
title: Azure İzleyici tarafından toplanan izleme verilerinin | Microsoft Docs
description: İzleme verilerine Azure İzleyicisi tarafından toplanan basit ve neredeyse gerçek zamanlı senaryoları ve Gelişmiş analiz için Log Analytics'te depolanır günlükleri destekleme özelliğine sahip olan ölçümleri ayrılır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: bwren
ms.openlocfilehash: c9929149c029d15d496eac0eb530371418e1e1f2
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53323516"
---
# <a name="monitoring-data-collected-by-azure-monitor"></a>Azure İzleyici tarafından toplanan verileri izleme
[Azure İzleyici](../overview.md) yardımcı olan bir hizmeti izlemek, uygulamalarınızın ve bunların bağımlı kaynakları olduğundan. Telemetri ve diğer verileri izlenen kaynaklardan bu işleve merkezi depolamadır. Bu makalede, Azure İzleyici tarafından kullanılan bu veriler nasıl depolanır ve kapsamlı bir açıklama sağlar.

Azure İzleyici tarafından toplanan tüm verileri iki temel türlerinden birine uyan [ölçümleri](#metrics) ve [günlükleri](#logs). Zaman içinde belirli bir noktada bir sistem bazı yönlerini açıklayan bir sayısal değerler ölçümleridir. Bunlar, basit ve gerçek zamanlı senaryoları destekleme yeteneği. Farklı türlerde veri kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve günlükleri içerir. Olaylarla ve izlemelerle gibi telemetri depolanır günlükleri olarak ayrıca performans verilerini ve böylece tüm analiz için birleştirilebilir.

![Azure İzleyiciye Genel Bakış](media/data-collection/overview.png)

## <a name="metrics"></a>Ölçümler
Sayısal değerleri, belirli bir zamanda bir sistem bazı yönlerini açıklamak ölçümleridir. Bunlar, basit ve gerçek zamanlı senaryoları destekleme yeteneği. Ölçümler, değeri değiştirir olup olmadığını düzenli aralıklarla toplanır. Bunlar sık örneklenebilir ve bir uyarı ile göreceli olarak basit bir mantıksal hızla harekete çünkü uyarmak için yararlıdır. 

Örneğin, işlemci kullanımı dakikada bir sanal makineden toplayabilir veya kullanıcı sayısını her 10 dakikada uygulamanıza oturum. Bir uyarı, bunların değerleri toplanan veya tanımlı bir eşiğin bile iki değer arasındaki farkı harekete.

Azure'da ölçüm belirli öznitelikler aşağıdaki gibidir:

* Ölçüm 's tanımında aksi belirtilmediği sürece bir dakikalık sıklığında toplanmadı.
* Bir ölçüm adı ve kategori olarak davranan bir ad alanı tarafından benzersiz şekilde tanımlanır.
* 93 gün boyunca depolanır. Uzun vadeli eğilimleri belirleme için Log Analytics ölçümleri kopyalayabilirsiniz.

Her bir ölçüm değeri aşağıdaki özelliklere sahiptir:
* Değer toplandığı zaman.
* Ölçüm değeri türünü temsil eder.
* Kaynak değeri ile ilişkilidir.
* Değer.
* Bazı ölçümler, sonraki bölümde açıklandığı gibi birden çok boyutta olabilir. Özel ölçümler, en fazla 10 boyuta sahip olabilir.

### <a name="multi-dimensional-metrics"></a>Çok boyutlu ölçümleri
Bir ölçüm boyutlarını ölçüm değeri tanımlamak için ek veri taşıyan ad-değer çiftleridir. Örneğin, bir ölçüm _kullanılabilir disk alanı_ adlı bir boyutta olabilir _sürücü_ değerlerle _C:_, _D:_, hangi görüntüleme izin veya kullanılabilir disk alanı tüm sürücüler her biri için ayrı ayrı sürücü. 

Aşağıdaki örnekte adlı kuramsal bir ölçüm için iki veri kümesi gösterildiği _ağ aktarım hızı_. İlk veri kümesi herhangi bir boyutu vardır. İki boyutlu ı_p Address_ değerlerle ikinci bir veri kümesi gösterir ve _yönü_:

### <a name="network-throughput"></a>Ağ aktarım hızı

 |Zaman damgası        | Ölçüm değeri | 
   | ------------- |:-------------| 
   | 8/9/2017 8:14 | 1,331.8 KB/sn | 
   | 8/9/2017 8:15 | 1,141.4 KB/sn |
   | 8/9/2017 8:16 | 1,110.2 KB/sn |

Temel bir soru cevap ister yalnızca "my ağ aktarım hızı belirli bir zamanda neydi?" boyutlu Bu ölçüm olabilir.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ aktarım hızı + iki boyutlu ("IP" ve "Yönü")

| Zaman damgası          | Boyut "IP" | Boyut "Yönü" | Ölçüm değeri| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Gönder" =    | 646.5 KB/sn |
   | 8/9/2017 8:14 | IP "192.168.5.2" = | Yön "Al" = | 420.1 KB/sn |
   | 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Gönder" =    | 150.0 KB/sn | 
   | 8/9/2017 8:14 | IP "10.24.2.15" =  | Yön "Al" = | 115.2 KB/sn |
   | 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Gönder" =    | 515.2 KB/sn |
   | 8/9/2017 8:15 | IP "192.168.5.2" = | Yön "Al" = | 371.1 KB/sn |
   | 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Gönder" =    | 155.0 KB/sn |
   | 8/9/2017 8:15 | IP "10.24.2.15" =  | Yön "Al" = | 100.1 KB/sn |

Bu ölçüm, "ağ aktarım hızı için her bir IP adresi neydi?" ve "karşı gönderilen veri miktarını alındı?" gibi soruları yanıtlayabilirsiniz Çok boyutlu ölçümler, boyutsuz ölçümler için kıyasla ek analiz ve tanılama değer taşır.

### <a name="value-of-metrics"></a>Ölçüm değeri
Tek tek ölçüler genellikle küçük hakkında fikir, kendi sağlar. Basit eşik karşılaştırması dışında herhangi bir bağlam olmadan tek bir değer sağlarlar. Bunlar, modelleri ve eğilimlerini belirlemek için diğer ölçümleri ile birleştirildiğinde veya belirli değerleri etrafında bağlam sağlamak günlükleri ile birleştirildiğinde değerli. 

Örneğin, uygulamanız bir anda kullanıcıların belirli bir sayıda uygulama durumunu ilgili çok az söyleyebilir. Ancak kullanıcılar, aynı ölçümü birden çok değeri tarafından belirtilen bir ani düşme bir sorun olduğunu gösteriyor olabilir. Uygulama tarafından oluşturulan ve ayrı bir ölçüme göre belirtilen aşırı özel durumlar açılır neden olan bir uygulama sorunu tespit edebilecek. Uygulama bileşenlerinden hataları belirlemek için oluşturduğu olayları kök nedeni belirlemenize yardımcı olabilir.

### <a name="sources-of-metric-data"></a>Ölçüm veri kaynakları
Azure İzleyici tarafından toplanan ölçümleri üç temel kaynakları vardır. Tüm Bu ölçümler nerede bunlar birlikte kaynaklarını bağımsız olarak değerlendirilebilen ölçüm Mağazası'nda mevcuttur.

**Platform ölçümleri** Azure kaynakları tarafından oluşturulur ve bunların sistem durumu ve performans görünürlük sağlar. Her kaynak türünü oluşturur bir [farklı ölçüm kümesini](../../monitoring-and-diagnostics/monitoring-supported-metrics.md) gerekli herhangi bir yapılandırma olmadan. 

**Uygulama ölçümleri** performans sorunları tespit edin ve eğilimler, uygulamanızın nasıl kullanıldığını izlemenize yardımcı olur ve izlenen uygulamalar için Application Insights tarafından oluşturulur. Bu tür değerleri olarak içerir _sunucu yanıt süresi_ ve _tarayıcı özel durumları_.

**Özel ölçümler** otomatik olarak kullanılabilir olan ek olarak standart ölçüm tanımladığınız ölçümleridir. Özel ölçümler, tek bir kaynak, kaynak ile aynı bölgede karşı oluşturulmalıdır. Aşağıdaki yöntemleri kullanarak özel ölçümler oluşturabilirsiniz:
    - [Uygulamanızda özel ölçümler tanımlayın](../../application-insights/app-insights-api-custom-events-metrics.md) Application Insights tarafından izlenir. Bunlar, ek olarak standart'ı uygulama ölçümlerini ayarlanır.
    - Windows sanal makinelerinizdeki kullanarak özel ölçümler yayımlama [Windows Tanılama uzantısı (WAD)](../../azure-monitor/platform/diagnostics-extension-overview.md).
    - Linux sanal makinelerinizden kullanarak özel ölçümler yayımlama [InfluxData Telegraf aracı](https://www.influxdata.com/time-series-platform/telegraf/).
    - Özel ölçümler, özel ölçümler API kullanarak bir Azure hizmetinden yazın.
    
![Ölçümlerine genel bakış](media/data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>Ölçümler ile neler?
Ölçümler ile gerçekleştirebileceğiniz görevler aşağıdakileri içerir:

- Kullanım [ölçüm Gezgini](../../monitoring-and-diagnostics/monitoring-metric-charts.md) toplanan ölçümlerin analiz etmek ve bunları bir vykreslit v grafu için. Grafiklere sabitleyerek (örneğin, bir sanal makine, Web sitesi veya mantıksal uygulama) kaynak performansını izleyen bir [Azure panosuna](../../azure-portal/azure-portal-dashboards.md).
- Yapılandırma bir [ölçüm uyarısı kuralının](alerts-metric.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) ne zaman bir eşiği aştığında ölçümü.
- Kullanım [otomatik ölçeklendirme](../../monitoring-and-diagnostics/monitoring-overview-autoscale.md) artırabilir veya azaltabilirsiniz bir Eşiği aşan bir ölçüme göre kaynakları.
- Yol ölçümlerinin Log analytics'e günlük verileriyle birlikte ölçüm verilerini analiz etmek ve ölçüm değerleri 93 günden daha uzun süre saklamak için. 
- Stream için ölçümleri bir [olay hub'ı](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) kendisine yönlendirmek için [Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md) veya harici sistemlere bağlanma.
- [Arşiv](../../monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data.md) kaynağınızın denetim ya da çevrimdışı raporlamaya uyumluluk, performans veya sistem durumu geçmişi.
- Bir komut satırı veya özel bir uygulama kullanarak ölçüm değerleri erişim [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0) veya [REST API](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).



### <a name="viewing-metrics"></a>Ölçümleri görüntüleme
Azure İzleyici ölçümleri veritabanında azure'da ölçümleri toplanır. Bu, zaman serisi veritabanı 93 gün boyunca hızlı alma ve depoları ölçüm değerleri için en iyi duruma getirilmiş. Ölçümler için Log Analytics uzun süreli analiz ve eğilimler için kopyalayın.

Ölçüm verilerini yukarıda açıklandığı gibi çeşitli şekillerde kullanılır. Kullanım [ölçüm Gezgini](../../monitoring-and-diagnostics/monitoring-metric-charts.md) doğrudan ölçüm mağazanızdaki verileri analiz etmek ve zaman içinde birden çok ölçüm değerleri grafik. Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. Ölçümleri kullanarak da alabilirsiniz [Azure REST API izleme](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Ölçüm Gezgini](media/data-collection/metrics-explorer.png)





## <a name="logs"></a>Günlükler
Farklı türlerde veri kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve günlükleri içerir. Günlükler, ölçümler gibi sayısal değerleri içeren ancak genellikle ayrıntılı açıklamalar içeren metin verileri içerir. Bunlar daha fazla ölçüm arasından kendi yapısında farklılık, farklı ve düzenli aralıklarla toplanır değildir.

Ortak günlük girişi tutularak toplanan bir olay türüdür. Bunlar, bir uygulama veya hizmet tarafından oluşturulan ve genellikle kendi başlarına tüm bağlam sağlamak için yeterli bilgi içerir. Örneğin, bir olay belirli bir kaynağa oluşturulmuş veya değiştirilmiş, artan trafiğe yanıt olarak başlatılan yeni bir ana bilgisayar veya bir uygulamada bir hata algılandı olduğunu gösterebilir.

Günlükler, çeşitli kaynaklardan alınan verilerin birleştirilmesi için karmaşık bir analiz ve zaman içindeki eğilimleri belirlemek için özellikle yararlı olur. Veri biçimi farklılık gösterebileceğinden, ihtiyaç duydukları yapısını kullanarak özel günlükleri uygulamalar oluşturabilirsiniz. Ölçümler, eğilimleri belirlemek için diğer izleme verilerinin ve diğer veri analizi ile birleştirmek için günlüklerinde bile çoğaltılır.



### <a name="log-analytics"></a>Log Analytics
Azure İzleyici tarafından toplanan günlükler, çeşitli kaynaklardan telemetri ve diğer verileri toplayan Log analytics'te depolanır. Bu, zengin bir sorgu dili ve uygulamalarınızın ve kaynaklarınızın çalışmasını Öngörüler sunan bir analiz altyapısı sağlar. Diğer Azure Hizmetleri gibi [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md) Azure Yönetimi genelinde ortak bir veri platformu sağlamak amacıyla Log Analytics'te verilerini depolar.

> [!IMPORTANT]
> Ayrı bir bölümde depolanır dışında Application Insights verileri gibi diğer günlük verileri Log Analytics'te depolanır. Bu, diğer Log Analytics verilerini aynı işlevselliği destekler, ancak kullanmalısınız [Application Insights konsol](../../application-insights/app-insights-analytics.md) veya [Application Insights API](https://dev.applicationinsights.io/) bu verilere erişmek için. Kullanabileceğiniz bir [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) diğer günlük verileriyle birlikte uygulama verilerini analiz etmek için.


### <a name="sources-of-log-data"></a>Günlük verisi kaynakları
Log Analytics, çeşitli kaynaklardan hem Azure içindeki ve şirket içi kaynaklardan veri toplayabilir. Log Analytics'e yazılan veri kaynakları şunları içerir:

- [Etkinlik günlükleri](collect-activity-logs.md) Azure kaynaklarından, yapılandırmaları ve sistem durumu hakkında bilgiler içerir ve [tanılama günlükleri](../../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) işleyişlerini Öngörüler sağlayın.
- Aracılarda [Windows](../../log-analytics/log-analytics-windows-agent.md) ve [Linux](../learn/quick-collect-linux-computer.md) konuk işletim sistemi ve uygulamaları şunlara göre Log analytics'e telemetri gönderen sanal makineler [veri kaynakları](agent-data-sources.md) , siz yapılandırırsınız.
- Uygulama verileri tarafından toplanan [Application Insights](https://docs.microsoft.com/azure/application-insights/).
- Belirli bir uygulama veya hizmetten Öngörüler sağlayan veri [izleme çözümleri](../insights/solutions.md) veya kapsayıcı öngörüleri, VM Insights veya kaynak grubu Insights gibi özellikleri.
- Tarafından toplanan güvenlik verileri [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/).
- [Ölçümleri](#metrics) Azure kaynaklarından. Bu, ölçümleri 93 günden daha uzun süre saklamak için ve diğer günlük verilerinizi çözümlemenizi sağlar.
- Yazılan telemetri [Azure depolama](azure-storage-iis-table.md).
- Özel verileri kullanarak herhangi bir REST API istemcisi [HTTP veri toplayıcı API'sini](data-collector-api.md) istemci veya bir [Azure Logic App](https://docs.microsoft.com/azure/logic-apps/) iş akışı.

![Log Analytics bileşenleri](media/data-collection/logs-overview.png)




### <a name="what-can-you-do-with-logs"></a>Günlükleri ile neler?
Günlükleri ile gerçekleştirebileceğiniz görevler aşağıdakileri içerir:

- Kullanım [Log Analytics sayfa](../log-query/get-started-portal.md) Azure portalında günlük verilerini analiz sorguları yazma.  Sabitleme, tablolar veya grafikler için çizilir sonuçları bir [Azure panosuna](../../azure-portal/azure-portal-dashboards.md).
- Yapılandırma bir [günlük uyarı kuralı](alerts-log.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) zaman sorgunun sonuçlarını eşleşen belirli bir sonuç.
- Log Analytics kullanarak verileri temel alan bir iş akışı derleme [Logic Apps](~/articles/logic-apps/index.yml).
- Sorgu sonuçlarını dışarı aktarma [Power BI](powerbi.md) farklı görselleştirme kullanın ve Azure dışındaki kullanıcılarla paylaşmak için.
- Bir komut satırı veya özel bir uygulama kullanarak ölçüm değerleri erişim [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/?view=azurermps-6.8.1) veya [REST API](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Günlük verilerini görüntüleme
Tüm verileri Log Analytics kullanarak alınır bir [günlük sorgusu](../log-query/log-query-overview.md) belirli bir veri kümesini belirtir. Sorguları kullanarak yazılır [Log Analytics sorgu diline](../log-query/get-started-queries.md) hızlı bir şekilde almak, birleştirmek ve toplanan verileri çözümlemek için zengin bir sorgu dili olan. Kullanım [Log Analytics sayfa](../log-query/portals.md) doğrudan analiz etmek için Azure portalında, ölçüm verileri depolamak ve zaman içinde birden çok ölçüm değerleri grafik. Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. Ölçümleri kullanarak da alabilirsiniz [Azure REST API izleme](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Günlükler](media/data-collection/logs.png)

## <a name="convert-monitoring-data"></a>İzleme verileri dönüştürme

### <a name="metrics-to-logs"></a>Günlükler için ölçümleri
Zengin sorgu dilini kullanarak diğer veri türlerine sahip karmaşık bir analiz gerçekleştirmek için Log Analytics ölçümleri kopyalayabilirsiniz. Ayrıca, zaman içinde eğilim gerçekleştirmenize olanak tanıyan ölçümleri daha uzun süre günlük verileri koruyabilirsiniz. Zaman ölçümleri veya diğer herhangi bir performans verilerini verileri bir günlük olarak davranan Log analytics'te depolanır. Neredeyse gerçek zamanlı analiz ve eğilim ve analiz diğer verilerle için günlükleri kullanırken uyarı desteklemek için ölçümleri kullanın.

Azure kaynaklardan ölçümleri toplamaya yönelik rehberlik alabilirsiniz [toplamak Azure hizmeti günlükleri ve Log analytics'teki kullanım ölçümlerini](collect-azure-metrics-logs.md). Azure PaaS kaynakları'ndan kaynakları ölçümleri toplamaya ilişkin yönergeler almak [Log Analytics ile Azure PaaS kaynak ölçümleri toplamayı yapılandırmak](collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Ölçümler için günlükleri
Daha düşük gecikme süresi ve daha düşük bir maliyetle uyarılar oluşturmak için yukarıda açıklandığı gibi günlükleri, daha hızlı yanıt ölçümleridir. Log Analytics önemli ölçüde ölçümler için uygun olabilir, ancak Azure ölçümleri veritabanında saklanmaz sayısal veri toplar.  Aracılar ve yönetim çözümlerinden toplanan performans verilerini buna yaygın bir örnektir. Bu değerlerden bazıları, ölçüm Gezgini ile analizi ve Uyarılar için kullanılabilir olduğu ölçüm veritabanına kopyalanabilir.

Bu özellik açıklaması kullanılabilir [Azure İzleyici günlükler için ölçüm uyarıları oluşturma](../../monitoring-and-diagnostics/monitoring-metric-alerts-logs.md). Değerleri destek listesi kullanılabilir [Azure İzleyici ile desteklenen ölçümler](../../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Stream veri harici sistemlere bağlanma
İzleme verilerini analiz etmek için Azure'da araçlarını kullanabilmenin yanı sıra güvenlik bilgileri ve Olay yönetimi (SIEM) ürün gibi bir dış araç iletmek için bir gereksinim olabilir. Bu iletme genellikle doğrudan izlenen kaynakları üzerinden yapılır [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

Veri izleme farklı türleri için rehberlik alma [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [izleme verilerini kullanılabilir](data-sources.md) azure'daki farklı kaynakları.
