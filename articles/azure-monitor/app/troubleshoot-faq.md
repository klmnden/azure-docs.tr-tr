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
ms.openlocfilehash: ab1327b42a76a6e76183d84cb1750cce8b85228f
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604271"
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Sık Sorulan Sorular

## <a name="configuration-problems"></a>Yapılandırma sorunları
*Ayarlama konusunda sorun yaşıyorum my:*

* [.NET uygulaması](asp-net-troubleshoot-no-data.md)
* [Zaten çalışan bir uygulamayı izleme](monitor-performance-live-website-now.md#troubleshoot)
* [Azure tanılama](../../azure-monitor/platform/diagnostics-extension-to-application-insights.md)
* [Java web uygulaması](java-troubleshoot.md)

*Benim sunucumdan veri alabilirim*

* [Küme güvenlik duvarı özel durumları](ip-addresses.md)
* [Bir ASP.NET sunucusu ayarlama](monitor-performance-live-website-now.md)
* [Bir Java sunucusu ayarlama](java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Application Insights ile kullanabilir miyim...?

* [Web uygulamaları - şirket içi bir IIS sunucusunda veya VM](asp-net.md)
* [Java web uygulamaları](java-get-started.md)
* [Node.js uygulamaları](nodejs.md)
* [Azure'da Web uygulamaları](azure-web-apps.md)
* [Azure'da bulut Hizmetleri](cloudservices.md)
* [Docker'ı çalıştıran uygulama sunucuları](docker.md)
* [Tek sayfa web uygulamaları](javascript.md)
* [SharePoint](sharepoint.md)
* [Windows masaüstü uygulaması](windows-desktop.md)
* [Diğer platformlar](platforms.md)

## <a name="is-it-free"></a>Ücretsiz mi?

Evet, Deneysel için kullanın. Temel fiyatlandırma planı, uygulamanızın bir belirli kullanım hakkı her ay ücretsiz olarak gönderebilir. Ücretsiz Kullanım Hakkı kapak geliştirme ve az sayıda kullanıcı için bir uygulama yayımlama için yeteri kadar büyük. Belirtilen veri miktarından fazla işlenmekte olan önlemek için üst sınır ayarlayabilirsiniz.

Daha büyük miktarda telemetri Gb ile ücretlendirilir. Nasıl yapılır ilgili bazı ipuçları sağlıyoruz [ücretlerinizin sınırlamak](pricing.md).

Kurumsal plan, her web sunucusu düğümüne telemetri gönderen her gün için bir ücret doğurur. Büyük ölçekte sürekli dışarı aktarma'yı kullanmak istiyorsanız uygundur.

[Fiyatlandırma planı okuma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Bunun ne kadar maliyetlendirme?

* Açık **kullanım ve Tahmini maliyetler sayfasını** içinde Application Insights kaynağı. Bir son kullanım grafiğini yoktur. İsterseniz, bir veri hacmi üst sınırı ayarlayabilirsiniz.
* Açık [Azure faturalama dikey](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) faturalarınızı tüm kaynaklar arasında görmek için.

## <a name="q14"></a>Hangi, Application Insights'ı Projemde değiştirmez?
Ayrıntılar projenin türüne bağlıdır. Bir web uygulaması için:

* Bu dosyalar, projenize ekler:

  * Applicationınsights.config.
  * ai.js
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
Bkz: [sürüm notları](release-notes.md) uygulama türüne uygun SDK'sı.

## <a name="update"></a>Hangi Azure kaynak Projem için veri gönderen nasıl değiştirebilirim?
Çözüm Gezgini'nde sağ `ApplicationInsights.config` ve **güncelleştirme Application Insights**. Azure'da mevcut veya yeni bir kaynak veri gönderebilir. Güncelleştirme Sihirbazı'nı izleme anahtarını sunucu SDK'sı verilerinizi göndereceği yeri belirler applicationınsights.config dosyasını değiştirir. "Tümünü Güncelleştir" kaldırmadıysanız, web sayfaları'nda göründüğü anahtarı da değişecektir.

## <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuza Application Insights web apps'te yapılandırmak için kullanabileceğiniz bir masaüstü uygulaması. Telemetri toplama yoktur: uygulama yapılandırırken değil durdurabilirsiniz. 

[Daha fazla bilgi edinin](monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Application Insights tarafından hangi telemetri toplanır?

Sunucu web uygulamaları:

* HTTP istekleri
* [Bağımlılıkları](asp-net-dependencies.md). Çağrı: SQL veritabanı; Dış hizmetler için HTTP çağrıları; Azure Cosmos DB, tablo, blob depolama ve kuyruk. 
* [Özel durumlar](asp-net-exceptions.md) ve Yığın izlemeleri.
* [Performans sayaçları](performance-counters.md) - kullanırsanız [Durum İzleyicisi](monitor-performance-live-website-now.md), [Azure izleme](azure-web-apps.md), veya [Application Insights toplanan yazıcı](java-collectd.md).
* [Özel olaylar ve ölçümler](api-custom-events-metrics.md) , kod.
* [İzleme günlükleri](asp-net-trace-logs.md) uygun Toplayıcı yapılandırırsanız.

Gelen [istemci web sayfaları](javascript.md):

* [Sayfa görüntüleme sayıları](usage-overview.md)
* [AJAX çağrıları](asp-net-dependencies.md) istekte bir çalışan komut dosyasından.
* Sayfa görüntüleme yükleme verileri
* Kullanıcı ve oturum sayıları
* [Kimliği doğrulanmış kullanıcı kimlikleri](api-custom-events-metrics.md#authenticated-users)

Yapılandırmadan, diğer kaynaklardan:

* [Azure tanılama](../platform/diagnostics-extension-to-application-insights.md)
* [Analiz için içeri aktarma](../platform/data-collector-api.md)
* [Log Analytics](../platform/data-collector-api.md)
* [Logstash](../platform/data-collector-api.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>Filtreleme veya miyim bazı telemetri Değiştir?

Evet, sunucuda şunu yazabilirsiniz:

* Filtre veya uygulamanızdan gönderilmeden önce özellikleri seçili telemetri öğelerini eklemek için telemetri işlemcisi'ni kullanın.
* Tüm telemetri öğeleri için özellikler eklemek için telemetri başlatıcısını.

Daha fazla bilgi edinin [ASP.NET](api-filtering-sampling.md) veya [Java](java-filter-telemetry.md).

## <a name="how-are-city-countryregion-and-other-geo-location-data-calculated"></a>Şehir, ülke/bölge ve diğer coğrafi konum verilerini nasıl hesaplanır?

IP adresini kullanarak web istemcisi (IPv4 veya IPv6) baktığımızda [GeoLite2](https://dev.maxmind.com/geoip/geoip2/geolite2/).

* Tarayıcı telemetrisi: Gönderenin IP adresi topluyoruz.
* Sunucu telemetri: Application Insights modülü için istemci IP adresi toplar. Varsa toplanmaz `X-Forwarded-For` ayarlanır.

Yapılandırabileceğiniz `ClientIpHeaderTelemetryInitializer` farklı bir üst bilgisinden IP adresini almak için. Bazı sistemlerde, örneğin, bir proxy tarafından taşınması, yük dengeleyici veya CDN `X-Originating-IP`. [Daha fazla bilgi edinin](https://apmtips.com/blog/2016/07/05/client-ip-address/).

Yapabilecekleriniz [Power BI](export-power-bi.md ) isteği telemetrinizi bir haritada görüntülemek için.


## <a name="data"></a>Veri, portalda ne kadar süreyle tutulduğunu? Güvenli mi?
Bir göz atın [veri saklama ve gizlilik][data].

## <a name="could-personal-data-be-sent-in-the-telemetry"></a>Kişisel verileri telemetri gönderilebilir mi?

Bu, kodunuzu tür veri gönderiyorsa mümkündür. Yığın izlemelerini değişkenlerinde kişisel veri içeriyorsa de oluşabilir. Geliştirme ekibiniz, kişisel verileri doğru şekilde işlediğinden emin olmak için risk değerlendirmeleri yürütmeniz gerekir. [Veri saklama ve gizlilik hakkında daha fazla bilgi edinin](data-retention-privacy.md).

**Tüm** coğrafi konum özniteliklerini aranır sonra istemci web adresinin sekizlik tabanda 0 olarak her zaman ayarlanmış.

## <a name="my-instrumentation-key-is-visible-in-my-web-page-source"></a>My izleme anahtarını my web sayfası kaynağında görülebilir. 

* Bu izleme çözümleri, genel yöntemdir.
* Verilerinizi çalmak için kullanılamaz.
* Veri veya tetikleyici uyarılarınızı eğriltmek için kullanılabilir.
* Tüm müşteriler bu tür sorunları oluşmuş kullanıcılarımız değil.

Şunları yapabilirsiniz:

* İki ayrı izleme (Application Insights kaynakları ayırın) anahtarlar, istemci ve sunucu verileri için kullanın. Veya
* Çalıştıran sunucunuzda bir ara sunucu yazabilir ve proxy üzerinden verileri göndermek web istemcisi vardır.

## <a name="post"></a>Gönderme verisi tanılama aramasındaki nasıl görebilirim?
Gönderme verisi otomatik olarak oturum yok, ancak bir TrackTrace çağrı kullanabilirsiniz: ileti parametreyi verileri yerleştirin. Bu, üzerinde filtre uygulanamıyor rağmen bir uzun dize özellikleri barındırabileceğiniz boyut sınırını sahiptir.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Tek veya birden fazla Application Insights kaynaklarını kullanmalı mıyım?

Tek bir kaynak tüm bileşenleri veya rolleri için tek bir iş sisteminde kullanın. Bağımsız uygulamaların yanı sıra, geliştirme, test ve yayın sürümleri için ayrı kaynaklar kullanın.

* [Buraya bakın](separate-resources.md)
* [Örnek - worker ve web rolleri ile bulut hizmeti](cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>İzleme anahtarını dinamik olarak nasıl değiştirebilirim?

* [Tartışması](separate-resources.md)
* [Örnek - worker ve web rolleri ile bulut hizmeti](cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Kullanıcı ve oturum nelerdir sayılır?

* JavaScript SDK'sı, etkinlikleri gruplandırmak için gelen kullanıcıları tanımlamak için web istemcisi, bir kullanıcı tanımlama bilgisinde ve oturum tanımlama bilgisini ayarlar.
* İstemci tarafı komut dosyası varsa, [sunucu tanımlama bilgilerini ayarlama](https://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Farklı tarayıcılar veya içinde InPrivate/gizli Tarama modunda sitenizdeki bir gerçek kullanıcı kullanır veya farklı makineler sonra birden çok kez sayılır olur.
* Makineleri ve tarayıcılar arasında bir oturum açma kullanıcı tanımlamak için bir çağrı ekleyin [setAuthenticatedUserContext()](api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a> Application Insights içinde her şeyi etkinleştirdiniz mi?
| Görmeniz gereken | Bunu alma | İstediğiniz neden |
| --- | --- | --- |
| Kullanılabilirlik grafikleri |[Web testleri](monitor-web-app-availability.md) |Web uygulamanız çalışıyor bildirin |
| Server app perf: yanıt süreleri... |[Projenize Application Insights ekleme](asp-net.md) veya [sunucuda AI Durum İzleyicisi yükleyin](monitor-performance-live-website-now.md) (veya kendi kodunuzu yazma [bağımlılıkları izlemek](api-custom-events-metrics.md#trackdependency)) |Performans sorunları tespit edin |
| Bağımlılık telemetrisi |[Sunucu üzerinde yapay ZEKA Durum İzleyicisi'ni yükleyin](monitor-performance-live-website-now.md) |Veritabanları veya diğer dış bileşenlere sorunları tanılayın |
| Özel durumlardan yığın izlemelerini alın |[Kodunuzda TrackException çağrıları eklemenize](asp-net-exceptions.md) (ancak bazı otomatik olarak raporlanır) |Algılama ve özel durumları tanılama |
| Günlük izlemelerini arama |[Günlüğe kaydetme bağdaştırıcı ekleme](asp-net-trace-logs.md) |Özel durumlar, performans sorunlarını tanılayın |
| İstemci kullanım temelleri: sayfa görüntülemeleri, oturumlar... |[Web sayfalarında JavaScript Başlatıcı](javascript.md) |Kullanım analizi |
| İstemci özel ölçümler |[Web sayfaları'nda izleme çağrıları](api-custom-events-metrics.md) |Kullanıcı deneyimini geliştirin |
| Sunucu özel ölçümler |[Sunucu izleme çağrıları](api-custom-events-metrics.md) |İş zekası |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Neden arama ve ölçümler grafiklerde sayıları eşit?

[Örnekleme](sampling.md) gerçekten portala uygulamanızdan gönderilen telemetri öğelerinin (istekler, özel olaylar vb.) sayısını azaltır. Aramada, gerçekte alınan öğe sayısını görürsünüz. Olayların sayısını görüntüleyen ölçüm grafiklerde oluştu özgün olay sayısını görürsünüz. 

Her öğe taşıyan iletilen bir `itemCount` kaç tane özgün olay öğeyi gösteren bir özelliği temsil eder. Örnekleme işleminde gözlemlemek için Analytics'te bu sorguyu çalıştırabilirsiniz:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Otomasyon

### <a name="configuring-application-insights"></a>Application Insights'ı yapılandırma

Yapabilecekleriniz [PowerShell betikleri yazma](powershell.md) için Azure Kaynak İzleyicisi'ni kullanma:

* Oluşturun ve Application Insights kaynaklarını güncelleştirin.
* Fiyatlandırma planı ayarlayın.
* İzleme anahtarını alın.
* Ölçüm uyarısı Ekle.
* Kullanılabilirlik testi ekleyin.

Ölçüm Gezgini'nde raporu ayarlayamıyor veya sürekli dışarı aktarmayı ayarlayın.

### <a name="querying-the-telemetry"></a>Telemetri sorgulama

Kullanım [REST API](https://dev.applicationinsights.io/) çalıştırılacak [Analytics](analytics.md) sorgular.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Bir olay hakkında bir uyarı nasıl ayarlayabilirim?

Ölçümleri yalnızca Azure uyarılardır. Değeri eşiği aştığında, olay her gerçekleştiğinde, özel bir ölçü oluşturun. Bir uyarı ölçüme göre ayarlayın. Dikkat: ölçüm her iki yönde; eşikle kesiştiğinde bir bildirim alırsınız. Başlangıç değeri yüksek veya düşük olmasına ne olursa olsun ilk kesen kadar bildirim alamazsınız; her zaman birkaç dakikalık bir gecikme yoktur.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Bir Azure web uygulaması ve Application Insights arasında veri aktarımı ücreti var mıdır?

* Azure web uygulamanıza bir veri merkezinde barındırılan, bir Application Insights toplama bitiş noktası olduğunda, ücret alınmaz. 
* Konak veri Merkezinize hiç toplama bitiş noktası yoktur sonra uygulamanızın telemetri ödenmesini [Azure ücretleri giden](https://azure.microsoft.com/pricing/details/bandwidth/).

Bu işlem, Application Insights kaynağınız burada barındırılan bağımlı değildir. Yalnızca bizim uç dağıtımına da bağlıdır.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Application ınsights'ı portala telemetri gönderebilir miyim?

SDK'larımızda kullanın ve kullanmak öneririz [SDK API'si](api-custom-events-metrics.md). Çeşitli için SDK çeşitlerini vardır [platformları](platforms.md). Bu SDK'ları, arabelleğe alma, sıkıştırma, azaltma, yeniden denemeler ve benzeri işler. Ancak, [alımı şema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) ve [uç nokta Protokolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) ortaktır.

## <a name="can-i-monitor-an-intranet-web-server"></a>Bir intranet web sunucusu izleyebilirim?

Evet, ancak güvenlik duvarı özel durumları ya da proxy yeniden yönlendirmeleri hizmetlerimizi trafiğine izin vermek ihtiyacınız olacak.
- QuickPulse `https://rt.services.visualstudio.com:443` 
- ApplicationIdProvider `https://dc.services.visualstudio.com:443` 
- TelemetryChannel `https://dc.services.visualstudio.com:443` 


Hizmetlerini ve IP adreslerini tam listemizi gözden geçirin [burada](../../azure-monitor/app/ip-addresses.md).

### <a name="firewall-exception"></a>Güvenlik Duvarı özel durumu

Web sunucunuza bizim uç noktalarına telemetri göndermesine izin verin. 

### <a name="gateway-redirect"></a>Ağ geçidi yönlendirme

Sunucunuza giden trafik bir ağ geçidi yapılandırma uç noktaları yazarak intranetinizde yol.
Bu "Bitiş" özellikleri, yapılandırmada mevcut değilse, bu sınıflar Applicationınsights.config örnekte aşağıda gösterilen varsayılan değerleri kullanır. 

Ağ geçidi trafiği bizim uç noktasının temel adresine yönlendirmesi. Yapılandırmanızda, varsayılan değerlerle değiştirin `http://<your.gateway.address>/<relative path>`.


#### <a name="example-applicationinsightsconfig-with-default-endpoints"></a>Varsayılan uç noktaları ile örnek Applicationınsights.config:
```xml
<ApplicationInsights>
  ...
  <TelemetryModules>
    <Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <QuickPulseServiceEndpoint>https://rt.services.visualstudio.com/QuickPulseService.svc</QuickPulseServiceEndpoint>
    </Add>
  </TelemetryModules>
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

### <a name="proxy-passthrough"></a>Proxy geçiş

Makine düzeyinde veya uygulama düzeyinde yapılandırarak proxy geçiş gerçekleştirilebilir proxy.
Daha fazla bilgi için dotnet'ın makaleye bakın [DefaultProxy](https://docs.microsoft.com/dotnet/framework/configure-apps/file-schema/network/defaultproxy-element-network-settings).
 
 Örnek Web.config:
 ```xml
<system.net>
    <defaultProxy>
      <proxy proxyaddress="http://xx.xx.xx.xx:yyyy" bypassonlocal="true"/>
    </defaultProxy>
</system.net>
```
 

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Bir intranet sunucusunda kullanılabilirlik web testleri çalıştırabilir miyim?

Bizim [web testleri](monitor-web-app-availability.md) dünyanın dört bir yanında dağıtılan varlık noktaları çalıştırın. İki çözümü vardır:

* Güvenlik Duvarı kapı - sunucunuzdan istekleri olanak [web test aracıları uzun ve değiştirilebilir listesini](ip-addresses.md).
* Sunucunuzdan intranetinize içinde düzenli aralıklarla istekler göndermek için kendi kodunuzu yazın. Bu amaç için Visual Studio web testleri çalıştırabilir. Test edicinin TrackAvailability() API'sini kullanarak sonuçları Application Insights'a gönderebilir.

## <a name="how-long-does-it-take-for-telemetry-to-be-collected"></a>Ne kadar süre için toplanacak telemetri sürer?

Çoğu Application Insights verilerini 5 dakikadan kısa bir gecikme vardır. Bazı veriler, daha uzun sürebilir; genellikle daha büyük günlük dosyaları. Daha fazla bilgi için [Application Insights SLA](https://azure.microsoft.com/support/legal/sla/application-insights/v1_2/).

## <a name="more-answers"></a>Daha fazla soru yanıtı
* [Uygulama anlayışları Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: data-retention-privacy.md
[platforms]: platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
