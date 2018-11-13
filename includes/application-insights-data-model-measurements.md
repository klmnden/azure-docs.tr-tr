---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 3986b77cfad167134bf8ada1e3cbb73ad64dd3ca
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51572839"
---
Özel ölçümler koleksiyonu. Bu koleksiyona telemetri öğeyle ilişkili ölçüm adlı raporu kullanın. Tipik kullanım örnekleridir:
- Bağımlılık Telemetrisi yükü boyutu
- İstek Telemetri tarafından işlenen kuyruk öğe sayısı
- Bu müşteri sürdü Sihirbazı Adım tamamlandı olay Telemetri adımda tamamlanması zaman.

Sorgulayabilirsiniz [özel ölçümleri](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) Application analytics'te:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Özel ölçümler, ait oldukları telemetri öğesinin ile ilişkilidir. Bu ölçüler içeren telemetri öğeyle tabi örnekleme değildirler. Diğer telemetri türleri bağımsız bir değere sahip bir ölçüm izlemek için [ölçüm telemetri](../articles/application-insights/app-insights-api-custom-events-metrics.md).

En fazla anahtar uzunluğu: 150
