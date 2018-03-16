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
ms.openlocfilehash: 9cea9b18cd7434a34138d5cecad8a8fd7f10d2e5
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) durumlarda yönetme

[Dayanıklı işlevleri](durable-functions-overview.md) orchestration örnekleri başlatılabilir, sonlandırıldı, sorgulanan ve bildirim olayları gönderilir. Tüm örnek Yönetimi yapılır kullanarak [bağlama orchestration istemci](durable-functions-bindings.md). Bu makalede her örnek yönetim işlemi ayrıntılara gider.

## <a name="starting-instances"></a>Başlangıç örnekleri

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) bir orchestrator işlevinin yeni bir örneğini başlatır. Bu sınıfın örnekleri edinilen kullanarak `orchestrationClient` bağlama. Bu yöntem enqueues işlevi belirtilen adda kullanır başlangıcı tetikler denetim kuyruğa bir ileti dahili olarak, `orchestrationTrigger` tetiklemek bağlama. 

Düzenleme işlemi başlatıldığında görev tamamlar. Düzenleme işlemi 30 saniye içinde başlamanız gerekir. Daha uzun sürerse bir `TimeoutException` oluşturulur. 

Parametreleri [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) aşağıdaki gibidir:

* **Ad**: zamanlamak için orchestrator işlevinin adı.
* **Giriş**: orchestrator işlev giriş olarak iletilmesi gereken herhangi bir JSON seri hale getirilebilir veri.
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

.NET olmayan diller için işlevi çıktı bağlama yeni örnekleri başlatmak için kullanılabilir. Bu durumda, yukarıdaki üç parametre alanları olan herhangi bir JSON serileştirilebilir nesnenin kullanılabilir. Örneğin, aşağıdaki Node.js işlevi göz önünde bulundurun:

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

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı orchestration örneği durumunu sorgular. Bu alan bir `instanceId` (gerekli) `showHistory` (isteğe bağlı) ve `showHistoryOutput` (isteğe bağlı) parametre olarak. Varsa `showHistory` ayarlanır `true`, yanıt yürütme geçmişini içerir. Varsa `showHistoryOutput` ayarlanır `true` yürütme geçmişini etkinlik çıkışları de içerir. Yöntemi, aşağıdaki özelliklere sahip bir nesne döndürür:

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
* **Geçmiş**: orchestration yürütme geçmişini. Bu alan yalnızca, doldurulur `showHistory` ayarlanır `true`.
    
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

## <a name="wait-for-orchestration-completion"></a>Orchestration tamamlanmasını bekleyin

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıf düzenlemenizi sağlayan bir [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) fiili çıktıyı bir orchestration örnekten zaman uyumlu olarak almak için kullanılan bir API. Varsayılan değer 10 saniye olan yöntemini kullanır `timeout` ve 1 saniye için `retryInterval` zaman bunlar ayarlı değil.  

Bu API kullanımı gösterilmiştir HTTP tetikleyicisi işlevi örnek aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

İşlev 2 saniyelik zaman aşımı ve 0,5 ikinci yeniden deneme aralığı kullanarak aşağıdaki satırla çağrılabilir:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gereken zamana bağlı olarak iki durum vardır:

1. Tanımlanan zaman aşımı süresi içinde (Bu durumda 2 saniye) orchestration örnekleri tamamlamak, yanıtı zaman uyumlu olarak teslim gerçek orchestration örnek çıktı:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

2. Orchestration örnekleri tanımlı zaman aşımı süresi içinde (Bu durumda 2 saniye) tamamlayamıyor, yanıt bir açıklanan varsayılan **HTTP API URL bulma**:

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Web kancası URL'leri biçimi çalıştırdığınız Azure işlevleri ana bilgisayarın hangi sürümünün bağlı olarak değişebilir. Önceki örnekte için Azure işlevleri 2.0 ana bilgisayardır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [HTTP API'leri örneği için yönetim kullanmayı öğrenin](durable-functions-http-api.md)
