<properties
    pageTitle="Visual Studio Application Insights’a başlarken | Microsoft Azure"
    description="Visual Studio Application Insights ile şirket içi.veya Microsoft Azure web uygulamanızın kullanımını, kullanılabilirliğini ve performansını analiz edin."
    services="application-insights"
    documentationCenter=""
    authors="alancameronwills"
    manager="douge"/>

<tags
    ms.service="application-insights"
    ms.workload="tbd"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="03/31/2016"
    ms.author="awills"/>

# Visual Studio Application Insights’a başlarken

*Application Insights önizlemededir.*

Sorunları tespit edin, sorunları çözün ve uygulamanızı sürekli geliştirin. Canlı uygulamanızdaki sorunları hemen tanılayın. Kullanıcılarınızın bununla neler yaptığını anlayın.

Yapılandırma çok kolaydır; sonuçları birkaç dakika içinde görürsünüz.

Şu anda iOS, Android, Windows uygulamalarını, J2EE ve ASP.NET web uygulamalarını, WCF hizmetlerini desteklemekteyiz. Web uygulamaları Azure veya kendi şirket içi sunucularınızda çalıştırılabilir. JavaScript SDK’miz herhangi bir web sayfasında çalışır.

[Giriş animasyonuna göz atın](https://www.youtube.com/watch?v=fX2NtGrh-Y0).

## Başlarken

Aşağıdaki diyagramda görünen giriş noktalarının bileşimleriyle, herhangi bir sırada başlayın. Size uygun yolu izleyin.

Application Insights, uygulamanıza bir SDK ekleyerek çalışır; bu SDK [Azure portalına](https://portal.azure.com) telemetri gönderir. Çeşitli platform, dil ve desteklenen IDE bileşimleri için farklı SDK’ler vardır.

[Microsoft Azure](http://azure.com)’de bir hesabınızın olması gerekir. Kuruluşunuzda bir grup hesabına erişiminiz olabileceği gibi, Kullandıkça öde hesabı da almak isteyebilirsiniz. Application Insights’ta ücretsiz bir katman vardır; bu nedenle uygulamanız yaygınlaşana kadar ödeme yapmanız gerekmez. [Fiyatlandırma sayfasını](https://azure.microsoft.com/pricing/details/application-insights/) gözden geçirin.

Ne istiyorsunuz | Ne yapılmalı | Ne alacaksınız
---|---|---
 <a href="app-insights-asp-net.md">![ASP.NET](./media/app-insights-get-started/appinsights-gs-i-01-perf.png)</a> | <a href="app-insights-asp-net.md">Web projenize Application Insights SDK ekleme</a> <br/> ![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-asp-net.md">![Performansı ve kullanımı izleme](./media/app-insights-get-started/appinsights-gs-r-01-perf.png)</a>
<a href="app-insights-monitor-performance-live-website-now.md">![ASP.NET sitesi zaten canlı](./media/app-insights-get-started/appinsights-gs-i-04-red2.png)</a><br/><a href="app-insights-monitor-performance-live-website-now.md">![Bağımlılığı ve performansı izleme](./media/app-insights-get-started/appinsights-gs-i-03-red.png)</a>|<a href="app-insights-monitor-performance-live-website-now.md">IIS sunucunuza Durum İzleyicisi yükleme</a> <br/> ![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-performance-live-website-now.md">![ASP.NET bağımlılığını izleme](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="insights-perf-analytics.md">![Azure web uygulaması veya VM](./media/app-insights-get-started/appinsights-gs-i-10-azure.png)</a>|<a href="insights-perf-analytics.md">Azure web uygulamanızda veya VM’de Insights’ı etkinleştirme</a> <br/> ![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="insights-perf-analytics.md">![Bağımlılığı ve performansı izleme](./media/app-insights-get-started/appinsights-gs-r-03-red.png)</a>
<a href="app-insights-java-get-started.md">![Java](./media/app-insights-get-started/appinsights-gs-i-11-java.png)</a>|<a href="app-insights-java-get-started.md">Java projenize SDK ekleme</a><br/>![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-java-get-started.md">![Performansı ve kullanımı izleme](./media/app-insights-get-started/appinsights-gs-r-10-java.png)</a>
<a href="app-insights-web-track-usage.md">![JavaScript](./media/app-insights-get-started/appinsights-gs-i-02-usage.png)</a>|<a href="app-insights-web-track-usage.md">Application Insights betiğini web sayfalarınıza ekleme</a><br/>![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-web-track-usage.md">![sayfa görünümleri ve tarayıcı performansı](./media/app-insights-get-started/appinsights-gs-r-02-usage.png)</a>
<a href="app-insights-monitor-web-app-availability.md">![Kullanılabilirlik](./media/app-insights-get-started/appinsights-gs-i-05-avail.png)</a>|<a href="app-insights-monitor-web-app-availability.md">Web testleri oluşturma</a><br/>![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="app-insights-monitor-web-app-availability.md">![Kullanılabilirlik](./media/app-insights-get-started/appinsights-gs-r-05-avail.png)</a>
<a href="app-insights-platforms.md">![iOS, Android ve Windows cihazları](./media/app-insights-get-started/appinsights-gs-i-07-device.png)</a>|<a href="http://hockeyapp.net">HockeyApp kullanma</a><br/>![gets](./media/app-insights-get-started/appinsights-00arrow.png) | <a href="http://hockeyapp.net">![Kilitlenme ve kullanım verileri](./media/app-insights-get-started/appinsights-gs-r-06-device.png)</a>

## Destek ve geri bildirim


* Sorular ve sorunlar:
 * [Sorun giderme][qna]
 * [MSDN Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)
 * [StackOverflow](http://stackoverflow.com/questions/tagged/ms-application-insights)
* Hatalar:
 * [Bağlan](https://connect.microsoft.com/VisualStudio/Feedback/LoadSubmitFeedbackForm?FormID=6076)
* Öneriler:
 * [UserVoice](https://visualstudio.uservoice.com/forums/357324)
* Kod örnekleri
 * [Kod örnekleri](app-insights-code-samples.md)



## <a name="video"></a>Videolar


> [AZURE.VIDEO 218]

> [AZURE.VIDEO usage-monitoring-application-insights]

> [AZURE.VIDEO performance-monitoring-application-insights]



<!--Link references-->

[qna]: app-insights-troubleshoot-faq.md



<!----HONumber=Jun16_HO2-->


