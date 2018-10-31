---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: functions
ms.topic: include
ms.date: 10/19/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: da0cbab59d9c801419ac4b3704fee3275f337fd9
ms.sourcegitcommit: 1d3353b95e0de04d4aec2d0d6f84ec45deaaf6ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50251279"
---
Kaç tane işlev çağrılarını belirtir ne zaman toplanan [ölçümler için Application Insights hesaplama](../articles/azure-functions/functions-monitoring.md#configure-the-aggregator). 

```json
{
    "aggregator": {
        "batchSize": 1000,
        "flushTimeout": "00:00:30"
    }
}
```

|Özellik |Varsayılan  | Açıklama |
|---------|---------|---------| 
|batchSize|1000|En fazla istek sayısını toplanacak.| 
|flushTimeout|00:00:30|En uzun süreyi toplanacak dönem.| 

İşlev çağrılarını sınırlar ilk iki zaman toplanan ulaşıldı.