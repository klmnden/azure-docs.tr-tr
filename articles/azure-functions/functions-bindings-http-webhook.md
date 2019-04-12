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
ms.openlocfilehash: a1d66cf4506e3b8f58572576db908812f4e2be07
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2019
ms.locfileid: "59490419"
---
# <a name="azure-functions-http-triggers-and-bindings"></a>Azure işlevleri HTTP Tetikleyicileri ve bağlamaları

Bu makalede, HTTP Tetikleyicileri ve Azure işlevleri'nde çıkış bağlamaları ile nasıl çalışılacağı açıklanmaktadır.

Yanıt için HTTP tetikleyicisi özelleştirilebilir [Web kancaları](https://en.wikipedia.org/wiki/Webhook).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

Bu makalede kod, .NET Core kullanan işlevler 2.x sözdizimi için varsayılan olarak. 1.x söz dizimi hakkında daha fazla bilgi için bkz: [1.x işlev şablonları](https://github.com/Azure/azure-functions-templates/tree/v1.x/Functions.Templates/Templates).

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

HTTP bağlantıları sağlanır [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet paketi sürüm 1.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.Http) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

HTTP bağlantıları sağlanır [Microsoft.Azure.WebJobs.Extensions.Http](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Http) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.Http/) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

HTTP tetikleyicisi olan bir HTTP isteği bir işlev çağırma sağlar. HTTP tetikleyicisi, sunucusuz API oluşturma ve Web kancaları için yanıtlamak için kullanabilirsiniz.

Varsayılan olarak, bir HTTP tetikleyicisi işlevlerini boş bir gövdeye sahip HTTP 200 OK döndürür 1.x veya işlev boş bir gövdeye sahip HTTP 204 Hayır içerik 2.x. Yanıt değiştirmek için yapılandırma bir [HTTP çıktı bağlamasını](#output).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [F#](#trigger---f-example)
* [Java](#trigger---java-examples)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , arayan bir `name` parametresi sorgu dizesi veya HTTP isteğinin gövdesi. Dönüş değeri için çıkış bağlaması kullanılır, ancak bir dönüş değeri öznitelik gerekli olmadığını unutmayın.

```cs
[FunctionName("HttpTriggerCSharp")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] 
    HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
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

İçin bağlanan bir C# kodu işte `HttpRequest`:

```cs
#r "Newtonsoft.Json"

using System.Net;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using Newtonsoft.Json;

public static async Task<IActionResult> Run(HttpRequest req, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

Özel bir nesne yerine adlarınıza bağlayabileceğiniz `HttpRequest`. Bu nesne istek gövdesinden oluşturulur ve JSON olarak ayrıştırılır. Benzer şekilde, çıktı bağlama ve 200 durum koduyla birlikte yanıt gövdesi olarak döndürülen HTTP yanıtına bir tür geçirilebilir.

```csharp
using System.Net;
using System.Threading.Tasks;
using Microsoft.Extensions.Logging;

public static string Run(Person person, ILogger log)
{   
    return person.Name != null
        ? (ActionResult)new OkObjectResult($"Hello, {person.Name}")
        : new BadRequestObjectResult("Please pass an instance of Person.");
}

public class Person {
     public string Name {get; set;}
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

### <a name="trigger---java-examples"></a>Tetikleyici - Java örnekleri

* [Sorgu dizesi parametresi okuyun](#read-parameter-from-the-query-string-java)
* [Bir POST isteğinin gövdesinden okuma](#read-body-from-a-post-request-java)
* [Bir rota parametresini okuma](#read-parameter-from-a-route-java)
* [Bir POST isteğinin gövdesinden okuma POJO'ya](#read-pojo-body-from-a-post-request-java)

Aşağıdaki örnekler bağlama HTTP tetikleyicisi bir *function.json* dosya ve ilgili [Java işlevleri](functions-reference-java.md) bağlama kullanın. 

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

#### <a name="read-parameter-from-the-query-string-java"></a>Sorgu dizesi (Java) parametresinden okuma  

Bu örnek adlı bir parametre okur ```id```kullanır ve sorgu dizesi bir JSON belgesi oluşturmak için döndürülen içerik türüyle istemciye ```application/json```. 

```java
    @FunctionName("TriggerStringGet")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("GET parameters are: " + request.getQueryParameters());

        // Get named parameter
        String id = request.getQueryParameters().getOrDefault("id", "");

        // Convert and display
        if (id.isEmpty()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String name = "fake_name";
            final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                         "\"description\": \"" + name + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-body-from-a-post-request-java"></a>Gövde, bir POST isteği (Java) okuyun  

Bu örnek olarak bir POST isteğinin gövdesi okuduğu bir ```String```ve istemci, içerik türü ile döndürülen bir JSON belgesi oluşturmak için kullandığı ```application/json```.

```java
    @FunctionName("TriggerStringPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<String>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(""));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String body = request.getBody().get();
            final String jsonDocument = "{\"id\":\"123456\", " + 
                                         "\"description\": \"" + body + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-parameter-from-a-route-java"></a>Parametre bir yol (Java) okuyun  

Bu örnek adlı zorunlu bir parametre okur ```id```ve isteğe bağlı bir parametre ```name``` rota yolu ve kullandığı bunları bir JSON belgesi oluşturmak için döndürülen içerik türüyle istemciye ```application/json```. T

```java
    @FunctionName("TriggerStringRoute")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.GET}, 
              authLevel = AuthorizationLevel.ANONYMOUS,
              route = "trigger/{id}/{name=EMPTY}") // name is optional and defaults to EMPTY
            HttpRequestMessage<Optional<String>> request,
            @BindingName("id") String id,
            @BindingName("name") String name,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Route parameters are: " + id);

        // Convert and display
        if (id == null) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final String jsonDocument = "{\"id\":\"" + id + "\", " + 
                                         "\"description\": \"" + name + "\"}";
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(jsonDocument)
                          .build();
        }
    }
```

#### <a name="read-pojo-body-from-a-post-request-java"></a>Okuma POJO'ya gövdesinden bir POST isteği (Java)  

Kodu işte ```ToDoItem``` sınıfı, bu örnekte, başvurulan:

```java

public class ToDoItem {

  private String id;
  private String description;  

  public ToDoItem(String id, String description) {
    this.id = id;
    this.description = description;
  }

  public String getId() {
    return id;
  }

  public String getDescription() {
    return description;
  }
  
  @Override
  public String toString() {
    return "ToDoItem={id=" + id + ",description=" + description + "}";
  }
}

```

Bu örnek, bir POST isteğinin gövdesi okur. İstek gövdesi otomatik olarak serbest içine serileştirilmiş alır bir ```ToDoItem``` nesnesini ve içerik türüyle istemciye döndürülen ```application/json```. ```ToDoItem``` Parametresi atandığı gibi işlevler çalışma zamanı tarafından seri ```body``` özelliği ```HttpMessageResponse.Builder``` sınıfı.

```java
    @FunctionName("TriggerPojoPost")
    public HttpResponseMessage run(
            @HttpTrigger(name = "req", 
              methods = {HttpMethod.POST}, 
              authLevel = AuthorizationLevel.ANONYMOUS)
            HttpRequestMessage<Optional<ToDoItem>> request,
            final ExecutionContext context) {
        
        // Item list
        context.getLogger().info("Request body is: " + request.getBody().orElse(null));

        // Check request body
        if (!request.getBody().isPresent()) {
            return request.createResponseBuilder(HttpStatus.BAD_REQUEST)
                          .body("Document not found.")
                          .build();
        } 
        else {
            // return JSON from to the client
            // Generate document
            final ToDoItem body = request.getBody().get();
            return request.createResponseBuilder(HttpStatus.OK)
                          .header("Content-Type", "application/json")
                          .body(body)
                          .build();
        }
    }
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [HttpTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/dev/src/WebJobs.Extensions.Http/HttpTriggerAttribute.cs) özniteliği.

Web kancası türü ve rota şablonu için özelliklerin vardır ve yetkilendirme düzeyi ve izin verilen HTTP yöntemleri öznitelik oluşturucu parametresi ayarlayabilirsiniz. Bu ayarlar hakkında daha fazla bilgi için bkz. [tetikleyici - yapılandırma](#trigger---configuration). İşte bir `HttpTrigger` özniteliği bir yöntem imzası:

```csharp
[FunctionName("HttpTriggerCSharp")]
public static Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Anonymous)] HttpRequest req)
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
| **yön** | yok| Gerekli - kümesine olmalıdır `in`. |
| **ad** | yok| Gereklidir - değişken adı işlev kodu isteği veya istek gövdesi için kullanılır. |
| <a name="http-auth"></a>**authLevel** |  **authLevel** |Anahtarlar, varsa, işlevin çalıştırılabilmesi için istekte bulunması gerekenleri belirler. Yetkilendirme düzeyi aşağıdaki değerlerden biri olabilir: <ul><li><code>anonymous</code>&mdash;Hiçbir API anahtarı gereklidir.</li><li><code>function</code>&mdash;İşleve özgü API anahtarı gereklidir. Belirtilmezse varsayılan değer budur.</li><li><code>admin</code>&mdash;Ana anahtarı gereklidir.</li></ul> Daha fazla bilgi için konudaki [yetkilendirme anahtarları](#authorization-keys). |
| **Yöntemleri** |**Yöntemler** | Bir dizi işlev yanıt vereceği HTTP yöntemleri. Belirtilmemişse, işlev tüm HTTP yöntemlerine yanıt verir. Bkz: [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **yol** | **Yol** | İstek, işlevinizin yanıt URL'lerini denetleme için rota şablonu tanımlar. Varsayılan değer sağlanmazsa `<functionname>`. Daha fazla bilgi için [http uç noktasına özelleştirme](#customize-the-http-endpoint). |
| **WebHookType** | **WebHookType** | _Yalnızca sürüm 1.x çalışma zamanı için desteklenmiyor._<br/><br/>HTTP tetikleyicisi olarak davranacak şekilde yapılandırır bir [Web kancası](https://en.wikipedia.org/wiki/Webhook) belirtilen sağlayıcının alıcı. Ayarlamamanız `methods` bu özelliği ayarlarsanız özelliği. Web kancası türü aşağıdaki değerlerden biri olabilir:<ul><li><code>genericJson</code>&mdash;Genel amaçlı bir Web kancası uç noktası olmadan belirli bir sağlayıcı için mantığı. Bu ayar, yalnızca HTTP POST ve ile kullanmak için istekleri kısıtlar `application/json` içerik türü.</li><li><code>github</code>&mdash;İşlev yanıtlar [GitHub Web kancası](https://developer.github.com/webhooks/). Kullanmayın _authLevel_ GitHub Web kancası özellik. Daha fazla bilgi için bu makalenin devamındaki GitHub Web kancaları bölümüne bakın.</li><li><code>slack</code>&mdash;İşlev yanıtlar [Slack Web kancaları](https://api.slack.com/outgoing-webhooks). Kullanmayın _authLevel_ Slack Web kancaları ile özelliği. Daha fazla bilgi için bu makalenin devamındaki Slack Web kancaları bölümüne bakın.</li></ul>|

## <a name="trigger---usage"></a>Tetikleyici - kullanım

İçin C# ve F# İşlevler, ya da giriş, tetikleyici türü bildirebilirsiniz `HttpRequest` veya özel bir tür. Seçerseniz `HttpRequest`, istek nesnesi tam erişim elde edersiniz. Özel bir tür için çalışma zamanı nesne özelliklerini ayarlamak için JSON istek gövdesini ayrıştırmak çalışır.

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
public static Task<IActionResult> Run(HttpRequest req, string category, int? id, ILogger log)
{
    if (id == null)
    {
        return (ActionResult)new OkObjectResult($"All {category} items were requested.");
    }
    else
    {
        return (ActionResult)new OkObjectResult($"{category} item with id = {id} has been requested.");
    }
    
    // -----
    log.LogInformation($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
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

İşlev uygulamanızı kullanıyorsa [App Service kimlik doğrulaması / yetkilendirme](../app-service/overview-authentication-authorization.md), kodunuzdan kimlik doğrulamasından geçen istemcilerin ilgili bilgileri görüntüleyebilirsiniz. Bu bilgiler olarak kullanılabilir [istek üst platform tarafından eklenen](../app-service/app-service-authentication-how-to.md#access-user-claims). 

Ayrıca, veri bağlama gelen bu bilgileri okuyabilir. Bu özellik yalnızca işlevler 2.x çalışma zamanı için kullanılabilir. Ayrıca şu anda yalnızca .NET dilleri için kullanılabilir.

Bu bilgiler .NET dillerinde olarak kullanılabilir bir [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal?view=netstandard-2.0). ClaimsPrincipal kullanılabilir aşağıdaki örnekte gösterildiği gibi istek bağlamının bir parçası olarak:

```csharp
using System.Net;
using Microsoft.AspNetCore.Mvc;
using System.Security.Claims;

public static IActionResult Run(HttpRequest req, ILogger log)
{
    ClaimsPrincipal identities = req.HttpContext.User;
    // ...
    return new OkObjectResult();
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
* **İşlev anahtarları**: Bu anahtarların tanımlandıkları yalnızca belirli işlevler için geçerlidir. Bunlar yalnızca, bir API anahtarı kullanıldığında, bu işlev erişim sağlar.

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

    https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?code=<API_KEY>

Adlı bir sorgu dizesi değişkeni anahtar eklenebilir `code`, yukarıdaki gibi. Olarak da eklenebilir bir `x-functions-key` HTTP üstbilgisi. Anahtarın değeri, işlev için tanımlanan herhangi bir işlev tuşu veya herhangi bir ana bilgisayar anahtarı olabilir.

Anahtarları gerektirmeyen anonim isteklere izin verebilirsiniz. Ayrıca ana anahtarı kullanılması gerekebilir. Kullanarak varsayılan yetkilendirme düzeyi değiştirme `authLevel` JSON bağlama bir özellik. Daha fazla bilgi için [tetikleyici - yapılandırma](#trigger---configuration).

> [!NOTE]
> İşlevleri yerel olarak çalışırken, yetkilendirme bakılmaksızın belirtilen kimlik doğrulama düzeyi ayarı devre dışı bırakıldı. Azure'a yayımlama sonrasında `authLevel` tetikleyicinize ayarı zorunlu tutulur.



### <a name="secure-an-http-endpoint-in-production"></a>Bir HTTP uç noktası üretimde güvenliğini sağlama

Tam olarak üretim ortamında işlevi uç noktalarınızı güvenliğini sağlamak için uygulama aşağıdaki işlevi uygulama düzeyinde güvenlik seçeneklerden birini dikkate almanız gerekir:

* App Service kimlik doğrulamasını etkinleştirmek / işlev uygulamanız için yetkilendirme. İstemcilerin kimliğini doğrulamak için Azure Active Directory (AAD) ve çeşitli üçüncü taraf kimlik sağlayıcıları kullanan App Service platformu sağlar. İşlevleriniz için özel yetkilendirme kurallarını uygulamak için bunu kullanabilirsiniz ve işlev kodunuzu kullanıcı bilgileri ile çalışabilirsiniz. Daha fazla bilgi için bkz. [kimlik doğrulama ve yetkilendirme Azure App Service'te](../app-service/overview-authentication-authorization.md) ve [istemci kimliklerle çalışma](#working-with-client-identities).

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

* **Sorgu dizesi**: Sağlayıcı anahtar adı geçen `clientid` gibi sorgu dizesi parametresi, `https://<APP_NAME>.azurewebsites.net/api/<FUNCTION_NAME>?clientid=<KEY_NAME>`.
* **İstek üstbilgisi**: Sağlayıcı anahtar adı geçen `x-functions-clientid` başlığı.

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
| **yön** | Ayarlanmalıdır `out`. |
|**ad** | İşlev kodu bir yanıt için kullanılan bir değişken adı veya `$return` dönüş değeri kullanılacak. |

## <a name="output---usage"></a>Çıkış - kullanım

Bir HTTP yanıt göndermek için dil standardı yanıt desenleri kullanın. İşlev dönüş türü, C# veya C# betiği olun `IActionResult` veya `Task<IActionResult>`. C# ' ta dönen değer özniteliği gerekli değildir.

Örneğin yanıt bkz [tetikleyicisi örneğinde](#trigger---example).

## <a name="next-steps"></a>Sonraki adımlar

[Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
