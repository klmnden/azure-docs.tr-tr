---
title: Azure işlevleri için Azure Blob Depolama bağlamaları
description: Azure Blob Depolama Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/12/2018
ms.author: glenga
ms.openlocfilehash: eccaf205ae4705848b591442ca0fdb2aab44b9c6
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="azure-blob-storage-bindings-for-azure-functions"></a>Azure işlevleri için Azure Blob Depolama bağlamaları

Bu makalede Azure Blob Depolama bağlamaları Azure işlevlerinde ile nasıl çalışılacağını açıklar. Tetiklemek, giriş ve BLOB'lar için bağlamaları çıktı Azure işlevleri destekler. Makaleyi her bağlama için bir bölüm içerir:

* [BLOB tetikleyici](#trigger)
* [BLOB giriş bağlama](#input)
* [BLOB çıkış bağlama](#output)

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> [Yalnızca BLOB Depolama hesapları](../storage/common/storage-create-storage-account.md#blob-storage-accounts) için blob Tetikleyicileri desteklenmez. BLOB Depolama Tetikleyicileri genel amaçlı depolama hesabı gerektirir. Giriş ve çıkış bağlamaları için yalnızca blob depolama hesaplarını kullanabilirsiniz.

## <a name="packages"></a>Paketler

Blob Depolama bağlamaları sağlanan [Microsoft.Azure.WebJobs](http://www.nuget.org/packages/Microsoft.Azure.WebJobs) NuGet paketi. Paket için kaynak kodunu konusu [azure webjobs sdk](https://github.com/Azure/azure-webjobs-sdk/tree/master/src) GitHub depo.

[!INCLUDE [functions-package-auto](../../includes/functions-package-auto.md)]

## <a name="trigger"></a>Tetikleyici

Yeni veya güncelleştirilmiş bir blob algılandığında bir işlev başlatmak için Blob Depolama tetikleyici kullanın. Blob içeriklerini işleve giriş olarak sağlanır.

> [!NOTE]
> Tüketim plan üzerinde bir blob tetikleyici kullanırken olabilir en fazla 10 dakikalık bir gecikmeyle bir işlev uygulaması boşta geçti sonra yeni BLOB'lar işleme. İşlev uygulaması çalışmaya başladıktan sonra BLOB'ları hemen işlenir. Bu ilk gecikmeyi önlemek için aşağıdaki seçeneklerden birini göz önünde bulundurun:
> - Always On özellikli ile bir uygulama hizmeti planını kullan.
> - Başka bir mekanizma, blob adı içeren bir kuyruk iletisi gibi işleme blob tetiklemek için kullanın. Bir örnek için bkz: [blob giriş bağlamaları örnek bu makalenin sonraki bölümlerinde](#input---example).

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , Yazar bir günlük bir blob eklendiğinde veya güncelleştirdiğiniz `samples-workitems` kapsayıcı.

```csharp
[FunctionName("BlobTriggerCSharp")]        
public static void Run([BlobTrigger("samples-workitems/{name}")] Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

Dize `{name}` blob tetikleyici yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns) tetikleme blob dosya adını erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için bkz: [Blob adı desenleri](#trigger---blob-name-patterns) bu makalenin ilerisinde yer.

Hakkında daha fazla bilgi için `BlobTrigger` özniteliği için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama blob tetikleyici gösterir bir *function.json* dosya ve [C# betik (.csx)](functions-reference-csharp.md) bağlama kullanan kod. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `samples-workitems` kapsayıcı.

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

Dize `{name}` blob tetikleyici yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns) tetikleme blob dosya adını erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için bkz: [Blob adı desenleri](#trigger---blob-name-patterns) bu makalenin ilerisinde yer.

Hakkında daha fazla bilgi için *function.json* dosya özellikleri için bkz: [yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

İçin bağlayan bir C# kodu İşte bir `Stream`:

```cs
public static void Run(Stream myBlob, TraceWriter log)
{
   log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
}
```

İçin bağlayan bir C# kodu İşte bir `CloudBlockBlob`:

```cs
#r "Microsoft.WindowsAzure.Storage"

using Microsoft.WindowsAzure.Storage.Blob;

public static void Run(CloudBlockBlob myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name}\nURI:{myBlob.StorageUri}");
}
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama blob tetikleyici gösterir bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlama kullanır. Bir blob eklendiğinde veya güncelleştirdiğiniz işlevi günlüğe yazar `samples-workitems` kapsayıcı.

Burada *function.json* dosyası:

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

Dize `{name}` blob tetikleyici yolunda `samples-workitems/{name}` oluşturur bir [ifade bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns) tetikleme blob dosya adını erişmek için işlev kodu kullanabilirsiniz. Daha fazla bilgi için bkz: [Blob adı desenleri](#trigger---blob-name-patterns) bu makalenin ilerisinde yer.

Hakkında daha fazla bilgi için *function.json* dosya özellikleri için bkz: [yapılandırma](#trigger---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function(context) {
    context.log('Node.js Blob trigger function processed', context.bindings.myBlob);
    context.done();
};
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), bir blob tetikleyici yapılandırmak için aşağıdaki öznitelikler kullanın:

* [BlobTriggerAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobTriggerAttribute.cs)

  Özniteliğin Oluşturucusu izlemek için kapsayıcıyı belirten bir yol dizesini alır ve isteğe bağlı olarak bir [blob adı deseni](#trigger---blob-name-patterns). Bir örneği aşağıda verilmiştir:

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

  Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

* [StorageAccountAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/StorageAccountAttribute.cs)

  Depolama hesabı belirtmek için başka bir yol sağlar. Oluşturucusu depolama bağlantı dizesi içeren bir uygulama ayarı adını alır. Öznitelik parametre, yöntemi veya sınıf düzeyinde uygulanabilir. Aşağıdaki örnek, sınıf ve yöntem düzeyindeki gösterir:

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

Depolama hesabı şu sırayla belirlenir:

* `BlobTrigger` Özniteliğin `Connection` özelliği.
* `StorageAccount` Aynı parametre olarak uygulanan öznitelik `BlobTrigger` özniteliği.
* `StorageAccount` İşlevi için uygulanan öznitelik.
* `StorageAccount` Sınıfına uygulanan öznitelik.
* İşlev uygulaması ("AzureWebJobsStorage" uygulama ayarı) için varsayılan depolama hesabı.

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `BlobTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | ayarlanmalıdır `blobTrigger`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır.|
|**direction** | yok | ayarlanmalıdır `in`. Azure portalında tetikleyici oluşturduğunuzda, bu özelliği otomatik olarak ayarlanır. Özel durumlar belirtilmiştir [kullanım](#trigger---usage) bölümü. |
|**Adı** | yok | Blob içinde işlev kodu temsil eden değişken adı. | 
|**Yol** | **BlobPath** |İzlemek için kapsayıcı.  Olabilir bir [blob adı deseni](#trigger-blob-name-patterns). | 
|**Bağlantı** | **Bağlantı** | Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.<br><br>Bağlantı dizesi için bir genel amaçlı depolama hesabı, olmamalıdır bir [yalnızca blob depolama hesabı](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

C# ve C# betiği aşağıdaki parametre türleri için tetikleyici blob kullanabilirsiniz:

* `Stream`
* `TextReader`
* `string`
* `Byte[]`
* JSON olarak serileştirilebilir bir POCO
* `ICloudBlob`<sup>1</sup>
* `CloudBlockBlob`<sup>1</sup>
* `CloudPageBlob`<sup>1</sup>
* `CloudAppendBlob`<sup>1</sup>

<sup>1</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` C# sınıf kitaplığı'nda.

Bağlama `string`, `Byte[]`, veya POCO yalnızca önerilen blob boyutu küçükse, tüm blob olarak belleğe içeriği yüklenir. Genellikle, kullanılması tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için bkz: [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin ilerisinde yer.

JavaScript'te, kullanarak giriş blob veri erişim `context.bindings.<name from function.json>`.

## <a name="trigger---blob-name-patterns"></a>Tetikleyici - blob adı desenleri

Bir blob adı desende belirtebilirsiniz `path` özelliğinde *function.json* veya `BlobTrigger` özniteliği Oluşturucusu. Ad deseni olabilir bir [filtre veya bağlama ifade](functions-triggers-bindings.md#binding-expressions-and-patterns). Aşağıdaki bölümler örnekler sağlar.

### <a name="get-file-name-and-extension"></a>Dosya adını ve uzantısını Al

Aşağıdaki örnek, blob dosya adını ve uzantısını ayrı olarak bağlamak gösterilmektedir:

```json
"path": "input/{blobname}.{blobextension}",
```
Blob ise *özgün Blob1.txt*, değerlerini `blobname` ve `blobextension` değişkenleri işlevi kodda *özgün Blob1* ve *txt*.

### <a name="filter-on-blob-name"></a>BLOB adına göre filtrele

Aşağıdaki örnek Tetikleyicileri yalnızca BLOB'ları `input` dizesiyle "özgün-" Başlat kapsayıcı:

```json
"path": "input/original-{name}",
```
 
Blob adı ise *özgün Blob1.txt*, değeri `name` işlevi kodda değişkenidir `Blob1`.

### <a name="filter-on-file-type"></a>Dosya türü üzerinde filtreleme

Aşağıdaki örnek tetikler yalnızca *.png* dosyaları:

```json
"path": "samples/{name}.png",
```

### <a name="filter-on-curly-braces-in-file-names"></a>Dosya adlarındaki süslü ayraçlar filtre

Dosya adlarındaki süslü ayraçlar aramak için iki küme ayraçları kullanarak küme ayraçları çıkış. Aşağıdaki örnek filtreler süslü ayraçlar sahip BLOB'lar için:

```json
"path": "images/{{20140101}}-{name}",
```

Blob ise *{20140101}-soundfile.mp3*, `name` işlevi kodda değişken değeri *soundfile.mp3*. 

## <a name="trigger---metadata"></a>Tetikleyici - meta verileri

Blob tetikleyici çeşitli meta veri özelliklerini sağlar. Bu özellikler, diğer bağlamaları bağlama ifadelerinde bir parçası olarak ya da kodunuzu parametre olarak kullanılabilir. Bu değerler olarak aynı semantiklerine sahip [CloudBlob](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.cloudblob?view=azure-dotnet) türü.

|Özellik  |Tür  |Açıklama  |
|---------|---------|---------|
|`BlobTrigger`|`string`|Tetikleyici blob yolu.|
|`Uri`|`System.Uri`|Birincil konumu için blob'un URI.|
|`Properties` |[BlobProperties](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties)|Blob'un Sistem Özellikleri. |
|`Metadata` |`IDictionary<string,string>`|Kullanıcı tanımlı meta veriler blob'a.|

Örneğin, aşağıdaki C# komut dosyası ve JavaScript örnekler yolu kapsayıcı dahil olmak üzere tetikleme blob oturum:

```csharp
public static void Run(string myBlob, string blobTrigger, TraceWriter log)
{
    log.Info($"Full blob path: {blobTrigger}");
} 
```

```javascript
module.exports = function (context, myBlob) {
    context.log("Full blob path:", context.bindingData.blobTrigger);
    context.done();
};
```

## <a name="trigger---blob-receipts"></a>Tetikleyici - blob giriş

Azure işlevleri çalışma zamanı yok blob Tetik işlevi için aynı yeni veya güncelleştirilmiş blob birden çok kez çağrılır sağlar. Belirtilen blob sürümü işlediğinde tutar belirlemek için *blob giriş*.

Azure işlevleri depoları Alındılar adlı bir kapsayıcıda blob *azure Web işleri konakları* işlev uygulamanız için Azure depolama hesabında (uygulama ayarı tarafından tanımlanan `AzureWebJobsStorage`). Bir blob giriş aşağıdaki bilgileri içerir:

* Tetiklenen işlevi ("*&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*", örneğin:"MyFunctionApp.Functions.CopyBlob")
* Kapsayıcı adı
* Blob türü ("BlockBlob" veya "PageBlob")
* Blob adı
* ETag (örneğin bir blob sürüm tanıtıcısını: "0x8D1DC6E70A277EF")

Bir blob yeniden işlemeyerek zorlamak için bu blobundan blob giriş silme *azure Web işleri konakları* kapsayıcı el ile.

## <a name="trigger---poison-blobs"></a>Tetikleyici - zararlı BLOB'ları

Belirli bir blob için bir blob Tetik işlevi başarısız olduğunda, Azure işlevleri, varsayılan olarak 5 kez toplam işlev yeniden dener. 

Tüm 5 deneme başarısız olursa, Azure işlevleri adlı bir depolama kuyruğuna bir ileti ekler *webjobs blobtrigger poison*. Kuyruk iletisini zararlı BLOB'lar için aşağıdaki özellikleri içeren bir JSON nesnesidir:

* FunctionId (biçimde  *&lt;işlevi uygulama adı >*. İşlevler.  *&lt;işlev adı >*)
* BlobType ("BlockBlob" veya "PageBlob")
* Kapsayıcı adı
* BlobName
* ETag (örneğin bir blob sürüm tanıtıcısını: "0x8D1DC6E70A277EF")

## <a name="trigger---concurrency-and-memory-usage"></a>Tetikleyici - eşzamanlılık ve bellek kullanımı

En fazla eş zamanlı işlev çağrılarını tarafından denetlenir blob tetikleyicisi bir sıra dahili olarak kullandığı için [host.json sıraları yapılandırmasında](functions-host-json.md#queues). Varsayılan ayarlar 24 çağrılarına eşzamanlılık sınırlar. Bu sınır ayrı ayrı bir blob tetikleyici kullanan her bir işlev için geçerlidir.

[Tüketim planı](functions-scale.md#how-the-consumption-plan-works) 1,5 GB bellek bir sanal makineye (VM) üzerinde bir işlev uygulaması sınırlar. Bellek işlevleri çalışma ve her eş zamanlı yürütme işlevi örneği tarafından kullanılır. Belleğe tüm blob blob tetiklenen bir işlev, bu işlev tarafından yalnızca BLOB'lar için kullanılan en fazla bellek 24 ise * maksimum blob boyutu. Örneğin, bir işlev uygulaması üç blob tetiklemeli işlevleri ve varsayılan ayarlarıyla 3 * 24 = 72 en fazla VM başına eşzamanlılığı olurdu işlev çağrılarını.

JavaScript işlevleri tüm blob belleğe yüklenemedi ve C# işlevleri yaparsanız, için bağ `string`, `Byte[]`, veya POCO.

## <a name="trigger---polling"></a>Tetiklemek - yoklama

İzlenmekte olan blob kapsayıcısı 10. 000'den fazla BLOB'ları içeriyorsa, işlevleri çalışma zamanı taramaları için yeni veya değiştirilmiş BLOB'lar izlemek için günlük dosyalarını. Bu işlem gecikmelere yol açabilir. Blob oluşturulduktan sonra bir işlev birkaç dakika kadar veya daha uzun tetiklenen değil. Ayrıca, [depolama günlüklerine "en iyi çaba" üzerinde oluşturulan](/rest/api/storageservices/About-Storage-Analytics-Logging) temel. Tüm olayları yakalanır garantisi yoktur. Bazı koşullarda günlükleri eksik olabilir. Daha hızlı veya daha güvenilir blob işleme gerekiyorsa oluşturma göz önünde bulundurun bir [kuyruk iletisi](../storage/queues/storage-dotnet-how-to-use-queues.md) blob oluşturduğunuzda. Ardından bir [sıra tetikleyici](functions-bindings-storage-queue.md) blob işlemek için bir blob tetikleyici yerine. Olay kılavuzunu kullanın başka bir seçenektir; öğretici bkz [otomatikleştirme yeniden boyutlandırma karşıya olay kılavuz kullanarak görüntüleri](../event-grid/resize-images-on-storage-blob-upload-event.md).

## <a name="input"></a>Girdi

BLOB'ları okumak için bir Blob Depolama giriş bağlama kullanın.

## <a name="input---example"></a>Girişi - örnek

Dile özgü örneğe bakın:

* [C#](#input---c-example)
* [C# betik (.csx)](#input---c-script-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example"></a>Giriş - C# örnek

Aşağıdaki örnek bir [C# işlevi](functions-dotnet-class-library.md) sıra tetikleyici ve bir giriş blob bağlama kullanır. Blob'unun boyutu işlevi kaydeder ve kuyruk iletisini blob adını içerir.

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```        

### <a name="input---c-script-example"></a>Giriş - C# kod örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlama gösterir bir *function.json* dosya ve [C# betik (.csx)](functions-reference-csharp.md) bağlamaları kullanan kod. İşlev metin blob bir kopyasını oluşturur. İşlev kopyalamak için blob adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-kopyalama*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği blob adı belirtmek için kullanılan `path` özellikleri:

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlama gösterir bir *function.json* dosya ve [JavaScript kodu](functions-reference-node.md) bağlamaları kullanır. İşlevi bir blobu bir kopyasını oluşturur. İşlev kopyalamak için blob adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-kopyalama*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği blob adı belirtmek için kullanılan `path` özellikleri:

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

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs).

Özniteliğin Oluşturucusu yolunu blob alır ve bir `FileAccess` okuma veya yazma, aşağıdaki örnekte gösterildiği gibi gösteren parametre:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read)] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}

```

Ayarlayabileceğiniz `Connection` özelliğini kullanmak için depolama hesabı aşağıdaki örnekte gösterildiği gibi belirtin:

```csharp
[FunctionName("BlobInput")]
public static void Run(
    [QueueTrigger("myqueue-items")] string myQueueItem,
    [Blob("samples-workitems/{queueTrigger}", FileAccess.Read, Connection = "StorageConnectionAppSetting")] Stream myBlob,
    TraceWriter log)
{
    log.Info($"BlobInput processed blob\n Name:{myQueueItem} \n Size: {myBlob.Length} bytes");
}
```

Kullanabileceğiniz `StorageAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde depolama hesabı belirtin. Daha fazla bilgi için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Blob` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | ayarlanmalıdır `blob`. |
|**direction** | yok | ayarlanmalıdır `in`. Özel durumlar belirtilmiştir [kullanım](#input---usage) bölümü. |
|**Adı** | yok | Blob içinde işlev kodu temsil eden değişken adı.|
|**Yol** |**BlobPath** | Blob yolu. | 
|**Bağlantı** |**Bağlantı**| Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.<br><br>Bağlantı dizesi için bir genel amaçlı depolama hesabı, olmamalıdır bir [yalnızca blob depolama hesabı](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|
|yok | **Erişim** | Okuma yazma veya değiştirilecek olup olmadığını gösterir. |

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

<sup>1</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` C# sınıf kitaplığı'nda.

Bağlama `string` veya `Byte[]` yalnızca tüm blob içeriklerini belleğe yüklenen olarak blob boyutu küçük olup olmadığını önerilir. Genellikle, kullanılması tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için bkz: [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin önceki.

JavaScript'te, blob verileri kullanarak erişim `context.bindings.<name from function.json>`.

## <a name="output"></a>Çıktı

BLOB'ları yazmak için BLOB Depolama çıkış bağlamaları kullanın.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# betik (.csx)](#output---c-script-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnek bir [C# işlevi](functions-dotnet-class-library.md) blob tetikleyici kullanan ve iki blob bağlamaları çıktı. İşlevi bir görüntü blob'u oluşturulmasını tarafından tetiklenen *örnek görüntüleri* kapsayıcı. Görüntü blob'u, küçük ve orta ölçekli kopyalarını oluşturur. 

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

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlama gösterir bir *function.json* dosya ve [C# betik (.csx)](functions-reference-csharp.md) bağlamaları kullanan kod. İşlev metin blob bir kopyasını oluşturur. İşlev kopyalamak için blob adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-kopyalama*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği blob adı belirtmek için kullanılan `path` özellikleri:

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

C# betik kod aşağıdaki gibidir:

```cs
public static void Run(string myQueueItem, string myInputBlob, out string myOutputBlob, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    myOutputBlob = myInputBlob;
}
```

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

<!--Same example for input and output. -->

Aşağıdaki örnek, blob giriş ve çıkış bağlama gösterir bir *function.json* dosya ve [JavaScript kodu] bağlamaları kullanır (işlevleri başvuru node.md). İşlevi bir blobu bir kopyasını oluşturur. İşlev kopyalamak için blob adını içeren bir kuyruk iletisi tarafından tetiklenir. Yeni blob adlı *{originalblobname}-kopyalama*.

İçinde *function.json* dosyası `queueTrigger` meta veri özelliği blob adı belirtmek için kullanılan `path` özellikleri:

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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [BlobAttribute](https://github.com/Azure/azure-webjobs-sdk/blob/master/src/Microsoft.Azure.WebJobs/BlobAttribute.cs).

Özniteliğin Oluşturucusu yolunu blob alır ve bir `FileAccess` okuma veya yazma, aşağıdaki örnekte gösterildiği gibi gösteren parametre:

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

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

Kullanabileceğiniz `StorageAccount` öznitelik sınıfı, yöntemi veya parametre düzeyinde depolama hesabı belirtin. Daha fazla bilgi için bkz: [tetikleyici - öznitelikleri](#trigger---attributes).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `Blob` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** | yok | ayarlanmalıdır `blob`. |
|**direction** | yok | Ayarlanmalıdır `out` bir çıktı bağlama için. Özel durumlar belirtilmiştir [kullanım](#output---usage) bölümü. |
|**Adı** | yok | Blob içinde işlev kodu temsil eden değişken adı.  Kümesine `$return` işlevi dönüş değeri başvurmak için.|
|**Yol** |**BlobPath** | Blob yolu. | 
|**Bağlantı** |**Bağlantı**| Bu bağlama için kullanılacak depolama bağlantı dizesi içeren bir uygulama ayarı adı. Uygulama ayarı adı "AzureWebJobs" ile başlıyorsa, yalnızca adını buraya kalanı belirtebilirsiniz. Örneğin, ayarlarsanız `connection` bir uygulama ayarı "AzureWebJobsMyStorage." adlı "MyStorage" işlevleri çalışma zamanı arar. Bırakır `connection` boş işlevleri çalışma zamanı varsayılan depolama bağlantı dizesi adlı uygulama ayarını kullanan `AzureWebJobsStorage`.<br><br>Bağlantı dizesi için bir genel amaçlı depolama hesabı, olmamalıdır bir [yalnızca blob depolama hesabı](../storage/common/storage-create-storage-account.md#blob-storage-accounts).|
|yok | **Erişim** | Okuma yazma veya değiştirilecek olup olmadığını gösterir. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

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

<sup>1</sup> "içindeki" bağlamayı gerektirir `direction` içinde *function.json* veya `FileAccess.Read` C# sınıf kitaplığı'nda.

<sup>2</sup> "ınout" bağlama gerektirir `direction` içinde *function.json* veya `FileAccess.ReadWrite` C# sınıf kitaplığı'nda.

Zaman uyumsuz işlevlerde dönüş değerini kullanın veya `IAsyncCollector` yerine bir `out` parametresi.

Bağlama `string` veya `Byte[]` yalnızca tüm blob içeriklerini belleğe yüklenen olarak blob boyutu küçük olup olmadığını önerilir. Genellikle, kullanılması tercih edilir bir `Stream` veya `CloudBlockBlob` türü. Daha fazla bilgi için bkz: [eşzamanlılık ve bellek kullanımı](#trigger---concurrency-and-memory-usage) bu makalenin önceki.


JavaScript'te, blob verileri kullanarak erişim `context.bindings.<name from function.json>`.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama |  Başvuru |
|---|---|
| Blob | [BLOB hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/blob-service-error-codes) |
| BLOB, tablo, kuyruk |  [Depolama hata kodları](https://docs.microsoft.com/rest/api/storageservices/fileservices/common-rest-api-error-codes) |
| BLOB, tablo, kuyruk |  [Sorun giderme](https://docs.microsoft.com/rest/api/storageservices/fileservices/troubleshooting-api-operations) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir Blob Depolama tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-storage-blob-triggered-function.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
