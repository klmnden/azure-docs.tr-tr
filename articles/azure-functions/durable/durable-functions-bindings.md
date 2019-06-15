---
title: Dayanıklı işlevler - Azure için bağlamaları
description: Tetikleyiciler ve bağlamalar için Azure işlevleri için dayanıklı işlevler uzantısını kullanma
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 678e370977cadae642207f91a02136404fb6c34e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60710542"
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri) için bağlamaları

[Dayanıklı işlevler](durable-functions-overview.md) uzantısı orchestrator ve etkinlik işlevlerin yürütülmesini denetlemenize iki yeni tetikleyici bağlamaları tanıtır. Dayanıklı işlevler çalışma zamanı için bir istemci olarak davranan bir çıkış bağlaması da tanıtılmaktadır.

## <a name="orchestration-triggers"></a>Orchestration Tetikleyicileri

Orchestration tetikleyici dayanıklı orchestrator işlevleri yazmanızı sağlar. Bu tetikleyici yeni orchestrator işlevi örneklerin başlatılmasını ve "bir görevi beklemekten" mevcut orchestrator işlevi örnekleri sürdürme destekler.

Azure işlevleri için Visual Studio Araçları kullandığınızda, orchestration tetikleyici kullanılarak yapılandırılan [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) .NET özniteliği.

Komut dosyası dilleri içinde orchestrator işlevleri yazdığınızda (örneğin, JavaScript veya C# komut dosyası), orchestration tetikleyici aşağıdaki JSON nesnesinde tanımlanan `bindings` dizisi *function.json* Dosya:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` orchestration adıdır. Bu orchestrator işlevi yeni örneklerini başlamak istediğinizde, istemcileri kullanması gereken değer budur. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adını kullanılır.

Dahili olarak Bu tetikleyici bağlama kuyrukları işlev uygulaması için varsayılan depolama hesabında bir dizi yoklar. Bu kuyruklar açıkça bağlama özelliklerinde yapılandırılmamış neden olan uzantı iç uygulama Ayrıntıları ' dir.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Bazı düzenleme tetikleyicisi ile ilgili notlar şunlardır:

* **Tek iş parçacığı** -tek dağıtıcı iş parçacığı, bir tek ana bilgisayar örneği üzerindeki tüm orchestrator işlevi yürütme için kullanılır. Bu nedenle, orchestrator işlev kodunu verimlidir ve tüm g/ç gerçekleştirmez sağlamak önemlidir. Bu iş parçacığı dayanıklı işlevler özel görev türleri üzerinde bekleyen zaman dışında herhangi bir zaman uyumsuz iş yapmaz sağlamak önemlidir.
* **Poison ileti işleme** -düzenleme Tetikleyicileri içinde zehirli ileti desteği yoktur.
* **Mesaj görünürlük** -düzenleme tetikleyici iletileri sıradan çıkarılan ve yapılandırılabilir bir süre için görünmez tutulur. İşlev uygulaması çalışır ve iyi durumda olduğu sürece bu iletiler görünürlüğünü otomatik olarak yenilenir.
* **Dönüş değerleri** -değerler için JSON seri hale getirilmiş ve orchestration geçmiş tablosu, Azure tablo depolamadaki kalıcı döndürür. Bu dönüş değerleri bağlama, daha sonra açıklanan düzenleme istemcisi tarafından sorgulanabilir.

> [!WARNING]
> Orchestrator işlevleri hiçbir zaman herhangi bir giriş veya çıktı bağlaması düzenleme dışında bağlama tetikleyin. Bunun yapılması, bu bağlantıları tek iş parçacığı oluşturma ve g/ç kurallarına uyma değil çünkü dayanıklı görev uzantısı sorunlara neden olma olasılığı vardır.

> [!WARNING]
> JavaScript orchestrator işlevlerin hiçbir zaman bildirilmelidir `async`.

### <a name="trigger-usage-net"></a>Tetikleyici kullanım (.NET)

Destekler bağlama düzenleme tetikleyici hem giriş hem de çıkarır. Çıkış işleme ve girişi hakkında bilmeniz gereken bazı noktalar şunlardır:

* **girişleri** -.NET düzenleme işlevleri desteği yalnızca [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) parametre türü. İşlev imzası doğrudan girişlerinde seri durumdan çıkarma desteklenmiyor. Kod kullanmalıdır [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1)(.NET) veya `getInput` işlev giriş orchestrator getirilecek yöntemi (JavaScript). Bu giriş, JSON seri hale getirilebilir türler olmalıdır.
* **çıkaran** -girişleri yanı sıra çıkış değerleri düzenleme Tetikleyicileri destekler. İşlev dönüş değeri, çıkış değeri atamak için kullanılır ve JSON seri hale getirilebilir olması gerekir. .NET işlevi döndürürse `Task` veya `void`, `null` değeri çıktı olarak kaydedilir.

### <a name="trigger-sample"></a>Tetikleyici örnek

Basit bir "Merhaba Dünya" orchestrator işlevi aşağıdaki gibi görünmelidir bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> `context` JavaScript nesne DurableOrchestrationContext temsil etmiyor ancak [işlevi bağlamı bir bütün olarak.](../functions-reference-node.md#context-object). Orchestration yöntemleri aracılığıyla erişebileceğiniz `context` nesnenin `df` özelliği.

> [!NOTE]
> JavaScript düzenleyicileri kullanması gereken `return`. `durable-functions` Kitaplığı çağırmadan üstlenir `context.done` yöntemi.

İşte bu nedenle bir etkinlik işlevinin nasıl çağrıldığını gösteren bir "Hello World" örnek etkinlik İşlevler, çoğu orchestrator işlev çağırın:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = df.orchestrator(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivity("SayHello", name);
    return result;
});
```

## <a name="activity-triggers"></a>Etkinlik Tetikleyicileri

Etkinlik tetikleyici orchestrator işlevleri tarafından çağrılan işlevleri yazmanızı sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyici kullanılarak yapılandırılan [ActivityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .NET özniteliği.

Geliştirme için VS Code veya Azure portalını kullanıyorsanız, etkinlik Tetikleyici tarafından aşağıdaki JSON nesnesinde tanımlanan `bindings` dizisi *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` Etkinlik adıdır. Bu, bu etkinlik işlevi çağırmak için orchestrator işlevlerini kullanan değerdir. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adını kullanılır.

Dahili olarak bir işlev uygulaması için varsayılan depolama hesabını sırada Bu tetikleyici bağlama yoklar. Bir iç uygulama ayrıntısı bağlama özellikleri açıkça yapılandırılmadı neden olan uzantı kuyruğudur.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Etkinlik tetikleyicisi ile ilgili bazı notlar şunlardır:

* **İş parçacığı** -düzenleme tetikleyici, etkinlik Tetikleyicileri iş parçacığı etrafında herhangi bir kısıtlama yok veya g/ç. Normal işlevler gibi işlenebilir.
* **Poison ileti işleme** -etkinlik Tetikleyicileri içinde zehirli ileti desteği yoktur.
* **Mesaj görünürlük** -etkinlik tetikleyici iletileri sıradan çıkarılan ve yapılandırılabilir bir süre için görünmez tutulur. İşlev uygulaması çalışır ve iyi durumda olduğu sürece bu iletiler görünürlüğünü otomatik olarak yenilenir.
* **Dönüş değerleri** -değerler için JSON seri hale getirilmiş ve orchestration geçmiş tablosu, Azure tablo depolamadaki kalıcı döndürür.

> [!WARNING]
> Depolama arka ucu etkinlik işlevler için bir uygulama ayrıntısı olduğunu ve kullanıcı kodu bu depolama varlıklarla doğrudan etkileşimde bulunmamalıdır.

### <a name="trigger-usage-net"></a>Tetikleyici kullanım (.NET)

Destekler bağlama etkinlik tetikleyicisi girişlerinin hem, yalnızca düzenleme tetikleyicisi gibi çıkarır. Çıkış işleme ve girişi hakkında bilmeniz gereken bazı noktalar şunlardır:

* **girişleri** -.NET etkinliği işlevleri yerel olarak kullanan [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) parametre türü. Alternatif olarak, bir etkinlik işlevi, JSON seri hale getirilebilir parametre türü ile bildirilebilir. Kullanırken `DurableActivityContext`, çağırabilirsiniz [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) getirmek ve seri durumdan etkinlik işlevi için giriş.
* **çıkaran** -etkinlik işlevlerini destekleyen girişleri yanı sıra çıkış değerleri. İşlev dönüş değeri, çıkış değeri atamak için kullanılır ve JSON seri hale getirilebilir olması gerekir. .NET işlevi döndürürse `Task` veya `void`, `null` değeri çıktı olarak kaydedilir.
* **meta veri** -.NET etkinliği işlevleri için bağlayabilirsiniz bir `string instanceId` parametresini üst düzenleme örneğinin Kimliğini alın.

### <a name="trigger-sample"></a>Tetikleyici örnek

Basit bir "Merhaba Dünya" etkinlik işlevi nasıl görünebileceğini gösteren bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

.NET için varsayılan parametre türü `ActivityTriggerAttribute` bağlamanın `DurableActivityContext`. Ancak, aynı işlevi olarak Basitleştirilmiş böylece doğrudan JSON-serializeable için bağlama desteği (ilkel türler), ayrıca türleri .NET etkinliği Tetikleyicileri aşağıdaki gibidir:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
module.exports = async function(context) {
    return `Hello ${context.bindings.name}!`;
};
```

Aynı işlevin şu şekilde Basitleştirilmiş böylece JavaScript bağlamaları ayrıca ek parametreler geçirilebilir:

```javascript
module.exports = async function(context, name) {
    return `Hello ${name}!`;
};
```

### <a name="passing-multiple-parameters"></a>Birden çok parametre geçirme

Bir etkinlik işlevine birden çok parametreler doğrudan geçirilecek mümkün değildir. Bu durumda bir nesneler dizisi geçirin veya kullanmak için önerilir [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) .NET nesneleri.

Aşağıdaki örnek, yeni özellikleri kullanarak [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) ile eklenen [C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples):

```csharp
[FunctionName("GetCourseRecommendations")]
public static async Task<dynamic> RunOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string major = "ComputerScience";
    int universityYear = context.GetInput<int>();

    dynamic courseRecommendations = await context.CallActivityAsync<dynamic>("CourseRecommendations", (major, universityYear));
    return courseRecommendations;
}

[FunctionName("CourseRecommendations")]
public static async Task<dynamic> Mapper([ActivityTrigger] DurableActivityContext inputs)
{
    // parse input for student's major and year in university
    (string Major, int UniversityYear) studentInfo = inputs.GetInput<(string, int)>();

    // retrieve and return course recommendations by major and university year
    return new {
        major = studentInfo.Major,
        universityYear = studentInfo.UniversityYear,
        recommendedCourses = new []
        {
            "Introduction to .NET Programming",
            "Introduction to Linux",
            "Becoming an Entrepreneur"
        }
    };
}
```

## <a name="orchestration-client"></a>Düzenleme istemcisi

Düzenleme istemcisi bağlama orchestrator işlevleri ile etkileşim işlevleri yazmanızı sağlar. Örneğin, aşağıdaki yollarla düzenleme örneklerinde davranabilir:

* Bunları başlatın.
* Bunların durumunu sorgulayın.
* Bunları sonlandırın.
* Çalıştırdıkları sırada için olayları gönderirsiniz.
* Örnek geçmişini temizle.

Visual Studio kullanıyorsanız, orchestration istemciye kullanarak bağlayabilirsiniz [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .NET özniteliği.

Komut dosyası dilleri kullanıyorsanız (örneğin *.csx* veya *.js* dosyaları) için geliştirme, düzenleme tetikleyici aşağıdaki JSON nesnesinde tarafından tanımlanan `bindings` dizisi  *Function.JSON*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "in"
}
```

* `taskHub` -Birden çok işlev uygulamaları burada aynı depolama hesabını paylaşmak ancak birbirinden yalıtılmış olmasına gerek senaryolarda kullanılır. Belirtilmezse, varsayılan değerden `host.json` kullanılır. Bu değer, hedef orchestrator işlevleri tarafından kullanılan değerle eşleşmelidir.
* `connectionName` -Depolama hesabı bağlantı dizesi içeren bir uygulama ayarı adı. Bu bağlantı dizesi tarafından temsil edilen bir depolama hesabı hedef orchestrator işlevleri tarafından kullanılan hizmet örneğiyle aynı olmalıdır. Belirtilmezse, varsayılan depolama hesabı bağlantı dizesi bir işlev uygulaması için kullanılır.

> [!NOTE]
> Çoğu durumda, bu özellikleri atlamak ve varsayılan davranışı Uygula öneririz.

### <a name="client-usage"></a>İstemci kullanım

.NET işlevleri'nde, genellikle bağlamak `DurableOrchestrationClient`, size sağlayan tam erişim için tüm istemci dayanıklı işlevler tarafından desteklenen API'ler. JavaScript'te, aynı API'leri tarafından sunulan `DurableOrchestrationClient` döndürülen nesne `getClient`. İstemci nesnesi API'leri şunlardır:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)
* [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_) (şu anda yalnızca .NET)

Alternatif olarak, .NET işlevleri adlarınıza bağlayabileceğiniz `IAsyncCollector<T>` burada `T` olduğu [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) veya `JObject`.

Bkz: [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) bu işlemler hakkında daha fazla ayrıntı için API belgeleri.

> [!WARNING]
> JavaScript içinde yerel olarak geliştirirken, ortam değişkenini ayarlamak gerekir `WEBSITE_HOSTNAME` için `localhost:<port>`, örn. `localhost:7071` yöntemleri kullanmak üzere `DurableOrchestrationClient`. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="client-sample-visual-studio-development"></a>İstemci örneği (Visual Studio geliştirme)

"HelloWorld" düzenleme başlatan bir örnek kuyruk ile tetiklenen bir işlev aşağıda verilmiştir.

```csharp
[FunctionName("QueueStart")]
public static Task Run(
    [QueueTrigger("durable-function-trigger")] string input,
    [OrchestrationClient] DurableOrchestrationClient starter)
{
    // Orchestration input comes from the queue message content.
    return starter.StartNewAsync("HelloWorld", input);
}
```

### <a name="client-sample-not-visual-studio"></a>İstemci örneği (Visual Studio değil)

Geliştirme için Visual Studio kullanmıyorsanız, aşağıdakileri oluşturabilirsiniz *function.json* dosya. Bu örnek, bağlama dayanıklı düzenleme istemcinin kullandığı kuyruk ile tetiklenen bir işlev yapılandırma işlemi gösterilmektedir:

```json
{
  "bindings": [
    {
      "name": "input",
      "type": "queueTrigger",
      "queueName": "durable-function-trigger",
      "direction": "in"
    },
    {
      "name": "starter",
      "type": "orchestrationClient",
      "direction": "in"
    }
  ]
}
```

Yeni orchestrator işlevi örnekleri dile özgü örnekleri aşağıda verilmiştir.

#### <a name="c-sample"></a>C# örneği

Aşağıdaki örnek, bir C# betik işlevini yeni bir işlev örneği başlatmak için bağlama dayanıklı düzenleme istemcisi kullanmayı gösterir:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>JavaScript örneği

Aşağıdaki örnek bir JavaScript işlevinden yeni bir işlev örneği başlatmak için bağlama dayanıklı düzenleme istemcisi kullanmayı gösterir:

```javascript
const df = require("durable-functions");

module.exports = async function (context) {
    const client = df.getClient(context);
    return instanceId = await client.startNew("HelloWorld", undefined, context.bindings.input);
};
```

Örnekleri başlatma hakkında daha fazla ayrıntı bulunabilir [örnek Yönetim](durable-functions-instance-management.md).

<a name="host-json"></a>

## <a name="hostjson-settings"></a>Host.JSON ayarları

[!INCLUDE [durabletask](../../../includes/functions-host-json-durabletask.md)]

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Denetim noktası oluşturma ve yeniden yürütme davranışları hakkında bilgi edinin](durable-functions-checkpointing-and-replay.md)