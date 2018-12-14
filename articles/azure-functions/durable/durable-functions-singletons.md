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
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: b083b9a09b478ca5ad68e19d3a2133fb529da851
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/13/2018
ms.locfileid: "53342961"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) içindeki tekil düzenleyicileri

Arka plan işleri ya da aktör stili düzenlemeler, genellikle yalnızca bir örneğini sağlamak gereken belirli bir orchestrator aynı anda çalışır. Bu yapılabilir [dayanıklı işlevler](durable-functions-overview.md) belirli bir atayarak kimliği bir orchestrator için onu oluştururken örneği.

## <a name="singleton-example"></a>Tek örnek

Aşağıdaki C# ve JavaScript örnekler bir singleton arka plan iş düzenleme oluşturan bir HTTP tetikleyici işlevi. Bu yalnızca bir örneği belirtilen örnek kimliği için mevcut kodu sağlar.

### <a name="c"></a>C#

```cs
[FunctionName("HttpStartSingle")]
public static async Task<HttpResponseMessage> RunSingle(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/{instanceId}")] HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient starter,
    string functionName,
    string instanceId,
    ILogger log)
{
    // Check if an instance with the specified ID already exists.
    var existingInstance = await starter.GetStatusAsync(instanceId);
    if (existingInstance == null)
    {
        // An instance with the specified ID doesn't exist, create one.
        dynamic eventData = await req.Content.ReadAsAsync<object>();
        await starter.StartNewAsync(functionName, instanceId, eventData);
        log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionsName;

    // Check if an instance with the specified ID already exists.
    const existingInstance = await client.getStatus(instanceId);
    if (!existingInstance) {
        // An instance with the specified ID doesn't exist, create one.
        const eventData = req.body;
        await client.startNew(functionName, instanceId, eventData);
        context.log(`Started orchestration with ID = '${instanceId}'.`);
        return client.createCheckStatusResponse(req, instanceId);
    } else {
        // An instance with the specified ID exists, don't create one.
        return {
            status: 409,
            body: `An instance with ID '${instanceId}' already exists.`,
        };
    }
};
```

Varsayılan olarak, GUID rastgele Kimlikleridir örneği oluşturulur. Ancak bu durumda, örnek kimliği rota verileri URL'den geçirilir. Kod çağrıları [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetStatusAsync_) (C#) veya `getStatus` belirtilen Kimliğe sahip bir örneği zaten çalışıp çalışmadığını denetlemek için (JavaScript). Aksi takdirde, bu kimliğe sahip bir örneği oluşturulur

> [!WARNING]
> JavaScript içinde yerel olarak geliştirirken, ortam değişkenini ayarlamak gerekir `WEBSITE_HOSTNAME` için `localhost:<port>`, örn. `localhost:7071` yöntemleri kullanmak üzere `DurableOrchestrationClient`. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

> [!NOTE]
> Bu örnekte, olası bir yarış durumu yoktur. İki örneğini **HttpStartSingle** iki farklı oluşturulabilir tekli olan diğer üzerine yazar örnekleri aynı anda yürütme sonucu. Gereksinimlerinize bağlı olarak, bu istenmeyen yan etkileri olabilir. Bu nedenle, hiçbir iki isteği Bu tetikleyici işlevi eşzamanlı olarak yürütebilir sağlamak önemlidir.

Uygulama ayrıntılarını orchestrator işlevi gerçekten önemli değil. Başlatan ve tamamlanan bir normal orchestrator işlevi olabilir veya sürekli çalışan bir olabilir (diğer bir deyişle, bir [dış düzenleme](durable-functions-eternal-orchestrations.md)). Aynı anda çalışan yalnızca bir örneğine olan önemli noktasıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırma hakkında bilgi edinin](durable-functions-sub-orchestrations.md)
