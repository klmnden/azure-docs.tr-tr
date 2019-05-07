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
ms.date: 02/24/2019
ms.author: glenga
ms.openlocfilehash: 2eea1a1d30558765a2f8320b0b23efdbe3368807
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65140952"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure işlevleri JavaScript Geliştirici Kılavuzu

Bu kılavuz, Azure işlevleri ile JavaScript Yazma ayrıntılı olarak incelenmektedir hakkında bilgi içerir.

Dışarı aktarılan bir JavaScript işlevidir `function` tetiklendiğinde çalışır ([Tetikleyiciler içinde function.json yapılandırılmış](functions-triggers-bindings.md)). Her bir işleve geçirilen ilk bağımsız değişken bir `context` alıcı ve gönderen bağlama verileri, günlüğe kaydetme ve çalışma zamanı ile iletişim kurmak için kullanılan nesne.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). İlk oluşturmak için İşlevler hızlı başlangıcı tamamlamak kullanarak işlev [Visual Studio Code](functions-create-first-function-vs-code.md) veya [portalında](functions-create-first-azure-function.md).

Bu makalede ayrıca destekler [TypeScript uygulama geliştirme](#typescript).

## <a name="folder-structure"></a>klasör yapısı

Bir JavaScript proje için gereken klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değiştirilebilir. Daha fazla bilgi için [KomutDosyası](#using-scriptfile) bölümüne bakın.

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
```

Proje kök dizininde yok paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyası (.js) ve bağlama yapılandırma dosyası (function.json) ile bir klasörü vardır. Adını `function.json`ait üst dizinidir her zaman, işlevin adı.

Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı içinde tanımlanmıştır `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](./functions-bindings-register.md#local-development-with-azure-functions-core-tools-and-extension-bundles). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

## <a name="exporting-a-function"></a>Bir işlevi dışa aktarma

JavaScript işlevleri dışa, aracılığıyla [ `module.exports` ](https://nodejs.org/api/modules.html#modules_module_exports) (veya [ `exports` ](https://nodejs.org/api/modules.html#modules_exports)). Dışarı aktarılan işlevinizi tetiklendiğinde yürüten bir JavaScript işlevi olmalıdır.

Varsayılan olarak, İşlevler çalışma zamanı işlevinizde arar `index.js`burada `index.js` kendi ilişkili olarak aynı üst dizine paylaşır `function.json`. Varsayılan durumda, yalnızca dışarı aktarma, dosyadan veya adlandırılmış dışarı aktarma, dışarı aktarılan işlevin olmalıdır `run` veya `index`. Dosya konumunu yapılandırmanız ve işlevinizin adını dışarı aktarma hakkında bilgi edinin: [işlevinizin giriş noktası yapılandırma](functions-reference-node.md#configure-function-entry-point) aşağıda.

Dışarı aktarılan işlevinizi yürütülmesine sayıda bağımsız değişken geçirildi. Her zaman, gereken ilk bağımsız değişken olduğu bir `context` nesne. İşlevinizi zaman uyumlu ise (Promise döndürmeyen), geçmesi gereken `context` nesnesini çağırmak kadar `context.done` doğru kullanımı için gereklidir.

```javascript
// You should include context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
```

### <a name="exporting-an-async-function"></a>Bir zaman uyumsuz işlev dışarı aktarma
Kullanırken [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirim veya düz JavaScript [gösterir](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/Promise) sürüm 2.x çalışma zamanı işlevleri değil açıkça çağırmak için size gereken [ `context.done` ](#contextdone-method) işlevinizi tamamlandığını göstermek için geri çağırma. İşlevinizi, dışarı aktarılan zaman uyumsuz işlev/Promise tamamlandığında da tamamlanır. Sürüm 1.x çalışma zamanını hedefleyen işlevler için hala çağırmalısınız [ `context.done` ](#contextdone-method) kodunuz tamamlandığında yürütülüyor.

Aşağıdaki örnek tetiklendi ve hemen bir kısayoldur günlüğe kaydeden basit bir işlevdir.

```javascript
module.exports = async function (context) {
    context.log('JavaScript trigger function processed a request.');
};
```

Bir zaman uyumsuz işlev verirken almak için bir çıkış bağlaması de yapılandırabilirsiniz `return` değeri. Yalnızca bir çıkış bağlaması varsa, bu önerilir.

Kullanarak bir çıkış atamak `return`, değiştirme `name` özelliğini `$return` içinde `function.json`.

```json
{
  "type": "http",
  "direction": "out",
  "name": "$return"
}
```

Bu durumda, işlevinizi aşağıdaki örnekteki gibi görünmelidir:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');
    // You can call and await an async method here
    return {
        body: "Hello, world!"
    };
}
```

## <a name="bindings"></a>Bağlamalar 
JavaScript'te [bağlamaları](functions-triggers-bindings.md) yapılandırılır ve işlevin function.json içinde tanımlanır. İşlevleri, çeşitli yollarla bağlamalarla etkileşim kurun.

### <a name="inputs"></a>Girişler
Giriş, Azure işlevleri'nde iki kategoriye ayrılmıştır: Tetikleyici girişi biridir ve diğer ek girişi. Tetikleyici ve diğer giriş bağlamaları (bağlamalarını `direction === "in"`) üç yolla bir işlev tarafından okunabilir:
 - **_[Önerilen]_  İşlevinize geçirilen parametreler olarak.** İçinde tanımlanan aynı sırada işlevine geçirilen *function.json*. `name` Tanımlanan özellik *function.json* olması gerektiği olsa da, parametre adıyla eşleşmesi gerekmez.
 
   ```javascript
   module.exports = async function(context, myTrigger, myInput, myOtherInput) { ... };
   ```
   
 - **Üyeleri olarak [ `context.bindings` ](#contextbindings-property) nesne.** Her üye tarafından adlandırılan `name` tanımlanan özellik *function.json*.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + context.bindings.myTrigger);
       context.log("This is myInput: " + context.bindings.myInput);
       context.log("This is myOtherInput: " + context.bindings.myOtherInput);
   };
   ```
   
 - **JavaScript kullanarak girdi olarak [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) nesne.** Bu, aslında giriş parametre olarak geçirmeyi aynıdır, ancak girişleri dinamik olarak işlemenizi sağlar.
 
   ```javascript
   module.exports = async function(context) { 
       context.log("This is myTrigger: " + arguments[1]);
       context.log("This is myInput: " + arguments[2]);
       context.log("This is myOtherInput: " + arguments[3]);
   };
   ```

### <a name="outputs"></a>Çıkışlar
Çıkış (bağlamalarını `direction === "out"`) çeşitli yollarla bir işlevde tarafından yazılabilir. Tüm durumlarda `name` tanımlandığı gibi bağlama özelliğini *function.json* işlevinizde yazılan nesne üyesinin adı karşılık gelir. 

Veri (Bu yöntemleri birleştirmek yok) aşağıdaki yollardan biriyle bir çıkış bağlamaları atayabilirsiniz:

- **_[Birden çok çıkış için önerilen]_  Döndüren bir nesne.** İşlev döndüren bir zaman uyumsuz/Promise kullanıyorsanız, bir nesne ile atanan çıktı verilerini döndürebilir. Aşağıdaki örnekte çıkış bağlamaları "httpResponse" ve "queueOutput" olarak adlandırılan *function.json*.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      return {
          httpResponse: {
              body: retMsg
          },
          queueOutput: retMsg
      };
  };
  ```

  Zaman uyumlu bir işlevin kullanıyorsanız, bu nesneyi kullanarak döndürebilir [ `context.done` ](#contextdone-method) (örneğe bakın).
- **_[Tek çıkış için önerilen]_  $Return bağlama adını kullanarak ve doğrudan değer döndürüyor.** Bu, yalnızca zaman uyumsuz/döndüren işlevleri Promise çalışır. Örnekte bakın [bir zaman uyumsuz işlev dışarı aktarma](#exporting-an-async-function). 
- **Değerler atamada `context.bindings`**  doğrudan context.bindings için değerler atayabilirsiniz.

  ```javascript
  module.exports = async function(context) {
      let retMsg = 'Hello, world!';
      context.bindings.httpResponse = {
          body: retMsg
      };
      context.bindings.queueOutput = retMsg;
      return;
  };
  ```

### <a name="bindings-data-type"></a>Bağlamaları veri türü

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

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı kullanan bir `context` nesne için ve işlevinizden veri iletmek için ve çalışma zamanı ile iletişim kurmasına izin vermek için. Bağlam nesnesini okumak ve verileri bağlamaları ayarlanırken, günlükleri yazmak ve kullanarak kullanılabilir `context.done` , dışarı aktarılan işlevin zaman uyumlu olduğunda geri çağırma.

`context` Nesnesi, her zaman ilk parametre olarak bir işlev. Önemli yöntemler gibi olduğundan dahil edilecek `context.done` ve `context.log`. İnovasyonunuz ne olursa olsun istediğiniz nesnenin adı verebilirsiniz (örneğin, `ctx` veya `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(ctx) {
    // function logic goes here :)
    ctx.done();
};
```

### <a name="contextbindings-property"></a>context.bindings property

```js
context.bindings
```

Okumak veya veri bağlama atamak için kullanılan adlandırılmış bir nesne döndürür. Giriş ve veri bağlama tetikleyici üzerinde özelliklerini okuyarak erişilebilir `context.bindings`. Çıkış veri bağlama verileri ekleyerek atanabilir. `context.bindings`

Örneğin, aşağıdaki bağlama tanımlar, function.json, bir kuyruktan içeriğine erişmek izin `context.bindings.myInput` ve kullanarak bir kuyruk çıkış atama `context.bindings.myOutput`.

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

Çıkış veri bağlama kullanarak tanımlamak seçebileceğiniz `context.done` yöntemi yerine `context.binding` nesne (aşağıya bakın).

### <a name="contextbindingdata-property"></a>context.bindingData property

```js
context.bindingData
```

Tetikleyici meta verileri ve işlev çağırma verilerini içeren adlandırılmış bir nesne döndürür (`invocationId`, `sys.methodName`, `sys.utcNow`, `sys.randGuid`). Tetikleyici meta veri örneği için bkz. Bu [event hubs örneği](functions-bindings-event-hubs.md#trigger---javascript-example).

### <a name="contextdone-method"></a>Context.Done yöntemi

```js
context.done([err],[propertyBag])
```

Kodunuzu tamamlandığını bilmeniz çalışma zamanının olanak tanır. İşlevinizi kullandığında [ `async function` ](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Statements/async_function) bildirimi gerektirmeyen kullanılacak `context.done()`. `context.done` Geri çağırma örtük olarak çağrılır. Düğüm 8 veya sürümü gerektiren sonraki bir sürümünü kullanılabilen zaman uyumsuz işlevleri işlevler çalışma zamanının 2.x.

İşlevinizi bir zaman uyumsuz işlev değilse **çağırmalısınız** `context.done` çalışma zamanının işlevinizi tamamlandığını bildirmek için. Yürütme zaman aşımına if eksik.

`context.done` Yöntemi geri hem bir kullanıcı tanımlı hata çalışma zamanı ve çıktı bağlaması verilerini içeren bir JSON nesnesi geçirme olanak tanır. Geçirilen özellikleri `context.done` ayarlanan herhangi bir şey üzerine `context.bindings` nesne.

```javascript
// Even though we set myOutput to have:
//  -> text: 'hello world', number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method overwrites the myOutput binding to be: 
//  -> text: 'hello there, world', noNumber: true
```

### <a name="contextlog-method"></a>Context.log yöntemi  

```js
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

## <a name="writing-trace-output-to-the-console"></a>İzleme çıktısı konsola yazma 

İşlevleri'nde, kullandığınız `context.log` konsola izleme çıkışını yazmak için yöntemleri. İşlevler'ın v2.x içinde İzleme çıkışı kullanarak `console.log` işlevi uygulama düzeyinde yakalanır. Gelen veren anlamına gelir `console.log` bir belirli bir işlev çağrısı için bağlı değil ve belirli bir işlevin günlüklerini içinde görüntülenmez. Ancak, Application Insights'a yayılması. İşlevleri v1.x içinde kullanamazsınız `console.log` konsola yazma için.

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

Eşik izleme düzeyi için kolaylaştıran konsola yazma için tanımladığınız işlevleri 1.x sağlar izlemeleri işlevinizden konsoluna yazılan denetlenmesine. Konsoluna yazılan tüm izlemeleri eşiği ayarlamak için kullanın `tracing.consoleLevel` host.json dosyasındaki özellik. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir. Aşağıdaki örnek, ayrıntılı günlük kaydını etkinleştirmek için izleme eşiği ayarlar:

```json
{
    "tracing": {
        "consoleLevel": "verbose"
    }
}  
```

Değerleri **consoleLevel** adları için karşılık gelen `context.log` yöntemleri. Konsola tüm izleme günlüğü devre dışı bırakmak için ayarlanmış **consoleLevel** için _kapalı_. Daha fazla bilgi için [host.json başvurusu](functions-host-json-v1.md).

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

+ **Gelen `req` ve `res` özellikleri `context` nesne.** Bu şekilde, geleneksel düzeni HTTP verilere tam kullanmak zorunda olmak yerine bağlamı nesnesinden kullanabileceğiniz `context.bindings.name` deseni. Aşağıdaki örnek nasıl erişeceğinizi gösterir `req` ve `res` üzerindeki nesneleri `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ **Adlandırılmış giriş ve çıkış bağlamaları.** Bu şekilde, HTTP tetikleyicisini ve bağlamalarını aynı diğer herhangi bir bağlama olarak çalışır. Aşağıdaki örnek, bir adlandırılmış kullanarak yanıt nesnesini ayarlar `response` bağlama: 

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
+ **_[Yalnızca yanıtı]_  Çağırarak `context.res.send(body?: any)`.** Bir HTTP yanıtı girişi ile oluşturulan `body` yanıt gövdesi olarak. `context.done()` örtük olarak çağrılır.

+ **_[Yalnızca yanıtı]_  Çağırarak `context.done()`.** Özel bir HTTP bağlaması için geçirilen yanıtı döndürür `context.done()` yöntemi. Aşağıdaki HTTP çıktı bağlamasını tanımlar bir `$return` çıkış parametresi:

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
| 2.x  | _Etkin LTS_ ve tek sayılı _geçerli_ Node.js sürümleri (8.11.1 ve önerilen 10.14.1). Sürüm WEBSITE_NODE_DEFAULT_VERSION ayarlamak [uygulama ayarı](functions-how-to-use-azure-function-app-settings.md#settings).|

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

İşlevlerde, [uygulama ayarları](functions-app-settings.md), gibi hizmet bağlantısı dizeleri sunulur ortam değişkenleri olarak yürütme sırasında. Bu ayarları kullanarak erişebileceğiniz `process.env`, burada gösterildiği gibi `GetEnvironmentVariable` işlevi:

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

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Uygulama ayarlarını okuma yerel olarak çalıştırılırken [local.settings.json](functions-run-local.md#local-settings-file) proje dosyası.

## <a name="configure-function-entry-point"></a>İşlev giriş noktası yapılandırma

`function.json` Özellikleri `scriptFile` ve `entryPoint` , dışarı aktarılan işlevin adını ve konumunu yapılandırmak için kullanılabilir. Javascript'inizi transpiled olduğunda bu özellikleri önemli olabilir.

### <a name="using-scriptfile"></a>kullanma `scriptFile`

Bir JavaScript işlevi yürütüldüğü varsayılan olarak, `index.js`, kendi ilişkili olarak aynı üst dizine paylaşan bir dosya `function.json`.

`scriptFile` Aşağıdaki örnekteki gibi bir klasör yapısı almak için kullanılabilir:

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

Bu kullanılarak yapılandırılabilir `entryPoint` içinde `function.json`, aşağıdaki örnekte olduğu gibi:

```json
{
  "entryPoint": "logFoo",
  "bindings": [
    ...
  ]
}
```

İşlevler'ın v2.x içinde destekleyen `this` kullanıcı işlevindeki bir parametre, işlev kodunu ardından olabilir aşağıdaki örnekte olduğu gibi:

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

Bu örnekte, bir nesne dışarı olsa da, bulunduğuna yürütmeleri arasında durum koruma için hiçbir garanti dikkat edin önemlidir.

## <a name="local-debugging"></a>Yerel hata ayıklama

İle başlatıldığında `--inspect` parametresi, bir Node.js işlemi belirtilen bağlantı noktasında hata ayıklama bir istemci için dinler. Azure işlevleri'nde 2.x, ortam değişkenine ya da uygulama ayarı ekleyerek, kodunuz çalışır Node.js işlemine geçirilecek bağımsız değişkenler belirtebilirsiniz `languageWorkers:node:arguments = <args>`. 

Yerel olarak hata ayıklamak için ekleme `"languageWorkers:node:arguments": "--inspect=5858"` altında `Values` içinde [local.settings.json](https://docs.microsoft.com/azure/azure-functions/functions-run-local#local-settings-file) dosyası ve bir hata ayıklayıcı bağlantı noktası 5858 ekleyin.

VS Code kullanarak hata ayıklama yapılırken `--inspect` parametresi, kullanarak otomatik olarak eklenir `port` projenin launch.json dosyasındaki değeri.

Sürüm ayarı 1.x, `languageWorkers:node:arguments` çalışmaz. Hata ayıklama bağlantı ile seçilebilir [ `--nodeDebugPort` ](https://docs.microsoft.com/azure/azure-functions/functions-run-local#start) Azure işlevleri çekirdek araçları parametresi.

## <a name="typescript"></a>TypeScript

Sürümünü hedeflediğinizde işlevler çalışma zamanının 2.x hem [Visual Studio Code için Azure işlevleri](functions-create-first-function-vs-code.md) ve [Azure işlevleri çekirdek Araçları](functions-run-local.md) destekleyen bir şablon kullanarak işlev uygulamaları oluşturmanızı sağlar TypeScript işlev uygulaması projeleri. Şablon oluşturur `package.json` ve `tsconfig.json` derleyin, daha kolay hale getirmek proje dosyaları çalıştırın ve JavaScript işlevleri bu araçlarla TypeScript koddan yayımlayın.

Oluşturulan bir `.funcignore` dosyası, hangi dosyaların hariç tutulur, Azure'a bir proje yayımlandığında belirtmek için kullanılır.  

TypeScript dosyalar (.ts), JavaScript dosyaları (.js) içine transpiled `dist` çıkış dizini. TypeScript şablonlarını kullanın [ `scriptFile` parametre](#using-scriptfile) içinde `function.json` karşılık gelen .js dosyasının konumunu belirtmek için `dist` klasör. Çıkış konumu kullanarak şablon tarafından ayarlanmış `outDir` parametresinde `tsconfig.json` dosya. Bu ayar ya da klasörün adını değiştirirseniz, çalışma zamanı çalıştırılacak kodu bulmak mümkün değil.

> [!NOTE]
> TypeScript için Deneysel desteği mevcut sürüm işlevler çalışma zamanının 1.x. Deneysel sürüm transpiles işlevi çağrıldığında JavaScript dosyalarına TypeScript dosyaları. Sürüm 2.x, bu Deneysel destek yerini transpilation ana bilgisayar başlatılmadan önce yapan aracı temelli yöntemi tarafından ve dağıtım sürecinde.

Yerel olarak geliştirme ve bir TypeScript projesi dağıtma biçimini, geliştirme aracına bağlıdır.

### <a name="visual-studio-code"></a>Visual Studio Code

[Visual Studio Code için Azure işlevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) uzantısı TypeScript kullanarak işlevlerinizi geliştirmenize olanak tanır. Temel araçları, Azure işlevleri uzantının bir gereksinimdir.

Visual Studio Code'da bir TypeScript işlev uygulaması oluşturmak için yalnızca seçtiğiniz `TypeScript` ne zaman bir işlev uygulaması oluşturma ve dili seçmeniz istenir.

Bastığınızda **F5** (func.exe) ana bilgisayar başlatılmadan önce uygulamayı çalıştırmak için yerel olarak transpilation gerçekleştirilir. 

Azure kullanarak işlev uygulamanızı dağıtırken **işlev uygulaması Dağıt...**  düğmesi, Azure işlevleri uzantısı ilk JavaScript dosyalarının bir üretime hazır derleme TypeScript kaynak dosyası oluşturur.

### <a name="azure-functions-core-tools"></a>Azure işlevleri temel araçları

Core Araçları'nı kullanarak bir TypeScript işlevi uygulaması projesi oluşturmak için işlev uygulamanızı oluştururken typescript dil seçeneğini belirtmeniz gerekir. Bu aşağıdaki yollardan biriyle yapabilirsiniz:

- Çalıştırma `func init` komutu, select `node` dil yığını, ve ardından `typescript`.

- `func init --worker-runtime typescript` komutunu çalıştırın.

Temel araçları kullanarak yerel olarak işlev uygulaması kodunuzu çalıştırmak için kullandığınız `npm start` komutu yerine `func host start`. `npm start` Komutu için aşağıdaki komutlar eşdeğerdir:

- `npm run build`
- `func extensions install`
- `tsc`
- `func start`

Kullanmadan önce [ `func azure functionapp publish` ] Azure'a dağıtmak için komut, ilk çalıştırmalısınız `npm run build:production` komutu. Bu komut, bir üretim ortamına hazır derleme JavaScript dosyalarının kullanılarak dağıtılan TypeScript kaynak dosyası oluşturur. [ `func azure functionapp publish` ].

## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleri için dikkat edilmesi gerekenler

JavaScript işlevleri ile çalışırken, aşağıdaki bölümlerde konuları unutmayın.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek vCPU App Service planı seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, bir plan ile birden çok Vcpu yerine tek vCPU planı seçmeniz önerilir. Bugün, işlevleri çalışır JavaScript işlevleri daha verimli bir şekilde tek vCPU VM'ler üzerinde ve daha büyük sanal makineleri kullanarak beklenen performans iyileştirmeleri üretmez. Gerekli olduğunda, el ile daha fazla tek vCPU VM örneği ekleyerek genişletebilir veya otomatik ölçeklendirme etkinleştirebilirsiniz. Daha fazla bilgi için [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service%2ftoc.json).

### <a name="cold-start"></a>Hazırlıksız başlatma

Geliştirme Azure işlevleri'nde sunucusuz barındırma modeli, soğuk başladığında gerçeğe var. *Hazırlıksız başlatma* olgusu başvuruyor işlev uygulamanızı belirli bir süre sonra ilk kez başlatıldığında başlatılması uzun sürdüğünü. Büyük bağımlılık ağaçları ile JavaScript işlevleri için özellikle hazırlıksız başlatma önemli olabilir. Hazırlıksız başlatma sürecini hızlandırmak için [bir paket dosyası işlevlerinizin çalıştığı](run-functions-from-deployment-package.md) mümkün olduğunda. Paket modeli çalıştırmadan varsayılan olarak birçok dağıtım yöntemlerini kullanmanız, ancak bu değişiklik, büyük kaldırmanıza yaşıyorsanız ve bu şekilde çalışan, önemli bir iyileştirme sunabilir.

### <a name="connection-limits"></a>Bağlantı sınırları

Azure işlevleri uygulamada bir hizmete özgü istemcisini kullandığınızda, yeni bir istemci her işlev Çağırma ile oluşturmayın. Bunun yerine, tek bir statik istemci genel kapsamda oluşturun. Daha fazla bilgi için [Azure işlevleri'nde bağlantıları yönetme](manage-connections.md).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

+ [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
+ [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
+ [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

['func azure functionapp publish']: functions-run-local.md#project-file-deployment
