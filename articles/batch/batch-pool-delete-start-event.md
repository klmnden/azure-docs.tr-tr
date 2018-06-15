---
title: Azure Batch havuzu delete başlangıç olayı | Microsoft Docs
description: Batch havuzu delete başlangıç olayı referansı.
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
ms.openlocfilehash: 8737b9ff6452730ff5a55fa7324e37f0fe715433
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30312068"
---
# <a name="pool-delete-start-event"></a>Havuz silme başlangıç olayı

 Bir havuzu silme işlemi başlatıldığında bu olay yayınlanır. Zaman uyumsuz bir olay havuzu silme olduğuna göre silme işlemi tamamlandıktan sonra yayınlaması için bir havuz Sil Tamam olayını bekleyebilirsiniz.

 Aşağıdaki örnek, havuzu silme başlangıç olayı gövdesi gösterir.

```
{
    "id": "myPool1"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|Dize|Havuzun kimliği.|