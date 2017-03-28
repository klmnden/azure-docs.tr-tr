---
title: "Azure Application Insights ile ASP.NET Web uygulaması analizi ayarlama | Microsoft Belgeleri"
description: "Şirket içi veya Azure’de barındırılan ASP.NET web siteniz için performans, kullanılabilirlik ve kullanım analizi yapılandırın."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/14/2017
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: ae869be6ed9f304629498f416ffdda96252bdf9c
ms.lasthandoff: 03/16/2017


---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>ASP.NET web siteniz için Application Insights'ı ayarlama
[Azure Application Insights](app-insights-overview.md), [performans sorunlarını ve özel durumlarını algılamanıza ve tanılamanıza](app-insights-detect-triage-diagnose.md) yardımcı olmak için canlı uygulamanızı izler. Ayrca, [uygulamanızın nasıl kullanıldığını keşfetmenize](app-insights-overview-usage.md) de yardımcı olur. Azure App Service’in Web Apps özelliğinin yanı sıra, kendi şirket içi IIS sunucularınızda ya da bulut sanal makinelerinizde barındırılan uygulamalar için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* Visual Studio 2013 güncelleştirme 3 veya sonraki bir sürümü. Ne kadar yeniyse o kadar iyidir.
* Bir [Microsoft Azure](http://azure.com) aboneliği. Takımınızın veya kuruluşunuzun Azure aboneliği varsa, abonelik sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi aboneliğe ekleyebilir.

İlginizi çekiyorsa inceleyebileceğiniz alternatif konu başlıkları da mevcuttur:

* [Çalışma zamanında bir web uygulamasını izleme](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Adım: Application Insights SDK’yı ekleme

Çözüm Gezgini'nde web uygulaması projenize sağ tıklayın ve **Ekle**, **Application Insights Telemetrisi...**'ni veya **Application Insights'ı Yapılandır**'ı seçin.

![Application Insights Telemetrisi Ekle seçeneğinin vurgulandığı Çözüm Gezgini’nin ekran görüntüsü](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(Visual Studio 2015'te Yeni Proje iletişim kutusunda da Application Insights ekleme seçeneği mevcuttur.)

Application Insights yapılandırma sayfasına gidin:

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/app-insights-asp-net/visual-studio-register-dialog.png)

1. Azure'a erişmek için kullandığınız hesabı ve aboneliği seçin.
2. Uygulamanızdan gelen verileri görmek istediğiniz Azure kaynağını seçin. Genellikle her uygulama için ayrı bir kaynak oluşturmanız gerekir. Verilerin depolandığı kaynak grubunu veya konumu ayarlamak isterseniz **Ayarları yapılandır**'a tıklayın. Kaynak grupları, verilere erişimi denetlemek için kullanılır. Örneğin aynı sistemin parçalarını oluşturan birden uygulamanız varsa bunların Application Insights verilerini aynı kaynak grubuna ekleyebilirsiniz.
3. Application Insights, belirli bir telemetri hacmine kadar ücretsizdir. Ücretlendirmeden kaçınmak için bu seviyede bir sınır belirleyebilirsiniz. Kaynak oluşturulduktan sonra portalda **Özellikler + fiyatlandırma**, **Veri hacmi yönetimi**, **Günlük hacim sınırı** sayfasından seçiminizi değiştirebilirsiniz.
4. Devam etmek ve web uygulamanızda Application Insights'ı yapılandırmak için **Kaydol**'a tıklayın. Telemetri hem hata ayıklama sırasında hem de uygulamanızı yayımladıktan sonra [Azure portalına](https://portal.azure.com) gönderilir.
5. Alternatif olarak uygulamanıza yalnızca Application Insights SDK'sını ekleyebilirsiniz. Bu durumda hata ayıklama sırasında telemetri verilerini Visual Studio'da görebilirsiniz. Daha sonra bu yapılandırma sayfasına dönebilir veya uygulamanızı dağıtana kadar bekleyip [telemetriyi çalışma zamanında açabilirsiniz](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> 2. Adım: Uygulamanızı çalıştırma
F5 tuşuna basarak uygulamanızı çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da, günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’nun ekran görüntüsü. Hata ayıklama sırasında Application Insights düğmesi görünür.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry-in-visual-studio-or-application-insights"></a>3. Adım: Visual Studio’da veya Application Insights’ta telemetrinizi görme
Visual Studio’da veya Application Insights web portalında telemetrinizi görebilirsiniz.

**Visual Studio’da** Application Insights penceresini açın. **Application Insights** düğmesine tıklayın veya Çözüm Gezgini’nden projenize sağ tıklayıp **Application Insights**’ı seçin ve ardından **Canlı Telemetride Ara**’ya tıklayın.

Visual Studio Application Insights Arama penceresinde, uygulamanızın sunucu tarafında oluşturulan telemetri için **Hata Ayıklama oturumundan alınan veriler** görünümüne bakın. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

![Application Insights penceresindeki Hata ayıklama oturumundan alınan veriler görünümünün ekran görüntüsü.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Herhangi bir veri gösterilmiyorsa zaman aralığının doğru olduğundan emin olup Ara simgesine tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="the-application-insights-web-portal"></a>Application Insights web portalı
Yalnızca SDK’yı yüklemeyi seçmediyseniz telemetriyi **Application Insights web portalında** da görüntüleyebilirsiniz. Portalda Visual Studio’dakinden daha çok grafik, analiz aracı ve pano bulunur.

Application Insights kaynağınızı açın. [Azure portalında](https://portal.azure.com/) oturum açıp portalda bulun ya da Visual Studio’da projeye sağ tıklayıp sizi yönlendirmesini sağlayın.

![Visual Studio’da Application Insights portalının nasıl açılacağını gösteren ekran görüntüsü](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Erişim hatası alırsanız birden çok Microsoft kimlik bilgileri kümeniz olabilir ve yanlış kümeyle oturum açmış olabilirsiniz. Portalda oturumunuzu kapatıp yeniden oturum açın.

Portal, uygulamanızdan alınan telemetri görünümünde açılır.
![Application Insights’a genel bakış sayfasının ekran görüntüsü](./media/app-insights-asp-net/66.png)

Daha fazla ayrıntı görmek istediğiniz kutucuğa veya grafiğe tıklayın.

### <a name="more-details-in-the-application-insights-web-portal"></a>Application Insights web portalında daha fazla ayrıntı sunulur
İşte portalın sizin için nasıl daha fazla ayrıntı sağladığıyla ilgili birkaç örnek.

* [**Canlı Ölçüm Akışı**](app-insights-live-stream.md) neredeyse anlık telemetriyi görüntüler.

    ![Portalın ekran görüntüsü. Genel Bakış dikey penceresinden Canlı Akış’a tıklayın.](./media/app-insights-asp-net/livestream.png)

    Uygulamanız çalıştığı sırada Canlı Ölçüm Akışı’nı açın ve bunların bağlanmasına izin verin.

    Canlı Ölçüm Akışı, yalnızca gönderildiği andan sonraki bir dakikaya ait telemetriyi gösterir. Daha eski verileri araştırmak için Search, Ölçüm Gezgini ve Analiz özelliklerini kullanın. Verilerin bu sayfalarda görünmesi birkaç dakika sürebilir.

* [**Search**](app-insights-diagnostic-search.md), istekler, özel durumlar ve sayfa görüntüleme sayısı gibi tek tek olayları gösterir. Olay türü, terim eşleşmesi ve özellik değerlerine göre filtreleyebilirsiniz. Olaylara tıklayarak özelliklerini ve ilgili olayları görebilirsiniz.

    ![Portalın ekran görüntüsü. Genel Bakış dikey penceresinden Ara’ya tıklayın.](./media/app-insights-asp-net/search.png)

 * Geliştirme modunda, çok sayıda bağımlılık (AJAX) olayı görebilirsiniz. Bunlar tarayıcı ve sunucu öykünücüsü arasındaki eşitleme işlemleridir. Bunları gizlemek için **Bağımlılık** filtresine tıklayın.
* Grafiklerde istek ve hata oranları gibi [**toplu ölçümler**](app-insights-metrics-explorer.md) görüntülenir. Daha ayrıntılı bilginin bulunduğu dikey pencereyi açmak için herhangi bir grafiğe tıklayın. Herhangi bir grafiğin filtrelerini ve boyutunu ayarlamak için **Düzenle** etiketine tıklayın.

    ![Portalda sağlanan toplu ölçümler dikey penceresinin ekran görüntüsü](./media/app-insights-asp-net/metrics.png)

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>4. Adım: Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetriniz Application Insights portalında biriktirilir ve burada ölçümlerinizi izleyebilir, telemetrinizde arama yapabilir ve [panolar](app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, güçlü [Analiz sorgu dili](app-insights-analytics.md)’ni kullanarak kullanımı ve performansı analiz edebilir ya da belirli olayları bulabilirsiniz.

Telemetrinizi tanılama araması ve [eğilimler](app-insights-visual-studio-trends.md) gibi araçlarla [Visual Studio](app-insights-visual-studio.md)’da analiz etmeye de devam edebilirsiniz.

> [!NOTE]
> Uygulamanız [azaltma sınırlarına](app-insights-pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](app-insights-sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.
>
>

## <a name="land"></a> Application Insights Ekle komutu ne işe yarar?
Application Insights, uygulamanızdan alınan telemetriyi Azure’da barındırılan Application Insights portalına gönderir.

![Telemetri hareketlerinin diyagramı](./media/app-insights-asp-net/01-scheme.png)

Yani bu komut üç işe yarar:

1. Projenize Application Insights Web SDK’sı NuGet paketini ekler. Paketi Visual Studio’da görmek için projenize sağ tıklayıp **NuGet Paketlerini Yönet**’i seçin.
2. [Azure portalında](https://portal.azure.com/) bir Application Insights kaynağı oluşturur. Verilerinizi burada görürsünüz. Kaynağı tanımlayan *izleme anahtarını* alır.
3. İzleme anahtarını `ApplicationInsights.config` öğesine ekler, böylece SDK portala telemetri gönderebilir.

İsterseniz bu adımları [ASP.NET 4](app-insights-windows-services.md) veya [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started) için el ile uygulayabilirsiniz.

### <a name="upgrade-to-future-sdk-versions"></a>Gelecek SDK sürümlerine yükseltme
[SDK’nın yeni bir sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltmek için **NuGet paket yöneticisini** yeniden açıp yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.Web**’i seçip **Yükselt**’i seçin.

ApplicationInsights.config’de herhangi bir özelleştirme gerçekleştirdiyseniz yükseltmeden önce dosyanın bir kopyasını kaydedin. Daha sonra, yaptığınız değişiklikleri yeni sürümle birleştirin.

## <a name="add-more-telemetry"></a>Daha fazla telemetri ekleme
Ekleyebileceğiniz diğer telemetri türleri aşağıda verilmiştir.
### <a name="dependencies-exceptions-and-performance-counters"></a>Bağımlılıklar, özel durumlar ve performans sayaçları

Web uygulamalarınız hakkında ek telemetri edinmek için her IIS sunucusu makinesine [durum izleyicisini yükleyin](http://go.microsoft.com/fwlink/?LinkId=506648). Zaten yüklüyse, herhangi bir şey yapmanız gerekmez. (Uygulamayı çalışma zamanında izlemeye başlamak için durum izleyicisini zaten kullanmış olabilirsiniz.)

Derleme zamanı SDK'sının yanı sıra durum izleyicisini kullanarak, aşağıdakileri içeren daha kapsamlı bir telemetri elde edersiniz:

* [Performans sayaçları](app-insights-performance-counters.md): CPU, bellek, disk ve uygulamanızla ilgili diğer performans sayaçları.
* [Özel durumlar](app-insights-asp-net-exceptions.md): Bazı özel durumlar için daha ayrıntılı telemetri.
* [Bağımlılıklar](app-insights-asp-net-dependencies.md): Dönüş değerlerini içerir.

### <a name="webpages-and-single-page-apps"></a>Web sayfaları ve tek sayfalı uygulamalar
1. Sayfa görüntüleme sayısı, yükleme süreleri, tarayıcı özel durumları, AJAX çağrı performansı, kullanıcı ve oturum sayıları ile ilgili verileri görmek için web sayfalarınıza [JavaScript kod parçacığını](app-insights-javascript.md) ekleyin. Bunlar Tarayıcı ve Kullanım dikey pencerelerinde görünür.
2. Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için [özel olaylar kodlayın](app-insights-api-custom-events-metrics.md).


### <a name="diagnostic-code"></a>Tanılama kodu
Sorun mu yaşıyorsunuz? Tanılamasına yardımcı olmak için uygulamanıza kod eklemek isterseniz birkaç farklı seçenekten yararlanabilirsiniz:

* [Günlük izlemelerini yakalama](app-insights-asp-net-trace-logs.md): Olayları günlükle izlemek için zaten Log4N, NLog veya System.Diagnostics.Trace kullanıyorsanız çıktı Application Insights’a gönderilebilir. Bu çıktıyı isteklerle ilişkilendirebilir, analiz edebilir ve çıktıda arama yapabilirsiniz.
* [Özel olaylar ve ölçümler](app-insights-api-custom-events-metrics.md): Sunucu veya web sayfası kodunda TrackEvent() ve TrackMetric() kullanın.
* [Telemetriyi ek özelliklerle etiketleyin](app-insights-api-filtering-sampling.md#add-properties).

Belirli olayları bulmak ve ilişkilendirmek için [Arama](app-insights-diagnostic-search.md)’yı ve daha güçlü sorgular gerçekleştirmek için [Analiz](app-insights-analytics.md)’i kullanın.

## <a name="alerts"></a>Uyarılar
Uygulamanızda sorun olup olmadığını ilk siz öğrenin.

* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](app-insights-proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](app-insights-alerts.md): Bir metrik tarafından herhangi bir eşiğin aşılması durumunda uyarı almak için bunları ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

Varsayılan olarak, uyarı bildirimleri Azure aboneliğinin sahibine gönderilir.

![Örnek uyarı e-postasının ekran görüntüsü](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
Uygulama sürümünü izlemek için `buildinfo.config` dosyasının Microsoft Build Engine işleminiz tarafından oluşturulduğundan emin olun. .csproj dosyanızda şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup>
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bu sayede, [tanılama aramaları](app-insights-diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](app-insights-metrics-explorer.md) sürüme göre filtreleyebilirsiniz.

Bununla birlikte, derleme sürüm numarasının Visual Studio’daki geliştirici derlemesi tarafından değil de yalnızca Microsoft Build Engine tarafından oluşturulduğunu fark edersiniz.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Visual Studio Team Services’ı kullanıyorsanız yeni bir sürüm yayımladığınızda grafiklerinize [ek açıklama işaretçisi](app-insights-annotations.md) eklenmesini sağlayabilirsiniz. Aşağıdaki görüntüde bu işaretin nasıl göründüğü gösterilmiştir.

![Bir grafikteki örnek sürüm ek açıklamasının ekran görüntüsü](./media/app-insights-asp-net/release-annotation.png)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
**[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md)**<br/>Telemetri, tanılama araması ve kodun detayına gitme ile hata ayıklama hakkında bilgi içerir.

**[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/> Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma hakkında bilgi içerir.

