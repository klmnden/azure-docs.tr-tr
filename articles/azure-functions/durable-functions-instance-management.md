---
title: "Dayanıklı işlevleri - Azure durumlarda yönetme"
description: "Azure işlevleri için dayanıklı işlevleri uzantısı durumlarda yönetmeyi öğrenin."
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
ms.openlocfilehash: a938e5949896ad3bfa91903106d56ccdf827c725
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2018
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) durumlarda yönetme

[Dayanıklı işlevleri](durable-functions-overview.md) orchestration örnekleri başlatılabilir, sonlandırıldı, sorgulanan ve bildirim olayları gönderilir. Tüm örnek Yönetimi yapılır kullanarak [bağlama orchestration istemci](durable-functions-bindings.md). Bu makalede her örnek yönetim işlemi ayrıntılara gider.

## <a name="starting-instances"></a>Başlangıç örnekleri

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) bir orchestrator işlevinin yeni bir örneğini başlatır. Bu sınıfın örnekleri edinilen kullanarak `orchestrationClient` bağlama. Bu yöntem enqueues işlevi belirtilen adda kullanır başlangıcı tetikler denetim kuyruğa bir ileti dahili olarak, `orchestrationTrigger` tetiklemek bağlama.

Parametreleri [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) aşağıdaki gibidir:

* **Ad**: zamanlamak için orchestrator işlevinin adı.
* **Giriş**: hangi orchestrator işlev giriş olarak iletilmesi gereken herhangi bir JSON seri hale getirilebilir veri.
* **InstanceId**: (isteğe bağlı) benzersiz kimliği örneği. Belirtilmezse, bir rastgele örnek kimliği oluşturulur.

Basit bir C# örnek aşağıda verilmiştir:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    TraceWriter log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.Info($"Started orchestration with ID = '{instanceId}'.");
}
```

.NET olmayan diller için işlevi çıktı bağlama yeni örnekleri başlatmak için kullanılabilir. Bu durumda, alanlar olarak yukarıdaki üç parametre olan herhangi bir JSON serileştirilebilir nesnenin kullanılabilir. Örneğin, aşağıdaki Node.js işlevi göz önünde bulundurun:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null);
};
```

> [!NOTE]
> Örnek kimliği için rastgele bir tanımlayıcı kullanmanızı öneririz Bu, orchestrator işlevleri arasında birden çok VM ölçeklendirme eşit yük dağıtımı sağlamaya yardımcı olur. Rastgele olmayan örneği kimlikleri kullanmak için uygun kimliği dış bir kaynaktan gelmelidir veya uygularken saattir [singleton orchestrator](durable-functions-singletons.md) düzeni.

## <a name="querying-instances"></a>Örnekleri sorgulama

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı orchestration örneği durumunu sorgular. Bu alan bir `instanceId` bir parametre olarak ve aşağıdaki özelliklere sahip bir nesne döndürür:

* **Ad**: orchestrator işlevin adı.
* **InstanceId**: orchestration örnek kimliği (aynı olmalıdır `instanceId` giriş).
* **CreatedTime**: orchestrator işlevi başladığı çalıştıran zaman.
* **LastUpdatedTime**: zaman orchestration son belirttiğinizde.
* **Giriş**: JSON değeri olarak işlevinin giriş.
* **Çıktı**: işlevi (işlev tamamladıysa) JSON değer olarak çıktı. Bu özellik, orchestrator işlevi başarısız oldu, hata ayrıntıları dahil edilir. Orchestrator işlevi sonlandırıldı, bu özellik sonlandırma sağlanan nedeni (varsa) içerir.
* **RuntimeStatus**: şu değerlerden biri:
    * **Çalışan**: örnek çalışmaya başladı.
    * **Tamamlanan**: örneği normal şekilde tamamlandı.
    * **ContinuedAsNew**: örnek kendisi ile yeni bir geçmiş yeniden başlatıldı. Bu geçici bir durumdur.
    * **Başarısız**: örnek bir hata ile başarısız oldu.
    * **Sonlandırılmış**: örnek beklenmedik şekilde sona erdirildi.
    
Bu yöntem `null` örneği mevcut değil veya çalışan henüz başlatılmadı.

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

> [!NOTE]
> Örnek sorgu şu anda yalnızca C# orchestrator işlevleri için desteklenir.

## <a name="terminating-instances"></a>Sonlandırma örnekleri

Çalışan bir örneği kullanarak sonlandırılabilir [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. İki parametreler bir `instanceId` ve `reason` günlükleri ve örnek durum yazılır dize. Sonlandırılmış bir örneği sonraki ulaştığında hemen sonra çalışmayı durdurur `await` zaten açık değilse, nokta veya hemen sonlandırılacak bir `await`.

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

> [!NOTE]
> Örnek sonlandırma şu anda yalnızca C# orchestrator işlevleri için desteklenir.

## <a name="sending-events-to-instances"></a>Olayları örneklerine gönderme

Olay bildirimleri kullanarak örnekleri çalıştırmak için gönderilebilir [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. Bu olaylar işleyebilir örnekleri olan bir çağrı bekleyen o [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Parametreleri [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) aşağıdaki gibidir:

* **InstanceId**: örneğinin benzersiz kimliği.
* **EventName**: gönderilecek olayın adı.
* **EventData**: örneğine göndermek için bir JSON seri hale getirilebilir yükü.

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

> [!NOTE]
> Olaylar oluşturma, şu anda yalnızca C# orchestrator işlevleri için desteklenir.

> [!WARNING]
> Belirtilen orchestration örneği yok ise *kimliği örneği* veya örnek belirtilen değil bekliyorsa *olay adı*, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz: [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [HTTP API'leri örneği için yönetim kullanmayı öğrenin](durable-functions-http-api.md)
