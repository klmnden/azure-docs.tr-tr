---
title: Azure Application Insights Telemetri veri modeli - özel durum Telemetrisi | Microsoft Docs
description: Özel durum telemetrisi için Application Insights veri modeli
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2017
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: dc5480b90ef6b5520f47c51f0c105202d7071089
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47093602"
---
# <a name="exception-telemetry-application-insights-data-model"></a>Özel durum telemetrisi: Application Insights veri modeli

İçinde [Application Insights](app-insights-overview.md), izlenen uygulamanın yürütülmesi sırasında oluşan bir işlenen veya işlenmeyen özel durum bir özel durumun örneğini temsil eder.

## <a name="problem-id"></a>Sorun kimliği

Burada kodda özel durum oluştu, tanımlayıcısı'ı tıklatın. Özel durumları gruplandırmak için kullanılır. Genellikle bir özel durum türü ve birleşim çağrı yığınındaki bir işleve.

En fazla uzunluk: 1024 karakter

## <a name="severity-level"></a>Önem derecesi

İzleme önem düzeyi. Değeri olabilir `Verbose`, `Information`, `Warning`, `Error`, `Critical`.

## <a name="exception-details"></a>Özel durum ayrıntıları

(Genişletilmesi için)

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [veri modeli](application-insights-data-model.md) için Application Insights türleri ve veri modeli.
- Bilgi edinmek için nasıl [Application Insights ile web uygulamalarınızda özel durumları tanılama](app-insights-asp-net-exceptions.md).
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
