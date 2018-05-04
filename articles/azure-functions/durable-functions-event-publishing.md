---
title: Dayanıklı Azure olay kılavuzuna (Önizleme) yayımlama işlevleri
description: Dayanıklı işlevleri otomatik Azure olay kılavuz yayımlamayı yapılandırmak öğrenin.
services: functions
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/20/2018
ms.author: tdykstra
ms.openlocfilehash: 6e7fdd4faa4213681813733aa8afe81d56835862
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="durable-functions-publishing-to-azure-event-grid-preview"></a>Dayanıklı Azure olay kılavuzuna (Önizleme) yayımlama işlevleri

Bu makalede, Azure dayanıklı işlevlerini (örneğin, tamamlanmış ve başarısız oluşturulan) orchestration yaşam döngüsü olayları için özel bir yayımlama ayarlamak gösterilmiştir [Azure olay kılavuz konusu](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

Bu özellik faydalı olduğu bazı senaryolar şunlardır:

* **DevOps senaryoları ister mavi/yeşil dağıtımları**: herhangi bir görevi uygulamadan önce çalıştırıyorsanız bilmek isteyebilirsiniz [yan yana dağıtım stratejisini](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-versioning#side-by-side-deployments).

* **Gelişmiş izleme ve tanılama desteği**:, orchestration durum bilgileri SQL veritabanı veya CosmosDB gibi sorguları için en iyi hale getirilmiş bir dış deposunda izlemek.

* **Uzun süre çalışan arka plan etkinliği**: dayanıklı işlevleri uzun süre çalışan arka plan etkinliği için kullanırsanız, bu özellik, geçerli durumunu öğrenmek için yardımcı olur.

## <a name="prerequisites"></a>Önkoşullar

* Yükleme [Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 1.3.0-rc veya üzeri dayanıklı işlevleri projenizdeki.
* Yükleme [Azure Storage öykünücüsü](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator).
* Yükleme [Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest) veya [Azure bulut Kabuğu](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)

## <a name="create-a-custom-event-grid-topic"></a>Özel olay kılavuz konu oluştur

Dayanıklı işlevlerden olayları göndermek için bir olay kılavuz konusu oluşturun. Aşağıdaki yönergeler Azure CLI kullanarak bir konu oluşturmayı gösterir. PowerShell veya Azure portalını kullanarak bunu yapma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [EventGrid Quickstarts: özel olay - PowerShell oluşturma](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-powershell)
* [EventGrid Quickstarts: özel olay - Azure portal oluşturma](https://docs.microsoft.com/en-us/azure/event-grid/custom-event-quickstart-portal)

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Bir kaynak grubu ile oluşturmak `az group create` komutu. Şu anda, olay kılavuz tüm bölgelere desteklemiyor. Bölgeler desteklendiği hakkında daha fazla bilgi için bkz: [olay kılavuz genel bakış](https://docs.microsoft.com/en-us/azure/event-grid/overview). 

```bash
az group create --name eventResourceGroup --location westus2
```

### <a name="create-a-custom-topic"></a>Özel konu oluşturma

Bir olay kılavuz konusu, olay sonrası kullanıcı tanımlı bir uç sağlar. `<topic_name>` değerini konunuz için benzersiz bir adla değiştirin. Bir DNS girişi azaldığından konu adı benzersiz olmalıdır.

```bash
az eventgrid topic create --name <topic_name> -l westus2 -g eventResourceGroup 
```

## <a name="get-the-endpoint-and-key"></a>Uç nokta ve anahtarınızı edinin

Uç noktası konunun alın. Değiştir `<topic_name>` seçtiğiniz ada sahip.

```bash
az eventgrid topic show --name <topic_name> -g eventResourceGroup --query "endpoint" --output tsv
```

Konu anahtarı edinin. Değiştir `<topic_name>` seçtiğiniz ada sahip.

```bash
az eventgrid topic key list --name <topic_name> -g eventResourceGroup --query "key1" --output tsv
```

Şimdi konuya olaylarını gönderebilir.

## <a name="configure-azure-event-grid-publishing"></a>Azure olay kılavuz yayımlamayı yapılandırma

Dayanıklı işlevleri projenizde bulmak `host.json` dosya.

Ekleme `EventGridTopicEndpoint` ve `EventGridKeySettingName` içinde bir `durableTask` özelliği.

```json
{
    "durableTask": {
        "EventGridTopicEndpoint": "https://<topic_name>.westus2-1.eventgrid.azure.net/api/events",
        "EventGridKeySettingName": "EventGridKey"
    }
}
```

* **EventGridTopicEndpoint** -olay kılavuz konusu uç noktası.
* **EventGridKeySettingName** -Azure işlevinizi üzerinde uygulama ayarının anahtarını. Dayanıklı işlevleri olay kılavuz konu anahtarı değerini alır.

Yapılandırdığınız sonra `host.json` dosya, olay kılavuz konuya yaşam döngüsü olayları göndermek için bilgisayarınızı dayanıklı işlevleri proje başlatır. İşlev uygulaması ve yerel olarak çalıştırdığınızda çalıştırdığınızda bu çalışır.

Uygulama konu anahtarı işlevi uygulama ayarı ve `local.setting.json`. Aşağıdaki JSON örneğidir `local.settings.json` yerel hata ayıklama için. Değiştir `<topic_key>` konu anahtarı ile.  

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

Olduğundan emin olun [depolama öykünücüsü](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-emulator) çalışıyor. Çalıştırmak için iyi bir fikirdir `AzureStorageEmulator.exe clear all` yürütmeden önce komutu.

## <a name="create-functions-that-listen-for-events"></a>Olaylarını dinleyecek işlevler oluştur

Bir işlev uygulaması oluşturun. Olay kılavuz konusu ile aynı bölgede bulmak en iyisidir.

### <a name="create-an-event-grid-trigger-function"></a>Bir olay kılavuz Tetik işlevi oluşturma

Yaşam döngüsü olayları almak için bir işlev oluşturun. Seçin **özel işlevi**. 

![Bir özel bir işlev Oluştur'u seçin.](media/durable-functions-event-publishing/functions-portal.png)

Olay kılavuz tetikleyicisi seçip `C#`.

![Olay kılavuz tetikleyicisi seçin.](media/durable-functions-event-publishing/eventgrid-trigger.png)

İşlevin adını girin ve ardından `Create`.

![Olay kılavuz tetikleyicisi oluşturun.](media/durable-functions-event-publishing/eventgrid-trigger-creation.png)

Bir işlev aşağıdaki kodla oluşturulur: 

```csharp
#r "Newtonsoft.Json"
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

`Add Event Grid Subscription` öğesini seçin. Bu işlem, bir olay kılavuz abonelik için oluşturduğunuz olay kılavuz konu ekler. Daha fazla bilgi için bkz: [Azure olay kılavuzunda kavramları](https://docs.microsoft.com/en-us/azure/event-grid/concepts)

![Olay kılavuz tetikleyicisi bağlantıyı seçin.](media/durable-functions-event-publishing/eventgrid-trigger-link.png)

Seçin `Event Grid Topics` için **konu türü**. Olay kılavuz konu için oluşturduğunuz kaynak grubunu seçin. Ardından olay kılavuz konunun örneği seçin. Tuşuna `Create`.

![Event Grid aboneliği oluşturun.](media/durable-functions-event-publishing/eventsubscription.png)

Şimdi yaşam döngüsü olayları almaya hazır. 

## <a name="create-durable-functions-to-send-the-events"></a>Olayları göndermek için dayanıklı işlevleri oluşturun.

Dayanıklı işlevleri projenizdeki yerel makinenizde hata ayıklama başlatın.  Aşağıdaki kod dayanıklı işlevler için şablon kodu ile aynıdır. Zaten yapılandırılmış `host.json` ve `local.settings.json` yerel makinenizde. 

```csharp
using System.Collections.Generic;
using System.Net.Http;
using System.Threading.Tasks;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace LifeCycleEventSpike
{
    public static class Sample
    {
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
        public static string SayHello([ActivityTrigger] string name, TraceWriter log)
        {
            log.Info($"Saying hello to {name}.");
            return $"Hello {name}!";
        }

        [FunctionName("Sample_HttpStart")]
        public static async Task<HttpResponseMessage> HttpStart(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")]HttpRequestMessage req,
            [OrchestrationClient]DurableOrchestrationClient starter,
            TraceWriter log)
        {
            // Function input comes from the request content.
            string instanceId = await starter.StartNewAsync("Sample", null);
            log.Info($"Started orchestration with ID = '{instanceId}'.");
            return starter.CreateCheckStatusResponse(req, instanceId);
        }
    }
}
```

Çağırırsanız `Sample_HttpStart` Postman veya tarayıcınızı ile yaşam döngüsü olayları göndermek dayanıklı işlevi başlatır. Uç noktanın genellikle olup `http://localhost:7071/api/Sample_HttpStart` yerel hata ayıklama için.

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
        "eventType": 0
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
        "eventType": 1
    },
    "eventType": "orchestratorEvent",
    "eventTime": "2018-04-20T09:28:36.5061317Z",
    "dataVersion": "1.0",
    "metadataVersion": "1",
    "topic": "/subscriptions/<your_subscription_id>/resourceGroups/eventResourceGroup/providers/Microsoft.EventGrid/topics/durableTopic"
}
2018-04-20T09:28:37.098 [Info] Function completed (Success, Id=36fadea5-198b-4345-bb8e-2837febb89a2, Duration=0ms)
```

## <a name="event-schema"></a>Olay şeması

Aşağıdaki listede yaşam döngüsü olayları şema açıklanmaktadır:

* **Kimliği**: olay kılavuz olay için benzersiz tanımlayıcı.
* **konu**: olay konu yolu. `durable/orchestrator/{orchestrationRuntimeStatus}`. `{orchestrationRuntimeStatus}` olacaktır `Running`, `Completed`, `Failed`, ve `Terminated`.  
* **veri**: dayanıklı işlevler belirli parametreleri.
    * **hubName**: [TaskHub](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-task-hubs) adı.
    * **functionName**: Orchestrator işlev adı.
    * **InstanceId**: dayanıklı işlevleri InstanceId'si.
    * **neden**: izleme olayla ilişkili ek veriler. Daha fazla bilgi için bkz: [tanılama dayanıklı işlevlerinde (Azure işlevleri)](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-diagnostics)
    * **eventType**: Orchestration çalışma zamanı durumu. 0: çalışan, 1: tamamlandı, 2: ContinuedAsNew, 3: başarısız oldu, 4: iptal edildi, 5: sonlandırıldı, 6: beklemede. 
* **eventType**: "orchestratorEvent"
* **eventTime**: Olay saati (UTC).
* **dataVersion**: yaşam döngüsü olay şema sürümü.
* **metadataVersion**: meta veri sürümü.
* **konu**: EventGrid konu kaynak.

## <a name="how-to-test-locally"></a>Yerel olarak test etme

Yerel olarak test etmek için kullanmak [ngrok](functions-bindings-event-grid.md#local-testing-with-ngrok).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Örnek Yönetimi dayanıklı işlevlerinde öğrenin](durable-functions-instance-management.md)

> [!div class="nextstepaction"]
> [Sürüm oluşturma dayanıklı işlevlerinde öğrenin](durable-functions-versioning.md)
