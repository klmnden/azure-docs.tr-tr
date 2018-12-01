---
title: Ölçümleri Stream özel ölçümleri ve tanılama Azure Application ınsights ile canlı | Microsoft Docs
description: Özel ölçümler ile gerçek zamanlı web uygulamanızı izleme ve bir canlı akışı hataları, izlemeler ve olaylar ile ilgili sorunları tanılayın.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/24/2018
ms.reviewer: sdash
ms.author: mbullwin
ms.openlocfilehash: 8c414f7993de1589a3992e247400c3596fd18631
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52720482"
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Canlı ölçümler Stream: İzleme ve tanılama 1 saniyelik gecikme süresi

Gelen Canlı ölçümleri Stream kullanarak canlı, üretimde web uygulamanızın beating Kalp araştırma [Application Insights](app-insights-overview.md). Seçin ve hizmetinize herhangi Ayaklanma olmadan gerçek zamanlı olarak izlemek için Ölçümler ve performans sayaçlarını filtreleyin. Yığın izlemelerinden örnek başarısız istekler ve özel durumları inceleyin. İle birlikte [Profiler](app-insights-profiler.md), [anlık görüntü hata ayıklayıcısı](app-insights-snapshot-debugger.md), ve [performans testi](app-insights-monitor-web-app-availability.md#performance-tests), Canlı ölçümleri Stream, canlı web için güçlü ve bozucu bir tanılama aracı sağlar Site.

Canlı ölçümler Stream ile şunları yapabilirsiniz:

* Serbest kalır, ancak bir düzeltme doğrulama performans ve hata sayılarını izleyerek.
* Test yükleri ve sorunları tanılayabilir etkisini izleyin Canlı. 
* Belirli test oturumları odak veya bilinen sorunlar seçme ve filtreleme, izlemek istediğiniz ölçümleri filtreleyin.
* Özel durum izlemeleri gelişmelerden alın.
* En uygun KPI'ları bulmak için filtreleri ile denemeler yapın.
* Tüm Windows performans sayacı Canlı izleyin.
* Kolayca sorunları ve filtresi sunucuya yalnızca tüm KPI/canlı akış sahip bir sunucu tanımlayın.

[![Canlı ölçümler Stream video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

## <a name="get-started"></a>başlarken

1. Henüz yapmadıysanız [Application Insights yüklü](app-insights-asp-net.md) ASP.NET web uygulamanızda veya [Windows server uygulamasını](app-insights-windows-services.md), bunu şimdi yapın. 
2. **En son sürüme güncelleştirme** Application Insights paketi. Visual Studio'da, projenize sağ tıklayıp seçin **Nuget paketlerini Yönet**. Açık **güncelleştirmeleri** sekmesinde, onay **ön sürümü dahil et**ve tüm Microsoft.ApplicationInsights.* paketleri seçin.

    Uygulamanızı yeniden dağıtın.

3. İçinde [Azure portalında](https://portal.azure.com), uygulamanız için Application Insights kaynağını açın ve ardından canlı Stream açın.

4. [Denetim kanalı güvenli](#secure-the-control-channel) , müşteri adları gibi hassas verileri filtrelerinizi kullanabilirsiniz.


![Genel Bakış dikey penceresinde, Canlı Stream'a tıklayın.](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Veri yok mu? Sunucu güvenlik duvarınızdan denetleyin

Denetleme [Canlı ölçümleri Stream için giden bağlantı noktalarının](app-insights-ip-addresses.md#outgoing-ports) sunucularınızı Güvenlik Duvarı'nda açıktır. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Canlı ölçümler Stream ölçüm Gezgini ve analiz farkı nedir?

| |Canlı Akış | Ölçüm Gezgini ve analiz |
|---|---|---|
|Gecikme süresi|Bir saniye içinde görüntülenen verileri|Dakika boyunca toplanır.|
|Hiçbir bekletme|Grafik üzerinde olduğundan ve ardından atılır ancak veri devam ettirir.|[Veriler 90 gün boyunca saklanır.](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|İsteğe bağlı|Canlı ölçümler açarken veriler akışla|SDK'ın yüklü ve etkin olduğunda, veriler gönderilir|
|Ücretsiz|Canlı Stream veri için ücret alınmaz|Konusu [fiyatlandırması](app-insights-pricing.md)
|Örnekleme|Tüm seçili ölçümlerini ve sayaçlarını aktarılır. Hataları ve Yığın izlemeleri örneklenir. TelemetryProcessors uygulanmaz.|Olayları olabilir [örneklenir](app-insights-api-filtering-sampling.md)|
|Denetim kanalı|Filtre denetimi sinyalleri için SDK'sı gönderilir. Öneririz [güvenli bu kanal](#secure-channel).|Portala tek yönlü iletişim|


## <a name="select-and-filter-your-metrics"></a>Seçin ve ölçümlerinizi Filtrele

(En son SDK'sı ile klasik ASP.NET uygulamalarında kullanılabilir.)

Portaldan herhangi bir Application Insights telemetri üzerinde isteğe bağlı filtreler uygulayarak özel KPI Canlı izleyebilirsiniz. Ne zaman, fare herhangi grafik üzerinde gösteren filtre denetimi tıklayın. Aşağıdaki grafikte bir URL ve süresi öznitelikleri filtreleri ile özel bir istek sayısı KPI çizim. Zaman içinde herhangi bir noktada belirtilen ölçütlerle eşleşen telemetri bir canlı akışı gösteren Stream Önizleme bölümüyle filtrelerinizi doğrulayın. 

![Özel bir istek KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

Bir değer sayısından farklı izleyebilirsiniz. Seçenekler herhangi bir Application Insights telemetri olabilecek akışın türüne bağlıdır: istekleri, bağımlılıkları, özel durumlar, izlemeler, olaylar veya ölçümleri. Kendi olabilir [özel ölçüm](app-insights-api-custom-events-metrics.md#properties):

![Değer seçenekleri](./media/app-insights-live-stream/live-stream-valueoptions.png)

Application Insights telemetri ek olarak, akış seçenekler arasından seçim yapma ve performans sayacının adını sağlayan herhangi bir Windows performans sayacı da izleyebilirsiniz.

Canlı ölçümleri iki noktalarda toplanır: her sunucuda yerel olarak ve tüm sunucular arasında. Varsayılan olarak ilgili açılan menülerde diğer seçenekleri belirleyerek ya da değiştirebilirsiniz.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Örnek Telemetri: Özel Canlı Tanılama Olayları
Varsayılan olarak, canlı akış olayları başarısız istekler ve bağımlılık çağrıları, özel durumlar, olaylarla ve izlemelerle örnekleri gösterir. Zamanında uygulanan ölçütlerini herhangi bir noktada görmek için filtre simgesini tıklayın. 

![Varsayılan Canlı akış](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Ölçümleri etkinleştirildiğinde, herhangi bir Application Insights telemetri türü için herhangi bir rastgele ölçüt belirtebilirsiniz. Bu örnekte, belirli bir istek hataları, izler ve olayları seçiyoruz. Ayrıca tüm özel durumlar ve bağımlılık hataları seçiyoruz.

![Özel bir canlı akışı](./media/app-insights-live-stream/live-stream-events.png)

Not: Özel durum iletisi tabanlı ölçüt için en dıştaki özel durum iletisi şu anda kullanın. İç özel durum iletisi zararsız özel durumla filtrelemek için önceki örnekte (aşağıdaki "<--" sınırlayıcı) "istemci bağlantısı kesildi." bir ileti kullanın değil-"İstek içeriği okunurken hata" ölçütleri içerir.

Canlı akıştaki bir öğenin ayrıntılarını tıklayarak bakın. Tıklayarak akış duraklatabilirsiniz **duraklatmak** yalnızca aşağı kaydırma veya öğeyi tıklatarak. Başa dön veya durdurulduğundan sırasında toplanan öğelerinin sayaç tıklayarak kaydırma sonra canlı akışı devam eder.

![Örneklenen Canlı hataları](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Sunucu örneği tarafından Filtrele

Belirli server rol örneğinde izlemek istiyorsanız, sunucu tarafından filtre uygulayabilirsiniz.

![Örneklenen Canlı hataları](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK gereksinimleri
Özel Canlı ölçümleri Stream sürüm 2.4.0-beta2 bulunan ya da daha yeni olan [web için Application Insights SDK'sı](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). NuGet Paket Yöneticisi'nden "Öncesini" seçeneğini unutmayın.

## <a name="secure-the-control-channel"></a>Denetim kanalının güvenliğini sağlayın
Belirttiğiniz özel filtreler ölçütlere geri Application Insights SDK'sı Canlı ölçümleri bileşeni gönderilir. Filtreler, büyük olasılıkla customerIDs gibi hassas bilgiler içerebilir. Kanal güvenli izleme anahtarını ek olarak gizli bir API anahtarı ile yapabilirsiniz.
### <a name="create-an-api-key"></a>API anahtarı oluşturma

![API anahtarı oluşturma](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>API anahtarı yapılandırmanıza ekleyin

### <a name="classic-aspnet"></a>Klasik ASP.NET

Applicationınsights.config dosyasında AuthenticationApiKey için QuickPulseTelemetryModule ekleyin:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
Ya da isteğe bağlı olarak kod içinde QuickPulseTelemetryModule üzerinde ayarlayın:

```csharp
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;

             TelemetryConfiguration configuration = new TelemetryConfiguration();
            configuration.InstrumentationKey = "YOUR-IKEY-HERE";

            QuickPulseTelemetryProcessor processor = null;

            configuration.TelemetryProcessorChainBuilder
                .Use((next) =>
                {
                    processor = new QuickPulseTelemetryProcessor(next);
                    return processor;
                })
                        .Build();

            var QuickPulse = new QuickPulseTelemetryModule()
            {

                AuthenticationApiKey = "YOUR-API-KEY"
            };
            QuickPulse.Initialize(configuration);
            QuickPulse.RegisterTelemetryProcessor(processor);
            foreach (var telemetryProcessor in configuration.TelemetryProcessors)
                {
                if (telemetryProcessor is ITelemetryModule telemetryModule)
                    {
                    telemetryModule.Initialize(configuration);
                    }
                }

```

### <a name="aspnet-core-requires-application-insights-aspnet-core-sdk-230-beta-or-greater"></a>ASP.NET Core (Application Insights ASP.NET Core SDK gerektirir 2.3.0-beta veya üzeri)

Startup.cs dosyanız aşağıdaki gibi değiştirin:

İlk olarak ekleyin

``` C#
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
```

Ardından içinde Createservicereplicalisteners() yöntemi ekleyin:

``` C#
services.ConfigureTelemetryModule<QuickPulseTelemetryModule>( module => module.AuthenticationApiKey = "YOUR-API-KEY-HERE");
```


Ancak, tanıyacak ve tüm bağlı sunucular güven, kimliği doğrulanmış kanalı olmadan özel filtreler deneyebilirsiniz. Bu seçenek altı ay için kullanılabilir. Bu geçersiz kılma her yeni oturumun bir kez gereklidir veya ne zaman yeni bir sunucu gelir çevrimiçi.

![Canlı ölçümler kimlik doğrulama seçenekleri](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Filtre ölçütlerinde CustomerID gibi olası duyarlı bilgileri girmeden önce kimliği doğrulanmış kanalı ayarlamanızı öneririz.
>

## <a name="generating-a-performance-test-load"></a>Performans testi yük oluşturma

Yük artışı etkisini izlemek Performans Test dikey penceresini kullanın. Bu eşzamanlı kullanıcıların sayısı gelen istekleri benzetimini yapar. Ya da "el ile testler" çalıştırabilirsiniz (ping testleri) tek bir URL ya da çalıştırılabilir bir [çok adımlı web performans testi](app-insights-monitor-web-app-availability.md#multi-step-web-tests) (ile bir kullanılabilirlik testi aynı şekilde) yüklediğiniz.

> [!TIP]
> Performans testi oluşturduktan sonra test ve canlı Stream dikey ayrı bir pencerede açabilirsiniz. Sıraya alınan performans testi başladığında ve canlı akışı izleyin, aynı anda görebilirsiniz.
>


## <a name="troubleshooting"></a>Sorun giderme

Veri yok mu? Uygulamanız, korunan bir ağda ise: Canlı ölçümleri Stream diğer Application Insights telemetri değerinden farklı bir IP adresi kullanır. Emin [bu IP adreslerini](app-insights-ip-addresses.md) güvenlik duvarınızdan açıktır.



## <a name="next-steps"></a>Sonraki adımlar
* [Application Insights ile kullanımı izleme](app-insights-usage-overview.md)
* [Tanılama aramayı kullanma](app-insights-diagnostic-search.md)
* [Profil Oluşturucu](app-insights-profiler.md)
* [Anlık görüntü hata ayıklayıcısı](app-insights-snapshot-debugger.md)
