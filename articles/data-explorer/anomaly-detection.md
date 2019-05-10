---
title: Zaman serisi anomali algılama ve Azure veri Gezgini'nde tahmin
description: Anomali algılama ve Azure Veri Gezgini'ni kullanarak tahmin için zaman serisi verilerini analiz etmeyi öğrenin.
author: orspod
ms.author: orspodek
ms.reviewer: jasonh
ms.service: data-explorer
ms.topic: conceptual
ms.date: 04/24/2019
ms.openlocfilehash: f40350129a12c7865051bcae80b74b6f9c069179
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65233533"
---
# <a name="anomaly-detection-and-forecasting-in-azure-data-explorer"></a>Anomali algılama ve Azure veri Gezgini'nde tahmin

Azure Veri Gezgini, bulut Hizmetleri veya IOT cihazlardan telemetri verileri koleksiyonu devam eden gerçekleştirir. Bu veriler, hizmet durumu, fiziksel üretim işlemleri, kullanım eğilimlerini ve yük tahmin izleme gibi çeşitli öngörüleri için analiz edilir. Analiz, zaman serisi tipik normal temel desenine göre ölçüm sapma desenini bulmak için seçilen ölçümleri üzerinde gerçekleştirilir. Azure Veri Gezgini, oluşturma, düzenleme ve birden çok zaman serisinin analiz için yerel destek içerir. Bunu oluşturabilir ve gerçek zamanlı izleme çözümleri ve iş akışları etkinleştirme saniye cinsinden zaman serisi binlerce analiz edin.

Bu makalede Azure Veri Gezgini zaman serisi anomali algılama ve tahmin özellikleri ayrıntılı olarak açıklanmaktadır. İlgili zaman serisi işlevleri, burada özgün her zaman serisi ayrılmış Dönemsel, eğilim, bir güçlü iyi bilinen ayrıştırma modeline dayanır ve kalan bileşenleri. Anomalileri, aykırı değerler kalan bileşende algılandığında, ancak tahmin dönemsel extrapolating tarafından gerçekleştirilir ve bileşenleri eğilim. Azure Veri Gezgini uygulamasını otomatik mevsimsellik algılama, aykırı güçlü analiz ve vektör haline getirilmiş uygulama binlerce işlem zaman serisinin saniyeler içinde temel ayrıştırma modeliyle önemli ölçüde geliştirir.

## <a name="prerequisites"></a>Önkoşullar

Okuma [zaman serisi analiz Azure veri Gezgini'nde](/azure/data-explorer/time-series-analysis) için zaman serisi özelliklerine genel bakış.

## <a name="time-series-decomposition-model"></a>Zaman serisi ayrıştırma modeli

Zaman serisi tahmini ve anomali algılama için Azure Veri Gezgini yerel uygulama iyi bilinen ayrıştırma modeli kullanır. Bu model, düzenli ve eğilim davranışı, hizmet trafiği, bileşen sinyal ve gelecekteki ölçü değerleri tahmin ve anormal olanları algılamak için IOT düzenli ölçümler gibi bildirim beklenen ölçümlerinin zaman serisi için uygulanır. Bu regresyon işlemin, daha önce bilinen dışında varsayılır dönemsel ve eğilim davranışı, zaman serisi dağıtılan rastgele. Ardından, gelecekteki ölçüm değerleri dönemsel ve topluca temel adlı eğilim bileşenleri tahmin eder ve kalan bölümü yoksay. Yalnızca kalan bölümü kullanarak aykırı analize dayalı anormal değerleri de algılayabilir.
Bir ayrıştırma model oluşturmak için işlev kullanın [ `series_decompose()` ](/azure/kusto/query/series-decomposefunction). `series_decompose()` İşlevi zaman serisi kümesini alır ve her zaman serisi, Dönemsel için eğilim, fazlalığı ve temel bileşenlerini otomatik olarak çözer. 

Örneğin, aşağıdaki sorguyu kullanarak trafik bir iç web hizmetinin bozabilir:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (baseline, seasonal, trend, residual) = series_decompose(num, -1, 'linefit')  //  decomposition of a set of time series to seasonal, trend, residual, and baseline (seasonal+trend)
| render timechart with(title='Web app. traffic of a month, decomposition', ysplit=panels)
```

![Zaman serisi ayrıştırma](media/anomaly-detection/series-decompose-timechart.png)

* Orijinal zaman serisi etiketli **num** (kırmızı içinde). 
* İşlevi kullanarak tarafından mevsimsellik otomatik algılama işlemi başlatır [ `series_periods_detect()` ](/azure/kusto/query/series-periods-detectfunction) ve ayıklar **dönemsel** desende (mor).
* Dönemsel desen orijinal zaman serisinden çıkarılır ve bir doğrusal regresyon işlevi kullanarak çalıştırılan [ `series_fit_line()` ](/azure/kusto/query/series-fit-linefunction) bulunacak **eğilim** (mavi) bileşeni.
* Eğilim işlevi çıkarır ve kalan **kalan** (yeşil) bileşeni.
* Son olarak, işlev dönemsel ekler ve eğilim oluşturmak için bileşenleri **temel** (mavi içinde).

## <a name="time-series-anomaly-detection"></a>Zaman serisi anomali algılama

İşlev [ `series_decompose_anomalies()` ](/azure/kusto/query/series-decompose-anomaliesfunction) anormal noktaları zaman serisi kümesine göre bulur. Bu işlev çağrıları `series_decompose()` ayrıştırma model ve ardından çalışmalarını oluşturmak için [ `series_outliers()` ](/azure/kusto/query/series-outliersfunction) kalan bileşen üzerindeki. `series_outliers()` anomali puanları kullanarak Tukey'nın dilimi test kalan bileşenin her noktası hesaplar. Anomali puanları 1.5 yukarıda veya aşağıda-1.5 hafif bir anomali ortaya çıkmasına neden belirtmek veya sırasıyla reddedebilirsiniz. Anomali 3.0 yukarıda veya aşağıda-3.0 güçlü bir anomali puanlar. 

Aşağıdaki sorgu iç web hizmeti trafiğinin anormallikleri sağlar:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend (anomalies, score, baseline) = series_decompose_anomalies(num, 1.5, -1, 'linefit')
| render anomalychart with(anomalycolumns=anomalies, title='Web app. traffic of a month, anomalies') //use "| render anomalychart with anomalycolumns=anomalies" to render the anomalies as bold points on the series charts.
```

![Zaman serisi anomali algılama](media/anomaly-detection/series-anomaly-detection.png)

* Özgün zaman serisinde (kırmızı). 
* Temel (dönemsel + eğilimi) (mavi) bileşeni.
* Anormal noktaları (mor) özgün zaman serisi üstünde. Anormal noktaları beklenen temel düzey değerleri önemli ölçüde sapma.

## <a name="time-series-forecasting"></a>Zaman serilerini tahmin etme

İşlev [ `series_decompose_forecast()` ](/azure/kusto/query/series-decompose-forecastfunction) gelecekteki değerlerini bir dizi zaman serisi tahmin eder. Bu işlev çağrıları `series_decompose()` model ve daha sonra için her zaman serisi ayrıştırma oluşturmak için temel bileşen geleceğe extrapolates.

Aşağıdaki sorguda sonraki haftanın web hizmeti trafiğinin tahmin sağlar:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid 
| where sid == 'TS1'   //  select a single time series for a cleaner visualization
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week by Time Series Decmposition')
```

![Zaman serilerini tahmin etme](media/anomaly-detection/series-forecasting.png)

* Özgün ölçümü (kırmızı). Gelecekteki değerleri eksik ve varsayılan olarak 0 olarak ayarlayın.
* Temel bileşeni (mavi), sonraki haftanın değerleri tahmin etmek için tahmin.

## <a name="scalability"></a>Ölçeklenebilirlik

Azure Veri Gezgini sorgu dili sözdizimi birden çok zaman serisi işlemek tek bir çağrı sağlar. Benzersiz en iyi duruma getirilmiş uygulanması etkili anomali algılama ve binlerce gerçek zamanlıya yakın senaryolar sayaçları izlerken tahmin için kritik olan hızlı performans sağlar.

Aşağıdaki sorgu, aynı anda üç zaman serisi işlenmesini gösterir:

```kusto
let min_t = datetime(2017-01-05);
let max_t = datetime(2017-02-03 22:00);
let dt = 2h;
let horizon=7d;
demo_make_series2
| make-series num=avg(num) on TimeStamp from min_t to max_t+horizon step dt by sid
| extend offset=case(sid=='TS3', 4000000, sid=='TS2', 2000000, 0)   //  add artificial offset for easy visualization of multiple time series
| extend num=series_add(num, offset)
| extend forecast = series_decompose_forecast(num, toint(horizon/dt))
| render timechart with(title='Web app. traffic of a month, forecasting the next week for 3 time series')
```

![Zaman serisi ölçeklenebilirlik](media/anomaly-detection/series-scalability.png)

## <a name="summary"></a>Özet

Bu belge, zaman serisi anomali algılama ve tahmin için yerel bir Azure Veri Gezgini işlevleri ayrıntıları. Özgün her zaman serisi, anomali algılama ve/veya tahmin Dönemsel, eğilim ve fazlalığı bileşenleri içinde ayrılmış. Bu işlevler için gerçek zamanlıya yakın izleme senaryolar, hata algılama, Tahmine dayalı Bakım ve isteğe bağlı gibi kullanılabilir ve tahmin yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [makine öğrenimi özellikleri](/azure/data-explorer/machine-learning-clustering) Azure veri Gezgini'nde.
