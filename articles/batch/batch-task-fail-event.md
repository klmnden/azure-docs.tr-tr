---
title: Azure Batch görev başarısız olay | Microsoft Docs
description: Toplu Görev başvurusunu olay başarısız.
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
ms.openlocfilehash: c4636ebbfb6737be07d01a3861ec6b9da66ff966
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
ms.locfileid: "30313105"
---
# <a name="task-fail-event"></a>Görev başarısızlık olayı

 Bu olay, bir hata ile bir görev tamamlandıktan sonra yayınlanır. Şu anda tüm sıfır olmayan çıkış kodları hata olarak kabul edilir. Bu olay yayılan *ek olarak* bir görevi tamamlamak olay ve bir görev başarısız olduğunda algılamak için kullanılabilir.


 Aşağıdaki örnek, bir görevin başarısız olay gösterir.

```
{
    "jobId": "job-0000000001",
    "id": "task-5",
    "taskType": "User",
    "systemTaskVersion": 0,
    "nodeInfo": {
        "poolId": "pool-001",
        "nodeId": "tvm-257509324_1-20160908t162728z"
    },
    "multiInstanceSettings": {
        "numberOfInstances": 1
    },
    "constraints": {
        "maxTaskRetryCount": 2
    },
    "executionInfo": {
        "startTime": "2016-09-08T16:32:23.799Z",
        "endTime": "2016-09-08T16:34:00.666Z",
        "exitCode": 1,
        "retryCount": 2,
        "requeueCount": 0
    }
}
```

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|jobId|Dize|Görevi içeren işi kimliği.|
|id|Dize|Görev kimliği.|
|taskType|Dize|Görev türü. Bu, her bir iş yöneticisi görevi olduğunu gösteren'JobManager ' veya 'kullanıcı bir iş yöneticisi görevi değil belirten' olabilir. Bu olay, iş hazırlama görevleri, iş sürüm görevleri veya başlangıç görevleri için gösterilen değil.|
|systemTaskVersion|Int32|Bir görev iç yeniden deneme sayacı budur. Dahili olarak Batch hizmeti geçici sorunlar için hesap için bir görevi yeniden deneyebilirsiniz. Bu sorunları iç zamanlama hatalar içerebilir veya kurtarma girişimleri işlem düğümleri hatalı bir durumda.|
|[nodeInfo](#nodeInfo)|Karmaşık Tür|İşlem düğümünde görevin çalıştırıldığı hakkındaki bilgileri içerir.|
|[multiInstanceSettings](#multiInstanceSettings)|Karmaşık Tür|Görevi birden çok işlem düğümleri gerektiren çok örnekli görev olduğunu belirtir.  Bkz: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) Ayrıntılar için.|
|[Kısıtlamaları](#constraints)|Karmaşık Tür|Bu görev için geçerli yürütme kısıtlamaları.|
|[executionInfo](#executionInfo)|Karmaşık Tür|Görevin yürütülmesi hakkında bilgiler içerir.|

###  <a name="nodeInfo"></a> nodeInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|poolId|Dize|Görevin çalıştırıldığı havuzun kimliği.|
|nodeId|Dize|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|numberOfInstances|Int32|Görevin gerektirdiği işlem düğüm sayısı.|

###  <a name="constraints"></a> Kısıtlamaları

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maksimum görev yeniden sayısı. Çıkış kodu sıfır değilse toplu işlem hizmeti görevi yeniden dener.<br /><br /> Bu değer özellikle yeniden deneme sayısını kontrol edin. Toplu işlem hizmeti görevi bir kez dener ve bu sınıra kadar daha sonra yeniden deneyebilir. Örneğin, en fazla yeniden deneme sayısı 3, toplu çalıştığında bir görevin en fazla ise 4 (bir ilk deneyin ve 3 yeniden denemeyi) zaman.<br /><br /> En fazla yeniden deneme sayısının 0 olması durumunda toplu işlem hizmetinin görevleri yeniden denemez.<br /><br /> En fazla yeniden deneme sayısı -1 ise, Batch hizmeti görevleri sınır olmaksızın yeniden dener.<br /><br /> Varsayılan değer 0 (yeniden deneme)'dır.|


###  <a name="executionInfo"></a> executionInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|startTime|DateTime|Görev çalıştıran başladığı zaman. 'Çalışıyor' karşılık gelen **çalıştıran** görev kaynak dosyaları veya uygulama paketleri belirtiyorsa, başlangıç saati görev başladığı indirilirken veya bunlar dağıtma zaman yansıtır şekilde belirtin.  Görev yeniden ya da yeniden, bu görev çalıştıran başladığı en son ne zaman olur.|
|endTime|DateTime|Hangi görev tamamlandı süre.|
|exitCode|Int32|Görev çıkış kodu.|
|retryCount|Int32|Görev Batch hizmeti tarafından denenen sayısı. Belirtilen MaxTaskRetryCount kadar bir sıfır olmayan çıkış kodu ile bulunup bulunmadığını görev denenir.|
|requeueCount|Int32|Batch hizmeti tarafından bir kullanıcı isteğin sonucu olarak görev yeniden kuyruğa sayısı.<br /><br /> Yeniden boyutlandırma veya havuza küçültme) bir havuz (veya ne zaman işi devre dışı bırakılıyor, kullanıcı kullanıcı kaldırır düğümleri çalışan düğümlerine görevleri belirttiğinizde çalıştırılmak üzere yeniden kuyruğa. Bu sayı, şu nedenlerden dolayı kaç kez görev sıraya izler.|
