---
title: Zamanlayıcı tetikleyicisi için Azure işlevleri
description: Azure işlevleri Zamanlayıcı Tetikleyicileri kullanmayı öğrenme.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.assetid: d2f013d1-f458-42ae-baf8-1810138118ac
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/27/2017
ms.author: tdykstra
ms.custom: ''
ms.openlocfilehash: a8844ea44bf604944c5980b0d41ab5d01a30b876
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="timer-trigger-for-azure-functions"></a>Zamanlayıcı tetikleyicisi için Azure işlevleri 

Bu makalede Azure işlevleri Tetikleyicileri Zamanlayıcı ile nasıl çalışılacağını açıklar. Zamanlayıcı tetikleyicisi bir işlev bir zamanlamaya göre çalışmasını sağlar. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Paketler

Zamanlayıcı tetikleyicisi sağlanan [Microsoft.Azure.WebJobs.Extensions](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions) NuGet paketi. Paket için kaynak kodunu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/) GitHub depo.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-package-versions](../../includes/functions-package-versions.md)]

## <a name="example"></a>Örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="c-example"></a>C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , beş dakikada bir çalışır:

```cs
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if(myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
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

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [TimerTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerTriggerAttribute.cs).

Özniteliğin Oluşturucusu CRON ifade alır veya `TimeSpan`. Kullanabileceğiniz `TimeSpan` yalnızca işlev uygulaması bir uygulama hizmeti plan üzerinde çalışıyorsa. Aşağıdaki örnek, bir CRON ifadesi gösterir:

```csharp
[FunctionName("TimerTriggerCSharp")]
public static void Run([TimerTrigger("0 */5 * * * *")]TimerInfo myTimer, TraceWriter log)
{
    if (myTimer.IsPastDue)
    {
        log.Info("Timer is running late!");
    }
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
}
 ```

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `TimerTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "TimerTrigger" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | yok | "İçin" ayarlanması gerekir. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodu Zamanlayıcı nesneyi temsil eden değişken adı. | 
|**schedule**|**ScheduleExpression**|A [CRON ifade](#cron-expressions) veya [TimeSpan](#timespan) değeri. A `TimeSpan` bir uygulama hizmeti planı üzerinde çalışan bir işlev uygulaması için kullanılabilir. Bir uygulama ayarı zamanlama ifadeyi ve ayar adı sarmalanmış uygulama için bu özelliği ayarlamak **%** Bu örnekte olduğu gibi işaretlerini: "% ScheduleAppSetting %". |
|**runOnStartup**|**RunOnStartup**|Varsa `true`, çalışma zamanı başladığında işlevi çağrılır. Eylemsizlik nedeniyle boşta geçtikten sonra işlev uygulaması uyanır Örneğin, çalışma zamanı başlar. ne zaman işlev uygulaması, işlev değişiklikleri nedeniyle ve çıkışı işlev uygulaması ölçeklendirir zaman yeniden başlatır. Bu nedenle **runOnStartup** olursa, nadiren herhangi bir zamanda ayarlanması gerektiğini `true`, yüksek oranda beklenmeyen zamanlarda yürütme kodu yapacak şekilde.|
|**UseMonitor**|**UseMonitor**|Kümesine `true` veya `false` zamanlama izlenen olup olmadığını belirtmek için. İzleme zamanlaması bile işlevi app örnekleri yeniden başlattığınızda zamanlama doğru yönetilmesini sağlamak için yardımcı olmak için zamanlama yinelenme devam ettirir. Açıkça ayarlanmazsa varsayılan olup olmadığını `true` yinelenme aralığı 1 dakikadan daha uzun olan tabloları için. Dakika başına birden çok kez tetiklemek zamanlamaları, varsayılan değer `false`.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="usage"></a>Kullanım

Bir zamanlayıcı tetikleyicisi işlevi çağrıldığında, [Zamanlayıcı nesne](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions/Extensions/Timers/TimerInfo.cs) işlevdeki geçirilir. Aşağıdaki JSON timer nesnesi bir örnek gösterimidir. 

```json
{
    "Schedule":{
    },
    "ScheduleStatus": {
        "Last":"2016-10-04T10:15:00+00:00",
        "LastUpdated":"2016-10-04T10:16:00+00:00",
        "Next":"2016-10-04T10:20:00+00:00"
    },
    "IsPastDue":false
}
```

`IsPastDue` Özelliği `true` geçerli işlev çağrısını olduğunda zamanlanmış zamanından sonra. Örneğin, bir işlev uygulamayı yeniden eksik bir çağrı neden olabilir.

## <a name="cron-expressions"></a>CRON ifadeleri 

Azure işlevleri Zamanlayıcı tetikleyicisi için CRON ifadesi altı alanları içerir: 

`{second} {minute} {hour} {day} {month} {day-of-week}`

Her bir alan değerleri aşağıdaki türlerden biri olabilir:

|Tür  |Örnek  |Tetiklendiğinde  |
|---------|---------|---------|
|Belirli bir değer |<nobr>"0 5 * * * *"</nobr>|hh saatte (saatte bir kez) olduğu hh:05:00|
|Tüm değerleri (`*`)|<nobr>"0 * 5 * * *"</nobr>|5:mm adresindeki: her gün 00 mm dakikada (60 günde kez) saat olduğu|
|Bir aralığı (`-` işleci)|<nobr>"5-7 * * * * *"</nobr>|hh:mm:05, hh:mm:06 ve SS: dd saatte (3 kez dakika) dakikada olduğu hh:mm:07|  
|Değer kümesini (`,` işleci)|<nobr>"5,8,10 * * * * *"</nobr>|hh:mm:05, hh:mm:08 ve SS: dd saatte (3 kez dakika) dakikada olduğu hh:mm:10|
|Bir aralık değeri (`/` işleci)|<nobr>"0 */5 * * * *"</nobr>|hh:05:00, hh:10:00, vb. hh saatte (12 kat bir saat) olduğu hh:55:00 aracılığıyla hh:15:00|

### <a name="cron-examples"></a>CRON örnekleri

CRON ifadeleri Azure işlevlerinde Zamanlayıcı tetikleyicisi için kullanabileceğiniz bazı örnekleri aşağıda verilmiştir.

|Örnek|Tetiklendiğinde  |
|---------|---------|
|"0 */5 * * * *"|her beş dakikada|
|"0 0 * * * *"|bir kez saatte en üstünde|
|"0 0 */2 * * *"|Her iki saatte|
|"0 0 9-17 ** *"|saatte 09: 00'dan 18: 00 için|
|"0 30 9 * * *"|09:30:00 her gün|
|"0 30 9 ** 1-5"|09:30:00 her hafta içi günü|

>[!NOTE]   
>Çevrimiçi CRON ifade örnekleri bulabilirsiniz, ancak bunların kaç atlayın `{second}` alan. Bunlardan birini kopyalarsanız, eksik Ekle `{second}` alan. Genellikle bu alanda bir yıldız sıfır isteyeceksiniz.

### <a name="cron-time-zones"></a>CRON saat dilimleri

CRON ifade sayıları bir saat ve tarih, zaman aralığı bakın. Örneğin, 5'te `hour` alanı 5:00 arasında her 5 saat başvurur.

CRON ifadelerle kullanılan varsayılan saat dilimini Eşgüdümlü Evrensel Saat (UTC) ' dir. CRON İfadenizde başka bir saat dilimini temel olmasını adlı işlev uygulamanız için bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE`. İstenen saat dilimini adına gösterildiği gibi değeri [Microsoft saat dilimi dizin](https://technet.microsoft.com/library/cc749073). 

Örneğin, *Doğu Standart Saati* olan UTC-05:00. 10:00 AM EST her gün yangın tetikleyin, UTC saat dilimi hesapları aşağıdaki CRON deyimi kullanın, Zamanlayıcı sağlamak için:

```json
"schedule": "0 0 15 * * *",
``` 

Veya adlı işlev uygulamanız için bir uygulama ayarı oluşturmak `WEBSITE_TIME_ZONE` ve değerine **Doğu Standart Saati**.  Ardından aşağıdaki CRON ifade kullanır: 

```json
"schedule": "0 0 10 * * *",
``` 

## <a name="timespan"></a>TimeSpan

 A `TimeSpan` bir uygulama hizmeti planı üzerinde çalışan bir işlev uygulaması için kullanılabilir.

CRON ifade aksine bir `TimeSpan` değeri her işlev çağrısını arasındaki zaman aralığını belirtir. Bir işlev belirtilen aralığından daha uzun çalıştırdıktan sonra tamamlandığında Zamanlayıcı hemen işlevi yeniden çağırır.

Bir dize olarak ifade edilen `TimeSpan` biçimi `hh:mm:ss` zaman `hh` değerinden 24 arasıdır. İlk iki basamak 24 veya daha büyük olduğunda biçimidir `dd:hh:mm`. İşte bazı örnekler:

|Örnek |Tetiklendiğinde  |
|---------|---------|
|"01:00:00" | her saat        |
|"00:01:00"|dakikada         |
|"24:00:00" | 24 her gün        |

## <a name="scale-out"></a>Ölçeklendirme

Birden çok örneklerine giden bir işlev uygulaması ölçeklendirir ise Zamanlayıcı tetiklenen işlevi yalnızca tek bir örneğini boyunca tüm örneklerde çalıştırılır.

## <a name="function-apps-sharing-storage"></a>Depolama paylaşımı işlevi uygulamaları

Bir depolama hesabı birden çok işlev uygulama arasında paylaşıyorsanız, her işlev uygulaması farklı bir sahip olduğundan emin olun `id` içinde *host.json*. Atlayabilirsiniz `id` özelliği veya elle her işlevi uygulamanın `id` için farklı bir değer. Zamanlayıcı tetikleyicisi depolama kilidi olacağını yalnızca bir zamanlayıcı örneğini birden çok örneklerine giden bir işlev uygulaması ölçeklendirir olduğunda emin olmak için kullanır. İki işlev uygulamalarının aynı paylaşıyorsanız `id` ve her bir zamanlayıcı tetikleyicisi kullanan, yalnızca bir zamanlayıcı çalışır.

## <a name="retry-behavior"></a>Yeniden deneme davranışını

Bir işlev başarısız olduktan sonra sıra tetikleyici Zamanlayıcı tetikleyicisi yeniden değil. Bir işlev başarısız olduğunda, onu is't zamanlamaya göre yeniden açana kadar çağrılır.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Zamanlayıcı tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-scheduled-function.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
