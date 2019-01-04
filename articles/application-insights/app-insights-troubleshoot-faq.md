---
title: Azure Application Insights ile ilgili SSS | Microsoft Docs
description: Sık sorulan Application Insights hakkında sorular.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: mbullwin
ms.openlocfilehash: 5cd720225144a34163f8d4802b63aca6a439e2c7
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017676"
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Sık Sorulan Sorular

## <a name="configuration-problems"></a>Yapılandırma sorunları
*Ayarlama konusunda sorun yaşıyorum my:*

* [.NET uygulaması](../azure-monitor/app/asp-net-troubleshoot-no-data.md)
* [Zaten çalışan bir uygulamayı izleme](../azure-monitor/app/monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure tanılama](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Java web uygulaması](../azure-monitor/app/java-troubleshoot.md)

*Benim sunucumdan veri alabilirim*

* [Küme güvenlik duvarı özel durumları](../azure-monitor/app/ip-addresses.md)
* [Bir ASP.NET sunucusu ayarlama](../azure-monitor/app/monitor-performance-live-website-now.md)
* [Bir Java sunucusu ayarlama](../azure-monitor/app/java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Application Insights ile kullanabilir miyim...?

* [Web uygulamaları - şirket içi bir IIS sunucusunda veya VM](../azure-monitor/app/asp-net.md)
* [Java web uygulamaları](../azure-monitor/app/java-get-started.md)
* [Node.js uygulamaları](../azure-monitor/app/nodejs.md)
* [Azure'da Web uygulamaları](../azure-monitor/app/azure-web-apps.md)
* [Azure'da bulut Hizmetleri](../azure-monitor/app/cloudservices.md)
* [Docker'ı çalıştıran uygulama sunucuları](../azure-monitor/app/docker.md)
* [Tek sayfa web uygulamaları](../azure-monitor/app/javascript.md)
* [SharePoint](app-insights-sharepoint.md)
* [Windows masaüstü uygulaması](app-insights-windows-desktop.md)
* [Diğer platformlar](../azure-monitor/app/platforms.md)

## <a name="is-it-free"></a>Ücretsiz mi?

Evet, Deneysel için kullanın. Temel fiyatlandırma planı, uygulamanızın bir belirli kullanım hakkı her ay ücretsiz olarak gönderebilir. Ücretsiz Kullanım Hakkı kapak geliştirme ve az sayıda kullanıcı için bir uygulama yayımlama için yeteri kadar büyük. Belirtilen veri miktarından fazla işlenmekte olan önlemek için üst sınır ayarlayabilirsiniz.

Daha büyük miktarda telemetri Gb ile ücretlendirilir. Nasıl yapılır ilgili bazı ipuçları sağlıyoruz [ücretlerinizin sınırlamak](../azure-monitor/app/pricing.md).

Kurumsal plan, her web sunucusu düğümüne telemetri gönderen her gün için bir ücret doğurur. Büyük ölçekte sürekli dışarı aktarma'yı kullanmak istiyorsanız uygundur.

[Fiyatlandırma planı okuma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Bunun ne kadar maliyetlendirme?

* Açık **kullanım ve Tahmini maliyetler sayfasını** içinde Application Insights kaynağı. Bir son kullanım grafiğini yoktur. İsterseniz, bir veri hacmi üst sınırı ayarlayabilirsiniz.
* Açık [Azure faturalama dikey](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) faturalarınızı tüm kaynaklar arasında görmek için.

## <a name="q14"></a>Hangi, Application Insights'ı Projemde değiştirmez?
Ayrıntılar projenin türüne bağlıdır. Bir web uygulaması için:

* Bu dosyalar, projenize ekler:

  * Applicationınsights.config.
  * Ai.js
* Bu NuGet paketlerini yükler:

  * *Application Insights API* -core API'si
  * *Web uygulamaları için Application Insights API* - sunucudan telemetri göndermek için kullanılır
  * *JavaScript uygulamaları için Application Insights API* - istemciden telemetri göndermek için kullanılır

    Paketler Bu bütünleştirilmiş kodları şunlardır:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Öğeleri ekler:

  * Web.config
  * Packages.config
* (Yeni, yalnızca - projelerin, [var olan bir projeye Application Insights ekleme][start], bu el ile yapmanız gerekir.) Application Insights kaynak kimliği ile bunları başlatmak için istemci ve sunucu kod parçacıkları ekler Örneğin, bir MVC uygulamasında kodu ana sayfaya Views/Shared/_Layout.cshtml eklenir

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Daha eski SDK sürümlerinden nasıl yükseltebilirim?
Bkz: [sürüm notları](../azure-monitor/app/release-notes.md) uygulama türüne uygun SDK'sı.

## <a name="update"></a>Hangi Azure kaynak Projem için veri gönderen nasıl değiştirebilirim?
Çözüm Gezgini'nde sağ `ApplicationInsights.config` ve **güncelleştirme Application Insights**. Azure'da mevcut veya yeni bir kaynak veri gönderebilir. Güncelleştirme Sihirbazı'nı izleme anahtarını sunucu SDK'sı verilerinizi göndereceği yeri belirler applicationınsights.config dosyasını değiştirir. "Tümünü Güncelleştir" kaldırmadıysanız, web sayfaları'nda göründüğü anahtarı da değişecektir.

## <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuza Application Insights web apps'te yapılandırmak için kullanabileceğiniz bir masaüstü uygulaması. Telemetri toplama yoktur: uygulama yapılandırırken değil durdurabilirsiniz. 

[Daha fazla bilgi edinin](../azure-monitor/app/monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Application Insights tarafından hangi telemetri toplanır?

Sunucu web uygulamaları:

* HTTP istekleri
* [Bağımlılıkları](../azure-monitor/app/asp-net-dependencies.md). Çağrı: SQL veritabanı; Dış hizmetler için HTTP çağrıları; Azure Cosmos DB, tablo, blob depolama ve kuyruk. 
* [Özel durumlar](../azure-monitor/app/asp-net-exceptions.md) ve Yığın izlemeleri.
* [Performans sayaçları](../azure-monitor/app/performance-counters.md) - kullanırsanız [Durum İzleyicisi](../azure-monitor/app/monitor-performance-live-website-now.md), [Azure izleme](../azure-monitor/app/azure-web-apps.md), veya [Application Insights toplanan yazıcı](../azure-monitor/app/java-collectd.md).
* [Özel olaylar ve ölçümler](../azure-monitor/app/api-custom-events-metrics.md) , kod.
* [İzleme günlükleri](../azure-monitor/app/asp-net-trace-logs.md) uygun Toplayıcı yapılandırırsanız.

Gelen [istemci web sayfaları](../azure-monitor/app/javascript.md):

* [Sayfa görüntüleme sayıları](app-insights-usage-overview.md)
* [AJAX çağrıları](../azure-monitor/app/asp-net-dependencies.md) istekte bir çalışan komut dosyasından.
* Sayfa görüntüleme yükleme verileri
* Kullanıcı ve oturum sayıları
* [Kimliği doğrulanmış kullanıcı kimlikleri](../azure-monitor/app/api-custom-events-metrics.md#authenticated-users)

Yapılandırmadan, diğer kaynaklardan:

* [Azure tanılama](../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Analiz için içeri aktarma](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api)
* [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api)
* [Logstash](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-collector-api)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Filtreleme veya miyim bazı telemetri Değiştir?

Evet, sunucuda şunu yazabilirsiniz:

* Filtre veya uygulamanızdan gönderilmeden önce özellikleri seçili telemetri öğelerini eklemek için telemetri işlemcisi'ni kullanın.
* Tüm telemetri öğeleri için özellikler eklemek için telemetri başlatıcısını.

Daha fazla bilgi edinin [ASP.NET](../azure-monitor/app/api-filtering-sampling.md) veya [Java](../azure-monitor/app/java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Şehir, ülke ve diğer coğrafi konum verilerini nasıl hesaplanır?

IP adresini kullanarak web istemcisi (IPv4 veya IPv6) baktığımızda [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Tarayıcı telemetrisi: Gönderenin IP adresi topluyoruz.
* Sunucu telemetri: Application Insights modülü için istemci IP adresi toplar. Varsa toplanmaz `X-Forwarded-For` ayarlanır.

Yapılandırabileceğiniz `ClientIpHeaderTelemetryInitializer` farklı bir üst bilgisinden IP adresini almak için. Bazı sistemlerde, örneğin, bir proxy tarafından taşınması, yük dengeleyici veya CDN `X-Originating-IP`. [Daha fazla bilgi edinin](https://apmtips.com/blog/2016/07/05/client-ip-address/).

Yapabilecekleriniz [Power BI](app-insights-export-power-bi.md) isteği telemetrinizi bir haritada görüntülemek için.


## <a name="data"></a>Veri, portalda ne kadar süreyle tutulduğunu? Güvenli mi?
Bir göz atın [veri saklama ve gizlilik][data].

## <a name="could-personal-data-be-sent-in-the-telemetry"></a>Kişisel verileri telemetri gönderilebilir mi?

Bu, kodunuzu tür veri gönderiyorsa mümkündür. Yığın izlemelerini değişkenlerinde kişisel veri içeriyorsa de oluşabilir. Geliştirme ekibiniz, kişisel verileri doğru şekilde işlediğinden emin olmak için risk değerlendirmeleri yürütmeniz gerekir. [Veri saklama ve gizlilik hakkında daha fazla bilgi edinin](../azure-monitor/app/data-retention-privacy.md).

**Tüm** coğrafi konum özniteliklerini aranır sonra istemci web adresinin sekizlik tabanda 0 olarak her zaman ayarlanmış.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>My iKey my web sayfası kaynağında görülebilir. 

* Bu izleme çözümleri, genel yöntemdir.
* Verilerinizi çalmak için kullanılamaz.
* Veri veya tetikleyici uyarılarınızı eğriltmek için kullanılabilir.
* Tüm müşteriler bu tür sorunları oluşmuş kullanıcılarımız değil.

Şunları yapabilirsiniz:

* (Application Insights kaynakları ayırın) iki ayrı İkey'leri istemci ve sunucu verileri için kullanın. Veya
* Çalıştıran sunucunuzda bir ara sunucu yazabilir ve proxy üzerinden verileri göndermek web istemcisi vardır.

## <a name="post"></a>Gönderme verisi tanılama aramasındaki nasıl görebilirim?
Gönderme verisi otomatik olarak oturum yok, ancak bir TrackTrace çağrı kullanabilirsiniz: ileti parametreyi verileri yerleştirin. Bu, üzerinde filtre uygulanamıyor rağmen bir uzun dize özellikleri barındırabileceğiniz boyut sınırını sahiptir.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Tek veya birden fazla Application Insights kaynaklarını kullanmalı mıyım?

Tek bir kaynak tüm bileşenleri veya rolleri için tek bir iş sisteminde kullanın. Bağımsız uygulamaların yanı sıra, geliştirme, test ve yayın sürümleri için ayrı kaynaklar kullanın.

* [Buraya bakın](app-insights-separate-resources.md)
* [Örnek - worker ve web rolleri ile bulut hizmeti](../azure-monitor/app/cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>İzleme anahtarını dinamik olarak nasıl değiştirebilirim?

* [Tartışması](app-insights-separate-resources.md)
* [Örnek - worker ve web rolleri ile bulut hizmeti](../azure-monitor/app/cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Kullanıcı ve oturum nelerdir sayılır?

* JavaScript SDK'sı, etkinlikleri gruplandırmak için gelen kullanıcıları tanımlamak için web istemcisi, bir kullanıcı tanımlama bilgisinde ve oturum tanımlama bilgisini ayarlar.
* İstemci tarafı komut dosyası varsa, [sunucu tanımlama bilgilerini ayarlama](https://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Farklı tarayıcılar veya içinde InPrivate/gizli Tarama modunda sitenizdeki bir gerçek kullanıcı kullanır veya farklı makineler sonra birden çok kez sayılır olur.
* Makineleri ve tarayıcılar arasında bir oturum açma kullanıcı tanımlamak için bir çağrı ekleyin [setAuthenticatedUserContext()](../azure-monitor/app/api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a> Application Insights içinde her şeyi etkinleştirdiniz mi?
| Görmeniz gereken | Bunu alma | İstediğiniz neden |
| --- | --- | --- |
| Kullanılabilirlik grafikleri |[Web testleri](../azure-monitor/app/monitor-web-app-availability.md) |Web uygulamanız çalışıyor bildirin |
| Server app perf: yanıt süreleri... |[Projenize Application Insights ekleme](../azure-monitor/app/asp-net.md) veya [sunucuda AI Durum İzleyicisi yükleyin](../azure-monitor/app/monitor-performance-live-website-now.md) (veya kendi kodunuzu yazma [bağımlılıkları izlemek](../azure-monitor/app/api-custom-events-metrics.md#trackdependency)) |Performans sorunları tespit edin |
| Bağımlılık telemetrisi |[Sunucu üzerinde yapay ZEKA Durum İzleyicisi'ni yükleyin](../azure-monitor/app/monitor-performance-live-website-now.md) |Veritabanları veya diğer dış bileşenlere sorunları tanılayın |
| Özel durumlardan yığın izlemelerini alın |[Kodunuzda TrackException çağrıları eklemenize](../azure-monitor/app/asp-net-exceptions.md) (ancak bazı otomatik olarak raporlanır) |Algılama ve özel durumları tanılama |
| Günlük izlemelerini arama |[Günlüğe kaydetme bağdaştırıcı ekleme](../azure-monitor/app/asp-net-trace-logs.md) |Özel durumlar, performans sorunlarını tanılayın |
| İstemci kullanım temelleri: sayfa görüntülemeleri, oturumlar... |[Web sayfalarında JavaScript Başlatıcı](../azure-monitor/app/javascript.md) |Kullanım analizi |
| İstemci özel ölçümler |[Web sayfaları'nda izleme çağrıları](../azure-monitor/app/api-custom-events-metrics.md) |Kullanıcı deneyimini geliştirin |
| Sunucu özel ölçümler |[Sunucu izleme çağrıları](../azure-monitor/app/api-custom-events-metrics.md) |İş zekası |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Neden arama ve ölçümler grafiklerde sayıları eşit?

[Örnekleme](../azure-monitor/app/sampling.md) gerçekten portala uygulamanızdan gönderilen telemetri öğelerinin (istekler, özel olaylar vb.) sayısını azaltır. Aramada, gerçekte alınan öğe sayısını görürsünüz. Olayların sayısını görüntüleyen ölçüm grafiklerde oluştu özgün olay sayısını görürsünüz. 

Her öğe taşıyan iletilen bir `itemCount` kaç tane özgün olay öğeyi gösteren bir özelliği temsil eder. Örnekleme işleminde gözlemlemek için Analytics'te bu sorguyu çalıştırabilirsiniz:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Otomasyon

### <a name="configuring-application-insights"></a>Application Insights'ı yapılandırma

Yapabilecekleriniz [PowerShell betikleri yazma](../azure-monitor/app/powershell.md) için Azure Kaynak İzleyicisi'ni kullanma:

* Oluşturun ve Application Insights kaynaklarını güncelleştirin.
* Fiyatlandırma planı ayarlayın.
* İzleme anahtarını alın.
* Ölçüm uyarısı Ekle.
* Kullanılabilirlik testi ekleyin.

Ölçüm Gezgini'nde raporu ayarlayamıyor veya sürekli dışarı aktarmayı ayarlayın.

### <a name="querying-the-telemetry"></a>Telemetri sorgulama

Kullanım [REST API](https://dev.applicationinsights.io/) çalıştırılacak [Analytics](../azure-monitor/app/analytics.md) sorgular.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Bir olay hakkında bir uyarı nasıl ayarlayabilirim?

Ölçümleri yalnızca Azure uyarılardır. Değeri eşiği aştığında, olay her gerçekleştiğinde, özel bir ölçü oluşturun. Bir uyarı ölçüme göre ayarlayın. Dikkat: ölçüm her iki yönde; eşikle kesiştiğinde bir bildirim alırsınız. Başlangıç değeri yüksek veya düşük olmasına ne olursa olsun ilk kesen kadar bildirim alamazsınız; her zaman birkaç dakikalık bir gecikme yoktur.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Bir Azure web uygulaması ve Application Insights arasında veri aktarımı ücreti var mıdır?

* Azure web uygulamanıza bir veri merkezinde barındırılan, bir Application Insights toplama bitiş noktası olduğunda, ücret alınmaz. 
* Konak veri Merkezinize hiç toplama bitiş noktası yoktur sonra uygulamanızın telemetri ödenmesini [Azure ücretleri giden](https://azure.microsoft.com/pricing/details/bandwidth/).

Bu işlem, Application Insights kaynağınız burada barındırılan bağımlı değildir. Yalnızca bizim uç dağıtımına da bağlıdır.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Application ınsights'ı portala telemetri gönderebilir miyim?

SDK'larımızda kullanın ve kullanmak öneririz [SDK API'si](../azure-monitor/app/api-custom-events-metrics.md). Çeşitli için SDK çeşitlerini vardır [platformları](../azure-monitor/app/platforms.md). Bu SDK'ları, arabelleğe alma, sıkıştırma, azaltma, yeniden denemeler ve benzeri işler. Ancak, [alımı şema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) ve [uç nokta Protokolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) ortaktır.

## <a name="can-i-monitor-an-intranet-web-server"></a>Bir intranet web sunucusu izleyebilirim?

İki yöntem aşağıda verilmiştir:

### <a name="firewall-door"></a>Güvenlik Duvarı kapısı

Web sunucunuza bizim uç noktalarına telemetri göndermesine izin https://dc.services.visualstudio.com:443 ve https://rt.services.visualstudio.com:443. 

### <a name="proxy"></a>Ara sunucu

Sunucunuza giden trafik bir ağ geçidi, bu ayarlar örnekte Applicationınsights.config üzerine yazarak intranetinizde yol. Bu "Bitiş" özellikleri, yapılandırmada mevcut değilse, aşağıdaki örnekte gösterilen varsayılan değerler bu sınıfların kullanacaklardır.

#### <a name="example-applicationinsightsconfig"></a>Örnek Applicationınsights.config:
```xml
<ApplicationInsights>
    ...
    <TelemetryChannel>
         <EndpointAddress>https://dc.services.visualstudio.com/v2/track</EndpointAddress>
    </TelemetryChannel>
    ...
    <ApplicationIdProvider Type="Microsoft.ApplicationInsights.Extensibility.Implementation.ApplicationId.ApplicationInsightsApplicationIdProvider, Microsoft.ApplicationInsights">
        <ProfileQueryEndpoint>https://dc.services.visualstudio.com/api/profiles/{0}/appId</ProfileQueryEndpoint>
    </ApplicationIdProvider>
    ...
</ApplicationInsights>
```

_Not ApplicationIdProvider v2.6.0 içinde itibaren kullanılabilir_

Ağ geçidi trafiği yönlendirmek https://dc.services.visualstudio.com:443

Yukarıdaki değerlerle değiştirin: `http://<your.gateway.address>/<relative path>`
 
Örnek: 
```
http://<your.gateway.endpoint>/v2/track 
http://<your.gateway.endpoint>/api/profiles/{0}/apiId
```

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Bir intranet sunucusunda kullanılabilirlik web testleri çalıştırabilir miyim?

Bizim [web testleri](../azure-monitor/app/monitor-web-app-availability.md) dünyanın dört bir yanında dağıtılan varlık noktaları çalıştırın. İki çözümü vardır:

* Güvenlik Duvarı kapı - sunucunuzdan istekleri olanak [web test aracıları uzun ve değiştirilebilir listesini](../azure-monitor/app/ip-addresses.md).
* Sunucunuzdan intranetinize içinde düzenli aralıklarla istekler göndermek için kendi kodunuzu yazın. Bu amaç için Visual Studio web testleri çalıştırabilir. Test edicinin TrackAvailability() API'sini kullanarak sonuçları Application Insights'a gönderebilir.

## <a name="how-long-does-it-take-for-telemetry-to-be-collected"></a>Ne kadar süre için toplanacak telemetri sürer?

Çoğu Application Insights verilerini 5 dakikadan kısa bir gecikme vardır. Bazı veriler, daha uzun sürebilir; genellikle daha büyük günlük dosyaları. Daha fazla bilgi için [Application Insights SLA](https://azure.microsoft.com/support/legal/sla/application-insights/v1_2/).

## <a name="more-answers"></a>Daha fazla soru yanıtı
* [Uygulama anlayışları Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: ../azure-monitor/app/data-retention-privacy.md
[platforms]: ../azure-monitor/app/platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
