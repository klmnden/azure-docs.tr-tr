---
title: Azure Application Insights ile ASP.NET Web uygulaması analizi ayarlama | Microsoft Docs
description: Performans, kullanılabilirlik ve kullanıcı davranış analizi araçları, ASP.NET Web sitesi için yapılandırma şirket içinde barındırılan veya Azure'da.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: mbullwin
ms.openlocfilehash: 498633d9f73c4a9b669ddd3469b62c01d2a19397
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958721"
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>ASP.NET web siteniz için Application Insights'ı ayarlama

Bu yordam ASP.NET web uygulamanızı [Azure Application Insights](app-insights-overview.md) hizmetine telemetri gönderecek şekilde yapılandırır. Kendi IIS sunucunuzda şirket içi olarak veya Bulut’ta barındırılan ASP.NET uygulamaları için çalışır. Uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olan grafikler ve güçlü bir sorgu dilinin yanı sıra hata ya da performans sorunları hakkında otomatik uyarılar alırsınız. Çoğu geliştirici, özellikleri bu haliyle mükemmel bulsa da, gerekirse telemetriyi genişletip özelleştirebilirsiniz.

Visual Studio'da kurulum yalnızca birkaç tıklama ile yapılır. Telemetri hacmini sınırlayarak ücret doğmamasını sağlayabilirsiniz. Bunun yapılması, çok fazla kullanıcı olmadan bir siteyi deneyip hatalarını ayıklamanıza veya izlemenize olanak tanır. Devam edip üretim merkezinizi izlemeye karar verdiğinizde, sınırı daha sonra kolayca artırabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar
Application Insights’ı ASP.NET web sitenize eklemek için şunu yapmanız gerekir:

- [Windows için Visual Studio 2017](https://www.visualstudio.com/downloads/)’yi aşağıdaki iş yükleri ile yükleyin:
    - ASP.NET ve web geliştirme
    - Azure geliştirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="ide"></a> 1. Adım: Application Insights SDK’yı ekleme

> [!IMPORTANT]
> Application Insights ekleme işlemi, ASP.NET şablon türüne göre değişiklik gösterir. **Boş** bir şablon veya **Azure Mobil Uygulaması** şablonu kullanıyorsanız **Proje** > **Application Insights Telemetri Ekleme** seçeneğini belirleyin. Diğer tüm ASP.NET şablonları için aşağıdaki yönergelere başvurun. 

Çözüm Gezgini’nde web uygulamanızın adına sağ tıklayın ve **Application Insights’ı Yapılandır** seçeneğini belirleyin

![Application Insights’ı Yapılandır seçeneğinin vurgulandığı Çözüm Gezgini’nin ekran görüntüsü](./media/app-insights-asp-net/0001-configure-application-insights.png)

(Application Insights SDK sürümünüze bağlı olarak, son SDK sürümüne güncelleştirme yapmanız istenebilir. İstenirse **SDK’yı güncelleştir**’i seçin.)

![Ekran görüntüsü: Yeni bir Microsoft Application Insights SDK sürümü mevcut. SDK’yı güncelleştir seçeneğinin vurgulandığı görüntü](./media/app-insights-asp-net/0002-update-sdk.png)

Application Insights Yapılandırması ekranı:

**Ücretsiz Olarak Başla**’yı seçin.

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/app-insights-asp-net/0004-start-free.png)

Verilerin depolandığı kaynak grubunu veya konumu ayarlamak isterseniz **Ayarları yapılandır**'a tıklayın. Kaynak grupları, verilere erişimi denetlemek için kullanılır. Örneğin aynı sistemin parçalarını oluşturan birden uygulamanız varsa bunların Application Insights verilerini aynı kaynak grubuna ekleyebilirsiniz.

 **Kaydol**’u seçin. 

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/app-insights-asp-net/0005-register-ed.png)

 Telemetri hem hata ayıklama sırasında hem de uygulamanızı yayımladıktan sonra [Azure portalına](https://portal.azure.com) gönderilir.
> [!NOTE]
> Hata ayıklama sırasında portala telemetri göndermek istemiyorsanız, uygulamanıza Application Insights SDK’sını ekleyin, ancak portalda bir kaynak yapılandırmayın. Hata ayıklama sırasında telemetri verilerini Visual Studio'da görebilirsiniz. Daha sonra bu yapılandırma sayfasına dönebilir veya uygulamanızı dağıtana kadar bekleyip [telemetriyi çalışma zamanında açabilirsiniz](app-insights-monitor-performance-live-website-now.md).

## <a name="run"></a> 2. Adım: Uygulamanızı çalıştırma
F5 tuşuna basarak uygulamanızı çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’nun ekran görüntüsü. Hata ayıklama sırasında Application Insights düğmesi görünür.](./media/app-insights-asp-net/0006-Events.png)

## <a name="step-3-see-your-telemetry"></a>Adım 3: Telemetrinize bakma
Visual Studio’da veya Application Insights web portalında telemetrinizi görebilirsiniz. Uygulamanızın hatalarını ayıklamanıza yardımcı olması için Visual Studio'da telemetri arayın. Sisteminiz canlıyken web portalında performans ve kullanımı izleyin. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visual Studio'da telemetrinize bakma

Visual Studio’da Application Insights verilerini görüntülemek için şunları yapın.  **Çözüm Gezgini** > **Bağlı Hizmetler**’i seçin > **Application Insights**’a sağ tıklayın ve ardından **Canlı Telemetri Ara**’ya tıklayın.

Visual Studio Application Insights Arama penceresinde, uygulamanızın sunucu tarafında oluşturulan telemetri için uygulamanızdan alınan veriler görünümü açılır. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

![Application Insights penceresindeki Hata ayıklama oturumundan alınan veriler görünümünün ekran görüntüsü.](./media/app-insights-asp-net/55.png)

> [!Tip]
> Herhangi bir veri gösterilmiyorsa zaman aralığının doğru olduğundan emin olup Ara simgesine tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Web portalında telemetriye bakma

Yalnızca SDK’yı yüklemeyi seçmediyseniz telemetriyi Application Insights web portalında da görüntüleyebilirsiniz. Portalda, Visual Studio’ya kıyasla daha çok grafik, analiz aracı ve bileşenler arası görünüm bulunur. Portal ayrıca uyarılar sağlar.

Application Insights kaynağınızı açın. [Azure portalında](https://portal.azure.com/) oturum açarak aradığınız öğeyi orada bulabilir veya **Çözüm Gezgini** > **Bağlı Hizmetler**’i seçip > **Application Insights** > **Application Insights Portal’ı aç**’a sağ tıklayarak sayfaya yönlendirilebilirsiniz.

Portal, uygulamanızdan alınan telemetri görünümünde açılır.

![Application Insights’a genel bakış sayfasının ekran görüntüsü](./media/app-insights-asp-net/66.png)

Daha fazla ayrıntı görmek için portalda istediğiniz kutucuğa veya grafiğe tıklayın.

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>4. Adım: Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetriniz Application Insights portalında biriktirilir ve burada ölçümlerinizi izleyebilir, telemetrinizde arama yapabilir ve [panolar](app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, güçlü [Log Analytics sorgu dili](https://aka.ms/LogAnalyticsLanguage)’ni kullanarak kullanımı ve performansı analiz edebilir ya da belirli olayları bulabilirsiniz.

Telemetrinizi tanılama araması ve [eğilimler](app-insights-visual-studio-trends.md) gibi araçlarla [Visual Studio](app-insights-visual-studio.md)’da analiz etmeye de devam edebilirsiniz.

> [!NOTE]
> Uygulamanız [azaltma sınırlarına](app-insights-pricing.md#limits-summary) yaklaşmak için yeterli telemetri gönderiyorsa, otomatik [örnekleme](app-insights-sampling.md) etkinleştirilir. Örnekleme, tanılama amaçlı bağlantı verilerini korurken uygulamanızdan gönderilen telemetri miktarını azaltır.
>
>

## <a name="land"></a> İşiniz tamamlandı

Tebrikler! Application Insights paketini uygulamanıza yüklediniz ve Azure üzerinde Application Insights hizmetine telemetri gönderecek şekilde yapılandırdınız.

![Telemetri hareketlerinin diyagramı](./media/app-insights-asp-net/01-scheme.png)

Uygulamanızın telemetrisini alan Azure kaynağı bir *izleme anahtarı* ile tanımlanır. Bu anahtarı ApplicationInsights.config dosyasında bulabilirsiniz.


## <a name="upgrade-to-future-sdk-versions"></a>Gelecek SDK sürümlerine yükseltme
[SDK’nın yeni bir sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltme yapmak için, **NuGet paket yöneticisini** açıp yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.Web**’i seçip **Yükselt**’i seçin.

ApplicationInsights.config’de herhangi bir özelleştirme gerçekleştirdiyseniz yükseltmeden önce dosyanın bir kopyasını kaydedin. Daha sonra, yaptığınız değişiklikleri yeni sürümle birleştirin.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

İlginizi çekiyorsa inceleyebileceğiniz alternatif konu başlıkları da mevcuttur:

* [Çalışma zamanında bir web uygulamasını izleme](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

### <a name="more-telemetry"></a>Daha fazla telemetri

* **[Tarayıcı ve sayfa yükleme verileri](app-insights-javascript.md)** - Web sayfalarınıza bir kod parçacığı ekleyin.
* **[Daha ayrıntılı bağımlılık ve özel durum izlemesi alın](app-insights-monitor-performance-live-website-now.md)** - Sunucunuza Durum İzleyicisi yükleyin.
* Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için **[özel olaylar kodlayın](app-insights-api-custom-events-metrics.md)**.
* **[Günlük verilerini alma](app-insights-asp-net-trace-logs.md)** - Günlük verilerini telemetrinizle ilişkilendirin.

### <a name="analysis"></a>Analiz

* **[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md)**<br/>Telemetri, tanılama araması ve kodun detayına gitme ile hata ayıklama hakkında bilgi içerir.
* **[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/> Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma hakkında bilgi içerir.
* **[Analytics](../log-analytics/query-language/get-started-analytics-portal.md)** - Güçlü sorgu dili.

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](app-insights-proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](app-insights-alerts.md): Bir metrik tarafından herhangi bir eşiğin aşılması durumunda uyarı almak için bunları ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

### <a name="automation"></a>Otomasyon

* [Application Insights kaynağı oluşturmayı otomatikleştirme](app-insights-powershell.md)
