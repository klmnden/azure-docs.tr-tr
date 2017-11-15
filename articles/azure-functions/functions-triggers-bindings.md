---
title: "Tetikleyicileri ve bağlamaları Azure işlevlerinde ile çalışırsınız | Microsoft Docs"
description: "Kod yürütmeyi çevrimiçi olayları ve bulut tabanlı hizmetlere bağlanmak için Azure işlevleri Tetikleyicileri ve bağlamaları kullanmayı öğrenin."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari"
ms.assetid: cbc7460a-4d8a-423f-a63e-1cd33fef7252
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/30/2017
ms.author: glenga
ms.openlocfilehash: 7d22a6749216486de6132a6d39e2dcf683d0e678
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure işlevleri Tetikleyicileri ve bağlamaları kavramları
Azure işlevleri aracılığıyla Azure ve diğer hizmetleri olaylara yanıt kodu yazmanızı sağlar *Tetikleyicileri* ve *bağlamaları*. Bu makalede Tetikleyicileri kavramsal genel bakış olduğundan ve tüm bağlamaları desteklenen programlama dilleri. Tüm bağlamaları için ortak olan özellikleri aşağıda açıklanmıştır.

## <a name="overview"></a>Genel Bakış

Tetikleyicileri ve bağlamaları: bir işlev nasıl çağrıldığını tanımlamanın bildirim temelli yolu ve hangi veri ile birlikte çalışır. A *tetikleyici* bir işlev nasıl çağrıldığını tanımlar. Bir işlev tam olarak bir tetikleyici olması gerekir. Tetikleyiciler genellikle işlevi tetiklenen yükü olduğu veri ilişkilendirdiniz. 

Giriş ve çıkış *bağlamaları* kodunuzu içindeki verileri bağlanmak için bildirim temelli bir yolunu sağlar. Benzer şekilde tetikleyicileri, bağlantı dizeleri ve diğer özellikleri işlevi yapılandırmanızda belirtin. Bağlamaları isteğe bağlıdır ve bir işlev birden fazla giriş varsa ve bağlamaları çıktı. 

Tetikleyicileri ve bağlamaları kullanarak, daha fazla genel ve mu değil stillerinizin kod Hizmetleri ayrıntılarını yazabilirsiniz ile hangi etkileşim kurar. İşlevi kodunuz için giriş değerleri Hizmetleri yalnızca haline gelen veri. (Yeni bir satır Azure tablo Depolama'da oluşturma) gibi başka bir hizmete veri çıkışı için yönteminin dönüş değerini kullanın. Ya da birden çok değer çıktı gerekiyorsa, bir yardımcı nesnesi kullanın. Tetikleyicileri ve bağlamaları sahip bir **adı** bir tanımlayıcıdır özelliği kodunuzda bağlama erişmek için kullanabilirsiniz.

Tetikleyicileri ve bağlamaları'nda yapılandırabileceğiniz **tümleştir** Azure işlevleri portalındaki sekmesindedir. Adlı bir dosya UI perde arkasında değiştirir *function.json* işlevi dizindeki dosyayı. Bu dosyayı değiştirerek düzenleyebilirsiniz **Gelişmiş Düzenleyici**.

Aşağıdaki tabloda tetikleyiciler ve Azure işlevleri ile desteklenen bağlamaları gösterir. 

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

### <a name="example-queue-trigger-and-table-output-binding"></a>Örnek: bağlama sırası tetikleyici ve tablo çıktısı

Her Azure kuyruk depolamada yeni bir ileti görüntülendiğinde, Azure Table depolama alanına yeni bir satır yazmak istediğinizi varsayalım. Bu senaryo, bir Azure kuyruk kullanarak uygulanabilir tetikleyici ve bir Azure Table Storage çıkış bağlama. 

Bir Azure kuyruk depolama tetikleyicisi aşağıdaki bilgileri gerektirir **tümleştir** sekmesi:

* Azure kuyruk depolama için Azure depolama hesabı bağlantı dizesi içeren uygulama ayarı adı
* Kuyruk adı
* Kuyruk iletinin içeriğini okuma için kodunuzu tanımlayıcıda `order`.

Azure Table depolama alanına yazmak için aşağıdaki Ayrıntılar ile bir çıktı bağlama kullanın:

* Azure tablo depolaması için Azure depolama hesabı bağlantı dizesi içeren uygulama ayarı adı
* Tablo adı
* Çıktı öğeleri veya dönüş değeri işlevinden oluşturmak için kodunuzda tanımlayıcısı.

Bağlamaları bağlantı dizeleri kullanmak en iyi zorlamak için uygulama ayarlarında depolanan değerleri alıştırma *function.json* değil hizmeti gizlilik içeren ve bunun yerine yalnızca adlarını uygulama ayarları içerir.

Sonra kodunuzu Azure Storage ile tümleştirmek için sağlanan tanımlayıcılar kullanın.

```cs
#r "Newtonsoft.Json"

using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table Storage
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

Burada *function.json* önceki kod karşılık gelir. Aynı yapılandırmayı, işlev uygulaması dilinden bağımsız olarak kullanılabileceğini unutmayın.

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
Görüntülemek ve içeriğini düzenlemek için *function.json* Azure portalında tıklatın **Gelişmiş Düzenleyici** seçeneği **tümleştir** işlevinizi sekmesinde.

Daha fazla kod örnekleri ve Azure Storage ile tümleştirme hakkında bilgi için bkz: [Azure işlevleri Tetikleyicileri ve bağlamaları Azure Storage için](functions-bindings-storage.md).

### <a name="binding-direction"></a>Bağlama yönü

Tüm Tetikleyicileri ve bağlamaları sahip bir `direction` özelliği:

- Tetikleyiciler için her zaman yönüdür`in`
- Giriş ve çıkış bağlamaları kullanmak `in` ve`out`
- Özel bir yön bazı bağlamaları Destek `inout`. Kullanırsanız `inout`, yalnızca **Gelişmiş Düzenleyici** kullanılabilir **tümleştir** sekmesi.

## <a name="using-the-function-return-type-to-return-a-single-output"></a>İşlev dönüş türü tek bir çıktı döndürmek için kullanma

Önceki örnekte işlevi dönüş değeri özel name parametresi kullanılarak elde edilir bir bağlama çıkış sağlamak için nasıl kullanılacağını gösterir `$return`. (Bu yalnızca C#, JavaScript ve F # gibi bir dönüş değerine sahip dillerde desteklenir.) Bir işlev birden çok çıktı bağlaması içeriyorsa `$return` çıkış bağlamaları yalnızca biri için. 

```json
// excerpt of function.json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

C#, JavaScript ve F # çıkış bağlamalarla türleri kullanılır Göster aşağıda örnekler nasıl döndür.

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

.NET içinde türleri giriş verileri için veri türünü tanımlamak için kullanın. Örneğin, kullanın `string` sıra tetikleyici, ikili ve bir POCO nesne seri durumdan özel bir tür olarak okumak için bir bayt dizisi metnini bağlamak için.

JavaScript gibi dinamik olarak yazılan diller için kullanmak `dataType` bağlama tanımında özelliği. Örneğin, bir HTTP isteği ikili biçimde içeriğini okumak için türü kullanın `binary`:

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

## <a name="trigger-metadata-properties"></a>Tetikleyici meta veri özellikleri

Bir tetikleyici (örneğin, bir işlev tetiklenen kuyruk iletisini) tarafından sağlanan veri yükü yanı sıra birçok Tetikleyicileri ek meta veri değerleri sağlayın. Bu değerleri C# ve F # veya özelliklerinde giriş parametresi olarak kullanılabilir `context.bindings` JavaScript nesne. 

Örneğin, bir Azure depolama kuyruğu tetikleyicisi aşağıdaki özellikleri destekler:

* QueueTrigger - geçerli bir dize, ileti içeriği tetikleme
* DequeueCount
* expirationTime
* Kimlik
* InsertionTime
* NextVisibleTime
* PopReceipt

Her tetikleyici için meta veri özelliklerini ayrıntılarını karşılık gelen başvuru konusunda açıklanmıştır. Belgeleri de kullanılabilir de **tümleştir** portal sekmesinde, **belgelerine** bağlama yapılandırma alanı aşağıdaki bölümüne.  

Örneğin, bazı gecikmeler BLOB Tetikleyicileri sahip olduğundan, işlevinizi çalıştırmak için bir sıra tetikleyici kullanabilirsiniz (bkz [Blob Depolama tetikleyici](functions-bindings-storage-blob.md#blob-storage-trigger)). Kuyruk iletisini üzerinde tetiklemek için blob filename içerecektir. Kullanarak `queueTrigger` meta veri özelliği, tüm yapılandırmanızı yerine kodunuzu bu davranış belirtebilirsiniz.

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

Bir tetikleyiciden meta veri özelliklerini de kullanılabilir bir *ifade bağlama* aşağıdaki bölümde açıklandığı gibi başka bir bağlama için.

## <a name="binding-expressions-and-patterns"></a>İfadeler ve desenler bağlama

Tetikleyicileri ve bağlamaları en güçlü özelliklerden biridir *ifadeleri bağlama*. İçinde bağlama, diğer bağlamaları veya kodunuzu daha sonra kullanılabilir düzeni ifadeler tanımlayabilirsiniz. Tetikleyici meta verileri göster önceki bölümde örnek olarak ifadeleri, bağlama de kullanılabilir.

Örneğin, belirli bir blob depolama kapsayıcısını, benzer görüntülerinde yeniden boyutlandırmak istediğinizi varsayalım **görüntü Boyutlandır** şablonunda **yeni işlev** sayfası. Git **yeni işlev** dil -> **C#** senaryo -> **örnekleri** -> **ImageResizer CSharp**. 

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

Dikkat `filename` parametresi hem blob tetikleyicinizin tanımını yanı sıra blob kullanılır bağlama çıktı. Bu parametre, işlev kodu de kullanılabilir.

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


### <a name="random-guids"></a>Rastgele GUID'leri
Azure işlevleri aracılığıyla GUID'ler, bağlama oluşturmak için bir kolaylık sözdizimi sağlar `{rand-guid}` ifade bağlama. Aşağıdaki örnekte bir benzersiz blob adı oluşturmak için bu kullanır: 

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="current-time"></a>Geçerli saat

Bağlama deyimi kullanabilirsiniz `DateTime`, çözümler için `DateTime.UtcNow`.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{DateTime}"
}
```

## <a name="bind-to-custom-input-properties-in-a-binding-expression"></a>Bir bağlama ifadesinde özel giriş özellikleri bağlayın

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

C# ve diğer .NET dilleri bildirim temelli bağlamaları aksine bir kesinlik temelli bağlama desen kullanabilirsiniz *function.json*. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde kullanışlıdır. Daha fazla bilgi için bkz: [kesinlik temelli bağlamaları aracılığıyla çalışma zamanında bağlama](functions-reference-csharp.md#imperative-bindings) C# Geliştirici Başvurusu.

## <a name="next-steps"></a>Sonraki adımlar
Belirli bir bağlama hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [HTTP ve web kancaları](functions-bindings-http-webhook.md)
- [Zamanlayıcı](functions-bindings-timer.md)
- [Kuyruk depolama](functions-bindings-storage-queue.md)
- [Blob depolama](functions-bindings-storage-blob.md)
- [Tablo depolama](functions-bindings-storage-table.md)
- [Olay Hub’ı](functions-bindings-event-hubs.md)
- [Service Bus](functions-bindings-service-bus.md)
- [Azure Cosmos DB](functions-bindings-documentdb.md)
- [Microsoft Graph](functions-bindings-microsoft-graph.md)
- [SendGrid](functions-bindings-sendgrid.md)
- [Twilio](functions-bindings-twilio.md)
- [Notification Hubs](functions-bindings-notification-hubs.md)
- [Mobile Apps](functions-bindings-mobile-apps.md)
- [Dış dosya](functions-bindings-external-file.md)
