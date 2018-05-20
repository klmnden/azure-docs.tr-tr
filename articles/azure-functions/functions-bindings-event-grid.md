---
title: Azure işlevleri için olay kılavuz tetikleyici
description: Azure işlevleri olay kılavuz olayları nasıl ele alınacağını anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/26/2018
ms.author: tdykstra
ms.openlocfilehash: 9228b1e80c8c46780a24d33e13fcedbd8da63ac3
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="event-grid-trigger-for-azure-functions"></a>Azure işlevleri için olay kılavuz tetikleyici

Bu makalede nasıl yapılacağını açıklar [olay kılavuz](../event-grid/overview.md) Azure işlevleri olayları.

Olay kılavuz içinde gerçekleşen olayları hakkında bilgilendirmek için HTTP istekleri gönderir bir Azure hizmeti olduğundan *yayımcılar*. Bir yayımcı hizmet veya olay kaynağı kaynak değil. Örneğin, bir yayımcı bir Azure blob storage hesabı olur ve [bir blob karşıya yükleme veya silme bir olaydır](../storage/blobs/storage-blob-event-overview.md). Bazı [Azure hizmetlerine sahip olayları olay kılavuza yayımlamak için yerleşik destek](../event-grid/overview.md#event-sources). 

Olay *işleyicileri* almak ve işlemek olaylar. Azure işlevleri biridir birkaç [olay kılavuz olayları işlemek için yerleşik destek sahip Azure Hizmetleri](../event-grid/overview.md#event-handlers). Bu makalede, bir olay kılavuz tetikleyicisi bir olay Olay kılavuzdan alındığında bir işlevi çağırmak için nasıl kullanılacağını öğrenin.

İsterseniz, olay kılavuz olayları işlemek için bir HTTP tetikleyicisi kullanabilirsiniz; bkz: [bir HTTP tetikleyicisi bir olay kılavuz tetikleyici olarak kullanacağınız](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerisinde yer.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages"></a>Paketler

Olay kılavuz tetikleyici içinde sağlanan [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi. Paket için kaynak kodunu konusu [azure işlevleri eventgrid uzantı](https://github.com/Azure/azure-functions-eventgrid-extension) GitHub depo.

<!--
If you want to bind to the `Microsoft.Azure.EventGrid.Models.EventGridEvent` type instead of `JObject`, install the [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) package.
-->

[!INCLUDE [functions-package](../../includes/functions-package.md)]

[!INCLUDE [functions-package-versions](../../includes/functions-package-versions.md)]

## <a name="example"></a>Örnek

Bir olay kılavuz tetikleyicisi dile özgü örneğin bakın:

* [C#](#c-example)
* [C# betik (.csx)](#c-script-example)
* [JavaScript](#javascript-example)

Bir HTTP tetikleyicisi örnek için bkz: [HTTP tetikleyicisini kullanma](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerisinde yer.

### <a name="c-example"></a>C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , bağlar `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, TraceWriter log)
        {
            log.Info(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

<!--
The following example shows a [C# function](functions-dotnet-class-library.md) that binds to `EventGridEvent`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
            public static void EventGridTest([EventGridTrigger] Microsoft.Azure.EventGrid.Models.EventGridEvent eventGridEvent, TraceWriter log)
        {
            log.Info("C# Event Grid function processed a request.");
            log.Info($"Subject: {eventGridEvent.Subject}");
            log.Info($"Time: {eventGridEvent.EventTime}");
            log.Info($"Data: {eventGridEvent.Data.ToString()}");
        }
    }
}
```
-->

Daha fazla bilgi için bkz: [paketleri](#packages), [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

### <a name="c-script-example"></a>C# kod örneği

Aşağıdaki örnek, bir tetikleyici bağlama gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

İçin bağlayan bir C# kodu işte `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

<!--
Here's C# script code that binds to `EventGridEvent`:

```csharp
#r "Newtonsoft.Json"
#r "Microsoft.Azure.WebJobs.Extensions.EventGrid"
#r "Microsoft.Azure.EventGrid"

using Microsoft.Azure.WebJobs.Extensions.EventGrid;
Using Microsoft.Azure.EventGrid.Models;

public static void Run(EventGridEvent eventGridEvent, TraceWriter log)
{
    log.Info("C# Event Grid function processed a request.");
    log.Info($"Subject: {eventGridEvent.Subject}");
    log.Info($"Time: {eventGridEvent.EventTime}");
    log.Info($"Data: {eventGridEvent.Data.ToString()}");
}
```
-->

Daha fazla bilgi için bkz: [paketleri](#packages), [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bir tetikleyici bağlama gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, eventGridEvent) {
    context.log("JavaScript Event Grid function processed a request.");
    context.log("Subject: " + eventGridEvent.subject);
    context.log("Time: " + eventGridEvent.eventTime);
    context.log("Data: " + JSON.stringify(eventGridEvent.data));
    context.done();
};
```
     
## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) özniteliği.

Burada bir `EventGridTrigger` bir yöntem imzası özniteliğinde:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, TraceWriter log)
{
    ...
}
 ```

Tam bir örnek için bkz: [C# örnek](#c-example).

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya. Oluşturucu parametreleri veya ayarlamak için özellikler yok `EventGridTrigger` özniteliği.

|Function.JSON özelliği |Açıklama|
|---------|---------|----------------------|
| **type** | Gerekli - kümesine olmalıdır `eventGridTrigger`. |
| **direction** | Gerekli - kümesine olmalıdır `in`. |
| **Adı** | Gerekli - işlevi kodda olay verileri alan parametresi için kullanılan değişken adı. |

## <a name="usage"></a>Kullanım

C# ve F # işlevleri için aşağıdaki parametre türleri için olay kılavuz tetikleyici kullanabilirsiniz:

* `JObject`
* `string`
* `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`-Ortak alanlar için özelliklerin tüm olay türleri için tanımlar. **Bu tür kullanım**, ancak değişimi için NuGet henüz yayımlanmadı.

JavaScript işlevleri için parametre adlı tarafından *function.json* `name` olay nesnesine başvuru özelliğine sahiptir.

## <a name="event-schema"></a>Olay şeması

Bir olay kılavuz olay verilerini bir HTTP istek gövdesindeki bir JSON nesnesi olarak alınır. JSON aşağıdaki örneğe benzer:

```json
[{
  "topic": "/subscriptions/{subscriptionid}/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstore",
  "subject": "/blobServices/default/containers/{containername}/blobs/blobname.jpg",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2018-01-23T17:02:19.6069787Z",
  "id": "{guid}",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "{guid}",
    "requestId": "{guid}",
    "eTag": "0x8D562831044DDD0",
    "contentType": "application/octet-stream",
    "contentLength": 2248,
    "blobType": "BlockBlob",
    "url": "https://egblobstore.blob.core.windows.net/{containername}/blobname.jpg",
    "sequencer": "000000000000272D000000000003D60F",
    "storageDiagnostics": {
      "batchId": "{guid}"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

Gösterilen örnekte, bir öğenin bir dizidir. Olay kılavuz her zaman bir dizi gönderir ve birden fazla olay dizisinde gönderebilir. Çalışma zamanı, işlevi her dizi öğesi için bir kez çağırır.

JSON veri olay üst düzey özelliklerinde içeriğini sırasında tüm olay türleri arasında aynıdır `data` her olay türü için özellik özeldir. Gösterilen örnek bir için blob depolama olayıdır.

Ortak ve olaya özgü özellikleri açıklamalar için bkz [olay özellikleri](../event-grid/event-schema.md#event-properties) olay kılavuz belgelerinde.

`EventGridEvent` Türü, yalnızca üst düzey özellikleri tanımlar; `Data` özelliği bir `JObject`. 

## <a name="create-a-subscription"></a>Abonelik oluşturma

Olay kılavuz HTTP isteklerini almaya başlamak için işlevi çağırır uç nokta URL'sini belirten bir olay kılavuz abonelik oluşturun.

### <a name="azure-portal"></a>Azure portalına

Olay kılavuz tetikleyici ile Azure portalında geliştirmek işlevleri seçin **ekleme olay kılavuz abonelik**.

![Portalda abonelik oluşturma](media/functions-bindings-event-grid/portal-sub-create.png)

Bu bağlantıyı seçtiğinizde, portal açar **olay aboneliği Oluştur** sayfa uç nokta URL'si ile doldurulmuş.

![Doldurulmuş uç nokta URL'si](media/functions-bindings-event-grid/endpoint-url.png)

Azure portalını kullanarak abonelikleri oluşturma hakkında daha fazla bilgi için bkz: [özel olay oluştur - Azure portalında](../event-grid/custom-event-quickstart-portal.md) olay kılavuz belgelerinde.

### <a name="azure-cli"></a>Azure CLI

Kullanarak bir abonelik oluşturmak için [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), kullanın [az eventgrid olay aboneliği oluşturmak](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_create) komutu.

Komut işlevi çağırır uç noktası URL'si gereklidir. Aşağıdaki örnek URL deseni gösterir:

```
https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}
```

Sistem anahtarı bir olay kılavuz tetikleyicisi için uç nokta URL'si dahil edilecek sahip bir yetkilendirme anahtardır. Aşağıdaki bölümde, sistem anahtarını almak açıklanmaktadır.

Aşağıda, bir blob storage hesabıyla (Sistem anahtarı için bir yer tutucu) abone bir örnek verilmiştir:

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name glengablobstorage --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://glengastorageevents.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=LUwlnhIsNtSiUjv/sNtSiUjvsNtSiUjvsNtSiUjvYb7XDonDUr/RUg==
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [blob depolama quickstart](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) veya diğer olay kılavuz quickstarts.

### <a name="get-the-system-key"></a>Sistem anahtarı alma

Aşağıdaki API (HTTP GET) kullanarak sistem anahtarını elde edebilirsiniz:

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={adminkey}
```

Bir yönetim API'si, gerektirdiği şekilde budur, [yönetici anahtarını](functions-bindings-http-webhook.md#authorization-keys). (Bir olay kılavuz Tetik işlevi çağırma) için Sistem anahtarı (işlev uygulaması üzerinde gerçekleştirilecek yönetim görevleri) için yönetici anahtarı ile karıştırmayın. Bir olay kılavuz konuya abone olduğunuzda, sistem anahtarını kullandığınızdan emin olun.

Sistem anahtarı sağlar yanıt bir örneği burada verilmiştir:

```
{
  "name": "eventgridextensionconfig_extension",
  "value": "{the system key for the function}",
  "links": [
    {
      "rel": "self",
      "href": "{the URL for the function, without the system key}"
    }
  ]
}
```

Daha fazla bilgi için bkz: [yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys) HTTP tetikleyicisi başvurusu makalesinde. 

Alternatif olarak, anahtar değeri kendiniz belirtmek için bir HTTP PUT gönderebilirsiniz.

## <a name="local-testing-with-requestbin"></a>Yerel RequestBin ile test etme

> [!NOTE]
> RequestBin site şu anda kullanılabilir değil, ancak bu yaklaşımı kullanabilirsiniz https://hookbin.com yerine. Bu site kapalı ise, kullanabileceğiniz [ngrok](#local-testing-with-ngrok).

Bir olay kılavuz tetikleyicisi test etmek için yerel olarak, bulutta kendi kaynaktan yerel makinenize teslim olay kılavuz HTTP isteklerini almak zorunda. Bunu yapmanın bir yolu, çevrimiçi ve el ile bunları yeniden göndermeyi yerel makinenizde istekleri yakalayarak şöyledir:

2. [RequestBin uç noktası oluşturma](#create-a-RequestBin-endpoint).
3. [Bir olay kılavuz abonelik oluşturma](#create-an-event-grid-subscription) , olayları RequestBin uç noktasına gönderir.
4. [İstek oluşturmak](#generate-a-request) ve istek gövdesi RequestBin sitesinden kopyalayın.
5. [El ile istek gönderme](#manually-post-the-request) olay Kılavuzunuzun localhost URL'sini işlevi tetikler.

Bitirdiğinizde test, aynı abonelik için üretim uç nokta güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI komutu.

### <a name="create-a-requestbin-endpoint"></a>RequestBin uç noktası oluşturma

RequestBin, HTTP isteklerini kabul eder ve istek gövdesi gösterir bir açık kaynak aracıdır. http://requestb.in URL Azure olay kılavuz tarafından özel işleme alır. Sınama kolaylaştırmak için olay kılavuz doğru yanıt abonelik doğrulama isteklerine gerek kalmadan olaylar RequestBin URL'sine gönderir. Bir test aracı aynı Muamele yapılır: http://hookbin.com.

RequestBin yüksek verimlilik kullanım için tasarlanmamıştır. Aynı anda birden fazla olay gönderirseniz araçta tüm olaylarınızı göremeyebilirsiniz.

Bir uç nokta oluşturun.

![RequestBin uç noktası oluşturma](media/functions-bindings-event-grid/create-requestbin.png)

Uç nokta URL'sini kopyalayın.

![RequestBin uç noktasını kopyalayın](media/functions-bindings-event-grid/save-requestbin-url.png)

### <a name="create-an-event-grid-subscription"></a>Bir olay kılavuz Abonelik Oluştur

Test etmek istediğiniz türü, bir olay kılavuz aboneliği oluşturun ve RequestBin uç noktanızı verin. Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [abonelik oluşturma](#create-a-subscription) bu makalenin önceki.

### <a name="generate-a-request"></a>Bir isteği oluştur

HTTP trafiği RequestBin uç noktanız için oluşturacak bir olay tetikler.  Örneğin, bir blob depolama abonelik oluşturduysanız, karşıya yükleme veya bir blobu silin. RequestBin sayfanızda bir istek görüntülenir, istek gövdesini kopyalayın.

Abonelik doğrulama isteği ilk alınır; tüm doğrulama isteklerini yoksaymak ve olay isteği kopyalayın.

![RequestBin isteği gövdesinden kopyalama](media/functions-bindings-event-grid/copy-request-body.png)

### <a name="manually-post-the-request"></a>El ile istek gönderme

Olay kılavuz işlevinizi yerel olarak çalıştırın.

Gibi bir araç kullanın [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/httpscripting.html) bir HTTP POST isteği oluşturmak için:

* Ayarlanmış bir `Content-Type: application/json` üstbilgi.
* Ayarlanmış bir `aeg-event-type: Notification` üstbilgi.
* RequestBin veriler istek gövdesi yapıştırın. 
* Şu biçimi kullanarak URL'ye, olay kılavuz tetikleyici işlevinin gönderin:

```
http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

`functionName` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Aşağıdaki ekran görüntüleri, üst bilgilerini göstermek ve Postman gövdesinde isteyin:

![Postman üstbilgileri](media/functions-bindings-event-grid/postman2.png)

![Postman istek gövdesinde](media/functions-bindings-event-grid/postman.png)

Olay kılavuz Tetik işlevi yürütür ve günlükleri aşağıdaki örneğe benzer şekilde gösterir:

![Örnek olay kılavuz tetikleyici işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Yerel ngrok ile test etme

Bir olay kılavuz tetikleyicisi yerel olarak test etmek için başka bir HTTP bağlantısı Internet ve geliştirme bilgisayarınıza arasında otomatik hale getirmek için yoludur. Bunu adlı bir açık kaynak aracıyla yapabilirsiniz [ngrok](https://ngrok.com/):

3. [Bir ngrok uç noktası oluşturma](#create-an-ngrok-endpoint).
4. [Olay kılavuz tetikleyici işlevi çalıştırmak](#run-the-event-grid-trigger-function).
5. [Bir olay kılavuz abonelik oluşturma](#create-a-subscription) , olayları ngrok uç noktasına gönderir.
6. [Bir olay tetikler](#trigger-an-event).

Bitirdiğinizde test, aynı abonelik için üretim uç nokta güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az_eventgrid_event_subscription_update) Azure CLI komutu.

### <a name="create-an-ngrok-endpoint"></a>Bir ngrok uç noktası oluşturma

Karşıdan *ngrok.exe* gelen [ngrok](https://ngrok.com/)ve aşağıdaki komutu çalıştırın:

```
ngrok http -host-header=localhost 7071
```

Host-işlevler çalışma zamanı localhost gelen istekleri beklediği localhost üzerinde çalıştığında Üstbilgi parametresi gereklidir. çalışma zamanı yerel olarak çalıştığında 7071 varsayılan bağlantı noktası numarasıdır.

Komut aşağıdaki gibi bir çıktı oluşturur:

```
Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://263db807.ngrok.io -> localhost:7071
Forwarding                    https://263db807.ngrok.io -> localhost:7071

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```

Olay kılavuz aboneliğinizin https://{subdomain}.ngrok.io URL kullanacaksınız.

### <a name="run-the-event-grid-trigger-function"></a>Olay kılavuz tetikleyici işlevini çalıştırma

Abonelik oluşturulduğunda işlevinizi yerel olarak çalıştırılması gerekir böylece ngrok URL olay kılavuz özel işleyerek açılmıyor. Öyle değilse, doğrulama yanıt gönderilen değil ve abonelik oluşturma başarısız olur.

### <a name="create-a-subscription"></a>Abonelik oluşturma

Test etmek istediğiniz türü, bir olay kılavuz aboneliği oluşturun ve aşağıdaki desenini kullanarak ngrok uç noktanızı verin:

```
https://{subdomain}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={functionname}
``` 

`functionName` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Azure CLI kullanarak örnek aşağıda verilmiştir:

```
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName=EventGridTrigger
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz: [abonelik oluşturma](#create-a-subscription) bu makalenin önceki.

### <a name="trigger-an-event"></a>Bir olay tetikleme

HTTP trafiği ngrok uç noktanız için oluşturacak bir olay tetikler.  Örneğin, bir blob depolama abonelik oluşturduysanız, karşıya yükleme veya bir blobu silin.

Olay kılavuz Tetik işlevi yürütür ve günlükleri aşağıdaki örneğe benzer şekilde gösterir:

![Örnek olay kılavuz tetikleyici işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>Bir HTTP tetikleyicisi bir olay kılavuz tetikleyici olarak kullanın

Bir olay kılavuz INSTEAD OF tetikleyicisi bir HTTP tetikleyicisini kullanarak olayları işleyebilmesi için olay kılavuz olaylarını HTTP istekleri olarak alınır. Bunu olası bir nedeni işlevi çağırır uç nokta URL'sini üzerinde daha fazla denetim elde etmektir. 

Bir HTTP tetikleyicisi kullanırsanız, otomatik olarak yaptıkları olay kılavuz tetikleyici için kod yazmak zorunda:

* Doğrulama yanıt gönderir bir [abonelik doğrulama isteği](../event-grid/security-authentication.md#webhook-event-delivery).
* İstek gövdesinde yer alan olay dizisinin öğesi başına bir kez işlevi çağırır.

Aşağıdaki örnek C# kod bir HTTP tetikleyicisi için olay Daya tetikleyici davranışını taklit eder:

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"], 
        "Microsoft.EventGrid.SubscriptionValidationEvent", 
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.Info("Validate request received");
        return req.CreateResponse<object>(new
        {
            validationResponse = messages[0]["data"]["validationCode"]
        });
    }

    // The request is not for subscription validation, so it's for one or more events.
    foreach (JObject message in messages)
    {
        // Handle one event.
        EventGridEvent eventGridEvent = message.ToObject<EventGridEvent>();
        log.Info($"Subject: {eventGridEvent.Subject}");
        log.Info($"Time: {eventGridEvent.EventTime}");
        log.Info($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Bir HTTP tetikleyicisi için şu örnek JavaScript kodunu olay Daya tetikleyici davranışını taklit eder:

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        context.res = { status: 200, body: JSON.stringify({validationResponse: messages[0].data.validationCode}) }
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        for (var i = 0; i < messages.length; i++) {
            // Handle one event.
            var message = messages[i];
            context.log('Subject: ' + message.subject);
            context.log('Time: ' + message.eventTime);
            context.log('Data: ' + JSON.stringify(message.data));
        }
    }
    context.done();
};
```

Olay işleme kodunuzu döngü içinde gider `messages` dizi.

İşlev yerel olarak veya Azure'da çalıştırıldığında çağırma için kullanılacak URL hakkında daha fazla bilgi için bkz: [HTTP tetikleyicisi bağlama başvuru belgeleri](functions-bindings-http-webhook.md) 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Olay kılavuz hakkında daha fazla bilgi edinin](../event-grid/overview.md)
