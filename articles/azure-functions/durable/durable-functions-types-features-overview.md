---
title: İşlev türleri ve dayanıklı işlevler uzantısını Azure işlevleri'nin özellikleri
description: İşlevleri ve Azure işlevleri'nde bir dayanıklı işlevler düzenleme işlevi işlevi iletişimi destekleyen türleri hakkında daha fazla bilgi edinin.
services: functions
author: jeffhollan
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 07/04/2019
ms.author: azfuncdf
ms.openlocfilehash: de5019e0f91c92829082aed962bb9633da52b4a9
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812840"
---
# <a name="durable-functions-types-and-features-azure-functions"></a>Dayanıklı işlevler türleri ve özellikleri (Azure işlevleri)

Dayanıklı işlevler uzantısıdır [Azure işlevleri](../functions-overview.md). Dayanıklı işlevler için işlev yürütme durum bilgisi olan düzenleme kullanabilirsiniz. Dayanıklı bir işlevi farklı Azure işlevlerini yapılan bir çözümdür. İşlevleri farklı roller bir kalıcı işlevi düzenleme oynatabilirsiniz. 

Bu makalede, bir dayanıklı işlevler düzenleme kullanabileceğiniz işlevlerin türlerine genel bakış sağlar. Makale işlevleri bağlanmak için kullanabileceğiniz bazı ortak desenleri içerir. Dayanıklı işlevler uygulama geliştirme zorluklarınızın çözmenize nasıl yardımcı olabileceğini öğrenin.

![Dayanıklı işlevler türlerini gösteren görüntü][1]  

## <a name="types-of-durable-functions"></a>Dayanıklı işlevler türleri

Azure işlevleri'nde dört dayanıklı işlevi türlerini kullanabilirsiniz: etkinlik, orchestrator, varlık ve istemci.

### <a name="activity-functions"></a>Etkinlik işlevleri

Etkinlik, temel birim dayanıklı işlevi düzenleme iş işlevlerdir. Etkinlik işlevler ve işlemde düzenlenen görevleri işlevlerdir. Örneğin, bir sipariş işlemek için kalıcı bir işlev oluşturabilirsiniz. Stok denetimi, müşteri ücretlendirme ve bir oluşturma görevleri içerir. Her görev, bir etkinlik işlevi olacaktır. 

Etkinlik işlevleri bunları yapabileceğiniz iş türünü sınırlı değildir. Herhangi bir etkinlik işlevi yazabilirsiniz [dayanıklı işlevler destekleyen dil](durable-functions-overview.md#language-support). Dayanıklı görev framework çağrılan etkinlik her işlevi en az bir kez düzenleme sırasında yürütülecek garanti eder.

Kullanım bir [etkinlik tetikleyici](durable-functions-bindings.md#activity-triggers) etkinlik işlevi tetikleyebilirsiniz. Alma .NET işlevlerini bir [DurableActivityContext](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableActivityContext.html) bir parametre olarak. Ayrıca, tetikleyici girişlerinde işlevine geçirmek için başka bir nesne bağlayabilirsiniz. JavaScript'te bir giriş aracılığıyla erişebileceğiniz `<activity trigger binding name>` özelliği [ `context.bindings` nesne](../functions-reference-node.md#bindings).

Etkinlik işlevinizi orchestrator değerleri geri dönebilirsiniz. Göndermek ya da çok sayıda değeri bir etkinlik işlevinden geri dönme, kullanabileceğiniz [diziler veya diziler](durable-functions-bindings.md#passing-multiple-parameters). Orchestration örneğinden yalnızca bir etkinlik işlevi tetikleyebilirsiniz. Bir etkinlik işlevi ve başka bir işlev (örneğin, bir HTTP tetiklemeli işlevin) bazı kod paylaşabilir olsa da, her işlevin yalnızca bir tetikleyiciye sahip olabilir.

Daha fazla bilgi ve örnekler için bkz: [etkinlik işlevlerini](durable-functions-bindings.md#activity-triggers).

### <a name="orchestrator-functions"></a>Orchestrator işlevleri

Orchestrator işlevleri nasıl eylemleri yürütülür ve Eylemler yürütüldüğü sırada açıklanmaktadır. Orchestrator işlevler kod düzenleme açıklayın (C# veya JavaScript) gösterildiği gibi [dayanıklı işlevler desenleri ve teknik kavramlar](durable-functions-concepts.md). Düzenleme Eylemler dahil olmak üzere, birçok farklı türde olabilir [etkinlik işlevlerini](#activity-functions), [alt düzenlemeleri](#sub-orchestrations), [dış olayları beklemeyi](#external-events)ve [zamanlayıcılar](#durable-timers). Orchestrator işlevleri ile de çalışabilirler [varlık işlevleri](#entity-functions).

Bir düzenleyici işlevi tarafından tetiklenmesi gerekir bir [düzenleme tetikleyici](durable-functions-bindings.md#orchestration-triggers).

Bir orchestrator tarafından başlatılan bir [orchestrator istemci](#client-functions). Herhangi bir kaynaktan (HTTP, kuyruk, olay akışının) orchestrator tetikleyebilirsiniz. Her bir örneğini düzenleme örneği tanımlayıcısı var. Örneği tanımlayıcısı veya kullanıcı tarafından oluşturulan otomatik olarak oluşturulan (önerilen) olabilir. Örneği tanımlayıcısı için kullanabileceğiniz [örnekleri yönetme](durable-functions-instance-management.md) düzenleme.

Daha fazla bilgi ve örnekler için bkz: [düzenleme Tetikleyicileri](durable-functions-bindings.md#orchestration-triggers).

###  <a name="entity-functions"></a>Varlık işlevleri (Önizleme)

Varlık işlevleri tanımlayın okuma ve küçük parçaları olarak bilinen durumunu güncelleştirmek için işlemleri *dayanıklı varlıkları*. Bir özel tetikleyici türü işlevleriyle orchestrator işlevler gibi varlık işlevlerdir *varlık tetikleyici*. Orchestrator işlevleri farklı olarak, belirli bir kod kısıtlamalardan varlık işlevleri yoktur. Varlık işlevlerini de yönetmek durumu açıkça örtük olarak durumu aracılığıyla denetim akışını temsil eden yerine.

> [!NOTE]
> Varlık işlevlerini ve ilgili işlevleri, yalnızca kullanılabilir, dayanıklı işlevler 2.0 ve üzeri.

Varlık işlevleri hakkında daha fazla bilgi için bkz. [varlık işlevleri](durable-functions-preview.md#entity-functions) önizleme özelliği belgeleri.

### <a name="client-functions"></a>İstemci işlevleri

İstemci, düzenlemeleri ve varlıkların örnekleri oluşturma ve yönetme tetiklenen işlevler işlevlerdir. Dayanıklı işlevler ile etkileşim kurmak için giriş noktası etkili bir şekilde değildirler. Herhangi bir kaynakta (HTTP, kuyruk, olay akışının.) bir istemci işlevi tetikleyebilirsiniz. Bir istemci işlevini kullanan [düzenleme istemcisi bağlama](durable-functions-bindings.md#orchestration-client) dayanıklı düzenlemeleri ve varlıklar oluşturmak ve yönetmek için.

En temel istemci işlevi bir düzenleyici işlevi başlar ve ardından onay durumu yanıt döndüren bir HTTP tetiklemeli işlevin örneğidir. Bir örnek için bkz. [HTTP API URL'si bulma](durable-functions-http-api.md#http-api-url-discovery).

Daha fazla bilgi ve örnekler için bkz: [düzenleme istemcisi](durable-functions-bindings.md#orchestration-client).

## <a name="features-and-patterns"></a>Özellikler ve desenleri

Sonraki bölümlerde düzenleri dayanıklı işlevler türleri ve özellikleri açıklar.

### <a name="sub-orchestrations"></a>Alt düzenlemeler

Orchestrator işlevleri etkinlik işlevlerini çağırabilir, ancak diğer orchestrator işlevleri de çağırabilirsiniz. Örneğin, bir kitaplık dışında daha büyük bir düzenleme orchestrator işlevlerin oluşturabilirsiniz. Ya da bir düzenleyici işlevi birden çok örneği paralel olarak çalıştırılabilir.

Daha fazla bilgi ve örnekler için bkz: [alt düzenlemeleri](durable-functions-sub-orchestrations.md).

### <a name="durable-timers"></a>Dayanıklı zamanlayıcılar

[Dayanıklı işlevler](durable-functions-overview.md) sağlar *dayanıklı zamanlayıcılar* , orchestrator işlevlerinde gecikmelere uygulamak veya zaman uyumsuz işlemleri zaman aşımları ayarlamak için kullanabilirsiniz. Dayanıklı zamanlayıcılar orchestrator işlevleri yerine kullanmak `Thread.Sleep` ve `Task.Delay` (C#) veya `setTimeout()` ve `setInterval()` (JavaScript).

Daha fazla bilgi ve örnekler için bkz: [dayanıklı zamanlayıcılar](durable-functions-timers.md).

### <a name="external-events"></a>Dış olaylar

Orchestrator işlevleri düzenleme örneğini güncelleştirmek dış olaylar için bekleyebilirsiniz. Dayanıklı işlevler bu özellik, genellikle insan etkileşimi veya diğer dış geri çağırmaları işlemek için kullanışlıdır.

Daha fazla bilgi ve örnekler için bkz: [dış olayları](durable-functions-external-events.md).

### <a name="error-handling"></a>Hata işleme

Dayanıklı işlevler düzenlemeleri uygulamak için kodu kullanın. Hata işleme özelliklerini programlama dilini kullanabilirsiniz. Desenler ister `try` / `catch` çalışma alanınızı düzenleme. 

Dayanıklı işlevler ayrıca yerleşik yeniden deneme ilkeleri ile gelir. Bir eylem, gecikme ve bir özel durum oluştuğunda etkinlikler otomatik olarak yeniden deneyin. Yeniden deneme düzenleme terk olmadan geçici özel durumları işlemek için kullanabilirsiniz.

Daha fazla bilgi ve örnekler için bkz: [hata işleme](durable-functions-error-handling.md).

### <a name="cross-function-app-communication"></a>Çapraz işlev uygulaması iletişimi

Tek bir işlev uygulaması bağlamında bir kalıcı düzenleme çalıştırmasına rağmen düzenlemeleri arasında birçok işlev uygulamaları koordine etmek için desenler kullanabilirsiniz. Uygulamalar arası iletişimi, HTTP üzerinden oluşabilir, ancak her etkinlik için dayanıklı framework kullanarak iki uygulama arasında dayanıklı bir işlemi yine de koruyabilir anlamına gelir.

Aşağıdaki örnekler, çapraz işlev uygulaması düzenleme göstermektedir C# ve JavaScript. Her örnekte dış düzenleme bir etkinlik başlar. Başka bir etkinliği alır ve durumunu döndürür. Orchestrator durumu olmasını bekler `Complete` devam etmeden önce.

Çapraz işlev uygulaması düzenleme bazı örnekleri aşağıda verilmiştir:

#### <a name="c"></a>C#

```csharp
[FunctionName("OrchestratorA")]
public static async Task RunRemoteOrchestrator(
    [OrchestrationTrigger] DurableOrchestrationContext context)
{
    // Do some work...

    // Call a remote orchestration.
    string statusUrl = await context.CallActivityAsync<string>(
        "StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true)
    {
        bool isComplete = await context.CallActivityAsync<bool>("CheckIsComplete", statusUrl);
        if (isComplete)
        {
            break;
        }

        await context.CreateTimer(context.CurrentUtcDateTime.AddMinutes(1), CancellationToken.None);
    }

    // B is done. Now, go do more work...
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

    // Call a remote orchestration.
    const statusUrl = yield context.df.callActivity("StartRemoteOrchestration", "OrchestratorB");

    // Wait for the remote orchestration to complete.
    while (true) {
        const isComplete = yield context.df.callActivity("CheckIsComplete", statusUrl);
        if (isComplete) {
            break;
        }

        const waitTime = moment(context.df.currentUtcDateTime).add(1, "m").toDate();
        yield context.df.createTimer(waitTime);
    }

    // B is done. Now, go do more work...
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

Başlamak için dayanıklı ilk işlevinizi oluşturma [ C# ](durable-functions-create-first-csharp.md) veya [JavaScript](quickstart-js-vscode.md).

> [!div class="nextstepaction"]
> [Dayanıklı işlevler hakkında daha fazla bilgi](durable-functions-bindings.md)

<!-- Media references -->
[1]: media/durable-functions-types-features-overview/durable-concepts.png
