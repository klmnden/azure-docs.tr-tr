---
title: 'Azure işlevleri F # Geliştirici Başvurusu | Microsoft Docs'
description: 'F # kullanarak Azure işlevleri geliştirmek nasıl anlayın.'
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
editor: ''
tags: ''
keywords: 'Azure işlevleri, İşlevler, olay işleme, Web kancalarını, dinamik işlem, sunucusuz mimarisi, F #'
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: functions
ms.devlang: fsharp
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/09/2016
ms.author: syclebsc
ms.openlocfilehash: 2c84de3f38a49bc97fda04a7a4eb449a1f7d14bd
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="azure-functions-f-developer-reference"></a>Azure işlevleri F # Geliştirici Başvurusu

F # için Azure işlevleri küçük parçalarını kodu veya "işlevleri" bulutta kolayca çalıştırmak için bir çözümdür. Veri, F # işlevi işlev bağımsız değişkenleri aracılığıyla akar. Bağımsız değişken adları belirtilir `function.json`, ve işlevi Günlükçü ve iptal belirteçleri gibi şeyleri erişmek için önceden tanımlanmış adları vardır.

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx nasıl çalışır?
Bir `.fsx` F # betiği bir dosyadır. Bu, tek bir dosyada bulunan F # proje olarak düşünülebilir. Dosyası kodu programınızın (Bu durumda, Azure işlevinizi) içerir ve bağımlılıkları yönetmek için yönergeleri.

Kullandığınızda, bir `.fsx` bir Azure işlevi için yaygın olarak derlemeler "ortak" yerine işlevi kodlarına odaklanmasını olanak tanıyan sizin için otomatik olarak dahil gereklidir.

## <a name="binding-to-arguments"></a>Bağımsız değişkenler bağlama
Her bağlama bağımsız değişkenler, içinde ayrıntılı olarak bazı kümesini destekler [Azure işlevleri Tetikleyicileri ve bağlamaları Geliştirici Başvurusu](functions-triggers-bindings.md). Örneğin, bir blob tetikleyici destekleyen bağımsız değişken bağlamaları kullanarak bir F # kayıt ifade bir POCO biridir. Örneğin:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F # Azure işlevinizi bağımsız değişkenlerden biri veya daha fazla sürer. Biz Azure işlevleri bağımsız değişkenleri hakkında konuşurken biz başvurmak *giriş* bağımsız değişkenleri ve *çıkış* bağımsız değişkenler. Ne gibi ses giriş bağımsız değişkeni tam olduğundan: F # Azure işlevinizi için giriş. Bir *çıkış* değişkendir değişebilir veri veya `byref<>` geri veri iletmek için bir yol olarak hizmet veren bir bağımsız değişken *çıkışı* , işlevin.

Yukarıdaki örnekte `blob` bir giriş bağımsız değişkeni ve `output` bir çıktı bağımsız değişken. Kullandık bildirimi `byref<>` için `output` (eklemeye gerek yoktur `[<Out>]` ek açıklama). Kullanarak bir `byref<>` türü hangi kaydı veya bağımsız değişkeni başvuruda bulunduğu nesne değiştirmek, işlevi sağlar.

İle bir F # kaydı bir girdi türü olarak kullanıldığında, kayıt tanımı işaretlenmelidir `[<CLIMutable>]` işlevinizi için kayıt geçirmeden önce alanlarını uygun şekilde ayarlamak Azure işlevleri framework izin vermek üzere. Başlık altında `[<CLIMutable>]` kaydı özellikler için ayarlayıcılar oluşturur. Örneğin:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: TraceWriter) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

F # sınıfı, giriş ve çıkış bağımsız değişkenler her ikisi için de kullanılabilir. Bir sınıf için özellikleri genellikle alıcılar ve ayarlayıcılar gerekir. Örneğin:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Günlüğe kaydetme
Çıktıyı oturum, [akış günlükleri](../app-service/web-sites-enable-diagnostic-log.md) F #'ta işlevinizi türünde bir bağımsız değişken zamanınızı `TraceWriter`. Tutarlılık için bu bağımsız değişken adlandırılan olan öneririz `log`. Örneğin:

```fsharp
let Run(blob: string, output: byref<string>, log: TraceWriter) =
    log.Verbose(sprintf "F# Azure Function processed a blob: %s" blob)
    output <- input
```

## <a name="async"></a>Zaman uyumsuz
`async` İş akışı kullanılabilir, ancak sonuç döndürmesi gerekir bir `Task`. Bu, yapılabilir `Async.StartAsTask`, örneğin:

```fsharp
let Run(req: HttpRequestMessage) =
    async {
        return new HttpResponseMessage(HttpStatusCode.OK)
    } |> Async.StartAsTask
```

## <a name="cancellation-token"></a>İptal belirteci
İşlevinizi kapatma işleyebilmesini gerekiyorsa verebilirsiniz bir [ `CancellationToken` ](https://msdn.microsoft.com/library/system.threading.cancellationtoken.aspx) bağımsız değişkeni. Bu ile birleştirilebilir `async`, örneğin:

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

Şu ad alanlarından otomatik olarak açılır:

* `System`
* `System.Collections.Generic`
* `System.IO`
* `System.Linq`
* `System.Net.Http`
* `System.Threading.Tasks`
* `Microsoft.Azure.WebJobs`
* `Microsoft.Azure.WebJobs.Host`.

## <a name="referencing-external-assemblies"></a>Dış derlemelere başvurma
Framework'te derleme başvurularını ile benzer şekilde, eklenebilecek `#r "AssemblyName"` yönergesi.

```fsharp
#r "System.Web.Http"

open System.Net
open System.Net.Http
open System.Threading.Tasks

let Run(req: HttpRequestMessage, log: TraceWriter) =
    ...
```

Aşağıdaki derlemeler barındırma ortamı Azure işlevleri tarafından otomatik olarak eklenir:

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

Ayrıca, aşağıdaki derlemeler özel ortası ve simplename tarafından başvurulan (örneğin `#r "AssemblyName"`):

* `Newtonsoft.Json`
* `Microsoft.WindowsAzure.Storage`
* `Microsoft.ServiceBus`
* `Microsoft.AspNet.WebHooks.Receivers`
* `Microsoft.AspNEt.WebHooks.Common`.

Özel derleme başvurusu ihtiyacınız varsa, derleme dosyasına yükleyebilirsiniz bir `bin` klasörüne görelidir dosyasını kullanarak adlandırın (örneğin, işlev ve başvurusu  `#r "MyAssembly.dll"`). İşlev klasörünüze dosyaları karşıya yükleme hakkında daha fazla bilgi için paket yönetimi hakkında aşağıdaki bölümüne bakın.

## <a name="editor-prelude"></a>Düzenleyici Prelude
F # derleyici hizmetlerini destekleyen bir düzenleyici ad alanları ve Azure işlevleri otomatik olarak içeren derlemeler haberdar olmaz. Bu nedenle, kullandığınız derlemelerini bulmak Düzenleyicisi yardımcı olan bir prelude dahil edilecek ve ad alanları açıkça açmak için yararlı olabilir. Örneğin:

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

Azure işlevleri, kodunuzu yürütüldüğünde, kaynağıyla işler `COMPILED` tanımlı, bu nedenle Düzenleyicisi prelude yoksayıldı.

<a name="package"></a>

## <a name="package-management"></a>Paket Yönetimi
NuGet paketlerini bir F # işlevi kullanmak için ekleyin bir `project.json` dosyasını işlevin klasöre işlevi uygulamanın dosya sistemi. İşte bir örnek `project.json` NuGet paketi başvuru ekler dosya `Microsoft.ProjectOxford.Face` sürüm 1.1.0:

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

Yalnızca .NET Framework 4.6 desteklenmez, bu nedenle olduğundan emin olun, `project.json` dosyayı belirtir `net46` aşağıda gösterildiği gibi.

Karşıya yüklediğiniz zaman bir `project.json` dosya, çalışma zamanı paketleri alır ve paketi derleme başvuruları otomatik olarak ekler. Eklemeniz gerekmez `#r "AssemblyName"` yönergeleri. Yalnızca gerekli Ekle `open` deyimleri için `.fsx` dosya.

F # Hizmetleri derlemek Düzenleyicisi'nin etkileşim artırmak için Düzenleyicisi prelude otomatik olarak başvuru derlemeleri koymak isteyebilirsiniz.

### <a name="how-to-add-a-projectjson-file-to-your-azure-function"></a>Nasıl ekleneceği bir `project.json` Azure işlevinizi dosyasına
1. Azure portalında işlevinizi açarak yapabilirsiniz işlevi uygulamanızı emin yaparak Başlangıç çalışıyor. Bu ayrıca akış günlüklerine paket yükleme çıktısı görüntülenir burada erişmenizi sağlar.
2. Karşıya yüklemek için bir `project.json` dosya, açıklanan yöntemlerden birini kullanın [işlevi uygulama dosyaları güncelleştirmek nasıl](functions-reference.md#fileupdate). Kullanıyorsanız [Azure işlevleri için sürekli dağıtım](functions-continuous-deployment.md), ekleyebileceğiniz bir `project.json` ile dağıtım dalınızdaki eklemeden önce denemek için hazırlama dalı dosyasına.
3. Sonra `project.json` dosya eklenir, işlevinizi aşağıdaki örneğe benzer bir çıktı günlük akış görürsünüz:

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
Bir ortam değişkeni veya ayar değeri bir uygulamayı almak için `System.Environment.GetEnvironmentVariable`, örneğin:

```fsharp
open System.Environment

let Run(timer: TimerInfo, log: TraceWriter) =
    log.Info("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.Info("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.Fsx kodu yeniden kullanma
Diğer kodu kullanabilirsiniz `.fsx` kullanarak dosyaları bir `#load` yönergesi. Örneğin:

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

Yollar sağlar için `#load` göreli konumunu yönerge olan, `.fsx` dosya.

* `#load "logger.fsx"` işlev klasöründe bir dosya yükler.
* `#load "package\logger.fsx"` bulunan bir dosya yükler `package` işlevi klasöründe.
* `#load "..\shared\mylogger.fsx"` bulunan bir dosya yükler `shared` klasör başka bir deyişle, işlevi klasör ile aynı düzeyde doğrudan altında `wwwroot`.

`#load` Yönergesi yalnızca çalışır `.fsx` (F # betik) dosyaları ve değil `.fs` dosyaları.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [F # Kılavuzu](/dotnet/articles/fsharp/index)
* [Azure İşlevleri için En İyi Uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevlerini test etme](functions-test-a-function.md)
* [Azure işlevlerini ölçeklendirme](functions-scale.md)

