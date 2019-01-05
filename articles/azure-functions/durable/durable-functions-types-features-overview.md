---
title: İşlev türleri ve dayanıklı işlevler - Azure için özelliklerine genel bakış
description: İşlevler ve dayanıklı işlevi düzenleme kapsamında işlevi işlevi iletişim için izin veren rollerinin türlerini öğrenin.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: fbfee92343bfecfbe8395f95775ae1f107b99299
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54037285"
---
# <a name="overview-of-function-types-and-features-for-durable-functions-azure-functions"></a>İşlev türleri ve dayanıklı işlevler (Azure işlevleri) için özelliklerine genel bakış

Dayanıklı İşlevler, işlevi yürütme durum bilgisi olan düzenleme sağlar. Dayanıklı bir işlev, farklı Azure işlevleri'ni oluşan bir çözümdür. Bu işlevlerin her biri farklı rolleri düzenleme bir parçası olarak yürütebilirsiniz. Aşağıdaki belge işlevleri bir kalıcı işlevi düzenleme İlgili türdeki genel bir bakış sağlar. Ayrıca, bazı ortak desenleri'nin birbirine bağlama işlevleri içerir.  Hemen kullanmaya başlamak için dayanıklı ilk işlevinizi oluşturma [ C# ](durable-functions-create-first-csharp.md) veya [JavaScript](quickstart-js-vscode.md).

![Dayanıklı işlevler türleri][1]  

## <a name="types-of-functions"></a>İşlev türleri

### <a name="activity-functions"></a>Etkinlik işlevleri

Etkinlik, temel birim dayanıklı bir düzenleme iş işlevlerdir.  Etkinlik işlevler ve işlemde düzenlenmiş görevleri işlevlerdir.  Örneğin, müşteri sipariş işleme - Stok Kontrolü Ücret için kalıcı bir işlev oluşturun ve Sevkiyat oluşturma.  Bu görevlerin her biri bir etkinlik işlevi olacaktır.  Etkinlik işlevleri bunları yapabileceğini işler türünde herhangi bir kısıtlama yoktur.  Bunlar, tüm yazılabilir [dayanıklı işlevler tarafından desteklenen dil](durable-functions-overview.md#language-support). Dayanıklı görev Framework her çağrılan etkinlik işlevi düzenleme sırasında en az bir kez yürütülür garanti eder.

Bir etkinlik işlevi tarafından tetiklenmesi gerekir bir [etkinlik tetikleyici](durable-functions-bindings.md#activity-triggers).  .NET işlevleri alacak bir [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) bir parametre olarak. Ayrıca, tetikleyici girişlerinde işlevine geçirmek için başka bir nesne bağlayabilirsiniz. JavaScript'te, giriş aracılığıyla erişilebilir `<activity trigger binding name>` özelliği [ `context.bindings` nesne](../functions-reference-node.md#bindings).

Etkinlik işlevinizi orchestrator değerleri geri dönebilirsiniz.  Gönderme veya birden fazla değer döndüren bir etkinlik işlevden varsa [yararlanarak diziler veya diziler](durable-functions-bindings.md#passing-multiple-parameters).  Etkinlik işlevleri yalnızca bir düzenleme örneğinden tetiklenebilir.  Bazı kod etkinliği işlevi ve başka bir işlev (gibi HTTP ile tetiklenen bir işlev) arasında paylaşılabilir, ancak her işlevi yalnızca bir tetikleyici olabilir.

Daha fazla bilgi ve örnekler bulunabilir [dayanıklı işlevler bağlama makale](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator işlevleri

Orchestrator, kalıcı bir işlev kalbi işlevlerdir.  Siparişi eylemleri yürütülür ve yol orchestrator işlevleri açıklanmaktadır.  Orchestrator işlevler kod düzenleme açıklayın (C# veya JavaScript) gösterildiği gibi [dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md).  Düzenleme gibi eylemler, birçok farklı türde olabilir [etkinlik işlevlerini](#activity-functions), [alt düzenlemeleri](#sub-orchestrations), [dış olayları beklemeyi](#external-events), ve [ zamanlayıcılar](#durable-timers).  

Bir düzenleyici işlevi tarafından tetiklenmesi gerekir bir [düzenleme tetikleyici](durable-functions-bindings.md#orchestration-triggers).

Bir orchestrator tarafından başlatılan bir [orchestrator istemci](#client-functions) hangi kendisini tetiklenen tüm kaynağından (HTTP, kuyruklar, olay akışları).  Her bir örneğini düzenleme (önerilen) olan otomatik oluşturulmuş bir örnek tanımlayıcı sahip veya bu kullanıcı tarafından oluşturulan.  Bu tanımlayıcı için kullanılabilir [örnekleri yönetme](durable-functions-instance-management.md) düzenleme.

Daha fazla bilgi ve örnekler bulunabilir [dayanıklı işlevler bağlama makale](durable-functions-bindings.md#orchestration-triggers).

### <a name="client-functions"></a>İstemci işlevleri

İstemci, düzenleme, yeni örneklerini oluşturan tetiklenen işlevler işlevlerdir.  Dayanıklı bir düzenleme örneğini oluşturmak için giriş noktası değildirler.  İstemci işlevleri, herhangi bir tetikleyici (HTTP, kuyruklar, olay akışları, vb.) tarafından tetiklenen ve uygulama tarafından desteklenen herhangi bir dilde yazılmış.  Ek olarak, istemci işlevleri Tetikleyiciniz bir [düzenleme istemcisi](durable-functions-bindings.md#orchestration-client) bağlaması, sağlayan oluşturma ve dayanıklı düzenlemeleri yönetmek için.  En temel istemci işlevi bir düzenleyici işlevi başlar ve onay durumu yanıt olarak döndüren bir HTTP ile tetiklenen işlev örneğidir [bu aşağıdaki örnekte gösterilen](durable-functions-http-api.md#http-api-url-discovery).

Daha fazla bilgi ve örnekler bulunabilir [dayanıklı işlevler bağlama makale](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Özellikler ve desenleri

### <a name="sub-orchestrations"></a>Alt düzenlemeler

Etkinlik işlevlerini çağırma ek olarak orchestrator işlevleri diğer orchestrator işlevleri çağırabilir. Örneğin, bir kitaplık dışında daha büyük bir düzenleme orchestrator işlevlerin oluşturabilirsiniz. Ya da bir düzenleyici işlevi birden çok örneği paralel olarak çalıştırılabilir.

Daha fazla bilgi ve örnekler bulunabilir [alt düzenleme makale](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Dayanıklı zamanlayıcılar

[Dayanıklı işlevler](durable-functions-overview.md) sağlar *dayanıklı zamanlayıcılar* orchestrator işlevlerinde gecikmelere uygulamak veya zaman uyumsuz işlemleri zaman aşımları ayarlamak için kullanılacak. Dayanıklı zamanlayıcılar orchestrator işlevleri yerine kullanılmalıdır `Thread.Sleep` ve `Task.Delay` (C#) veya `setTimeout()` ve `setInterval()` (JavaScript).

Daha fazla bilgi ve örnekler dayanıklı zamanlayıcılar bulunabilir [dayanıklı zamanlayıcılar makale](durable-functions-timers.md)

### <a name="external-events"></a>Dış olaylar

Orchestrator işlevleri düzenleme örneğini güncelleştirmek dış olaylar için bekleyebilirsiniz. Dayanıklı İşlevler'ın bu özelliği, genellikle insan etkileşimi veya diğer dış geri çağırmaları işlemek için kullanışlıdır.

Daha fazla bilgi ve örnekler bulunabilir [dış olayları makale](durable-functions-external-events.md).

### <a name="error-handling"></a>Hata işleme

Dayanıklı işlevi düzenlemeleri kodda uygulanır ve hata işleme özelliklerini programlama dilini kullanabilirsiniz.  Bu, "try/catch", orchestration içinde çalışır gibi desenler anlamına gelir.  Dayanıklı işlevler ayrıca bazı yerleşik yeniden deneme ilkeleri ile gelir.  Bir eylem, gecikme ve özel durumlar etkinliklerini otomatik olarak yeniden deneyin.  Yeniden deneme düzenleme iptal gerek kalmadan geçici özel durumları işlemek izin verin.

Daha fazla bilgi ve örnekler bulunabilir [hata işleme makale](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Çapraz işlev uygulaması iletişimi

Dayanıklı bir düzenleme genellikle tek bir işlev uygulaması bağlamında yaşar, ancak sağlamak düzenlemeleri arasında birçok işlev uygulamaları koordine etmek desenler vardır.  Uygulamalar arası iletişimi, HTTP üzerinden geçekleşmiş olabileceğini olsa da, her etkinlik için dayanıklı framework kullanarak iki uygulama arasında dayanıklı bir işlemi yine de koruyabilir anlamına gelir.

İçinde bir çapraz işlev uygulaması düzenleme örnekleri C# ve JavaScript aşağıda verilmiştir.  Bir etkinlik dış düzenleme başlar. Başka bir etkinlik, ardından almak ve dönüş durumu.  Orchestrator, devam etmeden önce tamamlanması durum bekler.

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now go do more work...
}

[FunctionName("StartRemoteOrchestration")]
public static async Task<string> StartRemoteOrchestration([ActivityTrigger] string orchestratorName)
{
    using (var response = await HttpClient.PostAsync(
        $"https://appB.azurewebsites.net/orchestrations/{orchestratorName}",
        new StringContent("")))
    {
        string statusUrl = await response.Content.ReadAsAsync<string>();
        return statusUrl;
    }
}

[FunctionName("CheckIsComplete")]
public static async Task<bool> CheckIsComplete([ActivityTrigger] string statusUrl)
{
    using (var response = await HttpClient.GetAsync(statusUrl))
    {
        // 200 = Complete, 202 = Running
        return response.StatusCode == HttpStatusCode.OK;
    }
}
```

#### <a name="javascript-functions-2x-only"></a>JavaScript (yalnızca 2.x işlevleri)

```javascript
const df = require("durable-functions");
const moment = require("moment");

module.exports = df.orchestrator(function*(context) {
    // Do some work...

    // Call a remote orchestration
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now go do more work...
});
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, orchestratorName) {
    const options = {
        method: "POST",
        uri: `https://appB.azurewebsites.net/orchestrations/${orchestratorName}`,
        body: ""
    };

    const statusUrl = await request(options);
    return statusUrl;
};
```

```javascript
const request = require("request-promise-native");

module.exports = async function(context, statusUrl) {
    const options = {
        method: "GET",
        uri: statusUrl,
        resolveWithFullResponse: true,
    };

    const response = await request(options);
    // 200 = Complete, 202 = Running
    return response.statusCode === 200;
};
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı işlevler belgeleri okumaya devam edin](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
