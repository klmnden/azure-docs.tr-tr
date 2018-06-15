---
title: Teklileri dayanıklı işlev - Azure
description: Teklileri dayanıklı Functons uzantısı'nda Azure işlevlerinin için nasıl kullanılacağı.
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
ms.openlocfilehash: ea8b5db946d6b35ea4583d9170ec36e5f95e16cd
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/17/2018
ms.locfileid: "29972560"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Singleton orchestrators dayanıklı işlevlerinde (Azure işlevleri)

Arka plan işleri veya aktör stili düzenlemelerin, genellikle yalnızca bir örneğini sağlamak zorunda için belirli bir orchestrator aynı anda çalıştırılır. Bu yapılabilir [dayanıklı işlevleri](durable-functions-overview.md) belirli bir atayarak kimliği için bir orchestrator onu oluştururken örneği.

## <a name="singleton-example"></a>Tek örnek

Aşağıdaki C# örnek bir singleton arka plan işi orchestration oluşturan bir HTTP tetikleyicisi işlevi gösterir. Bu yalnızca bir örneğini belirtilen örneği için bir kimlik var. kodu sağlar

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    TraceWriter log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.Info($"Started orchestration with ID = '{instanceId}'.");
        return starter.CreateCheckStatusResponse(req, instanceId);
    }
    else
    {
        // An instance with the specified ID exists, don't create one.
        return req.CreateErrorResponse(
            HttpStatusCode.Conflict,
            $"An instance with ID '{instanceId}' already exists.");
    }
}
```

Varsayılan olarak, GUID rastgele ID'lerini örneği oluşturulur. Ancak bu durumda, örnek kimliği rota verileri URL'den geçirilir. Kod çağrıları [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) belirtilen Kimliğe sahip bir örneği zaten çalışıp çalışmadığını denetlemek için. Aksi takdirde, bu kimliğe sahip bir örneğinin oluşturulduğu

Orchestrator işlevi uygulama ayrıntılarını gerçekten önemli değildir. Başlatır ve tamamlandığında normal orchestrator işlev olabilir ya da devamlı çalıştıran biri olabilir (diğer bir deyişle, bir [Eternal Orchestration](durable-functions-eternal-orchestrations.md)). En önemli nokta yalnızca hiç bir örneğinin aynı anda çalışıyor olmasıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemelerin çağrı öğrenin](durable-functions-sub-orchestrations.md)
