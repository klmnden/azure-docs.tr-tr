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
ms.openlocfilehash: 0f09561c362494d1e41edf29b85dee77dbbc3678
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721604"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Bağımlılık telemetrisi: Application Insights veri modeli

Bağımlılık Telemetrisi (içinde [Application Insights](app-insights-overview.md)) etkileşim SQL ya da bir HTTP uç noktası gibi uzak bir bileşen ile izlenen bileşenin temsil eder.

## <a name="name"></a>Ad

İle bu bağımlılık çağrısının başlattığı komut adı. Düşük önem düzeyi değeri. Saklı yordam adı ve URL yolu şablonunu verilebilir.

## <a name="id"></a>Kimlik

Bağımlılık çağrısı örneği tanımlayıcısı. Bu bağımlılık çağrısının için karşılık gelen istek telemetri öğesinin ile bağıntı için kullanılır. Daha fazla bilgi için [bağıntı](application-insights-correlation.md) sayfası.

## <a name="data"></a>Veriler

Bu bağımlılık çağrısı tarafından başlatılan komutu. SQL deyimi ve HTTP URL'si ile tüm sorgu parametreleri verilebilir.

## <a name="type"></a>Tür

Bağımlılık türü adı. Bağımlılıkları mantıksal gruplandırmasını ve diğer alanlar gibi commandName ve resultCode yorumu düşük kardinalite değeri. SQL, Azure tablosu ve HTTP verilebilir.

## <a name="target"></a>Hedef

Hedef site bir bağımlılık çağrısının. Sunucu adı, ana bilgisayar adresi verilebilir. Daha fazla bilgi için [bağıntı](application-insights-correlation.md) sayfası.

## <a name="duration"></a>Süre

İstek süresi biçimde: `DD.HH:MM:SS.MMMMMM`. Olmalıdır küçüktür `1000` gün.

## <a name="result-code"></a>Sonuç kodu

Bağımlılık çağrısı Sonuç kodu. SQL hata kodu ve HTTP durum kodu verilebilir.

## <a name="success"></a>Başarılı

Başarılı veya başarısız çağrı göstergesi.

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Sonraki adımlar

- Bağımlılık izleme ayarlama [.NET](app-insights-asp-net-dependencies.md).
- Bağımlılık izleme ayarlama [Java](app-insights-java-agent.md).
- [Özel bağımlılık telemetri yazma](app-insights-api-custom-events-metrics.md#trackdependency)
- Bkz: [veri modeli](application-insights-data-model.md) için Application Insights türleri ve veri modeli.
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
