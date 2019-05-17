---
title: Azure Veri Gezgini'ni kullanarak zaman serisi verilerini çözümleyin
description: Zaman serisi verileri Azure Veri Gezgini'ni kullanarak bulutta analiz etmeyi öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/07/2019
ms.openlocfilehash: 7415e13a445a73af197362c6cfbd3a865a2fea02
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604056"
---
# <a name="time-series-analysis-in-azure-data-explorer"></a>Azure veri Gezgini'nde zaman serisi analizi

Azure Veri Gezgini (ADX), bulut Hizmetleri veya IOT cihazlardan telemetri verileri koleksiyonu devam eden gerçekleştirir. Bu verileri, hizmet durumu izleme, fiziksel üretim işlemleri ve kullanım eğilimlerini gibi çeşitli içgörüler çözümlenebilir. Analiz, zaman serisi tipik temel desenine göre deseninde bir sapma bulmak için seçilen ölçümleri üzerinde gerçekleştirilir.
ADX oluşturma, düzenleme ve birden çok zaman serisinin analiz için yerel destek içerir. Bu konuda, ADX oluşturmak ve analiz etmek için nasıl kullanıldığını öğreneceksiniz **saniye cinsinden zaman serisi binlerce**, neredeyse gerçek zamanlı izleme çözümleri ve iş akışları etkinleştirme.

## <a name="time-series-creation"></a>Zaman serisi oluşturma

Bu bölümde, çok sayıda basit ve sezgisel kullanarak düzenli zaman serisi oluşturacağız `make-series` işleci ve gerektikçe doldurma eksik değerler.
Zaman serisi analiz ilk adımı, bölüm ve bir dizi zaman serisi özgün telemetri tabloya Dönüştür sağlamaktır. Tablo, genellikle bir zaman damgası sütunu, bağlamsal boyutları ve isteğe bağlı ölçümleri içerir. Boyutlar veri bölümlemek için kullanılır. Hedefi bölüm başına zaman serisi binlerce düzenli zaman aralıklarında oluşturmaktır.

Girdi tablosu *demo_make_series1* rastgele web hizmeti trafiğinin 600 K kayıtları içerir. 10 kayıtları örneği için aşağıdaki komutu kullanın:

```kusto
demo_make_series1 | take 10 
```

Sonuçta elde edilen tabloda bir zaman damgası sütunu, üç bağlamsal boyutları sütun ve ölçüm içerir:

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | Zaman Damgası | BrowserVer | OsVer | Ülke/Bölge |
|   | 2016-08-25 09:12:35.4020000 | Chrome 51.0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:41.1120000 | Chrome 52.0 | Windows 10 |   |
|   | 2016-08-25 09:12:46.2300000 | Chrome 52.0 | Windows 7 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5100000 | Chrome 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:46.5570000 | Chrome 52.0 | Windows 10 | Letonya Cumhuriyeti |
|   | 2016-08-25 09:12:47.0470000 | Chrome 52.0 | Windows 8.1 | Hindistan |
|   | 2016-08-25 09:12:51.3600000 | Chrome 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:12:51.6930000 | Chrome 52.0 | Windows 7 | Hollanda |
|   | 2016-08-25 09:12:56.4240000 | Chrome 52.0 | Windows 10 | Birleşik Krallık |
|   | 2016-08-25 09:13:08.7230000 | Chrome 52.0 | Windows 10 | Hindistan |

Ölçüm olduğundan, biz yalnızca zaman serisi aşağıdaki sorguyu kullanarak işletim sistemi tarafından bölümlenmiş trafiği sayısı kendisini temsil eden bir dizi oluşturabilirsiniz:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| render timechart 
```

- Kullanım [ `make-series` ](/azure/kusto/query/make-seriesoperator) üç zaman serisi, bir dizi oluşturmak için işleç burada:
    - `num=count()`: zaman serisi trafik
    - `range(min_t, max_t, 1h)`: zaman serisi 1 saatlik zaman aralığı (eski ve yeni zaman damgalarının tablo kayıtlarını) depo oluşturuldu
    - `default=0`: normal zaman serisi oluşturmak eksik depo için dolgu yöntemini belirtin. Alternatif olarak [ `series_fill_const()` ](/azure/kusto/query/series-fill-constfunction), [ `series_fill_forward()` ](/azure/kusto/query/series-fill-forwardfunction), [ `series_fill_backward()` ](/azure/kusto/query/series-fill-backwardfunction) ve [ `series_fill_linear()` ](/azure/kusto/query/series-fill-linearfunction) değişiklikleri
    - `byOsVer`: işletim sistemi bölümü
- Gerçek zaman serisi veri yapısı, her saat başına toplanan değerinin sayısal bir dizidir. Kullandığımız `render timechart` görselleştirme için.

Yukarıdaki tabloda, üç bölüm sahibiz. Ayrı zaman serisi oluşturabiliriz: Windows 10 (kırmızı), grafikte görülen her işletim sistemi sürüm 8.1 (yeşil) ve 7 (mavi):

![Zaman serisi bölüm](media/time-series-analysis/time-series-partition.png)

## <a name="time-series-analysis-functions"></a>Zaman serisi analiz işlevleri

Bu bölümde, biz tipik serisi işlevleri işleme gerçekleştirir.
Zaman serisi kümesi oluşturulduktan sonra ADX bulunabilir işlemek ve çözümlemek için işlevleri giderek büyüyen bir listesi destekler [zaman serisi belgeleri](/azure/kusto/query/machine-learning-and-tsa). Biz işleme ve analiz etme zaman serisi birkaç temsili işlevleri açıklanmıştır.

### <a name="filtering"></a>Filtering

Filtreleme, sinyal işleme ve yararlı işleme görevlerini zaman serisi için ortak bir uygulama olan (örneğin, gürültülü sinyal kesintisiz, algılama değiştirme).
- İki genel filtre işlevleri vardır:
    - [`series_fir()`](/azure/kusto/query/series-firfunction): ALANININ filtre uygulanıyor. Ortalama ve farklılaşmayı değişiklik algılama zaman serisinin taşıma basit hesaplaması için kullanılır.
    - [`series_iir()`](/azure/kusto/query/series-iirfunction): IIR filtre uygulanıyor. Üstel Düzeltme ve kümülatif toplamı için kullanılır.
- `Extend` Yeni bir hareketli ortalama dizi ekleyerek ayarlayın zaman serisi 5 depo boyutu (adlı *ma_num*) sorgu için:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| render timechart
```

![Zaman serisi filtreleme](media/time-series-analysis/time-series-filtering.png)

### <a name="regression-analysis"></a>Gerileme analizini

ADX destekleyen doğrusal regresyon analiz eğilimini ve hataların zaman serisi tahmin etmek için bölümlenmiş.
- Kullanım [series_fit_line()](/azure/kusto/query/series-fit-linefunction) zaman serisi Genel eğilim algılama için en iyi satırına sığmayacak.
- Kullanım [series_fit_2lines()](/azure/kusto/query/series-fit-2linesfunction) senaryoları izlemek için yararlı olan eğilim değişiklikleri, temel göre algılamak için.

Örnek `series_fit_line()` ve `series_fit_2lines()` zaman serisi sorguda İşlevler:

```kusto
demo_series2
| extend series_fit_2lines(y), series_fit_line(y)
| render linechart with(xcolumn=x)
```

![Zaman serisi regresyon](media/time-series-analysis/time-series-regression.png)

- Mavi: orijinal zaman serisi
- Yeşil: satır donatılmıştır.
- Kırmızı: iki ekrana sığdırılmış satırları

> [!NOTE]
> İşlevi, doğru bir şekilde (düzeyi değiştirme) bağlantı noktası algıladı.

### <a name="seasonality-detection"></a>Mevsimsellik algılama

Çok sayıda ölçüm dönemsel (düzenli) desenlerini izleyin. Kullanıcı trafiğini bulut Hizmetleri, genellikle gece ve hafta sonu boyunca iş günü, Orta ve düşük geçici olarak yüksek olan günlük ve haftalık desenleri de içerir. Düzenli aralıklarla IOT algılayıcıları ölçü. Sıcaklık, baskısı veya nem gibi fiziksel ölçümleri de mevsimsel davranış gösterebilir.

Aşağıdaki örnek, bir ay (2 saat depo) web hizmeti trafiğinde mevsimsellik algılama uygular:

```kusto
demo_series3
| render timechart 
```

![Zaman serisi mevsimsellik](media/time-series-analysis/time-series-seasonality.png)

- Kullanım [series_periods_detect()](/azure/kusto/query/series-periods-detectfunction) dönemleri zaman serisi içinde otomatik olarak algılamak için. 
- Kullanım [series_periods_validate()](/azure/kusto/query/series-periods-validatefunction) size bir ölçüm belirli ayrı period(s) olmalı ve bunlar mevcut olduğunu doğrulamak istediğimiz biliyorsanız.

> [!NOTE]
> Belirli ayrı nokta mevcut değilse bir anomali öyledir.

```kusto
demo_series3
| project (periods, scores) = series_periods_detect(num, 0., 14d/2h, 2) //to detect the periods in the time series
| mv-expand periods, scores
| extend days=2h*todouble(periods)/1d
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | Dönem | Puanları | gün |
|   | 84 | 0.820622786055595 | 7 |
|   | 12 | 0.764601405803502 | 1 |

İşlevi, günlük ve haftalık mevsimsellik algılar. Hafta sonu günlerini weekdays ' farklı olduğu için günlük, haftalık değerinden puanlar.

### <a name="element-wise-functions"></a>Aralığın öğe düzeyinde çarpımının işlevleri

Zaman serisinde bulunan aritmetik ve mantıksal işlemler gerçekleştirilebilir. Kullanarak [series_subtract()](/azure/kusto/query/series-subtractfunction) biz olan bir kalan zaman serisi, özgün ham ölçüm ve düzleştirilmiş bir arasındaki farkı hesaplamak ve anormallikleri gerçek kalan sinyal arayın:

```kusto
let min_t = toscalar(demo_make_series1 | summarize min(TimeStamp));
let max_t = toscalar(demo_make_series1 | summarize max(TimeStamp));
demo_make_series1
| make-series num=count() default=0 on TimeStamp in range(min_t, max_t, 1h) by OsVer
| extend ma_num=series_fir(num, repeat(1, 5), true, true)
| extend residual_num=series_subtract(num, ma_num) //to calculate residual time series
| where OsVer == "Windows 10"   // filter on Win 10 to visualize a cleaner chart 
| render timechart
```

![Zaman serisi işlem](media/time-series-analysis/time-series-operations.png)

- Mavi: orijinal zaman serisi
- Kırmızı: zaman serisi düzleştirilmiş
- Yeşil: kalan zaman serisi

## <a name="time-series-workflow-at-scale"></a>Zaman serisi iş akışını uygun ölçekte

Aşağıdaki örnekte, bu işlevler anomali algılama için saniye cinsinden zaman serisi binlerce üzerinde ölçekli olarak nasıl çalıştırabilirsiniz gösterir. Bir DB hizmetin okuma sayısı ölçüsünü birkaç örnek telemetri Kayıtları Görüntüle seçeneği için dört gün aşağıdaki sorguyu çalıştırın:

```kusto
demo_many_series1
| take 4 
```

|   |   |   |   |   |   |
| --- | --- | --- | --- | --- | --- |
|   | TIMESTAMP | LOC | anonOp | DB | DataRead |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 262 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 5117853934049630089 | 241 | 0 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | -865998331941149874 | 262 | 279862 |
|   | 2016-09-11 21:00:00.0000000 | Loc 9 | 371921734563783410 | 255 | 0 |

Ve basit istatistikleri:

```kusto
demo_many_series1
| summarize num=count(), min_t=min(TIMESTAMP), max_t=max(TIMESTAMP) 
```

|   |   |   |   |
| --- | --- | --- | --- |
|   | sayı | Min\_t | max\_t |
|   | 2177472 | 2016-09-08 00:00:00.0000000 | 2016-09-11 23:00:00.0000000 |

Okunan ölçüm depo 1 saatlik zaman serisinde oluşturma (toplam dört gün * 24 saat = 96 noktaları), normal deseni dalgalanma sonuçlanır:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h)
| render timechart with(ymin=0) 
```

![Uygun ölçekte zaman serisi](media/time-series-analysis/time-series-at-scale.png)

Anormal desenleri olabilecek farklı örnekleri binlerce arasından tek normal zaman serisinde toplanır olduğundan yukarıdaki yanıltıcıdır, davranıştır. Bu nedenle, bir zaman serisi örnek başına oluştururuz. Bir örneği, Loc (konum), anonOp (işlem) ve DB (belirli bir makine) tarafından tanımlanır.

Ne kadar zaman serisi oluşturabiliriz?

```kusto
demo_many_series1
| summarize by Loc, Op, DB
| count
```

|   |   |
| --- | --- |
|   | Count |
|   | 18339 |

Şimdi, okuma sayısı ölçüsünü 18339 zaman serisi bir dizi oluşturmak için vereceğiz. Eklediğimiz `by` yapma serisi deyimi yan tümcesini doğrusal regresyon uygulayın ve en önemli ölçüde azaltarak olan iki zaman serisi eğilim üst seçin:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc 
| render timechart with(title='Service Traffic Outage for 2 instances (out of 18339)')
```

![Zaman serisi iki üst](media/time-series-analysis/time-series-top-2.png)

Örnekleri görüntüle:

```kusto
let min_t = toscalar(demo_many_series1 | summarize min(TIMESTAMP));  
let max_t = toscalar(demo_many_series1 | summarize max(TIMESTAMP));  
demo_many_series1
| make-series reads=avg(DataRead) on TIMESTAMP in range(min_t, max_t, 1h) by Loc, Op, DB
| extend (rsquare, slope) = series_fit_line(reads)
| top 2 by slope asc
| project Loc, Op, DB, slope 
```

|   |   |   |   |   |
| --- | --- | --- | --- | --- |
|   | LOC | OP | DB | eğimi |
|   | Loc 15 | 37 | 1151 | -102743.910227889 |
|   | Loc 13 | 37 | 1249 | -86303.2334644601 |

İki dakikadan az ADX yakın 20.000 zaman serisi analiz ve okuma sayısı aniden iptal edilen iki olağan dışı zaman serisi algılandı.

ADX hızlı performans ile birlikte bu Gelişmiş Özellikler, zaman serisi analiz için benzersiz ve güçlü bir çözüm sağlar.

## <a name="next-steps"></a>Sonraki adımlar

* Hakkında bilgi edinin [zaman serisi anomali algılama ve tahmin](/azure/data-explorer/anomaly-detection) Azure veri Gezgini'nde.
* Hakkında bilgi edinin [makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) Azure veri Gezgini'nde.