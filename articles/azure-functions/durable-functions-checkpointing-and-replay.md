---
title: "Kontrol noktalarına ve yeniden yürütme dayanıklı işlevlerinde - Azure"
description: "Denetim noktası oluşturma ve yanıt işleyişi dayanıklı işlevleri uzantısı'nda Azure işlevleri için öğrenin."
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
ms.openlocfilehash: b1bca62e256c1ede5df6888dd7c47ce2aa816bb9
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="checkpoints-and-replay-in-durable-functions-azure-functions"></a>Kontrol noktalarına ve yeniden yürütme dayanıklı işlevlerinde (Azure işlevleri)

Dayanıklı işlevlerin en önemli özelliklerinden biridir **güvenilir yürütme**. Orchestrator işlevler ve etkinlik işlevleri bir veri merkezi içinde farklı vm'lerinde çalışıyor olabilir ve bu sanal makineleri veya arka plandaki ağ altyapısı % 100 güvenilir değil.

Bu tüm dayanıklı işlevleri düzenlemelerin güvenilir bir biçimde yürütülmesini sağlar. Bunu sürücü işlev çağrısını depolama sıralara kullanarak ve düzenli aralıklarla denetim noktası oluşturma yürütme geçmişini depolama tablolara yapar (olarak bilinen bir bulut tasarım modeli kullanarak [olay kaynak Hizmeti'nden](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing)). Bu geçmiş sonra otomatik olarak bir orchestrator işlevi bellek içi durumunu yeniden oluşturma çalınabilir.

## <a name="orchestration-history"></a>Orchestration geçmişi

Aşağıdaki orchestrator işlevi olduğunu varsayalım.

```csharp
[FunctionName("E1_HelloSequence")]
public static async Task<List<string>> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    var outputs = new List<string>();

    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Tokyo"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "Seattle"));
    outputs.Add(await context.CallActivityAsync<string>("E1_SayHello", "London"));

    // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
    return outputs;
}
```

Her `await` deyimi, dayanıklı görev Framework kontrol noktalarını tablo depolama alanına işlevi yürütme durumu. Ne olarak adlandırılmıştır bu durumda *orchestration geçmişi*.

## <a name="history-table"></a>Geçmiş tablosu

Genel olarak bakıldığında, dayanıklı görev Framework her noktasında şunları yapar:

1. Yürütme geçmişini Azure Storage tablolara kaydeder.
2. Enqueues iletileri işlevler için orchestrator çağırma istiyor.
3. Orchestrator için Enqueues iletileri &mdash; Örneğin, dayanıklı Zamanlayıcı iletileri.

Denetim noktası işlemi tamamlandıktan sonra bunu yapmak daha fazla çalışma kadar bellekten kaldırılacak orchestrator işlevi ücretsizdir.

> [!NOTE]
> Azure depolama, veri kaydetme tablo depolama ve Kuyruklar arasında işlem garanti sağlamaz. Hataları işlemek için dayanıklı işlevleri depolama sağlayıcısını kullanır *nihai tutarlılık* desenleri. Bu düzenleri çökmesi ya da bir denetim noktası ortasında bağlantı kaybı ise hiçbir veri kaybı olduğundan emin olun.

Tamamlandığında, daha önce gösterilen işlevi geçmişini aşağıdakine benzer Azure Table Storage'nın (gösterim amacıyla kısaltılmış) arar:

| PartitionKey (InstanceId)                     | Olay türü             | Zaman damgası               | Girdi | Ad             | Sonuç                                                    | Durum | 
|----------------------------------|-----------------------|----------|--------------------------|-------|------------------|-----------------------------------------------------------|---------------------| 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:32.362Z |       |                  |                                                           |                     | 
| eaee885b | ExecutionStarted      | 2017-05-05T18:45:28.852Z | null   | E1_HelloSequence |                                                           |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:32.670Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:32.670Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.232Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.201Z |       |                  | "" "Merhaba Tokyo!" ""                                        |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.435Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.435Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.763Z |       |                  | "" "Merhaba Seattle!" ""                                      |                     | 
| eaee885b | TaskScheduled         | 2017-05-05T18:45:34.857Z |       | E1_SayHello      |                                                           |                     | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:34.857Z |       |                  |                                                           |                     | 
| eaee885b | OrchestratorStarted   | 2017-05-05T18:45:35.032Z |       |                  |                                                           |                     | 
| eaee885b | TaskCompleted         | 2017-05-05T18:45:34.919Z |       |                  | "" "Merhaba Londra!" ""                                       |                     | 
| eaee885b | ExecutionCompleted    | 2017-05-05T18:45:35.044Z |       |                  | "[""Tokyo! Hello" ifadesini",""Merhaba Seattle!" ",""Merhaba Londra!" "]" | Tamamlandı           | 
| eaee885b | OrchestratorCompleted | 2017-05-05T18:45:35.044Z |       |                  |                                                           |                     | 

Sütun değerleri birkaç Notlar:
* **PartitionKey**: orchestration örnek Kimliğini içerir.
* **EventType**: olay türünü temsil eder. Aşağıdaki türlerden biri olabilir:
    * **OrchestrationStarted**: orchestrator işlevi ilk kez çalıştırma veya bir bekleme sürdürülemez. `Timestamp` Sütunu belirleyici değerini doldurmak için kullanılan [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) API.
    * **ExecutionStarted**: ilk kez çalıştırma orchestrator işlevi başlatıldı. Bu olay işlevi girişinde de içeren `Input` sütun.
    * **TaskScheduled**: bir etkinlik işlevi zamanlandı. Etkinlik işlevin adını, yakalanan `Name` sütun.
    * **TaskCompleted**: tamamlanmış bir etkinlik işlevi. İşlevin sonucu olarak `Result` sütun.
    * **TimerCreated**: dayanıklı süreölçer oluşturuldu. `FireAt` Sütun süreölçer süresinin dolma zamanlanmış UTC saati içerir.
    * **TimerFired**: dayanıklı süreölçer tetiklenir.
    * **EventRaised**: bir dış olay orchestration örneğine gönderildi. `Name` Sütunu olayın adı yakalar ve `Input` sütun olay yükü yakalar.
    * **OrchestratorCompleted**: beklemenin orchestrator işlevi.
    * **ContinueAsNew**: orchestrator işlevi tamamlandı ve kendisini yeni durumuyla yeniden. `Result` Sütun giriş yeniden örneği olarak kullanılan değer içeriyor.
    * **ExecutionCompleted**: orchestrator işlevi tamamlanıncaya kadar çalıştıran (veya başarısız oldu). İşlev veya hata ayrıntılarını çıkışları depolanmış `Result` sütun.
* **Zaman damgası**: geçmiş olayın UTC zaman damgası.
* **Ad**: çağrıldı işlevin adı.
* **Giriş**: işlevinin giriş JSON biçimli.
* **Sonuç**: işlevi çıktısını; diğer bir deyişle, kendi dönüş değeri.

> [!WARNING]
> Hata ayıklama aracı olarak yararlı olsa da, herhangi bir bağımlılığı bu tabloda etkili olmaz. Dayanıklı işlevleri uzantısı geliştikçe değişebilir.

Gelen işlevin sürdürür her zaman bir `await`, dayanıklı görev Framework orchestrator işlevi en baştan yeniden çalıştırır. Geçerli zaman uyumsuz işlemi sürdü olup olmadığını belirlemek için yürütme geçmişi danışır her yeniden çalıştır üzerinde yerleştirin.  Framework işlemi gerçekleşen, bu işlemin çıktısı hemen başlayarak yeniden oynatılır ve diğerine geçer `await`. Bu işlem, tüm geçmişi, önceki değerlerine geri hangi noktada orchestrator işlevde tüm yerel değişkenler yeniden kadar devam eder.

## <a name="orchestrator-code-constraints"></a>Orchestrator kod kısıtlamaları

Yeniden yürütme davranışı bir orchestrator işlevinde yazılan kod türünü kısıtlamalar oluşturur:

* Orchestrator kodu olmalıdır **belirleyici**. Birden çok kez yeniden ve her zaman aynı sonucu gerekir. Örneğin, geçerli tarih/saat almak, rastgele sayılar almak, rastgele GUID'ler oluşturmak veya uzak uç noktalarına çağırmak için hiçbir doğrudan çağırır.

  Orchestrator kodu geçerli tarih/saat almak gerekirse, kullanması gereken [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) yeniden yürütme için güvenlidir API.

  Etkinlik işlevlerde belirleyici olmayan işlemleri yapılması gerekir. Bu, diğer giriş veya çıkış bağlamaları ile etkileşimi içerir. Bu, belirleyici olmayan tüm değerleri kez ilk yürütme sırasında oluşturulur ve yürütme geçmişini kaydedilmiş olduğunu sağlar. Sonraki yürütmelerde sonra kaydedilen değeri otomatik olarak kullanır.

* Orchestrator kodu olmalıdır **engellemeyen**. Örneğin, yani hiçbir g/ç ve hiçbir çağrıları `Thread.Sleep` veya eşdeğer API'leri.

  Bir orchestrator gecikme gerekiyorsa kullanabilirsiniz [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) API.

* Orchestrator kod gerekir **hiçbir zaman herhangi bir zaman uyumsuz işlemi başlatmak** dışındaki kullanarak [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) API. Örneğin, Hayır `Task.Run`, `Task.Delay` veya `HttpClient.SendAsync`. Dayanıklı görev Framework tek bir iş parçacığı üzerinde orchestrator kodu yürütür ve diğer zaman uyumsuz API tarafından zamanlanabilir diğer tüm iş parçacıkları ile etkileşime giremezler.

* **Sonsuz döngüler kaçınılmalıdır** orchestrator kod. Dayanıklı görev Framework orchestration işlevi ilerledikçe yürütme geçmişini kaydettiğinden, sonsuz bir döngüde belleğin tükenmek üzere orchestrator örneği neden olabilir. Sonsuz bir döngü senaryoları için API'leri gibi kullandığınız [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) işlevi yürütme yeniden başlatın ve önceki yürütme geçmişini atmak için.

Bu kısıtlamaların önce izlemek sabit olmayan uygulamada göz korkutucu adresindeki görünebilir ancak. Dayanıklı görev Framework yukarıdaki kurallarını ihlal algılamaya çalışır ve oluşturur bir `NonDeterministicOrchestrationException`. Ancak, en yüksek çaba bu algılama davranıştır ve bağımlı döndürmemelidir.

> [!NOTE]
> Bu kuralların hepsi sadece tarafından tetiklenen işlevleri uygulamak `orchestrationTrigger` bağlama. Etkinlik işlevleri tetiklenen tarafından `activityTrigger` bağlama ve kullanan işlevler `orchestrationClient` bağlama, hiçbir tür sınırlamaları vardır.

## <a name="durable-tasks"></a>Dayanıklı görevleri

> [!NOTE]
> Bu bölümde dayanıklı görev Framework'ün iç uygulama ayrıntıları açıklanmaktadır. Bu bilgiler bilmeden dayanıklı işlevlerini kullanabilirsiniz. Yalnızca yeniden yürütme davranışı anlamanıza yardımcı olmak için tasarlanmıştır.

Güvenli bir şekilde orchestrator işlevlerde beklemenin görevler zaman zaman denir *dayanıklı görevleri*. Bu oluşturulan ve dayanıklı görev çerçevesi tarafından yönetilen görevlerdir. Örnekler tarafından döndürülen görevleri `CallActivityAsync`, `WaitForExternalEvent`, ve `CreateTimer`.

Bunlar *dayanıklı görevleri* dahili listesi kullanılarak yönetilen `TaskCompletionSource` nesneleri. Yeniden yürütme sırasında bu görevleri orchestrator kod yürütmeyi bir parçası olarak oluşturulan ve dağıtıcı ilgili geçmiş olaylar numaralandırır olarak tamamlandı. Bu tüm tüm geçmişi yeniden kadar zaman uyumlu olarak tek bir iş parçacığı kullanılarak gerçekleştirilir. Geçmiş yeniden yürütme ucu tarafından tamamlanmamış dayanıklı görevleri gerçekleştirilmesi uygun eylemler vardır. Örneğin, bir ileti bir etkinlik işlevi çağırmak için sıraya alınan olabilir.

Burada açıklanan yürütme davranışını neden orchestrator işlev kodu asla gerekir anlamanıza yardımcı olması `await` dayanıklı olmayan göreve: dağıtıcı iş parçacığı tamamlanmasını bekleyin ve görev tarafından herhangi bir geri çağırma olası izleme bozuk olabilir orchestrator işlevi durumu. Bazı çalışma zamanı denetimleri Bunu önlemek denemek için verilmiştir.

Nasıl dayanıklı görev Framework orchestrator işlevlerini yürüten hakkında daha fazla bilgi isterseniz, yapmak için en iyi incelediğinizden şeydir [dayanıklı görev kaynak kodu github'da](https://github.com/Azure/durabletask). Özellikle, bkz: [TaskOrchestrationExecutor.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationExecutor.cs) ve [TaskOrchestrationContext.cs](https://github.com/Azure/durabletask/blob/master/src/DurableTask.Core/TaskOrchestrationContext.cs)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek yönetimi hakkında bilgi edinin](durable-functions-instance-management.md)
