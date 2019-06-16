---
title: Azure Batch havuz silme tamamlama olayı | Microsoft Docs
description: Sil Tamam olayını Batch havuzu için başvuru.
services: batch
author: laurenhughes
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: lahugh
ms.openlocfilehash: e715ccd0f5e79f9c640a3c060b0252b798748b4d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60775772"
---
# <a name="pool-delete-complete-event"></a>Havuz silme tamamlama olayı

 Havuz silme işlemi tamamlandıktan sonra bu olay yayılır.

 Aşağıdaki örnek, bir havuz silme tamamlama olayı gövdesi gösterir.

```
{
    "id": "myPool1",
    "startTime": "2016-09-09T22:13:48.579Z",
    "endTime": "2016-09-09T22:14:08.836Z"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|String|Havuz kimliği.|
|startTime|DateTime|Havuz silme zaman başlatıldı.|
|endTime|DateTime|Tamamlanma havuzunu silme zamanı.|

## <a name="remarks"></a>Açıklamalar
Durumları ve havuz yeniden boyutlandırma işlemi için hata kodları hakkında daha fazla bilgi için bkz. [bir hesaptan bir havuzunu silme](https://docs.microsoft.com/rest/api/batchservice/delete-a-pool-from-an-account).