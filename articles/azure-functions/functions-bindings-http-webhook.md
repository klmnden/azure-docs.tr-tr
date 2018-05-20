---
title: Azure işlevleri HTTP ve Web kancası bağlamaları
description: HTTP ve Web kancası Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik, sunucusuz mimarisi, HTTP, API REST işlem
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: tdykstra
ms.openlocfilehash: 422563f6a4e85884f4512d797d666e470835e2d2
ms.sourcegitcommit: 688a394c4901590bbcf5351f9afdf9e8f0c89505
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure işlevleri HTTP ve Web kancası bağlamaları

Bu makalede Azure işlevleri HTTP bağlantılarında ile nasıl çalışılacağını açıklar. Azure işlevleri destekler HTTP tetikleyiciler ve çıktı bağlar.

Bir HTTP tetikleyicisi yanıtlamak için özelleştirilebilir [kancalarını](https://en.wikipedia.org/wiki/Webhook). Bir Web kancası tetikleyici yalnızca bir JSON yükü kabul eder ve JSON doğrular. Daha kolay Web kancalarını GitHub ve Slack'e gibi belirli sağlayıcılardan işlemek için özel Web kancası tetikleyici sürümü vardır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages"></a>Paketler

HTTP bağlantıları sağlanan [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet paketi. Paket için kaynak kodunu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub depo.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-package-versions](../../includes/functions-package-versions.md)]

## <a name="trigger"></a>Tetikleyici

HTTP tetikleyicisi bir HTTP isteğiyle bir işlevi çağırmak olanak sağlar. Bir HTTP tetikleyicisi sunucusuz API'ları derleme ve Web kancası için yanıt vermek için kullanabilirsiniz. 

Varsayılan olarak, bir HTTP tetikleyicisi bir HTTP 200 Tamam durum kodu ve boş bir gövde ile isteğine yanıt verir. Yanıt değiştirmek için yapılandırma bir [HTTP bağlama çıktı](#http-output-binding).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , arar bir `name` parametresi sorgu dizesi veya HTTP istek gövdesi.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bir tetikleyici bağlama gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev arar bir `name` parametresi sorgu dizesi veya HTTP istek gövdesi.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

İçin bağlayan bir C# kodu işte `HttpRequestMessage`:

```csharp
using System.Net;
using System.Threading.Tasks;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

    // parse query parameter
    string name = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "name", true) == 0)
        .Value;

    // Get request body
    dynamic data = await req.Content.ReadAsAsync<object>();

    // Set name to query string or body data
    name = name ?? data?.name;

    return name == null
        ? req.CreateResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
        : req.CreateResponse(HttpStatusCode.OK, "Hello " + name);
}
```

Yerine özel bir nesne bağlayabilirsiniz `HttpRequestMessage`. Bu nesne JSON olarak ayrıştırılmış istek gövdesi oluşturulur. Benzer şekilde, bir tür bağlama çıkış ve 200 durum kodu ile birlikte yanıt gövdesi olarak döndürülen HTTP yanıtına geçirilebilir.

```csharp
using System.Net;
using System.Threading.Tasks;

public static string Run(CustomObject req, TraceWriter log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public String name {get; set;}
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F # örnek

Aşağıdaki örnek, bir tetikleyici bağlama gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev arar bir `name` parametresi sorgu dizesi veya HTTP istek gövdesi.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kod aşağıdaki gibidir:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic

let Run(req: HttpRequestMessage) =
    async {
        let q =
            req.GetQueryNameValuePairs()
                |> Seq.tryFind (fun kv -> kv.Key = "name")
        match q with
        | Some kv ->
            return req.CreateResponse(HttpStatusCode.OK, "Hello " + kv.Value)
        | None ->
            let! data = Async.AwaitTask(req.Content.ReadAsAsync<obj>())
            try
                return req.CreateResponse(HttpStatusCode.OK, "Hello " + data?name)
            with e ->
                return req.CreateErrorResponse(HttpStatusCode.BadRequest, "Please pass a name on the query string or in the request body")
    } |> Async.StartAsTask
```

Gereksinim duyduğunuz bir `project.json` NuGet başvurmak için kullanılan dosya `FSharp.Interop.Dynamic` ve `Dynamitey` derlemeler, aşağıdaki örnekte gösterildiği gibi:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir tetikleyici bağlama gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev arar bir `name` parametresi sorgu dizesi veya HTTP istek gövdesi.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
    context.done();
};
```
     
## <a name="trigger---webhook-example"></a>Tetikleyici - Web kancası örneği

Dile özgü örneğe bakın:

* [C#](#webhook---c-example)
* [C# betik (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### <a name="webhook---c-example"></a>Web kancası - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) genel bir JSON isteğine yanıt olarak bir HTTP 200 gönderir.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Web kancası - C# kod örneği

Aşağıdaki örnek, bağlama Web kancası tetikleyici gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

```csharp
#r "Newtonsoft.Json"

using System;
using System.Net;
using System.Threading.Tasks;
using Newtonsoft.Json;

public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
{
    string jsonContent = await req.Content.ReadAsStringAsync();
    dynamic data = JsonConvert.DeserializeObject(jsonContent);

    log.Info($"WebHook was triggered! Comment: {data.comment.body}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        body = $"New GitHub comment: {data.comment.body}"
    });
}
```

### <a name="webhook---f-example"></a>Web kancası - F # örnek

Aşağıdaki örnek, bağlama Web kancası tetikleyici gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kod aşağıdaki gibidir:

```fsharp
open System.Net
open System.Net.Http
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Response = {
    body: string
}

let Run(req: HttpRequestMessage, log: TraceWriter) =
    async {
        let! content = req.Content.ReadAsStringAsync() |> Async.AwaitTask
        let data = content |> JsonConvert.DeserializeObject
        log.Info(sprintf "GitHub WebHook triggered! %s" data?comment?body)
        return req.CreateResponse(
            HttpStatusCode.OK,
            { body = sprintf "New GitHub comment: %s" data?comment?body })
    } |> Async.StartAsTask
```

### <a name="webhook---javascript-example"></a>Web kancası - JavaScript örneği

Aşağıdaki örnek, bağlama Web kancası tetikleyici gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) özniteliği.

Web kancası türü ve rota şablonu için özelliklerin vardır ve düzeyi ve izin verilen HTTP yöntemleri özniteliği Oluşturucusu parametreler yetkilendirme ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz: [tetikleyici - yapılandırma](#trigger---configuration). Burada bir `HttpTrigger` bir yöntem imzası özniteliğinde:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `HttpTrigger` özniteliği.


|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type** | yok| Gerekli - kümesine olmalıdır `httpTrigger`. |
| **direction** | yok| Gerekli - kümesine olmalıdır `in`. |
| **Adı** | yok| Gerekli - istek veya istek gövdesi için işlevi kod içinde kullanılan değişken adı. |
| <a name="http-auth"></a>**AuthLevel** |  **AuthLevel** |Anahtarlar, varsa, işlevin çalıştırılabilmesi için istekte bulunması gerekenleri belirler. Yetki düzeyini aşağıdaki değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;API anahtarı gereklidir.</li><li><code>function</code>&mdash;Bir işlev özgü API anahtarı gereklidir. Bu, hiçbiri sağlanmazsa varsayılan değerdir.</li><li><code>admin</code>&mdash;Ana anahtar gereklidir.</li></ul> Daha fazla bilgi için, bkz [yetkilendirme anahtarları](#authorization-keys). |
| **Yöntemleri** |**Yöntemleri** | İşlev yanıt vereceği HTTP yöntemlerinin dizisi. Belirtilmezse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz: [http uç noktası özelleştirme](#customize-the-http-endpoint). |
| **Rota** | **Rota** | İçin işlevinizin yanıt URL'leri isteği denetlemek için rota şablonu tanımlar. Varsayılan değer hiçbiri sağlanmazsa `<functionname>`. Daha fazla bilgi için bkz: [http uç noktası özelleştirme](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |HTTP tetikleyici olarak görev yapması için yapılandırır bir [Web kancası](https://en.wikipedia.org/wiki/Webhook) belirtilen sağlayıcı için alıcı. Ayarlamazsanız `methods` bu özelliği ayarlarsanız özelliği. Web kancası türü aşağıdaki değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Genel amaçlı Web kancası uç noktası için belirli bir sağlayıcıyı mantığı olmadan. Bu ayar, yalnızca HTTP POST ve ile kullanmak için istekleri sınırlar `application/json` içerik türü.</li><li><code>github</code>&mdash;İşlev yanıtlar [GitHub Web kancası](https://developer.github.com/webhooks/). Kullanmayın _authLevel_ GitHub Web kancası özellik. Daha fazla bilgi için bu makalenin sonraki bölümlerinde GitHub Web kancası bölümüne bakın.</li><li><code>slack</code>&mdash;İşlev yanıtlar [Slack kancalarını](https://api.slack.com/outgoing-webhooks). Kullanmayın _authLevel_ Slack Web kancası özellik. Daha fazla bilgi için bu makalenin sonraki bölümlerinde Slack Web kancalarını bölümüne bakın.</li></ul>|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve F # işlevleri için giriş aşağıdakilerden biri olması için tetikleyici türünü bildirebilir `HttpRequestMessage` veya özel bir tür. Seçerseniz `HttpRequestMessage`, istek nesnesi tam erişim sağlamak. Bir özel tür için işlevleri çalışır nesne özelliklerini ayarlamak için JSON istek gövdesi ayrıştırılamadı. 

JavaScript işlevleri için işlevleri çalışma zamanı yerine request nesnesi istek gövdesinde sağlar. Daha fazla bilgi için bkz: [JavaScript tetikleyici örnek](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub Web kancaları

GitHub Web kancası için yanıt vermek için önce bir HTTP tetikleyicisi ile işlevinizi oluşturma ve ayarlama **webHookType** özelliğine `github`. İçine URL'sini ve API anahtarını kopyalayın **Web kancası eklemek** GitHub deponuz sayfası. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Bir örnek için, bkz. [GitHub web kancası tarafından tetiklenen bir işlev oluşturma](functions-create-github-webhook-triggered-function.md).

### <a name="slack-webhooks"></a>Slack Web kancaları

Slack Web kancası işlevi özel bir anahtar kayma belirtecinden ile yapılandırmanız gerekir, belirtmenize izin verir yerine bir belirteç sizin için oluşturur. Bkz: [yetkilendirme anahtarları](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>HTTP uç noktası özelleştirme

Bir HTTP tetikleyicisi ya da Web kancası, bir işlev oluşturduğunuzda varsayılan olarak işlevi adreslenebilir biçiminde bir yol şu şekildedir:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

İsteğe bağlı kullanarak bu yolun özelleştirebilirsiniz `route` HTTP tetikleyicisini özellikte bağlama girdisini. Örneğin, aşağıdaki *function.json* dosya tanımlayan bir `route` özelliği bir HTTP tetikleyicisi için:

```json
{
    "bindings": [
    {
        "type": "httpTrigger",
        "name": "req",
        "direction": "in",
        "methods": [ "get" ],
        "route": "products/{category:alpha}/{id:int?}"
    },
    {
        "type": "http",
        "name": "res",
        "direction": "out"
    }
    ]
}
```

Bu yapılandırma, işlevi artık özgün yol yerine aşağıdaki yol ile adreslenebilir kullanmaktır.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Bu adresi, iki parametrelerini desteklemek işlev kodu sağlar _kategori_ ve _kimliği_. Kullanabilirsiniz [Web API rota kısıtlaması](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) , parametrelere sahip. Aşağıdaki C# işlevi kodu her iki parametrelerini kullanır.

```csharp
public static Task<HttpResponseMessage> Run(HttpRequestMessage req, string category, int? id, 
                                                TraceWriter log)
{
    if (id == null)
        return  req.CreateResponse(HttpStatusCode.OK, $"All {category} items were requested.");
    else
        return  req.CreateResponse(HttpStatusCode.OK, $"{category} item with id = {id} has been requested.");
}
```

Burada, aynı rota parametreleri kullanan Node.js işlevi kodu verilmiştir.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Varsayılan olarak, tüm işlevi yollar ile önek *API*. Ayrıca özelleştirme veya önek kullanarak kaldırma `http.routePrefix` özelliğinde, [host.json](functions-host-json.md) dosya. Aşağıdaki örnek kaldırır *API* önekini için boş bir dize kullanarak rota öneki *host.json* dosya.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Yetkilendirme anahtarları

HTTP Tetikleyicileri ek güvenlik için anahtarları kullanmanıza olanak tanır. Standart bir HTTP tetikleyicisi istekte bulunması için anahtar gerektiren bir API anahtarı olarak kullanabilirsiniz. Web kancası bir ne sağlayıcının desteklediği bağlı olarak, çeşitli şekillerde isteklerinde yetkilendirmek için tuşlarını kullanabilirsiniz.

> [!NOTE]
> İşlevler yerel olarak çalıştırırken, yetkilendirme bağımsız devre dışı `authLevel` kümesinde `function.json`. Azure işlevleri için yayımlama hemen `authLevel` hemen, etkili olur.

Anahtarları azure'da işlevi uygulamanız bir parçası olarak depolanır ve bekleyen şifrelenir. Anahtarlarınızı görüntülemek için yeni kampanya oluşturmak veya anahtarları alma yeni değerler, işlevlerinizi portalında birine gidin ve "Manage" seçin 

Anahtarların iki tür vardır:

- **Ana bilgisayar anahtarları**: Bu anahtarları işlevi uygulamasında tüm işlevleri tarafından paylaşılır. Bir API anahtarı olarak kullanıldığında, bu işlev uygulaması içinde herhangi bir işlev erişime izin verin.
- **İşlev tuşları**: Bu anahtarları altında bunların tanımlanan yalnızca belirli işlevler için geçerlidir. Bunlar yalnızca, bir API anahtarı olarak kullanıldığında, bu işlev erişime izin ver.

Her anahtar için başvuru olarak adlandırılır ve işlev ve ana bilgisayar düzeyinde ("varsayılan" adlı) bir varsayılan anahtar yok. İşlev tuşları, ana bilgisayar anahtarları önceliklidir. Aynı ada sahip iki anahtar tanımlandığında, işlev tuşu her zaman kullanılır.

**Ana anahtar** her işlev uygulaması için varsayılan ana bilgisayar anahtarı "tanımlanan _master" adlı. Bu anahtarı iptal edilemiyor. Çalışma zamanı API yönetim erişim sağlar. Kullanarak `"authLevel": "admin"` JSON gerektirir; isteği sunulması için bu anahtar bağlama başka bir anahtar sonuçlanır Yetkilendirme hatası.

> [!IMPORTANT]  
> Ana anahtar ile yükseltilmiş izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşma veya gerekir yerel istemci uygulamalarında dağıtın. Yönetici yetki düzeyini seçerken dikkatli olun.

### <a name="api-key-authorization"></a>API anahtarı yetkilendirme

Varsayılan olarak, bir HTTP tetikleyicisi HTTP isteği bir API anahtarı gerektirir. Bu nedenle, HTTP isteği normalde aşağıdaki gibi görünür:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Anahtar adlı bir sorgu dizesi değişkeni dahil edilebilir `code`, yukarıdaki olarak veya içinde eklenebilir bir `x-functions-key` HTTP üstbilgisi. Anahtarın değerini işlevi için tanımlanan herhangi bir işlev tuşu veya tüm ana bilgisayar anahtarı olabilir.

Anahtarlar gerektirmez anonim isteklere izin verebilirsiniz. Ayrıca ana anahtar kullanılması gerekebilir. Kullanarak varsayılan yetkilendirme düzeyini değiştirmek `authLevel` JSON bağlama özelliği. Daha fazla bilgi için bkz: [tetikleyici - yapılandırma](#trigger---configuration).

### <a name="keys-and-webhooks"></a>Anahtarlar ve Web kancaları

Web kancası yetkilendirme Web kancası alıcı bileşeni tarafından HTTP tetikleyicisini parçası işlenir ve mekanizması Web kancası türüne göre değişir. Her mekanizması yok, ancak bir anahtar kullanır. Varsayılan olarak, "varsayılan" adlı işlevi anahtar kullanılır. Farklı bir anahtarı kullanmak için aşağıdaki yollardan biriyle istek anahtarı adıyla göndermek için Web kancası sağlayıcısı yapılandırın:

- **Sorgu dizesi**: anahtar adına Sağlayıcının geçirdiği `clientid` gibi sorgu dizesi parametresi `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **İstek üst bilgisi**: anahtar adına Sağlayıcının geçirdiği `x-functions-clientid` üstbilgi.

## <a name="trigger---limits"></a>Tetikleyici - sınırları

HTTP istek uzunluğu 100 MB (104,857,600 bayt) sınırlıdır ve URL uzunluğu (4.096 bayt) 4 KB ile sınırlıdır. Bu sınırlar tarafından belirtilen `httpRuntime` zamanının öğesinin [Web.config dosyasında](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Kullanan bir işlev, HTTP tetikleyicisini değil yaklaşık 2,5 dakika içinde ağ geçidi zaman aşımı tamamlayın ve HTTP 502 hata döndürür. İşlev çalışmaya devam eder ancak bir HTTP yanıtı döndüremedi görüntülenir. Uzun süre çalışan işlevleri için zaman uyumsuz desenleri izleyin ve isteğin durumunu burada ping bir konum döndürür öneririz. Bir işlev ne kadar çalıştırabilirsiniz hakkında daha fazla bilgi için bkz: [ölçek ve barındırma - tüketim planlama](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md) dosyası HTTP tetikleyicisi davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Çıktı

HTTP isteği gönderene yanıt bağlama HTTP çıkış kullanın. Bu bağlamanın bir HTTP tetikleyicisi gerektirir ve tetikleyici istekle ilişkili yanıt özelleştirmenizi sağlar. Bir HTTP bağlaması çıktısını alırsanız bir HTTP tetikleyicisi HTTP 200 OK olan boş bir gövde döndürür, sağlanır. 

## <a name="output---configuration"></a>Çıktı - yapılandırma

C# sınıfı kitaplıklar için hiçbir çıkış özel bağlama yapılandırma özellikleri vardır. Bir HTTP yanıtının göndermek için dönüş türü işlevi olun `HttpResponseMessage` veya `Task<HttpResponseMessage>`.

Diğer diller için bir HTTP bağlaması çıktı bir JSON nesnesi olarak tanımlanan `bindings` aşağıdaki örnekte gösterildiği gibi function.json dizisi:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya.

|Özellik  |Açıklama  |
|---------|---------|
| **type** |ayarlanmalıdır `http`. |
| **direction** | ayarlanmalıdır `out`. |
|**Adı** | Yanıt için işlevi kod içinde kullanılan değişken adı. |

## <a name="output---usage"></a>Çıktı - kullanım

HTTP veya Web kancası çağırana yanıt için çıktı parametresi kullanın. Dil standart yanıt desenleri de kullanabilirsiniz. Yanıtlar, örneğin bkz [tetikleyici örnek](#trigger---example) ve [Web kancası örnek](#trigger---webhook-example).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
