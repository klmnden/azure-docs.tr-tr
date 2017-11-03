---
title: "Azure işlevleri HTTP ve Web kancası bağlamaları | Microsoft Docs"
description: "HTTP ve Web kancası Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: mattchenderson
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik, sunucusuz mimarisi, HTTP, API REST işlem"
ms.assetid: 2b12200d-63d8-4ec1-9da8-39831d5a51b1
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: mahender
ms.openlocfilehash: 3c3247592cbe2bc382d220264b0c646ee566b8a7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-http-and-webhook-bindings"></a>Azure işlevleri HTTP ve Web kancası bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırma ve HTTP Tetikleyicileri ve bağlamaları Azure işlevlerinde çalışmak açıklanmaktadır.
Bu konularda sunucusuz API'ları derleme ve Web kancası için yanıt vermek için Azure işlevleri kullanabilirsiniz.

Azure işlevleri aşağıdaki bağlamaları sağlar:
- Bir [HTTP tetikleyicisini](#httptrigger) bir HTTP isteğiyle bir işlevi çağırmak olanak tanır. Bu yanıt için özelleştirilebilir [kancalarını](#hooktrigger).
- Bir [HTTP bağlama çıktı](#output) isteğine yanıt olanak tanır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

<a name="httptrigger"></a>

## <a name="http-trigger"></a>HTTP tetikleyicisi
HTTP tetikleyicisini işlevinizde bir HTTP isteğine yanıt olarak yürütür. Belirli bir URL veya HTTP yöntemleri kümesini yanıt verecek şekilde özelleştirebilirsiniz. Bir HTTP tetikleyicisi için Web kancası yanıt verecek şekilde de yapılandırılabilir. 

İşlevler Portalı'nı kullanarak, siz de hemen önceden yapılmış bir şablon kullanarak başlayabiliriz. Seçin **yeni işlev** ve "API & Web Kancalarını" arasından **senaryo** açılır. Şablonlardan birini seçin ve tıklatın **oluşturma**.

Varsayılan olarak, bir HTTP tetikleyicisi bir HTTP 200 Tamam durum kodu ve boş bir gövde ile isteğine yanıt verir. Yanıt değiştirmek için yapılandırma bir [HTTP çıktı bağlama](#output)

### <a name="configuring-an-http-trigger"></a>Bir HTTP tetikleyicisi yapılandırma
Bir HTTP tetikleyicisi bir JSON nesnesi tarafından tanımlanan `bindings` aşağıdaki örnekte gösterildiği gibi function.json dizisi:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function",
    "methods": [ "get" ],
    "route": "values/{id}"
},
```
Bağlama aşağıdaki özellikleri destekler:

|Özellik  |Açıklama  |
|---------|---------|
| **adı** | Gerekli - istek veya istek gövdesi için işlevi kod içinde kullanılan değişken adı. Bkz: [bir HTTP tetikleyicisi koddan çalışan](#httptriggerusage). |
| **türü** | Gerekli - kümesine olmalıdır `httpTrigger`. |
| **yönü** | Gerekli - kümesine olmalıdır `in`. |
| **authLevel** | Anahtarlar, varsa, işlevin çalıştırılabilmesi için istekte bulunması gerekenleri belirler. Değer şu değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;API anahtarı gereklidir.</li><li><code>function</code>&mdash;Bir işlev özgü API anahtarı gereklidir. Bu, hiçbiri sağlanmazsa varsayılan değerdir.</li><li><code>admin</code>&mdash;Ana anahtar gereklidir.</li></ul> Daha fazla bilgi için bkz: [anahtarlarla çalışma](#keys). |
| **yöntemleri** | İşlev yanıt vereceği HTTP yöntemlerinin dizisi. Belirtilmezse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz: [HTTP uç noktası özelleştirme](#url). |
| **Rota** | İçin işlevinizin yanıt URL'leri isteği denetlemek için rota şablonu tanımlar. Varsayılan değer hiçbiri sağlanmazsa `<functionname>`. Daha fazla bilgi için bkz: [HTTP uç noktası özelleştirme](#url). |
| **webHookType** | Belirtilen sağlayıcı için bir Web kancası alıcı olarak davranmak üzere HTTP tetikleyicisini yapılandırır. Kullanmayın _yöntemleri_ bu ayarı kullanırken özelliği. Değer şu değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Genel amaçlı Web kancası uç noktası için belirli bir sağlayıcıyı mantığı olmadan.</li><li><code>github</code>&mdash;İşlev için GitHub Web kancası yanıt verir. Kullanmayın _authLevel_ bu değeri kullanılırken özelliği.</li><li><code>slack</code>&mdash;İşlev Slack kancalarını yanıt verir. Kullanmayın _authLevel_ bu değeri kullanılırken özelliği.</li></ul> Daha fazla bilgi için bkz: [Web kancası için yanıt](#hooktrigger). |

<a name="httptriggerusage"></a>
### <a name="working-with-an-http-trigger-from-code"></a>Bir HTTP tetikleyicisi kodu ile çalışma
C# ve F # işlevleri için giriş aşağıdakilerden biri olması için tetikleyici türünü bildirebilir `HttpRequestMessage` veya özel bir .NET türü. Seçerseniz `HttpRequestMessage`, istek nesnesi tam erişim sağlamak. Özel bir .NET türü için işlevleri çalışır nesne özelliklerini ayarlamak için JSON istek gövdesi ayrıştırılamadı. 

Node.js işlevleri için işlevleri çalışma zamanı yerine request nesnesi istek gövdesinde sağlar. Daha fazla bilgi için bkz: [HTTP tetikleyicisi örnekleri](#httptriggersample).


<a name="output"></a>
## <a name="http-response-output-binding"></a>HTTP yanıtının çıkış bağlama
HTTP isteği gönderene yanıt bağlama HTTP çıkış kullanın. Bu bağlamanın bir HTTP tetikleyicisi gerektirir ve tetikleyici istekle ilişkili yanıt özelleştirmenizi sağlar. Bir HTTP bağlaması çıktısını alırsanız bir HTTP tetikleyicisi HTTP 200 OK olan boş bir gövde döndürür, sağlanır. 

### <a name="configuring-an-http-output-binding"></a>Bir HTTP yapılandırma bağlama çıktı
Bir HTTP bağlaması çıkış olan bir JSON nesnesinde tanımlanan `bindings` aşağıdaki örnekte gösterildiği gibi function.json dizisi:

```json
{
    "name": "res",
    "type": "http",
    "direction": "out"
}
```
Bağlama, aşağıdaki gereken özellikleri destekler:

|Özellik  |Açıklama  |
|---------|---------|
|**adı** | Yanıt için işlevi kod içinde kullanılan değişken adı. Bkz: [bir HTTP ile çalışma çıktı kodundan bağlama](#outputusage). |
| **türü** |ayarlanmalıdır `http`. |
| **yönü** | ayarlanmalıdır `out`. |

<a name="outputusage"></a>
### <a name="working-with-an-http-output-binding-from-code"></a>Bir HTTP ile çalışma kodundan bağlama çıktı
Http veya Web kancası çağırana yanıt için çıktı parametresi kullanın. Dil standart yanıt desenleri de kullanabilirsiniz. Yanıtlar, örneğin bkz [HTTP tetikleyicisi örnekleri](#httptriggersample) ve [Web kancası tetikleyici örnekleri](#hooktriggersample).


<a name="hooktrigger"></a>
## <a name="responding-to-webhooks"></a>Web kancası için yanıt
Bir HTTP tetikleyicisi ile _webHookType_ özelliği yanıt verecek şekilde yapılandırılmış [kancalarını](https://en.wikipedia.org/wiki/Webhook). Temel yapılandırma "genericJson" ayarını kullanır. Bu istekleri yalnızca HTTP POST ile ile kısıtlayan `application/json` içerik türü.

Tetikleyici Ayrıca belirli Web kancası sağlayıcısına gibi uyarlanabilir [GitHub](https://developer.github.com/webhooks/) veya [Slack'e](https://api.slack.com/outgoing-webhooks). Bir sağlayıcı belirtildiğinde işlevleri çalışma zamanı size sağlayıcısı doğrulama mantığını işleyebilir.  

### <a name="configuring-github-as-a-webhook-provider"></a>GitHub Web kancası sağlayıcısı olarak yapılandırma
GitHub Web kancası için yanıt vermek için önce bir HTTP tetikleyicisi ile işlevinizi oluşturma ve ayarlama **webHookType** özelliğine `github`. Ardından kopyalama kendi [URL](#url) ve [API anahtarı](#keys) içine **Web kancası eklemek** GitHub deponuz sayfasında. 

![](./media/functions-bindings-http-webhook/github-add-webhook.png)

Bir örnek için bkz: [GitHub Web kancası tarafından tetiklenen bir işlev oluşturun](functions-create-github-webhook-triggered-function.md).

### <a name="configuring-slack-as-a-webhook-provider"></a>Bir Web kancası sağlayıcısı olarak kayma yapılandırma
Slack Web kancası işlevi özel bir anahtar kayma belirtecinden ile yapılandırmanız gerekir, belirtmenize izin verir yerine bir belirteç sizin için oluşturur. Bkz: [anahtarlarla çalışma](#keys).

<a name="url"></a>
## <a name="customizing-the-http-endpoint"></a>HTTP uç noktası özelleştirme
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

    http://<yourapp>.azurewebsites.net/api/products/electronics/357

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

Burada, aynı rota parametrelerini kullanmak için Node.js işlev kodu verilmiştir.

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

Varsayılan olarak, tüm işlevi yollar ile önek *API*. Ayrıca özelleştirme veya önek kullanarak kaldırma `http.routePrefix` özelliğinde, *host.json* dosya. Aşağıdaki örnek kaldırır *API* önekini için boş bir dize kullanarak rota öneki *host.json* dosya.

```json
{
    "http": {
    "routePrefix": ""
    }
}
```

Güncelleştirme hakkında ayrıntılı bilgi için *host.json* bakın, işlevinizi dosyası [işlevi uygulama dosyaları güncelleştirmek nasıl](functions-reference.md#fileupdate). 

Diğer özellikler hakkında bilgi için yapılandırabileceğiniz, *host.json* dosya için bkz: [host.json başvuru](functions-host-json.md).


<a name="keys"></a>
## <a name="working-with-keys"></a>Anahtarları ile çalışma
HTTP Tetikleyicileri ek güvenlik için anahtarları kullanmanıza olanak tanır. Standart bir HTTP tetikleyicisi istekte bulunması için anahtar gerektiren bir API anahtarı olarak kullanabilirsiniz. Web kancası bir ne sağlayıcının desteklediği bağlı olarak, çeşitli şekillerde isteklerinde yetkilendirmek için tuşlarını kullanabilirsiniz.

Anahtarları azure'da işlevi uygulamanız bir parçası olarak depolanır ve bekleyen şifrelenir. Anahtarlarınızı görüntülemek için yeni bir tane oluşturun veya anahtarları alma yeni değerler, işlevlerinizi portalındaki birine gidin ve "Manage" seçin 

Anahtarların iki tür vardır:
- **Ana bilgisayar anahtarları**: Bu anahtarları işlevi uygulamasında tüm işlevleri tarafından paylaşılır. Bir API anahtarı olarak kullanıldığında, bu işlev uygulaması içinde herhangi bir işlev erişime izin verin.
- **İşlev tuşları**: Bu anahtarları altında bunların tanımlanan yalnızca belirli işlevler için geçerlidir. Bunlar yalnızca, bir API anahtarı olarak kullanıldığında, bu işlev erişime izin ver.

Her anahtar için başvuru olarak adlandırılır ve işlev ve ana bilgisayar düzeyinde ("varsayılan" adlı) bir varsayılan anahtar yok. **Ana anahtar** her işlev uygulaması için varsayılan ana bilgisayar anahtarı "tanımlanan _master" adlı. Bu anahtarı iptal edilemiyor. Çalışma zamanı API yönetim erişim sağlar. Kullanarak `"authLevel": "admin"` JSON gerektirir; isteği sunulması için bu anahtar bağlama başka bir anahtar sonuçlanır Yetkilendirme hatası.

> [!IMPORTANT]  
> Ana anahtar ile yükseltilmiş izinler nedeniyle, bu anahtarı üçüncü taraflarla paylaşma veya gerekir yerel istemci uygulamalarında dağıtın. Yönetici yetki düzeyini seçerken dikkatli olun.

### <a name="api-key-authorization"></a>API anahtarı yetkilendirme
Varsayılan olarak, bir HTTP tetikleyicisi HTTP isteği bir API anahtarı gerektirir. Bu nedenle, HTTP isteği normalde aşağıdaki gibi görünür:

    https://<yourapp>.azurewebsites.net/api/<function>?code=<ApiKey>

Anahtar adlı bir sorgu dizesi değişkeni dahil edilebilir `code`, yukarıdaki olarak veya içinde eklenebilir bir `x-functions-key` HTTP üstbilgisi. Anahtarın değerini işlevi için tanımlanan herhangi bir işlev tuşu veya tüm ana bilgisayar anahtarı olabilir.

Anahtarlar gerektirmez anonim isteklere izin verebilirsiniz. Ayrıca ana anahtar kullanılması gerekebilir. Kullanarak varsayılan yetkilendirme düzeyini değiştirmek `authLevel` JSON bağlama özelliği. Daha fazla bilgi için bkz: [HTTP tetikleyicisini](#httptrigger).

### <a name="keys-and-webhooks"></a>Anahtarlar ve Web kancaları
Web kancası yetkilendirme Web kancası alıcı bileşeni tarafından HTTP tetikleyicisini parçası işlenir ve mekanizması Web kancası türüne göre değişir. Her mekanizması yok, ancak bir anahtar kullanır. Varsayılan olarak, "varsayılan" adlı işlevi anahtar kullanılır. Farklı bir anahtarı kullanmak için aşağıdaki yollardan biriyle istek anahtarı adıyla göndermek için Web kancası sağlayıcısı yapılandırın:

- **Sorgu dizesi**: anahtar adına Sağlayıcının geçirdiği `clientid` gibi sorgu dizesi parametresi `https://<yourapp>.azurewebsites.net/api/<funcname>?clientid=<keyname>`.
- **İstek üst bilgisi**: anahtar adına Sağlayıcının geçirdiği `x-functions-clientid` üstbilgi.

> [!NOTE]
> İşlev tuşları, ana bilgisayar anahtarları önceliklidir. Aynı ada sahip iki anahtar tanımlandığında, işlev tuşu her zaman kullanılır.


<a name="httptriggersample"></a>
## <a name="http-trigger-samples"></a>HTTP tetikleyicisi örnekleri
Aşağıdaki HTTP tetikleyicisini olduğunu varsayalım `bindings` function.json dizisi:

```json
{
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
    "authLevel": "function"
},
```

Arar dile özgü örnek bkz bir `name` parametresi sorgu dizesi veya HTTP istek gövdesi.

* [C#](#httptriggercsharp)
* [F#](#httptriggerfsharp)
* [Node.js](#httptriggernodejs)


<a name="httptriggercsharp"></a>
### <a name="http-trigger-sample-in-c"></a>HTTP tetikleyicisi örnek C# #
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

Bir .NET nesnesine yerine bağlayabilirsiniz **HttpRequestMessage**. Bu nesne JSON olarak ayrıştırılmış istek gövdesi oluşturulur. Benzer şekilde, bir tür bağlama çıkış ve 200 durum kodu ile birlikte yanıt gövdesi olarak döndürülen HTTP yanıtına geçirilebilir.

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
}
```

<a name="httptriggerfsharp"></a>
### <a name="http-trigger-sample-in-f"></a>F # HTTP tetikleyicisi örnek #
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

Gereksinim duyduğunuz bir `project.json` NuGet başvurmak için kullanılan dosya `FSharp.Interop.Dynamic` ve `Dynamitey` derlemeler aşağıdaki gibi:

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

Bu, bağımlılıkları getirmek için NuGet kullanır ve komut dosyanızı başvuruyor.

<a name="httptriggernodejs"></a>
### <a name="http-trigger-sample-in-nodejs"></a>Node.JS HTTP tetikleyicisi örnek
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
     
<a name="hooktriggersample"></a>
## <a name="webhook-samples"></a>Web kancası örnekleri
Aşağıdaki Web kancası tetikleyici olduğunu varsayalım `bindings` function.json dizisi:

```json
{
    "webHookType": "github",
    "name": "req",
    "type": "httpTrigger",
    "direction": "in",
},
```

GitHub sorunu yorum günlükleri dile özgü örneğe bakın.

* [C#](#hooktriggercsharp)
* [F#](#hooktriggerfsharp)
* [Node.js](#hooktriggernodejs)

<a name="hooktriggercsharp"></a>

### <a name="webhook-sample-in-c"></a>Web kancası örnek C# #
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

<a name="hooktriggerfsharp"></a>

### <a name="webhook-sample-in-f"></a>Web kancası örnek F # #
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

<a name="hooktriggernodejs"></a>

### <a name="webhook-sample-in-nodejs"></a>Node.JS Web kancası örnek
```javascript
module.exports = function (context, data) {
    context.log('GitHub WebHook triggered!', data.comment.body);
    context.res = { body: 'New GitHub comment: ' + data.comment.body };
    context.done();
};
```


## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

