---
title: Azure işlevleri için Azure Service Bus bağlamaları
description: Azure işlevleri'nde, Azure Service Bus Tetikleyicileri ve bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 04/01/2017
ms.author: cshoe
ms.openlocfilehash: 199ce2fe24d76595493dc2128cebb3fcb642fcab
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241142"
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure işlevleri için Azure Service Bus bağlamaları

Bu makalede, Azure işlevleri'nde Azure Service Bus bağlamaları ile nasıl çalışılacağı açıklanmaktadır. Tetikleme ve çıkış bağlamaları Service Bus kuyrukları ve konuları için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Service Bus bağlamaları sağlanan [Microsoft.Azure.WebJobs.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus) NuGet paketi sürüm 2.x. 

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Service Bus bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Service Bus tetikleyicisi, bir Service Bus kuyruğuna veya konusuna iletilere yanıt vermek için kullanın. 

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) okuyan [ileti meta verileri](#trigger---message-metadata) ve Service Bus kuyruk iletisi kaydeder:

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    ILogger log)
{
    log.LogInformation($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
    log.LogInformation($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.LogInformation($"DeliveryCount={deliveryCount}");
    log.LogInformation($"MessageId={messageId}");
}
```

Bu örnek için Azure işlevleri sürümdür 1.x. Bu kod 2.x için iş yapmak için:

- [erişim hakları parametreyi atlayın](#trigger---configuration)
- Günlük parametresinden türünü değiştirme `TraceWriter` için `ILogger`
- Değişiklik `log.Info` için `log.LogInformation`

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, Service Bus tetiği bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev okur [ileti meta verileri](#trigger---message-metadata) ve Service Bus kuyruk iletisi günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System;

public static void Run(string myQueueItem,
    Int32 deliveryCount,
    DateTime enqueuedTimeUtc,
    string messageId,
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");

    log.Info($"EnqueuedTimeUtc={enqueuedTimeUtc}");
    log.Info($"DeliveryCount={deliveryCount}");
    log.Info($"MessageId={messageId}");
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F# örneği

Aşağıdaki örnek, Service Bus tetiği bağlama gösterir. bir *function.json* dosyası ve bir [ F# işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlevi, bir Service Bus kuyruk iletisi günlüğe kaydeder. 

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

İşte F# betik kodu:

```fsharp
let Run(myQueueItem: string, log: ILogger) =
    log.LogInformation(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki Java işlevi kullanır `@ServiceBusQueueTrigger` ek açıklamanın [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime) bir Service Bus kuyruğu tetikleyicisi yapılandırmasını tanımlamak için. İşlevi, iletinin kuyrukta yer alan ve günlüklere ekler.

```java
@FunctionName("sbprocessor")
 public void serviceBusProcess(
    @ServiceBusQueueTrigger(name = "msg",
                             queueName = "myqueuename",
                             connection = "myconnvarname") String message,
   final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
```

Java işlevleri, bir Service Bus konusuna bir ileti eklendiğinde da tetiklenebilir. Aşağıdaki örnekte `@ServiceBusTopicTrigger` tetikleyici yapılandırması tanımlamak için ek açıklama.

```java
@FunctionName("sbtopicprocessor")
    public void run(
        @ServiceBusTopicTrigger(
            name = "message",
            topicName = "mytopicname",
            subscriptionName = "mysubscription",
            connection = "ServiceBusConnection"
        ) String message,
        final ExecutionContext context
    ) {
        context.getLogger().info(message);
    }
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, Service Bus tetiği bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev okur [ileti meta verileri](#trigger---message-metadata) ve Service Bus kuyruk iletisi günlüğe kaydeder. 

Veri bağlama işte *function.json* dosyası:

```json
{
"bindings": [
    {
    "queueName": "testqueue",
    "connection": "MyServiceBusConnection",
    "name": "myQueueItem",
    "type": "serviceBusTrigger",
    "direction": "in"
    }
],
"disabled": false
}
```

JavaScript kodu şu şekildedir:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.log('EnqueuedTimeUtc =', context.bindingData.enqueuedTimeUtc);
    context.log('DeliveryCount =', context.bindingData.deliveryCount);
    context.log('MessageId =', context.bindingData.messageId);
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), Service Bus tetiği yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusTriggerAttribute.cs)

  Özniteliğin oluşturucusu, kuyruk veya konu ve abonelik adını alır. Azure işlevleri sürüm 1.x, bağlantının erişim hakları da belirtebilirsiniz. Erişim hakları belirtmezseniz varsayılan değerdir `Manage`. Daha fazla bilgi için [tetikleyici - yapılandırma](#trigger---configuration) bölümü.

  Bir dize parametresi ile kullanılan öznitelik gösteren bir örnek aşağıda verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliğini kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, ILogger log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAccountAttribute.cs)

  Kullanılacak hizmet veri yolu hesabı belirtmek için başka bir yol sağlar. Oluşturucusu bir Service Bus bağlantı dizesi içeren bir uygulama ayarı adı alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, ILogger log)
  {
      ...
  }
  ```

Service Bus hesabını kullanacak şekilde aşağıdaki sırada belirlenir:

* `ServiceBusTrigger` Özniteliğin `Connection` özelliği.
* `ServiceBusAccount` Özniteliği aynı parametresine uygulanan `ServiceBusTrigger` özniteliği.
* `ServiceBusAccount` İşleve uygulanmış bir öznitelik.
* `ServiceBusAccount` Sınıfına uygulanan bir öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `ServiceBusTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "ServiceBusTrigger için" olarak ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | "İçin" ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodu, kuyruk veya konuda ileti temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return için" ayarlayın. |
|**queueName**|**queueName**|İzlemek için Kuyruğun adı.  Yalnızca bir konu için bir kuyruk izleme ayarlayın.
|**topicName**|**topicName**|İzlemek için konunun adı. Bir kuyruk için bir konu, yalnızca izleme ayarlayın.|
|**subscriptionName**|**subscriptionName**|İzlemek için Abonelik adı. Bir kuyruk için bir konu, yalnızca izleme ayarlayın.|
|**bağlantı**|**bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyServiceBus." adlı bir uygulama ayarı için "MyServiceBus", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan Service Bus bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarı kullanır.<br><br>Bağlantı dizesini almak için gösterilen adımları [yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string). Service Bus ad alanı bir belirli bir kuyruğa veya konuya sınırlı olmayan bir bağlantı dizesi olmalıdır. |
|**erişimHakları**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, belirten `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izin kümesi `accessRights` "dinlemek için". Aksi takdirde, İşlevler çalışma zamanı gerektiren işlemler yapmaya başarısız olabilir, hakları yönetin. Azure işlevleri sürüm 2.x, bu özellik kullanılabilir değil depolama SDK'sı en son sürümünü desteklemediğinden işlemleri yönetin.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# betiği aşağıdaki parametre türleri için kuyruk veya konuda ileti kullanabilirsiniz:

* `string` İleti metni ise.
* `byte[]` -İkili veriler için kullanışlıdır.
* İleti, JSON içeriyorsa bir özel tür - Azure işlevleri JSON verileri seri durumdan çalışır.
* `BrokeredMessage` -Seri durumdan çıkarılmış mesajıyla size [BrokeredMessage.GetBody<T>()](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.getbody?view=azure-dotnet#Microsoft_ServiceBus_Messaging_BrokeredMessage_GetBody__1) yöntemi.

Bu parametreleri için Azure işlevleri sürüm olan 1.x; 2.x için kullanıyorsanız [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) yerine `BrokeredMessage`.

JavaScript'te, kuyruk veya konuda ileti kullanarak erişim `context.bindings.<name from function.json>`. Service Bus iletiyi bir dize veya bir JSON nesnesi olarak işleve geçirilir.

## <a name="trigger---poison-messages"></a>Tetikleyici - zehirli iletiler

Zehirli ileti işleme denetlenen veya Azure işlevleri'nde yapılandırılır. Service Bus kendisini zehirli iletileri işler.

## <a name="trigger---peeklock-behavior"></a>Tetikleyici - PeekLock davranışı

İşlevler çalışma zamanı içinde bir ileti alır [PeekLock modu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Çağrı `Complete` işlevi başarıyla tamamlanırsa ileti veya çağrı `Abandon` işlev başarısız olursa. İşlev daha uzun çalışırsa `PeekLock` kilit zaman aşımı işlevin çalıştığını sürece otomatik olarak yenilenir. 

`maxAutoRenewDuration` İçinde yapılandırılabilir *host.json*, eşlenir [OnMessageOptions.MaxAutoRenewDuration](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.messagehandleroptions.maxautorenewduration?view=azure-dotnet). Bu ayar için izin verilen en fazla 10 dakika, 5 dakikalık varsayılan işlevler zaman sınırı artırabilirsiniz ancak Service Bus belgeleri göre 5 dakikadır. Service Bus yenileme sınırı aşacağından için Service Bus işlevleri, daha sonra bunu istemezsiniz.

## <a name="trigger---message-metadata"></a>Tetikleyici - ileti meta verileri

Service Bus tetiği birkaç sağlar [meta veri özelliklerini](./functions-bindings-expressions-patterns.md#trigger-metadata). Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu özellikleri olan [BrokeredMessage](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`DeliveryCount`|`Int32`|Teslimat sayısı.|
|`DeadLetterSource`|`string`|Atılacak Mektubu kaynağı.|
|`ExpiresAtUtc`|`DateTime`|Sona erme saati UTC diliminde saat.|
|`EnqueuedTimeUtc`|`DateTime`|Sıraya alınan saati UTC diliminde saat.|
|`MessageId`|`string`|Service Bus yinelenen iletileri tanımlamak için etkinleştirilirse kullanabileceğiniz bir kullanıcı tanımlı bir değer.|
|`ContentType`|`string`|Belirli mantıksal uygulama için alıcı ve gönderen tarafından kullanılan bir içerik türü tanımlayıcısı.|
|`ReplyTo`|`string`|Kuyruk adresine yanıt.|
|`SequenceNumber`|`Int64`|Bir ileti için Service Bus tarafından atanmış benzersiz sayı.|
|`To`|`string`|Adresine gönderin.|
|`Label`|`string`|Uygulamaya özgü etiket.|
|`CorrelationId`|`string`|Bağıntı Kimliği|

> [!NOTE]
> Şu anda oturumu etkin kuyruklar ve abonelikler ile çalışan Service bus tetikleyicisi Önizleme aşamasındadır. Lütfen izleme [bu öğeyi](https://github.com/Azure/azure-webjobs-sdk/issues/529#issuecomment-491113458) bu ilgili herhangi bir ek güncelleştirme için. 

Bkz: [kod örnekleri](#trigger---example) bu makalenin önceki bölümlerinde bu özellikleri kullanın.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#servicebus) dosyası, Service Bus tetikleyicisi davranışını denetleyen ayarları içerir.

```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "maxAutoRenewDuration": "00:05:00"
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Bir kerede yalnızca tek bir kuyruk veya konuda ileti işleme için çalışma zamanının ayarlayın `maxConcurrentCalls` 1. |
|prefetchCount|yok|Varsayılan temel alınan MessageReceiver tarafından kullanılacak PrefetchCount.|
|maxAutoRenewDuration|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.|

## <a name="output"></a>Çıktı

Kuyruk veya konuda ileti göndermek için Azure Service Bus'ı çıkışı bağlama kullanın.

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [Java](#output---java-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , Service Bus kuyruk iletisi gönderir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, ILogger log)
{
    log.LogInformation($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bir Service Bus çıkış bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev, her 15 saniyede bir kuyruğa ileti göndermek için zamanlama tetikleyicisini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturan C# betik kodunu şu şekildedir:

```cs
public static void Run(TimerInfo myTimer, ILogger log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan aşağıda verilmiştir; C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, ILogger log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.LogInformation(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıkış - F# örneği

Aşağıdaki örnek, bir Service Bus çıkış bağlama gösterir. bir *function.json* dosyası ve bir [ F# betik işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlev, her 15 saniyede bir kuyruğa ileti göndermek için zamanlama tetikleyicisini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

İşte F# betik kodu, tek bir ileti oluşturur:

```fsharp
let Run(myTimer: TimerInfo, log: ILogger, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.LogInformation(message)
    outputSbQueue = message
```

### <a name="output---java-example"></a>Çıkış - Java örnek

Aşağıdaki örnek, bir Service Bus kuyruğuna bir ileti gönderen bir Java işlev gösterir. `myqueue` bir HTTP isteği tarafından tetiklendiğinde.

```java
@FunctionName("httpToServiceBusQueue")
@ServiceBusQueueOutput(name = "message", queueName = "myqueue", connection = "AzureServiceBusConnection")
public String pushToQueue(
  @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
  final String message,
  @HttpOutput(name = "response") final OutputBinding<T> result ) {
      result.setValue(message + " has been sent.");
      return message;
 }
```

 İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@QueueOutput` değeri bir Service Bus kuyruğuna yazılmış işlevi parametre üzerindeki ek açıklama.  Parametre türü olmalıdır `OutputBinding<T>`, burada T bir POJO'ya herhangi bir yerel Java türü.

Java işlevleri, bir Service Bus konu başlığına da yazabilirsiniz. Aşağıdaki örnekte `@ServiceBusTopicOutput` çıkış bağlaması yapılandırmasını tanımlamak için ek açıklama. 

```java
@FunctionName("sbtopicsend")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @ServiceBusTopicOutput(name = "message", topicName = "mytopicname", subscriptionName = "mysubscription", connection = "ServiceBusConnection") OutputBinding<String> message,
            final ExecutionContext context) {
        
        String name = request.getBody().orElse("Azure Functions");

        message.setValue(name);
        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
        
    }
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bir Service Bus çıkış bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev, her 15 saniyede bir kuyruğa ileti göndermek için zamanlama tetikleyicisini kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
    "bindings": [
        {
            "schedule": "0/15 * * * * *",
            "name": "myTimer",
            "runsOnStartup": true,
            "type": "timerTrigger",
            "direction": "in"
        },
        {
            "name": "outputSbQueue",
            "type": "serviceBus",
            "queueName": "testqueue",
            "connection": "MyServiceBusConnection",
            "direction": "out"
        }
    ],
    "disabled": false
}
```

Tek bir ileti oluşturur JavaScript betik kodunu şu şekildedir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = message;
    context.done();
};
```

Birden çok ileti oluşturur JavaScript betik kodunu şu şekildedir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueue = [];
    context.bindings.outputSbQueue.push("1 " + message);
    context.bindings.outputSbQueue.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.ServiceBus/ServiceBusAttribute.cs).

Özniteliğin oluşturucusu, kuyruk veya konu ve abonelik adını alır. Bağlantının erişim hakları de belirtebilirsiniz. Erişim haklarını ayarlama seçme içinde açıklanmıştır [çıkışı - yapılandırma](#output---configuration) bölümü. İşlev dönüş değeri için uygulanan öznitelik gösteren bir örnek aşağıda verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

Kullanabileceğiniz `ServiceBusAccount` sınıf, yöntem ya da parametre düzeyinde kullanılacak hizmet veri yolu hesabı belirtmek için özniteliği.  Daha fazla bilgi için [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `ServiceBus` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | "Service Bus" için ayarlanmış olması gerekir. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | "Out" ayarlanmalıdır. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | Kuyruk veya konuda işlev kodunu temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return için" ayarlayın. |
|**queueName**|**queueName**|Kuyruğun adı.  Yalnızca bir konu için kuyruk iletileri gönderme ayarlayın.
|**topicName**|**topicName**|İzlemek için konunun adı. Yalnızca bir kuyruk için konu iletileri gönderme ayarlayın.|
|**bağlantı**|**bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyServiceBus." adlı bir uygulama ayarı için "MyServiceBus", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan Service Bus bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarı kullanır.<br><br>Bağlantı dizesini almak için gösterilen adımları [yönetim kimlik bilgilerini alma](../service-bus-messaging/service-bus-quickstart-portal.md#get-the-connection-string). Service Bus ad alanı bir belirli bir kuyruğa veya konuya sınırlı olmayan bir bağlantı dizesi olmalıdır.|
|**erişimHakları**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, belirten `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izin kümesi `accessRights` "dinlemek için". Aksi takdirde, İşlevler çalışma zamanı gerektiren işlemler yapmaya başarısız olabilir, hakları yönetin. Azure işlevleri sürüm 2.x, bu özellik kullanılabilir değil depolama SDK'sı en son sürümünü desteklemediğinden işlemleri yönetin.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

Azure işlevleri'nde 1.x, çalışma zamanı oluşturur kuyruk yoksa ve ayarladığınız `accessRights` için `manage`. İşlevleri sürüm 2.x, kuyruk veya konuda zaten bulunmalıdır; bir kuyruk veya konu yok belirtirseniz işlev başarısız olur. 

C# ve C# betiği aşağıdaki parametre türleri için çıkış bağlaması kullanabilirsiniz:

* `out T paramName` - `T` JSON seri hale getirilebilir bir tür olabilir. Parametre değeri null ise, işlev işlevleri ileti ile null bir nesne oluşturur.
* `out string` -İşlev parametre değeri null ise işlevleri oluşturmaz bir ileti.
* `out byte[]` -İşlev parametre değeri null ise işlevleri oluşturmaz bir ileti.
* `out BrokeredMessage` -İşlev parametre değeri null ise işlevleri oluşturmaz bir ileti.
* `ICollector<T>` veya `IAsyncCollector<T>` - birden çok ileti oluşturmak için. Çağırdığınızda bir ileti oluşturulur `Add` yöntemi.

Zaman uyumsuz işlevleri'nde dönüş değerini kullanın veya `IAsyncCollector` yerine bir `out` parametresi.

Bu parametreleri için Azure işlevleri sürüm olan 1.x; 2.x için kullanıyorsanız [ `Message` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.servicebus.message) yerine `BrokeredMessage`.

JavaScript'te, kuyruk veya konu kullanarak erişim `context.bindings.<name from function.json>`. İçin bir dize, bir bayt dizisi veya bir Javascript nesnesi (JSON'a seri durumdan) atayabilirsiniz `context.binding.<name>`.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| Service Bus | [Service Bus hata kodları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-messaging-exceptions) |
| Service Bus | [Service Bus sınırları](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-quotas) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde sürümünde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır 2.x. Aşağıdaki örnek host.json dosyasını yalnızca bu bağlama için sürüm 2.x ayarları içerir. Sürümündeki genel yapılandırma ayarları hakkında daha fazla bilgi için 2.x bkz [sürümü Azure işlevleri için host.json başvurusu 2.x](functions-host-json.md).

> [!NOTE]
> İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "serviceBus": {
            "prefetchCount": 100,
            "messageHandlerOptions": {
                "autoComplete": false,
                "maxConcurrentCalls": 32,
                "maxAutoRenewDuration": "00:55:00"
            }
        }
    }
}
```

|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxAutoRenewDuration|00:05:00|En uzun süre içinde otomatik olarak ileti kilidi yenilenir.|
|Otomatik Tamamlama|true|Tetikleyici hemen tam (Otomatik Tamamlama) işaretlemek olup tam çağrı işlemenin tamamlanmasını bekleyin.|
|maxConcurrentCalls|16|İleti pompası başlatmalıdır geri çağırma eş zamanlı çağrı sayısı. Varsayılan olarak, İşlevler çalışma zamanı aynı anda birden çok ileti işler. Bir kerede yalnızca tek bir kuyruk veya konuda ileti işleme için çalışma zamanının ayarlayın `maxConcurrentCalls` 1. |
|prefetchCount|yok|Varsayılan temel alınan MessageReceiver tarafından kullanılacak PrefetchCount.|


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
