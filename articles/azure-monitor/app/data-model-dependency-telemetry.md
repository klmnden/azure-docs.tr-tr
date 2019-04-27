---
title: Azure Application Insights Telemetri veri modeli - bağımlılık Telemetrisi | Microsoft Docs
description: Bağımlılık telemetrisi için Application Insights veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: 3e3d6b8fdc9ac8dd28f73fecd6231e97a5645407
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60901034"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık Telemetrisi (içinde [Application Insights](../../azure-monitor/app/app-insights-overview.md)) etkileşim SQL ya da bir HTTP uç noktası gibi uzak bir bileşen ile izlenen bileşenin temsil eder.

## <a name="name"></a>Ad

İle bu bağımlılık çağrısının başlattığı komut adı. Düşük önem düzeyi değeri. Saklı yordam adı ve URL yolu şablonunu verilebilir.

## <a name="id"></a>Kimlik

Bağımlılık çağrısı örneği tanımlayıcısı. Bu bağımlılık çağrısının için karşılık gelen istek telemetri öğesinin ile bağıntı için kullanılır. Daha fazla bilgi için [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

## <a name="data"></a>Veriler

Bu bağımlılık çağrısı tarafından başlatılan komutu. SQL deyimi ve HTTP URL'si ile tüm sorgu parametreleri verilebilir.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıkları mantıksal gruplandırmasını ve diğer alanlar gibi commandName ve resultCode yorumu düşük kardinalite değeri. SQL, Azure tablosu ve HTTP verilebilir.

## <a name="target"></a>Hedef

Hedef site bir bağımlılık çağrısının. Sunucu adı, ana bilgisayar adresi verilebilir. Daha fazla bilgi için [bağıntı](../../azure-monitor/app/correlation.md) sayfası.

## <a name="duration"></a>Süre

İstek süresi biçimde: `DD.HH:MM:SS.MMMMMM`. Olmalıdır küçüktür `1000` gün.

## <a name="result-code"></a>Sonuç kodu

Bağımlılık çağrısı Sonuç kodu. SQL hata kodu ve HTTP durum kodu verilebilir.

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- Bağımlılık izleme ayarlama [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Bağımlılık izleme ayarlama [Java](../../azure-monitor/app/java-agent.md).
- [Özel bağımlılık telemetri yazma](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Bkz: [veri modeli](data-model.md) için Application Insights türleri ve veri modeli.
- Kullanıma [platformları](../../azure-monitor/app/platforms.md) Application Insights tarafından desteklenir.
