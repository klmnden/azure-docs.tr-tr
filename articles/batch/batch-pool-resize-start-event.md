---
title: Azure Batch havuzu yeniden boyutlandırma başlangıç olayı | Microsoft Docs
description: Başvuru için Batch havuz yeniden boyutlandırma başlangıç olayı.
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
ms.openlocfilehash: 63abeaca5a9c87945671e79a9c8d7a2512d0d777
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60774242"
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
|poolId|String|Havuz kimliği.|
|nodeDeallocationOption|String|Havuz boyutu azalıyorsa ne zaman düğüm havuzdan kaldırılabilir belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırın ve yeniden kuyruğa ekleyin. İş etkinleştirildiğinde görevler yeniden çalıştırılır. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **sonlandırma** – çalışan görevleri sonlandır. Görevleri yeniden çalışmaz. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **net_offline_option** – tamamlanması şu anda çalışan görevlerin izin ver. Hiç yeni görev beklenirken zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırma.<br /><br /> **Retaineddata** -izin şu anda çalışan görevlerin tamamlayın ve ardından tüm veri tutma sürelerinin sona görev için bekleyin. Hiç yeni görev beklenirken zamanlayın. Tüm görev bekletme süreleri dolmuş olduğunda düğümleri kaldırma.<br /><br /> Yeniden kuyruğa alma varsayılan değerdir.<br /><br /> Havuz boyutunu artırma sonra değeri şuna ayarlı **geçersiz**.|
|currentDedicated|Int32|Havuza atanmış işlem düğümleri sayısı.|
|targetDedicated|Int32|Havuz için istenen işlem düğümleri sayısı.|
|enableAutoScale|Bool|Havuz boyutunu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|isAutoPool|Bool|Havuza bir işin AutoPool mekanizması oluşturulup oluşturulmadığını belirtir.|
