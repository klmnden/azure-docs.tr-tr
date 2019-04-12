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
ms.date: 04/04/2019
ms.author: glenga
ms.openlocfilehash: 0e4c308e745cbf2ffbc18f64101043aff3ddde35
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59495694"
---
# <a name="monitor-azure-functions"></a>Azure İşlevlerini İzleme

[Azure işlevleri](functions-overview.md) ile yerleşik tümleştirme sunan [Azure Application Insights](../azure-monitor/app/app-insights-overview.md) işlevleri izlemek için. Bu makalede, Azure işlevleri'nin sistem tarafından oluşturulan günlük dosyalarını Application Insights'a gönderme yapılandırma gösterilmektedir.

Günlüğü, performans ve hata veri topladığı için Application ınsights'ı kullanmanızı öneririz. Otomatik olarak performans anomalileri algılar ve işlevlerinizin nasıl kullanıldığını anlamanıza ve sorunları tanılamanıza yardımcı olması için güçlü analiz araçları içerir. Performansı ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır. Bu gibi durumlarda, Application Insights bile yerel bir işlev uygulaması projesi geliştirme sırasında kullanabilirsiniz. Daha fazla bilgi için [Application Insights nedir?](../azure-monitor/app/app-insights-overview.md)

Azure işlevleri ile gerekli Application Insights Araçları'nı yerleşik olarak ihtiyacınız olan işlev uygulamanızı bir Application Insights kaynağına bağlamak için bir geçerli bir izleme anahtarı.

## <a name="application-insights-pricing-and-limits"></a>Application Insights fiyatlandırma ve limitler

Application Insights Tümleştirmesi ile işlev uygulamaları ücretsiz deneyebilirsiniz. Ne kadar veri ücretsiz işlenmesi için günlük bir sınır yoktur. Test sırasında bu sınıra ulaşılmasına neden. Günlük limitinize yaklaştığınıza olduğunda azure portalında ve e-posta bildirimleri sağlar. Bu uyarıları kaçırmayın ve sınırına, yeni günlükler Application Insights sorgularda görünmez. Gereksiz sorun giderme süresini önlemek için sınırını dikkat edin. Daha fazla bilgi için [Application ınsights fiyatlandırma ve veri hacmini yönetme](../azure-monitor/app/pricing.md).

## <a name="enable-application-insights-integration"></a>Application Insights tümleştirmesini etkinleştirme

Bir işlev uygulaması için Application Insights veri göndermek, bir Application Insights kaynağına ait izleme anahtarını bilmesi gerekir. Anahtar adlı ayar bir uygulamada olmalıdır **appınsıghts_ınstrumentatıonkey**.

### <a name="new-function-app-in-the-portal"></a>Portalda yeni bir işlev uygulaması

Olduğunda, [Azure portalında işlev uygulamanızı oluşturmak](functions-create-first-azure-function.md), Application Insights tümleştirmesi, varsayılan olarak etkindir. Application Insights kaynağını işlev uygulamanızı aynı ada sahip ve aynı bölgede veya en yakın bölgede oluşturulur.

Oluşturulan Application Insights kaynağını gözden geçirmek için genişletmek için seçmek **Application Insights** penceresi. Değiştirebileceğiniz **yeni kaynak adı** veya farklı bir seçim **konumu** içinde bir [her Azure coğrafyası](https://azure.microsoft.com/global-infrastructure/geographies/) istediğiniz verileri depolamak.

![Bir işlev uygulaması oluştururken Application Insights'ı etkinleştir](media/functions-monitoring/enable-ai-new-function-app.png)

Seçeneğini belirlediğinizde **Oluştur**, Application Insights kaynağı olan, işlev uygulaması ile oluşturulan `APPINSIGHTS_INSTRUMENTATIONKEY` uygulama ayarları'nda. Her şeyi geçmeye hazırdır.

<a id="manually-connect-an-app-insights-resource"></a>
### <a name="add-to-an-existing-function-app"></a>Var olan bir işlevi uygulamaya ekleme 

Kullanarak bir işlev uygulaması oluşturduğunuzda [Azure CLI](functions-create-first-azure-function-azure-cli.md), [Visual Studio](functions-create-your-first-function-visual-studio.md), veya [Visual Studio Code](functions-create-first-function-vs-code.md), Application Insights kaynağı oluşturmalısınız. Daha sonra izleme anahtarı bu kaynağı olarak bir uygulama ayarı işlev uygulamanıza ekleyebilirsiniz.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

İşlevler'ın önceki sürümlerinde yerleşik izleme, artık önerildiği kullanılır. Böyle bir işlev uygulaması için Application Insights tümleştirmesi etkinleştirirken, ayrıca gerekir [yerleşik günlüğünü devre dışı](#disable-built-in-logging).  

## <a name="view-telemetry-in-monitor-tab"></a>İzleyici sekmesi içinde telemetri görüntüleme

İle [Application Insights tümleştirmesi etkin](#enable-application-insights-integration), telemetri verileri görüntüleyebilirsiniz **İzleyici** sekmesi.

1. Application Insights yapılandırıldıktan sonra en az bir kez çalışan bir işlev ait işlev uygulaması sayfasını seçin. Ardından **İzleyici** sekmesi.

   ![İzleyici sekmesi seçin](media/functions-monitoring/monitor-tab.png)

1. Seçin **Yenile** düzenli aralıklarla, işlev çağrılarını listesinde görünene kadar.

   Bu listenin telemetri istemcisi sunucuya aktarım için verileri toplu işlemleri sırasında görünmesini beş dakikaya kadar sürebilir. (Gecikme uygulanmaz [Canlı ölçümleri Stream](../azure-monitor/app/live-stream.md). Günlükleri doğrudan sayfaya aktarılır, böylece sayfa yüklediğinizde bu hizmeti işlevleri ana bilgisayara bağlanır.)

   ![Çağrılarını listesi](media/functions-monitoring/monitor-tab-ai-invocations.png)

1. Belirli işlev çağrısı için günlükleri görmek için seçin **tarih** bu çağrı için sütun bağlantı.

   ![Çağrı Ayrıntıları bağlantı](media/functions-monitoring/invocation-details-link-ai.png)

   Bu çağrı için günlük çıktısı, yeni bir sayfa görünür.

   ![Çağrı Ayrıntıları](media/functions-monitoring/invocation-details-ai.png)

Her iki sayfa olduğunu gördüğünüz bir **Application Insights'ta Çalıştır** verileri alır. Application Insights Analytics sorgunuzun bağlantı.

![Application Insights'ta çalıştır](media/functions-monitoring/run-in-ai.png)

Aşağıdaki sorguda görüntülenir. Çağırma listesi son 30 gün içinde sınırlı olduğunu görebilirsiniz. Liste en fazla 20 satırları gösterir (`where timestamp > ago(30d) | take 20`). Son 30 gün boyunca hiçbir sınır çağırma ayrıntıları listesidir.

![Application Insights Analytics çağırma listesi](media/functions-monitoring/ai-analytics-invocation-list.png)

Daha fazla bilgi için [sorgu telemetri verilerini](#query-telemetry-data) bu makalenin ilerleyen bölümlerinde.

## <a name="view-telemetry-in-application-insights"></a>Application ınsights telemetrisini görüntüleme

Azure portalında bir işlev uygulamasından Application Insights'ı açmak için işlev uygulamasına ait Git **genel bakış** sayfası. Altında **özellikler yapılandırıldı**seçin **Application Insights**.

![İşlev uygulaması genel bakış sayfasından Application Insights'ı açın](media/functions-monitoring/ai-link.png)

Application Insights'ı kullanma hakkında daha fazla bilgi için bkz: [Application Insights belgeleri](https://docs.microsoft.com/azure/application-insights/). Bu bölümde, Application Insights verilerini görüntüleme ile ilgili bazı örnekler gösterilmektedir. Zaten Application Insights ile biliyorsanız, doğrudan gidebilirsiniz siz [nasıl yapılandırılacağı ve telemetri verilerini özelleştirme hakkında bölümlere](#configure-categories-and-log-levels).

![Application Insights genel bakış sekmesi](media/functions-monitoring/metrics-explorer.png)

Application Insights aşağıdaki alanları davranışı, performans ve hatalar işlevlerinizi değerlendirirken yararlı olabilir:

| Sekme | Açıklama |
| ---- | ----------- |
| **[Hatalar](../azure-monitor/app/asp-net-exceptions.md)** |  Grafikler ve işlev hataları ve sunucu özel durumları göre uyarılar oluşturun. **İşlem adı** işlev adıdır. Bağımlılıklar için özel telemetri uygulamak sürece hataları bağımlılıkları gösterilmez. |
| **[Performans](../azure-monitor/app/performance-counters.md)** | Performans sorunlarını analiz edin. |
| **Sunucular** | Kaynak kullanımı ve sunucu başına aktarım hızı görüntüleyin. Bu veri, İşlevler, temel alınan kaynakları burada bogging senaryoları hata ayıklama için yararlı olabilir. Sunucuları denir **bulut rolü örnekleri**. |
| **[Ölçümler](../azure-monitor/app/metrics-explorer.md)** | Grafikler ve ölçümlerine bağlı uyarılar oluşturun. Ölçümler, işlev çağrıları, yürütme süresi ve başarı oranları sayısını içerir. |
| **[Canlı Ölçüm Akışı](../azure-monitor/app/live-stream.md)** | Gerçek zamanlı olarak oluşturulduğundan, ölçüm verilerini görüntüleyin. |

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

| Tablo | Açıklama |
| ----- | ----------- |
| **izlemeleri** | İşlev kodunu ve çalışma zamanı tarafından oluşturulan günlükleri. |
| **istek** | Her işlev çağrısı için bir istek. |
| **özel durumlar** | Çalışma zamanı tarafından oluşturulan özel durumlar. |
| **customMetrics** | Başarılı ve başarısız olan çağrılar, başarı oranı ve süre sayısı. |
| **customEvents** | Örneğin çalışma zamanı tarafından izlenen olayları: HTTP isteklerinin bir işlev tetikler. |
| **PerformanceCounters** | İşlevleriniz gerektirdikçe sunucularının performansı hakkında bilgi. |

Kullanılabilirlik testleri ve istemci ve tarayıcı telemetrisi diğer tablolara içindir. Veri eklemek üzere özel telemetri uygulayabilirsiniz.

Bazı işlevler özgü verileri her tabloda olduğu bir `customDimensions` alan.  Örneğin, aşağıdaki sorgu günlüğü düzeyine sahip tüm izlemeleri alır `Error`.

```
traces 
| where customDimensions.LogLevel == "Error"
```

Çalışma zamanı sağlar `customDimensions.LogLevel` ve `customDimensions.Category` alanları. İşlev kodunuzu yazdığınız günlüklerine ek alanlar sağlayabilir. Bkz: [günlük yapılandırılmış](#structured-logging) bu makalenin ilerleyen bölümlerinde.

## <a name="configure-categories-and-log-levels"></a>Kategorileri yapılandırmadan ve oturum düzeyleri

Application Insights herhangi özel bir yapılandırma kullanabilirsiniz. Varsayılan yapılandırma, yüksek hacimli verileri neden olabilir. Visual Studio Azure aboneliği kullanıyorsanız, Application Insights için veri sınırınıza neden olabilir. Bu makalenin sonraki bölümlerinde yapılandırma ve özelleştirme işlevlerinizi Application Insights'a gönderme veri öğrenin. Bir işlev uygulaması için günlük yapılandırılan [host.json] dosya.

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

[Host.json] bir işlev uygulaması, Application Insights'a gönderir ne kadar günlük dosyası oluşturur. Her kategori için göndermek için en düşük günlük düzeyi belirtin. İki örnekleri vardır: İlk örnek hedefleri [işlevleri sürüm 2.x çalışma zamanı](functions-versions.md#version-2x) (.NET Core) ve sürüm 1.x çalışma zamanı için ikinci örnektir.

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
* Günlükleri kategorili `Host.Aggregator`, Application Insights için tüm günlükleri gönderin. `Trace` Günlük düzeyi ne bazı günlükçüleri çağrısı ile aynı olup `Verbose`, ancak `Trace` içinde [host.json] dosya.
* Diğer tüm günlükler için yalnızca gönderme `Information` düzeyi ve üzeri için Application Insights.

Kategori değeri [host.json] günlük kaydı için aynı değeri ile başlayan tüm kategorileri denetler. `Host` içinde [host.json] denetimleri için günlüğü `Host.General`, `Host.Executor`, `Host.Results`ve benzeri.

Varsa [host.json] aynı dize ile başlayan birden çok kategori içeren uzun olanlarla ilk eşleştirilir. Çalışma zamanı dışında her şeyi istediğinizi düşünelim `Host.Aggregator` günlüğe kaydetmek için `Error` düzeyi, ancak isterseniz `Host.Aggregator` günlüğe kaydetmek için `Information` düzeyi:

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

Önceki bölümde belirtildiği gibi çalışma zamanı bir süre içinde işlev yürütmelerini hakkındaki verileri toplar. 1.000 çalışmadan, hangisi önce geliyorsa veya varsayılan süre 30 saniyedir. Bu ayarı yapılandırabilirsiniz [host.json] dosya.  Bir örneği aşağıda verilmiştir:

```json
{
    "aggregator": {
      "batchSize": 1000,
      "flushTimeout": "00:00:30"
    }
}
```

## <a name="configure-sampling"></a>Örnekleme yapılandırma

Application Insights'ı olan bir [örnekleme](../azure-monitor/app/sampling.md) üzerinde çok fazla telemetri verileri üreten Koruyabileceğiniz özellik bazen yoğun yük yürütme tamamlandı. Gelen yürütme hızını belirtilen eşiği aşarsa, rastgele bazı gelen yürütme yok saymak Application ınsights'ı başlatır. Saniye başına yürütme sayısı 20'dir için varsayılan ayar (beş sürümünde 1.x). Örnekleme yapılandırabileceğiniz [host.json].  Bir örneği aşağıda verilmiştir:

### <a name="version-2x"></a>Sürüm 2.x 

```json
{
  "logging": {
    "applicationInsights": {
      "samplingSettings": {
        "isEnabled": true,
        "maxTelemetryItemsPerSecond" : 20
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
    public class HttpTrigger2
    {
        private readonly TelemetryClient telemetryClient;

        /// Using dependency injection will guarantee that you use the same configuration for telemetry collected automatically and manually.
        public HttpTrigger2(TelemetryConfiguration telemetryConfiguration)
        {
            this.telemetryClient = new TelemetryClient(telemetryConfiguration);
        }

        [FunctionName("HttpTrigger2")]
        public Task<IActionResult> Run(
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
            this.telemetryClient.TrackEvent(evt);

            // Track a Metric
            var metric = new MetricTelemetry("Test Metric", DateTime.Now.Millisecond);
            metric.Context.User.Id = name;
            this.telemetryClient.TrackMetric(metric);

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
            this.telemetryClient.TrackDependency(dependency);

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

## <a name="dependencies"></a>Bağımlılıklar

İşlev diğer hizmetlere olan bağımlılıkları otomatik olarak görünmüyor. Bağımlılıkları göstermek için özel kod yazabilirsiniz. Örnek kodda örnekler için bkz [ C# özel telemetri bölümü](#log-custom-telemetry-in-c-functions). Örnek kod sonuçlanıyor bir *Uygulama Haritası* Application ınsights'ın aşağıdaki görüntü gibi görünür:

![Uygulama eşlemesi](./media/functions-monitoring/app-map.png)

## <a name="report-issues"></a>Rapor sorunları

İşlevlerde Application Insights Tümleştirmesi ile ilgili bir sorun bildirin veya öneri ya da isteği yapmak için [Github'da bir sorun oluşturun](https://github.com/Azure/Azure-Functions/issues/new).

## <a name="streaming-logs"></a>Akış Günlükleri

Bir uygulama geliştirirken, genellikle günlük kaydı bilgilerini neredeyse gerçek zamanlı görmek yararlı olur. Azure portalında veya bir komut satırı oturumunda, yerel bilgisayarınızda işlevlerinizi tarafından oluşturulan günlük dosyalarının akışını görüntüleyebilirsiniz.

Bu çıktı işlevlerinizi sırasında hata ayıklama sırasında görülen eşdeğerdir [yerel geliştirme](functions-develop-local.md). Daha fazla bilgi için [günlüklerin akışını nasıl](../app-service/troubleshoot-diagnostic-logs.md#streamlogs).

> [!NOTE]
> Akış günlükleri işlevleri konak yalnızca tek bir örneğini destekler. İşlevinizi birden fazla örneğe ölçeklendirildiğinde diğer örneklerindeki verilerin günlük stream'de gösterilmez. [Canlı ölçümleri Stream](../azure-monitor/app/live-stream.md) Application Insights'ta birden çok örneği desteklenir. Ayrıca, gerçek zamanlı, akış analizini de dayanır ancak [veri örneklenen](#configure-sampling).

### <a name="portal"></a>Portal

Portalda akış günlüklerini görüntülemek için seçin **Platform özellikleri** işlev uygulamanızı sekmesindedir. Ardından, altında **izleme**, seçin **günlük akışını**.

![Portaldaki akış günlüklerini etkinleştirme](./media/functions-monitoring/enable-streaming-logs-portal.png)

Bu akış günlüğü uygulamanızı bağlanır ve uygulama günlüklerini penceresinde görüntülenir. Arasında geçiş yapabilirsiniz **uygulama günlükleri** ve **Web sunucusu günlüklerini**.  

![Portalda akış günlüklerini görüntüleyin](./media/functions-monitoring/streaming-logs-window.png)

### <a name="azure-cli"></a>Azure CLI

Akış günlüklerini kullanarak etkinleştirebilirsiniz [Azure komut satırı arabirimi (CLI)](/cli/azure/install-azure-cli). Azure CLI için aşağıdaki komutları kullanın oturum açmak, aboneliği ve günlük dosyalarını akışla aktarma'yı seçin:

```azurecli
az login
az account list
az account set --subscription <subscriptionNameOrId>
az webapp log tail --resource-group <RESOURCE_GROUP_NAME> --name <FUNCTION_APP_NAME>
```

### <a name="azure-powershell"></a>Azure PowerShell

Akış günlüklerini kullanarak etkinleştirebilirsiniz [Azure PowerShell](/powershell/azure/overview). PowerShell için aşağıdaki komutları kullanın Azure hesabınızı eklemek için abonelik ve akışı günlük dosyalarını seçin:

```powershell
Add-AzAccount
Get-AzSubscription
Get-AzSubscription -SubscriptionName "<subscription name>" | Select-AzSubscription
Get-AzWebSiteLog -Name <FUNCTION_APP_NAME> -Tail
```

## <a name="disable-built-in-logging"></a>Yerleşik günlük devre dışı bırakma

Application Insights'ı etkinleştirdiğinizde, Azure depolama kullanan yerleşik günlük kaydını devre dışı bırakın. Yerleşik günlük hafif iş yükleriyle test edilmesi için yararlıdır, ancak yüksek yük üretim kullanımı için tasarlanmamıştır. Üretim izlemek için Application ınsights'ı öneririz. Üretimde yerleşik günlük kaydı kullandıysanız, günlük kaydı eksik Azure depolama alanında azaltma nedeniyle olabilir.

Yerleşik günlük kaydetme devre dışı bırakmak için silme `AzureWebJobsDashboard` uygulama ayarı. Uygulama ayarları Azure portalında silme hakkında daha fazla bilgi için bkz: **uygulama ayarları** bölümünü [bir işlev uygulaması yönetme](functions-how-to-use-azure-function-app-settings.md#settings). Uygulama ayarı silmeden önce Azure depolama Tetikleyicileri veya bağlamaları için aynı işlev uygulamasında var olan bir işlev yok ayarını kullandığınızdan emin olun.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Application Insights](/azure/application-insights/)
* [ASP.NET Core günlüğe kaydetme](/aspnet/core/fundamentals/logging/)

[host.json]: functions-host-json.md
