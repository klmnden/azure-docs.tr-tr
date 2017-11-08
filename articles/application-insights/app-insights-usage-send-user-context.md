---
title: "Kullanıcı bağlamı kullanımını etkinleştirmek için kimlikleri karşılaştığında Azure Application Insights'ta gönderme | Microsoft Docs"
description: "Bunların her biri benzersiz, kalıcı bir kimlik dizesi Application ınsights'ta atayarak kullanıcıların hizmeti aracılığıyla nasıl hareket izler."
services: application-insights
documentationcenter: 
author: abgreg
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: csharp
ms.topic: article
ms.date: 08/02/2017
ms.author: mbullwin
ms.openlocfilehash: e872062eddd4ae74f6148673a8f0b27751e37ca4
ms.sourcegitcommit: 0930aabc3ede63240f60c2c61baa88ac6576c508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2017
---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Kullanıcı bağlamı Azure Application Insights kullanımı deneyimleri etkinleştirmek için kimlikleri Gönder

## <a name="tracking-users"></a>Kullanıcıları izleme

Application Insights izleme ve kullanıcılarınızın ürün kullanım araçlar aracılığıyla izlemenize olanak sağlar: 
* [Kullanıcılar, Oturumlar, Etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Huniler](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Bekletme](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Cohorts
* [Çalışma kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Zaman içinde bir kullanıcının ne yaptığını izlemek için her kullanıcı veya oturum için bir kimlik Application Insights gerekir. Aşağıdaki kimlikleri her özel olay veya sayfa görünümünde içerir.
- Kullanıcılar, Funnels, bekletme ve Cohorts: kullanıcı kimliğini de ekleyin.
- Oturumlar: oturum kimliğini de ekleyin.

Uygulamanız ile tümleşik çalışıyorsa [JavaScript SDK'sı](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), kullanıcı Kimliğini otomatik olarak izlenir.

## <a name="choosing-user-ids"></a>Kullanıcı kimliklerini seçme

Kullanıcı kimliklerini kullanıcılar zaman içinde nasıl davranacağını izlemek için kullanıcı oturumları arasında kalıcı olması. Kimliği sürdürmek için çeşitli yaklaşım vardır
- Hizmetinizi zaten bir kullanıcı tanımı.
- Hizmet bir tarayıcı erişimi varsa, tarayıcı bir Kimliğe sahip bir tanımlama bilgisi içinde geçirebilirsiniz. Kullanıcının tarayıcıda tanımlama bilgisi kaldığı sürece kimliği için korunur.
- Gerekirse, yeni bir kimliği her oturum kullanabilirsiniz, ancak kullanıcılar hakkında sonuçları sınırlı olacaktır. Örneğin, bir kullanıcının davranışına zamanla nasıl değiştiğini görmek mümkün olmayacaktır.

Kimliği bir GUID veya başka bir dize yeterince karmaşık her kullanıcının benzersiz şekilde tanımlamak için olmalıdır. Örneğin, uzun rastgele bir sayı olabilir.

Kimliği kullanıcıyla ilgili kişisel tanımlama bilgileri içeriyorsa, bu Application Insights bir kullanıcı kimliği olarak göndermek için uygun bir değer değil Böyle bir kimlik olarak gönderdiğiniz bir [kimliği doğrulanmış kullanıcı kimliği](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ancak kullanım senaryoları için kullanıcı kimliği gereksinimi yerine getirmiyor.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET uygulamaları: kullanıcı bağlamı bir ITelemetryInitializer ayarlama

Ayrıntılı olarak açıklandığı gibi bir telemetri Başlatıcı oluşturun [burada](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), Context.User.Id ve Context.Session.Id ve ayarlayın.

Bu örnek, sonra oturum süresinin sona bir tanımlayıcı için kullanıcı Kimliğini ayarlar. Mümkünse, oturumlar arasında devam eden bir kullanıcı kimliği kullanın.

```C#

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
            // For a full experience, track each user across sessions. For an incomplete view of user 
            // behavior within a session, store user ID on the HttpContext Session.
            // Set the user ID if we haven't done so yet.
            if (HttpContext.Current.Session["UserId"] == null)
            {
                HttpContext.Current.Session["UserId"] = Guid.NewGuid();
            }

            // Set the user id on the Application Insights telemetry item.
            telemetry.Context.User.Id = (string)HttpContext.Current.Session["UserId"];

            // Set the session id on the Application Insights telemetry item.
            telemetry.Context.Session.Id = HttpContext.Current.Session.SessionID;
        }
      }
    }
```

## <a name="next-steps"></a>Sonraki adımlar
- Kullanımı deneyimleri etkinleştirmek için göndermeye Başla [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olaylar veya sayfa görünümleri zaten gönderirseniz, kullanıcıların hizmetinizin kullanımını öğrenmek için kullanım araçları keşfedin.
    * [Kullanıma genel bakış](app-insights-usage-overview.md)
    * [Kullanıcıları, oturumlar ve olaylar](app-insights-usage-segmentation.md)
    * [Huniler](usage-funnels.md)
    * [Bekletme](app-insights-usage-retention.md)
    * [Çalışma kitapları](app-insights-usage-workbooks.md)
