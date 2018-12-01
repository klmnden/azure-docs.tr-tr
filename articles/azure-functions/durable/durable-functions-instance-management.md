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
ms.date: 11/27/2018
ms.author: azfuncdf
ms.openlocfilehash: cf408cc125362d2ea05c606a0b2e148d60647281
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679588"
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
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
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

Yukarıdaki kod function.json dosyasında ada sahip bir çıkış bağlaması tanımladığınız varsayar `starter` olarak yazın `orchestrationClient`. Bağlama tanımlanmamışsa, dayanıklı işlevi örneği oluşturulmayacak.

Dayanıklı işlevi çağrılacak function.json aşağıda açıklandığı gibi düzenleme istemcisi için bir bağlama olması için değiştirilmesi gerekir

```js
{
    "bindings": [{
        "name":"starter",
        "type":"orchestrationClient",
        "direction":"out"
    }]
}
```

> [!NOTE]
> Rastgele bir tanımlayıcı örnek kimliği için kullanın. Bu, orchestrator işlevleri arasında birden çok VM'yi ölçeklendirme eşit yük dağıtım sağlamaya yardımcı olur. Rastgele olmayan örnek kimlikleri kullanmak için uygun kimliği bir dış kaynaktan gelmelidir veya uygularken zamandır [singleton orchestrator](durable-functions-singletons.md) deseni.

### <a name="using-core-tools"></a>Temel araçları ile

Doğrudan üzerinden bir örneğini başlatmak mümkündür [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable start-new` komutu. Bunu, aşağıdaki parametreleri alır:

* **`function-name` (gerekli)** : İşlevin adını başlatmak için
* **`input` (isteğe bağlı)** : Her iki satır içi işleve veya bir JSON dosyası aracılığıyla giriş. Dosyalar için dosya ile önekini `@`, gibi `@path/to/file.json`.
* **`id` (isteğe bağlı)** : Düzenleme örnek kimliği. Belirtilmezse, rastgele bir GUID oluşturulur.
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla. 

> [!NOTE]
> Bir işlev uygulaması, kök dizininden yürütülür çekirdek araçları komutlarını varsayalım. Varsa `connection-string-setting` ve `task-hub-name` komutların herhangi bir dizinden çalıştırabilirsiniz, açıkça sağlanır. Bu komutlar çalıştıran bir işlev uygulaması konağa yürütülüp, ancak konak çalışmadığı sürece bazı efektler gözlenecek değil. Örneğin, `start-new` komut hedef görev hub'ına Başlangıç iletisi kuyruğa olur, ancak iletiyi işleyebilen çalışan bir işlev uygulaması konak işlemi olmadıkça düzenleme gerçekte çalışmaz.

Aşağıdaki komutu HelloWorld adlı işlev başlatmak ve dosyanın içeriğini geçirin ' sayacı-data.json' ona:
```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="querying-instances"></a>Örnekleri sorgulama

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) metodunda [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı düzenleme örneği durumunu sorgular.

Bu alan bir `instanceId` (gerekli) `showHistory` (isteğe bağlı) `showHistoryOutput` (isteğe bağlı) ve `showInput` (isteğe bağlı) parametre olarak. 
* **`showHistory`**: Eğer kümesine `true`, yanıt yürütme geçmişini içerir. 
* **`showHistoryOutput`**: Eğer kümesine `true`, yürütme geçmişini etkinlik çıktılarını içerecek. 
* **`showInput`**: Eğer kümesine `false`, yanıt işlevin giriş içermez. Varsayılan değer `true` şeklindedir.

Yöntemi, aşağıdaki özelliklere sahip bir JSON nesnesi döndürür:

* **Ad**: orchestrator işlevin adı.
* **InstanceId**: örnek kimliği düzenleme (aynı olmalıdır `instanceId` giriş).
* **Oluşturulma zamanı**: zaman, çalışan başlatıldığında Düzenleyici işlevi.
* **LastUpdatedTime**: zaman düzenleme son denetim noktası oluşturuldu.
* **Giriş**: işlev bir JSON değeri olarak giriş. Bu alan, doldurulmaz `showInput` false'tur.
* **CustomStatus**: JSON biçimindeki özel düzenleme durumu. 
* **Çıkış**:, (işlev Tamamlandı durumunda) bir JSON değeri olarak işlevin çıktısı. Bu özellik orchestrator işlevi başarısız oldu, hata ayrıntıları içerir. Bu özellik orchestrator işlevi sonlandırıldı, belirtilen sonlandırma nedenini (varsa) içerecektir.
* **RuntimeStatus**: Aşağıdaki değerlerden biri:
    * **Bekleyen**: örneği zamanlandı ancak çalışan henüz başlatılmadı.
    * **Çalışan**: örnek çalışmaya başladı.
    * **Tamamlanan**: örnek normal olarak tamamlandı.
    * **ContinuedAsNew**: örnek kendisi ile yeni bir geçmiş başlatıldı. Bu geçici bir durumdur.
    * **Başarısız**: örnek bir hata ile başarısız oldu.
    * **Sonlandırılan**: örnek aniden durduruldu.
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

### <a name="using-core-tools"></a>Temel araçları ile

Orchestration örneği aracılığıyla doğrudan durumunu almak mümkündür [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable get-runtime-status` komutu. Bunu, aşağıdaki parametreleri alır: 

* **`id` (gerekli)** : Düzenleme örnek kimliği
* **`show-input` (isteğe bağlı)** : Varsa kümesine `true`, yanıt işlevin giriş içerir. Varsayılan değer `false` şeklindedir.
* **`show-output` (isteğe bağlı)** : Varsa kümesine `true`, yanıt, işlevin çıktısı içerir. Varsayılan değer `false` şeklindedir.
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

Aşağıdaki komutu (giriş ve çıkış dahil) örneğini düzenleme örnek kimliği ile 0ab8c55a66644d68a3a8b220b12d209c durumunu almak. Bunu varsayar `func` işlev uygulamasının kök dizininden komutu çalıştırılıyor:
```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

`durable get-history` Komutu, orchestration örneği geçmişini almak için kullanılabilir. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Düzenleme örnek kimliği
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu ayrıca host.json durableTask:HubName aracılığıyla ayarlanabilir.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="querying-all-instances"></a>Tüm örnekleri sorgulama

Kullanabileceğiniz `GetStatusAsync` tüm düzenleme örneklerinin durumları sorgulamak için yöntemi. Hiçbir parametre almaz veya geçirebilirsiniz bir `CancellationToken` iptal etmek istemeniz durumunda nesne. Yöntemi aynı özelliklere sahip nesneleri döndürür `GetStatusAsync` yöntemi dışında parametrelerle geçmişi döndürmez. 

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="using-core-tools"></a>Temel araçları ile

Ayrıca doğrudan sorgu örnekleri için olası aracılığıyla [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable get-instances` komutu. Bunu, aşağıdaki parametreleri alır:

* **`top` (isteğe bağlı)** : Bu komut, disk belleği destekler. Bu parametre, istek başına alınan örneklerinin karşılık gelir. Varsayılan değer 10'dur.
* **`continuation-token` (isteğe bağlı)** : Hangi sayfa/SECTION almak için örnekleri belirtmek için bir belirteç. Her `get-instances` yürütme sonraki örnekleri kümesi için bir belirteç döndürür.
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

```bash
func durable get-instances
```

## <a name="querying-instances-with-filters"></a>Filtrelerle örnekleri sorgulama

Ayrıca `GetStatusAsync` yöntemi bir kümesiyle eşleşen düzenleme örneklerinin bir listesini almak için önceden tanımlanmış filtreler. Olası filtreleme seçeneklerini düzenleme oluşturulma zamanını ve orchestration çalışma zamanı durumunu içerir.

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="using-the-functions-core-tools"></a>İşlevleri temel araçları ile

`durable get-instances` Komut filtreleri ile de kullanılabilir. Yukarıda sözü edilen yanı sıra `top`, `continuation-token`, `connection-string-setting`, ve `task-hub-name` parametreler, üç filtre parametrelerini (`created-after`, `created-before`, ve `runtime-status`), kullanılabilir. 

* **`created-after` (isteğe bağlı)** : Bu tarih/saat sonra (UTC) oluşturulan örneklerini alır. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`created-before` (isteğe bağlı)** : Bu tarih/saat önce (UTC) oluşturulan örneklerini alır. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`runtime-status` (isteğe bağlı)** : Durumu ('çalışıyor', 'Tamamlandı' vb..) bu eşleşen örneklerini alır. Birden çok (boşlukla ayrılmış) durumları sağlayabilir.
* **`top` (isteğe bağlı)** : Örnek sayısı, istek başına alınır. Varsayılan değer 10'dur.
* **`continuation-token` (isteğe bağlı)** : Hangi sayfa/SECTION almak için örnekleri belirtmek için bir belirteç. Her `get-instances` yürütme sonraki örnekleri kümesi için bir belirteç döndürür.
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

Filtre varsa (`created-after`, `created-before`, veya `runtime-status`), ardından sağlanan `top` örnekleri ile çalışma zamanı durumu ve oluşturma zamanı için hiçbir şekilde alınabilir.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
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

### <a name="using-core-tools"></a>Temel araçları ile

Orchestration örneği aracılığıyla doğrudan sonlandırmak mümkündür [temel araçları](../functions-run-local.md) `durable terminate` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Sonlandırmak için düzenleme örnek kimliği
* **`reason` (isteğe bağlı)** : Sonlandırma nedeni
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

Aşağıdaki komut, orchestration örneği kimliği 0ab8c55a66644d68a3a8b220b12d209c sonlanırdı:
```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="sending-events-to-instances"></a>Örneklerine olayları gönderme

Olay bildirimleri kullanarak örnekleri çalıştırmak için gönderilebilir [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı. Bu olayları işleyebilir örnekleri olan bir çağrı bekleyen o [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_). 

Parametreleri [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) aşağıdaki gibidir:

* **InstanceId**: örneğinin benzersiz kimliği.
* **EventName**: gönderilecek olayın adı.
* **EventData**: örneğine göndermek için bir JSON seri hale getirilebilir yükü.

```csharp
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

### <a name="using-core-tools"></a>Temel araçları ile

Orchestration örneğine aracılığıyla doğrudan bir olay oluşturabilmelidir mümkündür [temel araçları](../functions-run-local.md) `durable raise-event` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Düzenleme örnek kimliği
* **`event-name` (isteğe bağlı)** : Yükseltmek için olayın adı. Varsayılan değer `$"Event_{RandomGUID}"`
* **`event-data` (isteğe bağlı)** : Düzenleme örneğine gönderilecek veri. Bu bir JSON dosyası yolu olabilir veya verileri doğrudan komut satırında sağlanabilir
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```
```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Orchestration tamamlanmasını bekle

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı kullanıma sunan bir [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) zaman uyumlu bir düzenleme örneğinden gerçek çıkış almak için kullanılan API. 10 saniye boyunca varsayılan değerini kullanmaktadır `timeout` ve 1 saniyeye `retryInterval` olduğunda bunlar ayarlanmadı.  

Bu API kullanımı gösterilmiştir HTTP tetikleyici işlevi bir örnek aşağıda verilmiştir:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/HttpSyncStart.cs)]

İşlevi, 2 saniyelik zaman aşımı ve 0,5 saniyelik yeniden deneme aralığı kullanarak aşağıdaki satırla çağrılabilir:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gerekli süreye bağlı olarak, iki durum vardır:

* Tanımlanan zaman aşımı süresi içinde (Bu örnekte 2 saniye) orchestration örnekleri tamamlamak, yanıtı zaman uyumlu olarak sunulan gerçek düzenleme örnek çıktı:

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

* Tanımlanan zaman aşımı süresi içinde (Bu örnekte 2 saniye) orchestration örnekleri tamamlanamıyor, yanıt bir açıklanan varsayılan **HTTP API URL'si bulma**:

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
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
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
* **RewindPostUri**: düzenleme örneği "geri" URL'si.

Etkinlik işlevleri örneği gönderebilir [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) izlemek veya bir düzenleme için bir olay için harici sistemlere bağlanma:

```csharp
[FunctionName("SendInstanceInfo")]
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

## <a name="rewinding-instances-preview"></a>Geri sarma örnekleri (Önizleme)

Başarısız düzenleme örnek olabilir *rewound* kullanarak daha önce sağlıklı duruma [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) API. Yeniden düzenleme koyarak çalıştığını *çalıştıran* durumu ve orchestration hata nedeniyle etkinlik ve/veya suborchestration yürütme hataları yeniden çalıştırma.

> [!NOTE]
> Bu API yerine uygun hata işleme ve yeniden deneme ilkelerine yönelik değildir. Bunun yerine, yalnızca, burada düzenleme örnekleri beklenmeyen nedenlerle başarısız durumda kullanılmak üzere tasarlanmıştır. Hata işleme ve yeniden deneme ilkeleri hakkında daha fazla ayrıntı için lütfen bkz [hata işleme](durable-functions-error-handling.md) konu.

Örnek Kullanım örneği için *rewind* bir dizi içeren bir iş akışı [İnsan bir onayları](durable-functions-overview.md#pattern-5-human-interaction). Birisi onaylarını gereklidir bildirir etkinlik işlevler bir dizi vardır varsayalım ve gerçek zamanlı yanıt bekleyin. Tüm etkinlikleri yanıtları onayını aldığınız veya başarısız bir veritabanı bağlantı dizesi geçersiz gibi bir uygulama yanlış yapılandırılması nedeniyle başka bir etkinlik zaman aşımına uğradı. Sonucu bir düzenleme hatası olan iş akışı inin. İle `RewindAsync` API, bir uygulama Yöneticisi yapılandırma hatası düzeltebileceğinizi ve *rewind* başarısız düzenleme yedekleme durumunu hemen önce hata. İnsan etkileşimi adımlardan hiçbiri reapproved gerekir ve orchestration artık başarıyla tamamlayabilir.

> [!NOTE]
> *Rewind* özellik dayanıklı zamanlayıcılar kullanan geri sarma düzenleme örnekleri desteklemez.

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

### <a name="using-core-tools"></a>Temel araçları ile

Orchestration örneği aracılığıyla doğrudan sarılacak mümkündür [temel araçları](../functions-run-local.md) `durable rewind` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Düzenleme örnek kimliği
* **`reason` (isteğe bağlı)** : Düzenleme örneği geri sarma nedeni
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Örnek geçmişini temizle

Orchestration geçmişi kullanılarak temizlenebilir [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_). İşlevi, bir düzenleme ile - varsa bunlar büyük ileti blobları ve Azure tablo satırları ilişkili tüm verileri kaldırır. Yönteminin iki aşırı yüklemesi vardır. İlk düzenleme örneğinin Kimliğine göre geçmişini temizler:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

İkinci örnek sonra belirtilen zaman aralığında tamamlanan tüm düzenleme örnekleri için geçmişini temizler Zamanlayıcı ile tetiklenen bir işlev gösterir. Bu durumda, bu en az 30 gün önce tamamlanan tüm örnekleri için verileri kaldırır. 12: 00 günde bir kez çalıştırmak için zamanlandı:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync( 
                    DateTime.MinValue,
                    DateTime.UtcNow.AddDays(-30),  
                    new List<OrchestrationStatus> 
                    { 
                        OrchestrationStatus.Completed
                    }); 
}
```

> [!NOTE]
> *PurgeInstanceHistory* parametresi yalnızca bir çalışma zamanı durumu tamamlandı -, kesildi veya başarısız düzenleme örnekleri işleyecek şekilde aşırı yükleme süre kabul etme.

### <a name="using-core-tools"></a>Temel araçları ile

Bir düzenleme örneğin geçmişi temizlemeniz mümkündür kullanarak [temel araçları](../functions-run-local.md) `durable purge-history` komutu. Benzer şekilde, ikinci C# yukarıdaki örnekte, belirtilen zaman aralığı boyunca oluşturulan tüm düzenleme örnekleri için geçmişini temizler. Temizleneceği örnekleri daha fazla çalışma zamanı durumuna göre filtrelenebilir. Komut çeşitli parametrelere sahiptir:

* **`created-after` (isteğe bağlı)** : Bu tarih/saat sonra (UTC) oluşturulan örnekleri geçmişini temizle. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`created-before` (isteğe bağlı)** : Bu tarih/saat önce (UTC) oluşturulan örnekleri geçmişini temizle. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`runtime-status` (isteğe bağlı)** : Durumu eşleşen ('çalışıyor', 'Tamamlandı' vb..) Bu örnekleri geçmişini temizle. Birden çok (boşlukla ayrılmış) durumları sağlayabilir.
* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

Aşağıdaki komut geçmişi 7:35 PM (UTC) 14 Kasım 2018'den önce oluşturulan tüm başarısız örneklerinin siler.
```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="deleting-a-task-hub"></a>Bir görev hub'ı siliniyor
Kullanarak [temel araçları](../functions-run-local.md) `durable delete-task-hub` komutu, belirli bir görev hub ile ilişkili tüm depolama yapıları silmek olabilir. Bu, Azure depolama tabloları, kuyrukları ve blobları içerir. Komut iki parametreye sahiptir: 

* **`connection-string-setting` (isteğe bağlı)** : Kullanmak için depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanmak için dayanıklı görev hub'ının adı. DurableFunctionsHub varsayılandır. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json) durableTask:HubName aracılığıyla.

Aşağıdaki komutu 'UserTest' görev hub'ı ile ilişkili tüm Azure depolama verileri siler.
```bash
func durable delete-task-hub --task-hub-name UserTest
```


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneğin yönetim HTTP API'lerini kullanmayı öğrenin](durable-functions-http-api.md)
