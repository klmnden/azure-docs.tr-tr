---
title: Visual Studio'da Azure Application Insights ile uygulama hatalarını ayıklama | Microsoft Docs
description: Hata ayıklama ve üretim sırasında wen uygulaması performans analizi ve tanılama.
services: application-insights
documentationcenter: .net
author: NumberByColors
manager: carmonm
ms.assetid: 2059802b-1131-477e-a7b4-5f70fb53f974
ms.service: application-insights
ms.custom: vs-azure
ms.workload: azure-vs
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/07/2017
ms.pm_owner: daviste;NumberByColors
ms.reviewer: mbullwin
ms.author: daviste
ms.openlocfilehash: 1b2f429129c0bb9098f4f5029cb07ce06bc5db13
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66255126"
---
# <a name="debug-your-applications-with-azure-application-insights-in-visual-studio"></a>Uygulamalarınızı Visual Studio'da Azure Application Insights ile hata ayıklama
Visual Studio’da (2015 ve sonraki sürümler) hem hata ayıklama hem de üretim sırasında [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md)’tan alınan telemetri verilerini kullanarak, ASP.NET web uygulamanızdaki performansı çözümleyebilir ve sorunları tanılayabilirsiniz.

ASP.NET web uygulamanızı Visual Studio 2017 veya sonraki bir sürümle oluşturduysanız, Application Insights SDK’sı zaten yüklüdür. Diğer sürümlerde, henüz yapmadıysanız [uygulamanıza Application Insights ekleyin](../../azure-monitor/app/asp-net.md).

Uygulamanızı canlı üretim sırasında izlemek için, normalde uyarlar ayarlayıp güçlü izleme araçları uygulayabileceğiniz [Azure portaldaki](https://portal.azure.com) Application Insights telemetrisini görüntülersiniz. Ancak, hata ayıklama için ayrıca Visual Studio’da telemetriyi arayıp çözümleyebilirsiniz. Hem üretim merkezinizden hem de çalıştırmaları geliştirme makinenizde hata ayıklama gelen telemetriyi analiz etmek için Visual Studio'yu kullanabilirsiniz. İkinci durumda, SDK’yı henüz Azure portala telemetri gönderecek şekilde yapılandırmadıysanız bile hata ayıklama çalıştırmalarını çözümleyebilirsiniz. 

## <a name="run"></a> Projenizin hatalarını ayıklama
F5 kullanarak web uygulamanızı yerel hata ayıklama modunda çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da, projenize Application Insights modülü tarafından günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio'da, hata ayıklama sırasında Application Insights düğmesi gösterilir.](./media/visual-studio/appinsights-09eventcount.png)

Telemetrinizde arama yapmak için bu düğmeye tıklayın. 

## <a name="application-insights-search"></a>Application Insights araması
Application Insights Arama penceresi günlüğe kaydedilmiş olayları gösterir. (Application ınsights'ı oluşturduğunuzda, Azure için azure'da oturum açarsanız aynı olayları Azure portalda arayabilirsiniz.)

![Projeye sağ tıklayın ve Application Insights, Ara’yı seçin](./media/visual-studio/34.png)

> [!NOTE] 
> Filtreleri seçtikten veya seçimini kaldırdıktan sonra, metin arama alanının sonundaki Ara düğmesine tıklayın.
>

Serbest metin arama işlevi olaylardaki tüm alanlarda çalışır. Örneğin, bir sayfanın URL’sinin bir kısmını ya da istemcinin şehri gibi bir özelliğin değerini veya bir izleme günlüğündeki belirli kelimeleri arayın.

Ayrıntılı özelliklerini görmek için herhangi bir etkinliğe tıklayın.

Web uygulamanıza gönderilen istekler için koda tıklayabilirsiniz.

![İstek Ayrıntıları altındaki koda tıklayın](./media/visual-studio/31.png)

Başarısız isteklerin veya özel durumların tanılanmasına yardımcı olması için ilgili öğeleri de açabilirsiniz.

![İstek Ayrıntıları altında ilgili öğelere gidin](./media/visual-studio/41.png)

## <a name="view-exceptions-and-failed-requests"></a>Görünüm özel durumları ve başarısız istekler
Arama penceresinde özel durum raporları gösterilir. (ASP.NET uygulamasının bazı eski türlerinde, çerçeve tarafından işlenen özel durumları görmek için [özel durum izlemeyi ayarlamanız](../../azure-monitor/app/asp-net-exceptions.md) gerekir.)

Yığın izlemesi almak için bir özel duruma tıklayın. Visual Studio’da uygulamanın kodu açıksa yığın izlemesinden tıklayarak ilgili kod satırına gidebilirsiniz.

![Özel durum yığın izlemesi](./media/visual-studio/17.png)

## <a name="view-request-and-exception-summaries-in-the-code"></a>Koddaki istek ve özel durum özetlerini görüntüleyin
Kod odağında her bir işleyici yönteminin, istekleri ve Application Insights tarafından son 24 saatte günlüğe kaydedilen özel durumların sayısını görürsünüz.

![Özel durum yığın izlemesi](./media/visual-studio/21.png)

> [!NOTE] 
> Kod Odağı, Application Insights verilerini yalnızca [uygulamanızı Application Insights portalına telemetri gönderecek şekilde yapılandırdıysanız](../../azure-monitor/app/asp-net.md) gösterir.
>

[Kod Odağı’nda Application Insights hakkında daha fazla bilgi](../../azure-monitor/app/visual-studio-codelens.md)

## <a name="trends"></a>Eğilimler
Eğilimler, uygulamanızın zaman içinde nasıl davrandığını görselleştirmeye yönelik bir araçtır. 

Application Insights araç çubuğu düğmesinden veya Application Insights Arama penceresinden **Telemetri Eğilimlerini Keşfet**’i seçin. Başlamak için beş genel sorgudan birini seçin. Telemetri türleri, zaman aralıkları ve diğer özelliklere göre farklı veri kümelerini çözümleyebilirsiniz. 

Verilerinizdeki anormallikleri bulmak için "Görünüm Türü" açılır listesi altındaki anormallik seçeneklerinden birini belirleyin. Pencerenin altındaki filtreleme seçenekleri, telemetrinizdeki belirli alt kümelere odaklanmayı kolaylaştırır.

![Eğilimler](./media/visual-studio/51.png)

[Eğilimler hakkında daha fazla bilgi](../../azure-monitor/app/visual-studio-trends.md).

## <a name="local-monitoring"></a>Yerel izleme
(Visual Studio 2015 Update'ten 2) SDK'sı (yani izleme anahtarı Applicationınsights.config dosyasında), Application Insights portalına telemetri gönderecek şekilde yapılandırmadıysanız tanılama penceresinde en son hata ayıklama oturumunuzda telemetri görüntüler. 

Daha önce uygulamanızın önceki bir sürümünü yayımladıysanız bu iyi bir şeydir. Hata ayıklama oturumlarınızdan alınan telemetrinin, yayımlanan uygulamanın Application Insights portalındaki telemetriyle karışmasını istemezsiniz.

Telemetriyi portala göndermeden önce hatalarını ayıklamak istediğiniz [özel telemetri](../../azure-monitor/app/api-custom-events-metrics.md) verilerine sahip olmanız da yararlı olur.

* *Başlangıçta, Application Insights’ı portala telemetri gönderecek şekilde tam olarak yapılandırdım. Ancak, artık telemetriyi yalnızca Visual Studio'da görmek istiyorum.*
  
  * Arama penceresinin Ayarlar bölümünde, uygulamanız portala telemetri gönderiyor olsa bile yerel tanılamalarda arama seçeneği vardır.
  * Portala telemetri gönderimini durdurmak için Applicationınsights.config’de `<instrumentationkey>...` satırını açıklama satırına dönüştürün. Telemetriyi yeniden portala göndermeye hazır olduğunuzda açıklamayı kaldırın.


## <a name="next-steps"></a>Sonraki adımlar
|  |  |
| --- | --- |
| **[Daha fazla veri ekleme](../../azure-monitor/app/asp-net-more.md)**<br/>Kullanımı, kullanılabilirliği, bağımlılıkları, özel durumları izleyin. Günlük altyapılarından izlemeleri tümleştirin. Özel telemetri yazın. |![Visual studio](./media/visual-studio/64.png) |
| **[Application Insights portalıyla çalışma](../../azure-monitor/app/overview-dashboard.md)**<br/>Panolar, güçlü tanılama ve analiz araçları, uyarılar, Canlı bağımlılık haritası, uygulama ve dışarı aktarılan telemetri verileri görüntüleyin. |![Visual studio](./media/visual-studio/62.png) |

