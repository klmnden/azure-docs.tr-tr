---
title: Dayanıklı işlevler - Azure dış olayları işleme
description: Azure işlevleri için dayanıklı işlevler uzantısını dış olayların nasıl ele alınacağını öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: eb024e11b78d13d5ab4544c634acef2ade8141c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730795"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) dış olayları işleme

Orchestrator işlevleri bekleyin ve dış olaylarını dinleyecek seçeneğine sahipsiniz. Bu özellik, [dayanıklı işlevler](durable-functions-overview.md) genellikle insan etkileşimi veya diğer dış Tetikleyicileri işlenmesi için kullanışlıdır.

> [!NOTE]
> Tek yönlü zaman uyumsuz işlemler dış olaylardır. Bunlar, nerede olayı gönderirken istemci orchestrator işlevi zaman uyumlu bir yanıt gerektiği durumlar için uygun değildir.

## <a name="wait-for-events"></a>Olayların tamamlanmasını bekleme

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) yöntemi zaman uyumsuz olarak bekleyin ve bir dış olay dinlemek bir düzenleyici işlevi sağlar. Dinleme orchestrator işlev bildirir *adı* olayın ve *verinin şeklini* almak bekliyor.

### <a name="c"></a>C#

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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const approved = yield context.df.waitForExternalEvent("Approval");
    if (approved) {
        // approval granted - do the approved action
    } else {
        // approval denied - send a notification
    }
});
```

Yukarıdaki örnekte, belirli tek bir olay için dinler ve, alındığında eylemi gerçekleştirir.

İçin birden çok olayı dinleyebilir üç olası olay bildirimlerinden birini bekler aşağıdaki örnekte, aynı anda ister.

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const event1 = context.df.waitForExternalEvent("Event1");
    const event2 = context.df.waitForExternalEvent("Event2");
    const event3 = context.df.waitForExternalEvent("Event3");

    const winner = yield context.df.Task.any([event1, event2, event3]);
    if (winner === event1) {
        // ...
    } else if (winner === event2) {
        // ...
    } else if (winner === event3) {
        // ...
    }
});
```

Önceki örnekte dinler *herhangi* birden çok olay. Beklenecek mümkündür *tüm* olayları.

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const applicationId = context.df.getInput();

    const gate1 = context.df.waitForExternalEvent("CityPlanningApproval");
    const gate2 = context.df.waitForExternalEvent("FireDeptApproval");
    const gate3 = context.df.waitForExternalEvent("BuildingDeptApproval");

    // all three departments must grant approval before a permit can be issued
    yield context.df.Task.all([gate1, gate2, gate3]);

    yield context.df.callActivity("IssueBuildingPermit", applicationId);
});
```

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) bazı giriş süresiz olarak bekler.  İşlev uygulaması, bekleme sırasında güvenli bir şekilde yüklenmemiş olabilir. Bu düzenleme örneği için bir olay ulaşan durumunda otomatik olarak sunucuyu uyandırdı ve hemen olayını işler.

> [!NOTE]
> İşlev uygulamanızın bir tüketim planı kullanıyorsa, bir düzenleyici işlevi bir görevden bekliyor. ancak faturalandırma ücret uygulanmaz `WaitForExternalEvent` (.NET) veya `waitForExternalEvent` (JavaScript) ne kadar süre bekleyeceğini ne olursa olsun.

. NET'te olay yükü beklenen türe dönüştürülürse, `T`, bir özel durum oluşturulur.

## <a name="send-events"></a>Olayları gönderme

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı, olayları gönderir `WaitForExternalEvent` (.NET) veya `waitForExternalEvent` (JavaScript) bekler.  `RaiseEventAsync` Yöntemi *eventName* ve *eventData* parametre olarak. Olay verileri, JSON seri hale getirilebilir olması gerekir.

Orchestrator işlevi örneğine "Onay" olay gönderen bir örnek kuyruk ile tetiklenen bir işlev aşağıda verilmiştir. Kuyruk iletisi gövdesinden düzenleme örnek kimliği gelir.

### <a name="c"></a>C#

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);
    await client.raiseEvent(instanceId, "Approval", true);
};
```

Dahili olarak `RaiseEventAsync` (.NET) veya `raiseEvent` (JavaScript) kaybolmamasının bekleme Düzenleyici işlevi tarafından toplanmış bir ileti. Örneği belirtilen beklemiyorsa *olay adı* olay iletisi bir bellek içi kuyruğuna eklenir. Orchestration örneği için dinleme daha sonra başladığında *olay adı* olay iletileri kuyruğa kontrol eder.

> [!NOTE]
> Belirtilen orchestration örneği yok ise *kimliği örnek*, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29). 

> [!WARNING]
> JavaScript içinde yerel olarak geliştirirken, ortam değişkenini ayarlamak gerekir `WEBSITE_HOSTNAME` için `localhost:<port>`, örn. `localhost:7071` yöntemleri kullanmak üzere `DurableOrchestrationClient`. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış düzenlemeler ayarlamayı öğrenin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Dış olaylar için bekleyen bir örneği çalıştırma](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimi için bekleyen bir örneği çalıştırma](durable-functions-phone-verification.md)