---
title: Azure Application Insights Telemetri veri modeli - ölçüm Telemetri | Microsoft Docs
description: Ölçüm telemetri için Application Insights veri modeli
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
ms.openlocfilehash: 0973c86d055ff4ebbe7e5a3c4a2ca4e3dcabc6a0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60900469"
---
# <a name="metric-telemetry-application-insights-data-model"></a>Ölçüm telemetri: Application Insights veri modeli

Ölçüm telemetri tarafından desteklenen iki tür vardır [Application Insights](../../azure-monitor/app/app-insights-overview.md): tek bir ölçüm ve önceden toplanmış ölçümü. Yalnızca bir ad ve değer bunun tek ölçümüdür. Önceden toplanmış ölçüm toplama aralığı ve standart sapmasını ölçüm minimum ve maksimum değerini belirtir.

Bu toplama süresi bir dakika olan önceden toplanmış ölçüm telemetri varsayar.

Application Insights tarafından desteklenen çeşitli iyi bilinen ölçüm adları vardır. Bu ölçümler performanceCounters tabloya yerleştirilir.

Ölçüm sisteminin ve işlem sayaçları temsil eden:

| **.NET adı**             | **Platformu belirsiz adı** | **REST API adı** | **Açıklama**
| ------------------------- | -------------------------- | ----------------- | ---------------- 
| `\Processor(_Total)\% Processor Time` | İş sürüyor... | [processorCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessorCpuPercentage) | Toplam makine CPU
| `\Memory\Available Bytes`                 | İş sürüyor... | [memoryAvailableBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FmemoryAvailableBytes) | Bayt cinsinden bilgisayarda çalışan işlemler için kullanılabilir fiziksel bellek miktarını gösterir. Boşluk miktarını sıfırlanmış, ücretsiz ve bekleme bellek listelerinde fiyatının eklenmesiyle hesaplanır. Boş bellek kullanıma hazırdır; önceki bir işlem tarafından kullanılan veri görmemesi sonraki işlemlerin sıfırlarla doldurulur bellek sayfalarının sıfırlanmış bellek oluşur; bir işlemin çalışma kümesi (fiziksel bellek) diske yoldaki kaldırıldı ancak çağrılmaya hala kullanılabilir bellek bekleme bellektir. Bkz: [bellek nesne](https://msdn.microsoft.com/library/ms804008.aspx)
| `\Process(??APP_WIN32_PROC??)\% Processor Time` | İş sürüyor... | [processCpuPercentage](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessCpuPercentage) | Uygulama barındırma işleminin CPU
| `\Process(??APP_WIN32_PROC??)\Private Bytes`      | İş sürüyor... | [processPrivateBytes](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessPrivateBytes) | Uygulama barındırma işlemi tarafından kullanılan bellek
| `\Process(??APP_WIN32_PROC??)\IO Data Bytes/sec` | İş sürüyor... | [processIOBytesPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FprocessIOBytesPerSecond) | Uygulama barındırma işlemi tarafından g/ç işlemlerinin oranı çalıştırır
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests/Sec`             | İş sürüyor... | [requestsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsPerSecond) | uygulama tarafından işlenen isteklerin oranı 
| `\.NET CLR Exceptions(??APP_CLR_PROC??)\# of Exceps Thrown / sec`    | İş sürüyor... | [exceptionsPerSecond](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FexceptionsPerSecond) | uygulama tarafından oluşturulan özel durum oranı
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Request Execution Time`   | İş sürüyor... | [requestExecutionTime](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestExecutionTime) | ortalama istek yürütme süresi
| `\ASP.NET Applications(??APP_W3SVC_PROC??)\Requests In Application Queue` | İş sürüyor... | [requestsInQueue](https://dev.applicationinsights.io/apiexplorer/metrics?appId=DEMO_APP&apiKey=DEMO_KEY&metricId=performanceCounters%2FrequestsInQueue) | işlem kuyruktaki bekleyen istek sayısı

## <a name="name"></a>Ad

Kullanıcı Arabirimi ve Application Insights portalında görmek istediğiniz ölçüm adı. 

## <a name="value"></a>Değer

Ölçüm için tek değer. Toplama için tek tek ölçüler toplamı.

## <a name="count"></a>Sayı

Ölçüm ağırlık toplanmış ölçümün. Bir ölçüm için ayarlanmamalıdır.

## <a name="min"></a>Min

Toplanan bir ölçüm en düşük değeri. Bir ölçüm için ayarlanmamalıdır.

## <a name="max"></a>Maks

Toplanan bir ölçüm en yüksek değeri. Bir ölçüm için ayarlanmamalıdır.

## <a name="standard-deviation"></a>Standart sapma

Toplanan bir ölçüm standart sapması. Bir ölçüm için ayarlanmamalıdır.

## <a name="custom-properties"></a>Özel Özellikler

Özel özellik Metrik `CustomPerfCounter` kümesine `true` ölçüm windows performans sayacı temsil ettiğini gösterir. Bu ölçümler performanceCounters tablosunda yerleştirilir. Değil customMetrics. Ayrıca bu ölçüm adı, kategori, sayaç ve örnek adlarını ayıklamak için ayrıştırılır.

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Nasıl kullanacağınızı öğrenin [özel olaylar ve ölçümler için Application Insights API](../../azure-monitor/app/api-custom-events-metrics.md#trackmetric).
- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
