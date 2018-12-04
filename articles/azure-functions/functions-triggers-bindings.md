---
title: Tetikleyiciler ve bağlamalar Azure işlevleri'nde
description: Tetikleyicileri ve bağlamaları, Azure işlevleri'nde, kod yürütme çevrimiçi etkinliklerimizden ve bulut tabanlı hizmetlere bağlamak için kullanmayı öğrenin.
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: azure işlevleri, işlevler, olay işleme, web kancaları, dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.devlang: multiple
ms.topic: reference
ms.date: 09/24/2018
ms.author: cshoe
ms.openlocfilehash: b071bfe83ba9ef653db2d6d1debad4e3dfa02580
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52845935"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure işlevleri Tetikleyicileri ve bağlamaları kavramları

Bu makalede, Tetikleyicileri ve bağlamaları Azure işlevleri'nde kavramsal bir genel bakıştır. Tüm bağlamaları ve tüm desteklenen diller için ortak olan özellikler aşağıda açıklanmıştır.

## <a name="overview"></a>Genel Bakış

A *tetikleyici* nasıl bir işlev çağrılır tanımlar. Bir işlev tam olarak bir tetikleyici olmalıdır. Tetikleyiciler genellikle işlevi tetikleyen yük olan ilgili verilere sahiptir.

Giriş ve çıkış *bağlamaları* kodunuz içinde verilere bağlanmak için bildirim temelli bir yöntemini sağlar. Bağlamaları isteğe bağlıdır ve bir işlev sahip birden fazla giriş ve çıkış bağlamaları. 

Tetikleyicileri ve bağlamaları, runbook'a kod ile çalışıyorsanız Hizmetleri ayrıntılarını önlemenize olanak tanır. İşlevinizi verileri (örneğin, bir kuyruk iletisinin içeriği) işlevi parametreleri alır. Verileri (örneğin, bir kuyruk iletisi oluşturmak için), işlev dönüş değeri kullanarak gönderin. C# ve C# betiği, veri göndermek için alternatif yöntemler, `out` parametreleri ve [toplayıcısı nesneleri](functions-reference-csharp.md#writing-multiple-output-values).

Azure portalını kullanarak işlevleri geliştirirken, Tetikleyicileri ve bağlamaları yapılandırılan bir *function.json* dosya. Portal, bu yapılandırma için bir kullanıcı Arabirimi sağlar ancak doğrudan değiştirerek dosyayı düzenleyebilirsiniz **Gelişmiş Düzenleyici**.

Bir sınıf kitaplığı oluşturmak için Visual Studio kullanarak işlevleri geliştirirken, Tetikleyicileri ve bağlamaları yöntemleri ve öznitelikleri parametrelerle tasarlayarak yapılandırın.

## <a name="example-trigger-and-binding"></a>Örnek tetikleyici ve bağlama

Azure kuyruk depolama alanında yeni bir iletinin göründüğü her durumda, Azure tablo depolama alanına yeni bir satır yazmak istediğiniz varsayalım. Bu senaryo, bir Azure kuyruğundaki kullanarak uygulanabilir depolama tetikleyicisi ve Azure tablo depolama çıkış bağlaması. 

İşte bir *function.json* bu senaryo için dosya. 

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

İçindeki ilk öğeyi `bindings` kuyruk depolama tetikleyicisi dizisidir. `type` Ve `direction` tetikleyici özelliklerini tanımlayın. `name` Özelliği kuyruk iletisi içeriğini alan işlev parametresi tanımlar. İzlemek için kuyruk adı yer `queueName`, ve uygulama ayarının tarafından tanımlanan bağlantı dizesidir `connection`.

İkinci öğe `bindings` dizidir Azure tablo depolama çıktı bağlaması. `type` Ve `direction` bağlama özelliklerini tanımlayın. `name` Dönüş değeri işlevini kullanarak bu durumda, özellik belirtir nasıl yeni bir tablo satırının işlevi sağlar. Tablo adı kullanılıyor `tableName`, ve uygulama ayarının tarafından tanımlanan bağlantı dizesidir `connection`.

İçeriğini görüntüleyip *function.json* Azure portalında **Gelişmiş Düzenleyici** seçeneğini **tümleştir** işlevinizin sekmesi.

> [!NOTE]
> Değerini `connection` değil bağlantı dizesinin kendisinin bağlantı dizesi içeren bir uygulama ayarı adı. Bağlamaları kullanın bağlantı dizeleri en iyi zorlamak için uygulama ayarlarında depolanan uygulama *function.json* hizmet gizli dizileri içermiyor.

Aşağıda, bu tetikleyici ve bağlama ile çalışan bir C# kodu verilmiştir. Kuyruk iletisi içeriği sağlayan parametrenin adı olduğuna dikkat edin `order`; çünkü bu adı gereklidir `name` özellik değeri *function.json* olduğu `order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
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

Bir JavaScript işlevi ile aynı function.json dosya kullanılabilir:

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

Bir sınıf kitaplığı, aynı tetikleyici ve bağlama bilgileri &mdash; kuyruk ve tablo adları, depolama hesapları, işlev giriş ve çıkış parametrelerini &mdash; öznitelikleri function.json dosyası tarafından sağlanır. Bir örneği aşağıda verilmiştir:

```csharp
 public static class QueueTriggerTableOutput
 {
     [FunctionName("QueueTriggerTableOutput")]
     [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
     public static Person Run(
         [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order, 
         ILogger log)
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

Bağlamaları önizlemededir veya üretim kullanımı için onaylanmış olan hakkında bilgi için bkz: [desteklenen diller](supported-languages.md).

## <a name="register-binding-extensions"></a>Bağlama uzantıları kaydetme

Bazı geliştirme ortamlarında, açıkça zorunda *kaydetme* kullanmak istediğiniz bir bağlama. Bağlama uzantıları NuGet paketlerinde sağlanır ve bir uzantıyı kaydetmek için bir paket yükleyin. Aşağıdaki tabloda, ne zaman ve nasıl bağlama uzantıları kaydetme gösterir.

|Geliştirme ortamı |Kayıt<br/> işlevlerde 1.x  |Kayıt<br/> işlevlerde 2.x  |
|---------|---------|---------|
|Azure portal|Automatic|[Otomatik İstemi ile](#azure-portal-development)|
|Azure işlevleri çekirdek Araçları'nı kullanarak yerel|Automatic|[Çekirdek araçları CLI komutlarını kullanın](#local-development-azure-functions-core-tools)|
|Visual Studio 2017'yi kullanarak C# sınıf kitaplığı|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2017)|[NuGet araçları kullanın](#c-class-library-with-visual-studio-2017)|
|Visual Studio Code kullanarak C# sınıf kitaplığı|Yok|[.NET Core CLI kullanma](#c-class-library-with-visual-studio-code)|

Açık kaydı otomatik olarak tüm sürümleri ve ortamları kayıtlı olduğundan gerektirmeyen özel durumlar aşağıdaki bağlama türleridir: HTTP ve Zamanlayıcı.

### <a name="azure-portal-development"></a>Azure portal geliştirme

Bu bölüm yalnızca işlevlere uygulanır 2.x. Bağlama uzantıları işlevleri açıkça kaydedilmesi gerekmez 1.x.

Bir işlev oluşturma veya bağlama eklemek, tetikleyicisi veya bağlaması uzantısı kaydı gerektirdiğinde, sizden istenir. Tıklayarak istemine yanıt **yükleme** uzantısını kaydetmek için. Yükleme, bir tüketim planında 10 dakikaya kadar sürebilir.

Yalnızca bir kez bir verilen işlev uygulaması için her bir uzantı yüklemeniz gerekir. Portalda veya güncelleştirmek için kullanılabilir olmayan desteklenen bağlamaları için yüklenmiş bir uzantı ayrıca [el ile yüklemek veya Azure Portalı'ndan uzantılarını bağlama işlevleri güncelleştirme](install-update-binding-extensions-manual.md).  

### <a name="local-development-azure-functions-core-tools"></a>Yerel geliştirme Azure işlevleri çekirdek araçları

Bu bölüm yalnızca işlevlere uygulanır 2.x. Bağlama uzantıları işlevleri açıkça kaydedilmesi gerekmez 1.x.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

<a name="local-csharp"></a>
### <a name="c-class-library-with-visual-studio-2017"></a>C# sınıf kitaplığı ile Visual Studio 2017

İçinde **Visual Studio 2017**, Paket Yöneticisi Konsolu'nu kullanarak paketleri yükleyebilirsiniz [Install-Package](https://docs.microsoft.com/nuget/tools/ps-ref-install-package) aşağıdaki örnekte gösterildiği gibi komut:

```powershell
Install-Package Microsoft.Azure.WebJobs.Extensions.ServiceBus -Version <target_version>
```

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<target_version>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

### <a name="c-class-library-with-visual-studio-code"></a>C# sınıf kitaplığı Visual Studio Code ile

İçinde **Visual Studio Code**, komut istemi kullanarak paketleri yükleyebilirsiniz [dotnet paketini ekleyin](https://docs.microsoft.com/dotnet/core/tools/dotnet-add-package) aşağıdaki örnekte gösterildiği gibi .NET Core CLI, komut:

```terminal
dotnet add package Microsoft.Azure.WebJobs.Extensions.ServiceBus --version <target_version>
```

.NET Core CLI, yalnızca Azure işlevleri 2.x geliştirme için kullanılabilir.

Kullanmak için belirli bir bağlama için paket adını bu bağlama için başvuru makalesinde verilmektedir. Bir örnek için bkz. [paketler Service Bus bağlama başvuru makalesinde bölümüne](functions-bindings-service-bus.md#packages---functions-1x).

Değiştirin `<target_version>` örnekte belirli bir paket sürümü ile gibi `3.0.0-beta5`. Geçerli sürümler tek tek Paket sayfalarında listelenen [NuGet.org](https://nuget.org). İşlevler çalışma zamanı için karşılık gelen ana sürüm bağlama için başvuru makalesinde 1.x veya 2.x belirtilir.

## <a name="binding-direction"></a>Bağlama yönü

Tüm tetikleyiciler ve bağlamalar bir `direction` özelliğinde *function.json* dosyası:

- Tetikleyiciler için her zaman yönü olur `in`
- Giriş ve çıkış bağlamaları kullanın `in` ve `out`
- Bazı bağlamalar destekleyen özel bir yön `inout`. Kullanırsanız `inout`, yalnızca **Gelişmiş Düzenleyici** kullanılabilir **tümleştir** sekmesi.

Kullanırken [öznitelikleri bir sınıf kitaplığı'nda](functions-dotnet-class-library.md) Tetikleyicileri ve bağlamaları yapılandırmak için yön bir öznitelik oluşturucuda sağlanan veya parametre türünden çıkarsanan.

## <a name="using-the-function-return-value"></a>İşlev dönüş değeri kullanma

Dönüş değerine sahip dillerde dönüş değerini bir çıkış bağlaması bağlayabilirsiniz:

* Bir C# sınıf kitaplığında, yöntemin dönüş değerini çıkış bağlama özniteliğini uygulayın.
* Diğer dillerde ayarlamak `name` özelliğinde *function.json* için `$return`.

Dönüş değeri, birden çok çıkış bağlamaları varsa, bunlardan yalnızca biri için kullanın.

C# ve C# betiği için bir çıkış bağlaması veri göndermek için alternatif yöntemler, `out` parametreleri ve [toplayıcısı nesneleri](functions-reference-csharp.md#writing-multiple-output-values).

Dönüş değeri kullanımını gösteren dile özgü örneğe bakın:

* [C#](#c-example)
* [C# betiği (.csx)](#c-script-example)
* [F#](#f-example)
* [JavaScript](#javascript-example)
* [Python](#python-example)

### <a name="c-example"></a>C# örneği

Dönüş değeri için zaman uyumsuz örneği tarafından izlenen bir çıkış bağlaması kullanan aşağıda verilmiştir; C# kodu:

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static string Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
[FunctionName("QueueTrigger")]
[return: Blob("output-container/{id}")]
public static Task<string> Run([QueueTrigger("inputqueue")]WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="c-script-example"></a>C# betiği örneği

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

Zaman uyumsuz örneği tarafından izlenen C# betik kodu, şu şekildedir:

```cs
public static string Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return json;
}
```

```cs
public static Task<string> Run(WorkItem input, ILogger log)
{
    string json = string.Format("{{ \"id\": \"{0}\" }}", input.Id);
    log.LogInformation($"C# script processed queue message. Item={json}");
    return Task.FromResult(json);
}
```

### <a name="f-example"></a>F#Örnek

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

İşte F# kod:

```fsharp
let Run(input: WorkItem, log: ILogger) =
    let json = String.Format("{{ \"id\": \"{0}\" }}", input.Id)   
    log.LogInformation(sprintf "F# script processed queue message '%s'" json)
    json
```

### <a name="javascript-example"></a>JavaScript örneği

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```

JavaScript'te, ikinci parametresinin, dönüş değeri gider `context.done`:

```javascript
module.exports = function (context, input) {
    var json = JSON.stringify(input);
    context.log('Node.js script processed queue message', json);
    context.done(null, json);
}
```

### <a name="python-example"></a>Python örnek

Çıkış bağlaması işte *function.json* dosyası:

```json
{
    "name": "$return",
    "type": "blob",
    "direction": "out",
    "path": "output-container/{id}"
}
```
Python kod aşağıdaki gibidir:

```python
def main(input: azure.functions.InputStream) -> str:
    return json.dumps({
        'name': input.name,
        'length': input.length,
        'content': input.read().decode('utf-8')
    })
```

## <a name="binding-datatype-property"></a>Bağlamanın dataType özelliği

. NET'te, parametre türü, giriş verileri için veri türünü tanımlamak için kullanın. Örneğin, kullanın `string` ikili ve için bir POCO nesnenin serisini kaldırmak için özel bir tür olarak okunacak bayt dizisi olarak bir kuyruk tetikleyicisi metnini bağlanacak.

JavaScript gibi dinamik olarak yazılan diller için kullanmak `dataType` özelliğinde *function.json* dosya. Örneğin, ikili biçimde bir HTTP isteği içeriğini okumak için ayarlanmış `dataType` için `binary`:

```json
{
    "type": "httpTrigger",
    "name": "req",
    "direction": "in",
    "dataType": "binary"
}
```

Diğer seçenekler için `dataType` olan `stream` ve `string`.

## <a name="binding-expressions-and-patterns"></a>Bağlama ifadeleri ve desenleri

Tetikleyiciler ve bağlamalar en güçlü özelliklerinden biri *ifadeleri bağlama*. İçinde *function.json* dosya ve işlev parametrelerini ve kod, çeşitli kaynaklardan gelen değerlerine çözmek ifadeleri kullanabilirsiniz.

Çoğu ifadenin ayraç içindeki sarmalama tarafından tanımlanır. Örneğin, bir kuyruğu tetikleme işlevi içinde `{queueTrigger}` kuyruğa ileti metni giderir. Varsa `path` özellik bir blob için çıktı bağlaması `container/{queueTrigger}` ve bir kuyruk iletisi ile tetiklenen işlev `HelloWorld`, adlı bir blob `HelloWorld` oluşturulur.

Bağlama ifade türleri

* [Uygulama ayarları](#binding-expressions---app-settings)
* [Tetikleyici dosya adı](#binding-expressions---trigger-file-name)
* [Tetikleyici meta verileri](#binding-expressions---trigger-metadata)
* [JSON yükü](#binding-expressions---json-payloads)
* [Yeni GUID](#binding-expressions---create-guids)
* [Geçerli tarih ve saat](#binding-expressions---current-time)

### <a name="binding-expressions---app-settings"></a>Bağlama ifadeleri - uygulama ayarları

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

### <a name="binding-expressions---trigger-file-name"></a>Bağlama ifadeleri - tetikleyici dosya adı

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
 
### <a name="binding-expressions---trigger-metadata"></a>Bağlama ifadeleri - tetikleyici meta verileri

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

### <a name="binding-expressions---json-payloads"></a>Bağlama ifadeleri - JSON yükü

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

#### <a name="dot-notation"></a>Nokta gösterimi

JSON yükünüzü özelliklerinde bazı özelliklere sahip nesneler varsa, bu nokta gösterimi kullanılarak doğrudan başvurabilirsiniz. Örneğin, JSON şöyle düşünün:

```json
{"BlobName": {
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

### <a name="binding-expressions---create-guids"></a>Bağlama ifadeleri - GUID oluştur

`{rand-guid}` Bağlama ifadesi bir GUID oluşturur. Aşağıdaki blob yolu bir `function.json` dosyası gibi bir ada sahip bir blob oluşturur *50710cb5 84b9 - 4d 9 87 d 83-a03d6976a682.txt*.

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

C# ve diğer .NET dilleri, bildirim temelli bağlamaları aksine bir kesinlik temelli bir bağlama deseni kullanabileceğiniz *function.json* ve öznitelikleri. Kesinlik temelli bağlama bağlama parametreleri tasarım yerine çalışma zamanında hesaplanması gerektiğinde yararlıdır. Daha fazla bilgi için bkz. [C# Geliştirici Başvurusu](functions-dotnet-class-library.md#binding-at-runtime) veya [C# betiği Geliştirici Başvurusu](functions-reference-csharp.md#binding-at-runtime).

## <a name="functionjson-file-schema"></a>Function.JSON dosya şeması

*Function.json* dosya şemasının kullanılabilir [ http://json.schemastore.org/function ](http://json.schemastore.org/function).

## <a name="handling-binding-errors"></a>Bağlama hataları işleme

[!INCLUDE [bindings errors intro](../../includes/functions-bindings-errors-intro.md)]

İşlevleri tarafından desteklenen çeşitli hizmetler için tüm ilgili hata konulara bağlantılar için bkz: [hata kodları bağlama](functions-bindings-error-pages.md#binding-error-codes) bölümünü [Azure işlevleri hata işleme](functions-bindings-error-pages.md) genel bakış konusu.  

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
