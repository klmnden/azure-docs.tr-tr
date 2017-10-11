---
title: "Azure Batch havuzu Sil Tamam olayını | Microsoft Docs"
description: "Batch havuzundaki başvurusunu complete olayını silin."
services: batch
author: tamram
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: tamram
ms.openlocfilehash: 890f2ba7fda37060c56177868d6214d517d91831
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="pool-delete-complete-event"></a>Havuz Sil Tamam olayını

 Bir havuzu silme işlemi tamamlandığında bu olay yayınlanır.

 Aşağıdaki örnek, bir havuz Sil Tamam olayını gövdesi gösterir.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Öğesi|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|
|startTime|Tarih saat|Havuzunu silme zamanı başlatıldı.|
|EndTime|Tarih saat|Tamamlanan havuzunu silme zamanı.|

## <a name="remarks"></a>Açıklamalar
Durumları ve havuzu yeniden boyutlandırma işlemi için hata kodları hakkında daha fazla bilgi için bkz: [bir hesaptan bir havuzunu silme](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).