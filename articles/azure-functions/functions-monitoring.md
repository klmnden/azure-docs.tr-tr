---
title: Azure İşlevlerini İzleme
description: Azure Application Insights, işlevi yürütme izlemek için Azure işlevleri ile kullanmayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 501722c3-f2f7-4224-a220-6d59da08a320
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/15/2018
ms.author: glenga
ms.openlocfilehash: 0224d9ba5a430635e4675c2fb2bf354e7c975f31
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58518739"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

[Azure işlevleri](functions-overview.md) ile yerleşik tümleştirme sunan [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) işlevleri izlemek için. Bu makalede, Azure işlevleri'nin sistem tarafından oluşturulan günlük dosyalarını Application Insights'a gönderme yapılandırma gösterilmektedir.

![Application Insights ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

Azure işlevleri de olan Application Insights kullanmaz yerleşik izleme. Daha fazla veri ve verileri çözümlemek için daha iyi bir yolu sağladığından Application ınsights'ı öneririz.

## <a name="application-insights-pricing-and-limits"></a>Application Insights fiyatlandırma ve limitler

Application Insights Tümleştirmesi ile işlev uygulamaları ücretsiz deneyebilirsiniz. Ne kadar veri ücretsiz işlenmesi için günlük bir sınır yoktur. Test sırasında bu sınıra ulaşılmasına neden. Günlük limitinize yaklaştığınıza olduğunda azure portalında ve e-posta bildirimleri sağlar. Bu uyarıları kaçırmayın ve sınırına, yeni günlükler Application Insights sorgularda görünmez. Gereksiz sorun giderme süresini önlemek için sınırını dikkat edin. Daha fazla bilgi için [Application ınsights fiyatlandırma ve veri hacmini yönetme](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmesini etkinleştirme

Bir işlev uygulaması için Application Insights veri göndermek, bir Application Insights kaynağına ait izleme anahtarını bilmesi gerekir. Anahtar adlı ayar bir uygulamada olmalıdır **appınsıghts_ınstrumentatıonkey**.

Bu bağlantıda ayarlayabilirsiniz [Azure portalında](https://portal.azure.com):

* [Yeni bir işlev uygulaması otomatik olarak bağlan](#new-function-app)
* [Application Insights kaynağını el ile bağlanma](#manually-connect-an-app-insights-resource)

### <a name="new-function-app"></a>Yeni işlev uygulaması
<!-- Add a transitional sentence to introduce the procedure. -->

1. İşlev uygulamasını Git **Oluştur** sayfası.

1. Ayarlama **Application Insights** geçiş **üzerinde**.

1. Seçin bir **Application Insights konumu**. İşlev uygulamanızın bölgesine en yakın olan hem de bir bölge seçin bir [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) istediğiniz verileri depolamak.

   ![Bir işlev uygulaması oluştururken Application Insights'ı etkinleştir](media/functions-monitoring/enable-ai-new-function-app.png)

1. Diğer gerekli bilgileri girin ve seçin **Oluştur**.

Sonraki adım [yerleşik günlüğünü devre dışı](#disable-built-in-logging).


<a id="manually-connect-an-app-insights-resource"></a>
### <a name="application-insights-resource"></a>Application Insights kaynağı 
<!-- Add a transitional sentence to introduce the procedure. -->

1. Application Insights kaynağı oluşturun. Uygulama türü ayarlayın **genel**.

   ![Genel türünde bir Application Insights kaynağı oluşturun](media/functions-monitoring/ai-general.png)

1. İzleme anahtarını kopyalama **Essentials** Application Insights kaynağına ait sayfa. Görüntülenen anahtar değerini almak için sonuna işaret edecek bir **kopyalamak için tıklayın** düğmesi.

   ![Application Insights izleme anahtarını kopyalama](media/functions-monitoring/copy-ai-key.png)

1. İşlev uygulamasının **uygulama ayarları** sayfasında [bir uygulama ayarı ekleme](functions-how-to-use-azure-function-app-settings.md#settings) seçerek **yeni ayar Ekle**. Yeni ayar adı **appınsıghts_ınstrumentatıonkey** kopyalanan izleme anahtarını yapıştırın.

   ![İzleme anahtarını uygulama ayarları Ekle](media/functions-monitoring/add-ai-key.png)

1. **Kaydet**’i seçin.

<!-- Before the next H2 heading, add transitional sentences to summarize why the procedures were necessary. -->

## <a name="disable-built-in-logging"></a>Yerleşik günlük devre dışı bırakma

Application Insights'ı etkinleştirdiğinizde, Azure depolama kullanan yerleşik günlük kaydını devre dışı bırakın. Yerleşik günlük hafif iş yükleriyle test edilmesi için yararlıdır, ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izlemek için Application ınsights'ı öneririz. Üretimde yerleşik günlük kaydı kullandıysanız, günlük kaydı eksik Azure depolama alanında azaltma nedeniyle olabilir.

Yerleşik günlük kaydetme devre dışı bırakmak için silme `AzureWebJobsDashboard` uygulama ayarı. Uygulama ayarları Azure portalında silme hakkında daha fazla bilgi için bkz: **uygulama ayarları** bölümünü [bir işlev uygulaması yönetme](functions-how-to-use-azure-function-app-settings.md#settings). Uygulama ayarı silmeden önce Azure depolama Tetikleyicileri veya bağlamaları için aynı işlev uygulamasında var olan bir işlev yok ayarını kullandığınızdan emin olun.

## <a name="view-telemetry-in-monitor-tab"></a>İzleyici sekmesi içinde telemetri görüntüleme

Application Insights tümleştirmesini ayarlama önceki bölümlerde gösterildiği gibi ayarladıktan sonra telemetri verileri görüntüleyebilirsiniz **İzleyici** sekmesi.

1. Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev ait işlev uygulaması sayfasını seçin. Ardından **İzleyici** sekmesi.

   ![İzleyici sekmesi seçin](media/functions-monitoring/monitor-tab.png)

1. Seçin **Yenile** düzenli aralıklarla, işlev çağrılarını listesinde görünene kadar.

   Bu listenin telemetri istemcisi sunucuya aktarım için verileri toplu işlemleri sırasında görünmesini beş dakikaya kadar sürebilir. (Gecikme uygulanmaz [Canlı ölçümleri Stream](../azure-monitor/app/live-stream.md). Günlükleri doğrudan sayfaya aktarılır, böylece sayfa yüklediğinizde bu hizmeti işlevleri ana bilgisayara bağlanır.)

   ![Çağrılarını listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. Belirli işlev çağrısı için günlükleri görmek için seçin **tarih** bu çağrı için sütun bağlantı.

   ![Çağrı Ayrıntıları bağlantı](media/functions-monitoring/invocation-details-link-ai.png)

   Bu çağrı için günlük çıktısı, yeni bir sayfa görünür.

   ![Çağrı Ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

Her iki sayfa (çağırma listesi ve çağrı bilgileri), verileri alır. Application Insights Analytics sorgunuzun bağlantı:

![Application Insights'ta çalıştır](media/functions-monitoring/run-in-ai.png)

![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Bu sorguları, çağırma listesi son 30 gün içinde sınırlı olduğunu görebilirsiniz. Liste en fazla 20 satırları gösterir (`where timestamp > ago(30d) | take 20`). Son 30 gün boyunca hiçbir sınır çağırma ayrıntıları listesidir.

Daha fazla bilgi için [sorgu telemetri verilerini](#query-telemetry-data) bu makalenin ilerleyen bölümlerinde.

## <a name="view-telemetry-in-application-insights"></a>Application ınsights telemetrisini görüntüleme

Azure portalında bir işlev uygulamasından Application Insights'ı açmak için işlev uygulamasına ait Git **genel bakış** sayfası. Altında **özellikler yapılandırıldı**seçin **Application Insights**.

![İşlev uygulaması genel bakış sayfasından Application Insights'ı açın](media/functions-monitoring/ai-link.png)

Application Insights'ı kullanma hakkında daha fazla bilgi için bkz: [Application Insights belgeleri](https://docs.microsoft.com/azure/application-insights/). Bu bölümde, Application Insights verilerini görüntüleme ile ilgili bazı örnekler gösterilmektedir. Zaten Application Insights ile biliyorsanız, doğrudan gidebilirsiniz siz [nasıl yapılandırılacağı ve telemetri verilerini özelleştirme hakkında bölümlere](#configure-categories-and-log-levels).

İçinde [ölçüm Gezgini](../azure-monitor/app/metrics-explorer.md), grafikleri ve ölçümlerine bağlı uyarılar oluşturabilirsiniz. Ölçümler, işlev çağrıları, yürütme süresi ve başarı oranları sayısını içerir.

![Ölçüm Gezgini](media/functions-monitoring/metrics-explorer.png)

Üzerinde [hataları](../azure-monitor/app/asp-net-exceptions.md) sekmesinde grafikler oluşturabilir ve uyarılar, özel durumlar işlevi hataları ve sunucuda temel. **İşlem adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamak sürece hataları bağımlılıkları gösterilmez.

![Hatalar](media/functions-monitoring/failures.png)

Üzerinde [performans](../azure-monitor/app/performance-counters.md) sekmesinde, performans sorunlarını çözümleyebilirsiniz.

![Performans](media/functions-monitoring/performance.png)

**Sunucuları** sekmesi kaynak kullanımı ve sunucu başına aktarım hızını gösterir. Bu veri, İşlevler, temel alınan kaynakları burada bogging senaryoları hata ayıklama için yararlı olabilir. Sunucuları denir **bulut rolü örnekleri**.

![Sunucular](media/functions-monitoring/servers.png)

[Canlı ölçümleri Stream](../azure-monitor/app/live-stream.md) sekmesi, gerçek zamanlı olarak oluşturulduğundan ölçüm verilerini gösterir.

![Canlı akış](media/functions-monitoring/live-stream.png)

## <a name="query-telemetry-data"></a>Telemetri verileri Sorgulama

[Application Insights Analytics](../azure-monitor/app/analytics.md) formunda bir veritabanındaki tabloların tüm telemetri verilerine erişmenizi. Analytics ayıklanması, düzenleme ve veri görselleştirme için bir sorgu dili sağlar.

![Analytics seçin](media/functions-monitoring/select-analytics.png)

![Analiz örneği](media/functions-monitoring/analytics-traces.png)

Son 30 dakika içindeki alt başına istek dağılımını gösteren bir sorgu örneği aşağıda verilmiştir.

```
requests
| where timestamp > ago(30m) 
| summarize count() by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```

Kullanılabilir tablolar gösterilen **şema** soldaki sekmesi. Aşağıdaki tablolarda işlev çağrılarını tarafından oluşturulan verileri bulabilirsiniz:

* **izlemeleri**: İşlev kodunu ve çalışma zamanı tarafından oluşturulan günlükleri.
* **İstekleri**: Her işlev çağrısı için bir istek.
* **Özel durumlar**: Çalışma zamanı tarafından oluşturulan özel durumlar.
* **customMetrics**: Başarılı ve başarısız olan çağrılar, başarı oranı ve süre sayısı.
* **customEvents**: Örneğin çalışma zamanı tarafından izlenen olayları: HTTP isteklerinin bir işlev tetikler.
* **performanceCounters**: İşlevleriniz gerektirdikçe sunucularının performansı hakkında bilgi.

Kullanılabilirlik testleri ve istemci ve tarayıcı telemetrisi diğer tablolara içindir. Veri eklemek üzere özel telemetri uygulayabilirsiniz.

Bazı işlevler özgü verileri her tabloda olduğu bir `customDimensions` alan.  Örneğin, aşağıdaki sorgu günlüğü düzeyine sahip tüm izlemeleri alır `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı sağlar `customDimensions.LogLevel` ve `customDimensions.Category` alanları. İşlev kodunuzu yazdığınız günlüklerine ek alanlar sağlayabilir. Bkz: [günlük yapılandırılmış](#structured-logging) bu makalenin ilerleyen bölümlerinde.

## <a name="configure-categories-and-log-levels"></a>Kategorileri yapılandırmadan ve oturum düzeyleri

Application Insights herhangi özel bir yapılandırma kullanabilirsiniz. Varsayılan yapılandırma, yüksek hacimli verileri neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri sınırınıza neden olabilir. Bu makalenin sonraki bölümlerinde yapılandırma ve özelleştirme işlevlerinizi Application Insights'a gönderme veri öğrenin.

### <a name="categories"></a>Kategoriler

Azure işlevleri Günlükçü içeren bir *kategori* her günlük için. Kategori günlük çalışma zamanı kodu veya işlev kodunuzu hangi kısmını yazdı gösterir. 

İşlevler çalışma zamanı günlüklerini "Ana" ile başlayan bir kategori oluşturur "İşlevi çalışmaya," "yürütülme işlevi" ve "işlev tamamlandı" günlükleri kategorisi "Host.Executor." 

İşlev kodunuzda günlükler yazar, kategorilerine "İşlev" olur.

### <a name="log-levels"></a>Günlük düzeyleri

Azure işlevleri Günlükçü de içeren bir *günlük düzeyi* her günlük. [LogLevel](/dotnet/api/microsoft.extensions.logging.loglevel) bir sabit listesidir ve tamsayı kodu göreli önemi gösterir:

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

### <a name="log-configuration-in-hostjson"></a>Host.json günlüğünü yapılandırma

[Host.json](functions-host-json.md) bir işlev uygulaması, Application Insights'a gönderir ne kadar günlük dosyası oluşturur. Her kategori için göndermek için en düşük günlük düzeyi belirtin. İki örnekleri vardır: İlk örnek hedefleri [işlevleri sürüm 2.x çalışma zamanı](functions-versions.md#version-2x) (.NET Core) ve sürüm 1.x çalışma zamanı için ikinci örnektir.

### <a name="version-2x"></a>Sürüm 2.x

Çalışma zamanı'nın v2.x [.NET Core günlük filtresine ilişkin hiyerarşi](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.1#log-filtering). 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host.Results": "Error",
      "Function": "Error",
      "Host.Aggregator": "Trace"
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1.x

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

* Günlükleri kategorili `Host.Results` veya `Function`, yalnızca gönderme `Error` düzeyi ve yukarıdaki Application ınsights. Günlüklerinde `Warning` düzeyi ve aşağıda göz ardı edilir.
* Günlükleri kategorili `Host.Aggregator`, Application Insights için tüm günlükleri gönderin. `Trace` Günlük düzeyi ne bazı günlükçüleri çağrısı ile aynı olup `Verbose`, ancak `Trace` içinde [host.json](functions-host-json.md) dosya.
* Diğer tüm günlükler için yalnızca gönderme `Information` düzeyi ve üzeri için Application Insights.

Kategori değeri [host.json](functions-host-json.md) günlük kaydı için aynı değeri ile başlayan tüm kategorileri denetler. `Host` içinde [host.json](functions-host-json.md) denetimleri için günlüğü `Host.General`, `Host.Executor`, `Host.Results`ve benzeri.

Varsa [host.json](functions-host-json.md) aynı dize ile başlayan birden çok kategori içeren uzun olanlarla ilk eşleştirilir. Çalışma zamanı dışında her şeyi istediğinizi düşünelim `Host.Aggregator` günlüğe kaydetmek için `Error` düzeyi, ancak isterseniz `Host.Aggregator` günlüğe kaydetmek için `Information` düzeyi:

### <a name="version-2x"></a>Sürüm 2.x 

```json
{
  "logging": {
    "fileLoggingMode": "always",
    "logLevel": {
      "default": "Information",
      "Host": "Error",
      "Function": "Error",
      "Host.Aggregator": "Information"
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1.x 

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

Bu günlüklerin en yazılır `Information` düzeyi. Adresindeki filtrelerseniz `Warning` veya üstü, bu verileri hiçbirini göremezsiniz.

### <a name="category-hostaggregator"></a>Kategori Host.Aggregator

Bu günlükler üzerinden sayıları ve ortalamalar, işlev çağrılarını sağlayan bir [yapılandırılabilir](#configure-the-aggregator) zaman dönem. Varsayılan süre 30 saniye veya 1.000 sonuçları, hangisinin önce geldiğine bağlı. 

Günlükleri kullanılabilir **customMetrics** Application ınsights'ta tablo. Çalıştırma, başarı oranı ve süre sayısı verilebilir.

![customMetrics sorgu](media/functions-monitoring/custom-metrics-query.png)

Bu günlüklerin en yazılır `Information` düzeyi. Adresindeki filtrelerseniz `Warning` veya üstü, bu verileri hiçbirini göremezsiniz.

### <a name="other-categories"></a>Diğer kategorileri

Kategoriler zaten olanlar dışındaki tüm günlükler listelenen kullanılabilir **izlemeleri** Application ınsights'ta tablo.

![izlemeleri sorgu](media/functions-monitoring/analytics-traces.png)

Kategoriler ile başlayan tüm günlüklerin `Host` işlevler çalışma zamanı tarafından yazılır. "İşlev çalışmaya" ve "işlev tamamlandı" günlükleri kategorisi olan `Host.Executor`. Başarılı çalıştırmalar için bu günlüklerin `Information` düzeyi. Özel durumlar günlüğe `Error` düzeyi. Ayrıca çalışma zamanı oluşturur `Warning` düzey günlükleri, örneğin: zehirli kuyruğa gönderilen iletilerin kuyruğa alın.

İşlev kodunuzu tarafından yazılan kategori gösterilmiş `Function` ve herhangi bir günlük düzeyi olabilir.

## <a name="configure-the-aggregator"></a>Toplayıcı yapılandırma

Önceki bölümde belirtildiği gibi çalışma zamanı bir süre içinde işlev yürütmelerini hakkındaki verileri toplar. 1.000 çalışmadan, hangisi önce geliyorsa veya varsayılan süre 30 saniyedir. Bu ayarı yapılandırabilirsiniz [host.json](functions-host-json.md) dosya.  Bir örneği aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırma

Application Insights'ı olan bir [örnekleme](../azure-monitor/app/sampling.md) zamanlarda, yoğun yük çok fazla telemetri verileri üreten gelen Koruyabileceğiniz özelliği. Gelen telemetri oranı belirtilen eşiği aşarsa, rastgele gelen öğelerden bazıları yok saymak Application ınsights'ı başlatır. En fazla saniye başına öğe sayısı için varsayılan ayar beştir. Örnekleme yapılandırabileceğiniz [host.json](functions-host-json.md).  Bir örneği aşağıda verilmiştir:

### <a name="version-2x"></a>Sürüm 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 5
      }
    }
  }
}
```

### <a name="version-1x"></a>Sürüm 1.x 

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

> [!NOTE]
> [Örnekleme](../azure-monitor/app/sampling.md) varsayılan olarak etkindir. Veriler eksik görünüyorsa, belirli izleme senaryonuza uygun örnekleme ayarlarınızı gerekebilir.

## <a name="write-logs-in-c-functions"></a>C# işlevleri günlüklerini yazma

Günlükleri olarak Application Insights izlemelerinde görünen işlev kodunuzu yazabilirsiniz.

### <a name="ilogger"></a>ILogger

Kullanım bir [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger) işlevlerinizi yerine parametresinde bir `TraceWriter` parametresi. Kullanılarak oluşturulan günlükleri `TraceWriter` Application Insights, Git ancak `ILogger` yapmanıza olanak sağlayan [günlük yapılandırılmış](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).

İle bir `ILogger` nesne çağırmanızı `Log<level>` [ILogger genişletme yöntemi](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.loggerextensions#methods) günlükleri oluşturmak için. Aşağıdaki kod yazma `Information` "İşlev" kategorili günlükleri

```cs
public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger logger)
{
    logger.LogInformation("Request for item with key={itemKey}.", id);
```

### <a name="structured-logging"></a>Yapılandırılmış günlük kaydı

Hangi parametreler bir günlük iletisinde kullanılır yer tutucuları, bunların adları sırasını belirler. Aşağıdaki kod olduğunu varsayalım:

```csharp
string partitionKey = "partitionKey";
string rowKey = "rowKey";
logger.LogInformation("partitionKey={partitionKey}, rowKey={rowKey}", partitionKey, rowKey);
```

Aynı ileti dizesi tutun ve parametre sırasını tersine, elde edilen ileti metni yanlış yerlerde değerleri gerekir.

Yer tutucuları, böylece yapılandırılmış günlük kaydı yapmak için bu şekilde ele alınır. Application Insights, parametre ad-değer çiftleri ve ileti dizesi depolar. Sonuç iletisi bağımsız değişkenler üzerinde sorgu alanları olmasıdır.

Günlükçü yöntem çağrınız önceki örnekteki gibi görünüyorsa, alanın sorgulayabilirsiniz `customDimensions.prop__rowKey`. `prop__` Önek, çalışma zamanı ekler ve işlev kodunuzu alanları alanlar arasında hiçbir çakışma olmadığından emin olmak için ekler eklenir.

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

### <a name="custom-metrics-logging"></a>Özel ölçümler günlüğe kaydetme

C# betik işlevleri'nde kullanabilirsiniz `LogMetric` genişletme yöntemini `ILogger` özel ölçümler Application Insights'da oluşturmak için. Bir örnek yöntem çağrısı şu şekildedir:

```csharp
logger.LogMetric("TestMetric", 1234);
```

Bu kodu çağırmak için bir alternatifidir `TrackMetric` .NET için Application Insights API kullanarak.

## <a name="write-logs-in-javascript-functions"></a>JavaScript işlevleri günlüklerini yazma

Node.js işlevleri'nde kullanmak `context.log` günlüklerini yazma izni. Yapılandırılmış günlük kaydı etkin değil.

```
context.log('JavaScript HTTP trigger function processed a request.' + context.invocationId);
```

### <a name="custom-metrics-logging"></a>Özel ölçümler günlüğe kaydetme

Çalıştırdığınız zaman [sürüm 1.x](functions-versions.md#creating-1x-apps) işlevler çalışma zamanı olan Node.js işlevleri kullanabilirsiniz `context.log.metric` özel ölçümler Application Insights'da oluşturmak için yöntemi. Bu yöntem, şu anda sürümünde desteklenmemektedir 2.x. Bir örnek yöntem çağrısı şu şekildedir:

```javascript
context.log.metric("TestMetric", 1234);
```

Bu kodu çağırmak için bir alternatifidir `trackMetric` için Application Insights Node.js SDK'sını kullanarak.

## <a name="log-custom-telemetry-in-c-functions"></a>Özel telemetri oturum C# işlevleri

Kullanabileceğiniz [Microsoft.applicationınsights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) Application Insights özel telemetri verileri göndermek için NuGet paketi. Aşağıdaki C# örnekte [API özel telemetri](../azure-monitor/app/api-custom-events-metrics.md). Örnek bir .NET sınıf kitaplığı için yazılmıştır, ancak Application Insights kod C# betiği için aynıdır.

### <a name="version-2x"></a>Sürüm 2.x

Sürüm 2.x çalışma zamanı otomatik olarak geçerli işleme telemetrinin bağıntısını kurmanızı Application Insights'ta yeni özellikler kullanıyor. El ile ayarlama işlemi gerek `Id`, `ParentId`, veya `Name` alanları.

```cs
using System;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace functionapp0915
{
    public static class HttpTrigger2
    {
        // In Functions v2, TelemetryConfiguration.Active is initialized with the InstrumentationKey
        // from APPINSIGHTS_INSTRUMENTATIONKEY. Creating a default TelemetryClient like this will 
        // automatically use that key for all telemetry. It will also enable telemetry correlation
        // with the current operation.
        // If you require a custom TelemetryConfiguration, create it initially with
        // TelemetryConfiguration.CreateDefault() to include this automatic correlation.
        private static TelemetryClient telemetryClient = new TelemetryClient();

        [FunctionName("HttpTrigger2")]
        public static Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", Route = null)]
            HttpRequest req, ExecutionContext context, ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            DateTime start = DateTime.UtcNow;

            // Parse query parameter
            string name = req.Query
                .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
                .Value;

            // Track an Event
            var evt = new EventTelemetry("Function called");
            evt.Context.User.Id = name;
            telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
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
            dependency.Context.User.Id = name;
            telemetryClient.TrackDependency(dependency);

            return Task.FromResult<IActionResult>(new OkResult());
        }
    }
}
```

### <a name="version-1x"></a>Sürüm 1.x

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

            // Parse query parameter
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
        
        // Correlate all telemetry with the current Function invocation
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

Remove() çağırmayın `TrackRequest` veya `StartOperation<RequestTelemetry>` çünkü yinelenen istekleri için bir işlev çağrısını görürsünüz.  İşlevler çalışma zamanı, istekleri otomatik olarak izler.

Ayarlamamanız `telemetryClient.Context.Operation.Id`. Aynı anda birçok işlev çalışırken bu genel ayarı yanlış bağıntı neden olur. Bunun yerine, yeni bir telemetri örneği oluşturma (`DependencyTelemetry`, `EventTelemetry`) ve kendi `Context` özelliği. Ardından ilgili telemetri örneğinde geçirin `Track` metodunda `TelemetryClient` (`TrackDependency()`, `TrackEvent()`). Bu yöntem, telemetri doğru bağıntı ayrıntıları geçerli işlev çağrısına sahip olmasını sağlar.

## <a name="log-custom-telemetry-in-javascript-functions"></a>JavaScript işlevleri de günlük özel telemetri

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
<!-- Add a transitional sentence to introduce the section. -->

### <a name="dependencies"></a>Bağımlılıklar

İşlev diğer hizmetlere olan bağımlılıkları otomatik olarak görünmüyor. Bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnek kodda örnekler için bkz [ C# özel telemetri bölümü](#log-custom-telemetry-in-c-functions). Örnek kod sonuçlanıyor bir *Uygulama Haritası* Application ınsights'ın aşağıdaki görüntü gibi görünür:

![Uygulama eşlemesi](media/functions-monitoring/app-map.png)

### <a name="report-issues"></a>Rapor sorunları

İşlevlerde Application Insights Tümleştirmesi ile ilgili bir sorun bildirin veya öneri ya da isteği yapmak için [Github'da bir sorun oluşturun](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="monitor-without-application-insights"></a>Application Insights olmadan İzleyicisi

İzleme işlevleri için Application ınsights'ı öneririz. Daha fazla veri ve verileri çözümlemek için daha iyi yollarını sunar. Ancak, Azure depolama kullanan yerleşik günlük kaydetme sistemi tercih ederseniz, bu yöntemi kullanmak üzere devam edebilirsiniz.

### <a name="azure-storage-account-for-logging"></a>Günlük kaydı için Azure depolama hesabı

Yerleşik günlük kullandığı bağlantı dizesinde belirtilen depolama hesabı `AzureWebJobsDashboard` uygulama ayarı. Bir işlev uygulaması sayfasını, bir işlev seçin ve ardından **İzleyici** sekmesini ve onu tutmak seçin **Klasik Görünüm**.

![Klasik görünümüne geç](media/functions-monitoring/switch-to-classic-view.png)

İşlev yürütmelerini listesini al Bir işlev yürütme süresi, girdi verilerini, hataları ve ilişkili günlük dosyalarını gözden geçirmek için seçin.

Application Insights etkinleştirilirse, yerleşik günlük kullanmaya geri dönebilirsiniz. Application ınsights'ı el ile devre dışı bırakın ve ardından **İzleyici** sekmesi. Application Insights tümleştirmesi devre dışı bırakmak için silme `APPINSIGHTS_INSTRUMENTATIONKEY` uygulama ayarı.

Bile **İzleyici** sekmesi, Application Insights verilerini gösterir, aksi takdirde dosya sistemindeki günlük verileri görebiliyor [yerleşik günlüğe yazma devre dışı](#disable-built-in-logging). Depolama kaynağı Git **dosyaları**, işlev için dosya hizmeti seçin. Ardından **LogFiles** > **uygulama** > **işlevleri** > **işlevi**  >  **your_function** için günlük dosyasına bakın.

### <a name="real-time-monitoring"></a>Gerçek zamanlı izleme

Bir komut satırı oturumu için günlük dosyaları yerel bir iş istasyonunda akışını yapabilirsiniz. Kullanım [Azure komut satırı arabirimi (CLI)](/cli/azure/install-azure-cli) veya [Azure PowerShell](/powershell/azure/overview).  

Azure CLI için aşağıdaki komutları kullanın oturum açmak, aboneliği ve günlük dosyalarını akışla aktarma'yı seçin:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <resource group name> --name <function app name>
```

Azure PowerShell için aşağıdaki komutları kullanın Azure hesabınızı eklemek için abonelik ve akışı günlük dosyalarını seçin:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <function app name> -Tail
```

Daha fazla bilgi için [günlüklerin akışını nasıl](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core günlüğe kaydetme](/aspnet/core/fundamentals/logging/)
