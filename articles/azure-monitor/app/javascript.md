---
title: JavaScript web uygulamaları için Azure Application Insights | Microsoft Docs
description: Sayfa görünümü ve oturum sayısını, web istemci verilerini alın ve kullanım desenlerini izleyin. JavaScript web sayfalarında özel durumları ve performans sorunlarını yakalayın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b710d09-6ab4-4004-b26a-4fa840039500
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: fee172eccd79fd28e281b2beece9702630ac39b5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901733"
---
# <a name="application-insights-for-web-pages"></a>Web sayfaları için Application Insights
Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa betiğinize [Application Insights](app-insights-overview.md)’ı ekleyerek, sayfa yüklemelerinin ve AJAX çağrılarının zamanlamalarının yanı sıra, tarayıcı özel durumları ile AJAX hatalarının sayılarını ve ayrıntılarını, ayrıca kullanıcı ve oturum sayılarını elde edebilirsiniz. Bunların tümü sayfaya, istemci işletim sistemi ve tarayıcı sürümüne, coğrafi konuma ve başka boyutlara göre kesimlere ayrılmıştır. Hata sayısı veya yavaş sayfa yüklemesi hakkında uyarı ayarlayabilirsiniz. Ayrıca JavaScript kodunuza izleme çağrıları ekleyerek web sayfası uygulamanızın farklı özelliklerinin nasıl kullanıldığını izleyebilirsiniz.

Application Insights tüm web sayfalarıyla kullanılabilir; kısa bir JavaScript eklemeniz yeterlidir. Web hizmetinizin [Java](java-get-started.md) veya [ASP.NET](asp-net.md) olması halinde, sunucunuzdan ve istemcilerinizden telemetri tümleştirebilirsiniz.

![portal.azure.com adresinde uygulamanızın kaynağını açıp Tarayıcı’ya tıklayın](media/javascript/03.png)

Bir [Microsoft Azure](https://azure.com) aboneliğine ihtiyacınız olacaktır. Takımınızın kurumsal bir aboneliği varsa sahibinden Microsoft Hesabınızı eklemesini isteyin.

## <a name="set-up-application-insights-for-your-web-page"></a>Web sayfalarınız için Application Insights’ı ayarlama
Yükleyici kod parçacığını web sayfalarınıza aşağıdaki şekilde ekleyin.

### <a name="open-or-create-application-insights-resource"></a>Application Insights kaynağı açma veya oluşturma
Sayfanızın performansı ve kullanımı hakkında verilerin görüntülendiği yer Application Insights kaynağıdır. 

[Azure portalda](https://portal.azure.com) oturum açın.

Uygulamanız sunucu tarafı için izlemeyi zaten ayarladıysanız zaten bir kaynağınız vardır:

![Gözat, Geliştirici Hizmetleri, Application Insights’ı seçin.](media/javascript/01-find.png)

Yoksa, bir tane oluşturun:

![Yeni, Geliştirici Hizmetleri, Application Insights’ı seçin.](media/javascript/01-create.png)

*Hala sorularınız mı var?* [Kaynak oluşturma hakkında daha fazla bilgi](create-new-resource.md ).

### <a name="add-the-sdk-script-to-your-app-or-web-pages"></a>Uygulamanıza veya web sayfalarınıza SDK betiği ekleme

```HTML
<!-- 
To collect user behavior analytics about your application, 
insert the following script into each page you want to track.
Place this code immediately before the closing </head> tag,
and before any other scripts. Your first data will appear 
automatically in just a few seconds.
-->
<script type="text/javascript">
var appInsights=window.appInsights||function(a){
  function b(a){c[a]=function(){var b=arguments;c.queue.push(function(){c[a].apply(c,b)})}}var c={config:a},d=document,e=window;setTimeout(function(){var b=d.createElement("script");b.src=a.url||"https://az416426.vo.msecnd.net/scripts/a/ai.0.js",d.getElementsByTagName("script")[0].parentNode.appendChild(b)});try{c.cookie=d.cookie}catch(a){}c.queue=[];for(var f=["Event","Exception","Metric","PageView","Trace","Dependency"];f.length;)b("track"+f.pop());if(b("setAuthenticatedUserContext"),b("clearAuthenticatedUserContext"),b("startTrackEvent"),b("stopTrackEvent"),b("startTrackPage"),b("stopTrackPage"),b("flush"),!a.disableExceptionTracking){f="onerror",b("_"+f);var g=e[f];e[f]=function(a,b,d,e,h){var i=g&&g(a,b,d,e,h);return!0!==i&&c["_"+f](a,b,d,e,h),i}}return c
  }({
      instrumentationKey:"<your instrumentation key>"
  });
  
window.appInsights=appInsights,appInsights.queue&&0===appInsights.queue.length&&appInsights.trackPageView();
</script>
```

Betiği, izlemek istediğiniz her sayfanın `</head>` etiketinin hemen önüne ekleyin. Web sayfanızda bir ana sayfa varsa betiği buraya koyabilirsiniz. Örneğin:

* ASP.NET MVC projesinde `View\Shared\_Layout.cshtml` içine koyabilirsiniz
* SharePoint sitesinde, denetim masasında [Site Ayarları / Ana Sayfa](sharepoint.md)’yı açın.

Betikte, verileri Application Insights kaynağınıza yönlendiren izleme anahtarı bulunur. 

([Betiğin daha ayrıntılı açıklaması](https://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

## <a name="detailed-configuration"></a>Ayrıntılı yapılandırma
Çoğunlukla gerekmese de, ayarlayabileceğiniz birkaç [parametre](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) vardır. Örneğin, sayfa başına görünümde bildirilen Ajax çağrılarını devre dışı bırakabilir veya çağrıların sayısını sınırlayabilirsiniz. Alternatif olarak, hata ayıklama modunu; telemetriyi toplu hale getirilmeden, ardışık düzende taşıyacak şekilde ayarlayabilirsiniz.

Bu parametreleri ayarlamak için kod parçacığında bu satırı bulun ve şunun arkasına virgülle ayrılmış daha fazla öğe ekleyin:

    })({
      instrumentationKey: "..."
      // Insert here
    });

[Kullanılabilir parametreler](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) şunlardır:

    // Send telemetry immediately without batching.
    // Remember to remove this when no longer required, as it
    // can affect browser performance.
    enableDebug: boolean,

    // Don't log browser exceptions.
    disableExceptionTracking: boolean,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, to reduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up to execution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set dynamically for an authenticated user.
    accountId: string,

## <a name="run"></a>Uygulamanızı çalıştırma
Web uygulamanızı çalıştırın, telemetri oluşturmak için bir süre bunu kullanın ve birkaç saniye bekleyin. Geliştirme makinenizdeki **F5** tuşunu kullanarak bunu çalıştırabilir ya da yayımlayabilir ve kullanıcıların bunu yürütmesine izin verebilirsiniz.

Web uygulamasının Application Insights’a gönderdiği telemetriyi denetlemek istiyorsanız, tarayıcınızın hata ayıklama araçlarını kullanın (birçok tarayıcıda **F12**). Veriler dc.services.visualstudio.com adresine gönderilir.

## <a name="explore-your-browser-performance-data"></a>Tarayıcı performans verilerinizi araştırma
Kullanıcılarınızın tarayıcılarından toplanan performans verilerini göstermek için Tarayıcı dikey penceresini açın.

![portal.azure.com adresinde uygulamanızın kaynağını açıp Ayarlar, Tarayıcı’ya tıklama](./media/javascript/03.png)

Henüz veri yok? Sayfanın üstündeki **Yenile**'ye tıklayın. Hala hiçbir şey yok mu? Bkz. [Sorun giderme](troubleshoot-faq.md).

Tarayıcı dikey penceresi, hazır filtrelerin ve grafik seçimlerinin bulunduğu [Ölçüm Gezgini dikey penceresidir](metrics-explorer.md). İsterseniz zaman aralığını, filtreleri ve grafik yapılandırmasını düzenleyebilir ve sonucu sık kullanılan olarak kaydedebilirsiniz. Asıl dikey pencere yapılandırmasına dönmek için **Varsayılanları geri yükle**’ye tıklayın.

## <a name="page-load-performance"></a>Sayfa yükleme performansı
Üst kısım sayfa yükleme sürelerinin bölümlenmiş bir grafiğidir. Grafiğin toplam yüksekliği yüklenecek ortalama süreyi ve kullanıcılarınızın tarayıcılarda uygulamanızdan görüntülenecek sayfaları temsil eder. Süre, düzen ve çalışma betikleri de dahil tüm zaman uyumlu yük etkinlikleri işlenene kadar tarayıcının ilk HTTP isteğini gönderdiği zamandan ölçülür. AJAX çağrılarından web bölümleri yükleme gibi zaman uyumsuz görevleri içermez.

Grafik, toplam sayfa yükleme süresini [W3C tarafından tanımlanan standart zamanlamalara](https://www.w3.org/TR/navigation-timing/#processing-model) böler. 

![](./media/javascript/08-client-split.png)

*Ağa bağlanma* süresinin çoğunlukla beklediğinizden kısa olduğunu unutmayın; bunun nedeni, tarayıcıdan sunucuya yapılan tüm isteklerin ortalaması olmasıdır. Tek tek isteklerin çoğunun bağlantı süresi 0 değerindedir; çünkü sunucuya zaten etkin bir bağlantı vardır.

### <a name="slow-loading"></a>Yavaş mı yükleniyor?
Yavaş sayfa yüklenmesi kullanım memnuniyetsizliğinin başlıca kaynaklarından biridir. Grafik yavaş sayfa yüklemeleri gösteriyorsa, bazı tanılama araştırmalarını yapmak kolaydır.

Grafik, uygulamanızdaki tüm sayfa yüklerinin ortalamasını gösterir. Sorunun belirli sayfalara sınırlı olup olmadığını görmek için, sayfa URL'siyle bölümlenmiş kılavuzun bulunduğu dikey pencereyi daha fazla inceleyin:

![](./media/javascript/09-page-perf.png)

Sayfa görünümü sayısına ve standart sapmaya dikkat edin. Sayfa sayısı çok azsa, sorun kullanıcıları çok etkilemez. Yüksek bir standart sapma (ortalamayla karşılaştırılabilir) tek tek ölçüler arasında çok sayıda farklılık gösterir.

**Tek URL’yi ve tek sayfa görünümünü yakınlaştırma.** Tam da bu URL için filtre uygulanmış tarayıcı grafiklerinin dikey penceresini görüntülemek için sayfa adlarından istediğinize tıklayın; bundan sonra da bir sayfa görünümü örneğine tıklayın.

![](./media/javascript/35.png)

Bu etkinlikle ilgili özelliklerin tam listesi için `...` seçeneğine tıklayın veya Ajax çağrılarını ve bağlantılı etkinlikleri inceleyin. Zaman uyumluysalar, Yavaş Ajax çağrıları genel sayfa yükleme süresini etkiler. İlgili etkinlikler aynı URL için sunucu isteklerini ekler (web sunucunuza Application Insights kurduysanız).

**Zaman içerisinde performans sayfası.** Tarayıcılar dikey penceresine dönün, Sayfa Görünümü Yükleme Süresi kılavuzunu çizgi grafiği olarak değiştirerek belirli zamanlarda yükselme olup olmadığına bakın:

![Kılavuzun başlığına tıklayın ve yeni bir grafik türü seçin](./media/javascript/10-page-perf-area.png)

**Diğer boyutlara göre bölme.** Sayfalarınızın yavaş yüklenme nedeni belirli bir tarayıcı, istemci işletim sistemi veya kullanıcı konumu olabilir mi? Yeni bir grafik ekleyin ve denemenizi **Gruplandır** boyutuyla yapın.

![](./media/javascript/21.png)

## <a name="ajax-performance"></a>AJAX Performansı
Web sayfalarınızda AJAX çağrılarının iyi iş çıkardığından emin olun. Bunlar çoğunlukla zaman uyumsuz olarak sayfanızı bölümlerini doldurmak için kullanılır. Genel sayfa hemen yüklenebilse de, kullanıcılarınızın boş web bölümlerine bakıp burada veri görünmesini bekleyerek hayal kırıklığına uğrayabilirler.

Web sayfasından oluşturulan AJAX çağrıları Tarayıcılar dikey penceresinde bağımlılıklar olarak gösterilir.

Dikey pencerenin üst kısmında özet grafikleri vardır:

![](./media/javascript/31.png)

daha aşağıda da ayrıntılı kılavuzlar:

![](./media/javascript/33.png)

Belirli ayrıntılar için herhangi bir satıra tıklayın.

> [!NOTE]
> Dikey pencerede Tarayıcılar filtresini silerseniz, bu grafiklere hem sunucu hem de AJAX bağımlılıkları eklenir. Filtreyi yeniden yapılandırmak için Varsayılanları Geri Yükle'ye tıklayın.
> 
> 

**Başarısız Ajax çağrılarını incelemek için** Bağımlılık hataları kılavuzuna gidin ve belirli örnekleri görmek için bir satıra tıklayın.

![](./media/javascript/37.png)

Ajax çağrısıyla ilgili tam telemetri için `...` seçeneğine tıklayın.

### <a name="no-ajax-calls-reported"></a>Hiç Ajax çağrısı bildirilmedi mi?
Ajax çağrılarında web sayfanızın betiğinden oluşturulan HTTP/HTTPS çağrıları vardır. Bunların bildirildiğini görmüyorsanız, kod parçacığının `disableAjaxTracking` öğesini veya `maxAjaxCallsPerView` [parametrelerini](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) ayarlayıp ayarlamadığını denetleyin.

## <a name="browser-exceptions"></a>Tarayıcı özel durumları
Tarayıcılar dikey penceresinde bir özel durum özet grafiği, dikey pencerenin daha aşağısındaysa özel durum türleri kılavuzu bulunur.

![](./media/javascript/39.png)

Tarayıcı özel durumlarının bildirildiğini görmüyorsanız, kod parçacığının `disableExceptionTracking`[parametresini](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) ayarlayıp ayarlamadığını denetleyin.

## <a name="inspect-individual-page-view-events"></a>Tek tek sayfa görünümü etkinliklerini inceleme

Sayfa görünümü telemetrisi çoğunlukla Application Insights tarafından analiz edilir; yalnızca birikmeli raporları, tüm kullanıcıların ortalamasını görürsünüz. Ancak, hata ayıklama amacıyla tek tek sayfa görünümü etkinliklerine de bakabilirsiniz.

Tanılama Ara dikey penceresinde Filtreler’i Sayfa Görünümü olarak ayarlayın.

![](./media/javascript/12-search-pages.png)

Daha fazla ayrıntı için herhangi bir olayı seçin. Daha da fazla ayrıntı görmek için, ayrıntılar sayfasında "..." öğesine tıklayın.

> [!NOTE]
> Kullanırsanız [arama](diagnostic-search.md), tam sözcükleri eşleştirmeye olduğunu fark edeceksiniz: "Abou" ve "bout" "Hakkında" eşleşmiyor.
> 
> 

Ayrıca sayfa görünümlerini aramak için güçlü [Log Analytics dilinden](https://docs.microsoft.com/azure/application-insights/app-insights-analytics-tour) de yararlanabilirsiniz.

### <a name="page-view-properties"></a>Sayfa görünümü özellikleri
* **Sayfa görünümü süresi** 
  
  * Varsayılan olarak, istemcinin tam yükleme isteğine ait sayfa yükleme süresi (yardımcı dosyalar da dahildir, ancak Ajax çağırıları gibi zaman uyumsuz görevler hariç tutulur). 
  * `overridePageViewDuration` seçeneğini [sayfa yapılandırması](#detailed-configuration) içinde ayarlarsanız, ilk `trackPageView` yürütmesi için istemci isteğinin zaman aralığıdır. betiğin başlatılmasından sonra trackPageView öğesini normal konumundan taşırsanız farklı bir değer yansıtır.
  * `overridePageViewDuration` ayarlanmış ve süre bağımsız değişkeni `trackPageView()` çağrısında verilmişse, bunun yerine bağımsız değişken değeri kullanılır. 

## <a name="custom-page-counts"></a>Özel sayfa sayıları
Varsayılan olarak, istemci tarayıcısına yeni bir sayfanın her yüklenişinde bir sayfa sayısı oluşur.  Ancak, ek sayfa görünümlerini de saymak isteyebilirsiniz. Örneğin, sayfa içeriğini sekmelerde görüntüleyebilir; sizse kullanıcı sekmeler arasında geçiş yaptığında sayfayı saymak istersiniz. Sayfadaki JavaScript kodu tarayıcının URL’sini değiştirmeden yeni içerik yükleyebilir.

İstemci kodunun uygun bir noktasına buradaki gibi JavaScript çağrısı ekleyin:

    appInsights.trackPageView(myPageName);

Sayfa adında,URL’deki karakterlerin aynısı bulunabilir, ancak "#" veya "?" karakterinden sonraki her şey göz ardı edilir.

## <a name="usage-tracking"></a>Kullanımı izleme
Uygulamanızla kullanıcılarınızın neler yaptığını bilmek ister misiniz?

* [Kullanıcı davranış analizi araçları hakkında bilgi edinin](usage-overview.md)
* [Özel etkinlikler ve ölçüm API’si hakkında bilgi edinin](api-custom-events-metrics.md).

## <a name="video"></a> Video


> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]



## <a name="next"></a> Sonraki adımlar
* [Kullanımı izleme](usage-overview.md)
* [Özel etkinlikler ve ölçümler](api-custom-events-metrics.md)
* [Build-measure-learn](usage-overview.md)

