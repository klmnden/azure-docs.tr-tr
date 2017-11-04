---
title: "Dayanıklı işlevleri - Azure için bağlamaları"
description: "Tetikleyicileri ve bağlamaları dayanıklı Functons uzantısı için Azure işlevleri için nasıl kullanılacağı."
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: 01016294c3ef6fd904a7582e4f9c16ef19330a20
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
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
    "version": "<Optional - version label of this orchestrator function>",
    "type": "orchestrationTrigger",
    "direction": "in"
}
```

* `orchestration`orchestration adıdır. Bu orchestrator işlevi yeni örneğini başlatmak istediğinizde, istemcileri kullanması gereken değer budur. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adı kullanılır.
* `version`orchestration, sürüm etiketi belirtin. Bir düzenleme yeni bir örneğini başlatılan istemcilere eşleşen sürüm etiketi eklemeniz gerekir. Bu özellik isteğe bağlıdır. Belirtilmezse, boş dize kullanılır. Sürüm oluşturma hakkında daha fazla bilgi için bkz: [sürüm](durable-functions-versioning.md).

> [!NOTE]
> Ayar değerleri için `orchestration` veya `version` özellikleri şu anda önerilmez.

Dahili olarak bir dizi işlev uygulaması için varsayılan depolama hesabı kuyruklarda Bu tetikleyici bağlama yoklar. Bu kuyruklar açıkça bağlama özelliklerinde yapılandırılmamış neden olan uzantının iç uygulama ayrıntılar verilmektedir.

### <a name="trigger-behavior"></a>Tetikleyici davranışı

Orchestration tetikleyici ilgili bazı notlar şunlardır:

* **Tek iş parçacığı oluşturma** -tek dağıtıcı iş parçacığı tek ana bilgisayar örneği üzerinde tüm orchestrator işlevi yürütmek için kullanılır. Bu nedenle, orchestrator işlev kodu etkin olduğundan ve tüm g/ç gerçekleştirmez emin olmak önemlidir. Bu iş parçacığı dayanıklı işlevleri özgü görev türlerinde bekleyen zaman dışında herhangi bir zaman uyumsuz iş yapmak emin olmak önemlidir.
* **Poising ileti işleme** -orchestration Tetikleyiciler içindeki zehir iletisi desteği yoktur.
* **İleti görünürlük** -Orchestration tetikleyici iletilerini kuyruktan çıkarıldı ve yapılandırılabilir bir süre için görünmez tutulur. İşlev uygulaması çalışıyor ve Sağlıklı olduğu sürece bu iletiler görünürlüğünü otomatik olarak yenilenir.
* **Dönüş değerleri** -değerleri JSON olarak serileştirilmiş ve Azure Table depolama orchestration geçmiş tablosuna kalıcı döndürür. Bu dönüş değerleri bağlama, daha sonra açıklanan orchestration istemci tarafından sorgulanabilir.

> [!WARNING]
> Orchestrator işlevleri hiçbir zaman herhangi bir giriş kullanın veya orchestration dışında bağlamaları çıkış tetiklemek bağlama. Bunun yapılması bu bağlamaları tek iş parçacığı oluşturma ve g/ç kuralları uyma değil çünkü dayanıklı görev uzantısı sorunlara neden olma olasılığı vardır.

### <a name="trigger-usage"></a>Tetikleyici kullanımı

Destekler bağlama orchestration tetikleyici girdi hem çıkarır. Girişi hakkında bilmeniz ve çıkış işleme bazı noktalar şunlardır:

* **girişleri** -Orchestration işlevlerini destekleyen yalnızca [DurableOrchestrationContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html) parametre türü. Seri durumdan çıkarma girişleri işlevi imzada doğrudan desteklenmez. Kod kullanmalıdır [GetInput\<T >](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_GetInput__1) işlevi girişleri orchestrator getirmek yöntemi. Bu girdi, JSON seri hale getirilebilir türler olmalıdır.
* **çıkarır** -Orchestration Tetikleyicileri çıkış değerlerinin yanı sıra girişleri destekler. İşlevinin dönüş değeri çıkış değeri atamak için kullanılır ve JSON Serileştirilebilir olmalıdır. Bir işlev döndürürse `Task` veya `void`, `null` değeri çıktısı olarak kaydedilir.

> [!NOTE]
> Orchestration Tetikleyicileri yalnızca C# ' de şu anda desteklenir.

### <a name="trigger-sample"></a>Tetikleyici örnek

En basit "Hello World" C# orchestrator işlevi aşağıdaki gibi görünmelidir bir örnek verilmiştir:

```csharp
[FunctionName("HelloWorld")]
public static string Run([OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = context.GetInput<string>();
    return $"Hello {name}!";
}
```

Burada olacak şekilde bir işlevi çağırmak nasıl oluşturulduğunu gösteren bir "Hello World" örnek çoğu orchestrator işlevleri diğer işlevleri arayın:

```csharp
[FunctionName("HelloWorld")]
public static async Task<string> Run(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    string name = await context.GetInput<string>();
    string result = await context.CallActivityAsync<string>("SayHello", name);
    return result;
}
```

## <a name="activity-triggers"></a>Etkinlik Tetikleyicileri

Etkinlik tetikleyici orchestrator işlevleri tarafından çağrılan işlevleri Yazar sağlar.

Visual Studio kullanıyorsanız, etkinlik tetikleyici kullanılarak yapılandırılmış [ActvityTriggerAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.ActivityTriggerAttribute.html) .NET özniteliği. 

Geliştirme için Azure portalını kullanıyorsanız, etkinlik tetikleyici aşağıdaki JSON nesnesinde tanımlanır `bindings` dizisi *function.json*:

```json
{
    "name": "<Name of input parameter in function signature>",
    "activity": "<Optional - name of the activity>",
    "version": "<Optional - version label of this activity function>",
    "type": "activityTrigger",
    "direction": "in"
}
```

* `activity`Etkinlik adıdır. Bu etkinlik işlevi çağırmak için orchestrator işlevlerini kullanmanızı değer budur. Bu özellik isteğe bağlıdır. Belirtilmezse, işlevin adı kullanılır.
* `version`Etkinliğin sürüm etiketi belirtin. Bir etkinlik çağırma orchestrator işlevleri eşleşen sürüm etiketi eklemeniz gerekir. Bu özellik isteğe bağlıdır. Belirtilmezse, boş dize kullanılır. Daha fazla bilgi için bkz: [sürüm](durable-functions-versioning.md).

> [!NOTE]
> Ayar değerleri için `activity` veya `version` özellikleri şu anda önerilmez.

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
* **çıkarır** -etkinlik Tetikleyicileri çıkış değerlerinin yanı sıra girişleri destekler. İşlevinin dönüş değeri çıkış değeri atamak için kullanılır ve JSON Serileştirilebilir olmalıdır. Bir işlev döndürürse `Task` veya `void`, `null` değeri çıktısı olarak kaydedilir.
* **meta veri** -etkinlik işlevleri için bağlayabilirsiniz bir `string instanceId` parametresi üst orchestration örnek kimliği alınamıyor.

> [!NOTE]
> Etkinlik Tetikleyicileri Node.js işlevlerde şu anda desteklenmemektedir.

### <a name="trigger-sample"></a>Tetikleyici örnek

Basit bir "Hello World" C# etkinlik işlevi aşağıdaki gibi görünmelidir bir örnek verilmiştir:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] DurableActivityContext helloContext)
{
    string name = helloContext.GetInput<string>();
    return $"Hello {name}!";
}
```

İçin varsayılan parametre türü `ActivityTriggerAttribute` bağlama `DurableActivityContext`. Ancak, böylece aynı işlevi olarak basit de doğrudan JSON-serializeable için bağlama desteği (ilkel dahil olmak üzere) türleri, etkinlik Tetikleyicileri aşağıdaki gibidir:

```csharp
[FunctionName("SayHello")]
public static string SayHello([ActivityTrigger] string name)
{
    return $"Hello {name}!";
}
```

## <a name="orchestration-client"></a>Orchestration istemci

Bağlama orchestration istemci orchestrator işlevleri ile etkileşim işlevler yazmanızı sağlar. Örneğin, aşağıdaki yollarla orchestration örneklerinde çalışabilir:
* Bunları başlatın.
* Durumlarını sorgu.
* Bunları sonlanır.
* Çalıştırdıkları sırada olayları gönderebilirsiniz.

Visual Studio kullanıyorsanız, kullanarak orchestration istemciye bağlayabilirsiniz [OrchestrationClientAttribute](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) .NET özniteliği.

Komut dosyası dili kullanıyorsanız (örneğin *.csx* dosyaları) geliştirme için orchestration tetikleyici aşağıdaki JSON nesnesi tarafından tanımlanan `bindings` function.json dizisi:

```json
{
    "name": "<Name of input parameter in function signature>",
    "taskHub": "<Optional - name of the task hub>",
    "connectionName": "<Optional - name of the connection string app setting>",
    "type": "orchestrationClient",
    "direction": "out"
}
```

* `taskHub`-Burada birden çok işlev uygulamalarının aynı depolama hesabını paylaşmak ancak birbirinden yalıtılmış olmasına gerek senaryolarda kullanılır. Belirtilmezse, varsayılan değerden `host.json` kullanılır. Bu değer hedef orchestrator işlevleri tarafından kullanılan değer eşleşmesi gerekir.
* `connectionName`-Depolama bağlantı dizesi içeren bir uygulama ayarı adı. Bu bağlantı dizesi tarafından temsil edilen depolama hesabının hedef orchestrator işlevler tarafından kullanılan adla aynı olması gerekir. Belirtilmezse, varsayılan bağlantı dizesini işlev uygulaması için kullanılır.

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

Geliştirme için Visual Studio kullanmıyorsanız, aşağıdaki function.json dosyası oluşturabilirsiniz. Bu örnek, bağlama dayanıklı orchestration istemci kullanan sıra tetiklenen bir işlev yapılandırma gösterilmektedir:

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

#### <a name="nodejs-sample"></a>Node.js örneği

Aşağıdaki örnek yeni bir işlev örnek bir Node.js işlevinden başlatmak için bağlama dayanıklı orchestration istemcisi kullanmayı gösterir:

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

