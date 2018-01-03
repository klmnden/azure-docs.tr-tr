---
title: "Azure işlevleri için Azure Service Bus bağlamaları"
description: "Azure Service Bus Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: daedacf0-6546-4355-a65c-50873e74f66b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/01/2017
ms.author: tdykstra
ms.openlocfilehash: 2df003d47291570b31e1091f34994e4023000981
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="azure-service-bus-bindings-for-azure-functions"></a>Azure işlevleri için Azure Service Bus bağlamaları

Bu makalede Azure işlevlerinde Azure Service Bus bağlamaları ile nasıl çalışılacağını açıklar. Tetikler ve Service Bus kuyrukları ve konuları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Tetikleyici

Hizmet veri yolu kuyruğu ya da konu iletilerine yanıt için Service Bus tetikleyici kullanın. 

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , hizmet veri yolu kuyruğu iletiyi günlüğe kaydeder.

```cs
[FunctionName("ServiceBusQueueTriggerCSharp")]                    
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] 
    string myQueueItem, 
    TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlevi bir Service Bus kuyruk iletisi günlüğe kaydeder.

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F # örnek

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlevi bir Service Bus kuyruk iletisi günlüğe kaydeder. 

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

F # betik kod aşağıdaki gibidir:

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama Service Bus tetikleyici gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlevi bir Service Bus kuyruk iletisi günlüğe kaydeder. 

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

JavaScript kodu şöyledir:

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), Service Bus tetikleyici yapılandırmak için aşağıdaki öznitelikler kullanın:

* [ServiceBusTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusTriggerAttribute.cs), NuGet paketi tanımlı [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Özniteliğin Oluşturucusu kuyruk veya konu ve abonelik adını alır. Bağlantının erişim hakları de belirtebilirsiniz. Erişim hakları belirtmezseniz, varsayılan değer `Manage`. Erişim hakları ayarlama seçme açıklandığı [tetikleyici - yapılandırma](#trigger---configuration) bölümü. Aşağıda, bir dize parametresi ile kullanılan öznitelik gösteren bir örnek verilmiştir:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue")] string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliği kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("ServiceBusQueueTriggerCSharp")]                    
  public static void Run(
      [ServiceBusTrigger("myqueue", Connection = "ServiceBusConnection")] 
      string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

  Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

* [ServiceBusAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAccountAttribute.cs), NuGet paketi tanımlı [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus)

  Kullanılacak hizmet veri yolu hesabını belirtmek için başka bir yol sağlar. Oluşturucusu hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [ServiceBusAccount("ClassLevelServiceBusAppSetting")]
  public static class AzureFunctions
  {
      [ServiceBusAccount("MethodLevelServiceBusAppSetting")]
      [FunctionName("ServiceBusQueueTriggerCSharp")]
      public static void Run(
          [ServiceBusTrigger("myqueue", AccessRights.Manage)] 
          string myQueueItem, TraceWriter log)
  {
      ...
  }
  ```

Hizmet veri yolu hesabı aşağıdaki sırayla belirlenir:

* `ServiceBusTrigger` Özniteliğin `Connection` özelliği.
* `ServiceBusAccount` Aynı parametre olarak uygulanan öznitelik `ServiceBusTrigger` özniteliği.
* `ServiceBusAccount` İşlevi için uygulanan öznitelik.
* `ServiceBusAccount` Sınıfına uygulanan öznitelik.
* "AzureWebJobsServiceBus" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `ServiceBusTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | "ServiceBusTrigger" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | "İçin" ayarlanması gerekir. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | İşlev kodu kuyruk veya konu iletisinde temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return" ayarlayın. | 
|**queueName**|**QueueName**|İzlemek için sırasının adı.  Yalnızca bir konu için bir sıra izliyorsanız seçin.
|**topicName**|**TopicName**|İzlemek için konu adı. Yalnızca bir sıra için bir konu izliyorsanız seçin.|
|**varlığıyla subscriptionName**|**Varlığıyla SubscriptionName**|İzlemek için Abonelik adı. Yalnızca bir sıra için bir konu izliyorsanız seçin.|
|**bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyServiceBus." adlı "MyServiceBus" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan hizmet veri yolu bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarını kullanır.<br><br>Bir bağlantı dizesi edinmek için gösterilen adımları izleyin [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır. |
|**erişimHakları**|**Erişim**|Bağlantı dizesi için erişim hakları. Kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, hangi gösterir `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izni, `accessRights` "dinlemek için". Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# kod kuyruk veya konu gibi bir yöntem parametresi kullanılarak erişim iletisi `string paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Yerine aşağıdaki türlerinden herhangi birini kullanabilirsiniz `string`:

* `byte[]`-İkili veriler için kullanışlıdır.
* JSON, ileti içeriyorsa, özel bir tür - Azure işlevleri JSON verilerini seri durumdan dener.
* `BrokeredMessage`-Seri durumdan çıkarılmış iletiyle verir [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) yöntemi.

JavaScript'te, kuyruk veya konu ileti kullanarak erişim `context.bindings.<name>`. `<name>`değeri belirtilen `name` özelliği *function.json*. Hizmet veri yolu ileti işlevine bir dize veya JSON nesnesi olarak geçirilir.

## <a name="trigger---poison-messages"></a>Tetikleyici - zarar iletileri

Zehirli ileti işleme denetlenen veya Azure işlevlerinde yapılandırılmamış. Hizmet veri yolu kendisi zarar iletileri işler.

## <a name="trigger---peeklock-behavior"></a>Tetikleyici - PeekLock davranışı

İşlevler çalışma zamanı bir iletisinde aldığı [PeekLock modu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode). Çağırır `Complete` işlevi başarıyla tamamlanırsa ileti veya çağrıları `Abandon` işlevi başarısız olursa. İşlev süreden uzun çalışırsa `PeekLock` zaman aşımı, kilidi otomatik olarak yenilenir.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#servicebus) dosyası Service Bus tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-event-hubs](../../includes/functions-host-json-service-bus.md)]

## <a name="output"></a>Çıktı

Kuyruk veya konu iletileri göndermek için Azure Service Bus çıkış bağlama kullanın.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) Service Bus kuyruğu ileti gönderir:

```cs
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string ServiceBusOutput([HttpTrigger] dynamic input, TraceWriter log)
{
    log.Info($"C# function processed: {input.Text}");
    return input.Text;
}
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

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

Tek bir ileti oluşturan bir C# kodu şöyledir:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Birden çok ileti oluşturan burada'nın C# betik kodu:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue messages created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

### <a name="output---f-example"></a>Çıktı - F # örnek

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [F # betik işlevi](functions-reference-fsharp.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

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

Tek bir ileti oluşturan bir F # kodu şöyledir:

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama Service Bus çıkış gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. 15 dakikada bir kuyruk iletisi göndermek için Zamanlayıcı tetikleyicisi işlevini kullanır.

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

Aşağıda, tek bir ileti oluşturur JavaScript kodu verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Aşağıda, birden fazla ileti oluşturur JavaScript kodu verilmiştir:

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = [];
    context.bindings.outputSbQueueMsg.push("1 " + message);
    context.bindings.outputSbQueueMsg.push("2 " + message);
    context.done();
};
```

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [ServiceBusAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.ServiceBus/ServiceBusAttribute.cs), NuGet paketi tanımlanan [Microsoft.Azure.WebJobs.ServiceBus](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.ServiceBus).

Özniteliğin Oluşturucusu kuyruk veya konu ve abonelik adını alır. Bağlantının erişim hakları de belirtebilirsiniz. Erişim hakları ayarlama seçme açıklandığı [çıktı - yapılandırma](#output---configuration) bölümü. Aşağıda, işlevin dönüş değeri için uygulanan öznitelik gösteren bir örnek verilmiştir:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliği kullanmak için Service Bus hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("ServiceBusOutput")]
[return: ServiceBus("myqueue", Connection = "ServiceBusConnection")]
public static string Run([HttpTrigger] dynamic input, TraceWriter log)
{
    ...
}
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

Kullanabileceğiniz `ServiceBusAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde kullanılacak hizmet veri yolu hesabını belirtin.  Daha fazla bilgi için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `ServiceBus` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**türü** | yok | "ServiceBus" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | yok | Out"için" olarak ayarlanmalıdır. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | yok | Sıra veya işlev kodu konudaki temsil eden değişken adı. İşlev dönüş değeri başvurmak için "$return" ayarlayın. | 
|**queueName**|**QueueName**|Kuyruk adı.  Yalnızca bir konu için sıraya ileti göndermek istiyorsanız ayarlayın.
|**topicName**|**TopicName**|İzlemek için konu adı. Yalnızca bir kuyruk için konu ileti göndermek istiyorsanız ayarlayın.|
|**varlığıyla subscriptionName**|**Varlığıyla SubscriptionName**|İzlemek için Abonelik adı. Yalnızca bir kuyruk için konu ileti göndermek istiyorsanız ayarlayın.|
|**bağlantı**|**Bağlantı**|Bu bağlama için kullanılacak hizmet veri yolu bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca kalanı adını belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyServiceBus." adlı "MyServiceBus" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan hizmet veri yolu bağlantı dizesi "AzureWebJobsServiceBus" adlı uygulama ayarını kullanır.<br><br>Bir bağlantı dizesi edinmek için gösterilen adımları izleyin [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials). Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır.|
|**erişimHakları**|**Erişim** |Bağlantı dizesi için erişim hakları. Değerleri, "manage" ve "dinleme" kullanılabilir. "Manage", bağlantı olduğunu belirten varsayılandır **Yönet** izinleri. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izinleri ayarlamanızı `accessRights` "dinlemek için". Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

C# ve C# betik kuyruk veya konu gibi bir yöntem parametresi kullanılarak erişim `out string paramName`. C# komut dosyası `paramName` içinde belirtilen değer `name` özelliği *function.json*. Aşağıdaki parametre türlerinden herhangi birini kullanabilirsiniz:

* `out T paramName` - `T`JSON seri hale getirilebilir türler olabilir. Parametre değeri null ise, işlev çıktığında işlevleri null bir nesne ile iletisi oluşturur.
* `out string`-İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.
* `out byte[]`-İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.
* `out BrokeredMessage`-İşlevi çıktığında parametre değeri null ise, işlevleri oluşturmaz bir ileti.

Bir C# veya C# betik işlevinde birden çok ileti oluşturmak için kullanabilirsiniz `ICollector<T>` veya `IAsyncCollector<T>`. Çağırdığınızda bir ileti oluşturulur `Add` yöntemi.

JavaScript'te, kuyruk veya konu başlığı kullanarak erişim `context.bindings.<name>`. `<name>`değeri belirtilen `name` özelliği *function.json*. Bir dize, bir bayt dizisi veya (JSON'a seri durumdan) bir Javascript nesnesi atayabileceğiniz `context.binding.<name>`.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
