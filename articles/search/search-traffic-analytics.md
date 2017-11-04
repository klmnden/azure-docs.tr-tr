---
title: "Azure arama için arama trafiği analizi | Microsoft Docs"
description: "Azure Search, kullanıcılarınız ve verilerinizi hakkında Öngörüler kilidini açmak için Microsoft Azure üzerinde barındırılan bulut arama hizmeti için arama trafiği analytics etkinleştirin."
services: search
documentationcenter: 
author: bernitorres
manager: jlembicz
editor: 
ms.assetid: b31d79cf-5924-4522-9276-a1bb5d527b13
ms.service: search
ms.devlang: multiple
ms.workload: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 04/05/2017
ms.author: betorres
ms.openlocfilehash: 303ca5c820f573dc0b58f1910f258403c3baad2a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-search-traffic-analytics"></a>Arama trafiği analytics nedir
Arama trafiği analytics arama hizmetiniz için bir geri döngü uygulamak için bir desen ' dir. Bu deseni gerekli veri ve Application Insights, birden çok platform hizmetlerinde izleme için bir endüstri lideri kullanarak Topla açıklar.

Arama trafiği analytics, arama hizmetinizi görünürlük elde etmek ve kullanıcılarınızın ve davranışları hakkında Öngörüler elde edebilirsiniz sağlar. Hangi kullanıcıların seçin hakkındaki verileri sağlayarak, daha fazla arama deneyiminizi geliştirmeye kararları ve sonuçları ne beklenen olduğunda kapatmak için mümkündür.

Azure arama ayrıntılı izleme ve izleme sağlamak için Azure Application Insights ve Power BI'ı tümleştiren bir telemetri çözüm sunar. Azure Search ile etkileşimi yalnızca API'leri aracılığıyla olduğundan, telemetriyi bu sayfasındaki yönergeleri izleyerek arama kullanan geliştiriciler tarafından uygulanmalıdır.

## <a name="identify-the-relevant-search-data"></a>İlgili arama verilerini tanımlayın

Yararlı arama ölçümleri sağlamak için arama uygulaması kullanıcılardan bazı sinyalleri oturum gereklidir. Bu sinyalleri kullanıcılar de ilginizi çekiyor mu içerik ve kendi gereksinimlerine göre uygun düşünün türünü belirtir.

Arama trafiği Analytics gereken iki sinyalleri vardır:

1. Oluşturulan kullanıcı arama olayları: yalnızca bir kullanıcı tarafından başlatılan arama sorguları ilginç. Arama istekleri modelleri, ek içerik veya herhangi bir iç bilgi doldurmak için kullanılan önemli değildir ve eğme ve sonuçlarınızı bias.

2. Oluşturulan kullanıcı tıklama olayları: Bu belgede tıklama tarafından size bir arama sorgusundan döndürülen belirli bir sonucun seçilmesi kullanıcı bakın. Tıklama genellikle bir belge belirli arama sorgusu için ilgili bir sonuç anlamına gelir.

Arama ve tıklatın olayları bir bağıntı kimliği ile bağlayarak, uygulamanızın kullanıcı davranışlarını analiz etmek mümkündür. Bu arama Öngörüler yalnızca arama trafiği günlükleri ile elde etmek mümkün.

## <a name="how-to-implement-search-traffic-analytics"></a>Arama trafiği analiz gerçekleştirme

Kullanıcı ile etkileşim gibi önceki bölümde belirtildiği sinyalleri arama uygulamadan toplanması gerekir. Application Insights bir Genişletilebilir izleme çözümü, esnek izleme seçeneklerini ile birden çok platform için kullanılabilir olur. Application Insights kullanımını veri analizini kolaylaştırmak için Azure Search'tarafından oluşturulan Power BI arama raporlar yararlanmak olanak sağlar.

İçinde [portal](https://portal.azure.com) Azure Search hizmet, arama trafiği Analytics dikey için sayfası bu telemetri deseni izleyen bir kopya sayfası içerir. Ayrıca seçin veya bir Application Insights kaynağı oluşturun ve gerekli verileri tek bir yerde görün.

![Arama trafiği Analytics yönergeleri][1]

### <a name="1-select-an-application-insights-resource"></a>1. Application Insights kaynağını seçin

Kullanın veya zaten yoksa, oluşturmak için Application Insights kaynağını seçmeniz gerekir. Gerekli özel olayları günlüğe kullanımda olduğundan bir kaynak kullanabilirsiniz.

Yeni bir Application Insights kaynak oluştururken, tüm uygulama türleri bu senaryo için geçerlidir. Kullanmakta olduğunuz platform için en uygun olanı seçin.

Uygulamanız için telemetri istemci oluşturmak için izleme anahtarı gerekir. Application Insights portal panosunda alın veya kullanmak istediğiniz örneği seçildiğinde, arama trafiği Analytics sayfasından alabilirsiniz.

### <a name="2-instrument-your-application"></a>2. Uygulamanızın izleme

Burada Application Insights kaynağı Yukarıdaki adımda, oluşturulan kullanan kendi arama uygulamasını izleme bu aşamasıdır. Bu işlem için dört adım vardır:

**I. Bir telemetri istemcisi oluşturma** bu olayları uygulama Öngörüler kaynağa gönderdiği nesnesidir.

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

Diğer diller ve platformlar için tam bkz [listesi](https://docs.microsoft.com/azure/application-insights/app-insights-platforms).

**II. Bağıntı için bir arama kimliği istek** arama istekleri tıklama ile ilişkilendirmek için bu iki ayrı olayları ilişkili bağıntı kimliği sağlamak gereklidir. Azure arama sahip bir üstbilgi istediğinde Kimliği Ara sağlar:

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

**III. Arama olayları günlüğe kaydedin**

Bir arama isteğine bir kullanıcı tarafından verilen her zaman, olarak arama olay Application Insights özel olay aşağıdaki şema ile oturum açmanız:

**ServiceName**: (dize) arama hizmeti adı **SearchId**: arama sorgusunun benzersiz tanıtıcısı (GUID) (arama yanıtta gelir) **IndexName**: olmasını (dize) arama hizmeti dizini Sorgulanan **QueryTerms**: (dize) arama terimleri kullanıcı tarafından girilen **ResultCount**: döndürülmedi belge (int) sayısı (arama yanıtta gelir)  **ScoringProfile**: varsa, kullanılan Puanlama profili adı (dize)

> [!NOTE]
> İstek sayısı = true, arama sorgusu $count ekleyerek oluşturulan kullanıcı sorgularının açık. Daha fazla bilgi [burada](https://docs.microsoft.com/rest/api/searchservice/search-documents#request)
>

> [!NOTE]
> Yalnızca kullanıcılar tarafından oluşturulan arama sorguları oturum unutmayın.
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

**IV. Tıklatın oturum olayları**

Arama analiz amacıyla günlüğe bir sinyal belgesinde, bir kullanıcı, her zaman. Bu olaylar aşağıdaki şema ile oturum için Application Insights özel olaylar kullanın:

**ServiceName**: (dize) arama hizmeti adı **SearchId**: ilgili arama sorgusunun benzersiz tanıtıcısı (GUID) **DocId**: (dize) belge tanımlayıcısı **konumu** : (int) derecesini belgenin arama sonuçları sayfası

> [!NOTE]
> Konum, uygulamanızın Kardinal sırayla ifade eder. Bu her zaman aynı karşılaştırma için izin vermek için olduğu sürece bu numarayı ayarlayabilirler.
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

    appInsights.TrackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

### <a name="3-analyze-with-power-bi-desktop"></a>3. Power BI Desktop ile çözümleme

Uygulamanızı işaretlenir ve uygulamanız için Application Insights doğru bir şekilde bağlandığından doğruladıktan sonra Power BI desktop için Azure Search tarafından oluşturulan önceden tanımlanmış bir şablonu kullanabilirsiniz.
Bu şablon, grafik içerir ve yardımcı tabloları uygunluğu ve arama performansını artırmak için daha fazla bilgiye dayalı kararlar.

Power BI Masaüstü şablonu oluşturmak için üç parça Application Insights hakkında bilgi gerekir. Bu veri kullanmak için kaynak seçtiğinizde arama trafiği Analytics sayfasında bulunabilir.

![Arama trafiği Analytics dikey Application Insights Data][2]

Power BI Masaüstü şablona dahil ölçümleri:

*   Oranı (CTRL aracılığıyla) tıklatın: toplam arama sayısı için belirli bir belge tıklayın kullanıcılar oranı.
*   Tıklama olmadan arar: koşulları en çok yapılan hiçbir tıklama kaydetmek sorgular
*   En belgeleri'ı tıklattınız: Son 24 saat içinde 7 gün ve 30 gün tıklattınız Kimliğine göre belge en.
*   Popüler belge terimi çiftleri: tıklandığında, belgede neden koşulları tıklama tarafından sıralanan.
*   Saat'ı tıklatın: tıklama kümelenmiş zamanından bu yana arama sorgusu tarafından

![Application Insights okuma için Power BI şablonu][3]


## <a name="next-steps"></a>Sonraki Adımlar
Arama hizmetinize ilişkin güçlü ve ayrıntılı veri almak için arama uygulamasını izleme.

Application Insights hakkında daha fazla bilgi bulabilirsiniz [burada](https://go.microsoft.com/fwlink/?linkid=842905). Application Insights ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/application-insights/) kendi farklı hizmet katmanları hakkında daha fazla bilgi edinmek için.

Harika raporları oluşturma hakkında daha fazla bilgi edinin. Bkz: [Power BI Desktop ile çalışmaya başlama](https://powerbi.microsoft.com/en-us/documentation/powerbi-desktop-getting-started/) Ayrıntılar için

<!--Image references-->
[1]: ./media/search-traffic-analytics/AzureSearch-TrafficAnalytics.png
[2]: ./media/search-traffic-analytics/AzureSearch-AppInsightsData.png
[3]: ./media/search-traffic-analytics/AzureSearch-PBITemplate.png
