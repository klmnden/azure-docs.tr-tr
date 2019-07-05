---
title: Azure işlevleri için Azure kuyruk depolama bağlamaları
description: Azure kuyruk depolama tetikleyicisi ve çıktı bağlaması Azure işlevleri'nde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/03/2018
ms.author: cshoe
ms.custom: cc996988-fb4f-47
ms.openlocfilehash: 9604ef276625d1fcc9164a9b75b94ebc22cb51e1
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480150"
---
# <a name="azure-queue-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure kuyruk depolama bağlamaları

Bu makalede, Azure işlevleri'nde Azure kuyruk depolama bağlamaları ile nasıl çalışılacağı açıklanmaktadır. Tetikleme ve çıktı bağlaması sıralar için Azure işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Kuyruk depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Queue) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Kuyruk depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Queues) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="encoding"></a>Encoding
İşlevleri beklediğiniz bir *base64* kodlamalı dize. Ayarlamaları kodlama türü için (veri olarak hazırlamak için bir *base64* kodlamalı dize) arama hizmetinde uygulanması gerekir.

## <a name="trigger"></a>Tetikleyici

Kuyruk tetikleyicisi, bir kuyruğa yeni öğe alındığında bir işlev başlatmak için kullanın. Kuyruk iletisi işleve giriş olarak sağlanır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)
* [Java](#trigger---java-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , yoklar `myqueue-items` sıraya almak ve bir günlük kuyruğu öğesinin işlendiği her zaman yazar.

```csharp
public static class QueueFunctions
{
    [FunctionName("QueueTrigger")]
    public static void QueueTrigger(
        [QueueTrigger("myqueue-items")] string myQueueItem, 
        ILogger log)
    {
        log.LogInformation($"C# function processed: {myQueueItem}");
    }
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir kuyruk tetikleyicisi bağlama gösterir. bir *function.json* dosya ve [C# betiği (.csx)](functions-reference-csharp.md) bağlama kullanan kod. İşlev yoklamalar `myqueue-items` sıraya almak ve bir günlük kuyruğu öğesinin işlendiği her zaman yazar.

İşte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```csharp
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.Extensions.Logging;
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
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem.AsString}\n" +
        $"queueTrigger={queueTrigger}\n" +
        $"expirationTime={expirationTime}\n" +
        $"insertionTime={insertionTime}\n" +
        $"nextVisibleTime={nextVisibleTime}\n" +
        $"id={id}\n" +
        $"popReceipt={popReceipt}\n" + 
        $"dequeueCount={dequeueCount}");
}
```

[Kullanım](#trigger---usage) bölümde açıklanmaktadır `myQueueItem`, tarafından adlı `name` function.json özelliği.  [İleti meta veriler bölümü](#trigger---message-metadata) tüm gösterilen değişkenleri açıklar.

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir kuyruk tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev yoklamalar `myqueue-items` sıraya almak ve bir günlük kuyruğu öğesinin işlendiği her zaman yazar.

İşte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "type": "queueTrigger",
            "direction": "in",
            "name": "myQueueItem",
            "queueName": "myqueue-items",
            "connection":"MyStorageConnectionAppSetting"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

> [!NOTE]
> Name parametresi olarak yansıtır `context.bindings.<name>` Kuyruk öğesi yükünü içeren JavaScript kodunda. Bu yük, aynı zamanda ikinci parametre olarak işleve geçirilir.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = async function (context, message) {
    context.log('Node.js queue trigger function processed work item', message);
    // OR access using context.bindings.<name>
    // context.log('Node.js queue trigger function processed work item', context.bindings.myQueueItem);
    context.log('expirationTime =', context.bindingData.expirationTime);
    context.log('insertionTime =', context.bindingData.insertionTime);
    context.log('nextVisibleTime =', context.bindingData.nextVisibleTime);
    context.log('id =', context.bindingData.id);
    context.log('popReceipt =', context.bindingData.popReceipt);
    context.log('dequeueCount =', context.bindingData.dequeueCount);
    context.done();
};
```

[Kullanım](#trigger---usage) bölümde açıklanmaktadır `myQueueItem`, tarafından adlı `name` function.json özelliği.  [İleti meta veriler bölümü](#trigger---message-metadata) tüm gösterilen değişkenleri açıklar.

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki Java örneğinde kuyruğuna yerleştirilir tetiklenen ileti günlüğe kaydeden işlevleri depolama kuyruğu tetikleyici gösterir `myqueuename`.

 ```java
 @FunctionName("queueprocessor")
 public void run(
    @QueueTrigger(name = "msg",
                   queueName = "myqueuename",
                   connection = "myconnvarname") String message,
     final ExecutionContext context
 ) {
     context.getLogger().info(message);
 }
 ```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kuyruk tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [QueueTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueTriggerAttribute.cs)

  Özniteliğin Oluşturucusu izlemek için kuyruk adı aşağıdaki örnekte gösterildiği gibi alır:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items")] string myQueueItem, 
      ILogger log)
  {
      ...
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

  ```csharp
  [FunctionName("QueueTrigger")]
  public static void Run(
      [QueueTrigger("myqueue-items", Connection = "StorageConnectionAppSetting")] string myQueueItem, 
      ILogger log)
  {
      ....
  }
  ```

  Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu bir depolama bağlantı dizesi içeren bir uygulama ayarı adı alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("QueueTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ...
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `QueueTrigger` Özniteliğin `Connection` özelliği.
* `StorageAccount` Özniteliği aynı parametresine uygulanan `QueueTrigger` özniteliği.
* `StorageAccount` İşleve uygulanmış bir öznitelik.
* `StorageAccount` Sınıfına uygulanan bir öznitelik.
* "AzureWebJobsStorage" uygulama ayarı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `QueueTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok| Ayarlanmalıdır `queueTrigger`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction**| yok | İçinde *function.json* yalnızca dosya. Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok |İşlev kodunu Kuyruk öğesi yükteki içeren değişkenin adı.  |
|**queueName** | **queueName**| Yoklama kurulacak Kuyruğun adı. |
|**bağlantı** | **bağlantı** |Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# betik gibi bir yöntem parametresi kullanarak ileti veri erişim `string paramName`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. Şu türlerden birine bağlayabilirsiniz:

* Nesnesi - işlevler çalışma zamanı bir JSON yüküne kodunuzda tanımlanan rastgele bir sınıfın bir örneği seri durumdan çıkarır. 
* `string`
* `byte[]`
* [CloudQueueMessage]

Bağlanılacak çalışırsanız `CloudQueueMessage` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

JavaScript'te kullanın `context.bindings.<name>` Kuyruk öğesi yükü erişmek için. JSON yükü ise bir nesnede seri durumda.

## <a name="trigger---message-metadata"></a>Tetikleyici - ileti meta verileri

Kuyruk tetikleyicisi birkaç sağlar [meta veri özelliklerini](./functions-bindings-expressions-patterns.md#trigger-metadata). Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu özellikleri olan [CloudQueueMessage](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage) sınıfı.

|Özellik|Tür|Açıklama|
|--------|----|-----------|
|`QueueTrigger`|`string`|Kuyruk Yükü (geçerli bir dize değilse). Kuyruk ileti yükü olarak bir dize ise `QueueTrigger` tarafından adlı değişken aynı değere sahip `name` özelliğinde *function.json*.|
|`DequeueCount`|`int`|Bu iletiyi sıradan çıkarılan sayısı.|
|`ExpirationTime`|`DateTimeOffset`|İleti süresinin dolduğu zaman.|
|`Id`|`string`|Kuyruk ileti kimliği.|
|`InsertionTime`|`DateTimeOffset`|İleti kuyruğuna eklenen süre.|
|`NextVisibleTime`|`DateTimeOffset`|İletiyi sonraki görünür olacak süre.|
|`PopReceipt`|`string`|İletinin üstten alma girişi.|

## <a name="trigger---poison-messages"></a>Tetikleyici - zehirli iletiler

Kuyruğu tetikleme işlevi başarısız olduğunda, Azure işlevleri işlev en fazla beş kez bir verilen kuyruk iletisi için ilk denemede dahil olmak üzere yeniden dener. Tüm beş denemesi başarısız olursa, İşlevler çalışma zamanı adlı bir kuyruğa bir ileti ekler  *&lt;originalqueuename >-zehirli*. İşlem iletileri bir işleve zehirli kuyruktan günlüğe yazma ya da bir bildirim göndererek el ile ilgili dikkat edilmesi gereken yazabilirsiniz.

Zehirli iletiler el ile işlemek için denetleme [dequeueCount](#trigger---message-metadata) kuyruk iletisi.

## <a name="trigger---polling-algorithm"></a>Tetikleyici - yoklama algoritması

Kuyruk tetikleyicisi boş kuyruk üzerinde depolama işlem maliyetleri yoklama etkisini azaltmak için bir rastgele üstel geri alma algoritması uygular.  Bir ileti bulunduğunda, çalışma zamanının iki saniye bekler ve ardından başka bir ileti için denetler; ileti bulunduğunda, yeniden denemeden önce yaklaşık dört saniye bekler. Bir kuyruk iletisi almak için sonraki başarısız girişimden sonra bekleme süresini bir dakika için varsayılan olarak en fazla bekleme zamanı ulaşıncaya kadar artmaya devam eder. En fazla bekleme zamanı aracılığıyla yapılandırılabilir `maxPollingInterval` özelliğinde [host.json dosya](functions-host-json.md#queues).

## <a name="trigger---concurrency"></a>Tetikleyici - eşzamanlılık

Birden çok kuyruk iletisi bekleyen olduğunda, kuyruk tetikleyicisi toplu olarak mesaj alır ve eşzamanlı olarak işlemek için örnekleri işlevi çağırır. Varsayılan olarak, toplu iş boyutu 16'dır. İşlenmekte olan sayısı 8'e aldığında, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. Bu nedenle en fazla bir eş zamanlı ileti işlevi bir sanal makine'de (VM) başına işlenen sayısı 24'tür. Bu sınırı her kuyruk ile tetiklenen bir işlev her VM'de ayrı ayrı uygular. İşlev uygulamanız için birden çok VM Ölçeklendirmesi eşitlenene, her VM için Tetikleyiciler bekleyin ve işlevleri çalıştırma. Bir işlev uygulaması için 3 VM Ölçeklendirmesi eşitlenene, örneğin, varsayılan en fazla eş zamanlı örnekleri kuyruk ile tetiklenen bir işlev 72 sayısıdır.

Toplu iş boyutu ve yeni bir batch başlama eşiği yapılandırılabilir [host.json dosya](functions-host-json.md#queues). Paralel yürütme kuyruğu ile tetiklenen bir işlev uygulaması işlevler için en aza indirmek istiyorsanız, toplu iş boyutu 1 olarak ayarlayabilirsiniz. Bu ayar, yalnızca tek bir sanal makineye (VM) işlev uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. 

Kuyruk tetikleyicisi otomatik olarak bir işleve bir kuyruk iletisi birden çok kez önlediği; işlevleri kere etkili olacak şekilde yazılması gerekmez.

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md#queues) dosyası kuyruğu tetikleyici davranışını denetleyen ayarları içerir. Bkz: [host.json ayarları](#hostjson-settings) kullanılabilir ayarlar ile ilgili ayrıntıları bölümü.

## <a name="output"></a>Output

Azure kuyruk depolama çıkış bağlaması bir kuyruğa ileti yazmak için kullanın.

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)
* [Java](#output---java-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) alınan her HTTP isteği için bir kuyruk iletisi oluşturur.

```csharp
[StorageAccount("AzureWebJobsStorage")]
public static class QueueFunctions
{
    [FunctionName("QueueOutput")]
    [return: Queue("myqueue-items")]
    public static string QueueOutput([HttpTrigger] dynamic input,  ILogger log)
    {
        log.LogInformation($"C# function processed: {input.Text}");
        return input.Text;
    }
}
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

Aşağıdaki örnek, bağlama HTTP tetikleyicisi gösterir. bir *function.json* dosya ve [C# betiği (.csx)](functions-reference-csharp.md) bağlama kullanan kod. İşlevi bir sıra öğesiyle oluşturur bir **CustomQueueMessage** alınan her HTTP isteği için nesne yükü.

İşte *function.json* dosyası:

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
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

Tek bir kuyruk iletisinin oluşturan C# betik kodunu şu şekildedir:

```cs
public class CustomQueueMessage
{
    public string PersonName { get; set; }
    public string Title { get; set; }
}

public static CustomQueueMessage Run(CustomQueueMessage input, ILogger log)
{
    return input;
}
```

Kullanarak aynı anda birden çok ileti gönderebilir bir `ICollector` veya `IAsyncCollector` parametresi. HTTP isteği verilerini biri diğeri sabit kodlu değer ile birden çok ileti gönderen aşağıda verilmiştir; C# betik kodunu:

```cs
public static void Run(
    CustomQueueMessage input, 
    ICollector<CustomQueueMessage> myQueueItems, 
    ILogger log)
{
    myQueueItems.Add(input);
    myQueueItems.Add(new CustomQueueMessage { PersonName = "You", Title = "None" });
}
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

Aşağıdaki örnek, bağlama HTTP tetikleyicisi gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, alınan her HTTP isteği için bir kuyruk öğesi oluşturur.

İşte *function.json* dosyası:

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
      "connection": "MyStorageConnectionAppSetting"
    }
  ]
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, input) {
    context.done(null, input.body);
};
```

Bir ileti dizisi için tanımlayarak aynı anda birden çok ileti gönderebilir `myQueueItem` çıktı bağlaması. Aşağıdaki JavaScript kodunu alınan her HTTP isteği için sabit kodlanmış değerlerle iki kuyruk iletileri gönderir.

```javascript
module.exports = function(context) {
    context.bindings.myQueueItem = ["message 1","message 2"];
    context.done();
};
```

### <a name="output---java-example"></a>Çıkış - Java örnek

 Aşağıdaki örnek, bir HTTP isteği tarafından tetiklendiğinde bir kuyruk iletisi için oluşturan bir Java işlev gösterir.

```java
@FunctionName("httpToQueue")
@QueueOutput(name = "item", queueName = "myqueue-items", connection = "AzureWebJobsStorage")
 public String pushToQueue(
     @HttpTrigger(name = "request", methods = {HttpMethod.POST}, authLevel = AuthorizationLevel.ANONYMOUS)
     final String message,
     @HttpOutput(name = "response") final OutputBinding&lt;String&gt; result) {
       result.setValue(message + " has been added.");
       return message;
 }
```

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@QueueOutput` ek açıklama parametreleri değeri kuyruk Depolama'ya yazılan.  Parametre türü olmalıdır `OutputBinding<T>`, burada T bir POJO'ya herhangi bir yerel Java türü.


## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [QueueAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/QueueAttribute.cs).

Öznitelik bir `out` parametresi veya işlevin dönüş değeri. Özniteliğin Oluşturucusu Kuyruğun adı aşağıdaki örnekte gösterildiği gibi alır:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("QueueOutput")]
[return: Queue("myqueue-items", Connection = "StorageConnectionAppSetting")]
public static string Run([HttpTrigger] dynamic input,  ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

Kullanabileceğiniz `StorageAccount` sınıf, yöntem ya da parametre düzeyinde depolama hesabı belirtmek için özniteliği. Daha fazla bilgi için tetikleyici - öznitelikleri bakın.

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Queue` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `queue`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | Ayarlanmalıdır `out`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | yok | İşlev kodunu kuyrukta temsil eden değişken adı. Kümesine `$return` işlev dönüş değeri başvurmak için.|
|**queueName** |**queueName** | Kuyruğun adı. |
|**bağlantı** | **bağlantı** |Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

Bir yöntem parametresi gibi kullanarak tek bir kuyruk iletisinin, C# ve C# betik yazma `out T paramName`. C# komut dosyası `paramName` değeri belirtilen `name` özelliği *function.json*. Yöntem dönüş türü yerine kullanabileceğiniz bir `out` parametresi ve `T` şu türlerden biri olabilir:

* JSON olarak seri hale getirilebilir bir nesne
* `string`
* `byte[]`
* [CloudQueueMessage] 

Bağlanılacak çalışırsanız `CloudQueueMessage` ve bir hata iletisi alırsanız, bir başvuru olduğundan emin olun [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

C# ve C# betiği şu türlerden birini kullanarak birden fazla kuyruk iletileri yazın: 

* `ICollector<T>` veya `IAsyncCollector<T>`
* [CloudQueue](/dotnet/api/microsoft.azure.storage.queue.cloudqueue)

JavaScript işlevleri'nde kullanmak `context.bindings.<name>` çıkış kuyruk iletisi erişmek için. Bir dize veya JSON seri hale getirilebilir bir nesne için kuyruk öğesi yükü kullanabilirsiniz.


## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama |  Başvuru |
|---|---|
| Kuyruk | [Kuyruk hata kodları](https://docs.microsoft.com/rest/api/storageservices/queue-service-error-codes) |
| BLOB, tablo, kuyruk | [Depolama hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tablo, kuyruk |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

<a name="host-json"></a>  

## <a name="hostjson-settings"></a>Host.JSON ayarları

Bu bölümde sürümünde bu bağlama için kullanılabilen genel yapılandırma ayarları açıklanmaktadır 2.x. Aşağıdaki örnek host.json dosyasını yalnızca bu bağlama için sürüm 2.x ayarları içerir. Sürümündeki genel yapılandırma ayarları hakkında daha fazla bilgi için 2.x bkz [sürümü Azure işlevleri için host.json başvurusu 2.x](functions-host-json.md).

> [!NOTE]
> İşlevlerde host.json başvurusu için 1.x, bkz: [Azure işlevleri için host.json başvurusu 1.x](functions-host-json-v1.md).

```json
{
    "version": "2.0",
    "extensions": {
        "queues": {
            "maxPollingInterval": "00:00:02",
            "visibilityTimeout" : "00:00:30",
            "batchSize": 16,
            "maxDequeueCount": 5,
            "newBatchThreshold": 8
        }
    }
}
```


|Özellik  |Varsayılan | Açıklama |
|---------|---------|---------|
|maxPollingInterval|00:00:01|Kuyruk arasındaki en büyük aralık yoklar. En düşük 00:00:00.100 (100 ms) olduğu ve en fazla 00:01:00 (1 dak) artırır. |
|visibilityTimeout|00:00:00|Bir ileti işlenirken yeniden denemeler arasındaki zaman aralığını başarısız olur. |
|batchSize|16|İşlevler çalışma zamanı aynı anda alır ve paralel olarak işler sıra iletilerinin sayısı. İşlenmekte olan sayı ne zaman alır aşağı `newBatchThreshold`, çalışma zamanı başka bir toplu iş alır ve bu iletileri işlemeye başlıyor. Eş zamanlı iletileri işlev işlenen sayısı, bu nedenle `batchSize` artı `newBatchThreshold`. Bu sınır, ayrı ayrı her kuyruk ile tetiklenen bir işlev için de geçerlidir. <br><br>Paralel yürütme bir kuyruğa alınan iletileri önlemek istiyorsanız, ayarlayabileceğiniz `batchSize` 1. Ancak, bu ayar yalnızca tek bir sanal makineye (VM) işlev uygulamanızın çalıştırdığı sürece eşzamanlılık ortadan kaldırır. İşlev uygulaması için birden çok VM Ölçeklendirmesi eşitlenene, her VM her kuyruk ile tetiklenen bir işlev bir örneğini çalıştırabilirsiniz.<br><br>En fazla `batchSize` 32'dir. |
|maxDequeueCount|5|Kaç defa zehirli kuyruğuna taşınmadan önce bir iletiyi işlemeyi deneyin.|
|newBatchThreshold|batchSize/2|Bu sayıya aynı anda işlenmekte olan ileti sayısını alır olduğunda, başka bir toplu iş çalışma zamanı alır.|

## <a name="next-steps"></a>Sonraki adımlar

* [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!--
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Queue storage trigger](functions-create-storage-queue-triggered-function.md)
-->

> [!div class="nextstepaction"]
> [Kuyruk depolama kullanan bir öğretici Git çıktı bağlaması](functions-integrate-storage-queue-output-binding.md)

<!-- LINKS -->

[CloudQueueMessage]: /dotnet/api/microsoft.azure.storage.queue.cloudqueuemessage
