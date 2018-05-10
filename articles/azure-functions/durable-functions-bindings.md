---
title: Dayanıklı işlevleri - Azure için bağlamaları
description: Tetikleyicileri ve bağlamaları dayanıklı Functons uzantısı için Azure işlevleri için nasıl kullanılacağı.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 370e6e2c569aaf6d9289bddccde2174b4dd2ee97
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="bindings-for-durable-functions-azure-functions"></a>Dayanıklı işlevleri (Azure işlevleri) için bağlamaları

[Dayanıklı işlevleri](durable-functions-overview.md) uzantısı orchestrator ve etkinlik işlevleri yürütmesini denetleyen iki yeni tetikleyici bağlamaları tanıtır. Aynı zamanda, dayanıklı işlevleri çalışma zamanı için bir istemci olarak davranan bir çıktı bağlama sunar.

## <a name="orchestration-triggers"></a>Orchestration Tetikleyicileri

Orchestration tetikleyici dayanıklı orchestrator işlevleri Yazar sağlar. Bu tetikleyici yeni orchestrator işlevi örnekleri başlatılıyor ve "görevi bekleniyor" var olan orchestrator işlevi örnekleri sürdürme destekler.

Azure işlevleri için Visual Studio Araçları kullandığınızda, orchestration tetikleyici kullanılarak yapılandırılmış [OrchestrationTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationTriggerAttribute.html) .NET özniteliği.

Orchestrator işlevleri (örneğin, Azure portalında) komut dosyası dillerde yazdığınızda, orchestration tetikleyici aşağıdaki JSON nesnesinde tanımlanır `bindings` dizisi *function.json* dosyası:

```json
{
    "name": "<Name of input parameter in function signature>",
    "orchestration": "<Optional - name of the orchestration>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration` orchestration adıdır. Bu orchestrator işlevi yeni örneğini başlatmak istediğinizde, istemcileri kullanması gereken değer budur. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adı kullanılır.

Dahili olarak bir dizi işlev uygulaması için varsayılan depolama hesabı kuyruklarda Bu tetikleyici bağlama yoklar. Bu kuyruklar açıkça bağlama özelliklerinde yapılandırılmamış neden olan uzantının iç uygulama ayrıntılar verilmektedir.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Orchestration tetikleyici ilgili bazı notlar şunlardır:

* **Tek iş parçacığı oluşturma** -tek dağıtıcı iş parçacığı tek ana bilgisayar örneği üzerinde tüm orchestrator işlevi yürütmek için kullanılır. Bu nedenle, orchestrator işlev kodu etkin olduğundan ve tüm g/ç gerçekleştirmez emin olmak önemlidir. Bu iş parçacığı dayanıklı işlevleri özgü görev türlerinde bekleyen zaman dışında herhangi bir zaman uyumsuz iş yapmak emin olmak önemlidir.
* **Poison ileti işleme** -orchestration Tetikleyiciler içindeki zehir iletisi desteği yoktur.
* **İleti görünürlük** -Orchestration tetikleyici iletilerini kuyruktan çıkarıldı ve yapılandırılabilir bir süre için görünmez tutulur. İşlev uygulaması çalışıyor ve Sağlıklı olduğu sürece bu iletiler görünürlüğünü otomatik olarak yenilenir.
* **Dönüş değerleri** -değerleri JSON olarak serileştirilmiş ve Azure Table depolama orchestration geçmiş tablosuna kalıcı döndürür. Bu dönüş değerleri bağlama, daha sonra açıklanan orchestration istemci tarafından sorgulanabilir.

> [!WARNING]
> Orchestrator işlevleri hiçbir zaman herhangi bir giriş kullanın veya orchestration dışında bağlamaları çıkış tetiklemek bağlama. Bunun yapılması bu bağlamaları tek iş parçacığı oluşturma ve g/ç kuralları uyma değil çünkü dayanıklı görev uzantısı sorunlara neden olma olasılığı vardır.

### <a name="trigger-usage"></a>Tetikleyici kullanımı

Destekler bağlama orchestration tetikleyici girdi hem çıkarır. Girişi hakkında bilmeniz ve çıkış işleme bazı noktalar şunlardır:

* **girişleri** -Orchestration işlevlerini destekleyen yalnızca [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) parametre türü. Seri durumdan çıkarma işlevi imzada doğrudan girdi desteklenmiyor. Kod kullanmalıdır [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1) işlevi girişleri orchestrator getirmek yöntemi. Bu girdi, JSON seri hale getirilebilir türler olmalıdır.
* **çıkarır** -Orchestration Tetikleyicileri çıkış değerlerinin yanı sıra girişleri destekler. İşlevinin dönüş değeri çıkış değeri atamak için kullanılır ve JSON Serileştirilebilir olmalıdır. Bir işlev döndürürse `Task` veya `void`, `null` değeri çıktısı olarak kaydedilir.

### <a name="trigger-sample"></a>Tetikleyici örnek

En basit "Hello World" orchestrator işlevi aşağıdaki gibi görünmelidir bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const name = context.df.getInput();
    return `Hello ${name}!`;
});
```

> [!NOTE]
> JavaScript orchestrators kullanması gereken `return`. `durable-functions` Kitaplığı çağırmadan mvc'deki `context.done` yöntemi.

Burada olacak şekilde bir etkinlik işlevi çağırmak nasıl oluşturulduğunu gösteren bir "Hello World" örnek etkinlik İşlevler, çoğu orchestrator işlevleri arayın:

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

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
const df = require("durable-functions");

module.exports = df(function*(context) {
    const name = context.df.getInput();
    const result = yield context.df.callActivityAsync("SayHello", name);
    return result;
});
```

## <a name="activity-triggers"></a>Etkinlik Tetikleyicileri

Etkinlik tetikleyici orchestrator işlevleri tarafından çağrılan işlevleri Yazar sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyici kullanılarak yapılandırılmış [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .NET özniteliği. 

Geliştirme için Azure portalını kullanıyorsanız, etkinlik tetikleyici aşağıdaki JSON nesnesinde tanımlanır `bindings` dizisi *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity` Etkinlik adıdır. Bu etkinlik işlevi çağırmak için orchestrator işlevlerini kullanmanızı değer budur. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adı kullanılır.

Dahili olarak Bu tetikleyici bağlama işlev uygulaması için varsayılan depolama hesabı kuyrukta yoklar. Bir iç uygulama ayrıntılarını bağlama özelliklerinde açıkça yapılandırılmamış neden olan uzantısı'nın sırasıdır.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Etkinlik tetikleyici ilgili bazı notlar şunlardır:

* **İş parçacığı oluşturma** -orchestration tetikleyici, iş parçacığı oluşturma geçici herhangi bir kısıtlamanın etkinlik Tetikleyicileri yok veya g/ç. Bunlar gibi normal işlevleri işlenebilir.
* **Poising ileti işleme** -etkinlik Tetikleyiciler içindeki zehir iletisi desteği yoktur.
* **İleti görünürlük** -etkinlik tetikleyici iletilerini kuyruktan çıkarıldı ve yapılandırılabilir bir süre için görünmez tutulur. İşlev uygulaması çalışıyor ve Sağlıklı olduğu sürece bu iletiler görünürlüğünü otomatik olarak yenilenir.
* **Dönüş değerleri** -değerleri JSON olarak serileştirilmiş ve Azure Table depolama orchestration geçmiş tablosuna kalıcı döndürür.

> [!WARNING]
> Depolama arka ucu etkinlik işlevler için bir uygulama olup ve kullanıcı kodu bu depolama varlıklarla doğrudan etkileşimde bulunmamalıdır.

### <a name="trigger-usage"></a>Tetikleyici kullanımı

Destekler bağlama etkinlik tetikleyici girdi hem, yalnızca orchestration tetikleyici gibi çıkarır. Girişi hakkında bilmeniz ve çıkış işleme bazı noktalar şunlardır:

* **girişleri** -etkinlik işlevlerini yerel olarak kullanan [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) parametre türü. Alternatif olarak, bir etkinlik işlevi, JSON seri hale getirilebilir bir parametre türü ile bildirilebilir. Kullandığınızda `DurableActivityContext`, çağırabilirsiniz [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html#Microsoft_Azure_WebJobs_DurableActivityContext_GetInput__1) giriş getirme ve seri durumdan etkinlik işlevi için.
* **çıkarır** -çıktı değerlerinin yanı sıra girişleri desteği etkinliği çalışır. İşlevinin dönüş değeri çıkış değeri atamak için kullanılır ve JSON Serileştirilebilir olmalıdır. Bir işlev döndürürse `Task` veya `void`, `null` değeri çıktısı olarak kaydedilir.
* **meta veri** -etkinlik işlevleri için bağlayabilirsiniz bir `string instanceId` parametresi üst orchestration örnek kimliği alınamıyor.

### <a name="trigger-sample"></a>Tetikleyici örnek

Basit bir "Hello World" etkinliği işlevi aşağıdaki gibi görünmelidir bir örnek verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
module.exports = function(context) {
    context.done(null, `Hello ${context.bindings.name}!`);
};
```

İçin varsayılan parametre türü `ActivityTriggerAttribute` bağlama `DurableActivityContext`. Ancak, böylece aynı işlevi olarak basit de doğrudan JSON-serializeable için bağlama desteği (ilkel dahil olmak üzere) türleri, etkinlik Tetikleyicileri aşağıdaki gibidir:

#### <a name="c"></a>C#

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

#### <a name="javascript-functions-v2-only"></a>JavaScript (yalnızca işlevler v2)

```javascript
module.exports = function(context, name) {
    context.done(null, `Hello ${name}!`);
};
```

### <a name="passing-multiple-parameters"></a>Birden çok parametreleri geçirme 

Doğrudan bir etkinlik işlevi için birden çok parametre geçirmek mümkün değil. Bu durumda nesnelerinin bir dizisi geçirin veya kullanmak için önerilir [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) nesneleri.

Aşağıdaki örnek yeni özellikleri kullanılıyor [ValueTuples](https://docs.microsoft.com/dotnet/csharp/tuples) eklendi [C# 7](https://docs.microsoft.com/dotnet/csharp/whats-new/csharp-7#tuples):

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

## <a name="orchestration-client"></a>Orchestration istemci

Bağlama orchestration istemci orchestrator işlevleri ile etkileşim işlevler yazmanızı sağlar. Örneğin, aşağıdaki yollarla orchestration örneklerinde çalışabilir:
* Bunları başlatın.
* Durumlarını sorgu.
* Bunları sonlanır.
* Çalıştırdıkları sırada olayları gönderebilirsiniz.

Visual Studio kullanıyorsanız, kullanarak orchestration istemciye bağlayabilirsiniz [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .NET özniteliği.

Komut dosyası dili kullanıyorsanız (örneğin *.csx* dosyaları) geliştirme için orchestration tetikleyici aşağıdaki JSON nesnesi tarafından tanımlanan `bindings` dizisi *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "out"
}
```

* `taskHub` -Burada birden çok işlev uygulamalarının aynı depolama hesabını paylaşmak ancak birbirinden yalıtılmış olmasına gerek senaryolarda kullanılır. Belirtilmezse, varsayılan değerden `host.json` kullanılır. Bu değer hedef orchestrator işlevleri tarafından kullanılan değer eşleşmesi gerekir.
* `connectionName` -Depolama hesabı bağlantı dizesi içeren bir uygulama ayarı adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabının hedef orchestrator işlevler tarafından kullanılan adla aynı olması gerekir. Belirtilmezse, varsayılan depolama hesabı bağlantı dizesi işlev uygulaması için kullanılır.

> [!NOTE]
> Çoğu durumda, bu özellikleri atlayın ve varsayılan davranışı kullanan öneririz.

### <a name="client-usage"></a>İstemci kullanımı

C# işlevlerde, genellikle bağlamak `DurableOrchestrationClient`, hangi size tam erişim için tüm istemci dayanıklı işlevleri tarafından desteklenen API. İstemci Nesne API'leri şunları içerir:

* [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_)
* [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_)
* [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_)
* [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_)

Alternatif olarak, bağlayabilirsiniz `IAsyncCollector<T>` nerede `T` olan [StartOrchestrationArgs](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.StartOrchestrationArgs.html) veya `JObject`.

Bkz: [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) bu işlemler hakkında daha fazla ayrıntı için API belgeleri.

### <a name="client-sample-visual-studio-development"></a>İstemci örnek (Visual Studio geliştirme)

"HelloWorld" orchestration başlayan bir örnek sıra tetiklemeli işlevin aşağıdadır.

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

### <a name="client-sample-not-visual-studio"></a>İstemci örnek (Visual Studio değil)

Geliştirme için Visual Studio kullanmıyorsanız, aşağıdaki oluşturabilirsiniz *function.json* dosya. Bu örnek, bağlama dayanıklı orchestration istemci kullanan sıra tetiklenen bir işlev yapılandırma gösterilmektedir:

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
      "direction": "out"
    }
  ],
  "disabled": false
} 
```

Yeni orchestrator işlevi örnekleri Başlat dile özgü örnekleri aşağıda verilmiştir.

#### <a name="c-sample"></a>C# örnek

Aşağıdaki örnek yeni bir işlev örnek C# betik işlevinden başlatmak için bağlama dayanıklı orchestration istemcisi kullanmayı gösterir:

```csharp
#r "Microsoft.Azure.WebJobs.Extensions.DurableTask"

public static Task<string> Run(string input, DurableOrchestrationClient starter)
{
    return starter.StartNewAsync("HelloWorld", input);
}
```

#### <a name="javascript-sample"></a>JavaScript örneği

Aşağıdaki örnek yeni bir işlev örnek bir JavaScript işlevinden başlatmak için bağlama dayanıklı orchestration istemcisi kullanmayı gösterir:

```js
module.exports = function (context, input) {
    var id = generateSomeUniqueId();
    context.bindings.starter = [{
        FunctionName: "HelloWorld",
        Input: input,
        InstanceId: id
    }];

    context.done(null, id);
};
```

Örnekleri başlatma hakkında daha fazla ayrıntı bulunabilir [örnek Yönetim](durable-functions-instance-management.md).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Denetim noktası oluşturma ve yeniden yürütme davranışları hakkında bilgi edinin](durable-functions-checkpointing-and-replay.md)

