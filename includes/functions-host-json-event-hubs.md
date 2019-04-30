---
ms.openlocfilehash: b5d8f67a70961aab21312b6f241081dcb33f66fb
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104466"
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

