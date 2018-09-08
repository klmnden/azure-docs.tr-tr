---
title: Dayanıklı işlevler - Azure örneklerini yönetme
description: Azure işlevleri için dayanıklı işlevler uzantısını durumlarda yönetmeyi öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: 72ea5e54bf86ce408700c0456f6d37f5f3c29924
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2018
ms.locfileid: "44091791"
---
# <a name="manage-instances-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) örneklerini yönetme

[Dayanıklı işlevler](durable-functions-overview.md) düzenleme örnekleri başlatıldı, sona erdi, sorgulanabilen ve gönderilen bildirim olayları. Tüm örnek Yönetimi yapılır kullanarak [düzenleme istemcisi bağlama](durable-functions-bindings.md). Bu makalede, her örnek yönetim işleminin ayrıntılarına gider.

## <a name="starting-instances"></a>Örnekleri başlatılıyor

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) metodunda [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) bir düzenleyici işlevi yeni bir örneğini başlatır. Bu sınıfın örnekleri satın alınabilir kullanarak `orchestrationClient` bağlama. Bu yöntem kaybolmamasının ardından başlangıcını kullanan belirtilen ada sahip bir işlev tetikler denetim kuyruğuna bir ileti dahili olarak `orchestrationTrigger` bağlama tetikleyin. 

Düzenleme işlemi başlatıldığında görev tamamlanır. Düzenleme işlemi, 30 saniye içinde başlamanız gerekir. Daha uzun sürerse bir `TimeoutException` oluşturulur. 

Parametreleri [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) aşağıdaki gibidir:

* **Ad**: zamanlamak için orchestrator işlevin adı.
* **Giriş**: orchestrator işleve giriş olarak geçirilmelidir herhangi bir JSON seri hale getirilebilir veri.
* **InstanceId**: örneğinin (isteğe bağlı) benzersiz kimliği. Bir rastgele örnek kimliği belirtilmezse oluşturulur.

Basit bir C# örneği aşağıda verilmiştir:

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

.NET diller için işlev çıktı bağlamasını yeni örnekleri başlatmak için kullanılabilir. Bu durumda, yukarıdaki üç parametre alan sahip herhangi bir JSON seri hale getirilebilir nesne kullanılabilir. Örneğin, aşağıdaki JavaScript işlevi göz önünde bulundurun:

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
> Rastgele bir tanımlayıcı örnek kimliği için kullanmanızı öneririz Bu, orchestrator işlevleri arasında birden çok VM'yi ölçeklendirme eşit yük dağıtım sağlamaya yardımcı olur. Rastgele olmayan örnek kimlikleri kullanmak için uygun kimliği bir dış kaynaktan gelmelidir veya uygularken zamandır [singleton orchestrator](durable-functions-singletons.md) deseni.

## <a name="querying-instances"></a>Örnekleri sorgulama

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) metodunda [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı düzenleme örneği durumunu sorgular. Bu alan bir `instanceId` (gerekli) `showHistory` (isteğe bağlı) ve `showHistoryOutput` (isteğe bağlı) parametre olarak. Varsa `showHistory` ayarlanır `true`, yanıt yürütme geçmişini içerir. Varsa `showHistoryOutput` ayarlanır `true` etkinlik çıktıları yürütme geçmişini de içerecektir. Yöntemi, aşağıdaki özelliklere sahip bir nesne döndürür:

* **Ad**: orchestrator işlevin adı.
* **InstanceId**: örnek kimliği düzenleme (aynı olmalıdır `instanceId` giriş).
* **Oluşturulma zamanı**: zaman, çalışan başlatıldığında Düzenleyici işlevi.
* **LastUpdatedTime**: zaman düzenleme son denetim noktası oluşturuldu.
* **Giriş**: işlev bir JSON değeri olarak giriş.
* **CustomStatus**: JSON biçimindeki özel düzenleme durumu. 
* **Çıkış**:, (işlev Tamamlandı durumunda) bir JSON değeri olarak işlevin çıktısı. Bu özellik orchestrator işlevi başarısız oldu, hata ayrıntıları içerir. Bu özellik orchestrator işlevi sonlandırıldı, belirtilen sonlandırma nedenini (varsa) içerecektir.
* **RuntimeStatus**: Aşağıdaki değerlerden biri:
    * **Bekleyen**: örneği zamanlandı ancak çalışan henüz başlatılmadı.
    * **Çalışan**: örnek çalışmaya başladı.
    * **Tamamlanan**: örnek normal olarak tamamlandı.
    * **ContinuedAsNew**: örnek kendisi ile yeni bir geçmiş başlatıldı. Bu geçici bir durumdur.
    * **Başarısız**: örnek bir hata ile başarısız oldu.
    * **Sonlandırılan**: örneği beklenmedik şekilde sonlandırıldı.
* **Geçmiş**: yürütme geçmişini düzenleme. Bu alan yalnızca, doldurulur `showHistory` ayarlanır `true`.
    
Bu yöntem döndürür `null` örneği mevcut değil veya çalışan henüz başlatılmadı.

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
## <a name="querying-all-instances"></a>Tüm örnekleri sorgulama

Kullanabileceğiniz `GetStatusAsync` tüm düzenleme örneklerinin durumları sorgulamak için yöntemi. Hiçbir parametre almaz veya geçirebilirsiniz bir `CancellationToken` iptal etmek istemeniz durumunda nesne. Yöntemi aynı özelliklere sahip nesneleri döndürür `GetStatusAsync` yöntemi dışında parametrelerle geçmişi döndürmez. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    TraceWriter log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.Info(JsonConvert.SerializeObject(instance));
    };
}
```

## <a name="terminating-instances"></a>Sondaki örnekleri

Çalışan bir düzenleme örneği kullanarak sonlandırılabilir [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. İki parametreler bir `instanceId` ve `reason` günlüklerine ve örnek durum yazılacak dize. Sonraki ulaştığında hemen sonra çalışan sonlandırılan bir örneği durdurur `await` zaten açık değilse, nokta veya hemen sonlandırılacak bir `await`. 

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
> Örnek sonlandırma şu anda dağıtılmaz. Etkinlik işlevleri ve alt düzenlemeleri tamamlama olup bunları adlı düzenleme örneği sonlandırıldı bağımsız olarak çalışır.

## <a name="sending-events-to-instances"></a>Örneklerine olayları gönderme

Olay bildirimleri kullanarak örnekleri çalıştırmak için gönderilebilir [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. Bu olayları işleyebilir örnekleri olan bir çağrı bekleyen o [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

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

> [!WARNING]
> Belirtilen orchestration örneği yok ise *kimliği örnek* veya örneği belirtilen beklemiyorsa *olay adı*, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

## <a name="wait-for-orchestration-completion"></a>Orchestration tamamlanmasını bekle

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı kullanıma sunan bir [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) zaman uyumlu bir düzenleme örneğinden gerçek çıkış almak için kullanılan API. 10 saniye boyunca varsayılan değerini kullanmaktadır `timeout` ve 1 saniyeye `retryInterval` olduğunda bunlar ayarlanmadı.  

Bu API kullanımı gösterilmiştir HTTP tetikleyici işlevi bir örnek aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

İşlevi, 2 saniyelik zaman aşımı ve 0,5 saniyelik yeniden deneme aralığı kullanarak aşağıdaki satırla çağrılabilir:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gereken süreyi bağlı olarak iki durum vardır:

1. Tanımlanan zaman aşımı süresi içinde (Bu örnekte 2 saniye) orchestration örnekleri tamamlamak, yanıtı zaman uyumlu olarak sunulan gerçek düzenleme örnek çıktı:

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

2. Tanımlanan zaman aşımı süresi içinde (Bu örnekte 2 saniye) orchestration örnekleri tamamlanamıyor, yanıt bir açıklanan varsayılan **HTTP API URL'si bulma**:

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
> Web kancası URL'leri biçimi, Azure işlevleri ana bilgisayarın hangi sürümünün kullanmakta olduğunuz bağlı olarak farklı olabilir. Önceki örnek için Azure işlevleri 2.0 ana bilgisayardır.

## <a name="retrieving-http-management-webhook-urls"></a>HTTP Yönetim Web kancası URL'lerini alma

Dış sistemler açıklanan varsayılan yanıtın bir parçası olan Web kancası URL'ler aracılığıyla dayanıklı işlevler ile iletişim kurabilir [HTTP API URL'si bulma](durable-functions-http-api.md). Ancak, Web kancası URL'leri da programlı olarak düzenleme istemcisi ya da bir etkinlik işlevi erişilebilir [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html)sınıfı. 

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) bir parametreye sahiptir:

* **InstanceId**: örneğinin benzersiz kimliği.

Yöntem bir örneğini döndürür [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) aşağıdaki dize özelliklere sahip:

* **Kimliği**: örnek kimliği düzenleme (aynı olmalıdır `InstanceId` giriş).
* **StatusQueryGetUri**: düzenleme örneğinin durumu URL'si.
* **SendEventPostUri**: düzenleme örneğinin "raise olay" URL'si.
* **TerminatePostUri**: düzenleme örneği "sonlandırma" URL'si.

Etkinlik işlevleri örneği gönderebilir [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) izlemek veya bir düzenleme için bir olay için harici sistemlere bağlanma:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneğin yönetim HTTP API'lerini kullanmayı öğrenin](durable-functions-http-api.md)
