---
title: "Durum bilgisi olan güvenilir hizmetler tanılama | Microsoft Docs"
description: "Durum Bilgisi Olan Reliable Services için tanılama işlevi"
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ae0e8f99-69ab-4d45-896d-1fa80ed45659
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: dekapur
ms.openlocfilehash: 63a7707f16bbf037c0c91da1d02093e2314dc06e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="diagnostic-functionality-for-stateful-reliable-services"></a>Durum Bilgisi Olan Reliable Services için tanılama işlevi
Durum bilgisi olan güvenilir hizmetler StatefulServiceBase sınıfı yayar [EventSource](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.aspx) hizmetinde hata ayıklama için kullanılan olayları nasıl çalışma zamanı işletim ve sorunu gidermenize yardımcı içine Öngörüler sağlayın.

## <a name="eventsource-events"></a>EventSource olaylarını
"Microsoft ServiceFabric Hizmetleri" durum bilgisi olan güvenilir hizmetler StatefulServiceBase sınıfı için EventSource adıdır. Bu olay kaynağı olaylarından görünür [tanılama olayları](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md#view-service-fabric-system-events-in-visual-studio) hizmet yüklenirken pencere [Visual Studio'da hata ayıklaması](service-fabric-debugging-your-application.md).

Araçlar ve toplama ve/veya EventSource olaylarını görüntüleme içinde yardımcı teknolojiler örnekleri [PerfView](http://www.microsoft.com/download/details.aspx?id=28567), [Microsoft Azure tanılama](../cloud-services/cloud-services-dotnet-diagnostics.md)ve [Microsoft TraceEvent Library](http://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent).

## <a name="events"></a>Olaylar
| Olay adı | Olay Kimliği | Düzey | Olay açıklaması |
| --- | --- | --- | --- |
| StatefulRunAsyncInvocation |1 |Bilgilendirme |Hizmet RunAsync görevi başlatıldığında yayılan |
| StatefulRunAsyncCancellation |2 |Bilgilendirme |Hizmet RunAsync görevi iptal edildiğinde yayılan |
| StatefulRunAsyncCompletion |3 |Bilgilendirme |Hizmet RunAsync görev tamamlandığında yayılan |
| StatefulRunAsyncSlowCancellation |4 |Uyarı |Hizmet RunAsync görevi iptal tamamlanması çok uzun sürerse yayılan |
| StatefulRunAsyncFailure |5 |Hata |Hizmet RunAsync görev özel durum oluşturduğunda yayılan |

## <a name="interpret-events"></a>Olayları yorumlama
StatefulRunAsyncInvocation, StatefulRunAsyncCompletion ve StatefulRunAsyncCancellation olayların hizmet yazıcı'yaşam döngüsü, bir hizmet zaman başlatıldı, iptal edildi veya tamamlandı zamanlama yanı sıra bir hizmet--anlamak için kullanışlıdır. Bu durumlarda kullanışlı olabilir hizmet sorunlarını hata ayıklama veya hizmet yaşam döngüsünü anlayın.

Hizmetiyle ilgili sorunlara belirttiğinden hizmeti yazıcıları Kapat StatefulRunAsyncSlowCancellation ve StatefulRunAsyncFailure olaylara dikkat etmeniz gerekir.

Hizmet RunAsync() görevi aykırı her StatefulRunAsyncFailure yayınlanır. Genellikle, oluşturulan bir özel bir hata veya hizmetinde hata gösterir. Ayrıca, farklı bir düğüme taşınır için özel durum hizmetinin başarısız olmasına neden olur. Bu, pahalı bir işlem olabilir ve hizmet taşınırken gelen istekleri geciktirebilir. Beklemeleri özel durumun nedeni belirlemek ve mümkünse, bunu azaltmak.

İptal isteği RunAsync görev için dört saniyeden daha uzun sürer her StatefulRunAsyncSlowCancellation yayınlanır. Bir hizmeti iptal tamamlanması çok uzun sürerse, hızlı bir şekilde başka bir düğümde yeniden başlatılmasını hizmetinin yeteneği etkiler. Bu hizmetin genel kullanılabilirliğini etkileyebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [PerfView EventSource sağlayıcıları](https://blogs.msdn.microsoft.com/vancem/2012/07/09/introduction-tutorial-logging-etw-events-in-c-system-diagnostics-tracing-eventsource/)
