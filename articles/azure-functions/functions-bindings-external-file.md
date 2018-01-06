---
title: "Azure işlevleri (Deneysel) için dış dosya bağlamaları"
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
ms.date: 11/27/2017
ms.author: alkarche
ms.openlocfilehash: 4e9c2c336df465d7488de84bd2a02cc5d9e42f30
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="azure-functions-external-file-bindings-experimental"></a>Azure işlevleri dış dosya bağlamalarını (Deneysel)
Bu makalede, Azure işlevleri (örneğin, Dropbox veya Google sürücü) farklı SaaS sağlayıcı dosyalarını işlemek gösterilmiştir. Tetiklemek, giriş ve dış dosyalar için bağlamaları çıktı Azure işlevleri destekler. Bu bağlamaların SaaS sağlayıcısı API bağlantıları oluşturma veya varolan API bağlantıları işlevi uygulamanızın kaynak grubundan kullanın.

> [!IMPORTANT]
> Dış dosya bağlamalarını Deneysel ve hiçbir zaman genellikle kullanılabilir (GA) durumuna ulaşmasını. Yalnızca Azure içinde bulunan 1.x işlevler ve bunları Azure işlevleri eklemek için herhangi bir plan yok 2.x. SaaS sağlayıcıları veri erişimi gerektiren senaryolar için kullanmayı [işlevlerini çağırma logic apps](functions-twitter-email.md). Bkz: [Logic Apps dosya sistemi bağlayıcı](../logic-apps/logic-apps-using-file-connector.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="available-file-connections"></a>Kullanılabilir bir dosya bağlantıları

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
> Dış dosya bağlantıları da kullanılabilir [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).

## <a name="trigger"></a>Tetikleyici

Dış dosya tetikleyici uzak bir klasör izlemenizi ve değişiklik algılandığında işlevi kodunuzu çalıştırmak sağlar.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C# betiği](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama bir dış dosya tetikleyicisi gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev izlenen klasöre eklenen her dosyanın içeriğini günlüğe kaydeder.

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myFile, TraceWriter log)
{
    log.Info($"C# File trigger function processed: {myFile}");
}
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama bir dış dosya tetikleyicisi gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev izlenen klasöre eklenen her dosyanın içeriğini günlüğe kaydeder.

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

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**türü** | ayarlanmalıdır `apiHubFileTrigger`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında UI tümleştir bir bağlantı eklediğinizde uygulama ayarı otomatik olarak oluşturulur.|
|**yol** | İzlemek için klasör ve isteğe bağlı olarak bir adı deseni.|

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

## <a name="trigger---usage"></a>Tetikleyici - kullanım

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

## <a name="trigger---poison-files"></a>Tetikleyici - zararlı dosyaları

Bir dış dosya Tetik işlevi başarısız olduğunda, Azure işlevleri, işlevi en fazla 5 kez (ilk denemede dahil) varsayılan olarak belirli bir dosya için yeniden dener.
İşlevler tüm 5 deneme başarısız olursa adlı bir depolama kuyruğuna bir ileti ekler *webjobs apihubtrigger poison*. Kuyruk iletisini zararlı dosyaları için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*)
* Dosya türü
* KlasörAdı
* Dosya adı
* ETag (örneğin, bir dosya sürümü tanımlayıcısı: "0x8D1DC6E70A277EF")

## <a name="input"></a>Girdi

Azure dış dosya giriş bağlaması, dış işlevinizi klasöründeki bir dosya kullanmanıza olanak sağlar.

## <a name="input---example"></a>Girişi - örnek

Dile özgü örneğe bakın:

* [C# betiği](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-script-example"></a>Giriş - C# kod örneği

Aşağıdaki örnek, giriş ve çıkış bağlamaları dış dosya gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev bir çıktı dosyasına bir giriş dosyası kopyalar.

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myQueueItem, string myInputFile, out string myOutputFile, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputFile = myInputFile;
}
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

Aşağıdaki örnek, giriş ve çıkış bağlamaları dış dosya gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev bir çıktı dosyasına bir giriş dosyası kopyalar.

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

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**türü** | ayarlanmalıdır `apiHubFile`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında UI tümleştir bir bağlantı eklediğinizde uygulama ayarı otomatik olarak oluşturulur.|
|**yol** | Klasör adı ve dosya adını içermelidir. Örneğin, bir [sıra tetikleyici](functions-bindings-storage-queue.md) işlevinizi, kullandığınız `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici iletisinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

## <a name="input---usage"></a>Giriş - kullanım

C# işlevlerde, girdi dosyası veri adlandırılmış bir parametre gibi işlevi imzanız kullanarak bağladığınız `<T> <name>`. `T`veri türü, verileri seri durumdan istediğiniz olduğunda ve `name` giriş bağlamasında belirtilen adı. Giriş dosyası kullanarak veri erişim node.js işlevlerde `context.bindings.<name>`.

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

## <a name="output"></a>Çıktı

Azure dış dosya çıktı bağlama dosyaları işlevinizi dış bir klasörde yazma olanak sağlar.

## <a name="output---example"></a>Çıktı - örnek

Bkz: [giriş bağlaması örnek](#input---example).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya.

|Function.JSON özelliği | Açıklama|
|---------|---------|----------------------|
|**türü** | ayarlanmalıdır `apiHubFile`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**yönü** | ayarlanmalıdır `out`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. |
|**adı** | İşlev kodu olay öğesinde temsil eden değişken adı. | 
|**bağlantı**| Bağlantı dizesi depolar uygulama ayarı tanımlar. Azure portalında UI tümleştir bir bağlantı eklediğinizde uygulama ayarı otomatik olarak oluşturulur.|
|**yol** | Klasör adı ve dosya adını içermelidir. Örneğin, bir [sıra tetikleyici](functions-bindings-storage-queue.md) işlevinizi, kullandığınız `"path": "samples-workitems/{queueTrigger}"` bir dosyaya işaret edecek şekilde `samples-workitems` tetikleyici iletisinde belirtilen dosya adıyla eşleşen bir ada sahip klasör.   

## <a name="output---usage"></a>Çıktı - kullanım

C# işlevlerde, çıktı dosyasına adlandırılmış kullanarak bağladığınız `out` işlevi imzanız parametresinde ister `out <T> <name>`, burada `T` veri türü, verileri seri hale getirmek istediğiniz olduğunda ve `name` , belirtilen adı Çıktı bağlama. Çıkış dosyası kullanarak erişim node.js işlevlerde `context.bindings.<name>`.

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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
