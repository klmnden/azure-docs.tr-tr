---
title: "Azure Application Insights algılama akıllı | Microsoft Docs"
description: "Application Insights uygulama telemetrinin otomatik derin çözümleme yapar ve olası sorunları sizi uyarır."
services: application-insights
documentationcenter: windows
author: rakefetj
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: f203b2a532ea721d9797c67a4750896e3ab2b9f7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="smart-detection-in-application-insights"></a>Application ınsights'ta akıllı algılama
 Akıllı algılama, web uygulamanızın olası performans sorunları otomatik olarak sizi uyarır. Uygulamanızı gönderdiği telemetriyi öngörülü analizini gerçekleştirir [Application Insights](app-insights-overview.md). Başarısızlık oranları ani bir artışa veya istemci veya sunucu performans anormal desenlerini ise bir uyarı alırsınız. Bu özellik, herhangi bir yapılandırma gerekir. Uygulamanız yeterli telemetri gönderirse çalışır.

Akıllı algılama uyarıları hem aldığınız e-postalar ve akıllı algılama dikey penceresinden erişebilirsiniz.

## <a name="review-your-smart-detections"></a>Akıllı algılamaların gözden geçirin
İki yolla algılamaların bulabilir:

* **Bir e-posta aldığınız** Application Insights gelen. Aşağıda, genel bir örnek verilmiştir:
  
    ![E-posta uyarısı](./media/app-insights-proactive-diagnostics/03.png)
  
    Daha fazla ayrıntı Portalı'nda açmak için büyük düğmesini tıklatın.
* **Akıllı algılama döşeme** uygulamanızın bir genel bakış dikey son uyarıların sayısını gösterir. Son uyarıları listesini görmek için kutucuğa tıklayın.

![Görünümü en son algılama](./media/app-insights-proactive-diagnostics/04.png)

Ayrıntılarını görmek için bir uyarı seçin.

## <a name="what-problems-are-detected"></a>Hangi sorunlar algılandığında?
Algılama üç tür vardır:

* [Algılama - hatası anormallikleri akıllı](app-insights-proactive-failure-diagnostics.md). Yük ve diğer etkenlere bağlı ile ilişkilendirerek machine learning, uygulamanız için başarısız istekleri beklenen oranını ayarlamak kullanırız. Hata oranı beklenen zarfının dışında kalırsa, size bir uyarı göndereceğiz.
* [Algılama - performans Anormalliklerini akıllı](app-insights-proactive-performance-diagnostics.md). Yanıt süresi bir işlem veya bağımlılık süresi geçmiş taban çizgisine göre yavaşlamadan veya yanıt süresi veya sayfa yükleme süresi anormal bir desen tanımlamak bildirimler alın.   
* [Akıllı algılama - Azure bulut hizmeti sorunları](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Uygulamanızı Azure bulut Hizmetleri'nde barındırılan ve rol örneği başlatma hataları, sık sık geri dönüştürülüyor veya çalışma zamanı çökme (Crash) varsa, uyarılar alırsınız.

(Her bir bildirim Yardım bağlantıları ilgili makaleler için atmanız.)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemek yardımcı olur:

* [Ölçüm Gezgini](app-insights-metrics-explorer.md)
* [Arama Gezgini](app-insights-diagnostic-search.md)
* [Analizi - güçlü sorgu dili](app-insights-analytics-tour.md)

Akıllı algılama tamamen otomatik olarak yapılır. Ancak, belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılmış ölçüm uyarıları](app-insights-alerts.md)
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) 

