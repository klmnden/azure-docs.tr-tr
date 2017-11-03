---
title: "Azure işlevleri dış dosya bağlamalarını (Önizleme) | Microsoft Docs"
description: "Dış dosya bağlamaları Azure işlevlerini kullanma"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: 7e3b396d290212d3875385521bd7ae92da196b95
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-external-file-bindings-preview"></a>Azure işlevleri dış dosya bağlamalarını (Önizleme)
Bu makalede sağlayıcılardan farklı SaaS (örneğin, OneDrive, Dropbox) dosyaları yönetmek yerleşik bağlamalar kullanılarak işlevinizi içinde gösterilmiştir. Tetiklemek, giriş ve dış dosya için bağlamaları çıktı Azure işlevleri destekler.

Bu bağlama SaaS sağlayıcısı API bağlantılar oluşturur veya mevcut API bağlantıları işlevi uygulamanızın kaynak grubundan kullanır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="supported-file-connections"></a>Desteklenen dosya bağlantıları

|Bağlayıcı|Tetikleyici|Girdi|Çıktı|
|:-----|:---:|:---:|:---:|
|[Kutusu](https://www.box.com)|x|x|x
|[Açılan kutu](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/app-service-deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive İş](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google sürücü](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Dış dosya bağlantıları da kullanılabilir [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

## <a name="external-file-trigger-binding"></a>Dış dosya tetiklemek bağlama

Azure dış dosya tetikleyici uzak bir klasör izlemenizi ve değişiklik algılandığında işlevi kodunuzu çalıştırmak sağlar.

Dış dosya tetikleyici aşağıdaki JSON nesneleri kullanan `bindings` function.json dizisi

```json
{
  "type": "apiHubFileTrigger",
  "name": "<Name of input parameter in function signature>",
  "direction": "in",
  "path": "<folder to monitor, and optionally a name pattern - see below>",
  "connection": "<name of external file connection - see above>"
}
```
<!---
See one of the following subheadings for more information:

* [Name patterns](#pattern)
* [File receipts](#receipts)
* [Handling poison files](#poison)
--->

<a name="pattern"></a>

### <a name="name-patterns"></a>Adı desenleri
Bir dosya adı deseni içinde belirttiğiniz `path` özelliği. Başvurulan klasörü SaaS sağlayıcı mevcut olmalıdır.
Örnekler:

```json
"path": "input/original-{name}",
```

Bu yol adlı bir dosyayı bulur *özgün Dosya1.ref* içinde *giriş* klasörü ve değerini `name` işlev kodu değişkende olacaktır `File1.txt`.

Bir örnek daha:

```json
"path": "input/{filename}.{fileextension}",
```

Bu yolu da adlı bir dosyayı bulur *özgün Dosya1.ref*, değerini `filename` ve `fileextension` işlev kodu değişkenleri olacaktır *özgün dosya1* ve *txt* .

Dosya uzantısı için sabit bir değer kullanarak dosyaları dosya türünü kısıtlayabilirsiniz. Örneğin:

```json
"path": "samples/{name}.png",
```

Bu durumda, yalnızca *.png* dosyalar *örnekleri* klasörü tetiklemek işlevi.

Süslü ayraçlar adı desenleri bulunan özel karakterleri var. Süslü ayraçlar içinde ada sahip dosya adlarını belirtmek için süslü ayraçlar çift.
Örneğin:

```json
"path": "images/{{20140101}}-{name}",
```

Bu yol adlı bir dosyayı bulur *{20140101}-soundfile.mp3* içinde *görüntüleri* klasörünü ve `name` işlev kodu değişken değerinin olacaktır *soundfile.mp3*.

<a name="receipts"></a>

<!--- ### File receipts
The Azure Functions runtime makes sure that no external file trigger function gets called more than once for the same new or updated file.
It does so by maintaining *file receipts* to determine if a given file version has been processed.

File receipts are stored in a folder named *azure-webjobs-hosts* in the Azure storage account for your function app
(specified by the `AzureWebJobsStorage` app setting). A file receipt has the following information:

* The triggered function ("*&lt;function app name>*.Functions.*&lt;function name>*", for example: "functionsf74b96f7.Functions.CopyFile")
* The folder name
* The file type ("BlockFile" or "PageFile")
* The file name
* The ETag (a file version identifier, for example: "0x8D1DC6E70A277EF")

To force reprocessing of a file, delete the file receipt for that file from the *azure-webjobs-hosts* folder manually.
--->
<a name="poison"></a>

### <a name="handling-poison-files"></a>Zararlı dosyaları işleme
Bir dış dosya Tetik işlevi başarısız olduğunda, Azure işlevleri, işlevi en fazla 5 kez (ilk denemede dahil) varsayılan olarak belirli bir dosya için yeniden dener.
İşlevler tüm 5 deneme başarısız olursa adlı bir depolama kuyruğuna bir ileti ekler *webjobs apihubtrigger poison*. Kuyruk iletisini zararlı dosyaları için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*)
* Dosya türü
* KlasörAdı
* Dosya adı
* ETag (örneğin, bir dosya sürümü tanımlayıcısı: "0x8D1DC6E70A277EF")


<a name="triggerusage"></a>

## <a name="trigger-usage"></a>Tetikleyici kullanımı
C# işlevlerde, girdi dosyası veri adlandırılmış bir parametre gibi işlevi imzanız kullanarak bağladığınız `<T> <name>`.
Burada `T` veri türü, verileri seri durumdan istediğiniz olduğunda ve `paramName` , belirtilen adı [JSON tetiklemek](#trigger). Giriş dosyası kullanarak veri erişim node.js işlevlerde `context.bindings.<name>`.

Dosya türlerinden herhangi birinde aşağıdaki seri durumdan çıkarılabiliyorsa:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON serileştirilmiş dosya verileri için kullanışlıdır.
  Özel bir giriş türü bildirirseniz (örneğin `FooType`), Azure işlevleri, belirtilen türe JSON verilerini seri durumdan dener.
* String - metin dosya verileri için yararlıdır.

C# işlevleri, şu türlerden birine de bağlayabilirsiniz ve işlevleri çalışma zamanı bu türünü kullanarak dosya verileri seri durumdan dener:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="trigger-sample"></a>Tetikleyici örnek
Bir dış dosya tetikleyicisi tanımlayan aşağıdaki function.json olduğunu varsayalım:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myFile",
            "type": "apiHubFileTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection": "<name of external file connection>"
        }
    ]
}
```

İzlenen klasöre eklenen her dosyanın içeriğini günlüklerini dile özgü örneğe bakın.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="trigger-usage-in-c"></a>C# tetikleyici kullanımı #

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

<!--
<a name="triggerfsharp"></a>
### Trigger usage in F# ##
```fsharp

```
-->

<a name="triggernodejs"></a>

### <a name="trigger-usage-in-nodejs"></a>Node.js tetikleyici kullanımı

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

<a name="input"></a>

## <a name="external-file-input-binding"></a>Dış dosya bağlama giriş
Azure dış dosya giriş bağlaması, dış işlevinizi klasöründeki bir dosya kullanmanıza olanak sağlar.

Bir işlev dış dosya girdisi aşağıdaki JSON nesneleri kullanır `bindings` function.json dizisi:

```json
{
  "name": "<Name of input parameter in function signature>",
  "type": "apiHubFile",
  "direction": "in",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
},
```

Şunlara dikkat edin:

* `path`Klasör adı ve dosya adını içermelidir. Örneğin, bir [sıra tetikleyici](functions-bindings-storage-queue.md) işlevinizi, kullandığınız `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici iletisinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

<a name="inputusage"></a>

## <a name="input-usage"></a>Giriş kullanımı
C# işlevlerde, girdi dosyası veri adlandırılmış bir parametre gibi işlevi imzanız kullanarak bağladığınız `<T> <name>`.
Burada `T` veri türü, verileri seri durumdan istediğiniz olduğunda ve `paramName` , belirtilen adı [bağlama giriş](#input). Giriş dosyası kullanarak veri erişim node.js işlevlerde `context.bindings.<name>`.

Dosya türlerinden herhangi birinde aşağıdaki seri durumdan çıkarılabiliyorsa:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON serileştirilmiş dosya verileri için kullanışlıdır.
  Özel bir giriş türü bildirirseniz (örneğin `InputType`), Azure işlevleri, belirtilen türe JSON verilerini seri durumdan dener.
* String - metin dosya verileri için yararlıdır.

C# işlevleri, şu türlerden birine de bağlayabilirsiniz ve işlevleri çalışma zamanı bu türünü kullanarak dosya verileri seri durumdan dener:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`


<a name="output"></a>

## <a name="external-file-output-binding"></a>Çıkış dış dosyası bağlama
Azure dış dosya çıktı bağlama dosyaları işlevinizi dış bir klasörde yazma olanak sağlar.

Bir işlev aşağıdaki JSON nesneleri kullanan çıktısı dış dosya `bindings` function.json dizisi:

```json
{
  "name": "<Name of output parameter in function signature>",
  "type": "apiHubFile",
  "direction": "out",
  "path": "<Path of input file - see below>",
  "connection": "<name of external file connection>"
}
```

Şunlara dikkat edin:

* `path`Klasör adı ve yazmak için dosya adını içermelidir. Örneğin, bir [sıra tetikleyici](functions-bindings-storage-queue.md) işlevinizi, kullandığınız `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici iletisinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

<a name="outputusage"></a>

## <a name="output-usage"></a>Çıktı kullanımı
C# işlevlerde, çıktı dosyasına adlandırılmış kullanarak bağladığınız `out` işlevi imzanız parametresinde ister `out <T> <name>`, burada `T` veri türü, verileri seri hale getirmek istediğiniz olduğunda ve `paramName` , belirtilen adı [bağlama çıktı](#output). Çıkış dosyası kullanarak erişim node.js işlevlerde `context.bindings.<name>`.

Şu türlerden birini kullanarak çıktı dosyasına yazabilirsiniz:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON serileştirmesi için kullanışlıdır.
  Özel çıkış türü bildirirseniz (örneğin `out OutputType paramName`), JSON içinde nesneyi serileştirmek Azure işlevleri çalışır. Çıkış parametresi null ise, işlev çıktığında işlevleri çalışma zamanı null bir nesne bir dosya oluşturur.
* String - (`out string paramName`) metin dosya verileri için kullanışlıdır. işlev çıktığında yalnızca dize parametresi null olmayan ise işlevleri çalışma zamanı bir dosya oluşturur.

C# işlevlerde şu türlerden birine de çıkarabilirsiniz:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

<a name="outputsample"></a>

<a name="sample"></a>

## <a name="input--output-sample"></a>Giriş + çıktı örneği
Aşağıdaki function.json olduğunu varsayalım tanımlayan bir [depolama kuyruğu tetikleyici](functions-bindings-storage-queue.md)dış dosyası giriş ve çıkış bir dış dosyası:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "<name of external file connection>",
      "direction": "in"
    },
    {
      "name": "myOutputFile",
      "type": "apiHubFile",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "<name of external file connection>",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

Giriş dosyası çıktı dosyasına kopyalar dile özgü örneğe bakın.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="usage-in-c"></a>C# kullanımı #

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

<!--
<a name="infsharp"></a>
### Input usage in F# ##
```fsharp

```
-->

<a name="innodejs"></a>

### <a name="usage-in-nodejs"></a>Node.js kullanımı

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
