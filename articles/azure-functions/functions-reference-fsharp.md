---
title: Azure işlevleri F# Geliştirici Başvurusu | Microsoft Docs
description: Kullanarak Azure işlevleri geliştirme hakkında anlamak F# betiği.
services: functions
documentationcenter: fsharp
author: sylvanc
manager: jbronsk
keywords: Azure işlevleri, İşlevler, olay işleme, Web kancaları, dinamik işlem, sunucusuz mimariF#
ms.assetid: e60226e5-2630-41d7-9e5b-9f9e5acc8e50
ms.service: azure-functions
ms.devlang: fsharp
ms.topic: reference
ms.date: 10/09/2018
ms.author: syclebsc
ms.openlocfilehash: 981ffce34c56f4becee2ed0c72da72baa220e395
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58894364"
---
# <a name="azure-functions-f-developer-reference"></a>Azure işlevleri F# Geliştirici Başvurusu

F#Azure işlevleri, küçük parçaları kodu veya "işlevleri" bulutta kolayca çalıştırmaya yönelik bir çözümdür. Veri akışları halinde, F# işlevi aracılığıyla işlev bağımsız değişkenleri. Bağımsız değişken adları içinde belirtilen `function.json`, ve işlevi Günlükçü ve iptal belirteçlerini gibi şeyleri erişmek için önceden tanımlanmış adları vardır. 

>[!IMPORTANT]
>F#betik (.fsx) tarafından desteklenen yalnızca [sürüm 1.x](functions-versions.md#creating-1x-apps) Azure işlevleri çalışma zamanı. Kullanmak istiyorsanız F# sürüm 2.x çalışma zamanı ile önceden derlenmiş bir kullanmalısınız F# sınıf kitaplığı projesi (.fs). Oluşturma, yönetme ve yayımlama bir F# sınıf kitaplığı projesi, olduğu gibi Visual Studio kullanarak bir [ C# sınıf kitaplığı projesi](functions-dotnet-class-library.md). İşlevleri sürümleri hakkında daha fazla bilgi için bkz. [Azure işlevleri çalışma zamanı sürümleri genel bakış](functions-versions.md).

Bu makalede, zaten okuduğunuz varsayılır [Azure işlevleri Geliştirici Başvurusu](functions-reference.md).

## <a name="how-fsx-works"></a>.Fsx nasıl çalışır?
Bir `.fsx` dosyası bir F# betiği. Bu, olarak düşünülebilir bir F# tek bir dosyada yer alan bir proje. (Bu durumda, Azure işlevinizi), programınız için kodu her iki dosyayı içeren ve bağımlılıkları yönetmek için yönergeleri.

Kullandığınızda, bir `.fsx` bir Azure işlevi için yaygın olarak gerekli derlemeleri işlevi "standart" yerine koda odaklanabilirsiniz sizin için otomatik olarak dahil edilir.

## <a name="folder-structure"></a>klasör yapısı

Klasör yapısı bir F# betik projesi aşağıdaki gibi görünür:

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

Var olan bir paylaşılan [host.json](functions-host-json.md) işlev uygulamasını yapılandırmak için kullanılan dosya. Her işlev, kendi kod dosyası (.fsx) ve bağlama yapılandırma dosyası (function.json) vardır.

Gerekli bağlama uzantıları [sürüm 2.x](functions-versions.md) işlevleri çalışma zamanı içinde tanımlanmıştır `extensions.csproj` dosyasıyla gerçek kitaplık dosyaları `bin` klasör. Yerel olarak geliştirirken gerekir [bağlama uzantıları kaydetme](./functions-bindings-register.md#local-development-azure-functions-core-tools). Azure portalında işlevleri geliştirirken, bu kayıt sizin yerinize yapılır.

## <a name="binding-to-arguments"></a>Bağımsız değişkenler için bağlama
Her bağlama bazı bağımsız değişkenler, içinde ayrıntılı olarak kümesini destekleyen [Azure işlevleri Tetikleyicileri ve bağlamaları Geliştirici Başvurusu](functions-triggers-bindings.md). Örneğin, blob Tetikleyicileri destekler bağımsız değişken bağlamaları biri kullanılarak ifade bir POCO olan bir F# kaydı. Örneğin:

```fsharp
type Item = { Id: string }

let Run(blob: string, output: byref<Item>) =
    let item = { Id = "Some ID" }
    output <- item
```

F# Azure işlevi, bir veya daha fazla bağımsız değişken alır. Biz Azure işlevleri bağımsız değişkenler hakkında konuşurken, diyoruz *giriş* bağımsız değişkenleri ve *çıkış* bağımsız değişkenler. Ne gibi ses giriş bağımsız değişkeni tam olarak olmadığı: giriş için kendi F# Azure işlevi. Bir *çıkış* değişkendir değişebilir veri veya `byref<>` geri veri iletmek için bir yol hizmet veren bağımsız değişken *kullanıma* işlevinizin.

Yukarıdaki örnekte `blob` bir giriş bağımsız değişkeni ve `output` bir çıkış bağımsız değişkeni. Kullanılan bildirim `byref<>` için `output` (eklemenize gerek yoktur `[<Out>]` ek açıklama). Kullanarak bir `byref<>` tür hangi kayıt veya bağımsız değişkenin başvurduğu nesneyi değiştirmek, işlevinizi sağlar.

Olduğunda bir F# kaydı, bir giriş türü olarak kullanıldığında, kayıt tanımı ile işaretlenmelidir `[<CLIMutable>]` kaydı işlevinize geçirmeden önce alanlarını uygun şekilde ayarlamak Azure işlevleri framework izin vermek üzere. Başlık altında `[<CLIMutable>]` ayarlayıcılar kayıt özellikleri oluşturur. Örneğin:

```fsharp
[<CLIMutable>]
type TestObject =
    { SenderName : string
      Greeting : string }

let Run(req: TestObject, log: ILogger) =
    { req with Greeting = sprintf "Hello, %s" req.SenderName }
```

Bir F# sınıfı da kullanılabilir hem giriş ve çıkış bağımsız değişkenler. Bir sınıf için özellikler genellikle alıcılar ve ayarlayıcılar gerekir. Örneğin:

```fsharp
type Item() =
    member val Id = "" with get,set
    member val Text = "" with get,set

let Run(input: string, item: byref<Item>) =
    let result = Item(Id = input, Text = "Hello from F#!")
    item <- result
```

## <a name="logging"></a>Günlüğe kaydetme
Çıkış için oturum, [akış günlükleri](../app-service/troubleshoot-diagnostic-logs.md) içinde F#, işlevinizi türünde bir bağımsız değişken almalıdır [ILogger](https://docs.microsoft.com/dotnet/api/microsoft.extensions.logging.ilogger). Tutarlılık sağlamak için bu bağımsız değişken adlı olan öneririz `log`. Örneğin:

```fsharp
let Run(blob: string, output: byref<string>, log: ILogger) =
    log.LogInformation(sprintf "F# Azure Function processed a blob: %s" blob)
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
İşleviniz kapatma düzgün biçimde işlemesi gerekiyorsa verebilirsiniz bir [ `CancellationToken` ](/dotnet/api/system.threading.cancellationtoken) bağımsız değişken. Bu ile birleştirilebilir `async`, örneğin:

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
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
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
open Microsoft.Extensions.Logging

let Run(req: HttpRequestMessage, log: ILogger) =
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
Destekleyen bir düzenleyici F# derleyici hizmetler ad alanları ve Azure işlevleri otomatik olarak içeren derlemeler duyarlı olmayacak. Bu nedenle, kullanmakta olduğunuz derlemelerini Bul Düzenleyicisi yardımcı olan bir tanıtımlar içeren ve açıkça ad alanlarını açmak için yararlı olabilir. Örneğin:

```fsharp
#if !COMPILED
#I "../../bin/Binaries/WebJobs.Script.Host"
#r "Microsoft.Azure.WebJobs.Host.dll"
#endif

open System
open Microsoft.Azure.WebJobs.Host
open Microsoft.Extensions.Logging

let Run(blob: string, output: byref<string>, log: ILogger) =
    ...
```

Azure işlevleri, kodunuzu yürütüldüğünde, kaynak ile işler `COMPILED` tanımlanan şekilde Düzenleyicisi tanıtımlar göz ardı edilir.

<a name="package"></a>

## <a name="package-management"></a>Paket yönetimi
NuGet paketlerini kullanmak için bir F# işlevi, ekleme bir `project.json` işlevin klasörüne işlevi uygulamanın dosya sisteminde dosya. İşte bir örnek `project.json` ekleyen bir NuGet paketi başvuru dosyası `Microsoft.ProjectOxford.Face` sürüm 1.1.0 yer:

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

Otomatik olarak başvuru derlemeleri, düzenleyicinin etkileşimi geliştirmek için düzenleyici tanıtımlar koymak isteyebilirsiniz F# derleme Hizmetleri.

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
open Microsoft.Extensions.Logging

let Run(timer: TimerInfo, log: ILogger) =
    log.LogInformation("Storage = " + GetEnvironmentVariable("AzureWebJobsStorage"))
    log.LogInformation("Site = " + GetEnvironmentVariable("WEBSITE_SITE_NAME"))
```

## <a name="reusing-fsx-code"></a>.Fsx kodu yeniden kullanma
Diğer kod kullanabileceğiniz `.fsx` dosyaları kullanarak bir `#load` yönergesi. Örneğin:

`run.fsx`

```fsharp
#load "logger.fsx"

let Run(timer: TimerInfo, log: ILogger) =
    mylog log (sprintf "Timer: %s" DateTime.Now.ToString())
```

`logger.fsx`

```fsharp
let mylog(log: ILogger, text: string) =
    log.LogInformation(text);
```

Yollar sağlar `#load` yönerge olan göreli konumunu, `.fsx` dosya.

* `#load "logger.fsx"` işlev klasöründe bir dosya yükler.
* `#load "package\logger.fsx"` bulunan bir dosya yükler `package` işlevi klasöründe.
* `#load "..\shared\mylogger.fsx"` bulunan bir dosya yükler `shared` klasör diğer bir deyişle, işlev klasör ile aynı düzeyde doğrudan altında `wwwroot`.

`#load` Yönergesi yalnızca çalışır `.fsx` (F# betik) dosyaları ile değil `.fs` dosyaları.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [F#Kılavuzu](/dotnet/articles/fsharp/index)
* [Azure işlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Azure işlevlerini test etme](functions-test-a-function.md)
* [Azure işlevlerini ölçeklendirme](functions-scale.md)

