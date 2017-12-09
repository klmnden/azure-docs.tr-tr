---
title: "Azure işlevleri izleme"
description: "Azure Application Insights ile Azure işlevleri işlev yürütme izleme için kullanmayı öğrenin."
services: functions
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/15/2017
ms.author: tdykstra
ms.openlocfilehash: 33d4a193cc3152bfab1f03dde32ad4f1bcb0afe1
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="monitor-azure-functions"></a>Azure işlevleri izleme

## <a name="overview"></a>Genel Bakış 

[Azure işlevleri](functions-overview.md) ile dahili tümleştirmeyi sunar [Azure Application Insights](../application-insights/app-insights-overview.md) işlevleri izleme. Bu makalede telemetri verilerini Application Insights'a gönderme işlevleri yapılandırma gösterilmektedir.

![Uygulama Öngörüler ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

İşlevler de sahip Application Insights kullanmayan yerleşik izleme. Daha fazla veri ve verileri çözümlemek için daha iyi yol sağladığından Application Insights öneririz. Yerleşik izleme hakkında daha fazla bilgi için bkz: [bu makalenin en son](#monitoring-without-application-insights).

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmeyi etkinleştir

Bir işlev uygulaması Application Insights'a veri göndermek, bir Application Insights örneğinin izleme anahtarını bilmesi gerekir. Bu bağlantı kurmak için iki yolla [Azure portal](https://portal.azure.com):

* [İşlev uygulaması oluşturduğunuzda, bağlı bir Application Insights örneği oluşturmayı](#new-function-app).
* [Varolan bir işlev uygulaması bir Application Insights bağlanın](#existing-function-app).
 
### <a name="new-function-app"></a>Yeni bir işlev uygulaması

Application Insights işlev uygulaması üzerinde etkinleştirmek **oluşturma** sayfa:

1. Ayarlama **Application Insights** geçiş **üzerinde**.

2. Seçin bir **uygulama Öngörüler konumu**.

   ![Bir işlev uygulaması oluşturulurken Application Insights etkinleştir](media/functions-monitoring/enable-ai-new-function-app.png)

### <a name="existing-function-app"></a>Varolan işlev uygulaması

İzleme anahtarı edinme ve bir işlev uygulaması kaydedin:

1. Application Insights örneği oluşturun. Uygulama türü ayarlayın **genel**.

   ![Genel tür Application Insights örneğini oluşturma](media/functions-monitoring/ai-general.png)

2. İzleme anahtarını kopyalama **Essentials** Application Insights örneğinin sayfası. Almak için görüntülenen anahtar değeri bitiş vurgulu bir **kopyalamak için tıklayın** düğmesi.

   ![Application Insights izleme anahtarını kopyalama](media/functions-monitoring/copy-ai-key.png)

1. İşlev uygulamasının **uygulama ayarları** sayfasında [bir uygulama ayarı ekleme](functions-how-to-use-azure-function-app-settings.md#settings) tıklayarak **yeni ayar Ekle**. Yeni bir ayar appınsıghts_ınstrumentatıonkey adlandırın ve kopyalanan izleme anahtarını yapıştırın.

   ![Uygulama ayarlarına izleme anahtarı Ekle](media/functions-monitoring/add-ai-key.png)

1. **Kaydet** düğmesine tıklayın.

## <a name="view-telemetry-data"></a>Telemetri verileri görüntüleme

Portalda işlevi uygulamasından bağlı Application Insights örneğine gitmek için seçin **Application Insights** işlevi uygulamanın bağlantısını **genel bakış** sayfası.

Application Insights kullanma hakkında daha fazla bilgi için bkz: [Application Insights belgelerine](https://docs.microsoft.com/azure/application-insights/). Bu bölüm, verileri Application Insights'ta görüntülemenin nasıl bazı örnekler göstermektedir. Application Insights ile bilginiz varsa, doğrudan gidebilirsiniz [yapılandırma ve telemetri verilerini özelleştirme hakkında bölümleri](#configure-categories-and-log-levels).

İçinde [ölçüm Gezgini](../application-insights/app-insights-metrics-explorer.md), grafikler oluşturabilirsiniz ve Uyarıları temel ölçümleri gibi işlev çağrılarını, yürütme zamanı ve başarı oranı sayısı.

![Ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

Üzerinde [hataları](../application-insights/app-insights-asp-net-exceptions.md) sekmesinde grafikler oluşturabilirsiniz ve uyarılar, özel durumlar işlevi hataları ve sunucu tabanlı. **İşlem adı** işlev adı. Hataları bağımlılıklar uygulamadan sürece gösterilmiyor [özel telemetri](#custom-telemetry-in-c-functions) bağımlılıklar için.

![hataları](media/functions-monitoring/failures.png)

Üzerinde [performans](../application-insights/app-insights-performance-counters.md) sekmesinde, performans sorunlarını analiz edebilirsiniz.

![Performans](media/functions-monitoring/performance.png)

**Sunucuları** sekmesi, kaynak kullanımı ve sunucu başına gösterir. Bu veriler işlevleri temel kaynaklarınıza nerede bogging senaryoları hata ayıklama için yararlı olabilir. Sunucuları denir **bulut rolü örnekleri**.

![Sunucular](media/functions-monitoring/servers.png)

[Ölçümleri bir canlı akışı](../application-insights/app-insights-live-stream.md) sekmesi, gerçek zamanlı olarak oluşturulduğundan ölçüm verilerini gösterir.

![Canlı akış](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Telemetri verileri Sorgulama

[Uygulama Öngörüler Analytics](../application-insights/app-insights-analytics.md) erişmenizi tüm telemetri verilerini bir veritabanındaki tabloların biçiminde. Analytics ayıklanması, düzenleme ve verileri görselleştirmek için bir sorgu dili sağlar.

![Analytics seçin](media/functions-monitoring/select-analytics.png)

![Analizi örneği](media/functions-monitoring/analytics-traces.png)

Aşağıda, bir sorgu örnek verilmiştir. Bu, son 30 dakikadan istek çalışan sayısı dağılımını gösterir.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Kullanılabilir tabloların gösterilen **şema** sol bölmenin sekmesi. Aşağıdaki tablolarda işlev çağrılarını tarafından oluşturulan veri bulabilirsiniz:

* **İzlemeler** -günlükleri işlevi kod ve çalışma zamanı tarafından oluşturulmuş.
* **istekleri** -her işlev çağrısı için bir tane.
* **özel durumlar** - çalışma zamanı tarafından karşılaşılan özel durumlar.
* **customMetrics** -sayısı başarılı ve başarısız çağrılarını, başarı oranı, süre.
* **customEvents** -olayları izlenen çalışma zamanı tarafından örneğin: bir işlev tetiklemek HTTP istekleri.
* **performans sayaçları** -işlevleri çalıştıran sunucularının performansı hakkında bilgi.

Diğer tablolar kullanılabilirlik testleri ve istemci/tarayıcı telemetri içindir. Veri bunlara eklemek için özel telemetri uygulayabilirsiniz.

Bazı işlevler özgü veriler her tablo içinde olan bir `customDimensions` alan.  Örneğin, aşağıdaki sorgu günlük düzeyi tüm izlemeler alır `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı sağlar `customDimensions.LogLevel` ve `customDimensions.Category`. Ek alanlar işlevi kodunuzda yazma günlüklerine sağlayabilir. Bkz: [günlüğü yapılandırılmış](#structured-logging) bu makalenin ilerisinde yer.

## <a name="configure-categories-and-log-levels"></a>Ve düzeyleri günlük kategorileri yapılandırabilirsiniz

Application Insights herhangi bir özel yapılandırma kullanabilirsiniz ancak varsayılan yapılandırmayı yüksek miktarda veriyi neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri uç isabet. Bu makalenin sonraki bölümlerinde, yapılandırma ve özelleştirme işlevlerinizi Application Insights'a gönderme veri gösterilmektedir.

### <a name="categories"></a>Kategoriler

Azure işlevleri Günlükçü içeren bir *kategori* her oturum için. Çalışma zamanı kodu veya işlevi kodunuzu hangi kısmına günlük yazdı kategori gösterir. 

İşlevler çalışma zamanı "Konak" ile başlayan bir kategoriye sahip günlükler oluşturur. Örneğin, "işlev başlatıldı," "işlevi yürütülen" ve "işlev tamamlandı" günlükleri "Host.Executor" kategorisi vardır. 

İşlev kodunuzda günlüklerini yazma, kendi kategori "İşlev" olur.

### <a name="log-levels"></a>Günlük düzeyleri

Azure işlevleri Günlükçü de içeren bir *günlük düzeyi* her günlüğü ile. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) bir numaralandırma ve tamsayı kodu göreli önemi gösterir:

|LogLevel    |Kod|
|------------|---|
|İzleme       | 0 |
|Hata ayıklama       | 1 |
|Bilgi | 2 |
|Uyarı     | 3 |
|Hata       | 4 |
|Kritik    | 5 |
|None        | 6 |

Günlük düzeyi `None` sonraki bölümde açıklanmıştır. 

### <a name="configure-logging-in-hostjson"></a>İçinde Host.JSON oturum açmayı Yapılandır

*Host.json* bir işlev uygulaması Application Insights'a gönderir ne kadar günlük dosyası yapılandırır. Her kategori için göndermek için en az günlük düzeyini gösterir. Örnek aşağıda verilmiştir:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Bu örnekte aşağıdaki kuralları ayarlar:

1. Kategori "Host.Results" veya "İşlev" ile günlükleri için yalnızca gönderme `Error` düzeyi ve yukarıdaki Application Insights için. Günlükleri `Warning` düzey ve aşağıda göz ardı edilir.
2. Ana bilgisayar kategorisiyle günlükleri için. Toplayıcı, yalnızca gönderme `Information` düzeyi ve yukarıdaki Application Insights için. Günlükleri `Debug` düzey ve aşağıda göz ardı edilir.
3. Diğer tüm günlükler için yalnızca gönderme `Information` düzeyi ve yukarıdaki Application Insights için.

Kategori değeri *host.json* aynı değerle başlayan tüm kategorileri için günlük kaydını denetler. Örneğin, "içinde ana bilgisayar" *host.json* "Host.General", "Host.Executor", "Host.Results" ve benzeri için günlüğe kaydedilmesini denetler.

Varsa *host.json* aynı dize ile başlayan birden çok kategori içeren uzun olanları ilk eşleştirilir. Örneğin, çalışma zamanı "oturum Host.Aggregator" dışında her şeyi istediğinizi varsayalım `Error` düzeyi, "Host.Aggregator" günlüklerine sırasında `Information` düzeyi:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host": "Error",
        "Function": "Error",
        "Host.Aggregator": "Information"
      }
    }
  }
}
```

Bir kategori için tüm günlükleri gizlemek için günlük düzeyini kullanabilirsiniz `None`. Hiçbir günlük bu kategoriyi yazılır ve yukarıdaki hiçbir günlük düzeyi yok.

Aşağıdaki bölümlerde, çalışma zamanı oluşturur günlükleri ana kategorileri açıklanmaktadır. 

### <a name="category-hostresults"></a>Kategori Host.Results

Bu günlükler "istekleri olarak" Application Insights'ta gösterir. Bunlar, başarı veya başarısızlık işlev gösterir.

![İstekleri grafik](media/functions-monitoring/requests-chart.png)

Bu günlüklerin adresindeki yazılır `Information` düzey, bunu, filtre uygularsanız `Warning` veya üstünü bu verilerin hiçbirini göremezsiniz.

### <a name="category-hostaggregator"></a>Kategori Host.Aggregator

Bu günlükler üzerinden sayısı ve işlev çağrılarını ortalamalar sağlayan bir [yapılandırılabilir](#configure-the-aggregator) dönem süre. Varsayılan süre 30 saniye veya 1.000 sonuçları, hangisi önce gelirse. 

Günlükleri kullanılabilir **customMetrics** Application Insights tablosunda. Örnekler sayı çalıştırır, başarı oranı ve süre.

![customMetrics sorgu](media/functions-monitoring/custom-metrics-query.png)

Bu günlüklerin adresindeki yazılır `Information` düzey, bunu, filtre uygularsanız `Warning` veya üstünü bu verilerin hiçbirini göremezsiniz.

### <a name="other-categories"></a>Diğer kategorileri

Listelenen tüm günlükleri zaten dışındaki kategorileri için kullanılabilir olan **izlemeleri** Application Insights tablosunda.

![izlemeler sorgu](media/functions-monitoring/analytics-traces.png)

"Ana" ile başlayan kategorileri tüm günlüklerin işlevleri çalışma zamanı tarafından yazılır. "İşlevi kullanmaya" ve "İşlevi tamamlandı" günlükleri "Host.Executor" kategorisi vardır. Bu günlükler başarılı çalıştırmaları için olan `Information` düzeyi; adresindeki özel durumları günlüğe `Error` düzeyi. Çalışma zamanı da oluşturur `Warning` düzey günlükleri, örneğin: sıraya zararlı kuyruğuna gönderilen ileti.

Günlükleri işlevi kodunuz tarafından yazılan "İşlevi" kategorisi vardır ve herhangi bir günlük düzeyi olabilir.

## <a name="configure-the-aggregator"></a>Toplayıcı yapılandırma

Önceki bölümde belirtildiği gibi çalışma zamanı bir süre boyunca işlevi yürütmeleri hakkındaki verileri toplar. Varsayılan süre 30 saniyedir veya 1.000 çalıştırır, hangisi daha önce gelir. Bu ayarı yapılandırabilirsiniz *host.json* dosya.  Örnek aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırın

Application Insights sahip bir [örnekleme](../application-insights/app-insights-sampling.md) çok fazla telemetri verilerini bazen yoğun yük oluşturan koruma özelliği. Belirtilen oranla telemetri öğe sayısını aştığında, rastgele gelen öğelerin bazıları yoksaymak Application Insights başlatır. Örnekleme içinde yapılandırabilirsiniz *host.json*.  Örnek aşağıda verilmiştir:

```json
{
  "applicationInsights": {
    "sampling": {
      "isEnabled": true,
      "maxTelemetryItemsPerSecond" : 5
    }
  }
}
```

## <a name="write-logs-in-c-functions"></a>C# işlevlerde günlüklerini yazma

Günlükleri Application Insights içindeki olarak görünür işlevi kodunuzu yazabilirsiniz.

### <a name="ilogger"></a>ILogger

Kullanım bir [ILogger](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) işlevlerinizi yerine parametresinde bir `TraceWriter` parametresi. Kullanılarak oluşturulan günlükleri `TraceWriter` uygulama Öngörüler'e gidin, ancak `ILogger` yapmanıza olanak sağlayan [günlüğü yapılandırılmış](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

İle bir `ILogger` çağırmanız nesne `Log<level>` [ILogger genişletme yöntemi](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loggerextensions#Methods_) günlükleri oluşturmak için. Örneğin, aşağıdaki yazma kod `Information` "İşlev" kategorisiyle günlükleri.

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Yapılandırılmış günlüğe kaydetme

Hangi parametreler günlük iletisinde kullanılan yer tutucuları, bunların adları sırasını belirler. Örneğin, aşağıdaki kodu olduğunu varsayalım:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Aynı ileti dizesi tutmak ve parametre sırasını tersine, sonuçta elde edilen ileti metni yanlış yerlerde değerleri gerekir.

Yapılandırılmış günlük kaydı yapabilmesi için yer tutucuları bu şekilde işlenir. Application Insights parametresi ad-değer çiftleri ileti dizesi ek olarak depolar. İleti bağımsız değişkenler üzerinde sorgulayabilirsiniz alanları hale sonucudur.

Örneğin, alan sorgu Günlükçü yöntem çağrısı önceki örnek gibi görünüyorsa, `customDimensions.prop__rowKey`. `prop__` Öneki hiçbir çakışmaları çalışma zamanı ekler ve işlev kodunuzu alanları alanlar arasında olduğundan emin olmak için ekler eklenir.

Alan başvurarak üzerinde özgün ileti dizesi sorgulayabilirsiniz `customDimensions.prop__{OriginalFormat}`.  

Bir örnek JSON gösterimi işte `customDimensions` verileri:

```json
{
  customDimensions: {
    "prop__{OriginalFormat}":"C# Queue trigger function processed: {message}",
    "Category":"Function",
    "LogLevel":"Information",
    "prop__message":"c9519cbf-b1e6-4b9b-bf24-cb7d10b1bb89"
  }
}
```

### <a name="logging-custom-metrics"></a>Özel ölçümleri günlüğe kaydetme  

C# betik işlevlerde, kullandığınız `LogMetric` genişletme yöntemi `ILogger` özel ölçümleri Application Insights'ta oluşturmak için. Örnek yöntem çağrısı şöyledir:

```csharp
logger.LogMetric("TestMetric", 1234); 
```

Bu kod bir arama alternatifidir `TrackMetric` kullanarak [.NET için Application Insights API'si](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevleri günlüklerini yazma

Node.js işlevlerini kullanmak `context.log` günlüklerini yazma izni. Yapılandırılmış günlük kaydı etkinleştirilmedi.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Özel ölçümleri günlüğe kaydetme  

Node.js işlevlerde kullandığınız `context.log.metric` özel ölçümleri Application Insights'ta oluşturmak için yöntemi. Örnek yöntem çağrısı şöyledir:

```javascript
context.log.metric("TestMetric", 1234); 
```

Bu kod bir arama alternatifidir `trackMetric` kullanarak [Application Insights için Node.js SDK'sı](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>C# işlevlerinde özel telemetri

Kullanabileceğiniz [Microsoft.ApplicationInsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) özel telemetri verilerini Application Insights'a gönderme için NuGet paketi.

İşte bir örnek kullanan C# kod [özel telemetri API'si](../application-insights/app-insights-api-custom-events-metrics.md). Örneğin, bir .NET sınıf kitaplığı için olmakla birlikte Application Insights kod C# komut dosyası için aynıdır.

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;
using Microsoft.Azure.WebJobs;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Linq;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        private static string key = TelemetryConfiguration.Active.InstrumentationKey = 
            System.Environment.GetEnvironmentVariable(
                "APPINSIGHTS_INSTRUMENTATIONKEY", EnvironmentVariableTarget.Process);

        private static TelemetryClient telemetry = 
            new TelemetryClient() { InstrumentationKey = key };

        [FunctionName("HttpTrigger2")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
            HttpRequestMessage req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // parse query parameter
            string name = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Get request body
            dynamic data = await req.Content.ReadAsAsync<object>();

            // Set name to query string or body data
            name = name ?? data?.name;

            telemetry.Context.Operation.Id = context.InvocationId.ToString();
            telemetry.Context.Operation.Name = "cs-http";
            if (!String.IsNullOrEmpty(name))
            {
                telemetry.Context.User.Id = name;
            }
            telemetry.TrackEvent("Function called");
            telemetry.TrackMetric("Test Metric", DateTime.Now.Millisecond);
            telemetry.TrackDependency("Test Dependency", 
                "swapi.co/api/planets/1/", 
                start, DateTime.UtcNow - start, true);

            return name == null
                ? req.CreateResponse(HttpStatusCode.BadRequest, 
                    "Please pass a name on the query string or in the request body")
                : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
        }
    }
}
```

Çağrı yok `TrackRequest` veya `StartOperation<RequestTelemetry>`, yinelenen istekleri için bir işlev çağrısını görürsünüz.  İşlevler çalışma zamanı istekleri otomatik olarak izler.

Ayarlama `telemetry.Context.Operation.Id` çağırma kimliği, işlevi her başlatıldığında. Verilen işlev çağrısı için tüm telemetri öğeleri ilişkilendirmenize olanak sağlar.

```cs
telemetry.Context.Operation.Id = context.InvocationId.ToString();
```

## <a name="custom-telemetry-in-javascript-functions"></a>Özel telemetri JavaScript işlevleri

[Application Insights Node.js SDK'sı](https://www.npmjs.com/package/applicationinsights) şu anda beta aşamasındadır. Özel telemetri Application Insights'a gönderir bazı örnek kod aşağıda verilmiştir:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup();
const client = appInsights.defaultClient;

module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    client.trackEvent({name: "my custom event", tagOverrides:{"ai.operation.id": context.invocationId}, properties: {customProperty2: "custom property value"}});
    client.trackException({exception: new Error("handled exceptions can be logged with this method"), tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackMetric({name: "custom metric", value: 3, tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackTrace({message: "trace message", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackDependency({target:"http://dbname", name:"select customers proc", data:"SELECT * FROM Customers", duration:231, resultCode:0, success: true, dependencyTypeName: "ZSQL", tagOverrides:{"ai.operation.id": context.invocationId}});
    client.trackRequest({name:"GET /customers", url:"http://myserver/customers", duration:309, resultCode:200, success:true, tagOverrides:{"ai.operation.id": context.invocationId}});

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```

`tagOverrides` Parametre kümeleri `operation_Id` işlev çağırma kimliği Bu ayar, tüm verilen işlev çağrısı için otomatik olarak oluşturulur ve özel telemetri ilişkilendirmenize olanak sağlar.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="dependencies"></a>Bağımlılıklar

Diğer hizmetlere işlevi olan bağımlılıkları otomatik olarak gösterme, ancak bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnek kodda [C# özel telemetri bölüm](#custom-telemetry-in-c-functions) gösterir nasıl. Örnek kod sonuçlanıyor bir *uygulama eşlemesi* Application ınsights'ta aşağıdakine benzer:

![Uygulama eşlemesi](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Rapor sorunları

İşlevlerinde Application Insights tümleştirme ile ilgili bir sorunu bildirmek için veya bir öneri ya da isteği yapmak için [Github'da bir sorun oluştur](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitoring-without-application-insights"></a>Application Insights izleme

Daha fazla veri ve verileri çözümlemek için daha iyi yol sağladığından işlevleri izlemek için Application Insights öneririz. Ancak, aynı zamanda telemetri ve günlük verilerini Azure portal sayfalarına bir işlev uygulaması için bulabilirsiniz. 

Seçin **İzleyici** bir işlev ve sekmesini işlevi yürütmeleri listesini alın. Bir işlev yürütme süresi, giriş verilerini, hataları ve ilişkili günlük dosyalarını gözden geçirmek için seçin.

> [!IMPORTANT]
> Kullanırken [barındırma planı tüketim](functions-overview.md#pricing) Azure işlevleri için **izleme** işlev uygulaması parçasında herhangi bir veri gösterme. Platform dinamik olarak ölçeklendirir ve işlem örnekleri tarafından yönetilir olmasıdır. Bu ölçümler tüketim plan üzerinde anlamlı değildir.

### <a name="real-time-monitoring"></a>Gerçek zamanlı izleme

Gerçek zamanlı izleme kullanılabilir tıklayarak **canlı olay akışı** işlevi üzerinde **İzleyici** sekmesi. Canlı olay akışında bir grafikte yeni bir tarayıcı sekmesinde görüntülenir.

> [!NOTE]
> Verilerinizi doldurulmalıdır başarısız olmasına neden olabilecek bilinen bir sorun yoktur. Canlı olay akışının içeren tarayıcı sekmesini kapatın ve ardından gerekebilir **canlı olay akışı** olay akışı verilerinizi düzgün bir şekilde doldurmak yeniden izin vermek için. 

Bu istatistikler gerçek zamanlı ancak gerçek yürütme verileri Grafikleme yaklaşık 10 saniye gecikme süresi olabilir.

### <a name="monitor-log-files-from-a-command-line"></a>Bir komut satırından izleme günlük dosyaları

Yerel iş istasyonunda Azure komut satırı arabirimi (CLI) 1.0 veya PowerShell kullanarak bir komut satırı oturumu için günlük dosyalarını akışını sağlayabilirsiniz.

### <a name="monitor-function-app-log-files-with-the-azure-cli-10"></a>Azure CLI 1.0 ile işlevi uygulama günlük dosyaları İzle

Başlamak için [Azure CLI 1.0 yüklemeyi](../cli-install-nodejs.md) ve [Azure'da oturum aç](../xplat-cli-connect.md).

Klasik Hizmet Yönetimi modunu etkinleştirmek, aboneliğinizi seçin ve günlük dosyalarını akış için aşağıdaki komutları kullanın:

```
azure config mode asm
azure account list
azure account set <subscriptionNameOrId>
azure site log tail -v <function app name>
```

### <a name="monitor-function-app-log-files-with-powershell"></a>PowerShell ile işlevi uygulama günlük dosyaları İzle

Başlamak için [Azure PowerShell'i yükleme ve yapılandırma](/powershell/azure/overview).

Azure hesabınızda eklemek, aboneliğinizi seçin ve günlük dosyalarını akışla aktarmak için aşağıdaki komutları kullanın:

```
PS C:\> Add-AzureAccount
PS C:\> Get-AzureSubscription
PS C:\> Get-AzureSubscription -SubscriptionName "MyFunctionAppSubscription" | Select-AzureSubscription
PS C:\> Get-AzureWebSiteLog -Name MyFunctionApp -Tail
```

Daha fazla bilgi için bkz: [nasıl yapılır: akış günlükleri web uygulamaları için](../app-service/web-sites-enable-diagnostic-log.md#streamlogs). 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Application Insights hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/application-insights/)

> [!div class="nextstepaction"]
> [İşlevlerini kullanan günlük framework hakkında daha fazla bilgi edinin](https://docs.microsoft.com/aspnet/core/fundamentals/logging?tabs=aspnetcore2x)
