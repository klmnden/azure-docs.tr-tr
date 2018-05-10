---
title: Dayanıklı işlevlerinde - Azure hataları işleme
description: Azure işlevleri için dayanıklı işlevleri uzantı hataları işlemek öğrenin.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/30/2018
ms.author: azfuncdf
ms.openlocfilehash: 944fab5ccc55bc9a697e870208338bd0e697672d
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı işlevlerinde (Azure işlevleri) hataları işleme

Dayanıklı işlevi düzenlemelerin kodda uygulanır ve programlama dili hata işleme özelliklerini kullanabilirsiniz. Bu durum dikkate alınarak, gerçekten yok herhangi yeni kavram olduğunda hata işleme ve maaş, düzenlemelerin ekleme hakkında bilgi almanız gerekir. Ancak, farkında olmanız gereken birkaç davranışları vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevleri hataları

Bir etkinlik işlevinde oluşturulan özel durumları orchestrator işlevine sıraya ve olarak oluşturulan bir `FunctionFailedException`. Orchestrator işlevinde gereksinimlerinizi karşılayacak hata işleme ve Dengeleme kodu yazabilirsiniz.

Örneğin, bir hesaptan fon aktaran aşağıdaki orchestrator işlevi göz önünde bulundurun:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static async Task Run(DurableOrchestrationContext context)
{
    var transferDetails = ctx.GetInput<TransferOperation>();

    await context.CallActivityAsync("DebitAccount",
        new
        { 
            Account = transferDetails.SourceAccount,
            Amount = transferDetails.Amount
        });

    try
    {
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.DestinationAccount,
                Amount = transferDetails.Amount
            });
    }
    catch (Exception)
    {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        await context.CallActivityAsync("CreditAccount",         
            new
            { 
                Account = transferDetails.SourceAccount,
                Amount = transferDetails.Amount
            });
    }
}
```

Varsa çağrısı **CreditAccount** işlevi hedef hesabı için başarısız olursa, orchestrator işlevi bu kaynak hesap fon'alacak kaydetmeye dengeler.

## <a name="automatic-retry-on-failure"></a>Hata durumunda otomatik yeniden dene

Ne zaman etkinlik işlevleri çağırmak veya alt orchestration işlevleri otomatik belirtebilirsiniz İlkesi yeniden deneyin. Aşağıdaki örnek 3 kereye kadar bir işlevi çağırmak çalışır ve denemeler arasındaki 5 saniye bekler:

```csharp
public static async Task Run(DurableOrchestrationContext context)
{
    var retryOptions = new RetryOptions(
        firstRetryInterval: TimeSpan.FromSeconds(5),
        maxNumberOfAttempts: 3);

    await ctx.CallActivityWithRetryAsync("FlakyFunction", retryOptions, null);
    
    // ...
}
```

`CallActivityWithRetryAsync` API alır bir `RetryOptions` parametresi. Alt orchestration çağırır kullanarak `CallSubOrchestratorWithRetryAsync` API aynı bu yeniden deneme ilkelerini kullanabilirsiniz.

Otomatik yeniden deneme ilkesi özelleştirmek için birkaç seçeneğiniz vardır. Bu ülkelere şunlar dahildir:

* **Deneme sayısı üst sınırı**: en fazla yeniden deneme sayısı.
* **İlk yeniden deneme aralığı**: ilk yeniden denemeden önce beklenecek süre.
* **Geri Çekilme katsayısı**: geri Çekilme çıktığını oranını belirlemek için kullanılan katsayısı. Varsayılan olarak 1.
* **En fazla yeniden deneme aralığı**: yeniden denemeler arasında beklenecek en fazla süreyi.
* **Zaman aşımı yeniden deneme**: Bunu yapmak için harcamanız en uzun süreyi yeniden dener. Sonsuza kadar yeniden denemeye varsayılan davranıştır.
* **Özel**: kullanıcı tanımlı bir geri çağırma işlevi çağrısı denenen olup olmadığını belirleyen belirtilebilir.

## <a name="function-timeouts"></a>İşlev zaman aşımları

Bir işlev çağrısı bir orchestrator işlevi içinde tamamlanması çok uzun sürüyorsa abandon isteyebilirsiniz. Bugün bunun için en uygun oluşturarak yoludur bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) kullanarak `context.CreateTimer` birlikte `Task.WhenAny`, aşağıdaki örnekte olduğu gibi:

```csharp
public static async Task<bool> Run(DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("FlakyFunction");
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

> [!NOTE]
> Bu mekanizma Süren etkinlik işlevi yürütme gerçekten sonlandırmak değil. Bunun yerine, sonuç yoksayıp geçmek orchestrator işlevi yalnızca sağlar. Bkz: [zamanlayıcılar](durable-functions-timers.md#usage-for-timeout) daha fazla bilgi için.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Bir orchestrator işlevi işlenmeyen bir özel durum ile başarısız olursa, özel durum ayrıntıları kaydedilir ve ile örnek tamamlandığında bir `Failed` durumu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sorunları tanılayın öğrenin](durable-functions-diagnostics.md)
