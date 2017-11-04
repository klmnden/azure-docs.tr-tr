---
title: "Azure uygulama Insights Telemetri veri modeli - bağımlılık Telemetrisi | Microsoft Docs"
description: "Bağımlılık telemetrisi için uygulama Öngörüler veri modeli"
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/17/2017
ms.author: mbullwin
ms.openlocfilehash: aa305c30dc358997420be6802d43fa69e45f4a5f
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/01/2017
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık Telemetrisi (içinde [Application Insights](app-insights-overview.md)) izlenen bileşenin SQL ya da bir HTTP uç noktası gibi uzak bir bileşeni ile etkileşim temsil eder.

## <a name="name"></a>Ad

Bu bağımlılık çağrısı ile başlatılan komut adı. Düşük önem düzeyi değeri. Saklı yordam adı ve URL yolu şablonunu örnektir.

## <a name="id"></a>Kimlik

Bir bağımlılık çağrısı örneği tanımlayıcısı. Bu bağımlılık çağrısına karşılık gelen istek telemetri öğesi ile bağıntı için kullanılır. Daha fazla bilgi için bkz: [bağıntı](application-insights-correlation.md) sayfası.

## <a name="data"></a>Veriler

Bu bağımlılık çağrısı tarafından başlatılan komutu. SQL deyimi ve HTTP URL içeren tüm sorgu parametrelerini örnektir.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıklar mantıksal gruplandırmasını ve commandName ve resultCode gibi diğer alanlar yorumu düşük önem düzeyi değeri. SQL Azure tablo ve HTTP örnektir.

## <a name="target"></a>Hedef

Hedef site bağımlılık çağrısı. Sunucu adı, ana bilgisayar adresi olarak gösterilebilir. Daha fazla bilgi için bkz: [bağıntı](application-insights-correlation.md) sayfası.

## <a name="duration"></a>Süre

Süre biçimde istek: `DD.HH:MM:SS.MMMMMM`. Olmalıdır değerinden `1000` gün.

## <a name="result-code"></a>Sonuç kodu

Bir bağımlılık araması Sonuç kodu. SQL hata kodu ve HTTP durum kodu örnektir.

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrının gösterimi.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümleri

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- Bağımlılık izleme ayarlama [.NET](app-insights-asp-net-dependencies.md).
- Bağımlılık izleme ayarlama [Java](app-insights-java-agent.md).
- [Özel bağımlılık telemetrisi yazma](app-insights-api-custom-events-metrics.md#trackdependency)
- Bkz: [veri modeli](application-insights-data-model.md) Application Insights türleri ve veri modeli için.
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
