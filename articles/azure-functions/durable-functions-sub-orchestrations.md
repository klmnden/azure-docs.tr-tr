---
title: Alt düzenlemelerin dayanıklı işlev - Azure
description: Azure işlevleri için dayanıklı işlevleri uzantısında düzenlemelerin düzenlemelerin çağırmanıza yapma.
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
ms.openlocfilehash: 7545a371749ed9af88f08af23cce3a513f494374
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="sub-orchestrations-in-durable-functions-azure-functions"></a>Alt düzenlemelerin dayanıklı işlevlerinde (Azure işlevleri)

Etkinlik işlevleri çağırma ek olarak orchestrator işlevleri diğer orchestrator işlevleri çağırabilir. Örneğin, bir kitaplık büyük orchestration orchestrator işlevlerin oluşturabilirsiniz. Veya bir orchestrator işlevinde birden çok örneğini paralel olarak çalıştırılabilir.

Bir orchestrator işlevi başka bir orchestrator işlevini çağırarak çağırabilir [CallSubOrchestratorAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorAsync_) veya [CallSubOrchestratorWithRetryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CallSubOrchestratorWithRetryAsync_) yöntemi. [Hata işleme ve maaş](durable-functions-error-handling.md#automatic-retry-on-failure) makale otomatik yeniden deneme hakkında daha fazla bilgi sahiptir.

Alt orchestrator işlevleri arayanın açısından etkinlik işlevleri gibi davranır. Bir değer döndürmeleri bir özel durum ve üst orchestrator işleviyle beklemenin.

> [!NOTE]
> `CallSubOrchestratorAsync` Ve `CallSubOrchestratorWithRetryAsync` yöntemleri henüz JavaScript'te kullanılabilir değildir.

## <a name="example"></a>Örnek

Aşağıdaki örnek bir IOT ("nesnelerin interneti") senaryo gösterilmektedir sağlanması birden çok aygıt olduğu. Her şey aşağıdaki gibi görünebilir cihazların gerçekleşmesi belirli bir orchestration vardır:

```csharp
public static async Task DeviceProvisioningOrchestration(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string deviceId = ctx.GetInput<string>();

    // Step 1: Create an installation package in blob storage and return a SAS URL.
    Uri sasUrl = await ctx.CallActivityAsync<Uri>("CreateInstallationPackage", deviceId);

    // Step 2: Notify the device that the installation package is ready.
    await ctx.CallActivityAsync("SendPackageUrlToDevice", Tuple.Create(deviceId, sasUrl));

    // Step 3: Wait for the device to acknowledge that it has downloaded the new package.
    await ctx.WaitForExternalEvent<bool>("DownloadCompletedAck");

    // Step 4: ...
}
```

Bu orchestrator işlevi olarak kullanılabilir-olduğu için bir kerelik cihaz sağlama veya daha büyük bir düzenleme parçası olabilir. İkinci durumda, üst orchestrator işlevi örneklerini zamanlayabilirsiniz `DeviceProvisioningOrchestration` kullanarak `CallSubOrchestratorAsync` API.

Burada, paralel olarak birden çok orchestrator işlevlerini çalıştırmak üzere nasıl oluşturulduğunu gösteren bir örnek verilmiştir.

```csharp
[FunctionName("ProvisionNewDevices")]
public static async Task ProvisionNewDevices(
    [OrchestrationTrigger] DurableOrchestrationContext ctx)
{
    string[] deviceIds = await ctx.CallActivityAsync<string[]>("GetNewDeviceIds");

    // Run multiple device provisioning flows in parallel
    var provisioningTasks = new List<Task>();
    foreach (string deviceId in deviceIds)
    {
        Task provisionTask = ctx.CallSubOrchestratorAsync("DeviceProvisioningOrchestration", deviceId);
        provisioningTasks.Add(provisionTask);
    }

    await Task.WhenAll(provisioningTasks);

    // ...
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Görev hub nedir ve nasıl yapılandırılacakları öğrenin](durable-functions-task-hubs.md)
