---
title: 'Azure işlevleri F # Geliştirici Başvurusu | Microsoft Docs'
description: 'F # kullanarak Azure işlevleri geliştirme hakkında bilgi edinin.'
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
keywords: 'Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimari, F #'
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: azure-functions
ms.devlang: fsharp
ms.topic: reference
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 5593f76511f43106d6743a158b051e118ef2a4a6
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125264"
---
# <a name="azure-functions-f-developer-reference"></a>Azure işlevleri F # Geliştirici Başvurusu

F # Azure işlevleri için küçük parçaları kodu veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Veri, F # işlevi işlev bağımsız değişkenleri aracılığıyla akar. Bağımsız değişken adları içinde belirtilen `function.json`, ve işlevi Günlükçü ve iptal belirteçlerini gibi şeyleri erişmek için önceden tanımlanmış adları vardır.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx nasıl çalışır?
Bir `.fsx` bir F # komut dosyasıdır. Bu, tek bir dosyada yer alan bir F # projesi olarak düşünülebilir. (Bu durumda, Azure işlevinizi), programınız için kodu her iki dosyayı içeren ve bağımlılıkları yönetmek için yönergeleri.

Kullandığınızda, bir `.fsx` bir Azure işlevi için yaygın olarak gerekli derlemeleri işlevi "standart" yerine koda odaklanabilirsiniz sizin için otomatik olarak dahil edilir.

## <a name="folder-structure"></a>klasör yapısı

Bir F # komut dosyası proje için klasör yapısı aşağıdaki gibi görünür:

```
FunctionsProject
 | - MyFirstFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - MySecondFunction
 | | - run.fsx
 | | - function.json
 | | - function.proj
 | - host.json
 | - extensions.csproj
 | - bin
```

İşlev uygulamasını yapılandırmak için kullanılan bir paylaşılan [host.json] (işlevler-host-json.md) dosyası yoktur. Her işlev, kendi kod dosyası (.fsx) ve bağlama yapılandırma dosyası (function.json) vardır.

Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı içinde tanımlanmıştır `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](functions-triggers-bindings.md#local-development-azure-functions-core-tools). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenler için bağlama
Her bağlama bazı bağımsız değişkenler, içinde ayrıntılı olarak kümesini destekleyen [Azure işlevleri Tetikleyicileri ve bağlamaları Geliştirici Başvurusu](functions-triggers-bindings.md). Örneğin, blob Tetikleyicileri destekler bağımsız değişken bağlamaları bir F # kayıt kullanarak ifade edilebilir bir POCO biridir. Örneğin:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F # Azure işlevi, bağımsız değişkenlerden biri veya daha fazla sürebilir. Biz Azure işlevleri bağımsız değişkenler hakkında konuşurken, diyoruz *giriş* bağımsız değişkenleri ve *çıkış* bağımsız değişkenler. Ne gibi ses giriş bağımsız değişkeni tam olarak olmadığı: Giriş, F # Azure işlevi. Bir *çıkış* değişkendir değişebilir veri veya `byref<>` geri veri iletmek için bir yol hizmet veren bağımsız değişken *kullanıma* işlevinizin.

Yukarıdaki örnekte `blob` bir giriş bağımsız değişkeni ve `output` bir çıkış bağımsız değişkeni. Kullanılan bildirim `byref<>` için `output` (eklemenize gerek yoktur `[<Out>]` ek açıklama). Kullanarak bir `byref<>` tür hangi kayıt veya bağımsız değişkenin başvurduğu nesneyi değiştirmek, işlevinizi sağlar.

İle bir F # kayıt, bir giriş türü olarak kullanıldığında, kayıt tanımı işaretlenmelidir `[<CLIMutable>]` kaydı işlevinize geçirmeden önce alanlarını uygun şekilde ayarlamak Azure işlevleri framework izin vermek üzere. Başlık altında `[<CLIMutable>]` ayarlayıcılar kayıt özellikleri oluşturur. Örneğin:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Bir F # sınıfı, giriş ve çıkış değişkenleri her ikisi için de kullanılabilir. Bir sınıf için özellikler genellikle alıcılar ve ayarlayıcılar gerekir. Örneğin:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Günlüğe kaydetme
Çıkış için oturum, [akış günlükleri](../app-service/web-sites-enable-diagnostic-log.md) F #'ta işlevinizi türünde bir bağımsız değişken almalıdır `TraceWriter`. Tutarlılık sağlamak için bu bağımsız değişken adlı olan öneririz `log`. Örneğin:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>zaman uyumsuz
`async` İş akışı kullanılabilir, ancak sonuç döndürmesi gerekir. bir `Task`. Bu ile yapılabilir `Async.StartAsTask`, örneğin:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>İptal belirteci
İşleviniz kapatma düzgün biçimde işlemesi gerekiyorsa verebilirsiniz bir [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) bağımsız değişken. Bu ile birleştirilebilir `async`, örneğin:

```fsharp
let Run(req: HttpRequestMessage, token: CancellationToken)
    let f = async {
        do! Async.Sleep(10)
        return new HttpResponseMessage(HttpStatusCode.OK)
    }
    Async.StartAsTask(f, token)
```

## <a name="importing-namespaces"></a>Ad alanlarını alma
Ad alanları normal şekilde açılabilir:

```fsharp
open System.Net
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Aşağıdaki ad alanlarını otomatik olarak açılır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Dış derlemeler başvurma
Framework'te derleme başvuruları ile benzer şekilde, eklenebilir `#r "AssemblyName"` yönergesi.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Aşağıdaki derlemeleri Azure işlevleri barındırma ortamı tarafından otomatik olarak eklenir:

* `mscorlib`,
* `System`
* `System.Core`
* `System.Xml`
* `System.Net.Http`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`
* `Microsoft.Azure.WebJobs.Extensions`
* `System.Web.Http`
* `System.Net.Http.Formatting`.

Ayrıca, aşağıdaki derlemeler özel büyük/küçük harfleri ve simplename tarafından başvurulabilir (örneğin `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Özel bir derleme başvurmanız gerekiyorsa, derleme dosyasına yükleyebilirsiniz. bir `bin` klasörüyle ilgili dosyasını kullanarak adlandırın (örneğin, işlevi ve başvuru  `#r "MyAssembly.dll"`). İşlev klasörünüze dosyaları karşıya yükleme hakkında daha fazla bilgi için üzerinde paket Yönetimi aşağıdaki bölüme bakın.

## <a name="editor-prelude"></a>Düzenleyici tanıtımlar
Ad alanları ve Azure işlevleri otomatik olarak içeren derlemeler, F # derleyici hizmetlerini destekleyen bir düzenleyici bilmez. Bu nedenle, kullanmakta olduğunuz derlemelerini Bul Düzenleyicisi yardımcı olan bir tanıtımlar içeren ve açıkça ad alanlarını açmak için yararlı olabilir. Örneğin:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open System
open Microsoft.Azure.WebJobs.Host

let Run(blob: string, output: byref<string>, log: TraceWriter) =
    ...
```

Azure işlevleri, kodunuzu yürütüldüğünde, kaynak ile işler `COMPILED` tanımlanan şekilde Düzenleyicisi tanıtımlar göz ardı edilir.

<a name="package"></a>

## <a name="package-management"></a>Paket yönetimi
F # işlevi NuGet paketlerini kullanmak için ekleme bir `project.json` işlevin klasörüne işlevi uygulamanın dosya sisteminde dosya. İşte bir örnek `project.json` ekleyen bir NuGet paketi başvuru dosyası `Microsoft.ProjectOxford.Face` sürüm 1.1.0 yer:

```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.ProjectOxford.Face": "1.1.0"
      }
    }
   }
}
```

Yalnızca .NET Framework 4.6 desteklenmez, bu nedenle emin olun, `project.json` dosyasını belirtir `net46` burada gösterildiği gibi.

Karşıya yüklerken, bir `project.json` dosya, çalışma zamanı paketlerini alır ve paket derlemelerine başvurular otomatik olarak ekler. Eklemenize gerek yoktur `#r "AssemblyName"` yönergeleri. Yalnızca gerekli Ekle `open` deyimleriyle, `.fsx` dosya.

F # derleme Hizmetleri ile düzenleyicinin etkileşimi geliştirmek için düzenleyici tanıtımlar otomatik olarak başvuru derlemeleri koymak isteyebilirsiniz.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Nasıl ekleneceği bir `project.json` Azure işlevinizi dosyasına
1. Azure Portal'da işlevinize açarak bunu yapabilirsiniz başlangıç işlev uygulamanızı emin olarak çalışıyor. Bu ayrıca akış günlüklerine paket yükleme çıkış burada görüntülenecek erişmenizi sağlar.
2. Karşıya yüklenecek bir `project.json` dosya, açıklanan yöntemlerden birini kullanın [işlevi uygulama dosyalarını nasıl güncelleştireceğinizi](functions-reference.md#fileupdate). Kullanıyorsanız [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md), ekleyebileceğiniz bir `project.json` dağıtım dalınızda eklemeden önce denemeler yapabilmeleri için hazırlama dalınızı dosyasına.
3. Sonra `project.json` dosya eklendiğinde, akış günlüğü işlevinizi aşağıdaki örneğe benzer bir çıktı görürsünüz:

```
2016-04-04T19:02:48.745 Restoring packages.
2016-04-04T19:02:48.745 Starting NuGet restore
2016-04-04T19:02:50.183 MSBuild auto-detection: using msbuild version '14.0' from 'D:\Program Files (x86)\MSBuild\14.0\bin'.
2016-04-04T19:02:50.261 Feeds used:
2016-04-04T19:02:50.261 C:\DWASFiles\Sites\facavalfunctest\LocalAppData\NuGet\Cache
2016-04-04T19:02:50.261 https://api.nuget.org/v3/index.json
2016-04-04T19:02:50.261
2016-04-04T19:02:50.511 Restoring packages for D:\home\site\wwwroot\HttpTriggerCSharp1\Project.json...
2016-04-04T19:02:52.800 Installing Newtonsoft.Json 6.0.8.
2016-04-04T19:02:52.800 Installing Microsoft.ProjectOxford.Face 1.1.0.
2016-04-04T19:02:57.095 All packages are compatible with .NETFramework,Version=v4.6.
2016-04-04T19:02:57.189
2016-04-04T19:02:57.189
2016-04-04T19:02:57.455 Packages restored.
```

## <a name="environment-variables"></a>Ortam değişkenleri
Bir ortam değişkeni veya değeri ayarlamak uygulama almak için kullanın `System.Environment.GetEnvironmentVariable`, örneğin:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.Fsx kodu yeniden kullanma
Diğer kod kullanabileceğiniz `.fsx` dosyaları kullanarak bir `#load` yönergesi. Örneğin:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: TraceWriter) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: TraceWriter, text: string) =
    log.Verbose(text);
```

Yollar sağlar `#load` yönerge olan göreli konumunu, `.fsx` dosya.

* `#load "logger.fsx"` işlev klasöründe bir dosya yükler.
* `#load "package\logger.fsx"` bulunan bir dosya yükler `package` işlevi klasöründe.
* `#load "..\shared\mylogger.fsx"` bulunan bir dosya yükler `shared` klasör diğer bir deyişle, işlev klasör ile aynı düzeyde doğrudan altında `wwwroot`.

`#load` Yönergesi yalnızca çalışır `.fsx` (F # betik) dosyaları ile değil `.fs` dosyaları.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [F # Kılavuzu](/dotnet/articles/fsharp/index)
* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevlerini test etme](functions-test-a-function.md)
* [Azure işlevlerini ölçeklendirme](functions-scale.md)

