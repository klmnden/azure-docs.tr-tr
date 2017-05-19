---
title: "Windows masaüstü uygulamaları için kullanımı ve performansı izleme"
description: "HockeyApp ve Application Insights ile Windows masaüstü uygulamanızın kullanımını ve performansını analiz edin."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 19040746-3315-47e7-8c60-4b3000d2ddc4
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 35ca040ed123f6330f09f7fb1bc6be9ddaf61808
ms.contentlocale: tr-tr
ms.lasthandoff: 04/07/2017


---
# <a name="monitoring-usage-and-performance-in-windows-desktop-apps"></a>Windows Masaüstü uygulamalarında kullanımı ve performansı izleme


[Azure Application Insights](app-insights-overview.md) ve [HockeyApp](https://hockeyapp.net), dağıttığınız uygulamanın kullanımını ve performansını izlemenize imkan tanır.

> [!IMPORTANT]
> Masaüstü ve cihaz uygulamalarını dağıtmak için [HockeyApp](https://hockeyapp.net)’i öneririz. HockeyApp ile dağıtımı, canlı testleri ve kullanıcı geri bildirimini yönetmenin yanı sıra kullanımı ve kilitlenme raporlarını izleyebilirsiniz. Ayrıca, [telemetrinizi dışarı aktarıp Analytics ile sorgulayabilirsiniz](app-insights-hockeyapp-bridge-app.md).
> 
> Bir masaüstü uygulamasından Application Insights’a telemetri gönderilebilir, ancak bu çoğunlukla hata ayıklama ve deneme amaçları için kullanışlıdır.
> 
> 

## <a name="to-send-telemetry-to-application-insights-from-a-windows-application"></a>Bir Windows uygulamasından Application Insights’a telemetri göndermek için
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
```C#

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


