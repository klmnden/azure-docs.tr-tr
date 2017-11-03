---
title: "Azure işlevleri Service Bus Tetikleyicileri ve bağlamaları | Microsoft Docs"
description: "Azure Service Bus Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: christopheranderson
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
ms.author: glenga
ms.openlocfilehash: 71149aaacc940a62e085cf1ce103a0214d05bd1c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-service-bus-bindings"></a>Azure işlevleri Service Bus bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırma ve Azure Service Bus bağlamaları Azure işlevlerinde çalışmak açıklanmaktadır. 

Tetikler ve Service Bus kuyrukları ve konuları için bağlamaları çıktı Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="service-bus-trigger"></a>Hizmet veri yolu tetikleyici
Hizmet veri yolu kuyruğu ya da konu iletilerine yanıt için Service Bus tetikleyici kullanın. 

Hizmet veri yolu kuyruk ve konu Tetikleyicileri aşağıdaki JSON nesneler tarafından tanımlanan `bindings` function.json dizisi:

* *sıra* tetikleyici:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

* *konu* tetikleyici:

    ```json
    {
        "name" : "<Name of input parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBusTrigger",
        "direction" : "in"
    }
    ```

Şunlara dikkat edin:

* İçin `connection`, [işlevi uygulamanıza bir uygulama ayarı oluşturmak](functions-how-to-use-azure-function-app-settings.md) hizmet veri yolu ad alanınıza bağlantı dizesi içeren, uygulama ayarı adı belirtin `connection` , tetikleyici bir özellik. Konumundaki gösterilen adımları izleyerek bağlantı dizesini elde [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır.
  Bırakır `connection` boş, tetikleyici bir varsayılan hizmet veri yolu bağlantı dizesi ayarı adlandırılmış bir uygulamada belirttiğinizi varsayar `AzureWebJobsServiceBus`.
* İçin `accessRights`, kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, hangi gösterir `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izni, `accessRights` için `listen`. Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin.

## <a name="trigger-behavior"></a>Tetikleyici davranışı
* **Tek iş parçacığı oluşturma** - varsayılan olarak, çoklu iletileri aynı anda işlevleri çalışma zamanı işlemler. Bir kerede yalnızca bir tek kuyruk veya konu ileti işleme için çalışma zamanı yönlendirmek için ayarlanmış `serviceBus.maxConcurrentCalls` 1 olarak *host.json*. 
  Hakkında bilgi için *host.json*, bkz: [klasör yapısını](functions-reference.md#folder-structure) ve [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json).
* **Zehirli ileti işleme** -Service Bus, denetlenmesi veya Azure işlevleri yapılandırma veya kod yapılandırılmış kendi zehirli ileti işleme yapar. 
* **PeekLock davranışı** -işlevler çalışma zamanı alan bir ileti [ `PeekLock` modu](../service-bus-messaging/service-bus-performance-improvements.md#receive-mode) ve çağrıları `Complete` işlevi başarıyla tamamlanırsa ileti veya çağrıları `Abandon` varsa işlevi başarısız olur. 
  İşlev süreden uzun çalışırsa `PeekLock` zaman aşımı, kilidi otomatik olarak yenilenir.

<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Tetikleyici kullanımı
Bu bölümde işlevi kodunuzda, Service Bus tetikleyici kullanmayı gösterir. 

C# ve F #'de Service Bus tetikleyici ileti herhangi aşağıdaki giriş türleri seri durumdan çıkarılabiliyorsa:

* `string`-dize iletileri için faydalı
* `byte[]`-ikili veriler için kullanışlıdır
* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON serileştirilmiş veriler için kullanışlıdır.
  Özel bir giriş türü gibi bildirme varsa `CustomType`, Azure işlevleri, belirtilen türe JSON verilerini seri durumdan dener.
* `BrokeredMessage`-ile seri durumdan çıkarılmış mesajıyla [BrokeredMessage.GetBody<T>()](https://msdn.microsoft.com/library/hh144211.aspx) yöntemi.

Node.js içinde Service Bus tetikleyici ileti işlevine bir dize veya JSON nesnesi olarak geçirilir.

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Tetikleyici örnek
Aşağıdaki function.json olduğunu varsayalım:

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

Hizmet veri yolu kuyruğu iletisini işler dile özgü örneğe bakın.

* [C#](#triggercsharp)
* [F#](#triggerfsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Tetikleyici örnek C# #

```cs
public static void Run(string myQueueItem, TraceWriter log)
{
    log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
}
```

<a name="triggerfsharp"></a>

### <a name="trigger-sample-in-f"></a>F # tetikleyici örnek #

```fsharp
let Run(myQueueItem: string, log: TraceWriter) =
    log.Info(sprintf "F# ServiceBus queue trigger function processed message: %s" myQueueItem)
```

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Node.js tetikleyici örnek

```javascript
module.exports = function(context, myQueueItem) {
    context.log('Node.js ServiceBus queue trigger function processed message', myQueueItem);
    context.done();
};
```

<a name="output"></a>

## <a name="service-bus-output-binding"></a>Hizmet veri yolu bağlama çıktı
Hizmet veri yolu kuyruk ve konu çıkış bir işlev için aşağıdaki JSON nesneleri kullan `bindings` function.json dizisi:

* *sıra* çıktı:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "queueName" : "<Name of the queue>",
        "connection" : "<Name of app setting that has your queue's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```
* *konu* çıktı:

    ```json
    {
        "name" : "<Name of output parameter in function signature>",
        "topicName" : "<Name of the topic>",
        "subscriptionName" : "<Name of the subscription>",
        "connection" : "<Name of app setting that has your topic's connection string - see below>",
        "accessRights" : "<Access rights for the connection string - see below>",
        "type" : "serviceBus",
        "direction" : "out"
    }
    ```

Şunlara dikkat edin:

* İçin `connection`, [işlevi uygulamanıza bir uygulama ayarı oluşturmak](functions-how-to-use-azure-function-app-settings.md) hizmet veri yolu ad alanınıza bağlantı dizesi içeren, uygulama ayarı adı belirtin `connection` çıkış bağlama özelliği. Konumundaki gösterilen adımları izleyerek bağlantı dizesini elde [yönetim kimlik bilgileri elde](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md#obtain-the-management-credentials).
  Bağlantı dizesi, belirli bir kuyruğa ya da konu bunlarla sınırlı olmamak bir hizmet veri yolu ad alanı için olmalıdır.
  Bırakır `connection` boş çıkış bağlama varsayılan hizmet veri yolu bağlantı dizesi ayarı adlandırılmış bir uygulamada belirttiğinizi varsayar `AzureWebJobsServiceBus`.
* İçin `accessRights`, kullanılabilir değerler `manage` ve `listen`. Varsayılan değer `manage`, hangi gösterir `connection` sahip **Yönet** izni. Sahip olmayan bir bağlantı dizesi kullanıyorsanız **Yönet** izni, `accessRights` için `listen`. Aksi halde, çalışma zamanı gerektiren işlemleri yapmaya başarısız olabilir işlevleri hakları yönetin.

<a name="outputusage"></a>

## <a name="output-usage"></a>Çıktı kullanımı
C# ve F #'de Azure işlevleri şu türlerden birini bir Service Bus kuyruk iletisi oluşturun:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) -parametre tanımına arar gibi `out T paramName` (C#).
  İşlevler JSON iletiye nesne seri durumdan çıkarır. Çıkış değeri null ise, işlev çıktığında işlevleri null bir nesne ile iletisi oluşturur.
* `string`-Parametre tanımına arar gibi `out string paraName` (C#). Parametre değeri null olmayan ise işlevi çıktığında işlevleri bir ileti oluşturur.
* `byte[]`-Parametre tanımına arar gibi `out byte[] paraName` (C#). Parametre değeri null olmayan ise işlevi çıktığında işlevleri bir ileti oluşturur.
* `BrokeredMessage`Parametre tanımına arar gibi `out BrokeredMessage paraName` (C#). Parametre değeri null olmayan ise işlevi çıktığında işlevleri bir ileti oluşturur.

C# işlevinde birden çok ileti oluşturmak için kullanabilirsiniz `ICollector<T>` veya `IAsyncCollector<T>`. Çağırdığınızda bir ileti oluşturulur `Add` yöntemi.

Node.js içinde bir dize, bir bayt dizisi veya (JSON'a seri durumdan) bir Javascript nesnesi atayabileceğiniz `context.binding.<paramName>`.

<a name="outputsample"></a>

## <a name="output-sample"></a>Çıkış örneği
Hizmet veri yolu kuyruğu çıkış tanımlayan aşağıdaki function.json olduğunu varsayalım:

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

Service bus kuyruğuna ileti gönderir dile özgü örneğe bakın.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>C# çıktı örneği #

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, out string outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue = message;
}
```

Veya birden çok iletileri oluşturmak için:

```cs
public static void Run(TimerInfo myTimer, TraceWriter log, ICollector<string> outputSbQueue)
{
    string message = $"Service Bus queue message created at: {DateTime.Now}";
    log.Info(message); 
    outputSbQueue.Add("1 " + message);
    outputSbQueue.Add("2 " + message);
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>F # çıktı örneği #

```fsharp
let Run(myTimer: TimerInfo, log: TraceWriter, outputSbQueue: byref<string>) =
    let message = sprintf "Service Bus queue message created at: %s" (DateTime.Now.ToString())
    log.Info(message)
    outputSbQueue = message
```

<a name="outnodejs"></a>

### <a name="output-sample-in-nodejs"></a>Node.js çıktı örneği

```javascript
module.exports = function (context, myTimer) {
    var message = 'Service Bus queue message created at ' + timeStamp;
    context.log(message);   
    context.bindings.outputSbQueueMsg = message;
    context.done();
};
```

Veya birden çok iletileri oluşturmak için:

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

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

