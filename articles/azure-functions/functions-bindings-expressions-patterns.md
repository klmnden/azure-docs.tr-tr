---
title: Azure işlevleri bağlamaları ifadeleri ve desenleri
description: Yaygın düzenlerini esas alarak farklı Azure işlevleri bağlama ifadeleri oluşturmayı öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: b9a44bd058e6148c6210c5e3be93745d18d8cb74
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480419"
---
# <a name="azure-functions-binding-expression-patterns"></a>Azure işlevleri bağlama ifade desenleri

En güçlü özelliklerinden biri [Tetikleyicileri ve bağlamaları](./functions-triggers-bindings.md) olduğu *ifadeleri bağlama*. İçinde *function.json* dosya ve işlev parametrelerini ve kod, çeşitli kaynaklardan gelen değerlerine çözmek ifadeleri kullanabilirsiniz.

Çoğu ifadenin ayraç içindeki sarmalama tarafından tanımlanır. Örneğin, bir kuyruğu tetikleme işlevi içinde `{queueTrigger}` kuyruğa ileti metni giderir. Varsa `path` özellik bir blob için çıktı bağlaması `container/{queueTrigger}` ve bir kuyruk iletisi ile tetiklenen işlev `HelloWorld`, adlı bir blob `HelloWorld` oluşturulur.

Bağlama ifade türleri

* [Uygulama ayarları](#binding-expressions---app-settings)
* [Tetikleyici dosya adı](#trigger-file-name)
* [Tetikleyici meta verileri](#trigger-metadata)
* [JSON yükü](#json-payloads)
* [Yeni GUID](#create-guids)
* [Geçerli tarih ve saat](#current-time)

## <a name="binding-expressions---app-settings"></a>Bağlama ifadeleri - uygulama ayarları

En iyi uygulama, gizli anahtarları ve bağlantı dizeleri yapılandırma dosyaları yerine, uygulama ayarlarını kullanarak yönetilmelidir. Bu parolalara erişimi sınırlar ve dosyaları depolamak güvenli olmasını sağlayan *function.json* genel kaynak denetimi depoları olarak.

Uygulama ayarları, ayrıca ortama bağlı yapılandırmasını değiştirmek istediğinizde yararlıdır. Örneğin, bir test ortamında farklı bir kuyruk veya blob depolama kapsayıcısı izlemek isteyebilirsiniz.

Uygulama ayarı bağlama ifadeleri gelen diğer bağlama ifadeleri farklı şekilde tanımlanır: küme ayracı yerine yüzde işaretleri sarmalanır. Örneğin blob çıkış bağlama yolu ise `%Environment%/newblob.txt` ve `Environment` uygulama ayarı değeri `Development`, bir blob oluşturulacaktır `Development` kapsayıcı.

Bir işlev yerel olarak çalışırken, uygulama ayar değerleri gelir *local.settings.json* dosya.

Unutmayın `connection` Tetikleyicileri ve bağlamaları özelliği özel bir durumdur ve otomatik olarak yüzde işaretleri olmadan uygulama ayarları olarak değerlerini çözümler. 

Aşağıdaki örnek, bir uygulama ayarı kullanan bir Azure kuyruk depolama tetikleyicisi `%input-queue-name%` tetiklenecek kuyruk tanımlamak için.

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "%input-queue-name%",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Sınıf kitaplıkları aynı yaklaşımı kullanabilirsiniz:

```csharp
[FunctionName("QueueTrigger")]
public static void Run(
    [QueueTrigger("%input-queue-name%")]string myQueueItem, 
    ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-file-name"></a>Tetikleyici dosya adı

`path` Bir Blob için tetikleyici diğer bağlamalarda tetikleme blob adını bakın ve işlev kodunu sağlayan bir desen olabilir. Desen, hangi BLOB'ları, bir işlev çağrısını tetikleyebilirsiniz belirttiğiniz filtre ölçütüne de içerebilir.

Örneğin, bağlama, aşağıdaki Blob tetikleyicisinin `path` desendir `sample-images/{filename}`, adlı bir bağlama ifadesi oluşturur `filename`:

```json
{
  "bindings": [
    {
      "name": "image",
      "type": "blobTrigger",
      "path": "sample-images/{filename}",
      "direction": "in",
      "connection": "MyStorageConnection"
    },
    ...
```

İfade `filename` sonra bir çıkış bağlaması içinde oluşturulan blob adını belirtmek için kullanılabilir:

```json
    ...
    {
      "name": "imageSmall",
      "type": "blob",
      "path": "sample-images-sm/{filename}",
      "direction": "out",
      "connection": "MyStorageConnection"
    }
  ],
}
```

İşlev kodunu kullanarak erişim bu aynı değere sahip `filename` parametre adı:

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, ILogger log)  
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Sınıf kitaplıkları özniteliklerinde bağlama ifadeleri ve düzenleri kullanmak için aynı özelliği uygular. Aşağıdaki örnekte, öznitelik oluşturucu parametresi aynıdır `path` önceki olarak değerleri *function.json* örnekleri: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    ILogger log)
{
    log.LogInformation($"Blob trigger processing: {filename}");
    // ...
}

```

Dosya adı uzantısı gibi bölümleri için ifadeleri de oluşturabilirsiniz. İfadeler ve desenleri Blob yol dizesi kullanma hakkında daha fazla bilgi için bkz. [depolama blob bağlama başvurusu](functions-bindings-storage-blob.md).

## <a name="trigger-metadata"></a>Tetikleyici meta verileri

Bir tetikleyici (örneğin, tetiklenen bir işlev bir kuyruk iletisinin içeriği) tarafından sağlanan veri yükü yanı sıra birçok tetikleyici ek meta veri değerleri sağlayın. Bu değerler, giriş parametreleri olarak kullanılabilir C# ve F# veya özellikleri `context.bindings` JavaScript nesnesi. 

Örneğin, bir Azure kuyruk depolama tetikleyicisi aşağıdaki özellikleri destekler:

* İleti içeriği geçerli bir dize ise tetikleme QueueTrigger-
* DequeueCount
* expirationTime
* Kimlik
* InsertionTime
* NextVisibleTime
* PopReceipt

Bu meta veri değerleri erişilebilir *function.json* dosya özellikleri. Örneğin, bir kuyruk tetikleyicisi kullanın ve kuyruk iletisini okumak istediğiniz bir blob adını içeren varsayalım. İçinde *function.json* kullanabileceğiniz dosyası `queueTrigger` BLOB meta veri özelliği `path` özelliği, aşağıdaki örnekte gösterildiği gibi:

```json
  "bindings": [
    {
      "name": "myQueueItem",
      "type": "queueTrigger",
      "queueName": "myqueue-items",
      "connection": "MyStorageConnection",
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "direction": "in",
      "connection": "MyStorageConnection"
    }
  ]
```

Meta veri özelliklerini her tetikleyicinin ayrıntılarını karşılık gelen başvuru makalesinde açıklanmıştır. Bir örnek için bkz. [kuyruğu tetikleyici meta verileri](functions-bindings-storage-queue.md#trigger---message-metadata). Belgeleri kullanılabilir ayrıca **tümleştir** Portalı'nın sekmesini, **belgeleri** bölümüne bağlama yapılandırma alanı.  

## <a name="json-payloads"></a>JSON yükü

Bir tetikleyici yükü JSON olduğunda, aynı işlevi ve işlev kodunu diğer bağlamalar için yapılandırma özelliklerine başvurabilirsiniz.

Aşağıdaki örnekte gösterildiği *function.json* dosya JSON biçiminde bir blob adı alan bir Web kancası işlevi için: `{"BlobName":"HelloWorld.txt"}`. Bir Blob giriş bağlama blob okur ve HTTP bağlama döndürür HTTP yanıtında blob içeriklerini çıktı. Blob giriş bağlamasına doğrudan başvurarak blob adı alır fark `BlobName` özelliği (`"path": "strings/{BlobName}"`)

```json
{
  "bindings": [
    {
      "name": "info",
      "type": "httpTrigger",
      "direction": "in",
      "webHookType": "genericJson"
    },
    {
      "name": "blobContents",
      "type": "blob",
      "direction": "in",
      "path": "strings/{BlobName}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "res",
      "type": "http",
      "direction": "out"
    }
  ]
}
```

Bu iş C# ve F#, aşağıdaki örnekte olduğu gibi seri için alanları tanımlayan bir sınıf gerekir:

```csharp
using System.Net;
using Microsoft.Extensions.Logging;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents, ILogger log)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    log.LogInformation($"Processing: {info.BlobName}");

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

JavaScript'te, JSON seri durumundan çıkarma otomatik olarak gerçekleştirilir.

```javascript
module.exports = function (context, info) {
    if ('BlobName' in info) {
        context.res = {
            body: { 'data': context.bindings.blobContents }
        }
    }
    else {
        context.res = {
            status: 404
        };
    }
    context.done();
}
```

### <a name="dot-notation"></a>Nokta gösterimi

JSON yükünüzü özelliklerinde bazı özelliklere sahip nesneler varsa, bu nokta gösterimi kullanılarak doğrudan başvurabilirsiniz. Örneğin, JSON şöyle düşünün:

```json
{
  "BlobName": {
    "FileName":"HelloWorld",
    "Extension":"txt"
  }
}
```

Doğrudan başvurabilirsiniz `FileName` olarak `BlobName.FileName`. Bu JSON biçimi ile işte `path` önceki örnekte özelliği gibi görünür:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

C# ', iki sınıf gerekir:

```csharp
public class BlobInfo
{
    public BlobName BlobName { get; set; }
}
public class BlobName
{
    public string FileName { get; set; }
    public string Extension { get; set; }
}
```

## <a name="create-guids"></a>GUID oluştur

`{rand-guid}` Bağlama ifadesi bir GUID oluşturur. Aşağıdaki blob yolu bir `function.json` dosyası gibi bir ada sahip bir blob oluşturur *50710cb5 84b9 - 4d 9 87 d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

## <a name="current-time"></a>Geçerli saati

Bağlama ifadesi `DateTime` çözümler `DateTime.UtcNow`. Aşağıdaki blob yolu bir `function.json` dosyası gibi bir ada sahip bir blob oluşturur *2018-02-16T17-59-55Z.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```
## <a name="binding-at-runtime"></a>Çalışma zamanında bağlama

C# ve diğer .NET dilleri, bildirim temelli bağlamaları aksine bir kesinlik temelli bir bağlama deseni kullanabileceğiniz *function.json* ve öznitelikleri. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Daha fazla bilgi için bkz. [C# Geliştirici Başvurusu](functions-dotnet-class-library.md#binding-at-runtime) veya [C# betiği Geliştirici Başvurusu](functions-reference-csharp.md#binding-at-runtime).

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure işlev dönüş değeri kullanma](./functions-bindings-return-value.md)
