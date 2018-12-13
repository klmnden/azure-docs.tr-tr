---
title: Arama trafiği analizi - Azure Search'ü
description: Kullanıcılarınız ve verileriniz hakkında Öngörüler Microsoft Azure üzerinde barındırılan bulut arama hizmeti olan Azure arama için arama trafiği analizi etkinleştirin.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 04/05/2017
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 4cc7434508e49715e95c87421db2bbed7e20de05
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310309"
---
# <a name="what-is-search-traffic-analytics"></a>Arama trafiği analizi nedir
Arama trafiği analizi, arama hizmetiniz için bir geri bildirim döngüsü uygulamak için kullanılan bir desendir. Bu düzen, gerekli verileri ve Application Insights, birden çok platform hizmetleri izlemek için endüstri lideri kullanarak toplamak nasıl açıklar.

Arama trafiği analizi, arama hizmetinize yönelik görünürlük elde edin ve kullanıcılarınız ve davranışları hakkında Öngörüler sağlar. Neyin kullanıcılarınızın seçin hakkındaki verileri sağlayarak, daha fazla arama deneyiminizi geliştirmeye kararlar ve sonuçları ne beklenmiyordu olduğunda kapatmak için mümkündür.

Azure arama, ayrıntılı izleme ve izleme sağlamak için Azure Application Insights ve Power BI ile tümleşen bir telemetri çözümü sunar. Yalnızca API Azure Search ile etkileşimi olduğu için bu sayfadaki yönergeleri izleyerek search'ü kullanarak geliştiriciler tarafından telemetri uygulanmalıdır.

## <a name="identify-the-relevant-search-data"></a>İlgili arama verileri tanımlayan

Yararlı arama ölçümleri sağlamak için bazı sinyaller arama uygulaması, kullanıcıların oturum açmak gereklidir. Bu sinyalleri kullanıcılar, ilgileniyor içeriği ve bunların ilgili dikkate türünü belirtir.

Arama trafiği analizi gereken iki sinyal vardır:

1. Oluşturulan kullanıcı arama olayları: yalnızca bir kullanıcı tarafından başlatılan arama sorguları ilginç. Modelleri, ek içeriği veya herhangi bir iç bilgi doldurmak için kullanılan arama istekleri önemli değildir ve Eğ ve sonuçlarınızı bias.

2. Kullanıcı tarafından oluşturulan olayları tıklayın: Bu belgedeki tıklama ile bir kullanıcı bir arama sorgusundan döndürülen belirli bir sonucun seçilmesi diyoruz. Genellikle bir belgeye özgü arama sorgusu için ilgili bir sonucu olduğunu anlamına gelir.

Bağıntı kimliği ile arama ve tıklatın olayları bağlantılandırarak, uygulamanız kullanıcıların davranışlarını analiz etmek mümkündür. Bu arama ınsights yalnızca arama trafik günlükleriyle almak mümkün.

## <a name="how-to-implement-search-traffic-analytics"></a>Arama trafiği analizi gerçekleştirme

Kullanıcı etkileşimine göre değişecek şekilde önceki bölümde belirtilen sinyaller arama uygulamadan toplanması gerekir. Application Insights, Genişletilebilir izleme çözümünü, esnek izleme seçenekleri ile birden çok platform için kullanılabilir. Application Insights'ın kullanım veri analizini kolaylaştırmak için Azure Search tarafından oluşturulan Power BI arama raporları avantajlarından yararlanmanıza imkan sağlar.

İçinde [portalı](https://portal.azure.com) sayfa arama trafiği analizi dikey penceresinde, Azure Search hizmeti için bu telemetri düzenini izleyerek için kural sayfası içerir. Ayrıca seçin veya bir Application Insights kaynağı oluşturun ve tek bir yerde gerekli verileri görebilirsiniz.

![Arama trafiği analizi yönergeleri][1]

### <a name="1-select-an-application-insights-resource"></a>1. Bir Application Insights kaynağını seçin

Kullanmak veya zaten yoksa, oluşturmak için Application Insights kaynağını seçmeniz gerekir. Zaten gerekli özel olayları oturum kullanımda olan bir kaynak kullanabilirsiniz.

Tüm uygulama türleri, yeni bir Application Insights kaynağı oluştururken, bu senaryo için geçerlidir. Kullanmakta olduğunuz platform için en uygun olanı seçin.

Uygulamanızın telemetri istemcisi oluşturmak için izleme anahtarı gerekir. Application Insights portal panosunda alın veya kullanmak istediğiniz örneği seçildiğinde, arama trafiği analizi sayfasından alabilirsiniz.

### <a name="2-instrument-your-application"></a>2. Uygulamanızı izleyin

Burada Yukarıdaki adımda oluşturduğunuz Application Insights kaynağını kullanarak, kendi arama uygulamasını izleme bu aşamasıdır. Bu işlem için dört adım vardır:

**EDİYORUM. Bir telemetri istemcisi oluşturma** bu Application Insights kaynağını olayları gönderen nesnesidir.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Diğer diller ve platformlar için tamamına bakın [listesi](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. İstek bağıntı kimliği Ara** arama istekleri tıklama ile ilişkilendirmek için bu iki ayrı olayları ilişkili bir bağıntı kimliği gereklidir. Azure Search'ü sahip bir üstbilgi istediğinizde bir arama kimliğiyle sağlar:

*C#*

    // This sample uses the Azure Search .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<ServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**III. Arama olayları günlüğe kaydet**

Bir kullanıcı tarafından bir arama talebi her zaman, olarak Application Insights ile özel bir olay aşağıdaki şemaya sahip bir arama olayının oturum açmanız:

**ServiceName**: arama hizmeti adı (dize) **SearchId**: arama sorgusu benzersiz tanıtıcısı (GUID) (arama yanıtta gelirse) **IndexName**: olacak şekilde arama hizmeti dizini (dize) Sorgulanan **QueryTerms**: (dize) arama terimleri kullanıcı tarafından girilen **ResultCount**: döndürülen belge (int) sayısı (gelen arama yanıtta)  **ScoringProfile**: varsa, kullanılan Puanlama profili adı (dize)

> [!NOTE]
> İstek sayısı $count ekleyerek oluşturulan kullanıcı sorguları arama sorgunuz için true =. Daha fazla bilgi [burada](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Yalnızca kullanıcılar tarafından oluşturulan arama sorguları oturum açmayı unutmayın.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**IV. Click oturum olayları**

Arama analiz amacıyla oturum açmış olmanız gerekir sinyaldir belge, bir kullanıcı tıkladığında her zaman. Application Insights özel olaylar aşağıdaki şema ile bu olaylarını günlüğe kaydedecek şekilde kullanın:

**ServiceName**: arama hizmeti adı (dize) **SearchId**: ilgili arama sorgusunun benzersiz tanıtıcısı (GUID) **DocId**: (dize) belge tanımlayıcısı **konumu**: (int) derece belgenin arama sonuçları sayfası

> [!NOTE]
> Uygulamanızı Kardinal sırayla konumunu ifade eder. Bu her zaman aynı, karşılaştırma için izin verecek şekilde olduğu sürece bu sayı ayarlamak ücretsizdir.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Power BI Desktop ile analiz

Uygulamanızı izleme eklenmiş ve uygulamanızı Application ınsights'ı doğru bir şekilde bağlandığından doğrulandı sonra Power BI desktop için Azure Search tarafından oluşturulan önceden tanımlanmış bir şablon kullanabilirsiniz.
Bu şablon grafikleri içerir ve yardımcı olacak tablolar ilgi ve arama performansını geliştirmek için daha bilinçli kararlar alın.

Power BI Masaüstü şablonu oluşturmak için üç parça Application Insights hakkında bilgi gerekir. Bu verileri kullanmak için bir kaynak seçtiğinizde arama trafiği analizi sayfasında bulunabilir.

![Application Insights verilerini arama trafiği analizi dikey][2]

Power BI Masaüstü şablonuna dahil ölçümleri:

*   (Ctrl) oranı üzerinden tıklayın: toplam arama sayısı için belirli bir belge tıklayarak kullanıcılar oranı.
*   Tıklama olmadan aramalar: koşulları için hiçbir tıklama kayıt sorguları üst
*   Belgeler'en çok tıklanan: Son 24 saat içindeki, 7 gün ve 30 gün tıklanan Kimliğine göre belge en.
*   Popüler belge terimi çiftleri: tıklattığında, belgede neden olan koşulları tarafından tıklamalarına sıralı.
*   Zaman tıklayın: arama sorgusu daraltılmasından göre tıklama kümelenmiş

![Application Insights okuma için Power BI şablonu][3]


## <a name="next-steps"></a>Sonraki Adımlar
Arama hizmetinizi hakkında güçlü ve bilgilendirici veri almak için arama uygulamanızı izleyin.

Application Insights hakkında daha fazla bilgi bulabilirsiniz [burada](https://go.microsoft.com/fwlink/?linkid=842905). Application Insights'ı ziyaret edin [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/application-insights/) , farklı hizmet katmanları hakkında daha fazla bilgi edinmek için.

Muhteşem raporlar oluşturma hakkında daha fazla bilgi edinin. Bkz: [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) için Ayrıntılar

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
