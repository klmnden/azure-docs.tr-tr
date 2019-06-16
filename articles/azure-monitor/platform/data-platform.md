---
title: Azure İzleyici, veri platformu | Microsoft Docs
description: İzleme verilerine Azure İzleyicisi tarafından toplanan, basit ve gerçek zamanlı senaryoları destekleme özelliğine sahip olan ölçümleri ve Gelişmiş analiz için kullanılan günlükleri ayrılır.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 8883c1e7f2874e1e2e61b8eca122f2ec294c7849
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60808938"
---
# <a name="azure-monitor-data-platform"></a>Azure İzleyici, veri platformu

Günümüzün karmaşık bilgi işlem ortamlarında hem buluttaki hem de şirket içi hizmetleri kullanan dağıtılmış uygulamaları çalıştıran observability etkinleştirme, her katman ve dağıtılmış bir sistemin her bileşen işletimsel veri toplanmasını gerektirir. Bu veriler üzerinde ayrıntılı Öngörüler gerçekleştirmek ve cam ile kuruluşunuzdaki çok sayıda proje katılımcıları desteklemek için farklı perspektiflerini tek bir bölme gelişmişe olmanız gerekir.

[Azure İzleyici](../overview.md) toplar ve çeşitli kaynakları burada kullanılabileceği analiz, Görselleştirme ve uyarı amacıyla ortak bir veri platformu ile verileri toplar. Bu, ayrıntılı Öngörüler izlenen tüm kaynaklarınız genelinde ve verilerle bile diğer hizmetlerden verilerine Azure İzleyicisi'nde depolama sağlayan, birden çok kaynaktan veri üzerinde tutarlı bir deneyim sağlar.


![Azure İzleyiciye Genel Bakış](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Azure İzleyicisi'nde observability verileri
Ölçümler, günlükler ve dağıtılmış izlemelerini sık için observability üç yapı taşları adlandırılır. Bunlar, bir izleme aracı toplamak ve izlenen bir sistemin yeterli observability sağlamak için analiz verileri farklı türde olur. Observability, verileri birden çok yapı taşları arasında bağlantı kurarak ve tüm izlenen kaynakları kümesi arasında verileri toplama tarafından gerçekleştirilebilir. Azure İzleyici birlikte birden çok kaynaktan veri depoladığından, veriler ilişkili olabilir ve Araçlar ortak bir dizi kullanarak Analiz. Ayrıca birden fazla Azure abonelikleri ve diğer hizmetlere yönelik verileri barındıran ek kiracılar, veri ilişkilendirir.

Azure kaynaklarını izleme önemli ölçüde oluşturur. Azure İzleyici ölçümleri veya günlükleri bir platform ya da izleme verilerini diğer kaynaklardan yanı sıra bu verileri birleştirir. Her izleme belirli senaryolar için optimize edilmiştir ve Azure İzleyici'de her farklı özellikleri destekler. Veri analizi, görsel öğeler veya uyarı gibi özellikleri en verimli ve hesaplı bir şekilde gerekli senaryonuz uygulayabilmesi farkları gerektirir. Insights gibi Azure İzleyici'de [Application Insights](../app/app-insights-overview.md) veya [VM'ler için Azure İzleyici](../insights/vminsights-overview.md) anlamak zorunda kalmadan belirli izleme senaryoları üzerinde odaklanmanıza olanak tanıyan çözümleme araçları iki veri türleri arasındaki farklar. 


### <a name="metrics"></a>Ölçümler
[Ölçümleri](data-platform-metrics.md) zaman içinde belirli bir noktada bir sistem bazı yönlerini açıklayan bir sayısal değerler. Bunlar düzenli aralıklarla toplanır ve bir zaman damgası, bir ad, değer ve bir veya daha fazla tanımlama etiketi ile tanımlanır. Diğer ölçümlere karşılaştırıldığında ve zaman içinde eğilimler için analiz algoritmaları, çeşitli ölçümleri toplanabilir. 

Azure İzleyicisi'nde ölçümler zaman damgası veri çözümlemesi için iyileştirilmiş bir zaman serisi veritabanına depolanır. Bu özellikle uyarı vermek için uygun ve hızlı ölçümler sağlar sorunları algılama. Sisteminizi performansıyla size ancak genelde sorunların kök nedenini belirlemek için günlükleri ile birleştirilmesi gerekir.

Azure portalı ile etkileşimli analiz için kullanılabilir ölçümleri [ölçüm Gezgini](../app/metrics-explorer.md). İçin eklenebilir bir [Azure panosuna](../learn/tutorial-app-dashboards.md) diğer verilerle birlikte Görselleştirme ve neredeyse gerçek zamanlı için kullanılan [uyarı](alerts-metric.md).

Veri kaynakları dahil olmak üzere Azure İzleyici ölçümleri hakkında daha fazla bilgiyi [Azure İzleyicisi'nde ölçümler](data-platform-metrics.md).

### <a name="logs"></a>Günlükler
[Günlükleri](data-platform-logs.md) sistem içinde gerçekleşen olaylardır. Bunlar farklı türlerde veri içerebilir ve yapılandırılmış veya serbest biçimli metin bir zaman damgasına sahip. Bunlar, günlük girişlerini ortamında olaylar oluşturmak ve bir sistem ağır yük altında daha fazla günlük birimi genellikle oluşturacak tutularak oluşturulabilir.

Azure İzleyici günlüklerine temel alan bir Log Analytics çalışma alanında depolanır [Azure Veri Gezgini](/azure/data-explorer/) güçlü analiz altyapısı sağlar ve [zengin sorgu dili](/azure/kusto/query/). Günlükleri genellikle tanımlanmakta sorunun tam bağlam sağlamak için yeterli bilgi sağlar ve sorunların kök durumu belirlemek için değerlidir.

> [!NOTE]
> Azure İzleyici günlüklerine ve azure'da günlük veri kaynakları arasındaki farkı anlamak önemlidir. Örneğin, azure'da abonelik düzeyindeki olayların yazılan bir [etkinlik günlüğü](activity-logs-overview.md) , Azure İzleyici Menüsü'nden görüntüleyebilirsiniz. En fazla kaynak için kullanım bilgileri yazacak bir [tanılama günlüğü](diagnostic-logs-overview.md) , farklı konumlara iletebilir. Azure İzleyici günlüklerine etkinlik günlükleri ve diğer izleme verilerinin tüm kaynak kümesini üzerinde ayrıntılı analiz sağlamak üzere birlikte tanılama günlükleri toplayan bir günlük veri platformudur.


 Çalışabileceğiniz [oturum sorguları](../log-query/log-query-overview.md) ile etkileşimli olarak [Log Analytics](../log-query/portals.md) Azure portalında veya sonuçları Ekle bir [Azure panosuna](../learn/tutorial-app-dashboards.md) birlikte görselleştirme için diğer veri. Ayrıca oluşturabilirsiniz [günlük uyarıları](alerts-log.md) , zamanlama sorgu sonuçlarına dayalı bir uyarı tetiklemek.

Azure İzleyici veri kaynakları dahil olmak üzere günlükleri hakkında daha fazla bilgiyi [Azure İzleyici'de oturum](data-platform-logs.md).

### <a name="distributed-traces"></a>Dağıtılmış izlemeleri
İzlemeleri bir kullanıcı isteği aracılığıyla dağıtılmış bir sistemde izleyin ilgili olaylar dizisi ' dir. Farklı işlem performansını ve uygulama kodu davranışını belirlemek için kullanılabilir. Günlükler genellikle dağıtılmış bir sistemin tek tek bileşenler tarafından oluşturulur, ancak bir izleme işlemi ve uygulamanızın performansını tüm bileşenler kümesi arasında ölçer.

Azure İzleyici'de dağıtılmış izleme ile etkin [Application Insights SDK'sı](../app/distributed-tracing.md), ve izleme verilerini, Application Insights tarafından toplanan diğer uygulama günlük verileriyle depolanır. Kullanılabilir günlük sorguları, panolar ve uyarılar gibi diğer günlük verilerini olarak aynı analiz araçları sağlar.

Daha fazla bilgi, izleme dağıtılmış edinin [dağıtılmış izleme nedir?](../app/distributed-tracing.md).


## <a name="compare-azure-monitor-metrics-and-logs"></a>Azure İzleyici ölçüm ve günlükleri karşılaştırın

Aşağıdaki tabloda, ölçüm ve günlükleri Azure İzleyici'de karşılaştırır.

| Öznitelik  | Ölçümler | Günlükler |
|:---|:---|:---|
| Avantajlar | Basit ve uyarı verme gibi neredeyse gerçek zamanlı senaryoları yeteneğine. Sorunları hızlı algılanması için idealdir. | Analiz ile zengin sorgu dili. Ayrıntılı analiz ve kök nedenini belirlemek için idealdir. |
| Veriler | Yalnızca sayısal değerler | Metin veya sayısal veri |
| Yapı | Standart örnek saati, izlenmekte olan kaynak, sayısal bir değer özellikler kümesidir. Bazı ölçümler daha fazla tanımı için birden çok boyutta içerir. | Günlük türüne bağlı olarak bir özellik kümesi. |
| Koleksiyon | Düzenli aralıklarla toplanır. | Oluşturulacak Kayıt olaylarını tetiklemek gibi zaman zaman toplanan olabilir. |
| Azure portalında görüntüleme | Ölçüm Gezgini | Log Analytics |
| Veri kaynakları içerir | Platform ölçümleri Azure kaynaklarından toplanan.<br>Application Insights tarafından izlenen uygulamalar.<br>Uygulama veya API tarafından tanımlanan özel. | Uygulama ve tanılama günlükleri.<br>İzleme çözümleri.<br>Aracılar ve VM uzantıları.<br>Uygulama istekleri ve özel durumlar.<br>Azure Güvenlik Merkezi.<br>Veri Toplayıcı API'si |

## <a name="collect-monitoring-data"></a>İzleme verilerini toplama
Farklı [Azure İzleyici için veri kaynaklarını](data-sources.md) bir Log Analytics çalışma alanı (günlük) veya Azure İzleyici ölçüm veritabanı (ölçüler) ya da her ikisi için yazar. Diğer Azure depolama gibi başka bir konuma yazma ve günlükleri veya ölçümleri doldurmak için bazı yapılandırma gerektiren bazı kaynaklar doğrudan bu veri depoları için yazar. 

Bkz: [Azure İzleyicisi'nde ölçümler](data-platform-metrics.md) ve [Azure İzleyici'de oturum](data-platform-logs.md) her tür doldurmak farklı veri kaynakları listesi için.


## <a name="stream-data-to-external-systems"></a>Stream veri harici sistemlere bağlanma
İzleme verilerini analiz etmek için Azure'da araçlarını kullanabilmenin yanı sıra güvenlik bilgileri ve Olay yönetimi (SIEM) ürün gibi bir dış araç iletmek için bir gereksinim olabilir. Bu iletme genellikle doğrudan izlenen kaynakları üzerinden yapılır [Azure Event Hubs](/azure/event-hubs/). Bazı kaynakları, gerekli verileri almak için bir mantıksal uygulama gibi başka bir işlem kullanabilirsiniz, ancak verileri olay hub'ına doğrudan göndermek için yapılandırılabilir. Bkz: [Stream dış bir araç tarafından izleme verileri tüketim için olay hub'ına Azure](stream-monitoring-data-event-hubs.md) Ayrıntılar için.



## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi edinin [Azure İzleyicisi'nde ölçümler](data-platform-metrics.md).
- Daha fazla bilgi edinin [Azure İzleyici'de oturum](data-platform-logs.md).
- Hakkında bilgi edinin [izleme verilerini kullanılabilir](data-sources.md) azure'daki farklı kaynakları.
