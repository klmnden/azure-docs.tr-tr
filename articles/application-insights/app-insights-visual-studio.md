---
title: "Visual Studio’da Azure Application Insights ile çalışma | Microsoft Belgeleri"
description: "Hata ayıklama ve üretim sırasında wen uygulaması performans analizi ve tanılama."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/17/2016
ms.author: awills
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 7fe1b572104416fec62261c60228fa966d197dd9
ms.lasthandoff: 03/21/2017


---
# <a name="working-with-azure-application-insights-in-visual-studio"></a>Visual Studio’da Azure Application Insights ile çalışma
Visual Studio’da (2015 ve sonraki sürümler) hem hata ayıklama hem de üretim sırasında [Azure Application Insights](app-insights-overview.md)’tan alınan telemetri verilerini kullanarak, ASP.NET web uygulamanızdaki performansı çözümleyebilir ve sorunları tanılayabilirsiniz.

ASP.NET web uygulamanızı Visual Studio 2017 veya sonraki bir sürümle oluşturduysanız, Application Insights SDK’sı zaten yüklüdür. Diğer sürümlerde, henüz yapmadıysanız [uygulamanıza Application Insights ekleyin](app-insights-asp-net.md).

Uygulamanızı canlı üretim sırasında izlemek için, normalde uyarlar ayarlayıp güçlü izleme araçları uygulayabileceğiniz [Azure portaldaki](https://portal.azure.com) Application Insights telemetrisini görüntülersiniz. Ancak, hata ayıklama için ayrıca Visual Studio’da telemetriyi arayıp çözümleyebilirsiniz. Visual Studio’yu kullanarak telemetriyi hem üretim merkezinizden hem de dağıtım makinenizdeki hata ayıklama çalıştırmalarından çözümleyebilirsiniz. İkinci durumda, SDK’yı henüz Azure portala telemetri gönderecek şekilde yapılandırmadıysanız bile hata ayıklama çalıştırmalarını çözümleyebilirsiniz. 

## <a name="run"></a> Projenizin hatalarını ayıklama
F5 kullanarak web uygulamanızı yerel hata ayıklama modunda çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da, projenize Application Insights modülü tarafından kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/app-insights-visual-studio/appinsights-09eventcount.png)

Telemetrinizde arama yapmak için bu düğmeye tıklayın. 

## <a name="application-insights-search"></a>Application Insights araması
Application Insights Arama penceresi günlüğe kaydedilmiş olayları gösterir. (Application Insights’ı ayarlarken Azure’da oturum açarsanız aynı olayları Azure portalda da arayabilirsiniz.)

![Projeye sağ tıklayın ve Application Insights, Ara’yı seçin](./media/app-insights-visual-studio/34.png)

> [!NOTE] 
> Filtreleri seçtikten veya seçimini kaldırdıktan sonra, metin arama alanının sonundaki Ara düğmesine tıklayın.
>

Serbest metin arama işlevi olaylardaki tüm alanlarda çalışır. Örneğin, bir sayfanın URL’sinin bir kısmını ya da istemcinin şehri gibi bir özelliğin değerini veya bir izleme günlüğündeki belirli kelimeleri arayın.

Ayrıntılı özelliklerini görmek için herhangi bir etkinliğe tıklayın.

Web uygulamanıza gönderilen istekler için koda tıklayabilirsiniz.

![İstek Ayrıntıları altındaki koda tıklayın](./media/app-insights-visual-studio/31.png)

Başarısız isteklerin veya özel durumların tanılanmasına yardımcı olması için ilgili öğeleri de açabilirsiniz.

![İstek Ayrıntıları altında ilgili öğelere gidin](./media/app-insights-visual-studio/41.png)

## <a name="exceptions-and-failed-requests"></a>Özel durumlar ve başarısız istekler
Arama penceresinde özel durum raporları gösterilir. (ASP.NET uygulamasının bazı eski türlerinde, çerçeve tarafından işlenen özel durumları görmek için [özel durum izlemeyi ayarlamanız](app-insights-asp-net-exceptions.md) gerekir.)

Yığın izlemesi almak için bir özel duruma tıklayın. Visual Studio’da uygulamanın kodu açıksa yığın izlemesinden tıklayarak ilgili kod satırına gidebilirsiniz.

![Özel durum yığın izlemesi](./media/app-insights-visual-studio/17.png)

## <a name="request-and-exception-summaries-in-the-code"></a>Koddaki istek ve özel durum özetleri
Her bir işleyici yönteminin üzerindeki Kod Odağında, Application Insights tarafından son 24 saatte günlüğe kaydedilen istek ve özel durumların sayısını görürsünüz.

![Özel durum yığın izlemesi](./media/app-insights-visual-studio/21.png)

> [!NOTE] 
> Kod Odağı, Application Insights verilerini yalnızca [uygulamanızı Application Insights portalına telemetri gönderecek şekilde yapılandırdıysanız](app-insights-asp-net.md) gösterir.
>

[Kod Odağı’nda Application Insights hakkında daha fazla bilgi](app-insights-visual-studio-codelens.md)

## <a name="trends"></a>Eğilimler
Eğilimler, uygulamanızın zaman içinde nasıl davrandığını görselleştirmeye yönelik bir araçtır. 

Application Insights araç çubuğu düğmesinden veya Application Insights Arama penceresinden **Telemetri Eğilimlerini Keşfet**’i seçin. Başlamak için beş genel sorgudan birini seçin. Telemetri türleri, zaman aralıkları ve diğer özelliklere göre farklı veri kümelerini çözümleyebilirsiniz. 

Verilerinizdeki anormallikleri bulmak için "Görünüm Türü" açılır listesi altındaki anormallik seçeneklerinden birini belirleyin. Pencerenin altındaki filtreleme seçenekleri, telemetrinizdeki belirli alt kümelere odaklanmayı kolaylaştırır.

![Eğilimler](./media/app-insights-visual-studio/51.png)

[Eğilimler hakkında daha fazla bilgi](app-insights-visual-studio-trends.md).

## <a name="local-monitoring"></a>Yerel izleme
(Visual Studio 2015 Update 2’den) SDK’yı send telemetri verilerini Application Insights portalına gönderecek şekilde yapılandırmadıysanız (yani ApplicationInsights.config’de bir izleme anahtarı yoksa), tanılama penceresinde en son hata ayıklama oturumunuzdan alınan telemetri görüntülenir. 

Daha önce uygulamanızın önceki bir sürümünü yayımladıysanız bu iyi bir şeydir. Hata ayıklama oturumlarınızdan alınan telemetrinin, yayımlanan uygulamanın Application Insights portalındaki telemetriyle karışmasını istemezsiniz.

Telemetriyi portala göndermeden önce hatalarını ayıklamak istediğiniz [özel telemetri](app-insights-api-custom-events-metrics.md) verilerine sahip olmanız da yararlı olur.

* *Başlangıçta, Application Insights’ı portala telemetri gönderecek şekilde tam olarak yapılandırdım. Ancak, artık telemetriyi yalnızca Visual Studio'da görmek istiyorum.*
  
  * Arama penceresinin Ayarlar bölümünde, uygulamanız portala telemetri gönderiyor olsa bile yerel tanılamalarda arama seçeneği vardır.
  * Portala telemetri gönderimini durdurmak için Applicationınsights.config’de `<instrumentationkey>...` satırını açıklama satırına dönüştürün. Telemetriyi yeniden portala göndermeye hazır olduğunuzda açıklamayı kaldırın.


## <a name="whats-next"></a>Sırada ne var?
|  |  |
| --- | --- |
| **[Daha fazla veri ekleme](app-insights-asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. |![Visual studio](./media/app-insights-visual-studio/64.png) |
| **[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma. |![Visual studio](./media/app-insights-visual-studio/62.png) |


