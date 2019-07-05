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
ms.openlocfilehash: 8ee2dea364253d871d5624242d15d8a81ab6f08f
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465906"
---
# <a name="smart-detection-in-application-insights"></a>Application Insights, akıllı algılama
 Akıllı algılama, olası performans sorunlarını ve web uygulamanızda hata anomalileri otomatik olarak sizi uyarır. Uygulamanızın gönderdiği telemetri öngörülü analiz gerçekleştirir [Application Insights](../../azure-monitor/app/app-insights-overview.md). Hata oranları ani bir artış ya da istemci veya sunucu performans anormal desenleri ise bir uyarı alırsınız. Bu özellik, herhangi bir yapılandırma gerekir. Uygulamanızı yeterli telemetri gönderiyorsa çalışır.

Akıllı algılama tarafından hem aldığınız e-postaları ve akıllı algılama dikey penceresinden verilen algılamalar erişebilirsiniz.

## <a name="review-your-smart-detections"></a>Gözden geçirin, akıllı algılama
Algılamalar iki yolla bulabilir:

* **E-posta aldığınız** Application ınsights'tan. Tipik bir örnek aşağıda verilmiştir:
  
    ![E-posta uyarısı](./media/proactive-diagnostics/03.png)
  
    Daha fazla ayrıntı Portalı'nda açmak için büyük düğmesine tıklayın.
* **Akıllı algılama dikey** Application ınsights. Seçin **akıllı algılama** altında **Araştır** son algılamalar listesini görmek için menü.

![Görünümü son algılamalar](./media/proactive-diagnostics/04.png)

Bir algılama ayrıntılarını görmek için seçin.

## <a name="what-problems-are-detected"></a>Algılanan sorunları?
Akıllı algılama algılar ve sorunları, çeşitli hakkında gibi bildirir:

* [Akıllı algılama - hata Anomalileri](../../azure-monitor/app/proactive-failure-diagnostics.md). Yük ve diğer faktörlere ile ilişkilendirmek, uygulamanız için başarısız isteklerin beklenen oranını ayarlamak için machine learning kullanırız. Hata oranı beklenen zarfının dışında kalırsa bir uyarı göndereceğiz.
* [Akıllı algılama - performans Anomalileri](../../azure-monitor/app/proactive-performance-diagnostics.md). Yanıt süresi bir işlemi ya da bağımlılık süresi geçmiş taban çizgisine göre yavaşlamasıdır veya yanıt süresi veya sayfa yükleme süresi anormal bir düzen tanımlamanız durumunda bildirim alın.   
* Genel performans düşüşü yaşanması ve sorunları gibi [izleme düşüşleri](https://docs.microsoft.com/azure/azure-monitor/app/proactive-trace-severity), [bellek sızıntısı](https://docs.microsoft.com/azure/azure-monitor/app/proactive-potential-memory-leak), [Abnormal özel durum hacminde artış](https://docs.microsoft.com/azure/azure-monitor/app/proactive-exception-volume) ve [Güvenliktersdesenler](https://docs.microsoft.com/azure/azure-monitor/app/proactive-application-security-detection-pack).

(Her bildirim Yardım bağlantıları için ilgili makaleleri göz önüne almanız.)

## <a name="smart-detection-email-notifications"></a>Akıllı algılama e-posta bildirimleri

Kuralları olarak işaretlenmiş hariç tüm akıllı algılama kuralları _Önizleme_, algılamalar bulunduğunda, e-posta bildirimleri göndermek için varsayılan olarak yapılandırılır.

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

