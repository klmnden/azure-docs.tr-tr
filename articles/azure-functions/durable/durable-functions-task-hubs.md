---
title: Dayanıklı işlevler - Azure, görev hub'ları
description: Bilgi dayanıklı işlevler uzantısını Azure işlevleri için bir görev hub bulunmaktadır. Yapılandırma konusunda bilgi görev hub'ları yapılandırın.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 12/07/2017
ms.author: azfuncdf
ms.openlocfilehash: 596eedab39ff926fcdc880c82c49ac464b7ff23b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60730285"
---
# <a name="task-hubs-in-durable-functions-azure-functions"></a>Dayanıklı işlevler (Azure işlevleri), görev hub'ları

A *görev hub* içinde [dayanıklı işlevler](durable-functions-overview.md) düzenlemeleri için kullanılan Azure Storage kaynakları için mantıksal bir kapsayıcıdır. Aynı görev hub'ına ait oldukları zaman orchestrator ve etkinlik işlevleri yalnızca birbiriyle etkileşim kurabilir.

Birden fazla işlev uygulaması, bir depolama hesabı, her işlev uygulaması paylaşıp paylaşmadığını *gerekir* ayrı görev hub adı ile yapılandırılabilir. Bir depolama hesabı, birden çok görev hub'ları içerebilir. Aşağıdaki diyagram, paylaşılan ve ayrılmış depolama hesaplarında işlev uygulaması başına tek bir görev hub gösterir.

![Gösteren diyagram, paylaşılan ve ayrılmış depolama hesapları.](./media/durable-functions-task-hubs/task-hubs-storage.png)

## <a name="azure-storage-resources"></a>Azure Storage kaynakları

Bir görev hub'ı aşağıdaki depolama kaynaklarını oluşur:

* Bir veya daha fazla denetim sıralar.
* Bir iş öğesi kuyruk.
* Bir geçmiş tablosu.
* Bir örnek tablo.
* Bir veya daha fazla kira bloblarını içeren bir depolama kapsayıcısı.

Orchestrator veya etkinlik işlevlerini çalıştırdığınızda veya çalıştırmak için zamanlanan tüm bu kaynaklar varsayılan Azure depolama hesabında otomatik olarak oluşturulur. [Performansı ve ölçeği](durable-functions-perf-and-scale.md) makalede, bu kaynakları nasıl kullanıldığı açıklanmaktadır.

## <a name="task-hub-names"></a>Görev hub adları

Görev merkezleri içinde bildirilen bir ad tarafından tanımlanır *host.json* aşağıdaki örnekte gösterildiği gibi dosya:

### <a name="hostjson-functions-1x"></a>host.json (1.x işlevleri)

```json
{
  "durableTask": {
    "hubName": "MyTaskHub"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.json (2.x işlevleri)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "MyTaskHub"
    }
  }
}
```

Görev merkezleri yapılandırılabilir uygulama ayarlarını kullanarak aşağıda gösterildiği gibi *host.json* dosyası örneği:

### <a name="hostjson-functions-1x"></a>host.json (1.x işlevleri)

```json
{
  "durableTask": {
    "hubName": "%MyTaskHub%"
  }
}
```

### <a name="hostjson-functions-2x"></a>host.json (2.x işlevleri)

```json
{
  "version": "2.0",
  "extensions": {
    "durableTask": {
      "hubName": "%MyTaskHub%"
    }
  }
}
```

Görev hub adı değeri olarak ayarlanır `MyTaskHub` uygulama ayarı. Aşağıdaki `local.settings.json` nasıl tanımlanacağını gösterir `MyTaskHub` olarak ayarlama `samplehubname`:

```json
{
  "IsEncrypted": false,
  "Values": {
    "MyTaskHub" : "samplehubname"
  }
}
```

İşte bir önceden derlenmiş C# nasıl yazılacağını kullanan bir işlev örneği bir [OrchestrationClientBinding](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.OrchestrationClientAttribute.html) bir uygulama ayarı yapılandırılmış bir görev hub ile çalışmak için:

```csharp
[FunctionName("HttpStart")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, methods: "post", Route = "orchestrators/{functionName}")] HttpRequestMessage req,
    [OrchestrationClient(TaskHub = "%MyTaskHub%")] DurableOrchestrationClientBase starter,
    string functionName,
    ILogger log)
{
    // Function input comes from the request content.
    dynamic eventData = await req.Content.ReadAsAsync<object>();
    string instanceId = await starter.StartNewAsync(functionName, eventData);

    log.LogInformation($"Started orchestration with ID = '{instanceId}'.");

    return starter.CreateCheckStatusResponse(req, instanceId);
}
```

Ve JavaScript için gerekli yapılandırma aşağıda verilmiştir. Görev hub özelliğinde `function.json` dosyası uygulama ayarı ayarlayın:

```json
{
    "name": "input",
    "taskHub": "%MyTaskHub%",
    "type": "orchestrationClient",
    "direction": "in"
}
```

Görev hub adları bir harf ile başlamalı ve yalnızca harf ve sayı oluşur. Belirtilmezse, varsayılan addır **DurableFunctionsHub**.

> [!NOTE]
> Paylaşılan depolama hesabında birden fazla görev merkezleri olduğunda ne bir görev hub birbirinden ayırır adıdır. Paylaşılan depolama hesabı paylaşımı birden fazla işlev uygulaması varsa, her görev hub için farklı adlar açıkça yapılandırmalısınız *host.json* dosyaları. Aksi takdirde birden fazla işlev uygulaması birbiriyle tanımsız davranışlara neden iletiler için rekabete girer.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Sürüm oluşturma nasıl ele alınacağını öğrenin](durable-functions-versioning.md)
