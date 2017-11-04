---
title: "İşlevler için Azure Cosmos DB bağlamaları | Microsoft Docs"
description: "Azure Cosmos DB Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: christopheranderson
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.assetid: 3d8497f0-21f3-437d-ba24-5ece8c90ac85
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/19/2017
ms.author: glenga
ms.openlocfilehash: d05c0342e771e229a7175570ad227c4359980990
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-cosmos-db-bindings-for-functions"></a>İşlevler için Azure Cosmos DB bağlamaları
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Bu makalede nasıl yapılandırılacağı ve kod Azure Cosmos DB bağlamaları Azure işlevlerinde açıklanmaktadır. Tetiklemek, giriş ve Azure Cosmos DB bağlantılarında çıkış işlevleri destekler.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Sunucusuz Azure Cosmos DB ile bilgisayar ile ilgili daha fazla bilgi için bkz: [Azure Cosmos DB: sunucusuz veritabanı Azure işlevleri kullanarak bilgi işlem](..\cosmos-db\serverless-computing-database.md).

<a id="trigger"></a>
<a id="cosmosdbtrigger"></a>

## <a name="azure-cosmos-db-trigger"></a>Azure Cosmos DB tetikleyici

Azure Cosmos DB tetikleyicisi kullanan [Azure Cosmos DB Değiştir Akış](../cosmos-db/change-feed.md) bölümler değişikliklerini dinlemek için. Tetikleyici depolamak için kullanır ikinci bir koleksiyon gerektirir _kiraları_ bölümler üzerinden.

İzlenmekte olan koleksiyonu ve kiraları içeren koleksiyonu çalışmak tetikleyici için kullanılabilir olmalıdır.

Azure Cosmos DB tetikleyici aşağıdaki özellikleri destekler:

|Özellik  |Açıklama  |
|---------|---------|
|**türü** | ayarlanmalıdır `cosmosDBTrigger`. |
|**adı** | Belge değişikliklerle listesini temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**yönü** | ayarlanmalıdır `in`. Bu parametre, Azure portalında tetikleyici oluşturduğunuzda otomatik olarak ayarlanır. |
|**connectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabınıza bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**databaseName** | İzlenmekte olan derlemesiyle Azure Cosmos DB veritabanının adı. |
|**collectionName** | İzlenmekte olan koleksiyon adı. |
| **leaseConnectionStringSetting** | (İsteğe bağlı) Kira koleksiyonunu tutan hizmeti ile bağlantı dizesi içeren bir uygulama ayarı adı. Ayarlandığında değil, `connectionStringSetting` değeri kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. |
| **leaseDatabaseName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyonunu tutan veritabanının adı. Değil olarak ayarlandığında, değeri `databaseName` ayarı kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. |
| **leaseCollectionName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyon adı. Değil olarak ayarlandığında, değerin `leases` kullanılır. |
| **createLeaseCollectionIfNotExists** | (İsteğe bağlı) Ayarlandığında `true`, önceden var olmayan kiraları koleksiyonu otomatik olarak oluşturulur. Varsayılan değer `false`. |
| **leaseCollectionThroughput** | (İsteğe bağlı) İstek kiraları koleksiyonu oluşturulduğunda atamak için birimi miktarını tanımlar. Bu ayarı yalnızca kullanılan olduğunda, `createLeaseCollectionIfNotExists` ayarlanır `true`. Bu parametre, bağlama oluşturulduğunda portal kullanılarak otomatik olarak ayarlanır.

>[!NOTE] 
>Kira koleksiyonuna bağlanmak için kullanılan bağlantı dizesi yazma izinlerine sahip olmalıdır.

Bu özellikler Azure portalında veya düzenleyerek işlevi için Tümleştirme sekmesindeki ayarlanabilir `function.json` proje dosyası.

## <a name="using-an-azure-cosmos-db-trigger"></a>Bir Azure Cosmos DB tetikleyicisi kullanma

Bu bölümde Azure Cosmos DB Tetikleyici kullanma örnekleri içerir. Örneklerde, aşağıdakine benzer bir tetikleyici meta veri varsayılmaktadır:

```json
{
  "type": "cosmosDBTrigger",
  "name": "documents",
  "direction": "in",
  "leaseCollectionName": "leases",
  "connectionStringSetting": "<connection-app-setting>",
  "databaseName": "Tasks",
  "collectionName": "Items",
  "createLeaseCollectionIfNotExists": true
}
```
 
Bir Azure Cosmos DB tetikleyici Portalı'nda bir işlev uygulaması oluşturmak nasıl bir örnek için bkz: [Azure Cosmos DB tarafından tetiklenen bir işlev oluşturun](functions-create-cosmos-db-triggered-function.md). 

### <a name="trigger-sample-in-c"></a>Tetikleyici örnek C# #
```cs 
    #r "Microsoft.Azure.Documents.Client"
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    using System;
    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Verbose("Documents modified " + documents.Count);
        log.Verbose("First document Id " + documents[0].Id);
    }
```


### <a name="trigger-sample-in-javascript"></a>JavaScript tetikleyici örnek
```javascript
    module.exports = function (context, documents) {
        context.log('First document Id modified : ', documents[0].id);

        context.done();
    }
```

<a id="docdbinput"></a>

## <a name="documentdb-api-input-binding"></a>DocumentDB API giriş bağlama
DocumentDB API giriş bağlaması Azure Cosmos DB belge alır ve işlev adlandırılmış giriş parametresi olarak geçirir. Kimliği belirlenebilir belge işlevi çağırır Tetikle temel. 

DocumentDB API giriş bağlaması aşağıdaki özelliklere sahip *function.json*:

|Özellik  |Açıklama  |
|---------|---------|
|**adı**     | İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**türü**     | ayarlanmalıdır `documentdb`.        |
|**databaseName** | Belge içeren veritabanı.        |
|**collectionName**  | Belge içeren koleksiyonu adı. |
|**Kimliği**     | Alınacak belge kimliği. Bu özellik bağlamaları parametreleri destekler. Daha fazla bilgi için bkz: [bir bağlama ifadesinde özel giriş özellikleri bağlamak](functions-triggers-bindings.md#bind-to-custom-input-properties-in-a-binding-expression). |
|**sqlQuery**     | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Çalışma zamanı bağlamaları gibi örnekte sorgu destekler: `SELECT * FROM c where c.departmentId = {departmentId}`.        |
|**bağlantı**     |Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |
|**yönü**     | ayarlanmalıdır `in`.         |

Her ikisi de ayarlanamıyor **kimliği** ve **sqlQuery** özellikleri. Hiçbiri ayarlanmışsa, tüm koleksiyon alınır.

## <a name="using-a-documentdb-api-input-binding"></a>Bir DocumentDB API giriş bağlama işlemini kullanma

* İşlev başarıyla çıktığında, C# ve F # işlevleri adlandırılmış giriş parametreleri aracılığıyla giriş belgeye yapılan değişiklikler otomatik olarak kalıcıdır. 
* JavaScript işlevleri güncelleştirmeleri otomatik olarak işlevi çıkış duruma getirilmez. Bunun yerine, kullanın `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` güncelleştirme yapmak için. Bkz: [JavaScript örnek](#injavascript).

<a name="inputsample"></a>

## <a name="input-sample-for-single-document"></a>Tek belge için giriş örneği
Aşağıdaki olduğunu varsayalım DocumentDB API bağlamasında giriş `bindings` function.json dizisi:

```json
{
  "name": "inputDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "id" : "{queueTrigger}",
  "connection": "MyAccount_COSMOSDB",     
  "direction": "in"
}
```

Belgenin metin değeri güncelleştirmek için bu giriş bağlama kullanır dile özgü örneğe bakın.

* [C#](#incsharp)
* [F#](#infsharp)
* [JavaScript](#injavascript)

<a name="incsharp"></a>
### <a name="input-sample-in-c"></a>C# giriş örneği #

```cs
// Change input document contents using DocumentDB API input binding 
public static void Run(string myQueueItem, dynamic inputDocument)
{   
  inputDocument.text = "This has changed.";
}
```
<a name="infsharp"></a>

### <a name="input-sample-in-f"></a>F # giriş örneği #

```fsharp
(* Change input document contents using DocumentDB API input binding *)
open FSharp.Interop.Dynamic
let Run(myQueueItem: string, inputDocument: obj) =
  inputDocument?text <- "This has changed."
```

Bu örnek gerektiren bir `project.json` belirten dosyası `FSharp.Interop.Dynamic` ve `Dynamitey` NuGet bağımlılıklar:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Eklemek için bir `project.json` dosya için bkz: [F # paket Yönetimi](functions-reference-fsharp.md#package).

<a name="injavascript"></a>

### <a name="input-sample-in-javascript"></a>JavaScript giriş örneği

```javascript
// Change input document contents using DocumentDB API input binding, using context.bindings.inputDocumentOut
module.exports = function (context) {   
  context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
  context.bindings.inputDocumentOut.text = "This was updated!";
  context.done();
};
```

## <a name="input-sample-with-multiple-documents"></a>Giriş örneği ile birden çok belge

Bir SQL sorgusu tarafından belirtilen birden çok belge almak sorgu parametrelerini özelleştirmek için bir sıra tetikleyici kullanarak istediğiniz varsayalım. 

Bu örnekte, bir parametre sırası tetikleyici sağlar `departmentId`. Bir kuyruk iletisi, `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür. Aşağıda, kullanmak *function.json*:

```
{
    "name": "documents",
    "type": "documentdb",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connection": "CosmosDBConnection"
}
```

### <a name="input-sample-with-multiple-documents-in-c"></a>Giriş örneği birden çok belge C# ile

```csharp
public static void Run(QueuePayload myQueueItem, IEnumerable<dynamic> documents)
{   
    foreach (var doc in documents)
    {
        // operate on each document
    }    
}

public class QueuePayload
{
    public string departmentId { get; set; }
}
```

### <a name="input-sample-with-multiple-documents-in-javascript"></a>JavaScript içindeki birden çok belge giriş örnekle

```javascript
module.exports = function (context, input) {    
    var documents = context.bindings.documents;
    for (var i = 0; i < documents.length; i++) {
        var document = documents[i];
        // operate on each document
    }       
    context.done();
};
```

## <a id="docdboutput"></a>DocumentDB API bağlama çıktı
Sağlar bağlama DocumentDB API çıktı yeni bir belge bir Azure Cosmos DB veritabanına yazar. Aşağıdaki özelliklere sahip *function.json*:

|Özellik  |Açıklama  |
|---------|---------|
|**adı**     | İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**türü**     | ayarlanmalıdır `documentdb`.        |
|**databaseName** | Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**collectionName**  | Belgenin oluşturulduğu koleksiyon adı. |
|**createIfNotExists**     | Yoksa koleksiyon oluşturulduğunda olup olmadığını gösteren bir Boole değeri. Varsayılan değer *false*. Yeni koleksiyon etkileri maliyet ayrılmış işleme ile oluşturulan olmasıdır. Daha fazla ayrıntı için lütfen ziyaret [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**bağlantı**     |Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |
|**yönü**     | ayarlanmalıdır `out`.         |

## <a name="using-a-documentdb-api-output-binding"></a>Bir DocumentDB API kullanarak çıktıyı bağlama
Bu bölümde işlevi kodunuzda bağlama, DocumentDB API çıkış kullanmayı gösterir.

Çıktı parametresi, işlevinde yazdığınızda, varsayılan olarak, bir belge veritabanınızda oluşturulur. Bu belge otomatik olarak oluşturulan GUID belge kimliği olarak sahiptir. Çıktı belgenin belge kimliği belirterek belirtebilirsiniz `id` özelliği JSON nesnesinde çıktı parametresi geçirildi. 

>[!Note]  
>Var olan bir belgeyi Kimliğini belirttiğinizde, yeni çıktı belgenin üzerine. 

Birden çok belge çıktısını almak için de bağlayabilirsiniz `ICollector<T>` veya `IAsyncCollector<T>` burada `T` desteklenen türlerden biri.

<a name="outputsample"></a>

## <a name="documentdb-api-output-binding-sample"></a>DocumentDB API çıkış bağlama örneği
Aşağıdaki olduğunu varsayalım DocumentDB API bağlamasında çıktı `bindings` function.json dizisi:

```json
{
  "name": "employeeDocument",
  "type": "documentDB",
  "databaseName": "MyDatabase",
  "collectionName": "MyCollection",
  "createIfNotExists": true,
  "connection": "MyAccount_COSMOSDB",     
  "direction": "out"
}
```

Ve şu biçimde JSON alan sıra için bir sıra giriş bağlama sahip:

```json
{
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Ve her kayıt için şu biçimde Azure Cosmos DB belgeleri oluşturmak isterseniz:

```json
{
  "id": "John Henry-123456",
  "name": "John Henry",
  "employeeId": "123456",
  "address": "A town nearby"
}
```

Belgeler, veritabanına eklemek için bu çıktı bağlama kullanan dile özgü örneğine bakın.

* [C#](#outcsharp)
* [F#](#outfsharp)
* [JavaScript](#outjavascript)

<a name="outcsharp"></a>

### <a name="output-sample-in-c"></a>C# çıktı örneği #

```cs
#r "Newtonsoft.Json"

using System;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

public static void Run(string myQueueItem, out object employeeDocument, TraceWriter log)
{
  log.Info($"C# Queue trigger function processed: {myQueueItem}");

  dynamic employee = JObject.Parse(myQueueItem);

  employeeDocument = new {
    id = employee.name + "-" + employee.employeeId,
    name = employee.name,
    employeeId = employee.employeeId,
    address = employee.address
  };
}
```

<a name="outfsharp"></a>

### <a name="output-sample-in-f"></a>F # çıktı örneği #

```fsharp
open FSharp.Interop.Dynamic
open Newtonsoft.Json

type Employee = {
  id: string
  name: string
  employeeId: string
  address: string
}

let Run(myQueueItem: string, employeeDocument: byref<obj>, log: TraceWriter) =
  log.Info(sprintf "F# Queue trigger function processed: %s" myQueueItem)
  let employee = JObject.Parse(myQueueItem)
  employeeDocument <-
    { id = sprintf "%s-%s" employee?name employee?employeeId
      name = employee?name
      employeeId = employee?employeeId
      address = employee?address }
```

Bu örnek gerektiren bir `project.json` belirten dosyası `FSharp.Interop.Dynamic` ve `Dynamitey` NuGet bağımlılıklar:

```json
{
  "frameworks": {
    "net46": {
      "dependencies": {
        "Dynamitey": "1.0.2",
        "FSharp.Interop.Dynamic": "3.0.0"
      }
    }
  }
}
```

Eklemek için bir `project.json` dosya için bkz: [F # paket Yönetimi](functions-reference-fsharp.md#package).

<a name="outjavascript"></a>

### <a name="output-sample-in-javascript"></a>JavaScript çıktı örneği

```javascript
module.exports = function (context) {

  context.bindings.employeeDocument = JSON.stringify({ 
    id: context.bindings.myQueueItem.name + "-" + context.bindings.myQueueItem.employeeId,
    name: context.bindings.myQueueItem.name,
    employeeId: context.bindings.myQueueItem.employeeId,
    address: context.bindings.myQueueItem.address
  });

  context.done();
};
```
