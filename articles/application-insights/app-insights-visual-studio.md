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
    ms.date="06/21/2016" 
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


![Özel durum yığın izlemesi](./media/app-insights-visual-studio/17.png)

Ayrıca, her bir yöntemin üzerindeki Kod Odağında Application Insights tarafından son 24 saatte günlüğe kaydedilen özel durumların sayısını görürsünüz.

![Özel durum yığın izlemesi](./media/app-insights-visual-studio/21.png)


## Yerel izleme



(Visual Studio 2015 Update 2’den) SDK’yı send telemetri verilerini Application Insights portalına gönderecek şekilde yapılandırmadıysanız (yani ApplicationInsights.config’de bir izleme anahtarı yoksa), tanılama penceresinde en son hata ayıklama oturumunuzdan alınan telemetri görüntülenir. 

Daha önce uygulamanızın önceki bir sürümünü yayımladıysanız bu iyi bir şeydir. Hata ayıklama oturumlarınızdan alınan telemetrinin, yayımlanan uygulamanın Application Insights portalındaki telemetriyle karışmasını istemezsiniz.

Telemetriyi portala göndermeden önce hatalarını ayıklamak istediğiniz [özel telemetri](app-insights-api-custom-events-metrics.md) verilerine sahip olmanız da yararlı olur.


* *İlk olarak Application Insights’ı portala telemetri gönderecek şekilde tam olarak yapılandırdım. Ancak, artık telemetriyi yalnızca Visual Studio'da görmek istiyorum.*

 * Arama penceresinin Ayarlar bölümünde, uygulamanız portala telemetri gönderiyor olsa bile yerel tanılamalarda arama seçeneği vardır.
 * Portala telemetri gönderimini durdurmak için Applicationınsights.config’de `<instrumentationkey>...` satırını açıklama satırına dönüştürün. Telemetriyi yeniden portala göndermeye hazır olduğunuzda açıklamayı kaldırın.

## Eğilimler

Eğilimler, uygulamanızın zaman içinde nasıl davrandığını görselleştirmeye yönelik bir araçtır. 

Application Insights araç çubuğu düğmesinden veya Application Insights Arama penceresinden **Telemetri Eğilimlerini Keşfet**’i seçin. Başlamak için beş genel sorgudan birini seçin. Telemetri türleri, zaman aralıkları ve diğer özelliklere göre farklı veri kümelerini çözümleyebilirsiniz. 

Verilerinizdeki anormallikleri bulmak için "Görünüm Türü" açılır listesi altındaki anormallik seçeneklerinden birini belirleyin. Pencerenin altındaki filtreleme seçenekleri, telemetrinizdeki belirli alt kümelere odaklanmayı kolaylaştırır.

![Eğilimler](./media/app-insights-visual-studio/51.png)

[Eğilimler hakkında daha fazla bilgi](app-insights-visual-studio-trends.md).

## Sırada ne var?

||
|---|---
|**[Daha fazla veri ekleme](app-insights-asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. | ![Visual studio](./media/app-insights-asp-net/64.png)
|**[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma. |![Visual studio](./media/app-insights-asp-net/62.png)


 



<!--HONumber=Aug16_HO4-->


