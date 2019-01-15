---
title: Azure Batch havuzu yeniden boyutlandırma başlangıç olayı | Microsoft Docs
description: Başvuru için Batch havuz yeniden boyutlandırma başlangıç olayı.
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
ms.openlocfilehash: 3cabf18cfd771151e62d64dc1d2b47b250ac5471
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2019
ms.locfileid: "54258280"
---
# <a name="pool-resize-start-event"></a>Havuz yeniden boyutlandırma başlangıç olayı

 Havuz yeniden boyutlandırma başlatıldığında bu olay yayılır. Havuz yeniden boyutlandırma zaman uyumsuz bir olay olduğundan, yeniden boyutlandırma işlemi tamamlandıktan sonra derleyicisindeki bir havuz yeniden boyutlandırma tamamlama olayı bekleyebilirsiniz.

 Aşağıdaki örnek, bir havuzu 0'dan 2 düğüm için el ile bir yeniden boyutlandırma ile boyutlandırma için bir havuz yeniden boyutlandırma başlangıç olayı gövdesi gösterir.

```
{
    "poolId": "myPool1",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 0,
    "targetDedicated": 2,
    "enableAutoScale": false,
    "isAutoPool": false
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|poolId|Dize|Havuz kimliği.|
|nodeDeallocationOption|Dize|Havuz boyutu azalıyorsa ne zaman düğüm havuzdan kaldırılabilir belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırın ve yeniden kuyruğa ekleyin. İş etkinleştirildiğinde görevler yeniden çalıştırılır. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **sonlandırma** – çalışan görevleri sonlandır. Görevleri yeniden çalışmaz. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **net_offline_option** – tamamlanması şu anda çalışan görevlerin izin ver. Hiç yeni görev beklenirken zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırma.<br /><br /> **Retaineddata** -izin şu anda çalışan görevlerin tamamlayın ve ardından tüm veri tutma sürelerinin sona görev için bekleyin. Hiç yeni görev beklenirken zamanlayın. Tüm görev bekletme süreleri dolmuş olduğunda düğümleri kaldırma.<br /><br /> Yeniden kuyruğa alma varsayılan değerdir.<br /><br /> Havuz boyutunu artırma sonra değeri şuna ayarlı **geçersiz**.|
|currentDedicated|Int32|Havuza atanmış işlem düğümleri sayısı.|
|targetDedicated|Int32|Havuz için istenen işlem düğümleri sayısı.|
|enableAutoScale|bool|Havuz boyutunu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|isAutoPool|bool|Havuza bir işin AutoPool mekanizması oluşturulup oluşturulmadığını belirtir.|
