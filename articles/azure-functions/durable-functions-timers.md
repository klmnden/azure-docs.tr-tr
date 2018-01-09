---
title: "Dayanıklı işlevlerinde - Azure zamanlayıcılar"
description: "Dayanıklı zamanlayıcılar Azure işlevleri için dayanıklı işlevleri uzantısı'nda uygulama hakkında bilgi edinin."
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
ms.openlocfilehash: e29e472860890e3f44af79c42c31ff524acb9276
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="timers-in-durable-functions-azure-functions"></a>Dayanıklı işlevlerinde (Azure işlevleri) zamanlayıcılar

[Dayanıklı işlevleri](durable-functions-overview.md) sağlar *dayanıklı zamanlayıcılar* orchestrator işlevlerinde gecikmelere uygulamak için veya zaman aşımları zaman uyumsuz eylemleri ayarlamak için kullanılacak. Dayanıklı zamanlayıcılar orchestrator işlevleri yerine kullanılmalıdır `Thread.Sleep` veya `Task.Delay`.

Çağırarak dayanıklı Zamanlayıcı oluşturma [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) yönteminde [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html). Yöntemi, belirtilen tarih ve saat sürdürür bir görev döndürür.

## <a name="timer-limitations"></a>Zamanlayıcı sınırlamaları

Yalnızca 4:30 şöyle görünür hale bir ileti 4:30 şöyle sırasında temel alınan dayanıklı görev Framework enqueues süresi bir süreölçer oluşturduğunuzda. Azure işlevleri tüketim planında çalıştırırken, yeni görünür süreölçer iletisi işlev uygulaması uygun bir VM üzerinde etkinleştirilmiş güvence altına alır.

> [!WARNING]
> * Dayanıklı zamanlayıcılar en son Azure storage'da sınırlamaları nedeniyle 7 günden daha uzun olamaz. Üzerinde çalıştığımız bir [zamanlayıcılar 7 gün ötesine genişletmek için özellik isteği](https://github.com/Azure/azure-functions-durable-extension/issues/14).
> * Her zaman kullanmak [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) yerine `DateTime.UtcNow` dayanıklı Süreölçerde göreli bir son tarih hesaplanırken örnekleri gösterildiği gibi.

## <a name="usage-for-delay"></a>Gecikme için kullanım

Aşağıdaki örnek dayanıklı zamanlayıcılar yürütme geciktirme için kullanma gösterilmektedir. Örnek bir fatura bildirim on gün boyunca her gün işleniyor.

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallFunctionAsync("SendBillingEvent");
    }
}
```

> [!WARNING]
> Sonsuz döngüler orchestrator işlevlerinde kaçının. Güvenli ve verimli bir şekilde sonsuz bir döngü senaryolarını uygulayan hakkında daha fazla bilgi için bkz: [Eternal düzenlemelerin](durable-functions-eternal-orchestrations.md). 

## <a name="usage-for-timeout"></a>Zaman aşımı için kullanım

Bu örnek dayanıklı zamanlayıcılar zaman aşımları uygulamak için nasıl kullanılacağı gösterilmektedir.

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallFunctionAsync("GetQuote");
        Task timeoutTask = context.CreateTimer(deadline, cts.Token);

        Task winner = await Task.WhenAny(activityTask, timeoutTask);
        if (winner == activityTask)
        {
            // success case
            cts.Cancel();
            return true;
        }
        else
        {
            // timeout case
            return false;
        }
    }
}
```

> [!WARNING]
> Kullanım bir `CancellationTokenSource` kodunuzu tamamlanmasını bekler değil, sağlam bir süreölçer iptal etmek için. Dayanıklı görev Framework "tamamlandı" Tüm Bekleyen Görevler tamamlandı veya iptal kadar bir orchestration's durumu değiştirmez.

Bu mekanizma Süren etkinlik işlevi yürütme gerçekten sonlandırmak değil. Bunun yerine, sonuç yoksayıp geçmek orchestrator işlevi yalnızca sağlar. İşlev uygulamanız tüketim planı kullanıyorsa, hala herhangi bir zaman ve terk edilmiş etkinlik işlevi tarafından kullanılan bellek için Fatura edilecek. Varsayılan olarak, beş dakikalık bir zaman aşımı tüketim planında çalıştıran işlevleri sahiptir. Bu sınır aşılırsa, tüm yürütme durdurup kaçak fatura durumu önlemek için Azure işlevleri konak dönüştürülmeden. [İşlevi zaman aşımı yapılandırılabilir](functions-host-json.md#functiontimeout).

Zaman aşımları orchestrator işlevlerde uygulamak nasıl daha ayrıntılı bir örnek için bkz: [insan etkileşimi & zaman aşımları - telefonu doğrulama](durable-functions-phone-verification.md) gözden geçirme.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yükseltmek ve dış olayları işlemek hakkında bilgi edinin](durable-functions-external-events.md)

