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
ms.openlocfilehash: 6577d4ae0f248ac234b2506a6adba04afde5ffce
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure işlevleri için Azure Event Hubs bağlamaları

Bu makale ile nasıl çalışılacağını açıklar [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure işlevleri için bağlamaları. Tetikler ve olay hub'ları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Tetikleyici

Bir olay hub'ı olay akışı gönderilen bir olayın yanıtlamak için olay hub'ları tetikleyici kullanın. Olay hub'ına tetikleyecek için okuma erişimi olmalıdır.

Bir olay hub'ları Tetik işlevi tetiklendiğinde tetikler ileti işlevdeki bir dize olarak geçirilir.

## <a name="trigger---scaling"></a>Tetiklemek - ölçeklendirme

Her bir örneğini Event Hub-Triggered işlevi yalnızca 1 EventProcessorHost (EPH) örneği tarafından desteklenir. Olay hub'ları yalnızca 1 EPH belli bir bölüm üzerinde bir kira elde etmenizi sağlar.

Örneğin, aşağıdaki Kurulum ve bir olay hub'ına yönelik tahminler başlamadan varsayın:

1. 10 bölüm.
1. 1000 olayları eşit olarak dağıtılmış tüm bölümler her bölüm > 100 iletilerinde =.

İlk işlevinizin etkinleştirildiğinde, da yalnızca 1 örneği işlevi yoktur. Şimdi bu işlevi örneğinin Function_0 çağırın. Function_0 üzerindeki tüm 10 bölüm kiralama almak için yönetir 1 EPH sahip olur. 0-9 bölümlerden olayları okumaya başlar. Bu noktadan itibaren aşağıdakilerden biri gerçekleşir:

* **Yalnızca 1 işlevi örneği gereklidir** -Function_0 Azure işlevlerini ölçeklendirme mantığı devreye girer önce tüm 1000 işleyebilir. Bu nedenle, tüm 1000 iletileri Function_0 tarafından işlenir.

* **Daha fazla 1 işlevi örneği ekleme** -Azure işlevlerini ölçeklendirme mantığı Function_1, yeni bir örnek oluşmasını Function_0 işleyebilmesi için daha fazla ileti olduğunu belirler. Olay hub'ları algılar iletileri okumak yeni bir EPH örneği çalışıyor. Olay hub'ları EPH örneklerinde bölümleri dengelemesini başlar, örn., bölümlerin 0-4 Function_0 için atanan ve bölümler 5-9 Function_1 için atanır. 

* **Ekleme N daha örnekleri işlev** -Azure işlevlerini ölçeklendirme mantığı Function_0 ve Function_1 işleyebilmesi için daha fazla ileti olduğunu belirler. Ayrıca, Function_2... N, N olay hub'ı paritions büyük olduğu için yeniden ölçeklenir. Olay hub'ları yükler bölümleri arasında Function_0 bakiye... 9 örnekleri.

Azure işlevleri geçerli mantığı ölçeklendirme N bölümleri sayısından büyüktür olgu benzersizdir. Bu, her zaman EPH diğer örneklerden çıktıklarında bir kilit üzerinde bölümler hızla almak kullanıma hazır örnekleri emin için gerçekleştirilir. Kullanıcılar yalnızca işlev örneğini yürütüldüğünde, kullanılan kaynaklar için sizden ücret ve bu aşırı sağlama için sizden ücret istenmese.

Tüm işlevi yürütmeleri hatasız başarılı olursa, kontrol noktaları ilişkili depolama hesabına eklenir. Onay işaret eden başarılı olduğunda, tüm 1000 iletileri hiçbir zaman yeniden alınması gerekir.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) olay hub'ı tetikleyicisi ileti gövdesini kaydeder.

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

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Özniteliğin Oluşturucusu olay hub'ın adı, tüketici grubu adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [tetiklemek yapılandırma bölümü](#trigger---configuration). Burada bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnection")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

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

## <a name="trigger---hostjson-properties"></a>Trigger - host.json properties

[Host.json](functions-host-json.md#eventhub) dosyası olay hub'ları tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıktı

Yazma için bir olay akışında olayları bağlama olay hub'ları çıkış kullanın. Yazma olayları için bir olay hub'ına gönderme izni olmalıdır.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , Yazar bir ileti yönteminin dönüş değeri çıkış olarak kullanan bir olay hub'ına:

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

İçin [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Özniteliğin Oluşturucusu olay hub'ı adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnection")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

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

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
