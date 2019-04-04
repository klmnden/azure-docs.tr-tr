---
title: Dayanıklı işlevler desenleri ve Azure işlevleri'nde teknik kavramlar
description: Nasıl dayanıklı işlevler uzantısını Azure işlevleri'nde, durum bilgisi olan kod yürütme bulut avantajlarını sağladığını öğrenin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: azfuncdf
ms.openlocfilehash: e54fe17e80382348bcf463624043f7922a29d1c1
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58892764"
---
# <a name="durable-functions-patterns-and-technical-concepts-azure-functions"></a>Dayanıklı işlevler desenleri ve teknik kavramlar (Azure işlevleri)

Dayanıklı işlevler uzantısıdır [Azure işlevleri](../functions-overview.md) ve [Azure WebJobs](../../app-service/web-sites-create-web-jobs.md). Dayanıklı İşlevler, durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmak için kullanabilirsiniz. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. 

Bu makalede, ilgili davranışları dayanıklı işlevler uzantısını Azure işlevleri ve sık karşılaşılan uygulama desenleri için ayrıntılı bilgileri sağlar. Bilgilerin dayanıklı işlevler geliştirme zorluklarınızın çözmeye yardımcı olmak için nasıl kullanılacağını belirlemenize yardımcı olabilir.

> [!NOTE]
> Dayanıklı İşlevler, tüm uygulamalar için uygun olmayan bir Azure işlevleri için Gelişmiş bir uzantı. Bu makalede açıklanan kavramlar ile güçlü bir benzerlik sahip olduğunuzu varsayar [Azure işlevleri](../functions-overview.md) ve zorlukları sunucusuz uygulama geliştirme.

## <a name="patterns"></a>Desenler

Bu bölümde, bazı ortak uygulama desenleri burada dayanıklı işlevler yararlı olabilir açıklanmaktadır.

### <a name="chaining"></a>Desen #1: İşlev zinciri oluşturma

Desen zincirleme işlevinde işlevler bir dizi belirli bir sırayla yürütür. Bu düzende, başka bir işlev girişi, bir işlevin çıktısı uygulanır.

![Diyagram Düzeni zincirleme işlevi](./media/durable-functions-concepts/function-chaining.png)

Dayanıklı işlevler kısaca aşağıdaki örnekte gösterildiği gibi deseni zincirleme işlevi uygulamak için kullanabilirsiniz:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task<object> Run(DurableOrchestrationContext context)
{
    try
    {
        var x = await context.CallActivityAsync<object>("F1");
        var y = await context.CallActivityAsync<object>("F2", x);
        var z = await context.CallActivityAsync<object>("F3", y);
        return  await context.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // Error handling or compensation goes here.
    }
}
```

> [!NOTE]
> Önceden derlenmiş bir kalıcı işlevi yazma arasındaki farklar vardır C# ve önceden derlenmiş bir kalıcı işlevi yazma C# örnekte gösterilen betik. İçinde bir C# işlevi, önceden derlenmiş ilgili özniteliklerle sürekli parametreleri düzenlenmiş. Bir örnek `[OrchestrationTrigger]` özniteliğini `DurableOrchestrationContext` parametresi. İçinde bir C# dayanıklı işlevi parametreleri doğru düzenlenmiş değil, çalışma zamanı değişkenleri işleve eklenemiyor ve bir hata oluşursa önceden derlenmiş. Daha fazla örnek için bkz. [azure-functions-durable-extension samples github'da](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples).

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const x = yield context.df.callActivity("F1");
    const y = yield context.df.callActivity("F2", x);
    const z = yield context.df.callActivity("F3", y);
    return yield context.df.callActivity("F4", z);
});
```

Bu örnekte, değerleri `F1`, `F2`, `F3`, ve `F4` diğer işlevler işlev uygulamasına adlarıdır. Denetim akışı yapılarını kodlama normal kesinliği kullanarak uygulayabilirsiniz. Kodu En üstten aşağı yürütür. Mevcut dil denetim akışı semantiğini koşullular ve döngüler gibi kod içerebilir. Hata işleme mantığı ekleyebileceğiniz `try` / `catch` / `finally` engeller.

Kullanabileceğiniz `context` parametre [DurableOrchestrationContext] \(.NET\) ve `context.df` adına göre diğer işlevleri çağırmak, parametreler ve işlev dönüş nesnesi (JavaScript) çıktı. Her zaman kod çağrıları `await` (C#) veya `yield` (JavaScript) dayanıklı işlevler framework kontrol noktalarını ilerleme durumunu geçerli işlev örneği. Sanal makine ve işlem sürecin yarısında yürütme dönüştürülürse, işlev örneği önceki gelen sürdürür `await` veya `yield` çağırın. Daha fazla bilgi için sonraki bölümde, düzeni 2 bakın: Fan çıkış/giriş.

> [!NOTE]
> `context` JavaScript nesnesi temsil eden tüm [işlevi bağlam](../functions-reference-node.md#context-object), yalnızca [DurableOrchestrationContext] parametresi.

### <a name="fan-in-out"></a>Desen #2: Fan çıkış/fanı

Fan çıkış/desende fanı, paralel olarak birden çok işlevleri yürütmek ve tüm işlevler için bekleyin. Genellikle, bazı toplama iş işlevlerini döndürülen sonuçlar üzerinde gerçekleştirilir.

![Çıkış fan diyagramı/desen fanı](./media/durable-functions-concepts/fan-out-fan-in.png)

Normal işlevlerde olduğu, birden çok ileti bir kuyruğa gönderebilirsiniz işlevi sağlayarak fan. Geri fanning çok daha zor olur. ' De, normal bir işlevde aşamayı yaymak için kuyruk ile tetiklenen bir işlev uç ve ardından çıkış işlev ne zaman açtıklarını izlemek için kod yazın. 

Bu düzen görece basit kod ile dayanıklı işlevler uzantısını işler:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var parallelTasks = new List<Task<int>>();

    // Get a list of N work items to process in parallel.
    object[] workBatch = await context.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = context.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }

    await Task.WhenAll(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    int sum = parallelTasks.Sum(t => t.Result);
    await context.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const parallelTasks = [];

    // Get a list of N work items to process in parallel.
    const workBatch = yield context.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(context.df.callActivity("F2", workBatch[i]));
    }

    yield context.df.Task.all(parallelTasks);

    // Aggregate all N outputs and send the result to F3.
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield context.df.callActivity("F3", sum);
});
```

Yaygın iş için birden çok örneğini dağıtılır `F2` işlevi. İş görevlerinin dinamik listesi kullanılarak izlenir. .NET `Task.WhenAll` API veya JavaScript `context.df.Task.all` API çağrıldığında için tüm çağrılan işlevlerin tamamlanması beklenecek. Ardından, `F2` çıkışları dinamik görev listesinden toplanır ve geçirilen işlevi `F3` işlevi.

Sırasında gerçekleşen otomatik denetim noktası `await` veya `yield` çağırmak `Task.WhenAll` veya `context.df.Task.all` midway kilitlenebilir veya yeniden başlatma zaten tamamlanmış bir görevin yeniden başlatma gerektirmez, sağlar.

### <a name="async-http"></a>#3. Desen: Zaman uyumsuz HTTP API'leri

Zaman uyumsuz HTTP API'lerini deseni dış istemcilerle uzun süren işlemlerin durumunu koordine sorununu giderir. Bu desen uygulamak için bir ortak bir HTTP tetikleyici uzun süre çalışan eylem çağrısı sağlayarak yoludur. Ardından, istemci işlemi tamamlandığında öğrenmek için istemci yoklayan bir durum uç noktasına yönlendirir.

![HTTP API düzeni diyagramı](./media/durable-functions-concepts/async-http-api.png)

Dayanıklı İşlevler, uzun süre çalışan işlev yürütmelerini ile etkileşim kurmak için yazdığınız kodu kolaylaştıran yerleşik API'ler sağlar. Dayanıklı işlevler hızlı başlangıç örnekleri ([ C# ](durable-functions-create-first-csharp.md) ve [JavaScript](quickstart-js-vscode.md)) yeni orchestrator işlevi örneklerini başlatmak için kullanabileceğiniz basit bir REST komutu göster. Uzantı örneği başlatıldıktan sonra Web kancası orchestrator işlevi durumu HTTP API'lerini kullanıma sunar. 

Aşağıdaki örnek, bir orchestrator başlayıp durumunu sorgulamak REST komutları gösterir. Anlaşılsın diye, bazı ayrıntılar örnekten göz ardı edilir.

```
> curl -X POST https://myfunc.azurewebsites.net/orchestrators/DoWork -H "Content-Length: 0" -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"id":"b79baf67f717453ca9e86c5da21e03ec", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 202 Accepted
Content-Type: application/json
Location: https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec

{"runtimeStatus":"Running","lastUpdatedTime":"2017-03-16T21:20:47Z", ...}

> curl https://myfunc.azurewebsites.net/admin/extensions/DurableTaskExtension/b79baf67f717453ca9e86c5da21e03ec -i
HTTP/1.1 200 OK
Content-Length: 175
Content-Type: application/json

{"runtimeStatus":"Completed","lastUpdatedTime":"2017-03-16T21:20:57Z", ...}
```

Dayanıklı işlevler çalışma zamanı durumu yönettiğinden, kendi durumunu izleme mekanizması uygulamak gerek yoktur.

Dayanıklı işlevler uzantısını uzun süre çalışan düzenlemeleri yöneten yerleşik Web kancaları sahiptir. Kendi işlev tetikler (örneğin, HTTP, bir kuyruk veya Azure Event Hubs) kullanarak kendiniz Bu desen uygulayabilirsiniz ve `orchestrationClient` bağlama. Örneğin, bir kuyruk iletisi sonlandırma tetiklemek için kullanabilirsiniz. Ya da yerleşik Web kancaları kimlik doğrulaması için oluşturulmuş bir anahtar kullanmak yerine bir Azure Active Directory kimlik doğrulama İlkesi tarafından korunan bir HTTP tetikleyicisi kullanabilir.

HTTP API modelini nasıl bazı örnekleri aşağıda verilmiştir:

#### <a name="c"></a>C#

```csharp
// An HTTP-triggered function starts a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // The function name comes from the request URL.
    // The function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
// An HTTP-triggered function starts a new orchestrator function instance.
const df = require("durable-functions");

module.exports = async function (context, req) {
    const client = df.getClient(context);

    // The function name comes from the request URL.
    // The function input comes from the request content.
    const eventData = req.body;
    const instanceId = await client.startNew(req.params.functionName, undefined, eventData);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    return client.createCheckStatusResponse(req, instanceId);
};
```

> [!WARNING]
> Geliştirirken yerel olarak JavaScript'te yöntemlerini kullanmayı `DurableOrchestrationClient`, ortam değişkenini ayarlamalıdır `WEBSITE_HOSTNAME` için `localhost:<port>` (örneğin, `localhost:7071`). Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu 28](https://github.com/Azure/azure-functions-durable-js/issues/28).

. NET'te, [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parametredir arasında bir değer `orchestrationClient` dayanıklı işlevler uzantısını parçası olan bağlama, çıktı. JavaScript'te çağırarak bu nesne döndürülür `df.getClient(context)`. Bu nesneler için yeni veya var olan orchestrator işlevi örnekleri başlatmak, olayları göndermek, sonlandırma ve sorgulamak için kullanabileceğiniz yöntemler sağlar.

Yukarıdaki örneklerde, HTTP ile tetiklenen bir işlev alır bir `functionName` gelen URL değeri ve değerine geçirir [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). [CreateCheckStatusResponse](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateCheckStatusResponse_System_Net_Http_HttpRequestMessage_System_String_) API ardından bağlama içeren bir yanıt döndürür bir `Location` üst bilgi ve örnek hakkında ek bilgiler. Kullanmaya başlama örneği durumu arayın veya örneği sonlandırmak için bilgiler daha sonra kullanabilirsiniz.

### <a name="monitoring"></a>Desen #4: İzleme

Bir iş akışı'nın esnek, yinelenen bir işlemde İzleyici desenini gösterir. Belirli bir koşul karşılanana kadar örnek sorgulanır. Normal kullanabileceğiniz [Zamanlayıcı tetikleyicisi](../functions-bindings-timer.md) temel adres için senaryo, Dönemsel temizleme işi, ancak kendi aralığı gibi statiktir ve örnek yaşam süreleri yönetme karmaşık olur. Dayanıklı İşlevler, esnek yineleme aralıkları oluşturun, görev ömürleri yönetmek ve tek bir düzenleme işlemler birden çok izleyici oluşturmak için kullanabilirsiniz.

İzleyici düzeni önceki bir zaman uyumsuz HTTP API senaryosu tersine çevirmek için örneğidir. Yerine bir dış istemcinin uzun süreli bir işlemi izlemek bir uç nokta gösterme, uzun süre çalışan İzleyici dış uç noktası kullanır ve bir durum değişikliği için bekler.

![İzleyici düzeni diyagramı](./media/durable-functions-concepts/monitor.png)

Birkaç kod satırıyla, dayanıklı işlevler rastgele uç noktaları inceleyin birden çok izleyici oluşturmak için kullanabilirsiniz. Bir koşul karşılandığında, İzleyici yürütme sona erdirebilirsiniz veya [DurableOrchestrationClient](durable-functions-instance-management.md) izleyiciler sonlandırabilirsiniz. Bir izleyicinin değiştirebilirsiniz `wait` zaman aralığı dayalı belirli bir koşula göre (örneğin, üstel geri alma.) 

Aşağıdaki kod, temel bir izleyici uygular:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    int jobId = context.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();

    while (context.CurrentUtcDateTime < expiryTime)
    {
        var jobStatus = await context.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform an action when a condition is met.
            await context.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        var nextCheck = context.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await context.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform more work here, or let the orchestration end.
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const jobId = context.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(context.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield context.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform an action when a condition is met.
            yield context.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration sleeps until this time.
        const nextCheck = moment.utc(context.df.currentUtcDateTime).add(pollingInterval, 's');
        yield context.df.createTimer(nextCheck.toDate());
    }

    // Perform more work here, or let the orchestration end.
});
```

Bir istek alındığında, iş kimliği için yeni bir düzenleme örneği oluşturulur Örnek, bir koşul karşılandığında ve döngü çıkıldı kadar durumu yoklar. Dayanıklı bir zamanlayıcı yoklama aralığını denetler. Ardından, daha fazla iş gerçekleştirilebilir veya orchestration sonlandırabilirsiniz. Zaman `context.CurrentUtcDateTime` (.NET) veya `context.df.currentUtcDateTime` (JavaScript) aşıyor `expiryTime` değeri, İzleyici sona erer.

### <a name="human"></a>Desen #5: İnsan etkileşimi

Birçok otomatik işlemler insan etkileşimi tür içerir. Kişiler olarak yüksek oranda kullanılabilir ve bulut Hizmetleri olarak duyarlı olmayan otomatik bir işlem okunacağından içeren zor olmasıdır. Otomatik bir işlem, bunun için zaman aşımları ve telafi mantığını kullanarak sağlayabilir.

Bir onay işlemi, insan etkileşimi içeren bir iş sürecini örneğidir. Bir yönetici onayını belirli bir dolar tutarını aşan bir harcama rapor için gerekli olabilir. Yöneticisi, harcama raporlarını (belki de manager tatile oluştu) 72 saat içinde onaylamazsa, yükseltme işlemi onayı birisinden (belki de yöneticinin yöneticisinin) almak için devreye girer.

![İnsan etkileşimi düzeni diyagramı](./media/durable-functions-concepts/approval.png)

Bu örnekte, bir düzenleyici işlevi kullanarak desen uygulayabilirsiniz. Orchestrator'ı kullanan bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) onayı. Zaman aşımı oluşması durumunda orchestrator iletir. Orchestrator bekler bir [dış olay](durable-functions-external-events.md), bir insan etkileşimi tarafından oluşturulan bir bildirim gibi.

Bu örnekler, insan etkileşimi deseni göstermek için bir onay işlemi oluşturur:

#### <a name="c-script"></a>C# betiği

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    await context.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = context.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = context.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = context.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await context.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await context.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("RequestApproval");

    const dueTime = moment.utc(context.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = context.df.createTimer(dueTime.toDate());

    const approvalEvent = context.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield context.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield context.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield context.df.callActivity("Escalate");
    }
});
```

Dayanıklı Zamanlayıcı oluşturmak için arama `context.CreateTimer` (.NET) veya `context.df.createTimer` (JavaScript). Bildirim tarafından alınan `context.WaitForExternalEvent` (.NET) veya `context.df.waitForExternalEvent` (JavaScript). Ardından, `Task.WhenAny` (.NET) veya `context.df.Task.any` (JavaScript) İlerlet karar vermek için çağırılır (ilk zaman aşımı gerçekleşir) veya (zaman aşımından önce onay alınan) onay işlemi.

Dış istemciden olay bildirimi için bekleyen bir düzenleyici işlevi kullanarak sunabilir [yerleşik HTTP API'lerini](durable-functions-http-api.md#raise-event) kullanarak veya [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API'SİNDEN başka bir işlev:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    const isApproved = true;
    await client.raiseEvent(instanceId, "ApprovalEvent", isApproved);
};
```

## <a name="the-technology"></a>Teknoloji

Arka planda üst kısmındaki dayanıklı işlevler uzantısını oluşturulmuştur [dayanıklı görev Framework](https://github.com/Azure/durabletask), bir açık kaynak kitaplığı github'da dayanıklı görev düzenlemeleri oluşturmak için kullanılır. Azure işlevleri, sunucusuz Azure WebJobs gelişimi, dayanıklı işlevler dayanıklı görev Framework sunucusuz gelişimi gibidir. Microsoft ve diğer kuruluşlardan dayanıklı görev Framework kritik süreçlerini otomatikleştirmek için kapsamlı bir şekilde kullanın. Bu sunucusuz Azure işlevleri ortam için uygun bir kullanımdır olur.

### <a name="event-sourcing-checkpointing-and-replay"></a>Olay kaynağını belirleme, denetim noktası ve yeniden yürütme

Orchestrator işlevleri güvenilir bir şekilde kullanarak yürütme durumlarını korumak [olay kaynağını belirleme](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing) tasarım deseni. Dayanıklı işlevler uzantısını doğrudan bir düzenleme geçerli durumunu depolamak yerine işlevi düzenlemesi gereken eylemler tam serisini kaydetmek için bir yalnızca ekleme deposu kullanır. Bir yalnızca ekleme deposu "tam çalışma zamanı durumunu dökme" karşılaştırıldığında birçok avantaj vardır. Yüksek performans, ölçeklenebilirlik ve yanıt hızını avantajına sahip olur. İşlem verilerini ve tam denetim kayıtlarını ve geçmişini de nihai tutarlılık alın. Denetim kayıtlarını güvenilir telafi eylemlerine destekler.

Dayanıklı İşlevler, olay kaynağını şeffaf bir şekilde belirleme kullanır. Planda, `await` (C#) veya `yield` bir düzenleyici işlevi (JavaScript) işlecinde dayanıklı görev Framework dağıtıcısıyla orchestrator iş parçacığının denetim verir. Dağıtıcı, orchestrator işlevi (bir veya daha fazla alt işlevlerini çağırma veya kalıcı bir zamanlayıcı zamanlama) planlanan yeni eylemler ardından depolama alanına kaydeder. Saydam tamamlama eylemi, orchestration örneği yürütme geçmişine ekler. Geçmiş depolama tablosunda depolanır. İşleme işlem iletileri asıl işi zamanlamak için bir kuyruk ekler. Bu noktada, orchestrator işlevi bellekten olabilir. 

Azure işlevleri tüketim planı kullanıyorsanız, orchestrator işlevi faturalandırması durdurur. Ne zaman, işlevi yeniden yapmak için daha fazla iş yoktur ve durumunu yeniden düzenlenir.

Ne zaman bir düzenleme işlevi verildiğinde yapmak için daha fazla iş (örneğin, bir yanıt iletisi alındı veya sağlam bir süreölçerin süresi), orchestrator uyanır ve yeniden başından itibaren yerel durumu yeniden oluşturmak için tüm işlevi yürütür. 

Bir işlevi çağırmak kod çalışırsa, yeniden yürütme sırasında (veya diğer zaman uyumsuz iş), geçerli düzenleme yürütme geçmişini dayanıklı görev Framework danışır. Bu, bulunursa [etkinlik işlevi](durable-functions-types-features-overview.md#activity-functions) zaten yürütüldü ve bir sonuç vermedi, bu işlevin sonucu başlayarak yeniden oynatılır ve orchestrator kod çalışmaya devam eder. Yeniden yürütme, işlev kodunu işlemi tamamlanana kadar veya yeni zaman uyumsuz çalışma planladı kadar devam eder.

### <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Orchestrator kodunun yeniden yürütme davranışını kısıtlamaları bir düzenleyici işlevi içinde yazdığınız kodun türünü oluşturur. Örneğin, orchestrator kodunun birden çok kez yürütülecektir ve her zaman aynı sonucu üretmelidir belirleyici olmalıdır. Kısıtlamaları tam listesi için bkz. [Orchestrator kod kısıtlamaları](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints).

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

Dayanıklı işlevler uzantısını yapılandırılmış izleme verileri otomatik olarak yayar [Application Insights](../functions-monitoring.md) , işlev uygulamanızı Azure Application Insights izleme anahtarı ile ayarlayın. İzleme verileri, eylemleri ve, düzenlemeleri ilerlemesini izlemek için kullanabilirsiniz.

İşte bir örnek kullandığınızda, olayları izleme dayanıklı işlevler Application Insights portalında nasıl göründüğünü [Application Insights Analytics](../../application-insights/app-insights-analytics.md):

![Application Insights sorgu sonuçları](./media/durable-functions-concepts/app-insights-1.png)

Yapılandırılmış verileri yararlı bulabilirsiniz `customDimensions` alanındaki her günlük girişi. Tam olarak genişletilmiş bir girdinin bir örnek aşağıda verilmiştir:

![Bir Application Insights sorgu customDimensions alanı](./media/durable-functions-concepts/app-insights-2.png)

Dayanıklı görev Framework dağıtıcı yeniden yürütme davranışı nedeniyle yeniden yürütülmüş eylemler için yedekli günlük girişlerini görmek bekleyebilirsiniz. Yedekli günlük girişlerini temel altyapısını yeniden yürütme davranışını anlamanıza yardımcı olabilir. [Tanılama](durable-functions-diagnostics.md) makale yalnızca "gerçek zamanlı" günlükleri görebilirsiniz, böylece yeniden yürütme günlükleri filtrelemek için örnek sorgular.

## <a name="storage-and-scalability"></a>Depolama ve ölçeklenebilirlik

Dayanıklı işlevler uzantısını yürütme geçmişi durumu ve tetikleyici işlevi yürütme kalıcı hale getirmek için Azure depolama blobları, tablolar ve Kuyruklar kullanır. İşlev uygulamasının varsayılan depolama hesabı kullanabilir veya ayrı bir depolama hesabı yapılandırabilirsiniz. Depolama aktarım hızı sınırlara göre ayrı bir hesap isteyebilirsiniz. Orchestrator kodları bu depolama hesaplarında varlıklarla etkileşim kurmaz. Dayanıklı görev Framework varlıkları doğrudan uygulama ayrıntısı yönetir.

Orchestrator işlevleri, etkinlik işlevlerini zamanlayabilir ve yanıtlarını iç iletileri aracılığıyla alırsınız. Bir işlev uygulaması Azure işlevleri tüketim planında çalıştığında [Azure işlevleri ölçek denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) bu kuyruklar izler. Yeni bilgi işlem örnekleri, gerektikçe eklenir. Ölçeği birden çok VM için bir düzenleyici işlevi orchestrator işlev çağrıları birkaç farklı Vm'lere çalışabilecek etkinlik işlevlerini sırasında bir VM üzerinde çalışabilir. Dayanıklı işlevler ölçek davranış hakkında daha fazla bilgi için bkz. [performansı ve ölçeği](durable-functions-perf-and-scale.md).

Orchestrator hesapları için yürütme geçmişini tablo Depolama'da saklanır. Belirli bir VM örneği rehydrates olduğunda, orchestrator yerel durumunu yeniden oluşturmak için tablo Depolama'yı yürütme geçmişini getirir. Geçmiş tablo depolamada kullanılabilir olması için kullanışlı bir açısını araçlarını gibi kullanabileceğiniz olan [Azure Depolama Gezgini](../../vs-azure-tools-storage-manage-with-storage-explorer.md) , düzenlemeleri geçmişini görmek için.

Depolama BLOB'ları orchestration örneklerinin genişleme birden çok VM arasında koordine etmek için bir kiralama mekanizmasının öncelikli olarak kullanılır. Depolama BLOB'ları, doğrudan tabloları veya Kuyrukları depolanamaz büyük iletiler için verileri tutar.

![Bir Azure Depolama Gezgini ekran görüntüsü](./media/durable-functions-concepts/storage-explorer.png)

> [!WARNING]
> Tablo depolama yürütme geçmişini görmek kolay olsa da bu tabloda herhangi bir bağımlılığın yapmayın. Dayanıklı işlevler uzantısını geliştikçe tablo değişebilir.

## <a name="known-issues"></a>Bilinen sorunlar

Tüm bilinen sorunları izlenmesi gereken [GitHub sorunları](https://github.com/Azure/azure-functions-durable-extension/issues) listesi. Bir sorunla karşılaşırsanız ve sorunu Github'da bulunamıyor, yeni bir sorun açın. Sorunun ayrıntılı bir açıklama ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

Dayanıklı işlevler hakkında daha fazla bilgi için bkz: [dayanıklı işlevler işlev türleri ve özellikleri](durable-functions-types-features-overview.md). 

Kullanmaya başlamak için:

> [!div class="nextstepaction"]
> [Dayanıklı ilk işlevinizi oluşturma](durable-functions-create-first-csharp.md)

[DurableOrchestrationContext]: https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html
