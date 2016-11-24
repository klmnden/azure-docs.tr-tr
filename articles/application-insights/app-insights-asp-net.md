---
title: "Application Insights ile ASP.NET Web uygulaması analizini ayarlama | Microsoft Belgeleri"
description: "Şirket içi veya Azure’de barındırılan ASP.NET web siteniz için performans, kullanılabilirlik ve kullanım analizi yapılandırın."
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: douge
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/13/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: dea21a59b189d1d3d474cbc5e67f64df485a1981
ms.openlocfilehash: a97e20b208d92e03bd4458605aaa48ef7c389e32


---
# <a name="set-up-application-insights-for-aspnet"></a>ASP.NET için Application Insights’ı ayarlama
[Azure Application Insights](app-insights-overview.md), [performans sorunlarını ve özel durumlarını saptayıp tanılamanıza](app-insights-detect-triage-diagnose.md) ve [uygulamanızın nasıl kullanıldığını keşfetmenize](app-insights-overview-usage.md) yardımcı olmak amacıyla canlı uygulamanızı izler.  Azure web uygulamalarının yanı sıra şirket içi kendi IIS sunucularınızda veya bulut VM’lerinde ler barındırılan uygulamalar için çalışır.

## <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* Visual Studio 2013 güncelleştirme 3 veya sonraki bir sürümü. Ne kadar yeniyse o kadar iyidir.
* Bir [Microsoft Azure](http://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi buna ekleyebilir. 

İlgileniyorsanız bakmak için alternatif makaleler de vardır:

* [Çalışma zamanında web uygulamasını düzenleme](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud services](app-insights-cloudservices.md)

## <a name="a-nameidea-1-add-application-insights-sdk"></a><a name="ide"></a> 1. Application Insights SDK’si ekleme
### <a name="if-its-a-new-project"></a>Yeni bir projeyse...
Visual Studio'da yeni bir proje oluşturduğunuzda, Application Insights’ın seçili olduğundan emin olun. 

![ASP.NET projesi oluşturma](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)

### <a name="-or-if-its-an-existing-project"></a>... veya mevcut bir projeyse
Çözüm Gezgini'nde projeye sağ tıklayın ve **Application Insights Telemetrisi Ekle**’yi veya **Application Insights'ı Yapılandır**’ı seçin.

![Application Insights Ekle’yi seçme](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* ASP.NET Core projesi? - [Birkaç kod satırını düzeltmek için bu yönergeleri izleyin](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 

## <a name="a-nameruna-2-run-your-app"></a><a name="run"></a> 2. Uygulamanızı çalıştırma
F5 ile uygulamanızı çalıştırın ve deneyin: Birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da, günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz. 

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-asp-net/54.png)

## <a name="3-see-your-telemetry"></a>3. Telemetrinize bakın…
### <a name="-in-visual-studio"></a>...Visual Studio’da
Visual Studio'da Application Insights penceresini açın: Application Insights düğmesine tıklayın ya da Çözüm Gezgini'nde projenize sağ tıklayın:

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-asp-net/55.png)

Bu görünümde, uygulamanızın sunucu tarafında oluşturulan telemetri gösterilmektedir. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](app-insights-visual-studio.md).

<a name="monitor"></a> 

### <a name="-in-the-portal"></a>... portalda
*Yalnızca SDK Yükle* ’yu seçtiğiniz sürece Application Insights web portalındaki telemetriyi de görebilirsiniz. 

Portalda Visual Studio’ya göre daha fazla grafik, analiz aracı ve pano bulunur. 

Application Insights kaynağınızı [Azure portalında](https://portal.azure.com/) açın.

![Projenize sağ tıklayıp Azure portalını açın](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Portal, uygulamanızdan telemetrinin bir görünümünü açar: ![](./media/app-insights-asp-net/66.png)

* İlk telemetri [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nda görünür.
* **Ara**’da (1) tek tek etkinlikler görünür. Verilerin görünmesi birkaç dakika sürebilir. Özelliklerini görmek için herhangi bir etkinliğe tıklayın. 
* Toplanan ölçümler grafiklerde (2) görünür. Verilerin burada görünmesi bir veya iki dakika sürebilir. Daha ayrıntılı bilginin bulunduğu dikey pencereyi açmak için herhangi bir grafiğe tıklayın.

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](app-insights-dashboards.md).

## <a name="4-publish-your-app"></a>4. Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetrinizin Application Insights portalında oluşturulduğunu görebilir ve burada ölçümleri izleyebilir, telemetrinizde arama yapabilir ve [panolar](app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, kullanım ve performansı analiz etmek veya belirli olayları bulmak için güçlü [Analytics sorgu dilini](app-insights-analytics.md) kullanabilirsiniz. 

Tanılama araması ve [Eğilimler](app-insights-visual-studio-trends.md) gibi araçları kullanarak, [Visual Studio](app-insights-visual-studio.md)’da telemetrinizi analiz etmeye devam edebilirsiniz.

> [!NOTE]
> Uygulamanız [azaltma sınırlarına](app-insights-pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](app-insights-sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.
> 
> 

## <a name="a-namelanda-what-did-add-application-insights-do"></a><a name="land"></a> ‘Application Insights Ekle’ seçeneği ne yapar?
Application Insights telemetriyi uygulamanızdan Application Insights portalına (Microsoft Azure’de barındırılır) gönderir:

![](./media/app-insights-asp-net/01-scheme.png)

Sonuçta komut şu üçünü yapar:

1. Application Insights Web SDK NuGet paketini projenize ekleyin. Visual Studio’da görmek için, projenize sağ tıklayın ve NuGet Paketlerini Yönet’i seçin.
2. Application Insights kaynağını [Azure portalında](https://portal.azure.com/) oluşturun. Verilerinizi göreceğiniz yer burasıdır. Kaynağı belirten *izleme anahtarını* alır.
3. İzleme anahtarını `ApplicationInsights.config` öğesine ekler, böylece SDK portala telemetri gönderebilir.

İsterseniz bu adımları [ASP.NET 4](app-insights-windows-services.md) veya [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started) için el ile uygulayabilirsiniz.

### <a name="to-upgrade-to-future-sdk-versions"></a>Gelecekteki SDK sürümlerine yükseltmek için
[SDK'nın yeni sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. Microsoft.ApplicationInsights.Web’i ve Yükselt’i seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.

## <a name="add-more-telemetry"></a>Daha fazla telemetri ekleme
### <a name="web-pages-and-single-page-apps"></a>Web sayfaları ve tek sayfalık uygulamalar
1. Sayfa görüntülemeleri, yükleme süreleri, tarayıcı özel durumları, AJAX çağrı performansı, kullanıcı ve oturum sayıları hakkında veri içeren Tarayıcı ve Kullanım dikey pencerelerini açmak için web sayfalarına [JavaScript kod parçacığını ekleyin](app-insights-javascript.md).
2. Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için [özel olaylar kodlayın](app-insights-api-custom-events-metrics.md).

### <a name="dependencies-exceptions-and-performance-counters"></a>Bağımlılıklar, özel durumlar ve performans sayaçları
Uygulamanız hakkında ek telemetri almak için sunucu makinelerinizin her birine [Durum İzleyicisi’ni yükleyin](app-insights-monitor-performance-live-website-now.md). Şunları elde edersiniz:

* [Performans sayaçları](app-insights-performance-counters.md) - 
  Uygulamanıza ilişkin CPU, bellek, disk ve diğer performans sayaçları. 
* [Özel durumlar](app-insights-asp-net-exceptions.md) - bazı özel durumlar için daha ayrıntılı telemetri.
* [Bağımlılıklar](app-insights-asp-net-dependencies.md) - REST API veya SQL hizmetleri için çağrılar. Dış bileşenlerin yavaş yanıt vermesinin, uygulamanızda performans sorunlarına yol açıp açmadığını öğrenin. (Uygulamanız .NET 4.6 üzerinde çalışıyorsa bu telemetriyi almak için Durum İzleyicisi’ne ihtiyacınız yoktur.)

### <a name="diagnostic-code"></a>Tanılama kodu
Sorun mu yaşıyorsunuz? Tanılamasına yardımcı olmak için uygulamanıza kod eklemek isterseniz birkaç farklı seçenekten yararlanabilirsiniz:

* [Günlük izlemelerini alma](app-insights-asp-net-trace-logs.md): İzleme olaylarını günlüğe kaydetmek için zaten Log4N, NLog veya System.Diagnostics.Trace’i kullanıyorsanız çıkış, isteklerle ilişkilendirmeniz, arama yapabilmeniz ve çözümleyebilmeniz için Application Insights’a gönderilebilir. 
* [Özel olaylar ve ölçümler](app-insights-api-custom-events-metrics.md): Sunucu veya web sayfası kodunda TrackEvent() ve TrackMetric() öğesini kullanın.
* [Ek özelliklere sahip etiket telemetrisi](app-insights-api-filtering-sampling.md#add-properties)

Belirli olayları bulmak ve ilişkilendirmek için [Arama](app-insights-diagnostic-search.md)’yı ve daha güçlü sorgular gerçekleştirmek için [Analiz](app-insights-analytics.md)’i kullanın.

## <a name="alerts"></a>Uyarılar
Uygulamanızda sorun olup olmadığını ilk siz öğrenin. (Kullanıcılarınızın size söylemesini beklemeyin!) 

* Sitenizin web’de göründüğünden emin olmak için [web testleri oluşturun](app-insights-monitor-web-app-availability.md).
* [Proaktif tanılama](app-insights-proactive-diagnostics.md) otomatik olarak çalışır (uygulamanızda belirlenen en düşük miktarda bir trafik varsa). Bunları ayarlamak için bir şey yapmanıza gerek yoktur. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* Bir ölçüm eşiği aştığında sizi uyaracak [ölçüm uyarıları ayarlayın](app-insights-alerts.md). Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

Varsayılan olarak, uyarı bildirimleri Azure aboneliğinin sahibine gönderilir. 

![uyarı e-posta örneği](./media/app-insights-asp-net/alert-email.png)

## <a name="version-and-release-tracking"></a>Sürüm ve sürüm izleme
### <a name="track-application-version"></a>Uygulama sürümünü izleme
`buildinfo.config` öğesinin MSBuild işleminiz tarafından oluşturulduğundan emin olun. .csproj dosyanızda şunları ekleyin:  

```XML

    <PropertyGroup>
      <GenerateBuildInfoConfigFile>true</GenerateBuildInfoConfigFile>    <IncludeServerNameInBuildInfo>true</IncludeServerNameInBuildInfo>
    </PropertyGroup> 
```

Yapı bilgisi mevcut olduğunda Application Insights web modülü **Uygulama sürümünü** telemetrinin her bir öğesine bir özellik olarak ekler. Bunu yaptığınızda, [tanılama aramaları](app-insights-diagnostic-search.md) gerçekleştirirken veya [ölçümleri keşfederken](app-insights-metrics-explorer.md) sürüme göre filtreleyebilirsiniz. 

Ancak, derleme sürüm numarasının Visual Studio’daki geliştirici derlemesi tarafından değil, yalnızca MS Build tarafından oluşturulduğuna dikkat edin.

### <a name="release-annotations"></a>Sürüm ek açıklamaları
Visual Studio Team Services’ı kullanıyorsanız yeni bir sürüm yayımladığınızda grafiklerinize [ek açıklama işaretçisi](app-insights-annotations.md) eklenmesini sağlayabilirsiniz.

![sürüm ek açıklama örneği](./media/app-insights-asp-net/release-annotation.png)

## <a name="next-steps"></a>Sonraki adımlar
|  |
| --- | --- |
| **[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md)**<br/>Telemetriyle hata ayıklama, tanılama arama, kodun ayrıntısına gitme. |
| **[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma. |
| **[Daha fazla veri ekleme](app-insights-asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. |




<!--HONumber=Nov16_HO3-->


