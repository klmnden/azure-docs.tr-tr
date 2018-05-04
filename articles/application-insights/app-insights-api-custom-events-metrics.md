---
title: Özel olayları ve ölçümleri için Application Insights API'si | Microsoft Docs
description: Birkaç satır kod, cihaz veya masaüstü uygulaması, Web sayfası veya kullanımı izlemek ve sorunlarını tanılamak için hizmetinizi ekleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/17/2017
ms.author: mbullwin
ms.openlocfilehash: e93b3348c933f65067114bfce4ac517f1204af34
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Özel olayları ve ölçümleri için Application Insights API'si

Birkaç satır kod uygulamanızda kullanıcıların ne ile yaptıklarını bulmak için veya sorunları tanılamak için ekleyin. Aygıt ve Masaüstü uygulamaları, web istemcileri ve web sunucuları telemetri gönderebilir. Kullanım [Azure Application Insights](app-insights-overview.md) çekirdek özel olayları ve ölçümleri ve kendi sürümleri standart telemetri göndermeyi telemetri API'si. Bu API standart Application Insights veri toplayıcıları kullanan aynı API'dir.

## <a name="api-summary"></a>API özeti
Birkaç küçük Çeşitlemeler dışında tüm platformlar genelinde Tekdüzen API'dir.

| Yöntem | İçin kullanılır |
| --- | --- |
| [`TrackPageView`](#page-views) |Sayfalar, ekranlar, dikey veya formlar. |
| [`TrackEvent`](#trackevent) |Kullanıcı eylemleri ve diğer olaylar. Kullanıcı davranışı izlemek veya performansını izlemek için kullanılır. |
| [`TrackMetric`](#trackmetric) |Performans ölçümleri gibi sıra uzunlukları belirli olayları ile ilgili değildir. |
| [`TrackException`](#trackexception) |Tanılama için günlük özel durumları. İzleme burada bunlar ile ilgili diğer olaylar oluşur ve Yığın izlemeleri inceleyin. |
| [`TrackRequest`](#trackrequest) |Sunucu istek süresi Performans Analizi ve sıklığı günlüğü. |
| [`TrackTrace`](#tracktrace) |Tanılama günlük iletileri. Üçüncü taraf günlükleri de yakalayabilirsiniz. |
| [`TrackDependency`](#trackdependency) |Uygulamanızı bağlıdır dış bileşenlere çağrılar sıklığı ve süresi günlüğü. |

Yapabilecekleriniz [özellikleri ve ölçümleri ekleme](#properties) bu telemetri çağrılardan bir çoğuna.

## <a name="prep"></a>Başlamadan önce
Application Insights SDK'sı üzerinde bir başvuru henüz yoksa:

* Application Insights SDK'sı projenize ekleyin:

  * [ASP.NET projesi](app-insights-asp-net.md)
  * [Java projesi](app-insights-java-get-started.md)
  * [Node.js projesi](app-insights-nodejs.md)
  * [Her Web sayfasındaki JavaScript](app-insights-javascript.md) 
* Cihaz veya web sunucusu kodunuzda şunları içerir:

    *C# ' TA:* `using Microsoft.ApplicationInsights;`

    *Visual Basic:* `Imports Microsoft.ApplicationInsights`

    *Java:* `import com.microsoft.applicationinsights.TelemetryClient;`
    
    *Node.js:* `var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>Bir TelemetryClient örneği Al
Bir örneğini almak `TelemetryClient` (Web sayfalarındaki JavaScript'te hariç):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();
    
*Node.js*

    var telemetry = applicationInsights.defaultClient;


TelemetryClient iş parçacığı güvenlidir.

ASP.NET ve Java projeleri için gelen HTTP isteklerini otomatik olarak yakalanır. Uygulamanızın başka bir modül için TelemetryClient ek örneklerini oluşturmak isteyebilirsiniz. Örneğin, rapor iş mantığı olaylarını ara yazılım sınıfı bir TelemetryClient örneği olabilir. Kullanıcı kimliği ve DeviceID makine tanımlamak için gibi özellikleri ayarlayabilirsiniz. Bu bilgiler örneği gönderdiği tüm olayları eklenir. 

*C#*

    TelemetryClient.Context.User.Id = "...";
    TelemetryClient.Context.Device.Id = "...";

*Java*

    telemetry.getContext().getUser().setId("...);
    telemetry.getContext().getDevice().setId("...");

Node.js projelerinde kullandığınız `new applicationInsights.TelemetryClient(instrumentationKey?)` yeni bir örneği, ancak bu oluşturmak için tekli yalıtılmış yapılandırmasından gerektiren senaryolar için önerilir `defaultClient`.

## <a name="trackevent"></a>TrackEvent
Application ınsights'ta bir *özel olay* içinde görüntüleyebilirsiniz bir veri noktası [ölçüm Gezgini](app-insights-metrics-explorer.md) toplanan sayılara olarak ve buna [tanılama arama](app-insights-diagnostic-search.md) olarak tek tek yineleme. (Bu MVC veya diğer framework "olayları." ile ilişkili değil)

INSERT `TrackEvent` çeşitli olaylarının sayılacağı kodunuzda çağırır. Ne sıklıkla kullanıcıların belirli bir özellik, ne sıklıkta bunlar belirli hedeflere ulaşmak veya hatalar belirli tür belki ne sıklıkta yaptıkları seçin.

Örneğin, bir oyun uygulamada bir kullanıcı oyun WINS bir olay gönderebilir:

*JavaScript*

    appInsights.trackEvent("WinGame");

*C#*

    telemetry.TrackEvent("WinGame");

*Visual Basic*

    telemetry.TrackEvent("WinGame")

*Java*

    telemetry.trackEvent("WinGame");
    
*Node.js*

    telemetry.trackEvent({name: "WinGame"});

### <a name="view-your-events-in-the-microsoft-azure-portal"></a>Microsoft Azure Portalı'nda, olayları görüntülemek
Olaylarınızı sayısını görmek için açık bir [ölçüm Gezgini](app-insights-metrics-explorer.md) dikey penceresinde, yeni bir grafik ekleyin ve seçin **olayları**.  

![Özel olaylar sayısını bakın](./media/app-insights-api-custom-events-metrics/01-custom.png)

Farklı olayları sayısı karşılaştırmak için grafik türünü ayarlamak **kılavuz**ve olay adı tarafından Grup:

![Gruplandırma ve grafik türünü ayarlayın](./media/app-insights-api-custom-events-metrics/07-grid.png)

Kılavuzda, bu olay oluşumlarını görmek için bir olay adı tıklayın. Daha fazla ayrıntı - listedeki olayı tıklatın.

![Aracılığıyla olaylarını incelemek](./media/app-insights-api-custom-events-metrics/03-instances.png)

Arama veya ölçüm Gezgini belirli olayları odaklanmak için dikey 's filtresi ilgilendiğiniz olay adları ayarla:

![Filtreler açın, olay adı'nı genişletin ve bir veya daha fazla değerleri seçin](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Analytics'te özel olaylar

Telemetriyi kullanılabilir `customEvents` tablosundaki [uygulama Öngörüler Analytics](app-insights-analytics.md). Her satır için bir çağrı temsil eden `trackEvent(..)` uygulamanızda. 

Varsa [örnekleme](app-insights-sampling.md) içinde ItemCount özelliği 1'den büyük bir değer gösterir işlemdir. Örnek ItemCount == trackEvent() için 10 çağrıları, örnekleme işlemi yalnızca bunlardan birinin aktarılan 10 anlamına gelir. Özel olaylar doğru sayısını almak için bu nedenle kullanım kodu gibi kullanmalısınız `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Application Insights belirli olaylara bağlı olmayan ölçümleri grafik. Örneğin, bir kuyruk uzunluğu düzenli aralıklarla izlemek. Ölçümleri, bağımsız değişkenleri ve eğilimleri daha az ilgi ölçümlerdir ve böylece istatistiksel grafikler kullanışlıdır.

Ölçümleri Application Insights'a gönderme için kullanabileceğiniz `TrackMetric(..)` API. Ölçüm göndermek için iki yolu vardır: 

* Tek bir değer. Uygulamanızda bir ölçüm gerçekleştirdiğiniz her zaman, karşılık gelen değerle Application Insights'a gönderme. Örneğin, bir kapsayıcı öğe sayısını açıklayan bir ölçüm olduğunu varsayalım. Belirli bir zaman diliminde ilk üç öğeleri kapsayıcıya yerleştirdiğiniz ve ardından iki öğeyi kaldırın. Buna göre çağırırdı `TrackMetric` iki kez: ilk değer geçirme `3` ve ardından değeri `-2`. Application Insights, sizin adınıza her iki değerin de depolar. 

* Toplama. Ölçümleri ile çalışırken, her tek nadiren ilgi ölçüsüdür. Bunun yerine belirli bir süre içinde ne Özet önemlidir. Bu tür bir özeti adlı _toplama_. Yukarıdaki örnekte, toplama ölçüm bu döneme ait toplamıdır `1` ve ölçüm değerlerin sayısını `2`. Toplama yaklaşım kullanırken, yalnızca çağırma `TrackMetric` zaman süresi başına bir kez ve toplam değerler gönderin. Bu önemli ölçüde performans ve maliyet ek yükü daha az veri noktası Application Insights'a hala ilgili tüm bilgileri toplanırken göndererek küçültebilirsiniz beri önerilen yaklaşımdır.

### <a name="examples"></a>Örnekler:

#### <a name="single-values"></a>Tek değer

Tek bir ölçü değeri göndermek için:

*JavaScript*

 ```Javascript
     appInsights.trackMetric("queueLength", 42.0);
 ```

*C#*

```csharp
    var sample = new MetricTelemetry();
    sample.Name = "metric name";
    sample.Value = 42.3;
    telemetryClient.TrackMetric(sample);
```

*Java*

```Java
    
    telemetry.trackMetric("queueLength", 42.0);

```

*Node.js*

 ```Javascript
     telemetry.trackMetric({name: "queueLength", value: 42.0});
 ```

#### <a name="aggregating-metrics"></a>Ölçümleri toplama

Birleşik ölçümleri uygulamanızdan göndermeden önce önerilir, bant genişliğini azaltmak için maliyet ve performansı artırmak için.
Kod bir araya getirildiği bir örneği burada verilmiştir:

*C#*

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

namespace MetricAggregationExample
{
    /// <summary>
    /// Aggregates metric values for a single time period.
    /// </summary>
    internal class MetricAggregator
    {
        private SpinLock _trackLock = new SpinLock();

        public DateTimeOffset StartTimestamp    { get; }
        public int Count                        { get; private set; }
        public double Sum                       { get; private set; }
        public double SumOfSquares              { get; private set; }
        public double Min                       { get; private set; }
        public double Max                       { get; private set; }
        public double Average                   { get { return (Count == 0) ? 0 : (Sum / Count); } }
        public double Variance                  { get { return (Count == 0) ? 0 : (SumOfSquares / Count)
                                                                                  - (Average * Average); } }
        public double StandardDeviation         { get { return Math.Sqrt(Variance); } }

        public MetricAggregator(DateTimeOffset startTimestamp)
        {
            this.StartTimestamp = startTimestamp;
        }

        public void TrackValue(double value)
        {
            bool lockAcquired = false;

            try
            {
                _trackLock.Enter(ref lockAcquired);

                if ((Count == 0) || (value < Min))  { Min = value; }
                if ((Count == 0) || (value > Max))  { Max = value; }
                Count++;
                Sum += value;
                SumOfSquares += value * value;
            }
            finally
            {
                if (lockAcquired)
                {
                    _trackLock.Exit();
                }
            }
        }
    }   // internal class MetricAggregator

    /// <summary>
    /// Accepts metric values and sends the aggregated values at 1-minute intervals.
    /// </summary>
    public sealed class Metric : IDisposable
    {
        private static readonly TimeSpan AggregationPeriod = TimeSpan.FromSeconds(60);

        private bool _isDisposed = false;
        private MetricAggregator _aggregator = null;
        private readonly TelemetryClient _telemetryClient;

        public string Name { get; }

        public Metric(string name, TelemetryClient telemetryClient)
        {
            this.Name = name ?? "null";
            this._aggregator = new MetricAggregator(DateTimeOffset.UtcNow);
            this._telemetryClient = telemetryClient ?? throw new ArgumentNullException(nameof(telemetryClient));

            Task.Run(this.AggregatorLoopAsync);
        }

        public void TrackValue(double value)
        {
            MetricAggregator currAggregator = _aggregator;
            if (currAggregator != null)
            {
                currAggregator.TrackValue(value);
            }
        }

        private async Task AggregatorLoopAsync()
        {
            while (_isDisposed == false)
            {
                try
                {
                    // Wait for end end of the aggregation period:
                    await Task.Delay(AggregationPeriod).ConfigureAwait(continueOnCapturedContext: false);

                    // Atomically snap the current aggregation:
                    MetricAggregator nextAggregator = new MetricAggregator(DateTimeOffset.UtcNow);
                    MetricAggregator prevAggregator = Interlocked.Exchange(ref _aggregator, nextAggregator);

                    // Only send anything is at least one value was measured:
                    if (prevAggregator != null && prevAggregator.Count > 0)
                    {
                        // Compute the actual aggregation period length:
                        TimeSpan aggPeriod = nextAggregator.StartTimestamp - prevAggregator.StartTimestamp;
                        if (aggPeriod.TotalMilliseconds < 1)
                        {
                            aggPeriod = TimeSpan.FromMilliseconds(1);
                        }

                        // Construct the metric telemetry item and send:
                        var aggregatedMetricTelemetry = new MetricTelemetry(
                                Name,
                                prevAggregator.Count,
                                prevAggregator.Sum,
                                prevAggregator.Min,
                                prevAggregator.Max,
                                prevAggregator.StandardDeviation);
                        aggregatedMetricTelemetry.Properties["AggregationPeriod"] = aggPeriod.ToString("c");

                        _telemetryClient.Track(aggregatedMetricTelemetry);
                    }
                }
                catch(Exception ex)
                {
                    // log ex as appropriate for your application
                }
            }
        }

        void IDisposable.Dispose()
        {
            _isDisposed = true;
            _aggregator = null;
        }
    }   // public sealed class Metric
}
```

### <a name="custom-metrics-in-metrics-explorer"></a>Ölçümleri Explorer'da özel ölçümleri

Sonuçları görmek için ölçümleri Gezgini'ni açın ve yeni bir grafik ekleyin. Ölçüm göstermek için grafik düzenleyin.

> [!NOTE]
> Özel ölçüm, kullanılabilir ölçümlere listesinde görünmesi birkaç dakika sürebilir.
>

![Yeni bir grafik ekleyin veya bir grafik seçin ve özel altında ölçüm seçin](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Analytics'te özel ölçümleri

Telemetriyi kullanılabilir `customMetrics` tablosundaki [uygulama Öngörüler Analytics](app-insights-analytics.md). Her satır için bir çağrı temsil eden `trackMetric(..)` uygulamanızda.
* `valueSum` -Bu ölçümler toplamıdır. Ortalama değer almak için bölün `valueCount`.
* `valueCount` -Bu toplanan ölçümleri sayısını `trackMetric(..)` çağırın.

## <a name="page-views"></a>Sayfa görüntülemeleri
Her ekranı veya sayfa yüklendiğinde, bir aygıt veya Web sayfası uygulamasında varsayılan olarak sayfa görünümü telemetrisi gönderilir. Ancak, sayfa görünümleri ek veya farklı zamanlarda izlemek için değiştirebilirsiniz. Örneğin, sekmeler veya dikey pencereler görüntüleyen bir uygulama, kullanıcının yeni bir dikey pencere açıldığında bir sayfayı izlemek isteyebilirsiniz.

![Genel Bakış dikey penceresinde kullanım Mercek](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Sayfa görünümü telemetrisi olduğunda kullanıcı ve oturum grafikleri Canlı gelmesi için kullanıcı ve oturum verilerini sayfa görünümleri birlikte özellikleri olarak gönderilir.

### <a name="custom-page-views"></a>Özel sayfa görünümleri
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Java*

    telemetry.trackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Farklı HTML sayfaları içinde birden çok sekme varsa, URL çok belirtebilirsiniz:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Zamanlama sayfası görünümleri
Varsayılan olarak, kez bildirildi **sayfa görünümü yükleme süresi** tarayıcı isteği gönderdiğinde tarayıcının sayfası yük olayı çağrılıncaya kadar gelen ölçülür.

Bunun yerine, şunlardan birini yapabilirsiniz:

* Açık bir süre kümesinde [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) çağırın: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Çağrıları zamanlama sayfası görünümü kullanmak `startTrackPage` ve `stopTrackPage`.

*JavaScript*

    // To start timing a page:
    appInsights.startTrackPage("Page1");

...

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

İlk parametre olarak kullandığınız adı başlatma ve durdurma çağrıları ilişkilendirir. Geçerli sayfa adı varsayar.

Ölçümleri Gezgini'nde görüntülenen sonuç sayfa yükleme süreleri başlatma ve durdurma çağrıları arasındaki aralığı türetilir. Bu size gerçekten zaman hangi aralığı bağlıdır.

### <a name="page-telemetry-in-analytics"></a>Sayfa telemetri analizi

İçinde [Analytics](app-insights-analytics.md) iki tablo tarayıcı işlemleri verileri göster:

* `pageViews` Tablosu URL ve sayfa başlığını hakkındaki verileri içerir
* `browserTimings` Tablosu gelen verileri işlemek için geçen süre gibi istemci performansı hakkında veri içerir

Tarayıcı farklı sayfaları işlemek için gereken süreyi bulmak için:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

Farklı tarayıcılar popularities bulmak için:

```
pageViews | summarize count() by client_Browser
```

Sayfa görünümleri AJAX çağrıları ilişkilendirilecek bağımlılıklarla Katıl:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
SDK sunucusu, HTTP isteklerini günlüğe kaydetmek için TrackRequest kullanır.

İstekleri bir bağlamda benzetimini yapmak istiyorsanız, ayrıca kendiniz çalışan web hizmeti modülü burada yok çağırabilirsiniz.

Ancak, istek telemetri göndermek için önerilen burada isteği görevi gören yoludur bir <a href="#operation-context">işlemi bağlam</a>.

## <a name="operation-context"></a>İşlem bağlamı
İşlem bağlamı ile ilişkilendirerek telemetri öğeleri birlikte ilişkilendirebilirsiniz. Standart isteği izleme modülü, özel durumlar ve bir HTTP istek gerçekleştirilirken gönderilen diğer olayları için bunu yapar. İçinde [arama](app-insights-diagnostic-search.md) ve [Analytics](app-insights-analytics.md), kendi işlemi kimliği kullanarak istek ile ilişkili herhangi bir olayı kolayca bulabilirsiniz

Bkz: [Telemetri bağıntı Application ınsights'ta](application-insights-correlation.md) bağıntı hakkında daha fazla ayrıntı için.

Telemetri el ile izlerken bu yöntemi kullanarak telemetri bağıntı emin olmak için en kolay yolu:

*C#*

```csharp
// Establish an operation context and associated telemetry item:
using (var operation = telemetryClient.StartOperation<RequestTelemetry>("operationName"))
{
    // Telemetry sent in here will use the same operation ID.
    ...
    telemetryClient.TrackTrace(...); // or other Track* calls
    ...
    // Set properties of containing telemetry item--for example:
    operation.Telemetry.ResponseCode = "200";

    // Optional: explicitly send telemetry item:
    telemetryClient.StopOperation(operation);

} // When operation is disposed, telemetry item is sent.
```

Bir işlemin bağlamını ayarlanması birlikte `StartOperation` telemetri öğesi, belirttiğiniz türü oluşturur. İşlemi çıkardığınızda veya açıkça çağırırsanız telemetri öğesi gönderir `StopOperation`. Kullanırsanız `RequestTelemetry` telemetri türü olarak başlatma ve durdurma arasında zaman aralıklarında süresinin ayarlanır.

Bir işlem kapsamı içinde bildirilen telemetri öğeler 'alt' gibi işlem haline gelir. İşlem bağlamı iç içe geçmiş. 

Aramada, işlem bağlamı oluşturmak için kullanılan **ilgili öğeler** listesi:

![İlgili öğeler](./media/app-insights-api-custom-events-metrics/21.png)

Bkz: [izlemek Application Insights .NET SDK ile özel işlemler](application-insights-custom-operations-tracking.md) izleme özel işlemler hakkında daha fazla bilgi için.

### <a name="requests-in-analytics"></a>Analytics istekleri 

İçinde [uygulama Öngörüler Analytics](app-insights-analytics.md), içinde Göster istekleri `requests` tablo.

Varsa [örnekleme](app-insights-sampling.md) olduğundan, işlem ItemCount özelliği bir değer 1'den büyük gösterir. Örnek ItemCount == trackRequest() için 10 çağrıları, örnekleme işlemi yalnızca bunlardan birinin aktarılan 10 anlamına gelir. İstek ve istek adlarıyla kesimli ortalama süresi doğru sayısı almak için kodu gibi kullanın:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Özel durumlar Application Insights'a gönderme:

* İçin [saymanız](app-insights-metrics-explorer.md), bir sorun sıklığını belirtisi olarak.
* İçin [ayrı ayrı oluşumları inceleyin](app-insights-diagnostic-search.md).

Raporları Yığın izlemeleri içerir.

*C#*

    try
    {
        ...
    }
    catch (Exception ex)
    {
       telemetry.TrackException(ex);
    }

*Java*

    try {
        ...
    } catch (Exception ex) {
        telemetry.trackException(ex);
    }

*JavaScript*

    try
    {
       ...
    }
    catch (ex)
    {
       appInsights.trackException(ex);
    }
    
*Node.js*

    try
    {
       ...
    }
    catch (ex)
    {
       telemetry.trackException({exception: ex});
    }

Her zaman TrackException açıkça çağırın zorunda kalmamak için SDK'ları birçok özel durumlarını otomatik olarak yakalama.

* ASP.NET: [özel durumları yakalamak için kod yazma](app-insights-asp-net-exceptions.md).
* J2EE: [özel durumları yakalanan otomatik olarak](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Özel durumları otomatik olarak yakalanır. Otomatik olarak toplanmasını devre dışı bırakmak istiyorsanız, bir satırı, Web sayfalarındaki Ekle kod parçacığını ekleyin:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Analytics özel durumları

İçinde [uygulama Öngörüler Analytics](app-insights-analytics.md), özel durumlar gösterilmiyor `exceptions` tablo.

Varsa [örnekleme](app-insights-sampling.md) işlem içinde `itemCount` özelliği değeri 1'den büyük gösterir. Örnek ItemCount == 10 trackException() çağrıları ekleme, örnekleme işlemi yalnızca bunlardan birinin aktarılan 10 anlamına gelir. Özel durum türüne göre kesimli özel durumlarının doğru bir sayıyı almak için kodu gibi kullanın:

```
exceptions | summarize sum(itemCount) by type
```

Önemli yığın bilgilerin çoğunu ayrı değişkenlere zaten ayıklanan ancak birbirinden çekme `details` daha fazlasına yapısı. Bu yapı dinamik olduğundan, beklediğiniz türü sonucu atamalısınız. Örneğin:

```AIQL
exceptions
| extend method2 = tostring(details[0].parsedStack[1].method)
```

Özel durumlar ilgili isteklerinde ile ilişkilendirilecek bir birleşim kullanın:

```
exceptions
| join (requests) on operation_Id 
```

## <a name="tracktrace"></a>TrackTrace
Application Insights "içerik haritası Kılavuzu" göndererek sorunların tanılanmasına yardımcı olmak için TrackTrace kullanın. Tanılama veri öbekleri göndermek ve bunları inceleyin [tanılama arama](app-insights-diagnostic-search.md).

.NET içinde [oturum bağdaştırıcıları](app-insights-asp-net-trace-logs.md) portalına üçüncü taraf günlükleri göndermek için bu API'yi kullanın.

Java için [standart günlükçüleri ister Log4J, Logback](app-insights-java-trace-logs.md) portalına üçüncü taraf günlükleri göndermek için uygulama Öngörüler Log4j veya Logback Appenders kullanın.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);

*Java*

    telemetry.trackTrace(message, SeverityLevel.Warning, properties);
    
*Node.js*

    telemetry.trackTrace({message: message, severity:applicationInsights.Contracts.SeverityLevel.Warning, properties:properties});


İleti içeriği arama yapabilirsiniz ancak (özellik değerlerini), üzerinde filtre uygulayamazsınız.

Boyut sınırını `message` özellikleri sınırından daha yüksektir.
TrackTrace avantajı, iletide oldukça uzun veri koyabilirsiniz ' dir. Örneğin, POST verilerini şifreleyebilirsiniz.  

Ayrıca, bir önem düzeyi iletinize ekleyebilirsiniz. Ve diğer telemetri gibi filtre veya arama izlemeleri farklı kümeleri için yardımcı olmak için özellik değerlerini ekleyebilirsiniz. Örneğin:

*C#*

```C#
    var telemetry = new Microsoft.ApplicationInsights.TelemetryClient();
    telemetry.TrackTrace("Slow database response",
                   SeverityLevel.Warning,
                   new Dictionary<string,string> { {"database", db.ID} });
```

*Java*

```Java

    Map<String, Integer> properties = new HashMap<>();
    properties.put("Database", db.ID);
    telemetry.trackTrace("Slow Database response", SeverityLevel.Warning, properties);

```

İçinde [arama](app-insights-diagnostic-search.md), daha sonra kolayca belirli bir veritabanı ile ilgili tüm iletileri belirli önem düzeyine sahip bir filtre.


### <a name="traces-in-analytics"></a>İçindeki analizi

İçinde [uygulama Öngörüler Analytics](app-insights-analytics.md), TrackTrace çağrıları gösterilmiyor `traces` tablo.

Varsa [örnekleme](app-insights-sampling.md) içinde ItemCount özelliği 1'den büyük bir değer gösterir işlemdir. Örnek ItemCount == 10, 10 çağrı anlamına gelir `trackTrace()`, örnekleme işlemi yalnızca bunlardan birinin aktarılan. İzleme çağrıları doğru sayısını almak için bu nedenle kod gibi kullanmalısınız `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
TrackDependency çağrısı bir dış kod parçası, yapılan çağrıların başarı oranları ve yanıt sürelerini izlemek için kullanın. Sonuçlar portalında bağımlılık grafiklerinde görüntülenir.

*C#*

```csharp
var success = false;
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
try
{
    success = dependency.Call();
}
finally
{
    timer.Stop();
    telemetry.TrackDependency("myDependency", "myCall", startTime, timer.Elapsed, success);
}
```

*Java*

```Java
    boolean success = false;
    long startTime = System.currentTimeMillis();
    try {
        success = dependency.call();
    }
    finally {
        long endTime = System.currentTimeMillis();
        long delta = endTime - startTime;
        RemoteDependencyTelemetry dependencyTelemetry = new RemoteDependencyTelemetry("My Dependency", "myCall", delta, success);
        telemetry.setTimeStamp(startTime);
        telemetry.trackDependency(dependencyTelemetry);
    }

```

*JavaScript*

```Javascript
var success = false;
var startTime = new Date().getTime();
try
{
    success = dependency.Call();
}
finally
{
    var elapsed = new Date() - startTime;
    telemetry.trackDependency({dependencyTypeName: "myDependency", name: "myCall", duration: elapsed, success:success});
}
```

Sunucu SDK'ları içerdiğini unutmayın bir [bağımlılık Modülü](app-insights-asp-net-dependencies.md) bulur ve bazı bağımlılık çağrıları otomatik olarak--Örneğin, veritabanları ve REST API'leri izler. İş modülü yapmak için sunucunuzda bir aracı yüklemeniz gerekir. 

Java'da, bazı bağımlılık çağrıları otomatik olarak kullanarak izlenebilir [Java Agent](app-insights-java-agent.md).

Bu çağrı otomatik izleme catch değil çağrılarını izlemek istiyorsanız veya aracıyı yüklemek istemiyorsanız kullanın.

C# standart bağımlılık izleme modül devre dışı bırakmak için düzenleme [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) ve başvuru silme `DependencyCollector.DependencyTrackingTelemetryModule`. Standart bağımlılıkları otomatik olarak toplamasını istemiyorsanız, Java, java aracı Lütfen yüklemeyin.

### <a name="dependencies-in-analytics"></a>Analytics bağımlılıkları

İçinde [uygulama Öngörüler Analytics](app-insights-analytics.md), trackDependency çağrıları Göster `dependencies` tablo.

Varsa [örnekleme](app-insights-sampling.md) içinde ItemCount özelliği 1'den büyük bir değer gösterir işlemdir. Örnek ItemCount == trackDependency() için 10 çağrıları, örnekleme işlemi yalnızca bunlardan birinin aktarılan 10 anlamına gelir. Hedef bileşen tarafından kesimli bağımlılıkların doğru bir sayıyı almak için kodu gibi kullanın:

```
dependencies | summarize sum(itemCount) by target
```

Bağımlılıkları ile ilgili isteklerinde ilişkilendirilecek bir birleşim kullanın:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Düzenleniyor veri
Normalde, SDK zamanlarda kullanıcı üzerindeki etkiyi en aza indirmek için seçilen verileri gönderir. Uygulamada kapandıktan SDK kullanıyorsanız, ancak, bazı durumlarda, arabellek--Örneğin, temizlemek isteyebilirsiniz.

*C#*
 
 ```C#
    telemetry.Flush();
    // Allow some time for flushing before shutdown.
    System.Threading.Thread.Sleep(5000);
```

*Java*

```Java
    telemetry.flush();
    //Allow some time for flushing before shutting down
    Thread.sleep(5000);
```

    
*Node.js*

    telemetry.flush();

İşlevi için zaman uyumsuz olduğuna dikkat edin [server telemetri kanalı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

İdeal olarak, flush() yöntemini uygulama kapatma etkinliğinde kullanılmalıdır.

## <a name="authenticated-users"></a>Kimliği doğrulanmış kullanıcılar
Bir web uygulaması, kullanıcıların (varsayılan) tanımlama bilgileri tarafından tanımlanır. Bir kullanıcı birden çok kez uygulamanız farklı makine ya da tarayıcı erişimi engelliyorsa veya tanımlama bilgilerini sildiğinizde sayılması.

Kullanıcılar uygulamanıza oturum açarsa, tarayıcı kodda kimliği doğrulanmış kullanıcı kimliği ayarlayarak daha doğru bir sayı elde edebilirsiniz:

*JavaScript*

```JS
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

Bir ASP.NET web örneğin MVC uygulaması:

*Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Oturum açma kullanıcı asıl adını kullanmak gerekli değildir. Yalnızca o kullanıcı için benzersiz bir kimliği olmak zorundadır. Boşluk veya karakterlerden herhangi birini içermemelidir `,;=|`.

Kullanıcı Kimliği ayrıca bir oturum tanımlama bilgisinde ayarlamak ve sunucuya gönderilen. SDK sunucusu yüklediyseniz, kimliği doğrulanmış kullanıcı kimliği hem istemci hem de sunucu telemetri bağlamı özelliklerini bir parçası olarak gönderilir. Daha sonra filtre ve onu Search'te.

Uygulamanız kullanıcıların hesaplara grupları varsa, (aynı karakter kısıtlamalarla birlikte) hesabı için bir tanıtıcı geçiş yapabilir.

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

İçinde [ölçüm Gezgini](app-insights-metrics-explorer.md), sayar bir grafik oluşturabilirsiniz **kimliği doğrulanmış kullanıcılar,**, ve **kullanıcı hesaplarını**.

Ayrıca [arama](app-insights-diagnostic-search.md) belirli kullanıcı adları ve hesapları ile istemci veri noktaları için.

## <a name="properties"></a>Filtreleme, aramayı ve özelliklerini kullanarak verilerinizi kesimlere
Özellikleri ve ölçülerini, olaylar ekleme (ve ayrıca ölçümlere görünümleri, özel durumlar ve diğer telemetri verilerini sayfasında).

*Özellikler* kullanım raporlarında telemetrinizi filtrelemek için kullanabileceğiniz dize değerlerdir. Örneğin, uygulamanızı birkaç oyunlar sağlıyorsa, oyunun adından da her olay için ekleyebilirsiniz, böylece hangi oyunlar daha popüler görebilirsiniz.

8192 bir dize uzunluk sınırı yoktur. (Büyük veri parçalarını göndermek istiyorsanız, ileti parametresini kullanmak [TrackTrace](#track-trace).)

*Ölçümleri* grafik sunulan sayısal değerlerdir. Örneğin, aşamalı bir artış, oyun severler elde puanları içinde olup olmadığını görmek isteyebilirsiniz. Böylece ayrı alabilir grafikleri olay ile gönderilen özelliklerine göre kesimlere ayrılmıştır veya yığın grafik farklı oyunlar için.

Ölçüm değerleri doğru görüntülenebilmesi için bunlar büyük veya 0 değerine eşit olmalıdır.

Bazı vardır [özellikleri, özellik değerlerini ve ölçümleri sayısı sınırları](#limits) kullanabileceğiniz.

*JavaScript*

    appInsights.trackEvent
      ("WinGame",
         // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );

    appInsights.trackPageView
        ("page name", "http://fabrikam.com/pageurl.html",
          // String properties:
         {Game: currentGame.name, Difficulty: currentGame.difficulty},
         // Numeric metrics:
         {Score: currentGame.score, Opponents: currentGame.opponentCount}
         );


*C#*

    // Set up some properties and metrics:
    var properties = new Dictionary <string, string>
       {{"game", currentGame.Name}, {"difficulty", currentGame.Difficulty}};
    var metrics = new Dictionary <string, double>
       {{"Score", currentGame.Score}, {"Opponents", currentGame.OpponentCount}};

    // Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics);

*Node.js*

    // Set up some properties and metrics:
    var properties = {"game": currentGame.Name, "difficulty": currentGame.Difficulty};
    var metrics = {"Score": currentGame.Score, "Opponents": currentGame.OpponentCount};

    // Send the event:
    telemetry.trackEvent({name: "WinGame", properties: properties, measurements: metrics});


*Visual Basic*

    ' Set up some properties:
    Dim properties = New Dictionary (Of String, String)
    properties.Add("game", currentGame.Name)
    properties.Add("difficulty", currentGame.Difficulty)

    Dim metrics = New Dictionary (Of String, Double)
    metrics.Add("Score", currentGame.Score)
    metrics.Add("Opponents", currentGame.OpponentCount)

    ' Send the event:
    telemetry.TrackEvent("WinGame", properties, metrics)


*Java*

    Map<String, String> properties = new HashMap<String, String>();
    properties.put("game", currentGame.getName());
    properties.put("difficulty", currentGame.getDifficulty());

    Map<String, Double> metrics = new HashMap<String, Double>();
    metrics.put("Score", currentGame.getScore());
    metrics.put("Opponents", currentGame.getOpponentCount());

    telemetry.trackEvent("WinGame", properties, metrics);


> [!NOTE]
> Kişisel bilgi özelliklerinde oturum değil dikkatli olun.
>
>

*Ölçümleri kullandıysanız*, ölçümleri Gezgini'ni açın ve gelen ölçümü seçin **özel** Grup:

![Ölçümleri Gezgini'ni açın, grafiği seçin ve ölçümü seçin](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Ölçüm görünmüyorsa veya **özel** başlık değil vardır, seçimi dikey penceresini kapatın ve daha sonra yeniden deneyin. Ölçümleri bazen ardışık düzen üzerinden toplanacak bir saat sürebilir.

*Özellikleri ve ölçümleri kullandıysanız*, ölçüm özelliği tarafından segmentlere:

![Gruplandırma ayarlayın ve ardından grupla altında özellik seçin](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*Tanılama arama*, özellikleri ve bir olay oluşumlarını ölçümlerini görüntüleyebilirsiniz.

![Bir örnek seçin ve ardından "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Kullanım **arama** belirli bir özellik değeri olan olay örnekleri görmek için alanı.

![Arama terimi yazın](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[Arama ifadeleri hakkında daha fazla bilgi](app-insights-diagnostic-search.md).

### <a name="alternative-way-to-set-properties-and-metrics"></a>Özellikler ve ölçümleri ayarlamak için alternatif yol
Daha kullanışlı ise, ayrı bir nesne olayda parametrelerinin toplayabilirsiniz:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Aynı telemetriyi öğesi örneği yeniden yok (`event` Bu örnekte) Track*() birden çok kez çağrılması. Bu yanlış yapılandırma ile gönderilecek telemetri neden olabilir.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Özel ölçümleri ve analizi özellikleri

İçinde [Analytics](app-insights-analytics.md), özel Ölçümler ve özelliklerini göster `customMeasurements` ve `customDimensions` her telemetri kayıt öznitelikleri.

Örneğin, istek telemetrinizi "oyuna" adlı bir özellik eklediyseniz, bu sorgu "oyunun" farklı değerler oluşumlarını sayar ve özel ölçüm "puan" ortalama göster:

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

Şunlara dikkat edin:

* Bir değer customDimensions ya da customMeasurements JSON ayıkladığınızda, dinamik türüne sahip ve bu nedenle, gereken cast onu `tostring` veya `todouble`.
* Hesaba olasılığı için [örnekleme](app-insights-sampling.md), kullanmanız gereken `sum(itemCount)`değil `count()`.



## <a name="timed"></a> Zamanlama olayları
Bazen bir eylemi gerçekleştirmek için gereken süreyi grafik istersiniz. Örneğin, ne kadar kullanıcılar bilmek isteyebilirsiniz oyun seçimlerini göz önüne alın. Bu ölçüm parametresini kullanabilirsiniz.

*C#*

```C#
    var stopwatch = System.Diagnostics.Stopwatch.StartNew();

    // ... perform the timed action ...

    stopwatch.Stop();

    var metrics = new Dictionary <string, double>
       {{"processingTime", stopwatch.Elapsed.TotalMilliseconds}};

    // Set up some properties:
    var properties = new Dictionary <string, string>
       {{"signalSource", currentSignalSource.Name}};

    // Send the event:
    telemetry.TrackEvent("SignalProcessed", properties, metrics);
```

*Java*

```Java
    long startTime = System.currentTimeMillis();

    // perform timed action

    long endTime = System.currentTimeMillis();
    Map<String, Double> metrics = new HashMap<>();
    metrics.put("ProcessingTime", endTime-startTime);

    // Setup some propereties
    Map<String, String> properties = new HashMap<>();
    properties.put("signalSource", currentSignalSource.getName());

    //send the event
    telemetry.trackEvent("SignalProcessed", properties, metrics);

```


## <a name="defaults"></a>Özel telemetri için varsayılan özellikler
Varsayılan özellik değerleri bazı yazdığınız özel olaylar için ayarlamak istiyorsanız, bunları TelemetryClient örneğinde ayarlayabilirsiniz. Bu istemciden gönderilen her telemetri öğesine eklenir.

*C#*

    using Microsoft.ApplicationInsights.DataContracts;

    var gameTelemetry = new TelemetryClient();
    gameTelemetry.Context.Properties["Game"] = currentGame.Name;
    // Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame");

*Visual Basic*

    Dim gameTelemetry = New TelemetryClient()
    gameTelemetry.Context.Properties("Game") = currentGame.Name
    ' Now all telemetry will automatically be sent with the context property:
    gameTelemetry.TrackEvent("WinGame")

*Java*

    import com.microsoft.applicationinsights.TelemetryClient;
    import com.microsoft.applicationinsights.TelemetryContext;
    ...


    TelemetryClient gameTelemetry = new TelemetryClient();
    TelemetryContext context = gameTelemetry.getContext();
    context.getProperties().put("Game", currentGame.Name);

    gameTelemetry.TrackEvent("WinGame");
    
*Node.js*

    var gameTelemetry = new applicationInsights.TelemetryClient();
    gameTelemetry.commonProperties["Game"] = currentGame.Name;

    gameTelemetry.TrackEvent({name: "WinGame"});



Tek tek telemetri çağrıları kendi özelliği sözlükler varsayılan değerleri geçersiz kılabilirsiniz.

*JavaScript için web istemcileri*, [JavaScript telemetri başlatıcıları kullanmak](#js-initializer).

*Tüm telemetri özellikleri eklemek için*, standart toplama modüllerden verileri dahil olmak üzere [uygulamak `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Örnekleme, filtreleme ve telemetri işleniyor
SDK'dan gelen gönderilmeden önce telemetri işlemek üzere kod yazabilirsiniz. İşleme HTTP isteği koleksiyonu ve bağımlılık koleksiyonu gibi standart telemetri modüllerden gönderilen veriler içerir.

[Özellikler ekleme](app-insights-api-filtering-sampling.md#add-properties) uygulayarak telemetri için `ITelemetryInitializer`. Örneğin, diğer özelliklerinden sürüm numaraları veya hesaplanan değerler ekleyebilirsiniz.

[Filtreleme](app-insights-api-filtering-sampling.md#filtering) değiştirebilir veya SDK'dan gelen uygulayarak gönderilmeden önce telemetri atmak `ITelemetryProcesor`. Ne gönderildiğinde veya atılan denetlemek, ancak ölçümlerinizi etkisi hesaba sahip. Öğeler atılsın nasıl bağlı olarak, ilgili öğeleri arasında gezinme becerisini kaybedebilirsiniz.

[Örnekleme](app-insights-api-filtering-sampling.md) uygulamanızdan portala gönderilen verilerin hacmi azaltmak için paketlenmiş bir çözümdür. Bunu görüntülenen ölçümlerin etkilemeden yapar. Ve bunu özel durumlar, istekleri ve sayfa görünümleri gibi ilgili öğeleri arasında gezinme tarafından sorunları tanılama yeteneğinizi etkilemeden yapar.

[Daha fazla bilgi edinin](app-insights-api-filtering-sampling.md).

## <a name="disabling-telemetry"></a>Telemetri devre dışı bırakma
İçin *dinamik olarak durdurmak ve başlatmak* telemetri iletimini ve koleksiyon:

*C#*

```csharp

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```

*Java*

```Java
    
    telemetry.getConfiguration().setTrackingDisabled(true);

```

İçin *seçili standart toplayıcıları devre dışı*--Örneğin, performans sayaçları, HTTP isteklerini veya bağımlılıkları--silin veya açıklama ilgili satırları çıkışı [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md). Örneğin, kendi TrackRequest veri göndermek istiyorsanız, bunu yapabilirsiniz.

*Node.js*

```Javascript

    telemetry.config.disableAppInsights = true;
```

İçin *seçili standart toplayıcıları devre dışı*--Örneğin, performans sayaçları, HTTP isteklerini veya bağımlılıkları--başlatma zamanında SDK başlatma kodunuza yapılandırma yöntemleri zincir:

```Javascript

    applicationInsights.setup()
        .setAutoCollectRequests(false)
        .setAutoCollectPerformance(false)
        .setAutoCollectExceptions(false)
        .setAutoCollectDependencies(false)
        .setAutoCollectConsole(false)
        .start();
```

Bu toplayıcıları başlatma devre dışı bırakmak için yapılandırma nesnesi kullanın: `applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="debug"></a>Geliştirici modu
Hata ayıklama sırasında sonuçları hemen görebilmeniz için ardışık düzen üzerinden öncelikli telemetrinizi sağlamak kullanışlıdır. Size yardımcı ayrıca get ek ileti telemetri herhangi bir sorun izleme. Uygulamanızı azaltabileceğinden, bir üretim ortamına kapatın.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a> Seçili özel telemetri izleme anahtarı ayarlama
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a> Dinamik izleme anahtarı
Geliştirme, test ve üretim ortamlarını telemetri karıştırma önlemek için şunları yapabilirsiniz [ayrı Application Insights kaynakları oluşturmak](app-insights-create-new-resource.md) ve kendi anahtarları ortamına bağlı olarak değiştirin.

Yapılandırma dosyasından izleme anahtarını almak yerine onu kodunuzda ayarlayabilirsiniz. Anahtar başlatma yöntemini, bir ASP.NET hizmetinde global.aspx.cs gibi ayarlayın:

*C#*

    protected void Application_Start()
    {
      Microsoft.ApplicationInsights.Extensibility.
        TelemetryConfiguration.Active.InstrumentationKey =
          // - for example -
          WebConfigurationManager.Settings["ikey"];
      ...

*JavaScript*

    appInsights.config.instrumentationKey = myKey;



Web sayfalarındaki, web sunucusunun durumu, yerine tam anlamıyla betiğe kodlama ayarlamak isteyebilirsiniz. Bir ASP.NET uygulamasında oluşturulan Örneğin, bir Web sayfasında:

*Razor JavaScript'te*

    <script type="text/javascript">
    // Standard Application Insights webpage script:
    var appInsights = window.appInsights || function(config){ ...
    // Modify this part:
    }({instrumentationKey:  
      // Generate from server property:
      @Microsoft.ApplicationInsights.Extensibility.
         TelemetryConfiguration.Active.InstrumentationKey;
    }) // ...


## <a name="telemetrycontext"></a>TelemetryContext
TelemetryClient tüm telemetri verilerinin yanı sıra gönderilen değerleri içeren bir içerik özelliği vardır. Normalde standart telemetri modülleri tarafından ayarlanmış, ancak Ayrıca bunları kendiniz ayarlayabilirsiniz. Örneğin:

    telemetry.Context.Operation.Name = "MyOperationName";

Bu değerleri kendiniz ayarlarsanız, ilgili satırından kaldırmayı düşünün [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), böylece bilgilerinizi ve standart değerlerini kafası yok.

* **Bileşen**: uygulama ve onun sürümü.
* **Aygıt**: uygulamanın çalıştığı cihaz hakkındaki verileri. (Web uygulamalarında sunucu veya telemetri gönderilen istemci aygıt budur.)
* **InstrumentationKey**: Application Insights kaynağı telemetri göründüğü azure'da. Bu genellikle Applicationınsights.config kayıt.
* **Konum**: cihazın coğrafi konumunu.
* **İşlem**: web uygulamalarında, geçerli HTTP isteği. Diğer uygulama türleri, bu Grup olaylarına birlikte ayarlayabilirsiniz.
  * **Kimliği**: öğeleri ilişkili herhangi bir olay tanılama arama incelediğinizde, bulabilmesi için farklı olayları, karşılık gelen oluşturulan bir değer.
  * **Ad**: bir tanımlayıcı, genellikle HTTP istek URL'si.
  * **SyntheticSource**: değilse null veya boş, isteğin kaynak robot veya web testine belirlenmiştir gösteren bir dize. Varsayılan olarak, ölçüm Gezgini hesaplamalarda gelen dışlanır.
* **Özellikleri**: tüm telemetri verileri ile gönderilen özellikleri. Tek tek parça * çağrılarında geçersiz kılınabilir.
* **Oturum**: kullanıcı oturumu. Kimliği, kullanıcı bir süredir etkin ayarlanmadı bağlandığınızda değiştirilir oluşturulan bir değere ayarlanır.
* **Kullanıcı**: kullanıcı bilgileri.

## <a name="limits"></a>Sınırlar
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

Veri hızı sınırı basarsa önlemek için [örnekleme](app-insights-sampling.md).

Verileri Tutuluyor ne kadar süreyle belirlemek için bkz: [veri saklama ve gizlilik](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Başvuru belgeleri
* [ASP.NET başvurusu](https://msdn.microsoft.com/library/dn817570.aspx)
* [Java başvurusu](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript başvurusu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK kod
* [ASP.NET Core SDK](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [Windows Server paketleri](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [Node.js SDK’sı](https://github.com/Microsoft/ApplicationInsights-Node.js)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)
* [Tüm platformlar](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Sorular
* *Hangi özel durumları Track_() çağrıları throw?*

    Yok. Try-catch yan tümcelerinde kaydırma gerek yoktur. SDK sorunla karşılaşırsa, hata ayıklama konsol çıktısı iletileri kaydedecek ve--tanılama Arama'da iletileri üzerinden--alırsanız.
* *Portaldan veri almak için bir REST API var mı?*

    Evet, [veri erişim API](https://dev.applicationinsights.io/). Veri ayıklamak için diğer yolları dahil [Analytics'ten Power BI verme](app-insights-export-power-bi.md) ve [sürekli verme](app-insights-export-telemetry.md).

## <a name="next"></a>Sonraki adımlar
* [Arama olayları ve günlükleri](app-insights-diagnostic-search.md)

* [Sorun giderme](app-insights-troubleshoot-faq.md)


