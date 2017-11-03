---
title: "Dayanıklı işlevlerinde - Azure dış olayları işleme"
description: "Azure işlevleri için dayanıklı işlevleri uzantısında dış olayları işlemek öğrenin."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 23c99031ae3146a83867d10bd97d4eee8878188a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı işlevlerinde (Azure işlevleri) dış olayları işleme

Orchestrator işlevleri bekleyin ve dış olaylarını dinleyecek seçeneğine sahipsiniz. Bu özellik, [dayanıklı işlevleri](durable-functions-overview.md) genellikle insan etkileşimi veya diğer dış Tetikleyicileri işlemek için yararlıdır.

## <a name="wait-for-events"></a>Olayların tamamlanmasını bekleme

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) yöntemi zaman uyumsuz olarak bekleyip dış bir olayı dinleme orchestrator işlevi sağlar. Arayan bildirir *adı* olayın ve *veri şekli* almak bekliyor.

```csharp
[FunctionName("BudgetApproval")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    bool approved = await context.WaitForExternalEvent<bool>("Approval");
    if (approved)
    {
        // approval granted - do the approved action
    }
    else
    {
        // approval denied - send a notification
    }
}
```

Önceki örnekte, tek bir olay için dinler ve, alındığında eylemi gerçekleştirir.

Birden çok olaylarını dinleyecek aynı anda üç olası olay bildirimleri biri için bekleyeceği aşağıdaki örnekte, ister.

```csharp
[FunctionName("Select")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var event1 = context.WaitForExternalEvent<float>("Event1");
    var event2 = context.WaitForExternalEvent<bool>("Event2");
    var event3 = context.WaitForExternalEvent<int>("Event3");

    var winner = await Task.WhenAny(event1, event2, event3);
    if (winner == event1)
    {
        // ...
    }
    else if (winner == event2)
    {
        // ...
    }
    else if (winner == event3)
    {
        // ...
    }
}
```

Önceki örnekte dinler *herhangi* birden çok olay. Beklemek mümkündür *tüm* olayları.

```csharp
[FunctionName("NewBuildingPermit")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string applicationId = context.GetInput<string>();

    var gate1 = context.WaitForExternalEvent("CityPlanningApproval");
    var gate2 = context.WaitForExternalEvent("FireDeptApproval");
    var gate3 = context.WaitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    await Task.WhenAll(gate1, gate2, gate3);

    await context.CallActivityAsync("IssueBuildingPermit", applicationId);
}
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) için bazı giriş sonsuza kadar bekler.  İşlev uygulaması bekleme sırasında güvenli bir şekilde yüklenmemiş olabilir. Bir olay için bu orchestration örneği varsa ve geldiğinde, otomatik olarak uyandırdı ve hemen olayını işler.

> [!NOTE]
> Bir orchestrator işlevi görevden bekleyen sırada fatura hiçbir Ücret tahakkuk eden `WaitForExternalEvent`, ne kadar bekleyeceğini olsun.

Beklenen türe dönüştürülemiyor olay yükü `T`, bir özel durum.

## <a name="send-events"></a>Olayları gönderme

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı, olaylar gönderir `WaitForExternalEvent` bekler.  `RaiseEventAsync` Yöntemi alır *eventName* ve *eventData* parametre olarak. Olay verileri JSON Serileştirilebilir olmalıdır.

Orchestrator işlevi örneğine "Onay" olay gönderen bir örnek sıra tetiklemeli işlevin aşağıdadır. Orchestration örnek kimliği kuyruk iletisini gövdesinden gelir.

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

Dahili olarak, `RaiseEventAsync` enqueues bekleme orchestrator işleviyle toplanma bir ileti.

> [!WARNING]
> Belirtilen orchestration örneği yok ise *kimliği örneği* veya örnek belirtilen değil bekliyorsa *olay adı*, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Eternal düzenlemelerin öğrenin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Dış olaylar için bekleyen bir örnek çalıştırın](durable-functions-counter.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimi için bekleyen bir örnek çalıştırın](durable-functions-phone-verification.md)

