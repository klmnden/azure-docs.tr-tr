---
title: "Azure Batch havuzu delete başlangıç olayı | Microsoft Docs"
description: "Batch havuzu delete başlangıç olayı referansı."
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
ms.openlocfilehash: f8a5241dce422e5c826ab428da6d7bc93284a1cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="pool-delete-start-event"></a>Havuz delete başlangıç olayı

 Bir havuzu silme işlemi başlatıldığında bu olay yayınlanır. Zaman uyumsuz bir olay havuzu silme olduğuna göre silme işlemi tamamlandıktan sonra yayınlaması için bir havuz Sil Tamam olayını bekleyebilirsiniz.

 Aşağıdaki örnek, havuzu silme başlangıç olayı gövdesi gösterir.

```
{
    "id": "myPool1"
}
```

|Öğesi|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|