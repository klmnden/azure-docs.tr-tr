---
title: Akıllı algılama Azure Application ınsights | Microsoft Docs
description: Application Insights, uygulama telemetrinizde gerçekleştirilen derin analizden otomatik gerçekleştirir ve olası sorunları sizi uyarır.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: 0083c157eab489943f94ed1453c66a5c8d2f291a
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50960319"
---
# <a name="smart-detection-in-application-insights"></a>Application Insights, akıllı algılama
 Akıllı algılama, web uygulamanızdaki olası performans sorunlarını otomatik olarak sizi uyarır. Uygulamanızın gönderdiği telemetri öngörülü analiz gerçekleştirir [Application Insights](app-insights-overview.md). Hata oranları ani bir artış ya da istemci veya sunucu performans anormal desenleri ise bir uyarı alırsınız. Bu özellik, herhangi bir yapılandırma gerekir. Uygulamanızı yeterli telemetri gönderiyorsa çalışır.

Akıllı algılama uyarıları, hem aldığınız e-postaları ve akıllı algılama dikey penceresinden erişebilirsiniz.

## <a name="review-your-smart-detections"></a>Gözden geçirin, akıllı algılama
Algılamalar iki yolla bulabilir:

* **E-posta aldığınız** Application ınsights'tan. Tipik bir örnek aşağıda verilmiştir:
  
    ![E-posta uyarısı](./media/app-insights-proactive-diagnostics/03.png)
  
    Daha fazla ayrıntı Portalı'nda açmak için büyük düğmesine tıklayın.
* **Akıllı algılama kutucuk** uygulamanızın genel bakış dikey penceresinde son uyarıların sayısını gösterir. En son uyarıların bir listesi görmek için kutucuğa tıklayın.

![Görünümü son algılamalar](./media/app-insights-proactive-diagnostics/04.png)

Ayrıntılarını görmek için bir uyarı seçin.

## <a name="what-problems-are-detected"></a>Algılanan sorunları?
Algılama üç tür vardır:

* [Akıllı algılama - hata Anomalileri](app-insights-proactive-failure-diagnostics.md). Yük ve diğer faktörlere ile ilişkilendirmek, uygulamanız için başarısız isteklerin beklenen oranını ayarlamak için machine learning kullanırız. Hata oranı beklenen zarfının dışında kalırsa bir uyarı göndereceğiz.
* [Akıllı algılama - performans Anomalileri](app-insights-proactive-performance-diagnostics.md). Yanıt süresi bir işlemi ya da bağımlılık süresi geçmiş taban çizgisine göre yavaşlamasıdır veya yanıt süresi veya sayfa yükleme süresi anormal bir düzen tanımlamanız durumunda bildirim alın.   
* [Akıllı algılama - Azure bulut hizmeti sorunları](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Uygulamanızı Azure Cloud Services'da barındırılan ve rol örneği başlatma hataları, sık geri dönüştürme veya çalışma zamanı kilitlenmeleri varsa uyarı alın.

(Her bildirim Yardım bağlantıları için ilgili makaleleri göz önüne almanız.)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemenize yardımcı:

* [Ölçüm Gezgini](app-insights-metrics-explorer.md)
* [Arama Gezgini](app-insights-diagnostic-search.md)
* [Analytics - güçlü sorgu dili](../log-analytics/query-language/get-started-analytics-portal.md)

Akıllı algılama tamamen otomatik olarak gerçekleşir. Ancak belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılan ölçüm uyarıları](app-insights-alerts.md)
* [Kullanılabilirlik web testleri](app-insights-monitor-web-app-availability.md) 

