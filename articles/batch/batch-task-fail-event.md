---
title: Azure Batch görev başarısızlık olayı | Microsoft Docs
description: Toplu görev için başvuru event başarısız.
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
ms.date: 05/15/2018
ms.author: v-junlch
ms.openlocfilehash: f37769ceb761b8c8bc4834568813bb1b7af7f66a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60549997"
---
# <a name="task-fail-event"></a>Görev başarısızlık olayı

 Bu olay ile hata bir görev tamamlandığında yayılır. Şu anda tüm sıfır olmayan çıkış kodları hata olarak kabul edilir. Bu olay yayılan *ek olarak* görev tamamlama olayı ve bir görev başarısız olduğunda algılamak için kullanılabilir.


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
|iş kimliği|String|Görevi içeren işi kimliği.|
|id|String|Görevin kimliği.|
|taskType|String|Görev türü. Bu, 'bir iş yöneticisi görevi, belirten JobManager' olabilir veya bir iş yöneticisi görevi değil belirten'User ' olabilir. Bu olay, iş hazırlama görevleri, iş sürüm görevleri veya başlangıç görevleri yayılan değil.|
|systemTaskVersion|Int32|Bu görevde iç yeniden deneme sayacı toplanır. Dahili olarak Batch hizmeti hesap için geçici bir sorun için bir görevi yeniden deneyebilirsiniz. Bu sorunları iç zamanlama hatalar içerebilir veya girişimleri, kurtarılır işlem düğümleri hatalı durumda.|
|[nodeInfo](#nodeInfo)|Karmaşık Tür|İşlem düğümünde görevin çalıştırıldığı hakkındaki bilgileri içerir.|
|[multiInstanceSettings](#multiInstanceSettings)|Karmaşık Tür|Görev birden çok işlem düğümü gerektiren bir çok örnekli görev olduğunu belirtir.  Bkz: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) Ayrıntılar için.|
|[Kısıtlamaları](#constraints)|Karmaşık Tür|Bu görevin geçerli yürütme kısıtlamaları.|
|[executionInfo](#executionInfo)|Karmaşık Tür|Görevin yürütülmesi hakkında bilgi içerir.|

###  <a name="nodeInfo"></a> nodeInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|poolId|String|Görevin çalıştırıldığı Havuz kimliği.|
|NodeId|String|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiInstanceSettings"></a> multiInstanceSettings

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|numberOfInstances|Int32|Görevin gerektirdiği işlem düğüm sayısı.|

###  <a name="constraints"></a> Kısıtlamaları

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maksimum görev yeniden deneme sayısı. Çıkış kodu sıfır değilse toplu işlem hizmeti görevi yeniden dener.<br /><br /> Bu değer özel yeniden deneme sayısını kontrol edin. Batch hizmeti görevi bir kez dener ve sonra bu sınıra kadar yeniden deneyebilir. Örneğin, en fazla yeniden deneme sayısı 3, Batch çalışır bir görevin en fazla ise 4 (bir ilk deneme ve 3 yeniden deneme) zaman.<br /><br /> En fazla yeniden deneme sayısının 0 olması durumunda toplu işlem hizmeti görevleri yeniden denemez.<br /><br /> En fazla yeniden deneme sayısı -1 olması durumunda, Batch hizmeti görevleri sınır olmaksızın yeniden dener.<br /><br /> Varsayılan değer 0 (yeniden deneme yok) ' dir.|


###  <a name="executionInfo"></a> executionInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|startTime|DateTime|Hangi çalışan görev başlama zamanı. 'Çalışıyor' karşılık gelen **çalıştıran** durum görev kaynak dosyaları veya uygulama paketlerini belirtir, başlangıç zamanı, görevi başlattığınız indirerek veya bu dağıtımı zaman yansıtmasını sağlayın.  Görev yeniden ya da yeniden, çalışan, görevi başlattığınız en son zamanı budur.|
|endTime|DateTime|Hangi görev tamamlanma zamanı.|
|ExitCode|Int32|Görevin çıkış kodu.|
|RetryCount|Int32|Görev Batch hizmeti tarafından yeniden deneme sayısı. Belirtilen MaxTaskRetryCount kadar bir sıfır olmayan çıkış kodu ile çıkılıyorsa görevi yeniden denenir.|
|requeueCount|Int32|Batch hizmeti tarafından bir kullanıcı isteğinin sonucu olarak görev yeniden kuyruğa sayısı.<br /><br /> Çalışan düğümlerine görevleri yeniden boyutlandırma veya havuza küçültme) bir havuz (veya zaman işi devre dışı bırakılıyor, kullanıcı kullanıcı kaldırır düğümleri belirttiğinizde, yürütme için yeniden kuyruğa. Bu sayı, bu nedenle kaç kez görevi yeniden kuyruğa izler.|

<!-- Update_Description: update metedata properties -->