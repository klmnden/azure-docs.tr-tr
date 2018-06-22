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
ms.openlocfilehash: 90de751f416ca611f3c674232c224199ad7af717
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310176"
---
Uygulama başına (yani, izleme anahtarı başına) ölçüm ve olay sayısı için bazı limitler mevcuttur. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| Kaynak | Varsayılan limit | Not
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla veri gerekiyorsa, portalda, sınırı artırabilirsiniz 1.000 GB'a kadar. 1000 GB'den büyük olan kapasiteleri için posta gönderin AIDataCap@microsoft.com.
| Azaltma | 32 K olaylar/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/application-insights/app-insights-diagnostic-search.md), [Analytics](../articles/application-insights/app-insights-analytics.md) ve [Ölçüm Gezgini](../articles/application-insights/app-insights-metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md#multi-step-web-tests) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| En fazla olay boyutu | 64 K | 
| Özellik ve ölçüm adı uzunluğu | 150 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Özellik değeri dize uzunluğu | 8,192 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| İzleme ve özel durum iletisi uzunluğu | 10 K | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Uygulama başına [kullanılabilirlik testi](../articles/application-insights/app-insights-monitor-web-app-availability.md) sayısı | 100 |
| [Profil Oluşturucu](../articles/application-insights/app-insights-profiler.md) veri saklama | 5 gün |
| [Profil Oluşturucu](../articles/application-insights/app-insights-profiler.md) gün başına gönderilen veriler | 10 GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/application-insights/app-insights-pricing.md).