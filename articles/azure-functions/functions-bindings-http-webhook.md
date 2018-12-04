---
title: Azure işlevleri HTTP Tetikleyicileri ve bağlamaları
description: HTTP Tetikleyicileri ve bağlamaları Azure işlevleri'nde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik, sunucusuz mimari, HTTP, API işlem REST
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/21/2017
ms.author: cshoe
ms.openlocfilehash: 33f04f9deced7c4bc1c27cea5e8c431d4cd5512a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52849335"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>Azure işlevleri HTTP Tetikleyicileri ve bağlamaları

Bu makalede, HTTP Tetikleyicileri ve Azure işlevleri'nde çıkış bağlamaları ile nasıl çalışılacağı açıklanmaktadır.

Yanıt için HTTP tetikleyicisi özelleştirilebilir [Web kancaları](https://en.wikipedia.org/wiki/Webhook).

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

Varsayılan olarak, bir HTTP tetikleyicisi işlevlerini boş bir gövdeye sahip HTTP 200 OK döndürür 1.x veya işlev boş bir gövdeye sahip HTTP 204 Hayır içerik 2.x. Yanıt değiştirmek için yapılandırma bir [HTTP çıktı bağlamasını](#output).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi. Dönüş değeri için çıkış bağlaması kullanılır, ancak bir dönüş değeri öznitelik gerekli olmadığını unutmayın.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<HttpResponseMessage> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)]HttpRequestMessage req, 
    ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

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
using Microsoft.Extensions.Logging;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, ILogger log)
{
    log.LogInformation($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");

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
using Microsoft.Extensions.Logging;

public static string Run(CustomObject req, ILogger log)
{
    return "Hello " + req?.name;
}

public class CustomObject {
     public string name {get; set;}
}
```

### <a name="trigger---f-example"></a>Tetikleyici - F# örneği

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [ F# işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlev arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi.

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

İşte F# kod:

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

### <a name="trigger---python-example"></a>Tetikleyici - Python örnek

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [funkce Pythonu](functions-reference-python.md) bağlama kullanan. İşlev arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi.

İşte *function.json* dosyası:

```json
{
    "scriptFile": "__init__.py",
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

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func

def main(req: func.HttpRequest) -> func.HttpResponse:
    logging.info('Python HTTP trigger function processed a request.')

    name = req.params.get('name')
    if not name:
        try:
            req_body = req.get_json()
        except ValueError:
            pass
        else:
            name = req_body.get('name')

    if name:
        return func.HttpResponse(f"Hello {name}!")
    else:
        return func.HttpResponse(
            "Please pass a name on the query string or in the request body",
            status_code=400
        )
```

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki örnek, bir tetikleyici bağlamasında gösterir. bir *function.json* dosyası ve bir [Java işlevi](functions-reference-java.md) bağlama kullanan. İşlev bir "Hello" ile tetikleme istek gövdesi Karşılama ön ek bir istek gövdesi ile bir HTTP durum kodu 200 yanıtı döndürür.


İşte *function.json* dosyası:

```json
{
    "disabled": false,    
    "bindings": [
        {
            "authLevel": "anonymous",
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

Java kod aşağıdaki gibidir:

```java
@FunctionName("hello")
public HttpResponseMessage<String> hello(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS), Optional<String> request,
                        final ExecutionContext context)
    {
        // default HTTP 200 response code
        return String.format("Hello, %s!", request);
    }
}
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) özniteliği.

Web kancası türü ve rota şablonu için özelliklerin vardır ve yetkilendirme düzeyi ve izin verilen HTTP yöntemleri öznitelik oluşturucu parametresi ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz. [tetikleyici - yapılandırma](#trigger---configuration). İşte bir `HttpTrigger` özniteliği bir yöntem imzası:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static HttpResponseMessage Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequestMessage req)
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
| <a name="http-auth"></a>**authLevel** |  **authLevel** |Anahtarlar, varsa, işlevin çalıştırılabilmesi için istekte bulunması gerekenleri belirler. Yetkilendirme düzeyi aşağıdaki değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;Hiçbir API anahtarı gereklidir.</li><li><code>function</code>&mdash;İşleve özgü API anahtarı gereklidir. Belirtilmezse varsayılan değer budur.</li><li><code>admin</code>&mdash;Ana anahtarı gereklidir.</li></ul> Daha fazla bilgi için konudaki [yetkilendirme anahtarları](#authorization-keys). |
| **Yöntemleri** |**Yöntemleri** | Bir dizi işlev yanıt vereceği HTTP yöntemleri. Belirtilmemişse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz: [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **yol** | **yol** | İstek, işlevinizin yanıt URL'lerini denetleme için rota şablonu tanımlar. Varsayılan değer sağlanmazsa `<functionname>`. Daha fazla bilgi için [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **webHookType** | **WebHookType** | _Yalnızca sürüm 1.x çalışma zamanı için desteklenmiyor._<br/><br/>HTTP tetikleyicisi olarak davranacak şekilde yapılandırır bir [Web kancası](https://en.wikipedia.org/wiki/Webhook) belirtilen sağlayıcının alıcı. Ayarlamamanız `methods` bu özelliği ayarlarsanız özelliği. Web kancası türü aşağıdaki değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Genel amaçlı bir Web kancası uç noktası olmadan belirli bir sağlayıcı için mantığı. Bu ayar, yalnızca HTTP POST ve ile kullanmak için istekleri kısıtlar `application/json` içerik türü.</li><li><code>github</code>&mdash;İşlev yanıtlar [GitHub Web kancası](https://developer.github.com/webhooks/). Kullanmayın _authLevel_ GitHub Web kancası özellik. Daha fazla bilgi için bu makalenin devamındaki GitHub Web kancaları bölümüne bakın.</li><li><code>slack</code>&mdash;İşlev yanıtlar [Slack Web kancaları](https://api.slack.com/outgoing-webhooks). Kullanmayın _authLevel_ Slack Web kancaları ile özelliği. Daha fazla bilgi için bu makalenin devamındaki Slack Web kancaları bölümüne bakın.</li></ul>|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

İçin C# ve F# İşlevler, ya da giriş, tetikleyici türü bildirebilirsiniz `HttpRequestMessage` veya özel bir tür. Seçerseniz `HttpRequestMessage`, istek nesnesi tam erişim elde edersiniz. Özel bir tür için çalışma zamanı nesne özelliklerini ayarlamak için JSON istek gövdesini ayrıştırmak çalışır.

JavaScript işlevleri için istek gövdesi istek nesnesi yerine işlevler çalışma zamanı sağlar. Daha fazla bilgi için [JavaScript tetikleyicisi örneğinde](#trigger---javascript-example).

### <a name="customize-the-http-endpoint"></a>HTTP uç noktasına özelleştirme

Bir işlev için bir HTTP tetikleyicisi oluşturduğunuzda varsayılan olarak işlev biçiminde bir yol ile adreslenebilir:

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
                                                ILogger log)
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

### <a name="working-with-client-identities"></a>İstemci kimlikleri ile çalışma

İşlev uygulamanızı kullanıyorsa [App Service kimlik doğrulaması / yetkilendirme](../app-service/app-service-authentication-overview.md), kodunuzdan kimlik doğrulamasından geçen istemcilerin ilgili bilgileri görüntüleyebilirsiniz. Bu bilgiler olarak kullanılabilir [istek üst platform tarafından eklenen](../app-service/app-service-authentication-how-to.md#access-user-claims). 

Ayrıca, veri bağlama gelen bu bilgileri okuyabilir. Bu özellik yalnızca işlevler 2.x çalışma zamanı için kullanılabilir. Ayrıca şu anda yalnızca .NET dilleri için kullanılabilir.

Bu bilgiler .NET languagues içinde olarak kullanılabilir bir [ClaimsPrincipal](https://docs.microsoft.com/en-us/dotnet/api/system.security.claims.claimsprincipal?view=netstandard-2.0). ClaimsPrincipal kullanılabilir aşağıdaki örnekte gösterildiği gibi istek bağlamının bir parçası olarak:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkResult();
}
```

Alternatif olarak, ClaimsPrincipal yalnızca işlev imzası ek bir parametre olarak dahil edilebilir:

```csharp
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;
using Newtonsoft.Json.Linq;

public static void Run(JObject input, ClaimsPrincipal principal, ILogger log)
{
    // ...
    return;
}

```

### <a name="authorization-keys"></a>Yetkilendirme anahtarları

İşlevler geliştirme sırasında HTTP işlevi uç noktalarınıza erişmek daha zor hale getirmek için anahtarları kullanmanıza imkan tanır.  Standart bir HTTP tetikleyicisi, böyle bir API anahtarı istekteki gerektirebilir. 

> [!IMPORTANT]
> Anahtarlar, geliştirme sırasında HTTP uç noktalarınızı karartmak yardımcı olabilir, ancak bunlar üretimde HTTP tetikleyicisi güvenliğini sağlamak için bir yol olarak amaçlanmamıştır. Daha fazla bilgi için bkz. [üretimde bir HTTP uç noktası güvenli](#secure-an-http-endpoint-in-production).

> [!NOTE]
> İşlevleri 1.x çalışma zamanı içinde Web kancası sağlayıcıları, sağlayıcı neyi desteklediğine bağlı olarak çeşitli şekillerde çeşitli isteklerini yetkilendirmek için anahtarları kullanabilir. Bu bölümünde ele alınmıştır [Web kancaları ve anahtarları](#webhooks-and-keys). Sürüm 2.x çalışma zamanı, Web kancası sağlayıcıları için yerleşik destek içermez.

İki tür anahtarlar vardır:

* **Ana bilgisayar anahtarları**: Bu anahtarlar işlev uygulamasında tüm işlevleri tarafından paylaşılır. Bir API anahtarı kullanıldığında, bu işlev uygulaması içinde herhangi bir işlev erişime izin verin.
* **İşlev anahtarları**: Bu anahtarları altında tanımlı yalnızca belirli işlevler için geçerlidir. Bunlar yalnızca, bir API anahtarı kullanıldığında, bu işlev erişim sağlar.

Her anahtar için başvuru olarak adlandırılır ve işlevi ve ana bilgisayar düzeyinde ("varsayılan" adlı) bir varsayılan anahtar yoktur. İşlev tuşları, ana bilgisayar anahtarlarını önceliklidir. İki anahtar da aynı ada sahip tanımlandığında, işlev anahtarı her zaman kullanılır.

Her işlev uygulaması, ayrıca özel bir sahip **ana anahtarı**. Bu anahtar adlı bir konak anahtardır `_master`, çalışma zamanı API'leri yönetimsel erişim sağlar. Bu anahtar iptal edilemiyor. Ayarlarsanız bir yetkilendirme düzeyini `admin`, istekleri, ana anahtarı; kullanmalıdır herhangi bir tuşa Yetkilendirme hatası oluşur.

> [!CAUTION]  
> İşlev uygulamanızın ana anahtar ile verilen yükseltilmiş izinler nedeniyle değil üçüncü taraflarla bu anahtarı paylaşan veya yerel istemci uygulamaları dağıtabilirsiniz. Yönetici yetki düzeyi seçerken dikkatli olun.

### <a name="obtaining-keys"></a>Anahtarları alma

Anahtarlar, azure'daki işlev uygulamanızın bir parçası olarak depolanır ve bekleme sırasında şifrelenir. Anahtarlarınızı görüntülemek için yeni değerler oluşturmak veya yeni değerler için anahtarları alma, HTTP ile tetiklenen işlevlerde birine gidin [Azure portalında](https://portal.azure.com) seçip **Yönet**.

![Portalda işlev tuşlarını yönetin.](./media/functions-bindings-http-webhook/manage-function-keys.png)

Program aracılığıyla işlev tuşlarını almak için hiçbir desteklenen API yoktur.

### <a name="api-key-authorization"></a>API anahtarı kimlik doğrulama

Çoğu HTTP tetikleyici şablonları, istekteki bir API anahtarı gerektirir. Bu nedenle, HTTP isteği normalde şu URL gibi görünür:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Adlı bir sorgu dizesi değişkeni anahtar eklenebilir `code`, yukarıdaki gibi. Olarak da eklenebilir bir `x-functions-key` HTTP üstbilgisi. Anahtarın değeri, işlev için tanımlanan herhangi bir işlev tuşu veya herhangi bir ana bilgisayar anahtarı olabilir.

Anahtarları gerektirmeyen anonim isteklere izin verebilirsiniz. Ayrıca ana anahtarı kullanılması gerekebilir. Kullanarak varsayılan yetkilendirme düzeyi değiştirme `authLevel` JSON bağlama bir özellik. Daha fazla bilgi için [tetikleyici - yapılandırma](#trigger---configuration).

> [!NOTE]
> İşlevleri yerel olarak çalışırken, yetkilendirme bakılmaksızın belirtilen kimlik doğrulama düzeyi ayarı devre dışı bırakıldı. Azure'a yayımlama sonrasında `authLevel` tetikleyicinize ayarı zorunlu tutulur.



### <a name="secure-an-http-endpoint-in-production"></a>Bir HTTP uç noktası üretimde güvenliğini sağlama

Tam olarak üretim ortamında işlevi uç noktalarınızı güvenliğini sağlamak için uygulama aşağıdaki işlevi uygulama düzeyinde güvenlik seçeneklerden birini dikkate almanız gerekir:

* App Service kimlik doğrulamasını etkinleştirmek / işlev uygulamanız için yetkilendirme. İstemcilerin kimliğini doğrulamak için Azure Active Directory (AAD) ve çeşitli üçüncü taraf kimlik sağlayıcıları kullanan App Service platformu sağlar. İşlevleriniz için özel yetkilendirme kurallarını uygulamak için bunu kullanabilirsiniz ve işlev kodunuzu kullanıcı bilgileri ile çalışabilirsiniz. Daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme Azure App Service'te](../app-service/app-service-authentication-overview.md) ve [istemci kimliklerle çalışma](#working-with-client-identities).

* Azure API Management (APIM) isteklerinin kimliğini doğrulamak için kullanın. APIM API'si güvenlik seçenekleri gelen istekler için çeşitli sağlar. Daha fazla bilgi için bkz. [API Management kimlik doğrulama ilkeleri](../api-management/api-management-authentication-policies.md). Yerinde APIM ile işlev uygulamanızı APIM Örneğinize IP adresi yalnızca gelen istekleri kabul edecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [IP adresi sınırlamaları](ip-addresses.md#ip-address-restrictions).

* Bir Azure App Service ortamı (ASE) için işlev uygulamanızı dağıtın. ASE işlevlerinizi çalıştırmak için adanmış bir barındırma ortamı sağlar. ASE gelen tüm istekleri kimliğini doğrulamak için kullanabileceğiniz tek bir ön uç ağ geçidi yapılandırmanızı sağlar. Daha fazla bilgi için [App Service ortamı için bir Web uygulaması Güvenlik Duvarı (WAF) yapılandırma](../app-service/environment/app-service-app-service-environment-web-application-firewall.md).

Bu işlev uygulama düzeyinde güvenlik yöntemlerden birini kullanarak, HTTP ile tetiklenen işlev kimlik doğrulama düzeyini ayarlamalısınız `anonymous`.

### <a name="webhooks"></a>Web Kancaları

> [!NOTE]
> Web kancası modu, yalnızca sürüm için kullanılabilir işlevler çalışma zamanının 1.x. HTTP Tetikleyicileri sürümünde performansını artırmak için bu değişiklik yapılmıştır 2.x.

Sürümünde 1.x, Web kancası şablonları, Web kancası yükü için ek doğrulama sağlar. Sürüm 2.x temel HTTP tetikleyicisi hala çalışır ve Web kancaları için önerilen yaklaşımdır. 

#### <a name="github-webhooks"></a>GitHub Web kancası

GitHub Web kancası için yanıt için ilk işlevinizi ile HTTP tetikleyicisi oluşturma ve ayarlama **webHookType** özelliğini `github`. Ardından kendi URL'sini ve API anahtarını kopyalayın **Web kancası Ekle** GitHub deponuza sayfası. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

#### <a name="slack-webhooks"></a>Slack Web kancaları

Slack Web kancası Slack belirteçten ile bir işleve özgü anahtar yapılandırmak için belirtmenize izin verir yerine bir belirteç sizin için oluşturur. Bkz: [yetkilendirme anahtarları](#authorization-keys).

### <a name="webhooks-and-keys"></a>Web kancaları ve anahtarlar

Web kancası yetkilendirme Web kancası alıcı bileşeni tarafından HTTP tetikleyicisi bir parçası olarak işlenir ve mekanizması Web kancası türüne göre değişir. Her mekanizmasının bir anahtara bağlıdır. Varsayılan olarak, "varsayılan" adlı işlev anahtarı kullanılır. Farklı bir anahtar kullanmak için aşağıdaki yollardan biriyle anahtar adı ile istek göndermek için Web kancası sağlayıcı yapılandırın:

* **Sorgu dizesi**: sağlayıcı anahtar adı geçen `clientid` gibi sorgu dizesi parametresi, `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
* **İstek üstbilgisi**: sağlayıcı anahtar adı geçen `x-functions-clientid` başlığı.

## <a name="trigger---limits"></a>Tetikleyici - sınırları

HTTP isteği uzunluğu (104,857,600 bayt) 100 MB ile sınırlıdır ve URL uzunluğu (bayt 4.096) 4 KB sınırlıdır. Bu sınırlar tarafından belirtilen `httpRuntime` çalışma zamanının öğesinin [Web.config dosyasını](https://github.com/Azure/azure-webjobs-sdk-script/blob/v1.x/src/WebJobs.Script.WebHost/Web.config).

Kullanan bir işlev, HTTP tetikleyicisi olmayan tamamlamak yaklaşık 2,5 dakika içinde ağ geçidi zaman aşımına uğrar ve HTTP 502 hata döndürür. İşlev çalışmaya devam eder, ancak bir HTTP yanıtının geri dönmek mümkün olmayacaktır. Uzun süre çalışan işlevler için zaman uyumsuz desenleri izleyin ve burada isteğinin durumu ping atabilirsiniz bir konum döndürür öneririz. Bir işlev ne kadar çalıştırabilirsiniz hakkında daha fazla bilgi için bkz: [tüketim ölçeklendirme ve barındırma - planı](functions-scale.md#consumption-plan).

## <a name="trigger---hostjson-properties"></a>Tetikleyici - host.json özellikleri

[Host.json](functions-host-json.md) dosyası HTTP tetikleyici davranışını denetleyen ayarları içerir.

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

## <a name="output"></a>Çıktı

HTTP isteği gönderene yanıt bağlama HTTP çıkış kullanın. Bu bağlama, bir HTTP tetikleyicisi gerektirir ve tetikleyicinin istekle ilişkili yanıt özelleştirmenizi sağlar. Bir HTTP çıktı bağlaması HTTP tetikleyicisi HTTP 200 OK işlevleri boş bir gövdeye sahip döndürür değil sağlanan ise 1.x veya işlev boş bir gövdeye sahip HTTP 204 Hayır içerik 2.x.

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya. C# sınıf kitaplıkları için bunlar için karşılık gelen öznitelik özellikleri yoktur *function.json* özellikleri.

|Özellik  |Açıklama  |
|---------|---------|
| **type** |Ayarlanmalıdır `http`. |
| **direction** | Ayarlanmalıdır `out`. |
|**Adı** | İşlev kodu bir yanıt için kullanılan bir değişken adı veya `$return` dönüş değeri kullanılacak. |

## <a name="output---usage"></a>Çıkış - kullanım

Bir HTTP yanıt göndermek için dil standardı yanıt desenleri kullanın. İşlev dönüş türü, C# veya C# betiği olun `HttpResponseMessage` veya `Task<HttpResponseMessage>`. C# ' ta dönen değer özniteliği gerekli değildir.

Örneğin yanıt bkz [tetikleyicisi örneğinde](#trigger---example).

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
