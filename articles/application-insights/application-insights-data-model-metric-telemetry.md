---
title: "Azure uygulama Insights Telemetri veri modeli - ölçüm Telemetri | Microsoft Docs"
description: "Ölçüm telemetri için uygulama Öngörüler veri modeli"
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
ms.openlocfilehash: 4cfdf3b70f6fdb2ddd5f89a72c931d3b6be54132
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="metric-telemetry-application-insights-data-model"></a>Ölçüm telemetri: Application Insights veri modeli

Ölçüm telemetri tarafından desteklenen iki tür vardır [Application Insights](app-insights-overview.md): tek ölçüm ve önceden toplanan ölçüm. Yalnızca bir ad ve değer bunun tek ölçüsüdür. Önceden toplanan ölçümü toplama aralığı ve standart sapmasını ölçüm minimum ve maksimum değerini belirtir.

Önceden toplanan ölçüm telemetri bu toplama süresi bir dakika olduğu varsayılır.

Application Insights tarafından desteklenen çeşitli iyi bilinen ölçüm adı vardır. 

Sistem ve işlem sayaçları temsil eden ölçüm:

| **.NET adı**             | **Platform belirsiz adı** | **REST API adı** | **Açıklama**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | İş sürüyor... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | Toplam makine CPU
| `\Memory\Available Bytes`                 | İş sürüyor... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | disk üzerindeki kullanılabilir bellek
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | İş sürüyor... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | Uygulama barındırma işleminin CPU
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | İş sürüyor... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | Uygulama barındırma işlemi tarafından kullanılan bellek
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | İş sürüyor... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | Uygulama barındırma işlem tarafından g/ç işlemlerinin oranı çalıştırır
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | İş sürüyor... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | uygulama tarafından işlenen istek oranı 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | İş sürüyor... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | uygulama tarafından oluşturulan özel durumları oranı
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | İş sürüyor... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | ortalama istek yürütme süresi
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | İş sürüyor... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | bir kuyruk işlemek için bekleyen istek sayısı

## <a name="name"></a>Ad

Application Insights portalı ve kullanıcı arabirimini görmek istediğiniz ölçüm adı. 

## <a name="value"></a>Değer

Ölçüm için tek bir değer. Toplama için tek tek ölçüler toplamı.

## <a name="count"></a>Sayı

Toplanan ölçüm ölçüm ağırlığı. Ölçüm için ayarlanmamalıdır.

## <a name="min"></a>Min

Toplanan ölçüm en küçük değeri. Ölçüm için ayarlanmamalıdır.

## <a name="max"></a>Maks.

Toplanan ölçüm maksimum değeri. Ölçüm için ayarlanmamalıdır.

## <a name="standard-deviation"></a>Standart sapma

Toplanan ölçüm standart sapması. Ölçüm için ayarlanmamalıdır.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin [özel olayları ve ölçümleri için Application Insights API'si](app-insights-api-custom-events-metrics.md#trackmetric).
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
