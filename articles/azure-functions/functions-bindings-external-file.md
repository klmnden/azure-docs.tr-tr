---
title: Azure işlevleri (Deneysel) için dış dosya bağlamaları
description: Azure işlevleri'nde dış dosya bağlamaları kullanma
services: functions
author: craigshoemaker
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 11/27/2017
ms.author: cshoe
ms.openlocfilehash: 765eab8dfc1163c4d9e0337a1af840278ae1a82c
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/17/2018
ms.locfileid: "53546275"
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure işlevleri dış dosya bağlamaları (Deneysel)
Bu makalede, Azure işlevleri'nde (örneğin, Dropbox veya Google Drive gibi) farklı SaaS sağlayıcıları dosyalarını işlemek gösterilmektedir. Azure işlevleri destekler tetiklemek, girdi ve çıktı bağlaması için dış dosyaları. Bu bağlamaları SaaS sağlayıcıları için API bağlantıları oluşturma ve işlev uygulamanızın kaynak grubundan mevcut API bağlantıları kullanın.

> [!IMPORTANT]
> Dış dosya bağlamaları, Deneysel ve genel kullanıma (GA) durumu hiçbir zaman ulaşın. Bunlar yalnızca Azure'da eklenir 1.x işlevleri ve Azure işlevleri'ne eklenecek planlanmamaktadır 2.x. SaaS sağlayıcıları verilere erişim gerektiren senaryolar için kullanmayı [işlevlerini çağıran logic apps'i](functions-twitter-email.md). Bkz: [Logic Apps dosya sistemi Bağlayıcısı](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Kullanılabilir dosya bağlantıları

|Bağlayıcı|Tetikleyici|Girdi|Çıktı|
|:-----|:---:|:---:|:---:|
|[Box](https://www.box.com)|x|x|x
|[Dropbox](https://www.dropbox.com)|x|x|x
|[FTP](https://docs.microsoft.com/azure/app-service/deploy-ftp)|x|x|x
|[OneDrive](https://onedrive.live.com)|x|x|x
|[OneDrive İş](https://onedrive.live.com/about/business/)|x|x|x
|[SFTP](https://docs.microsoft.com/azure/connectors/connectors-create-api-sftp)|x|x|x
|[Google Drive'a](https://www.google.com/drive/)||x|x|

> [!NOTE]
> Dış dosya bağlantıları da kullanılabilir [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Tetikleyici

Dış dosya tetikleyici uzak bir klasör izlemek ve değişiklik algılandığında, işlev kodunuzun çalıştırmanıza olanak tanır.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C# betiği](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir dış dosya tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi izlenen klasöre eklenen her bir dosyanın içeriğini günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

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

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir dış dosya tetikleyicisi bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi izlenen klasöre eklenen her bir dosyanın içeriğini günlüğe kaydeder.

Veri bağlama işte *function.json* dosyası:

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

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js File trigger function processed', context.bindings.myFile);
    context.done();
};
```

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**type** | Ayarlanmalıdır `apiHubFileTrigger`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | İşlev kodunu olay öğeyi temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında tümleştir UI içinde bir bağlantı eklediğinizde, uygulama ayarı otomatik olarak oluşturulur.|
|**Yolu** | İzlemek için klasör ve isteğe bağlı olarak bir adı deseni.|

### <a name="name-patterns"></a>Adı modelleri

Bir dosya adı deseni de belirtebilirsiniz `path` özelliği. Başvurulan klasörü, SaaS sağlayıcısı mevcut olmalıdır.

Örnekler:

```json
"path": "input/original-{name}",
```

Bu yolu adlı bir dosya bulur *özgün Dosya1.ref* içinde *giriş* klasörü ve değerini `name` işlev kodunu bir değişkende olacaktır `File1.txt`.

Bir örnek daha:

```json
"path": "input/{filename}.{fileextension}",
```

Bu yolu da adlı bir dosya bulur *özgün Dosya1.ref*, değeri `filename` ve `fileextension` işlevinin kodundaki değişkenleri olacaktır *özgün dosya1* ve *txt* .

Dosya uzantısı için sabit bir değer kullanarak dosyaları dosya türünü kısıtlayabilirsiniz. Örneğin:

```json
"path": "samples/{name}.png",
```

Bu durumda, yalnızca *.png* dosyalar *örnekleri* klasör işlevi tetikleyin.

Küme ayracı adı desenleri özel karakterler şunlardır. Küme ayraçları içinde ada sahip dosya adlarını belirtmek için küme ayracı çift.
Örneğin:

```json
"path": "images/{{20140101}}-{name}",
```

Bu yolu adlı bir dosya bulur  *{20140101}-soundfile.mp3* içinde *görüntüleri* klasöründe ve `name` işlev kodunu değişken değeri olacak *soundfile.mp3*.

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# işlevleri'nde giriş dosya verileri için işlev imzasında gibi adlandırılmış bir parametre kullanarak bağladığınız `<T> <name>`.
Burada `T` , verileri seri durumdan çıkarılacak istediğiniz veri türünü olduğu ve `paramName` içinde belirtilen ad [JSON tetikleme](#trigger). Node.js işlevleri'nde giriş dosyası kullanarak veri erişim `context.bindings.<name>`.

Dosya şu türlerden birini seri durumdan çıkarılabiliyorsa:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON ile seri hale getirilmiş dosya verileri için kullanışlıdır.
  Özel bir giriş türü bildirirseniz (örneğin `FooType`), Azure işlevleri, belirtilen türe JSON verilerini seri durumdan dener.
* Dize - metin dosya verileri için yararlıdır.

C# işlevleri, şu türlerden birine de bağlayabilirsiniz ve İşlevler çalışma zamanı bu türü kullanarak dosya verilerini seri durumdan dener:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

<!--- ## Trigger - file receipts
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

## <a name="trigger---poison-files"></a>Tetikleyici - zehirli dosyaları

Bir dış dosya tetikleyici işlevi başarısız olduğunda, Azure işlevleri, işlev en fazla 5 kez (ilk denemede dahil) varsayılan olarak belirli bir dosya için yeniden dener.
İşlevleri 5 tüm denemeler başarısız olursa, adlı bir depolama kuyruğuna bir ileti ekler *webjobs apihubtrigger poison*. Kuyruk iletisi zehirli dosyaları için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlev uygulaması adı >*. İşlevler.  *&lt;işlev adı >*)
* fileType
* KlasörAdı
* Dosya adı
* ETag (örneğin, bir dosya sürüm tanımlayıcısı: "0x8D1DC6E70A277EF")

## <a name="input"></a>Girdi

Azure harici dosya giriş bağlama, bir dış işlevinizi klasöründeki bir dosyayı kullanmak sağlar.

## <a name="input---example"></a>Giriş - örnek

Dile özgü örneğe bakın:

* [C# betiği](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Giriş - C# betiği örneği

Aşağıdaki örnek, giriş ve çıkış bağlamaları, dış dosya gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlev bir giriş dosyası için bir çıktı dosyası kopyalar.

Veri bağlama işte *function.json* dosyası:

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

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

Aşağıdaki örnek, giriş ve çıkış bağlamaları, dış dosya gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlev bir giriş dosyası için bir çıktı dosyası kopyalar.

Veri bağlama işte *function.json* dosyası:

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

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**type** | Ayarlanmalıdır `apiHubFile`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | İşlev kodunu olay öğeyi temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında tümleştir UI içinde bir bağlantı eklediğinizde, uygulama ayarı otomatik olarak oluşturulur.|
|**Yolu** | Klasör adı ve dosya adını içermelidir. Örneğin, bir [kuyruk tetikleyicisi](functions-bindings-storage-queue.md) işlevinizde kullanabileceğiniz `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici message içinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

## <a name="input---usage"></a>Giriş - kullanım

C# işlevleri'nde giriş dosya verileri için işlev imzasında gibi adlandırılmış bir parametre kullanarak bağladığınız `<T> <name>`. `T` veri türü, verileri seri durumdan çıkarılacak istediğiniz olduğu ve `name` giriş bağlamasında belirtilen adı. Node.js işlevleri'nde giriş dosyası kullanarak veri erişim `context.bindings.<name>`.

Dosya şu türlerden birini seri durumdan çıkarılabiliyorsa:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON ile seri hale getirilmiş dosya verileri için kullanışlıdır.
  Özel bir giriş türü bildirirseniz (örneğin `InputType`), Azure işlevleri, belirtilen türe JSON verilerini seri durumdan dener.
* Dize - metin dosya verileri için yararlıdır.

C# işlevleri, şu türlerden birine de bağlayabilirsiniz ve İşlevler çalışma zamanı bu türü kullanarak dosya verilerini seri durumdan dener:

* `string`
* `byte[]`
* `Stream`
* `StreamReader`
* `TextReader`

## <a name="output"></a>Çıktı

Azure harici dosyasının çıkış bağlaması işlevinizi dış bir klasörde dosyaları yazmanıza olanak sağlar.

## <a name="output---example"></a>Çıkış - örnek

Bkz: [giriş bağlama örnek](#input---example).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**type** | Ayarlanmalıdır `apiHubFile`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | Ayarlanmalıdır `out`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** | İşlev kodunu olay öğeyi temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında tümleştir UI içinde bir bağlantı eklediğinizde, uygulama ayarı otomatik olarak oluşturulur.|
|**Yolu** | Klasör adı ve dosya adını içermelidir. Örneğin, bir [kuyruk tetikleyicisi](functions-bindings-storage-queue.md) işlevinizde kullanabileceğiniz `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici message içinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

## <a name="output---usage"></a>Çıkış - kullanım

C# işlevleri'nde, çıktı dosyasına adlandırılmış kullanarak bağlama `out` , işlev imzası parametresinde ister `out <T> <name>`burada `T` veri türü, verileri seri hale getirmek istediğiniz olduğu ve `name` , belirtilen adı Çıkış bağlaması. Node.js işlevleri kullanarak çıkış dosyasını erişim `context.bindings.<name>`.

Şu türlerden birini kullanarak çıktı dosyasına yazabilirsiniz:

* Tüm [nesne](https://msdn.microsoft.com/library/system.object.aspx) - JSON seri hale getirme için kullanışlıdır.
  Özel çıkış türü bildirirseniz (örneğin `out OutputType paramName`), nesne JSON'a seri hale getirmek Azure işlevleri çalışır. Çıkış parametresi null ise, işlev işlevler çalışma zamanı bir dosya null bir nesne oluşturur.
* Dize - (`out string paramName`) metin dosya verileri için kullanışlıdır. İşlevler çalışma zamanı yalnızca işlev dize parametresi null olmayan bir dosya oluşturur.

C# işlevleri şu türlerden birine de çıkarabilirsiniz:

* `TextWriter`
* `Stream`
* `CloudFileStream`
* `ICloudFile`
* `CloudBlockFile`
* `CloudPageFile`

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
