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
ms.openlocfilehash: 567163a5d5ce37eeffb5ef2bc6f9adb7c5b027ec
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66255730"
---
# <a name="monitoring-usage-and-performance-in-classic-windows-desktop-apps"></a>Klasik Windows Masaüstü uygulamalarında kullanımı ve performansı izleme

Şirket içinde, Azure’da ve diğer bulutlarda barındırılan tüm uygulamalar Application Insights’tan faydalanabilir. Tek sınırlama Application Insights hizmetine [iletişim izni verme](../../azure-monitor/app/ip-addresses.md) gerekliliğidir. Evrensel Windows Platformu (UWP) uygulamalarını izlemek için [Visual Studio App Center](../../azure-monitor/learn/mobile-center-quickstart.md)’ı öneririz.

## <a name="to-send-telemetry-to-application-insights-from-a-classic-windows-application"></a>Bir Klasik Windows uygulamasından Application Insights’a telemetri göndermek için
1. [Azure portalında](https://portal.azure.com) [bir Application Insights kaynağı oluşturun](../../azure-monitor/app/create-new-resource.md ). Uygulama türü olarak ASP.NET uygulamasını seçin.
2. İzleme Anahtarının bir kopyasını oluşturun. Yeni oluşturduğunuz kaynağın Temel Bileşenler açılan penceresinde anahtarı bulun. 
3. Visual Studio’da uygulama projenizin NuGet paketlerini düzenleyin ve şunu ekleyin: Microsoft.ApplicationInsights.WindowsServer. (Alternatif olarak, standart telemetri toplama modülleri olmaksızın yalnızca API’nın kendisini istiyorsanız Microsoft.ApplicationInsights seçeneğini belirleyin.)
4. İzleme anahtarını kodunuzda ayarlayın:
   
    `TelemetryConfiguration.Active.InstrumentationKey = "` *anahtarınız* `";`
   
    veya ApplicationInsights.config öğesinde ayarlayın (standart telemetri paketlerinden birini yüklediyseniz):
   
    `<InstrumentationKey>`*anahtarınız*`</InstrumentationKey>` 
   
    ApplicationInsights.config dosyasını kullanırsanız, bunun özelliklerinin **Build Action = Content, Copy to Output Directory = Copy** olarak ayarlandığından emin olun.
5. Telemetri göndermek için [API’yi kullanın](../../azure-monitor/app/api-custom-events-metrics.md).
6. Uygulamanızı çalıştırın ve Azure portalında oluşturduğunuz kaynaktaki telemetriyi görebilirsiniz.

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
* [Pano oluşturma](../../azure-monitor/app/overview-dashboard.md)
* [Tanılama Araması](../../azure-monitor/app/diagnostic-search.md)
* [Ölçümleri keşfetme](../../azure-monitor/app/metrics-explorer.md)
* [Analytics sorguları yazma](../../azure-monitor/app/analytics.md)

