<properties 
    pageTitle="Visual Studio’da Application Insights ile çalışma" 
    description="Hata ayıklama ve üretim sırasında performans analizi ve tanılama." 
    services="application-insights" 
    documentationCenter=".net"
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="05/25/2016" 
    ms.author="awills"/>


# Visual Studio’da Application Insights ile çalışma

Visual Studio’da (2015 ve sonraki sürümler) hem hata ayıklama hem de üretim sırasında [Visual Studio Application Insights](app-insights-overview.md)’tan alınan telemetri verilerini kullanarak performansı çözümleyebilir ve sorunları tanılayabilirsiniz.

[Application Insights’ı uygulamanıza yükleme](app-insights-asp-net.md) işlemini henüz gerçekleştirmediyseniz bunu şimdi yapın.

## <a name="run"></a> Projenizin hatalarını ayıklama

F5 ile uygulamanızı çalıştırın ve deneyin: Birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Tanılama aramayı açmak için bu düğmeye tıklayın. 



## Tanılama arama

Arama penceresi günlüğe kaydedilmiş olayları gösterir. (Application Insights’ı ayarlarken Azure’da oturum açarsanız aynı olayları portalda da arayabilirsiniz.)

![Projeye sağ tıklayın ve Application Insights, Ara’yı seçin](./media/app-insights-visual-studio/34.png)

Serbest metin arama işlevi olaylardaki tüm alanlarda çalışır. Örneğin, bir sayfanın URL’sinin bir kısmını ya da istemcinin şehri gibi bir özelliğin değerini veya bir izleme günlüğündeki belirli kelimeleri arayın.

Ayrıntılı özelliklerini görmek için herhangi bir etkinliğe tıklayın.

Başarısız isteklerin veya özel durumların tanılanmasına yardımcı olması için İlgili Öğeler sekmesini de açabilirsiniz.


![](./media/app-insights-visual-studio/41.png)



## Tanılama hub’ı

Tanılama Hub’ı (Visual Studio 2015 veya sonraki sürümlerde), Application Insights sunucusunun telemetri verileri oluşturuldukça bunları gösterir. Bu işlev, SDK’yı Azure portaldaki bir kaynağa bağlamadan yalnızca yüklemeyi tercih ettiyseniz bile çalışır.

![Tanılama Araçları penceresini açın ve Application Insights olaylarını denetleyin.](./media/app-insights-visual-studio/31.png)


## Özel Durumlar

[Özel durum izleme ayarladıysanız](app-insights-asp-net-exceptions.md), Arama penceresinde özel durum raporları görünür. 

Yığın izlemesi almak için bir özel duruma tıklayın. Visual Studio’da uygulamanın kodu açıksa yığın izlemesinden tıklayarak ilgili kod satırına gidebilirsiniz.


![](./media/app-insights-visual-studio/17.png)


## Yerel izleme



(Visual Studio 2015 Update 2’den) SDK’yı send telemetri verilerini Application Insights portalına gönderecek şekilde yapılandırmadıysanız (yani ApplicationInsights.config’de bir izleme anahtarı yoksa), tanılama penceresinde en son hata ayıklama oturumunuzdan alınan telemetri görüntülenir. 

Daha önce uygulamanızın önceki bir sürümünü yayımladıysanız bu iyi bir şeydir. Hata ayıklama oturumlarınızdan alınan telemetrinin, yayımlanan uygulamanın Application Insights portalındaki telemetriyle karışmasını istemezsiniz.

Telemetriyi portala göndermeden önce hatalarını ayıklamak istediğiniz [özel telemetri](app-insights-api-custom-events-metrics.md) verilerine sahip olmanız da yararlı olur.


* *İlk olarak Application Insights’ı portala telemetri gönderecek şekilde tam olarak yapılandırdım. Ancak, artık telemetriyi yalnızca Visual Studio'da görmek istiyorum.*

 * Arama penceresinin Ayarlar bölümünde, uygulamanız portala telemetri gönderiyor olsa bile yerel tanılamalarda arama seçeneği vardır.
 * Portala telemetri gönderimini durdurmak için Applicationınsights.config’de `<instrumentationkey>...` satırını açıklama satırına dönüştürün. Telemetriyi yeniden portala göndermeye hazır olduğunuzda açıklamayı kaldırın.





## Gelecekteki SDK sürümlerine yükseltmek için

[SDK'nın yeni sürümüne](app-insights-release-notes-dotnet.md) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. Microsoft.ApplicationInsights.Web’i ve Yükselt’i seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.



## Sırada ne var?

||
|---|---
|**[Daha fazla veri ekleme](app-insights-asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. | ![Visual studio](./media/app-insights-asp-net/64.png)
|**[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma. |![Visual studio](./media/app-insights-asp-net/62.png)


 



<!----HONumber=Jun16_HO2-->


