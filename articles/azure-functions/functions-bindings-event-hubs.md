---
title: Azure işlevleri için Azure Event Hubs bağlamaları
description: Azure işlevleri'nde Azure Event Hubs bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/08/2017
ms.author: tdykstra
ms.openlocfilehash: 51f64f6f74875c6afac350dc9cc235573b89c524
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2018
ms.locfileid: "38989597"
---
# <a name="azure-event-hubs-bindings-for-azure-functions"></a>Azure işlevleri için Azure Event Hubs bağlamaları

Bu makalede ile nasıl çalışılacağı açıklanmaktadır [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) bağlamalar için Azure işlevleri. Tetikleme ve çıkış bağlamaları Event Hubs için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Azure işlevleri sürüm için 1.x, Event Hubs bağlamaları altında sağlanmıştır [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi sürüm 2.x.
Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs) GitHub deposu.


[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

İşlevler için kullanım 2.x [Microsoft.Azure.WebJobs.Extensions.EventHubs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs) paketi, sürüm 3.x.
Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Event Hubs tetikleyici için bir olay hub'ı olay akışı gönderilen olaya yanıt vermek için kullanın. Olay hub'ına tetikleyicinin ayarlamak için okuma erişimi olmalıdır.

Bir olay hub'ları tetikleyici işlevi tetiklendiğinde tetikleyen iletinin dize olarak işleve geçirilir.

## <a name="trigger---scaling"></a>Tetikleme - ölçeklendirme

Her bir örneğini Event Hub-Triggered işlevi yalnızca 1 EventProcessorHost (EPH) örneği tarafından desteklenir. Olay hub'ları sağlar yalnızca 1 EPH belli bir bölüm üzerinde bir kira elde edebilirsiniz.

Örneğin, aşağıdaki Kurulum ve bir olay hub'ına yönelik varsayımlarda başlamadan varsayalım:

1. 10 bölümler.
1. 1000 olayları eşit olarak dağıtılmış = > 100 iletileri her bölümde tüm bölümler.

İlk işlevinizi etkin olduğunda da yalnızca 1 örneğinin işlevi yoktur. Bu işlev örneği Function_0 adlandıralım. Tüm 10 bölümleri üzerinde bir kira almak için yöneten 1 EPH Function_0 olacaktır. 0-9 bölümlerden olayları okumaya başlar. Bu noktadan itibaren aşağıdakilerden biri gerçekleşir:

* **Yalnızca 1 işlevi örneği gereklidir** -Function_0 Azure işlevlerini ölçeklendirme mantıksal devreye girer önce tüm 1000 işleyebilir. Bu nedenle, tüm 1000 iletileri Function_0 tarafından işlenir.

* **1 daha fazla işlev örneği ekleme** -Azure işlevlerini ölçeklendirme mantıksal Function_1, yeni bir örneği oluşmasını Function_0, işleyebileceğinden daha fazla ileti olduğunu belirler. Olay hub'ları algılar iletileri okumak yeni EPH örneği çalışıyor. Event hubs'ı bölümleri EPH örneği arasında Yük Dengeleme başlar, örneğin, bölümlerin 0-4 Function_0 için atanan ve bölüm 5-9 Function_1 için atanır. 

* **Ekle N fazla işlev örnekleri** -Azure işlevlerini ölçeklendirme mantıksal Function_0 hem Function_1 bunlar işleyebileceğinden daha fazla ileti olduğunu belirler. Function_2... N, N olay hub'ı bölümleri büyük olduğu için yeniden ölçeklendirilir. Event hubs'ı yükler bölümler arasında Function_0 bakiye... 9 örnekleri.

Azure işlevleri geçerli mantıksal ölçeklendirme N bölüm sayısından daha büyük hale benzersizdir. Bu, her zaman diğer örneklerden kullanılabilir oldukça bölümleri üzerinde hızlı bir şekilde bir kilidi almak hazır EPH örneklerini emin olmak için gerçekleştirilir. Kullanıcılar yalnızca işlev örneği yürütüldüğünde, kullanılan kaynaklar için ücret alınır ve bu fazladan sağlama için ücretlendirilmez.

Tüm işlev yürütmelerinin hata olmadan başarılı olursa, kontrol noktaları ilişkili depolama hesabına eklenir. Onay işaret eden başarılı olduğunda, tüm 1000 iletileri hiçbir zaman yeniden alınmalıdır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , günlükleri Olay hub'ı tetikleyicisi ileti gövdesi.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Erişim elde etmek için [olay meta verilerinin](#trigger---event-metadata) işlevi kod içinde bağlama bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (bir kullanılması deyimi için `Microsoft.ServiceBus.Messaging`). Aynı özellikleri Yöntem imzasında bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almak için her iki yönde gösterir:

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

Bir toplu işte olayları almak, olun `string` veya `EventData` dizisi:

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

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

C# betik kodunu şu şekildedir:

```cs
using System;

public static void Run(string myEventHubMessage, TraceWriter log)
{
    log.Info($"C# Event Hub trigger function processed a message: {myEventHubMessage}");
}
```

Erişim elde etmek için [olay meta verilerinin](#trigger---event-metadata) işlevi kod içinde bağlama bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (bir kullanılması deyimi için `Microsoft.ServiceBus.Messaging`). Aynı özellikleri Yöntem imzasında bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almak için her iki yönde gösterir:

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

Bir toplu işte olayları almak, olun `string` veya `EventData` dizisi:

```cs
public static void Run(string[] eventHubMessages, TraceWriter log)
{
    foreach (var message in eventHubMessages)
    {
        log.Info($"C# Event Hub trigger function processed a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F # örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

F # kodu şu şekildedir:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Info(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev okur [olay meta verilerinin](#trigger---event-metadata) ve iletiyi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
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

Bir toplu işte olayları almak için ayarlanmış `cardinality` için `many` içinde *function.json* aşağıdaki örneklerde gösterildiği gibi dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

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
```json
{
  "type": "eventHubTrigger",
  "name": "eventHubMessages",
  "direction": "in",
  "eventHubName": "MyEventHub",
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

Özniteliğin oluşturucusu, olay hub'ı adı, tüketici grubu adını ve bağlantı dizesini içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [tetikleyecek yapılandırma bölümü](#trigger---configuration). İşte bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `EventHubTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `eventHubTrigger`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodunu olay öğeyi temsil eden değişken adı. | 
|**Yolu** |**EventHubName** | 1.x yalnızca çalışır. Olay hub'ının adı.  | 
|**eventHubName** |**EventHubName** | 2.x yalnızca çalışır. Olay hub'ının adı.  |
|**consumerGroup** |**ConsumerGroup** | Ayarlar isteğe bağlı bir özellik [tüketici grubu](../event-hubs/event-hubs-features.md#event-consumers) hub'ındaki olayları abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır. | 
|**önem düzeyi** | yok | JavaScript için. Kümesine `many` toplu işleme etkinleştirmek için.  Atlanırsa veya kümesine `one`, tek bir ileti için işleve geçirildi. | 
|**bağlantı** |**bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak, bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğme [ad alanı](../event-hubs/event-hubs-create.md#create-an-event-hubs-namespace), olay hub kendisi değil. Bu bağlantı dizesini en az tetikleyici etkinleştirmek için Okuma izinlerine sahip olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Tetikleyici - olay meta verileri

Olay hub'ları tetikleme birkaç sağlar [meta veri özelliklerini](functions-triggers-bindings.md#binding-expressions---trigger-metadata). Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu özellikleri olan [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`PartitionContext`|[PartitionContext](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.partitioncontext)|`PartitionContext` Örneği.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan saati UTC diliminde saat.|
|`Offset`|`string`|Verileri olay hub'ı bölüm akışa göre uzaklığı. Uzaklık, bir işaretçi veya Event Hubs akış içinde bir olay için tanımlayıcı dayalıdır. Event Hubs akış ilişkin bir bölüm içinde benzersiz tanımlayıcısıdır.|
|`PartitionKey`|`string`|İçin hangi olay veri gönderilmesi gereken bir bölüm.|
|`Properties`|`IDictionary<String,Object>`|Kullanıcı özellikleri olay verileri.|
|`SequenceNumber`|`Int64`|Olay mantıksal sıra sayısı.|
|`SystemProperties`|`IDictionary<String,Object>`|Olay verileri dahil olmak üzere Sistem Özellikleri.|

Bkz: [kod örnekleri](#trigger---example) bu makalenin önceki bölümlerinde bu özellikleri kullanın.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#eventhub) dosyası Event Hubs tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıktı

Event Hubs çıkış olayları için olay akışının yazmaya bağlaması kullanın. Olaylar için yazma olay hub'ına gönderme izni olmalıdır.

Gerekli paket başvuruları yerinde olduğundan emin olun: [1.x işlevleri](#packages---functions-1.x) veya [2.x işlevleri](#packages---functions-2.x) 

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

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

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Bir ileti oluşturan C# betik kodunu şu şekildedir:

```cs
using System;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, TraceWriter log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.Verbose(msg);   
    outputEventHubMessage = msg;
}
```

Birden çok ileti oluşturan aşağıda verilmiştir; C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, TraceWriter log)
{
    string message = $"Event Hub message created at: {DateTime.Now}";
    log.Info(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıkış - F # örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

F # kodu şu şekildedir:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: TraceWriter) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.Verbose(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 1.x işlevleri ve ikincisi için işlevleri olan 2.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Tek bir ileti gönderir JavaScript kodu aşağıda verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();
    context.log('Event Hub message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Event Hub message created at: " + timeStamp;
    context.done();
};
```

Birden çok ileti gönderen JavaScript kodunu aşağıda verilmiştir:

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

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçin [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) özniteliği.

Özniteliğin oluşturucusu, olay hub'ı adını ve bağlantı dizesini içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [çıkışı - yapılandırma](#output---configuration). İşte bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `EventHub` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "EventHub için" olarak ayarlanmalıdır. |
|**direction** | yok | "Out" ayarlanmalıdır. Bu parametre, Azure portalında bağlamayı oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Olay temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**Yolu** |**EventHubName** | 1.x yalnızca çalışır. Olay hub'ının adı.  | 
|**eventHubName** |**EventHubName** | 2.x yalnızca çalışır. Olay hub'ının adı.  |
|**bağlantı** |**bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak, bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğme *ad alanı*, olay hub kendisi değil. Bu bağlantı dizesi için olay akışını ileti göndermek için gönderme izinleri olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

C# ve C# betik gibi bir yöntem parametresi kullanarak ileti göndermek `out string paramName`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. Birden çok ileti yazmak için kullanabileceğiniz `ICollector<string>` veya `IAsyncCollector<string>` yerine `out string`.

JavaScript'te, çıkış olayı kullanarak erişim `context.bindings.<name>`. `<name>` değer belirtilen `name` özelliği *function.json*.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
