---
title: Özel olaylar ve ölçümler için Application Insights API | Microsoft Docs
description: Birkaç satır kod, cihaz veya masaüstü uygulaması, Web sayfası veya kullanımını izlemek ve sorunları tanılamak için hizmetinizi ekleyin.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 80400495-c67b-4468-a92e-abf49793a54d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: mbullwin
ms.openlocfilehash: 5c33e1a5568de5fffb5ea9cedb43bdc04aeaeba7
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38306769"
---
# <a name="application-insights-api-for-custom-events-and-metrics"></a>Özel olaylar ve ölçümler için Application Insights API

Uygulamanızda hangi kullanıcılar ile nasıl kullandığını görün veya sorunlarının tanılanmasına yardımcı olmak için birkaç satırlık bir kod ekleyin. Cihaz ve Masaüstü uygulamaları, web istemcileri ve web sunucularından telemetri gönderebilir. Kullanım [Azure Application Insights](app-insights-overview.md) çekirdek özel olaylar ve ölçümler ve kendi sürümleri standart telemetri göndermek için API telemetri. Bu API, standart Application Insights veri toplayıcıları kullanın aynı bir API'dir.

## <a name="api-summary"></a>API özeti
Birkaç küçük farklılıklar dışında tüm platformlar arasında Tekdüzen bir API'dir.

| Yöntem | İçin kullanılan |
| --- | --- |
| [`TrackPageView`](#page-views) |Sayfalar, ekranlar, dikey pencereleri veya forms. |
| [`TrackEvent`](#trackevent) |Kullanıcı eylemlerini ve diğer olayları. Kullanıcı davranışını izleme veya performansını izlemek için kullanılır. |
| [`TrackMetric`](#trackmetric) |Belirli olayları ile ilgili olmayan kuyruk uzunluğu gibi performans ölçümleri. |
| [`TrackException`](#trackexception) |Tanılama için özel durumları günlüğe kaydetme. Diğer olayları ile ilgili olarak ortaya ve Yığın izlemeleri inceleyin burada izleme. |
| [`TrackRequest`](#trackrequest) |Günlüğe kaydetme sıklığı ve performans analizi için sunucu isteği süresi. |
| [`TrackTrace`](#tracktrace) |Tanılama günlüğü iletileri. Üçüncü taraf günlükleri de yakalayabilirsiniz. |
| [`TrackDependency`](#trackdependency) |Uygulamanızın bağımlı dış bileşenlere çağrılar sıklığı ve süresi günlüğe kaydetme. |

Yapabilecekleriniz [özellikler ve ölçümler ekleme](#properties) çoğu bu telemetri çağrıları.

## <a name="prep"></a>Başlamadan önce
Application Insights SDK'sı hakkında başvuru henüz yoksa:

* Application Insights SDK'sını projenize ekleyin:

  * [ASP.NET projesi](app-insights-asp-net.md)
  * [Java projesi](app-insights-java-get-started.md)
  * [Node.js projesi](app-insights-nodejs.md)
  * [Her bir Web sayfasındaki JavaScript](app-insights-javascript.md) 
* Cihazınıza veya web sunucusu kodunuza şunu ekleyin:

    *C# İÇİN:* `using Microsoft.ApplicationInsights;`

    *Visual Basic:* `Imports Microsoft.ApplicationInsights`

    *Java:* `import com.microsoft.applicationinsights.TelemetryClient;`
    
    *Node.js:* `var applicationInsights = require("applicationinsights");`

## <a name="get-a-telemetryclient-instance"></a>TelemetryClient örneği Al
Bir kopyasını almak `TelemetryClient` (JavaScript'te sayfalarında hariç):

*C#*

    private TelemetryClient telemetry = new TelemetryClient();

*Visual Basic*

    Private Dim telemetry As New TelemetryClient

*Java*

    private TelemetryClient telemetry = new TelemetryClient();
    
*Node.js*

    var telemetry = applicationInsights.defaultClient;


TelemetryClient, iş parçacığı açısından güvenlidir.

ASP.NET ve Java projeleri için gelen HTTP isteklerini otomatik olarak yakalanır. Uygulamanızın başka bir modül için ek TelemetryClient örneklerini oluşturmak isteyebilirsiniz. Örneğin, bir TelemetryClient örneği raporu iş mantığı olayları ara yazılım sınıfı olabilir. Makineyi tanımlamak için kullanıcı kimliği ve cihaz kimliği gibi özellikleri ayarlayabilirsiniz. Bu bilgileri, örnek gönderen tüm olayları eklenir. 

*C#*

    TelemetryClient.Context.User.Id = "...";
    TelemetryClient.Context.Device.Id = "...";

*Java*

    telemetry.getContext().getUser().setId("...);
    telemetry.getContext().getDevice().setId("...");

Node.js projelerinde kullanabileceğiniz `new applicationInsights.TelemetryClient(instrumentationKey?)` yeni bir örneği, ancak bu oluşturmak için tekli yalıtılmış yapılandırmasından gerektiren senaryolar için önerilir `defaultClient`.

## <a name="trackevent"></a>TrackEvent
Application ınsights'ta bir *özel olay* görüntülemek için bir veri noktasıdır [ölçüm Gezgini](app-insights-metrics-explorer.md) toplam bir sayı olarak ve buna [tanılama araması](app-insights-diagnostic-search.md) olarak tek tek tekrar. (Bu MVC veya diğer framework "olaylar" ilgili değildir)

INSERT `TrackEvent` çeşitli olaylarının kodunuzda çağırır. Ne sıklıkla kullanıcıların belirli bir özellik, ne sıklıkta bunlar belirli hedeflere ulaşmak veya hatalarının belirli türlerini yaptıkları belki de ne sıklıkta seçin.

Örneğin, bir oyun uygulaması, bir kullanıcı oyun WINS bir olay gönderebilir:

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
Olayların sayısını görmek için bir [ölçüm Gezgini](app-insights-metrics-explorer.md) dikey penceresinde, yeni bir grafik ekleyin ve seçin **olayları**.  

![Özel olay sayısı bakın](./media/app-insights-api-custom-events-metrics/01-custom.png)

Grafik türü farklı olay sayısını Karşılaştırılacak kümesine **kılavuz**ve olay adıyla Grup:

![Grafik türünü ve gruplandırma](./media/app-insights-api-custom-events-metrics/07-grid.png)

Kılavuzda, bu olay oluşumlarını görmek için bir olay adı tıklayın. -Daha fazla ayrıntı görmek için listedeki olayı tıklayın.

![Olayları detaya gidin](./media/app-insights-api-custom-events-metrics/03-instances.png)

Arama veya ölçüm Gezgini belirli olayları odaklanmak için ilgilendiğiniz olay adları için filtre dikey penceresinin ayarlayın:

![Filtreler açın, olay adını genişletin ve bir veya daha fazla değer seçin](./media/app-insights-api-custom-events-metrics/06-filter.png)

### <a name="custom-events-in-analytics"></a>Analytics'te özel olaylar

Telemetriyi kullanılabilir `customEvents` tablosundaki [Application Insights Analytics](app-insights-analytics.md). Her satır için bir çağrı temsil eden `trackEvent(..)` uygulamanızda. 

Varsa [örnekleme](app-insights-sampling.md) ItemCount özelliği 1'den büyük bir değer gösterir, işlemde olduğu. İçin örnek ItemCount == trackEvent() 10 çağrısına örnekleme işlemi yalnızca bir tanesi aktarılan 10 anlamına gelir. Özel olaylar doğru sayısını almak için bu nedenle gibi bir kod kullanın kullanmalısınız `customEvent | summarize sum(itemCount)`.


## <a name="trackmetric"></a>TrackMetric

Application Insights belirli olaylara bağlı olmayan ölçümleri grafik. Örneğin, bir kuyruk uzunluğu düzenli aralıklarla izleyebilir. Ölçümler, tek tek ölçüler çözümlenmeyebileceği ve eğilimleri daha az ilgi çeken ve bu nedenle istatistiksel grafikleri kullanışlıdır.

Ölçümler Application Insights'a gönderme için kullanabileceğiniz `TrackMetric(..)` API. Bir ölçüm göndermek için iki yolu vardır: 

* Tek bir değer. Uygulamanızda bir ölçüm gerçekleştirdiğiniz her seferinde, karşılık gelen değerle Application Insights'a gönderme. Örneğin, bir kapsayıcı içindeki öğe sayısını açıklayan bir ölçüm olduğunu varsaymaktadır. Belirli bir zaman diliminde ilk üç öğe kapsayıcının içine yerleştirin ve ardından, iki öğeyi kaldırın. Buna çağrı yapıyordu `TrackMetric` iki kez: ilk değeri geçirdiğini `3` ve değer `-2`. Application Insights sizin adınıza her iki değeri depolar. 

* Toplama. Ölçümler ile çalışırken, her tek bir ölçüm nadiren ilgi çekecektir. Bunun yerine belirli bir dönemde ne bir özeti önemlidir. Böyle bir özeti adlı _toplama_. Yukarıdaki örnekte, bu döneme ait toplam ölçüm toplamı olan `1` ve ölçüm değerleri sayısı `2`. Toplama yaklaşımı kullanarak, yalnızca çağırma `TrackMetric` zaman süresi başına bir kez ve toplam değerler gönderin. Bu önemli ölçüde performans ve maliyet ek yükü daha az veri noktası için Application Insights, yine de ilgili tüm bilgileri toplanırken göndererek küçültebilirsiniz olduğundan bu önerilen bir yaklaşımdır.

### <a name="examples"></a>Örnekler:

#### <a name="single-values"></a>Tek değerler

Tek bir ölçüm değeri göndermek için:

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

#### <a name="aggregating-metrics"></a>Ölçüm toplama

İçin toplu ölçümleri uygulamanızdan göndermeden önce önerilir, bant genişliğini azaltmak için maliyet ve performansı artırmak için.
Toplama koduna ilişkin bir örnek aşağıda verilmiştir:

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

### <a name="custom-metrics-in-metrics-explorer"></a>Ölçüm Gezgini'nde özel ölçümler

Sonuçları görmek için ölçüm Gezgini'ni açın ve yeni bir grafik ekleyin. Grafiği, ölçümü gösterecek şekilde düzenleyin.

> [!NOTE]
> Özel ölçüm, kullanılabilir ölçümler listede görünmesi birkaç dakika sürebilir.
>

![Yeni bir grafik ekleyin veya bir grafiği seçin ve özel'ın altında ölçüm seçin](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

### <a name="custom-metrics-in-analytics"></a>Analytics'te özel ölçümler

Telemetriyi kullanılabilir `customMetrics` tablosundaki [Application Insights Analytics](app-insights-analytics.md). Her satır için bir çağrı temsil eden `trackMetric(..)` uygulamanızda.
* `valueSum` -Bu ölçümler toplamıdır. Ortalama değer almak için bölen `valueCount`.
* `valueCount` -Bu toplanan ölçümlerin sayısı `trackMetric(..)` çağırın.

## <a name="page-views"></a>Sayfa görünümleri
Her ekran veya bir sayfa yüklendiğinde bir cihaz veya Web uygulamasında sayfa görünümü telemetrisini varsayılan olarak gönderilir. Ancak, ek veya bunlardan farklı zamanlarda sayfa görünümleri izlemek için değiştirebilirsiniz. Örneğin, sekmeler veya dikey pencereleri görüntüleyen bir uygulama, kullanıcının yeni bir dikey pencere açıldığında bir sayfayı izlemek isteyebilirsiniz.

![Genel Bakış dikey penceresinde kullanım odağı](./media/app-insights-api-custom-events-metrics/appinsights-47usage-2.png)

Sayfa görünümü telemetrisini olduğunda kullanıcı ve oturum grafikleri Canlı gelmesi için kullanıcı ve oturum verilerini birlikte sayfa görüntüleme özellikleri olarak gönderilir.

### <a name="custom-page-views"></a>Özel sayfa görüntülemeleri
*JavaScript*

    appInsights.trackPageView("tab1");

*C#*

    telemetry.TrackPageView("GameReviewPage");

*Java*

    telemetry.trackPageView("GameReviewPage");

*Visual Basic*

    telemetry.TrackPageView("GameReviewPage")


Farklı HTML sayfaları içinde birkaç sekme varsa, URL'yi çok belirtebilirsiniz:

    appInsights.trackPageView("tab1", "http://fabrikam.com/page1.htm");

### <a name="timing-page-views"></a>Zamanlama sayfa görüntülemeleri
Varsayılan olarak kez bildirilen **sayfa görüntüleme yükleme süresi** tarayıcı sayfa yükleme olayı çağrılana kadar tarayıcı isteği gönderdiğinde, gelen ölçülür.

Bunun yerine, şunlardan birini yapabilirsiniz:

* Açık bir süre kümesinde [trackPageView](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#trackpageview) çağrı: `appInsights.trackPageView("tab1", null, null, null, durationInMilliseconds);`.
* Zamanlama çağrı sayfa görünümünü kullanın `startTrackPage` ve `stopTrackPage`.

*JavaScript*

    // To start timing a page:
    appInsights.startTrackPage("Page1");

...

    // To stop timing and log the page:
    appInsights.stopTrackPage("Page1", url, properties, measurements);

İlk parametre olarak kullandığınız adı başlatma ve durdurma çağrıları ilişkilendirir. Geçerli sayfa adı için varsayılan olarak ayarlanır.

Ölçüm Gezgini'nde görüntülenen sonuç sayfa yükleme süreleri başlatma ve durdurma aramaları aralığını türetilmiştir. Bu, gerçekten zaman hangi aralığı aittir.

### <a name="page-telemetry-in-analytics"></a>Analytics'te sayfa telemetrisi

İçinde [Analytics](app-insights-analytics.md) iki tablo tarayıcı işlemlerden verileri göster:

* `pageViews` Tablo URL ve sayfa başlığını hakkındaki verileri içerir
* `browserTimings` Tablo gelen veriyi işlemek için geçen süre gibi istemci performansıyla ilgili verileri içerir

Ne kadar tarayıcı farklı sayfalar işlenme bulmak için:

```
browserTimings | summarize avg(networkDuration), avg(processingDuration), avg(totalDuration) by name 
```

Farklı tarayıcıların popularities bulmak için:

```
pageViews | summarize count() by client_Browser
```

Sayfa görüntülemeleri için AJAX çağrıları ilişkilendirilecek bağımlılıklarla Katıl:

```
pageViews | join (dependencies) on operation_Id 
```

## <a name="trackrequest"></a>TrackRequest
Sunucu SDK'sı TrackRequest HTTP isteklerini günlüğe kaydetmek için kullanır.

Bir bağlamda istek benzetimi yapmak istiyorsanız, ayrıca kendiniz, çalışan web hizmeti modülünüzün olmadığı burada çağırabilirsiniz.

Ancak, burada isteği olarak görev yapar istek telemetrisi göndermek için önerilen yöntem olduğu bir <a href="#operation-context">işlemi bağlam</a>.

## <a name="operation-context"></a>İşlem bağlamı
İşlem bağlamı ile ilişkilendirerek telemetri öğelerinin birlikte ilişkilendirebilirsiniz. Standart istek izleme modülü, özel durumlar ve bir HTTP isteği işlenirken gönderilen diğer olayları için bunu yapar. İçinde [arama](app-insights-diagnostic-search.md) ve [Analytics](app-insights-analytics.md), kendi işlem kimliği kullanarak istekle ilişkili olaylar kolayca bulabilirsiniz

Bkz: [Application ınsights Telemetri bağıntısı](application-insights-correlation.md) bağıntı hakkında daha fazla ayrıntı için.

Telemetri el ile izleme sırasında bu deseni kullanılarak telemetri bağıntısı sağlamanın en kolay yolu:

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

Bir işlem bağlamını ayarlayarak birlikte `StartOperation` belirttiğiniz türünde bir telemetri öğesi oluşturur. İşlemi çıkardığınızda veya açıkça çağırmak telemetri öğesinin gönderir `StopOperation`. Kullanırsanız `RequestTelemetry` telemetri türü olarak başlatma ve durdurma arasındaki zaman aralığı süresi ayarlanır.

Telemetri öğelerinin bir işlem kapsamı içinde bildirilen 'alt' gibi bir işlem haline gelir. İşlem bağlamları iç içe. 

Oluşturmak için kullanılan araması için işlem bağlamı **ilgili öğeleri** listesi:

![İlgili öğeler](./media/app-insights-api-custom-events-metrics/21.png)

Bkz: [Application Insights .NET SDK ile özel işlemleri izleme](application-insights-custom-operations-tracking.md) özel işlemleri izleme hakkında daha fazla bilgi için.

### <a name="requests-in-analytics"></a>Analytics'te istekleri 

İçinde [Application Insights Analytics](app-insights-analytics.md), içinde Göster istekleri `requests` tablo.

Varsa [örnekleme](app-insights-sampling.md) olduğundan, işlem ItemCount özelliği bir değer 1'den büyük gösterir. İçin örnek ItemCount == trackRequest() 10 çağrısına örnekleme işlemi yalnızca bir tanesi aktarılan 10 anlamına gelir. İstek ve ortalama süresi isteğin adlarına göre segmentlere doğru sayısını almak için kod aşağıdaki gibi kullanın:

```AIQL
requests | summarize count = sum(itemCount), avgduration = avg(duration) by name
```


## <a name="trackexception"></a>TrackException
Özel durumlar, Application Insights'a gönder:

* İçin [saymanız](app-insights-metrics-explorer.md), olarak sıklık bir sorunun göstergesidir.
* İçin [ayrı ayrı örnekleri inceleyin](app-insights-diagnostic-search.md).

Raporlar, yığın izlemelerini içerir.

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

Her zaman TrackException açıkça çağırmak zorunda kalmamak için SDK'ları çok sayıda özel durumları otomatik olarak yakalayın.

* ASP.NET: [özel durumları yakalamak için kod yazma](app-insights-asp-net-exceptions.md).
* J2EE: [özel durumları otomatik olarak yakalanır](app-insights-java-get-started.md#exceptions-and-request-failures).
* JavaScript: Özel durumları otomatik olarak yakalanır. Otomatik olarak toplanmasını devre dışı bırakmak isterseniz, bir satırı, Web sayfalarındaki eklediğiniz kod parçacığını ekleyin:

    ```
    ({
      instrumentationKey: "your key"
      , disableExceptionTracking: true
    })
    ```

### <a name="exceptions-in-analytics"></a>Analytics'te özel durumları

İçinde [Application Insights Analytics](app-insights-analytics.md), özel durumlar gösterilir `exceptions` tablo.

Varsa [örnekleme](app-insights-sampling.md) işleminde, `itemCount` özelliği değeri 1'den büyük gösterir. İçin örnek ItemCount == 10 trackException() çağrıları ekleme, örnekleme işlemi yalnızca bir tanesi aktarılan 10 anlamına gelir. Özel durum türüne göre segmentlere özel durumlarını doğru sayısını almak için kod aşağıdaki gibi kullanın:

```
exceptions | summarize sum(itemCount) by type
```

En önemli yığın bilgileri ayrı değişkenlere zaten ayıklanan ancak uzaklıkta çekme `details` yapısını daha fazla yararlanın. Bu yapı dinamik olduğundan, beklediğiniz türü sonucu atamalısınız. Örneğin:

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
TrackTrace "içerik haritası Kılavuzu" Application Insights'a göndererek, sorunların tanılanmasına yardımcı olmak için kullanın. Tanılama veri öbekleri göndermek ve bunları İnceleme [tanılama araması](app-insights-diagnostic-search.md).

. NET'te [oturum bağdaştırıcıları](app-insights-asp-net-trace-logs.md) Portalı'na üçüncü taraf günlükleri göndermek için bu API'yi kullanın.

Java için [standart günlükçüleri ister Log4J, Logback](app-insights-java-trace-logs.md) Portalı'na üçüncü taraf günlükleri göndermek için Application Insights Log4j veya Logback Appenders kullanın.

*C#*

    telemetry.TrackTrace(message, SeverityLevel.Warning, properties);

*Java*

    telemetry.trackTrace(message, SeverityLevel.Warning, properties);
    
*Node.js*

    telemetry.trackTrace({message: message, severity:applicationInsights.Contracts.SeverityLevel.Warning, properties:properties});


İleti içeriği arama yapabilirsiniz, ancak (özellik değerlerini), üzerinde filtre uygulanamıyor.

Üzerindeki boyut sınırı `message` özellikleri sınırdan çok daha yüksek.
TrackTrace bir avantajı, iletide görece uzun veri koyabilirsiniz ' dir. Örneğin, gönderme verisi şifreleyebilirsiniz.  

Ayrıca, önem derecesi mesajınızı ekleyebilirsiniz. Ve diğer telemetri verilerinin filtre veya arama izlemeler farklı kümeleri için yardımcı olması için özellik değerlerini ekleyebilirsiniz. Örneğin:

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

İçinde [arama](app-insights-diagnostic-search.md), daha sonra kolayca için belirli bir veritabanı ile ilgili tüm iletileri belirli bir önem derecesi düzeyi filtreleyebilirsiniz.


### <a name="traces-in-analytics"></a>Analytics'te izlemeleri

İçinde [Application Insights Analytics](app-insights-analytics.md), TrackTrace çağrıları gösterilir `traces` tablo.

Varsa [örnekleme](app-insights-sampling.md) ItemCount özelliği 1'den büyük bir değer gösterir, işlemde olduğu. İçin örnek ItemCount == 10, 10 yapılan çağrıların anlamına gelir `trackTrace()`, örnekleme işlemi yalnızca bir tanesi aktarılan. İzleme çağrıları doğru sayısını almak için bu nedenle kod gibi kullanmalısınız `traces | summarize sum(itemCount)`.

## <a name="trackdependency"></a>TrackDependency
TrackDependency çağrı yanıt süreleri ve başarı oranları bir dış kod parçasına yapılan çağrıların izlemek için kullanın. Sonuçlar, portaldaki bağımlılık grafiklerinde görüntülenir.

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
     // The call above has been made obsolete in the latest SDK. The updated call follows this format:
     // TrackDependency (string dependencyTypeName, string dependencyName, string data, DateTimeOffset startTime, TimeSpan duration, bool success);
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

Sunucu SDK'ları içerdiğini unutmayın bir [bağımlılık Modülü](app-insights-asp-net-dependencies.md) bulur ve belirli bir bağımlılık çağrıları otomatik olarak--Örneğin, veritabanları ve REST API'leri için izler. İş modülü yapmak sunucunuza bir aracı yüklemek zorunda. 

Java dilinde bazı bağımlılık çağrıları otomatik olarak kullanarak izlenebilir [Java aracı](app-insights-java-agent.md).

Bu çağrı, otomatik izleme catch değil çağrıları izlemek istiyorsanız veya aracıyı yüklemek istemiyorsanız kullanın.

C# ' de standart bağımlılık izleme Modülü'devre dışı bırakmak için düzenleme [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md) ve başvurusunu silmek `DependencyCollector.DependencyTrackingTelemetryModule`. Java dilinde standart bağımlılıkları otomatik olarak toplamasını istemiyorsanız lütfen java aracı yüklemeyin.

### <a name="dependencies-in-analytics"></a>Analytics bağımlılıkları

İçinde [Application Insights Analytics](app-insights-analytics.md), trackDependency çağrıları Göster `dependencies` tablo.

Varsa [örnekleme](app-insights-sampling.md) ItemCount özelliği 1'den büyük bir değer gösterir, işlemde olduğu. İçin örnek ItemCount == trackDependency() 10 çağrısına örnekleme işlemi yalnızca bir tanesi aktarılan 10 anlamına gelir. Bölümlenmiş hedef bileşen tarafından bağımlılıkları doğru sayısını almak için kod aşağıdaki gibi kullanın:

```
dependencies | summarize sum(itemCount) by target
```

Bağımlılıkları ilgili isteklerinde ile ilişkilendirilecek bir birleşim kullanın:

```
dependencies
| join (requests) on operation_Id 
```

## <a name="flushing-data"></a>Veri düzenleniyor
Normalde, SDK, bazen kullanıcının üzerindeki etkiyi en aza indirmek için seçilen veri gönderir. Uygulamada kapanırken SDK kullanıyorsanız, ancak bazı durumlarda, arabellek--örneğin temizlemek isteyebilirsiniz.

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

İşlev için zaman uyumsuz olduğunu unutmayın [sunucu telemetri kanalı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/).

İdeal olarak, uygulamayı kapatma etkinliğinde flush() yöntemini kullanılmalıdır.

## <a name="authenticated-users"></a>Kimliği doğrulanmış kullanıcılar
Bir web uygulaması kullanıcılar tarafından tanımlama (varsayılan) tanımlanır. Bir kullanıcı birden çok kez bunlar başka bir makine ya da tarayıcı uygulamanıza erişmek veya kullanıcılar tanımlama bilgilerini silmeniz halinde sayılması.

Kullanıcıların uygulamanızda oturum açarsa, kimliği doğrulanmış kullanıcı kimliği tarayıcı kodda ayarlayarak daha doğru sayısı alabilirsiniz:

*JavaScript*

```JS
// Called when my app has identified the user.
function Authenticated(signInId) {
    var validatedId = signInId.replace(/[,;=| ]+/g, "_");
    appInsights.setAuthenticatedUserContext(validatedId);
    ...
}
```

Bir ASP.NET web MVC uygulama, örneğin:

*Razor*

        @if (Request.IsAuthenticated)
        {
            <script>
                appInsights.setAuthenticatedUserContext("@User.Identity.Name
                   .Replace("\\", "\\\\")"
                   .replace(/[,;=| ]+/g, "_"));
            </script>
        }

Oturum açma kullanıcı asıl adı kullanmak için gerekli değildir. Bunu yalnızca söz konusu kullanıcı için benzersiz bir kimliği olması gerekir. Boşluk veya karakterlerin hiçbirini içermemesi `,;=|`.

Kullanıcı Kimliği ayrıca bir oturum tanımlama bilgilerinde ayarlayın ve sunucuya gönderilen. Kimliği doğrulanmış kullanıcı kimliği, sunucu SDK'sı yüklü değilse, istemci ve sunucu telemetri bağlam özelliklerini bir parçası olarak gönderilir. Ardından filtre uygulayabilirsiniz ve üzerindeki arama.

Uygulamanız kullanıcıların hesaplarına veri grupları, Hesapla (aynı karakter kısıtlamaları) için bir tanımlayıcı de geçirebilirsiniz.

      appInsights.setAuthenticatedUserContext(validatedId, accountId);

İçinde [ölçüm Gezgini](app-insights-metrics-explorer.md), sayan bir grafik oluşturabilir miyim **kimliği doğrulanmış kullanıcılar,**, ve **kullanıcı hesaplarını**.

Ayrıca [arama](app-insights-diagnostic-search.md) belirli kullanıcı adları ve hesapları ile istemci veri noktaları için.

## <a name="properties"></a>Filtreleme, aramayı ve özelliklerini kullanarak verilerinizi kesimlere
Açabilir özellikler ve ölçümler olaylarınızı (ve ölçümler için görünümleri, özel durumlar ve diğer telemetri verilerini de sayfa).

*Özellikleri* telemetrinizi kullanım raporları filtrelemek için kullanabileceğiniz dize değerlerdir. Örneğin, uygulamanız çeşitli oyunlar sağlıyorsa, oyunun adından da her olaya ekleyebilirsiniz, böylece hangi oyunlar daha popüler olduğunu görebilirsiniz.

8192 bir dize uzunluk sınırı yoktur. (İleti parametresini büyük öbekler halinde veri göndermek istiyorsanız, kullanın [TrackTrace](#track-trace).)

*Ölçümleri* grafik olarak sunulan sayısal değerlerdir. Örneğin, oyuncularınıza elde puanları aşamalı bir artış olup olmadığını görmek isteyebilirsiniz. Olay ile gönderilen özelliklere göre ayrı alabilmesi grafikler bölümlenebilecek veya Yığılmış grafikler için farklı oyunlar.

Doğru şekilde görüntülenecek ölçüm değerleri için bunlar büyük veya 0'a eşit olmalıdır.

Bazı kısıtlamalar var. [özellikleri, özellik değerlerini ve ölçümleri sayısına yönelik sınırlar](#limits) kullanabileceğiniz.

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
> Kişisel olarak tanımlanabilen bilgiler özelliklerinde günlüğe kaydetmemeyi dikkat edin.
>
>

*Ölçümleri kullandıysanız*ölçüm Gezgini'ni açın ve gelen bir ölçüm seçin **özel** Grup:

![Ölçüm Gezgini'ni açın, grafik seçin ve bir ölçüm seçin](./media/app-insights-api-custom-events-metrics/03-track-custom.png)

> [!NOTE]
> Ölçümünüzün görünmüyorsa veya **özel** başlık var olmadığından, seçimi dikey pencereyi kapatın ve daha sonra tekrar deneyin. Ölçümleri bazen işlem hattı toplanacak saat arasında sürebilir.

*Özellikler ve ölçümler kullandıysanız*, ölçüm özelliğiyle segmentlere ayırın:

![Gruplandırma ayarlayın ve ardından özelliği altında gruplandırma ölçütü seçin](./media/app-insights-api-custom-events-metrics/04-segment-metric-event.png)

*Tanılama aramasındaki*, özellikleri ve bir olay oluşumlarını ölçümlerini görüntüleyebilirsiniz.

![Bir örnek seçin ve ardından "..."](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-4.png)

Kullanım **arama** belirli bir özellik değeri olan olayın tekrarlarının görmek için alan.

![Arama terimi yazın](./media/app-insights-api-custom-events-metrics/appinsights-23-customevents-5.png)

[Arama ifadeleri hakkında daha fazla bilgi](app-insights-diagnostic-search.md).

### <a name="alternative-way-to-set-properties-and-metrics"></a>Özellikler ve ölçümler için alternatif bir yolu
Daha kolay ise ayrı bir nesnede bir olay parametrelerinin toplayabilirsiniz:

    var event = new EventTelemetry();

    event.Name = "WinGame";
    event.Metrics["processingTime"] = stopwatch.Elapsed.TotalMilliseconds;
    event.Properties["game"] = currentGame.Name;
    event.Properties["difficulty"] = currentGame.Difficulty;
    event.Metrics["Score"] = currentGame.Score;
    event.Metrics["Opponents"] = currentGame.Opponents.Length;

    telemetry.TrackEvent(event);

> [!WARNING]
> Aynı telemetriyi öğesi örneği yeniden kullanmayın (`event` Bu örnekte) Track*() birden çok kez çağırmak için. Bu, yanlış yapılandırma ile gönderilecek telemetri neden olabilir.
>
>

### <a name="custom-measurements-and-properties-in-analytics"></a>Özel Ölçümler ve analiz özellikleri

İçinde [Analytics](app-insights-analytics.md), özel Ölçümler ve özelliklerini göster `customMeasurements` ve `customDimensions` her telemetri kaydının öznitelikleri.

Örneğin, istek telemetrinizi "oyuna" adlı bir özellik eklediyseniz, bu sorgu farklı değerler "oyunun" oluşumlarını sayar ve özel ölçüm "puan" ortalamasını gösterir:

```
requests
| summarize sum(itemCount), avg(todouble(customMeasurements.score)) by tostring(customDimensions.game) 
```

Şunlara dikkat edin:

* CustomDimensions veya customMeasurements JSON değeri ayıklamak, dinamik türünde ve bu nedenle, gereken dönüştürme, `tostring` veya `todouble`.
* Hesaba algılanması için [örnekleme](app-insights-sampling.md), kullanmanız gereken `sum(itemCount)`değil `count()`.



## <a name="timed"></a> Zamanlama olayları
Bazen bir eylemi gerçekleştirmek için ne kadar sürer grafik istersiniz. Örneğin, ne kadar kullanıcılar bilmek isteyebilirsiniz bir oyun seçeneklerini göz önünde bulundurmanız için gerçekleştirin. Bu ölçüm parametresini kullanabilirsiniz.

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
Varsayılan özellik değerlerini bazı yazdığınız özel olaylar için ayarlamak istiyorsanız, bunları bir TelemetryClient örneğinde ayarlayabilirsiniz. Bu istemciden gönderilen her telemetri öğesine eklenir.

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



Tek bir telemetri çağrıları kendi özellik sözlükleri varsayılan değerleri geçersiz kılabilirsiniz.

*JavaScript için web istemcileri*, [JavaScript telemetri başlatıcıları kullanın](#js-initializer).

*Tüm telemetri özellikleri eklemek için*, standart toplama modüllerden veri dahil olmak üzere [uygulamak `ITelemetryInitializer` ](app-insights-api-filtering-sampling.md#add-properties).

## <a name="sampling-filtering-and-processing-telemetry"></a>Örnekleme, filtreleme ve telemetri işleme
SDK'sından gönderilmeden önce telemetrinizi işlemek için kod yazabilirsiniz. HTTP isteği koleksiyonu ve bağımlılık toplama gibi standart telemetri modüllerden gönderilen verileri işlemeyi içerir.

[Özellikler ekleme](app-insights-api-filtering-sampling.md#add-properties) uygulayarak telemetriye `ITelemetryInitializer`. Örneğin, diğer özelliklerden sürüm numaraları veya hesaplanan değerler ekleyebilirsiniz.

[Filtreleme](app-insights-api-filtering-sampling.md#filtering) değiştirebilir veya telemetri SDK'sından uygulayarak gönderilmeden önce iptal `ITelemetryProcesor`. Ne gönderildiğinde veya iptal denetimi ancak ölçümlerinizi üzerindeki etkisini hesabı gerekir. Öğeler atılsın nasıl bağlı olarak, ilgili öğeleri arasında gezinme olanağı kaybedebilirsiniz.

[Örnekleme](app-insights-api-filtering-sampling.md) portala uygulamanızdan gönderilen veri hacmini azaltmak için paketlenmiş bir çözümdür. Bunu görüntülenen ölçümlerin etkilemeden yapar. Ve bunu özel durumlar, istekler ve sayfa görüntülemeleri gibi ilgili öğeleri arasında giderek sorunları tanılama yeteneğinizi etkilemeden yapar.

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

İçin *seçili standart Toplayıcı devre dışı*--Örneğin, performans sayaçları, HTTP istekleri ve bağımlılıkları--silin veya ilgili satırları açıklama [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md). Örneğin, kendi TrackRequest veri göndermek istiyorsanız bunu yapabilirsiniz.

*Node.js*

```Javascript

    telemetry.config.disableAppInsights = true;
```

İçin *seçili standart Toplayıcı devre dışı*--Örneğin, performans sayaçları, HTTP istekleri ve bağımlılıkları--başlatma sırasında yapılandırma yöntemleri, SDK'sını başlatma kodunuzun zincir:

```Javascript

    applicationInsights.setup()
        .setAutoCollectRequests(false)
        .setAutoCollectPerformance(false)
        .setAutoCollectExceptions(false)
        .setAutoCollectDependencies(false)
        .setAutoCollectConsole(false)
        .start();
```

Başlatmadan sonra bu toplayıcıları devre dışı bırakmak için Yapılandırma nesnesini kullanın: `applicationInsights.Configuration.setAutoCollectRequests(false)`

## <a name="debug"></a>Geliştirici modu
Hata ayıklama sırasında işlem hattı sonuçları hemen görmenize olanak tanıyan hızlandırılmış telemetrinizi sağlamak kullanışlıdır. Size yardımcı ayrıca Al ek ileti telemetri herhangi bir sorun izleme. Uygulamanızı azaltabileceğinden, bir üretim ortamında kapatın.

*C#*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = true;

*Visual Basic*

    TelemetryConfiguration.Active.TelemetryChannel.DeveloperMode = True


## <a name="ikey"></a> Seçili özel telemetri için izleme anahtarını ayarlama
*C#*

    var telemetry = new TelemetryClient();
    telemetry.InstrumentationKey = "---my key---";
    // ...


## <a name="dynamic-ikey"></a> Dinamik izleme anahtarı
Telemetri geliştirme, test ve üretim ortamları ayarlama karıştırmaktan kaçının için [ayrı bir Application Insights kaynakları oluşturma](app-insights-create-new-resource.md) ve ortamına bağlı olarak kendi anahtarlarını değiştirin.

İzleme anahtarını yapılandırma dosyasından almak yerine, kodunuzda ayarlayabilirsiniz. Bir ASP.NET hizmetinde global.aspx.cs gibi bir başlatma yöntemi anahtarını ayarlayın:

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



Web sayfalarındaki, web sunucusunun durumu yerine tam anlamıyla bir betiğe kodlama ayarlamak isteyebilirsiniz. Bir ASP.NET uygulamasında oluşturulan Örneğin, bir Web sayfasında:

*Razor, JavaScript*

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
TelemetryClient gönderilen yanı sıra tüm telemetri veri değerlerini içeren bir içerik özelliğine sahiptir. Standart telemetri modülleri tarafından normal olarak ayarlanır, ancak Ayrıca bunları kendiniz ayarlayabilirsiniz. Örneğin:

    telemetry.Context.Operation.Name = "MyOperationName";

Bu değerlerden herhangi birini kendinize ayarlarsanız, ilgili satırından kaldırmayı göz önünde bulundurun [Applicationınsights.config](app-insights-configuration-with-applicationinsights-config.md), böylece, değerler ve değerlerin standart karıştırılması yok.

* **Bileşen**: uygulama ve sürümü.
* **Cihaz**: uygulamanın çalıştığı cihazın ilgili veriler. (Web apps'te, sunucu veya telemetri gönderen bir istemci cihaz budur.)
* **Instrumentationkey**: telemetri göründüğü Azure Application Insights kaynağı. Bunu Applicationınsights.config genellikle seçilir.
* **Konum**: cihaz coğrafi konumu.
* **İşlem**: web Apps, geçerli HTTP isteği. Diğer uygulama türleri, bu grubu olayları birlikte ayarlayabilirsiniz.
  * **Kimliği**: ilgili öğeleri herhangi bir olay tanılama aramasındaki incelediğinizde, bulabilmesi için farklı olaylara karşılık gelen bir oluşturulan bir değer.
  * **Ad**: bir tanımlayıcı, genellikle bir HTTP isteği URL'si.
  * **SyntheticSource**: değilse null veya boşsa, isteğin kaynak bir robot veya web testi belirlenmiştir gösteren bir dize. Varsayılan olarak, bu hesaplamalar ölçüm Gezgini'nde'den çıkarılır.
* **Özellikler**: gönderilen tüm telemetri veri ile özellikleri. Tek parça * çağrılarında kılınabilir.
* **Oturum**: kullanıcının oturumu. Kimlik, kullanıcı bir süredir etkin olmayan kaldırıldığında, değiştirildiğinde oluşturulan bir değere ayarlanır.
* **Kullanıcı**: kullanıcı bilgileri.

## <a name="limits"></a>Sınırlar
[!INCLUDE [application-insights-limits](../../includes/application-insights-limits.md)]

Veri hızı sınırı ulaşmaktan kaçınmak için kullanın [örnekleme](app-insights-sampling.md).

Veriler tutulur ne kadar süreyle belirlemek için bkz: [veri saklama ve gizlilik](app-insights-data-retention-privacy.md).

## <a name="reference-docs"></a>Başvuru belgeleri
* [ASP.NET başvurusu](https://msdn.microsoft.com/library/dn817570.aspx)
* [Java başvurusu](http://dl.windowsazure.com/applicationinsights/javadoc/)
* [JavaScript başvurusu](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md)
* [Android SDK](https://github.com/Microsoft/ApplicationInsights-Android)
* [iOS SDK](https://github.com/Microsoft/ApplicationInsights-iOS)

## <a name="sdk-code"></a>SDK kodu
* [ASP.NET Core SDK'sı](https://github.com/Microsoft/ApplicationInsights-aspnetcore)
* [ASP.NET 5](https://github.com/Microsoft/ApplicationInsights-dotnet)
* [Windows Server paketleri](https://github.com/Microsoft/applicationInsights-dotnet-server)
* [Java SDK](https://github.com/Microsoft/ApplicationInsights-Java)
* [Node.js SDK’sı](https://github.com/Microsoft/ApplicationInsights-Node.js)
* [JavaScript SDK'sı](https://github.com/Microsoft/ApplicationInsights-JS)
* [Tüm platformlar](https://github.com/Microsoft?utf8=%E2%9C%93&query=applicationInsights)

## <a name="questions"></a>Sorular
* *Hangi özel durumları Track_() çağrıları fırlatabilir?*

    Yok. Try-catch yan tümcelerinde kaydırılmasına gerekmez. SDK'sı sorunla karşılaşırsa, hata ayıklama konsol çıkışında iletileri başlar.SSH ve--tanılama araması'nda iletileri üzerinden--alırsanız.
* *Portaldan veri almak için bir REST API'si var mı?*

    Evet, [veri erişimi API'si](https://dev.applicationinsights.io/). Verileri ayıklamak için diğer yolları [Power BI'a Analytics'ten dışarı](app-insights-export-power-bi.md) ve [sürekli dışarı aktarma](app-insights-export-telemetry.md).

## <a name="next"></a>Sonraki adımlar
* [Arama olayları ve günlükleri](app-insights-diagnostic-search.md)

* [Sorun giderme](app-insights-troubleshoot-faq.md)


