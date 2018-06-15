---
title: Azure işlevleri için JavaScript Geliştirici Başvurusu | Microsoft Docs
description: JavaScript kullanarak işlevleri geliştirmek nasıl anlayın.
services: functions
documentationcenter: na
author: tdykstra
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
ms.author: tdykstra
ms.openlocfilehash: 78f29cd4a20861e40bb7f7f398979b8d93387a7b
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936635"
---
# <a name="azure-functions-javascript-developer-guide"></a>Azure işlevleri JavaScript Geliştirici Kılavuzu

Azure işlevleri için JavaScript deneyimi olarak geçirilen bir işlev dışarı aktarmak kolaylaştırır bir `context` alırken ve verileri bağlamaları aracılığıyla gönderme ve çalışma zamanı ile iletişim kurmak için nesne.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

## <a name="exporting-a-function"></a>Bir işlev dışarı aktarma
Tüm JavaScript işlevleri tek bir dışarı aktarma `function` aracılığıyla `module.exports` işlevi bulmak ve çalıştırmak çalışma zamanı. Bu işlev, her zaman içermelidir bir `context` nesnesi.

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // Additional inputs can be accessed by the arguments property
    if(arguments.length === 4) {
        context.log('This function has 4 inputs');
    }
};
// or you can include additional inputs in your arguments
module.exports = function(context, myTrigger, myInput, myOtherInput) {
    // function logic goes here :)
};
```

Bağlamaları `direction === "in"` boyunca kullanabileceğiniz anlamına gelir işlevi bağımsız geçirilen [ `arguments` ](https://msdn.microsoft.com/library/87dw3w1k.aspx) yeni girişler dinamik olarak işlenecek (kullanarak örneğin, `arguments.length` tüm girişlerinizi yinelemek için). Yalnızca bir tetikleyici ve hiçbir ek girişleri varsa başvurulmadan beklendiği tetikleyici verilerinize erişebilir bu işlevi kullanışlı çünkü, `context` nesnesi.

Bağımsız değişkenler her zaman boyunca işlevi içinde gerçekleşme içinde sırayla geçirilir *function.json*dışarı deyiminizde belirtmeyin olsa bile. Örneğin, `function(context, a, b)` ve şekilde değiştirin `function(context, a)`, değeri almaya devam `b` başvurma tarafından işlevi kodda `arguments[2]`.

Yönü, bağımsız olarak tüm bağlamaları boyunca de geçirilir `context` (aşağıdaki betiği bakın) nesne. 

## <a name="context-object"></a>bağlam nesnesi
Çalışma zamanı modülünü kullanan bir `context` nesne işlevinizi veri geçirmek için ve çalışma zamanı ile iletişim kurmasına izin vermek için.

`context` Nesnesi her zaman ilk parametre bir işlevi olarak ve yöntemleri gibi içerdiğinden dahil edilmelidir `context.done` ve `context.log`, çalışma zamanı doğru bir şekilde kullanmak için gerekli. Ne olursa olsun istediğiniz nesne adı verebilirsiniz (örneğin, `ctx` veya `c`).

```javascript
// You must include a context, but other arguments are optional
module.exports = function(context) {
    // function logic goes here :)
};
```

### <a name="contextbindings-property"></a>Context.Bindings özelliği

```
context.bindings
```
Tüm girdi ve çıktı verilerini içeren adlandırılmış bir nesne döndürür. Örneğin, aşağıdaki bağlama tanımında, *function.json* sıradan içeriğini erişim sağlar `context.bindings.myInput` nesnesi. 

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

Kodunuzu bitirdi runtime bildirir. Çağırmalısınız `context.done`, veya başka çalışma zamanı hiçbir zaman işlevinizi tamamlandıktan ve yürütme zaman aşımı olur anlar. 

`context.done` Yöntemi geri hem bir kullanıcı tanımlı hata çalışma zamanı ve özellikler üzerine bir özellik paketi özelliklerinin geçirilecek verir `context.bindings` nesnesi.

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
Varsayılan izleme düzeyinde akış konsol günlükleri yazmanızı sağlar. Üzerinde `context.log`, ek yöntemleri günlüğü diğer izleme düzeyleri konsol günlüğüne yazmanıza olanak tanıyan kullanılabilir:


| Yöntem                 | Açıklama                                |
| ---------------------- | ------------------------------------------ |
| **hata (_ileti_)**   | Hata düzeyi oturum açma ya da daha düşük yazar.   |
| **warn (_ileti_)**    | Uyarı düzeyi oturum açma ya da daha düşük yazar. |
| **bilgi (_ileti_)**    | Oturum açma veya alt bilgi düzeyine yazar.    |
| **verbose (_ileti_)** | Ayrıntılı düzeyinde günlüğe kaydetme yazar.           |

Aşağıdaki örnek, uyarı izleme düzeyinde konsola yazar:

```javascript
context.log.warn("Something has happened."); 
```
Host.json dosyasında günlüğe kaydetme için izleme düzeyi eşiğini ayarlayın ya da kapatabilirsiniz.  Günlüklerine yazma hakkında daha fazla bilgi için sonraki bölüme bakın.

## <a name="binding-data-type"></a>Bağlama veri türü

Bir giriş bağlaması için veri türünü tanımlamak için `dataType` bağlama tanımında özelliği. Örneğin, bir HTTP isteği ikili biçimde içeriğini okumak için türü kullanın `binary`:

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

İşlevlerde, kullandığınız `context.log` İzleme çıktısı konsola yazma yöntemleri. Bu noktada, kullanamazsınız `console.log` konsola yazma.

Çağırdığınızda `context.log()`, iletinizi olan varsayılan izleme düzeyi konsoluna yazılır _bilgisi_ izleme düzeyi. Aşağıdaki kod bilgisi izleme düzeyinde konsola yazar:

```javascript
context.log({hello: 'world'});  
```

Önceki kod, aşağıdaki kodu eşdeğerdir:

```javascript
context.log.info({hello: 'world'});  
```

Aşağıdaki kod hata düzeyinde konsola yazar:

```javascript
context.log.error("An error has occurred.");  
```

Çünkü _hata_ yüksek izleme günlüğü etkin olduğu sürece düzeyi, bu izleme çıktısı tüm izleme düzeylerde yazılır.  


Tüm `context.log` yöntemlerini desteklemek Node.js tarafından desteklenen aynı parametre biçimi [util.format yöntemi](https://nodejs.org/api/util.html#util_util_format_format). Varsayılan izleme düzeyi kullanarak konsola aşağıdaki kodu göz önünde bulundurun:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=' + req.originalUrl);
context.log('Request Headers = ' + JSON.stringify(req.headers));
```

Ayrıca, aynı kod şu biçimde yazabilirsiniz:

```javascript
context.log('Node.js HTTP trigger function processed a request. RequestUri=%s', req.originalUrl);
context.log('Request Headers = ', JSON.stringify(req.headers));
```

### <a name="configure-the-trace-level-for-console-logging"></a>Konsol günlüğe kaydetme için izleme düzeyini yapılandırın

İşlevler şekilde izlemeleri, işlevlerden konsoluna yazılan denetim kolay hale getirir konsola yazma için eşik izleme düzeyi tanımlamanıza olanak sağlar. Konsola yazılan tüm izlemeleri eşiği ayarlamak için kullanın `tracing.consoleLevel` host.json dosyasında özellik. Bu ayar, tüm işlevlere işlevi uygulamanızda geçerlidir. Aşağıdaki örnek, ayrıntılı günlük kaydını etkinleştirmek için izleme eşik ayarlar:

```json
{ 
    "tracing": {      
        "consoleLevel": "verbose"     
    }
}  
```

Değerleri **consoleLevel** adları için karşılık gelen `context.log` yöntemleri. Konsola tüm izleme günlüğü devre dışı bırakmak için ayarlanmış **consoleLevel** için _devre dışı_. Daha fazla bilgi için bkz: [host.json başvuru](functions-host-json.md).

## <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

HTTP ve Web kancası Tetikleyicileri ve bağlamaları istek ve yanıt nesneleri HTTP ileti temsil etmek için kullanır. çıktı.  

### <a name="request-object"></a>İstek nesnesi

`request` Nesnesi aşağıdaki özelliklere sahiptir:

| Özellik      | Açıklama                                                    |
| ------------- | -------------------------------------------------------------- |
| _Gövde_        | İstek gövdesini içeren bir nesne.               |
| _Üstbilgileri_     | İstek üstbilgilerini içeren bir nesne.                   |
| _Yöntemi_      | İsteğin HTTP yöntemi.                                |
| _originalUrl_ | İstek URL'si.                                        |
| _Parametreleri_      | İsteğin Yönlendirme parametreleri içeren bir nesne. |
| _Sorgu_       | Sorgu parametreleri içeren bir nesne.                  |
| _rawBody_     | Dize olarak ileti gövdesi.                           |


### <a name="response-object"></a>Yanıt nesnesi

`response` Nesnesi aşağıdaki özelliklere sahiptir:

| Özellik  | Açıklama                                               |
| --------- | --------------------------------------------------------- |
| _Gövde_    | Yanıtın gövdesini içeren bir nesne.         |
| _Üstbilgileri_ | Yanıt üst bilgileri içeren bir nesne.             |
| _isRaw_   | Biçimlendirme için yanıt atlanır gösterir.    |
| _Durumu_  | Yanıtının HTTP durum kodu.                     |

### <a name="accessing-the-request-and-response"></a>İstek ve yanıt erişme 

HTTP tetikleyicileri ile çalışırken, HTTP istek ve yanıt nesneleri herhangi üç yolla erişebilirsiniz:

+ Adlandırılmış giriş ve çıkış bağlamaları. Bu şekilde, HTTP tetikleyicisini ve bağlamaları başka bir bağlama ile aynı çalışır. Aşağıdaki örnek, bir adlandırılmış kullanarak yanıt nesnesini ayarlar `response` bağlama: 

    ```javascript
    context.bindings.response = { status: 201, body: "Insert succeeded." };
    ```

+ Gelen `req` ve `res` özellikleri `context` nesnesi. Bu şekilde, geleneksel düzeni verilere erişmek HTTP için tam kullanmak zorunda olmak yerine bağlam nesnesinden kullanabileceğiniz `context.bindings.name` düzeni. Aşağıdaki örnekte nasıl erişeceğinizi gösterir `req` ve `res` üzerinde nesneleri `context`:

    ```javascript
    // You can access your http request off the context ...
    if(context.req.body.emoji === ':pizza:') context.log('Yay!');
    // and also set your http response
    context.res = { status: 202, body: 'You successfully ordered more coffee!' }; 
    ```

+ Çağırarak `context.done()`. Özel türde bir HTTP bağlaması için geçirilen yanıtı döndürür `context.done()` yöntemi. Aşağıdaki HTTP bağlama çıktı tanımlayan bir `$return` çıkış parametresi:

    ```json
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    }
    ``` 
    Bu çıktı bağlama, çağırdığınızda yanıt vermesini bekliyor `done()`aşağıdaki gibi:

    ```javascript
     // Define a valid response object.
    res = { status: 201, body: "Insert succeeded." };
    context.done(null, res);   
    ```  

## <a name="node-version-and-package-management"></a>Düğüm sürümü ve paket Yönetimi

Aşağıdaki tabloda her ana işlevleri çalışma zamanı sürümü tarafından kullanılan Node.js sürümünü gösterir:

| İşlevler sürümü | Node.js sürümü | 
|---|---|
| 1.x | 6.11.2 (çalışma zamanı tarafından kilitlenmiş) |
| 2.x  |> = 8.4.0 geçerli LTS önerilen 8.9.4 ile. Sürüm WEBSITE_NODE_DEFAULT_VERSION kullanarak ayarlamak [uygulama ayarı](functions-how-to-use-azure-function-app-settings.md#settings).|

Yazdırma tarafından kullanma çalışma zamanı geçerli sürümü görebilirsiniz `process.version` herhangi işlevi.

Aşağıdaki adımları işlevi uygulamanızda paketleri dahil sağlar: 

1. `https://<function_app_name>.scm.azurewebsites.net` kısmına gidin.

2. Tıklatın **Debug konsol** > **CMD**.

3. Git `D:\home\site\wwwroot`, package.json dosyanızı sürükleyin **wwwroot** sayfanın üst yarısındaki klasörü.  
    Dosya başka yollarla işlevi uygulamanıza da karşıya yükleyebilirsiniz. Daha fazla bilgi için bkz: [işlevi uygulama dosyaları güncelleştirmek nasıl](functions-reference.md#fileupdate). 

4. Package.json dosyası karşıya yüklendikten sonra çalıştırmak `npm install` komutunu **Kudu uzaktan yürütme konsol**.  
    Bu eylem package.json dosyasında belirtilen paket indirir ve işlev uygulaması yeniden başlatır.

Gereksinim duyduğunuz paketleri yüklendikten sonra işleve çağırarak aldıktan `require('packagename')`, aşağıdaki örnekte olduğu gibi:

```javascript
// Import the underscore.js library
var _ = require('underscore');
var version = process.version; // version === 'v6.5.0'

module.exports = function(context) {
    // Using our imported underscore.js library
    var matched_names = _
        .where(context.bindings.myInput.names, {first: 'Carla'});
```

Tanımlamanız gerekir bir `package.json` işlevi uygulamanızın kök dizinindeki dosyasını. Dosya tanımlama aynı önbelleğe alınmış paketleri, en iyi performans sağlayan paylaşmak uygulamasında tüm işlevleri sağlar. Sürüm çakışması ortaya çıkarsa, ekleyerek çözebilmek bir `package.json` belirli bir işlev klasöründe bulunan dosyadır.  

## <a name="environment-variables"></a>Ortam değişkenleri
Bir ortam değişkeni veya ayar değeri bir uygulamayı almak için `process.env`, aşağıda gösterildiği gibi `GetEnvironmentVariable` işlevi:

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
## <a name="considerations-for-javascript-functions"></a>JavaScript işlevleri için ilgili önemli noktalar

JavaScript işlevleri ile çalışırken, aşağıdaki iki bölümlerdeki konuları unutmayın.

### <a name="choose-single-vcpu-app-service-plans"></a>Uygulama hizmeti planları tek vCPU seçin

Uygulama hizmeti planı kullanan bir işlev uygulaması oluşturduğunuzda, bir plan ile birden çok Vcpu'lar yerine bir tek vCPU planı seçmenizi öneririz. Bugün, işlevleri çalışır JavaScript işlevleri daha verimli bir şekilde tek vCPU Vm'lerinde ve büyük sanal makineleri kullanarak beklenen performans iyileştirmeleri üretmez. Gerekli olduğunda, el ile daha fazla tek vCPU VM örnekleri ekleyerek ölçeğini veya Otomatik ölçek etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [örnek sayısı el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md?toc=%2fazure%2fapp-service-web%2ftoc.json).    

### <a name="typescript-and-coffeescript-support"></a>TypeScript ve CoffeeScript desteği
Doğrudan destek henüz otomatik derleme TypeScript veya CoffeeScript için çalışma zamanı mevcut olmadığından, bu tür destek dışında çalışma zamanı, dağıtım sırasında yapılması gerekir. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

