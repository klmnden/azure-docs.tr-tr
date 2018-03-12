---
title: "İşlevler için Azure Cosmos DB bağlamaları"
description: "Azure Cosmos DB Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
keywords: "Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi"
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: glenga
ms.openlocfilehash: 5d90b2cd977522eab267c8c86a35e47bc61248a8
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2018
---
# <a name="azure-cosmos-db-bindings-for-azure-functions"></a>Azure işlevleri için Azure Cosmos DB bağlamaları

Bu makale ile nasıl çalışılacağını açıklar [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) Azure işlevlerinde bağlar. Tetiklemek, giriş ve Azure Cosmos DB bağlantılarında çıktı Azure işlevleri destekler.

> [!NOTE]
> Bu bağlama DocumentDB ilk olarak adlandırılıyordu. İşlevler sürümünde 1.x, yalnızca tetikleyici edildi Cosmos DB; yeniden adlandırıldı giriş bağlaması, çıktı bağlama ve NuGet paketi DocumentDB adını korur. İçinde [işlevleri sürüm 2.x](functions-versions.md), bağlamaları ve paketi yeniden adlandırılmış Cosmos DB de olmuştur. Bu makalede 1.x adlarını kullanır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="trigger"></a>Tetikleyici

Azure Cosmos DB tetikleyicisi kullanan [Azure Cosmos DB Değiştir Akış](../cosmos-db/change-feed.md) bölümler değişikliklerini dinlemek için. Değişiklik akış ekler ve güncelleştirmeler, değil silme yayımlar. 

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# script (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir belirli veritabanı ve koleksiyonu tetikler.

```cs
    using System.Collections.Generic;
    using Microsoft.Azure.Documents;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;

    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
            log.Info("Documents modified " + documents.Count);
            log.Info("First document Id " + documents[0].Id);
    }
```

### <a name="trigger---c-script-example"></a>Tetikleyici - C# kod örneği

Aşağıdaki örnek, bağlama Cosmos DB tetikleyici gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. Cosmos DB kayıtları değiştirildiğinde işlevi günlük iletisi yazar.

Veri bağlama işte *function.json* dosyası:

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

C# betik kod aşağıdaki gibidir:
 
```cs 
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
      log.Verbose("Documents modified " + documents.Count);
      log.Verbose("First document Id " + documents[0].Id);
    }
```

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama Cosmos DB tetikleyici gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. Cosmos DB kayıtları değiştirildiğinde işlevi günlük iletisi yazar.

Veri bağlama işte *function.json* dosyası:

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

JavaScript kod aşağıdaki gibidir:

```javascript
    module.exports = function (context, documents) {
      context.log('First document Id modified : ', documents[0].id);

      context.done();
    }
```

## <a name="trigger---attributes"></a>Tetikleyici - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/Trigger/CosmosDBTriggerAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Özniteliğin Oluşturucusu koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında daha fazla bilgi için bkz: [tetikleyici - yapılandırma](#trigger---configuration). Burada bir `CosmosDBTrigger` yöntemi imza özniteliği örnekte:

```csharp
    [FunctionName("DocumentUpdates")]
    public static void Run(
        [CosmosDBTrigger("database", "collection", ConnectionStringSetting = "myCosmosDB")]
    IReadOnlyList<Document> documents,
        TraceWriter log)
    {
        ...
    }
```

Tam bir örnek için bkz: [tetikleyici - C# örnek](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `CosmosDBTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü** || ayarlanmalıdır `cosmosDBTrigger`. |
|**Yönü** || ayarlanmalıdır `in`. Bu parametre, Azure portalında tetikleyici oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** || Belge değişikliklerle listesini temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**connectionStringSetting**|**ConnectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabınıza bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**databaseName**|**DatabaseName**  | İzlenmekte olan derlemesiyle Azure Cosmos DB veritabanının adı. |
|**collectionName** |**collectionName** | İzlenmekte olan koleksiyon adı. |
|**leaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (İsteğe bağlı) Kira koleksiyonunu tutan hizmeti ile bağlantı dizesi içeren bir uygulama ayarı adı. Ayarlandığında değil, `connectionStringSetting` değeri kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. Bağlantı dizesi kiraları koleksiyon için yazma izinlerine sahip olmalıdır.|
|**leaseDatabaseName** |**LeaseDatabaseName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyonunu tutan veritabanının adı. Değil olarak ayarlandığında, değeri `databaseName` ayarı kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. |
|**leaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyon adı. Değil olarak ayarlandığında, değerin `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (İsteğe bağlı) Ayarlandığında `true`, önceden var olmayan kiraları koleksiyonu otomatik olarak oluşturulur. Varsayılan değer `false`. |
|**leasesCollectionThroughput**| **LeasesCollectionThroughput**| (İsteğe bağlı) İstek kiraları koleksiyonu oluşturulduğunda atamak için birimi miktarını tanımlar. Bu ayarı yalnızca kullanılan olduğunda, `createLeaseCollectionIfNotExists` ayarlanır `true`. Bu parametre, bağlama oluşturulduğunda portal kullanılarak otomatik olarak ayarlanır.
| |**LeaseOptions** | Bir örnekte özelliklerini ayarlayarak kira seçeneklerini yapılandırmak [ChangeFeedHostOptions](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.changefeedprocessor.changefeedhostoptions) sınıfı.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Tetikleyici depolamak için kullanır ikinci bir koleksiyon gerektirir _kiraları_ bölümler üzerinden. İzlenmekte olan koleksiyonu ve kiraları içeren koleksiyonu çalışmak tetikleyici için kullanılabilir olmalıdır.

 >[!IMPORTANT]
 > Birden çok işlevleri Cosmos DB tetikleyici aynı koleksiyon için kullanmak üzere yapılandırıldıysa, işlevlerin her biri bir adanmış kira koleksiyonu kullanmanız gerekir. Aksi takdirde işlevleri yalnızca biri tetiklenir. 

Tetikleyici belgeyi güncelleştirilmiş ya da eklenen, onu yalnızca belge sunar göstermez. Güncelleştirmeleri ve eklemeleri farklı şekilde ele almanız gerekir, zaman damgası alanları ekleme veya güncelleştirme uygulayarak bunu yapabilirsiniz.

## <a name="input"></a>Girdi

Azure Cosmos DB giriş bağlaması bir veya daha fazla Azure Cosmos DB belgeleri alır ve bunları işlevinin giriş parametresi olarak geçirir. Belge kimliği veya sorgu parametreleri işlevi çağırır tetikleyiciye bağlı belirlenebilir. 

>[!NOTE]
> Yok Azure Cosmos DB girdisi kullanın veya bir Cosmos DB hesabında MongoDB API kullanıyorsanız bağlamaları çıktı. Veri bozulması mümkündür.

## <a name="input---example-1"></a>Giriş - örnek 1

Tek bir belgenin okur dile özgü örneğe bakın:

* [C#](#input---c-example)
* [C# script (.csx)](#input---c-script-example)
* [F#](#input---f-example)
* [JavaScript](#input---javascript-example)

### <a name="input---c-example"></a>Giriş - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir belgenin bir belirli veritabanı ve koleksiyonu alır. 

İlk olarak, `Id` ve `Maker` değerleri bir `CarReview` örneği bir sıraya geçirilir. Cosmos bağlamanın kullandığı DB `Id` ve `Maker` belge veritabanından almak için kuyruk iletisi.

```cs
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Host;

    namespace CosmosDB
    {
        public static class SingleEntry
        {
            [FunctionName("SingleEntry")]
            public static void Run(
                [QueueTrigger("car-reviews", Connection = "StorageConnectionString")] CarReview carReview,
                [DocumentDB("cars", "car-reviews", PartitionKey = "{maker}", Id= "{id}", ConnectionStringSetting = "CarReviewsConnectionString")] CarReview document,
                TraceWriter log)
            {
                log.Info( $"Selected Review - {document?.Review}"); 
            }
        }
    }
```

Burada `CarReview` POCO:

 ```cs
    public class CarReview
    {
        public string Id { get; set; }
        public string Maker { get; set; }
        public string Description { get; set; }
        public string Model { get; set; }
        public string Image { get; set; }
        public string Review { get; set; }
    }
 ```

### <a name="input---c-script-example"></a>Giriş - C# kod örneği

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "inputDocument",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connection": "MyAccount_COSMOSDB",     
    "direction": "in"
}
```
[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding 
    public static void Run(string myQueueItem, dynamic inputDocument)
    {   
      inputDocument.text = "This has changed.";
    }
```

<a name="infsharp"></a>

### <a name="input---f-example"></a>Giriş - F # örnek

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

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

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kod aşağıdaki gibidir:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
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

### <a name="input---javascript-example"></a>Giriş - JavaScript örneği

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "inputDocumentIn",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "documentDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connection": "MyAccount_COSMOSDB",
    "direction": "out"
}
```
[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

```javascript
    // Change input document contents using Azure Cosmos DB input binding, using context.bindings.inputDocumentOut
    module.exports = function (context) {   
    context.bindings.inputDocumentOut = context.bindings.inputDocumentIn;
    context.bindings.inputDocumentOut.text = "This was updated!";
    context.done();
    };
```

## <a name="input---example-2"></a>Giriş - örnek 2

Birden çok belge okur dile özgü örneğe bakın:

* [C#](#input---c-example-2)
* [C# script (.csx)](#input---c-script-example-2)
* [JavaScript](#input---javascript-example-2)

### <a name="input---c-example-2"></a>Giriş - C# Örnek 2

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir SQL sorgusunu çalıştırır. Kullanılacak `SqlQuery` parametresi, en son beta sürümünü yüklemeniz gereken `Microsoft.Azure.WebJobs.Extensions.DocumentDB` NuGet paketi.

```csharp
    using System.Net;
    using System.Net.Http;
    using System.Collections.Generic;
    using Microsoft.Azure.WebJobs;
    using Microsoft.Azure.WebJobs.Extensions.Http;

    [FunctionName("CosmosDBSample")]
    public static HttpResponseMessage Run(
        [HttpTrigger(AuthorizationLevel.Anonymous, "get")] HttpRequestMessage req,
        [DocumentDB("test", "test", ConnectionStringSetting = "CosmosDB", SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")] IEnumerable<object> documents)
    {
        return req.CreateResponse(HttpStatusCode.OK, documents);
    }
```

### <a name="input---c-script-example-2"></a>Giriş - C# betik örnek 2

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlamasında gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. Sorgu parametrelerini özelleştirmek için bir sıra tetikleyici kullanarak bir SQL sorgusu tarafından belirtilen birden çok belge işlevi alır.

Bir parametre sırası tetikleyici sağlar `departmentId`. Bir kuyruk iletisi, `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür. 

Veri bağlama işte *function.json* dosyası:

```json
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

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

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

### <a name="input---javascript-example-2"></a>Giriş - JavaScript örnek 2

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlamasında gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. Sorgu parametrelerini özelleştirmek için bir sıra tetikleyici kullanarak bir SQL sorgusu tarafından belirtilen birden çok belge işlevi alır.

Bir parametre sırası tetikleyici sağlar `departmentId`. Bir kuyruk iletisi, `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür. 

Veri bağlama işte *function.json* dosyası:

```json
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

[Yapılandırma](#input---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

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

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Özniteliğin Oluşturucusu koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında daha fazla bilgi için bkz: [yapılandırma bölümü aşağıdaki](#input---configuration). 

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `DocumentDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     || ayarlanmalıdır `documentdb`.        |
|**Yönü**     || ayarlanmalıdır `in`.         |
|**Adı**     || İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**databaseName** |**DatabaseName** |Belge içeren veritabanı.        |
|**collectionName** |**collectionName** | Belge içeren koleksiyonu adı. |
|**id**    | **Kimlik** | Alınacak belge kimliği. Bu özelliği destekleyen [ifadeleri bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns). Her ikisi de ayarlamazsanız **kimliği** ve **sqlQuery** özellikleri. Bunlardan herhangi birinin ayarlamazsanız, tüm koleksiyon alınır. |
|**sqlQuery**  |**SqlQuery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Bu örnekte olduğu gibi çalışma zamanı bağlamaları özelliğini destekler: `SELECT * FROM c where c.departmentId = {departmentId}`. Her ikisi de ayarlamazsanız **kimliği** ve **sqlQuery** özellikleri. Bunlardan herhangi birinin ayarlamazsanız, tüm koleksiyon alınır.|
|**Bağlantı**     |**ConnectionStringSetting**|Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |
|**partitionKey**|**PartitionKey**|Arama için bölüm anahtarı değerini belirtir. Bağlama parametreler içerebilir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

İşlev başarıyla çıktığında, C# ve F # işlevleri adlandırılmış giriş parametreleri aracılığıyla giriş belgeye yapılan değişiklikler otomatik olarak kalıcıdır. 

JavaScript işlevleri güncelleştirmeleri otomatik olarak işlevi çıkış duruma getirilmez. Bunun yerine, kullanın `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` güncelleştirme yapmak için. Bkz: [JavaScript örnek](#input---javascript-example).

## <a name="output"></a>Çıktı

Sağlar bağlama Azure Cosmos DB çıktı yeni bir belge bir Azure Cosmos DB veritabanına yazar. 

>[!NOTE]
> Yok Azure Cosmos DB girdisi kullanın veya bir Cosmos DB hesabında MongoDB API kullanıyorsanız bağlamaları çıktı. Veri bozulması mümkündür.

## <a name="output---example"></a>Çıktı - örnek

Dile özgü örneğe bakın:

* [C#](#output---c-example)
* [C# script (.csx)](#output---c-script-example)
* [F#](#output---f-example)
* [JavaScript](#output---javascript-example)

### <a name="output---c-example"></a>Çıktı - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) kuyruk depolama biriminden iletisinde sağlanan verileri kullanarak bir veritabanı için bir belge ekleyen.

```cs
    using System;
    using Microsoft.Azure.WebJobs;

    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        document = new { Text = myQueueItem, id = Guid.NewGuid() };
    }
```

### <a name="output---c-script-example"></a>Çıktı - C# kod örneği

Aşağıdaki örnek, bağlama Azure Cosmos DB çıktı gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlevi, JSON şu biçimde alan sıra için bir sıra giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlev, her kayıt için şu biçimde Azure Cosmos DB belgeleri oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Veri bağlama işte *function.json* dosyası:

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

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

C# betik kod aşağıdaki gibidir:

```cs
    #r "Newtonsoft.Json"

    using Microsoft.Azure.WebJobs.Host;
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

Birden çok belge oluşturmak için bağlayabilirsiniz `ICollector<T>` veya `IAsyncCollector<T>` burada `T` desteklenen türlerden biri.

### <a name="output---f-example"></a>Çıktı - F # örnek

Aşağıdaki örnek, bağlama Azure Cosmos DB çıktı gösterir bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlevi, JSON şu biçimde alan sıra için bir sıra giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlev, her kayıt için şu biçimde Azure Cosmos DB belgeleri oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Veri bağlama işte *function.json* dosyası:

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
[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

F # kod aşağıdaki gibidir:

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

### <a name="output---javascript-example"></a>Çıktı - JavaScript örneği

Aşağıdaki örnek, bağlama Azure Cosmos DB çıktı gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlevi, JSON şu biçimde alan sıra için bir sıra giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlev, her kayıt için şu biçimde Azure Cosmos DB belgeleri oluşturur:

```json
{
    "id": "John Henry-123456",
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

Veri bağlama işte *function.json* dosyası:

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

[Yapılandırma](#output---configuration) bölümde, bu özellikleri açıklanmaktadır.

JavaScript kod aşağıdaki gibidir:

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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) NuGet paketi tanımlı öznitelik [Microsoft.Azure.WebJobs.Extensions.DocumentDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB).

Özniteliğin Oluşturucusu koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `CosmosDB` yöntemi imza özniteliği örnekte:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `CosmosDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**Türü**     || ayarlanmalıdır `documentdb`.        |
|**Yönü**     || ayarlanmalıdır `out`.         |
|**Adı**     || İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**databaseName** | **DatabaseName**|Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**collectionName** |**collectionName**  | Belgenin oluşturulduğu koleksiyon adı. |
|**createIfNotExists**  |**CreateIfNotExists**    | Yoksa koleksiyon oluşturulduğunda olup olmadığını gösteren bir Boole değeri. Varsayılan değer *false* yeni koleksiyonları etkileri maliyet ayrılmış işleme ile oluşturulduğundan. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**partitionKey**|**PartitionKey** |Zaman `CreateIfNotExists` true ise, oluşturulan koleksiyonu için bölüm anahtar yolu tanımlar.|
|**collectionThroughput**|**CollectionThroughput**| Zaman `CreateIfNotExists` true ise, tanımlar [işleme](../cosmos-db/set-throughput.md) oluşturulan koleksiyonu.|
|**Bağlantı**    |**ConnectionStringSetting** |Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Çıktı parametresi, işlevinde yazdığınızda, varsayılan olarak, bir belge veritabanınızda oluşturulur. Bu belge otomatik olarak oluşturulan GUID belge kimliği olarak sahiptir. Belirterek, belge kimliği çıktı belgesinin belirtebilirsiniz `id` özelliği JSON nesnesinde çıktı parametresi geçirildi. 

> [!Note]  
> Var olan bir belgeyi Kimliğini belirttiğinizde, yeni çıktı belgenin üzerine. 

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB hata kodları](https://docs.microsoft.com/en-us/rest/api/documentdb/http-status-codes-for-documentdb) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Cosmos DB tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Cosmos DB ile bilgisayar sunucusuz veritabanı hakkında daha fazla bilgi edinin](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
