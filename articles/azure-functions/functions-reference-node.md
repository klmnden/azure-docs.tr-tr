---
title: Azure işlevleri için JavaScript Geliştirici Başvurusu | Microsoft Docs
description: JavaScript kullanarak işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.assetid: 45dedd78-3ff9-411f-bb4b-16d29a11384c
ms.service: functions
ms.devlang: nodejs
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/04/2018
ms.author: glenga
ms.openlocfilehash: 6099a818651cf75a75159f43748720b3eb01e4de
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43287830"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure işlevleri JavaScript Geliştirici Kılavuzu

Azure işlevleri için JavaScript deneyimi olarak geçirilen bir işlevi dışa aktarmak kolaylaştırır bir `context` bağlamaları aracılığıyla veri gönderme ve alma ve çalışma zamanı ile iletişim kurmak için bir nesne.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

## <a name="exporting-a-function"></a>Bir işlevi dışa aktarma
Her bir JavaScript işlevi, tek bir vermelisiniz `function` aracılığıyla `module.exports` işlevi bulmak ve çalıştırmak çalışma zamanı. Bu işlev her zaman almalıdır bir `context` ilk parametre olarak nesne.

```javascript
// You must include a context, other arguments are optional
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
    context.done();
};
// You can also use 'arguments' to dynamically handle inputs
module.exports = function(context) {
    context.log('Number of inputs: ' + arguments.length);
    // Iterates through trigger and input binding data
    for (i = 1; i < arguments.length; i++){
        context.log(arguments[i]);
    }
    context.done();
};
```

Giriş ve tetikleyici bağlamaları (bağlamalarını `direction === "in"`) işlevi için parametre olarak geçirilebilir. İçinde tanımlanan aynı sırada işlevine geçirilen *function.json*. JavaScript kullanarak girişleri dinamik olarak işleyebilir [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) nesne. Örneğin, `function(context, a, b)` ve değiştirmek için `function(context, a)`, değeri almaya devam `b` başvuran tarafından işlevi kodda `arguments[2]`.

Yön bağımsız olarak tüm bağlamaları boyunca da geçirilir `context` kullanarak nesne `context.bindings` özelliği.

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı kullanan bir `context` nesne için ve işlevinizden veri iletmek için ve çalışma zamanı ile iletişim kurmasına izin vermek için.

`context` Nesne her zaman ilk parametre olarak bir işlev ve yöntemler gibi olduğundan dahil edilmesi gereken `context.done` ve `context.log`, bunlar doğru çalışma zamanı kullanmak için gereklidir. İnovasyonunuz ne olursa olsun istediğiniz nesnenin adı verebilirsiniz (örneğin, `ctx` veya `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
    context.done();
};
```

### <a name="contextbindings-property"></a>Context.Bindings özelliği

```
context.bindings
```
Tüm girdi ve çıktı verilerini içeren adlandırılmış bir nesne döndürür. Örneğin, aşağıdaki bağlama tanımında, *function.json* kuyruktan içeriğini erişmenizi sağlar `context.bindings.myInput` nesne. 

```json
{
    "type":"queue",
    "direction":"in",
    "name":"myInput"
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

### <a name="contextdone-method"></a>Context.Done yöntemi
```
context.done([err],[propertyBag])
```

Kodunuzu bitirdi çalışma zamanı bildirir. İşlevinizi kullanıyorsa `async function` bildirimi (kullanılabilir işlevler sürüm 8 + düğümü kullanan 2.x), kullanın gerekmez `context.done()`. `context.done` Geri çağırma örtük olarak çağrılır.

İşlevinizi bir zaman uyumsuz işlev değilse **çağırmalısınız `context.done`**  çalışma zamanının işlevinizi tamamlandığını bildirmek için. Yürütme zaman aşımı eksik olması durumunda olur.

`context.done` Yöntemi sayesinde çalışma zamanı ve özellikler üzerine bir özellik paketi özellikleri hem de bir kullanıcı tanımlı hata geri geçirmek `context.bindings` nesne.

```javascript
// Even though we set myOutput to have:
//  -> text: hello world, number: 123
context.bindings.myOutput = { text: 'hello world', number: 123 };
// If we pass an object to the done function...
context.done(null, { myOutput: { text: 'hello there, world', noNumber: true }});
// the done method will overwrite the myOutput binding to be: 
//  -> text: hello there, world, noNumber: true
```

### <a name="contextlog-method"></a>Context.log yöntemi  

```
context.log(message)
```
Varsayılan izleme düzeyinde akış yönlendirilen konsol günlüklerini yazmanızı sağlar. Üzerinde `context.log`ek yöntemler günlüğe kaydetme, diğer izleme düzeylerinde konsol günlüğüne yazmanıza olanak tanıyan kullanılabilir:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **hata (_ileti_)**   | Oturum açma veya daha düşük hata düzeyini yazar.   |
| **uyar (_ileti_)**    | Oturum açma veya daha düşük uyarı düzeyi için yazar. |
| **Info (_ileti_)**    | Oturum açma veya alt bilgi düzeyine yazar.    |
| **ayrıntılı (_ileti_)** | Ayrıntılı düzeyinde günlüğe kaydetme için yazar.           |

Aşağıdaki örnek, uyarı izleme düzeyini konsola yazar:

```javascript
context.log.warn("Something has happened."); 
```
Host.json dosyasında günlüğe kaydetme için izleme düzeyi eşiği ayarlamanıza veya kapatın.  Günlüklerin yazılacağı hakkında daha fazla bilgi için sonraki bölüme bakın.

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

Diğer seçenekler için `dataType` olan `stream` ve `string`.

## <a name="writing-trace-output-to-the-console"></a>İzleme çıktısı konsola yazma 

İşlevleri'nde, kullandığınız `context.log` konsola izleme çıkışını yazmak için yöntemleri. Bu noktada, kullanamazsınız `console.log` konsola yazma için.

Çağırdığınızda `context.log()`, iletinizin olduğundan varsayılan izleme düzeyini konsolda yazılan _bilgisi_ izleme düzeyi. Aşağıdaki kod, bilgi izleme düzeyini konsola yazar:

```javascript
context.log({hello: 'world'});  
```

Yukarıdaki kod, aşağıdaki koda eşdeğerdir:

```javascript
context.log.info({hello: 'world'});  
```

Aşağıdaki kod, hata düzeyinde konsola yazar:

```javascript
context.log.error("An error has occurred.");  
```

Çünkü _hata_ en yüksek izleme günlük kaydının etkin olduğu sürece düzeyi, bu izleme, tüm izleme çıkış yazılır.  


Tüm `context.log` yöntemleri destekler Node.js tarafından desteklenen aynı parametre biçimi [util.format yöntemi](https://nodejs.org/api/util.html#util_util_format_format). Varsayılan izleme düzeyini kullanarak konsola aşağıdaki kodu göz önünde bulundurun:

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

`request` Nesne, aşağıdaki özelliklere sahiptir:

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

`response` Nesne, aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _Gövde_    | Yanıtın gövdesini içeren bir nesne.         |
| _Üst bilgileri_ | Yanıt üst bilgilerini içeren bir nesne.             |
| _isRaw_   | Biçimlendirme yanıt atlanır gösterir.    |
| _Durumu_  | Yanıtın HTTP durum kodu.                     |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıt erişme 

HTTP tetikleyicileri ile çalışırken, HTTP istek ve yanıt nesneleri üç yoldan herhangi birini erişebilirsiniz:

+ Adlandırılmış giriş ve çıkış bağlamaları. Bu şekilde, HTTP tetikleyicisini ve bağlamalarını aynı diğer herhangi bir bağlama olarak çalışır. Aşağıdaki örnek, bir adlandırılmış kullanarak yanıt nesnesini ayarlar `response` bağlama: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Gelen `req` ve `res` özellikleri `context` nesne. Bu şekilde, geleneksel düzeni HTTP verilere tam kullanmak zorunda olmak yerine bağlamı nesnesinden kullanabileceğiniz `context.bindings.name` deseni. Aşağıdaki örnek nasıl erişeceğinizi gösterir `req` ve `res` üzerindeki nesneleri `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Çağırarak `context.done()`. Özel bir HTTP bağlaması için geçirilen yanıtı döndürür `context.done()` yöntemi. Aşağıdaki HTTP çıktı bağlamasını tanımlar bir `$return` çıkış parametresi:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Bu çıkış bağlaması, çağırdığınızda yanıt vermesini bekliyor `done()`gibi:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Düğüm sürümü ve paket Yönetimi

Aşağıdaki tabloda her önemli işlevler çalışma zamanı sürümü tarafından kullanılan Node.js sürümü gösterilmektedir:

| İşlevler sürümü | Node.js sürümü | 
|---|---|
| 1.x | 6.11.2 (çalışma zamanı tarafından kilitlendi) |
| 2.x  | _Etkin LTS_ ve _geçerli_ Node.js sürümleri (8.11.1 ve önerilen 10.6.0). Sürüm WEBSITE_NODE_DEFAULT_VERSION ayarlamak [uygulama ayarı](functions-how-to-use-azure-function-app-settings.md#settings).|

Çalışma zamanı tarafından yazdırma kullanarak geçerli sürümü gördüğünüz `process.version` herhangi bir işlevden.

Aşağıdaki adımları paketleri işlev uygulamanıza eklemenize olanak tanır: 

1. `https://<function_app_name>.scm.azurewebsites.net` kısmına gidin.

2. Tıklayın **konsol hata ayıklama** > **CMD**.

3. Git `D:\home\site\wwwroot`ve ardından, package.json dosyasına sürükleyin **wwwroot** sayfanın üst kısmında klasörü.  
    Dosya başka yollarla işlev uygulamanıza da karşıya yükleyebilirsiniz. Daha fazla bilgi için [işlevi uygulama dosyalarını nasıl güncelleştireceğinizi](functions-reference.md#fileupdate). 

4. Package.json dosyası karşıya yüklendikten sonra Çalıştır `npm install` komutunu **Kudu uzaktan yürütme konsol**.  
    Bu eylem, package.json dosyası içinde belirtilen paketleri indirir ve işlev uygulamasını yeniden başlatır.

Gereksinim paketlerin yüklendikten sonra bunları işlevinize çağrı yaparak aldığınız `require('packagename')`, aşağıdaki örnekte olduğu gibi:

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Tanımladığınız bir `package.json` işlev uygulamanızın kök dosya. Dosya tanımlama uygulamasında tüm işlevleri aynı önbelleğe eklenen paketleri en iyi performansı veren paylaşmak olanak tanır. Sürüm çakışması ortaya çıkarsa, ekleyerek giderebileceğiniz bir `package.json` belirli bir işlev bir klasörde dosya.  

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
## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleri için dikkat edilmesi gerekenler

JavaScript işlevleri ile çalışırken, aşağıdaki iki bölümü, konuları unutmayın.

### <a name="choose-single-vcpu-app-service-plans"></a>Tek vCPU App Service planı seçin

App Service planını kullanan bir işlev uygulaması oluşturduğunuzda, bir plan ile birden çok Vcpu yerine tek vCPU planı seçmeniz önerilir. Bugün, işlevleri çalışır JavaScript işlevleri daha verimli bir şekilde tek vCPU VM'ler üzerinde ve daha büyük sanal makineleri kullanarak beklenen performans iyileştirmeleri üretmez. Gerektiğinde, el ile daha fazla tek vCPU VM örneği ekleyerek genişletebilir ya da otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Daha fazla bilgi için [örnek sayısını elle veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>TypeScript ve CoffeeScript desteği
Doğrudan desteği henüz otomatik derleme TypeScript veya CoffeeScript için çalışma zamanı var olmadığından, dağıtım sırasında çalışma zamanı dışında işlenecek tür desteğini gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

