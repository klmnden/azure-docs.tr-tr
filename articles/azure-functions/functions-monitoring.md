---
title: Azure İşlevlerini İzleme
description: Azure Application ınsights'ı Azure işlevleri ile işlevi yürütme izleme için kullanmayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/15/2017
ms.author: glenga
ms.openlocfilehash: 66d04ca93a79f4d9cdd9f162c6cd3210ae35f4d2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902714"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

## <a name="overview"></a>Genel Bakış 

[Azure işlevleri](functions-overview.md) ile yerleşik tümleştirme sunan [Azure Application Insights](../application-insights/app-insights-overview.md) işlevleri izleme. Bu makalede, Application Insights'a telemetri verileri göndermek için işlevleri yapılandırmak gösterilmektedir.

![Application Insights Ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

İşlevler ayrıca sahip [yerleşik izleme, Application Insights kullanmaz](#monitoring-without-application-insights). Daha fazla veri ve verileri çözümlemek için daha iyi bir yolu sağladığından Application ınsights'ı öneririz.

## <a name="application-insights-pricing-and-limits"></a>Application Insights fiyatlandırma ve limitler

Application Insights Tümleştirmesi ile işlev uygulamaları ücretsiz deneyebilirsiniz. Ancak, ne kadar veri ücretsiz işlenebilecek bir günlük sınır ve test sırasında bu sınırı neden olabilir. Azure portalı ve e-posta bildirimleri, sizin bölümüyle iletişime geçerken günlük sınırınızı sağlar.  Ancak bu uyarıları kaçırmayın ve sınırına, Application Insights sorgularda yeni günlükler görünmez. Bu nedenle gereksiz sorun giderme süresini önlemek için sınırını unutmayın. Daha fazla bilgi için [Application ınsights fiyatlandırma ve veri hacmini yönetme](../application-insights/app-insights-pricing.md).

## <a name="enable-app-insights-integration"></a>App Insights tümleştirmesini etkinleştirme

Bir işlev uygulaması için Application Insights veri göndermek, bir Application Insights kaynağına ait izleme anahtarını bilmesi gerekir. Anahtarı appınsıghts_ınstrumentatıonkey adlı bir uygulama ayarı sağlanması gerekir.

Bu bağlantıda ayarlayabilirsiniz [Azure portalında](https://portal.azure.com):

* [Otomatik olarak yeni bir işlev uygulaması için](#new-function-app)
* [App Insights kaynağı el ile bağlanma](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Yeni işlev uygulaması

1. İşlev uygulamasını Git **Oluştur** sayfası.

1. Ayarlama **Application Insights** geçiş **üzerinde**.

2. Seçin bir **Application Insights konumu**.

   İşlev uygulamanızın bölgesine en yakın olan bölgeyi seçin, bir [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) verilerinizin depolanmasını istediğiniz.

   ![Bir işlev uygulaması oluştururken Application Insights'ı etkinleştir](media/functions-monitoring/enable-ai-new-function-app.png)

3. Diğer gerekli bilgileri girin.

1. **Oluştur**’u seçin.

Sonraki adım [yerleşik günlüğünü devre dışı](#disable-built-in-logging).

### <a name="manually-connect-an-app-insights-resource"></a>App Insights kaynağı el ile bağlanma 

1. Application Insights kaynağı oluşturun. Uygulama türü ayarlayın **genel**.

   ![Genel tür, bir Application Insights kaynağı oluşturma](media/functions-monitoring/ai-general.png)

2. İzleme anahtarını kopyalama **Essentials** Application Insights kaynağına ait sayfa. Görüntülenen anahtar değerini almak için sonuna üzerinde vurgulu bir **kopyalamak için tıklayın** düğmesi.

   ![Application Insights izleme anahtarını kopyalama](media/functions-monitoring/copy-ai-key.png)

1. İşlev uygulamasının **uygulama ayarları** sayfasında [bir uygulama ayarı ekleme](functions-how-to-use-azure-function-app-settings.md#settings) tıklayarak **yeni ayar Ekle**. Yeni ayar appınsıghts_ınstrumentatıonkey adlandırın ve kopyalanan izleme anahtarını yapıştırın.

   ![İzleme anahtarını uygulama ayarları Ekle](media/functions-monitoring/add-ai-key.png)

1. **Kaydet**’e tıklayın.

## <a name="disable-built-in-logging"></a>Yerleşik günlük devre dışı bırakma

Application Insights'ı etkinleştirirseniz, devre dışı bırakmanızı öneririz [Azure depolama kullanan yerleşik günlük](#logging-to-storage). Yerleşik günlük hafif iş yükleri ile test etmek için kullanışlıdır ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izlemek için Application Insights önerilir. Günlük kaydı, üretimde yerleşik günlük kaydı kullandıysanız, Azure depolama kapasitesi azaltıldığı eksik olabilir.

Yerleşik günlük kaydetme devre dışı bırakmak için silme `AzureWebJobsDashboard` uygulama ayarı. Uygulama ayarları Azure portalında silme hakkında daha fazla bilgi için bkz: **uygulama ayarları** bölümünü [bir işlev uygulaması yönetme](functions-how-to-use-azure-function-app-settings.md#settings). Uygulama ayarı silmeden önce aynı işlev uygulamasında var olan bir işlev yok, Azure depolama Tetikleyicileri veya bağlamaları için kullandığınızdan emin olun.

## <a name="view-telemetry-in-monitor-tab"></a>İzleyici sekmesi içinde telemetri görüntüleme

Application Insights tümleştirmesini ayarlama önceki bölümlerde gösterildiği gibi ayarladıktan sonra telemetri verileri görüntüleyebilirsiniz **İzleyici** sekmesi.

1. Application Insights yapılandırılmış ve seçip sonra en az bir kez çalışan bir işlev işlevi uygulaması sayfasında seçin **İzleyici** sekmesi.

   ![İzleyici sekmesi seçin](media/functions-monitoring/monitor-tab.png)

2. Seçin **Yenile** düzenli aralıklarla işlev çağrılarını listesinde görünene kadar.

   Bu listenin görünecek şekilde iletilmesi için sunucunun telemetri istemci toplu verileri nedeniyle 5 dakika kadar sürebilir. (Bu gecikme uygulanmaz [Canlı ölçümleri Stream](../application-insights/app-insights-live-stream.md). Günlükleri doğrudan sayfaya aktarılır, böylece sayfa yüklediğinizde bu hizmeti işlevleri ana bilgisayara bağlanır.)

   ![Çağrılarını listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

2. Belirli işlev çağrısı için günlükleri görmek için seçin **tarih** bu çağrı için sütun bağlantı.

   ![Çağrı Ayrıntıları bağlantı](media/functions-monitoring/invocation-details-link-ai.png)

   Bu çağrı için günlük çıktısı, yeni bir sayfa görünür.

   ![Çağrı Ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

Her iki sayfa (çağırma liste ve Ayrıntılar) verileri alır. Application Insights Analytics sorgunuzun Bağla:

![Application Insights'ta çalıştır](media/functions-monitoring/run-in-ai.png)

![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Bu sorguları, çağırma listesi son 30 gün, 20'den fazla satır sınırlı olduğunu görebilirsiniz (`where timestamp > ago(30d) | take 20`) ve çağırma ayrıntıları listesi hiçbir sınır son 30 gündür.

Daha fazla bilgi için [sorgu telemetri verilerini](#query-telemetry-data) bu makalenin ilerleyen bölümlerinde.

## <a name="view-telemetry-in-app-insights"></a>App ınsights telemetrisini görüntüleme

Azure portalında bir işlev uygulamasından Application Insights'ı açmak için seçmeniz **Application Insights** bağlantısını **özellikler yapılandırıldı** işlevi uygulamanın bir bölümünü **genelbakış** sayfası.

![Application Insights bağlantı genel bakış sayfasında](media/functions-monitoring/ai-link.png)


Application Insights'ı kullanma hakkında daha fazla bilgi için bkz: [Application Insights belgeleri](https://docs.microsoft.com/azure/application-insights/). Bu bölümde, Application Insights verilerini görüntüleme ile ilgili bazı örnekler gösterilmektedir. Application Insights ile bilginiz varsa, doğrudan gidebilirsiniz [yapılandırma ve telemetri verilerini özelleştirme hakkında bölümlere](#configure-categories-and-log-levels).

İçinde [ölçüm Gezgini](../application-insights/app-insights-metrics-explorer.md)grafikler oluşturabilirsiniz ve uyarılar gibi ölçümlere dayalı olarak işlev çağrılarını, yürütme süresi ve başarı oranı sayısı.

![Ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

Üzerinde [hataları](../application-insights/app-insights-asp-net-exceptions.md) sekmesinde grafikler oluşturabilir ve uyarılar, özel durumlar işlevi hataları ve sunucuda temel. **İşlem adı** işlev adıdır. Bağımlılıklar hataları uyguladığınız sürece gösterilmiyor [özel telemetri](#custom-telemetry-in-c-functions) bağımlılıklar için.

![Başarısızlıklar](media/functions-monitoring/failures.png)

Üzerinde [performans](../application-insights/app-insights-performance-counters.md) sekmesinde, performans sorunlarını çözümleyebilirsiniz.

![Performans](media/functions-monitoring/performance.png)

**Sunucuları** sekmesi kaynak kullanımı ve sunucu başına aktarım hızını gösterir. Bu veri, İşlevler, temel alınan kaynakları burada bogging senaryoları hata ayıklama için yararlı olabilir. Sunucuları denir **bulut rolü örnekleri**.

![Sunucular](media/functions-monitoring/servers.png)

[Canlı ölçümleri Stream](../application-insights/app-insights-live-stream.md) sekmesi, gerçek zamanlı olarak oluşturulduğundan ölçüm verilerini gösterir.

![Canlı akış](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Telemetri verileri Sorgulama

[Application Insights Analytics](../application-insights/app-insights-analytics.md) erişmenizi tüm telemetri verilerini bir veritabanındaki tabloları biçiminde. Analytics ayıklanması, düzenleme ve veri görselleştirme için bir sorgu dili sağlar.

![Analytics seçin](media/functions-monitoring/select-analytics.png)

![Analiz örneği](media/functions-monitoring/analytics-traces.png)

Bir sorgu örneği aşağıda verilmiştir. Bu, son 30 dakika üzerinden çalışan başına istek dağılımını gösterir.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Kullanılabilir tablolar gösterilen **şema** sol bölmenin sekmesi. Aşağıdaki tablolarda işlev çağrılarını tarafından oluşturulan verileri bulabilirsiniz:

* **izlemeleri** -günlükleri ve işlev kodunu çalışma zamanı tarafından oluşturuldu.
* **istekleri** -her işlev çağrısı için bir tane.
* **özel durumlar** - çalışma zamanı tarafından oluşturulan özel durumlar.
* **customMetrics** -sayısı başarılı ve başarısız olan çağrılar, başarı oranı, süre.
* **customEvents** -olayları izlenen çalışma zamanında, örneğin: bir işlev tetiklemek HTTP istekleri.
* **performanceCounters** -işlevleriniz gerektirdikçe sunucularının performansı hakkında bilgi.

Kullanılabilirlik testleri ve istemci/tarayıcı telemetrisi diğer tablolara içindir. Veri eklemek üzere özel telemetri uygulayabilirsiniz.

Bazı işlevler özgü verileri her tabloda olduğu bir `customDimensions` alan.  Örneğin, aşağıdaki sorgu günlüğü düzeyine sahip tüm izlemeleri alır `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı sağlar `customDimensions.LogLevel` ve `customDimensions.Category`. İşlev kodunuzu yazdığınız günlüklerine ek alanlar sağlayabilir. Bkz: [günlük yapılandırılmış](#structured-logging) bu makalenin ilerleyen bölümlerinde.

## <a name="configure-categories-and-log-levels"></a>Kategorileri yapılandırmadan ve oturum düzeyleri

Application Insights herhangi özel bir yapılandırma kullanabilirsiniz, ancak varsayılan yapılandırmanın yüksek hacimli verileri neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri sınırınıza neden olabilir. Bu makalenin geri kalanında, yapılandırma ve özelleştirme işlevlerinizi Application Insights'a gönderme verileri gösterilmektedir.

### <a name="categories"></a>Kategoriler

Azure işlevleri Günlükçü içeren bir *kategori* her günlük için. Kategori günlük çalışma zamanı kodu veya işlev kodunuzu hangi kısmını yazdı gösterir. 

İşlevler çalışma zamanı ile başlayan bir kategori "Ana" sahip olan günlükler oluşturur. Örneğin, "işlevi çalışmaya," "işlevin yürütüldüğü" ve "işlev tamamlandı" günlükleri "Host.Executor" kategorisine sahip. 

İşlev kodunuzda günlüklerini yazma kategorilerine "İşlev" olur.

### <a name="log-levels"></a>Günlük düzeyleri

Azure işlevleri Günlükçü de içeren bir *günlük düzeyi* her günlük. [LogLevel](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.loglevel#Microsoft_Extensions_Logging_LogLevel) bir sabit listesidir ve tamsayı kodu göreli önemi gösterir:

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

### <a name="configure-logging-in-hostjson"></a>Host.JSON içinde açmayı Yapılandır

*Host.json* bir işlev uygulaması, Application Insights'a gönderir ne kadar günlük dosyası oluşturur. Her kategori için göndermek için en düşük günlük düzeyi belirtin. Bir örneği aşağıda verilmiştir:

```json
{
  "logger": {
    "categoryFilter": {
      "defaultLevel": "Information",
      "categoryLevels": {
        "Host.Results": "Error",
        "Function": "Error",
        "Host.Aggregator": "Trace"
      }
    }
  }
}
```

Bu örnekte, aşağıdaki kurallar ayarlar:

1. Kategori "Host.Results" veya "İşlev" ile günlükleri için yalnızca gönderme `Error` düzeyi ve üzeri için Application Insights. Günlüklerinde `Warning` düzeyi ve aşağıda göz ardı edilir.
2. Host.Aggregator kategorili günlükleri için Application Insights için tüm günlükleri gönderin. `Trace` Günlük düzeyi ne bazı günlükçüleri çağrısı ile aynı olup `Verbose`, ancak `Trace` içinde *host.json* dosya.
3. Diğer tüm günlükler için yalnızca gönderme `Information` düzeyi ve üzeri için Application Insights.

Kategori değeri *host.json* günlük kaydı için aynı değeri ile başlayan tüm kategorileri denetler. Örneğin, "içinde ana bilgisayar" *host.json* "Host.General", "Host.Executor", "Host.Results" ve benzeri için günlüğe kaydetmeyi denetler.

Varsa *host.json* aynı dize ile başlayan birden çok kategori içeren uzun olanlarla ilk eşleştirilir. Örneğin, çalışma zamanı günlüğe kaydetmek için "Host.Aggregator" dışında her şeyi istediğinizi varsayalım `Error` düzeyi, ancak isterseniz "Host.Aggregator günlüğe kaydetmek için" `Information` düzeyi:

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

Bir kategori için tüm günlükleri bastırmak için günlük düzeyi kullanabilirsiniz `None`. Günlük, bu kategoriye yazılır ve yukarıdaki hiçbir günlük düzeyi yoktur.

Aşağıdaki bölümlerde, çalışma zamanının oluşturduğu günlükleri ana kategorileri açıklanmaktadır. 

### <a name="category-hostresults"></a>Kategori Host.Results

Bu günlükler, Application Insights'ta "requests" gösterir. Bunlar, başarı veya başarısızlık işlev gösterir.

![İstekleri grafiği](media/functions-monitoring/requests-chart.png)

Bu günlüklerin en yazılır `Information` düzey, bu nedenle, filtre `Warning` veya üstü, bu verileri hiçbirini göremezsiniz.

### <a name="category-hostaggregator"></a>Kategori Host.Aggregator

Bu günlükler üzerinden sayıları ve ortalamalar, işlev çağrılarını sağlayan bir [yapılandırılabilir](#configure-the-aggregator) zaman dönem. Varsayılan süre 30 saniye veya 1.000 sonuçları, hangisinin önce geldiğine bağlı. 

Günlükleri kullanılabilir **customMetrics** Application ınsights'ta tablo. Örnekler sayı çalıştırır, başarı oranı ve süre.

![customMetrics sorgu](media/functions-monitoring/custom-metrics-query.png)

Bu günlüklerin en yazılır `Information` düzey, bu nedenle, filtre `Warning` veya üstü, bu verileri hiçbirini göremezsiniz.

### <a name="other-categories"></a>Diğer kategorileri

Kategoriler zaten olanlar dışındaki tüm günlükler listelenen kullanılabilir **izlemeleri** Application ınsights'ta tablo.

![izlemeleri sorgu](media/functions-monitoring/analytics-traces.png)

"Ana" ile başlayan kategorileri tüm günlüklerin işlevler çalışma zamanı tarafından yazılır. "İşlev çalışmaya" ve "İşlev tamamlandı" günlükleri "Host.Executor" kategorilerini içerir. Başarılı çalıştırmalar için bu günlüklerin `Information` düzeyi; özel durumlar, günlüğe kaydedilen `Error` düzeyi. Ayrıca çalışma zamanı oluşturur `Warning` düzey günlükleri, örneğin: zehirli kuyruğa gönderilen iletilerin kuyruğa alın.

Günlükleri, işlev kodunuzun tarafından yazılan, "İşlev" kategorisi olan ve herhangi bir günlük düzeyi olabilir.

## <a name="configure-the-aggregator"></a>Toplayıcı yapılandırma

Önceki bölümde belirtildiği gibi çalışma zamanı bir süre içinde işlev yürütmelerini hakkındaki verileri toplar. 1.000 çalışmadan, hangisi önce geliyorsa veya varsayılan süre 30 saniyedir. Bu ayarı yapılandırabilirsiniz *host.json* dosya.  Bir örneği aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırma

Application Insights'ı olan bir [örnekleme](../application-insights/app-insights-sampling.md) zamanlarda, yoğun yük çok fazla telemetri verileri üreten gelen Koruyabileceğiniz özelliği. Gelen telemetri oranı belirtilen eşiği aşarsa, rastgele gelen öğelerden bazıları yok saymak Application ınsights'ı başlatır. 5 saniye başına öğe sayısı için varsayılan ayardır. Örnekleme yapılandırabileceğiniz *host.json*.  Bir örneği aşağıda verilmiştir:

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

## <a name="write-logs-in-c-functions"></a>C# işlevleri günlüklerini yazma

Günlükleri olarak Application Insights izlemelerinde görünen işlev kodunuzu yazabilirsiniz.

### <a name="ilogger"></a>ILogger

Kullanım bir [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) işlevlerinizi yerine parametresinde bir `TraceWriter` parametresi. Günlükleri kullanarak oluşturduğunuz `TraceWriter` Application Insights'a Git ancak `ILogger` yapmanıza olanak sağlayan [günlük yapılandırılmış](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

İle bir `ILogger` çağırmanızı nesne `Log<level>` [ILogger genişletme yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) günlükleri oluşturmak için. Örneğin, aşağıdaki yazma kod `Information` "İşlev" kategorili günlükleri.

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Yapılandırılmış günlük kaydı

Hangi parametreler bir günlük iletisinde kullanılır yer tutucuları, bunların adları sırasını belirler. Örneğin, aşağıdaki kod olduğunu varsayalım:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Aynı ileti dizesi tutun ve parametre sırasını tersine, elde edilen ileti metni yanlış yerlerde değerleri gerekir.

Yer tutucuları, böylece yapılandırılmış günlük kaydı yapmak için bu şekilde ele alınır. Application Insights ek ileti dizesi parametresi ad-değer çiftleri depolar. Sonuç iletisi bağımsız değişkenler üzerinde sorgu alanları olmasıdır.

Örneğin, alan sorgu Günlükçü yöntem çağrınız önceki örnekteki gibi görünüyorsa, `customDimensions.prop__rowKey`. `prop__` Önek, çalışma zamanı ekler ve işlev kodunuzu alanları alanlar arasında hiçbir çakışma olmadığından emin olmak için ekler eklenir.

Ayrıca, alanın başvurarak özgün ileti dizesi sorgulayabilirsiniz `customDimensions.prop__{OriginalFormat}`.  

İşte bir örnek JSON temsili `customDimensions` veri:

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

### <a name="logging-custom-metrics"></a>Özel ölçümler günlüğe kaydetme  

C# betik işlevleri'nde kullanabilirsiniz `LogMetric` genişletme yöntemini `ILogger` özel ölçümler Application Insights'da oluşturmak için. Bir örnek yöntem çağrısı şu şekildedir:

```csharp
logger.LogMetric("TestMetric", 1234); 
```

Bu kodu çağırmak için bir alternatifidir `TrackMetric` kullanarak [.NET için Application Insights API](#custom-telemetry-in-c-functions).

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevleri günlüklerini yazma

Node.js işlevleri'nde kullanmak `context.log` günlüklerini yazma izni. Yapılandırılmış günlük kaydı etkinleştirilmedi.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="logging-custom-metrics"></a>Özel ölçümler günlüğe kaydetme  

Node.js işlevleri'nde kullanabilirsiniz `context.log.metric` özel ölçümler Application Insights'da oluşturmak için yöntemi. Bir örnek yöntem çağrısı şu şekildedir:

```javascript
context.log.metric("TestMetric", 1234); 
```

Bu kodu çağırmak için bir alternatifidir `trackMetric` kullanarak [Application ınsights Node.js SDK'sı](#custom-telemetry-in-javascript-functions).

## <a name="custom-telemetry-in-c-functions"></a>C# işlevlerdeki özel telemetri

Kullanabileceğiniz [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) Application Insights özel telemetri verileri göndermek için NuGet paketi.

İşte bir örnek kullanan C# kod [API özel telemetri](../application-insights/app-insights-api-custom-events-metrics.md). Örnek bir .NET sınıf kitaplığı için yazılmıştır, ancak Application Insights kod C# betiği için aynıdır.

```cs
using System;
using System.Net;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
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

        private static TelemetryClient telemetryClient = 
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
         
            // Track an Event
            var evt = new EventTelemetry("Function called");
            UpdateTelemetryContext(evt.Context, context, name);
            telemetryClient.TrackEvent(evt);
            
            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            UpdateTelemetryContext(metric.Context, context, name);
            telemetryClient.TrackMetric(metric);
            
            // Track a Dependency
            var dependency = new DependencyTelemetry
                {
                    Name = "GET api/planets/1/",
                    Target = "swapi.co",
                    Data = "https://swapi.co/api/planets/1/",
                    Timestamp = start,
                    Duration = DateTime.UtcNow - start,
                    Success = true
                };
            UpdateTelemetryContext(dependency.Context, context, name);
            telemetryClient.TrackDependency(dependency);
        }
        
        // This correllates all telemetry with the current Function invocation
        private static void UpdateTelemetryContext(TelemetryContext context, ExecutionContext functionContext, string userName)
        {
            context.Operation.Id = functionContext.InvocationId.ToString();
            context.Operation.ParentId = functionContext.InvocationId.ToString();
            context.Operation.Name = functionContext.FunctionName;
            context.User.Id = userName;
        }
    }    
}
```

Remove() çağırmayın `TrackRequest` veya `StartOperation<RequestTelemetry>`, yinelenen bir işlev çağrısını isteklerinde görürsünüz.  İşlevler çalışma zamanı, istekleri otomatik olarak izler.

Ayarlamamanız `telemetryClient.Context.Operation.Id`. Bu genel bir ayardır ve aynı anda birçok işlev çalışırken yanlış correllation neden olur. Bunun yerine, yeni bir telemetri örneği oluşturma (`DependencyTelemetry`, `EventTelemetry`) ve kendi `Context` özelliği. Ardından ilgili telemetri örneğinde geçirin `Track` metodunda `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Bu, telemetri doğru correllation ayrıntıları geçerli işlev çağrısına sahip olmasını sağlar.

## <a name="custom-telemetry-in-javascript-functions"></a>JavaScript işlevleri'nde özel telemetri

[Application Insights Node.js SDK'sı](https://www.npmjs.com/package/applicationinsights) şu anda beta sürümünde olan. Application Insights özel telemetri gönderen bazı örnek kodlar aşağıda verilmiştir:

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

    context.done();
};
```

`tagOverrides` Parametre kümeleri `operation_Id` işlev çağırma kimliği Bu ayar tüm verilen işlevi çağrısı için otomatik olarak oluşturulan ve özel telemetrinin bağıntısını kurmanızı sağlar.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="dependencies"></a>Bağımlılıklar

İşlev diğer hizmetlere olan bağımlılıkları otomatik olarak gösterme, ancak bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnek kodda [C# özel telemetri bölümü](#custom-telemetry-in-c-functions) gösterir nasıl. Örnek kod sonuçlanıyor bir *Uygulama Haritası* Application ınsights'ta aşağıdakine benzer:

![Uygulama eşlemesi](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Rapor sorunları

İşlevlerde Application Insights Tümleştirmesi ile ilgili bir sorun bildirin veya öneri ya da isteği yapmak için [Github'da bir sorun oluşturun](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitoring-without-application-insights"></a>Application Insights izleme

Daha fazla veri ve verileri çözümlemek için daha iyi bir yolu sağladığından işlevleri izleme için Application ınsights'ı öneririz. Ancak, Azure depolama kullanan yerleşik günlük kaydetme sistemi tercih ederseniz, kullanmaya devam edebilirsiniz.

### <a name="logging-to-storage"></a>Depolama için günlükleri

Yerleşik günlük kullandığı bağlantı dizesinde belirtilen depolama hesabı `AzureWebJobsDashboard` uygulama ayarı. Bir işlev uygulaması sayfasını, bir işlev seçin ve ardından **İzleyici** sekmesini tıklatın ve Klasik görünümde tutmak için seçin.

![Klasik görünümüne geç](media/functions-monitoring/switch-to-classic-view.png)

 İşlev yürütmelerini listesini al Bir işlev yürütme süresi, girdi verilerini, hataları ve ilişkili günlük dosyalarını gözden geçirmek için seçin.

Application Insights'ı daha önce etkinleştirilmiş, ancak artık geri yerleşik günlük için Application ınsights'ı el ile devre dışı bırakın ve ardından gitmek istiyorsanız **İzleyici** sekmesi. Application Insights tümleştirmesi devre dışı bırakmak için appınsıghts_ınstrumentatıonkey uygulama ayarı silin.

Bile **İzleyici** sekmesi, Application Insights verilerini gösterir, aksi takdirde dosya sistemindeki günlük verileri görebiliyor [yerleşik günlüğe yazma devre dışı](#disable-built-in-logging). Depolama kaynak dosyaları için işlevi için dosya hizmeti seçin ve ardından Git Git `LogFiles > Application > Functions > Function > your_function` için günlük dosyasına bakın.

### <a name="real-time-monitoring"></a>Gerçek zamanlı izleme

Bir komut satırı oturumu için günlük dosyalarını kullanarak bir yerel iş istasyonu üzerinde akışını [Azure komut satırı arabirimi (CLI)](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).  

Azure CLI için aşağıdaki komutları kullanın oturum açmak, aboneliği ve günlük dosyalarını akışla aktarma'yı seçin:

```
az login
az account list
az account set <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Azure PowerShell için aşağıdaki komutları kullanın Azure hesabınızı eklemek için abonelik ve akışı günlük dosyalarını seçin:

```
PS C:\> Add-AzureAccount
PS C:\> Get-AzureSubscription
PS C:\> Get-AzureSubscription -SubscriptionName "<subscription name>" | Select-AzureSubscription
PS C:\> Get-AzureWebSiteLog -Name <function app name> -Tail
```

Daha fazla bilgi için [günlüklerin akışını nasıl](../app-service/web-sites-enable-diagnostic-log.md#streamlogs).

### <a name="viewing-log-files-locally"></a>Günlük dosyaları yerel olarak görüntüleme

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core günlüğe kaydetme](/aspnet/core/fundamentals/logging/)
