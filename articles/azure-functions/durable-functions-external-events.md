---
title: Dayanıklı işlevler - Azure dış olayları işleme
description: Azure işlevleri için dayanıklı işlevler uzantısını dış olayların nasıl ele alınacağını öğrenin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 98380cc5b9daff314283ac4e45e5edf7b5601e1b
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49983955"
---
# <a name="handling-external-events-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) dış olayları işleme

Orchestrator işlevleri bekleyin ve dış olaylarını dinleyecek seçeneğine sahipsiniz. Bu özellik, [dayanıklı işlevler](durable-functions-overview.md) genellikle insan etkileşimi veya diğer dış Tetikleyicileri işlenmesi için kullanışlıdır.

## <a name="wait-for-events"></a>Olayların tamamlanmasını bekleme

[WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) yöntemi zaman uyumsuz olarak bekleyin ve bir dış olay dinlemek bir düzenleyici işlevi sağlar. Dinleme orchestrator işlev bildirir *adı* olayın ve *verinin şeklini* almak bekliyor.

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

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
> İşlev uygulamanızın bir tüketim planı kullanıyorsa, bir düzenleyici işlevi bir görevden bekliyor. ancak faturalandırma ücret uygulanmaz `WaitForExternalEvent`, ne kadar süre bekleyeceğini ne olursa olsun.

. NET'te olay yükü beklenen türe dönüştürülürse, `T`, bir özel durum oluşturulur.

## <a name="send-events"></a>Olayları gönderme

[RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı, olayları gönderir `WaitForExternalEvent` bekler.  `RaiseEventAsync` Yöntemi *eventName* ve *eventData* parametre olarak. Olay verileri, JSON seri hale getirilebilir olması gerekir.

Orchestrator işlevi örneğine "Onay" olay gönderen bir örnek kuyruk ile tetiklenen bir işlev aşağıda verilmiştir. Kuyruk iletisi gövdesinden düzenleme örnek kimliği gelir.

```csharp
[FunctionName("ApprovalQueueProcessor")]
public static async Task Run(
    [QueueTrigger("approval-queue")] string instanceId,
    [OrchestrationClient] DurableOrchestrationClient client)
{
    await client.RaiseEventAsync(instanceId, "Approval", true);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)
JavaScript'te bir rest çağrılacak sahibiz bir olay için dayanıklı işlevi bekliyor tetiklemek için API.
Aşağıdaki kod, "istek" paketini kullanır. Aşağıdaki yöntem, herhangi bir olay için dayanıklı işlevi herhangi bir örneğine yükseltmek için kullanılabilir

```js
function raiseEvent(instanceId, eventName) {
        var url = `<<BASE_URL>>/runtime/webhooks/durabletask/instances/${instanceId}/raiseEvent/${eventName}?taskHub=DurableFunctionsHub`;
        var body = <<BODY>>
            
        return new Promise((resolve, reject) => {
            request({
                url,
                json: body,
                method: "POST"
            }, (e, response) => {
                if (e) {
                    return reject(e);
                }

                resolve();
            })
        });
    }
```

<< BASE_URL >> temel URL'si olacaktır, işlev uygulaması. Kodu yerel olarak çalıştırıyorsanız sonra aşağıdakine benzer görünecektir http://localhost:7071 veya https://< olarak azure'da<functionappname>>. azurewebsites.net


Dahili olarak `RaiseEventAsync` kaybolmamasının bekleme Düzenleyici işlevi tarafından toplanmış bir ileti.

> [!WARNING]
> Belirtilen orchestration örneği yok ise *kimliği örnek* veya örneği belirtilen beklemiyorsa *olay adı*, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dış düzenlemeler ayarlamayı öğrenin](durable-functions-eternal-orchestrations.md)

> [!div class="nextstepaction"]
> [Dış olaylar için bekleyen bir örneği çalıştırma](durable-functions-phone-verification.md)

> [!div class="nextstepaction"]
> [İnsan etkileşimi için bekleyen bir örneği çalıştırma](durable-functions-phone-verification.md)

