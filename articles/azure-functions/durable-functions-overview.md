---
title: "Dayanıklı işlevlerine genel bakış - Azure"
description: "Azure işlevleri dayanıklı işlevleri uzantısı için giriş."
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
ms.openlocfilehash: 04d660d5fdd878788c09e46b078b2e2b043b7dbb
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2017
---
# <a name="durable-functions-overview-azure-functions"></a>Dayanıklı işlevlerine genel bakış (Azure işlevleri)

*Dayanıklı işlevleri* uzantısıdır [Azure işlevleri](functions-overview.md) ve [Azure Web işleri](../app-service/web-sites-create-web-jobs.md) olanak tanıyan sunucusuz bir ortamda durum bilgisi olan işlevler yazma. Uzantı durumu, kontrol noktaları ve yeniden başlatmalar sizin için yönetir.

Uzantı, durum bilgisi olan iş akışları işlevi adlı yeni bir tür tanımlamanıza olanak sağlar bir *orchestrator işlevi*. Orchestrator işlevleri avantajlarından bazıları şunlardır:

* Bunlar, kodda iş akışları tanımlayın. Herhangi bir JSON şemaları veya tasarımcıları gereklidir.
* Zaman uyumlu ve zaman uyumsuz olarak bunlar diğer işlevleri çağırabilir. Çağrılan işlevlerin çıktısını yerel değişkenler için kaydedilebilir.
* Bunlar otomatik olarak denetim noktası işlevi bekler her kendi devam ediyor. Yerel durumu, hiçbir zaman işlemi geri dönüştürüldüğünde veya VM yeniden kaybolur.

> [!NOTE]
> Dayanıklı işlevleri Azure işlevleri için Gelişmiş bir uzantısıdır ve tüm uygulamalar için uygun değil. Bu makalenin geri kalanında, güçlü bir aşina sahibi olduğunuzu varsayar [Azure işlevleri](functions-overview.md) kavramları ve sorunları söz konusu sunucusuz bir uygulama geliştirme.

Dayanıklı işlevler için birincil kullanım durumu sunucusuz uygulamalardaki karmaşık, durum bilgisi olan koordinasyon sorunları basitleştirir. Aşağıdaki bölümlerde dayanıklı işlevlerinden yararlanabilirsiniz bazı tipik uygulama düzenleri açıklanmaktadır.

## <a name="pattern-1-function-chaining"></a>Deseni #1: zincirleme işlevi

*İşlev zincirleme* belirli bir sırada işlevleri bir dizi yürütmenin düzeni başvuruyor. Genellikle bir işlev çıktısını başka bir işlevin giriş uygulanması gerekir.

![İşlev zincirleme diyagramı](media/durable-functions-overview/function-chaining.png)

Dayanıklı işlevleri sağlar. Bu deseni yönelik olarak kısaca koda uygulanması.

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

"F1", "F2", "F3" ve "F4" işlev uygulaması diğer işlevlerin adları değerlerdir. Denetim akışı normal kesinliği yapıları kodlama kullanılarak uygulanır. Diğer bir deyişle, kod yukarıdan aşağı yürütür ve mevcut dil denetim akışı semantiği, koşulları ve döngüleri gibi içerebilir.  Hata işleme mantığı try/catch/finally bloğu dahil edilebilir.

`ctx` Parametre ([DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html)) parametreleri geçirme, ada göre diğer işlevleri yürütmesini ve işlevi çıktı döndürmek için yöntemleri sağlar. Her zaman kodu çağrıları `await`, dayanıklı işlevleri framework *kontrol noktaları* geçerli işlevi örneğinin ilerleme. Yarıda yürütme aracılığıyla işlem ya da VM dönüştürülürse, işlev örneğini önceki sürdürür `await` çağırın. Bunun hakkında daha fazla daha sonra yeniden başlatma davranışı.

## <a name="pattern-2-fan-outfan-in"></a>Desen #2: Fan-dışarı/fan-arada

*Fan-dışarı/fan-arada* paralel olarak birden çok işlevler yürütmek ve tüm tamamlanması bekleniyor desenini başvuruyor.  Genellikle işlevlerden döndürülen sonuçlar üzerinde bazı toplama iş yapılır.

![Fan-dışarı/fan-arada diyagramı](media/durable-functions-overview/fan-out-fan-in.png)

Normal işlevleriyle fanning bir sıraya birden çok ileti gönderme işlevi sağlayarak yapılabilir. Ancak, geri fanning çok daha zordur. Sıra tetiklemeli işlevleri sonlandırmak ve işlev çıkışları depolamak izlemek için kod yazmanız gerekir. Dayanıklı işlevleri uzantısını bu deseni oldukça basit kod ile işler.

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

Yayma iş işlevi birden çok örneğini dağıtılmış `F2`, ve iş görevleri dinamik bir listesini kullanarak izlenir. .NET `Task.WhenAll` API tüm çağrılan işlevlerin tamamlanması için beklenecek çağrılır. Ardından `F2`işlevi çıkarır dinamik görev listesinden toplanır ve oturum geçirilen `F3` işlevi.

Konumundaki olur otomatik denetim `await` çağırmak `Task.WhenAll` herhangi çökmesi ya da yeniden başlatma yarıda aracılığıyla zaten tamamlanan görevler herhangi bir yeniden başlatma gerektirmez sağlar.

## <a name="pattern-3-async-http-apis"></a>Desen #3: Zaman uyumsuz HTTP API'leri

Tüm dış istemcilerle uzun süre çalışan işlemleri durumunu Eşgüdümleme sorununu üçüncü düzeni hakkındadır. Bu desen uygulamak için bir genel bir HTTP çağrısı tarafından tetiklenen uzun süre çalışan eylemini sağlayarak yoludur ve işlem tamamlandığında öğrenmek için yoklayabilir durum uç noktasına istemci yeniden yönlendirme.

![HTTP API diyagramı](media/durable-functions-overview/async-http-api.png)

Dayanıklı işlevleri uzun süre çalışan işlevi yürütmeleri ile etkileşmek için yazdığınız kodu basitleştirmek yerleşik API'ler sağlar. [Örnekleri](durable-functions-install.md) Göster yeni orchestrator işlevi örnekleri başlatmak için kullanılan basit bir REST komutu. Uzantı örneği başladıktan sonra Web kancası HTTP API'leri sorgulayan orchestrator işlevi durum kullanıma sunar. Aşağıdaki örnek, bir orchestrator başlatmak ve durumunu sorgulamak için REST komutları gösterir. Daha anlaşılır olması için bazı ayrıntılar örnekten göz ardı edilir.

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

Durumu dayanıklı işlevleri çalışma zamanı tarafından yönetildiğinden, bu kendi durum mekanizması izleme uygulamak sahip değilsiniz.

Uzun süre çalışan düzenlemelerin yönetmek için yerleşik Web kancalarını dayanıklı işlevleri uzantısına sahip olsa bile, bu deseni kendi işlevi Tetikleyicileri (örneğin HTTP, kuyruk veya olay hub'ı) kullanarak kendiniz uygulayabilirsiniz ve `orchestrationClient` bağlama. Örneğin, bir kuyruk iletisi sonlandırma tetiklemek için kullanabilirsiniz.  Veya yerleşik Web kancalarını oluşturulan anahtar kimlik doğrulaması için kullanmak yerine bir Azure Active Directory kimlik doğrulama İlkesi tarafından korunan bir HTTP tetikleyicisi kullanabilir. 

```cs
// HTTP-triggered function to start a new orchestrator function instance.
public static async Task<HttpResponseMessage> Run(
    HttpRequestMessage req,
    DurableOrchestrationClient starter,
    string functionName,
    TraceWriter log)
{
    // Function name comes from the request URL.
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);
    
    log.Info($"Started orchestration with ID = '{instanceId}'.");
    
    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) `starter` parametredir arasında bir değer `orchestrationClient` bağlama, dayanıklı işlevleri uzantısı'nın parçası olduğu çıktı. İçin başlangıç gönderen olaylar için sonlandırma ve yeni veya var olan orchestrator işlevi örnekleri için sorgulama yöntemleri sağlar. Yukarıdaki örnekte, bir HTTP tetiklenen-işlevi alır bir `functionName` gelen URL ve değeri geçişleri değerinden [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_). Bu bağlama API ardından içeren bir yanıt döndürür bir `Location` üstbilgi ve daha sonra aramak için kullanılabilecek örnek hakkında ek bilgi yukarı başlatılan örneğinin durumunu veya sonlandırmak.

## <a name="pattern-4-stateful-singletons"></a>Deseni #4: Durum bilgisi olan teklileri

Çoğu işlevleri bir açık başlangıç ve bitiş vardır ve dış olay kaynakları ile doğrudan etkileşim yoktur. Ancak, düzenlemelerin destekleyen bir [durum bilgisi olan tek](durable-functions-singletons.md) davranmasına veren düzeni güvenilir ister [aktörler](https://en.wikipedia.org/wiki/Actor_model) dağıtılmış bilgi işlem.

Aşağıdaki diyagram, dış kaynaklardan gelen olayları işlerken sonsuz bir döngüde çalışır bir işlev gösterir.

![Durum bilgisi olan singleton diyagramı](media/durable-functions-overview/stateful-singleton.png)

Dayanıklı işlevleri aktör modeli Uygulaması olmamasına karşın, orchestrator işlevleri aynı çalışma zamanı özellikleri çoğunun vardır. Örneğin, uzun süre çalışan (büyük olasılıkla sonsuz), durum bilgisi olan, güvenilir, tek iş parçacıklı, konum saydam ve genel olarak adreslenebilir vardır. Bu orchestrator işlevleri "Aktör" için kullanışlı hale getirir-senaryoları ister.

Durum bilgisiz ve bu nedenle değil uygun bir durum bilgisi olan singleton deseni uygulamak için bunun sıradan işlevlerdir. Ancak, dayanıklı işlevleri uzantısı durum bilgisi olan singleton deseni uygulamak için görece basit hale getirir. Aşağıdaki kod bir sayaç uygulayan bir basit orchestrator işlevdir.

```cs
public static async Task Run(DurableOrchestrationContext ctx)
{
    int counterState = ctx.GetInput<int>();

    string operation = await ctx.WaitForExternalEvent<string>("operation");
    if (operation == "incr")
    {
        counterState++;
    }
    else if (operation == "decr")
    {
        counterState--;
    }

    ctx.ContinueAsNew(counterState);
}
```

Bu kodu bir "eternal orchestration" açıklayabilir &mdash; diğer bir deyişle, bir başlar ve hiçbir zaman biter. Aşağıdakileri yapar:

* Bir giriş değerinin ile başlayan `counterState`.
* Bir ileti süresiz olarak bekler adlı `operation`.
* Yerel durumunu güncelleştirmek için bazı mantığı gerçekleştirir.
* "Kendisini çağırarak yeniden" `ctx.ContinueAsNew`.
* Yeniden bekler süresiz olarak sonraki işlemi için.

## <a name="pattern-5-human-interaction"></a>Desen #5: İnsan etkileşimi

Birçok işlemleri insan etkileşimi çeşit içerir. Otomatik bir işlem içinde insanlar içeren hakkında hassas kişiler her zaman olarak yüksek oranda kullanılabilir ve bulut Hizmetleri olarak yanıt veren olmadığından şeydir. Bunlar çoğunlukla zaman aşımları ve maaş mantığı kullanarak bunu ve otomatik işlemleri için bu ayırmanız gerekir.

Bir insan etkileşimi içeren bir iş sürecinin bir onay işlemi örnektir. Örneğin, bir yönetici onayını belirli bir miktar aşan gider raporu için gerekli olabilir. Yöneticisi 72 saat (tatile oluştu olabilir) içinde onaylamaz, bir yükseltme işlemi onayı birisinden (belki de manager'ın Yöneticisi) almak için devreye girer.

![İnsan etkileşimi diyagramı](media/durable-functions-overview/approval.png)

Bu deseni orchestrator işlevi kullanılarak uygulanabilir. Orchestrator kullanmak istediğiniz bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) onay istemek ve zaman aşımı durumunda ilerletmek için. Beklemeniz bir [dış olay](durable-functions-external-events.md), bazı insan etkileşimi tarafından oluşturulan bildirim olacaktır.

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
            await ctx.CallActivityAsync("HandleApproval", approvalEvent.Result);
        }
        else
        {
            await ctx.CallActivityAsync("Escalate");
        }
    }
}
```

Dayanıklı Zamanlayıcı çağrılarak oluşturulan `ctx.CreateTimer`. Bildirim tarafından alınan `ctx.WaitForExternalEvent`. Ve `Task.WhenAny` ilerletmek karar vermek için çağrılır (ilk zaman aşımı gerçekleşir) veya işlem onay (zaman aşımından önce onay alındı).

## <a name="the-technology"></a>Teknoloji

Arka planda üstünde dayanıklı işlevleri uzantısı oluşturulmuştur [dayanıklı görev Framework](https://github.com/Azure/durabletask), dayanıklı görev düzenlemelerin oluşturmak için bir açık kaynak kitaplığı github'da. Çok nasıl Azure işlevleri Azure Web işleri sunucusuz evrimi gibi dayanıklı işlevleri dayanıklı görev Framework sunucusuz evrimi olmasıdır. Dayanıklı görev Framework yoğun olarak Microsoft içinde ve dışında da kritik süreçlerini otomatikleştirmek için kullanılır. Sunucusuz Azure işlevleri ortamı için doğal bir sığdırma olur.

### <a name="event-sourcing-checkpointing-and-replay"></a>Olay kaynak belirleme, denetim noktası oluşturma ve yeniden yürütme

Orchestrator işlevler olarak bilinen bir bulut tasarım modeli kullanarak yürütme durumlarına güvenilir bir şekilde korumak [olay kaynak Hizmeti'nden](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing). Doğrudan depolamak yerine *geçerli* dayanıklı uzantısı bir düzenleme durumunu kaydetmek için bir yalnızca append deposu kullanır *tam Eylemler dizisi* işlevi orchestration tarafından gerçekleştirilecek. Bu, performans, ölçeklenebilirlik ve "tam çalışma zamanı durumu döküm alma için" karşılaştırıldığında yanıtlama hızı artırma gibi birçok avantaj vardır. Nihai tutarlılık sağlamak için işlem verilerini ve tam denetim izleri ve geçmiş koruyarak diğer avantajlar şunlardır. Denetim izleri güvenilir karşılayan Eylemler etkinleştirin.

Olay kaynak kullanımını bu uzantı tarafından saydamdır. Kapak altında `await` işleci bir orchestrator işlevinde dayanıklı görev Framework dağıtıcısıyla orchestrator iş parçacığının denetim verir. Dağıtıcı orchestrator işlevi (bir veya daha fazla alt işlevleri çağırma veya dayanıklı süreölçer zamanlama gibi) zamanlanmış eylemlere yeni depolama birimine sonra kaydeder. Bu saydam yürütme eylem iliştirilir *yürütme geçmişini* orchestration örneği. Geçmiş dayanıklı depolama alanında depolanır. Yürütme eylem ardından iletileri asıl işi zamanlamak için bir kuyruğa ekler. Bu noktada, orchestrator işlevi bellekten olabilir. Azure işlevleri tüketim planlama kullanıyorsanız, bunun için fatura durdurur.  Yapmak için daha fazla iş olduğunda işlevi yeniden ve durumunu yeniden düzenlenir.

Orchestration işlevi yapmak için daha fazla iş verildikten sonra (örneğin, bir yanıt iletisi aldı veya dayanıklı süreölçer süresi), orchestrator yeniden uyanır ve yeniden Başlat tüm işlevinden yerel durumu yeniden oluşturmak için yürütür. Bu yeniden yürütme sırasında bir işlevi çağırmak kodu çalışırsa (veya başka bir zaman uyumsuz iş), dayanıklı görev Framework ile danışır *yürütme geçmişini* geçerli orchestration. Etkinlik işlevi zaten yürütüldü ve bazı sonuç verdiğini, onu bu işlevin sonucu başlayarak yeniden oynatılır ve orchestrator kodu çalışmaya devam bulursa. Bu işlev kodu nereye tamamlanmadan veya zamanlanmış yeni zaman uyumsuz iş olduğu bir noktasına ulaşana kadar gerçekleştiği devam eder.

### <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Yeniden yürütme davranışı bir orchestrator işlevinde yazılan kod türünü kısıtlamalar oluşturur. Örneğin, birden çok kez yeniden ve her zaman aynı sonucu gerekir orchestrator kodu belirleyici, olmalıdır. Kısıtlamaları tam listesi bulunabilir [Orchestrator kod kısıtlamaları](durable-functions-checkpointing-and-replay.md#orchestrator-code-constraints) bölümünü **denetim noktası oluşturma ve yeniden başlatma** makalesi.

## <a name="language-support"></a>Dil desteği

Şu anda C# yalnızca desteklenen dayanıklı işlevler için dilidir. Bu, orchestrator işlevlerini ve etkinlik işlevlerini içerir. Azure işlevlerini destekleyen tüm diller için destek gelecekte ekleyeceğiz. Azure işlevleri bkz [GitHub depo sorunlar listesine](https://github.com/Azure/azure-functions-durable-extension/issues) işi desteklemek bizim ek dil en son durumunu görmek için.

## <a name="monitoring-and-diagnostics"></a>İzleme ve tanılama

Dayanıklı işlevleri uzantısı için yapılandırılmış izleme verileri otomatik olarak yayar [Application Insights](functions-monitoring.md) işlev uygulaması bir Application Insights anahtarla yapılandırıldığında. Bu izleme verilerine davranışı ve, düzenlemelerin ilerlemesini izlemek için kullanılabilir.

İşte dayanıklı olaylarını izleme işlevleri gibi Application Insights portalı kullanarak benzediğini gösteren bir örnek [uygulama Öngörüler Analytics](https://docs.microsoft.com/azure/application-insights/app-insights-analytics):

![App Insights sorgu sonuçları](media/durable-functions-overview/app-insights-1.png)

İçine paketlenmiş yararlı yapılandırılmış verilerin çok `customDimensions` her günlük girişinin alanındaki. Tümüyle genişletilemiyor böyle bir giriş bir örneği burada verilmiştir.

![App Insights sorgu customDimensions alanı](media/durable-functions-overview/app-insights-2.png)

Dayanıklı görev Framework dağıtıcı yeniden yürütme davranışını nedeniyle yeniden yürütülmüş Eylemler yedekli günlük girişlerini görmek bekleyebilirsiniz. Bu, çekirdek altyapısı yeniden yürütme davranışını anlamak yararlı olabilir. [Tanılama](durable-functions-diagnostics.md) makale yalnızca "gerçek zamanlı" günlükleri görebilmeleri yeniden yürütme günlükleri filtreleyin örnek sorguları gösterir.

## <a name="storage-and-scalability"></a>Depolama ve ölçeklenebilirlik

Dayanıklı işlevleri uzantısı, yürütme geçmişi durumu ve tetikleyici işlevi yürütme kalıcı hale getirmek için Azure Storage kuyruklarını, tabloları ve blobları kullanır. İşlev uygulaması için varsayılan depolama hesabı kullanılabilir veya ayrı bir depolama hesabı yapılandırabilirsiniz. Depolama işleme sınırları nedeniyle ayrı bir hesap isteyebilirsiniz. Orchestrator kodları bu depolama hesaplarından varlıklarda etkileşimde gerekmez ve gereken değil. Varlıkları uygulama ayrıntı olarak doğrudan dayanıklı görev çerçevesi tarafından yönetilir.

Orchestrator işlevleri etkinlik işlevleri zamanlamak ve yanıtlarını aracılığıyla iç sıra iletileri alabilirsiniz. Bir işlev uygulaması Azure işlevleri tüketim planında çalıştığında, bu sıraların tarafından izlenen [Azure işlevlerini ölçeklendirme denetleyicisi](functions-scale.md#how-the-consumption-plan-works) ve yeni örnekleri gerektiğinde eklendiği işlem. Etkinlik işlevleri çağırır birkaç farklı sanal makinelerin çalıştırırken çıkışı için birden çok VM ölçeği, orchestrator işlevi bir VM üzerinde çalışabilir. Dayanıklı işlevlerin ölçek davranışı hakkında daha fazla ayrıntı bulabilirsiniz [performansı ve ölçeği](durable-functions-perf-and-scale.md).

Tablo depolama orchestrator hesaplar için yürütme geçmişini depolamak için kullanılır. Belirli bir VM örneği rehydrates olduğunda, böylece yerel durumuna yeniden oluşturabilirsiniz, yürütme geçmişiyle table storage'getirir. Tablo depolama alanında kullanılabilir geçmiş sahip ilgili kullanışlı özelliklerinden biri olduğundan göz atın ve gibi araçları kullanılarak, düzenlemelerin geçmişini görmek [Microsoft Azure Storage Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer).

![Azure Depolama Gezgini ekran görüntüsü](media/durable-functions-overview/storage-explorer.png)

> [!WARNING]
> Kolay ve tablo depolama yürütme geçmişini görmek uygun olmakla birlikte, bu tabloda herhangi bir bağımlılığı almaktan kaçının. Dayanıklı işlevleri uzantısı geliştikçe değişebilir.

## <a name="known-issues-and-faq"></a>Bilinen sorunlar ve SSS

Genel olarak, tüm bilinen sorunlar, izlenmesi gereken [GitHub sorunları](https://github.com/Azure/azure-functions-durable-extension/issues) listesi. Bir sorunla karşılaşırsanız ve sorunu Github'da bulunamıyor, yeni bir sorun açın ve ayrıntılı sorun açıklamasını içerir. Yalnızca bir soru sormak istiyorsanız, bile GitHub sorunu açın ve bir soru olarak etiketlemek çekinmeyin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevleri belgeleri okuma devam](durable-functions-bindings.md)

> [!div class="nextstepaction"]
> [Dayanıklı işlevleri uzantısı ve örnekleri yükleyin](durable-functions-install.md)

