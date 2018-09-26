---
title: Azure Application Insights Telemetri veri modeli - olay Telemetrisi | Microsoft Docs
description: Olay telemetrisi için Application Insights veri modeli
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
ms.openlocfilehash: 062478783465edc2d3afa4b80a22f119e68da049
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47091715"
---
# <a name="event-telemetry-application-insights-data-model"></a>Olay telemetrisi: Application Insights veri modeli

Olay telemetri öğelerini oluşturabilirsiniz (içinde [Application Insights](app-insights-overview.md)), uygulamanızda gerçekleşen bir olayı göstermek için. Genellikle düğmesine tıklayın veya kullanıma alma siparişi gibi bir kullanıcı etkileşimi olduğu. Uygulama yaşam döngüsü olay başlatma ya da yapılandırma güncelleştirme gibi da olabilir. 

Anlamsal olarak, olaylar olabilir veya isteklerine ilişkili değil. Ancak, doğru kullandıysanız, olay telemetri isteklerini veya izlemeleri daha daha önemlidir. Olayları iş telemetriyi temsil eder ve ayırmak için bir konu daha az agresiftir [örnekleme](app-insights-api-filtering-sampling.md).

## <a name="name"></a>Ad

Olay adı. Uygun gruplandırma ve yararlı ölçümler izin vermek için ayrı olay adları az sayıda oluşturur, böylece uygulamanız kısıtlayın. Örneğin, bir olayın oluşturulan her örneği için ayrı bir ad kullanmayın.

En fazla uzunluk: 512 karakter

## <a name="custom-properties"></a>Özel Özellikler

[!INCLUDE [application-insights-data-model-properties](../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Özel ölçümler

[!INCLUDE [application-insights-data-model-measurements](../../includes/application-insights-data-model-measurements.md)]

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [veri modeli](application-insights-data-model.md) için Application Insights türleri ve veri modeli.
- [Özel olay telemetri yazma](app-insights-api-custom-events-metrics.md#trackevent)
- Kullanıma [platformları](app-insights-platforms.md) Application Insights tarafından desteklenir.
