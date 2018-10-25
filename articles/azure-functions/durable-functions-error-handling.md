---
title: Dayanıklı işlevler - Azure, hataları işleme
description: Dayanıklı işlevler uzantısını hataları işlemek için Azure işlevleri hakkında bilgi edinin.
services: functions
author: kashimiz
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/23/2018
ms.author: azfuncdf
ms.openlocfilehash: 61496d91c9ec2cd1dcf498df04d2dab6629e009c
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984137"
---
# <a name="handling-errors-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) hataları işleme

Dayanıklı işlevi düzenlemeleri kodda uygulanır ve hata işleme özelliklerini programlama dilini kullanabilirsiniz. Bunu aklınızda gerçekten yok hata işleme ve maaş, düzenlemeleri ekleme yaparken öğrenmek için gereken tüm yeni kavramları. Ancak, bilmeniz gereken birkaç davranışları vardır.

## <a name="errors-in-activity-functions"></a>Etkinlik işlevlerini hataları

Bir etkinlik işlevinde oluşturulan herhangi bir özel durum orchestrator işleve sıraya ve olarak oluşturulan bir `FunctionFailedException`. Orchestrator işlevindeki gereksinimlerinize uygun hata işleme ve Dengeleme kodu yazabilirsiniz.

Örneğin, bir hesaptan para aktarımı aşağıdaki orchestrator işlevi göz önünde bulundurun:

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const transferDetails = context.df.getInput();

    yield context.df.callActivity("DebitAccount",
        {
            account = transferDetails.sourceAccount,
            amount = transferDetails.amount,
        }
    );

    try {
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.destinationAccount,
                amount = transferDetails.amount,
            }
        );
    }
    catch (error) {
        // Refund the source account.
        // Another try/catch could be used here based on the needs of the application.
        yield context.df.callActivity("CreditAccount",
            {
                account = transferDetails.sourceAccount,
                amount = transferDetails.amount,
            }
        );
    }
});
```

Çağrı **CreditAccount** işlevi hedef hesabı için başarısız olursa, orchestrator işlevi bunun için kaynak hesap fon alacak kaydetme dengeler.

## <a name="automatic-retry-on-failure"></a>Hata durumunda otomatik yeniden deneme

Etkinlik işlevlerini veya alt düzenleme işlevler çağırdığınızda, otomatik yeniden deneme ilkesi belirtebilirsiniz. Aşağıdaki örnek, en fazla üç kez bir işlevi çağırmak çalışır ve her yeniden deneme arasındaki 5 saniye bekler:

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const retryOptions = new df.RetryOptions(5000, 3);
    
    yield context.df.callActivityWithRetry("FlakyFunction", retryOptions);

    // ...
});
```

`CallActivityWithRetryAsync` (C#) Veya `callActivityWithRetry` (JS) API alır bir `RetryOptions` parametresi. Suborchestration çağrıları kullanarak `CallSubOrchestratorWithRetryAsync` (C#) veya `callSubOrchestratorWithRetry` (JS) API'si aynı bu yeniden deneme ilkelerini kullanabilirsiniz.

Otomatik yeniden deneme ilkesi özelleştirmek için birkaç seçenek vardır. Bu ülkelere şunlar dahildir:

* **En fazla deneme sayısı**: yeniden deneme sayısı.
* **İlk yeniden deneme aralığı**: ilk yeniden denemeden önce beklenecek süre.
* **Geri alma katsayısı**: geri alma sayısında artış oranını belirlemek için kullanılan katsayısı. Varsayılan olarak 1.
* **En fazla yeniden deneme aralığı**: yeniden denemeler arasında beklenecek en uzun süreyi.
* **Yeniden deneme zaman aşımının**: harcama yapmak için en uzun süreyi yeniden dener. Süresiz olarak yeniden denemek için varsayılan davranıştır.
* **Tanıtıcı**: bir işlev çağrısı denenen olup olmadığını belirleyen kullanıcı tanımlı bir geri çağırma belirtilebilir.

## <a name="function-timeouts"></a>İşlevi zaman aşımları

Bir düzenleyici işlevi içinde bir işlev çağrısının tamamlanması çok uzun sürüyorsa bırakmasını isteyebilirsiniz. Bugün Bunu yapmak için uygun yolu oluşturmaktır bir [dayanıklı Zamanlayıcı](durable-functions-timers.md) kullanarak `context.CreateTimer` birlikte `Task.WhenAny`, aşağıdaki örnekte olduğu gibi:

#### <a name="c"></a>C#

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("FlakyFunction");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    } else {
        // timeout case
        return false;
    }
});
```

> [!NOTE]
> Bu mekanizma Süren etkinlik işlevi yürütme sonlanmamasına. Bunun yerine, yalnızca sonucunu yoksay ve geçmek için orchestrator işlevi sağlar. Daha fazla bilgi için [zamanlayıcılar](durable-functions-timers.md#usage-for-timeout) belgeleri.

## <a name="unhandled-exceptions"></a>İşlenmeyen özel durumlar

Bir düzenleyici işlevi işlenmeyen bir özel durum ile başarısız olursa özel durumun ayrıntılarını günlüğe kaydedilir ve örneği ile tamamlanan bir `Failed` durumu.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sorunları tanılamayı öğrenin](durable-functions-diagnostics.md)
