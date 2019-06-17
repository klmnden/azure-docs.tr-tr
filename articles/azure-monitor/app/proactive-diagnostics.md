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
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mbullwin
ms.openlocfilehash: edbd7000001ae6927078e2f1bb9e348cc78f9efa
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61299678"
---
# <a name="smart-detection-in-application-insights"></a>Application Insights, akıllı algılama
 Akıllı algılama, web uygulamanızdaki olası performans sorunlarını otomatik olarak sizi uyarır. Uygulamanızın gönderdiği telemetri öngörülü analiz gerçekleştirir [Application Insights](../../azure-monitor/app/app-insights-overview.md). Hata oranları ani bir artış ya da istemci veya sunucu performans anormal desenleri ise bir uyarı alırsınız. Bu özellik, herhangi bir yapılandırma gerekir. Uygulamanızı yeterli telemetri gönderiyorsa çalışır.

Akıllı algılama uyarıları, hem aldığınız e-postaları ve akıllı algılama dikey penceresinden erişebilirsiniz.

## <a name="review-your-smart-detections"></a>Gözden geçirin, akıllı algılama
Algılamalar iki yolla bulabilir:

* **E-posta aldığınız** Application ınsights'tan. Tipik bir örnek aşağıda verilmiştir:
  
    ![E-posta uyarısı](./media/proactive-diagnostics/03.png)
  
    Daha fazla ayrıntı Portalı'nda açmak için büyük düğmesine tıklayın.
* **Akıllı algılama kutucuk** uygulamanızın genel bakış dikey penceresinde son uyarıların sayısını gösterir. En son uyarıların bir listesi görmek için kutucuğa tıklayın.

![Görünümü son algılamalar](./media/proactive-diagnostics/04.png)

Ayrıntılarını görmek için bir uyarı seçin.

## <a name="what-problems-are-detected"></a>Algılanan sorunları?
Algılama üç tür vardır:

* [Akıllı algılama - hata Anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md). Yük ve diğer faktörlere ile ilişkilendirmek, uygulamanız için başarısız isteklerin beklenen oranını ayarlamak için machine learning kullanırız. Hata oranı beklenen zarfının dışında kalırsa bir uyarı göndereceğiz.
* [Akıllı algılama - performans Anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md). Yanıt süresi bir işlemi ya da bağımlılık süresi geçmiş taban çizgisine göre yavaşlamasıdır veya yanıt süresi veya sayfa yükleme süresi anormal bir düzen tanımlamanız durumunda bildirim alın.   
* [Akıllı algılama - Azure bulut hizmeti sorunları](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). Uygulamanızı Azure Cloud Services'da barındırılan ve rol örneği başlatma hataları, sık geri dönüştürme veya çalışma zamanı kilitlenmeleri varsa uyarı alın.

(Her bildirim Yardım bağlantıları için ilgili makaleleri göz önüne almanız.)

## <a name="smart-detection-email-notifications"></a>Akıllı algılama e-posta bildirimleri

Önizleme olarak işaretlenmiş kuralları hariç tüm akıllı algılama kuralları algılamalar bulunduğunda, e-posta bildirimleri göndermek için varsayılan olarak yapılandırılır.

Belirli bir akıllı algılama kuralına için e-posta bildirimlerini yapılandırma yapılabilir akıllı algılama açarak **ayarları** dikey penceresinde ve açılır kuralı seçmeye **düzenleme kuralı** dikey penceresi.

Alternatif olarak, Azure Resource Manager şablonlarını kullanarak yapılandırmasını değiştirebilirsiniz. [Azure Resource Manager şablonlarını kullanarak Application ınsights'ı yönetme akıllı algılama kurallarını](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config) daha fazla ayrıntı için.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Sonraki adımlar
Bu tanılama araçları, uygulamanızdan alınan telemetri incelemenize yardımcı:

* [Ölçüm Gezgini](../../azure-monitor/app/metrics-explorer.md)
* [Arama Gezgini](../../azure-monitor/app/diagnostic-search.md)
* [Analytics - güçlü sorgu dili](../../azure-monitor/log-query/get-started-portal.md)

Akıllı algılama tamamen otomatik olarak gerçekleşir. Ancak belki de daha fazla bazı uyarıları ayarlamak ister misiniz?

* [El ile yapılandırılan ölçüm uyarıları](../../azure-monitor/app/alerts.md)
* [Kullanılabilirlik web testleri](../../azure-monitor/app/monitor-web-app-availability.md) 

