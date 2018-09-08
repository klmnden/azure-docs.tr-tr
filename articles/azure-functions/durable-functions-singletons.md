---
title: Dayanıklı işlevler - Azure için teklileri
description: Teklileri dayanıklı Functons uzantısı'nda Azure işlevleri için kullanma
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 8177fb6e8dea83ab2b8b12183cdcca6e43f92ade
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095105"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) içindeki tekil düzenleyicileri

Arka plan işleri ya da aktör stili düzenlemeler, genellikle yalnızca bir örneğini sağlamak gereken belirli bir orchestrator aynı anda çalışır. Bu yapılabilir [dayanıklı işlevler](durable-functions-overview.md) belirli bir atayarak kimliği bir orchestrator için onu oluştururken örneği.

## <a name="singleton-example"></a>Tek örnek

Aşağıdaki C# örneği, bir singleton arka plan iş düzenleme oluşturan bir HTTP tetikleyici işlevi gösterir. Bu yalnızca bir örneği belirtilen örnek kimliği için mevcut kodu sağlar.

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

Varsayılan olarak, GUID rastgele Kimlikleridir örneği oluşturulur. Ancak bu durumda, örnek kimliği rota verileri URL'den geçirilir. Kod çağrıları [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) belirtilen Kimliğe sahip bir örneği zaten çalışıp çalışmadığını denetlemek için. Aksi takdirde, bu kimliğe sahip bir örneği oluşturulur

> [!NOTE]
> Bu örnekte, olası bir yarış durumu yoktur. İki örneğini **HttpStartSingle** iki farklı oluşturulabilir tekli olan diğer üzerine yazar örnekleri aynı anda yürütme sonucu. Gereksinimlerinize bağlı olarak, bu istenmeyen yan etkileri olabilir. Bu nedenle, hiçbir iki isteği Bu tetikleyici işlevi eşzamanlı olarak yürütebilir sağlamak önemlidir.

Uygulama ayrıntılarını orchestrator işlevi gerçekten önemli değil. Başlatan ve tamamlanan bir normal orchestrator işlevi olabilir veya sürekli çalışan bir olabilir (diğer bir deyişle, bir [dış düzenleme](durable-functions-eternal-orchestrations.md)). Aynı anda çalışan yalnızca bir örneğine olan önemli noktasıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırma hakkında bilgi edinin](durable-functions-sub-orchestrations.md)
