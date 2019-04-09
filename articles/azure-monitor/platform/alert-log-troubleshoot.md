---
title: Azure İzleyici'de günlüğü uyarılarına sorunlarını giderme | Microsoft Docs
description: Sık karşılaşılan sorunlar, hataları ve çözümleme için günlük uyarı kuralları azure'da.
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: vinagara
ms.subservice: alerts
ms.openlocfilehash: aa42e8975432de8ca489cf9b1b6dd509c9fb01c1
ms.sourcegitcommit: 045406e0aa1beb7537c12c0ea1fbf736062708e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "59005298"
---
# <a name="troubleshooting-log-alerts-in-azure-monitor"></a>Azure İzleyicisi'nde sorun giderme günlük uyarıları  

## <a name="overview"></a>Genel Bakış

Bu makalede, Azure İzleyici'de günlük uyarıları ayarlama sırasında görülen yaygın sorunların nasıl giderileceğini gösterir. Ayrıca, çözümler işlevselliği veya günlük uyarıları yapılandırma ile ilgili sık sorulan soruların yanıtlarını sağlar. 

Terim **günlük uyarıları** yangın günlük sorguda bağlı uyarılar açıklar bir [Log Analytics çalışma alanı](../learn/tutorial-viewdata.md) veya [Application Insights](../../azure-monitor/app/analytics.md). Daha fazla ilgili işlevler, terminolojisi ve türlerinde öğrenin [günlük uyarıları - genel bakış](../platform/alerts-unified-log.md).

> [!NOTE]
> Bu makalede, Azure portal'ı gösterdiğinde durumları ve uyarı kuralını tetikleyen ve ilişkili bir eylem grupları tarafından gerçekleştirilen bir bildirim dikkate almaz. Böyle durumlarda, ayrıntıları bu makalede şirket edinmek [Eylem grupları](../platform/action-groups.md).


## <a name="log-alert-didnt-fire"></a>Günlük uyarı yangın gelmedi

İşte bazı yaygın nedenler neden yapılandırılmış bir [Azure İzleyici'de günlük uyarı kuralı](../platform/alerts-log.md) değil durumu göster [olarak *harekete* beklendiğinde](../platform/alerts-managing-alert-states.md). 

### <a name="data-ingestion-time-for-logs"></a>Günlükler için veri alım zamanı

Günlük uyarı düzenli aralıklarla çalışan temel sorgunuzu [Log Analytics](../learn/tutorial-viewdata.md) veya [Application Insights](../../azure-monitor/app/analytics.md). Azure İzleyici, binlerce müşteri çeşitli kaynaklardan gelen verileri terabayta kadar dünya genelindeki işlediğinden, hizmet için değişen gecikme süresini saldırılara açıktır. Daha fazla bilgi için [veri alımı zaman Azure İzleyici günlüklerine](../platform/data-ingestion-time.md).

Veri alımı gecikme gidermek için sistem bekler ve uyarı sorgusu, gerekli verileri değil henüz alınır bulursa, birden çok kez yeniden dener. Sistem ayarlamak üssel olarak artan bir bekleme süresi vardır. Bunlar geciktirmek için veriyi kullanılabilir olduktan sonra günlük uyarı yalnızca Tetikleyiciler yavaş günlük verisi alımı nedeniyle olabilir. 

### <a name="incorrect-time-period-configured"></a>Süre yanlış yapılandırılmış

Makalesinde açıklandığı [günlük uyarıları için terimler](../platform/alerts-unified-log.md#log-search-alert-rule---definition-and-types), dönem içinde belirtilen yapılandırmasını sorgu için zaman aralığını belirten zaman. Sorgu yalnızca bu zaman aralığı içinde oluşturulmuş olan kayıtları döndürür. Süre kötüye kullanımı önlemek günlük sorgusu için alınan verileri sınırlar ve hiçbir zaman komut bozar (gibi *önce*) günlük sorguda kullanılan. Örneğin, zaman aralığı 60 dakika olarak ayarlanmıştır ve sorgu 13: 15'te çalıştırılan 12:15 PM arasında 13: 15'te oluşturulan kayıtları için günlük sorgusu kullanılır. Günlük sorgu süresi komutu gibi kullanıyorsa *önce (1d)*, sorgu süre için o interval.* olarak ayarlandığından yine de yalnızca 13: 15'te 12:15 PM arasında verileri kullanır.

Bu nedenle, onay yapılandırma dönemdeki Sorgunuzla eşleşen. Günlük sorgusu kullanıyorsa, daha önce belirtilen örneğin *önce (1d)* yeşil işaretçisi ile gösterildiği gibi ardından zaman aralığı 24 saat veya 1440 dakika (içinde belirtilen kırmızı) olarak sorguyu tasarlandığı gibi yürüten emin olmak için ayarlanması gerekir.

![Zaman aralığı](media/alert-log-troubleshoot/LogAlertTimePeriod.png)

### <a name="suppress-alerts-option-is-set"></a>Seçenek kümesi uyarıları bastır

Makalenin 8 adımda açıklandığı [Azure portalında günlük uyarı kuralı oluşturma](../platform/alerts-log.md#managing-log-alerts-from-the-azure-portal), günlük uyarıları sağlayan bir **uyarıları bastır** seçeneği için yapılandırılmış bir süre tetikleme ve bildirim eylemlerini gizlemek için saat. Sonuç olarak, çünkü bu oldu, ancak engellendi sırasında bir uyarı yangın olmadı düşünebilirsiniz.  

![Uyarıları Gizle](media/alert-log-troubleshoot/LogAlertSuppress.png)

### <a name="metric-measurement-alert-rule-is-incorrect"></a>Ölçüm ölçüsü uyarı kuralı yanlış

**Ölçüm ölçüsü günlük uyarıları** özel özellikleri ve kısıtlı uyarı sorgusu söz dizimi olan günlük uyarı alt olan. Bir ölçüm ölçüsü günlük uyarı kuralı bir ölçüm zaman serisi olmasını çıkış sorgu gerektirir; diğer bir deyişle, ayrı bir tabloyla eşit olarak süreler karşılık gelen toplam değerler ile birlikte boyutlandırılmış. Ayrıca, kullanıcıların tabloda AggregatedValue yanı sıra bir ek değişkeni sahip olmayı seçebilirsiniz. Tabloyu sıralamak için bu değişkenleri kullanılabilir. 

Örneğin, bir ölçüm ölçüsü günlük uyarı kuralı olarak yapılandırılan varsayalım:

- Sorgu şöyleydi: `search *| summarize AggregatedValue = count() by $table, bin(timestamp, 1h)`  
- 6 saatlik süre
- 50 eşiği
- üç ardışık ihlaller uyarı mantığı
- Toplama $table seçilen bağlı

Komut içerdiğinden *... Özetleme ölçütü* ve iki değişken (zaman damgası & $table), sağlanan sistem için $table seçer *üzerinde toplama*. Alana göre sonuç tabloyu sıralar *$table* aşağıda gösterildiği gibi ve ardından her bir tablo türü (gibi availabilityResults) için birden çok AggregatedValue 3 veya daha fazla ardışık ihlaller olup olmadığını görmek için bakar.

![Birden çok değer ile ölçüm ölçüm sorgu yürütme](media/alert-log-troubleshoot/LogMMQuery.png)

Olarak *üzerinde toplama* tanımlanan *$table*, verileri $table sütununda (kırmızı); olduğu gibi sıralanan sonra Grup ve türleri için konum *üzerinde toplama* alan $table için (yani) Örnek: değerleri availabilityResults bir çizim/varlık (olarak vurgulanmış turuncu) olarak kabul edilir. Bu değeri Çizdirmek/varlık uyarı hizmetinin (gösterildiği yeşil) oluşan üç ardışık ihlaller Tablo değeri 'availabilityResults' için hangi uyarı tetiklenir denetler. Benzer şekilde, tüm diğer değerini üç ardışık ihlaller görülürse - $table, başka bir uyarı bildirimi aynı şeyi olarak tetiklenir; Uyarı hizmetiyle (turuncu) olduğu gibi bir çizim/varlık değerleri zamanına göre otomatik olarak sıralama.

Şimdi varsayalım, ölçüm ölçüsü günlük uyarı kuralı ne zaman değiştirildiğini ve sorgu `search *| summarize AggregatedValue = count() by bin(timestamp, 1h)` aynı kalan üç ardışık ihlaller için uyarı mantığı eklemeden önce yapılandırma geri kalanı ile. "Toplama sırasında" seçeneği bu durumda olacaktır varsayılan: zaman damgası. Sorgu için yalnızca bir değer sağlandığından *... Özetleme ölçütü* (yani) *zaman damgası*; yürütme çıktısı, aşağıda gösterildiği gibi olacaktır sonunda önceki örneğe benzer.

   ![Tekil değer ile ölçüm ölçüm sorgu yürütme](media/alert-log-troubleshoot/LogMMtimestamp.png)

Olarak *üzerinde toplama* tanımlı zaman damgasını, veriler üzerinde sıralanır *zaman damgası* sütun (olduğu gibi "kırmızı); sonra da biz Grup zaman damgası tarafından örneğin: değerleri `2018-10-17T06:00:00Z` bir çizim/varlık (olarak kabul edilir Turuncu renkte vurgulanmış). Bu değeri Çizdirmek/varlık yok ardışık ihlaller tekrarlanan (her zaman damgası değeri yalnızca bir giriş olduğundan) ve bu nedenle uyarı asla tetiklenmez uyarı hizmetinin bulabilirsiniz. Bu nedenle böyle bir durumda, kullanıcı aşağıdakilerden birini yapmalısınız:

- Yapılandırılmış "Toplama sırasında" alanını kullanarak yapılan sıralama doğru işlevsiz bir değişken veya mevcut bir değişken (gibi $table) ekleyin.
- (Veya) dayalı uyarı mantığı kullanmak için uyarı kuralı yeniden *toplam ihlal* bunun yerine uygun şekilde

## <a name="log-alert-fired-unnecessarily"></a>Günlük uyarı gereksiz yere tetiklendi

Ayrıntılı sonraki bazı yaygın nedenler neden olan bir yapılandırılmış [Azure İzleyici'de günlük uyarı kuralı](../platform/alerts-log.md) görüntülendiğinde tetiklenebilir [Azure uyarıları](../platform/alerts-managing-alert-states.md), harekete beklemiyoruz.

### <a name="alert-triggered-by-partial-data"></a>Kısmi veriler tarafından tetiklenen uyarı

Log Analytics ve Application Insights'ı destekleyen analiz alımı gecikmeleri ve işleme tabi olan; hangi nedeniyle sağlanan günlük uyarı sorgusu çalıştırıldığında - zaman olabilir bir servis talebi almalarının hiçbir veri ya da yalnızca mevcut olan bazı veriler. Daha fazla bilgi için [Azure İzleyici'de veri alma süresini oturum](../platform/data-ingestion-time.md).

Uyarı kuralı nasıl yapılandırıldığına bağlı olarak olabilir yanlış firing varsa yok veya kısmi veri günlüklerinde uyarı yürütme zamanında. Bu gibi durumlarda, size uyarı sorgusu veya yapılandırma değiştirmenizi önerin. 

Örneğin, günlük uyarı kuralı yapılandırdıysanız hiçbir veri (sıfır kaydı) veya kısmi sonuçlar (bir kayıt) olduğunda bir analytics sorgusunun sonuçlarından sayısı 5'ten az olduğunda tetiklemek için daha sonra bir uyarı tetikler. Ancak, veri alımı gecikme sonrasında tam veri ile aynı sorgu sonucu 10 kayıt sağlayabilir.

### <a name="alert-query-output-misunderstood"></a>Uyarı sorgusu çıkış yanlış

Log analytics sorgu uyarılara ilişkin mantığı sağlar. Analytics sorgusu, çeşitli büyük veri ve matematik işlevleri kullanabilir.  Uyarı hizmeti sorgunuzu verilerle zaman belirtilen dönem için belirtilen aralıklarla yürütür. Uyarı hizmeti için sağlanan sorgu görünümünde hafif değişiklikler seçilen uyarı türüne göre yapar. Bu, "yürütülecek sorgu" bölümünde görülebilir *sinyal mantığını yapılandırma* ekranını aşağıda gösterildiği gibi: ![Yürütülecek sorgu](media/alert-log-troubleshoot/LogAlertPreview.png)

İçinde gösterilen **yürütülecek sorgu** kutusudur günlük uyarı hizmetinin çalıştırılanlar. Belirtilen sorgu yanı sıra aracılığıyla timespan çalıştırabileceğiniz [analiz portalı](../log-query/portals.md) veya [analizi API'si](https://docs.microsoft.com/rest/api/loganalytics/) ne olabileceği gibi aslında bir uyarı oluşturmadan önce uyarı sorgusu çıkış anlamak istiyorsanız.

## <a name="next-steps"></a>Sonraki adımlar

- Hakkında bilgi edinin [oturum uyarılar Azure uyarıları](../platform/alerts-unified-log.md)
- Daha fazla bilgi edinin [Application Insights](../../azure-monitor/app/analytics.md)
- Daha fazla bilgi edinin [oturum sorguları](../log-query/log-query-overview.md)
