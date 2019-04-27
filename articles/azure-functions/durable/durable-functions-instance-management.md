---
title: Dayanıklı işlevler - Azure örneklerini yönetme
description: Azure işlevleri için dayanıklı işlevler uzantısını durumlarda yönetmeyi öğrenin.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
origin.date: 12/07/2018
ms.date: 03/19/2019
ms.author: v-junlch
ms.openlocfilehash: ee96bc5e17051ab37be34eecbb8e4fe35599cd5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60730778"
---
# <a name="manage-instances-in-durable-functions-in-azure"></a>Azure'da dayanıklı işlevler örneklerini yönetme

Kullanıyorsanız [dayanıklı işlevler](durable-functions-overview.md) uzantısı için Azure işlevleri veya bunu başlatmak istiyorsanız en iyi kullanım dışına aldığınızdan emin olun. Dayanıklı işlevler düzenleme örneklerinizin bunları yönetme hakkında daha fazla bilgi öğrenerek iyileştirebilirsiniz. Bu makalede, her örnek yönetim işleminin ayrıntılarına gider.

Başlangıç ve örnekler, sonlandırma gibi ve örnekleri, tüm örnekleri ve sorgu örnekleri filtrelerle sorgulama olanağı dahil olmak üzere sorgulayabilirsiniz. Ayrıca, örneklerine olayları göndermek, orchestration tamamlanmasını bekleyin ve HTTP Yönetim Web kancası URL'lerini alın. Bu makale, geri sarma örnekleri, örnek geçmişi temizleniyor ve görev hub'ı siliniyor dahil olmak üzere diğer yönetim işlemlerini de kapsar.

Dayanıklı işlevler içinde her yönetim işlemlerini uygulamak istediğiniz seçeneğiniz vardır. Bu makalede kullanan örnekler [Azure işlevleri çekirdek Araçları](../functions-run-local.md) hem .NET için (C#) ve JavaScript.

## <a name="start-instances"></a>Başlangıç örnekleri

Orchestration örneğini başlatabilir olmanız önemlidir. Dayanıklı işlevler bağlama başka bir işlevin tetikleyici kullanırken yaygın olarak yapılır.

[StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) metodunda [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) (.NET) veya `startNew` üzerinde `DurableOrchestrationClient` (JavaScript) yeni bir örneğini başlatır. Kullanarak bu sınıfın örneklerinin edindiğiniz `orchestrationClient` bağlama. Bu yöntem kaybolmamasının ardından başlangıcını kullanan belirtilen ada sahip bir işlev tetikler denetim kuyruğuna bir ileti dahili olarak `orchestrationTrigger` bağlama tetikleyin.

Düzenleme işlemi başarıyla zamanlandı bu zaman uyumsuz işlemi tamamlar. Düzenleme işlemi, 30 saniye içinde başlamanız gerekir. Uzun sürerse göreceğiniz bir `TimeoutException`.

> [!WARNING]
> JavaScript içinde yerel olarak geliştirirken, ortam değişkenini ayarlamak `WEBSITE_HOSTNAME` için `localhost:<port>` (örneğin, `localhost:7071`) yöntemleri kullanmak üzere `DurableOrchestrationClient`. Bu gereksinim hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-js/issues/28).

### <a name="net"></a>.NET

Parametreleri [StartNewAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_StartNewAsync_) aşağıdaki gibidir:

* **Ad**: Zamanlamak için orchestrator işlevinin adı.
* **Giriş**: Tüm JSON seri hale getirilebilir veriler orchestrator işleve giriş olarak geçirilmelidir.
* **InstanceId**: (İsteğe bağlı) Örneğinin benzersiz kimliği. Bu parametreyi belirtmezseniz, yöntem rastgele bir kimliği kullanır.

Basit bir C# örneği aşağıda verilmiştir:

```csharp
[FunctionName("HelloWorldManualStart")]
public static async Task Run(
    [ManualTrigger] string input,
    [OrchestrationClient] DurableOrchestrationClient starter,
    ILogger log)
{
    string instanceId = await starter.StartNewAsync("HelloWorld", input);
    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

Parametreleri `startNew` aşağıdaki gibidir:

* **Ad**: Zamanlamak için orchestrator işlevinin adı.
* **InstanceId**: (İsteğe bağlı) Örneğinin benzersiz kimliği. Bu parametreyi belirtmezseniz, yöntem rastgele bir kimliği kullanır.
* **Giriş**: (İsteğe bağlı) Tüm JSON seri hale getirilebilir veriler orchestrator işleve giriş olarak geçirilmelidir.

Basit bir JavaScript örnek aşağıda verilmiştir:

```javascript
const df = require("durable-functions");

module.exports = async function(context, input) {
    const client = df.getClient(context);

    const instanceId = await client.startNew("HelloWorld", undefined, input);
    context.log(`Started orchestration with ID = ${instanceId}.`);
};
```

> [!TIP]
> Rastgele bir tanımlayıcı örnek kimliği için kullanın. Bu, birden çok VM arasında orchestrator işlevleri ölçeklendirme eşit yük dağıtım sağlamaya yardımcı olur. Rastgele olmayan örnek kimlikleri kullanmak için uygun kimliği bir dış kaynaktan gelmelidir veya uygularken zamandır [singleton orchestrator](durable-functions-singletons.md) deseni.

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Doğrudan kullanarak bir örnek de başlatabilirsiniz [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable start-new` komutu. Bunu, aşağıdaki parametreleri alır:

* **`function-name` (gerekli)** : Başlamak için işlevin adı.
* **`input` (isteğe bağlı)** : Her iki satır içi işleve ya da bir JSON dosyası girin. Dosyalar için bir önek ile dosyanın yolunu ekleyin `@`, gibi `@path/to/file.json`.
* **`id` (isteğe bağlı)** : Orchestration örneği kimliği. Bu parametreyi belirtmezseniz, rastgele bir GUID komutunu kullanır.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. AzureWebJobsStorage varsayılandır.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. DurableFunctionsHub varsayılandır. Ayrıca bunu ayarlayabilirsiniz [host.json](durable-functions-bindings.md#host-json) durableTask:HubName kullanarak.

> [!NOTE]
> Çekirdek Araçları komut, bir işlev uygulaması, kök dizinden çalıştırdığınız varsayılır. Açıkça sağlarsanız `connection-string-setting` ve `task-hub-name` parametreleri, komutların herhangi bir dizinden çalıştırabilirsiniz. Bu komutlar çalıştıran bir işlev uygulaması konağa çalıştırabilirsiniz, ancak konak çalışmadığı sürece bazı etkileri gözlemleyin olamaz bulabilirsiniz. Örneğin, `start-new` komut hedef görev hub'ı, ancak orchestration başlangıç iletiye değil çalıştırdığı çalıştıran bir işlev uygulaması ana bilgisayar işlemi olmadıkça kaybolmamasının ileti işleyebilir.

Aşağıdaki komut, HelloWorld adlı işlev başlar ve dosyanın içeriğini geçirir `counter-data.json` ona:

```bash
func durable start-new --function-name HelloWorld --input @counter-data.json --task-hub-name TestTaskHub
```

## <a name="query-instances"></a>Sorgu örnekleri

Çalışmalarınızı, düzenlemeleri yönetmek için bir parçası olarak, büyük olasılıkla düzenleme örneği (örneğin, olup, normalde tamamlandı veya başarısız) durumu hakkında bilgi toplamak gerekir.

[GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_) metodunda [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı (.NET) veya `getStatus` metodunda `DurableOrchestrationClient` sınıfı (JavaScript) sorguları düzenleme durumu örneği.

Sürdüğünü bir `instanceId` (gerekli) `showHistory` (isteğe bağlı) `showHistoryOutput` (isteğe bağlı) ve `showInput` (isteğe bağlı, yalnızca .NET) parametre olarak.

* **`showHistory`**: Varsa kümesine `true`, yürütme geçmişini yanıtı içerir.
* **`showHistoryOutput`**: Varsa kümesine `true`, etkinlik çıktıları yürütme geçmişini içerir.
* **`showInput`**: Varsa kümesine `false`, yanıt işlevin giriş içermez. Varsayılan değer `true` şeklindedir. (Yalnızca .NET)

Yöntemi, aşağıdaki özelliklere sahip bir JSON nesnesi döndürür:

* **Ad**: Orchestrator işlevinin adı.
* **InstanceId**: Orchestration örnek kimliği (aynı olmalıdır `instanceId` giriş).
* **Oluşturulma zamanı**: Çalışan, orchestrator işlevi başlama zamanı.
* **LastUpdatedTime**: Zaman düzenleme son denetim noktası oluşturuldu.
* **Giriş**: İşlev bir JSON değeri olarak giriş. Bu alan, doldurulmuş değil `showInput` false'tur.
* **CustomStatus**: JSON biçimindeki özel düzenleme durumu.
* **Çıkış**: (İşlev Tamamlandı durumunda) bir JSON değeri olarak işlev çıkışı. Bu özellik orchestrator işlevi başarısız oldu, hata ayrıntıları içerir. Bu özellik orchestrator işlevi sonlandırıldıysa sonlandırma nedenini (varsa) içerir.
* **runtimeStatus**: Aşağıdaki değerlerden biri:
  * **Bekleyen**: Örnek zamanlandı, ancak çalışan henüz başlatılmadı.
  * **Çalışan**: Örneği çalıştırma başlatıldı.
  * **Tamamlanan**: Örnek normal olarak tamamlandı.
  * **ContinuedAsNew**: Örnek kendisi ile yeni bir geçmiş yeniden başlatıldı. Bu geçici bir durumdur.
  * **Başarısız**: Örneğin, bir hatayla başarısız oldu.
  * **Sonlandırılan**: Örnek aniden durduruldu.
* **Geçmiş**: Orchestration yürütme geçmişini. Bu alan yalnızca, doldurulur `showHistory` ayarlanır `true`.

Bu yöntem döndürür `null` örneği mevcut değil veya çalışan henüz başlatılmadı.

### <a name="c"></a>C#

```csharp
[FunctionName("GetStatus")]
public static async Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    var status = await client.GetStatusAsync(instanceId);
    // do something based on the current status.
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const status = await client.getStatus(instanceId);
    // do something based on the current status.
}
```

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Kullanarak, doğrudan düzenleme örneği durumunu almak mümkündür [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable get-runtime-status` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Orchestration örneği kimliği.
* **`show-input` (isteğe bağlı)** : Varsa kümesine `true`, yanıt işlevin giriş içeriyor. Varsayılan değer `false` şeklindedir.
* **`show-output` (isteğe bağlı)** : Varsa kümesine `true`, işlev çıkışı yanıtı içerir. Varsayılan değer `false` şeklindedir.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

Aşağıdaki komutu (giriş ve çıkış dahil) örneğini düzenleme örnek kimliği ile 0ab8c55a66644d68a3a8b220b12d209c durumunu alır. Çalıştığını varsayar `func` komutunu işlev uygulaması kök dizini:

```bash
func durable get-runtime-status --id 0ab8c55a66644d68a3a8b220b12d209c --show-input true --show-output true
```

Kullanabileceğiniz `durable get-history` düzenleme örneği geçmişini almak için komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Orchestration örneği kimliği.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu ayrıca host.json içinde durableTask:HubName kullanılarak ayarlanabilir.

```bash
func durable get-history --id 0ab8c55a66644d68a3a8b220b12d209c
```

## <a name="query-all-instances"></a>Tüm örnekleri sorgulama

Sorgu bir örneği yerine bir zaman, düzenleme, bunların tümünün aynı anda sorgulamak için daha verimli bulabilirsiniz.

Kullanabileceğiniz `GetStatusAsync` (.NET) veya `getStatusAll` tüm düzenleme örneklerinin durumları sorgulamak için yöntemi (JavaScript). . NET'te, geçirdiğiniz bir `CancellationToken` iptal etmek istemeniz durumunda nesne. Yöntemi aynı özelliklere sahip nesneleri döndürür `GetStatusAsync` parametrelerle yöntemi.

### <a name="c"></a>C#

```csharp
[FunctionName("GetAllStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IList<DurableOrchestrationStatus> instances = await client.GetStatusAsync(); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const instances = await client.getStatusAll();
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Ayrıca kullanarak doğrudan sorgu örneklerine mümkündür [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable get-instances` komutu. Bunu, aşağıdaki parametreleri alır:

* **`top` (isteğe bağlı)** : Bu komut, disk belleği destekler. Bu parametre, istek başına alınan örneklerinin karşılık gelir. Varsayılan değer 10'dur.
* **`continuation-token` (isteğe bağlı)** : Çözümlenecek sayfa veya almak için örnekleri bölümünü göstermek için bir belirteç. Her `get-instances` yürütme sonraki örnekleri kümesi için bir belirteç döndürür.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

```bash
func durable get-instances
```

## <a name="query-instances-with-filters"></a>Filtrelerle sorgu örnekleri

Ne standart örnek sorguda sağlayan tüm bilgileri ihtiyacınız yoksa? Örneğin, ne yalnızca orchestration oluşturulma zamanını veya orchestration çalışma zamanı durumu aradığınız? Filtreler uygulayarak sorgunuzu daraltabilirsiniz.

Kullanım `GetStatusAsync` (.NET) veya `getStatusBy` (JavaScript) yöntem bir kümesiyle eşleşen düzenleme örneklerinin bir listesini almak için önceden tanımlanmış filtreler.

### <a name="c"></a>C#

```csharp
[FunctionName("QueryStatus")]
public static async Task Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
    [OrchestrationClient] DurableOrchestrationClient client,
    ILogger log)
{
    IEnumerable<OrchestrationRuntimeStatus> runtimeStatus = new List<OrchestrationRuntimeStatus> {
        OrchestrationRuntimeStatus.Completed,
        OrchestrationRuntimeStatus.Running
    };
    IList<DurableOrchestrationStatus> instances = await starter.GetStatusAsync(
        new DateTime(2018, 3, 10, 10, 1, 0),
        new DateTime(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    ); // You can pass CancellationToken as a parameter.
    foreach (var instance in instances)
    {
        log.LogInformation(JsonConvert.SerializeObject(instance));
    };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, req) {
    const client = df.getClient(context);

    const runtimeStatus = [
        df.OrchestrationRuntimeStatus.Completed,
        df.OrchestrationRuntimeStatus.Running,
    ];
    const instances = await client.getStatusBy(
        new Date(2018, 3, 10, 10, 1, 0),
        new Date(2018, 3, 10, 10, 23, 59),
        runtimeStatus
    );
    instances.forEach((instance) => {
        context.log(JSON.stringify(instance));
    });
};
```

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Azure işlevleri çekirdek araçları da kullanabilirsiniz `durable get-instances` filtrelerle komutu. Yukarıda sözü edilen yanı sıra `top`, `continuation-token`, `connection-string-setting`, ve `task-hub-name` kullanabileceğiniz parametreler, üç filtre parametreleri (`created-after`, `created-before`, ve `runtime-status`).

* **`created-after` (isteğe bağlı)** : Bu tarih/saat sonra (UTC) oluşturulan örneklerini alır. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`created-before` (isteğe bağlı)** : Bu tarih/saat önce (UTC) oluşturulan örneklerini alır. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`runtime-status` (isteğe bağlı)** : (Örneğin, çalışan veya tamamlanmış) bir özel durumu ile örneklerini alır. Birden çok (boşlukla ayrılmış) durumları sağlayabilir.
* **`top` (isteğe bağlı)** : Örnek sayısı, istek başına aldı. Varsayılan değer 10'dur.
* **`continuation-token` (isteğe bağlı)** : Çözümlenecek sayfa veya almak için örnekleri bölümünü göstermek için bir belirteç. Her `get-instances` yürütme sonraki örnekleri kümesi için bir belirteç döndürür.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

Herhangi bir filtre sağlamıyorsa (`created-after`, `created-before`, veya `runtime-status`), yalnızca komutu alır `top` örneklerle çalışma zamanı durumu ve oluşturma zamanı için hiçbir haklısın.

```bash
func durable get-instances --created-after 2018-03-10T13:57:31Z --created-before  2018-03-10T23:59Z --top 15
```

## <a name="terminate-instances"></a>Örnekleri Sonlandır

Orchestration örneği çalıştırmak için çok uzun sürüyor veya herhangi bir nedenle tamamlanmadan önce bunu durdurmak yeterlidir varsa, bunu sonlandıramaz seçeneğiniz vardır.

Kullanabileceğiniz [TerminateAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_TerminateAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı (.NET) veya `terminate` yöntemi `DurableOrchestrationClient` sınıfı (JavaScript). İki parametreler bir `instanceId` ve `reason` örneği durumu ve günlüklere yazılır dize. Sonlandırılan bir örneği durdurur sonraki ulaştığında hemen sonra çalışan `await` (.NET) veya `yield` (JavaScript) noktası veya sona erer hemen zaten olması durumunda bir `await` veya `yield`.

### <a name="c"></a>C#

```csharp
[FunctionName("TerminateInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "It was time to be done.";
    return client.TerminateAsync(instanceId, reason);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "It was time to be done.";
    return client.terminate(instanceId, reason);
};
```

> [!NOTE]
> Örnek sonlandırma yay değil. Etkinlik işlevleri ve alt düzenlemeleri hesaplanacak bunları adlı düzenleme örneği sonlandırıldı bağımsız olarak çalıştırın.

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Ayrıca orchestration örneği doğrudan kullanarak sonlandırabilirsiniz [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable terminate` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Sonlandırmak için düzenleme örneği kimliği.
* **`reason` (isteğe bağlı)** : Sonlandırma nedeni.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

Aşağıdaki komutu sona erer 0ab8c55a66644d68a3a8b220b12d209c düzenleme örneği kimliği:

```bash
func durable terminate --id 0ab8c55a66644d68a3a8b220b12d209c --reason "It was time to be done."
```

## <a name="send-events-to-instances"></a>Örneklerine olayları gönderme

Bazı senaryolarda bekleyin ve dış olayları dinlemek orchestrator işlevleriniz için önemlidir. Bu içerir [izleme işlevleri](durable-functions-concepts.md#monitoring) ve bekliyorsunuz işlevleri [insan etkileşimi](durable-functions-concepts.md#human).

Örnekleri kullanarak çalıştırılması için olay bildirimleri gönderme [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı (.NET) veya `raiseEvent` yöntemi `DurableOrchestrationClient` (sınıfı JavaScript için). Bu olayları işleyebilir örnekleri olan bir çağrı bekleyen o [WaitForExternalEvent](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationContext.html#Microsoft_Azure_WebJobs_DurableOrchestrationContext_WaitForExternalEvent_) (.NET) veya `waitForExternalEvent` (JavaScript).

Parametreleri [RaiseEventAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RaiseEventAsync_) (.NET) ve `raiseEvent` (JavaScript) aşağıdaki gibidir:

* **InstanceId**: Örneğinin benzersiz kimliği.
* **EventName**: Gönderilecek olayın adı.
* **EventData**: JSON seri hale getirilebilir bir yükü örneğine göndermek için.

### <a name="c"></a>C#

```csharp
[FunctionName("RaiseEvent")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    int[] eventData = new int[] { 1, 2, 3 };
    return client.RaiseEventAsync(instanceId, "MyEvent", eventData);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const eventData = [ 1, 2, 3 ];
    return client.raiseEvent(instanceId, "MyEvent", eventData);
};
```

> [!IMPORTANT]
> Belirtilen örnek Kimliğine sahip hiçbir düzenleme örneği varsa veya örneği belirtilen olay adına beklemiyorsa, olay iletisi göz ardı edilir. Bu davranış hakkında daha fazla bilgi için bkz. [GitHub sorunu](https://github.com/Azure/azure-functions-durable-extension/issues/29).

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Ayrıca orchestration örneği için bir olay doğrudan kullanarak yükseltebilirsiniz [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable raise-event` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Orchestration örneği kimliği.
* **`event-name` (isteğe bağlı)** : Yükseltmek için olayın adı. Varsayılan değer: `$"Event_{RandomGUID}"`.
* **`event-data` (isteğe bağlı)** : Orchestration örneğine gönderilecek veri. Bu bir JSON dosyası yolu olabilir veya doğrudan komut satırında veriler sağlayabilir.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

```bash
func durable raise-event --id 0ab8c55a66644d68a3a8b220b12d209c --event-name MyEvent --event-data @eventdata.json
```

```bash
func durable raise-event --id 1234567 --event-name MyOtherEvent --event-data 3
```

## <a name="wait-for-orchestration-completion"></a>Orchestration tamamlanmasını bekle

Uzun süre çalışan düzenlemeleri içinde bekleyin ve düzenleme sonuçlarını almak isteyebilirsiniz. Bu gibi durumlarda da düzenleme üzerinde bir zaman aşımı süresini tanımlamak kullanışlıdır. Zaman aşımı aşılırsa, orchestration durumunu, sonuçları yerine döndürülmelidir.

[DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı kullanıma sunan bir [WaitForCompletionOrCreateCheckStatusResponseAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_WaitForCompletionOrCreateCheckStatusResponseAsync_) .NET API. Bu API, gerçek çıktısını bir düzenleme örneğinden eş zamanlı olarak almak için kullanabilirsiniz. JavaScript'te `DurableOrchestrationClient` sınıfı kullanıma sunan bir `waitForCompletionOrCreateCheckStatusResponse` aynı amaçla API. Bunlar ayarlanmaz, yöntemleri, 10 saniye boyunca varsayılan değeri kullanın. `timeout`ve 1 saniyeye `retryInterval`.  

Bu API kullanımı gösterilmiştir HTTP tetikleyici işlevi bir örnek aşağıda verilmiştir:

```C#
// Copyright (c) .NET Foundation. All rights reserved.
// Licensed under the MIT License. See LICENSE in the project root for license information.

using System;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;

namespace VSSample
{
    public static class HttpSyncStart
    {
        private const string Timeout = "timeout";
        private const string RetryInterval = "retryInterval";

        [FunctionName("HttpSyncStart")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}/wait")]
            HttpRequestMessage req,
            [OrchestrationClient] DurableOrchestrationClientBase starter,
            string functionName,
            ILogger log)
        {
            // Function input comes from the request content.
            dynamic eventData = await req.Content.ReadAsAsync<object>();
            string instanceId = await starter.StartNewAsync(functionName, eventData);

            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

            TimeSpan timeout = GetTimeSpan(req, Timeout) ?? TimeSpan.FromSeconds(30);
            TimeSpan retryInterval = GetTimeSpan(req, RetryInterval) ?? TimeSpan.FromSeconds(1);
            
            return await starter.WaitForCompletionOrCreateCheckStatusResponseAsync(
                req,
                instanceId,
                timeout,
                retryInterval);
        }

        private static TimeSpan? GetTimeSpan(HttpRequestMessage request, string queryParameterName)
        {
            string queryParameterStringValue = request.RequestUri.ParseQueryString()[queryParameterName];
            if (string.IsNullOrEmpty(queryParameterStringValue))
            {
                return null;
            }

            return TimeSpan.FromSeconds(double.Parse(queryParameterStringValue));
        }
    }
}
```

```Javascript
const df = require("durable-functions");

const timeout = "timeout";
const retryInterval = "retryInterval";

module.exports = async function (context, req) {
    const client = df.getClient(context);
    const instanceId = await client.startNew(req.params.functionName, undefined, req.body);

    context.log(`Started orchestration with ID = '${instanceId}'.`);

    const timeoutInMilliseconds = getTimeInSeconds(req, timeout) || 30000;
    const retryIntervalInMilliseconds = getTimeInSeconds(req, retryInterval) || 1000;

    return client.waitForCompletionOrCreateCheckStatusResponse(
        context.bindingData.req,
        instanceId,
        timeoutInMilliseconds,
        retryIntervalInMilliseconds);
};

function getTimeInSeconds (req, queryParameterName) {
    const queryValue = req.query[queryParameterName];
    return queryValue
        ? queryValue // expected to be in seconds
        * 1000 : undefined;
}
```

Aşağıdaki satırı ile işlevi çağırın. Zaman aşımı ve 0,5 saniye 2 saniye için yeniden deneme aralığı kullanın:

```bash
    http POST http://localhost:7071/orchestrators/E1_HelloSequence/wait?timeout=2&retryInterval=0.5
```

Orchestration örneğinden yanıt almak için gerekli süreye bağlı olarak, iki durum vardır:

* (Bu örnekte 2 saniye) tanımlanan zaman aşımı süresi içinde düzenleme örnekleri tamamlayın ve yanıt zaman uyumlu olarak sunulan gerçek düzenleme örnek çıktı:

    ```http
        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:14:29 GMT
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        [
            "Hello Tokyo!",
            "Hello Seattle!",
            "Hello London!"
        ]
    ```

* Tanımlanan zaman aşımı süresi içinde düzenleme örnekleri tamamlanamıyor ve yanıt bir açıklanan varsayılan [HTTP API URL'si bulma](durable-functions-http-api.md):

    ```http
        HTTP/1.1 202 Accepted
        Content-Type: application/json; charset=utf-8
        Date: Thu, 14 Dec 2017 06:13:51 GMT
        Location: http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}
        Retry-After: 10
        Server: Microsoft-HTTPAPI/2.0
        Transfer-Encoding: chunked

        {
            "id": "d3b72dddefce4e758d92f4d411567177",
            "sendEventPostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/raiseEvent/{eventName}?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "statusQueryGetUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177?taskHub={taskHub}&connection={connection}&code={systemKey}",
            "terminatePostUri": "http://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/terminate?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}",
            "rewindPostUri": "https://localhost:7071/admin/extensions/DurableTaskExtension/instances/d3b72dddefce4e758d92f4d411567177/rewind?reason={text}&taskHub={taskHub}&connection={connection}&code={systemKey}"
        }
    ```

> [!NOTE]
> Web kancası URL'leri biçimi, Azure işlevleri ana bilgisayarın hangi sürümünün kullanmakta olduğunuz bağlı olarak farklı olabilir. Önceki örnek için Azure işlevleri 2.x yöneticisidir.

## <a name="retrieve-http-management-webhook-urls"></a>HTTP Yönetim Web kancası URL'lerini alma

Bir dış sistem veya düzenleme için olay izlemek için kullanabilirsiniz. Dış sistemler açıklanan varsayılan yanıtın bir parçası olan Web kancası URL'ler aracılığıyla dayanıklı işlevler ile iletişim kurabilir [HTTP API URL'si bulma](durable-functions-http-api.md). Ancak, Web kancası URL'leri da programlı olarak düzenleme istemcisi veya bir etkinlik işlevinde erişilebilir. Kullanarak bunu [CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) yöntemi [DurableOrchestrationClient](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html) sınıfı (.NET) veya `createHttpManagementPayload` yöntemi `DurableOrchestrationClient` sınıfı (JavaScript).

[CreateHttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_CreateHttpManagementPayload_) ve `createHttpManagementPayload` bir parametreye sahip:

* **InstanceId**: Örneğinin benzersiz kimliği.

Yöntemleri bir örneğini döndürür [HttpManagementPayload](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.Extensions.DurableTask.HttpManagementPayload.html#Microsoft_Azure_WebJobs_Extensions_DurableTask_HttpManagementPayload_) (.NET) veya bir ' % s'nesnesi (JavaScript) dizesi aşağıdaki özelliklere sahip:

* **Kimliği**: Orchestration örnek kimliği (aynı olmalıdır `InstanceId` giriş).
* **StatusQueryGetUri**: Orchestration örneği durumu URL'si.
* **SendEventPostUri**: Orchestration örneği "raise olay" URL'si.
* **TerminatePostUri**: Orchestration örneği "sonlandırma" URL'si.
* **RewindPostUri**: Orchestration örneği "geri" URL'si.

Etkinlik işlevleri bu nesneler bir örneğini veya düzenleme için olay izlemek için dış sistemler gönderebilirsiniz:

### <a name="c"></a>C#

```csharp
[FunctionName("SendInstanceInfo")]
public static void SendInstanceInfo(
    [ActivityTrigger] DurableActivityContext ctx,
    [OrchestrationClient] DurableOrchestrationClient client,
    [DocumentDB(
        databaseName: "MonitorDB",
        collectionName: "HttpManagementPayloads",
        ConnectionStringSetting = "CosmosDBConnection")]out dynamic document)
{
    HttpManagementPayload payload = client.CreateHttpManagementPayload(ctx.InstanceId);

    // send the payload to Cosmos DB
    document = new { Payload = payload, id = ctx.InstanceId };
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

modules.exports = async function(context, ctx) {
    const client = df.getClient(context);

    const payload = client.createHttpManagementPayload(ctx.instanceId);

    // send the payload to Cosmos DB
    context.bindings.document = JSON.stringify({
        id: ctx.instanceId,
        payload,
    });
};
```

## <a name="rewind-instances-preview"></a>Geri Sar örnekleri (Önizleme)

Beklenmeyen bir nedenden dolayı bir düzenleme hatası varsa, *rewind* örneği bir API'yi kullanarak daha önce sağlıklı bir duruma bu amaçla oluşturulmuş.

> [!NOTE]
> Bu API yerine uygun hata işleme ve yeniden deneme ilkelerine yönelik değildir. Bunun yerine, yalnızca, burada düzenleme örnekleri beklenmeyen nedenlerle başarısız durumda kullanılmak üzere tasarlanmıştır. Hata işleme ve yeniden deneme ilkeleri hakkında daha fazla bilgi için bkz. [hata işleme](durable-functions-error-handling.md) konu.

Kullanım [RewindAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_RewindAsync_System_String_System_String_) (.NET) veya `rewindAsync` (JavaScript) API düzenleme moduna geri *çalıştıran* durumu. Orchestration hata nedeniyle etkinlik veya suborchestration yürütme hataları yeniden çalıştırın.

Örneğin, bir dizi içeren bir iş akışı sahip varsayalım [İnsan bir onayları](durable-functions-concepts.md#human). Bir dizi onaylarını gereklidir ve gerçek zamanlı yanıt bekleyin birinin bildir etkinlik işlev vardır varsayalım. Tüm onay etkinliklerinin yanıtları aldığınız veya zaman aşımına uğradı, başka bir etkinliği bir veritabanı bağlantı dizesi geçersiz gibi bir uygulama yanlış yapılandırma nedeniyle başarısız olduğunu varsayalım. Sonucu bir düzenleme hatası olan iş akışı inin. İle `RewindAsync` (.NET) veya `rewindAsync` (JavaScript) API, bir uygulama yönetici yapılandırması hatayı düzeltin ve başarısız düzenleme hemen önce hata durumuna geri. İnsan etkileşimi adımlardan hiçbiri reapproved gerekir ve orchestration artık başarıyla tamamlayabilir.

> [!NOTE]
> *Rewind* özelliği, geri sarma dayanıklı zamanlayıcılar kullanmak düzenleme örnekleri desteklemez.

### <a name="c"></a>C#

```csharp
[FunctionName("RewindInstance")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    string reason = "Orchestrator failed and needs to be revived.";
    return client.RewindAsync(instanceId, reason);
}
```

### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");

module.exports = async function(context, instanceId) {
    const client = df.getClient(context);

    const reason = "Orchestrator failed and needs to be revived.";
    return client.rewind(instanceId, reason);
};
```

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Doğrudan kullanarak bir düzenleme örneği sarabilirsiniz [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable rewind` komutu. Bunu, aşağıdaki parametreleri alır:

* **`id` (gerekli)** : Orchestration örneği kimliği.
* **`reason` (isteğe bağlı)** : Orchestration örneği geri sarma nedeni.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

```bash
func durable rewind --id 0ab8c55a66644d68a3a8b220b12d209c --reason "Orchestrator failed and needs to be revived."
```

## <a name="purge-instance-history"></a>Örnek geçmişini temizle

Bir düzenleme ile ilişkili tüm verileri kaldırmak için örnek geçmişini temizleyebilirsiniz. Örneğin, varsa bunlar büyük ileti bloblar ve Azure tablo satırları ve kurtulun isteyebilirsiniz. Bunu yapmak için [PurgeInstanceHistoryAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_PurgeInstanceHistoryAsync_) API.

> [!NOTE]
> `PurgeInstanceHistoryAsync` API'si şu anda yalnızca C#.

 Yönteminin iki aşırı yüklemesi vardır. İlki, orchestration örneğinin Kimliğini göre geçmiş temizler:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [ManualTrigger] string instanceId)
{
    return client.PurgeInstanceHistoryAsync(instanceId);
}
```

İkinci örnek sonra belirtilen zaman aralığında tamamlanan tüm düzenleme örnekleri için geçmişini temizler Zamanlayıcı ile tetiklenen bir işlev gösterir. Bu durumda, en az 30 gün önce tamamlanan tüm örnekleri için verileri kaldırır. Her gün, 12: 00 için bir kez çalıştırmak için zamanlanan:

```csharp
[FunctionName("PurgeInstanceHistory")]
public static Task Run(
    [OrchestrationClient] DurableOrchestrationClient client,
    [TimerTrigger("0 0 12 * * *")]TimerInfo myTimer)
{
    return client.PurgeInstanceHistoryAsync(
                    DateTime.MinValue,
                    DateTime.UtcNow.AddDays(-30),  
                    new List<OrchestrationStatus>
                    {
                        OrchestrationStatus.Completed
                    });
}
```

> [!NOTE]
> Saat ile tetiklenen bir işlev işleminin başarılı olması çalışma zamanı durumu olmalıdır **tamamlandı**, **kesildi**, veya **başarısız**.

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Kullanarak bir düzenleme örneğin geçmişi temizleyebilirsiniz [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable purge-history` komutu. Benzer şekilde, ikinci C# önceki bölümdeki örnek, belirtilen zaman aralığı boyunca oluşturulan tüm düzenleme örnekleri için geçmişini temizler. Bu gibi durumlarda, temizlenen örnekleri daha fazla çalışma zamanı durumuna göre filtreleyebilirsiniz. Komut çeşitli parametrelere sahiptir:

* **`created-after` (isteğe bağlı)** : Bu tarih/saat sonra (UTC) oluşturulan örnekleri geçmişini temizle. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`created-before` (isteğe bağlı)** : Bu tarih/saat önce (UTC) oluşturulan örnekleri geçmişini temizle. ISO 8601 tarih/saat kabul biçimlendirilmiş.
* **`runtime-status` (isteğe bağlı)** : Örnekleri (örneğin, çalışan veya tamamlanmış) bir özel durumu ile geçmişini temizle. Birden çok (boşlukla ayrılmış) durumları sağlayabilir.
* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

Aşağıdaki komut geçmişi 7:35 PM (UTC) 14 Kasım 2018'den önce oluşturulan tüm başarısız örneklerinin siler.

```bash
func durable purge-history --created-before 2018-11-14T19:35:00.0000000Z --runtime-status failed
```

## <a name="delete-a-task-hub"></a>Bir görev hub'ını Sil

Kullanarak [Azure işlevleri çekirdek Araçları](../functions-run-local.md) `durable delete-task-hub` komutu, belirli bir görev hub ile ilişkili tüm depolama yapıtları silebilirsiniz. Bu, Azure depolama tabloları, kuyrukları ve blobları içerir. Komut iki parametreye sahiptir:

* **`connection-string-setting` (isteğe bağlı)** : Kullanılacak depolama bağlantı dizesini içeren uygulama ayarının adı. Varsayılan değer: `AzureWebJobsStorage`.
* **`task-hub-name` (isteğe bağlı)** : Kullanılacak dayanıklı işlevler görev hub'ının adı. Varsayılan değer: `DurableFunctionsHub`. Bu da ayarlanabilir [host.json](durable-functions-bindings.md#host-json), durableTask:HubName kullanarak.

Aşağıdaki komut, ilişkili tüm Azure depolama verileri siler. `UserTest` görev hub.

```bash
func durable delete-task-hub --task-hub-name UserTest
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örneğin yönetim HTTP API'lerini kullanmayı öğrenin](durable-functions-http-api.md)

<!-- Update_Description: wording update -->