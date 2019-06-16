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
ms.openlocfilehash: df85aafc81b199610c02f0faecb06e804fda24bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60899306"
---
# <a name="trace-telemetry-application-insights-data-model"></a>İzleme telemetrisi: Application Insights veri modeli

İzleme telemetri (içinde [Application Insights](../../azure-monitor/app/app-insights-overview.md)) temsil eden `printf` stili metin arama izleme deyimleri. `Log4Net`, `NLog`, ve diğer metin tabanlı bir günlük dosyası girişlerini bu türü örneğine çevrilir. İzleme ölçümleri bir genişletilebilirlik yok.

## <a name="message"></a>`Message`

İzleme iletisi.

En fazla uzunluk: 32.768 karakter

## <a name="severity-level"></a>Önem derecesi

İzleme önem düzeyi. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Application ınsights'ta .NET izleme günlüklerini İnceleme](../../azure-monitor/app/asp-net-trace-logs.md).
- [İzleme günlükleri Application Insights Java keşfedin](../../azure-monitor/app/java-trace-logs.md).
- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- [Özel İzleme telemetrisi yazma](../../azure-monitor/app/api-custom-events-metrics.md#tracktrace)
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
