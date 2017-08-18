---
title: Node.js hizmetlerini Azure Application Insights ile izleme | Microsoft Docs
description: "Application Insights ile Node.js hizmetlerindeki performansı izleyin ve sorunları tanılayın."
services: application-insights
documentationcenter: nodejs
author: joshgav
manager: carmonm
ms.assetid: 2ec7f809-5e1a-41cf-9fcd-d0ed4bebd08c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/01/2017
ms.author: bwren
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: 76a8025cd2a67533beb321c88e924517c1977dfc
ms.contentlocale: tr-tr
ms.lasthandoff: 05/02/2017

---

# <a name="monitor-your-nodejs-services-and-apps-with-application-insights"></a>Application Insights ile Node.js hizmetlerinizi ve uygulamalarınızı izleme

[Azure Application Insights](app-insights-overview.md), [performans ve diğer sorunları keşfetmeye ve hızlıca tanılamaya](app-insights-detect-triage-diagnose.md) yardımcı olmak üzere, arka uç hizmetlerinizi ve bileşenlerinizi dağıtmanızdan sonra izler. Veri merkeziniz, Azure VM’leri, Web Apps ve hatta diğer genel bulutlar dahil herhangi bir yerde barındırılan Node.js hizmetleri için kullanın.

İzleme verilerinizi almak, depolamak ve araştırmak için kodunuza aracı ekleme ve Azure’da karşılık gelen Application Insights kaynağını ayarlamaya yönelik aşağıdaki yönergeleri izleyin. Aracı daha fazla analiz ve araştırma için verileri bu kaynağa gönderir.

Node.js aracısı gelen ve giden HTTP isteklerini, birçok sistem ölçümünü ve özel durumları otomatik olarak izleyebilir. Sürüm 0.20 sonrası sürümlerde, `mongodb`, `mysql` ve `redis` gibi bazı yaygın üçüncü taraf paketlerini de izleyebilir. Gelen bir HTTP isteği ile ilgili tüm olaylar, daha hızlı sorun giderme için birbiriyle ilişkilendirilir.

Daha sonra açıklanacak olan aracı API’si ile el ile izleyerek, uygulama ve sisteminizin daha fazla yönünü izleyebilirsiniz.

![Örnek performans izleme grafikleri](./media/app-insights-nodejs/10-perf.png)

## <a name="getting-started"></a>Başlarken

Şimdi, bir uygulama veya hizmet için izlemeyi ayarlama adımlarına bakalım.

### <a name="resource"></a> App Insights kaynağı oluşturma

**Başlamadan önce**, bir Azure aboneliğine sahip olduğunuzdan emin olun veya [ücretsiz olarak yeni bir tane edinin][azure-free-offer]. Kuruluşunuzun bir Azure aboneliğini zaten varsa, yöneticiniz [bu yönergeleri][add-aad-user] izleyerek sizi aboneliğe ekleyebilir.

[azure-free-offer]: https://azure.microsoft.com/en-us/free/
[add-aad-user]: https://docs.microsoft.com/en-us/azure/active-directory/active-directory-users-create-azure-portal

Şimdi [Azure portalında][portal] oturum açın ve aşağıda gösterilen şekilde bir Application Insights kaynağı oluşturun - "Yeni" > "Geliştirici araçları" > "Application Insights" öğesine tıklayın. Kaynak; telemetri verilerini, bu veriler için depolamayı, kayıtlı rapor ve panoları, kural ve uyarı yapılandırmasını almak ve diğer işlemlere yönelik bir uç nokta içerir.

![App Insights kaynağı oluşturma](./media/app-insights-nodejs/03-new_appinsights_resource.png)

Kaynak oluşturma sayfasındaki uygulama türü açılır listesinden "Node.js Uygulaması" öğesini seçin. Uygulama türü, sizin için oluşturulan varsayılan pano ve rapor kümesini belirler. Ancak endişelenmeyin, herhangi bir Application Insights kaynağı herhangi bir dil ve platformdan veri toplayabilir.

![Yeni App Insights kaynağı formu](./media/app-insights-nodejs/04-create_appinsights_resource.png)

### <a name="agent"></a> Node.js aracısını ayarlama

Şimdi aracıyı veri toplayabilmesi için uygulamanıza eklemeniz gerekiyor.
İlk olarak, aşağıda gösterildiği gibi kaynağınızın İzleme Anahtarını (bundan böyle `ikey`‘iniz olarak ifade edilecektir) portaldan kopyalayın. App Insights sistemi, verileri Azure kaynağınıza eşlemek için bu anahtarı kullanır; bu nedenle, bu anahtarı aracının kullanabilmesi için bir ortam değişkeninde ya da kodunuzda belirtmeniz gerekir.  

![İzleme anahtarını kopyalama](./media/app-insights-nodejs/05-appinsights_ikey_portal.png)

Ardından, Node.js aracı kitaplığını package.json aracılığıyla uygulamanızın bağımlılıklarına ekleyin. Uygulamanızın kök klasöründen şunu çalıştırın:

```bash
npm install applicationinsights --save
```

Şimdi kitaplığı kodunuza açıkça yüklemeniz gerekir. Aracı diğer birçok kitaplığa izleme eklediği için, aracıyı diğer `require` deyimlerinden de önce olmak üzere mümkün olduğunca erken yüklemeniz gerekir. Başlamak için ilk .js dosyanızın üstüne şunu ekleyin:

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>");
appInsights.start();
```

`setup` yöntemi, izleme anahtarını (ve bu nedenle Azure kaynağını) izlenen tüm öğeler için varsayılan olarak kullanılacak şekilde yapılandırır. Telemetri verilerini toplayıp göndermeye başlamak için yapılandırma tamamlandıktan sonra `start` öğesini çağırın.

Ayrıca `setup()` veya `getClient()` konumuna el ile geçirmek yerine APPINSIGHTS\_INSTRUMENTATIONKEY ortam değişkeni aracılığıyla bir ikey sağlayabilirsiniz. Bu uygulama, ikey’leri yürütülen kaynak kodunun dışında tutmanıza ve farklı ortamlar için farklı ikey’ler belirtmenize olanak tanır.

Ek yapılandırma seçenekleri aşağıda belirtilmiştir.

İzleme anahtarını boş olmayan bir dizeye ayarlayarak, aracıyı telemetri göndermeden deneyebilirsiniz.

### <a name="monitor"></a> Uygulamanızı izleme

Aracı, Node.js çalışma zamanı ve bazı yaygın üçüncü taraf modülleriyle ilgili telemetriyi otomatik olarak toplar. Şimdi uygulamanızı kullanarak bu verilerin bazılarını oluşturun.

Ardından, [Azure portalında][portal] daha önce oluşturduğunuz Application Insights kaynağına göz atın ve aşağıdaki görüntüde gösterildiği gibi Genel Bakış zaman çizelgesinde ilk birkaç veri noktanızı arayın. Daha fazla ayrıntı için grafiklere tıklayın.

![İlk veri noktaları](./media/app-insights-nodejs/12-first-perf.png)

Aşağıdaki görüntüde gösterildiği gibi, uygulamanız için bulunan topolojiyi görüntülemek için Uygulama eşlemesi düğmesine tıklayın. Daha fazla bilgi için eşlemedeki bileşenlere tıklayın.

![Basit uygulama eşlemesi](./media/app-insights-nodejs/06-appinsights_appmap.png)

Uygulamanız hakkında daha fazla bilgi almak ve sorunları gidermek için "Araştır" bölümü altında mevcut olan diğer görünümleri kullanın.

![Araştır bölümü](./media/app-insights-nodejs/07-appinsights_investigate_blades.png)

#### <a name="no-data"></a>Veri yok mu?

Aracı gönderilecek verileri toplu hale getirdiği için, öğelerin portalde gösterilmesi biraz gecikebilir. Verileri kaynağınızda görmüyorsanız, aşağıdaki düzeltmelerden bazılarını deneyin:

* Uygulamayı biraz daha kullanın; daha fazla telemetri oluşturmak için daha fazla eylem gerçekleştirin.
* Portal kaynak görünümünde **Yenile**’ye tıklayın. Grafikler kendilerini düzenli aralıklarla otomatik olarak yeniler, ancak yenileme işlemi bunu hemen gerçekleşmeye zorlar.
* [Gerekli giden bağlantı noktalarının](app-insights-ip-addresses.md) açık olduğunu doğrulayın.
* [Ara](app-insights-diagnostic-search.md) kutucuğunu açın ve olayları tek tek arayın.
* [SSS][] sayfasını inceleyin.


## <a name="agent-configuration"></a>Aracı Yapılandırması

Aracının yapılandırma yöntemleri ve varsayılan değerleri aşağıda verilmiştir.

Bir hizmetteki olayları tam olarak ilişkilendirmek için `.setAutoDependencyCorrelation(true)` ayarını yaptığınızdan emin olun. Bunun yapılması, aracının Node.js içindeki zaman uyumsuz geri çağırmalar arasında içeriği izlemesine olanak tanır.

```javascript
const appInsights = require("applicationinsights");
appInsights.setup("<instrumentation_key>")
    .setAutoDependencyCorrelation(false)
    .setAutoCollectRequests(true)
    .setAutoCollectPerformance(true)
    .setAutoCollectExceptions(true)
    .setAutoCollectDependencies(true)
    .start();
```

## <a name="agent-api"></a>Aracı API'si

<!-- TODO: Fully document agent API. -->

.NET aracı API’si [burada](app-insights-api-custom-events-metrics.md) eksiksiz olarak açıklanmıştır.

Application Insights Node.js istemcisini kullanarak herhangi bir istek, olay, ölçüm veya özel durumu izleyebilirsiniz. Aşağıdaki örnek, kullanılabilir API'lerden bazılarını göstermektedir.

```javascript
let appInsights = require("applicationinsights");
appInsights.setup().start(); // assuming ikey in env var
let client = appInsights.getClient();

client.trackEvent("my custom event", {customProperty: "custom property value"});
client.trackException(new Error("handled exceptions can be logged with this method"));
client.trackMetric("custom metric", 3);
client.trackTrace("trace message");

let http = require("http");
http.createServer( (req, res) => {
  client.trackRequest(req, res); // Place at the beginning of your request handler
});
```

### <a name="track-your-dependencies"></a>Bağımlılıklarınızı izleme

```javascript
let appInsights = require("applicationinsights");
let client = appInsights.getClient();

var success = false;
let startTime = Date.now();
// execute dependency call here....
let duration = Date.now() - startTime;
success = true;

client.trackDependency("dependency name", "command name", duration, success);
```

### <a name="add-a-custom-property-to-all-events"></a>Tüm olaylara özel bir özellik ekleme

```javascript
appInsights.client.commonProperties = {
    environment: process.env.SOME_ENV_VARIABLE
};
```

### <a name="track-http-get-requests"></a>HTTP GET isteklerini izleme

```javascript
var server = http.createServer((req, res) => {
    if ( req.method === "GET" ) {
            appInsights.client.trackRequest(req, res);
    }
    // other work here....
    res.end();
});
```

### <a name="track-server-startup-time"></a>Sunucu başlangıç saatini izleme

```javascript
let start = Date.now();
server.on("listening", () => {
    let duration = Date.now() - start;
    appInsights.client.trackMetric("server startup time", duration);
});
```

## <a name="more-resources"></a>Diğer kaynaklar

* [Portalda telemetrinizi izleme](app-insights-dashboards.md)
* [Telemetriniz üzerinden Analiz sorguları yazma](app-insights-analytics-tour.md)

<!--references-->

[portal]: https://portal.azure.com/
[SSS]: app-insights-troubleshoot-faq.md

