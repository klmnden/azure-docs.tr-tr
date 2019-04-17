---
title: Dayanıklı işlevler - Azure zamanlayıcılar
description: Dayanıklı zamanlayıcılar dayanıklı işlevler uzantısını Azure işlevleri için uygulamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/08/2018
ms.author: azfuncdf
ms.openlocfilehash: a05f75a7e38ee7cd4dc056629d9acaacad875e08
ms.sourcegitcommit: 5f348bf7d6cf8e074576c73055e17d7036982ddb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2019
ms.locfileid: "59608249"
---
# <a name="timers-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) zamanlayıcılar

[Dayanıklı işlevler](durable-functions-overview.md) sağlar *dayanıklı zamanlayıcılar* orchestrator işlevlerinde gecikmelere uygulamak veya zaman uyumsuz işlemleri zaman aşımları ayarlamak için kullanılacak. Dayanıklı zamanlayıcılar orchestrator işlevleri yerine kullanılmalıdır `Thread.Sleep` ve `Task.Delay` (C#) veya `setTimeout()` ve `setInterval()` (JavaScript).

Çağırarak dayanıklı bir zamanlayıcı oluşturma [CreateTimer](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CreateTimer_) yöntemi [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) . NET'te, veya `createTimer` yöntemi `DurableOrchestrationContext` JavaScript içinde. Yöntemi, belirtilen tarih ve saatini temel sürdürür bir görev döndürür.

## <a name="timer-limitations"></a>Zamanlayıcı sınırlamaları

4:30 pm sırasında temel alınan dayanıklı görev Framework kaybolmamasının yalnızca 4:30 pm görünür hale gelmesi bir ileti süresinin dolduğu Zamanlayıcı oluşturduğunuzda. Azure işlevleri tüketim planında çalıştırırken, işlev uygulaması uygun bir VM üzerinde etkin yeni görünür Zamanlayıcı iletiyi sağlayacaktır.

> [!NOTE]
> * Dayanıklı zamanlayıcılar en son Azure storage'da sınırlamaları nedeniyle, 7 günden daha uzun olamaz. Üzerinde çalışmakta olduğumuz bir [zamanlayıcılar 7 gün ötesine genişletmek için özellik isteği](https://github.com/Azure/azure-functions-durable-extension/issues/14).
> * Her zaman [CurrentUtcDateTime](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_CurrentUtcDateTime) yerine `DateTime.UtcNow` . NET'te ve `currentUtcDateTime` yerine `Date.now` veya `Date.UTC` dayanıklı bir Zamanlayıcının göreli bir son tarih ile ilgili işlem yapılırken örneklerde gösterildiği gibi JavaScript içinde .

## <a name="usage-for-delay"></a>Kullanım gecikme

Aşağıdaki örnek, yürütme geciktirmeye dayanıklı zamanlayıcılar kullanmayı gösterir. Örneğin, on gün için her gün bir fatura bildirimi veriyor.

### <a name="c"></a>C#

```csharp
[FunctionName("BillingIssuer")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    for (int i = 0; i < 10; i++)
    {
        DateTime deadline = context.CurrentUtcDateTime.Add(TimeSpan.FromDays(1));
        await context.CreateTimer(deadline, CancellationToken.None);
        await context.CallActivityAsync("SendBillingEvent");
    }
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    for (let i = 0; i < 10; i++) {
        const dayOfMonth = context.df.currentUtcDateTime.getDate();
        const deadline = moment.utc(context.df.currentUtcDateTime).add(1, 'd');
        yield context.df.createTimer(deadline.toDate());
        yield context.df.callActivity("SendBillingEvent");
    }
});
```

> [!WARNING]
> Sonsuz döngüler orchestrator işlevlerde kaçının. Güvenle ve etkili bir şekilde sonsuz döngü senaryoların nasıl uygulanacağına hakkında daha fazla bilgi için bkz. [dış düzenlemeler](durable-functions-eternal-orchestrations.md).

## <a name="usage-for-timeout"></a>Kullanım için zaman aşımı

Bu örnekte, dayanıklı zamanlayıcılar zaman aşımları uygulamak için nasıl kullanılacağı gösterilmektedir.

### <a name="c"></a>C#

```csharp
[FunctionName("TryGetQuote")]
public static async Task<bool> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    TimeSpan timeout = TimeSpan.FromSeconds(30);
    DateTime deadline = context.CurrentUtcDateTime.Add(timeout);

    using (var cts = new CancellationTokenSource())
    {
        Task activityTask = context.CallActivityAsync("GetQuote");
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

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```js
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    const deadline = moment.utc(context.df.currentUtcDateTime).add(30, "s");

    const activityTask = context.df.callActivity("GetQuote");
    const timeoutTask = context.df.createTimer(deadline.toDate());

    const winner = yield context.df.Task.any([activityTask, timeoutTask]);
    if (winner === activityTask) {
        // success case
        timeoutTask.cancel();
        return true;
    }
    else
    {
        // timeout case
        return false;
    }
});
```

> [!WARNING]
> Kullanım bir `CancellationTokenSource` dayanıklı Zamanlayıcı (C#) veya Çağrı iptal etmek için `cancel()` döndürülen üzerinde `TimerTask` (JavaScript), kodunuzu tamamlanmasını beklemeleri gerekir. Dayanıklı görev Framework "tamamlandı" bekleyen tüm görevlerin tamamlandığını veya iptal kadar düzenleme 's durumunu değiştirmez.

Bu mekanizma Süren etkinlik işlevi yürütme sonlanmamasına. Bunun yerine, yalnızca sonucunu yoksay ve geçmek için orchestrator işlevi sağlar. İşlev uygulamanızın bir tüketim planı kullanılıyorsa, her zaman ve terk edilmiş etkinlik işlevi tarafından kullanılan bellek hala faturalandırılırsınız. Varsayılan olarak, tüketim planında çalıştırmayı işlevleri beş dakikalık bir zaman aşımı vardır. Bu sınır aşılırsa tüm yürütmeyi durdurun ve kaçan bir fatura durumu önlemek için Azure işlevleri konak dönüştürülmeden. [İşlevi zaman aşımı yapılandırılabilir](../functions-host-json.md#functiontimeout).

Zaman aşımları orchestrator işlevlerini uygulamak nasıl daha ayrıntılı bir örnek için bkz: [insan etkileşimi & zaman aşımı - telefon doğrulama](durable-functions-phone-verification.md) gözden geçirme.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yükseltme ve dış olayları işleme hakkında bilgi edinin](durable-functions-external-events.md)
