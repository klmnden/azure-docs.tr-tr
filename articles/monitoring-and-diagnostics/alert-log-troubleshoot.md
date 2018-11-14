---
title: Azure İzleyicisi'nde sorun giderme günlük uyarıları
description: Sık karşılaşılan sorunlar, hataları ve çözümleme için günlük uyarı kuralları azure'da.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 68488788f73c9662b5d1eaa3b670f2120941defc
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2018
ms.locfileid: "51616495"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Azure İzleyicisi'nde sorun giderme günlük uyarıları  
## <a name="overview"></a>Genel Bakış
Bu makalede, Azure İzleyici'de günlük uyarıları ayarlama sırasında görülen yaygın sorunların nasıl giderileceğini gösterir. Ayrıca, çözümler işlevselliği veya günlük uyarıları yapılandırma ile ilgili sık sorulan soruların yanıtlarını sağlar. 

Terim **günlük uyarıları** yangın özel bir sorgunun bağlı uyarılar açıklamak için [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerinde öğrenin [günlük uyarıları - genel bakış](monitor-alerts-unified-log.md).

> [!NOTE]
> Bu makalede, Azure portal'ı gösterdiğinde durumları ve uyarı kuralını tetikleyen ve ilişkili bir eylem grupları tarafından gerçekleştirilen bir bildirim dikkate almaz. Böyle durumlarda, ayrıntıları bu makalede şirket edinmek [Eylem grupları](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>Günlük uyarı yangın gelmedi

İşte bazı yaygın nedenler neden yapılandırılmış bir [Azure İzleyici'de günlük uyarı kuralı](alert-log.md) değil durumu göster [olarak *harekete* beklendiğinde](monitoring-alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alım zamanı
Günlük uyarı düzenli aralıklarla çalışan temel sorgunuzu [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). Log Analytics, binlerce müşteri çeşitli kaynaklardan gelen verileri terabayta kadar dünya genelindeki işlediğinden, hizmet için değişen gecikme süresini saldırılara açıktır. Daha fazla bilgi için [Log Analytics veri alımı zamanında](../log-analytics/log-analytics-data-ingestion-time.md).

Veri alımı gecikme gidermek için sistem bekler ve uyarı sorgusu, gerekli verileri değil henüz alınır bulursa, birden çok kez yeniden dener. Sistem ayarlamak üssel olarak artan bir bekleme süresi vardır. Bunlar geciktirmek için veriyi kullanılabilir olduktan sonra günlük uyarı yalnızca Tetikleyiciler yavaş günlük verisi alımı nedeniyle olabilir. 

### <a name="incorrect-time-period-configured"></a>Süre yanlış yapılandırılmış
Makalesinde açıklandığı [günlük uyarıları için terimler](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), dönem içinde belirtilen yapılandırmasını sorgu için zaman aralığını belirten zaman. Sorgu yalnızca bu zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (önce ister) günlük sorguda kullanılan. 
*Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırılan 12:15 PM arasında 13: 15'te oluşturulan kayıtları için günlük sorgusu kullanılır. Günlük sorgu süresi komutu gibi kullanıyorsa *önce (1d)*, bu aralık için zaman dilimini ayarlandığından sorgu hala yalnızca 13: 15'te 12:15 PM arasında verileri kullanır.*

Bu nedenle, onay yapılandırma dönemdeki Sorgunuzla eşleşen. Günlük sorgusu kullanıyorsa, daha önce belirtilen örneğin *önce (1d)* yeşil işaretçisi ile gösterildiği gibi ardından zaman aralığı 24 saat veya 1440 dakika (içinde belirtilen kırmızı) olarak sorguyu tasarlandığı gibi yürüten emin olmak için ayarlanması gerekir.

![Zaman aralığı](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Seçenek kümesi uyarıları bastır
Makalenin 8 adımda açıklandığı [Azure portalında günlük uyarı kuralı oluşturma](alert-log.md#managing-log-alerts-from-the-azure-portal), günlük uyarıları sağlayan bir **uyarıları bastır** seçeneği için yapılandırılmış bir süre tetikleme ve bildirim eylemlerini gizlemek için saat. Sonuç olarak, çünkü bu oldu, ancak engellendi sırasında bir uyarı yangın olmadı düşünebilirsiniz.  

![Uyarıları Gizle](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Ölçüm ölçüsü uyarı kuralı yanlış
**Ölçüm ölçüsü günlük uyarıları** özel özellikleri ve kısıtlı uyarı sorgusu söz dizimi olan günlük uyarı alt olan. Bir ölçüm ölçüsü günlük uyarı kuralı bir ölçüm zaman serisi olmasını çıkış sorgu gerektirir; diğer bir deyişle, ayrı bir tabloyla eşit olarak süreler karşılık gelen toplam değerler ile birlikte boyutlandırılmış. Ayrıca, kullanıcıların tabloda AggregatedValue yanı sıra bir ek değişkeni sahip olmayı seçebilirsiniz. Tabloyu sıralamak için bu değişkenleri kullanılabilir. 

Örneğin, bir ölçüm ölçüsü günlük uyarı kuralı olarak yapılandırılan varsayalım:
- Sorgu şöyleydi: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- 6 saatlik süre
- 50 eşiği
- üç ardışık ihlaller uyarı mantığı
- Toplama $table seçilen bağlı

Komut içerdiğinden *... Özetleme ölçütü* ve iki değişken (zaman damgası & $table), sağlanan sistem için "Toplama sırasında" $table seçer. Alana göre sonuç tabloyu sıralar *$table* aşağıda gösterildiği gibi ve ardından her bir tablo türü (gibi availabilityResults) için birden çok AggregatedValue 3 veya daha fazla ardışık ihlaller olup olmadığını görmek için bakar.

![Birden çok değer ile ölçüm ölçüm sorgu yürütme](./media/monitor-alerts-unified/LogMMQuery.png)

"Toplama sırasında" $table – olduğu gibi veriler (kırmızı); olduğu gibi $table sütununda sıralanır Grup ve "Toplama sırasında" alanı türleri için konum (yani) $table – örneğin: değerleri availabilityResults bir çizim/varlık (olarak vurgulanmış turuncu) olarak kabul edilir. Bu değeri Çizdirmek/varlık – uyarı hizmetinin (gösterildiği yeşil) oluşan üç ardışık ihlaller Tablo değeri 'availabilityResults' için hangi uyarı tetiklenir denetler. Benzer şekilde, tüm diğer değerini üç ardışık ihlaller görülürse - $table, başka bir uyarı bildirimi için aynı tetiklenir; Uyarı hizmetiyle (turuncu) olduğu gibi bir çizim/varlık değerleri zamanına göre otomatik olarak sıralama.

Şimdi varsayalım, ölçüm ölçüsü günlük uyarı kuralı ne zaman değiştirildiğini ve sorgu `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)` aynı kalan üç ardışık ihlaller için uyarı mantığı eklemeden önce yapılandırma geri kalanı ile. "Toplama sırasında" seçeneği bu durumda olacaktır varsayılan: zaman damgası. Sorgu için yalnızca bir değer sağlandığından... Özetleme ölçütü zaman damgası (yani); yürütme sonunda önceki örneğe benzer bir çıktı aşağıda gösterildiği gibi olacaktır. 

   ![Tekil değer ile ölçüm ölçüm sorgu yürütme](./media/monitor-alerts-unified/LogMMtimestamp.png)

"Toplama sırasında" zaman damgası – olduğu gibi (kırmızı); olduğu gibi zaman damgası sütunu üzerinde veri sıralandı Biz zaman damgası tarafından – örneğin grubu sonra: değerleri `2018-10-17T06:00:00Z` bir çizim/varlık (olarak vurgulanmış turuncu) olarak kabul edilir. Bu değeri Çizdirmek/varlık – uyarı hizmetinin hiçbir ardışık ihlaller tekrarlanan (her zaman damgası değeri yalnızca bir giriş olduğundan) ve bu nedenle uyarı asla tetiklenmez bulabilirsiniz. Bu nedenle böyle bir durumda ya da kullanıcı gerekir-
- Yapılandırılmış "Toplama sırasında" alanını kullanarak yapılan sıralama doğru işlevsiz bir değişken veya mevcut bir değişken (gibi $table) ekleyin.
- (Veya) dayalı uyarı mantığı kullanmak için uyarı kuralı yeniden *toplam ihlal* bunun yerine uygun şekilde
 
## <a name="log-alert-fired-unnecessarily"></a>Günlük uyarı gereksiz yere tetiklendi
Ayrıntılı sonraki bazı yaygın nedenler neden olan bir yapılandırılmış [Azure İzleyici'de günlük uyarı kuralı](alert-log.md) görüntülendiğinde tetiklenebilir [Azure uyarıları](monitoring-alerts-managing-alert-states.md), harekete beklemiyoruz.

### <a name="alert-triggered-by-partial-data"></a>Kısmi veriler tarafından tetiklenen uyarı
Log Analytics ve Application Insights'ı destekleyen analiz alımı gecikmeleri ve işleme tabi olan; hangi nedeniyle sağlanan günlük uyarı sorgusu çalıştırıldığında - zaman olabilir bir servis talebi almalarının hiçbir veri ya da yalnızca mevcut olan bazı veriler. Daha fazla bilgi için [Log Analytics veri alımı zamanında](../log-analytics/log-analytics-data-ingestion-time.md).

Uyarı kuralı nasıl yapılandırıldığına bağlı olarak olabilir yanlış firing durumunda yok ya da uyarı yürütme zamanında günlüklerinde kısmi veri. Bu gibi durumlarda, uyarı sorgusu veya yapılandırma değiştirildiğinde önerilir. *Günlük uyarı kuralı, örneğin, analiz sorgusundan gelen sonuçları sayısını olduğunda değerinden (5; diyelim) için yapılandırılmış Daha sonra hiçbir veri (sıfır kaydı) veya kısmi sonuçlar (bir kayıt) olduğunda uyarı kuralı tetiklenir. Burada-sonra alımı gecikme gibi aynı sorgu Analytics'te sorgu tam verilerle çalıştırıldığında sonucu 10 kayıtları olarak sağlayabilir.*

### <a name="alert-query-output-misunderstood"></a>Uyarı sorgusu çıkış yanlış
Günlük uyarıları için uyarı vermek için mantık analytics sorgusu kullanıcı tarafından sağlanır. Sağlanan analytics sorgusu, çeşitli büyük veri ve matematik işlevleri belirli yapıları oluşturmak için kullanabilirsiniz. Uyarı hizmeti ile verileri belirtilen zaman aralığı için belirtilen aralıklarla müşteri tarafından sağlanan sorgu yürütülür; sağlanan - seçilen uyarı türüne göre sorgu değişir uyarı hizmeti Zarif sağlar ve aynı yapılandırma sinyal mantığını ekranında, "yürütülecek sorgu" bölümünde aşağıda gösterildiği gibi gerektirdiğine Tanık: ![yürütülecek sorgu](./media/monitor-alerts-unified/LogAlertPreview.png)
 
İçinde gösterilen **yürütülecek sorgu** bölümdür hangi günlük uyarı hizmetinin çalışır; kullanıcı belirtilen sorgu yanı sıra aracılığıyla timespan çalıştırılabilir [analiz portalı](../log-analytics/log-analytics-log-search-portals.md) veya [analizi API'si](https://docs.microsoft.com/rest/api/loganalytics/) -Bunlar uyarı oluşturması işleminden önce anlamak istiyorsanız, hangi uyarı sorgusu çıkış olabilir.
 
## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](monitor-alerts-unified-log.md)
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinin [Log Analytics](../log-analytics/log-analytics-overview.md). 

