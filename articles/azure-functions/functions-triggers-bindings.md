---
title: Tetikleyicileri ve bağlamaları Azure işlevlerinde
description: Kod yürütmeyi çevrimiçi olayları ve bulut tabanlı hizmetlere bağlanmak için Azure işlevleri Tetikleyicileri ve bağlamaları kullanmayı öğrenin.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/24/2018
ms.author: tdykstra
ms.openlocfilehash: 305f7a54e290b8628401c21f033f8be7017d4a91
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083874"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure işlevleri Tetikleyicileri ve bağlamaları kavramları

Bu makalede Tetikleyicileri ve bağlamaları Azure işlevlerinde kavramsal bir genel bakıştır. Tüm bağlamalar ve tüm desteklenen diller için ortak olan özellikleri aşağıda açıklanmıştır.

## <a name="overview"></a>Genel Bakış

A *tetikleyici* bir işlev nasıl çağrıldığını tanımlar. Bir işlev tam olarak bir tetikleyici olması gerekir. Tetikleyiciler genellikle işlevi tetiklenen yükü olduğu veri ilişkilendirdiniz.

Giriş ve çıkış *bağlamaları* kodunuzu içindeki verileri bağlanmak için bildirim temelli bir yolunu sağlar. Bağlamaları isteğe bağlıdır ve bir işlev birden fazla giriş varsa ve bağlamaları çıktı. 

Tetikleyicileri ve bağlamaları cmdlet'e kod çalıştığınız Hizmetleri ayrıntılarını engellemenize olanak tanır. İşlevinizi verileri (örneğin, bir kuyruk iletisi içeriği) işlevi parametreleri alır. İşlev dönüş değerini kullanarak (örneğin, bir kuyruk iletisi oluşturmak için) veri gönderme bir `out` parametresi veya [Toplayıcı nesnesi](functions-reference-csharp.md#writing-multiple-output-values).

Azure portalını kullanarak işlevleri geliştirirken Tetikleyicileri ve bağlamaları yapılandırılan bir *function.json* dosya. Portal, bu yapılandırma için bir kullanıcı Arabirimi sağlar ancak doğrudan değiştirerek dosyasını düzenleyebilirsiniz **Gelişmiş Düzenleyici**.

Sınıf kitaplığı oluşturmak için Visual Studio kullanarak işlevleri geliştirirken, Tetikleyicileri ve bağlamaları yöntemleri ve öznitelikleri ile parametreleri tasarlayarak yapılandırın.

## <a name="example-trigger-and-binding"></a>Örnek tetikleyici ve bağlama

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

İlk öğe `bindings` kuyruk depolama tetikleyici dizisidir. `type` Ve `direction` özelliklerini tetikleyici tanımlayın. `name` Özelliği sıraya ileti içeriğini alan işlev parametresi tanımlar. İzlemek için sırasının adı olarak `queueName`, ve bağlantı dizesi tarafından tanımlanan uygulama ayarı `connection`.

İkinci öğe `bindings` dizidir Azure Table Storage bağlama çıktı. `type` Ve `direction` özelliklerini bağlama tanımlayın. `name` Dönüş değeri işlevini kullanarak bu durumda, özellik belirtir nasıl yeni tablo satırı işlevi sağlar. Tablonun adını bulunduğu `tableName`, ve bağlantı dizesi tarafından tanımlanan uygulama ayarı `connection`.

Görüntülemek ve içeriğini düzenlemek için *function.json* Azure portalında tıklatın **Gelişmiş Düzenleyici** seçeneği **tümleştir** işlevinizi sekmesinde.

> [!NOTE]
> Değeri `connection` bağlantı dizesi, bağlantı dizesinin kendisini içeren bir uygulama ayarı adı. Bağlamaları bağlantısı kullanmak en iyi zorlamak için uygulama ayarlarında depolanan dizeleri alıştırma *function.json* hizmet gizli içermiyor.

Aşağıda, bu tetikleyici ve bağlama ile çalışan bir C# kodu verilmiştir. Kuyruk iletisi içeriği sağlayan parametresinin adı olduğuna dikkat edin `order`; çünkü bu ad gereklidir `name` özellik değeri *function.json* olduğu `order` 

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

Bir sınıf kitaplığı, aynı tetikleyici ve bağlama bilgileri &mdash; kuyruk ve tablo adları, depolama hesapları, işlev giriş ve çıkış parametreleri &mdash; function.json dosya yerine öznitelikleri tarafından sağlanır. Bir örneği aşağıda verilmiştir:

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

## <a name="supported-bindings"></a>Desteklenen bağlamaları

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Bağlamaları önizlemede veya üretim kullanımı için onaylanan olduğu hakkında bilgi için bkz: [desteklenen diller](supported-languages.md).

## <a name="register-binding-extensions"></a>Bağlama uzantılarını kaydetme

Bazı geliştirme ortamlarında, açıkça zorunda *kaydetmek* kullanmak istediğiniz bir bağlama. Bağlama uzantıları NuGet paketlerini sağlanır ve uzantı kaydetmek için bir paket yükleyin. Aşağıdaki tabloda, ne zaman ve nasıl bağlama uzantıları kaydetmek gösterir.

|Geliştirme ortamı |Kayıt<br/> işlevlerinde 1.x  |Kayıt<br/> işlevlerinde 2.x  |
|---------|---------|---------|
|Azure portalına|Automatic|[Otomatik istemiyle](#azure-portal-development)|
|Yerel Azure işlevleri çekirdek araçlarını kullanma|Automatic|[Çekirdek araçları CLI komutları kullanın](#local-development-azure-functions-core-tools)|
|Visual Studio 2017 kullanarak C# sınıf kitaplığı|[NuGet araçlarını kullanma](#c-class-library-with-visual-studio-2017)|[NuGet araçlarını kullanma](#c-class-library-with-visual-studio-2017)|
|Visual Studio kodu kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanın](#c-class-library-with-visual-studio-code)|

Bunlar otomatik olarak tüm sürümleri ve ortamlara kayıtlı olduğundan, açık kayıt gerektirmeyen özel durumlar aşağıdaki bağlama türleri şunlardır: HTTP, Zamanlayıcı ve Azure Storage (BLOB, kuyruklar ve tablolar). 

### <a name="azure-portal-development"></a>Azure portal geliştirme

Bu bölüm, yalnızca işlevleriyle geçerlidir 2.x. Bağlama uzantıları işlevlerde açıkça kaydedilmesi gerekmez 1.x.

Bir işlev oluşturun veya bir bağlama eklemek, tetikleyici veya bağlama uzantısı kayıt gerektirdiğinde istenir. Tıklayarak komutuna yanıt **yükleme** uzantısını kaydetmek için. Yüklemesi tüketim plan üzerinde 10 dakikaya kadar sürebilir.

Yalnızca bir kez verilen işlev uygulaması için her bir uzantı yüklemeniz gerekir. 

### <a name="local-development-azure-functions-core-tools"></a>Yerel geliştirme Azure işlevleri çekirdek araçları

Bu bölüm, yalnızca işlevleriyle geçerlidir 2.x. Bağlama uzantıları işlevlerde açıkça kaydedilmesi gerekmez 1.x.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
### <a name="c-class-library-with-visual-studio-2017"></a>C# sınıf kitaplığı Visual Studio 2017 ile

İçinde **Visual Studio 2017**, Paket Yöneticisi konsolunu kullanarak paketlerini yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.ServiceBus --Version <target_version>
```

Belirtilen bağlama için kullanılacak paketi adını başvurusu makalesinde Bu bağlama için sağlanır. Bir örnek için bkz: [paketleri hizmet veri yolu bağlama başvurusu makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştir `<target_version>` paketin belirli bir sürümle örnekteki gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanına karşılık gelen ana sürüm 1.x veya 2.x bağlama için başvuru makaledeki belirtilir.

### <a name="c-class-library-with-visual-studio-code"></a>C# sınıf kitaplığı Visual Studio Code ile

İçinde **Visual Studio Code**, komut istemini kullanarak gelen paketlerini yükleyebilirsiniz [dotnet eklemek paket](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) aşağıdaki örnekte gösterildiği gibi .NET Core CLI komutu:

```terminal
dotnet add package Microsoft.Azure.WebJobs.ServiceBus --version <target_version>
```

.NET Core CLI yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Belirtilen bağlama için kullanılacak paketi adını başvurusu makalesinde Bu bağlama için sağlanır. Bir örnek için bkz: [paketleri hizmet veri yolu bağlama başvurusu makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştir `<target_version>` paketin belirli bir sürümle örnekteki gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanına karşılık gelen ana sürüm 1.x veya 2.x bağlama için başvuru makaledeki belirtilir.

## <a name="binding-direction"></a>Bağlama yönü

Tüm Tetikleyicileri ve bağlamaları sahip bir `direction` özelliğinde *function.json* dosyası:

- Tetikleyiciler için her zaman yönüdür `in`
- Giriş ve çıkış bağlamaları kullanmak `in` ve `out`
- Özel bir yön bazı bağlamaları Destek `inout`. Kullanırsanız `inout`, yalnızca **Gelişmiş Düzenleyici** kullanılabilir **tümleştir** sekmesi.

Kullandığınızda [öznitelikleri bir sınıf kitaplığı'nda](functions-dotnet-class-library.md) Tetikleyicileri ve bağlamaları yapılandırmak için yönü bir öznitelik oluşturucuda sağlanan veya parametre türünden sonuçlandı.

## <a name="using-the-function-return-value"></a>İşlev dönüş değerini kullanarak

Bir dönüş değerine sahip diller için dönüş değerini bir çıkış bağlama bağlayabilirsiniz:

* Bir C# sınıf kitaplığı'nda, yönteminin dönüş değeri çıkış bağlama özniteliğini uygulayın.
* Diğer dillerde ayarlamak `name` özelliğinde *function.json* için `$return`.

Birden çok öğe yazmanız gereken kullanırsanız bir [Toplayıcı nesnesi](functions-reference-csharp.md#writing-multiple-output-values) dönüş değeri yerine. Birden çok çıktı bağlamalar varsa, dönüş değeri yalnızca bunlardan birinin için kullanın.

Dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betik (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>C# örnek

Dönüş değerini zaman uyumsuz örneği tarafından izlenen bir çıktı bağlama için kullanan burada'nın C# kod:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>C# kod örneği

Çıktı bağlama işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Zaman uyumsuz örneği tarafından izlenen C# betik kodu, şöyledir:

```cs
public static string Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, TraceWriter log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.Info($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>F # örnek

Çıktı bağlama işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

F # kod aşağıdaki gibidir:

```fsharp
let Run(input: WorkItem, log: TraceWriter) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.Info(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>JavaScript örneği

Çıktı bağlama işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript'te, ikinci parametresinde dönüş değeri gider `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
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

## <a name="binding-expressions-and-patterns"></a>İfadeler ve desenler bağlama

Tetikleyicileri ve bağlamaları en güçlü özelliklerden biridir *ifadeleri bağlama*. İçinde *function.json* dosya ve işlev parametrelerini ve kod değerlerine çeşitli kaynaklardan çözmek ifadeleri kullanabilirsiniz.

Çoğu ifadenin süslü ayraçlar içinde kaydırma tarafından tanımlanır. Örneğin, bir sıra Tetik işlevi içinde `{queueTrigger}` sıraya ileti metnini çözümleniyor. Varsa `path` özellik bir blob için çıkış bağlama `container/{queueTrigger}` ve işlevi bir kuyruk iletisi tarafından tetiklenir `HelloWorld`, adlandırılmış bir blob `HelloWorld` oluşturulur.

Bağlama ifade türleri

* [Uygulama ayarları](#binding-expressions---app-settings)
* [Tetikleyici dosya adı](#binding-expressions---trigger-file-name)
* [Tetikleyici meta verileri](#binding-expressions---trigger-metadata)
* [JSON yükü](#binding-expressions---json-payloads)
* [Yeni bir GUID](#binding-expressions---create-guids)
* [Geçerli tarih ve saat](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Bağlama ifadeleri - uygulama ayarları

En iyi uygulama, parolaları ve bağlantı dizeleri yapılandırma dosyalarını yerine uygulama ayarları kullanılarak yönetilmelidir. Bu bu Sırları erişimi sınırlar ve dosyalarını depolamak güvenli olmasını sağlayan *function.json* ortak kaynak denetimi depoları olarak.

Uygulama ayarları da ortamına bağlı yapılandırmasını değiştirmek istediğinizde kullanışlıdır. Örneğin, bir test ortamında, farklı bir sıra veya blob depolama kapsayıcısı izlemek isteyebilir.

Uygulama ayarı bağlama ifadeleri gelen diğer bağlama ifadeleri farklı şekilde tanımlanır: süslü ayraçlar yerine yüzde işaretleri sarılır. Örneğin blob çıkış bağlama yolu ise `%Environment%/newblob.txt` ve `Environment` uygulama ayarı değer `Development`, bir blob içinde oluşturulacak `Development` kapsayıcı.

Bir işlev yerel olarak çalıştırırken, uygulama ayarı değerleri alınması *local.settings.json* dosya.

Unutmayın `connection` Tetikleyicileri ve bağlamaları özelliği özel bir durumdur ve yüzde işaretleri olmadan uygulama ayarları olarak değerleri otomatik olarak çözer. 

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

### <a name="binding-expressions---trigger-file-name"></a>Bağlama ifadeleri - tetikleyici dosya adı

`path` Bir Blob için tetikleyici diğer bağlamalarda tetikleme blob adını başvurun ve işlev kodu olanak sağlayan bir desen olabilir. Desen hangi BLOB'ları bir işlev çağrısını tetikleyebilir belirtin filtreleme ölçütlerini de içerir.

Örneğin, bağlama, aşağıdaki Blob tetikleyici içinde `path` deseni `sample-images/{filename}`, adlandırılmış bir bağlama deyimi oluşturur `filename`:

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

İfade `filename` oluşturulan blob adını belirtmek için bir çıktı bağlamasında sonra kullanılabilir:

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

İşlev kodu kullanarak erişim bu aynı değere sahip `filename` bir parametre adı olarak:

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

Sınıf kitaplıkları öznitelikleri bağlama ifadeleri ve desenler aynı kullanabilme uygular. Aşağıdaki örnekte, öznitelik Oluşturucusu aynı parametreleridir `path` önceki olarak değerleri *function.json* örnekler: 

```csharp
[FunctionName("ResizeImage")]
public static void Run(
    [BlobTrigger("sample-images/{filename}")] Stream image,
    [Blob("sample-images-sm/{filename}", FileAccess.Write)] Stream imageSmall,
    string filename,
    TraceWriter log)
{
    log.Info($"Blob trigger processing: {filename}");
    // ...
}

```

Dosya adı uzantısı gibi bölümlerinin ifadeler de oluşturabilirsiniz. İfadeler ve desenler Blob yol dizesi nasıl kullanılacağı hakkında daha fazla bilgi için bkz: [depolama blob bağlama başvurusu](functions-bindings-storage-blob.md).
 
### <a name="binding-expressions---trigger-metadata"></a>Bağlama ifadeleri - tetikleyici meta verileri

Bir tetikleyici (örneğin, bir işlev tetiklenen kuyruk iletisini içerik) tarafından sağlanan veri yükü yanı sıra birçok Tetikleyicileri ek meta veri değerleri sağlayın. Bu değerleri C# ve F # veya özelliklerinde giriş parametresi olarak kullanılabilir `context.bindings` JavaScript nesne. 

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

### <a name="binding-expressions---json-payloads"></a>Bağlama ifadeleri - JSON yükü

Bir tetikleyici yükü JSON olduğunda, aynı işlev ve işlev kodu diğer bağlamaları için yapılandırma özelliklerini başvurabilirsiniz.

Aşağıdaki örnekte gösterildiği *function.json* JSON içinde blob adını alır bir Web kancası işlevi için dosya: `{"BlobName":"HelloWorld.txt"}`. Bir Blob giriş bağlama blob okur ve HTTP bağlama döndürür HTTP yanıtı blob içeriğinin çıktı. Blob giriş bağlaması doğrudan başvurarak blob adı alır fark `BlobName` özelliği (`"path": "strings/{BlobName}"`)

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
      "path": "strings/{BlobName.FileName}.{BlobName.Extension}",
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

Bu C# ve F # içinde çalışmak, aşağıdaki örnekte olduğu gibi seri durumdan çıkarılacak alanlarını tanımlayan bir sınıf gerekir:

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

#### <a name="dot-notation"></a>Noktalı gösterim

JSON yükünüzü özelliklerin bazıları özellikleri nesneleriyle varsa, bunları noktalı gösterim kullanılarak doğrudan başvurabilirsiniz. Örneğin, JSON öğenizin şöyle varsayın:

```json
{"BlobName": {
  "FileName":"HelloWorld",
  "Extension":"txt"
  }
}
```

Doğrudan başvurabilir `FileName` olarak `BlobName.FileName`. Bu JSON biçimi ile işte `path` önceki örnekte özelliği uygulamamız gibi görünecektir:

```json
"path": "strings/{BlobName.FileName}.{BlobName.Extension}",
```

C# ' ta iki sınıf gerekir:

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

### <a name="binding-expressions---create-guids"></a>Bağlama ifadeleri - GUID'ler oluşturma

`{rand-guid}` İfade bağlama bir GUID oluşturur. Aşağıdaki blob yolu bir `function.json` dosyası gibi bir ada sahip bir blob oluşturur *50710cb5-84b9 - 4d 9 87 d 83-a03d6976a682.txt*.

```json
{
  "type": "blob",
  "name": "blobOutput",
  "direction": "out",
  "path": "my-output-container/{rand-guid}"
}
```

### <a name="binding-expressions---current-time"></a>Bağlama ifadeleri - geçerli saati

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

C# ve diğer .NET dilleri bildirim temelli bağlamaları aksine bir kesinlik temelli bağlama desen kullanabilirsiniz *function.json* ve öznitelikleri. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde kullanışlıdır. Daha fazla bilgi için bkz: [C# Geliştirici Başvurusu](functions-dotnet-class-library.md#binding-at-runtime) veya [C# betik Geliştirici Başvurusu](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>Function.JSON dosyası şeması

*Function.json* dosyası şeması şu adreste [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Bağlama hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

İşlevleri tarafından desteklenen çeşitli hizmetler için tüm ilgili hata konulara bağlantılar için bkz: [hata kodları bağlama](functions-bindings-error-pages.md#binding-error-codes) bölümünü [Azure işlevleri hata işleme](functions-bindings-error-pages.md) genel bakış konusunun.  

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
