---
title: Azure uygulama Insights Telemetri veri modeli - izleme Telemetri | Microsoft Docs
description: "İzleme telemetri için uygulama Öngörüler veri modeli"
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
ms.openlocfilehash: 0398774e21d89fd084e6929bc5e410697d2aafaa
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetri izleme: Application Insights veri modeli

İzleme telemetri (içinde [Application Insights](app-insights-overview.md)) temsil eden `printf` stili metin arama izleme deyimleri. `Log4Net`, `NLog`, ve diğer metin tabanlı günlük dosyası girişlerini, bu türdeki örneklerin dönüştürülür. İzleme ölçümler bir genişletilebilirlik yok.

## <a name="message"></a>İleti

İzleme iletisi.

En fazla uzunluk: 32768 karakterleri

## <a name="severity-level"></a>Önem düzeyi

Önem düzeyi izleme. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application ınsights'ta .NET izleme günlükleri keşfedin](app-insights-asp-net-trace-logs.md).
- [İzleme günlüklerini Application Insights'ta Java keşfedin](app-insights-java-trace-logs.md).
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- [Özel İzleme telemetri yazma](app-insights-api-custom-events-metrics.md#tracktrace)
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
