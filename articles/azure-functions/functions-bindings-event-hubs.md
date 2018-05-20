---
title: Azure işlevleri için Azure Event Hubs bağlamaları
description: Azure Event Hubs bağlamaları Azure işlevlerini kullanmak nasıl anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 8516f6d1f598e79bcb47922f02926f75c328861b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2018
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure işlevleri için Azure Event Hubs bağlamaları

Bu makale ile nasıl çalışılacağını açıklar [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) Azure işlevleri için bağlamaları. Tetikler ve olay hub'ları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Paketler

Azure işlevleri sürümü için 1.x, olay hub'ları bağlamaları içinde verilmiştir [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi. İşlevler için 2.x, kullanım [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) paket. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/) GitHub depo.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

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

* **Ekleme N daha örnekleri işlev** -Azure işlevlerini ölçeklendirme mantığı Function_0 ve Function_1 işleyebilmesi için daha fazla ileti olduğunu belirler. Ayrıca, Function_2... N, N olay hub'ı bölümleri büyük olduğu için yeniden ölçeklenir. Olay hub'ları yükler bölümleri arasında Function_0 bakiye... 9 örnekleri.

Azure işlevleri geçerli mantığı ölçeklendirme N bölümleri sayısından büyüktür olgu benzersizdir. Bu, her zaman EPH diğer örneklerden çıktıklarında bir kilit üzerinde bölümler hızla almak kullanıma hazır örnekleri emin için gerçekleştirilir. Kullanıcılar yalnızca işlev örneğini yürütüldüğünde, kullanılan kaynaklar için sizden ücret ve bu aşırı sağlama için sizden ücret istenmese.

Tüm işlevi yürütmeleri hatasız başarılı olursa, kontrol noktaları ilişkili depolama hesabına eklenir. Onay işaret eden başarılı olduğunda, tüm 1000 iletileri hiçbir zaman yeniden alınması gerekir.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) olay hub'ı tetikleyicisi ileti gövdesini kaydeder.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Erişmek için [olay meta verisini](#trigger---event-metadata) işlev kodu bağlamak bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (kullanarak bir gerektirir bildirimi `Microsoft.ServiceBus.Messaging`). Aynı özellikler yöntemi imzada bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı veri almak için her iki yönde gösterir:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage, 
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
}
```

Bir toplu işlemde olaylarını almak için olun `string` veya `EventData` bir dizi:

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string[] eventHubMessages, TraceWriter log)
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
  "connection": "myEventHubReadConnectionAppSetting"
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

Erişmek için [olay meta verisini](#trigger---event-metadata) işlev kodu bağlamak bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (kullanarak bir gerektirir bildirimi `Microsoft.ServiceBus.Messaging`). Aynı özellikler yöntemi imzada bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı veri almak için her iki yönde gösterir:

```cs
#r "Microsoft.ServiceBus"
using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc, 
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.GetBytes())}");
    // Metadata accessed by binding to EventData
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.Offset}");
    // Metadata accessed by using binding expressions
    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"SequenceNumber={sequenceNumber}");
    log.Info($"Offset={offset}");
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
  "connection": "myEventHubReadConnectionAppSetting"
}
```

F # kod aşağıdaki gibidir:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama olay hub'ı tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev okur [olay meta verisini](#trigger---event-metadata) ve iletiyi günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventHubMessage) {
    context.log('Event Hubs trigger function processed message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);
     
    context.done();
};
```

Bir toplu işlemde olayları alacak şekilde ayarlanmış `cardinality` için `many` içinde *function.json* dosyası:


```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "path": "MyEventHub",
  "cardinality": "many",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventHubMessages) {
    context.log(`JavaScript eventhub trigger function called for message array ${eventHubMessages}`);
    
    eventHubMessages.forEach(message => {
        context.log(`Processed message ${message}`);
    });

    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubTriggerAttribute.cs) özniteliği.

Özniteliğin Oluşturucusu olay hub'ın adı, tüketici grubu adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [tetiklemek yapılandırma bölümü](#trigger---configuration). Burada bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `EventHubTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | Geçerli Değil | ayarlanmalıdır `eventHubTrigger`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | Geçerli Değil | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**Adı** | Geçerli Değil | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**Yol** |**EventHubName** | Olay hub'ın adı. | 
|**consumerGroup** |**ConsumerGroup** | Ayarlar isteğe bağlı bir özellik [tüketici grubu](../event-hubs/event-hubs-features.md#event-consumers) hub olaylara abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır. | 
|**Önem düzeyi** | Geçerli Değil | JavaScript için. Kümesine `many` toplu işleme etkinleştirmek için.  Atlanmış veya kümesine `one`, tek bir ileti işlevine geçirilen. | 
|**Bağlantı** |**Bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini [ad alanı](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), olay hub kendisini değil. Bu bağlantı dizesi en az tetikleyici etkinleştirmek için Okuma izinleriniz olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Tetikleyici - olay meta verileri

Olay hub'ları tetikleyici birkaç sağlar [meta veri özelliklerini](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Bu özellikler, diğer bağlamaları bağlama ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Özelliklerini bunlar [EventData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.servicebus.messaging.eventdata) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|`PartitionContext` Örneği.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan zamanı, UTC.|
|`Offset`|`string`|Olay hub'ı bölüm akış göre veri uzaklığı. İşaretleyici veya Event Hubs akışındaki olay tanımlayıcısı uzaklığı. Bir olay hub'ları akış bölüm içinde benzersiz tanımlayıcısıdır.|
|`PartitionKey`|`string`|Hangi olay veri gönderilmesi gereken bölüm.|
|`Properties`|`IDictionary<String,Object>`|Olay verileri kullanıcı özellikleri.|
|`SequenceNumber`|`Int64`|Olay mantıksal sıra sayısı.|
|`SystemProperties`|`IDictionary<String,Object>`|Olay verileri de dahil olmak üzere Sistem Özellikleri.|

Bkz: [kod örnekleri](#trigger---example) bu makalede daha önce bu özellikleri kullanın.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#eventhub) dosyası olay hub'ları tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıkış

Yazma için bir olay akışında olayları bağlama olay hub'ları çıkış kullanın. Yazma olayları için bir olay hub'ına gönderme izni olmalıdır.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , Yazar bir ileti yönteminin dönüş değeri çıkış olarak kullanan bir olay hub'ına:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
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
    "connection": "MyEventHubSendAppSetting",
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
    "connection": "MyEventHubSendAppSetting",
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
    "connection": "MyEventHubSendAppSetting",
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

İçin [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) özniteliği.

Özniteliğin Oluşturucusu olay hub'ı adını ve bağlantı dizesi içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
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
|**type** | Geçerli Değil | "EventHub" olarak ayarlanmalıdır. |
|**direction** | Geçerli Değil | Out"için" olarak ayarlanmalıdır. Bu parametre, Azure portalında bağlama oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | Geçerli Değil | Olay temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**Yol** |**EventHubName** | Olay hub'ın adı. | 
|**Bağlantı** |**Bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğmesini *ad alanı*, olay hub kendisini değil. Bu bağlantı dizesi olay akışının ileti göndermek için Gönder izinleri olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Yöntem parametresi gibi kullanarak, C# ve C# betik iletileri göndermek `out string paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Birden çok ileti yazmak için kullanabileceğiniz `ICollector<string>` veya `IAsyncCollector<string>` yerine `out string`.

JavaScript'te, çıktı olayını kullanarak erişim `context.bindings.<name>`. `<name>` değeri belirtilen `name` özelliği *function.json*.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
