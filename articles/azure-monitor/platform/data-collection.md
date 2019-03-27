---
title: Azure İzleyici tarafından toplanan izleme verilerinin | Microsoft Docs
description: İzleme verilerine Azure İzleyicisi tarafından toplanan, basit ve gerçek zamanlı senaryoları destekleme özelliğine sahip olan ölçümleri ve Gelişmiş analiz için kullanılan günlükleri ayrılır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: azure-monitor
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/05/2018
ms.author: bwren
ms.openlocfilehash: e6d953841e5c22c21640f874ecad942f8db76ad1
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448892"
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
* 93 gün boyunca depolanır. Uzun vadeli eğilimleri belirlemek için günlüklere ölçümleri kopyalayabilirsiniz.

Her bir ölçüm değeri aşağıdaki özelliklere sahiptir:
* Değer toplandığı zaman.
* Ölçüm değeri türünü temsil eder.
* Kaynak değeri ile ilişkilidir.
* Değer.
* Bazı ölçümler, sonraki bölümde açıklandığı gibi birden çok boyutta olabilir. Özel ölçümler, en fazla 10 boyuta sahip olabilir.

### <a name="multi-dimensional-metrics"></a>Çok boyutlu ölçümleri
Bir ölçüm boyutlarını ölçüm değeri tanımlamak için ek veri taşıyan ad-değer çiftleridir. Örneğin, bir ölçüm _kullanılabilir disk alanı_ adlı bir boyutta olabilir _sürücü_ değerlerle _C:_, _D:_, hangi görüntüleme izin veya kullanılabilir disk alanı tüm sürücüler her biri için ayrı ayrı sürücü.

Aşağıdaki örnekte adlı kuramsal bir ölçüm için iki veri kümesi gösterildiği _ağ aktarım hızı_. İlk veri kümesi herhangi bir boyutu vardır. İki boyutlu değerlerle ikinci bir veri kümesi gösterir _IP adresi_ ve _yönü_:

### <a name="network-throughput"></a>Ağ aktarım hızı

| Zaman damgası     | Ölçüm değeri |
| ------------- |:-------------|
| 8/9/2017 8:14 | 1,331.8 KB/sn |
| 8/9/2017 8:15 | 1,141.4 KB/sn |
| 8/9/2017 8:16 | 1,110.2 KB/sn |

Temel bir soru cevap ister yalnızca "my ağ aktarım hızı belirli bir zamanda neydi?" boyutlu Bu ölçüm olabilir.

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>Ağ aktarım hızı + iki boyutlu ("IP" ve "Yönü")

| Zaman damgası     | Boyut "IP"   | Boyut "Yönü" | Ölçüm değeri|
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

**Platform ölçümleri** Azure kaynakları tarafından oluşturulur ve bunların sistem durumu ve performans görünürlük sağlar. Her kaynak türünü oluşturur bir [farklı ölçüm kümesini](metrics-supported.md) gerekli herhangi bir yapılandırma olmadan. 

**Uygulama ölçümleri** performans sorunları tespit edin ve eğilimler, uygulamanızın nasıl kullanıldığını izlemenize yardımcı olur ve izlenen uygulamalar için Application Insights tarafından oluşturulur. Bu tür değerleri olarak içerir _sunucu yanıt süresi_ ve _tarayıcı özel durumları_.

**Özel ölçümler** otomatik olarak kullanılabilir olan ek olarak standart ölçüm tanımladığınız ölçümleridir. Özel ölçümler, tek bir kaynak, kaynak ile aynı bölgede karşı oluşturulmalıdır. Aşağıdaki yöntemleri kullanarak özel ölçümler oluşturabilirsiniz:
- [Uygulamanızda özel ölçümler tanımlayın](../../azure-monitor/app/api-custom-events-metrics.md) Application Insights tarafından izlenir. Bunlar, ek olarak standart'ı uygulama ölçümlerini ayarlanır.
- Windows sanal makinelerinizdeki kullanarak özel ölçümler yayımlama [Windows Tanılama uzantısı (WAD)](../../azure-monitor/platform/diagnostics-extension-overview.md).
- Linux sanal makinelerinizden kullanarak özel ölçümler yayımlama [InfluxData Telegraf aracı](https://www.influxdata.com/time-series-platform/telegraf/).
- Özel ölçümler, özel ölçümler API kullanarak bir Azure hizmetinden yazın.

![Ölçümlerine genel bakış](media/data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>Ölçümler ile neler?
Ölçümler ile gerçekleştirebileceğiniz görevler aşağıdakileri içerir:

- Kullanım [ölçümleri analiz](metrics-charts.md) toplanan ölçümlerin analiz etmek ve bunları bir vykreslit v grafu için. Grafiklere sabitleyerek (örneğin, bir sanal makine, Web sitesi veya mantıksal uygulama) kaynak performansını izleyen bir [Azure panosuna](../../azure-portal/azure-portal-dashboards.md).
- Yapılandırma bir [ölçüm uyarısı kuralının](alerts-metric.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) ne zaman bir eşiği aştığında ölçümü.
- Kullanım [otomatik ölçeklendirme](autoscale-overview.md) artırabilir veya azaltabilirsiniz bir Eşiği aşan bir ölçüme göre kaynakları.
- Yol ölçümlerinin günlüklerine günlük verileriyle birlikte ölçüm verilerini analiz etmek ve ölçüm değerleri 93 günden daha uzun süre saklamak için. 
- Stream için ölçümleri bir [olay hub'ı](stream-monitoring-data-event-hubs.md) kendisine yönlendirmek için [Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md) veya harici sistemlere bağlanma.
- [Arşiv](../../azure-monitor/learn/tutorial-archive-data.md) kaynağınızın denetim ya da çevrimdışı raporlamaya uyumluluk, performans veya sistem durumu geçmişi.
- Bir komut satırı veya özel bir uygulama kullanarak ölçüm değerleri erişim [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.insights/) veya [REST API](rest-api-walkthrough.md).



### <a name="viewing-metrics"></a>Ölçümleri görüntüleme
Azure İzleyici'de ölçümleri bir zaman serisi içinde depolanan veritabanı 93 gün boyunca hızlı alma ve depoları ölçüm değerleri için en iyi duruma getirilmiş. Uzun süreli analiz ve eğilimler için günlükleri ölçümleri kopyalayın.

Ölçüm verilerini yukarıda açıklandığı gibi çeşitli şekillerde kullanılır. Kullanım [ölçümleri analiz](metrics-charts.md) doğrudan ölçüm mağazanızdaki verileri analiz etmek ve zaman içinde birden çok ölçüm değerleri grafik. Etkileşimli olarak grafikleri görüntülemek veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. Ölçümleri kullanarak da alabilirsiniz [Azure REST API izleme](rest-api-walkthrough.md).

![Ölçümleri analiz](media/data-collection/metrics-explorer.png)

## <a name="logs"></a>Günlükler

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Farklı türlerde veri kayıtlarını her türü için farklı özellik kümeleri ile düzenlenir ve günlükleri içerir. Günlükler, ölçümler gibi sayısal değerleri içeren ancak genellikle ayrıntılı açıklamalar içeren metin verileri içerir. Bunlar daha fazla ölçüm arasından kendi yapısında farklılık, farklı ve düzenli aralıklarla toplanır değildir.

Ortak günlük girişi tutularak toplanan bir olay türüdür. Bunlar, bir uygulama veya hizmet tarafından oluşturulan ve genellikle kendi başlarına tüm bağlam sağlamak için yeterli bilgi içerir. Örneğin, bir olay belirli bir kaynağa oluşturulmuş veya değiştirilmiş, artan trafiğe yanıt olarak başlatılan yeni bir ana bilgisayar veya bir uygulamada bir hata algılandı olduğunu gösterebilir.

Günlükler, çeşitli kaynaklardan alınan verilerin birleştirilmesi için karmaşık bir analiz ve zaman içindeki eğilimleri belirlemek için özellikle yararlı olur. Veri biçimi farklılık gösterebileceğinden, ihtiyaç duydukları yapısını kullanarak özel günlükleri uygulamalar oluşturabilirsiniz. Ölçümler, eğilimleri belirlemek için diğer izleme verilerinin ve diğer veri analizi ile birleştirmek için günlüklerinde bile çoğaltılır.



### <a name="sources-of-log-data"></a>Günlük verisi kaynakları
Azure İzleyici, çeşitli kaynaklardan hem de azure'daki ve şirket içi kaynaklardan gelen günlük verilerini toplayabilir. Günlük veri kaynakları şunları içerir:

- [Etkinlik günlükleri](collect-activity-logs.md) Azure kaynaklarından, yapılandırmaları ve sistem durumu hakkında bilgiler içerir ve [tanılama günlükleri](diagnostic-logs-stream-log-store.md) işleyişlerini Öngörüler sağlayın.
- Aracılarda [Windows](agent-windows.md) ve [Linux](../learn/quick-collect-linux-computer.md) konuk işletim sistemi ve uygulamaları için Azure İzleyici'ayarına göre telemetri gönderen sanal makineler [veri kaynakları](data-sources.md) , siz yapılandırırsınız.
- Uygulama verileri tarafından toplanan [Application Insights](https://docs.microsoft.com/azure/application-insights/).
- Belirli bir uygulama veya hizmetten Öngörüler sağlayan veri [izleme çözümleri](../insights/solutions.md) veya kapsayıcı öngörüleri, VM Insights veya kaynak grubu Insights gibi özellikleri.
- Tarafından toplanan güvenlik verileri [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/).
- [Ölçümleri](#metrics) Azure kaynaklarından. Bu, ölçümleri 93 günden daha uzun süre saklamak için ve diğer günlük verilerinizi çözümlemenizi sağlar.
- Yazılan telemetri [Azure depolama](azure-storage-iis-table.md).
- Özel verileri kullanarak herhangi bir REST API istemcisi [HTTP veri toplayıcı API'sini](data-collector-api.md) istemci veya bir [Azure Logic App](https://docs.microsoft.com/azure/logic-apps/) iş akışı.

![Günlükleri genel bakış](media/data-collection/logs-overview.png)

### <a name="what-can-you-do-with-logs"></a>Günlükleri ile neler?
Günlükleri ile gerçekleştirebileceğiniz görevler aşağıdakileri içerir:

- Kullanım [Log Analytics](../log-query/get-started-portal.md) Azure portalında günlük verilerini analiz sorguları yazma. Sabitleme, tablolar veya grafikler için çizilir sonuçları bir [Azure panosuna](../../azure-portal/azure-portal-dashboards.md).
- Yapılandırma bir [günlük uyarı kuralı](alerts-log.md) bildirim gönderen veya alan [eylemi otomatik](action-groups.md) zaman sorgunun sonuçlarını eşleşen belirli bir sonuç.
- Günlük veri kullanımına dayalı bir iş akışı derleme [Logic Apps](~/articles/logic-apps/index.yml).
- Sorgu sonuçlarını dışarı aktarma [Power BI](powerbi.md) farklı görselleştirme kullanın ve Azure dışındaki kullanıcılarla paylaşmak için.
- Bir komut satırı veya özel bir uygulama kullanarak ölçüm değerleri erişim [PowerShell cmdlet'leri](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/) veya [REST API](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Günlük verilerini görüntüleme
Azure İzleyici'de tüm günlük verilerini kullanarak alınır bir [günlük sorgusu](../log-query/log-query-overview.md) ile yazılmış [Kusto sorgu dili](../log-query/get-started-queries.md), hızlı bir şekilde almak, birleştirmek ve toplanan verileri çözümlemek yapmanıza olanak tanıyan. Kullanım [Log Analytics](../log-query/portals.md) yazma ve sorgular Azure Portalı'nda test etmek için. İş sonuçları ile etkileşimli olarak veya bunları diğer görselleştirmeler ile bunları görüntülemek için panoya sabitleyin. Günlükleri kullanarak da alabilirsiniz [Azure REST API izleme](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

> [!IMPORTANT]
> Application Insights verilerini diğer günlük verilerini Azure İzleyici'de ayrı bir bölümden depolanır. Bu, diğer günlük verilerini aynı işlevselliği destekler, ancak kullanmalısınız [Application Insights konsol](../app/analytics.md) veya [Application Insights API](https://dev.applicationinsights.io/) bu verilere erişmek için. Kullanabileceğiniz bir [kaynaklar arası sorgu](../log-query/cross-workspace-query.md) diğer günlük verileriyle birlikte uygulama verilerini analiz etmek için.

![Günlükler](media/data-collection/logs.png)

## <a name="convert-monitoring-data"></a>İzleme verileri dönüştürme

### <a name="metrics-to-logs"></a>Günlükler için ölçümleri
Azure İzleyici'nın zengin bir sorgu dilini kullanarak diğer veri türleri ile karmaşık bir analiz gerçekleştirmek için günlükleri ölçümleri kopyalayabilirsiniz. Ayrıca, zaman içinde eğilim gerçekleştirmenize olanak tanıyan ölçümleri daha uzun süre günlük verileri koruyabilirsiniz. Neredeyse gerçek zamanlı analiz ve eğilim ve analiz diğer verilerle için günlükleri kullanırken uyarı desteklemek için ölçümleri kullanın.

Azure kaynaklardan ölçümleri toplamaya yönelik rehberlik alabilirsiniz [toplamak Azure hizmeti günlükleri ve ölçümleri kullanılmak üzere Azure İzleyici](collect-azure-metrics-logs.md). Azure PaaS kaynakları'ndan kaynakları ölçümleri toplamaya ilişkin yönergeler almak [Azure İzleyici ile Azure PaaS kaynak ölçümleri toplamayı yapılandırmak](collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Ölçümler için günlükleri
Daha düşük gecikme süresi ve daha düşük bir maliyetle uyarılar oluşturmak için yukarıda açıklandığı gibi günlükleri, daha hızlı yanıt ölçümleridir. Sayısal veri önemli ölçüde, ölçümler için uygun olabilir ancak ölçümleriniz Azure İzleyici'de depolanan değil günlükleri olarak depolanır. Aracılar ve yönetim çözümlerinden toplanan performans verilerini buna yaygın bir örnektir. Bu değerlerden bazıları burada ölçüm Gezgini ile analizi ve Uyarılar için kullanılabilir ölçümleri, kopyalanabileceği.

Bu özellik açıklaması kullanılabilir [Azure İzleyici günlükler için ölçüm uyarıları oluşturma](alerts-metric-logs.md). Değerleri destek listesi kullanılabilir [Azure İzleyici ile desteklenen ölçümler](metrics-supported.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Stream veri harici sistemlere bağlanma
İzleme verilerini analiz etmek için Azure'da araçlarını kullanabilmenin yanı sıra güvenlik bilgileri ve Olay yönetimi (SIEM) ürün gibi bir dış araç iletmek için bir gereksinim olabilir. Bu iletme genellikle doğrudan izlenen kaynakları üzerinden yapılır [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/).

Veri izleme farklı türleri için rehberlik alma [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [izleme verilerini kullanılabilir](data-sources.md) azure'daki farklı kaynakları.
