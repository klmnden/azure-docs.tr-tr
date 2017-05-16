---
title: "Windows sunucu ve çalışan rolleri için Azure Application Insights | Microsoft Docs"
description: "Kullanım, kullanılabilirlik ve performansı analiz etmek için Application Insights SDK’sını ASP.NET uygulamanıza el ile ekleyin."
services: application-insights
documentationcenter: .net
author: alancameronwills
manager: carmonm
ms.assetid: 106ba99b-b57a-43b8-8866-e02f626c8190
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/01/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 0c4554d6289fb0050998765485d965d1fbc6ab3e
ms.openlocfilehash: 29598f052778759ed362e3aa4b997acb799717ef
ms.contentlocale: tr-tr
ms.lasthandoff: 04/13/2017


---
# <a name="manually-configure-application-insights-for-aspnet-applications"></a>ASP.NET uygulamaları için Application Insights’ı el ile yapılandırma
[Application Insights](app-insights-overview.md), web geliştiricilerinin canlı uygulamanızın performansını ve kullanımını izlemesine yönelik genişletilebilir bir araçtır. Bu aracı Windows sunucu, çalışan rolleri ve diğer ASP.NET uygulamalarını izlemek üzere el ile yapılandırabilirsiniz. Web uygulamaları için el il yapılandırma, Visual Studio’nun sunduğu [otomatik ayarın](app-insights-asp-net.md) bir alternatifidir.

![Örnek performans izleme grafikleri](./media/app-insights-windows-services/10-perf.png)

#### <a name="before-you-start"></a>Başlamadan önce
Gerekenler:

* Bir [Microsoft Azure](http://azure.com) aboneliği. Ekibinizin ve kuruluşunuzun Azure aboneliği varsa, sahibi [Microsoft hesabınızı](http://live.com) kullanarak sizi buna ekleyebilir.
* Visual Studio 2013 veya üstü.

## <a name="add"></a>1. Application Insights kaynağı oluşturma
[Azure portalında](https://portal.azure.com/) oturum açın ve yeni bir Application Insights kaynağı oluşturun. Uygulama türü olarak ASP.NET’i seçin.

![Yeni, Application Insights öğesine tıklayın](./media/app-insights-windows-services/01-new-asp.png)

Azure’da [kaynak](app-insights-resources-roles-access-control.md) bir hizmetin örneğidir. Bu kaynak, uygulamanızdan alınan telemetri verilerinin analiz edilip size sunulacağı yerdir.

Uygulama türü seçimi, kaynak dikey pencerelerinin varsayılan içeriğini ve [Ölçüm Gezgini](app-insights-metrics-explorer.md) içinde görünen özellikleri belirler.

#### <a name="copy-the-instrumentation-key"></a>İzleme Anahtarını Kopyalama
Kaynağı tanımlayan bu anahtarı kısa bir süre sonra verileri kaynağa yönlendirmek için SDK’ya yükleyeceksiniz.

![Özellikler'e tıklayın, anahtarı seçin ve ctrl + C tuşlarına basın](./media/app-insights-windows-services/02-props-asp.png)

Yeni bir kaynak oluşturmak üzere az önce uyguladığınız adımlar herhangi bir uygulamada izlemeyi başlatmanın iyi bir yoludur. Şimdi uygulamaya veri gönderebilirsiniz.

## <a name="sdk"></a>2. Uygulamanıza Application Insights paketi yükleme
Application Insights paketinin yüklenmesi ve yapılandırılması, üzerinde çalıştığınız platforma bağlı olarak değişir. ASP.NET uygulamaları için kolaydır.

1. Visual Studio'da web uygulaması projenizin NuGet paketlerini düzenleyin.
   
    ![Projeye sağ tıklayın ve Nuget Paketlerini Yönet’i seçin](./media/app-insights-windows-services/03-nuget.png)
2. Windows sunucu uygulamaları için Application Insights paketini yükleyin.
   
    !["Application Insights" araması yapın](./media/app-insights-windows-services/04-ai-nuget.png)
   
    *Diğer paketleri kullanabilir miyim?*
   
    Evet. API’yi yalnızca kendi telemetrinizi göndermek için kullanmak istiyorsanız Çekirdek API’yi (Microsoft.ApplicationInsights) seçin. Windows Server paketi Çekirdek API’nin yanı sıra performans sayacı koleksiyonu ve bağımlılık izlemesi gibi birkaç paketi daha otomatik olarak içerir. 

#### <a name="to-upgrade-to-future-package-versions"></a>Gelecekteki paket sürümlerine yükseltmek için
SDK’nın yeni sürümü zaman zaman yayınlanmaktadır.

[Paketin yeni sürümüne](https://github.com/Microsoft/ApplicationInsights-dotnet-server/releases/) yükseltme yapmak için NuGet paket yöneticisini yeniden açın ve yüklü paketleri filtreleyin. **Microsoft.ApplicationInsights.WindowsServer** ve **Yükselt**’i seçin.

ApplicationInsights.config dosyasında herhangi bir özelleştirme yaptıysanız yükseltmeden önce bir kopyasını kaydedin ve daha sonra değişikliklerinizi yeni sürümle birleştirin.

## <a name="3-send-telemetry"></a>3. Telemetri gönderme
**Yalnızca çekirdek API paketini yüklediyseniz:**

* Koddaki izleme anahtarını ayarlayın, örneğin `main()` içinde: 
  
    `TelemetryConfiguration.Active.InstrumentationKey = "` *anahtarınız* `";` 
* [API'yi kullanarak kendi telemetrinizi yazın](app-insights-api-custom-events-metrics.md#ikey).

**Diğer Application Insights paketlerini yüklediyseniz** izleme anahtarını ayarlamak için isterseniz .config dosyasını kullanabilirsiniz:

* ApplicationInsights.config dosyasını (NuGet yüklemesinde eklenen dosya) düzenleyin. Kapanış etiketinin hemen önüne şunu ekleyin:
  
    `<InstrumentationKey>` *kopyaladığınız izleme anahtarı* `</InstrumentationKey>`
* Çözüm Gezgini’nde ApplicationInsights.config dosyası özelliklerinin **Build Action = Content, Copy to Output Directory = Copy** olarak ayarlandığından emin olun.

[Farklı derleme yapılandırmalarında anahtarı değiştirmek istiyorsanız](app-insights-separate-resources.md) kodda izleme anahtarı belirlemeniz faydalı olacaktır. Kodda anahtarı ayarladıysanız `.config` dosyasında ayarlamanız gerekmez.

## <a name="run"></a> Projenizi çalıştırma
**F5** ile uygulamanızı çalıştırın ve şunu deneyin: birkaç telemetri oluşturmak için farklı sayfalar açın.

Visual Studio'da gönderilmiş etkinliklerin sayısını görürsünüz.

![Visual Studio’da olay sayımı](./media/app-insights-windows-services/appinsights-09eventcount.png)

## <a name="monitor"></a> Telemetrinizi görüntüleme
[Azure portal](https://portal.azure.com/)’a geri dönün ve Application Insights kaynağınıza göz atın.

Genel Bakış grafiklerinde veri arayın. İlk olarak yalnızca bir veya iki nokta görürsünüz. Örneğin:

![Daha fazla veri için tıklayın](./media/app-insights-windows-services/12-first-perf.png)

Daha ayrıntılı ölçümler görmek için herhangi bir grafiğe tıklayın. [Ölçümler hakkında daha fazla bilgi edinin.](app-insights-web-monitor-performance.md)

#### <a name="no-data"></a>Veri yok mu?
* Birkaç telemetri oluşturması için farklı sayfaları açarak uygulamayı kullanın.
* Olayları tek tek görmek için [Ara](app-insights-diagnostic-search.md) kutucuğunu açın. Bazı durumlarda olayların ölçüm ardışık düzenine ulaşması biraz daha uzun sürer.
* Birkaç saniye bekleyin ve **Yenile**’ye tıklayın. Grafikler kendilerini düzenli olarak yeniler, ancak bazı verilerin görüntülenmesini bekliyorsanız el ile yenileyebilirsiniz.
* Bkz. [Sorun giderme](app-insights-troubleshoot-faq.md).

## <a name="publish-your-app"></a>Uygulamanızı yayımlama
Şimdi uygulamanızı sunucunuza veya Azure’a dağıtın ve verilerin birikmesini izleyin.

![Visual Studio kullanarak uygulamanızı yayımlama](./media/app-insights-windows-services/15-publish.png)

Hata ayıklama modunda çalıştırdığınızda telemetri ardışık düzen üzerinden gönderilir, böylece görüntülenen verileri saniyeler içinde görebilirsiniz. Uygulamanızı Sürüm yapılandırmasında dağıttığınızda veriler daha yavaş birikir.

#### <a name="no-data-after-you-publish-to-your-server"></a>Sunucunuza yayımladıktan sonra veri yok mu?
Sunucunuzun güvenlik duvarında giden trafik için bağlantı noktalarını açın. Gerekli adreslerin listesi için [bu sayfaya](https://docs.microsoft.com/azure/application-insights/app-insights-ip-addresses) bakın 

#### <a name="trouble-on-your-build-server"></a>Derleme sunucunuzda sorun mu yaşıyorsunuz?
Lütfen [bu Sorun Giderme maddesine](app-insights-asp-net-troubleshoot-no-data.md#NuGetBuild) bakın.

> [!NOTE]
> Uygulamanız çok sayıda telemetri oluşturuyorsa (ve ASP.NET SDK sürüm 2.0.0-beta3 ya da üstünü kullanıyorsanız), uyarlamalı örnekleme modülü olayların yalnızca bir temsilci fraksiyonunu göndererek portala gönderilen hacmi otomatik olarak azaltır. Ancak, aynı istekle ilişkili olaylar grup olarak seçilir ya da seçimi kaldırılır, böylece ilgili olaylar arasında gezinebilirsiniz. 
> [Örnekleme hakkında bilgi edinin](app-insights-sampling.md).
> 
> 

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Sonraki adımlar
* Uygulamanızın 360 derecelik tam görünümünü elde etmek üzere [daha fazla telemetri ekleyin](app-insights-asp-net-more.md).


