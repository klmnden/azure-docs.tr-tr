---
title: Azure Batch havuzu Sil Tamam olayını | Microsoft Docs
description: Batch havuzundaki başvurusunu complete olayını silin.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: danlep
ms.openlocfilehash: bfcbcf40efc64ab1c79ee1a86e02502c68ad6d47
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="pool-delete-complete-event"></a>Havuz silme tamamlama olayı

 Bir havuzu silme işlemi tamamlandığında bu olay yayınlanır.

 Aşağıdaki örnek, bir havuz Sil Tamam olayını gövdesi gösterir.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|
|startTime|DateTime|Havuzunu silme zamanı başlatıldı.|
|endTime|DateTime|Tamamlanan havuzunu silme zamanı.|

## <a name="remarks"></a>Açıklamalar
Durumları ve havuzu yeniden boyutlandırma işlemi için hata kodları hakkında daha fazla bilgi için bkz: [bir hesaptan bir havuzunu silme](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).