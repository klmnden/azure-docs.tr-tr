---
title: Azure Batch havuzu yeniden boyutlandırma tamamlama olayı | Microsoft Docs
description: Batch havuzu başvurusunu yeniden boyutlandırma tamamlama olayı.
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
ms.openlocfilehash: 87c98b89a49adbad88841dccbd4ba47d370b2be7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60776431"
---
# <a name="pool-resize-complete-event"></a>Havuz yeniden boyutlandırma tamamlama olayı

 Havuz yeniden boyutlandırma tamamlandıysa veya başarısız olduğunda bu olay yayılır.

 Aşağıdaki örnek boyutu artar ve başarıyla tamamlanmış bir havuzu için havuz yeniden boyutlandırma tamamlama olayı gövdesi gösterir.

```
{
    "id": "p_1_0_01503750-252d-4e57-bd96-d6aa05601ad8",
    "nodeDeallocationOption": "invalid",
    "currentDedicated": 4,
    "targetDedicated": 4,
    "enableAutoScale": false,
    "isAutoPool": false,
    "startTime": "2016-09-09T22:13:06.573Z",
    "endTime": "2016-09-09T22:14:01.727Z",
    "result": "Success",
    "resizeError": "The operation succeeded"
}
```

|Öğe|Tür|Notlar|
|-------------|----------|-----------|
|id|String|Havuz kimliği.|
|nodeDeallocationOption|String|Havuz boyutu azalıyorsa ne zaman düğüm havuzdan kaldırılabilir belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **yeniden kuyruğa alma** – çalışan görevleri sonlandırın ve yeniden kuyruğa ekleyin. İş etkinleştirildiğinde görevler yeniden çalıştırılır. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **sonlandırma** – çalışan görevleri sonlandır. Görevleri yeniden çalışmaz. Görevleri sonlandırıldı hemen sonra düğümleri kaldırma.<br /><br /> **net_offline_option** – tamamlanması şu anda çalışan görevlerin izin ver. Hiç yeni görev beklenirken zamanlayın. Tüm görevler tamamlandığında düğümleri kaldırma.<br /><br /> **Retaineddata** -izin şu anda çalışan görevlerin tamamlayın ve ardından tüm veri tutma sürelerinin sona görev için bekleyin. Hiç yeni görev beklenirken zamanlayın. Tüm görev bekletme süreleri dolmuş olduğunda düğümleri kaldırma.<br /><br /> Yeniden kuyruğa alma varsayılan değerdir.<br /><br /> Havuz boyutunu artırma sonra değeri şuna ayarlı **geçersiz**.|
|currentDedicated|Int32|Havuza atanmış işlem düğümleri sayısı.|
|targetDedicated|Int32|Havuz için istenen işlem düğümleri sayısı.|
|enableAutoScale|Bool|Havuz boyutunu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|isAutoPool|Bool|Havuza bir işin AutoPool mekanizması oluşturulup oluşturulmadığını belirtir.|
|startTime|DateTime|Havuz yeniden boyutlandırma zaman başlatıldı.|
|endTime|DateTime|Tamamlanan havuz yeniden boyutlandırma zaman.|
|ResultCode|String|Yeniden boyutlandırma sonucu.|
|Sonuç iletisi|String|Yeniden boyutlandırma hatası sonucunun ayrıntılarını içerir.<br /><br /> Yeniden boyutlandırma başarıyla tamamlandı, işlemin başarılı olduğunu belirtir.|