---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66131689"
---
```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------| 
|maxBatchSize|64|Alma döngü alınan en yüksek olay sayısı.|
|prefetchCount|yok|Varsayılan temel alınan EventProcessorHost tarafından kullanılacak PrefetchCount.| 
|batchCheckpointFrequency|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay yığın sayısı.| 
