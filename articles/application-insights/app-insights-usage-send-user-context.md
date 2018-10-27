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
ms.date: 08/02/2017
ms.reviewer: abgreg;mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: bfb04f596a61ff79c75cd38473c9480a29b0e6c4
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139899"
---
#  <a name="send-user-context-ids-to-enable-usage-experiences-in-azure-application-insights"></a>Azure Application ınsights'ta kullanım deneyimlerini etkinleştirmelerine olanak kimlikleri kullanıcı bağlamı gönderme

## <a name="tracking-users"></a>Kullanıcılar izleme

Application Insights izlemek ve ürün kullanım araçlar kümesi yoluyla kullanıcılarınıza izlemenize olanak sağlar: 
* [Kullanıcılar, Oturumlar, Etkinlikler](https://docs.microsoft.com/azure/application-insights/app-insights-usage-segmentation)
* [Huniler](https://docs.microsoft.com/azure/application-insights/usage-funnels)
* [Bekletme](https://docs.microsoft.com/azure/application-insights/app-insights-usage-retention)
* Kohortlar
* [Çalışma kitapları](https://docs.microsoft.com/azure/application-insights/app-insights-usage-workbooks)

Zaman içinde bir kullanıcı ne yaptığını izlemek için her bir kullanıcı veya oturum için Application Insights bir kimliği gerekir. Aşağıdaki kimlikler her özel olay veya sayfa görünümünde içerir.
- Kullanıcılar, Huniler, bekletme ve Kohortlar: kullanıcı kimliğini içerir
- Oturumlarının: oturum kimliği Ekle

Uygulamanız ile tümleşikse [JavaScript SDK'sı](https://docs.microsoft.com/azure/application-insights/app-insights-javascript#set-up-application-insights-for-your-web-page), kullanıcı kimliği otomatik olarak izlenir.

## <a name="choosing-user-ids"></a>Kullanıcı kimliklerini seçme

Kullanıcı kimliklerini kullanıcıları zaman içinde nasıl davranacağını izlemek için kullanıcı oturumları arasında kalıcı olması. Kimliği kalıcı hale getirmeniz için çeşitli yaklaşımlar bulunmaktadır.
- Zaten sahip olduğunuz hizmetinizde kullanıcı tanımı.
- Hizmet, bir tarayıcı erişimi varsa, bu tarayıcı bir tanımlama bilgisi kimliği içine geçirebilirsiniz. Tanımlama bilgisi kullanıcının tarayıcısında kaldığı sürece kimliği için açık kalır.
- Gerekirse, yeni bir kimliği her oturumda kullanabilirsiniz, ancak kullanıcılar hakkında sonuçları sınırlı olacaktır. Örneğin, bir kullanıcının davranışını zamanla nasıl değiştiğini görmek mümkün olmayacaktır.

Kimliği bir GUID veya başka bir dize yeterince karmaşık her kullanıcıyı benzersiz şekilde tanımlamak için olmalıdır. Örneğin, uzun rastgele bir sayı olabilir.

Kimliği kullanıcıyla ilgili kişisel tanımlama bilgileri içeriyorsa, bu Application ınsights'ı bir kullanıcı kimliği olarak göndermek için uygun bir değer değil Böyle bir kimlik olarak gönderdiğiniz bir [kimliği doğrulanmış kullanıcı kimliği](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#authenticated-users), ancak kullanım senaryoları için kullanıcı kimliği gereksinimi karşılamıyordur.

## <a name="aspnet-apps-setting-the-user-context-in-an-itelemetryinitializer"></a>ASP.NET uygulamaları: kullanıcı bağlamı içinde bir ITelemetryInitializer ayarlama

Ayrıntılı olarak açıklandığı bir telemetri Başlatıcısı oluşturma [burada](https://docs.microsoft.com/azure/application-insights/app-insights-api-filtering-sampling#add-properties-itelemetryinitializer), Context.User.Id ve Context.Session.Id ayarlayın.

Bu örnek, süresi dolan sonra oturumun tanımlayıcı kullanıcı Kimliğini ayarlar. Mümkünse, oturumu arasında kalıcıdır bir kullanıcı kimliği kullanın.

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
- Kullanım deneyimlerini etkinleştirmek için göndermeye başlayın [özel olaylar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) veya [sayfa görünümleri](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Özel olay veya sayfa görüntülemesi zaten gönderirseniz, kullanıcıların hizmetinizin nasıl öğrenmek için kullanım araçları keşfedin.
    * [Kullanıma genel bakış](app-insights-usage-overview.md)
    * [Kullanıcılar, oturumlar ve olaylar](app-insights-usage-segmentation.md)
    * [Huniler](usage-funnels.md)
    * [Bekletme](app-insights-usage-retention.md)
    * [Çalışma kitapları](app-insights-usage-workbooks.md)
