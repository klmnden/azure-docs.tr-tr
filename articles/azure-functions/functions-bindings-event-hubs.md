---
title: "Azure işlevleri olay hub'ları bağlamaları | Microsoft Docs"
description: "Azure Event Hubs bağlamaları Azure işlevlerini kullanmak nasıl anlayın."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 06/20/2017
ms.author: wesmc
ms.openlocfilehash: 85eb6985ef3579b1b2313db3ce5f91c3471da72f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-event-hubs-bindings"></a>Azure işlevleri olay hub'ları bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırmak ve kullanmak açıklanmaktadır [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure işlevleri için bağlamaları.
Tetikler ve olay hub'ları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Event Hubs'a yeni istiyorsanız bkz [Event Hubs'a genel bakış](../event-hubs/event-hubs-what-is-event-hubs.md).

<a name="trigger"></a>

## <a name="event-hub-trigger"></a>Olay hub'ı tetikleyicisi
Bir olay hub'ı olay akışı gönderilen bir olayın yanıtlamak için olay hub'ları tetikleyici kullanın. Olay hub'ına tetikleyecek için okuma erişimi olmalıdır.

Olay hub'ları işlevi tetikleyici aşağıdaki JSON nesnesinde kullanan `bindings` function.json dizisi:

```json
{
    "type": "eventHubTrigger",
    "name": "<Name of trigger parameter in function signature>",
    "direction": "in",
    "path": "<Name of the event hub>",
    "consumerGroup": "Consumer group to use - see below",
    "connection": "<Name of app setting with connection string - see below>"
}
```

`consumerGroup`ayarlamak için kullanılan isteğe bağlı bir özellik [tüketici grubu](../event-hubs/event-hubs-features.md#event-consumers) hub olaylara abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır.  
`connection`Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı olması gerekir.
Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini *ad alanı*, olay hub kendisini değil. Bu bağlantı dizesi en az tetikleyici etkinleştirmek için Okuma izinleriniz olmalıdır.

[Ek ayarları](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) daha fazla olay hub'ları Tetikleyicileri ince için host.json dosyasında sağlanabilir.  

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Tetikleyici kullanımı
Bir olay hub'ları Tetik işlevi tetiklendiğinde tetikler ileti işlevdeki bir dize olarak geçirilir.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Tetikleyici örnek
Aşağıdaki olduğunu varsayalım, olay hub'ları tetiklemek `bindings` function.json dizisi:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

Olay hub'ı tetikleyicisi ileti gövdesini günlüklerini dile özgü örneğe bakın.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Tetikleyici örnek C# #

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Olayı olarak da alabileceğiniz bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) olay meta verilere erişmenizi nesnesi.

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Bir toplu işlemde olaylarını almak için yöntem imzası değiştirme `string[]` veya `EventData[]`.

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>F # tetikleyici örnek #

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Node.js tetikleyici örnek

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

<a name="output"></a>

## <a name="event-hubs-output-binding"></a>Olay hub'ları bağlama çıktı
Bir olay hub'ı olay akışına yazma olayları bağlama olay hub'ları çıkış kullanın. Yazma olayları için bir olay hub'ına gönderme izni olmalıdır.

Aşağıdaki JSON nesnesinde çıkış bağlama kullanır `bindings` function.json dizisi:

```json
{
    "type": "eventHub",
    "name": "<Name of output parameter in function signature>",
    "path": "<Name of event hub>",
    "connection": "<Name of app setting with connection string - see below>"
    "direction": "out"
}
```

`connection`Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı olması gerekir.
Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini *ad alanı*, olay hub kendisini değil. Bu bağlantı dizesi olay akışının ileti göndermek için Gönder izinleri olmalıdır.

## <a name="output-usage"></a>Çıktı kullanımı
Bu bölümde işlevi kodunuzda bağlama, olay hub'ları çıkış kullanmayı gösterir.

Aşağıdaki parametre türleri ile yapılandırılmış olay hub'ına iletileri çıkarabilirsiniz:

* `out string`
* `ICollector<string>`(birden çok ileti çıkışı için)
* `IAsyncCollector<string>`(zaman uyumsuz sürümü `ICollector<T>`)

<a name="outputsample"></a>

## <a name="output-sample"></a>Çıkış örneği
Aşağıdaki olduğunu varsayalım olay hub'ları bağlamasında çıktı `bindings` function.json dizisi:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Bir olay bile akışa Yazar dile özgü örneğine bakın.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>C# çıktı örneği #

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Veya birden çok iletileri oluşturmak için:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>F # çıktı örneği #

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

<a name="outnodejs"></a>

### <a name="output-sample-for-nodejs"></a>Node.js için çıktı örneği

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Veya birden çok ileti göndermek için

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Event Hub message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
