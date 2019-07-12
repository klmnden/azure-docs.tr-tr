---
title: PowerShell için Azure işlevleri Geliştirici Başvurusu
description: PowerShell kullanarak işlevleri geliştirme hakkında bilgi edinin.
services: functions
documentationcenter: na
author: tylerleonhardt
manager: jeconnoc
ms.service: azure-functions
ms.devlang: powershell
ms.topic: conceptual
ms.date: 04/22/2019
ms.author: tyleonha
ms.reviewer: glenga
ms.openlocfilehash: a75bdaf0e26193a5b2792b52923c085eff89b83f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706409"
---
# <a name="azure-functions-powershell-developer-guide"></a>Azure işlevleri PowerShell Geliştirici Kılavuzu

Bu makalede, PowerShell kullanarak Azure işlevleri nasıl yazdığınız ilişkin ayrıntılar sağlanır.

[!INCLUDE [functions-powershell-preview-note](../../includes/functions-powershell-preview-note.md)]

PowerShell Azure işlevi (işlev) tetiklendiğinde yürüten bir PowerShell Betiği temsil edilir. Her işlev betik ilgili olan `function.json` nasıl işlevi, nasıl tetiklendikten gibi davranacağını tanımlayan dosya ve giriş ve çıkış parametreleri. Daha fazla bilgi için bkz. [tetikleyiciler ve bağlama makale](functions-triggers-bindings.md). 

İçinde tanımlanan tüm giriş bağlamaları adları aynı parametreleri PowerShell betik işlevleri gibi diğer tür işlevlerin ele `function.json` dosya. A `TriggerMetadata` parametresi de geçirilen işlev çalışmaya tetikleyicisi hakkında daha fazla bilgi içeren.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md). Siz de tamamlamış olmanız gerekir [işlevleri hızlı PowerShell](functions-create-first-function-powershell.md) ilk PowerShell işlevinizi oluşturmak için.

## <a name="folder-structure"></a>klasör yapısı

Bir PowerShell proje için gereken klasör yapısı aşağıdaki gibi görünür. Bu varsayılan değiştirilebilir. Daha fazla bilgi için [KomutDosyası](#configure-function-scriptfile) bölümüne bakın.

```
PSFunctionApp
 | - MyFirstFunction
 | | - run.ps1
 | | - function.json
 | - MySecondFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - myFirstHelperModule
 | | | - myFirstHelperModule.psd1
 | | | - myFirstHelperModule.psm1
 | | - mySecondHelperModule
 | | | - mySecondHelperModule.psd1
 | | | - mySecondHelperModule.psm1
 | - local.settings.json
 | - host.json
 | - requirements.psd1
 | - profile.ps1
 | - extensions.csproj
 | - bin
```

Proje kök dizininde yok paylaşılan [ `host.json` ](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev kendi bağlama yapılandırma dosyasını ve kod dosyası (.ps1) sahip bir klasör olan (`function.json`). Function.json dosyanın üst dizininde her zaman işlevinizin adını adıdır.

Bazı bağlamalar bulunması gerekir. bir `extensions.csproj` dosya. Uzantılar, gerekli bağlama [sürüm 2.x](functions-versions.md) işlevler çalışma zamanını, şurada tanımlanan `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](functions-bindings-register.md#extension-bundles). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

PowerShell işlevi uygulamalarda, isteğe bağlı olarak olabilir bir `profile.ps1` çalıştırılacak bir işlev uygulaması başlatıldığında çalışır (Aksi halde olarak biliyor bir  *[hazırlıksız başlatma](#cold-start)* . Daha fazla bilgi için [PowerShell profiliniz](#powershell-profile).

## <a name="defining-a-powershell-script-as-a-function"></a>Bir PowerShell Betiği bir işlev olarak tanımlama

Varsayılan olarak, İşlevler çalışma zamanı işlevinizde arar `run.ps1`burada `run.ps1` kendi ilişkili olarak aynı üst dizine paylaşır `function.json`.

Betiğinizi yürütülmesine sayıda bağımsız değişken geçirildi. Bu parametreleri işlemek için ekleme bir `param` en üstüne aşağıdaki örnekte olduğu gibi betik bloğu:

```powershell
# $TriggerMetadata is optional here. If you don't need it, you can safely remove it from the param block
param($MyFirstInputBinding, $MySecondInputBinding, $TriggerMetadata)
```

### <a name="triggermetadata-parameter"></a>TriggerMetadata parametresi

`TriggerMetadata` Parametresi tetikleyici hakkında ek bilgi sağlamak için kullanılır. Ek meta veri bağlama bağlama farklılık gösterir ancak tüm içerdikleri bir `sys` şu verileri içeren özellik:

```powershell
$TriggerMetadata.sys
```

| Özellik   | Description                                     | Type     |
|------------|-------------------------------------------------|----------|
| utcNow     | UTC biçiminde işlevi, tetiklendi.        | Datetime |
| methodName | Tetiklendi işlevin adı     | dize   |
| RandGuid   | Bu işlev yürütmesi için benzersiz bir GUID | dize   |

Her tetikleyici türü meta verileri farklı bir dizi vardır. Örneğin, `$TriggerMetadata` için `QueueTrigger` içeren `InsertionTime`, `Id`, `DequeueCount`, başka şeylerin yanında. Kuyruk tetikleyicinin meta veriler hakkında daha fazla bilgi için Git [Sırası Tetikleyicileri resmi belgelerine](functions-bindings-storage-queue.md#trigger---message-metadata). Belgeleri kontrol [Tetikleyicileri](functions-triggers-bindings.md) tetikleyici meta verileri içinde ne geldiğini görmek için birlikte çalışıyoruz.

## <a name="bindings"></a>Bağlamalar

PowerShell'de, [bağlamaları](functions-triggers-bindings.md) yapılandırılır ve işlevin function.json içinde tanımlanır. İşlevleri, çeşitli yollarla bağlamalarla etkileşim kurun.

### <a name="reading-trigger-and-input-data"></a>Okuma tetikleyici ve giriş verileri

Tetikleyici, giriş bağlamaları, işleve geçirilen parametre olarak okunur. Giriş bağlamaları sahip bir `direction` kümesine `in` function.json içinde. `name` Tanımlanan özellik `function.json` parametrenin adı yer `param` blok. PowerShell adlandırılmış parametreler için bağlama kullandığından, parametre sırası önemli değildir. Ancak, içinde tanımlanan bağlamalardan sırasını takip etmek bir en iyi yöntemdir `function.json`.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)
```

### <a name="writing-output-data"></a>Çıkış veri yazma

İşlevler, bir çıkış bağlaması olan bir `direction` kümesine `out` function.json içinde. Kullanarak bir çıkış bağlaması için yazabilirsiniz `Push-OutputBinding` cmdlet'ini işlevler çalışma zamanı için kullanılabilir. Tüm durumlarda `name` tanımlandığı gibi bağlama özelliğini `function.json` karşılık gelen `Name` parametresinin `Push-OutputBinding` cmdlet'i.

Aşağıdakileri nasıl çağrılacağını gösterir `Push-OutputBinding` işlevi betiğinizde:

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Push-OutputBinding -Name myQueue -Value $myValue
```

İşlem hattı aracılığıyla belirli bir bağlama için bir değer de geçirebilirsiniz.

```powershell
param($MyFirstInputBinding, $MySecondInputBinding)

Produce-MyOutputValue | Push-OutputBinding -Name myQueue
```

`Push-OutputBinding` belirtilen değere göre farklı davranır `-Name`:

* Belirtilen ad geçerli bir çıkış bağlama çözümlenemiyor, ardından bir hata oluşturulur.

* Çıkış bağlaması değerlerinin koleksiyonunu kabul ettiğinde çağırabilirsiniz `Push-OutputBinding` art arda birden çok değer gönderin.

* Çıkış bağlaması, yalnızca bir tek değer kabul eder, çağırma `Push-OutputBinding` ikinci kez bir hata oluşturur.

#### <a name="push-outputbinding-syntax"></a>`Push-OutputBinding` Söz dizimi

Arama için geçerli Parametreler şunlardır `Push-OutputBinding`:

| Ad | Type | Konum | Açıklama |
| ---- | ---- |  -------- | ----------- |
| **`-Name`** | Dize | 1\. | Çıkış bağlaması adını ayarlamak istersiniz. |
| **`-Value`** | Object | 2 | Çıkış bağlaması değerini ayarlamak ByValue ardışık düzen tarafından kabul edilen istediğiniz. |
| **`-Clobber`** | SwitchParameter | adlı | (İsteğe bağlı) Bu seçenek belirtildiğinde, bir belirtilen çıkış bağlaması için ayarlanacak değer zorlar. | 

Aşağıdaki ortak parametreleri de desteklenir: 
* `Verbose`
* `Debug`
* `ErrorAction`
* `ErrorVariable`
* `WarningAction`
* `WarningVariable`
* `OutBuffer`
* `PipelineVariable`
* `OutVariable` 

Daha fazla bilgi için [hakkında CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216).

#### <a name="push-outputbinding-example-http-responses"></a>Anında iletme OutputBinding örnek: HTTP yanıtları

HTTP tetikleyicisi adlı bir çıkış bağlaması kullanan bir yanıt döndürür `response`. Aşağıdaki örnekte, çıktı bağlaması `response` "çıkış #1" değerine sahiptir:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #1"
})
```

Çıkış yalnızca bir tek değer kabul eder, HTTP olduğundan bir hata oluşan `Push-OutputBinding` ikinci kez çağrılır.

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #2"
})
```

Yalnızca tek değerler kabul çıktıları kullanabilirsiniz `-Clobber` bir koleksiyona eklemek istediğiniz yerine eski değeri geçersiz kılmak için parametre. Aşağıdaki örnek, bir değer zaten eklediğinizi varsayar. Kullanarak `-Clobber`, aşağıdaki örnek yanıttan "çıkış #3" değerini döndürmek için mevcut değeri geçersiz kılar:

```powershell
PS >Push-OutputBinding -Name response -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "output #3"
}) -Clobber
```

#### <a name="push-outputbinding-example-queue-output-binding"></a>Anında iletme OutputBinding örnek: Kuyruk çıktı bağlaması

`Push-OutputBinding` Veri çıkış bağlamaları, gibi göndermek için kullanılan bir [Azure kuyruk depolama çıkış bağlaması](functions-bindings-storage-queue.md#output). Aşağıdaki örnekte, sıraya yazılan ileti "çıkış #1" değerine sahiptir:

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #1"
```

Bir depolama kuyruğu için çıktı bağlaması birden çok çıkış değerleri kabul eder. İlk iki öğe içeren bir liste kuyruğa yazdıktan sonra bu durumda, aşağıdaki örnekte çağırma: "çıkış #1" ve "çıkış #2".

```powershell
PS >Push-OutputBinding -Name outQueue -Value "output #2"
```

Aşağıdaki örnek önceki iki sonra çağrıldığında, iki daha fazla değer çıkış koleksiyona ekler:

```powershell
PS >Push-OutputBinding -Name outQueue -Value @("output #3", "output #4")
```

Sıraya yazılan, ileti bu dört değeri içeriyor: "çıkış #1", "çıkış #2", "çıkış #3" ve "çıkış #4".

#### <a name="get-outputbinding-cmdlet"></a>`Get-OutputBinding` Cmdlet'i

Kullanabileceğiniz `Get-OutputBinding` , çıkış bağlamaları için ayarlanmış değerleri almak için cmdlet'i. Bu cmdlet ile ilgili değerleri çıkış bağlamaları adlarını içeren bir hashtable alır. 

Kullanımının bir örneği verilmiştir `Get-OutputBinding` geçerli bağlama değer döndürmek için:

```powershell
Get-OutputBinding
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
MyOtherQueue                   myData
```

`Get-OutputBinding` Ayrıca adlı bir parametreyi içeren `-Name`, aşağıdaki örnekte olduğu gibi döndürülen bağlama filtrelemek için kullanılabilir:

```powershell
Get-OutputBinding -Name MyQ*
```

```Output
Name                           Value
----                           -----
MyQueue                        myData
```

Joker karakter (*) de desteklenmektedir `Get-OutputBinding`.

## <a name="logging"></a>Günlüğe Kaydetme

PowerShell işlevlerde günlüğü normal PowerShell günlükleri gibi çalışır. Günlüğü cmdlet'leri, her çıkış akımına yazmak için kullanabilirsiniz. Her cmdlet işlevleri tarafından kullanılan bir günlük düzeyine eşler.

| Günlük düzeyi işlevleri | Günlüğe kaydetme cmdlet'i |
| ------------- | -------------- |
| Hata | **`Write-Error`** |
| Uyarı | **`Write-Warning`**  | 
| Bilgiler | **`Write-Information`** <br/> **`Write-Host`** <br /> **`Write-Output`**      | Bilgiler | Yazar _bilgi_ düzeyinde günlüğe kaydetme. |
| Hata ayıklama | **`Write-Debug`** |
| İzleme | **`Write-Progress`** <br /> **`Write-Verbose`** |

Bu cmdlet'lerinin yanı sıra, herhangi bir şey işlem hattının yazılan yönlendireceği `Information` günlük düzeyi ve görüntülenen varsayılan biçimlendirme PowerShell ile.

> [!IMPORTANT]
> Kullanarak `Write-Verbose` veya `Write-Debug` cmdlet'leri değil ayrıntılı görmek ve hata ayıklama düzeyinde günlüğe kaydetme yeterli. Gerçekten önem verdiğiniz günlükleri düzeyini bildirir günlük düzeyi eşiği de yapılandırmanız gerekir. Daha fazla bilgi için bkz. [işlev uygulaması günlük düzeyini yapılandırma](#configure-the-function-app-log-level).

### <a name="configure-the-function-app-log-level"></a>İşlev uygulaması günlük düzeyini yapılandırma

Azure işlevleri denetimi yolu işlevleri Yazar günlüklere kolaylaştırmak için eşik düzeyi tanımlamanıza olanak sağlar. Konsoluna yazılan tüm izlemeleri eşiği ayarlamak için kullanın `logging.logLevel.default` özelliğinde [ `host.json` dosya][host.json başvurusu]. Bu ayar, işlev uygulamanızdaki tüm işlevler için geçerlidir.

Aşağıdaki örnek, tüm işlevler için ayrıntılı günlük kaydını etkinleştirmek için yönelik eşiği ayarlayan ancak adlı bir işlev için hata ayıklama günlük kaydını etkinleştirme eşiği ayarlayan `MyFunction`:

```json
{
    "logging": {
        "logLevel": {
            "Function.MyFunction": "Debug",
            "default": "Trace"
        }
    }
}  
```

Daha fazla bilgi için [host.json başvurusu].

### <a name="viewing-the-logs"></a>Günlükleri görüntüleme

İşlev uygulamanız Azure üzerinde çalışıyorsa, izlemek için Application Insights'ı kullanabilirsiniz. Okuma [Azure işlevleri izleme](functions-monitoring.md) görüntüleme ve işlev günlükleri sorgulama hakkında daha fazla bilgi edinmek için.

İşlev uygulamanızı geliştirme için yerel olarak çalıştırıyorsanız, varsayılan dosya sistemine kaydeder. Konsolunda günlükleri görmek için ayarlanmış `AZURE_FUNCTIONS_ENVIRONMENT` ortam değişkenine `Development` işlev uygulamasını başlatmadan önce.

## <a name="triggers-and-bindings-types"></a>Tetikleyiciler ve bağlamalar türleri

İşlev uygulamanızı kullanmak için kullanabileceğiniz bir dizi Tetikleyicileri ve bağlamaları vardır. Tetikleyiciler ve bağlamalar tam listesini [burada bulunabilir](functions-triggers-bindings.md#supported-bindings).

Tüm tetikleyiciler ve bağlamalar kodda birkaç gerçek veri türleri olarak temsil edilir:

* Hashtable
* dize
* byte[]
* int
* double
* HttpRequestContext
* HttpResponseContext

Bu listedeki ilk beş tür standart .NET türleridir. Yalnızca kullanılan son iki [HttpTrigger tetikleyici](#http-triggers-and-bindings).

İşlevlerinizi her bağlama parametresi şu türlerden birinde olmalıdır.

### <a name="http-triggers-and-bindings"></a>HTTP Tetikleyicileri ve bağlamaları

HTTP ve Web kancası Tetikleyicileri ve bağlamaları, HTTP iletileri temsil etmek için istek ve yanıt nesneleri kullanın. çıkışı.

#### <a name="request-object"></a>İstek nesnesi

Betiğe geçirilen istek nesnesi türüdür `HttpRequestContext`, aşağıdaki özelliklere sahiptir:

| Özellik  | Description                                                    | Type                      |
|-----------|----------------------------------------------------------------|---------------------------|
| **`Body`**    | İstek gövdesini içeren bir nesne. `Body` verilere göre en iyi türü seri hale getirilir. Verileri JSON ise, örneğin, karma tablosu olarak geçirilir. Verileri bir dize ise, bunu bir dize olarak geçirilir. | object |
| **`Headers`** | İstek üst bilgilerini içeren bir sözlük.                | Sözlük < string, string ><sup>*</sup> |
| **`Method`** | İsteğin HTTP yöntemi.                                | dize                    |
| **`Params`**  | İstek yönlendirme parametrelerini içeren bir nesne. | Sözlük < string, string ><sup>*</sup> |
| **`Query`** | Sorgu parametrelerini içeren bir nesne.                  | Sözlük < string, string ><sup>*</sup> |
| **`Url`** | İsteğin URL'si.                                        | dize                    |

<sup>*</sup> Tüm `Dictionary<string,string>` anahtarları duyarsızdır.

#### <a name="response-object"></a>Yanıt nesnesi

Yanıt nesnesini geri göndermesi gerektiğini türüdür `HttpResponseContext`, aşağıdaki özelliklere sahiptir:

| Özellik      | Description                                                 | Type                      |
|---------------|-------------------------------------------------------------|---------------------------|
| **`Body`**  | Yanıtın gövdesini içeren bir nesne.           | object                    |
| **`ContentType`** | Yanıtın içerik türünü ayarlamak için bir kısa el. | dize                    |
| **`Headers`** | Yanıt üst bilgilerini içeren bir nesne.               | Sözlük veya karma tablosu   |
| **`StatusCode`**  | Yanıtın HTTP durum kodu.                       | dize veya tamsayı             |

#### <a name="accessing-the-request-and-response"></a>İstek ve yanıt erişme

HTTP tetikleyicileri ile çalışırken, HTTP isteği, diğer giriş bağlama işlemiyle olduğu aynı şekilde erişebilirsiniz. İçinde `param` blok.

Kullanım bir `HttpResponseContext` aşağıda gösterildiği gibi bir yanıt döndürülecek nesne:

`function.json`

```json
{
  "bindings": [
    {
      "type": "httpTrigger",
      "direction": "in",
      "authLevel": "anonymous"
    },
    {
      "type": "http",
      "direction": "out"
    }
  ]
}
```

`run.ps1`

```powershell
param($req, $TriggerMetadata)

$name = $req.Query.Name

Push-OutputBinding -Name res -Value ([HttpResponseContext]@{
    StatusCode = [System.Net.HttpStatusCode]::OK
    Body = "Hello $name!"
})
```

Bu işlev çağırma sonucu şöyle olacaktır:

```
PS > irm http://localhost:5001?Name=Functions
Hello Functions!
```

### <a name="type-casting-for-triggers-and-bindings"></a>Tetikleyiciler ve bağlamalar için tür atama

Belirli bağlamaları blob bağlama gibi parametrenin türünü belirtebilirsiniz.

Örneğin, bir dize olarak sağlanan Blob depolamadan veri sağlamak için şu tür başvurusuna ekleyin my `param` engelle:

```powershell
param([string] $myBlob)
```

## <a name="powershell-profile"></a>PowerShell profili

PowerShell'de, bir PowerShell profili kavramı yoktur. PowerShell profilleri ile ilgili bilgi sahibi değilseniz bkz [profilleri hakkında](/powershell/module/microsoft.powershell.core/about/about_profiles).

İşlev uygulaması başladığında PowerShell işlevleri'nde profili betiğini yürütür. İşlev uygulamaları başlangıç ilk kez dağıttığınızda ve sonrasında idled ([hazırlıksız başlatma](#cold-start)).

Visual Studio Code ve Azure işlevleri temel araçları, bir varsayılan gibi araçları kullanarak bir işlev uygulaması oluşturduğunuzda `profile.ps1` sizin için oluşturulur. Varsayılan profil korunur [Core araçları GitHub deposunda](https://github.com/Azure/azure-functions-core-tools/blob/dev/src/Azure.Functions.Cli/StaticResources/profile.ps1) ve içerir:

* Azure otomatik MSI kimlik doğrulaması.
* Azure PowerShell'i temel yetenekleri `AzureRM` isterseniz PowerShell diğer adları.

## <a name="powershell-version"></a>PowerShell sürümü

Aşağıdaki tabloda her önemli işlevler çalışma zamanı sürümü tarafından kullanılan PowerShell sürümünü gösterir:

| İşlevler sürümü | PowerShell sürümü                             |
|-------------------|------------------------------------------------|
| 1.x               | Windows PowerShell 5.1 (çalışma zamanı tarafından kilitlendi) |
| 2.x               | PowerShell Core 6                              |

Geçerli sürümü tarafından yazdırma gördüğünüz `$PSVersionTable` herhangi bir işlevden.

## <a name="dependency-management"></a>Bağımlılık yönetimi

Azure modüllerini hizmet tarafından yönetme PowerShell işlevleri destekler. Host.json değiştirme ve managedDependency etkin özelliğin true olarak ayarlanması, requirements.psd1 dosya işlenebilir. En son Azure modüllerine otomatik olarak indirilir ve işlevi için kullanılabilir.

host.json
```json
{
    "managedDependency": {
        "enabled": true
    }
}
```

Requirements.psd1

```powershell
@{
    Az = '1.*'
}
```

Kendi özel yararlanarak modül veya modülleri [PowerShell Galerisi](https://powershellgallery.com) nasıl, normalde yaptığınız değerinden biraz daha farklıdır.

Yerel makinenizde modülünü yüklediğinizde, küresel olarak kullanılabilir klasörlerinde birine gider, `$env:PSModulePath`. Azure'da işlevinizi çalıştığından, makinenizde yüklü modülleri erişemezsiniz. Bunu gerektiren `$env:PSModulePath` uygulama için bir PowerShell işlevi farklıdır `$env:PSModulePath` normal bir PowerShell betik.

İşlevlerde, `PSModulePath` iki yolları içerir:

* A `Modules` işlev uygulamanızın kök dizininde mevcut bir klasör.
* Bir yol için bir `Modules` PowerShell dil çalışan içinde bulunduğu klasör.

### <a name="function-app-level-modules-folder"></a>İşlevi uygulama düzeyinde `Modules` klasörü

Özel modüller ya da PowerShell modülleri PowerShell Galerisi'nden kullanmak için üzerinde işlevlerinizi bağımlı modülleri yerleştirebilirsiniz bir `Modules` klasör. Bu klasörden modülleri işlevler çalışma zamanının otomatik olarak büyük/küçük harf kullanılabilir. Herhangi bir işlevde işlev uygulaması bu modülleri kullanabilirsiniz.

Bu özelliğin avantajlarından yararlanmak için oluşturun bir `Modules` işlev uygulamanızın kök klasöründe. İşlevlerinizi bu konumda, kullanmak istediğiniz modülleri kaydedin.

```powershell
mkdir ./Modules
Save-Module MyGalleryModule -Path ./Modules
```

Kullanma `Save-Module` işlevlerinizi kullanmak modüllerinin Tümünü Kaydet veya kendi özel modüllerle Kopyala `Modules` klasör. Bir modül klasörle işlev uygulamanız aşağıdaki klasör yapısına sahip olmalıdır:

```
PSFunctionApp
 | - MyFunction
 | | - run.ps1
 | | - function.json
 | - Modules
 | | - MyGalleryModule
 | | - MyOtherGalleryModule
 | | - MyCustomModule.psm1
 | - local.settings.json
 | - host.json
```

Bu işlev uygulamanızı başlattığınızda, PowerShell dil çalışan ekler `Modules` klasörüne `$env:PSModulePath` böylece normal bir PowerShell komut dosyası olduğu gibi modülü autoloading güvenebilirsiniz.

### <a name="language-worker-level-modules-folder"></a>Dil alt düzey `Modules` klasörü

Birkaç modülü, PowerShell dil çalışan tarafından yaygın olarak kullanılır. Bu modüller son konumda tanımlanan `PSModulePath`. 

Geçerli modüllerin listesini aşağıdaki gibidir:

* [Microsoft.PowerShell.Archive](https://www.powershellgallery.com/packages/Microsoft.PowerShell.Archive): gibi arşivlerin çalışmak için kullanılan modül `.zip`, `.nupkg`ve diğerleri.
* **ThreadJob**: İş parçacığı tabanlı bir uygulama PowerShell işin API'leri.

En son sürümü bu modüllerin işlevleri tarafından kullanılır. Bu modül belirli bir sürümünü kullanmak için belirli bir sürüm de koyabilirsiniz `Modules` işlev uygulamanızın klasör.

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, [uygulama ayarları](functions-app-settings.md), gibi hizmet bağlantısı dizeleri sunulur ortam değişkenleri olarak yürütme sırasında. Bu ayarları kullanarak erişebileceğiniz `$env:NAME_OF_ENV_VAR`, aşağıdaki örnekte gösterildiği gibi:

```powershell
param($myTimer)

Write-Host "PowerShell timer trigger function ran! $(Get-Date)"
Write-Host $env:AzureWebJobsStorage
Write-Host $env:WEBSITE_SITE_NAME
```

[!INCLUDE [Function app settings](../../includes/functions-app-settings.md)]

Uygulama ayarlarını okuma yerel olarak çalıştırılırken [local.settings.json](functions-run-local.md#local-settings-file) proje dosyası.

## <a name="concurrency"></a>Eşzamanlılık

Varsayılan olarak, işlevleri PowerShell çalışma zamanı yalnızca bir işlevin bir çağrı birer birer işleyebilirsiniz. Ancak, bu eşzamanlılık düzeyi aşağıdaki durumlarda yeterli olmayabilir:

* Ne zaman aynı anda çok sayıda çağrıları işlemek çalışıyorsunuz.
* İşlev uygulamasının içindeki diğer işlevleri çağırma işlevleri olduğunda.

Tamsayı değerine aşağıdaki ortam değişkenini ayarlayarak bu davranışı değiştirebilirsiniz:

```
PSWorkerInProcConcurrencyUpperBound
```

Bu ortam değişkenini ayarladığınız [uygulama ayarları](functions-app-settings.md) işlev uygulamanızın.

### <a name="considerations-for-using-concurrency"></a>Eşzamanlılık kullanma konuları

PowerShell bir _tek iş parçacıklı_ varsayılan komut dosyası dilidir. Ancak, aynı işlemde birden çok PowerShell çalışma alanları kullanılarak eşzamanlılık eklenebilir. Bu özellik, Azure işlevleri PowerShell çalışma zamanının nasıl çalıştığını olur.

Bu yaklaşım bazı dezavantajları vardır.

#### <a name="concurrency-is-only-as-good-as-the-machine-its-running-on"></a>Eşzamanlılık yalnızca üzerinde çalıştırıldığı makine iyidir

İşlev uygulamanız çalışıyorsa bir [App Service planı](functions-scale.md#app-service-plan) yalnızca bir çekirdek destekleyen ve eşzamanlılık çok yardımcı olmaz. Yük dengelemeye yardımcı olacak ek hiçbir çekirdek olduğundan olmasıdır. Bu durumda, içerik-çalışma alanları arasında geçiş yap tek çekirdekli sahip olduğunda performans farklılık gösterebilir.

[Tüketim planı](functions-scale.md#consumption-plan) eşzamanlılık yararlanamaz yalnızca bir çekirdek kullanarak çalıştırır. Bunun yerine eşzamanlılık tam olarak yararlanmak isterseniz, işlevlerinizin yeterli çekirdek içeren özel bir App Service planı üzerinde çalışan bir işlev uygulamasına dağıtın.

#### <a name="azure-powershell-state"></a>Azure PowerShell durumu

Azure PowerShell kullanan bazı _işlem düzeyinde_ bağlamları ve tasarruf aşırı yazmaya yardımcı olmak için durum. İşlev uygulamanızda eşzamanlılık üzerinde etkinleştirmek ve durum değişikliği eylemleri, Bununla birlikte, yarış koşulları bulunabileceğini. Bu yarış, belirli bir durumu bir çağrı güvenir ve diğer çağırma durumu değiştirildi çünkü hata ayıklama zordur.

Büyük değer yoktur eşzamanlılık Azure PowerShell ile de bu yana bazı işlemler, önemli miktarda zaman alabilir. Ancak, dikkatli devam gerekir. Bir yarış durumu karşılaştığınız şüpheleniyorsanız, eşzamanlılık ayarlamak geri `1` ve isteği yeniden deneyin.

## <a name="configure-function-scriptfile"></a>Yapılandırma işlevi `scriptFile`

Bir PowerShell işlevi yürütüldüğü varsayılan olarak, `run.ps1`, kendi ilişkili olarak aynı üst dizine paylaşan bir dosya `function.json`.

`scriptFile` Özelliğinde `function.json` aşağıdaki gibi görünen bir klasör yapısı almak için kullanılabilir:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.ps1
```

Bu durumda, `function.json` için `myFunction` içeren bir `scriptFile` özelliği ile çalıştırmak için dışarı aktarılan işlevin dosyasına başvuruyor.

```json
{
  "scriptFile": "../lib/PSFunction.ps1",
  "bindings": [
    // ...
  ]
}
```

## <a name="use-powershell-modules-by-configuring-an-entrypoint"></a>EntryPoint yapılandırarak PowerShell modüllerini kullanma

Bu makalede, varsayılan olarak PowerShell işlevleri göstermiştir `run.ps1` şablonları tarafından oluşturulan betik dosyası.
Bununla birlikte, PowerShell modülleri işlevlerinizi de içerebilir. Belirli işlev kodunuzu modülünde kullanarak başvurabilirsiniz `scriptFile` ve `entryPoint` function.json alanlarında ' yapılandırma dosyası.

Bu durumda, `entryPoint` bir işlev veya başvurulan PowerShell modülü cmdlet adı `scriptFile`.

Aşağıdaki klasör yapısına göz önünde bulundurun:

```
FunctionApp
 | - host.json
 | - myFunction
 | | - function.json
 | - lib
 | | - PSFunction.psm1
```

Burada `PSFunction.psm1` içerir:

```powershell
function Invoke-PSTestFunc {
    param($InputBinding, $TriggerMetadata)

    Push-OutputBinding -Name OutputBinding -Value "output"
}

Export-ModuleMember -Function "Invoke-PSTestFunc"
```

Bu örnekte, yapılandırmasını `myFunction` içeren bir `scriptFile` başvuran özelliği `PSFunction.psm1`, başka bir klasörde bir PowerShell modülü olan.  `entryPoint` Özellik başvurularını `Invoke-PSTestFunc` modülündeki giriş noktası işlevi.

```json
{
  "scriptFile": "../lib/PSFunction.psm1",
  "entryPoint": "Invoke-PSTestFunc",
  "bindings": [
    // ...
  ]
}
```

Bu yapılandırmayla `Invoke-PSTestFunc` tam olarak yürütülen bir `run.ps1` gerekir.

## <a name="considerations-for-powershell-functions"></a>PowerShell işlevler için dikkat edilmesi gerekenler

PowerShell işlevleri ile çalışırken, aşağıdaki bölümlerde konuları unutmayın.

### <a name="cold-start"></a>Hazırlıksız başlatma

Azure işlevleri'nde geliştirirken [sunucusuz barındırma modeli](functions-scale.md#consumption-plan), soğuk başlangıçlar, bir gerçeklik. *Hazırlıksız başlatma* için süre başvurduğu bir isteği işlemek için çalıştırmaya başlamak işlev uygulamanız için alır. İşlev uygulamanızın etkin olmadığı dönemler sırasında kapatma çünkü daha sık tüketim planı hazırlıksız başlatma gerçekleşir.

### <a name="bundle-modules-instead-of-using-install-module"></a>Kullanmak yerine paket modülleri `Install-Module`

Betiğinizi her çağrıda çalıştırılır. Kullanmaktan kaçının `Install-Module` betiğinizde. Bunun yerine kullanın `Save-Module` yayımlamadan önce işlevinizi modülü indiriliyor boşa gerek kalmaz. Hazırlıksız başlatma işlemlerinden doğan işlevlerinizi etkileyen, işlev uygulamanızı dağıtmayı göz önünde bulundurun bir [App Service planı](functions-scale.md#app-service-plan) kümesine *her zaman açık* veya bir [Premium planı](functions-scale.md#premium-plan).

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)

[Host.JSON başvurusu]: functions-host-json.md
