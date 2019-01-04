---
title: include dosyası
description: include dosyası
services: application-insights
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 06/21/2018
ms.author: mbullwin
ms.custom: include file
ms.openlocfilehash: 6b9b40c457766573be0d72cb7f4ebe2328a070f5
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54026273"
---
Uygulama başına (yani, izleme anahtarı başına) ölçüm ve olay sayısı için bazı limitler mevcuttur. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| Kaynak | Varsayılan limit | Not
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla veri gerekiyorsa, Portalı'nda limiti artırabilirsiniz en fazla 1000 GB. 1.000 GB'den büyük olan kapasiteler için e-posta Gönder AIDataCap@microsoft.com.
| Azaltma | 32 bin olay/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) ve [Ölçüm Gezgini](../articles/azure-monitor/app/metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| En büyük olay boyutu | 64 K | 
| Özellik ve ölçüm adı uzunluğu | 150 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Özellik değeri dize uzunluğu | 8,192 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| İzleme ve özel durum iletisi uzunluğu | 10 K | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Uygulama başına [kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md) sayısı | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) veri saklama | 5 gün |
| [Profiler](../articles/azure-monitor/app/profiler.md) günde gönderilen veriler | 10 GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/azure-monitor/app/pricing.md).