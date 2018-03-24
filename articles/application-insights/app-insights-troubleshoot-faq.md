---
title: Azure Application Insights ile ilgili SSS | Microsoft Docs
description: Application Insights hakkında sık sorulan sorular.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0e3b103c-6e2a-4634-9e8c-8b85cf5e9c84
ms.service: application-insights
ms.workload: mobile
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/12/2017
ms.author: mbullwin
ms.openlocfilehash: 721799703923339d397113fc278cdeb6c6dbb88f
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="application-insights-frequently-asked-questions"></a>Application Insights: Sık sorulan sorular

## <a name="configuration-problems"></a>Yapılandırma sorunları
*Sorun ayarı yaşıyorum my:*

* [.NET uygulaması](app-insights-asp-net-troubleshoot-no-data.md)
* [Zaten çalışan uygulama izleme](app-insights-monitor-performance-live-website-now.md#troubleshooting-runtime-configuration-of-application-insights)
* [Azure tanılama](app-insights-azure-diagnostics.md)
* [Java web uygulaması](app-insights-java-troubleshoot.md)

*Hiçbir veri sunucumdan alıyorum*

* [Set güvenlik duvarı özel durumlar](app-insights-ip-addresses.md)
* [ASP.NET sunucusu kurma](app-insights-monitor-performance-live-website-now.md)
* [Java sunucusunu ayarlama](app-insights-java-agent.md)

## <a name="can-i-use-application-insights-with-"></a>Application Insights ile kullanabilirim...?

* [Web uygulamaları IIS sunucusu - şirket içi veya VM](app-insights-asp-net.md)
* [Java web uygulamaları](app-insights-java-get-started.md)
* [Node.js uygulamaları](app-insights-nodejs.md)
* [Azure Web uygulamaları](app-insights-azure-web-apps.md)
* [Azure bulut Hizmetleri](app-insights-cloudservices.md)
* [Docker içinde çalışan uygulama sunucuları](app-insights-docker.md)
* [Tek sayfa web uygulamaları](app-insights-javascript.md)
* [Sharepoint](app-insights-sharepoint.md)
* [Windows masaüstü uygulaması](app-insights-windows-desktop.md)
* [Diğer platformlar](app-insights-platforms.md)

## <a name="is-it-free"></a>Ücretsiz mi?

Evet, Deneysel için kullanın. Temel fiyatlandırma planı, uygulamanızın bir belirli indirimi veri her ay ücretsiz olarak gönderebilir. Ücretsiz indirimi kapak geliştirme ve küçük bir kullanıcı sayısı için bir uygulama yayımlamak için yeterince büyük olduğundan. Belirtilen miktarda veri birden fazla işlenmekte olan önlemek için bir uç ayarlayabilirsiniz.

Telemetri daha büyük birimleri Gb ile ücretlendirilirsiniz. İçin bazı ipuçları sağladığımız [ücretlerinizi sınırlamak](app-insights-pricing.md).

Kurumsal planı her web sunucusu düğümüne telemetri gönderen her gün için bir ücret doğurur. Büyük ölçekte sürekli verme kullanmak istiyorsanız uygundur.

[Fiyatlandırma planı okuma](https://azure.microsoft.com/pricing/details/application-insights/).

## <a name="how-much-is-it-costing"></a>Bu ne kadar maliyetlendirme?

* Açık **özellikleri + fiyatlandırma** Application Insights kaynağı sayfasında. Bir grafik son kullanım yoktur. İsterseniz, bir veri birimi cap ayarlayabilirsiniz.
* Açık [Azure faturalama dikey](https://portal.azure.com/#blade/Microsoft_Azure_Billing/BillingBlade/Overview) tüm kaynaklar arasında faturalarınızı görmek için.

## <a name="q14"></a>Ne my projesinde Application Insights değiştirebilir?
Ayrıntıları proje türüne göre değişir. Web uygulaması için:

* Bu dosyaları projenize ekler:

  * ApplicationInsights.config.
  * ai.js
* Bu NuGet paketlerini yükler:

  * *Application Insights API'si* -API çekirdek
  * *Web uygulamaları için Application Insights API'si* - telemetri sunucusundan göndermek için kullanılan
  * *JavaScript uygulamaları için Application Insights API'si* - telemetri istemciden göndermek için kullanılan

    Paketler bu derlemeler şunlardır:
  * Microsoft.ApplicationInsights
  * Microsoft.ApplicationInsights.Platform
* Öğeleri ekler:

  * Web.config
  * packages.config
* (Yeni varsa yalnızca - projeleri, [varolan bir projeye Application Insights Ekle][start], bu el ile yapmanız gerekir.) Application Insights kaynak kimliği ile başlatmak için istemci ve sunucu kodu parçacıkları ekler Örneğin, bir MVC uygulamasında kodu ana sayfaya Views/Shared/_Layout.cshtml eklenir

## <a name="how-do-i-upgrade-from-older-sdk-versions"></a>Eski SDK sürümlerden nasıl yükseltme?
Bkz: [sürüm notları](app-insights-release-notes.md) SDK'sı, uygulama türüne uygun.

## <a name="update"></a>Proje için verileri gönderir hangi Azure kaynak nasıl değiştirebilir miyim?
Çözüm Gezgini'nde sağ `ApplicationInsights.config` ve **güncelleştirme Application Insights**. Varolan veya yeni bir kaynak azure'da veri gönderebilir. Güncelleştirme Sihirbazı'nı nereye sunucusu SDK verilerinizi gönderir belirler Applicationınsights.config izleme anahtarını değiştirir. "Tümünü Güncelleştir" kaldırmadıysanız web sayfalarınıza göründüğü anahtar da değiştirir.

## <a name="what-is-status-monitor"></a>Durum İzleyicisi nedir?

IIS web sunucunuza Application Insights web uygulamalarında yapılandırmak için kullanabileceğiniz bir masaüstü uygulaması. Telemetri toplamak değil: bir uygulama yapılandırırken değil durdurabilirsiniz. 

[Daha fazla bilgi edinin](app-insights-monitor-performance-live-website-now.md#questions).

## <a name="what-telemetry-is-collected-by-application-insights"></a>Hangi telemetri Application Insights tarafından toplanır?

Sunucu web uygulamaları:

* HTTP istekleri
* [Bağımlılıklar](app-insights-asp-net-dependencies.md). Çağrıları: SQL veritabanları; Dış hizmetler için HTTP çağrıları; Azure Cosmos DB, tablo, blob depolama ve sıra. 
* [Özel durumlar](app-insights-asp-net-exceptions.md) ve Yığın izlemeleri.
* [Performans sayaçları](app-insights-performance-counters.md) - kullanırsanız [Durum İzleyicisi](app-insights-monitor-performance-live-website-now.md), [Azure izleme](app-insights-azure-web-apps.md) veya [Application Insights collectd yazan](app-insights-java-collectd.md).
* [Özel olayları ve ölçümleri](app-insights-api-custom-events-metrics.md) , kod.
* [İzleme günlükleri](app-insights-asp-net-trace-logs.md) uygun Toplayıcı yapılandırırsanız.

Gelen [istemci web sayfaları](app-insights-javascript.md):

* [Sayfa görünümü sayar](app-insights-web-track-usage.md)
* [AJAX çağrıları](app-insights-asp-net-dependencies.md) çalışan komut dosyasından yapılan istekleri.
* Sayfa görünümü veri yükleme
* Kullanıcı ve oturum sayısı
* [Kimliği doğrulanmış kullanıcı kimlikleri](app-insights-api-custom-events-metrics.md#authenticated-users)

Bunları yapılandırırsanız, diğer kaynaklardan:

* [Azure tanılama](app-insights-azure-diagnostics.md)
* [Docker kapsayıcıları](app-insights-docker.md)
* [İçeri aktarma tablolara analizi](app-insights-analytics-import.md)
* [Log Analytics](https://azure.microsoft.com/blog/omssolutionforappinsightspublicpreview/)
* [Logstash](app-insights-analytics-import.md)

## <a name="can-i-filter-out-or-modify-some-telemetry"></a>I filtrelemek veya birkaç telemetri değiştirmek için?

Evet, sunucuda şunu yazabilirsiniz:

* Filtre veya uygulamanızdan gönderilmeden önce seçilen telemetri öğelerine özellikleri eklemek için telemetri işlemcisi'ni kullanın.
* Telemetri Başlatıcı telemetri tüm öğelerine özellikleri ekleyin.

Daha fazla bilgi edinin [ASP.NET](app-insights-api-filtering-sampling.md) veya [Java](app-insights-java-filter-telemetry.md).

## <a name="how-are-city-country-and-other-geo-location-data-calculated"></a>Şehir, ülke ve diğer coğrafi konum verileri nasıl hesaplanır?

Biz IP adresini kullanarak web istemcisi (IPv4 veya IPv6) aramak [GeoLite2](http://dev.maxmind.com/geoip/geoip2/geolite2/).

* Tarayıcı telemetri: Gönderenin IP adresi toplarız.
* Sunucu telemetri: Application Insights modülü istemci IP adresi toplar. Varsa toplanmaz `X-Forwarded-For` ayarlanır.

Yapılandırabileceğiniz `ClientIpHeaderTelemetryInitializer` üstbilgisi farklı IP adresi almak için. Bazı sistemlerde, örneğin, bir proxy tarafından taşınırsa, yük dengeleyici veya CDN ile `X-Originating-IP`. [Daha fazla bilgi edinin](http://apmtips.com/blog/2016/07/05/client-ip-address/).

Yapabilecekleriniz [Power BI](app-insights-export-power-bi.md) bir haritada isteği telemetrinizi görüntülenecek.


## <a name="data"></a>Ne kadar süreyle verileri portalda Tutuluyor? Güvenli mi?
Bir göz atalım [veri saklama ve gizlilik][data].

## <a name="might-personally-identifiable-information-pii-be-sent-in-the-telemetry"></a>Kişisel olarak tanımlanabilir bilgileri (PII) telemetri gönderilmiş olabilir?

Bu, kodunuzu bu tür veri gönderiyorsa mümkün olur. Yığın izlemeleri değişkenlerde PII eklerseniz de oluşabilir. Geliştirme ekibiniz PII düzgün şekilde işlendiğinden emin olmak için risk değerlendirmeleri yürütmeniz gerekir. [Veri saklama ve gizlilik hakkında daha fazla bilgi edinmek](app-insights-data-retention-privacy.md).

**Tüm** coğrafi konum öznitelikleri aranır sonra istemci web adresi sekizlisinin 0 olarak her zaman ayarlanmış.

## <a name="my-ikey-is-visible-in-my-web-page-source"></a>My iKey my web sayfası kaynağında görünür olur. 

* Bu çözümleri izleme yaygın bir uygulamadır.
* Verilerinizi çalmak için kullanılamaz.
* Veri veya tetikleyici uyarılarınızı eğme için kullanılabilir.
* Biz herhangi bir müşteriye tür sorunları oluşturdu duymuş değil.

Size şunları yapabilir:

* (Application Insights kaynakları ayırın) iki ayrı iKeys istemci ve sunucu verileri için kullanın. Veya
* Sunucunuzda çalışan bir proxy yazma ve proxy üzerinden veri gönderme web istemcisi.

## <a name="post"></a>Tanılama arama posta verileri nasıl görürüm?
Gönderme verisi otomatik olarak oturum yoktur, ancak bir TrackTrace çağrı kullanabilirsiniz: ileti parametresinde verileri yerleştirin. Filtre uygulayamazsınız rağmen bu dize özellikleri sınırlamaları daha uzun bir boyut sınırı vardır.

## <a name="should-i-use-single-or-multiple-application-insights-resources"></a>Tek veya birden çok Application Insights kaynağı kullanmalıyım?

Tek bir kaynak tüm bileşenleri veya rolleri için tek bir iş sisteminde kullanın. Ayrı kaynaklar ve bağımsız uygulamalar geliştirme, test ve yayın sürümleri için kullanın.

* [Buraya bakın](app-insights-separate-resources.md)
* [Örnek - worker ve web rolleri sahip bulut hizmeti](app-insights-cloudservices.md)

## <a name="how-do-i-dynamically-change-the-instrumentation-key"></a>Nasıl izleme anahtarını dinamik olarak değişir mi?

* [Burada tartışma](app-insights-separate-resources.md)
* [Örnek - worker ve web rolleri sahip bulut hizmeti](app-insights-cloudservices.md)

## <a name="what-are-the-user-and-session-counts"></a>Sayıları oturumu ve kullanıcı nelerdir?

* JavaScript SDK'sı etkinlikleri gruplandırmak için döndürmeyi kullanıcıları tanımlamak için web istemcisi, bir kullanıcı tanımlama bilgisinde ve oturum tanımlama bilgisi ayarlar.
* İstemci tarafı komut dosyası varsa, şunları yapabilirsiniz [sunucuda tanımlama bilgilerini ayarlama](http://apmtips.com/blog/2016/07/09/tracking-users-in-api-apps/).
* Farklı tarayıcılar veya içinde-özel/ıncognito gözatma kullanarak sitenizdeki bir gerçek kullanıcı kullanıyor veya farklı makinelerde sonra birden çok kez sayılacaktır varsa.
* Makineler ve tarayıcılar arasında bir oturum açma kullanıcı tanımlamak için bir çağrı ekleyin [setAuthenticatedUserContext()](app-insights-api-custom-events-metrics.md#authenticated-users).

## <a name="q17"></a> Her şeyi Application Insights etkinleştirdiniz mi?
| Görmeniz gerekir | Bunu alma | İstediğiniz neden |
| --- | --- | --- |
| Kullanılabilirlik grafikleri |[Web testleri](app-insights-monitor-web-app-availability.md) |Web uygulamanız çalışıyor bildirin |
| Sunucu uygulaması perf: yanıt süreleri... |[Application Insights projenize eklemek](app-insights-asp-net.md) veya [sunucuda AI Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md) (veya kendi kodunuzu yazma [izlemek bağımlılıkları](app-insights-api-custom-events-metrics.md#trackdependency)) |Performans sorunlarını Algıla |
| Bağımlılık telemetrisi |[Sunucuda AI Durum İzleyicisi yükleme](app-insights-monitor-performance-live-website-now.md) |Veritabanları veya diğer dış bileşenlere ile ilgili sorunları tanılamak |
| Özel durumlar Yığın izlemeleri Al |[Kodunuzda TrackException çağrıları ekleme](app-insights-asp-net-exceptions.md) (ancak bazı otomatik olarak bildirilen) |Algılamak ve tanılamak özel durumlar |
| Arama günlük izlemelerini |[Bir günlük bağdaştırıcısı ekleme](app-insights-asp-net-trace-logs.md) |Özel durumlar, performans sorunlarını tanılamanıza |
| İstemci kullanım temelleri: sayfa görünümleri, oturumlar,... |[Web sayfalarında JavaScript Başlatıcı](app-insights-javascript.md) |Kullanım analizi |
| İstemci özel ölçümleri |[Web sayfalarında izleme çağırır](app-insights-api-custom-events-metrics.md) |Kullanıcı deneyimini geliştirmek |
| Sunucu özel ölçümleri |[Sunucu izleme çağrıları](app-insights-api-custom-events-metrics.md) |İş zekası |

## <a name="why-are-the-counts-in-search-and-metrics-charts-unequal"></a>Neden arama ve ölçümler grafiklerde sayısına eşit olmayan misiniz?

[Örnekleme](app-insights-sampling.md) (istekleri, özel etkinlikler ve benzeri) uygulamanızdan portala gönderilen gerçekten telemetri öğe sayısını azaltır. Aramada, aslında alınan öğe sayısı bakın. Etkinliklerin sayısını görüntüleyen ölçüm grafiklerde oluştu özgün olayların sayısının bakın. 

İzleme olan her bir öğe aktarılan bir `itemCount` öğenin kaç özgün olayları gösterir özelliğini temsil eder. Örnekleme işleminde izlemek için bu sorguyu analizleri çalıştırabilirsiniz:

```
    requests | summarize original_events = sum(itemCount), transmitted_events = count()
```


## <a name="automation"></a>Otomasyon

### <a name="configuring-application-insights"></a>Application Insights yapılandırma

Yapabilecekleriniz [PowerShell komut dosyaları yazmak](app-insights-powershell.md) Azure Kaynak Yöneticisi kullanarak:

* Oluşturun ve Application Insights kaynakları güncelleştirin.
* Fiyatlandırma planı ayarlayın.
* İzleme anahtarını alır.
* Ölçüm uyarı ekleyin.
* Bir kullanılabilirlik test ekleyin.

Ölçüm Gezgini Raporu ayarlarken veya sürekli verme ayarlayın.

### <a name="querying-the-telemetry"></a>Telemetri sorgulama

Kullanım [REST API](https://dev.applicationinsights.io/) çalıştırmak için [Analytics](app-insights-analytics.md) sorgular.

## <a name="how-can-i-set-an-alert-on-an-event"></a>Bir olaya nasıl bir uyarı ayarlarım?

Azure uyarıları yalnızca üzerinde ölçümleridir. Bir değeri eşik, olay oluştuğunda yayılan özel bir ölçü oluşturun. Daha sonra bir uyarı üzerinde ölçüm ayarlayın. Unutmayın: her iki yönde; eşiği ölçüm kestiği her bir bildirim alacaksınız Başlangıç değeri yüksek veya düşük olup olsun ilk aşma kadar bir bildirim vermeyecektir; her zaman birkaç dakikalık bir gecikme yoktur.

## <a name="are-there-data-transfer-charges-between-an-azure-web-app-and-application-insights"></a>Bir Azure web uygulaması ve Application Insights arasında veri aktarımı ücretlerine var mı?

* Azure web uygulamanızın bir veri merkezinde barındırılıp barındırılmadığını Application Insights toplama bitiş noktası olduğunda, ücretsizdir. 
* Ana bilgisayar veri merkezinizde hiçbir toplama bitiş noktası olan sonra uygulamanızın telemetrisinde erişimliye [ücretleri giden Azure](https://azure.microsoft.com/pricing/details/bandwidth/).

Bu, Application Insights kaynağınıza burada barındırılan bağımlı değil. Ayrıca bizim uç noktaları dağıtım noktasında sadece bağlıdır.

## <a name="can-i-send-telemetry-to-the-application-insights-portal"></a>Application Insights portala telemetri gönderebilir miyim?

Bizim SDK'ları kullanın ve kullanmak öneririz [SDK API'si](app-insights-api-custom-events-metrics.md). Çeşitli için SDK'sı çeşitlemelerini vardır [platformları](app-insights-platforms.md). Bu SDK'ları arabelleğe alma, sıkıştırma, azaltma, yeniden deneme ve benzeri işler. Ancak, [alım şema](https://github.com/Microsoft/ApplicationInsights-dotnet/tree/develop/Schema/PublicSchema) ve [endpoint Protokolü](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/ENDPOINT-PROTOCOL.md) ortaktır.

## <a name="can-i-monitor-an-intranet-web-server"></a>Bir intranet web sunucusuna izleyebilir mi?

İki yöntem şunlardır:

### <a name="firewall-door"></a>Güvenlik Duvarı kapı

Web sunucunuzun bizim uç noktalarına telemetri göndermesine izin https://dc.services.visualstudio.com:443 ve https://rt.services.visualstudio.com:443. 

### <a name="proxy"></a>Ara sunucu

Trafiği sunucunuzdan bu Applicationınsights.Config'de ayarlayarak, intranetinizdeki bir ağ geçidi yönlendirme:

```XML
<TelemetryChannel>
    <EndpointAddress>your gateway endpoint</EndpointAddress>
</TelemetryChannel>
```

Ağ geçidiniz için trafiği yönlendirmek https://dc.services.visualstudio.com:443/v2/track

## <a name="can-i-run-availability-web-tests-on-an-intranet-server"></a>Kullanılabilirlik web testleri bir intranet sunucusunda çalıştırabilir miyim?

Bizim [web testleri](app-insights-monitor-web-app-availability.md) noktalarında dünya dağıtılan varlık çalıştırın. İki çözümü vardır:

* Güvenlik Duvarı kapı - sunucunuzdan istekleri olanak [web test aracılarını uzun ve değiştirilebilir listesini](app-insights-ip-addresses.md).
* Sunucunuzdan intranetinize içinde düzenli istekleri göndermek için kendi kodunuzu yazın. Bu amaç için Visual Studio web testleri çalıştırabilirsiniz. Test sonuçları TrackAvailability() API kullanarak Application Insights'a gönderebilir.

## <a name="more-answers"></a>Daha fazla yanıtlar
* [Uygulama Öngörüler Forumu](https://social.msdn.microsoft.com/Forums/vstudio/en-US/home?forum=ApplicationInsights)

<!--Link references-->

[data]: app-insights-data-retention-privacy.md
[platforms]: app-insights-platforms.md
[start]: app-insights-overview.md
[windows]: app-insights-windows-get-started.md
