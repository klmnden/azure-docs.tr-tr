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
ms.openlocfilehash: 31ff6ae66b13eb5e5abcde94e8d3df7f6e32f63c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66130804"
---
Bununla ilgili bazı sınırlamalar ölçümleri ve uygulama başına olay sayısı diğer bir deyişle, izleme anahtarı başına. Limitler seçtiğiniz [fiyatlandırma planına](https://azure.microsoft.com/pricing/details/application-insights/) bağlıdır.

| Resource | Varsayılan limit | Not
| --- | --- | --- |
| Günlük toplam veri | 100 GB | Bir uç ayarlayarak verileri azaltabilirsiniz. Daha fazla veri gerekiyorsa, Portalı'nda limiti artırabilirsiniz en fazla 1000 GB. 1.000 GB'den büyük olan kapasiteler için e-posta Gönder AIDataCap@microsoft.com.
| Azaltma | 32.000 olay/saniye | Sınır bir dakika içinde ölçülür.
| Veri saklama | 90 gün | Bu kaynak [Search](../articles/azure-monitor/app/diagnostic-search.md), [Analytics](../articles/azure-monitor/app/analytics.md) ve [Ölçüm Gezgini](../articles/azure-monitor/app/metrics-explorer.md) içindir.
| [Çok adımlı kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md#multi-step-web-tests) ayrıntılı sonuçlarını saklama | 90 gün | Bu kaynak her adımın ayrıntılı sonuçlarını verir.
| En büyük olay boyutu | 64,000 |
| Özellik ve ölçüm adı uzunluğu | 150 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Özellik değeri dize uzunluğu | 8,192 | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| İzleme ve özel durum iletisi uzunluğu | 32,768  | Bkz: [yazın şemaları](https://github.com/Microsoft/ApplicationInsights-Home/blob/master/EndpointSpecs/Schemas/Docs/).
| Uygulama başına [kullanılabilirlik testi](../articles/azure-monitor/app/monitor-web-app-availability.md) sayısı | 100 |
| [Profiler](../articles/azure-monitor/app/profiler.md) veri saklama | 5 gün |
| [Profiler](../articles/azure-monitor/app/profiler.md) günde gönderilen veriler | 10 GB |

Daha fazla bilgi için bkz. [Application Insights fiyatlandırma ve kotaları hakkında](../articles/azure-monitor/app/pricing.md).