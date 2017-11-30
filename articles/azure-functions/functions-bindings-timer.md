---
title: "Zamanlayıcı tetikleyicisi için Azure işlevleri"
description: "Azure işlevleri Zamanlayıcı Tetikleyicileri kullanmayı öğrenme."
services: functions
documentationcenter: na
author: tdykstra
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
ms.author: tdykstra
ms.custom: 
ms.openlocfilehash: fd9c1d40ba1398c7ca3f48f0423457482da9a483
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="timer-trigger-for-azure-functions"></a>Zamanlayıcı tetikleyicisi için Azure işlevleri 

Bu makalede Azure işlevleri Tetikleyicileri Zamanlayıcı ile nasıl çalışılacağını açıklar. Zamanlayıcı tetikleyicisi bir işlev bir zamanlamaya göre çalışmasını sağlar. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [Önceden derlenmiş C#](#trigger---c-example)
* [C# betiği](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi önceden derlenmiş](functions-dotnet-class-library.md) , beş dakikada bir çalışır:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
```

### <a name="c-script-example"></a>C# kod örneği

Aşağıdaki örnek, bağlama Zamanlayıcı tetikleyicisi gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev, bu işlev çağrısını kaçırılan zamanlama oluşması nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

C# betik kod aşağıdaki gibidir:

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

### <a name="f-example"></a>F # örnek

Aşağıdaki örnek, bağlama Zamanlayıcı tetikleyicisi gösterir bir *function.json* dosyası ve bir [F # betik işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev, bu işlev çağrısını kaçırılan zamanlama oluşması nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

F # betik kod aşağıdaki gibidir:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter ) =
    if (myTimer.IsPastDue) then
        log.Info("F# function is running late.")
    let now = DateTime.Now.ToLongTimeString()
    log.Info(sprintf "F# function executed at %s!" now)
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bağlama Zamanlayıcı tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev, bu işlev çağrısını kaçırılan zamanlama oluşması nedeniyle olup olmadığını belirten bir günlüğe yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "schedule": "0 */5 * * * *",
    "name": "myTimer",
    "type": "timerTrigger",
    "direction": "in"
}
```

JavaScript kodu şöyledir:

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

## <a name="attributes"></a>Öznitelikler

İçin [C# önceden derlenmiş](functions-dotnet-class-library.md) işlevlerini kullanmak [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs), NuGet paketi tanımlı [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions).

Aşağıdaki örnekte gösterildiği gibi özniteliğin Oluşturucusu CRON ifade alır:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
   ...
}
 ```

Belirleyebileceğiniz bir `TimeSpan` işlevi uygulamanıza bir uygulama hizmeti planı (tüketim plan değil) çalıştırıyorsa CRON ifade yerine.

Tam bir örnek için bkz: [önceden derlenmiş C# örnek](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `TimerTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | "TimerTrigger" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | "İçin" ayarlanması gerekir. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | İşlev kodu Zamanlayıcı nesneyi temsil eden değişken adı. | 
|**zamanlama**|**ScheduleExpression**|Tüketim plan üzerinde CRON ifade ile zamanlama tanımlayabilirsiniz. Bir uygulama hizmeti planı kullanıyorsanız, ayrıca kullanabileceğiniz bir `TimeSpan` dize. Aşağıdaki bölümlerde CRON ifadeler açıklanmaktadır. Bir uygulama ayarı zamanlama ifadeyi ve bu özellik sarmalanmış bir değere ayarlayın  **%**  Bu örnekte olduğu gibi işaretlerini: "% NameOfAppSettingWithCRONExpression %". |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

### <a name="cron-format"></a>CRON biçimi 

A [CRON ifade](http://en.wikipedia.org/wiki/Cron#CRON_expression) için Azure işlevleri Zamanlayıcı tetikleyicisi bu altı alanları içerir: 

```
{second} {minute} {hour} {day} {month} {day-of-week}
```

>[!NOTE]   
>Birçok çevrimiçi Bul CRON ifadelerin atlayın `{second}` alan. Bunlardan birini kopyalarsanız, eksik Ekle `{second}` alan.

### <a name="cron-time-zones"></a>CRON saat dilimleri

CRON ifadelerle kullanılan varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir. CRON İfadenizde başka bir saat dilimini temel olmasını adlı işlev uygulamanız için yeni bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE`. İstenen saat dilimini adına gösterildiği gibi değeri [Microsoft saat dilimi dizin](https://technet.microsoft.com/library/cc749073(v=ws.10).aspx). 

Örneğin, *Doğu Standart Saati* olan UTC-05:00. 10:00 AM EST her gün yangın tetikleyin, UTC saat dilimi hesapları aşağıdaki CRON deyimi kullanın, Zamanlayıcı sağlamak için:

```json
"schedule": "0 0 15 * * *",
``` 

Alternatif olarak, adlı işlev uygulamanız için yeni bir uygulama ayarı ekleyebilirsiniz `WEBSITE_TIME_ZONE` ve değerine **Doğu Standart Saati**.  Ardından aşağıdaki CRON ifade 10:00 AM EST için kullanılabilir: 

```json
"schedule": "0 0 10 * * *",
``` 
### <a name="cron-examples"></a>CRON örnekleri

CRON ifadeleri Azure işlevlerinde Zamanlayıcı tetikleyicisi için kullanabileceğiniz bazı örnekleri aşağıda verilmiştir. 

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

## <a name="usage"></a>Kullanım

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

## <a name="scale-out"></a>Ölçeklendirme

Zamanlayıcı tetikleyicisi çok örnekli genişleme destekler. Belirli Zamanlayıcı işlevi tek bir örneğini boyunca tüm örneklerde çalıştırılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Zamanlayıcı tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
