---
title: Dayanıklı işlevler genel bakış - Azure
description: Azure işlevleri için dayanıklı işlevler uzantısını giriş.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 2f1b1568b75edba800cdac0fd5970ddf90126d9a
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2018
ms.locfileid: "50087591"
---
# <a name="durable-functions-overview"></a>Dayanıklı işlevler genel bakış

*Dayanıklı işlevler* uzantısıdır [Azure işlevleri](functions-overview.md) ve [Azure WebJobs](../app-service/web-sites-create-web-jobs.md) durum bilgisi olan işlevleri, sunucusuz bir ortamda yazmanızı sağlayan. Uzantı durumu ve kontrol noktaları yeniden sizin yerinize yönetir.

Uzantı çağrılan işlev yeni bir tür durum bilgisi olan iş akışları tanımlamanızı sağlar bir [ *orchestrator işlevi*](durable-functions-types-features-overview.md#orchestrator-functions). Orchestrator işlevleri avantajlarından bazıları şunlardır:

* Kod içinde iş akışları tanımlarlar. Herhangi bir JSON şemalarının veya tasarımcıları gereklidir.
* Zaman uyumlu ve zaman uyumsuz olarak bunlar diğer işlevler çağırabilir. Çağrılan işlevler çıkışını yerel değişkenlere kaydedilebilir.
* Bunlar otomatik olarak denetim noktası ilerleme durumunu her işlev bekler. Yerel durumu, hiçbir zaman işlemi geri dönüştüren veya VM yeniden kaybolur.

> [!NOTE]
> Dayanıklı İşlevler, Gelişmiş bir uzantı için Azure işlevleri, tüm uygulamalar için uygun değil. Bu makalenin geri kalanında, güçlü bir konusunda sahibi olduğunuzu varsayar [Azure işlevleri](functions-overview.md) kavramları ve zorluklarını sunucusuz uygulama geliştirme sürecine dahil.

Dayanıklı işlevler için birincil kullanım durumu, sunucusuz uygulamalar karmaşık, durum bilgisi olan koordinasyon sorunlarını basitleştirme olduğu. Aşağıdaki bölümlerde, dayanıklı işlevlerden yararlanabilirsiniz bazı tipik uygulama desenleri açıklanmaktadır.

## <a name="pattern-1-function-chaining"></a>Desen #1: zincirleme işlevi

*İşlev zincirleme* işlevler bir dizi belirli bir sırayla yürütülmesi deseni ifade eder. Genellikle, bir işlevin çıktısı başka bir işlev girişi uygulanması gerekir.

![Zincirleme diyagram işlevi](media/durable-functions-overview/function-chaining.png)

Dayanıklı İşlevler, bu düzen kısaca koda uygulanması olanak verir.

#### <a name="c-script"></a>C# betiği

```cs
public static async Task<object> Run(DurableOrchestrationContext ctx)
{
    try
    {
        var x = await ctx.CallActivityAsync<object>("F1");
        var y = await ctx.CallActivityAsync<object>("F2", x);
        var z = await ctx.CallActivityAsync<object>("F3", y);
        return  await ctx.CallActivityAsync<object>("F4", z);
    }
    catch (Exception)
    {
        // error handling/compensation goes here
    }
}
```
> [!NOTE]
> Önceden derlenmiş bir kalıcı işlevi C# vs'de C# betik örneği önce gösterilen yazılırken küçük farklılıklar vardır. C# önceden derlenmiş işlevi sürekli parametreleri ilgili öznitelikleri ile donatılmış gerekir. Bir örnek `[OrchestrationTrigger]` özniteliğini `DurableOrchestrationContext` parametresi. Parametreleri doğru donatılmış değil, çalışma zamanı değişkenleri işleve eklemesine mümkün olmaz ve hata verirsiniz. Lütfen [örnek](https://github.com/Azure/azure-functions-durable-extension/blob/master/samples) daha fazla örnek için.

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(ctx) {
    const x = yield ctx.df.callActivity("F1");
    const y = yield ctx.df.callActivity("F2", x);
    const z = yield ctx.df.callActivity("F3", y);
    return yield ctx.df.callActivity("F4", z);
});
```

"F1", "F2", "F3" ve "F4" değerlerini işlev uygulamasına diğer işlevlerin adlarıdır. Denetim akışı yapılarını kodlama normal zorunlu uygulanır. Diğer bir deyişle, kod yukarıdan aşağıya yürütür ve mevcut dil denetim akışı semantiğini koşullular ve döngüler gibi içerebilir.  Hata işleme mantığı, try/catch/finally bloklarında eklenebilir.

`ctx` Parametre ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) adı, parametreleri geçirme, diğer işlevler çağırma ve işlev çıkış döndüren yöntemler sağlar. Her zaman kod çağrıları `await`, dayanıklı işlevler framework *kontrol noktaları* ilerleme durumunu geçerli işlev örneği. Sanal makine ve işlem sürecin yarısında yürütme dönüştürülürse, işlev örneği önceki sürdürür `await` çağırın. Bunun hakkında daha fazla daha sonra yeniden başlatma davranışı.

## <a name="pattern-2-fan-outfan-in"></a>Desen #2: Fan-dışarı/fan-arada

*Fan-dışarı/fan-arada* birden çok işlevleri paralel olarak yürütülen ve tüm tamamlanması bekleniyor deseni ifade eder.  Genellikle bazı toplama iş işlevlerini döndürülen sonuçlar üzerinde gerçekleştirilir.

![Fan-dışarı/fan-arada diyagramı](media/durable-functions-overview/fan-out-fan-in.png)

Normal işlevlerde olduğu fanning sahip birden fazla ileti bir kuyruğa gönderebilirsiniz işlevi tarafından yapılabilir. Ancak geri fanning çok daha zor olabilir. Kuyruk ile tetiklenen işlev sonlandırmak ve işlev çıktılarının depolanması ne zaman açtıklarını izlemek için kod yazmanız gerekir. Bu düzen görece basit kod ile dayanıklı işlevler uzantısını işler.

#### <a name="c-script"></a>C# betiği

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    var parallelTasks = new List<Task<int>>();
 
    // get a list of N work items to process in parallel
    object[] workBatch = await ctx.CallActivityAsync<object[]>("F1");
    for (int i = 0; i < workBatch.Length; i++)
    {
        Task<int> task = ctx.CallActivityAsync<int>("F2", workBatch[i]);
        parallelTasks.Add(task);
    }
 
    await Task.WhenAll(parallelTasks);
 
    // aggregate all N outputs and send result to F3
    int sum = parallelTasks.Sum(t => t.Result);
    await ctx.CallActivityAsync("F3", sum);
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```js
const df = require("durable-functions");

module.exports = df.orchestrator(function*(ctx) {
    const parallelTasks = [];

    // get a list of N work items to process in parallel
    const workBatch = yield ctx.df.callActivity("F1");
    for (let i = 0; i < workBatch.length; i++) {
        parallelTasks.push(ctx.df.callActivity("F2", workBatch[i]));
    }

    yield ctx.df.task.all(parallelTasks);

    // aggregate all N outputs and send result to F3
    const sum = parallelTasks.reduce((prev, curr) => prev + curr, 0);
    yield ctx.df.callActivity("F3", sum);
});
```

Yaygın iş işlevinin birden fazla örneğe dağıtılmış `F2`, ve dinamik görevlerinin listesini kullanarak iş izlenir. .NET `Task.WhenAll` API tüm çağrılan işlevlerin tamamlanması için beklenecek çağrılır. Ardından `F2`çıkışları dinamik görev listesinden toplanır ve geçirilen üzerinde işlevi `F3` işlevi.

Sırasında gerçekleşen otomatik denetim noktası `await` çağırmak `Task.WhenAll` herhangi bir kilitlenme veya yeniden başlatma sürecin yarısında zaten tamamlanan görevler herhangi bir yeniden başlatma gerektirmez sağlar.

## <a name="pattern-3-async-http-apis"></a>#3. Desen: Zaman uyumsuz HTTP API'leri

Tüm dış istemcilerle uzun süren işlemlerin durumunu koordine sorununu üçüncü deseni hakkındadır. Bir HTTP çağrısı tarafından tetiklenen uzun süre çalışan işlem sağlayarak bu deseni uygulamak için yaygın bir yolu olan ve ardından işlem tamamlandığında öğrenmek yoklamak bir durum uç noktasına istemci yeniden yönlendirme.

![HTTP API'si diyagramı](media/durable-functions-overview/async-http-api.png)

Dayanıklı İşlevler, uzun süre çalışan işlev yürütmelerini ile etkileşim kurmak için yazdığınız kodu kolaylaştıran yerleşik API'ler sağlar. [Örnekleri](durable-functions-install.md) Göster yeni orchestrator işlevi örneklerini başlatmak için kullanılan basit bir REST komutu. Uzantı örneği başlatıldıktan sonra Web kancası HTTP API'lerini orchestrator işlevi durumunu sorgulayan kullanıma sunar. Aşağıdaki örnek, bir orchestrator başlatmak ve durumunu sorgulamak için REST komutları gösterir. Anlaşılsın diye, bazı ayrıntılar örnekten göz ardı edilir.

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

Dayanıklı işlevler çalışma zamanı tarafından yönetilen durumu olduğundan, kendi durumunu izleme mekanizması uygulamak zorunda değilsiniz.

Dayanıklı işlevler uzantısını uzun süre çalışan düzenlemeleri yönetmek için yerleşik Web kancaları olmasına rağmen bu düzen kendi işlevi tetikleyicilerini (örneğin, HTTP, kuyruk veya olay hub'ı) kullanarak kendiniz uygulayabileceğiniz ve `orchestrationClient` bağlama. Örneğin, bir kuyruk iletisi sonlandırma tetiklemek için kullanabilirsiniz.  Veya yerleşik Web kancaları kimlik doğrulaması için oluşturulmuş bir anahtar kullanmak yerine bir Azure Active Directory kimlik doğrulama İlkesi tarafından korunan bir HTTP tetikleyicisi kullanabilir. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    ILogger log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parametredir arasında bir değer `orchestrationClient` dayanıklı işlevler uzantısını parçası olan bağlama, çıktı. Bunun için başlangıç gönderen olaylar için sonlandırılması ve yeni veya var olan orchestrator işlevi örnekleri için sorgulama yöntemleri sağlar. Önceki örnekte, bir HTTP ile tetiklenen-işlev alır bir `functionName` değerini gelen URL ve değerini geçişleri [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Bu bağlama API ardından içeren bir yanıt döndürür bir `Location` üstbilgi ve daha sonra aramak için kullanılabilecek örneği hakkında ek bilgi kullanmaya başlama örneği durumu yukarı ya da sonlandırın.

## <a name="pattern-4-monitoring"></a>Desen #4: izleme

İzleyici deseni esnek bir başvuruyor *yinelenen* bir iş akışında - Örneğin, belirli koşulların karşılanması kadar yoklama işlemi. Normal bir zamanlayıcı tetikleyicisi bir düzenli temizleme işini gibi basit bir senaryoyu ele ancak kendi aralığı, statik ve örnek yaşam süreleri yönetme karmaşık hale gelir. Dayanıklı işlevler esnek yineleme aralıkları, görevin ömrü yönetimi ve birden çok İzleyici, tek bir düzenleme işlemleri oluşturma olanağı sağlar.

Bir örnek, önceki zaman uyumsuz HTTP API senaryosu ters. Uzun süreli bir işlemi izlemek bir dış istemci için bir uç nokta kullanıma sunmak yerine, bazı durum değişikliği için bekleyen dış uç noktası, uzun süre çalışan İzleyicisi'ni kullanır.

![İzleme diyagramı](media/durable-functions-overview/monitor.png)

Dayanıklı işlevler kullanarak, birkaç kod satırıyla rastgele uç noktaları inceleyin birden çok monitör oluşturulabilir. Bazı koşullar karşılanması veya tarafından sonlandırılacak izleyiciler yürütme sona erdirebilirsiniz [DurableOrchestrationClient](durable-functions-instance-management.md), ve bunların bekleme aralığının bazı koşullar (yani üstel geri alma.) göre değiştirilebilir. Aşağıdaki kod, temel bir izleyici uygular.

#### <a name="c-script"></a>C# betiği

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int jobId = ctx.GetInput<int>();
    int pollingInterval = GetPollingInterval();
    DateTime expiryTime = GetExpiryTime();
    
    while (ctx.CurrentUtcDateTime < expiryTime) 
    {
        var jobStatus = await ctx.CallActivityAsync<string>("GetJobStatus", jobId);
        if (jobStatus == "Completed")
        {
            // Perform action when condition met
            await ctx.CallActivityAsync("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        var nextCheck = ctx.CurrentUtcDateTime.AddSeconds(pollingInterval);
        await ctx.CreateTimer(nextCheck, CancellationToken.None);
    }

    // Perform further work here, or let the orchestration end
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(ctx) {
    const jobId = ctx.df.getInput();
    const pollingInternal = getPollingInterval();
    const expiryTime = getExpiryTime();

    while (moment.utc(ctx.df.currentUtcDateTime).isBefore(expiryTime)) {
        const jobStatus = yield ctx.df.callActivity("GetJobStatus", jobId);
        if (jobStatus === "Completed") {
            // Perform action when condition met
            yield ctx.df.callActivity("SendAlert", machineId);
            break;
        }

        // Orchestration will sleep until this time
        const nextCheck = moment.utc(ctx.df.currentUtcDateTime).add(pollingInterval, 's');
        yield ctx.df.createTimer(nextCheck.toDate());
    }

    // Perform further work here, or let the orchestration end
});
```

Bir istek alındığında, iş kimliği için yeni bir düzenleme örneği oluşturulur Örnek, bir koşul karşılandığında ve döngü çıkıldı kadar durumu yoklar. Dayanıklı bir Zamanlayıcı, yoklama aralığını denetlemek için kullanılır. Daha fazla iş gerçekleştirilebilir veya orchestration sonlandırabilirsiniz. Zaman `ctx.CurrentUtcDateTime` aşıyor `expiryTime`, İzleyici sona erer.

## <a name="pattern-5-human-interaction"></a>Desen #5: İnsan etkileşimi

Birçok işlemlerinden insan etkileşimi tür oluşur. Otomatik bir işlem okunacağından içeren hakkında zor olan şey kişiler her zaman olarak yüksek oranda kullanılabilir ve bulut Hizmetleri olarak olmamasıdır. Otomatik işlemler bunu izin vermeniz gerekir ve bunlar çoğunlukla zaman aşımları ve telafi mantığını kullanarak bunu yapabilirsiniz.

Bir iş sürecini insan etkileşimi içeren bir onay işlemi örneğidir. Örneğin, bir yönetici onayını belirli bir miktar aşan bir harcama rapor için gerekli olabilir. 72 saat (belki de bunlar tatile oluştu) içinde yöneticiyi onaylamaz, yükseltme işlemi onayı birisinden (belki de yöneticinin yöneticisinin) almak için devreye girer.

![İnsan etkileşimi diyagramı](media/durable-functions-overview/approval.png)

Bu düzen bir düzenleyici işlevi kullanılarak uygulanır. Orchestrator kullanacağınız bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) onay isteyin ve zaman aşımı oluşması halinde ilerletebilirsiniz. İçin beklemeniz gerekir bir [dış olay](durable-functions-external-events.md), bazı insan etkileşimi tarafından oluşturulan bildirim olacaktır.

#### <a name="c-script"></a>C# betiği

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    await ctx.CallActivityAsync("RequestApproval");
    using (var timeoutCts = new CancellationTokenSource())
    {
        DateTime dueTime = ctx.CurrentUtcDateTime.AddHours(72);
        Task durableTimeout = ctx.CreateTimer(dueTime, timeoutCts.Token);

        Task<bool> approvalEvent = ctx.WaitForExternalEvent<bool>("ApprovalEvent");
        if (approvalEvent == await Task.WhenAny(approvalEvent, durableTimeout))
        {
            timeoutCts.Cancel();
            await ctx.CallActivityAsync("ProcessApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```js
const df = require("durable-functions");
const moment = require('moment');

module.exports = df.orchestrator(function*(ctx) {
    yield ctx.df.callActivity("RequestApproval");

    const dueTime = moment.utc(ctx.df.currentUtcDateTime).add(72, 'h');
    const durableTimeout = ctx.df.createTimer(dueTime.toDate());

    const approvalEvent = ctx.df.waitForExternalEvent("ApprovalEvent");
    if (approvalEvent === yield ctx.df.Task.any([approvalEvent, durableTimeout])) {
        durableTimeout.cancel();
        yield ctx.df.callActivity("ProcessApproval", approvalEvent.result);
    } else {
        yield ctx.df.callActivity("Escalate");
    }
});
```

Dayanıklı Zamanlayıcı çağrılarak oluşturulan `ctx.CreateTimer`. Bildirim tarafından alınan `ctx.WaitForExternalEvent`. Ve `Task.WhenAny` İlerlet karar vermek için çağrılır (ilk zaman aşımı gerçekleşir) veya işlem onay (zaman aşımından önce onay aldı).

Dış bir istemci kullanarak bekleyen orchestrator işlevi için olay bildirimi sunabilir [yerleşik HTTP API'lerini](durable-functions-http-api.md#raise-event) kullanarak veya [DurableOrchestrationClient.RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_System_String_System_String_System_Object_) API'SİNDEN başka bir işlev:

```csharp
public static async Task Run(string instanceId, DurableOrchestrationClient client)
{
    bool isApproved = true;
    await client.RaiseEventAsync(instanceId, "ApprovalEvent", isApproved);
}
```

## <a name="the-technology"></a>Teknoloji

Arka planda üst kısmındaki dayanıklı işlevler uzantısını oluşturulmuştur [dayanıklı görev Framework](https://github.com/Azure/durabletask), dayanıklı görev düzenlemeleri oluşturmak için bir GitHub üzerinde açık kaynak kitaplığı. Çok nasıl Azure işlevleri Azure WebJobs sunucusuz gelişimi gibi dayanıklı işlevler dayanıklı görev Framework sunucusuz gelişimi yükledik. Dayanıklı görev Framework yoğun olarak Microsoft içinde ve dışında da kritik işlemleri otomatik hale getirmek için kullanılır. Bu sunucusuz Azure işlevleri ortam için uygun bir kullanımdır olur.

### <a name="event-sourcing-checkpointing-and-replay"></a>Olay kaynağını belirleme, denetim noktası ve yeniden yürütme

Orchestrator İşlevler, yürütme durumlarını olarak bilinen bir tasarım desenini kullanarak güvenilir bir şekilde korumak [olay kaynağını belirleme](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). Doğrudan depolamak yerine *geçerli* düzenleme, dayanıklı uzantısı durumunu kaydetmek için bir yalnızca ekleme deposu kullanan *tam Eylemler dizisi* işlevi düzenleme tarafından gerçekleştirilen. Bu performansı, ölçeklenebilirliği ve yanıt hızını "tam çalışma zamanı durumunu dökme" karşılaştırıldığında iyileştirme de dahil olmak üzere birçok avantaj sunar. Diğer avantajları işlem verilerinde nihai tutarlılık sağlayan ve tam denetim kayıtlarını ve geçmişini içerir. Denetim kayıtlarını güvenilir telafi eylemlerine olanak.

Olay kaynağını belirleme kullanımını bu uzantı tarafından saydamdır. Kapak altında `await` orchestrator işlevde işlecini verir orchestrator iş parçacığının denetim dayanıklı görev Framework dağıtıcısıyla. Dağıtıcı, orchestrator işlevi (bir veya daha fazla alt işlevlerini çağırma veya kalıcı bir zamanlayıcı zamanlama) planlanan yeni eylemler ardından depolama alanına kaydeder. Bu saydam işleme eylemi ekler *yürütme geçmişini* düzenleme örneği. Geçmiş depolama tablosunda depolanır. İşleme işlem iletileri asıl işi zamanlamak için bir kuyruk ekler. Bu noktada, orchestrator işlevi bellekten olabilir. Azure işlevleri tüketim planı kullanıyorsanız, bunu faturalandırması durdurur.  Yapmak için daha fazla iş olduğunda işlevi yeniden başlatılır ve durumunu yeniden düzenlenir.

Daha fazla iş yapmak için bir düzenleme işlevi verildikten sonra (örneğin, bir yanıt iletisi alındı veya sağlam bir süreölçerin süresi), orchestrator yeniden uyanır ve tüm işlevi en başından itibaren yerel durumu yeniden oluşturmak için yeniden yürütür. Bu yeniden yürütme sırasında bir işlevi çağırmak kod çalışırsa (veya diğer zaman uyumsuz iş), dayanıklı görev Framework ile danışır *yürütme geçmişini* geçerli düzenleme. Bu, bulunursa [etkinlik işlevi](durable-functions-types-features-overview.md#activity-functions) yürütülen ve veriyor bazı sonucu zaten, bu işlevin sonucu başlayarak yeniden oynatılır ve orchestrator kod çalışmaya devam eder. Bu işlev kodunu alır burada tamamlandıktan veya zamanlanmış yeni zaman uyumsuz çalışma sahip bir noktaya kadar'olmuyor devam eder.

### <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Yeniden yürütme davranışını kısıtlamaları bir orchestrator işlevinde yazılmış kodun türünü oluşturur. Örneğin, birden çok kez yeniden yürütülmesi gereken ve her zaman aynı sonucu üretmelidir orchestrator kod belirleyici, olması gerekir. Kısıtlamaları tam listesini bulabilirsiniz [Orchestrator kod kısıtlamaları](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) bölümünü **denetim noktası oluşturma ve yeniden başlatma** makalesi.

## <a name="language-support"></a>Dil desteği

Şu anda C# (işlevler v1 ve v2), F # ve JavaScript (yalnızca işlevler v2) için desteklenen diller yalnızca dayanıklı işlevlerdir. Bu, orchestrator işlevlerini ve etkinlik işlevlerini içerir. Gelecekte, Azure işlevleri desteklediği tüm dilleri için destek ekleyeceğiz. Azure işlevleri [GitHub depo sorunlar listesini](https://github.com/Azure/azure-functions-durable-extension/issues) iş destek sayfamızı ek dil en son durumunu görmek için.

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

Dayanıklı işlevler uzantısını yapılandırılmış izleme verileri otomatik olarak yayar [Application Insights](functions-monitoring.md) işlev uygulaması ile bir Application Insights izleme anahtarı yapılandırıldığında. Bu izleme verilerini, ilerleme durumunu, düzenlemeleri ve davranışını izlemek için kullanılabilir.

Dayanıklı işlevler olayları izleme Application Insights portal kullanarak nasıl göründüğünü ilişkin bir örnek aşağıda verilmiştir [Application Insights Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![App Insights sorgu sonuçları](media/durable-functions-overview/app-insights-1.png)

Birçok yararlı yapılandırılmış verilerin halinde paketlenmiş `customDimensions` alanındaki her günlük girişi. Tümüyle genişletilemiyor gibi bir giriş örneği aşağıda verilmiştir.

![App Insights sorgu customDimensions alanı](media/durable-functions-overview/app-insights-2.png)

Dayanıklı görev Framework dağıtıcı yeniden yürütme davranışı nedeniyle yeniden yürütülmüş eylemler için yedekli günlük girişlerini görmek bekleyebilirsiniz. Bu, temel altyapısını yeniden yürütme davranışını anlamak yararlı olabilir. [Tanılama](durable-functions-diagnostics.md) makale yalnızca "gerçek zamanlı" günlükleri görebilirsiniz, böylece yeniden yürütme günlüğünü filtrelemek için örnek sorgular.

## <a name="storage-and-scalability"></a>Depolama ve ölçeklenebilirlik

Dayanıklı işlevler uzantısını, yürütme geçmişini durumu ve tetikleyici işlevi yürütme kalıcı hale getirmek için Azure depolama kuyruklarına, tablo ve BLOB'ları kullanır. İşlev uygulaması için varsayılan depolama hesabı kullanılabilir veya ayrı bir depolama hesabı yapılandırabilirsiniz. Depolama aktarım hızı sınırlarına nedeniyle ayrı bir hesap isteyebilirsiniz. Orchestrator kodları bu depolama hesaplarında varlıklarla etkileşim gerekmez ve kullanmalısınız değil. Varlıklar, uygulama ayrıntısı doğrudan dayanıklı görev Framework tarafından yönetilir.

Orchestrator işlevleri, etkinlik işlevlerini zamanlayabilir ve yanıtlarını iç iletileri aracılığıyla alırsınız. Bir işlev uygulaması Azure işlevleri tüketim planında çalıştığında, bu kuyruk tarafından izlenen [Azure işlevlerini ölçeklendirme denetleyicisi](functions-scale.md#how-the-consumption-plan-works) ve yeni örnekleri, gerektikçe eklenir işlem. Çağrı etkinlik işlevlere birkaç farklı Vm'lere çalıştırırken bir düzenleyici işlevi için birden çok VM ölçeği, bir VM üzerinde çalıştırabilirsiniz. Dayanıklı işlevler ölçek davranışı üzerinde daha fazla ayrıntı bulabilirsiniz [performansı ve ölçeği](durable-functions-perf-and-scale.md).

Tablo depolama, orchestrator hesapları için yürütme geçmişi depolamak için kullanılır. Belirli bir VM örneği rehydrates olduğunda, böylece yerel durumunu yeniden oluşturabilirsiniz, yürütme geçmişini tablo Depolama'yı getirir. Geçmiş tablo depolamada kullanılabilir olması hakkında kullanışlı şeylerden biri olduğundan göz atın ve gibi araçları kullanarak, düzenlemeleri geçmişini görebilir [Microsoft Azure Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

Depolama BLOB'ları orchestration örneklerinin genişleme birden çok VM arasında koordine etmek için öncelikle bir kiralama mekanizmasının kullanılır. Bunlar doğrudan tabloları veya Kuyrukları depolanamaz büyük iletiler için verileri tutmak için kullanılır.

![Azure Depolama Gezgini ekran görüntüsü](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Tablo depolama yürütme geçmişini görmek kolay olsa da bu tabloyu temel bağımlılığın alma kaçının. Dayanıklı işlevler uzantısını geliştikçe değişebilir.

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Tüm bilinen sorunları izlenmesi gereken [GitHub sorunları](https://github.com/Azure/azure-functions-durable-extension/issues) listesi. Bir sorunla karşılaşırsanız ve sorunu Github'da bulunamıyor, yeni bir sorun açın ve sorunun ayrıntılı bir açıklama ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevler belgeleri okumaya devam edin](durable-functions-types-features-overview.md)

> [!div class="nextstepaction"]
> [Örnekler ve dayanıklı işlevler uzantısını yükleme](durable-functions-install.md)

