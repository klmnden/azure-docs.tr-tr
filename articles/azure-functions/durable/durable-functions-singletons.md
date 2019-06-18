---
title: Dayanıklı işlevler - Azure için teklileri
description: Teklileri Azure işlevleri için dayanıklı işlevler uzantısını kullanma
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: c032ba046668310ff71d067d22a805fc6446667c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64683796"
---
# <a name="singleton-orchestrators-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) içindeki tekil düzenleyicileri

Arka plan işleri genellikle emin olmanız gerekir, aynı anda belirli bir orchestrator yalnızca bir örneğini çalıştırır. Bu yapılabilir [dayanıklı işlevler](durable-functions-overview.md) belirli bir atayarak kimliği bir orchestrator için onu oluştururken örneği.

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

Function.json dosyası aşağıda verilmiştir:
```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "route": "orchestrators/{functionName}/{instanceId}",
      "methods": ["post"]
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

JavaScript kod aşağıdaki gibidir:
```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instanceId = req.params.instanceId;
    const functionName = req.params.functionName;

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
> Bu örnekte, olası bir yarış durumu yoktur. İki örneğini **HttpStartSingle** eşzamanlı olarak yürütün, her iki işlev çağrıları başarılı olarak bildirir, ancak yalnızca bir düzenleme örneği gerçekten başlayacak. Gereksinimlerinize bağlı olarak, bu istenmeyen yan etkileri olabilir. Bu nedenle, hiçbir iki isteği Bu tetikleyici işlevi eşzamanlı olarak yürütebilir sağlamak önemlidir.

Uygulama ayrıntılarını orchestrator işlevi gerçekten önemli değil. Başlatan ve tamamlanan bir normal orchestrator işlevi olabilir veya sürekli çalışan bir olabilir (diğer bir deyişle, bir [dış düzenleme](durable-functions-eternal-orchestrations.md)). Aynı anda çalışan yalnızca bir örneğine olan önemli noktasıdır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Alt düzenlemeleri çağırma hakkında bilgi edinin](durable-functions-sub-orchestrations.md)
