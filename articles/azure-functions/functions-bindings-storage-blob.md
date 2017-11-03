---
title: "Azure işlevleri Blob Storage bağlamaları | Microsoft Docs"
description: "Azure Storage Tetikleyicileri ve bağlamaları Azure işlevlerinde kullanmayı öğrenme."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: aba8976c-6568-4ec7-86f5-410efd6b0fb9
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2017
ms.author: glenga
ms.openlocfilehash: 79b9dbee89a4e33a1768343b9242d6b2b1118355
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2017
---
# <a name="azure-functions-blob-storage-bindings"></a>Azure işlevleri Blob Depolama bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede, yapılandırma ve Azure Blob Depolama bağlamaları Azure işlevlerinde çalışmak açıklanmaktadır. Azure işlevleri destekler tetikleyici, giriş ve Azure Blob Depolama bağlantılarında çıkış. Tüm bağlama kullanılabilir olan özellikler için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları kavramları](functions-triggers-bindings.md).

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> A [yalnızca depolama hesabı blob](../storage/common/storage-create-storage-account.md#blob-storage-accounts) desteklenmiyor. BLOB Depolama Tetikleyicileri ve bağlamaları genel amaçlı depolama hesabı gerektirir. 
> 

<a name="trigger"></a>
<a name="storage-blob-trigger"></a>
## <a name="blob-storage-triggers-and-bindings"></a>BLOB Depolama Tetikleyicileri ve bağlamaları

Azure Blob Depolama tetikleyici kullanarak, yeni veya güncelleştirilmiş bir blob algılandığında işlevi kodunuzu adı verilir. Blob içeriklerini işleve giriş olarak sağlanır.

Bir blob depolama tetikleyici kullanarak tanımlarsınız **tümleştir** işlevleri portalındaki sekmesi. Portal aşağıdaki tanımında oluşturur **bağlamaları** bölümünü *function.json*:

```json
{
    "name": "<The name used to identify the trigger data in your code>",
    "type": "blobTrigger",
    "direction": "in",
    "path": "<container to monitor, and optionally a blob name pattern - see below>",
    "connection": "<Name of app setting - see below>"
}
```

BLOB giriş ve çıkış bağlamalar kullanılarak tanımlanır `blob` bağlama türü olarak:

```json
{
  "name": "<The name used to identify the blob input in your code>",
  "type": "blob",
  "direction": "in", // other supported directions are "inout" and "out"
  "path": "<Path of input blob - see below>",
  "connection":"<Name of app setting - see below>"
},
```

* `path` İfadeleri ve filtre parametrelerini bağlama özelliğini destekler. Bkz: [adı desenleri](#pattern).
* `connection` Özelliği, depolama bağlantı dizesi içeren bir uygulama ayarı adı içermelidir. Standart Düzenleyici'de Azure portalında **tümleştir** sekmesinde bir depolama hesabı seçtiğinizde, bu uygulama ayarı yapılandırır.

> [!NOTE]
> Tüketim plan üzerinde bir blob tetikleyici kullanırken olabilir en fazla 10 dakikalık bir gecikmeyle bir işlev uygulaması boşta geçti sonra yeni BLOB'lar işleme. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Bu ilk gecikmeyi önlemek için aşağıdaki seçeneklerden birini göz önünde bulundurun:
> - Always On özellikli ile bir uygulama hizmeti planını kullan.
> - Başka bir mekanizma, blob adı içeren bir kuyruk iletisi gibi işleme blob tetiklemek için kullanın. Bir örnek için bkz: [sıra tetikleyicisi blob ile girişi bağlamayı](#input-sample).

<a name="pattern"></a>

### <a name="name-patterns"></a>Adı desenleri
Bir blob adı desende belirtebilirsiniz `path` filtre veya bağlama bir ifade olabilir özelliği. Bkz: [ifadeleri ve desenler bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns).

Örneğin, "özgün" dizesiyle başlayan BLOB'ları için filtre uygulamak için aşağıdaki tanımını kullanın. Bu yol adlı bir blob bulur *özgün Blob1.txt* içinde *giriş* kapsayıcı ve değerini `name` işlevi kodda değişkenidir `Blob1`.

```json
"path": "input/original-{name}",
```

Blob dosya adını ve uzantısını ayrı olarak bağlamak için iki desenlerini kullanın. Bu yolu da adlı bir blob bulur *özgün Blob1.txt*, değerini `blobname` ve `blobextension` değişkenleri işlevi kodda *özgün Blob1* ve *txt*.

```json
"path": "input/{blobname}.{blobextension}",
```

Dosya uzantısı için sabit bir değer kullanarak BLOB dosya türünü kısıtlayabilirsiniz. Örneğin, yalnızca .png dosyalarda tetiklemek için şu biçimi kullanın:

```json
"path": "samples/{name}.png",
```

Süslü ayraçlar adı desenleri bulunan özel karakterleri var. Süslü ayraçlar sahip blob adlarını belirtmek için iki ayraçlar kullanılmasını küme ayraçları kaçış. Aşağıdaki örnek adlı bir blob bulur *{20140101}-soundfile.mp3* içinde *görüntüleri* kapsayıcı ve `name` işlevi kodda değişken değeri *soundfile.mp3*. 

```json
"path": "images/{{20140101}}-{name}",
```

### <a name="trigger-metadata"></a>Tetikleyici meta verileri

Blob tetikleyici çeşitli meta veri özelliklerini sağlar. Bu özellikler, diğer bağlamaları bağlamaları ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Bu değerler olarak aynı semantiklerine sahip [CloudBlob](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet).

- **BlobTrigger**. `string` yazın. Tetikleyici blob yolu
- **URI**. `System.Uri` yazın. Birincil konumu için blob'un URI.
- **Özellikler**. `Microsoft.WindowsAzure.Storage.Blob.BlobProperties` yazın. Blob'un Sistem Özellikleri.
- **Meta veri**. `IDictionary<string,string>` yazın. Kullanıcı tanımlı meta veriler blob'a.

<a name="receipts"></a>

### <a name="blob-receipts"></a>BLOB giriş
Azure işlevleri çalışma zamanı yok blob Tetik işlevi için aynı yeni veya güncelleştirilmiş blob birden çok kez çağrılır sağlar. Belirtilen blob sürümü işlediğinde tutar belirlemek için *blob giriş*.

Azure işlevleri depoları Alındılar adlı bir kapsayıcıda blob *azure Web işleri konakları* işlev uygulamanız için Azure depolama hesabında (uygulama ayarı tarafından tanımlanan `AzureWebJobsStorage`). Bir blob giriş aşağıdaki bilgileri içerir:

* Tetiklenen işlevi ("*&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*", örneğin:"MyFunctionApp.Functions.CopyBlob")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (örneğin bir blob sürüm tanıtıcısını: "0x8D1DC6E70A277EF")

Bir blob yeniden işlemeyerek zorlamak için bu blobundan blob giriş silme *azure Web işleri konakları* kapsayıcı el ile.

<a name="poison"></a>

### <a name="handling-poison-blobs"></a>Zararlı BLOB'ları işleme
Belirli bir blob için bir blob Tetik işlevi başarısız olduğunda, Azure işlevleri, varsayılan olarak 5 kez toplam işlev yeniden dener. 

Tüm 5 deneme başarısız olursa, Azure işlevleri adlı bir depolama kuyruğuna bir ileti ekler *webjobs blobtrigger poison*. Kuyruk iletisini zararlı BLOB'lar için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*)
* BlobType ("BlockBlob" veya "PageBlob")
* Kapsayıcı adı
* BlobName
* ETag (örneğin bir blob sürüm tanıtıcısını: "0x8D1DC6E70A277EF")

### <a name="blob-polling-for-large-containers"></a>Blob büyük kapsayıcıları için yoklama
İzlenmekte olan blob kapsayıcısı 10. 000'den fazla BLOB'ları içeriyorsa, işlevleri çalışma zamanı taramaları için yeni veya değiştirilmiş BLOB'lar izlemek için günlük dosyalarını. Bu işlem, gerçek zamanlı değildir. Blob oluşturulduktan sonra bir işlev birkaç dakika kadar veya daha uzun tetiklenen değil. Ayrıca, [depolama günlüklerine "en iyi çaba" üzerinde oluşturulan](/rest/api/storageservices/About-Storage-Analytics-Logging) temel. Tüm olayları yakalanır garantisi yoktur. Bazı koşullarda günlükleri eksik olabilir. Daha hızlı veya daha güvenilir blob işleme gerekiyorsa oluşturma göz önünde bulundurun bir [kuyruk iletisi](../storage/queues/storage-dotnet-how-to-use-queues.md) blob oluşturduğunuzda. Ardından, bir [sıra tetikleyici](functions-bindings-storage-queue.md) blob işlemek için bir blob tetikleyici yerine.

<a name="triggerusage"></a>

## <a name="using-a-blob-trigger-and-input-binding"></a>Blob tetikleyici kullanılarak ve giriş bağlama
.NET işlevlerde gibi bir yöntem parametresi kullanılarak blob veri erişim `Stream paramName`. Burada, `paramName` , belirtilen değer [tetikleyici yapılandırma](#trigger). Node.js işlevlerde kullanarak giriş blob veri erişim `context.bindings.<name>`.

.NET içinde aşağıdaki listede türlerinden herhangi birini bağlayabilirsiniz. Giriş bağlama olarak kullandıysanız, bu türlerinden bazıları gerektiren bir `inout` yönde bağlama *function.json*. Gelişmiş Düzenleyicisi'ni kullanmanız gerekir böylece bu yönünü standart Düzenleyicisi tarafından desteklenmiyor.

* `TextReader`
* `Stream`
* `ICloudBlob`("ınout" bağlama yönü gerektirir)
* `CloudBlockBlob`("ınout" bağlama yönü gerektirir)
* `CloudPageBlob`("ınout" bağlama yönü gerektirir)
* `CloudAppendBlob`("ınout" bağlama yönü gerektirir)

Metin BLOB'ları beklenen, bir .NET de bağlayabilirsiniz `string` türü. Bu, yalnızca tüm blob içeriklerini belleğe yüklenen olarak blob boyutu, küçükse önerilir. Genellikle, kullanılması tercih edilir bir `Stream` veya `CloudBlockBlob` türü.

## <a name="trigger-sample"></a>Tetikleyici örnek
Bir blob depolama tetikleyici tanımlar aşağıdaki function.json olduğunu varsayalım:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems",
            "connection":"MyStorageAccount"
        }
    ]
}
```

İzlenen kapsayıcıya eklenen her bir blob içeriğini günlüklerini dile özgü örneğine bakın.

* [C#](#triggercsharp)
* [Node.js](#triggernodejs)

<a name="triggercsharp"></a>

### <a name="blob-trigger-examples-in-c"></a>BLOB tetikleyici örnekler C# #

```cs
// Blob trigger sample using a Stream binding
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

```cs
// Blob trigger binding to a CloudBlockBlob
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

<a name="triggernodejs"></a>

### <a name="trigger-example-in-nodejs"></a>Tetikleyici örnekte Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```
<a name="outputusage"></a>
<a name="storage-blob-output-binding"></a>

## <a name="using-a-blob-output-binding"></a>Bir blob kullanarak çıktıyı bağlama

.NET işlevlerde ya da kullanım gereken bir `out string` işlev imzası veya kullanım türlerinden birini aşağıdaki listede parametresi. Node.js işlevlerde çıkış blob kullanarak erişim `context.bindings.<name>`.

.NET işlevlerde şu türlerden birine çıkarabilirsiniz:

* `out string`
* `TextWriter`
* `Stream`
* `CloudBlobStream`
* `ICloudBlob`
* `CloudBlockBlob` 
* `CloudPageBlob` 

<a name="input-sample"></a>

## <a name="queue-trigger-with-blob-input-and-output-sample"></a>Sıra tetikleyiciyle blob girdi ve çıktı örneği
Aşağıdaki function.json olduğunu varsayalım tanımlayan bir [kuyruk depolama tetikleyici](functions-bindings-storage-queue.md)bir blob depolama giriş ve bir blob depolama çıkış. Kullanımına dikkat edin `queueTrigger` meta veri özelliği. blob giriş ve çıkış `path` özellikleri:

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
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnection",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
``` 

Giriş blob çıkış blob kopyalar dile özgü örneğine bakın.

* [C#](#incsharp)
* [Node.js](#innodejs)

<a name="incsharp"></a>

### <a name="blob-binding-example-in-c"></a>BLOB bağlama örnek C# #

```cs
// Copy blob from input to output, based on a queue trigger
public static void Run(string myQueueItem, Stream myInputBlob, out Stream myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

<a name="innodejs"></a>

### <a name="blob-binding-example-in-nodejs"></a>BLOB bağlama Node.js örneğinde

```javascript
// Copy blob from input to output, based on a queue trigger
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

## <a name="next-steps"></a>Sonraki adımlar
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

