---
title: Kullanıcı bağlamı kimlikleri kullanımını etkinleştirmek için Azure Application Insights'ta deneyimleri gönderme | Microsoft Docs
description: Bunların her biri benzersiz, kalıcı bir kimlik dizesi Application ınsights'ta atayarak hizmetinizi kullanıcıları nasıl taşımak izleyin.
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: conceptual
ms.date: 01/03/2019
ms.reviewer: abgreg;mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: 7c458867b89a76a2f19bbd632c8a884c629f5765
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60371844"
---
# <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Azure Application ınsights'ta kullanım deneyimlerini etkinleştirmelerine olanak kimlikleri kullanıcı bağlamı gönderme

## <a name="tracking-users"></a>Kullanıcılar izleme

Application Insights izlemek ve ürün kullanım araçlar kümesi yoluyla kullanıcılarınıza izlemenize olanak sağlar:

- [Kullanıcılar, Oturumlar, Etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
- [Huniler](https://docs.microsoft.com/azure/application-insights/usage-funnels)
- [Bekletme](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention) Kohortlar
- [Çalışma kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Zaman içinde bir kullanıcı ne yaptığını izlemek için her bir kullanıcı veya oturum için Application Insights bir kimliği gerekir. Aşağıdaki kimlikler her özel olay veya sayfa görünümünde içerir.

- Kullanıcılar, Huniler, bekletme ve Kohortlar: Kullanıcı kimliğini içerir
- Oturumlar: Oturum kimliği Ekle

> [!NOTE]
> Bu, kullanıcı etkinliği Application Insights ile izleme için manuel adımlar anahat oluşturma, Gelişmiş bir makaledir. Birçok web uygulamalarıyla **adımları gerekli olmayabilir**, varsayılan sunucu tarafı olarak SDK'ları ile birlikte [tarayıcı/istemci-tarafı JavaScript SDK'sı](../../azure-monitor/app/website-monitoring.md ), genellikle otomatik olarak izlemek yeterli olur kullanıcı etkinliği. Yapılandırmadıysanız, [istemci-tarafı izleme](../../azure-monitor/app/website-monitoring.md ) sunucu tarafı SDK'sı ek olarak, öncelikle bunu ve kullanıcı davranış analizi araçları beklendiği gibi işlemi yapıyorsanız görmek için test edin.

## <a name="choosing-user-ids"></a>Kullanıcı kimliklerini seçme

Kullanıcı kimliklerini kullanıcıları zaman içinde nasıl davranacağını izlemek için kullanıcı oturumları arasında kalıcı olması. Kimliği kalıcı hale getirmeniz için çeşitli yaklaşımlar bulunmaktadır.

- Zaten sahip olduğunuz hizmetinizde kullanıcı tanımı.
- Hizmet, bir tarayıcı erişimi varsa, bu tarayıcı bir tanımlama bilgisi kimliği içine geçirebilirsiniz. Tanımlama bilgisi kullanıcının tarayıcısında kaldığı sürece kimliği için açık kalır.
- Gerekirse, yeni bir kimliği her oturumda kullanabilirsiniz, ancak kullanıcılar hakkında sonuçları sınırlı olacaktır. Örneğin, bir kullanıcının davranışını zamanla nasıl değiştiğini görmek mümkün olmayacaktır.

Kimliği bir GUID veya başka bir dize yeterince karmaşık her kullanıcıyı benzersiz şekilde tanımlamak için olmalıdır. Örneğin, uzun rastgele bir sayı olabilir.

Kimliği kullanıcıyla ilgili kişisel tanımlama bilgileri içeriyorsa, bu Application ınsights'ı bir kullanıcı kimliği olarak göndermek için uygun bir değer değil Böyle bir kimlik olarak gönderdiğiniz bir [kimliği doğrulanmış kullanıcı kimliği](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ancak kullanım senaryoları için kullanıcı kimliği gereksinimi karşılamıyordur.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET uygulamaları: Kullanıcı bağlamı içinde bir ITelemetryInitializer ayarlama

Ayrıntılı olarak açıklandığı bir telemetri Başlatıcısı oluşturma [burada](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer). Oturum kimliği istek telemetrisi aracılığıyla geçirin ve Context.User.Id Context.Session.Id ayarlayın.

Bu örnek, süresi dolan sonra oturumun tanımlayıcı kullanıcı Kimliğini ayarlar. Mümkünse, oturumu arasında kalıcıdır bir kullanıcı kimliği kullanın.

### <a name="telemetry-initializer"></a>Telemetri başlatıcısını

```csharp
using System;
using System.Web;
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.Extensibility;

namespace MvcWebRole.Telemetry
{
  /*
   * Custom TelemetryInitializer that sets the user ID.
   *
   */
  public class MyTelemetryInitializer : ITelemetryInitializer
  {
    public void Initialize(ITelemetry telemetry)
    {
        var ctx = HttpContext.Current;

        // If telemetry initializer is called as part of request execution and not from some async thread
        if (ctx != null)
        {
            var requestTelemetry = ctx.GetRequestTelemetry();
 
            // Set the user and session ids from requestTelemetry.Context.User.Id, which is populated in Application_PostAcquireRequestState in Global.asax.cs.
            if (requestTelemetry != null && !string.IsNullOrEmpty(requestTelemetry.Context.User.Id) &&
                (string.IsNullOrEmpty(telemetry.Context.User.Id) || string.IsNullOrEmpty(telemetry.Context.Session.Id)))
            {
                // Set the user id on the Application Insights telemetry item.
                telemetry.Context.User.Id = requestTelemetry.Context.User.Id;
 
                // Set the session id on the Application Insights telemetry item.
                telemetry.Context.Session.Id = requestTelemetry.Context.User.Id;
            }
        }
    }
  }
}
```

### <a name="globalasaxcs"></a>Global.asax.cs

```csharp
using System.Web;
using System.Web.Http;
using System.Web.Mvc;
using System.Web.Optimization;
using System.Web.Routing;

namespace MvcWebRole.Telemetry
{
    public class MvcApplication : HttpApplication
    {
        protected void Application_Start()
        {
            AreaRegistration.RegisterAllAreas();
            GlobalConfiguration.Configure(WebApiConfig.Register);
            FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);
            RouteConfig.RegisterRoutes(RouteTable.Routes);
            BundleConfig.RegisterBundles(BundleTable.Bundles);
        }
 
        protected void Application_PostAcquireRequestState()
        {
            var requestTelemetry = Context.GetRequestTelemetry();
 
            if (HttpContext.Current.Session != null && requestTelemetry != null && string.IsNullOrEmpty(requestTelemetry.Context.User.Id))
            {
                requestTelemetry.Context.User.Id = Session.SessionID;
            }
        }
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    - [Kullanıma genel bakış](usage-overview.md)
    - [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
    - [Huniler](usage-funnels.md)
    - [Bekletme](usage-retention.md)
    - [Çalışma kitapları](../../azure-monitor/app/usage-workbooks.md)
