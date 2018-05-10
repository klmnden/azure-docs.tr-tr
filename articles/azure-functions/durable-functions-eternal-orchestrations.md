---
title: Eternal düzenlemelerin dayanıklı işlevlerinde - Azure
description: Azure işlevleri için dayanıklı işlevleri uzantısını kullanarak eternal düzenlemelerin uygulama öğrenin.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: f42526430599e47e673d359433e91b4687cbeb9e
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Eternal düzenlemelerin dayanıklı işlevlerinde (Azure işlevleri)

*Eternal düzenlemelerin* asla sona orchestrator işlevlerdir. Kullanmak istediğinizde yararlı oldukları [dayanıklı işlevleri](durable-functions-overview.md) toplayıcısını ve sonsuz bir döngüde gerektiren her senaryo için.

## <a name="orchestration-history"></a>Orchestration geçmişi

İçinde anlatıldığı gibi [denetim noktası oluşturma ve yeniden yürütme](durable-functions-checkpointing-and-replay.md), dayanıklı görev Framework her işlevi orchestration geçmişini izler. Bu geçmiş yeni çalışması zamanlamak orchestrator işlevi devam sürekli sürece artar. Orchestrator işlevi sonsuz döngüye girer ve sürekli olarak iş zamanlamaları, bu geçmişi oldukça büyük büyümesine ve önemli performans sorunlarına neden olabilir. *Eternal orchestration* kavram, bu tür sorunları sonsuz döngüler gereken uygulamalar için azaltmak için tasarlanmıştır.

## <a name="resetting-and-restarting"></a>Sıfırlamak ve yeniden başlatma

Sonsuz döngüler kullanmak yerine, orchestrator işlevleri durumlarına çağırarak sıfırlama [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) yöntemi. Bu yöntem, yeni giriş için orchestrator işlevi nesil hale tek bir JSON seri hale getirilebilir parametre alır.

Zaman `ContinueAsNew` çağrılır, kendisine sonlandırılır önce bir ileti örneği enqueues. İleti, yeni giriş değeri örneğiyle yeniden başlatır. Aynı örnek kimliği tutulur, ancak orchestrator işlevin geçmişi etkili bir şekilde kesilir.

> [!NOTE]
> Dayanıklı görev Framework aynı örnek kimliği korur ancak dahili olarak yeni bir oluşturur *yürütme kimliği* tarafından sıfırlama orchestrator işlevi için `ContinueAsNew`. Bu yürütme kimliği genellikle dışarıdan gösterilmez, ancak orchestration yürütme hata ayıklama sırasında bilmenizi yararlı olabilir.

> [!NOTE]
> `ContinueAsNew` Yöntemi henüz JavaScript'te kullanılabilir değil.

## <a name="periodic-work-example"></a>Periyodik iş örneği

Bir kullanım örneği eternal düzenlemelerin için düzenli iş süresiz olarak yapması gereken kodudur.

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

Bu örnek Zamanlayıcı tetiklenen bir işlev arasındaki fark, bir zamanlamaya göre temizleme tetikleyici burada kez dayalı olmayan ' dir. Örneğin, her saat bir işlev yürüten CRON zamanlama, 1: 00'de, 2:00, 3:00 yürütecek vb. ve çakışma sorunla potansiyel olarak çalışabilir. Temizleme 30 dakikadan uzun sürerse Bu örnekte, ancak daha sonra onu 1: 00'de, 2:30 ' zamanlanacak 4:00, vb. ve hiçbir çakışma olasılığı vardır.

## <a name="counter-example"></a>Sayaç örneği

Basitleştirilmiş bir örneği burada verilmiştir bir *sayaç* dinler işlevi *artırma* ve *azaltma* sonsuza kadar olaylar.

```csharp
[FunctionName("SimpleCounter")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    int counterState = context.GetInput<int>();

    string operation = await context.WaitForExternalEvent<string>("operation");

    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }
    
    context.ContinueAsNew(counterState);
}
```

## <a name="exit-from-an-eternal-orchestration"></a>Bir eternal orchestration Çık

Bir orchestrator işlevi sonunda tamamlaması gereken sonra yapmanız gereken tek şey *değil* çağrısı `ContinueAsNew` ve çıkış işlevi sağlar.

Sonsuz bir döngüde ve durdurulması gerekiyor orchestrator işlevidir kullanırsanız [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) durdurmak için yöntem. Daha fazla bilgi için bkz: [örnek Yönetimi](durable-functions-instance-management.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Singleton düzenlemelerin uygulamak öğrenin](durable-functions-singletons.md)
