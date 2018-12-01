---
title: Windows masaüstü uygulamaları için kullanımı ve performansı izleme
description: Application Insights ile Windows masaüstü uygulamanızın kullanımını ve performansını analiz edin.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: mbullwin
ms.openlocfilehash: 10044f17baee8fb4d7afe9223abdbb518952e71f
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52741867"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Klasik Windows Masaüstü uygulamalarında kullanımı ve performansı izleme

Şirket içinde, Azure’da ve diğer bulutlarda barındırılan tüm uygulamalar Application Insights’tan faydalanabilir. Tek sınırlama Application Insights hizmetine [iletişim izni verme](app-insights-ip-addresses.md) gerekliliğidir. Evrensel Windows Platformu (UWP) uygulamalarını izlemek için [Visual Studio App Center](app-insights-mobile-center-quickstart.md)’ı öneririz.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Bir Klasik Windows uygulamasından Application Insights’a telemetri göndermek için
1. [Azure portalında](https://portal.azure.com) [bir Application Insights kaynağı oluşturun](app-insights-create-new-resource.md). Uygulama türü olarak ASP.NET uygulamasını seçin.
2. İzleme Anahtarının bir kopyasını oluşturun. Yeni oluşturduğunuz kaynağın Temel Bileşenler açılan penceresinde anahtarı bulun. 
3. Visual Studio’da uygulama projenizin NuGet paketlerini düzenleyin ve şunu ekleyin: Microsoft.ApplicationInsights.WindowsServer. (Alternatif olarak, standart telemetri toplama modülleri olmaksızın yalnızca API’nın kendisini istiyorsanız Microsoft.ApplicationInsights seçeneğini belirleyin.)
4. İzleme anahtarını kodunuzda ayarlayın:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *anahtarınız* `";`
   
    veya ApplicationInsights.config öğesinde ayarlayın (standart telemetri paketlerinden birini yüklediyseniz):
   
    `<InstrumentationKey>`*anahtarınız*`</InstrumentationKey>` 
   
    ApplicationInsights.config dosyasını kullanırsanız, bunun özelliklerinin **Build Action = Content, Copy to Output Directory = Copy** olarak ayarlandığından emin olun.
5. Telemetri göndermek için [API’yi kullanın](app-insights-api-custom-events-metrics.md).
6. Uygulamanızı çalıştırdığınızda, oluşturduğunuz kaynaktaki telemetriyi Azure Portal’da görürsünüz.

## <a name="telemetry"></a>Örnek kod
```csharp

    public partial class Form1 : Form
    {
        private TelemetryClient tc = new TelemetryClient();
        ...
        private void Form1_Load(object sender, EventArgs e)
        {
            // Alternative to setting ikey in config file:
            tc.InstrumentationKey = "key copied from portal";

            // Set session data:
            tc.Context.User.Id = Environment.UserName;
            tc.Context.Session.Id = Guid.NewGuid().ToString();
            tc.Context.Device.OperatingSystem = Environment.OSVersion.ToString();

            // Log a page view:
            tc.TrackPageView("Form1");
            ...
        }

        protected override void OnClosing(CancelEventArgs e)
        {
            stop = true;
            if (tc != null)
            {
                tc.Flush(); // only for desktop apps

                // Allow time for flushing:
                System.Threading.Thread.Sleep(1000);
            }
            base.OnClosing(e);
        }

```

## <a name="next-steps"></a>Sonraki adımlar
* [Pano oluşturma](app-insights-dashboards.md)
* [Tanılama Araması](app-insights-diagnostic-search.md)
* [Ölçümleri keşfetme](app-insights-metrics-explorer.md)
* [Analytics sorguları yazma](app-insights-analytics.md)

