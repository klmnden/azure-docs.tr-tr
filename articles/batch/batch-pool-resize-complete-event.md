---
title: Azure Batch havuzu yeniden boyutlandırma complete olayını | Microsoft Docs
description: Batch havuzundaki başvurusunu complete olayını yeniden boyutlandırın.
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
ms.openlocfilehash: e91ba664a69d28cae1f82710d427bd2a391305a2
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30315670"
---
# <a name="pool-resize-complete-event"></a>Havuz yeniden boyutlandırma tamamlama olayı

 Havuzu yeniden boyutlandırma tamamlandı veya başarısız olduğunda bu olay yayınlanır.

 Aşağıdaki örnek boyutu artar ve başarıyla tamamlandı bir havuz için havuzu yeniden boyutlandırma complete olayını gövdesi gösterir.

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
|id|Dize|Havuzun kimliği.|
|nodeDeallocationOption|Dize|Havuz boyutunun küçülmesi durumunda ne zaman düğüm havuzdan kaldırılabilir belirtir.<br /><br /> Olası değerler şunlardır:<br /><br /> **requeue** – yürütülen görevleri sonlandırır ve onları yeniden kuyruğa alır. İş etkinleştirildiğinde görevler yeniden yürütülür. Görevler sonlandırıldı hemen düğümleri kaldırın.<br /><br /> **sonlandırma** – çalışan görevlerin sonlandır. Görevler yeniden çalışmaz. Görevler sonlandırıldı hemen düğümleri kaldırın.<br /><br /> **net_offline_option** – tamamlamak için izin şu anda çalışan görevler. Beklenirken hiç yeni görev zamanlamaz. Tüm görevler tamamlandığında düğümleri kaldırın.<br /><br /> **Retaineddata** -izin şu anda çalışan görevlerin tamamlayın ve ardından tüm veri saklama sürelerini süresi dolmak üzere görev için bekleyin. Beklenirken hiç yeni görev zamanlamaz. Tüm görev bekletme süreleri dolduğunda düğümleri kaldırın.<br /><br /> Requeue varsayılan değerdir.<br /><br /> Havuz boyutunun artırılması sonra değer kümesine **geçersiz**.|
|currentDedicated|Int32|Şu anda havuzuna atanmış işlem düğümleri sayısı.|
|targetDedicated|Int32|Havuzu için istenen işlem düğümleri sayısı.|
|enableAutoScale|Bool|Havuz boyutu otomatik olarak zaman içinde ayarlar olup olmadığını belirtir.|
|isAutoPool|Bool|Bir işin AutoPool mekanizması havuzu oluşturulup oluşturulmadığını belirtir.|
|startTime|DateTime|Havuzu yeniden boyutlandırma zaman başlatıldı.|
|endTime|DateTime|Tamamlanan havuzu yeniden boyutlandırma süre.|
|resultCode|Dize|Yeniden boyutlandırma sonucu.|
|resultMessage|Dize|Yeniden boyutlandırma hatası sonucu ayrıntılarını içerir.<br /><br /> Yeniden boyutlandırma formu başarıyla tamamlanırsa işlemin başarılı olduğunu belirtir.|