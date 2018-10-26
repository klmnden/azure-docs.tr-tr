---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 0a3f739bae3ec758c8e25d581d3e2b9feb4af5b1
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50132733"
---
```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Bir kerede yalnızca tek bir kuyruk veya konuda ileti işleme için çalışma zamanının ayarlayın `maxConcurrentCalls` 1. | 
|prefetchCount|yok|Varsayılan temel alınan MessageReceiver tarafından kullanılacak PrefetchCount.| 
|autoRenewTimeout|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.| 
