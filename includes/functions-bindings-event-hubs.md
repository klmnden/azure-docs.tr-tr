---
author: craigshoemaker
ms.service: azure-functions
ms.topic: include
ms.date: 03/05/2019
ms.author: cshoe
ms.openlocfilehash: 421e0db48f045c5cbce52a0641902e6d2a11276e
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188167"
---
## <a name="trigger"></a>Tetikleyici

İşlev tetikleyicisi için bir olay hub'ı olay akışı gönderilen olaya yanıt vermek için kullanın. Tetikleyicinin ayarlamak için temel olay hub'ına okuma erişimi olmalıdır. İşlev tetiklendiğinde, işleve geçirilen ileti bir dize olarak yazılır.

## <a name="trigger---scaling"></a>Tetikleme - ölçeklendirme

Bir olayı tetiklendi işlevi her bir örneği tarafından tek bir yedeklenen [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği. Yalnızca bir (olay hub'ları tarafından desteklenen) tetikleyici sağlar [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği, belirli bir bölüme bir kira alabilirsiniz.

Örneğin, bir olay hub'ı aşağıdaki gibi düşünün:

* 10 bölümleri
* Her bölümde 100 iletileri olan tüm bölümler arasında eşit olarak dağıtılmış 1.000 olayları

İlk işlevinizi etkinleştirildiğinde, yalnızca bir örneğini işlevi yoktur. İlk işlev örneği adlandıralım `Function_0`. `Function_0` İşleve sahip tek bir örneğini [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) kira tüm on bölümlerde tutan. Bu örnek, 0-9 bölümlerden olayları okunuyor. Bu noktadan itibaren aşağıdakilerden biri gerçekleşir:

* **Yeni işlev örnekleri gerekmeyen**: `Function_0` mantıksal ölçeklendirme işlevi etkinleşmeden önce tüm 1.000 olayları işleyebilir. Bu durumda, tüm 1.000 iletileri tarafından işlenen `Function_0`.

* **Ek işlev örneği eklenen**: Varsa işlevleri belirleyen mantıksal ölçeklendirme `Function_0` daha yeni bir işlev uygulaması örneği işlem daha fazla ileti yok (`Function_1`) oluşturulur. Bu yeni işlev de ilişkili bir örneği olduğu [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor). Temel Event hubs'ı yeni bir ana bilgisayar örneği iletileri okumak çalışıyor algılıyoruz, yükü dengelenen bölümler arasında kendi ana bilgisayar örneği. Örneğin, 0-4 bölüm için atanabilir `Function_0` ve 5-9 bölümleri `Function_1`.

* **Daha fazla işlev örneği eklenir N**: Varsa işlevleri belirleyen mantıksal ölçeklendirme hem `Function_0` ve `Function_1` sahip kullanıcılar, yeni işleyebileceğinden daha fazla ileti `Functions_N` işlevi uygulama örneği oluşturulur.  Uygulamalar noktasına oluşturulan burada `N` olay hub'ı bölümleri sayısından büyüktür. Yük dengeleyen bölümleri, bu durumda örneklerinde Örneğimizde, Event Hubs yeniden `Function_0`... `Functions_9`.

Ne zaman, Ölçek, işlev `N` örnekleri olan olay hub'ı bölümleri sayısından büyük bir sayı. Bunu sağlamak için yapılır [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.processor) örneği diğer örneklerin kullanılabilir oldukça bölümler kilitler elde etmek kullanılabilir. Yalnızca işlev örneği yürütüldüğünde kullanılan kaynaklar için ücretlendirilirsiniz. Diğer bir deyişle, bu aşırı sağlama için ücretlendirilmez.

Tüm işlev yürütme (ile veya hata olmadan) tamamlandığında denetim noktaları ilişkili depolama hesabına eklenir. Onay işaret eden başarılı olduğunda, tüm 1.000 ileti hiçbir zaman yeniden alınır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) , günlükleri Olay hub'ı tetikleyicisi ileti gövdesi.

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
{
    log.LogInformation($"C# function triggered to process a message: {myEventHubMessage}");
}
```

Erişim elde etmek için [olay meta verilerinin](#trigger---event-metadata) işlevi kod içinde bağlama bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (bir kullanılması deyimi için `Microsoft.Azure.EventHubs`). Aynı özellikleri Yöntem imzasında bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almak için her iki yönde gösterir:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run(
    [EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    ILogger log)
{
    log.LogInformation($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    // Metadata accessed by binding to EventData
    log.LogInformation($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.LogInformation($"Offset={myEventHubMessage.SystemProperties.Offset}");
    // Metadata accessed by using binding expressions in method parameters
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"SequenceNumber={sequenceNumber}");
    log.LogInformation($"Offset={offset}");
}
```

Bir toplu işte olayları almak, olun `string` veya `EventData` dizisi.  

> [!NOTE]
> Yukarıdaki örnekte ile yöntemi parametrelerine bağlanamıyor toplu alırken ister `DateTime enqueuedTimeUtc` ve bunlar her almalıdır `EventData` nesnesi  

```cs
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] EventData[] eventHubMessages, ILogger log)
{
    foreach (var message in eventHubMessages)
    {
        log.LogInformation($"C# function triggered to process a message: {Encoding.UTF8.GetString(message.Body)}");
        log.LogInformation($"EnqueuedTimeUtc={message.SystemProperties.EnqueuedTimeUtc}");
    }
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](../articles/azure-functions/functions-reference-csharp.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya.

#### <a name="version-2x"></a>Sürüm 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Sürüm 1.x

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
    log.Info($"C# function triggered to process a message: {myEventHubMessage}");
}
```

Erişim elde etmek için [olay meta verilerinin](#trigger---event-metadata) işlevi kod içinde bağlama bir [EventData](/dotnet/api/microsoft.servicebus.messaging.eventdata) nesne (bir kullanılması deyimi için `Microsoft.Azure.EventHubs`). Aynı özellikleri Yöntem imzasında bağlama ifadeleri kullanarak da erişebilirsiniz.  Aşağıdaki örnek, aynı verileri almak için her iki yönde gösterir:

```cs
#r "Microsoft.Azure.EventHubs"

using System.Text;
using System;
using Microsoft.ServiceBus.Messaging;
using Microsoft.Azure.EventHubs;

public static void Run(EventData myEventHubMessage,
    DateTime enqueuedTimeUtc,
    Int64 sequenceNumber,
    string offset,
    TraceWriter log)
{
    log.Info($"Event: {Encoding.UTF8.GetString(myEventHubMessage.Body)}");
    log.Info($"EnqueuedTimeUtc={myEventHubMessage.SystemProperties.EnqueuedTimeUtc}");
    log.Info($"SequenceNumber={myEventHubMessage.SystemProperties.SequenceNumber}");
    log.Info($"Offset={myEventHubMessage.SystemProperties.Offset}");

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
        log.Info($"C# function triggered to process a message: {message}");
    }
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F# örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [ F# işlevi](../articles/azure-functions/functions-reference-fsharp.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya. 

#### <a name="version-2x"></a>Sürüm 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Sürüm 1.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "path": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

İşte F# kod:

```fsharp
let Run(myEventHubMessage: string, log: TraceWriter) =
    log.Log(sprintf "F# eventhub trigger function processed work item: %s" myEventHubMessage)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](../articles/azure-functions/functions-reference-node.md) bağlama kullanan. İşlev okur [olay meta verilerinin](#trigger---event-metadata) ve iletiyi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya.

#### <a name="version-2x"></a>Sürüm 2.x

```json
{
  "type": "eventHubTrigger",
  "name": "myEventHubMessage",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

#### <a name="version-1x"></a>Sürüm 1.x

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
    context.log('Function triggered to process a message: ', myEventHubMessage);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('SequenceNumber =', context.bindingData.sequenceNumber);
    context.log('Offset =', context.bindingData.offset);

    context.done();
};
```

Bir toplu işte olayları almak için ayarlanmış `cardinality` için `many` içinde *function.json* aşağıdaki örneklerde gösterildiği gibi dosya.

#### <a name="version-2x"></a>Sürüm 2.x

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

#### <a name="version-1x"></a>Sürüm 1.x

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

### <a name="trigger---python-example"></a>Tetikleyici - Python örnek

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [funkce Pythonu](../articles/azure-functions/functions-reference-python.md) bağlama kullanan. İşlev okur [olay meta verilerinin](#trigger---event-metadata) ve iletiyi günlüğe kaydeder.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya.

```json
{
  "type": "eventHubTrigger",
  "name": "event",
  "direction": "in",
  "eventHubName": "MyEventHub",
  "connection": "myEventHubReadConnectionAppSetting"
}
```

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func

def main(event: func.EventHubEvent):
    logging.info('Function triggered to process a message: ', event.get_body())
    logging.info('  EnqueuedTimeUtc =', event.enqueued_time)
    logging.info('  SequenceNumber =', event.sequence_number)
    logging.info('  Offset =', event.offset)
```

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [Java işlevi](../articles/azure-functions/functions-reference-java.md) bağlama kullanan. İşlevi, olay hub'ı tetikleyicisi ileti gövdesi günlüğe kaydeder.

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

İçinde [C# sınıfı kitaplıklar](../articles/azure-functions/functions-dotnet-class-library.md), kullanın [EventHubTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.EventHubs/EventHubTriggerAttribute.cs) özniteliği.

Özniteliğin oluşturucusu, olay hub'ı adı, tüketici grubu adını ve bağlantı dizesini içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [tetikleyecek yapılandırma bölümü](#trigger---configuration). İşte bir `EventHubTriggerAttribute` özniteliği örneği:

```csharp
[FunctionName("EventHubTriggerCSharp")]
public static void Run([EventHubTrigger("samples-workitems", Connection = "EventHubConnectionAppSetting")] string myEventHubMessage, ILogger log)
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
|**Yolu** |**EventHubName** | 1\.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
|**EventHubName** |**EventHubName** | 2\.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
|**consumerGroup** |**consumerGroup** | Ayarlar isteğe bağlı bir özellik [tüketici grubu](../articles/event-hubs/event-hubs-features.md)#event-tüketicileri) hub'ındaki olayları abone olmak için kullanılır. Atlanırsa, `$Default` tüketici grubu kullanılır. |
|**önem düzeyi** | yok | JavaScript için. Kümesine `many` toplu işleme etkinleştirmek için.  Atlanırsa veya kümesine `one`, tek bir ileti için işleve geçirildi. |
|**bağlantı** |**bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak, bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğme [ad alanı](../articles/event-hubs/event-hubs-create.md)#create-bir-event-hubs-ad alanı), olay hub kendisi değil. Bu bağlantı dizesini en az tetikleyici etkinleştirmek için Okuma izinlerine sahip olmalıdır.|
|**Yolu**|**EventHubName**|Olay hub'ının adı. Uygulama ayarları başvurulabilir `%eventHubName%`|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="trigger---event-metadata"></a>Tetikleyici - olay meta verileri

Olay hub'ları tetikleme birkaç sağlar [meta veri özelliklerini](../articles/azure-functions/./functions-bindings-expressions-patterns.md). Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu özellikleri olan [EventData](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventdata) sınıfı.

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

[Host.json](../articles/azure-functions/functions-host-json.md#eventhub) dosyası Event Hubs tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../articles/azure-functions/../../includes/functions-host-json-event-hubs.md)]

## <a name="output"></a>Çıktı

Event Hubs çıkış olayları için olay akışının yazmaya bağlaması kullanın. Olaylar için yazma olay hub'ına gönderme izni olmalıdır.

Gerekli paket başvuruları karşılandığından emin olun: 1.x işlevleri veya 2.x işlevleri

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [Java](#output---java-example)
* [JavaScript](#output---javascript-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](../articles/azure-functions/functions-dotnet-class-library.md) , Yazar bir ileti yönteminin dönüş değeri çıkış olarak kullanan bir olay hub'ına:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
{
    log.LogInformation($"C# Timer trigger function executed at: {DateTime.Now}");
    return $"{DateTime.Now}";
}
```

Aşağıdaki örnek nasıl kullanılacağını gösterir `IAsyncCollector` toplu iletiler göndermek için arabirim. Bir olay Hub'ından gelen ve sonucu başka bir olay Hub'ına gönderme iletilerini işlerken bu yaygın bir senaryodur.

```csharp
[FunctionName("EH2EH")]
public static async Task Run(
    [EventHubTrigger("source", Connection = "EventHubConnectionAppSetting")] EventData[] events,
    [EventHub("dest", Connection = "EventHubConnectionAppSetting")]IAsyncCollector<string> outputEvents,
    ILogger log)
{
    foreach (EventData eventData in events)
    {
        // do some processing:
        var myProcessedEvent = DoSomething(eventData);

        // then send the message
        await outputEvents.AddAsync(JsonConvert.SerializeObject(myProcessedEvent));
    }
}
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](../articles/azure-functions/functions-reference-csharp.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

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
using Microsoft.Extensions.Logging;

public static void Run(TimerInfo myTimer, out string outputEventHubMessage, ILogger log)
{
    String msg = $"TimerTriggerCSharp1 executed at: {DateTime.Now}";
    log.LogInformation(msg);   
    outputEventHubMessage = msg;
}
```

Birden çok ileti oluşturan aşağıda verilmiştir; C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, ICollector<string> outputEventHubMessage, ILogger log)
{
    string message = $"Message created at: {DateTime.Now}";
    log.LogInformation(message);
    outputEventHubMessage.Add("1 " + message);
    outputEventHubMessage.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıkış - F# örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [ F# işlevi](../articles/azure-functions/functions-reference-fsharp.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

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

İşte F# kod:

```fsharp
let Run(myTimer: TimerInfo, outputEventHubMessage: byref<string>, log: ILogger) =
    let msg = sprintf "TimerTriggerFSharp1 executed at: %s" DateTime.Now.ToString()
    log.LogInformation(msg);
    outputEventHubMessage <- msg;
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](../articles/azure-functions/functions-reference-node.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

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
    context.log('Message created at: ', timeStamp);   
    context.bindings.outputEventHubMessage = "Message created at: " + timeStamp;
    context.done();
};
```

Birden çok ileti gönderen JavaScript kodunu aşağıda verilmiştir:

```javascript
module.exports = function(context) {
    var timeStamp = new Date().toISOString();
    var message = 'Message created at: ' + timeStamp;

    context.bindings.outputEventHubMessage = [];

    context.bindings.outputEventHubMessage.push("1 " + message);
    context.bindings.outputEventHubMessage.push("2 " + message);
    context.done();
};
```

### <a name="output---python-example"></a>Çıkış - Python örnek

Aşağıdaki örnek, bir olay hub'ı tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [funkce Pythonu](../articles/azure-functions/functions-reference-python.md) bağlama kullanan. İşlevi, bir olay hub'ına bir ileti yazar.

Event Hubs, veri bağlama Aşağıdaki örnekler *function.json* dosya.

```json
{
    "type": "eventHub",
    "name": "$return",
    "eventHubName": "myeventhub",
    "connection": "MyEventHubSendAppSetting",
    "direction": "out"
}
```

Tek bir ileti gönderir Python kodu aşağıda verilmiştir:

```python
import datetime
import logging
import azure.functions as func

def main(timer: func.TimerRequest) -> str:
    timestamp = datetime.datetime.utcnow()
    logging.info('Message created at: %s', timestamp);   
    return 'Message created at: {}'.format(timestamp)
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

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@EventHubOutput` ek açıklama parametreleri değeri, olay Hub'ına yayımlanan.  Tür parametresi olmalıdır `OutputBinding<T>` , burada T bir POJO'ya veya herhangi bir yerel Java türü.

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçin [C# sınıfı kitaplıklar](../articles/azure-functions/functions-dotnet-class-library.md), kullanın [EventHubAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/EventHubs/EventHubAttribute.cs) özniteliği.

Özniteliğin oluşturucusu, olay hub'ı adını ve bağlantı dizesini içeren bir uygulama ayarı adı alır. Bu ayarlar hakkında daha fazla bilgi için bkz. [çıkışı - yapılandırma](#output---configuration). İşte bir `EventHub` özniteliği örneği:

```csharp
[FunctionName("EventHubOutput")]
[return: EventHub("outputEventHubMessage", Connection = "EventHubConnectionAppSetting")]
public static string Run([TimerTrigger("0 */5 * * * *")] TimerInfo myTimer, ILogger log)
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
|**name** | yok | Olay temsil eden işlevi kod içinde kullanılan değişken adı. |
|**Yolu** |**EventHubName** | 1\.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
|**EventHubName** |**EventHubName** | 2\.x yalnızca çalışır. Olay hub'ının adı. Bu değer, olay hub'ı adı da bağlantı dizesinde mevcut olduğunda, çalışma zamanında bu özellik geçersiz kılar. |
|**bağlantı** |**bağlantı** | Olay hub'ın ad bağlantı dizesi içeren bir uygulama ayarı adı. Tıklayarak, bu bağlantı dizesini kopyalayın **bağlantı bilgilerini** için düğme *ad alanı*, olay hub kendisi değil. Bu bağlantı dizesi için olay akışını ileti göndermek için gönderme izinleri olmalıdır.|

[!INCLUDE [app settings to local.settings.json](../articles/azure-functions/../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

C# ve C# betik gibi bir yöntem parametresi kullanarak ileti göndermek `out string paramName`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. Birden çok ileti yazmak için kullanabileceğiniz `ICollector<string>` veya `IAsyncCollector<string>` yerine `out string`.

JavaScript'te, çıkış olayı kullanarak erişim `context.bindings.<name>`. `<name>` değer belirtilen `name` özelliği *function.json*.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Olay Hub'ı | [İşlemler Kılavuzu](https://docs.microsoft.com/rest/api/eventhub/publisher-policy-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde sürümünde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır 2.x. Aşağıdaki örnek host.json dosyasını yalnızca bu bağlama için sürüm 2.x ayarları içerir. Sürümündeki genel yapılandırma ayarları hakkında daha fazla bilgi için 2.x bkz [sürümü Azure işlevleri için host.json başvurusu 2.x](../articles/azure-functions/functions-host-json.md).

> [!NOTE]
> İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](../articles/azure-functions/functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxBatchSize|64|Alma döngü alınan en yüksek olay sayısı.|
|prefetchCount|yok|Varsayılan temel alınan EventProcessorHost tarafından kullanılacak PrefetchCount.|
|batchCheckpointFrequency|1|Bir EventHub imleç denetim noktası oluşturmadan önce işlenecek olay yığın sayısı.|