---
title: Ölçümleri akışı özel ölçümleri ve Tanılama, Azure Application Insights ile canlı | Microsoft Docs
description: Özel ölçümleri ile gerçek zamanlı web uygulamanızı izlemek ve canlı akış hataları, izlemeleri ve olayları ile ilgili sorunları tanılamak.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 05/24/2017
ms.author: mbullwin; Soubhagya.Dash
ms.openlocfilehash: 49b343fca94e853a29807521f4213a5a85725f52
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="live-metrics-stream-monitor--diagnose-with-1-second-latency"></a>Canlı ölçümleri akış: 1 saniye gecikme süresi ile Tanıla & zle 

Canlı ölçümleri akıştan kullanarak canlı, üretim web uygulamanızın göndermeyi Kalp araştırma [Application Insights](app-insights-overview.md). Seçin ve tüm rahatsızlık hizmetinize olmadan gerçek zamanlı olarak izlemek için ölçümleri ve performans sayaçlarını filtreleyin. Yığın izlemeleri örnek başarısız istekler ve özel durumları inceleyin. İle birlikte [profil oluşturucu](app-insights-profiler.md), [anlık görüntü hata ayıklayıcı](app-insights-snapshot-debugger.md), ve [performans testi](app-insights-monitor-web-app-availability.md#performance-tests), Canlı ölçümleri akış için canlı web güçlü ve bozucu bir tanılama aracı sağlar Site.

Canlı ölçümleri akış ile şunları yapabilirsiniz:

* Serbest olsa da, bir düzeltme doğrulamak performans ve hata sayısını izlerken tarafından.
* Sorunlarını tanılamak ve test yükleri etkisini izleme Canlı. 
* Belirli test oturumlarını odaklanmak veya seçerek ve izlemek istediğiniz ölçümleri filtreleme bilinen sorunlar filtreleyebilirsiniz.
* Özel durum izlemeleri olduğu alın.
* En uygun KPI'ları Bul filtrelerle denemeler yapın.
* Performans sayacı Canlı herhangi bir Windows izleyin.
* Kolayca sorunlar ve bu sunucuya yalnızca tüm KPI/canlı akış filtre sahip bir sunucu tanımlayın.

[![Canlı ölçümleri akış video](./media/app-insights-live-stream/youtube.png)](https://www.youtube.com/watch?v=zqfHf1Oi5PY)

Ölçümler bir canlı akışı ASP.NET uygulamalarını çalıştıran şirket içi veya bulutta şu anda kullanılabilir değil. 

## <a name="get-started"></a>başlarken

1. Henüz yapmadıysanız [Application Insights yüklü](app-insights-asp-net.md) , ASP.NET web uygulamanızda veya [Windows server uygulaması](app-insights-windows-services.md), bunu şimdi yapın. 
2. **En son sürüme güncelleştirin** Application Insights paketi. Visual Studio'da, projenize sağ tıklayın ve seçin **Manage Nuget paketleri**. Açık **güncelleştirmeleri** sekmesi, onay **dahil et**ve tüm Microsoft.ApplicationInsights.* paketleri seçin.

    Uygulamanızı yeniden dağıtın.

3. İçinde [Azure portal](https://portal.azure.com), uygulamanız için Application Insights kaynağı açın ve ardından canlı akış'ı açın.

4. [Denetim kanalı güvenli](#secure-the-control-channel) müşteri adları gibi hassas verileri, filtrelerinizi kullandığınız.


![Genel Bakış dikey penceresinde, canlı akış'a tıklayın.](./media/app-insights-live-stream/live-stream-2.png)

### <a name="no-data-check-your-server-firewall"></a>Veri yok mu? Sunucu güvenlik duvarının denetleyin

Denetleme [bağlantı noktaları için Canlı ölçümleri akış giden](app-insights-ip-addresses.md#outgoing-ports) sunucularınızın Güvenlik Duvarı'nda açıktır. 


## <a name="how-does-live-metrics-stream-differ-from-metrics-explorer-and-analytics"></a>Ölçümler bir canlı akışı nasıl ölçüm Gezgini ve analizi farkı nedir?

| |Canlı Akış | Ölçüm Gezgini ve analizi |
|---|---|---|
|Gecikme süresi|Bir saniye içinde görüntülenen verileri|Dakika boyunca bir araya getirilir|
|Hiçbir bekletme|Grafikte olduğu ve sonra atılır verileri devam eder.|[90 gün boyunca tutulur veri](app-insights-data-retention-privacy.md#how-long-is-the-data-kept)|
|İsteğe bağlı|Canlı ölçümleri açarken veri akışı|SDK'ın yüklü ve etkin olduğunda veriler gönderilir|
|Ücretsiz|Canlı akış verileri için herhangi bir ücret alınmaz|Konusu [fiyatlandırma](app-insights-pricing.md)
|Örnekleme|Seçilen tüm ölçümler ve sayaçlar aktarılır. Hataları ve Yığın izlemeleri örneklenen. TelemetryProcessors uygulanmaz.|Olayları olabilir [örneklenen](app-insights-api-filtering-sampling.md)|
|Denetim kanalı|Filtre denetimi sinyalleri SDK gönderilir. Öneririz [bu kanal güvenli](#secure-channel).|Portala tek yönlü iletişim|


## <a name="select-and-filter-your-metrics"></a>Seçin ve ölçümlerinizi filtre

(Klasik ASP.NET uygulamalarını son SDK'sı ile kullanılabilir.)

Portaldan herhangi Application Insights telemetriyi rasgele filtreleri uygulayarak özel KPI dinamik izleyebilirsiniz. Ne zaman, fare aşağıdakilerden grafikleri üzerinde gösteren filtre denetimi tıklatın. Aşağıdaki grafikte URL ve süre öznitelikleri filtreleri ile özel bir istek sayısı KPI Çizdirmek. Bir canlı akış zamanında herhangi bir noktada belirtilen ölçütlerle eşleşen telemetri gösteren akış Önizleme bölümüyle filtrelerinizi doğrulayın. 

![Özel istek KPI](./media/app-insights-live-stream/live-stream-filteredMetric.png)

Sayısını başka bir değer izleyebilirsiniz. Seçenekler herhangi Application Insights telemetri olabilir akış türüne bağlıdır: istekleri, bağımlılıkları, özel durumlar, izlemeleri, olayları veya ölçümleri. Bu, kendi olabilir [özel ölçüm](app-insights-api-custom-events-metrics.md#properties):

![Değer seçenekleri](./media/app-insights-live-stream/live-stream-valueoptions.png)

Application Insights telemetri yanı sıra akış seçeneklerden birini seçerek ve performans sayacının adını sağlayan tüm Windows performans sayacı da izleyebilirsiniz.

Canlı ölçümleri iki noktalarda toplanır: her sunucuda yerel olarak ve tüm sunucular genelinde. Varsayılan değer olarak ilgili aşağı açılan listeler diğer seçenekleri belirleyerek ya da değiştirebilirsiniz.

## <a name="sample-telemetry-custom-live-diagnostic-events"></a>Örnek Telemetri: Özel Canlı Tanılama Olayları
Varsayılan olarak, olayların canlı akış başarısız isteklerin ve bağımlılık çağrıları, özel durumlar, olayları ve izlemeleri örnekleri gösterilir. Zamanında uygulanan ölçütlerini herhangi bir noktada görmek için filtre simgesine tıklayın. 

![Varsayılan Canlı akış](./media/app-insights-live-stream/live-stream-eventsdefault.png)

Olarak ölçümleri ile Application Insights telemetri türlerinden herhangi birini için herhangi bir rastgele ölçüt belirtebilirsiniz. Bu örnekte, biz belirli bir istek hataları, izlemeler ve olaylar seçmiş olursunuz. Biz de tüm özel durumlar ve bağımlılık hataları seçmiş olursunuz.

![Özel canlı akış](./media/app-insights-live-stream/live-stream-events.png)

Not: Özel durum iletisi tabanlı ölçütü için şu anda en dıştaki özel durum iletisi kullanın. İç özel durum iletisi zararsız durumla filtrelemek için önceki örnekte (aşağıdaki "<--" sınırlayıcı) "istemci bağlantısı kesildi." Kullanım bir ileti değil-"İstek içeriği okunurken hata oluştu" ölçütler içeriyor.

Bir öğenin Canlı akıştaki ayrıntılarını tıklayarak bakın. Akış tıklayarak ya da duraklatabilir **duraklatma** yalnızca aşağı kaydırma veya bir öğeyi tıklatarak. Başa dön veya durdurulduğundan sırasında toplanan öğeleri sayaç tıklayarak kaydırma sonra canlı akış devam eder.

![Örneklenen Canlı hataları](./media/app-insights-live-stream/live-metrics-eventdetail.png)

## <a name="filter-by-server-instance"></a>Sunucu örneği tarafından filtre

Belirli bir sunucu rolü örneği izlemek istiyorsanız, sunucu tarafından filtre uygulayabilirsiniz.

![Örneklenen Canlı hataları](./media/app-insights-live-stream/live-stream-filter.png)

## <a name="sdk-requirements"></a>SDK gereksinimleri
Özel Canlı ölçümleri akış sürüm 2.4.0-beta2 bulunan ya da daha yeni olan [Application Insights SDK Web](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/). NuGet Paket Yöneticisi'nden "Yayın öncesi Ekle" seçeneğini unutmayın.

## <a name="secure-the-control-channel"></a>Denetim kanalının güvenliğini sağlayın.
Belirttiğiniz özel filtreler ölçütlere geri Application Insights SDK'sı Canlı ölçümleri bileşeni gönderilir. Filtreler olası customerIDs gibi hassas bilgiler içerebilir. Kanal güvenli izleme anahtarını ek olarak gizli bir API anahtarı ile yapabilirsiniz.
### <a name="create-an-api-key"></a>API anahtarı oluşturma

![API anahtarı oluşturma](./media/app-insights-live-stream/live-metrics-apikeycreate.png)

### <a name="add-api-key-to-configuration"></a>API anahtarı yapılandırmasına ekleyin

# <a name="net-standardtabnet-standard"></a>[.NET Standard](#tab/.net-standard)

Applicationınsights.config dosyasında AuthenticationApiKey QuickPulseTelemetryModule ekleyin:
``` XML

<Add Type="Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse.QuickPulseTelemetryModule, Microsoft.AI.PerfCounterCollector">
      <AuthenticationApiKey>YOUR-API-KEY-HERE</AuthenticationApiKey>
</Add>

```
Veya isteğe bağlı olarak kod içinde üzerinde QuickPulseTelemetryModule ayarlayın:

``` C#

    module.AuthenticationApiKey = "YOUR-API-KEY-HERE";

```
# <a name="net-core-tabnet-core"></a>[.NET core] (# sekmesini/.net-çekirdek)

Haline dosyanız aşağıdaki gibi değiştirin:

Önce ekleyin.

``` C#
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector.QuickPulse;
using Microsoft.ApplicationInsights.Extensibility;
```

Daha sonra altında yapılandırma yöntemi ekleyin:

``` C#
  QuickPulseTelemetryModule dep;
            var modules = app.ApplicationServices.GetServices<ITelemetryModule>();
            foreach (var module in modules)
            {
                if (module is QuickPulseTelemetryModule)
                {
                    dep = module as QuickPulseTelemetryModule;
                    dep.AuthenticationApiKey = "YOUR-API-KEY-HERE";
                    dep.Initialize(TelemetryConfiguration.Active);
                }
            }
```

---

Ancak, algılar ve tüm bağlı sunucular güveniyorsanız, kimliği doğrulanmış kanal olmadan özel filtreler deneyebilirsiniz. Bu seçenek altı ay için kullanılabilir. Bu geçersiz kılma kez her yeni bir oturum gereklidir veya ne zaman yeni bir sunucu gelir çevrimiçi.

![Canlı ölçümleri kimlik doğrulama seçenekleri](./media/app-insights-live-stream/live-stream-auth.png)

>[!NOTE]
>Kimliği doğrulanmış kanalının filtre ölçütünü CustomerID gibi olası hassas bilgileri girmeden önce ayarlamak öneririz.
>

## <a name="generating-a-performance-test-load"></a>Performans testi yükleme oluşturma

Bir yük artışı etkisini izlemek isterseniz, performansı Test dikey penceresini kullanın. Bu, eşzamanlı kullanıcıların sayısı gelen istekleri benzetimini yapar. Ya da "el ile testleri" çalıştırabilirsiniz (ping testleri) tek bir URL veya çalıştırabilirsiniz bir [çok adımlı web performans testi](app-insights-monitor-web-app-availability.md#multi-step-web-tests) (ile bir kullanılabilirlik test aynı şekilde) karşıya.

> [!TIP]
> Performans testi oluşturduktan sonra ayrı windows test ve canlı akış dikey penceresini açın. Sıraya alınan performans testi başladığında ve izleme canlı akış aynı anda görebilirsiniz.
>


## <a name="troubleshooting"></a>Sorun giderme

Veri yok mu? Uygulamanızı korunan bir ağda ise: Canlı ölçümleri akış diğer Application Insights telemetri daha farklı bir IP adreslerini kullanır. Emin olun [bu IP adreslerinden](app-insights-ip-addresses.md) duvarınızda açıktır.



## <a name="next-steps"></a>Sonraki adımlar
* [Application Insights ile kullanım izleme](app-insights-web-track-usage.md)
* [Tanılama aramayı kullanma](app-insights-diagnostic-search.md)
* [Profil Oluşturucu](app-insights-profiler.md)
* [Anlık görüntü hata ayıklayıcı](app-insights-snapshot-debugger.md)
