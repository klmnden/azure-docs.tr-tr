---
title: Azure Application Insights Telemetri veri modeli - izleme Telemetrisi | Microsoft Docs
description: İzleme telemetrisi için Application Insights veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: f87b2d1bd105a9a9b8eabb3f2f6686c36f71e6df
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721740"
---
# <a name="trace-telemetry-application-insights-data-model"></a>Telemetri izleme: Application Insights veri modeli

İzleme telemetri (içinde [Application Insights](app-insights-overview.md)) temsil eden `printf` stili metin arama izleme deyimleri. `Log4Net`, `NLog`, ve diğer metin tabanlı bir günlük dosyası girişlerini bu türü örneğine çevrilir. İzleme ölçümleri bir genişletilebilirlik yok.

## <a name="message"></a>İleti

İzleme iletisi.

En fazla uzunluk: 32.768 karakter

## <a name="severity-level"></a>Önem derecesi

İzleme önem düzeyi. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application ınsights'ta .NET izleme günlüklerini İnceleme](app-insights-asp-net-trace-logs.md).
- [İzleme günlükleri Application Insights Java keşfedin](app-insights-java-trace-logs.md).
- Bkz: [veri modeli](application-insights-data-model.md) için Application Insights türleri ve veri modeli.
- [Özel İzleme telemetrisi yazma](app-insights-api-custom-events-metrics.md#tracktrace)
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
