---
title: Azure işlevleri için Azure Blob Depolama bağlamaları
description: Azure Blob Depolama Tetikleyicileri ve bağlamaları Azure işlevleri'nde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 11/15/2018
ms.author: cshoe
ms.openlocfilehash: f54da6e350b2cf9027b6e9e02ace2a90e292e1ce
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66472351"
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure Blob Depolama bağlamaları

Bu makalede, Azure işlevleri'nde Azure Blob Depolama bağlamaları ile nasıl çalışılacağı açıklanmaktadır. Azure işlevleri destekler tetiklemek, giriş ve çıkış bağlamaları blobları için. Makale her bağlama için bir bölüm içerir:

* [BLOB tetikleyicisi](#trigger)
* [BLOB giriş bağlama](#input)
* [BLOB çıktı bağlaması](#output)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Olay Kılavuzu tetikleyicisi yalnızca blob depolama hesapları için büyük ölçekli veya gecikme süresini azaltmak için Blob Depolama tetikleyici yerine kullanın. Daha fazla bilgi için [tetikleyici](#trigger) bölümü.

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Blob Depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi sürüm 2.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/v2.x/src/Microsoft.Azure.WebJobs.Storage/Blob) GitHub deposu.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

[!INCLUDE [functions-storage-sdk-version](../../includes/functions-storage-sdk-version.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Blob Depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage) NuGet paketi sürüm 3.x. Paket için kaynak kodu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs) GitHub deposu.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Yeni veya güncelleştirilmiş bir blob algılandığında bir işlevin Blob Depolama tetikleyicisi başlatır. Blob içeriği işleve giriş olarak sağlanır.

[Event Grid tetikleyicisinin](functions-bindings-event-grid.md) için yerleşik desteği mevcuttur [blob olayları](../storage/blobs/storage-blob-event-overview.md) ve yeni veya güncelleştirilmiş bir blob algılandığında bir işlev başlatmak için de kullanılabilir. Bir örnek için bkz. [görüntüsünü yeniden boyutlandırma, Event Grid ile](../event-grid/resize-images-on-storage-blob-upload-event.md) öğretici.

Event Grid, Blob Depolama tetikleyici yerine aşağıdaki senaryolar için kullanın:

* Blob Storage hesapları
* Yüksek ölçek
* Gecikme süresini en aza indirme

### <a name="blob-storage-accounts"></a>Blob Storage hesapları

[BLOB Depolama hesapları](../storage/common/storage-account-overview.md#types-of-storage-accounts) blob giriş için desteklenir ve çıkış bağlamaları blob Tetikleyicileri için değil. BLOB Depolama Tetikleyicileri bir genel amaçlı depolama hesabı gerektirir.

### <a name="high-scale"></a>Yüksek ölçek

Büyük ölçekli gevşek tanımlanabilir 100. 000'den fazla BLOB'ları içeren kapsayıcı olarak veya depolama hesaplarının saniye başına 100'den fazla blob güncelleştirmelerine sahip olması.

### <a name="latency-issues"></a>Gecikme sorunlarını

İşlev uygulamanızın bir tüketim planında ise, olabilir bir 10 dakikaya kadar bir işlev uygulaması boşta geçti, yeni BLOB'lar işleme. Bu gecikmeyi önlemek için Always On özellikli bir App Service planına geçiş yapabilirsiniz. Ayrıca bir [Event Grid tetikleyicisinin](functions-bindings-event-grid.md) Blob Depolama hesabınız ile. Bir örnek için bkz. [Event Grid öğretici](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json). 

### <a name="queue-storage-trigger"></a>Kuyruk depolama tetikleyicisi

Event Grid yanı sıra BLOB'ları işlemek için başka bir alternatif kuyruk depolama tetikleyicisi, ancak bunu blob olayları için yerleşik destek yok. Oluştururken veya BLOB'ları güncelleştirirken kuyruk iletileri oluşturmak gerekir. Yaptığınızdan varsayılmaktadır bir örnek için bkz: [blob giriş bağlama örnek bu makalenin devamındaki](#input---example).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [Java](#trigger---java-example)
* [JavaScript](#trigger---javascript-example)
* [Python](#trigger---python-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , yazar günlük bir blob eklendiğinde veya güncelleştirdiğiniz `samples-workitems` kapsayıcı.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Dize `{name}` blob tetikleyicisi yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](./functions-bindings-expressions-patterns.md) tetikleme blob dosya adı erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için [Blob adı desenlerinin](#trigger---blob-name-patterns) bu makalenin ilerleyen bölümlerinde.

Hakkında daha fazla bilgi için `BlobTrigger` özniteliği için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bir blob tetikleyicisi bağlama gösterir. bir *function.json* dosya ve [Python kodu](functions-reference-python.md) bağlama kullanan. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `samples-workitems` [kapsayıcı](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources).

Veri bağlama işte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Dize `{name}` blob tetikleyicisi yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](./functions-bindings-expressions-patterns.md) tetikleme blob dosya adı erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için [Blob adı desenlerinin](#trigger---blob-name-patterns) bu makalenin ilerleyen bölümlerinde.

Hakkında daha fazla bilgi için *function.json* dosya özellikleri, bkz: [yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

İçin bağlanan bir C# kodu İşte bir `Stream`:

```cs
public static void Run(Stream myBlob, string name, ILogger log)
{
   log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

İçin bağlanan bir C# kodu İşte bir `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, ILogger log)
{
    log.LogInformation($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bir blob tetikleyicisi bağlama gösterir. bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlama kullanan. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `samples-workitems` kapsayıcı.

İşte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "myBlob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Dize `{name}` blob tetikleyicisi yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](./functions-bindings-expressions-patterns.md) tetikleme blob dosya adı erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için [Blob adı desenlerinin](#trigger---blob-name-patterns) bu makalenin ilerleyen bölümlerinde.

Hakkında daha fazla bilgi için *function.json* dosya özellikleri, bkz: [yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

### <a name="trigger---python-example"></a>Tetikleyici - Python örnek

Aşağıdaki örnek, bir blob tetikleyicisi bağlama gösterir. bir *function.json* dosya ve [Python kodu](functions-reference-python.md) bağlama kullanan. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `samples-workitems` [kapsayıcı](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources).

İşte *function.json* dosyası:

```json
{
    "scriptFile": "__init__.py",
    "disabled": false,
    "bindings": [
        {
            "name": "myblob",
            "type": "blobTrigger",
            "direction": "in",
            "path": "samples-workitems/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Dize `{name}` blob tetikleyicisi yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](./functions-bindings-expressions-patterns.md) tetikleme blob dosya adı erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için [Blob adı desenlerinin](#trigger---blob-name-patterns) bu makalenin ilerleyen bölümlerinde.

Hakkında daha fazla bilgi için *function.json* dosya özellikleri, bkz: [yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func

def main(myblob: func.InputStream):
    logging.info('Python Blob trigger function processed %s', myblob.name)
```

### <a name="trigger---java-example"></a>Tetikleyici - Java örnek

Aşağıdaki örnek, bir blob tetikleyicisi bağlama gösterir. bir *function.json* dosya ve [Java kod](functions-reference-java.md) bağlama kullanan. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `myblob` kapsayıcı.

İşte *function.json* dosyası:

```json
{
    "disabled": false,
    "bindings": [
        {
            "name": "file",
            "type": "blobTrigger",
            "direction": "in",
            "path": "myblob/{name}",
            "connection":"MyStorageAccountAppSetting"
        }
    ]
}
```

Java kod aşağıdaki gibidir:

```java
@FunctionName("blobprocessor")
public void run(
  @BlobTrigger(name = "file",
               dataType = "binary",
               path = "myblob/{name}",
               connection = "MyStorageAccountAppSetting") byte[] content,
  @BindingName("name") String filename,
  final ExecutionContext context
) {
  context.getLogger().info("Name: " + filename + " Size: " + content.length + " bytes");
}
```


## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), bir blob tetikleyicisi yapılandırmak için aşağıdaki öznitelikleri kullanın:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobTriggerAttribute.cs)

  Özniteliğin Oluşturucusu izlemek için kapsayıcı gösteren yol dizesi alır ve isteğe bağlı olarak bir [blob adı deseni](#trigger---blob-name-patterns). Bir örneği aşağıda verilmiştir:

  ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
  ```

  Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

   ```csharp
  [FunctionName("ResizeImage")]
  public static void Run(
      [BlobTrigger("sample-images/{name}", Connection = "StorageConnectionAppSetting")] Stream image,
      [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
  {
      ....
  }
   ```

  Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Kullanılacak depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu bir depolama bağlantı dizesi içeren bir uygulama ayarı adı alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

  ```csharp
  [StorageAccount("ClassLevelStorageAppSetting")]
  public static class AzureFunctions
  {
      [FunctionName("BlobTrigger")]
      [StorageAccount("FunctionLevelStorageAppSetting")]
      public static void Run( //...
  {
      ....
  }
  ```

Kullanılacak depolama hesabı aşağıdaki sırada belirlenir:

* `BlobTrigger` Özniteliğin `Connection` özelliği.
* `StorageAccount` Özniteliği aynı parametresine uygulanan `BlobTrigger` özniteliği.
* `StorageAccount` İşleve uygulanmış bir öznitelik.
* `StorageAccount` Sınıfına uygulanan bir öznitelik.
* İşlev uygulaması (uygulama ayarı "AzureWebJobsStorage") için varsayılan depolama hesabı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `BlobTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `blobTrigger`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır.|
|**direction** | yok | Ayarlanmalıdır `in`. Bu özellik, Azure portalında tetikleyicisi oluşturduğunuzda otomatik olarak ayarlanır. Özel durumlar belirtilmiştir [kullanım](#trigger---usage) bölümü. |
|**name** | yok | İşlev kodunu blob temsil eden değişken adı. |
|**Yolu** | **BlobPath** |[Kapsayıcı](../storage/blobs/storage-blobs-introduction.md#blob-storage-resources) izlemek için.  Olabilir bir [blob adı deseni](#trigger---blob-name-patterns). |
|**bağlantı** | **bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.<br><br>Bağlantı dizesi, genel amaçlı depolama hesabı için olmamalıdır bir [Blob Depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# betiği aşağıdaki parametre türleri için tetikleme blob kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* JSON olarak seri hale getirilebilir bir POCO
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` bir C# sınıf kitaplığı'nda.

Depolama SDK'sı türlerinden birini bağlamak ve bir hata iletisi alıyorum denerseniz, bir başvuru sahip olduğunuzdan emin [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

Bağlama `string`, `Byte[]`, veya POCO yalnızca önerilen blob boyutu küçükse, tüm blob içeriğini belleğe yüklenir. Genellikle, kullanılacak tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin ilerleyen bölümlerinde.

JavaScript'te kullanarak giriş blob veri erişim `context.bindings.<name from function.json>`.

## <a name="trigger---blob-name-patterns"></a>Tetikleyici - blob adı modelleri

Blob adı deseni de belirtebilirsiniz `path` özelliğinde *function.json* veya `BlobTrigger` öznitelik Oluşturucusu. Ad deseni olabilir bir [filtre veya bağlama ifadesi](./functions-bindings-expressions-patterns.md). Aşağıdaki bölümlerde örnekler sunar.

### <a name="get-file-name-and-extension"></a>Dosya adını ve uzantısını alma

Aşağıdaki örnek, blob dosya adı ve uzantısı için ayrı ayrı bağlamanız gösterilmektedir:

```json
"path": "input/{blobname}.{blobextension}",
```
Blob ise *özgün Blob1.txt*, değerlerini `blobname` ve `blobextension` işlevinin kodundaki değişkenleri *özgün Blob1* ve *txt*.

### <a name="filter-on-blob-name"></a>BLOB adına göre filtrele

Aşağıdaki örnek Tetikleyicileri yalnızca blob'larda `input` "orijinal-" dizesi ile başlayan kapsayıcı:

```json
"path": "input/original-{name}",
```

Blob adı ise *özgün Blob1.txt*, değerini `name` olan işlev kodunu bir değişkende `Blob1`.

### <a name="filter-on-file-type"></a>Dosya türü üzerinde filtreleme

Aşağıdaki örnek yalnızca tetikler *.png* dosyaları:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Ayraç içindeki dosya adlarını filtreleme

Ayraç içindeki dosya adlarını aramak için Küme ayraçları, iki küme ayraçlarını kullanarak çıkış. Aşağıdaki örnek filtreler kaşlı ayraçlar içinde adına sahip BLOB'ları için:

```json
"path": "images/{{20140101}}-{name}",
```

Blob ise  *{20140101}-soundfile.mp3*, `name` işlev kodunu değişken değer *soundfile.mp3*.

## <a name="trigger---metadata"></a>Tetikleyici - meta verileri

Blob tetikleyicisi çeşitli meta veri özelliklerini sağlar. Bu özellikler, diğer bağlamalar bağlama ifadelerinde parçası olarak veya kodunuzu parametreler olarak kullanılabilir. Bu değerleri ile aynı semantiklere sahip [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.cloudblob?view=azure-dotnet) türü.

|Özellik  |Tür  |Açıklama  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Tetikleyici blob yolu.|
|`Uri`|`System.Uri`|Birincil konumu olarak blob URI'si.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.azure.storage.blob.blobproperties)|Blobun Sistem Özellikleri. |
|`Metadata` |`IDictionary<string,string>`|Kullanıcı tanımlı meta veriler blob için.|

Örneğin, aşağıdaki C# betiği ve JavaScript örnekler kapsayıcı dahil olmak üzere tetikleyici blob yolu oturum:

```csharp
public static void Run(string myBlob, string blobTrigger, ILogger log)
{
    log.LogInformation($"Full blob path: {blobTrigger}");
} 
```

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

## <a name="trigger---blob-receipts"></a>Tetikleyici - blob giriş

Azure işlevleri çalışma zamanı, blob bir tetikleyici işlev, aynı yeni veya güncelleştirilmiş blob için birden çok kez adlı sağlar. Belirtilen blob sürüm işlenmişse sakladığı belirlemek için *blob giriş*.

Azure işlevleri depoları giriş adlı bir kapsayıcı içinde blob *azure webjobs konakları* işlev uygulamanız için Azure depolama hesabında (uygulama ayarı tarafından tanımlanan `AzureWebJobsStorage`). Bir blob giriş bölümünde aşağıdaki bilgiler bulunur:

* Tetiklenen işlevi (" *&lt;işlev uygulaması adı >* . İşlevler.  *&lt;işlev adı >* ", örneğin: "MyFunctionApp.Functions.CopyBlob")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (örneğin, bir blob sürüm tanımlayıcısı: "0x8D1DC6E70A277EF")

Bir blobu yeniden işlemeyerek zorlamak için bu blobu için blob giriş Sil *azure webjobs konakları* kapsayıcı el ile. Yeniden işlemeyerek hemen gerçekleşmeyebilir olsa da sonraki bir noktada kapatmasında garanti.

## <a name="trigger---poison-blobs"></a>Tetikleyici - zehirli BLOB'ları

Belirli bir blobu için blob tetikleyici işlevi başarısız olduğunda, Azure işlevleri, varsayılan olarak 5 kez toplam işlev yeniden dener.

Azure işlevleri 5 tüm denemeler başarısız olursa, adlı bir depolama kuyruğuna bir ileti ekler *webjobs blobtrigger poison*. Kuyruk iletisi zehirli bloblar için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlev uygulaması adı >* . İşlevler.  *&lt;işlev adı >* )
* BlobType ("BlockBlob" veya "PageBlob")
* ContainerName
* BlobName
* ETag (örneğin, bir blob sürüm tanımlayıcısı: "0x8D1DC6E70A277EF")

## <a name="trigger---concurrency-and-memory-usage"></a>Tetikleyici - eşzamanlılık ve bellek kullanımı

En fazla eş zamanlı işlev çağrılarını sayısı tarafından denetlenir blob tetikleyicisi bir sıra dahili olarak kullandığı [host.json kuyrukları yapılandırmasında](functions-host-json.md#queues). Varsayılan ayarlar, 24 çağrılarına eşzamanlılık sınırlar. Bu sınır, ayrı olarak bir blob tetikleyicisi kullanan her bir işlev için de geçerlidir.

[Tüketim planı](functions-scale.md#how-the-consumption-and-premium-plans-work) 1,5 GB bellek bir sanal makineye (VM) üzerinde bir işlev uygulaması sınırlar. Bellek her eş zamanlı olarak yürütülen bir işlev örneği ve İşlevler çalışma zamanının kendisi tarafından kullanılır. Blob ile tetiklenen bir işlev tüm blob belleğine yükler, blobları için bu işlev tarafından kullanılan en fazla belleği 24'tür * en yüksek blob boyutu. Örneğin, üç blob ile tetiklenen işlev ve varsayılan ayarlarla bir işlev uygulaması 3 * 24 = 72 VM başına en fazla eşzamanlılığı gerekir işlev çağrıları.

JavaScript ve Java işlevleri, belleğe, tüm blob yüklemek ve C# işlevleri yaparsanız, adlarınıza `string`, `Byte[]`, veya POCO.

## <a name="trigger---polling"></a>Tetikleme - yoklama

İzlenmekte olan blob kapsayıcı (tüm kapsayıcılar arasında) 10. 000'den fazla BLOB varsa, İşlevler çalışma zamanı taramalar günlük dosyaları için yeni veya değiştirilmiş blobları izleyin. Bu işlem, gecikmelerine neden olabilir. Blob oluşturulduktan sonra bir işleve kadar birkaç dakika veya daha uzun tetiklenir değil.

> [!WARNING]
> Ayrıca, [depolama günlükleri "en iyi çaba" üzerinde oluşturulan](/rest/api/storageservices/About-Storage-Analytics-Logging) temel. Tüm olaylar yakalanır garantisi yoktur. Bazı koşullar altında günlükleri eksik olabilir.
> 
> Daha hızlı veya daha güvenilir blob işleme gerekiyorsa oluşturmayı göz önünde bulundurun bir [kuyruk iletisi](../storage/queues/storage-dotnet-how-to-use-queues.md) blob oluşturduğunuzda. Ardından bir [kuyruk tetikleyicisi](functions-bindings-storage-queue.md) blob işlemek için bir blob tetikleyicisi yerine. Event grid'i başka bir seçenektir; öğreticiye bakın [karşıya yüklenen görüntülerin Event Grid kullanarak otomatikleştirme yeniden boyutlandırma](../event-grid/resize-images-on-storage-blob-upload-event.md).
>

## <a name="input"></a>Girdi

Blob Depolama giriş bağlama blobları okumak için kullanın.

## <a name="input---example"></a>Giriş - örnek

Dile özgü örneğe bakın:

* [C#](#input---c-example)
* [C# betiği (.csx)](#input---c-script-example)
* [Java](#input---java-examples)
* [JavaScript](#input---javascript-example)
* [Python](#input---python-example)

### <a name="input---c-example"></a>Giriş - C# örneği

Aşağıdaki örnek, bir [C# işlevi](functions-dotnet-class-library.md) kuyruk tetikleyicisi ve giriş blob bağlama kullanır. Kuyruk iletisi blob adını içerir ve işlev blob boyutu günlüğe kaydeder.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

### <a name="input---c-script-example"></a>Giriş - C# betiği örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [C# betiği (.csx)](functions-reference-csharp.md) bağlamaları kullanan kod. İşlevi, bir metin blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlamaları kullanır. İşlevi, bir blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

### <a name="input---python-example"></a>Giriş - Python örnek

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [Python kodu](functions-reference-python.md) bağlamaları kullanır. İşlevi, bir blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "$return",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func

def main(queuemsg: func.QueueMessage, inputblob: func.InputStream) -> func.InputStream:
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    return inputblob
```

### <a name="input---java-examples"></a>Giriş - Java örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [HTTP tetikleyicisi, blob adı Sorgu dizesinden Ara](#http-trigger-look-up-blob-name-from-query-string-java)
* [Tetikleyici kuyruk, blob adı sıraya ileti Al](#queue-trigger-receive-blob-name-from-queue-message-java)

#### <a name="http-trigger-look-up-blob-name-from-query-string-java"></a>HTTP tetikleyicisi, blob adı (Java) Sorgu dizesinden Ara

 Aşağıdaki örnek, kullanan bir Java işlev gösterir. ```HttpTrigger``` bir blob depolama kapsayıcısında bir dosya adını içeren bir parametre almak için ek açıklama. ```BlobInput``` Dosyasını okur ve içeriği görmesine geçirir ardından ek açıklama bir ```byte[]```.

```java
  @FunctionName("getBlobSizeHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage blobSize(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    final ExecutionContext context) {
      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-receive-blob-name-from-queue-message-java"></a>Tetikleyici kuyruk, blob adı alma kuyruk iletisi (Java)

 Aşağıdaki örnek, kullanan bir Java işlev gösterir. ```QueueTrigger``` bir blob depolama kapsayıcısında bir dosya adını içeren bir ileti almak için ek açıklama. ```BlobInput``` Dosyasını okur ve içeriği görmesine geçirir ardından ek açıklama bir ```byte[]```.

```java
  @FunctionName("getBlobSize")
  @StorageAccount("Storage_Account_Connection_String")
  public void blobSize(
    @QueueTrigger(
      name = "filename", 
      queueName = "myqueue-items-sample") 
    String filename,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{queueTrigger}") 
    byte[] content,
    final ExecutionContext context) {
      context.getLogger().info("The size of \"" + filename + "\" is: " + content.length + " bytes");
  }
```

İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@BlobInput` ek açıklama parametreleri değeri bir blobun gelmesi.  Bu ek açıklama yerel Java türler, pojo'ları veya kullanarak boş değer atanabilir değer ile kullanılabilir `Optional<T>`.

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

Özniteliğin oluşturucusu için blob yolunu alır ve bir `FileAccess` okuma veya yazma, aşağıdaki örnekte gösterildiği gibi gösteren parametre:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    ILogger log)
{
    log.LogInformation($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Kullanabileceğiniz `StorageAccount` sınıf, yöntem ya da parametre düzeyinde depolama hesabı belirtmek için özniteliği. Daha fazla bilgi için [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Blob` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `blob`. |
|**direction** | yok | Ayarlanmalıdır `in`. Özel durumlar belirtilmiştir [kullanım](#input---usage) bölümü. |
|**name** | yok | İşlev kodunu blob temsil eden değişken adı.|
|**Yolu** |**BlobPath** | Blob yolu. |
|**bağlantı** |**bağlantı**| İçeren bir uygulama ayarı adı [depolama bağlantı dizesi](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-azure-storage-account) Bu bağlama için kullanılacak. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.<br><br>Bağlantı dizesi, genel amaçlı depolama hesabı için olmamalıdır bir [yalnızca blob depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|yok | **Erişim** | Okuma yazma ya da olup olmadığını gösterir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

C# ve C# betiği aşağıdaki parametre türleri için blob giriş bağlama kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* `CloudBlobContainer`
* `CloudBlobDirectory`
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` bir C# sınıf kitaplığı'nda.

Depolama SDK'sı türlerinden birini bağlamak ve bir hata iletisi alıyorum denerseniz, bir başvuru sahip olduğunuzdan emin [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

Bağlama `string` veya `Byte[]` yalnızca blob boyutu tüm blob içeriklerini belleğe yüklenen gibi küçük olup olmadığını önerilir. Genellikle, kullanılacak tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin üst kısmındaki.

JavaScript'te, blob kullanarak veri erişim `context.bindings.<name from function.json>`.

## <a name="output"></a>Çıktı

BLOB Depolama çıkış bağlamaları, BLOB'ları yazmak için kullanın.

## <a name="output---example"></a>Çıkış - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betiği (.csx)](#output---c-script-example)
* [Java](#output---java-examples)
* [JavaScript](#output---javascript-example)
* [Python](#output---python-example)

### <a name="output---c-example"></a>Çıkış - C# örneği

Aşağıdaki örnek, bir [C# işlevi](functions-dotnet-class-library.md) blob tetikleyicisi kullanan ve iki blob bağlamaları çıktı. Görüntü blob'u oluşturulmasına işlevi tetikleyen *örnek görüntüleri* kapsayıcı. Görüntü blob'u küçük ve orta ölçekli kopyalarını oluşturur.

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-sm/{name}", FileAccess.Write)] Stream imageSmall,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageMedium)
{
    var imageBuilder = ImageResizer.ImageBuilder.Current;
    var size = imageDimensionsTable[ImageSize.Small];

    imageBuilder.Build(image, imageSmall,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);

    image.Position = 0;
    size = imageDimensionsTable[ImageSize.Medium];

    imageBuilder.Build(image, imageMedium,
        new ResizeSettings(size.Item1, size.Item2, FitMode.Max, null), false);
}

public enum ImageSize { ExtraSmall, Small, Medium }

private static Dictionary<ImageSize, (int, int)> imageDimensionsTable = new Dictionary<ImageSize, (int, int)>() {
    { ImageSize.ExtraSmall, (320, 200) },
    { ImageSize.Small,      (640, 400) },
    { ImageSize.Medium,     (800, 600) }
};
```

### <a name="output---c-script-example"></a>Çıkış - C# betiği örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [C# betiği (.csx)](functions-reference-csharp.md) bağlamaları kullanan kod. İşlevi, bir metin blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kodunu şu şekildedir:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, ILogger log)
{
    log.LogInformation($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="output---javascript-example"></a>Çıkış - JavaScript örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlamaları kullanır. İşlevi, bir blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "myQueueItem",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "myInputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "myOutputBlob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputBlob = context.bindings.myInputBlob;
    context.done();
};
```

### <a name="output---python-example"></a>Çıkış - Python örnek

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlamaları, gösterir bir *function.json* dosya ve [Python kodu](functions-reference-python.md) bağlamaları kullanır. İşlevi, bir blob bir kopyasını oluşturur. İşlevin blob kopyalama adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-Kopyala*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği, blob adı belirtmek için kullanılır `path` özellikleri:

```json
{
  "bindings": [
    {
      "queueName": "myqueue-items",
      "connection": "MyStorageConnectionAppSetting",
      "name": "queuemsg",
      "type": "queueTrigger",
      "direction": "in"
    },
    {
      "name": "inputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "in"
    },
    {
      "name": "outputblob",
      "type": "blob",
      "path": "samples-workitems/{queueTrigger}-Copy",
      "connection": "MyStorageConnectionAppSetting",
      "direction": "out"
    }
  ],
  "disabled": false,
  "scriptFile": "__init__.py"
}
```

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

Python kod aşağıdaki gibidir:

```python
import logging
import azure.functions as func

def main(queuemsg: func.QueueMessage, inputblob: func.InputStream,
         outputblob: func.Out[func.InputStream]):
    logging.info('Python Queue trigger function processed %s', inputblob.name)
    outputblob.set(inputblob)
```

### <a name="output---java-examples"></a>Çıkış - Java örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [HTTP tetikleyicisi, OutputBinding kullanma](#http-trigger-using-outputbinding-java)
* [İşlev dönüş değeri kullanarak, kuyruk tetikleyicisi](#queue-trigger-using-function-return-value-java)

#### <a name="http-trigger-using-outputbinding-java"></a>OutputBinding (Java) kullanarak, HTTP tetikleyicisi

 Aşağıdaki örnek, kullanan bir Java işlev gösterir. ```HttpTrigger``` bir blob depolama kapsayıcısında bir dosya adını içeren bir parametre almak için ek açıklama. ```BlobInput``` Dosyasını okur ve içeriği görmesine geçirir ardından ek açıklama bir ```byte[]```. ```BlobOutput``` Ek açıklama bağlanır ```OutputBinding outputItem```, ardından işlevin giriş blob içeriğini yapılandırılmış depolama kapsayıcısına yazmak için kullanılır.

```java
  @FunctionName("copyBlobHttp")
  @StorageAccount("Storage_Account_Connection_String")
  public HttpResponseMessage copyBlobHttp(
    @HttpTrigger(name = "req", 
      methods = {HttpMethod.GET}, 
      authLevel = AuthorizationLevel.ANONYMOUS) 
    HttpRequestMessage<Optional<String>> request,
    @BlobInput(
      name = "file", 
      dataType = "binary", 
      path = "samples-workitems/{Query.file}") 
    byte[] content,
    @BlobOutput(
      name = "target", 
      path = "myblob/{Query.file}-CopyViaHttp")
    OutputBinding<String> outputItem,
    final ExecutionContext context) {
      // Save blob to outputItem
      outputItem.setValue(new String(content, StandardCharsets.UTF_8));

      // build HTTP response with size of requested blob
      return request.createResponseBuilder(HttpStatus.OK)
        .body("The size of \"" + request.getQueryParameters().get("file") + "\" is: " + content.length + " bytes")
        .build();
  }
```

#### <a name="queue-trigger-using-function-return-value-java"></a>İşlev dönüş değeri (Java) kullanarak, kuyruk tetikleyicisi

 Aşağıdaki örnek, kullanan bir Java işlev gösterir. ```QueueTrigger``` bir blob depolama kapsayıcısında bir dosya adını içeren bir ileti almak için ek açıklama. ```BlobInput``` Dosyasını okur ve içeriği görmesine geçirir ardından ek açıklama bir ```byte[]```. ```BlobOutput``` Ek açıklama, ardından giriş blob içeriğini yapılandırılmış depolama kapsayıcısına yazmak için çalışma zamanı tarafından kullanılan işlev dönüş değeri bağlanır.

```java
  @FunctionName("copyBlobQueueTrigger")
  @StorageAccount("Storage_Account_Connection_String")
  @BlobOutput(
    name = "target", 
    path = "myblob/{queueTrigger}-Copy")
  public String copyBlobQueue(
    @QueueTrigger(
      name = "filename", 
      dataType = "string",
      queueName = "myqueue-items") 
    String filename,
    @BlobInput(
      name = "file", 
      path = "samples-workitems/{queueTrigger}") 
    String content,
    final ExecutionContext context) {
      context.getLogger().info("The content of \"" + filename + "\" is: " + content);
      return content;
  }
```

 İçinde [Java Çalışma Zamanı Kitaplığı işlevleri](/java/api/overview/azure/functions/runtime), kullanın `@BlobOutput` değeri blob depolama içinde bir nesneye yazılabilir işlevi parametrelere ilişkin açıklama.  Parametre türü olmalıdır `OutputBinding<T>`Burada T herhangi bir yerel Java türü veya bir POJO'ya adıdır.

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/dev/src/Microsoft.Azure.WebJobs.Extensions.Storage/Blobs/BlobAttribute.cs).

Özniteliğin oluşturucusu için blob yolunu alır ve bir `FileAccess` okuma veya yazma, aşağıdaki örnekte gösterildiği gibi gösteren parametre:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write)] Stream imageSmall)
{
    ...
}
```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{name}")] Stream image,
    [Blob("sample-images-md/{name}", FileAccess.Write, Connection = "StorageConnectionAppSetting")] Stream imageSmall)
{
    ...
}
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-example).

Kullanabileceğiniz `StorageAccount` sınıf, yöntem ya da parametre düzeyinde depolama hesabı belirtmek için özniteliği. Daha fazla bilgi için [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `Blob` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | Ayarlanmalıdır `blob`. |
|**direction** | yok | Ayarlanmalıdır `out` bir çıkış bağlaması için. Özel durumlar belirtilmiştir [kullanım](#output---usage) bölümü. |
|**name** | yok | İşlev kodunu blob temsil eden değişken adı.  Kümesine `$return` işlev dönüş değeri başvurmak için.|
|**Yolu** |**BlobPath** | Blob kapsayıcısı yolu. |
|**bağlantı** |**bağlantı**| Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, adın Buraya yalnızca geri kalanında belirtebilirsiniz. Örneğin, ayarlarsanız `connection` "AzureWebJobsMyStorage." adlı bir uygulama ayarı için "Depolamam", İşlevler çalışma zamanı arar. Bırakırsanız `connection` boş, İşlevler çalışma zamanı varsayılan depolama bağlantı dizesi uygulama ayarlarında adlı kullanır `AzureWebJobsStorage`.<br><br>Bağlantı dizesi, genel amaçlı depolama hesabı için olmamalıdır bir [yalnızca blob depolama hesabı](../storage/common/storage-account-overview.md#types-of-storage-accounts).|
|yok | **Erişim** | Okuma yazma ya da olup olmadığını gösterir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

C# ve C# betik BLOB'ları yazmak için aşağıdaki türlerine bağlayabilirsiniz:

* `TextWriter`
* `out string`
* `out Byte[]`
* `CloudBlobStream`
* `Stream`
* `CloudBlobContainer`<sup>1</sup>
* `CloudBlobDirectory`
* `ICloudBlob`<sup>2</sup>
* `CloudBlockBlob`<sup>2</sup>
* `CloudPageBlob`<sup>2</sup>
* `CloudAppendBlob`<sup>2</sup>

<sup>1</sup> "içinde" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.Read` bir C# sınıf kitaplığı'nda. Ancak, BLOB kapsayıcısına yüklemek gibi işlemleri için yazmak için çalışma zamanı sağlayan kapsayıcı nesne kullanabilirsiniz.

<sup>2</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` bir C# sınıf kitaplığı'nda.

Depolama SDK'sı türlerinden birini bağlamak ve bir hata iletisi alıyorum denerseniz, bir başvuru sahip olduğunuzdan emin [doğru depolama SDK'sı sürüm](#azure-storage-sdk-version-in-functions-1x).

Zaman uyumsuz işlevleri'nde dönüş değerini kullanın veya `IAsyncCollector` yerine bir `out` parametresi.

Bağlama `string` veya `Byte[]` yalnızca blob boyutu tüm blob içeriklerini belleğe yüklenen gibi küçük olup olmadığını önerilir. Genellikle, kullanılacak tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin üst kısmındaki.


JavaScript'te, blob kullanarak veri erişim `context.bindings.<name from function.json>`.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama |  Başvuru |
|---|---|
| Blob | [BLOB hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| BLOB, tablo, kuyruk |  [Depolama hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tablo, kuyruk |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Blob storage trigger](functions-create-storage-blob-triggered-function.md)
--->
