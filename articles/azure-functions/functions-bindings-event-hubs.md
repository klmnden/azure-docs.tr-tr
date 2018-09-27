---
title: Azure işlevleri için Azure Event Hubs bağlamaları
description: Azure işlevleri'nde Azure Event Hubs bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.assetid: daf81798-7acc-419a-bc32-b5a41c6db56b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/08/2017
ms.author: glenga
ms.openlocfilehash: def921a5378e85caadfaebdd06fc61dba2661756
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223100"
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

Her bir olay hub'ı ile tetiklenen bir işlev örneği yalnızca biri tarafından desteklenen [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği. Olay hub'ları yalnızca bir sağlar [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği, belirli bir bölüme bir kira alabilirsiniz.

Örneğin, bir olay hub'ı aşağıdaki gibi düşünün:

* 10 bölümler.
* Her bölümde 100 iletileri olan tüm bölümler arasında eşit olarak dağıtılmış 1000 olay.

İlk işlevinizi etkinleştirildiğinde, yalnızca bir örneğini işlevi yoktur. Bu işlev örneği adlandıralım `Function_0`. `Function_0` tek bir sahip [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) tüm on bölümleri üzerinde bir kira var örneği. Bu örnek, 0-9 bölümlerden olayları okunuyor. Bu noktadan itibaren aşağıdakilerden biri gerçekleşir:

* **Yeni işlev örnekleri gerekmeyen**: `Function_0` ölçeklendirme mantıksal devreye girer işlevleri önce tüm 1000 olayları işleyebilir. Bu durumda, tüm 1000 iletileri tarafından işlenen `Function_0`.

* **Ek işlev örneği eklenen**: mantıksal ölçeklendirme işlevleri belirler `Function_0` , işleyebileceğinden daha fazla ileti yok. Bu durumda, yeni bir işlev uygulaması örneğinde (`Function_1`), yanı sıra yeni oluşturulan [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği. Olay hub'ları algılar iletileri okumak yeni bir ana bilgisayar örneği çalışıyor. Olay hub'ları yük dengeleyen bölümler arasında kendi ana bilgisayar örneği. Örneğin, 0-4 bölüm için atanabilir `Function_0` ve 5-9 bölümleri `Function_1`. 

* **Daha fazla işlev örneği eklenir N**: mantıksal ölçeklendirme işlevleri belirler hem `Function_0` ve `Function_1` bunlar işleyebileceğinden daha fazla ileti sahip. Yeni işlev uygulaması örnekleri `Function_2`... `Functions_N` nerede oluşturulduğunu, `N` olay hub'ı bölümleri sayısından büyüktür. Yük dengeleyen bölümleri, bu durumda örneklerinde Örneğimizde, Event Hubs yeniden `Function_0`... `Functions_9`. 

Dikkat edin işlevleri ölçeklenir `N` olay hub'ı bölümleri sayısından daha büyük bir sayı olan örnekleri. Bu her zaman emin olmak için gerçekleştirilir [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği diğer örneklerin kullanılabilir oldukça bölümler kilitler elde etmek kullanılabilir. Yalnızca işlev örneği yürütüldüğünde kullanılan kaynaklar için ücret ödersiniz; Bu hızlı sağlama için ücretlendirilmez.

Tüm işlev yürütme (ile veya hata olmadan) tamamlandığında denetim noktaları ilişkili depolama hesabına eklenir. Onay işaret eden başarılı olduğunda, tüm 1000 iletileri hiçbir zaman yeniden alınır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 


```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```
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

Bir toplu işte olayları almak için ayarlanmış `cardinality` için `many` içinde *function.json* aşağıdaki örneklerde gösterildiği gibi dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 

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
    
    eventHubMessages.forEach((message, index) => {
        context.log(`Processed message ${message}`);
        context.log(`EnqueuedTimeUtc = ${context.bindingData.enqueuedTimeUtcArray[index]}`);
        context.log(`SequenceNumber = ${context.bindingData.sequenceNumberArray[index]}`);
        context.log(`Offset = ${context.bindingData.offsetArray[index]}`);
    });

    context.done();
};
```

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [Java işlevi](functions-reference-java.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

```json
{
  "type": "eventHubTrigger",
  "name": "msg",
  "direction": "in",
  "eventHubName": "myeventhubname",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

```java
@FunctionName("ehprocessor")
public void eventHubProcessor(
  @EventHubTrigger(name = "msg",
                  eventHubName = "myeventhubname",
                  connection = "myconnvarname") String message,
       final ExecutionContext context ) 
       {
          context.getLogger().info(message);
 }
 ```

 İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `EventHubTrigger` değerini olay Hub'ından gelen parametre üzerindeki ek açıklama. Bu ek açıklamalar parametrelerle bir olay geldiğinde çalıştırmak işlev neden.  Bu ek açıklama yerel Java türler, pojo'ları veya isteğe bağlı kullanarak boş değer atanabilir değer ile kullanılabilir<T>. 

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
|**Yolu** |**EventHubName** | 1.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. | 
|**EventHubName** |**EventHubName** | 2.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
|**consumerGroup** |**consumerGroup** | Ayarlar isteğe bağlı bir özellik [tüketici grubu](../event-hubs/event-hubs-features.md#event-consumers) hub'ındaki olayları abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır. | 
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
* [Java](#output---java-example)

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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
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

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. İlk örnek için olan 2.x işlevleri ve ikincisi için işlevleri olan 1.x. 

```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```
```json
{
    "type": "eventHub",
    "name": "outputEventHubMessage",
    "path": "myeventhub",
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

### <a name="output---java-example"></a>Çıkış - Java örnek

Aşağıdaki örnek, geçerli saati bir olay Hub'ına ileti contianing yazan bir Java işlev gösterir.

```java
@}FunctionName("sendTime")
@EventHubOutput(name = "event", eventHubName = "samples-workitems", connection = "AzureEventHubConnection")
public String sendTime(
   @TimerTrigger(name = "sendTimeTrigger", schedule = "0 *&#47;5 * * * *") String timerInfo)  {
     return LocalDateTime.now().toString();
 }
 ```

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@EventHubOutput` ek açıklama parametreleri değeri, olay hub'ına poublished olacaktır.  Tür parametresi olmalıdır `OutputBinding<T>` , burada T bir POJO'ya veya herhangi bir yerel Java türü. 

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
|**Yolu** |**EventHubName** | 1.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. | 
|**EventHubName** |**EventHubName** | 2.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
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
