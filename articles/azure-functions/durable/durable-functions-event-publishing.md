---
title: Azure Event Grid'e (Önizleme) yayımlama dayanıklı İşlevler
description: Dayanıklı işlevler için otomatik Azure Event Grid yayımlama yapılandırmayı öğrenin.
services: functions
author: ggailey777
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: glenga
ms.openlocfilehash: c07a42349fbd81a46b1b7cd9bcad1978f891a6b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733785"
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Azure Event Grid'e (Önizleme) yayımlama dayanıklı İşlevler

Bu makalede, özel bir düzenleme yaşam döngüsü olayları (gibi tamamlanan ve hatalı oluşturulmuş) yayımlamak için dayanıklı işlevler ayarlama işlemi gösterilmektedir [Azure Event Grid konusu](https://docs.microsoft.com/azure/event-grid/overview).

Bu özellik yararlı olduğu bazı senaryolar aşağıda verilmiştir:

* **Mavi/yeşil dağıtımları gibi DevOps senaryolar**: Herhangi bir görevi uygulamadan önce çalışıp çalışmadığını bilmek isteyebilirsiniz [yan yana dağıtım stratejisini](durable-functions-versioning.md#side-by-side-deployments).

* **Gelişmiş izleme ve tanılama desteği**: SQL veritabanı veya CosmosDB gibi sorgular için en iyi duruma getirilmiş bir dış deposunda düzenleme durum bilgileri takip.

* **Uzun süre çalışan arka plan etkinliği**: Dayanıklı işlevler için bir uzun süre çalışan arka plan etkinliği kullanırsanız, bu özellik, geçerli durumunu öğrenmek için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

* Yükleme [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc veya dayanıklı işlevler projenizdeki sonraki bir sürümü.
* Yükleme [Azure Storage öykünücüsü](https://docs.microsoft.com/azure/storage/common/storage-use-emulator).
* Yükleme [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) veya [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)

## <a name="create-a-custom-event-grid-topic"></a>Özel olay Kılavuzu konusu oluşturma

Dayanıklı işlevler olayları göndermek için event grid konusu oluşturun. Aşağıdaki yönergeler, Azure CLI kullanarak bir konu oluşturmak nasıl göstermektedir. PowerShell veya Azure portalını kullanarak nasıl yapılacağı hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [EventGrid hızlı Başlangıçlar: Özel olay - PowerShell oluşturma](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-powershell)
* [EventGrid hızlı Başlangıçlar: Özel olay - Azure portalında oluşturma](https://docs.microsoft.com/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

`az group create` komutuyla bir kaynak grubu oluşturun. Azure Event Grid, tüm bölgeler şu anda desteklemiyor. Hangi bölgeler desteklenir daha fazla bilgi için bkz: [Azure Event Grid genel bakış](https://docs.microsoft.com/azure/event-grid/overview).

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Özel konu oluşturma

Event grid konusu, için olay gönderdiği bir kullanıcı tanımlı bir uç nokta sağlar. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Bir DNS girişi haline geldiğinden, konu adı benzersiz olmalıdır.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup
```

## <a name="get-the-endpoint-and-key"></a>Uç nokta ve anahtarını alma

Konu uç noktasını alın. Değiştirin `<topic_name>` seçtiğiniz ada sahip.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarı alın. Değiştirin `<topic_name>` seçtiğiniz ada sahip.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Şimdi olayları konuya gönderebilirsiniz.

## <a name="configure-azure-event-grid-publishing"></a>Azure Event Grid yayımlama yapılandırma

Dayanıklı işlevler projenizde Bul `host.json` dosya.

Ekleme `eventGridTopicEndpoint` ve `eventGridKeySettingName` içinde bir `durableTask` özelliği.

```json
{
    "durableTask": {
        "eventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "eventGridKeySettingName": "EventGridKey"
    }
}
```

Olası Azure Event Grid yapılandırma özelliklerini bulunabilir [host.json belgeleri](../functions-host-json.md#durabletask). Yapılandırdıktan sonra `host.json` dosya, işlev uygulamanızı gönderir yaşam döngüsü olayları için olay Kılavuzu konusu. İşlev uygulamanızı hem yerel hem de azure'da çalıştırdığınızda bu çalışır.'' '

İşlev uygulamasına uygulama ayarı için konu anahtarı ve `local.setting.json`. Aşağıdaki JSON örneğidir `local.settings.json` yerel hata ayıklama. Değiştirin `<topic_key>` konu anahtarı ile.  

```json
{
    "IsEncrypted": false,
    "Values": {
        "AzureWebJobsStorage": "UseDevelopmentStorage=true",
        "AzureWebJobsDashboard": "UseDevelopmentStorage=true",
        "EventGridKey": "<topic_key>"
    }
}
```

Emin olun [depolama öykünücüsü](https://docs.microsoft.com/azure/storage/common/storage-use-emulator) çalışmaktadır. Çalıştırmak için iyi bir fikirdir `AzureStorageEmulator.exe clear all` yürütmeden önce komutu.

## <a name="create-functions-that-listen-for-events"></a>Olayları dinleme yapan işlevler oluştur

Bir işlev uygulaması oluşturun. Olay Kılavuzu konusu aynı bölgede bulmak idealdir.

### <a name="create-an-event-grid-trigger-function"></a>Bir olay Kılavuzu tetikleyicisi işlevi oluşturma

Yaşam döngüsü olaylarını almak için bir işlev oluşturun. Seçin **özel işlev**.

![Bir özel işlev Oluştur'u seçin.](./media/durable-functions-event-publishing/functions-portal.png)

Olay Kılavuzu tetikleyicisi seçip `C#`.

![Olay Kılavuzu tetikleyicisini seçin.](./media/durable-functions-event-publishing/eventgrid-trigger.png)

İşlevin adını girin ve ardından `Create`.

![Olay Kılavuzu tetikleyicisi oluşturun.](./media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Aşağıdaki kod ile bir işlev oluşturulur:

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

public static void Run(JObject eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
}
```

`Add Event Grid Subscription` öğesini seçin. Bu işlem bir event grid aboneliği için oluşturduğunuz olay Kılavuzu konusu ekler. Daha fazla bilgi için [Azure Event Grid kavramları](https://docs.microsoft.com/azure/event-grid/concepts)

![Olay Kılavuzu tetikleyicisi bağlantıyı seçin.](./media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Seçin `Event Grid Topics` için **konu türü**. Olay Kılavuzu konusu için oluşturduğunuz kaynak grubunu seçin. Ardından, olay Kılavuzu konusu örneğini seçin. Basın `Create`.

![Event Grid aboneliği oluşturun.](./media/durable-functions-event-publishing/eventsubscription.png)

Yaşam döngüsü olaylarını almak artık hazırsınız.

## <a name="create-durable-functions-to-send-the-events"></a>Olayları göndermek için dayanıklı işlevler oluşturma

Dayanıklı işlevler projenizde, yerel makinenizde hata ayıklamayı başlatın.  Aşağıdaki kodu şablon kodunu dayanıklı işlevler için aynıdır. Zaten yapılandırmış `host.json` ve `local.settings.json` yerel makinenizde.

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
        [FunctionName("Sample")]
        public static async Task<List<string>> RunOrchestrator(
            [OrchestrationTrigger] DurableOrchestrationContext context)
        {
            var outputs = new List<string>();

            // Replace "hello" with the name of your Durable Activity Function.
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Tokyo"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "Seattle"));
            outputs.Add(await context.CallActivityAsync<string>("Sample_Hello", "London"));

            // returns ["Hello Tokyo!", "Hello Seattle!", "Hello London!"]
            return outputs;
        }

        [FunctionName("Sample_Hello")]
        public static string SayHello([ActivityTrigger] string name, ILogger log)
        {
            log.LogInformation($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            ILogger log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.LogInformation($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Eğer `Sample_HttpStart` yaşam döngüsü olayları göndermek dayanıklı işlevi Postman veya tarayıcınızı ile başlar. Genellikle uç noktadır `http://localhost:7071/api/Sample_HttpStart` yerel hata ayıklama.

Azure portalında oluşturduğunuz işlevden günlüklerine bakın.

```
2018-04-20T09:28:21.041 [Info] Function started (Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d)
2018-04-20T09:28:21.104 [Info] {
    "id": "054fe385-c017-4ce3-b38a-052ac970c39d",
    "subject": "durable/orchestrator/Running",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Running"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:19.6492068Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}

2018-04-20T09:28:21.104 [Info] Function completed (Success, Id=3301c3ef-625f-40ce-ad4c-9ba2916b162d, Duration=65ms)
2018-04-20T09:28:37.098 [Info] Function started (Id=36fadea5-198b-4345-bb8e-2837febb89a2)
2018-04-20T09:28:37.098 [Info] {
    "id": "8cf17246-fa9c-4dad-b32a-5a868104f17b",
    "subject": "durable/orchestrator/Completed",
    "data": {
        "hubName": "DurableFunctionsHub",
        "functionName": "Sample",
        "instanceId": "055d045b1c8a415b94f7671d8df693a6",
        "reason": "",
        "runtimeStatus": "Completed"
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Olay Şeması

Yaşam döngüsü olayları Şeması aşağıdaki listede açıklanmıştır:

* **`id`**: Event grid olayı için benzersiz tanımlayıcı.
* **`subject`**: Olay konu yolu. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` olacaktır `Running`, `Completed`, `Failed`, ve `Terminated`.  
* **`data`**: Dayanıklı işlevler belirli parametreler.
  * **`hubName`**: [TaskHub](durable-functions-task-hubs.md) adı.
  * **`functionName`**: Orchestrator işlev adı.
  * **`instanceId`**: Dayanıklı işlevler InstanceId.
  * **`reason`**: İzleme olayı ile ilgili ek veriler. Daha fazla bilgi için [Tanılama'da dayanıklı işlevler (Azure işlevleri)](durable-functions-diagnostics.md)
  * **`runtimeStatus`**: Düzenleme çalışma zamanı durumu. , Çalışan tamamlandı, başarısız, iptal edildi.
* **`eventType`**: "orchestratorEvent"
* **`eventTime`**: Olay saati (UTC).
* **`dataVersion`**: Yaşam döngüsü olay şeması sürümü.
* **`metadataVersion`**:  Meta veri sürümü.
* **`topic`**: Olay ızgarası konu kaynak.

## <a name="how-to-test-locally"></a>Yerel olarak test etme

Yerel olarak test etmek için [ngrok](../functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Dayanıklı İşlevler, örnek Yönetimi öğrenin](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Dayanıklı işlevler üzerinde sürüm oluşturmayı öğrenin](durable-functions-versioning.md)
