---
title: "Azure işlevleri kuyruk depolama bağlamaları | Microsoft Docs"
description: "Azure Storage Tetikleyicileri ve bağlamaları Azure işlevlerinde kullanmayı öğrenme."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: 4e6a837d-e64f-45a0-87b7-aa02688a75f3
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: b68ce106ceb25d19ee0bbde287891d553a448560
ms.sourcegitcommit: 963e0a2171c32903617d883bb1130c7c9189d730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/20/2017
---
# <a name="azure-functions-queue-storage-bindings"></a>Azure işlevleri kuyruk depolama bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod Azure kuyruk depolama bağlamaları Azure işlevlerinde açıklanmaktadır. Tetikler ve Azure sıralar bağlamaları çıktı Azure işlevleri destekler. Tüm bağlama kullanılabilir olan özellikler için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

<a name="trigger"></a>

## <a name="queue-storage-trigger"></a>Kuyruk depolama tetikleyici
Azure kuyruk depolama tetikleyici yeni iletiler için bir kuyruk depolama izlemek ve bunlara tepki sağlar. 

Kullanarak bir sıra tetikleyici tanımlamak **tümleştir** işlevleri portalındaki sekmesi. Portal aşağıdaki tanımında oluşturur **bağlamaları** bölümünü *function.json*:

```json
{
    "type": "queueTrigger",
    "direction": "in",
    "name": "<The name used to identify the trigger data in your code>",
    "queueName": "<Name of queue to poll>",
    "connection":"<Name of app setting - see below>"
}
```

* `connection` Özelliği, depolama bağlantı dizesi içeren bir uygulama ayarı adı içermelidir. Standart Düzenleyici'de Azure portalında **tümleştir** sekmesinde bir depolama hesabı seçtiğinizde, bu uygulama ayarı yapılandırır.

Ek ayarları sağlanabilir bir [host.json dosya](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) daha fazla kuyruk depolama Tetikleyicileri ince ayar yapmak için. Örneğin, yoklama aralığı host.json içinde sıra değiştirebilirsiniz.

<a name="triggerusage"></a>

## <a name="using-a-queue-trigger"></a>Bir kuyruk Tetikleyici kullanma
Node.js işlevlerde kuyruğu kullanarak veri erişim `context.bindings.<name>`.


.NET işlevlerde gibi bir yöntem parametresi kullanılarak sıra yükü erişim `CloudQueueMessage paramName`. Burada, `paramName` , belirtilen değer [tetikleyici yapılandırma](#trigger). Kuyruk iletisini şu türlerden birine seri durumdan çıkarılabiliyorsa:

* POCO nesnesi. Kuyruk yükü bir JSON nesnesi ise kullanın. İşlevler çalışma zamanı POCO nesnesine yükü seri durumdan çıkarır. 
* `string`
* `byte[]`
* [`CloudQueueMessage`]

<a name="meta"></a>

### <a name="queue-trigger-metadata"></a>Sıra tetikleyici meta verileri
Sıra tetikleyici çeşitli meta veri özelliklerini sağlar. Bu özellikler, diğer bağlamaları bağlama ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Değerleri olarak aynı semantiklerine sahip [ `CloudQueueMessage` ].

* **QueueTrigger** -sıra Yükü (geçerli bir dize ise)
* **DequeueCount** -türü `int`. Bu iletiyi kuyruktan çıkarıldı sayısı.
* **ExpirationTime** -türü `DateTimeOffset?`. İleti sona ereceği saat.
* **Kimliği** -türü `string`. Sıra ileti kimliği.
* **InsertionTime** -türü `DateTimeOffset?`. İleti sıraya eklenen süre.
* **NextVisibleTime** -türü `DateTimeOffset?`. Sonraki ileti görebileceği süre.
* **PopReceipt** -türü `string`. İleti pop giriş.

Sıra meta verilerde kullanılması hakkında bilgi [tetikleyici örnek](#triggersample).

<a name="triggersample"></a>

## <a name="trigger-sample"></a>Tetikleyici örnek
Bir kuyruk tetikleyici tanımlar aşağıdaki function.json olduğunu varsayalım:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionString"
        }
    ]
}
```

Alan ve sıra meta veri günlüklerini dile özgü örneğine bakın.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-sample-in-c"></a>Tetikleyici örnek C# #
```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Queue;
using System;

public static void Run(CloudQueueMessage myQueueItem, 
    DateTimeOffset expirationTime, 
    DateTimeOffset insertionTime, 
    DateTimeOffset nextVisibleTime,
    string queueTrigger,
    string id,
    string popReceipt,
    int dequeueCount,
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger sample in F# ## 
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-sample-in-nodejs"></a>Node.js tetikleyici örnek

```javascript
module.exports = function (context) {
    context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('queueTrigger =', context.bindingData.queueTrigger);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id=', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

### <a name="handling-poison-queue-messages"></a>Zarar iletileri işleme
Bir sıra Tetik işlevi başarısız olduğunda, Azure işlevleri bu işlev en fazla beş kez ilk denemede içeren verilen kuyruk iletisi için yeniden dener. Tüm beş deneme başarısız olursa işlevleri çalışma zamanı bir ileti adlı bir kuyruk depolama ekler.  *&lt;originalqueuename >-zararlı*. Günlüğe yazma veya el ile ilgili dikkat bir bildirim göndererek zararlı sırasından iletilerini işlemek için bir işlev gerekli yazabilirsiniz. 

El ile zarar iletileri işlemek için denetleyin `dequeueCount` kuyruk iletisinin (bkz [sıra tetikleyici meta verileri](#meta)).

<a name="output"></a>

## <a name="queue-storage-output-binding"></a>Kuyruk depolama bağlama çıktı
Azure kuyruk depolama çıkış bağlama kuyruğa ileti yazmak etkinleştirir. 

Bir kuyruk çıkış bağlama kullanarak tanımlarsınız **tümleştir** işlevleri portalındaki sekmesi. Portal aşağıdaki tanımında oluşturur **bağlamaları** bölümünü *function.json*:

```json
{
   "type": "queue",
   "direction": "out",
   "name": "<The name used to identify the trigger data in your code>",
   "queueName": "<Name of queue to write to>",
   "connection":"<Name of app setting - see below>"
}
```

* `connection` Özelliği, depolama bağlantı dizesi içeren bir uygulama ayarı adı içermelidir. Standart Düzenleyici'de Azure portalında **tümleştir** sekmesinde bir depolama hesabı seçtiğinizde, bu uygulama ayarı yapılandırır.

<a name="outputusage"></a>

## <a name="using-a-queue-output-binding"></a>Bir kuyruk kullanarak çıktıyı bağlama
Çıkış sırası kullanarak erişim node.js işlevlerde `context.bindings.<name>`.

.NET işlevlerde şu türlerden birine çıkarabilirsiniz. Tür parametresi olduğunda `T`, `T` desteklenen çıktı türlerinden birini gibi olmalıdır `string` veya bir POCO.

* `out T`(JSON olarak serileştirilmiş)
* `out string`
* `out byte[]`
* `out` [`CloudQueueMessage`] 
* `ICollector<T>`
* `IAsyncCollector<T>`
* [`CloudQueue`](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue)

Yöntemin dönüş türü olarak çıkış bağlama de kullanabilirsiniz.

<a name="outputsample"></a>

## <a name="queue-output-sample"></a>Sıra çıktı örneği
Aşağıdaki *function.json* bir HTTP tetikleyicisi bir sıra ile tanımlar bağlama çıktı:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "function",
      "name": "input"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "$return",
      "queueName": "outqueue",
      "connection": "MyStorageConnectionString",
    }
  ]
}
``` 

Gelen HTTP yükü ile bir kuyruk iletisi çıkarır dile özgü örneğe bakın.

* [C#](#outcsharp)
* [Node.js](#outnodejs)

<a name="outcsharp"></a>

### <a name="queue-output-sample-in-c"></a>C# sıra çıktı örneği #

```cs
// C# example of HTTP trigger binding to a custom POCO, with a queue output binding
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, TraceWriter log)
{
    return input;
}
```

Birden çok iletileri göndermek için kullandıkları bir `ICollector`:

```cs
public static void Run(CustomQueueMessage input, ICollector<CustomQueueMessage> myQueueItem, TraceWriter log)
{
    myQueueItem.Add(input);
    myQueueItem.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

<a name="outnodejs"></a>

### <a name="queue-output-sample-in-nodejs"></a>Node.js sıra çıktı örneği

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Veya birden çok ileti göndermek için

```javascript
module.exports = function(context) {
    // Define a message array for the myQueueItem output binding. 
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar

Kuyruk depolama Tetikleyicileri ve bağlamaları kullanan bir işlev örneği için bkz: [Azure kuyruk Depolama tarafından tetiklenen bir işlev oluşturun](functions-create-storage-queue-triggered-function.md).

[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

<!-- LINKS -->

['CloudQueueMessage']: /dotnet/api/microsoft.windowsazure.storage.queue.cloudqueuemessage
