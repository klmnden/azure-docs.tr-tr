---
title: Azure Application Insights analiz aracılığıyla bir tur | Microsoft Docs
description: Kısa örnekler ana tüm sorguların analytics'te Application Insights'ın güçlü bir arama aracı.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: bddf4a6d-ea8d-4607-8531-1fe197cc57ad
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: mbullwin
ms.openlocfilehash: 470779f80e998c3908cf28328cfb415d98c5e06c
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39579264"
---
# <a name="a-tour-of-analytics-in-application-insights"></a>Application ınsights analiz turu
[Analytics](app-insights-analytics.md) güçlü arama özelliğidir [Application Insights](app-insights-overview.md). Bu sayfalar, Log Analytics sorgu dili açıklanmaktadır.

* **[Tanıtım videosunu izlemek](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**.
* **[Test sürüşü Analytics sanal veri çubuğunda](https://analytics.applicationinsights.io/demo)**  uygulamanızın verilerini Application Insights'a henüz değil gönderiliyorsa.
* **[SQL-kullanıcıların kural sayfası](https://aka.ms/sql-analytics)**  en yaygın deyimleri çevirir.

Başlamanıza yardımcı olmak için bazı temel sorgular bir kılavuzda ele alalım.

## <a name="connect-to-your-application-insights-data"></a>Application Insights verilerinize bağlanma
Analytics, uygulamanızın açın [genel bakış dikey penceresinde](app-insights-dashboards.md) Application ınsights'ta:

![Portal.Azure.com açın, Application Insights kaynağınızı açın ve analiz tıklayın.](./media/app-insights-analytics-tour/001.png)

## <a name="takehttpsdocsloganalyticsiodocslanguage-referencetabular-operators-show-me-n-rows"></a>[Ele](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators): n satırları göster
Kullanıcı işlemleri (web uygulamanız tarafından alınan genellikle HTTP istekleri) günlüğe veri noktaları olarak adlandırılan bir tabloda depolanır `requests`. Her satır, uygulamanıza Application Insights SDK'sı alınan telemetri bir veri noktasıdır.

Tabloya birkaç örnek satırlarını inceleyerek başlayalım:

![sonuçlar](./media/app-insights-analytics-tour/010.png)

> [!NOTE]
> Git tıklamadan önce imleç deyiminde yere yerleştirin. Bir ifade üzerinde birden fazla satırı ayırabilirsiniz, ancak boş satırlar bir deyimde koymayın. Boş satırlar, çeşitli ayrı sorgulara penceresinde tutmak için kullanışlı bir yoludur.
>
>

Sütunları Seç, bunları, gruplandırma sütunları, sürükleyin ve sonra da filtreleme:

![Sütun Seçimi sonuçların üst sağ tıklayın](./media/app-insights-analytics-tour/030.png)

Ayrıntıları görmek için herhangi bir öğeyi genişletin:

![Tablo seçin ve yapılandırma sütunlar kullanın](./media/app-insights-analytics-tour/040.png)

> [!NOTE]
> Web tarayıcısında kullanılabilir sonuçları yeniden sıralamak için sütunun başlığına tıklayın. Ancak, bir büyük sonuç kümesi için tarayıcıya indirilen satır sayısı sınırlı olduğunu unutmayın. Bu şekilde sıralama yalnızca döndürülen sonuç kümesini sıralar ve her zaman gerçek yüksek veya en düşük öğelerini göstermiyor. Öğeleri güvenilir bir şekilde sıralamak için kullanmak `top` veya `sort` işleci.
>
>

## <a name="query-across-applications"></a>Uygulamalar arasında sorgu
Birden fazla Application Insights uygulamadan veri birleştirmek istiyorsanız, kullanın **uygulama** anahtar sözcüğünü uygulama tablo adıyla birlikte belirtin.  Bu sorguyu kullanarak iki farklı uygulama isteklerinden birleştirir **birleşim** komutu.


```AIQL

    union app('fabrikamstage').requests, app('fabrikamprod').requests
    
```

## <a name="tophttpsdocsloganalyticsiodocslanguage-referencetabular-operatorstop-operator-and-sorthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssort-operator"></a>[Üst](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/top-operator) ve [sıralama](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator)
`take` hızlı bir örnek bir sonuç almak kullanışlıdır ancak bir tablodaki belirli bir sırada gösterir. Sıralı bir görünüm elde edin, kullanın `top` (için örnek) veya `sort` (üzerinden tüm tablonun).

Belirli bir sütuna göre sıralanmış ilk n satırları göster:

```AIQL

    requests | top 10 by timestamp desc
```

* *Sözdizimi:* çoğu işleçler gibi anahtar sözcüğü parametrelere sahip `by`.
* `desc` azalan düzende = `asc` artan =.

![](./media/app-insights-analytics-tour/260.png)

`top...` Daha fazla yüksek performanslı bildiren yoludur `sort ... | take...`. Biz yazmış:

```AIQL

    requests | sort by timestamp desc | take 10
```

Sonuç aynı olur, ancak biraz daha yavaş çalışır. (Ayrıca yazabilirsiniz `order`, bir diğer adını olduğu `sort`.)

Sütun başlıklarını Tablo görünümünde, ekrandaki sonuçları sıralamak için de kullanılabilir. Ancak Elbette kullandıysanız `take` veya `top` hemen almak için sütun başlığına tıklanırsa, bir tablonun parçası yalnızca alınan kayıtları sıralamasını.

## <a name="wherehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorswhere-operator-filtering-on-a-condition"></a>[Burada](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/where-operator): bir koşula göre filtreleme

Döndürülen belirli Sonuç kodu yalnızca istekleri bakalım:

```AIQL

    requests
    | where resultCode  == "404"
    | take 10
```

![](./media/app-insights-analytics-tour/250.png)

`where` İşleci bir Boole ifadesi alır. Bunlarla ilgili bazı önemli noktalar şunlardır:

* `and`, `or`: Boole işleçleri
* `==`, `<>`, `!=` : eşittir ve eşit değildir
* `=~`, `!~` : büyük küçük harf duyarsız dize eşittir ve eşit değildir. Daha fazla dize karşılaştırma operatörleri çok fazla vardır.

<!---Read all about [scalar expressions]().--->

### <a name="find-unsuccessful-requests"></a>Başarısız istekleri bulun

Bir dize değeri büyük bir tamsayıya dönüştürmek-karşılaştırması:

```AIQL

    requests
    | where isnotempty(resultCode) and toint(resultCode) >= 400
```
<!---
`resultCode` has type string, so we must cast it app-insights-analytics-reference.md#casts for a numeric comparison.
--->

## <a name="time"></a>Zaman

Varsayılan olarak, sorgularınızı son 24 saat kısıtlanır. Ancak, bu aralığı değiştirebilirsiniz:

![](./media/app-insights-analytics-tour/change-time-range.png)

Zaman aralığı geçersiz kılma bahsetmeleri herhangi bir sorgu yazarak `timestamp` where yan tümcesinde. Örneğin:

```AIQL

    // What were the slowest requests over the past 3 days?
    requests
    | where timestamp > ago(3d)  // Override the time range
    | top 5 by duration
```

Zaman aralığı özelliği, 'bir kaynak tabloların her Bahsetme sonra eklenen where' yan tümcesi eşdeğerdir.

`ago(3d)` 'üç gün önce' anlamına gelir. Diğer zaman birimlerini saatleri içerir (`2h`, `2.5h`), dakika (`25m`) ve saniye (`10s`).

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

    // Between specific day/time range
    requests
    | where timestamp > datetime(2018-05-17T17:06:19.892Z) and timestamp <= datetime(2018-05-18T17:06:19.892Z)
    | where duration > 0

```

[Tarihler ve saatlere ilişkin başvuru](https://docs.loganalytics.io/docs/Language-Reference/Data-types/datetime).


## <a name="projecthttpsdocsloganalyticsiodocslanguage-referencetabular-operatorsproject-operator-select-rename-and-compute-columns"></a>[Proje](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator): sütunları işlem seçin ve yeniden adlandırma
Kullanım [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) yalnızca istediğiniz sütunları seçmek için:

```AIQL

    requests | top 10 by timestamp desc
             | project timestamp, name, resultCode
```

![](./media/app-insights-analytics-tour/240.png)

Ayrıca, sütunları yeniden adlandırma ve yenilerini tanımlayın:

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

* Sütun adları, boşluk içerebilir veya varsa bunlar köşeli parantez içindeki semboller şöyle: `['...']` veya `["..."]`
* `%` normal mod işleci olur.
* `1d` (bir basamak biri olan bir sahip ') bir timespan değişmez bir gün anlamına gelir. Daha fazla bazı timespan değişmez değerler şunlardır: `12h`, `30m`, `10s`, `0.01s`.
* `floor` (diğer ad `bin`) bir değer en yakın katına sağladığınız temel değerin aşağı yuvarlar. Bu nedenle `floor(aTime, 1s)` birer en yakın saniye aşağı yuvarlar.

İfadeler, normal tüm işleçleri içerebilir (`+`, `-`,...), ve bir dizi kullanışlı işlevi yoktur.

## <a name="extend"></a>Genişletme
Yalnızca mevcut ayarlara sütunlar eklemek istiyorsanız, kullanın [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator):

```AIQL

    requests
    | top 10 by timestamp desc
    | extend timeOfDay = floor(timestamp % 1d, 1s)
```

Kullanarak [ `extend` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/extend-operator) daha az ayrıntılıdır [ `project` ](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/project-operator) var olan tüm sütunları tutmak istiyorsanız.

### <a name="convert-to-local-time"></a>Yerel saate dönüştürün

Zaman damgaları her zaman UTC biçimindedir. İşiniz üzerinde ABD Pasifik Yakası ve Kış ise, yerel saati UTC-8 saat olacak şekilde, böyle:

```AIQL

    requests
    | top 10 by timestamp desc
    | extend localTime = timestamp - 8h
```

## <a name="summarizehttpsdocsloganalyticsiodocslanguage-referencetabular-operatorssummarize-operator-aggregate-groups-of-rows"></a>[Özetleme](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator): toplam satır grupları
`Summarize` Belirtilen bir geçerlidir *toplama işlevi* satır grupları üzerinden.

Örneğin, web uygulamanız gereken bir isteğe cevap vermesi zaman alanı bildirilir `duration`. Tüm istekler için ortalama yanıt süresi bakalım:

![](./media/app-insights-analytics-tour/410.png)

Veya sonucu farklı adlarının isteklerine ayırabiliriz:

![](./media/app-insights-analytics-tour/420.png)

`Summarize` akıştaki veri noktaları için gruplar halinde toplar `by` yan tümcesi eşit olarak değerlendirir. Her bir değeri `by` ifade - yukarıdaki örnekte her benzersiz işlem adı - sonuç tablosunda bir satıra sonuçlanıyor.

Ya da sonuçları tarafından günün saatini gruplandırabilirsiniz:

![](./media/app-insights-analytics-tour/430.png)

Nasıl kullandığımız fark `bin` işlevi (diğer adıyla `floor`). Yalnızca kullanılan `by timestamp`, her giriş satırı kendi küçük grubunda sonlandırır. Süreleri gibi sürekli bir skaler için ya da sayı sahibiz ayrık değerler yönetilebilir bir sayıya sürekli aralık ayırmak. `bin` -yalnızca bilinen yuvarlama aşağı olduğu `floor` function - bunu yapmanın en kolay yoludur.

Dizeleri aralıklarını azaltmak için aynı tekniği kullanabiliriz:

![](./media/app-insights-analytics-tour/440.png)

Kullanabileceğiniz bildirimi `name=` toplama ifadeler veya yan tarafından bir sonuç sütunun adını ayarlamak için.

## <a name="counting-sampled-data"></a>Verileri sayma örneklenir
`sum(itemCount)` olay sayısı için önerilen toplama olur. Çoğu durumda, ItemCount işlevi yalnızca gruptaki satır sayısını ayarlama ısweekday == 1. Ancak [örnekleme](app-insights-sampling.md) olan işleminde yalnızca özgün olayların bir fraksiyonunu korunur Application ınsights'ta veri noktaları olarak gördüğünüz her veri noktası için böylece `itemCount` olayları.

Örnekleme, özgün olayları ardından ItemCount %75 atar, örneğin, 4 == tutulan kayıtlara - diğer bir deyişle, tutulan her kayıt için vardı dört özgün kaydeder.

Uyarlamalı örnekleme, uygulamanızın yoğun olarak kullanılan zaman dönemlerinde sayınızı ItemCount neden olur.

ItemCount fiyatının, bu nedenle özgün olay sayısı için iyi bir gösterge sağlar.

![](./media/app-insights-analytics-tour/510.png)

Ayrıca bir `count()` toplama (ve sayı işlemi) gerçekten istediğiniz bir grup içindeki satırları sayma durumlar için.

Bir dizi var. [toplama işlevleri](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions).

## <a name="charting-the-results"></a>Sonuç grafiği
```AIQL

    exceptions
       | summarize count=sum(itemCount)  
         by bin(timestamp, 1h)
```

Varsayılan olarak, sonuçları bir tablo olarak görüntülenir:

![](./media/app-insights-analytics-tour/225.png)

Tablo görünümü daha iyi yapabiliriz. Grafik görünümünde sonuçları birlikte dikey göz atalım çubuk seçeneği:

![Grafiği tıklatın ardından dikey çubuk grafiği'ni seçin ve ata x ve y eksenleri](./media/app-insights-analytics-tour/230.png)

Ancak dikkat edin (Tablo görünümünde görebileceğiniz gibi) zamanına göre sonuçları biz sıralamak alamadık, grafik görüntüleme, tarih/saat her zaman doğru sırada gösterir.


## <a name="timecharts"></a>Timecharts
Kaç tane var olduğu her saat olaylardır göster:

```AIQL

    requests
      | summarize event_count=sum(itemCount)
        by bin(timestamp, 1h)
```

Grafik görüntüleme seçeneği belirleyin:

![zaman grafiğini](./media/app-insights-analytics-tour/080.png)

## <a name="multiple-series"></a>Birden fazla seri
Birden çok ifadelerinde `summarize` yan tümcesi birden çok sütun oluşturur.

Birden çok ifadelerinde `by` yan tümcesi birden çok satır değerlerinin her bir birleşimi için bir tane oluşturur.

```AIQL

    requests
    | summarize count_=sum(itemCount), avg(duration)
      by bin(timestamp, 1h), client_StateOrProvince, client_City
    | order by timestamp asc, client_StateOrProvince, client_City
```

![Saat ve konuma göre istekleri tablosu](./media/app-insights-analytics-tour/090.png)

### <a name="segment-a-chart-by-dimensions"></a>Bir grafik boyutlara göre bölme
Bir dize sütunu ve sayısal bir sütun içeren bir tabloda grafik, dize, sayısal veri noktalarını ayrı bir dizi bölmek için kullanılabilir. Birden fazla dize sütunu varsa, ayrıştırıcı olarak kullanılacak sütunu seçebilirsiniz.

![Analytics grafiği segmentlere ayırın.](./media/app-insights-analytics-tour/100.png)

#### <a name="bounce-rate"></a>Geri dönüş oranı

Bir Boole değeri bir ayrıştırıcı olarak kullanılacak bir dizeye dönüştürün:

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

### <a name="display-multiple-metrics"></a>Birden çok ölçümlerini görüntüleme
Zaman damgası yanı sıra sayısal birden fazla sütun içeren bir tabloda grafik, herhangi bir birleşimini görüntüleyebilirsiniz.

![Analytics grafiği segmentlere ayırın.](./media/app-insights-analytics-tour/110.png)

Seçmelisiniz **yoksa bölünmüş** birden çok sayısal sütunları seçmeden önce. Aynı anda birden fazla sayısal sütun görüntüleme olarak bir dize sütunu örneğe göre bölme yapılamaz.

## <a name="daily-average-cycle"></a>Günlük ortalama döngüsü
Ortalama gün içinde kullanım nasıl değişiyor?

Sayısı, zamanın bir gün, modül saatlerine binned istekleri:

```AIQL

    requests
    | where timestamp > ago(30d)  // Override "Last 24h"
    | where tostring(operation_SyntheticSource) == "" // real users
    | extend hour = bin(timestamp % 1d , 1h)
          + datetime("2016-01-01") // Allow render on line chart
    | summarize event_count=sum(itemCount) by hour
```

![Ortalama bir günün saat içeren çizgi grafik](./media/app-insights-analytics-tour/120.png)

> [!NOTE]
> Biz şu anda üzerinde bir çizgi grafiği görüntülemek için tarih saatleri için süreler dönüştürmek zorunda olduğunuza dikkat edin.
>
>

## <a name="compare-multiple-daily-series"></a>Birden fazla günlük seri karşılaştırın
Kullanım nasıl farklı ülkelerde günün saati değişiyor?

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
Kaç oturum vardır farklı uzunluktaki?

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

Son satırı datetime olarak dönüştürme için gereklidir. Şu anda yalnızca bir datetime ise bir grafiğin x ekseninin skalar görüntülenir.

`where` Yan tümcesi dışlar kesin oturumları (sessionDuration == 0) ve x ekseni uzunluğunu ayarlar.

![](./media/app-insights-analytics-tour/290.png)

## <a name="percentileshttpsdocsloganalyticsiodocslanguage-referenceaggregation-functionspercentiles"></a>[Yüzdebirlik değerleri](https://docs.loganalytics.io/docs/Language-Reference/Aggregation-functions/percentiles())
Hangi süreleri aralıkları farklı oturumları yüzdelerini kapsıyor?

Yukarıdaki sorguda kullanın, ancak son satırı değiştirin:

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

Ayrıca üst sınırı nerede kaldırdık birden fazla isteği ile tüm oturumları dahil olmak üzere doğru rakamlarını almak için yan tümcesi:

![Sonuç](./media/app-insights-analytics-tour/180.png)

İçinden görebiliriz:

* oturumlarının %5 3 dakikadan 34s kısa bir süre vardır;
* oturumlarının %50 36 dakikadan kısa bir sürede en son;
* 7 günden fazla %5 oturumları, en son

Her ikisi de aracılığıyla ayrı olarak client_CountryOrRegion sütun getirmek her ülke, sadece biz olması için ayrı bir dökümünü almak için işleçleri özetlenmektedir:

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
İstekler ve özel durumlar dahil olmak üzere birçok tabloları erişimi sahibiz.

Hata yanıtını döndürdü bir isteği ile ilgili özel durumları bulmak için şu tabloları üzerinde katılabilir `operation_Id`:

```AIQL

    requests
    | where toint(resultCode) >= 500
    | join (exceptions) on operation_Id
    | take 30
```


Kullanmak için iyi bir uygulamadır `project` ihtiyacımız birleştirme gerçekleştirmeden önce sütunları seçmek için.
Aynı maddelerinin, biz zaman damgası sütunu yeniden adlandırın.

## <a name="lethttpsdocsloganalyticsiodocslanguage-referencequery-statementslet-statement-assign-a-result-to-a-variable"></a>[İzin](https://docs.loganalytics.io/docs/Language-Reference/Query-statements/Let-statement): sonucu bir değişkene atayın.

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
> Analytics istemcisinde, sorgunun bölümlerini arasında boş satırlar yerleştirmeyin. Tümünün yürüttüğünüzden emin olun.
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
İç içe geçmiş nesnelerde bir kolayca erişilebilir. Örneğin, özel durumlar stream'de yapılandırılmış nesneleri bu gibi görebilirsiniz:

![Sonuç](./media/app-insights-analytics-tour/520.png)

İlgilendiğiniz özellikleri seçerek düzleştirebilirsiniz:

```AIQL

    exceptions | take 10
    | extend method1 = tostring(details[0].parsedStack[1].method)
```

Sonucu uygun türe dönüştürme yapmak gerektiğini unutmayın.


## <a name="custom-properties-and-measurements"></a>Özel özellikler ve ölçümler
Uygulamanızı bağlanıyorsa [özel boyutlar (Özellikler) ve özel ölçümleri](app-insights-api-custom-events-metrics.md#properties) olayları, sonra bunları göreceğiniz `customDimensions` ve `customMeasurements` nesneleri.

Örneğin, uygulamanız varsa:

```csharp

    var dimensions = new Dictionary<string, string>
                     {{"p1", "v1"},{"p2.d2", "v2"}};
    var measurements = new Dictionary<string, double>
                     {{"m1", 42.0}, {"m2", 43.2}};
    telemetryClient.TrackEvent("myEvent", dimensions, measurements);
```

Analytics'te bu değerleri ayıklamak için:

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

### <a name="special-characters"></a>Özel karakterler

Özel karakterler veya dil anahtar sözcükleri adlarında tanımlayıcıları için bunları aracılığıyla erişmeniz gereken `['` ve `']` veya bu adı kullanıyor `["` ve `"]`.

```AIQL

    customEvents
    | extend p2d2 = customDimensions.['p2.d2'], ...
```

[Tanımlayıcı adlandırma kuralları başvurusu](https://docs.loganalytics.io/docs/Learn/References/Naming-principles)

## <a name="dashboards"></a>Panolar
Birlikte, en önemli grafikleri ve tabloları hale getirmek için sonuçları bir panoya sabitleyebilirsiniz.

* [Azure paylaşılan Pano](app-insights-dashboards.md#share-dashboards): Raptiye simgesine tıklayın. Bunu yapmadan önce paylaşılan bir panoyu olması gerekir. Azure portalında açın veya bir pano oluşturun ve Paylaş'ı tıklatın.
* [Power BI Panosu](app-insights-export-power-bi.md): tıklayın dışarı aktarma, Power BI sorgu. Bu alternatif bir avantajı, sorgunuz çok çeşitli diğer sonuçlardan kaynakların yanı sıra görüntüleyebilirsiniz ' dir.

## <a name="combine-with-imported-data"></a>İçeri aktarılan verilerle birleştiriyoruz

Analytics Raporları Panoda harika görünecek ancak bazen daha digestible form verileri çevirmek istediğiniz. Örneğin, kimliği doğrulanmış kullanıcıların telemetriyi bir diğer ad tarafından tanımlanır varsayalım. Sonuç olarak, gerçek adlarını göstermek ister misiniz? Bunu yapmak için diğer adlar gerçek adlarını eşleyen bir CSV dosyası gerekir.

Bir veri dosyası içeri aktarabilir ve bunu gibi standart tablolardan (istekler, özel durumlar vb.) herhangi birini kullanabilirsiniz. Kendi kendine sorgulayın ya da diğer tablolarla katılın. Usermap adlı bir tablonuz ve sütunları vardır, örneğin, `realName` ve `userId`, çevirmek için kullanmayı `user_AuthenticatedId` istek telemetrisi alanındaki:

```AIQL

    requests
    | where notempty(user_AuthenticatedId)
    | project userId = user_AuthenticatedId
      // get the realName field from the usermap table:
    | join kind=leftouter ( usermap ) on userId
      // count transactions by name:
    | summarize count() by realName
```

Bir tablodaki şema dikey pencerenin altında içeri aktarmak için **diğer veri kaynakları**, verilerinizi örneği yükleyerek yeni bir veri kaynağı eklemek için yönergeleri izleyin. Ardından bu tanımı tabloları karşıya yüklemek için kullanabilirsiniz.

Başlangıçta göreceğiniz şekilde içe aktar özelliği şu anda önizlemede, bir "bize başvurun" bağlantısı "Diğer veri kaynakları." altında olan. Bu önizleme programına kaydolmak için kullanın ve bağlantıyı ardından bir "yeni veri kaynağı Ekle" düğmesi ile değiştirilecek.


## <a name="tables"></a>Tablolar
Uygulamanızdan alınan telemetri akışına birçok tabloları erişilebilir. Her tablo için kullanılabilen özellikleri şemasını pencerenin sol tarafında görünür olur.

### <a name="requests-table"></a>İstek tablosu
Web uygulaması ve sayfa adına göre segment sayısı HTTP isteklerine:

![Ada göre segmentlere sayısı istekleri](./media/app-insights-analytics-tour/analytics-count-requests.png)

En çok başarısız olan istekleri bulun:

![Ada göre segmentlere sayısı istekleri](./media/app-insights-analytics-tour/analytics-failed-requests.png)

### <a name="custom-events-table"></a>Özel olaylar tablo
Kullanırsanız [TrackEvent()](app-insights-api-custom-events-metrics.md#trackevent) kendi olayları göndermek için bu tablodan okuyabilirsiniz.

Burada, uygulama kodunuz bu satırları içeren bir örnek alalım:

```csharp

    telemetry.TrackEvent("Query",
       new Dictionary<string,string> {{"query", sqlCmd}},
       new Dictionary<string,double> {
           {"retry", retryCount},
           {"querytime", totalTime}})
```

Bu olayların sıklığı görüntüle:

![Özel olaylar ekran oranı](./media/app-insights-analytics-tour/analytics-custom-events-rate.png)

Ölçüler ve boyutlar, olaylarından ayıklayın:

![Özel olaylar ekran oranı](./media/app-insights-analytics-tour/analytics-custom-events-dimensions.png)

### <a name="custom-metrics-table"></a>Özel ölçümler tablo
Kullanıyorsanız [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) ölçüm kendi değerlerinizi göndermek, sonuçları bulabilirsiniz **customMetrics** akış. Örneğin:  

![Application Insights analytics, özel ölçümler](./media/app-insights-analytics-tour/analytics-custom-metrics.png)

> [!NOTE]
> İçinde [ölçüm Gezgini](app-insights-metrics-explorer.md), telemetri türüne bağlı tüm özel ölçümler birlikte kullanılarak gönderilen ölçümlerle birlikte ölçümler dikey penceresinde görünür `TrackMetric()`. Ancak Analytics, özel ölçümler yine de kendi stream'de TrackMetric tarafından gönderilen ölçümler görünür ancak bunlar - olayları veya istekleri ve benzeri - gerçekleştirilen telemetri hangi tür eklenir.
>
>

### <a name="performance-counters-table"></a>Performans sayaçları tablo
[Performans sayaçları](app-insights-performance-counters.md) gibi CPU, bellek, uygulamanız için temel sistem ölçümlerini Göster ve ağ kullanımı. SDK'sı kendi özel sayaçları dahil olmak üzere ek sayaçları göndermek için yapılandırabilirsiniz.

**PerformanceCounters** şema sunan `category`, `counter` adı ve `instance` her performans sayacının adı. Sayaç örneği adları yalnızca bazı performans sayaçları için geçerlidir ve genellikle sayısı ilişkili olduğu için işlem adını belirtin. Her uygulama için telemetriyi yalnızca bu uygulama için sayaçları görürsünüz. Örneğin, görmek için hangi sayaçları kullanılabilir:

![Application Insights analytics performans sayaçları](./media/app-insights-analytics-tour/analytics-performance-counters.png)

Kullanılabilir belleğin bir grafik seçilen süre içinde almak için:

![Application Insights Analytics bellek zaman grafiğini](./media/app-insights-analytics-tour/analytics-available-memory.png)

Gibi diğer telemetri **performanceCounters** bir sütunda da `cloud_RoleInstance` uygulamanızın üzerinde çalıştığı konak makinenin kimliğini belirtir. Örneğin, farklı makinelerde uygulamanızın performansını karşılaştırmak için şunu yazın:

![Application Insights Analytics rol örneği ile bölümlenmiş performans](./media/app-insights-analytics-tour/analytics-metrics-role-instance.png)

### <a name="exceptions-table"></a>Özel durumlar tablo
[Özel durumlar, uygulamanız tarafından bildirilen](app-insights-asp-net-exceptions.md) bu tabloda kullanılabilir.

Özel durum oluştuğunda uygulamanız işleme HTTP isteğini bulmak için üzerinde operation_ıd Katıl:

![Özel durumlar isteklerle operation_ıd üzerinde katılın](./media/app-insights-analytics-tour/analytics-exception-request.png)

### <a name="browser-timings-table"></a>Tarayıcı zamanlama tablosu
`browserTimings` Kullanıcılarınızın tarayıcılarda toplanan sayfası yükleme verilerini gösterir.

[Uygulamanız için istemci tarafı telemetri ayarlama](app-insights-javascript.md) Bu ölçümler görmek için.

Şemayı içerir [farklı aşamalarında yükleme işlemi sayfayı uzunluklarının gösteren ölçümleri](app-insights-javascript.md#page-load-performance). (Bunlar, kullanıcılarınızın bir sayfa okuma sürenin uzunluğunu göstermediği.)  

Farklı sayfalar popularities Göster ve yükleme süreleri her sayfa için:

![Sayfa yükleme sürelerinin analytics'te](./media/app-insights-analytics-tour/analytics-page-load.png)

### <a name="availability-results-table"></a>Kullanılabilirlik sonuçlar tablosu
`availabilityResults` sonuçları gösterilmektedir, [web testleri](app-insights-monitor-web-app-availability.md). Her bir çalıştırmanın testlerinizin her test konumdan ayrı olarak bildirilir.

![Sayfa yükleme sürelerinin analytics'te](./media/app-insights-analytics-tour/analytics-availability.png)

### <a name="dependencies-table"></a>Bağımlılıkları tablo
Veritabanları ve REST API'leri için uygulamanızı yapar ve diğer TrackDependency() için çağrıları çağrı sonuçlarını içerir. Ayrıca tarayıcısından oluşturulan AJAX çağrıları içerir.

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

Sunucu tarafı bağımlılık sonuçlar her zaman göster `success==False` Application Insights aracısı yüklü değilse. Ancak, diğer veriler doğrudur.

### <a name="traces-table"></a>İzlemeleri tablo
Kullanarak TrackTrace(), uygulamanız tarafından gönderilen telemetriyi içeren veya [diğer günlük altyapılarına](app-insights-asp-net-trace-logs.md).

## <a name="video"></a>Video 

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/123/player] 

Gelişmiş sorgular:

> [!VIDEO https://channel9.msdn.com/Events/Build/2016/P591/player]


## <a name="next-steps"></a>Sonraki adımlar
* [Analytics dil başvurusu](app-insights-analytics-reference.md)
* [SQL-kullanıcıların kural sayfası](https://aka.ms/sql-analytics) en yaygın deyimleri çevirir.

[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]
