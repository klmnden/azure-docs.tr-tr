---
title: "Node.js uygulamanızı izlemek için Application Insights SDK’sı ekleme | Microsoft Belgeleri"
description: "Application Insights ile şirket içi veya Microsoft Azure web uygulamanızın kullanımını, kullanılabilirliğini ve performansını analiz edin."
services: application-insights
documentationcenter: 
author: alancameronwills
manager: douge
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/30/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: fb80168b38be88ab18952569e6b6f9bcb53d473a


---
# <a name="add-application-insights-sdk-to-monitor-your-nodejs-app"></a>Node.js uygulamanızı izlemek için Application Insights SDK’sı ekleme
*Application Insights önizlemededir.*

[Visual Studio Application Insights](app-insights-overview.md), [performans sorunlarını ve özel durumlarını saptayıp tanılamanıza](app-insights-detect-triage-diagnose.md) ve [uygulamanızın nasıl kullanıldığını keşfetmenize](app-insights-overview-usage.md) yardımcı olmak amacıyla canlı uygulamanızı izler. Azure web uygulamalarının yanı sıra şirket içi kendi IIS sunucularınızda veya Azure sanal makinelerinde barındırılan uygulamalar için çalışır.

SDK, gelen HTTP isteği oranlarının ve yanıtlarının, performans sayaçlarının (CPU, bellek, RPS) ve işlenmeyen özel durumların otomatik olarak toplanmasını sağlar. Ayrıca, bağımlılıkları, ölçümleri veya diğer olayları izlemek için özel çağrılar ekleyebilirsiniz.

![Örnek performans izleme grafikleri](./media/app-insights-nodejs/10-perf.png)

#### <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* Visual Studio 2013 veya üstü. Ne kadar yeniyse o kadar iyidir.
* Bir [Microsoft Azure](http://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi buna ekleyebilir.

## <a name="a-nameaddacreate-an-application-insights-resource"></a><a name="add"></a>Application Insights kaynağı oluşturma
[Azure portalında][portal] oturum açın ve yeni bir Application Insights kaynağı oluşturun. Azure’da [kaynak][roles] bir hizmetin örneğidir. Bu kaynak, uygulamanızdan alınan telemetri verilerinin analiz edilip size sunulacağı yerdir.

![Yeni, Application Insights öğesine tıklayın](./media/app-insights-nodejs/01-new-asp.png)

Uygulama türü olarak Diğer’i seçin. Uygulama türü seçimi, kaynak dikey pencerelerinin varsayılan içeriğini ve [Ölçüm Gezgini][metrics] içinde görünen özellikleri belirler.

#### <a name="copy-the-instrumentation-key"></a>İzleme Anahtarını Kopyalama
Kaynağı tanımlayan bu anahtarı kısa bir süre sonra verileri kaynağa yönlendirmek için SDK’ya yükleyeceksiniz.

![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/app-insights-nodejs/02-props-asp.png)

## <a name="a-namesdka-install-the-sdk-in-your-application"></a><a name="sdk"></a> Uygulamanıza SDK'yı yükleme
```
npm install applicationinsights --save
```

## <a name="usage"></a>Kullanım
Bu işlem istek izleme, işlenmemiş özel durum izleme ve sistem performansı izlemeyi (CPU/Bellek/RPS) etkinleştirir.

```javascript

var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>").start();
```

İzleme anahtarı APPINSIGHTS_INSTRUMENTATIONKEY ortam değişkeninde de ayarlanabilir. Bu yapılırsa `appInsights.setup()` veya `appInsights.getClient()` çağrılırken bağımsız değişken gerekmez.

SDK’yı telemetri verilerini göndermeden deneyebilirsiniz: İzleme anahtarını boş olmayan bir dize olarak ayarlayın.

## <a name="a-nameruna-run-your-project"></a><a name="run"></a> Projenizi çalıştırma
Uygulamanızı çalıştırın ve deneyin: Birkaç telemetri oluşturmak için farklı sayfalar açın.

## <a name="a-namemonitora-view-your-telemetry"></a><a name="monitor"></a> Telemetrinizi görüntüleme
[Azure portal](https://portal.azure.com)’a geri dönün ve Application Insights kaynağınıza göz atın.

Genel Bakış sayfasında veri arayın. İlk olarak yalnızca bir veya iki nokta görürsünüz. Örneğin:

![Daha fazla veri için tıklayın](./media/app-insights-nodejs/12-first-perf.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın. [Ölçümler hakkında daha fazla bilgi edinin.][perf]

#### <a name="no-data"></a>Veri yok mu?
* Birkaç telemetri oluşturması için farklı sayfaları açarak uygulamayı kullanın.
* Olayları tek tek görmek için [Ara](app-insights-diagnostic-search.md) kutucuğunu açın. Bazı durumlarda olayların ölçüm ardışık düzenine ulaşması biraz daha uzun sürer.
* Birkaç saniye bekleyin ve **Yenile**’ye tıklayın. Grafikler kendilerini düzenli olarak yeniler, ancak bazı verilerin görüntülenmesini bekliyorsanız el ile yenileyebilirsiniz.
* Bkz. [Sorun giderme][qna].

## <a name="publish-your-app"></a>Uygulamanızı yayımlama
Şimdi uygulamanızı IIS veya Azure’a dağıtın ve verilerin birikmesini izleyin.

#### <a name="no-data-after-you-publish-to-your-server"></a>Sunucunuza yayımladıktan sonra veri yok mu?
Sunucunuzun güvenlik duvarında giden trafik için şu bağlantı noktalarını açın:

* `dc.services.visualstudio.com:443`
* `f5.services.visualstudio.com:443`

#### <a name="trouble-on-your-build-server"></a>Derleme sunucunuzda sorun mu yaşıyorsunuz?
Lütfen [bu Sorun Giderme maddesine](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild) bakın.

## <a name="customized-usage"></a>Özelleştirilmiş Kullanım
### <a name="disabling-autocollection"></a>Otomatik toplamayı devre dışı bırakma
```javascript
import appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false)
    .setAutoCollectPerformance(false)
    .setAutoCollectExceptions(false)
    // no telemetry will be sent until .start() is called
    .start();
```

### <a name="custom-monitoring"></a>Özel izleme
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

client.trackEvent("custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");
```

[Telemetri API’si hakkında daha fazla bilgi edinin](app-insights-api-custom-events-metrics.md).

### <a name="using-multiple-instrumentation-keys"></a>Birden çok izleme anahtarı kullanma
```javascript
import appInsights = require("applicationinsights");

// configure auto-collection with one instrumentation key
appInsights.setup("<instrumentation_key>").start();

// get a client for another instrumentation key
var otherClient = appInsights.getClient("<other_instrumentation_key>");
otherClient.trackEvent("custom event");
```

## <a name="examples"></a>Örnekler
### <a name="tracking-dependency"></a>Bağımlılık izleme
```javascript
import appInsights = require("applicationinsights");
var client = appInsights.getClient();

var startTime = Date.now();
// execute dependency call
var endTime = Date.now();

var elapsedTime = endTime - startTime;
var success = true;
client.trackDependency("dependency name", "command name", elapsedTime, success);
```



### <a name="manual-request-tracking-of-all-get-requests"></a>Tüm "GET" istekleri için el ile istek izleme
```javascript
var http = require("http");
var appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoCollectRequests(false) // disable auto-collection of requests for this example
    .start();

// assign common properties to all telemetry sent from the default client
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};

// track a system startup event
appInsights.client.trackEvent("server start");

// create server
var port = process.env.port || 1337
var server = http.createServer(function (req, res) {
    // track all "GET" requests
    if(req.method === "GET") {
        appInsights.client.trackRequest(req, res);
    }

    res.writeHead(200, { "Content-Type": "text/plain" });
    res.end("Hello World\n");
}).listen(port);

// track startup time of the server as a custom metric
var start = +new Date;
server.on("listening", () => {
    var end = +new Date;
    var duration = end - start;
    appInsights.client.trackMetric("StartupTime", duration);
});
```

## <a name="next-steps"></a>Sonraki adımlar
* [Portalda telemetrinizi izleme](app-insights-dashboards.md)
* [Telemetriniz üzerinden Analiz sorguları yazma](app-insights-analytics-tour.md)

<!--Link references-->

[knowUsers]: app-insights-overview-usage.md
[metrics]: app-insights-metrics-explorer.md
[perf]: app-insights-web-monitor-performance.md
[portal]: http://portal.azure.com/
[qna]: app-insights-troubleshoot-faq.md
[roles]: app-insights-resources-roles-access-control.md



<!--HONumber=Nov16_HO2-->


