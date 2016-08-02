<properties
    pageTitle="JavaScript web uygulamaları için Application Insights | Microsoft Azure"
    description="Sayfa görünümü ve oturum sayısını, web istemci verilerini alın ve kullanım desenlerini izleyin. JavaScript web sayfalarında özel durumları ve performans sorunlarını yakalayın."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/18/2016"
    ms.author="awills"/>

# Web sayfaları için Application Insights


[AZURE.INCLUDE [app-insights-selector-get-started-dotnet](../../includes/app-insights-selector-get-started-dotnet.md)]

Web sayfanızın veya uygulamanızın performansı ve kullanımı hakkında bilgi edinin. Sayfa betiğinize Visual Studio Application Insights ekleyin; sayfa yükünün ve AJAX çağrılarının zamanlamasına, tarayıcı özel durumlar ve AJAX hataları sayısına ve ayrıntılarına kadar kullanıcı ve oturum sayılarına da sahip olacaksınız. Bunların tümü sayfaya, istemci işletim sistemi ve tarayıcı sürümüne, coğrafi konuma ve başka boyutlara göre kesimlere ayrılmıştır. Hata sayısı veya yavaş sayfa yüklemesi hakkında da uyarı ayarlayabilirsiniz.

Application Insights’ı tüm web sayfalarıyla kullanabilirsiniz; kısa bir JavaScript eklemeniz yeterlidir. Web hizmetinizin [Java](app-insights-java-get-started.md) veya [ASP.NET](app-insights-asp-net.md) olması halinde, sunucunuzdan ve istemcilerinizden telemetri tümleştirebilirsiniz.

Size bir [Microsoft Azure](https://azure.com) aboneliği gerekecektir. Takımınızın kurumsal bir aboneliği varsa sahibinden Microsoft Hesabınızı eklemesini isteyin. Ücretsiz bir katman olduğundan, geliştirme ve küçük ölçekli kullanımın herhangi bir maliyeti olmayacaktır.


## Web sayfalarınız için Application Insights’ı ayarlama

Buna zaten sahip olabilirsiniz. Uygulamanızı yeni ASP.NET projesiyse ve Visual Studio Yeni proje iletişim kutusuna Application Insights’ı eklemeyi seçtiyseniz, betik dosyası eklenecek ve tüm ayarlarınız yapılacaktır.

Aksi takdirde, web sayfalarınıza bir kod parçacığını aşağıdaki gibi eklemeniz gerekecektir.

### Application Insights kaynağını açma

Sayfanızın performansı ve kullanımı hakkında verilerin görüntülendiği yer Application Insights kaynağıdır. 

[Azure portalda](https://portal.azure.com) oturum açın.

Uygulamanız sunucu tarafı için izlemeyi zaten ayarladıysanız zaten bir kaynağınız vardır:

![Gözat, Geliştirici Hizmetleri, Application Insights’ı seçin.](./media/app-insights-javascript/01-find.png)

Yoksa, bir tane oluşturun:

![Yeni, Geliştirici Hizmetleri, Application Insights’ı seçin.](./media/app-insights-javascript/01-create.png)


*Hala sorularınız mı var?* [Kaynak oluşturma hakkında daha fazla bilgi](app-insights-create-new-resource.md).


### Uygulamanıza veya web sayfalarınıza SDK betiği ekleme

Hızlı Başlangıç’ta web sayfaları için betik alın:

![Uygulamaya genel bakış dikey pencerenizde, Hızlı Başlat, Web sayfalarımı izlemeyi sağlayan kodu al'ı seçin. Betiği kopyalayın.](./media/app-insights-javascript/02-monitor-web-page.png)

Betiği izlemek istediğiniz her sayfanın `<head>` etiketinin hemen önüne ekleyin. Web sayfanızda bir ana sayfa varsa betiği buraya koyabilirsiniz. Örneğin:

* ASP.NET MVC projesinde buraya koyabilirsiniz `View\Shared\_Layout.cshtml`
* SharePoint sitesinde, denetim masasında [Site Ayarları / Ana Sayfa](app-insights-sharepoint.md)’yı açın.

Betikte, verileri Application Insights kaynağınıza yönlendiren izleme anahtarı bulunur. 

([Betiğin daha ayrıntılı açıklaması](http://apmtips.com/blog/2015/03/18/javascript-snippet-explained/))

*(İyi bilinen web sayfası altyapısı kullanıyorsanız, Application Insights bağdaştırıcılarını araştırın. Örneğin, [AngularJS modülü](http://ngmodules.org/modules/angular-appinsights) var.)*


## Ayrıntılı yapılandırma

Çoğunlukla gerekmese de, ayarlayabileceğiniz birkaç [parametre](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) vardır. Örneğin, sayfa görünümü başına rapor edilen Ajax çağrılarını devre dışı bırakabilir ya da sınırlayabilirsiniz (trafiğini azaltmak için); bunun yerine, telemetrinin toplu işlem olmadan işlem hattında hızlı taşınmasını sağlamak için hata ayıklama modu da ayarlayabilirsiniz.

Bu parametreleri ayarlamak için kod parçacığında bu satırı bulun ve şunun arkasına virgülle ayrılmış daha fazla öğe ekleyin:

    })({
      instrumentationKey: "..."
      // Insert here
    });

[Kullanılabilir parametreler](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) şunlardır:

    // Send telemetry immediately without batching.
    // Remember to remove this when no longer required, as it
    // can affect browser performance.
    enableDebug: true,

    // Don't log browser exceptions.
    disableExceptionTracking: true,

    // Don't log ajax calls.
    disableAjaxTracking: boolean,

    // Limit number of Ajax calls logged, to reduce traffic.
    maxAjaxCallsPerView: 10, // default is 500

    // Time page load up to execution of first trackPageView().
    overridePageViewDuration: boolean,

    // Set these dynamically for an authenticated user.
    appUserId: string,
    accountId: string,



## <a name="run"></a>Uygulamanızı çalıştırma

Web uygulamanızı çalıştırın, telemetri oluşturmak için bir süre bunu kullanın ve birkaç saniye bekleyin. Geliştirme makinenizdeki **F5** tuşunu kullanarak bunu çalıştırabilir ya da yayımlayabilir ve kullanıcıların bunu yürütmesine izin verebilirsiniz.

Web uygulamasının Application Insights’a gönderdiği telemetriyi denetlemek istiyorsanız, tarayıcınızın hata ayıklama araçlarını kullanın (birçok tarayıcıda **F12**). Veriler dc.services.visualstudio.com adresine gönderilir.

## Tarayıcı performans verilerinizi araştırma

Kullanıcılarınızın tarayıcılarından toplanan performans verilerini göstermek için Tarayıcılar dikey penceresini açın.

![portal.azure.com adresinde uygulamanızın kaynağını açıp Ayarlar, Tarayıcı’ya tıklama](./media/app-insights-javascript/03.png)


*Henüz veri yok mu? Sayfanın üstündeki **Yenile**'ye tıklayın. Hala hiçbir şey yok mu? Bkz. [Sorun giderme](app-insights-troubleshoot-faq.md).*

Tarayıcılar dikey penceresi, hazır filtrelerin ve grafik seçimlerinin bulunduğu[Ölçüm Gezgini dikey penceresidir](app-insights-metrics-explorer.md). İsterseniz zaman aralığını, filtreleri ve grafik yapılandırmasını düzenleyebilir ve sonucu sık kullanılan olarak kaydedebilirsiniz. Asıl dikey pencere yapılandırmasına dönmek için **Varsayılanları geri yükle**’ye tıklayın.

## Sayfa yükleme performansı

Üst kısım sayfa yükleme sürelerinin bölümlenmiş bir grafiğidir. Grafiğin toplam yüksekliği yüklenecek ortalama süreyi ve kullanıcılarınızın tarayıcılarda uygulamanızdan görüntülenecek sayfaları temsil eder. Süre, düzen ve çalışma betikleri de dahil tüm zaman uyumlu yük etkinlikleri işlenene kadar tarayıcının ilk HTTP isteğini gönderdiği zamandan ölçülür. AJAX çağrılarından web bölümleri yükleme gibi zaman uyumsuz görevleri içermez.

Grafik, toplam sayfa yükleme süresini [W3C tarafından tanımlanan standart zamanlamalara](http://www.w3.org/TR/navigation-timing/#processing-model) böler. 

![](./media/app-insights-javascript/08-client-split.png)

*Ağa bağlanma* süresinin çoğunlukla beklediğinizden kısa olduğunu unutmayın; bunun nedeni, tarayıcıdan sunucuya yapılan tüm isteklerin ortalaması olmasıdır. Tek tek isteklerin çoğunun bağlantı süresi 0 değerindedir; çünkü sunucuya zaten etkin bir bağlantı vardır.

### Yavaş mı yükleniyor?

Yavaş sayfa yüklenmesi kullanım memnuniyetsizliğinin başlıca kaynaklarından biridir. Grafik yavaş sayfa yüklemeleri gösteriyorsa, bazı tanılama araştırmalarını yapmak kolaydır.

Grafik, uygulamanızdaki tüm sayfa yüklerinin ortalamasını gösterir. Sorunun belirli sayfalara sınırlı olup olmadığını görmek için, sayfa URL'siyle bölümlenmiş kılavuzun bulunduğu dikey pencereyi daha fazla inceleyin:

![](./media/app-insights-javascript/09-page-perf.png)

Sayfa görünümü sayısına ve standart sapmaya dikkat edin. Sayfa sayısı çok azsa, sorun kullanıcıları çok etkilemez. Yüksek bir standart sapma (ortalamayla karşılaştırılabilir) tek tek ölçüler arasında çok sayıda farklılık gösterir.

**Tek URL’yi ve tek sayfa görünümünü yakınlaştırma.** Tam da bu URL için filtre uygulanmış tarayıcı grafiklerinin dikey penceresini görüntülemek için sayfa adlarından istediğinize tıklayın; bundan sonra da bir sayfa görünümü örneğine tıklayın.

![](./media/app-insights-javascript/35.png)

Bu etkinlikle ilgili özelliklerin tam listesi için `...` seçeneğine tıklayın veya Ajax çağrılarını ve bağlantılı etkinlikleri inceleyin. Zaman uyumluysalar, Yavaş Ajax çağrıları genel sayfa yükleme süresini etkiler. İlgili etkinlikler aynı URL için sunucu isteklerini ekler (web sunucunuza Application Insights kurduysanız).


**Zaman içerisinde performans sayfası.** Tarayıcılar dikey penceresine dönün, Sayfa Görünümü Yükleme Süresi kılavuzunu çizgi grafiği olarak değiştirerek belirli zamanlarda yükselme olup olmadığına bakın:

![Kılavuzun başlığına tıklayın ve yeni bir grafik türü seçin](./media/app-insights-javascript/10-page-perf-area.png)

**Diğer boyutlara göre bölme.** Sayfalarınızın yavaş yüklenme nedeni belirli bir tarayıcı, istemci işletim sistemi veya kullanıcı konumu olabilir mi? Yeni bir grafik ekleyin ve denemenizi **Gruplandır** boyutuyla yapın.

![](./media/app-insights-javascript/21.png)


## AJAX Performansı

Web sayfalarınızda AJAX çağrılarının iyi iş çıkardığından emin olun. Bunlar çoğunlukla zaman uyumsuz olarak sayfanızı bölümlerini doldurmak için kullanılır. Genel sayfa hemen yüklenebilse de, kullanıcılarınızın boş web bölümlerine bakıp burada veri görünmesini bekleyerek hayal kırıklığına uğrayabilirler.

Web sayfasından oluşturulan AJAX çağrıları Tarayıcılar dikey penceresinde bağımlılıklar olarak gösterilir.

Dikey pencerenin üst kısmında özet grafikleri vardır:

![](./media/app-insights-javascript/31.png)

daha aşağıda da ayrıntılı kılavuzlar:

![](./media/app-insights-javascript/33.png)

Belirli ayrıntılar için herhangi bir satıra tıklayın.


> [AZURE.NOTE] Dikey pencerede Tarayıcılar filtresini silerseniz, bu grafiklere hem sunucu hem de AJAX bağımlılıkları eklenir. Filtreyi yeniden yapılandırmak için Varsayılanları Geri Yükle'ye tıklayın.

**Başarısız Ajax çağrılarını incelemek için** Bağımlılık hataları kılavuzuna gidin ve belirli örnekleri görmek için bir satıra tıklayın.

![](./media/app-insights-javascript/37.png)

Ajax çağrısıyla ilgili tam telemetri için `...` seçeneğine tıklayın.

### Hiç Ajax çağrısı bildirilmedi mi?

AJAX çağrılarında web sayfanızın betiğinden oluşturulan HTTP çağrıları vardır. Bunların bildirildiğini görmüyorsanız, kod parçacığının `disableAjaxTracking` öğesini veya `maxAjaxCallsPerView` [parametrelerini](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) ayarlayıp ayarlamadığını denetleyin.

## Tarayıcı özel durumları

Tarayıcılar dikey penceresinde bir özel durum özet grafiği, dikey pencerenin daha aşağısındaysa özel durum türleri kılavuzu bulunur.

![](./media/app-insights-javascript/39.png)

Tarayıcı özel durumlarının bildirildiğini görmüyorsanız, kod parçacığının `disableExceptionTracking`[parametresini](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md#config) ayarlayıp ayarlamadığını denetleyin.

## Tek tek sayfa görünümü etkinliklerini inceleme

Sayfa görünümü telemetrisi çoğunlukla Application Insights tarafından analiz edilir; yalnızca birikmeli raporları, tüm kullanıcıların ortalamasını görürsünüz. Ancak, hata ayıklama amacıyla tek tek sayfa görünümü etkinliklerine de bakabilirsiniz.

Tanılama Ara dikey penceresinde Filtreler’i Sayfa Görünümü olarak ayarlayın.

![](./media/app-insights-javascript/12-search-pages.png)

Daha fazla ayrıntı için herhangi bir olayı seçin. Daha da fazla ayrıntı görmek için, ayrıntılar sayfasında "..." öğesine tıklayın.

> [AZURE.NOTE] [Ara](app-insights-diagnostic-search.md) seçeneğini kullanırsanız, tam sözcükleri eşleştirmeye özen gösterin: "Abou" ve "bout" girişleri "About" sözcüğüyle eşleşmezken, "Abou* " eşleşir. Bunun yanı sıra, bir terimi ararken sözcük joker karakterle başlayamaz. Örneğin, "*bou" araması "About" sözcüğüyle eşleşmez.

> [Tanılama araması hakkında daha fazla bilgi edinin](app-insights-diagnostic-search.md)

### Sayfa görünümü özellikleri

* **Sayfa görünümü süresi** 

 * Varsayılan olarak, istemcinin tam yükleme isteğine ait sayfa yükleme süresi (yardımcı dosyalar da dahildir, ancak Ajax çağırıları gibi zaman uyumsuz görevler hariç tutulur). 
 * `overridePageViewDuration` seçeneğini [sayfa yapılandırması](#detailed-configuration) içinde ayarlarsanız, ilk `trackPageView` yürütmesi için istemci isteğinin zaman aralığıdır. betiğin başlatılmasından sonra trackPageView öğesini normal konumundan taşırsanız farklı bir değer yansıtır.
 * `overridePageViewDuration` ayarlanmış ve süre bağımsız değişkeni `trackPageView()` çağrısında verilmişse, bunu yerine bağımsız değişken değeri kullanılır. 


## Özel sayfa sayıları

Varsayılan olarak, istemci tarayıcısına yeni bir sayfanın her yüklenişinde bir sayfa sayısı oluşur.  Ancak, ek sayfa görünümlerini de saymak isteyebilirsiniz. Örneğin, sayfa içeriğini sekmelerde görüntüleyebilir; sizse kullanıcı sekmeler arasında geçiş yaptığında sayfayı saymak istersiniz. Sayfadaki JavaScript kodu tarayıcının URL’sini değiştirmeden yeni içerik yükleyebilir.

İstemci kodunun uygun bir noktasına buradaki gibi JavaScript çağrısı ekleyin:

    appInsights.trackPageView(myPageName);

Sayfa adında, URL’deki aynı karakterler olabilir, ancak "#" veya "?" karakterinden sonraki her şey göz ardı edilir.



## Kullanımı izleme


Uygulamanızla kullanıcılarınızın neler yaptığını bilmek ister misiniz?

* [Kullanımı izleme hakkında bilgi edinin](app-insights-web-track-usage.md)
* [Özel etkinlikler ve ölçüm API’si hakkında bilgi edinin](app-insights-api-custom-events-metrics.md).


#### <a name="video"></a> Video: Kullanımı İzleme

> [AZURE.VIDEO tracking-usage-with-application-insights]

## <a name="next"></a> Sonraki adımlar

* [Kullanımı İzleme](app-insights-web-track-usage.md)
* [Özel etkinlikler ve ölçümler](app-insights-api-custom-events-metrics.md)
* [Build-measure-learn](app-insights-overview-usage.md)





<!--HONumber=Jun16_HO2-->


