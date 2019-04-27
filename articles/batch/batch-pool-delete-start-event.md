---
title: Azure Batch havuz silme başlangıç olayı | Microsoft Docs
description: Başvuru için Batch havuz silme başlangıç olayı.
services: batch
author: dlepow
manager: jeconnoc
ms.assetid: ''
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
origin.date: 04/20/2017
ms.date: 05/14/2018
ms.author: v-junlch
ms.openlocfilehash: 2352971af3844b56f93c16ebaf6cb23bd5fd8a5a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60774548"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Havuz silme işlemi başlatıldığında bu olay yayılır. Havuz silme zaman uyumsuz bir olay olduğundan, silme işlemi tamamlandıktan sonra derleyicisindeki bir havuz silme tamamlama olayı bekleyebilirsiniz.

 Aşağıdaki örnek, bir havuz silme başlangıç olayı gövdesi gösterir.

```
{
    "id": "myPool1"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|String|Havuz kimliği.|

<!-- Update_Description: update metedata properties -->