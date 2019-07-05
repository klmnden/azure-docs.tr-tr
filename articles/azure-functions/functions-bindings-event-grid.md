---
title: Azure işlevleri için olay Kılavuzu tetikleyicisi
description: Azure işlevleri'nde Event Grid olayların nasıl işleneceğini anlayın.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/04/2018
ms.author: cshoe
ms.openlocfilehash: f48eced2ebcc4ad92c5124194ed2e2df92f64f11
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480657"
---
# <a name="event-grid-trigger-for-azure-functions"></a>Azure işlevleri için olay Kılavuzu tetikleyicisi

Bu makalede nasıl yapılacağını açıklar [Event Grid](../event-grid/overview.md) Azure işlevleri'nde olayları.

Event Grid, içinde gerçekleşen olaylar hakkında bilgilendirmek için HTTP istekleri gönderen bir Azure hizmetidir; *yayımcılar*. Bir hizmet veya olay kaynaklı kaynak yayımcısıdır. Örneğin, bir Azure blob depolama hesabındaki bir yayımcı olur ve [blob karşıya yükleme veya silme bir olaydır](../storage/blobs/storage-blob-event-overview.md). Bazı [olayları Event Grid'e yayımlamak için yerleşik desteğe sahiptir. Azure Hizmetleri](../event-grid/overview.md#event-sources).

Olay *işleyicileri* almak ve olay işleme. Azure işlevleri, çeşitli birini [Event Grid olaylarını işlemek için yerleşik desteğe sahip olan Azure Hizmetleri](../event-grid/overview.md#event-handlers). Bu makalede, bir olay Kılavuzu tetikleyicisi Event Grid'den gelen bir olay alındığında bir işlevi çağırmak için nasıl kullanılacağını öğrenin.

Tercih ederseniz, Event Grid olaylarını işlemek için bir HTTP tetikleyicisi kullanabilirsiniz; bkz: [HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerleyen bölümlerinde. Olay iletildiğinde şu anda bir Event Grid tetikleyicisinin bir Azure işlev uygulaması için kullanamazsınız [CloudEvents şeması](../event-grid/cloudevents-schema.md). Bunun yerine bir HTTP tetikleyicisi kullanın.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Event Grid tetikleyicisinin sağlanan [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure işlevleri eventgrid uzantı](https://github.com/Azure/azure-functions-eventgrid-extension/tree/v2.x) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Event Grid tetikleyicisinin sağlanan [Microsoft.Azure.WebJobs.Extensions.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventGrid) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure işlevleri eventgrid uzantı](https://github.com/Azure/azure-functions-eventgrid-extension/tree/master) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="example"></a>Örnek

Bir olay Kılavuzu tetikleyicisi için dile özgü örneğe bakın:

* C#
* [C# betiği (.csx)](#c-script-example)
* [Java](#trigger---java-examples)
* [JavaScript](#javascript-example)
* [Python](#python-example)

HTTP tetikleyicisi örneği için bkz. [HTTP tetikleyicisi kullanmayı](#use-an-http-trigger-as-an-event-grid-trigger) bu makalenin ilerleyen bölümlerinde.

### <a name="c-2x"></a>C#(2.x)

Aşağıdaki örnek, bir işlevler gösterir 2.x [C# işlevi](functions-dotnet-class-library.md) için bağlar `EventGridEvent`:

```cs
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTest")]
        public static void EventGridTest([EventGridTrigger]EventGridEvent eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.Data.ToString());
        }
    }
}
```

Paketler, daha fazla bilgi için bkz. [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

### <a name="c-version-1x"></a>C#(Sürüm 1.x)

Aşağıdaki örnek, bir işlevler gösterir 1.x [C# işlevi](functions-dotnet-class-library.md) için bağlar `JObject`:

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.EventGrid;
using Microsoft.Azure.WebJobs.Host;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Microsoft.Extensions.Logging;

namespace Company.Function
{
    public static class EventGridTriggerCSharp
    {
        [FunctionName("EventGridTriggerCSharp")]
        public static void Run([EventGridTrigger]JObject eventGridEvent, ILogger log)
        {
            log.LogInformation(eventGridEvent.ToString(Formatting.Indented));
        }
    }
}
```

### <a name="c-script-example"></a>C# betiği örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan.

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

#### <a name="c-script-version-2x"></a>C#betik (sürüm 2.x)

İşte bağlar işlevleri 2.x C# betik kodu `EventGridEvent`:

```csharp
#r "Microsoft.Azure.EventGrid"
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Extensions.Logging;

public static void Run(EventGridEvent eventGridEvent, ILogger log)
{
    log.LogInformation(eventGridEvent.Data.ToString());
}
```

Paketler, daha fazla bilgi için bkz. [öznitelikleri](#attributes), [yapılandırma](#configuration), ve [kullanım](#usage).

#### <a name="c-script-version-1x"></a>C#betik (sürüm 1.x)

İşte bağlar işlevleri 1.x C# betik kodu `JObject`:

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(JObject eventGridEvent, TraceWriter log)
{
    log.Info(eventGridEvent.ToString(Formatting.Indented));
}
```

### <a name="javascript-example"></a>JavaScript örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan.

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

### <a name="python-example"></a>Python örnek

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [funkce Pythonu](functions-reference-python.md) bağlama kullanan.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "event",
      "direction": "in"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func


def main(event: func.EventGridEvent):
    logging.info("Python Event Grid function processed a request.")
    logging.info("  Subject: %s", event.subject)
    logging.info("  Time: %s", event.event_time)
    logging.info("  Data: %s", event.get_json())
```

### <a name="trigger---java-examples"></a>Tetikleyici - Java örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [Olay Kılavuzu tetikleyicisi, dize parametresi](#event-grid-trigger-string-parameter-java)
* [Olay Kılavuzu tetikleyicisi, POJO'ya parametresi](#event-grid-trigger-pojo-parameter-java)

Tetikleyici bağlamasında Aşağıdaki örnekler bir *function.json* dosya ve [Java işlevleri](functions-reference-java.md) bağlama kullanın ve ilk olay olarak alan, bir olay yazdırmak ```String``` ve ikinci bir POJO'ya olarak.

```json
{
  "bindings": [
    {
      "type": "eventGridTrigger",
      "name": "eventGridEvent",
      "direction": "in"
    }
  ]
}
```

#### <a name="event-grid-trigger-string-parameter-java"></a>Olay Kılavuzu tetikleyicisi, dize parametresinin (Java)

```java
  @FunctionName("eventGridMonitorString")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    String content, 
    final ExecutionContext context) {
      // log 
      context.getLogger().info("Event content: " + content);      
  }
```

#### <a name="event-grid-trigger-pojo-parameter-java"></a>Olay Kılavuzu tetikleyicisi, POJO'ya parametre (Java)

Bu örnek, üst düzey bir Event Grid olay özelliklerini temsil eden aşağıdaki POJO'ya kullanır:

```java
import java.util.Date;
import java.util.Map;

public class EventSchema {

  public String topic;
  public String subject;
  public String eventType;
  public Date eventTime;
  public String id;
  public String dataVersion;
  public String metadataVersion;
  public Map<String, Object> data;

}
```

Olayın bir JSON yükü geldiğinde, içine serileştirilmiş seri durumdan ```EventSchema``` POJO'ya işlevi tarafından kullanılacak. Bu, nesne yönelimli bir yolla olay özelliklerine erişmek işlev sağlar.

```java
  @FunctionName("eventGridMonitor")
  public void logEvent(
    @EventGridTrigger(
      name = "event"
    ) 
    EventSchema event, 
    final ExecutionContext context) {
      // log 
      context.getLogger().info("Event content: ");
      context.getLogger().info("Subject: " + event.subject);
      context.getLogger().info("Time: " + event.eventTime); // automatically converted to Date by the runtime
      context.getLogger().info("Id: " + event.id);
      context.getLogger().info("Data: " + event.data);
  }
```

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `EventGridTrigger` ek açıklama parametreleri değeri EventGrid ' gelmesi. Bu ek açıklamalar parametrelerle bir olay geldiğinde çalıştırmak işlev neden.  Bu ek açıklama yerel Java türler, pojo'ları veya kullanarak boş değer atanabilir değer ile kullanılabilir `Optional<T>`.

## <a name="attributes"></a>Öznitelikler

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [EventGridTrigger](https://github.com/Azure/azure-functions-eventgrid-extension/blob/master/src/EventGridExtension/EventGridTriggerAttribute.cs) özniteliği.

İşte bir `EventGridTrigger` özniteliği bir yöntem imzası:

```csharp
[FunctionName("EventGridTest")]
public static void EventGridTest([EventGridTrigger] JObject eventGridEvent, ILogger log)
{
    ...
}
```

Tam bir örnek için bkz. C# örnek.

## <a name="configuration"></a>Yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya. Oluşturucu parametresi veya ayarlamak için özellikler yok `EventGridTrigger` özniteliği.

|Function.JSON özelliği |Açıklama|
|---------|---------|
| **type** | Gerekli - kümesine olmalıdır `eventGridTrigger`. |
| **direction** | Gerekli - kümesine olmalıdır `in`. |
| **name** | Gereklidir - işlev kodu olay verileri alan parametresi için kullanılan bir değişken adı. |

## <a name="usage"></a>Kullanım

İçin C# ve F# Azure işlevlerinde 1.x İşlevler, aşağıdaki parametre türleri için Event Grid tetikleyicisinin kullanabilirsiniz:

* `JObject`
* `string`

İçin C# ve F# işlevleri Azure işlevleri'nde 2.x de aşağıdaki parametre türü için Event Grid tetikleyicisinin kullanılacak seçeneğiniz vardır:

* `Microsoft.Azure.EventGrid.Models.EventGridEvent`-Tüm olay türlerine abone ortak alanlar için özellikleri tanımlar.

> [!NOTE]
> Bağlanılacak çalışırsanız işlevleri v1'de `Microsoft.Azure.WebJobs.Extensions.EventGrid.EventGridEvent`, derleyici bir "kullanım dışı" iletisini görüntülemek ve kullanmanızı öneriyoruz `Microsoft.Azure.EventGrid.Models.EventGridEvent` yerine. Daha yeni türü kullanmak için başvuru [Microsoft.Azure.EventGrid](https://www.nuget.org/packages/Microsoft.Azure.EventGrid) NuGet paketini ve tam olarak nitelemek `EventGridEvent` tür adı ile önek tarafından `Microsoft.Azure.EventGrid.Models`. Bir C# betik işlevi NuGet paketlerine başvurmak hakkında daha fazla bilgi için bkz. [kullanarak NuGet paketleri](functions-reference-csharp.md#using-nuget-packages)

JavaScript işlevleri için parametre adlı tarafından *function.json* `name` özelliği, olay nesnesine bir başvuru içeriyor.

## <a name="event-schema"></a>Olay şeması

Bir Event Grid olay verilerini bir HTTP istek gövdesindeki bir JSON nesnesi olarak alınır. JSON, aşağıdaki örneğe benzer:

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

Gösterilen örnek bir öğeden oluşan bir dizidir. Event Grid, her zaman bir dizi gönderir ve dizideki birden fazla olay gönderebilir. Çalışma zamanının işlevinizi her dizi öğesi için bir kez çalıştırır.

JSON verilerini olay üst düzey özelliklere içeriğini sırasında tüm olay türleri arasında aynıdır `data` özelliği her olay türüne özeldir. Gösterilen örnekte, bir blob depolama olayı bulunur.

Ortak ve özel olay özellikleri açıklamaları için bkz. [olay özellikleri](../event-grid/event-schema.md#event-properties) Event Grid belgelerinde.

`EventGridEvent` Türü yalnızca üst düzey özellikler tanımlar; `Data` özelliği bir `JObject`.

## <a name="create-a-subscription"></a>Abonelik oluşturma

Event Grid HTTP isteklerini almaya başlaması için işlevi çağıran uç nokta URL'sini belirten bir Event Grid aboneliği oluşturun.

### <a name="azure-portal"></a>Azure portal

Azure portalında Event Grid tetikleyicisinin geliştirme işlevleri için seçin **Event Grid aboneliği Ekle**.

![Abonelik portalında oluşturma](media/functions-bindings-event-grid/portal-sub-create.png)

Bu bağlantıyı seçtiğinizde, portal açar **olay aboneliği oluşturma** sayfasında uç nokta URL'si ile doldurulmuş.

![Doldurulmuş bir uç nokta URL'si](media/functions-bindings-event-grid/endpoint-url.png)

Azure portalını kullanarak abonelikleri oluşturma hakkında daha fazla bilgi için bkz. [özel olay oluşturma - Azure portalı](../event-grid/custom-event-quickstart-portal.md) Event Grid belgelerinde.

### <a name="azure-cli"></a>Azure CLI

Kullanarak bir abonelik oluşturmak için [Azure CLI'yı](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest), kullanın [az eventgrid olay aboneliği oluşturma](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-create) komutu.

Komut işlevi çağıran uç nokta URL'sini gerektirir. Aşağıdaki örnek, sürüme özgü URL deseni gösterir:

#### <a name="version-2x-runtime"></a>Sürüm 2.x çalışma zamanı

    https://{functionappname}.azurewebsites.net/runtime/webhooks/eventgrid?functionName={functionname}&code={systemkey}

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma zamanı

    https://{functionappname}.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName={functionname}&code={systemkey}

Sistem, bir olay Kılavuzu tetikleyicisi için uç nokta URL'si dahil edilecek bir yetkilendirme anahtar anahtardır. Aşağıdaki bölümde, sistem anahtarını almak açıklanmaktadır.

(Sistem anahtarı için bir yer tutucu ile) bir blob depolama hesabına abone olan bir örnek aşağıda verilmiştir:

#### <a name="version-2x-runtime"></a>Sürüm 2.x çalışma zamanı

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/runtime/webhooks/eventgrid?functionName=imageresizefunc&code=<key>
```

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma zamanı

```azurecli
az eventgrid resource event-subscription create -g myResourceGroup \
--provider-namespace Microsoft.Storage --resource-type storageAccounts \
--resource-name myblobstorage12345 --name myFuncSub  \
--included-event-types Microsoft.Storage.BlobCreated \
--subject-begins-with /blobServices/default/containers/images/blobs/ \
--endpoint https://mystoragetriggeredfunction.azurewebsites.net/admin/extensions/EventGridExtensionConfig?functionName=imageresizefunc&code=<key>
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [blob depolama hızlı başlangıcı](../storage/blobs/storage-blob-event-quickstart.md#subscribe-to-your-storage-account) veya diğer Event Grid hızlı başlangıçları.

### <a name="get-the-system-key"></a>Sistem anahtarını alma

Aşağıdaki API (HTTP GET) kullanarak sistem anahtarı alabilirsiniz:

#### <a name="version-2x-runtime"></a>Sürüm 2.x çalışma zamanı

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgrid_extension?code={masterkey}
```

#### <a name="version-1x-runtime"></a>Sürüm 1.x çalışma zamanı

```
http://{functionappname}.azurewebsites.net/admin/host/systemkeys/eventgridextensionconfig_extension?code={masterkey}
```

İşlev uygulamanızı gerektiriyorsa Bu olduğundan bir yönetici API'si, [ana anahtarı](functions-bindings-http-webhook.md#authorization-keys). (Bir olay Kılavuzu tetikleyicisi işlevi çağırma için) sistemi anahtarı (için yönetim görevleri işlev uygulamasını gerçekleştirdiği) ana anahtarı ile karıştırmayın. Bir olay Kılavuzu konu başlığına abone olduğunuzda, sistem anahtarını kullandığınızdan emin olun.

Sistem anahtarı sağlayan yanıtın bir örnek aşağıda verilmiştir:

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

İşlev uygulamanız için ana anahtarı alma **işlev uygulaması ayarları** portalında sekmesi.

> [!IMPORTANT]
> Ana anahtarı, işlev uygulamanızı yönetici erişim sağlar. Bu anahtarı üçüncü taraflarla paylaşan veya yerel istemci uygulamaları Dağıt kullanmayın.

Daha fazla bilgi için [yetkilendirme anahtarları](functions-bindings-http-webhook.md#authorization-keys) makaledeki HTTP tetikleyici başvurusu.

Alternatif olarak, anahtar değeri kendiniz belirlemek için bir HTTP PUT gönderebilirsiniz.

## <a name="local-testing-with-viewer-web-app"></a>Yerel Görüntüleyici web uygulamasını test etme

Bir olay Kılavuzu tetikleyicisi test etmek için yerel olarak, Event Grid HTTP istekleri, kaynaktan bulutta yerel makinenize teslim gerekir. Bunu yapmanın bir yolu, çevrimiçi ve el ile bunları yerel makinenizde yeniden gönderme istekleri yakalayarak şöyledir:

1. [Görüntüleyicisi web uygulaması oluşturma](#create-a-viewer-web-app) , olay iletileri yakalar.
1. [Bir Event Grid aboneliği oluşturmak](#create-an-event-grid-subscription) , Olay Görüntüleyicisi'ni uygulamaya gönderir.
1. [Bir isteği](#generate-a-request) ve Görüntüleyicisi uygulamadan istek gövdesine kopyalayın.
1. [El ile istek gönderin](#manually-post-the-request) , Event Grid localhost URL'sini işlevi tetikleyin.

Bitirdiğinizde testi, aynı abonelik için üretim uç noktası güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI komutu.

### <a name="create-a-viewer-web-app"></a>Görüntüleyicisi web uygulaması oluşturma

Yakalama olay iletileri basitleştirmek için dağıtabileceğiniz bir [önceden oluşturulmuş bir web uygulaması](https://github.com/Azure-Samples/azure-event-grid-viewer) , olay iletileri görüntüler. Dağıtılan çözüm bir App Service planı, App Service web uygulaması ve GitHub'dan kaynak kod içerir.

Çözümü aboneliğinize dağıtmak için **Azure'a Dağıt**'ı seçin. Azure portalında parametre değerlerini girin.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

Dağıtımın tamamlanması birkaç dakika sürebilir. Dağıtım başarıyla gerçekleştirildikten sonra, web uygulamanızı görüntüleyip çalıştığından emin olun. Web tarayıcısında şu adrese gidin: `https://<your-site-name>.azurewebsites.net`

Siteyi görürsünüz ancak henüz yayımlanmış olay yoktur.

![Yeni siteyi görüntüleme](media/functions-bindings-event-grid/view-site.png)

### <a name="create-an-event-grid-subscription"></a>Event Grid aboneliği oluşturma

Test etmek istediğiniz türde bir Event Grid aboneliği oluşturun ve URL web uygulamanızdan uç noktası olarak olay bildirimi için verin. Web uygulamanızın uç noktası `/api/updates/` sonekini içermelidir. Bu nedenle, tam URL'si olur `https://<your-site-name>.azurewebsites.net/api/updates`

Azure portalını kullanarak abonelikleri oluşturma hakkında daha fazla bilgi için bkz. [özel olay oluşturma - Azure portalı](../event-grid/custom-event-quickstart-portal.md) Event Grid belgelerinde.

### <a name="generate-a-request"></a>Bir isteği oluştur

Web uygulaması uç noktanıza HTTP trafiğini oluşturacak bir olayı tetikleyin.  Örneğin, bir blob depolama aboneliği oluşturduysanız, karşıya yükleyin veya bir blobu silme. Web uygulamanıza istek gösterilir, istek gövdesine kopyalayın.

Abonelik doğrulama isteği ilk alınır; tüm doğrulama isteklerini yoksayar ve olay istek kopyalayın.

![Web uygulamasından istek gövdesine kopyalayın](media/functions-bindings-event-grid/view-results.png)

### <a name="manually-post-the-request"></a>El ile istek gönderin

Event Grid işleviniz yerel olarak çalıştırın.

Gibi bir araç kullanın [Postman](https://www.getpostman.com/) veya [curl](https://curl.haxx.se/docs/httpscripting.html) bir HTTP POST isteği oluşturmak için:

* Ayarlanmış bir `Content-Type: application/json` başlığı.
* Ayarlanmış bir `aeg-event-type: Notification` başlığı.
* RequestBin veri istek gövdesine yapıştırın.
* Event Grid tetikleyici işlevinizin URL'sini gönderin.
  * 2\.x için şu biçimi kullanın:

    ```
    http://localhost:7071/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
    ```

  * 1\.x kullanmak için:

    ```
    http://localhost:7071/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
    ```

`functionName` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Aşağıdaki ekran görüntüleri, üst bilgilerini göster ve istek gövdesi postman'deki:

![Postman üstbilgileri](media/functions-bindings-event-grid/postman2.png)

![İstek gövdesinde Postman](media/functions-bindings-event-grid/postman.png)

Olay Kılavuzu tetikleyicisi işlevi yürütür ve günlükleri aşağıdaki örneğe benzer gösterir:

![Örnek olay Kılavuzu tetikleyicisi işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="local-testing-with-ngrok"></a>Yerel ngrok ile test etme

Bir olay Kılavuzu tetikleyicisi yerel olarak test etmek için başka bir HTTP bağlantısı Internet ile geliştirme bilgisayarınızda arasında otomatik hale getirmek için yoludur. Gibi bir araç ile bunu yapabilirsiniz [ngrok](https://ngrok.com/):

1. [Ngrok uç nokta oluşturma](#create-an-ngrok-endpoint).
1. [Olay Kılavuzu tetikleyicisi işlevi çalıştırmak](#run-the-event-grid-trigger-function).
1. [Bir Event Grid aboneliği oluşturmak](#create-a-subscription) , olayları ngrok uç noktasına gönderir.
1. [Bir olayı tetikleyin](#trigger-an-event).

Bitirdiğinizde testi, aynı abonelik için üretim uç noktası güncelleştirerek kullanabilirsiniz. Kullanım [az eventgrid olay aboneliği güncelleştirme](https://docs.microsoft.com/cli/azure/eventgrid/event-subscription?view=azure-cli-latest#az-eventgrid-event-subscription-update) Azure CLI komutu.

### <a name="create-an-ngrok-endpoint"></a>Ngrok uç nokta oluşturma

İndirme *ngrok.exe* gelen [ngrok](https://ngrok.com/)ve aşağıdaki komutu çalıştırın:

```
ngrok http -host-header=localhost 7071
```

Host-komut localhost üzerinde çalıştığında, İşlevler çalışma zamanı localhost gelen istekleri beklediği için üstbilgi parametresi gereklidir. çalışma zamanı yerel olarak çalıştığında 7071 varsayılan bağlantı noktası numarasıdır.

Komut şuna benzer bir çıktı oluşturur:

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

Kullanacağınız `https://{subdomain}.ngrok.io` Event Grid aboneliğiniz için URL.

### <a name="run-the-event-grid-trigger-function"></a>Olay Kılavuzu tetikleyicisi işlevi çalıştırın

Abonelik oluşturulurken, işlevi yerel olarak çalışıyor olması gerekir böylece ngrok URL'si, Event Grid ile özel işlem elde edemez. Aksi takdirde, gönderilen doğrulama yanıtı değil ve abonelik oluşturma başarısız olur.

### <a name="create-a-subscription"></a>Abonelik oluşturma

Test etmek istediğiniz türde bir Event Grid aboneliği oluşturun ve ngrok uç noktanızı verin.

İşlevler için bu endpoint düzeni kullanın 2.x:

```
https://{SUBDOMAIN}.ngrok.io/runtime/webhooks/eventgrid?functionName={FUNCTION_NAME}
```

İşlevler için bu endpoint düzeni kullanın 1.x:

```
https://{SUBDOMAIN}.ngrok.io/admin/extensions/EventGridExtensionConfig?functionName={FUNCTION_NAME}
```

`{FUNCTION_NAME}` Parametresi, belirtilen adı olmalıdır `FunctionName` özniteliği.

Azure CLI kullanarak bir örnek aşağıda verilmiştir:

```azurecli
az eventgrid event-subscription create --resource-id /subscriptions/aeb4b7cb-b7cb-b7cb-b7cb-b7cbb6607f30/resourceGroups/eg0122/providers/Microsoft.Storage/storageAccounts/egblobstor0122 --name egblobsub0126 --endpoint https://263db807.ngrok.io/runtime/webhooks/eventgrid?functionName=EventGridTrigger
```

Bir aboneliğin nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [abonelik oluşturma](#create-a-subscription) bu makalenin üst kısmındaki.

### <a name="trigger-an-event"></a>Bir olay tetikleme

HTTP trafiğini ngrok uç noktanıza oluşturacak bir olayı tetikleyin.  Örneğin, bir blob depolama aboneliği oluşturduysanız, karşıya yükleyin veya bir blobu silme.

Olay Kılavuzu tetikleyicisi işlevi yürütür ve günlükleri aşağıdaki örneğe benzer gösterir:

![Örnek olay Kılavuzu tetikleyicisi işlev günlükleri](media/functions-bindings-event-grid/eg-output.png)

## <a name="use-an-http-trigger-as-an-event-grid-trigger"></a>HTTP tetikleyicisi bir Event Grid tetikleyici olarak kullanma

Olayları bir Event Grid INSTEAD OF tetikleyicisi bir HTTP tetikleyicisini kullanarak işleyebilmesi için event Grid olaylarını HTTP isteklerini alınır. Böylece olası bir nedeni hakkında daha fazla denetime işlevi çağıran bir uç nokta URL'si almaktır. Başka bir nedenle olayları almaya ihtiyacınız olduğunda [CloudEvents şeması](../event-grid/cloudevents-schema.md). Şu anda, Event Grid tetikleyicisinin CloudEvents şeması desteklemiyor. Bu bölümdeki örneklerde, Event Grid şema hem de CloudEvents şeması için çözümleri gösterir.

HTTP tetikleyicisi kullanıyorsanız, Event Grid tetikleyicisinin otomatik olarak yaptığı için kod yazmak zorundasınız:

* Doğrulama yanıt gönderir bir [abonelik doğrulama isteği](../event-grid/security-authentication.md#webhook-event-delivery).
* İstek gövdesinde yer alan olay dizi öğesi için bir sefer işlevi çağırır.

İşlevi yerel olarak veya Azure'da çalıştırıldığında çağırmak için kullanılacak URL hakkında daha fazla bilgi için bkz. [HTTP tetikleyici bağlama başvuru belgeleri](functions-bindings-http-webhook.md)

### <a name="event-grid-schema"></a>Olay Kılavuz şeması

Aşağıdaki örnek C# kod HTTP tetikleyicisi için olay Kılavuzu tetikleyicisi davranışını taklit eder. Bu örnek olay ızgarası şema teslim olaylar için kullanın.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, "post")]HttpRequestMessage req,
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var messages = await req.Content.ReadAsAsync<JArray>();

    // If the request is for subscription validation, send back the validation code.
    if (messages.Count > 0 && string.Equals((string)messages[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
    {
        log.LogInformation("Validate request received");
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
        log.LogInformation($"Subject: {eventGridEvent.Subject}");
        log.LogInformation($"Time: {eventGridEvent.EventTime}");
        log.LogInformation($"Event data: {eventGridEvent.Data.ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Aşağıdaki örnek JavaScript kodunu HTTP tetikleyicisi için Event Grid tetikleyici davranışını taklit eder. Bu örnek olay ızgarası şema teslim olaylar için kullanın.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var messages = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (messages.length > 0 && messages[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = messages[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for one or more events.
        // Event Grid schema delivers events in an array.
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

Döngü içinde olay işleme kodunuzu gider `messages` dizisi.

### <a name="cloudevents-schema"></a>CloudEvents şeması

Aşağıdaki örnek C# kod HTTP tetikleyicisi için olay Kılavuzu tetikleyicisi davranışını taklit eder.  CloudEvents şeması teslim edilen olaylara için bu örneği kullanın.

```csharp
[FunctionName("HttpTrigger")]
public static async Task<HttpResponseMessage> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    var requestmessage = await req.Content.ReadAsStringAsync();
    var message = JToken.Parse(requestmessage);

    if (message.Type == JTokenType.Array)
    {
        // If the request is for subscription validation, send back the validation code.
        if (string.Equals((string)message[0]["eventType"],
        "Microsoft.EventGrid.SubscriptionValidationEvent",
        System.StringComparison.OrdinalIgnoreCase))
        {
            log.LogInformation("Validate request received");
            return req.CreateResponse<object>(new
            {
                validationResponse = message[0]["data"]["validationCode"]
            });
        }
    }
    else
    {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        log.LogInformation($"Source: {message["source"]}");
        log.LogInformation($"Time: {message["eventTime"]}");
        log.LogInformation($"Event data: {message["data"].ToString()}");
    }

    return req.CreateResponse(HttpStatusCode.OK);
}
```

Aşağıdaki örnek JavaScript kodunu HTTP tetikleyicisi için Event Grid tetikleyici davranışını taklit eder. CloudEvents şeması teslim edilen olaylara için bu örneği kullanın.

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    var message = req.body;
    // If the request is for subscription validation, send back the validation code.
    if (message.length > 0 && message[0].eventType == "Microsoft.EventGrid.SubscriptionValidationEvent") {
        context.log('Validate request received');
        var code = message[0].data.validationCode;
        context.res = { status: 200, body: { "ValidationResponse": code } };
    }
    else {
        // The request is not for subscription validation, so it's for an event.
        // CloudEvents schema delivers one event at a time.
        var event = JSON.parse(message);
        context.log('Source: ' + event.source);
        context.log('Time: ' + event.eventTime);
        context.log('Data: ' + JSON.stringify(event.data));
    }
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

> [!div class="nextstepaction"]
> [Event Grid hakkında daha fazla bilgi edinin](../event-grid/overview.md)
