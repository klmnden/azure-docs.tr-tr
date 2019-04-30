---
author: mrbullwinkle
ms.service: application-insights
ms.topic: include
ms.date: 11/09/2018
ms.author: mbullwin
ms.openlocfilehash: 91141e314bf276d9138157c8a9d85d5262ac5907
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60900535"
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
 > Özel ölçümler, ait oldukları telemetri öğesinin ile ilişkilidir. Bu ölçüler içeren telemetri öğeyle tabi örnekleme değildirler. Diğer telemetri türleri bağımsız bir değere sahip bir ölçüm izlemek için [ölçüm telemetri](../articles/azure-monitor/app/api-custom-events-metrics.md).

En fazla anahtar uzunluğu: 150
