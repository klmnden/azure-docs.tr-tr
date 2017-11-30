---
title: "Azure işlevleri için Azure Event Hubs bağlamaları"
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
ms.date: 11/08/2017
ms.author: wesmc
ms.openlocfilehash: 70219ada2f4886f40d088486063afda2bc489611
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure işlevleri için Azure Event Hubs bağlamaları

Bu makale ile nasıl çalışılacağını açıklar [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure işlevleri için bağlamaları. Tetikler ve olay hub'ları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Tetikleyici

Bir olay hub'ı olay akışı gönderilen bir olayın yanıtlamak için olay hub'ları tetikleyici kullanın. Olay hub'ına tetikleyecek için okuma erişimi olmalıdır.

Bir olay hub'ları Tetik işlevi tetiklendiğinde tetikler ileti işlevdeki bir dize olarak geçirilir.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [Önceden derlenmiş C#](#trigger---c-example)
* [C# betiği](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği [C# önceden derlenmiş](functions-dotnet-class-library.md) olay hub'ı tetikleyicisi ileti gövdesini oturum kodu.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Olay meta verilerine erişmek için bağlamak bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (gerektiren bir `using` bildirimi `Microsoft.ServiceBus.Messaging`).

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```
Bir toplu işlemde olaylarını almak için olun `string` veya `EventData` bir dizi:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev olay hub'ı tetikleyicisi ileti gövdesini günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```
C# betik kod aşağıdaki gibidir:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Olay meta verilerine erişmek için bağlamak bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (kullanarak bir gerektirir bildirimi `Microsoft.ServiceBus.Messaging`).

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage, TraceWriter log)
{
    log.Info($"{Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
}
```

Bir toplu işlemde olaylarını almak için olun `string` veya `EventData` bir dizi:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F # örnek

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev olay hub'ı tetikleyicisi ileti gövdesini günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

F # kod aşağıdaki gibidir:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev olay hub'ı tetikleyicisi ileti gövdesini günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionString"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, myEventHubMessage) {
    context.log('Node.js eventhub trigger function processed work item', myEventHubMessage);    
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçin [C# önceden derlenmiş](functions-dotnet-class-library.md) işlevlerini kullanmak [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Özniteliğin Oluşturucusu olay hub'ın adı, tüketici grubu adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [tetiklemek yapılandırma bölümü](#trigger---configuration). Burada bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [tetikleyici - önceden derlenmiş C# örnek](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `EventHubTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | ayarlanmalıdır `eventHubTrigger`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**yol** |**EventHubName** | Olay hub'ın adı. | 
|**consumerGroup** |**ConsumerGroup** | Ayarlar isteğe bağlı bir özellik [tüketici grubu](../event-hubs/event-hubs-features.md#event-consumers) hub olaylara abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır. | 
|**bağlantı** |**Bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini *ad alanı*, olay hub kendisini değil. Bu bağlantı dizesi en az tetikleyici etkinleştirmek için Okuma izinleriniz olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#eventhub) dosyası olay hub'ları tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıktı

Yazma için bir olay akışında olayları bağlama olay hub'ları çıkış kullanın. Yazma olayları için bir olay hub'ına gönderme izni olmalıdır.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [Önceden derlenmiş C#](#output---c-example)
* [C# betiği](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi önceden derlenmiş](functions-dotnet-class-library.md) , Yazar bir ileti yönteminin dönüş değeri çıkış olarak kullanan bir olay hub'ına:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev olay hub'ına bir ileti yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Bir ileti oluşturan bir C# kodu şöyledir:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Birden çok ileti oluşturan burada'nın C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıktı - F # örnek

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev olay hub'ına bir ileti yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

F # kod aşağıdaki gibidir:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev olay hub'ına bir ileti yazar.

Veri bağlama işte *function.json* dosyası:

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSend",
    "direction": "out"
}
```

Tek bir ileti gönderir burada'nın JavaScript kodu:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Birden çok ileti gönderir burada'nın JavaScript kodu:

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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçin [C# önceden derlenmiş](functions-dotnet-class-library.md) işlevlerini kullanmak [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Özniteliğin Oluşturucusu olay hub'ı adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - önceden derlenmiş C# örnek](#output---c-example).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `EventHub` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | "EventHub" olarak ayarlanmalıdır. |
|**yönü** | yok | Out"için" olarak ayarlanmalıdır. Bu parametre, Azure portalında bağlama oluşturduğunuzda otomatik olarak ayarlanır. |
|**adı** | yok | Olay temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**yol** |**EventHubName** | Olay hub'ın adı. | 
|**bağlantı** |**Bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini *ad alanı*, olay hub kendisini değil. Bu bağlantı dizesi olay akışının ileti göndermek için Gönder izinleri olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Yöntem parametresi gibi kullanarak, C# ve C# betik iletileri göndermek `out string paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Birden çok ileti yazmak için kullanabileceğiniz `ICollector<string>` veya `IAsyncCollector<string>` yerine `out string`.

JavaScript'te, çıktı olayını kullanarak erişim `context.bindings.<name>`. `<name>`değeri belirtilen `name` özelliği *function.json*.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
