---
title: Azure uygulama Insights Telemetri veri modeli - olay Telemetri | Microsoft Docs
description: "Olay telemetri için uygulama Öngörüler veri modeli"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: dc6ef1771f4f09b274f12606d1278e1f29c23103
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="event-telemetry-application-insights-data-model"></a>Olay telemetri: Application Insights veri modeli

Telemetri öğeleri olay oluşturabilirsiniz (içinde [Application Insights](app-insights-overview.md)), uygulamanızda gerçekleşen bir olay temsil etmek için. Genellikle düğmesini tıklatın veya satın alma siparişi gibi bir kullanıcı etkileşimi değil. Ayrıca, uygulama yaşam döngüsü olay başlatma veya yapılandırma güncelleştirme gibi de olabilir. 

Anlam olarak, olaylar olabilir veya isteklerine ilişkili değil. Ancak, düzgün kullandıysanız, olay telemetri istekleri veya izlemeleri daha önemlidir. Olayları iş telemetri temsil eder ve ayırmak için bir konu daha az agresif [örnekleme](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Ad

Olay adı. Uygun gruplandırma ve yararlı ölçümler izin vermek için uygulamanızın ayrı olay adları az sayıda oluşturur şekilde kısıtlamak. Örneğin, bir olay oluşturulan her örneği için ayrı bir ad kullanmayın.

En fazla uzunluk: 512 karakteri

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümleri

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- [Özel olay telemetri yazma](app-insights-api-custom-events-metrics.md#trackevent)
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
