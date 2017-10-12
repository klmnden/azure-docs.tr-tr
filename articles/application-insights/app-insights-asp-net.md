---
title: "Azure Application Insights ile ASP.NET Web uygulaması analizi ayarlama | Microsoft Docs"
description: "Şirket içi veya Azure’de barındırılan ASP.NET web siteniz için performans, kullanılabilirlik ve kullanım analizi yapılandırın."
services: application-insights
documentationcenter: .net
author: CFreemanwa
manager: carmonm
ms.assetid: d0eee3c0-b328-448f-8123-f478052751db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/15/2017
ms.author: bwren
ms.openlocfilehash: cb247ee68da88265f7c51258644064463d44f8b5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-application-insights-for-your-aspnet-website"></a>ASP.NET web siteniz için Application Insights'ı ayarlama

Bu yordam ASP.NET web uygulamanızı [Azure Application Insights](app-insights-overview.md) hizmetine telemetri gönderecek şekilde yapılandırır. Kendi IIS sunucunuzda veya Bulut’ta barındırılan ASP.NET uygulamaları için çalışır. Uygulamanızın performansını ve nasıl kullanıldığını anlamanıza yardımcı olan grafikler ve güçlü bir sorgu dilinin yanı sıra hata ya da performans sorunları hakkında otomatik uyarılar alırsınız. Çoğu geliştirici, özellikleri bu haliyle mükemmel bulsa da, gerekirse telemetriyi genişletip özelleştirebilirsiniz.

Visual Studio'da kurulum yalnızca birkaç tıklama ile yapılır. Telemetri hacmini sınırlayarak ücret doğmamasını sağlayabilirsiniz. Bunun yapılması, çok fazla kullanıcı olmadan bir siteyi deneyip hatalarını ayıklamanıza veya izlemenize olanak tanır. Devam edip üretim merkezinizi izlemeye karar verdiğinizde, sınırı daha sonra kolayca artırabilirsiniz.

## <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* Visual Studio 2013 güncelleştirme 3 veya sonraki bir sürümü. Ne kadar yeniyse o kadar iyidir.
* Bir [Microsoft Azure](http://azure.com) aboneliği. Takımınızın veya kuruluşunuzun Azure aboneliği varsa, abonelik sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi aboneliğe ekleyebilir.

İlginizi çekiyorsa inceleyebileceğiniz alternatif konu başlıkları da mevcuttur:

* [Çalışma zamanında bir web uygulamasını izleme](app-insights-monitor-performance-live-website-now.md)
* [Azure Cloud Services](app-insights-cloudservices.md)

## <a name="ide"></a> 1. Adım: Application Insights SDK’yı ekleme

Çözüm Gezgini'nde web uygulaması projenize sağ tıklayın ve **Ekle** > **, Application Insights Telemetrisi...**'ni veya **Application Insights'ı Yapılandır**'ı seçin.

![Application Insights Telemetrisi Ekle seçeneğinin vurgulandığı Çözüm Gezgini’nin ekran görüntüsü](./media/app-insights-asp-net/appinsights-03-addExisting.png)

(Visual Studio 2015'te Yeni Proje iletişim kutusunda da Application Insights ekleme seçeneği mevcuttur.)

Application Insights yapılandırma sayfasına gidin:

![Uygulamanızı Application Insights'a kaydedin sayfasının ekran görüntüsü](./media/app-insights-asp-net/visual-studio-register-dialog.png)

**a.** Azure'a erişmek için kullandığınız hesabı ve aboneliği seçin.

**b.** Uygulamanızdan gelen verileri görmek istediğiniz Azure kaynağını seçin. Genellikle:

* Tek bir uygulamanın [farklı bileşenleri için tek kaynak](app-insights-monitor-multi-role-apps.md) kullanın. 
* İlişkisiz uygulamalar için ayrı kaynaklar oluşturun.
 
Verilerin depolandığı kaynak grubunu veya konumu ayarlamak isterseniz **Ayarları yapılandır**'a tıklayın. Kaynak grupları, verilere erişimi denetlemek için kullanılır. Örneğin aynı sistemin parçalarını oluşturan birden uygulamanız varsa bunların Application Insights verilerini aynı kaynak grubuna ekleyebilirsiniz.

**c.** Ücret yansımaması için ücretsiz veri hacmi sınırında bir sınır ayarlayın. Application Insights, belirli bir telemetri hacmine kadar ücretsizdir. Kaynak oluşturulduktan sonra portalda **Özellikler + fiyatlandırma** > **Veri hacmi yönetimi** > **Günlük hacim sınırı** sayfasından seçiminizi değiştirebilirsiniz.

**d.** Devam etmek ve web uygulamanızda Application Insights'ı yapılandırmak için **Kaydol**'a tıklayın. Telemetri hem hata ayıklama sırasında hem de uygulamanızı yayımladıktan sonra [Azure portalına](https://portal.azure.com) gönderilir.

**e.** Hata ayıklama sırasında portala telemetri göndermek istemiyorsanız, uygulamanıza Application Insights SDK’sını ekleyin, ancak portalda bir kaynak yapılandırmayın. Hata ayıklama sırasında telemetri verilerini Visual Studio'da görebilirsiniz. Daha sonra bu yapılandırma sayfasına dönebilir veya uygulamanızı dağıtana kadar bekleyip [telemetriyi çalışma zamanında açabilirsiniz](app-insights-monitor-performance-live-website-now.md).


## <a name="run"></a> 2. Adım: Uygulamanızı çalıştırma
F5 tuşuna basarak uygulamanızı çalıştırın. Farklı sayfalar açarak telemetri verileri oluşturun.

Visual Studio'da, günlüğe kaydedilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’nun ekran görüntüsü. Hata ayıklama sırasında Application Insights düğmesi görünür.](./media/app-insights-asp-net/54.png)

## <a name="step-3-see-your-telemetry"></a>Adım 3: Telemetrinize bakma
Visual Studio’da veya Application Insights web portalında telemetrinizi görebilirsiniz. Uygulamanızın hatalarını ayıklamanıza yardımcı olması için Visual Studio'da telemetri arayın. Sisteminiz canlıyken web portalında performans ve kullanımı izleyin. 

### <a name="see-your-telemetry-in-visual-studio"></a>Visual Studio'da telemetrinize bakma

Visual Studio’da Application Insights penceresini açın. **Application Insights** düğmesine tıklayın veya Çözüm Gezgini’nden projenize sağ tıklayıp **Application Insights**’ı seçin ve ardından **Canlı Telemetride Ara**’ya tıklayın.

Visual Studio Application Insights Arama penceresinde, uygulamanızın sunucu tarafında oluşturulan telemetri için **Hata Ayıklama oturumundan alınan veriler** görünümüne bakın. Filtrelerle denemeler yapın ve daha fazla ayrıntı için herhangi bir etkinliğe tıklayın.

![Application Insights penceresindeki Hata ayıklama oturumundan alınan veriler görünümünün ekran görüntüsü.](./media/app-insights-asp-net/55.png)

> [!NOTE]
> Herhangi bir veri gösterilmiyorsa zaman aralığının doğru olduğundan emin olup Ara simgesine tıklayın.

[Visual Studio’daki Application Insights araçları hakkında daha fazla bilgi edinin](app-insights-visual-studio.md).

<a name="monitor"></a>
### <a name="see-telemetry-in-web-portal"></a>Web portalında telemetriye bakma

Yalnızca SDK’yı yüklemeyi seçmediyseniz telemetriyi Application Insights web portalında da görüntüleyebilirsiniz. Portalda, Visual Studio’ya kıyasla daha çok grafik, analiz aracı ve bileşenler arası görünüm bulunur. Portal ayrıca uyarılar sağlar.

Application Insights kaynağınızı açın. [Azure portalında](https://portal.azure.com/) oturum açıp portalda bulun ya da Visual Studio’da projeye sağ tıklayıp sizi yönlendirmesini sağlayın.

![Visual Studio’da Application Insights portalının nasıl açılacağını gösteren ekran görüntüsü](./media/app-insights-asp-net/appinsights-04-openPortal.png)

> [!NOTE]
> Bir erişim hatası alırsanız: Birden çok Microsoft kimlik bilgisi kümeniz mi var ve yanlış küme ile mi oturum açtınız? Portalda oturumunuzu kapatıp yeniden oturum açın.

Portal, uygulamanızdan alınan telemetri görünümünde açılır.

![Application Insights’a genel bakış sayfasının ekran görüntüsü](./media/app-insights-asp-net/66.png)

Daha fazla ayrıntı görmek için portalda istediğiniz kutucuğa veya grafiğe tıklayın.

[Azure portalında Application Insights kullanma hakkında daha fazla bilgi edinin](app-insights-dashboards.md).

## <a name="step-4-publish-your-app"></a>4. Adım: Uygulamanızı yayımlama
Uygulamanızı IIS sunucunuza veya Azure’a yayımlayın. Her şeyin sorunsuz çalıştığından emin olmak için [Canlı Ölçümler Akışı](app-insights-metrics-explorer.md#live-metrics-stream)’nı izleyin.

Telemetriniz Application Insights portalında biriktirilir ve burada ölçümlerinizi izleyebilir, telemetrinizde arama yapabilir ve [panolar](app-insights-dashboards.md) ayarlayabilirsiniz. Ayrıca, güçlü [Log Analytics sorgu dili](https://docs.loganalytics.io/)’ni kullanarak kullanımı ve performansı analiz edebilir ya da belirli olayları bulabilirsiniz.

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
[SDK’nın yeni bir sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases) yükseltmek için **NuGet paket yöneticisini** yeniden açıp yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.Web**’i seçip **Yükselt**’i seçin.

ApplicationInsights.config’de herhangi bir özelleştirme gerçekleştirdiyseniz yükseltmeden önce dosyanın bir kopyasını kaydedin. Daha sonra, yaptığınız değişiklikleri yeni sürümle birleştirin.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar

### <a name="more-telemetry"></a>Daha fazla telemetri

* **[Tarayıcı ve sayfa yükleme verileri](app-insights-javascript.md)** - Web sayfalarınıza bir kod parçacığı ekleyin.
* **[Daha ayrıntılı bağımlılık ve özel durum izlemesi alın](app-insights-monitor-performance-live-website-now.md)** - Sunucunuza Durum İzleyicisi yükleyin.
* Kullanıcı eylemlerini saymak, zamanlamak veya ölçmek için **[özel olaylar kodlayın](app-insights-api-custom-events-metrics.md)**.
* **[Günlük verilerini alma](app-insights-asp-net-trace-logs.md)** - Günlük verilerini telemetrinizle ilişkilendirin.

### <a name="analysis"></a>Analiz

* **[Visual Studio’da Application Insights ile çalışma](app-insights-visual-studio.md)**<br/>Telemetri, tanılama araması ve kodun detayına gitme ile hata ayıklama hakkında bilgi içerir.
* **[Application Insights portalıyla çalışma](app-insights-dashboards.md)**<br/> Panolar, güçlü tanılama ve analiz araçları, uyarılar, uygulamanızın canlı bağımlılık haritası ve telemetriyi dışarı aktarma hakkında bilgi içerir.
* **[Analytics](app-insights-analytics-tour.md)** - Güçlü sorgu dili.

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](app-insights-monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](app-insights-proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](app-insights-alerts.md): Bir metrik tarafından herhangi bir eşiğin aşılması durumunda uyarı almak için bunları ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.

### <a name="automation"></a>Otomasyon

* [Application Insights kaynağı oluşturmayı otomatikleştirme](app-insights-powershell.md)
