---
title: Azure işlevleri HTTP ve Web kancası bağlamaları
description: Azure işlevleri HTTP ve Web kancası Tetikleyicileri ve bağlamaları kullanma hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik, sunucusuz mimari, HTTP, API işlem REST
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 5f6538c69139b8cd254b44cb9875e18a14c8fa8b
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344156"
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure işlevleri HTTP ve Web kancası bağlamaları

Bu makalede, Azure işlevleri'nde HTTP bağlamaları ile nasıl çalışılacağı açıklanmaktadır. Azure işlevleri desteklediği HTTP Tetikleyicileri ve çıkış bağlamaları.

Yanıt için HTTP tetikleyicisi özelleştirilebilir [Web kancaları](https://en.wikipedia.org/wiki/Webhook). Bir Web kancası tetikleyici yalnızca bir JSON yükü kabul eder ve JSON doğrular. GitHub ve Slack gibi bazı Sağlayıcılarda, gelen Web kancaları işlemek kolaylaştırmak özel Web kancası tetikleyicisine sürümü vardır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

HTTP bağlantıları sağlanır [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

HTTP bağlantıları sağlanır [Microsoft.Azure.WebJobs.Extensions.Http](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

HTTP tetikleyicisi olan bir HTTP isteği bir işlev çağırma sağlar. HTTP tetikleyicisi, sunucusuz API oluşturma ve Web kancaları için yanıtlamak için kullanabilirsiniz. 

Varsayılan olarak, bir HTTP tetikleyicisi işlevlerini boş bir gövdeye sahip HTTP 200 OK döndürür 1.x veya işlev boş bir gövdeye sahip HTTP 204 Hayır içerik 2.x. Yanıt değiştirmek için yapılandırma bir [HTTP çıktı bağlamasını](#http-output-binding).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi. Dönüş değeri için çıkış bağlaması kullanılır, ancak bir dönüş değeri öznitelik gerekli olmadığını unutmayın.

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

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi.

İşte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "authLevel": "function",
            "name": "req",
            "type": "httpTrigger",
            "direction": "in",
            "methods": [
                "get",
                "post"
            ]
        },
        {
            "name": "$return",
            "type": "http",
            "direction": "out"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

İçin bağlanan bir C# kodu işte `HttpRequestMessage`:

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

Özel bir nesne yerine adlarınıza bağlayabileceğiniz `HttpRequestMessage`. Bu nesne, JSON olarak ayrıştırılmış istek gövdesinden oluşturulur. Benzer şekilde, çıktı bağlama ve 200 durum koduyla birlikte yanıt gövdesi olarak döndürülen HTTP yanıtına bir tür geçirilebilir.

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

### <a name="trigger---f-example"></a>Tetikleyici - F # örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlev arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kodu şu şekildedir:

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

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi.

İşte *function.json* dosyası:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "function",
            "type": "httpTrigger",
            "direction": "in",
            "name": "req"
        },
        {
            "type": "http",
            "direction": "out",
            "name": "res"
        }
    ]
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context, req) {
    context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);

    if (req.query.name || (req.body && req.body.name)) {
        context.res = {
            // status defaults to 200 */
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
* [C# betiği (.csx)](#webhook---c-script-example)
* [F#](#webhook---f-example)
* [JavaScript](#webhook---javascript-example)

### <a name="webhook---c-example"></a>Web kancası - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) genel bir JSON isteğine yanıt olarak bir HTTP 200 gönderir.

```cs
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run([HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    return req.CreateResponse(HttpStatusCode.OK);
}
```

### <a name="webhook---c-script-example"></a>Web kancası - C# betiği örneği

Aşağıdaki örnek, bir Web kancası tetikleyici bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

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

### <a name="webhook---f-example"></a>Web kancası - F # örneği

Aşağıdaki örnek, bir Web kancası tetikleyici bağlama gösterir. bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kodu şu şekildedir:

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

Aşağıdaki örnek, bir Web kancası tetikleyici bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev GitHub sorunu açıklamaları günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "github",
      "name": "req"
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ],
  "disabled": false
}
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

Web kancası türü ve rota şablonu için özelliklerin vardır ve yetkilendirme düzeyi ve izin verilen HTTP yöntemleri öznitelik oluşturucu parametresi ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz. [tetikleyici - yapılandırma](#trigger---configuration). İşte bir `HttpTrigger` özniteliği bir yöntem imzası:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous, WebHookType = "genericJson")] HttpRequestMessage req)
{
    ...
}
 ```

Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `HttpTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
| **type** | yok| Gerekli - kümesine olmalıdır `httpTrigger`. |
| **direction** | yok| Gerekli - kümesine olmalıdır `in`. |
| **Adı** | yok| Gereklidir - değişken adı işlev kodu isteği veya istek gövdesi için kullanılır. |
| <a name="http-auth"></a>**authLevel** |  **AuthLevel** |Anahtarlar, varsa, işlevin çalıştırılabilmesi için istekte bulunması gerekenleri belirler. Yetkilendirme düzeyi aşağıdaki değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;Hiçbir API anahtarı gereklidir.</li><li><code>function</code>&mdash;İşleve özgü API anahtarı gereklidir. Belirtilmezse varsayılan değer budur.</li><li><code>admin</code>&mdash;Ana anahtarı gereklidir.</li></ul> Daha fazla bilgi için konudaki [yetkilendirme anahtarları](#authorization-keys). |
| **Yöntemleri** |**Yöntemleri** | Bir dizi işlev yanıt vereceği HTTP yöntemleri. Belirtilmemişse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz: [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **yol** | **yol** | İstek, işlevinizin yanıt URL'lerini denetleme için rota şablonu tanımlar. Varsayılan değer sağlanmazsa `<functionname>`. Daha fazla bilgi için [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** |HTTP tetikleyicisi olarak davranacak şekilde yapılandırır bir [Web kancası](https://en.wikipedia.org/wiki/Webhook) belirtilen sağlayıcının alıcı. Ayarlamamanız `methods` bu özelliği ayarlarsanız özelliği. Web kancası türü aşağıdaki değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Genel amaçlı bir Web kancası uç noktası olmadan belirli bir sağlayıcı için mantığı. Bu ayar, yalnızca HTTP POST ve ile kullanmak için istekleri kısıtlar `application/json` içerik türü.</li><li><code>github</code>&mdash;İşlev yanıtlar [GitHub Web kancası](https://developer.github.com/webhooks/). Kullanmayın _authLevel_ GitHub Web kancası özellik. Daha fazla bilgi için bu makalenin devamındaki GitHub Web kancaları bölümüne bakın.</li><li><code>slack</code>&mdash;İşlev yanıtlar [Slack Web kancaları](https://api.slack.com/outgoing-webhooks). Kullanmayın _authLevel_ Slack Web kancaları ile özelliği. Daha fazla bilgi için bu makalenin devamındaki Slack Web kancaları bölümüne bakın.</li></ul>|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve F # işlevleri için ya da giriş, tetikleyici türü bildirebilirsiniz `HttpRequestMessage` veya özel bir tür. Seçerseniz `HttpRequestMessage`, istek nesnesi tam erişim elde edersiniz. Özel bir tür için işlevleri çalışır nesne özelliklerini ayarlamak için JSON isteği gövdesi ayrıştırılamadı. 

JavaScript işlevleri için istek gövdesi istek nesnesi yerine işlevler çalışma zamanı sağlar. Daha fazla bilgi için [JavaScript tetikleyicisi örneğinde](#trigger---javascript-example).

### <a name="github-webhooks"></a>GitHub Web kancası

GitHub Web kancası için yanıt için ilk işlevinizi ile HTTP tetikleyicisi oluşturma ve ayarlama **webHookType** özelliğini `github`. Ardından kendi URL'sini ve API anahtarını kopyalayın **Web kancası Ekle** GitHub deponuza sayfası. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Bir örnek için, bkz. [GitHub web kancası tarafından tetiklenen bir işlev oluşturma](functions-create-github-webhook-triggered-function.md).

### <a name="slack-webhooks"></a>Slack Web kancaları

Slack Web kancası Slack belirteçten ile bir işleve özgü anahtar yapılandırmak için belirtmenize izin verir yerine bir belirteç sizin için oluşturur. Bkz: [yetkilendirme anahtarları](#authorization-keys).

### <a name="customize-the-http-endpoint"></a>HTTP uç noktasına özelleştirme

Bir işlev için bir HTTP tetikleyicisi veya Web kancasını oluşturduğunuzda varsayılan olarak işlev biçiminde bir yol ile adreslenebilir:

    http://<yourapp>.azurewebsites.net/api/<funcname> 

Bu yol isteğe bağlı kullanarak özelleştirebileceğiniz `route` HTTP tetikleyicisi özellikte bağlama giriş. Örneğin, aşağıdaki *function.json* dosyasını tanımlayan bir `route` özelliği HTTP tetikleyicisi için:

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

Bu yapılandırmayı kullanarak artık özgün yol yerine aşağıdaki yol ile adreslenebilir işlevdir.

```
http://<yourapp>.azurewebsites.net/api/products/electronics/357
```

Böylece, iki parametre adresi desteklemek işlev kodunu _kategori_ ve _kimliği_. Kullanabilirsiniz [Web API rota kısıtlaması](https://www.asp.net/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2#constraints) , parametrelere sahip. Aşağıdaki C# işlev kodunu her iki parametre kullanır.

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

Node.js işlevi aynı yol parametreleri kullanan bir kod aşağıda verilmiştir.

```javascript
module.exports = function (context, req) {

    var category = context.bindingData.category;
    var id = context.bindingData.id;

    if (!id) {
        context.res = {
            // status defaults to 200 */
            body: "All " + category + " items were requested."
        };
    }
    else {
        context.res = {
            // status defaults to 200 */
            body: category + " item with id = " + id + " was requested."
        };
    }

    context.done();
} 
```

Varsayılan olarak, tüm işlevi yollar ile ön ekli *API*. Ayrıca özelleştirme veya önekini kullanarak kaldırma `http.routePrefix` özelliğinde, [host.json](functions-host-json.md) dosya. Aşağıdaki örnek kaldırır *API* ön eki için boş bir dize kullanarak rota öneki *host.json* dosya.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

### <a name="authorization-keys"></a>Yetkilendirme anahtarları

HTTP Tetikleyicileri ek güvenlik için anahtarları kullanmanıza izin verir. Standart bir HTTP tetikleyicisi anahtar istekte mevcut olmasını gerektiren bir API anahtarı olarak kullanabilirsiniz. Web kancaları, çeşitli yollarla, sağlayıcı neyi desteklediğine bağlı olarak istekleri yetkilendirmek için tuşlarını kullanabilirsiniz.

> [!NOTE]
> İşlevleri yerel olarak çalışan, yetkilendirme olursa olsun devre dışı bırakılır `authLevel` kümesinde `function.json`. Azure işlevleri'ne yayımlama hemen sonra `authLevel` hemen etkinleşir.

Anahtarlar, azure'daki işlev uygulamanızın bir parçası olarak depolanır ve bekleme sırasında şifrelenir. Anahtarlarınızı görüntülemek için yeni etiketler oluşturabilir veya yeni değerler için anahtarları alma, işlevlerinizin portalında birine gidin ve "Yönet"'i seçin 

İki tür anahtarlar vardır:

- **Ana bilgisayar anahtarları**: Bu anahtarlar işlev uygulamasında tüm işlevleri tarafından paylaşılır. Bir API anahtarı kullanıldığında, bu işlev uygulaması içinde herhangi bir işlev erişime izin verin.
- **İşlev anahtarları**: Bu anahtarları altında tanımlı yalnızca belirli işlevler için geçerlidir. Bunlar yalnızca, bir API anahtarı kullanıldığında, bu işlev erişim sağlar.

Her anahtar için başvuru olarak adlandırılır ve işlevi ve ana bilgisayar düzeyinde ("varsayılan" adlı) bir varsayılan anahtar yoktur. İşlev tuşları, ana bilgisayar anahtarlarını önceliklidir. İki anahtar da aynı ada sahip tanımlandığında, işlev anahtarı her zaman kullanılır.

**Ana anahtarı** her işlev uygulaması için varsayılan ana bilgisayar anahtarı tanımlanan "ana" olarak adlandırılır. Bu anahtar iptal edilemiyor. Çalışma zamanı API'leri yönetimsel erişim sağlar. Kullanarak `"authLevel": "admin"` herhangi bir tuşa JSON gerektirir; istek sunulması için bu anahtarı bağlamasında Yetkilendirme hatası oluşur.

> [!IMPORTANT]  
> Ana anahtar ile yükseltilmiş izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşan veya gerekir yerel istemci uygulamaları dağıtın. Yönetici yetki düzeyi seçerken dikkatli olun.

### <a name="api-key-authorization"></a>API anahtarı kimlik doğrulama

Varsayılan olarak HTTP tetikleyicisi HTTP isteği bir API anahtarı gerektirir. Bu nedenle, HTTP isteği normalde aşağıdaki gibi görünür:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Adlı bir sorgu dizesi değişkeni anahtar eklenebilir `code`, yukarıdaki gibi veya içinde eklenebilir bir `x-functions-key` HTTP üstbilgisi. Anahtarın değeri, işlev için tanımlanan herhangi bir işlev tuşu veya herhangi bir ana bilgisayar anahtarı olabilir.

Anahtarları gerektirmeyen anonim isteklere izin verebilirsiniz. Ayrıca ana anahtarı kullanılması gerekebilir. Kullanarak varsayılan yetkilendirme düzeyi değiştirme `authLevel` JSON bağlama bir özellik. Daha fazla bilgi için [tetikleyici - yapılandırma](#trigger---configuration).

### <a name="keys-and-webhooks"></a>Anahtarlar ve Web kancaları

Web kancası yetkilendirme Web kancası alıcı bileşeni tarafından HTTP tetikleyicisi bir parçası olarak işlenir ve mekanizması Web kancası türüne göre değişir. Her mekanizması yapar, ancak bir anahtar kullanır. Varsayılan olarak, "varsayılan" adlı işlev anahtarı kullanılır. Farklı bir anahtar kullanmak için aşağıdaki yollardan biriyle anahtar adı ile istek göndermek için Web kancası sağlayıcı yapılandırın:

- **Sorgu dizesi**: sağlayıcı anahtar adı geçen `clientid` gibi sorgu dizesi parametresi, `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **İstek üstbilgisi**: sağlayıcı anahtar adı geçen `x-functions-clientid` başlığı.

## <a name="trigger---limits"></a>Tetikleyici - sınırları

HTTP isteği uzunluğu (104,857,600 bayt) 100 MB ile sınırlıdır ve URL uzunluğu (bayt 4.096) 4 KB sınırlıdır. Bu sınırlar tarafından belirtilen `httpRuntime` çalışma zamanının öğesinin [Web.config dosyasını](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Kullanan bir işlev, HTTP tetikleyicisi yaklaşık 2,5 dakika içinde ağ geçidi işlem zaman aşımı tamamlamak değil ve HTTP 502 hata döndürür. İşlev çalışmaya devam eder, ancak bir HTTP yanıtının geri dönmek mümkün olmayacaktır. Uzun süre çalışan işlevler için zaman uyumsuz desenleri izleyin ve burada isteğinin durumu ping atabilirsiniz bir konum döndürür öneririz. Bir işlev ne kadar çalıştırabilirsiniz hakkında daha fazla bilgi için bkz: [tüketim ölçeklendirme ve barındırma - planı](functions-scale.md#consumption-plan). 

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md) dosyası HTTP tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Çıktı

HTTP isteği gönderene yanıt bağlama HTTP çıkış kullanın. Bu bağlama, bir HTTP tetikleyicisi gerektirir ve tetikleyicinin istekle ilişkili yanıt özelleştirmenizi sağlar. Bir HTTP çıktı bağlaması HTTP tetikleyicisi HTTP 200 OK işlevleri boş bir gövdeye sahip döndürür değil sağlanan ise 1.x veya işlev boş bir gövdeye sahip HTTP 204 Hayır içerik 2.x.

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya. İçin C# sınıf kitaplıkları var. Bu karşılık gelen hiçbir öznitelik özellikleri vardır ve *function.json* özellikleri. 

|Özellik  |Açıklama  |
|---------|---------|
| **type** |Ayarlanmalıdır `http`. |
| **direction** | Ayarlanmalıdır `out`. |
|**Adı** | İşlev kodu bir yanıt için kullanılan bir değişken adı veya `$return` dönüş değeri kullanılacak. |

## <a name="output---usage"></a>Çıkış - kullanım

Bir HTTP yanıt göndermek için dil standardı yanıt desenleri kullanın. İşlev dönüş türü, C# veya C# betiği olun `HttpResponseMessage` veya `Task<HttpResponseMessage>`. C# ' ta dönen değer özniteliği gerekli değildir.

Örneğin yanıt bkz [tetikleyicisi örneğinde](#trigger---example) ve [Web kancası örnek](#trigger---webhook-example).

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
