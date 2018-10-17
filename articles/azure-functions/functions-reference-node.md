---
title: Azure işlevleri için JavaScript Geliştirici Başvurusu | Microsoft Docs
description: JavaScript kullanarak işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: azure-functions
ms.devlang: nodejs
ms.topic: reference
ms.date: 03/04/2018
ms.author: glenga
ms.openlocfilehash: eb9387cec98621e27aff7dcb40b8897e326c6706
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353501"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure işlevleri JavaScript Geliştirici Kılavuzu
Bu kılavuz, Azure işlevleri ile JavaScript Yazma ayrıntılı olarak incelenmektedir hakkında bilgi içerir.

Dışarı aktarılan bir JavaScript işlevidir `function` tetiklendiğinde yürütecek ([Tetikleyiciler içinde function.json yapılandırılmış](functions-triggers-bindings.md)). Her işleve geçirilen bir `context` alıcı ve gönderen bağlama verileri, günlüğe kaydetme ve çalışma zamanı ile iletişim kurmak için kullanılan nesne.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). Ayrıca, "Hızlı Başlangıçlar" altındaki bir öğretici uyguladığınız önerilir [ilk işlevinizi oluşturma](functions-create-first-function-vs-code.md).

## <a name="folder-structure"></a>klasör yapısı

Bir JavaScript proje için gereken klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değiştirilebilir unutmayın: bkz [KomutDosyası](functions-reference-node.md#using-scriptfile) bölümünde daha fazla ayrıntı için.

```
FunctionsProject
 | - MyFirstFunction
 | | - index.js
 | | - function.json
 | - MySecondFunction
 | | - index.js
 | | - function.json
 | - SharedCode
 | | - myFirstHelperFunction.js
 | | - mySecondHelperFunction.js
 | - node_modules
 | - host.json
 | - package.json
 | - extensions.csproj
 | - bin
```

Proje kök dizininde yok paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyası (.js) ve bağlama yapılandırma dosyası (function.json) ile bir klasörü vardır.

Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı içinde tanımlanmıştır `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](functions-triggers-bindings.md#local-development-azure-functions-core-tools). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

## <a name="exporting-a-function"></a>Bir işlevi dışa aktarma

JavaScript işlevleri dışa, aracılığıyla [ `module.exports` ](https://nodejs.org/api/modules.html#modules_module_exports) (veya [ `exports` ](https://nodejs.org/api/modules.html#modules_exports)). Varsayılan durumda, yalnızca dışarı aktarma, dosyasından adlı dışarı aktarma, dışarı aktarılan işlevin olmalıdır `run`, veya adlandırılmış dışarı aktarma `index`. Varsayılan konum işlevinizin `index.js`burada `index.js` ilgili olarak aynı üst dizine paylaşır `function.json`. Unutmayın adını `function.json`ait üst dizinidir her zaman, işlevin adı. 

Dosya konumunu yapılandırmanız ve işlevinizin adını dışarı aktarma hakkında bilgi edinin: [işlevinizin giriş noktası yapılandırma](functions-reference-node.md#configure-function-entry-point) aşağıda.

Dışarı aktarılan işlevin giriş noktanız her zaman almalıdır bir `context` ilk parametre olarak nesne.

```javascript
// You must include a context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```
```javascript
// You can also use 'arguments' to dynamically handle inputs
module.exports = async function(context) {
    context.log('Number of inputs: ' + arguments.length);
    // Iterates through trigger and input binding data
    for (i = 1; i < arguments.length; i++){
        context.log(arguments[i]);
    }
};
```

Tetikleyiciler ve bağlamalar giriş (bağlamalarını `direction === "in"`) işlevi için parametre olarak geçirilebilir. İçinde tanımlanan aynı sırada işlevine geçirilen *function.json*. JavaScript kullanarak girişleri de dinamik olarak işleyebilir [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) nesne. Örneğin, `function(context, a, b)` ve değiştirmek için `function(context, a)`, değeri almaya devam `b` başvuran tarafından işlevi kodda `arguments[2]`.

Yön bağımsız olarak tüm bağlamaları boyunca da geçirilir `context` kullanarak nesne `context.bindings` özelliği.

### <a name="exporting-an-async-function"></a>Bir zaman uyumsuz işlev dışarı aktarma
JavaScript kullanırken [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirim veya düz JavaScript [gösterir](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) (kullanılabilir değil hatasıyla işlevleri v1.x), açıkça çağırmak ihtiyacınız olmayan [ `context.done` ](#contextdone-method) işlevinizi tamamlandığını göstermek için geri çağırma. Dışarı aktarılan zaman uyumsuz işlev/Promise tamamlandığında işlevinizi tamamlanır.

Örneğin, tetiklendi ve hemen bir kısayoldur günlüğe kaydeden basit bir işlevi budur.
``` javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Bir zaman uyumsuz işlev verirken çıkış bağlamaları almak için de yapılandırabilirsiniz `return` değeri. Çıktılar kullanılarak atamaya alternatif bir yaklaşım budur [ `context.bindings` ](#contextbindings-property) özelliği.

Kullanarak bir çıkış atamak `return`, değiştirme `name` özelliğini `$return` içinde `function.json`.
```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```
JavaScript işlev kodunuzu şöyle görünebilir:
```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    // You can call and await an async method here
    return {
        body: "Hello, world!"
    };
}
```

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı kullanan bir `context` nesne için ve işlevinizden veri iletmek için ve çalışma zamanı ile iletişim kurmasına izin vermek için.

`context` Nesne her zaman ilk parametre olarak bir işlev ve yöntemler gibi olduğundan dahil edilmesi gereken `context.done` ve `context.log`, bunlar doğru çalışma zamanı kullanmak için gereklidir. İnovasyonunuz ne olursa olsun istediğiniz nesnenin adı verebilirsiniz (örneğin, `ctx` veya `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>Context.Bindings özelliği

```
context.bindings
```
Tüm girdi ve çıktı verilerini içeren adlandırılmış bir nesne döndürür. Örneğin, aşağıdaki bağlama tanımlar, *function.json* kuyruktan içeriğini erişmenizi sağlar `context.bindings.myInput` ve kullanarak bir kuyruk çıkış atama `context.bindings.myOutput`.

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
    ...
},
{
    "type":"queue",
    "direction":"out",
    "name":"myOutput"
    ...
}
```

```javascript
// myInput contains the input data, which may have properties such as "name"
var author = context.bindings.myInput.name;
// Similarly, you can set your output data
context.bindings.myOutput = { 
        some_text: 'hello world', 
        a_number: 1 };
```

Çıkış veri bağlama kullanarak tanımlamak seçebileceğinize dikkat edin `context.done` yöntemi yerine `context.binding` nesne (aşağıya bakın).

### <a name="contextbindingdata-property"></a>context.bindingData özelliği

```
context.bindingData
```
Tetikleyici meta verileri ve işlev çağırma verilerini içeren adlandırılmış bir nesne döndürür (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Tetikleyici meta veri örneği için bkz. Bu [event hubs örneği](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>Context.Done yöntemi
```
context.done([err],[propertyBag])
```

Kodunuzu bitirdi çalışma zamanı bildirir. İşlevinizi JavaScript kullanıyorsa [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirimi (kullanılabilir işlevler sürüm 8 + düğümü kullanan 2.x), kullanın gerekmez `context.done()`. `context.done` Geri çağırma örtük olarak çağrılır.

İşlevinizi bir zaman uyumsuz işlev değilse **çağırmalısınız** `context.done` çalışma zamanının işlevinizi tamamlandığını bildirmek için. Yürütme zaman aşımı eksik olması durumunda olur.

`context.done` Yöntemi geri hem bir kullanıcı tanımlı hata çalışma zamanı ve çıktı bağlaması verilerini içeren bir JSON nesnesi geçirme olanak tanır. Geçirilen özellikleri `context.done` ayarlanan herhangi bir şey üzerine yazar `context.bindings` nesne.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>Context.log yöntemi  

```
context.log(message)
```
Varsayılan izleme düzeyinde akış işlev günlükleri yazmanızı sağlar. Üzerinde `context.log`ek yöntemler günlüğe kaydetme, işlev günlükleri ile diğer izleme düzeylerinde yazmanıza olanak tanıyan kullanılabilir:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **hata (_ileti_)**   | Oturum açma veya daha düşük hata düzeyini yazar.   |
| **uyar (_ileti_)**    | Oturum açma veya daha düşük uyarı düzeyi için yazar. |
| **Info (_ileti_)**    | Oturum açma veya alt bilgi düzeyine yazar.    |
| **ayrıntılı (_ileti_)** | Ayrıntılı düzeyinde günlüğe kaydetme için yazar.           |

Aşağıdaki örnek, bir uyarı izleme düzeyini günlüğüne yazar:

```javascript
context.log.warn("Something has happened."); 
```
Yapabilecekleriniz [günlüğe kaydetme için izleme düzeyi eşiği yapılandırmak](#configure-the-trace-level-for-console-logging) host.json dosyasında. Günlükleri yazma ile ilgili daha fazla bilgi için bkz: [izleme çıkış yazma](#writing-trace-output-to-the-console) aşağıda.

Okuma [Azure işlevleri izleme](functions-monitoring.md) görüntüleme ve işlev günlükleri sorgulama hakkında daha fazla bilgi edinmek için.

## <a name="binding-data-type"></a>Bağlama veri türü

Bir giriş bağlaması için veri türünü tanımlamak için `dataType` bağlama tanımındaki özelliği. Örneğin, içeriği ikili biçimde bir HTTP isteğinin okunacak türünü kullanın. `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Seçenekler `dataType` şunlardır: `binary`, `stream`, ve `string`.

## <a name="writing-trace-output-to-the-console"></a>İzleme çıktısı konsola yazma 

İşlevleri'nde, kullandığınız `context.log` konsola izleme çıkışını yazmak için yöntemleri. İşlevler'ın v2.x içinde aracılığıyla ouputs izleme `console.log` işlevi uygulama düzeyinde yakalanır. Gelen veren anlamına gelir `console.log` bir belirli bir işlev çağrısı için bağlı değil ve bu nedenle belirli bir işlevin günlüklerini görüntülenmez. Ancak, Application Insights'a yay uygulanır. İşlevleri v1.x içinde kullanamazsınız `console.log` konsola yazma için. 

Çağırdığınızda `context.log()`, iletinizin olduğundan varsayılan izleme düzeyini konsolda yazılan _bilgisi_ izleme düzeyi. Aşağıdaki kod, bilgi izleme düzeyini konsola yazar:

```javascript
context.log({hello: 'world'});  
```

Bu kod, yukarıdaki kod eşdeğerdir:

```javascript
context.log.info({hello: 'world'});  
```

Bu kod, hata düzeyinde konsola yazar:

```javascript
context.log.error("An error has occurred.");  
```

Çünkü _hata_ en yüksek izleme günlük kaydının etkin olduğu sürece düzeyi, bu izleme, tüm izleme çıkış yazılır.

Tüm `context.log` yöntemleri destekler Node.js tarafından desteklenen aynı parametre biçimi [util.format yöntemi](https://nodejs.org/api/util.html#util_util_format_format). Varsayılan izleme düzeyini kullanarak işlev günlüklerini Yazar aşağıdaki kodu göz önünde bulundurun:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Ayrıca, aynı kodu şu biçimde yazabilirsiniz:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Konsol günlüğü için izleme düzeyini yapılandırın

İşlevleri şekilde izlemeleri, işlevlerinizden konsoluna yazılan denetim kolaylaştırır konsola yazma için eşik izleme düzeyini tanımlamanıza olanak sağlar. Konsoluna yazılan tüm izlemeleri eşiği ayarlamak için kullanın `tracing.consoleLevel` host.json dosyasındaki özellik. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir. Aşağıdaki örnek, ayrıntılı günlük kaydını etkinleştirmek için izleme eşiği ayarlar:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Değerleri **consoleLevel** adları için karşılık gelen `context.log` yöntemleri. Konsola tüm izleme günlüğü devre dışı bırakmak için ayarlanmış **consoleLevel** için _kapalı_. Daha fazla bilgi için [host.json başvurusu](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

HTTP ve Web kancası Tetikleyicileri ve bağlamaları, HTTP iletileri temsil etmek için istek ve yanıt nesneleri kullanın. çıkışı.  

### <a name="request-object"></a>İstek nesnesi

`context.req` (İstek) nesne, aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama                                                    |
| ------------- | -------------------------------------------------------------- |
| _Gövde_        | İstek gövdesini içeren bir nesne.               |
| _Üst bilgileri_     | İstek üst bilgilerini içeren bir nesne.                   |
| _Yöntemi_      | İsteğin HTTP yöntemi.                                |
| _originalUrl_ | İsteğin URL'si.                                        |
| _params_      | İstek yönlendirme parametrelerini içeren bir nesne. |
| _Sorgu_       | Sorgu parametrelerini içeren bir nesne.                  |
| _rawBody_     | Dize olarak iletinin gövdesi.                           |


### <a name="response-object"></a>Yanıt nesnesi

`context.res` (Yanıt) nesnesi aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _Gövde_    | Yanıtın gövdesini içeren bir nesne.         |
| _Üst bilgileri_ | Yanıt üst bilgilerini içeren bir nesne.             |
| _isRaw_   | Biçimlendirme yanıt atlanır gösterir.    |
| _Durumu_  | Yanıtın HTTP durum kodu.                     |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıt erişme 

HTTP tetikleyicileri ile çalışırken, çeşitli yollarla HTTP istek ve yanıt nesneleri erişebilirsiniz:

+ Gelen `req` ve `res` özellikleri `context` nesne. Bu şekilde, geleneksel düzeni HTTP verilere tam kullanmak zorunda olmak yerine bağlamı nesnesinden kullanabileceğiniz `context.bindings.name` deseni. Aşağıdaki örnek nasıl erişeceğinizi gösterir `req` ve `res` üzerindeki nesneleri `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Adlandırılmış giriş ve çıkış bağlamaları. Bu şekilde, HTTP tetikleyicisini ve bağlamalarını aynı diğer herhangi bir bağlama olarak çalışır. Aşağıdaki örnek, bir adlandırılmış kullanarak yanıt nesnesini ayarlar `response` bağlama: 

    ```json
    {
        "type": "http",
        "direction": "out",
        "name": "response"
    }
    ```
    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```
+ _[Yalnızca yanıtı]_  Çağırarak `context.res.send(body?: any)`. Bir HTTP yanıtı girişi ile oluşturulan `body` yanıt gövdesi olarak. `context.done()` örtük olarak çağrılır.

+ _[Yalnızca yanıtı]_  Çağırarak `context.done()`. Özel bir HTTP bağlaması için geçirilen yanıtı döndürür `context.done()` yöntemi. Aşağıdaki HTTP çıktı bağlamasını tanımlar bir `$return` çıkış parametresi:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version"></a>Düğüm sürümü

Aşağıdaki tabloda her önemli işlevler çalışma zamanı sürümü tarafından kullanılan Node.js sürümü gösterilmektedir:

| İşlevler sürümü | Node.js sürümü | 
|---|---|
| 1.x | 6.11.2 (çalışma zamanı tarafından kilitlendi) |
| 2.x  | _Etkin LTS_ ve _geçerli_ Node.js sürümleri (8.11.1 ve önerilen 10.6.0). Sürüm WEBSITE_NODE_DEFAULT_VERSION ayarlamak [uygulama ayarı](functions-how-to-use-azure-function-app-settings.md#settings).|

Çalışma zamanı kullanarak yukarıdaki uygulama ayarını denetleyerek veya yazdırma geçerli sürümü gördüğünüz `process.version` herhangi bir işlevden.

## <a name="dependency-management"></a>Bağımlılık yönetimi
İçinde gösterildiği gibi JavaScript kodunuzun topluluk kitaplıkları kullanmak için aşağıdaki örnekte, tüm bağımlılıkların işlev uygulamanızda Azure üzerinde yüklü olduğundan emin olmak gerekir.

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

> [!NOTE]
> Tanımladığınız bir `package.json` işlev uygulamanızın kök dosya. Dosya tanımlama uygulamasında tüm işlevleri aynı önbelleğe eklenen paketleri en iyi performansı veren paylaşmak olanak tanır. Sürüm çakışması ortaya çıkarsa, ekleyerek giderebileceğiniz bir `package.json` belirli bir işlev bir klasörde dosya.  

İşlev uygulamaları kaynak denetiminden tüm dağıtırken `package.json` deponuzda mevcut dosya, tetikleyecek bir `npm install` dağıtımı sırasında bir klasördeki. Ancak Portal veya CLI dağıtılırken paketleri el ile yüklemeniz gerekir.

İşlev uygulamanızı paketleri yüklemek için iki yolu vardır: 

### <a name="deploying-with-dependencies"></a>Bağımlılıkları ile dağıtma
1. Yerel olarak çalıştırarak tüm önkoşul paketleri yüklemek `npm install`.

2. Kodunuzu dağıtırsınız ve emin `node_modules` klasör dağıtıma dahil edilir. 


### <a name="using-kudu"></a>Kudu kullanarak
1. `https://<function_app_name>.scm.azurewebsites.net` kısmına gidin.

2. Tıklayın **konsol hata ayıklama** > **CMD**.

3. Git `D:\home\site\wwwroot`ve ardından, package.json dosyasına sürükleyin **wwwroot** sayfanın üst kısmında klasörü.  
    Dosya başka yollarla işlev uygulamanıza da karşıya yükleyebilirsiniz. Daha fazla bilgi için [işlevi uygulama dosyalarını nasıl güncelleştireceğinizi](functions-reference.md#fileupdate). 

4. Package.json dosyası karşıya yüklendikten sonra Çalıştır `npm install` komutunu **Kudu uzaktan yürütme konsol**.  
    Bu eylem, package.json dosyası içinde belirtilen paketleri indirir ve işlev uygulamasını yeniden başlatır.

## <a name="environment-variables"></a>Ortam değişkenleri
Bir ortam değişkeni veya değeri ayarlamak uygulama almak için kullanın `process.env`, burada gösterildiği gibi `GetEnvironmentVariable` işlevi:

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    context.log('Node.js timer trigger function ran!', timeStamp);   
    context.log(GetEnvironmentVariable("AzureWebJobsStorage"));
    context.log(GetEnvironmentVariable("WEBSITE_SITE_NAME"));

    context.done();
};

function GetEnvironmentVariable(name)
{
    return name + ": " + process.env[name];
}
```

## <a name="configure-function-entry-point"></a>İşlev giriş noktası yapılandırma

`function.json` Özellikleri `scriptFile` ve `entryPoint` , dışarı aktarılan işlevin adını ve konumunu yapılandırmak için kullanılabilir. Bunlar, JavaScript transpiled ise önemli olabilir.

### <a name="using-scriptfile"></a>kullanma `scriptFile`

Bir JavaScript işlevi yürütüldüğü varsayılan olarak, `index.js`, kendi ilişkili olarak aynı üst dizine paylaşan bir dosya `function.json`.

`scriptFile` şuna benzer bir klasör yapısı almak için kullanılabilir:
```
FunctionApp
 | - host.json
 | - myNodeFunction
 | | - function.json
 | - lib
 | | - nodeFunction.js
 | - node_modules
 | | - ... packages ...
 | - package.json
```

`function.json` İçin `myNodeFunction` içermelidir bir `scriptFile` çalıştırmak için dışarı aktarılan işlevin dosyasına işaret eden özelliği.
```json
{
  "scriptFile": "../lib/nodeFunction.js",
  "bindings": [
    ...
  ]
}
```

### <a name="using-entrypoint"></a>kullanma `entryPoint`

İçinde `scriptFile` (veya `index.js`), bir işlevi kullanarak dışarı aktarılmalıdır `module.exports` bulundu ve çalıştırmak için. Varsayılan olarak, bu dosyadaki adlı dışarı aktarma yalnızca dışarı aktarma tetiklendiğinde yürüten işlev olduğundan `run`, veya adlandırılmış dışarı aktarma `index`.

Bu kullanılarak yapılandırılabilir `entryPoint` içinde `function.json`:
```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

İşlevler'ın v2.x içinde destekleyen `this` kullanıcı işlevindeki bir parametre, işlev kodunu ardından olabilir gibi:
```javascript
class MyObj {
    constructor() {
        this.foo = 1;
    };
    
    function logFoo(context) { 
        context.log("Foo is " + this.foo); 
        context.done(); 
    }
}

const myObj = new MyObj();
module.exports = myObj;
```

Bu örnekte, bir nesne dışarı olsa da, bulunduğuna yürütmeleri arasında durum koruma etrafında hiçbir kuralının dikkat edin önemlidir.

## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleri için dikkat edilmesi gerekenler

JavaScript işlevleri ile çalışırken, aşağıdaki bölümlerde konuları unutmayın.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek vCPU App Service planı seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, bir plan ile birden çok Vcpu yerine tek vCPU planı seçmeniz önerilir. Bugün, işlevleri çalışır JavaScript işlevleri daha verimli bir şekilde tek vCPU VM'ler üzerinde ve daha büyük sanal makineleri kullanarak beklenen performans iyileştirmeleri üretmez. Gerektiğinde, el ile daha fazla tek vCPU VM örneği ekleyerek genişletebilir ya da otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>TypeScript ve CoffeeScript desteği
Doğrudan desteği henüz otomatik derleme TypeScript veya CoffeeScript için çalışma zamanı var olmadığından, dağıtım sırasında çalışma zamanı dışında işlenecek tür desteğini gerekir. 

### <a name="cold-start"></a>Hazırlıksız başlatma
Geliştirme Azure işlevleri'nde sunucusuz barındırma modeli, soğuk başladığında gerçeğe var. "Hazırlıksız başlatma" başvuruyor olgusu işlev uygulamanızı belirli bir süre sonra ilk kez başlatıldığında başlatılması uzun sürer. Büyük bağımlılık ağaçları ile JavaScript işlevleri için özel olarak, bu ana yavaşlama neden olabilir. İşlemi, mümkün olduğunda, hasten için [bir paket dosyası işlevlerinizin çalıştığı](run-functions-from-deployment-package.md). Birçok dağıtım yöntemi, varsayılan olarak bu modele tercih et ancak büyük kaldırmanıza yaşayan yapıyorsanız ve bir paket dosyasından çalışmıyor, bu çok büyük bir geliştirme olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

