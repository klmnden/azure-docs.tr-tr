<properties 
    pageTitle="Application Insights ile ASP.NET Web uygulaması analizini ayarlama | Microsoft Azure" 
    description="Şirket içi veya Azure’de barındırılan ASP.NET web siteniz için performans, kullanılabilirlik ve kullanım analizi yapılandırın." 
    services="application-insights" 
    documentationCenter=".net"
    authors="NumberByColors" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/09/2016" 
    ms.author="daviste"/>


# ASP.NET için Application Insights’ı ayarlama

[Visual Studio Application Insights](app-insights-overview.md), [performans sorunlarını ve özel durumlarını saptayıp tanılamanıza](app-insights-detect-triage-diagnose.md) ve [uygulamanızın nasıl kullanıldığını keşfetmenize](app-insights-overview-usage.md) yardımcı olmak amacıyla canlı uygulamanızı izler.  Azure web uygulamalarının yanı sıra şirket içi kendi IIS sunucularınızda veya bulut VM’lerinde ler barındırılan uygulamalar için çalışır.


## Başlamadan önce

Gerekenler:

* Visual Studio 2013 güncelleştirme 3 veya sonraki bir sürümü. Ne kadar yeniyse o kadar iyidir.
* Bir [Microsoft Azure](http://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi buna ekleyebilir. 

İlgileniyorsanız bakmak için alternatif makaleler de vardır:

* [Çalışma zamanında web uygulamasını düzenleme](app-insights-monitor-performance-live-website-now.md)
* [Azure Bulut Hizmetleri](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Application Insights SDK’si ekleme


### Yeni bir projeyse...

Visual Studio'da yeni bir proje oluşturduğunuzda, Application Insights’ın seçili olduğundan emin olun. 


![ASP.NET projesi oluşturma](./media/app-insights-asp-net/appinsights-01-vsnewp1.png)


### ... veya mevcut bir projeyse

Çözüm Gezgini'nde projeye sağ tıklayın ve **Application Insights Telemetrisi Ekle**’yi veya **Application Insights'ı Yapılandır**’ı seçin.

![Application Insights Ekle’yi seçme](./media/app-insights-asp-net/appinsights-03-addExisting.png)

* ASP.NET Core projesi? - [Birkaç kod satırını düzeltmek için bu yönergeleri izleyin](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started#add-application-insights-instrumentation-code-to-startupcs). 



## <a name="run"></a> 2. Uygulamanızı çalıştırma

F5 ile uygulamanızı çalıştırın ve deneyin: Birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da, günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz. 

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-asp-net/54.png)

## 3. Telemetrinize bakın…

### ...Visual Studio’da

Visual Studio'da Application Insights penceresini açın: Application Insights düğmesine tıklayın ya da Çözüm Gezgini'nde projenize sağ tıklayın:

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-asp-net/55.png)

Bu görünümde, uygulamanızın sunucu tarafında oluşturulan telemetri gösterilmektedir. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](app-insights-visual-studio.md).

<a name="monitor"></a> 
### ... portalda

*Yalnızca SDK Yükle* ’yu seçtiğiniz sürece Application Insights web portalındaki telemetriyi de görebilirsiniz. 

Portalda Visual Studio’ya göre daha fazla grafik, analiz aracı ve pano bulunur. 


Application Insights kaynağınızı [Azure portalında](https://portal.azure.com/) açın.

![Projenize sağ tıklayıp Azure portalını açın](./media/app-insights-asp-net/appinsights-04-openPortal.png)

Portal uygulamanızdan telemetrinin bir görünümünü açar:
![](./media/app-insights-asp-net/66.png)

* İlk telemetri [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nda görünür.
* **Ara**’da (1) tek tek etkinlikler görünür. Verilerin görünmesi birkaç dakika sürebilir. Özelliklerini görmek için herhangi bir etkinliğe tıklayın. 
* Toplanan ölçümler grafiklerde (2) görünür. Verilerin burada görünmesi bir veya iki dakika sürebilir. Daha ayrıntılı bilginin bulunduğu dikey pencereyi açmak için herhangi bir grafiğe tıklayın.

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](app-insights-dashboards.md).

## 4. Uygulamanızı yayımlama

Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetrinizin Application Insights portalında oluşturulduğunu görebilir ve burada ölçümleri izleyebilir, telemetrinizde arama yapabilir ve [panolar](app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, kullanım ve performansı analiz etmek veya belirli olayları bulmak için güçlü [Analytics sorgu dilini](app-insights-analytics.md) kullanabilirsiniz. 

Tanılama araması ve [Eğilimler](app-insights-visual-studio-trends.md) gibi araçları kullanarak, [Visual Studio](app-insights-visual-studio.md)’da telemetrinizi analiz etmeye devam edebilirsiniz.

> [AZURE.NOTE] Uygulamanız [azaltma sınırlarına](app-insights-pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](app-insights-sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.


##<a name="land"></a> ‘Application Insights Ekle’ ne yapar?

Application Insights telemetriyi uygulamanızdan Application Insights portalına (Microsoft Azure’de barındırılır) gönderir:

![](./media/app-insights-asp-net/01-scheme.png)

Sonuçta komut şu üçünü yapar:

1. Application Insights Web SDK NuGet paketini projenize ekleyin. Visual Studio’da görmek için, projenize sağ tıklayın ve NuGet Paketlerini Yönet’i seçin.
2. Application Insights kaynağını [Azure portalında](https://portal.azure.com/) oluşturun. Verilerinizi göreceğiniz yer burasıdır. Kaynağı belirten *izleme anahtarını* alır.
3. İzleme anahtarını `ApplicationInsights.config` öğesine ekler, böylece SDK portala telemetri gönderebilir.

İsterseniz bu adımları [ASP.NET 4](app-insights-windows-services.md) veya [ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started) için el ile uygulayabilirsiniz.

### Gelecekteki SDK sürümlerine yükseltmek için

[SDK'nın yeni sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. Microsoft.ApplicationInsights.Web’i ve Yükselt’i seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.



## Sonraki adımlar

| | 
|---|---
|**[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md)**<br/>Telemetriyle hata ayıklama, tanılama arama, kodun ayrıntısına gitme.|![Visual studio](./media/app-insights-asp-net/61.png)
|**[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma. |![Visual studio](./media/app-insights-asp-net/62.png)
|**[Daha fazla veri ekleme](app-insights-asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. | ![Visual studio](./media/app-insights-asp-net/64.png)









<!--HONumber=sep16_HO1-->


