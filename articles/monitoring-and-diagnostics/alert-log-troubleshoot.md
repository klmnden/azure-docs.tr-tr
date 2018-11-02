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
ms.openlocfilehash: 0e1cb2cdd6270590def11479cc5859d996d84caa
ms.sourcegitcommit: 6678e16c4b273acd3eaf45af310de77090137fa1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2018
ms.locfileid: "50749059"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Azure İzleyicisi'nde sorun giderme günlük uyarıları  

## <a name="overview"></a>Genel Bakış
Bu makalede, Azure İzleyici içinde günlük uyarıları ayarlama sırasında görülen yaygın sorunların ele gösterilmektedir. Ve işlevler veya günlük uyarıları yapılandırma ile ilgili sık sorulan soruların bir çözüm sağlar. Terim **günlük uyarıları** dayalı özel sorgu olduğu sinyal uyarılarını açıklamak için [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerden öğrenin [günlük uyarıları - genel bakış](monitor-alerts-unified-log.md).

> [!NOTE]
> Bu makalede, Azure portalında ve ilişkili eylem grupları aracılığıyla bildirim tetiklenen gibi uyarı kuralı gösterildiğinde durumları dikkate almaz. Böyle durumlarda, ayrıntıları bu makalede şirket edinmek [Eylem grupları](monitoring-action-groups.md).


## <a name="log-alert-didnt-fire"></a>Günlük uyarı yangın gelmedi

Ayrıntılı sonraki bazı yaygın nedenler neden olan bir yapılandırılmış [Azure İzleyici'de günlük uyarı kuralı](alert-log.md) görüntülendiğinde tetiklenir değil [Azure uyarıları](monitoring-alerts-managing-alert-states.md), harekete beklediğiniz. 

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alım zamanı
Günlük uyarı works düzenli aralıklarla göre müşteri tarafından sağlanan sorgusunu çalıştırarak [Log Analytics](../log-analytics/log-analytics-tutorial-viewdata.md) veya [Application Insights](../application-insights/app-insights-analytics.md). Her ikisi de muazzam miktarlardaki günlük verileri işler ve aynı işlevselliği sağlar, analizinin gücünden tarafından desteklenir. Log Analytics hizmeti dünya genelindeki terabayta kadar binlerce müşteri hem çeşitli kaynaklardan veri işleme gerektirir - hizmet için gecikme süresini saldırılara açıktır gibi. Daha fazla bilgi için [Log Analytics veri alımı zamanında](../log-analytics/log-analytics-data-ingestion-time.md).

Application Insights veya Log Analytics ortaya çıkabilecek veri alımı gecikme gidermek için günlükleri; Günlük uyarı bekler ve ne zaman veri henüz uyarı zaman aralığı için alınan değil bulduğu bir süre sonra yeniden denenir. Biz verilerin Log Analytics tarafından alınan gerekli süre bekleyin emin olmak için bir üssel olarak artan bekleme süresi ayarlama, günlük uyarıları var. Günlük uyarı kuralı tarafından sorgulanan günlükleri alma gecikmeleri etkileniyorsa, yalnızca veri alma sonrası Log Analytics ve üstel bir zaman aralığı nedeniyle günlük uyarı hizmetinin birden çok kez arada denemeden sonra kullanılabilir olduktan sonra bu nedenle daha sonra günlük uyarı tetikler .

### <a name="incorrect-time-period-configured"></a>Süre yanlış yapılandırılmış
Makalesinde açıklandığı [günlük uyarıları için terimler](monitor-alerts-unified-log.md#log-search-alert-rule---definition-and-types), süre yapılandırmasında belirtilen sorgu için zaman aralığını belirtir. Sorgu yalnızca bu zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (önce ister) günlük sorguda kullanılan. 
*Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorguyu, 13: 15'te çalıştırmak, 12:15 PM arasında 13: 15'te oluşturulan kayıtları döndürülür günlük sorgusu yürütülemedi. Günlük sorgusu komutu gibi önce zaman kullanıyorsa, şimdi (1d), günlük sorgusu çalıştırılması yalnızca 1:15 PM - 12:15 PM arasında verileri için veri yalnızca son 60 dakika için varmış gibi. Yedi günün verilerini günlük sorgusu belirtildiği için değil.*

Sorgu mantığınızı bağlı olarak, uygun bir zaman diliminde yapılandırma sağlanan denetleyin. Daha önce belirtilen Örneğin günlük önce kullanır (1 d) ile gösterildiği gibi sorgu yeşil işaret - ardından zaman aralığı 24 saat veya 1440 dakika (kırmızı renkte belirtildiği gibi) olarak ayarlanmalıdır, sağlanan sorgu emin olmak için doğru olarak envisaged yürütür.
    ![Zaman aralığı](./media/monitor-alerts-unified/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Seçenek kümesi uyarıları bastır
Makalenin 8 adımda açıklandığı [Azure portalında günlük uyarı kuralı oluşturma](alert-log.md#managing-log-alerts-from-the-azure-portal), günlük uyarı bir seçenek sağlar uyarı kuralının otomatik gizleme yapılandırın ve bildirim/tetikleyici görünürlüğe zaman miktarı için engelleme. Eylem grubu belirtilen süre için tetikleme değil sırasında yürütmek günlüğü uyarısı bastır uyarılar seçeneği neden olur **uyarıları bastır** seçenek ve bu nedenle kullanıcı bu uyarı olmadı çünkü bu yapılandırıldığı şekilde engellendi sırada harekete düşünüyor .
    ![Uyarıları bastır](./media/monitor-alerts-unified/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Ölçüm ölçüsü uyarı kuralı yanlış
Ölçüm ölçüsü günlük uyarı kuralı özel yeteneklere sahip ancak sırayla kısıtlama uyarı sorgu söz dizimi kullanan günlük uyarıları alt türüdür. Ölçüm ölçüsü günlük uyarı kuralı için bir ölçüm zaman serisi - sağlamak için uyarı sorgusu çıkışını gerekli AggregatedValue ayrı eşit boyutlu zaman süreler karşılık gelen değerleri ile birlikte içeren bir tablo hesaplanan. Ayrıca, kullanıcılar bilgisayar, düğüm, vb. AggregatedValue yanı sıra tablo ek değişkenlerine sahip seçebilirsiniz. tablodaki hangi verileri kullanarak sıralanabilir.

Örneğin, ölçüm ölçüsü günlük uyarı kuralı olarak yapılandırılan varsayalım:
- Sorgu şöyleydi: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- 6 saatlik süre
- 50 eşiği
- üç ardışık ihlaller uyarı mantığı
- Toplama $table seçilen bağlı

Komut içinde kullandık olduğundan artık özetlemek... tarafından sağlanan iki değişken ve: zaman damgası & $table; uyarı hizmeti için "Toplama sırasında" - temelde $table seçin, alana göre sonuç tabloyu sıralamak: aşağıda gösterildiği gibi $table - ve ardından her tablo için birden çok AggregatedValue göz 3 veya daha fazla ardışık ihlaller olup olmadığını görmek için (availabilityResults gibi) yazın.

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
 
İçinde gösterilen **yürütülecek sorgu** bölümdür hangi günlük uyarı hizmetinin çalışır; kullanıcı belirtilen sorgu yanı sıra aracılığıyla timespan çalıştırılabilir [analiz portalı](../log-analytics/log-analytics-log-search-portals.md) veya [analizi API'si](https://docs.microsoft.com/en-us/rest/api/loganalytics/) -Bunlar uyarı oluşturması işleminden önce anlamak istiyorsanız, hangi uyarı sorgusu çıkış olabilir.
 
## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](monitor-alerts-unified-log.md)
* Daha fazla bilgi edinin [Application Insights](../application-insights/app-insights-analytics.md)
* Daha fazla bilgi edinin [Log Analytics](../log-analytics/log-analytics-overview.md). 