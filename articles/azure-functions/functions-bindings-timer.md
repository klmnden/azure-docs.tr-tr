---
title: "Azure işlevleri Zamanlayıcı tetikleyicisi | Microsoft Docs"
description: "Azure işlevleri Zamanlayıcı Tetikleyicileri kullanmayı öğrenme."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: glenga
ms.custom: 
ms.openlocfilehash: 12beb090a95a31c7e83ae03a920016bdfbf474e3
ms.sourcegitcommit: c5eeb0c950a0ba35d0b0953f5d88d3be57960180
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/24/2017
---
# <a name="azure-functions-timer-trigger"></a>Azure işlevleri Zamanlayıcı tetikleyicisi

[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod Zamanlayıcı Tetikleyicileri Azure işlevleri açıklanmaktadır. Azure işlevleri tanımlanmış bir zamanlamaya göre işlevi kodunuzu çalıştırmak olanak sağlayan zamanlayıcı tetikleyicisi bağlama sahiptir. 

Zamanlayıcı tetikleyicisi çok örnekli genişleme destekler. Belirli Zamanlayıcı işlevi tek bir örneğini boyunca tüm örneklerde çalıştırılır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a id="trigger"></a>

## <a name="timer-trigger"></a>Zamanlayıcı tetikleyicisi
Aşağıdaki JSON nesnesinde bir işleve Zamanlayıcı tetikleyicisi kullanan `bindings` function.json dizisi:

```json
{
    "schedule": "<CRON expression - see below>",
    "name": "<Name of trigger parameter in function signature>",
    "type": "timerTrigger",
    "direction": "in"
}
```

Değeri `schedule` olan bir [CRON ifade](http://en.wikipedia.org/wiki/Cron#CRON_expression) altı bu alanları içerir: 

    {second} {minute} {hour} {day} {month} {day-of-week}
&nbsp;
>[!NOTE]   
>Birçok çevrimiçi Bul cron ifadelerin atlayın `{second}` alan. Bunlardan birini kopyalarsanız, ek için ayarlamanız gereken `{second}` alan. Belirli örnekler için bkz: [zamanlama örnekler](#examples) aşağıda.

CRON ifadelerle kullanılan varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir. CRON İfadenizde başka bir saat dilimini temel olmasını adlı işlev uygulamanız için yeni bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE`. İstenen saat dilimini adına gösterildiği gibi değeri [Microsoft saat dilimi dizin](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Örneğin, *Doğu Standart Saati* olan UTC-05:00. 10:00 AM EST her gün yangın tetikleyin, UTC saat dilimi hesapları aşağıdaki CRON deyimi kullanın, Zamanlayıcı sağlamak için:

```json
"schedule": "0 0 15 * * *",
``` 

Alternatif olarak, adlı işlev uygulamanız için yeni bir uygulama ayarı ekleyebilirsiniz `WEBSITE_TIME_ZONE` ve değerine **Doğu Standart Saati**.  Ardından aşağıdaki CRON ifade 10:00 AM EST için kullanılabilir: 

```json
"schedule": "0 0 10 * * *",
``` 


<a name="examples"></a>

## <a name="schedule-examples"></a>Zamanlama örnekleri
CRON ifadeler için kullanabileceğiniz bazı örnekleri şunlardır `schedule` özelliği. 

Her beş dakikada tetiklemek için:

```json
"schedule": "0 */5 * * * *"
```

Bir kez saatte en üstte tetiklemek için:

```json
"schedule": "0 0 * * * *",
```

Her iki saatte tetiklemek için:

```json
"schedule": "0 0 */2 * * *",
```

Saatte 09: 00'dan 18: 00 için tetiklemek için:

```json
"schedule": "0 0 9-17 * * *",
```

09:30:00 her gün tetiklemek için:

```json
"schedule": "0 30 9 * * *",
```

09:30:00 her hafta içi günü tetiklemek için:

```json
"schedule": "0 30 9 * * 1-5",
```

<a name="usage"></a>

## <a name="trigger-usage"></a>Tetikleyici kullanımı
Bir zamanlayıcı tetikleyicisi işlevi çağrıldığında, [Zamanlayıcı nesne](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) işlevdeki geçirilir. Aşağıdaki JSON timer nesnesi bir örnek gösterimidir. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00.012699+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

<a name="sample"></a>

## <a name="trigger-sample"></a>Tetikleyici örnek
Aşağıdaki Zamanlayıcı tetikleyicisi olduğunu varsayalım `bindings` function.json dizisi:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

Geç çalışıp çalışmadığını görmek için timer nesnesini okur dile özgü örneğe bakın.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Tetikleyici örnek C# #
```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}" );  
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>F # tetikleyici örnek #
```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Node.js tetikleyici örnek
```JavaScript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);   

    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

