---
title: "Azure Batch görev başlangıç olayı | Microsoft Docs"
description: "Toplu Görev Başlangıç olayı referansı."
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
ms.openlocfilehash: c47ab36c99dddd46a14c15018a2a46bf7f873ffa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="task-start-event"></a>Görev Başlangıç olayı

 Bu olay, bir görev bir işlem düğümünde başlatmak için Zamanlayıcı tarafından zamanlandı sonra yayınlanır. Görev yeniden deneme işlemi ya da yeniden kuyruğa bu olay yeniden aynı görevi, ancak yeniden deneme sayısı için yayınlaması ve sistem görev sürümü buna göre güncelleştirilir unutmayın.


 Aşağıdaki örnek, bir görevin Başlat olay gösterir.

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
        "retryCount": 0
    }
}
```

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|İş kimliği|Dize|Görevi içeren işi kimliği.|
|id|Dize|Görev kimliği.|
|taskType|Dize|Görev türü. Bu, her bir iş yöneticisi görevi olduğunu gösteren'JobManager ' veya 'kullanıcı bir iş yöneticisi görevi değil belirten' olabilir.|
|systemTaskVersion|Int32|Bir görev iç yeniden deneme sayacı budur. Dahili olarak Batch hizmeti geçici sorunlar için hesap için bir görevi yeniden deneyebilirsiniz. Bu sorunları iç zamanlama hatalar içerebilir veya kurtarma girişimleri işlem düğümleri hatalı bir durumda.|
|[nodeInfo](#nodeInfo)|Karmaşık türü|İşlem düğümünde görevin çalıştırıldığı hakkındaki bilgileri içerir.|
|[multiInstanceSettings](#multiInstanceSettings)|Karmaşık türü|Görevi birden çok işlem düğümleri gerektiren çok örnekli görev olduğunu belirtir.  Bkz: [multiInstanceSettings](https://docs.microsoft.com/rest/api/batchservice/get-information-about-a-task) Ayrıntılar için.|
|[kısıtlamaları](#constraints)|Karmaşık türü|Bu görev için geçerli yürütme kısıtlamaları.|
|[executionInfo](#executionInfo)|Karmaşık türü|Görevin yürütülmesi hakkında bilgiler içerir.|

###  <a name="nodeInfo"></a>nodeInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|poolId|Dize|Görevin çalıştırıldığı havuzun kimliği.|
|nodeId|Dize|Görevin çalıştırıldığı düğümün kimliği.|

###  <a name="multiInstanceSettings"></a>multiInstanceSettings

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|numberOfInstances|Int|Görevin gerektirdiği işlem düğüm sayısı.|

###  <a name="constraints"></a>kısıtlamaları

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|maxTaskRetryCount|Int32|Maksimum görev yeniden sayısı. Çıkış kodu sıfır değilse toplu işlem hizmeti görevi yeniden dener.<br /><br /> Bu değer özellikle yeniden deneme sayısını kontrol edin. Toplu işlem hizmeti görevi bir kez dener ve bu sınıra kadar daha sonra yeniden deneyebilir. Örneğin, en fazla yeniden deneme sayısı 3, toplu çalıştığında bir görevin en fazla ise 4 (bir ilk deneyin ve 3 yeniden denemeyi) zaman.<br /><br /> En fazla yeniden deneme sayısının 0 olması durumunda toplu işlem hizmetinin görevleri yeniden denemez.<br /><br /> En fazla yeniden deneme sayısı -1 ise, Batch hizmeti görevleri sınır olmaksızın yeniden dener.<br /><br /> Varsayılan değer 0 (yeniden deneme)'dır.|

###  <a name="executionInfo"></a>executionInfo

|Öğe adı|Tür|Notlar|
|------------------|----------|-----------|
|retryCount|Int32|Görev Batch hizmeti tarafından denenen sayısı. Belirtilen MaxTaskRetryCount kadar bir sıfır olmayan çıkış kodu ile bulunup bulunmadığını görev denenir|
