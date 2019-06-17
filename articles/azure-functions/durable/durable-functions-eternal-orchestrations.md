---
title: Dayanıklı işlevler - Azure, dış düzenlemeler
description: Dış düzenlemeler, Azure işlevleri için dayanıklı işlevler uzantısını kullanarak uygulamayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 99eabf3bc91887ff19b3a0bc9cf6647d32fa6750
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65787559"
---
# <a name="eternal-orchestrations-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) içinde dış düzenlemeler

*Dış düzenlemeler* hiçbir zaman son orchestrator işlevlerdir. Kullanmak istediğiniz zaman yararlı oldukları [dayanıklı işlevler](durable-functions-overview.md) toplayıcısını ve sonsuz bir döngüye gerektiren herhangi bir senaryo için.

## <a name="orchestration-history"></a>Orchestration geçmişi

İçinde anlatıldığı gibi [denetim noktası oluşturma ve yeniden yürütme](durable-functions-checkpointing-and-replay.md), dayanıklı görev Framework her işlev düzenleme geçmişini izler. Bu geçmiş, yeni iş zamanlamak orchestrator işlevi devam sürekli olarak sürece büyür. Orchestrator işlevi bir sonsuz döngüye giriyor ve sürekli işleri zamanlar, bu geçmiş oldukça büyük büyütün ve önemli performans sorunlarına neden olabilir. *Dış düzenleme* kavramı, bu tür sonsuz döngüler gerek duyan uygulamalar için sorunları gidermek için tasarlanmıştır.

## <a name="resetting-and-restarting"></a>Sıfırlama ve yeniden başlatma

Sonsuz döngüler kullanmak yerine, orchestrator işlevleri durumlarına çağırarak sıfırlama [ContinueAsNew](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_ContinueAsNew_) yöntemi. Bu yöntem, yeni giriş için orchestrator işlevi nesil olur tek bir JSON seri hale getirilebilir parametre alır.

Zaman `ContinueAsNew` çağrılırsa örneği kaybolmamasının kendisine çıkana önce bir ileti. İleti, yeni giriş değerine sahip örnek yeniden başlatır. Aynı örnek Kimliğine tutulur, ancak orchestrator işlevin geçmişi etkili bir şekilde kesildi.

> [!NOTE]
> Dayanıklı görev Framework aynı örnek Kimliğine tutar ancak dahili olarak yeni bir oluşturur *yürütme kimliği* tarafından sıfırlanır orchestrator işlevi için `ContinueAsNew`. Bu yürütme kimliği genellikle harici olarak gösterilmez, ancak orchestration yürütme hata ayıklaması hakkında bilmek de yararlı olabilir.

## <a name="periodic-work-example"></a>Periyodik iş örneği

Dış düzenlemeler için bir kullanım örneği süresiz olarak düzenli iş yapması gereken kod değil.

### <a name="c"></a>C#

```csharp
[FunctionName("Periodic_Cleanup_Loop")]
public static async Task Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    await context.CallActivityAsync("DoCleanup");

    // sleep for one hour between cleanups
    DateTime nextCleanup = context.CurrentUtcDateTime.AddHours(1);
    await context.CreateTimer(nextCleanup, CancellationToken.None);

    context.ContinueAsNew(null);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    yield context.df.callActivity("DoCleanup");

    // sleep for one hour between cleanups
    const nextCleanup = moment.utc(context.df.currentUtcDateTime).add(1, "h");
    yield context.df.createTimer(nextCleanup.toDate());

    context.df.continueAsNew(undefined);
});
```

Bu örnek ve Zamanlayıcı ile tetiklenen bir işlev arasındaki fark, bir zamanlamaya göre temizleme tetikleyici burada kez dayalı olmayan ' dir. Örneğin, saatte bir işlevi yürüten bir CRON zamanlama, 1: 00'da, 2:00, 3:00 yürütecek vb. ve çakışma bir sorunla karşılaşırsanız potansiyel olarak çalışabilir. Temizleme 30 dakika sürer, bu örnekte, ancak daha sonra 1: 00'da, 2:30, zamanlanacak 4:00, vb. ve hiçbir çakışma olasılığı yoktur.

## <a name="exit-from-an-eternal-orchestration"></a>Dış düzenleme Çık

Bir düzenleyici işlevi sonunda tamamlaması gereken sonra yapmanız gereken tek şey *değil* çağrı `ContinueAsNew` ve çıkış işlevi sağlar.

Bir düzenleyici işlevi sonsuz bir döngüye ve durdurulması gerekir, kullanın [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) durdurmak için yöntemi. Daha fazla bilgi için [örnek Yönetimi](durable-functions-instance-management.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Tekil düzenlemeler uygulamayı öğrenin](durable-functions-singletons.md)
