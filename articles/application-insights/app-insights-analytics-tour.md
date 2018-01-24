---
title: "Azure Application Insights Analytics üzerinden bir tur | Microsoft Docs"
description: "Tüm ana sorgularda Analytics, Application Insights güçlü arama aracının kısa örnekleri."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/06/2017
ms.author: mbullwin
ms.openlocfilehash: 271ccc126eeb9411646b68b32fd30ce32b5eef5c
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Application ınsights'ta Analytics turu
[Analytics](app-insights-analytics.md) güçlü arama özelliğidir [Application Insights](app-insights-overview.md). Bu sayfaları günlük analizi sorgu dili açıklanmaktadır.

* **[Tanıtım videosunu izleyin](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Benzetimli verilerimizi Analytics sürücüde test](https://analytics.applicationinsights.io/demo)**  uygulamanızı veriler Application Insights'a henüz değil gönderiliyorsa.
* **[SQL-kullanıcıların kopya sayfası](https://aka.ms/sql-analytics)**  en yaygın deyimleri çevirir.

Başlamanıza yardımcı olmak için bazı temel sorguları aracılığıyla ilerlemesi atalım.

## <a name="connect-to-your-application-insights-data"></a>Application Insights verilerinize bağlanın
Uygulamanızın analizi açmak [genel bakış dikey penceresinde](app-insights-dashboards.md) Application ınsights'ta:

![Portal.Azure.com açın, Application Insights kaynağınıza açın ve analizi'ı tıklatın.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsioquerylanguagequerylanguagetakeoperatorhtml-show-me-n-rows"></a>[Ele](https://docs.loganalytics.io/queryLanguage/query_language_takeoperator.html): n satırları göster
Kullanıcı işlemleri (web uygulamanız tarafından alınan genellikle HTTP isteklerini) oturum veri noktaları denilen bir tabloda depolanır `requests`. Her satır, uygulamanıza Application Insights SDK'sı alınan telemetri bir veri noktasıdır.

Tablonun birkaç örnek satırları inceleyerek başlayalım:

![sonuçlar](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Git tıklatmadan önce imleç bildiriminde yere yerleştirin. Bir deyim birden fazla hattından bölebilirsiniz ancak boş satırlar bir deyimde koymayın. Boş satırlar penceresinde birkaç ayrı sorgulara tutmak için kullanışlı bir yoldur.
>
>

Sütunları seçin, bunları sütunlara göre Gruplandır sürükleyin ve filtre:

![Sütun Seçimi sonuçlarının üst sağ tıklatın](./media/app-insights-analytics-tour/030.png)

Ayrıntıları görmek için herhangi bir öğeyi genişletin:

![Tablo seçin ve yapılandırma sütunları kullanın](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Web tarayıcısında kullanılabilir sonuçları yeniden sıralamak için sütun başlığını tıklatın. Ancak büyük sonuç kümesi için tarayıcıya indirilen satır sayısı sınırlı olduğunu unutmayın. Bu şekilde sıralama sadece döndürülen sonuç kümesini sıralar ve gerçek yüksek veya en düşük öğeleri her zaman göstermez. Öğeleri güvenilir bir şekilde sıralamak için kullanmak `top` veya `sort` işleci.
>
>

## <a name="query-across-applications"></a>Uygulamalar arasında sorgu
Birden çok Application Insights uygulamalardan veri birleştirmek istiyorsanız, kullanmak **uygulama** anahtar tablo adı birlikte uygulama belirtin.  Bu sorguyu kullanarak iki farklı uygulama isteklerinden birleştirir **UNION** komutu.


```AIQL

    union app('fabrikamstage').requests, app('fabrikamprod').requests
    
```

## <a name="tophttpsdocsloganalyticsioquerylanguagequerylanguagetopoperatorhtml-and-sorthttpsdocsloganalyticsioquerylanguagequerylanguagesortoperatorhtml"></a>[Üst](https://docs.loganalytics.io/queryLanguage/query_language_topoperator.html) ve [sıralama](https://docs.loganalytics.io/queryLanguage/query_language_sortoperator.html)
`take`hızlı bir örnek bir sonuç almak kullanışlıdır ancak belirli bir sırada satırları tablodan gösterir. Sıralı bir görünüm elde etmek için kullanın `top` (için bir örnek) veya `sort` (üzerinden tüm tablo).

Belirli bir sütuna göre sıralanmış ilk n satırları göster:

```AIQL

    requests | top 10 by timestamp desc
```

* *Sözdizimi:* çoğu işleçleri gibi anahtar sözcüğü parametrelere sahip `by`.
* `desc`azalan düzende = `asc` artan =.

![](./media/app-insights-analytics-tour/260.png)

`top...`Daha fazla kullanıcı bildiren yoludur `sort ... | take...`. Biz yazılı:

```AIQL

    requests | sort by timestamp desc | take 10
```

Sonuç aynı kalır, ancak biraz daha yavaş çalışır. (Ayrıca yazabilirsiniz `order`, bir diğer ad olduğu `sort`.)

Sütun üstbilgileri Tablo görünümünde ayrıca ekranında sonuçları sıralamak için kullanılabilir. Ancak, kullandıysanız Kuşkusuz `take` veya `top` hemen almak için sütun başlığına tıklayarak bir tablonun parçası sadece alınan kayıt yeniden sıralamak.

## <a name="wherehttpsdocsloganalyticsioquerylanguagequerylanguagewhereoperatorhtml-filtering-on-a-condition"></a>[Burada](https://docs.loganalytics.io/queryLanguage/query_language_whereoperator.html): bir koşula göre filtreleme

Belirli Sonuç kodu döndürdü yalnızca istekleri görelim:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

`where` İşleci bir Boole ifadesi alır. Bunlarla ilgili bazı önemli noktalar şunlardır:

* `and`, `or`: Boole işleçleri
* `==`, `<>`, `!=` : eşit ve eşit değil
* `=~`, `!~` : büyük küçük harf duyarlı dize eşit ve eşit değil. Daha fazla dize karşılaştırma işleçleri çok vardır.

<!---Read all about [scalar expressions]().--->

### <a name="find-unsuccessful-requests"></a>Başarısız istekleri Bul

Bir dize değeri büyük kullanmak için bir tamsayıya dönüştürmek-karşılaştırması:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Zaman

Varsayılan olarak, sorgularınızı son 24 saat kısıtlanır. Ancak bu aralığı değiştirebilirsiniz:

![](./media/app-insights-analytics-tour/change-time-range.png)

İlgili herhangi bir sorgu yazarak zaman aralığını geçersiz kılma `timestamp` where yan tümcesinde. Örneğin:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

Zaman aralığı özelliği, 'kaynak tabloları birinin her Bahsetme sonra eklenen where' yan tümcesi eşdeğerdir.

`ago(3d)`'üç gün önce' anlamına gelir. Diğer birimleri süreyi saat dahil et (`2h`, `2.5h`), dakika (`25m`) ve saniye (`10s`).

Diğer örnekler:

```AIQL

    // Last calendar week:
    requests
    | where timestamp > startofweek(now()-7d)
        and timestamp < startofweek(now())
    | top 5 by duration

    // First hour of every day in past seven days:
    requests
    | where timestamp > ago(7d) and timestamp % 1d < 1h
    | top 5 by duration

    // Specific dates:
    requests
    | where timestamp > datetime(2016-11-19) and timestamp < datetime(2016-11-21)
    | top 5 by duration

```

[Tarihler ve saatlere başvuru](https://docs.loganalytics.io/concepts/concepts_datatypes_datetime.html).


## <a name="projecthttpsdocsloganalyticsioquerylanguagequerylanguageprojectoperatorhtml-select-rename-and-compute-columns"></a>[Proje](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html): sütunları işlem seçin ve yeniden adlandırma
Kullanım [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) yalnızca istediğiniz sütunları seçmek için:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Sütunları yeniden adlandırın ve yenilerini tanımlayın:

```AIQL

    requests
    | top 10 by timestamp desc
    | project  
            name,
            response = resultCode,
            timestamp,
            ['time of day'] = floor(timestamp % 1d, 1s)
```

![Sonuç](./media/app-insights-analytics-tour/270.png)

* Sütun adları, boşluk içerebilir veya bunlar köşeli parantez içindeki, simgeler şöyle: `['...']` veya`["..."]`
* `%`normal mod işleci olur.
* `1d`(bir basamak biri olan sonra bir 'D ') bir timespan değişmez değer bir gün anlamına gelir. Daha fazla bazı timespan değişmez değerler şunlardır: `12h`, `30m`, `10s`, `0.01s`.
* `floor`(diğer ad `bin`) değeri sağladığınız taban değeri en yakın katına aşağı yuvarlar. Bu nedenle `floor(aTime, 1s)` aşağıya doğru en yakın ikinci bir saat yuvarlar.

İfadeleri, normal işleçleri içerebilir (`+`, `-`,...), ve bir dizi kullanışlı işlevi yoktur.

## <a name="extend"></a>Genişletme
Yalnızca var olanları sütun eklemek istiyorsanız, kullanmak [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Kullanarak [ `extend` ](https://docs.loganalytics.io/queryLanguage/query_language_extendoperator.html) daha az ayrıntılıdır [ `project` ](https://docs.loganalytics.io/queryLanguage/query_language_projectoperator.html) var olan tüm sütunları tutmak istiyorsanız.

### <a name="convert-to-local-time"></a>Yerel saate dönüştürme

Zaman damgaları her zaman UTC biçimindedir. Bu nedenle BİZE Pasifik Yakası olduğunuz ve Kış ise, bu istediğiniz:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```


## <a name="summarizehttpsdocsloganalyticsioquerylanguagequerylanguagesummarizeoperatorhtml-aggregate-groups-of-rows"></a>[Özetlemek](https://docs.loganalytics.io/queryLanguage/query_language_summarizeoperator.html): toplam satır grupları
`Summarize`Belirtilen bir geçerlidir *toplama işlevi* satır grupları üzerinden.

Örneğin, web uygulamanız için bir istek yanıt süresini alanında bildirilir `duration`. Tüm istekleri için ortalama yanıt süresi görelim:

![](./media/app-insights-analytics-tour/410.png)

Veya biz sonucu farklı adlar istekleri ayrı:

![](./media/app-insights-analytics-tour/420.png)

`Summarize`Akış veri noktaları gruplar halinde kendisi için toplar `by` yan tümcesi eşit olarak değerlendirir. Her değer `by` ifade - yukarıdaki örnekte her benzersiz işlem adı - sonuç tablosunda bir satırı sonuçlanıyor.

Veya sonuçları günün saatini göre gruplandırabilirsiniz:

![](./media/app-insights-analytics-tour/430.png)

Biz nasıl kullanıyorsanız fark `bin` işlevi (diğer adıyla `floor`). Yalnızca kullansaydık `by timestamp`, kendi az grubunda her giriş satır yapmış olursunuz. Süreleri gibi sürekli tüm skaler için veya numaraları, biz sahip yönetilebilir bir ayrık değerler numarada sürekli aralık ayırmak. `bin`-yalnızca bilinen yuvarlama aşağı olduğu `floor` işlev - bunu yapmanın en kolay yoludur.

Dizeleri aralıklarına azaltmak için size aynı yöntemi kullanabilirsiniz:

![](./media/app-insights-analytics-tour/440.png)

Kullanabileceğiniz bildirimi `name=` toplama ifadeleri veya tümcesi tarafından bir sonuç sütunu adını ayarlamak için.

## <a name="counting-sampled-data"></a>Sayım örneklenen verileri
`sum(itemCount)`olayları saymak için önerilen toplama var. Çoğu durumda, ItemCount işlevi yalnızca yukarı grubundaki satır sayısını sayar şekilde == 1. Ancak zaman [örnekleme](app-insights-sampling.md) olan işleminde, yalnızca özgün olayların kesir korunur Application ınsights'ta veri noktaları olarak gördüğünüz her veri noktası için vardır; böylece `itemCount` olaylar.

Örnekleme %75 özgün olayları, daha sonra ItemCount atar, örneğin, == 4 tutulan kayıtlarında - diğer bir deyişle, korunan her kayıt için vardı dört özgün kaydeder.

Uyarlamalı örnekleme uygulamanızı ağırlıklı olarak ne zaman kullanıldığını dönemlerde daha yüksek olacak şekilde ItemCount neden olur.

ItemCount birleşimi, bu nedenle özgün olay sayısı için iyi bir gösterge sağlar.

![](./media/app-insights-analytics-tour/510.png)

Ayrıca bir `count()` toplama (ve sayısı işlemi) durumlarda, gerçekten istediğiniz bir grup satır sayısı.

Bir dizi var. [toplama işlevleri](https://docs.loganalytics.io/learn/tutorials/aggregations.html).

## <a name="charting-the-results"></a>Sonuçları grafik
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Varsayılan olarak bir tablo olarak sonuçları görüntülenir:

![](./media/app-insights-analytics-tour/225.png)

Tablo görünümünde daha iyi yapabileceğimiz. Grafik görünümünde sonuçlarını ile dikey bakalım çubuk seçeneği:

![Grafiği tıklatın ardından dikey çubuk grafiği seçin ve ata x ve y eksenleri](./media/app-insights-analytics-tour/230.png)

Ancak dikkat edin (Tablo görünümünde görebileceğiniz gibi) zamanına göre sonuçları biz sıralamak alamadık, grafik görüntüleme, tarih/saat her zaman doğru sırada gösterir.


## <a name="timecharts"></a>Timecharts
Kaç tane var. her saat olaylardır göster:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Grafik görüntüleme seçeneği belirleyin:

![timechart](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Birden fazla seri
Birden çok ifadelerinde `summarize` yan tümcesi birden çok sütun oluşturur.

Birden çok ifadelerinde `by` yan tümcesi her değer birleşimlerinde birden çok satır oluşturur.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Saat ve konuma göre istekleri tablosu](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Bir grafik boyutlara göre bölme
Bir dize sütunu ve sayısal bir sütun içeren bir tablo grafik, dize, sayısal veri noktalarını ayrı bir dizi bölmek için kullanılabilir. Birden çok dize sütunu ise, hangi sütunun ayrıştırıcı kullanmak üzere seçebilirsiniz.

![Bir analiz grafik segment](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Oranı sıçrama

Ayrıştırıcı kullanılacak bir dize için bir Boole değeri dönüştürün:

```AIQL

    // Bounce rate: sessions with only one page view
    requests
    | where notempty(session_Id)
    | where tostring(operation_SyntheticSource) == "" // real users
    | summarize pagesInSession=sum(itemCount), sessionEnd=max(timestamp)
               by session_Id
    | extend isbounce= pagesInSession == 1
    | summarize count()
               by tostring(isbounce), bin (sessionEnd, 1h)
    | render timechart
```

### <a name="display-multiple-metrics"></a>Birden çok ölçümleri görüntüleme
Bir tabloda, zaman damgası yanı sıra birden fazla sayısal sütun grafik, herhangi bir bileşimini görüntüleyebilirsiniz.

![Bir analiz grafik segment](./media/app-insights-analytics-tour/110.png)

Seçmelisiniz **yok bölünmüş** birden çok sayısal sütunlar seçebilmeniz için önce. Aynı anda birden fazla sayısal sütun görüntüleme olarak bir dize sütunu tarafından bölünemez.

## <a name="daily-average-cycle"></a>Günlük ortalama döngüsü
Kullanım ortalama bir günün nasıl değişiyor?

Saate binned sayısı istekler, bir gün modulo zamana göre:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Saat cinsinden ortalama bir günün çizgi grafiği](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Şu anda üzerinde bir çizgi grafiği görüntülemek için tarih/saat için süreler dönüştürmek olduğunu fark.
>
>

## <a name="compare-multiple-daily-series"></a>Birden fazla günlük seri Karşılaştır
Nasıl kullanım günü zaman içinde farklı ülkelerde değişiyor mu?

```AIQL

     requests  
     | where timestamp > ago(30d)  // Override "Last 24h"
     | where tostring(operation_SyntheticSource) == "" // real users
     | extend hour= floor( timestamp % 1d , 1h)
           + datetime("2001-01-01")
     | summarize event_count=sum(itemCount)
       by hour, client_CountryOrRegion
     | render timechart
```

![Bölünmüş tarafından client_CountryOrRegion](./media/app-insights-analytics-tour/130.png)

## <a name="plot-a-distribution"></a>Bir dağıtım Çiz
Kaç tane oturumları vardır, farklı uzunlukta?

```AIQL

    requests
    | where timestamp > ago(30d) // override "Last 24h"
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sessionDuration = max_timestamp - min_timestamp
    | where sessionDuration > 1s and sessionDuration < 3m
    | summarize count() by floor(sessionDuration, 3s)
    | project d = sessionDuration + datetime("2016-01-01"), count_
```

Son satırı datetime olarak dönüştürme için gereklidir. Şu anda yalnızca bir datetime ise grafiğin x ekseni bir skaler görüntülenir.

`where` Yan tümcesi dışlar tek adımda oturumları (sessionDuration == 0) ve x ekseni uzunluğunu belirler.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsioquerylanguagequerylanguagepercentilesaggfunctionhtml"></a>[Yüzdebirlik değeri](https://docs.loganalytics.io/queryLanguage/query_language_percentiles_aggfunction.html)
Hangi süreleri aralıklarına oturumları farklı yüzdelerini kapak?

Yukarıdaki sorguda kullanır, ancak son satırı değiştirin:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s)
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
```

Biz de üst sınırı nerede kaldırılan birden fazla isteği ile tüm oturumları dahil olmak üzere doğru rakamları alabilmek için yan tümcesi:

![Sonuç](./media/app-insights-analytics-tour/180.png)

İçinden biz görebilirsiniz:

* %5 oturumlarının 3 dakikadan 34s daha kısa bir süre; yine de sahip istiyor musunuz?
* Son 36 dakikadan az %50 oturumlarının;
* 7 günden fazla %5 oturumlarının son

Client_CountryOrRegion sütunu ayrı ayrı her ikisi de aracılığıyla her ülke, biz yalnızca için getirmek ayrı bir döküm almak için işleçleri şekli özetlenmektedir:

```AIQL

    requests
    | where isnotnull(session_Id) and isnotempty(session_Id)
    | summarize min(timestamp), max(timestamp)
      by session_Id, client_CountryOrRegion
    | extend sesh = max_timestamp - min_timestamp
    | where sesh > 1s
    | summarize count() by floor(sesh, 3s), client_CountryOrRegion
    | summarize percentiles(sesh, 5, 20, 50, 80, 95)
      by client_CountryOrRegion
```

![](./media/app-insights-analytics-tour/190.png)

## <a name="join"></a>Birleştir
Biz, istekler ve özel durumlar dahil olmak üzere çeşitli tablolara erişebilirsiniz.

Başarısız bir yanıt döndürdü bir isteği ile ilgili özel durumlar bulmak için şu tabloları üzerinde birleştirebilirsiniz `session_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Kullanmak iyi bir uygulamadır `project` ihtiyacımız birleştirme gerçekleştirmeden önce sütunları seçmek için.
Aynı yan tümcelerinde biz zaman damgası sütunu yeniden adlandırın.

## <a name="lethttpsdocsloganalyticsioquerylanguagequerylanguageletstatementhtml-assign-a-result-to-a-variable"></a>[Let](https://docs.loganalytics.io/queryLanguage/query_language_letstatement.html): bir sonucu bir değişkene atayın

Kullanım `let` önceki ifade bölümlerini ayırmak için. Sonuçları aynıdır:

```AIQL

    let bad_requests =
      requests
        | where  toint(resultCode) >= 500  ;
    bad_requests
    | join (exceptions) on session_Id
    | take 30
```

> [!Tip] 
> Analytics istemcisinde, sorgunun bölümlerini arasında boş satırlar koymayın. Tüm bunu yürüttüğünüzden emin olun.
>

Kullanım `toscalar` tek tablo hücresi değerine dönüştürmek için:

```AIQL
let topCities =  toscalar (
   requests
   | summarize count() by client_City 
   | top n by count_ 
   | summarize makeset(client_City));
requests
| where client_City in (topCities(3)) 
| summarize count() by client_City;
```


### <a name="functions"></a>İşlevler

Kullanım *izin* bir işlev tanımlamak için:

```AIQL

    let usdate = (t:datetime)
    {
      strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
      bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
    };
    requests  
    | extend PST = usdate(timestamp-8h)
```

## <a name="accessing-nested-objects"></a>İç içe geçmiş nesnelere erişme
İç içe geçmiş nesnelerde kolayca erişilebilir. Örneğin, özel durumlar akışta yapılandırılmış nesneleri bu gibi görebilirsiniz:

![Sonuç](./media/app-insights-analytics-tour/520.png)

İlgilendiğiniz özellikleri seçerek düzleştirmek:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Uygun türü sonucu cast gerektiğini unutmayın.


## <a name="custom-properties-and-measurements"></a>Özel özellikler ve ölçümler
Uygulamanızı bağlanıyorsa [özel boyutları (Özellikler) ve özel ölçümleri](app-insights-api-custom-events-metrics.md#properties) olayları, sonra bunları görürsünüz `customDimensions` ve `customMeasurements` nesneleri.

Örneğin, uygulamanızın içeriyorsa:

```csharp

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Bu değerleri Analytics ayıklamak için:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      m1 = todouble(customMeasurements.m1) // cast to expected type

```

Özel boyut belirli bir tür olup olmadığını doğrulamak için:

```AIQL

    customEvents
    | extend p1 = customDimensions.p1,
      iff(notnull(todouble(customMeasurements.m1)), ...
```

## <a name="dashboards"></a>Panolar
Birlikte, en önemli grafikler ve tablolar getirmek için bir Panoda sonuçlarınızı sabitleyebilirsiniz.

* [Azure paylaşılan Pano](app-insights-dashboards.md#share-dashboards): PIN simgesine tıklayın. Bunu, bir paylaşılan Pano sahip olmanız gerekir. Azure portalında açın veya bir pano oluşturun ve Paylaşım'ı tıklatın.
* [Power BI panosuna](app-insights-export-power-bi.md): tıklatın verme, Power BI sorgu. Bu alternatif bir avantajı, çok çeşitli diğer sonuçlarından kaynakları yanında sorgunuzu görüntüleyebilirsiniz ' dir.

## <a name="combine-with-imported-data"></a>İçeri aktarılan verilerle birleştirin

Analytics Raporları Panoda harika bakın, ancak bazen daha digestible form verileri çevirmek istediğiniz. Örneğin, bir diğer ad tarafından tanımlanan, kimliği doğrulanmış kullanıcılar telemetri varsayalım. Sonuç olarak, gerçek adlarını göster ister misiniz? Bunu yapmak için diğer adlar gerçek adlarını eşler bir CSV dosyası gerekir.

Bir veri dosyasını içeri aktarın ve gibi standart tablolardan (istekleri, özel durumlar ve benzeri) herhangi birini kullanın. Bu, kendi sorgulamak ya da diğer tablolarla katılın. Örneğin, usermap adlı bir tablo varsa ve sütunları içeren `realName` ve `userId`, çevirmek için kullanın sonra `user_AuthenticatedId` isteği telemetri alanındaki:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Bir tablodaki şema dikey altında almak için **diğer veri kaynakları**, örnek verileriniz yükleyerek yeni bir veri kaynağı eklemek için yönergeleri izleyin. Ardından bu tanımı tabloları karşıya yüklemek için kullanabilirsiniz.

Başlangıçta göreceğiniz şekilde içe aktar özelliği şu anda önizlemede, "Diğer veri kaynakları." altında "bize başvurun" bağlantı değil. Önizleme programına kaydolmak için bunu kullanın ve bağlantıyı daha sonra bir "yeni veri kaynağı Ekle" düğmesi değiştirilecek.


## <a name="tables"></a>Tablolar
Uygulamanızdan alınan telemetri akışı, birden çok tablo erişilebilir. Her tablo için kullanılabilen özellikleri şeması pencerenin sol tarafında görünür olur.

### <a name="requests-table"></a>İstekler tablosu
Web uygulaması ve kesim sayfa adı tarafından sayısı HTTP isteklerine:

![Ada göre kesimli sayısı istekleri](./media/app-insights-analytics-tour/analytics-count-requests.png)

Çoğu başarısız istekleri bulun:

![Ada göre kesimli sayısı istekleri](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Özel olaylar tablo
Kullanırsanız [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) kendi olayları göndermek için bu tablodan okuyabilirsiniz.

Bu satırlar, uygulama kodunuzun içerdiği bir örnek atalım:

```csharp

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Bu olaylar sıklığını görüntüle:

![Özel olaylar görüntü oranı](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Ölçüleri ve boyutları olayların ayıklayın:

![Özel olaylar görüntü oranı](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Özel ölçümleri tablosu
Kullanıyorsanız [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) ölçüm değerlerinizi göndermek için kendi sonuçlarında bulacaksınız **customMetrics** akış. Örneğin:  

![Application Insights analytics'te özel ölçümleri](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> İçinde [ölçüm Gezgini](app-insights-metrics-explorer.md), herhangi bir telemetri türüne bağlı tüm özel ölçümleri arada ölçüm dikey penceresi kullanılarak gönderilen ölçümleri birlikte bulunma `TrackMetric()`. Ancak, analizleri özel ölçümleri hala hangi türde bir telemetri TrackMetric tarafından gönderilen ölçümleri kendi akışında görünürken bunlar - olayları veya istekleri vb. - taşınan eklenir.
>
>

### <a name="performance-counters-table"></a>Performans sayaçları tablosu
[Performans sayaçları](app-insights-performance-counters.md) ve ağ kullanımı gibi CPU, bellek, uygulamanız için temel sistem ölçümleri gösterilmektedir. SDK'sı kendi özel sayaçlar dahil ek sayaçları göndermek için yapılandırabilirsiniz.

**PerformanceCounters** şeması sunan `category`, `counter` adı ve `instance` her performans sayacı adı. Sayaç örneği adları yalnızca bazı performans sayaçları için geçerlidir ve genellikle sayısı ilgili olduğu işlemin adını belirtin. Her uygulama için telemetri yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, görmek için hangi sayaçları kullanılabilir:

![Application Insights analytics performans sayaçları](./media/app-insights-analytics-tour/analytics-performance-counters.png)

Kullanılabilir belleğin bir grafik seçili süresi içinde almak için:

![Application Insights analytics'te bellek timechart](./media/app-insights-analytics-tour/analytics-available-memory.png)

Gibi diğer telemetri **performanceCounters** de bir sütuna sahip `cloud_RoleInstance` uygulamanızı çalıştıran ana bilgisayar makine kimliğini gösterir. Örneğin, uygulamanızın performansı ile farklı makinelerde karşılaştırmak için şunu yazın:

![Performans rol örneğinde Application Insights tarafından analiz bölümlenmiş.](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Özel durumlar tablosu
[Özel durumlar, uygulamanız tarafından bildirilen](app-insights-asp-net-exceptions.md) bu tabloda kullanılabilir.

Özel durum oluştuğunda uygulamanızı işleme HTTP isteği bulmak için üzerinde operation_Id Katıl:

![Özel durumlar isteklerle operation_Id üzerinde katılma](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Tarayıcı zamanlamaları tablosu
`browserTimings`Kullanıcılarınızın tarayıcılarda toplanan sayfa yükleme verileri gösterir.

[İstemci tarafı telemetri için uygulamanızı ayarlayın](app-insights-javascript.md) Bu ölçümler görmek için.

Şeması içerir [yükleme işlemi sayfasının farklı aşamalar uzunlukları gösteren ölçümleri](app-insights-javascript.md#page-load-performance). (Bunlar, kullanıcılarınızın bir sayfa okuma süre göstermediği.)  

Farklı sayfalara popularities Göster ve her bir sayfa için zamanları yük:

![Sayfa yükleme sürelerinin analytics'te](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Kullanılabilirlik sonuçları tablosu
`availabilityResults`sonuçlarını gösterir, [web testleri](app-insights-monitor-web-app-availability.md). Her çalışma testlerinizin her test konumdan ayrı olarak bildirilir.

![Sayfa yükleme sürelerinin analytics'te](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Bağımlılıklar tablosu
Uygulamanızı veritabanları ve REST API'leri yapar ve diğer TrackDependency() için çağırır çağrıları sonuçlarını içerir. Ayrıca tarayıcıdan oluşturulan AJAX çağrıları içerir.

AJAX çağrıları tarayıcısından:

```AIQL

    dependencies | where client_Type == "Browser"
    | take 10
```

Bağımlılık çağrıları sunucudan:

```AIQL

    dependencies | where client_Type == "PC"
    | take 10
```

Sunucu tarafı bağımlılık sonuçları her zaman göster `success==False` Application Insights aracısı yüklü değilse. Ancak, diğer verileri doğru değildir.

### <a name="traces-table"></a>İzlemeler tablosu
TrackTrace(), kullanma, uygulamanız tarafından gönderilen telemetriyi içerir veya [başka günlük altyapılarına](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Video 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Gelişmiş sorgular:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Sonraki adımlar
* [Analytics dil başvurusu](app-insights-analytics-reference.md)
* [SQL-kullanıcıların kopya sayfası](https://aka.ms/sql-analytics) en yaygın deyimleri çevirir.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
