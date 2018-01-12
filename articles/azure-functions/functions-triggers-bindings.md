---
title: "Tetikleyicileri ve bağlamaları Azure işlevlerinde"
description: "Kod yürütmeyi çevrimiçi olayları ve bulut tabanlı hizmetlere bağlanmak için Azure işlevleri Tetikleyicileri ve bağlamaları kullanmayı öğrenin."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: a122271b5fdffd9db33a7dca5908e15f002041d7
ms.sourcegitcommit: 71fa59e97b01b65f25bcae318d834358fea5224a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2018
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure işlevleri Tetikleyicileri ve bağlamaları kavramları

Bu makalede Tetikleyicileri ve bağlamaları Azure işlevlerinde kavramsal bir genel bakıştır. Tüm bağlamalar ve tüm desteklenen diller için ortak olan özellikleri aşağıda açıklanmıştır.

## <a name="overview"></a>Genel Bakış

A *tetikleyici* bir işlev nasıl çağrıldığını tanımlar. Bir işlev tam olarak bir tetikleyici olması gerekir. Tetikleyiciler genellikle işlevi tetiklenen yükü olduğu veri ilişkilendirdiniz.

Giriş ve çıkış *bağlamaları* kodunuzu içindeki verileri bağlanmak için bildirim temelli bir yolunu sağlar. Bağlamaları isteğe bağlıdır ve bir işlev birden fazla giriş varsa ve bağlamaları çıktı. 

Tetikleyicileri ve bağlamaları cmdlet'e kod çalıştığınız Hizmetleri ayrıntılarını engellemenize olanak tanır. Verileri (örneğin, bir kuyruk iletisi içeriği), işlevi işlevi parametreleri alır. İşlev dönüş değerini kullanarak (örneğin, bir kuyruk iletisi oluşturmak için) veri gönderme bir `out` parametresi veya [Toplayıcı nesnesi](functions-reference-csharp.md#writing-multiple-output-values).

Azure portalını kullanarak işlevleri geliştirirken Tetikleyicileri ve bağlamaları yapılandırılan bir *function.json* dosya. Portal, bu yapılandırma için bir kullanıcı Arabirimi sağlar ancak doğrudan değiştirerek dosyasını düzenleyebilirsiniz **Gelişmiş Düzenleyici**.

Sınıf kitaplığı oluşturmak için Visual Studio kullanarak işlevleri geliştirirken, Tetikleyicileri ve bağlamaları yöntemleri ve öznitelikleri ile parametreleri tasarlayarak yapılandırın.

## <a name="supported-bindings"></a>Desteklenen bağlamaları

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Bağlamaları önizlemede veya üretim kullanımı için onaylanan olduğu hakkında bilgi için bkz: [desteklenen diller](supported-languages.md).

## <a name="example-queue-trigger-and-table-output-binding"></a>Örnek: bağlama sırası tetikleyici ve tablo çıktısı

Her Azure kuyruk depolama alanında yeni bir ileti görüntülendiğinde, Azure Table depolama alanına yeni bir satır yazmak istediğinizi varsayalım. Bu senaryo, bir Azure kuyruk kullanarak uygulanabilir depolama tetikleyici ve bir Azure Table depolama çıkış bağlama. 

Burada bir *function.json* bu senaryo için dosya. 

```json
{
  "bindings": [
    {
      "name": "order",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "name": "$return",
      "type": "table",
      "direction": "out",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

İlk öğe `bindings` kuyruk depolama tetikleyici dizisidir. `type` Ve `direction` özelliklerini tetikleyici tanımlayın. `name` Özelliği kuyruk iletisi içeriği alacak işlev parametresi tanımlar. İzlemek için sırasının adı olarak `queueName`, ve bağlantı dizesi tarafından tanımlanan uygulama ayarı `connection`.

İkinci öğe `bindings` dizidir Azure Table Storage bağlama çıktı. `type` Ve `direction` özelliklerini bağlama tanımlayın. `name` Özelliği, nasıl işlevi yeni tablo satırı bu durumda işlevin dönüş değerini kullanarak sağlayacak belirtir. Tablonun adını bulunduğu `tableName`, ve bağlantı dizesi tarafından tanımlanan uygulama ayarı `connection`.

Görüntülemek ve içeriğini düzenlemek için *function.json* Azure portalında tıklatın **Gelişmiş Düzenleyici** seçeneği **tümleştir** işlevinizi sekmesinde.

> [!NOTE]
> Değeri `connection` bağlantı dizesi, bağlantı dizesinin kendisini içeren bir uygulama ayarı adı. Bağlamaları bağlantısı kullanmak en iyi zorlamak için uygulama ayarlarında depolanan dizeleri alıştırma *function.json* hizmet gizli içermiyor.

Aşağıda, bu tetikleyici ve bağlama ile çalışan bir C# kodu verilmiştir. Kuyruk iletisi içeriği sağlayan parametresinin adı olduğuna dikkat edin `order`; çünkü bu ad gereklidir `name` özellik değeri *function.json* olduğu`order` 

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, TraceWriter log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

JavaScript işlevi ile aynı function.json dosyası kullanılabilir:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

Bir sınıf kitaplığı, aynı tetikleyici ve bağlama bilgileri &mdash; kuyruk ve tablo adları, depolama hesapları, işlev giriş ve çıkış parametreleri &mdash; öznitelikleri tarafından sağlanır:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         TraceWriter log)
     {
         return new Person() {
                 PartitionKey = "Orders",
                 RowKey = Guid.NewGuid().ToString(),
                 Name = order["Name"].ToString(),
                 MobileNumber = order["MobileNumber"].ToString() };
     }
 }

 public class Person
 {
     public string PartitionKey { get; set; }
     public string RowKey { get; set; }
     public string Name { get; set; }
     public string MobileNumber { get; set; }
 }
```

## <a name="binding-direction"></a>Bağlama yönü

Tüm Tetikleyicileri ve bağlamaları sahip bir `direction` özelliğinde *function.json* dosyası:

- Tetikleyiciler için her zaman yönüdür`in`
- Giriş ve çıkış bağlamaları kullanmak `in` ve`out`
- Özel bir yön bazı bağlamaları Destek `inout`. Kullanırsanız `inout`, yalnızca **Gelişmiş Düzenleyici** kullanılabilir **tümleştir** sekmesi.

Kullandığınızda [öznitelikleri bir sınıf kitaplığı'nda](functions-dotnet-class-library.md) Tetikleyicileri ve bağlamaları yapılandırmak için yönü bir öznitelik oluşturucuda sağlanan veya parametre türünden sonuçlandı.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>İşlev dönüş türü tek bir çıktı döndürmek için kullanma

Önceki örnekte işlevi dönüş değerini belirtilen bir bağlama çıkış sağlamak için nasıl kullanılacağını gösterir *function.json* özel değerini kullanarak `$return` için `name` özelliği. (Bu yalnızca C# kod, JavaScript ve F # gibi bir dönüş değerine sahip dillerde desteklenir.) Bir işlev birden çok çıktı bağlaması içeriyorsa `$return` çıkış bağlamaları yalnızca biri için. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

C# kod, JavaScript ve F # çıkış bağlamalarla türleri kullanılır Göster aşağıda örnekler nasıl döndür.

```cs
// C# example: use method return value for output binding
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
// C# example: async method, using return value for output binding
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

```javascript
// JavaScript: return a value in the second parameter to context.done
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

```fsharp
// F# example: use return value for output binding
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

## <a name="binding-datatype-property"></a>DataType özelliği için bağlama

.NET, parametre türü veri türü için giriş verileri tanımlamak için kullanın. Örneğin, kullanın `string` sıra tetikleyici, ikili ve bir POCO nesne seri durumdan özel bir tür olarak okumak için bir bayt dizisi metnini bağlamak için.

JavaScript gibi dinamik olarak yazılan diller için kullanmak `dataType` özelliğinde *function.json* dosya. Örneğin, bir HTTP isteği ikili biçimde içeriğini okumak için ayarlar `dataType` için `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Diğer seçenekler için `dataType` olan `stream` ve `string`.

## <a name="resolving-app-settings"></a>Uygulama ayarlarını çözümleme

En iyi uygulama, parolaları ve bağlantı dizeleri yapılandırma dosyalarını yerine uygulama ayarları kullanılarak yönetilmelidir. Bu, bu Sırları erişimi sınırlar ve depolamak güvenli hale getirir *function.json* ortak kaynak denetimi havuzunda.

Uygulama ayarları da ortamına bağlı yapılandırmasını değiştirmek istediğinizde kullanışlıdır. Örneğin, bir test ortamında, farklı bir sıra veya blob depolama kapsayıcısı izlemek isteyebilir.

Uygulama ayarları, yüzde işaretleri gibi içine alınmış bir değer her çözümlendirinden `%MyAppSetting%`. Unutmayın `connection` Tetikleyicileri ve bağlamaları özelliği özel bir durumdur ve uygulama ayarlarının değerleri otomatik olarak çözer. 

Aşağıdaki örnek bir uygulama ayarını kullanan bir Azure kuyruk depolama tetikleyicisi olduğundan `%input-queue-name%` üzerinde tetiklemek için sıra tanımlamak için.

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
    TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
}
```

## <a name="trigger-metadata-properties"></a>Tetikleyici meta veri özellikleri

Bir tetikleyici (örneğin, bir işlev tetiklenen kuyruk iletisini) tarafından sağlanan veri yükü yanı sıra birçok Tetikleyicileri ek meta veri değerleri sağlayın. Bu değerleri C# ve F # veya özelliklerinde giriş parametresi olarak kullanılabilir `context.bindings` JavaScript nesne. 

Örneğin, bir Azure kuyruk depolama tetikleyicisi aşağıdaki özellikleri destekler:

* QueueTrigger - geçerli bir dize, ileti içeriği tetikleme
* DequeueCount
* expirationTime
* Kimlik
* InsertionTime
* NextVisibleTime
* PopReceipt

Bu meta veri değerleri erişilebilir *function.json* dosya özellikleri. Örneğin, bir sıra tetikleyici kullanmanız ve kuyruk iletisini okumak istediğiniz bir blob adını içeren varsayalım. İçinde *function.json* kullanabileceğiniz dosyası `queueTrigger` blob meta veri özellik `path` özelliği, aşağıdaki örnekte gösterildiği gibi:

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

Her tetikleyici için meta veri özelliklerini ayrıntılarını karşılık gelen başvurusu makalesinde açıklanmıştır. Bir örnek için bkz: [sıra tetikleyici meta verileri](functions-bindings-storage-queue.md#trigger---message-metadata). Belgeleri de kullanılabilir de **tümleştir** portal sekmesinde, **belgelerine** bağlama yapılandırma alanı aşağıdaki bölümüne.  

## <a name="binding-expressions-and-patterns"></a>İfadeler ve desenler bağlama

Tetikleyicileri ve bağlamaları en güçlü özelliklerden biridir *ifadeleri bağlama*. Bir bağlama için bir yapılandırmada, daha sonra diğer bağlamaları veya kodunuzu kullanılabilir düzeni ifadeler tanımlayabilirsiniz. Tetikleyici meta verileri önceki bölümde gösterildiği gibi bağlama ifadelerinde de kullanılabilir.

Örneğin, bir benzer şekilde belirli blob depolama kapsayıcısını görüntülerinde yeniden boyutlandırmak istediğinizi varsayalım **görüntü Boyutlandır** şablonunda **yeni işlev** sayfasında Azure Portalı'nın (bkz **örnekleri**  senaryosu). 

Burada *function.json* tanımı:

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

Dikkat `filename` parametresi hem blob tetikleyicinizin tanımını hem de blob kullanılır bağlama çıktı. Bu parametre, işlev kodu de kullanılabilir.

```csharp
// C# example of binding to {filename}
public static void Run(Stream image, string filename, Stream imageSmall, TraceWriter log)  
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
} 
```

<!--TODO: add JavaScript example -->
<!-- Blocked by bug https://github.com/Azure/Azure-Functions/issues/248 -->

Sınıf kitaplıkları öznitelikleri bağlama ifadeleri ve desenler aynı kullanabilme uygular. Örneğin, bir sınıf kitaplığı'nda işlevi yeniden boyutlandırma bir görüntü şöyledir:

```csharp
[FunctionName("ResizeImage")]
[StorageAccount("AzureWebJobsStorage")]
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

### <a name="create-guids"></a>GUID oluşturma

`{rand-guid}` İfade bağlama bir GUID oluşturur. Aşağıdaki örnek, bir benzersiz blob adı oluşturmak için bir GUID kullanır: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Geçerli zaman

Bağlama ifadesi `DateTime` çözümler `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties"></a>Özel giriş özelliklerine bağlama

İfadeler bağlama tetikleyici yükü kendisini tanımlanan özellikler başvuruda bulunabilir. Örneğin, dinamik olarak bir Web kancası sağlanan bir dosya adı gelen bir blob depolama dosyasına bağlamak isteyebilirsiniz.

Örneğin, aşağıdaki *function.json* adlı bir özellik kullanır `BlobName` tetikleyici yükü gelen:

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

C# ve F # içinde Bunu başarmak için tetikleyici yükünde seri durumdan alanlarını tanımlayan bir POCO tanımlamanız gerekir.

```csharp
using System.Net;

public class BlobInfo
{
    public string BlobName { get; set; }
}
  
public static HttpResponseMessage Run(HttpRequestMessage req, BlobInfo info, string blobContents)
{
    if (blobContents == null) {
        return req.CreateResponse(HttpStatusCode.NotFound);
    } 

    return req.CreateResponse(HttpStatusCode.OK, new {
        data = $"{blobContents}"
    });
}
```

JavaScript özellikleri doğrudan kullanabilirsiniz ve JSON seri durumundan çıkarma otomatik olarak gerçekleştirilir.

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

## <a name="configuring-binding-data-at-runtime"></a>Çalışma zamanında veri bağlama yapılandırma

C# ve diğer .NET dilleri bildirim temelli bağlamaları aksine bir kesinlik temelli bağlama desen kullanabilirsiniz *function.json* ve öznitelikleri. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde kullanışlıdır. Daha fazla bilgi için bkz: [kesinlik temelli bağlamaları aracılığıyla çalışma zamanında bağlama](functions-reference-csharp.md#imperative-bindings) C# Geliştirici Başvurusu.

## <a name="functionjson-file-schema"></a>Function.JSON dosyası şeması

*Function.json* dosyası şeması şu adreste [http://json.schemastore.org/function](http://json.schemastore.org/function).

## <a name="next-steps"></a>Sonraki adımlar

Belirli bir bağlama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [HTTP ve web kancaları](functions-bindings-http-webhook.md)
- [Zamanlayıcı](functions-bindings-timer.md)
- [Kuyruk depolama](functions-bindings-storage-queue.md)
- [Blob depolama](functions-bindings-storage-blob.md)
- [Tablo depolama](functions-bindings-storage-table.md)
- [Olay Hub’ı](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-cosmosdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Dış dosya](functions-bindings-external-file.md)
