---
title: "Azure uygulama Insights Telemetri veri modeli - özel durum Telemetrisi | Microsoft Docs"
description: "Özel durum telemetrisi için uygulama Öngörüler veri modeli"
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
ms.openlocfilehash: 3c3c3a39c7986cc771fe4baf60ad9b316888f6ed
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Application Insights veri modeli

İçinde [Application Insights](app-insights-overview.md), izlenen uygulama yürütülmesi sırasında oluşan bir işlenen veya işlenmeyen özel durum örneğini temsil eder.

## <a name="problem-id"></a>Sorun kimliği

Burada kodda özel durum oluştu tanımlayıcısı. Gruplandırma özel durumlar için kullanılır. Genellikle bir özel durum türü ve birleşimi bir işleve çağrı yığını.

En fazla uzunluk: 1024 karakter

## <a name="severity-level"></a>Önem düzeyi

Önem düzeyi izleme. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Özel durum ayrıntıları

(Genişletilmesi için)

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümleri

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Bilgi edinmek için nasıl [web uygulamalarınızı Application Insights ile özel durumları tanılamak](app-insights-asp-net-exceptions.md).
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
