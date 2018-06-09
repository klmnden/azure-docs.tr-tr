---
title: İşlevler için Azure Cosmos DB bağlamaları 2.x (Önizleme)
description: Azure Cosmos DB Tetikleyicileri ve bağlamaları Azure işlevlerinde nasıl kullanılacağını anlayın.
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
editor: ''
tags: ''
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem sunucusuz mimarisi
ms.service: functions; cosmos-db
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/21/2017
ms.author: tdykstra
ms.openlocfilehash: e40ba6bcfa1b6247f62849626d4a7803a76362a1
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35234878"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-2x-preview"></a>Azure işlevleri için Azure Cosmos DB bağlamaları 2.x (Önizleme)

> [!div class="op_single_selector" title1="Select the version of the Azure Functions runtime you are using: "]
> * [Sürüm 1 - Genel Kullanım](functions-bindings-cosmosdb.md)
> * [Sürüm 2 - Önizleme](functions-bindings-cosmosdb-v2.md)

Bu makale ile nasıl çalışılacağını açıklar [Azure Cosmos DB](..\cosmos-db\serverless-computing-database.md) Azure işlevlerinde bağlamaları 2.x. Tetiklemek, giriş ve Azure Cosmos DB bağlantılarında çıktı Azure işlevleri destekler.

> [!NOTE]
> Bu makalede içindir [Azure işlevleri sürüm 2.x](functions-versions.md), Önizleme'de olduğu.  Bu bağlamaların işlevlerde kullanma hakkında bilgi için 1.x, bkz: [Azure işlevleri için Azure Cosmos DB bağlamaları 1.x](functions-bindings-cosmosdb.md).
>
> Bu bağlama DocumentDB ilk olarak adlandırılıyordu. İşlevler sürümünde 2.x, tetikleyici, bağlamaları ve paketin tüm Cosmos DB adlandırılır.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="packages---functions-2x"></a>Paketler - 2.x işlevleri

Azure Cosmos DB bağlamaları için işlevleri sürüm 2.x içinde verilmiştir [Microsoft.Azure.WebJobs.Extensions.CosmosDB](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB) NuGet paketi, sürüm 3.x. Kaynak kodu bağlamaları için konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/) GitHub depo.

[!INCLUDE [functions-package-v2](../../includes/functions-package-v2.md)]

## <a name="trigger"></a>Tetikleyici

Azure Cosmos DB tetikleyici kullanır [Azure Cosmos DB Değiştir Akış](../cosmos-db/change-feed.md) eklemeleri için dinlenecek ve bölümler üzerinden yapılan güncelleştirmeler. Değişiklik akış ekler ve güncelleştirmeler, değil silme yayımlar.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betik (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Tetikleyici örnekler atla](#trigger---attributes)

### <a name="trigger---c-example"></a>Tetikleyici - C# örnek

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) ekler veya güncelleştirir belirtilen veritabanı ve koleksiyonu olduğunda çağrılır.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class CosmosTrigger
    {
        [FunctionName("CosmosTrigger")]
        public static void Run([CosmosDBTrigger(
            databaseName: "ToDoItems",
            collectionName: "Items",
            ConnectionStringSetting = "CosmosDBConnection",
            LeaseCollectionName = "leases",
            CreateLeaseCollectionIfNotExists = true)]IReadOnlyList<Document> documents, 
            TraceWriter log)
        {
            if (documents != null && documents.Count > 0)
            {
                log.Info($"Documents modified: {documents.Count}");
                log.Info($"First document Id: {documents[0].Id}");
            }
        }
    }
}
```

[Tetikleyici örnekler atla](#trigger---attributes)

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

[Tetikleyici örnekler atla](#trigger---attributes)

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

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [CosmosDBTrigger](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/Trigger/CosmosDBTriggerAttribute.cs) özniteliği.

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
|**type** || ayarlanmalıdır `cosmosDBTrigger`. |
|**direction** || ayarlanmalıdır `in`. Bu parametre, Azure portalında tetikleyici oluşturduğunuzda otomatik olarak ayarlanır. |
|**Adı** || Belge değişikliklerle listesini temsil eden işlevi kod içinde kullanılan değişken adı. | 
|**ConnectionStringSetting**|**ConnectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabınıza bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**databaseName**|**databaseName**  | İzlenmekte olan derlemesiyle Azure Cosmos DB veritabanının adı. |
|**collectionName** |**collectionName** | İzlenmekte olan koleksiyon adı. |
|**LeaseConnectionStringSetting** | **LeaseConnectionStringSetting** | (İsteğe bağlı) Kira koleksiyonunu tutan hizmeti ile bağlantı dizesi içeren bir uygulama ayarı adı. Ayarlandığında değil, `connectionStringSetting` değeri kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. Bağlantı dizesi kiraları koleksiyon için yazma izinlerine sahip olmalıdır.|
|**LeaseDatabaseName** |**LeaseDatabaseName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyonunu tutan veritabanının adı. Değil olarak ayarlandığında, değeri `databaseName` ayarı kullanılır. Bu parametre, bağlama portalda oluşturulduğunda otomatik olarak ayarlanır. |
|**LeaseCollectionName** | **LeaseCollectionName** | (İsteğe bağlı) Kira depolamak için kullanılan koleksiyon adı. Değil olarak ayarlandığında, değerin `leases` kullanılır. |
|**CreateLeaseCollectionIfNotExists** | **CreateLeaseCollectionIfNotExists** | (İsteğe bağlı) Ayarlandığında `true`, önceden var olmayan kiraları koleksiyonu otomatik olarak oluşturulur. Varsayılan değer `false`. |
|**LeasesCollectionThroughput**| **LeasesCollectionThroughput**| (İsteğe bağlı) İstek kiraları koleksiyonu oluşturulduğunda atamak için birimi miktarını tanımlar. Bu ayarı yalnızca kullanılan olduğunda, `createLeaseCollectionIfNotExists` ayarlanır `true`. Bu parametre, bağlama oluşturulduğunda portal kullanılarak otomatik olarak ayarlanır.
|**LeaseCollectionPrefix**| **LeaseCollectionPrefix**| (İsteğe bağlı) Ayarlandığında, bir önek etkili bir şekilde iki ayrı Azure aynı kira koleksiyonu farklı önekleri kullanarak paylaşmak işlevlere izin verme, bu işlev için kira koleksiyonundaki oluşturulan kiraları ekler.
|**FeedPollDelay**| **FeedPollDelay**| (İsteğe bağlı) Geçerli sonra tüm değişiklikler kümesi, bu, milisaniye cinsinden bir bölüm akışta yeni değişiklikleri için yoklama arasında gecikme tanımladığında boşaltmış. 5000 (5 saniye) varsayılandır.
|**LeaseAcquireInterval**| **LeaseAcquireInterval**| (İsteğe bağlı) Ayarlandığında, bunu, milisaniye cinsinden aralığı bölümleri bilinen konak örnekler arasında eşit olarak dağıtılmış, işlem için bir görevi devre dışı kazandırın tanımlar. 13000 (13 saniye) varsayılandır.
|**LeaseExpirationInterval**| **LeaseExpirationInterval**| (İsteğe bağlı) Ayarlandığında, bunu, milisaniye cinsinden kira bir bölüm temsil eden bir kira alınır aralığı tanımlar. Kira bu aralıkta yenilenmezse süresi dolacak şekilde neden olur ve bölüm sahipliğini başka bir örneğine taşınır. 60000 (60 saniye) varsayılandır.
|**LeaseRenewInterval**| **LeaseRenewInterval**| (İsteğe bağlı) Ayarlandığında, bunu, milisaniye cinsinden tüm kira yenileme aralığı şu anda bir örneği tarafından tutulan bölümler için tanımlar. 17000 (17 saniye) varsayılandır.
|**CheckpointFrequency**| **CheckpointFrequency**| (İsteğe bağlı) Ayarlandığında, bunu, milisaniye cinsinden kira denetim noktaları arasındaki süreyi tanımlar. Başarılı işlev çağrısı sonra her zaman bir varsayılandır.
|**MaxItemsPerInvocation**| **MaxItemsPerInvocation**| (İsteğe bağlı) Ayarlandığında, maksimum işlev çağrısı alınan öğeleri özelleştirir.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Tetikleyici depolamak için kullanır ikinci bir koleksiyon gerektirir _kiraları_ bölümler üzerinden. İzlenmekte olan koleksiyonu ve kiraları içeren koleksiyonu çalışmak tetikleyici için kullanılabilir olmalıdır.

>[!IMPORTANT]
> Birden çok işlevleri Cosmos DB tetikleyici aynı koleksiyon için kullanmak üzere yapılandırıldıysa, işlevlerin her biri bir adanmış kira koleksiyonu kullanın veya farklı bir belirtin `LeaseCollectionPrefix` her işlevi için. Aksi takdirde işlevleri yalnızca biri tetiklenir. Öneki hakkında daha fazla bilgi için bkz: [yapılandırma bölümü](#trigger---configuration).

Tetikleyici belgeyi güncelleştirilmiş ya da eklenen, onu yalnızca belge sunar göstermez. Güncelleştirmeleri ve eklemeleri farklı şekilde ele almanız gerekir, zaman damgası alanları ekleme veya güncelleştirme uygulayarak bunu yapabilirsiniz.

## <a name="input"></a>Girdi

Azure Cosmos DB giriş bağlaması bir veya daha fazla Azure Cosmos DB belgeleri alır ve bunları işlevinin giriş parametresi olarak geçirir. Belge kimliği veya sorgu parametreleri işlevi çağırır tetikleyiciye bağlı belirlenebilir. 

>[!NOTE]
> Yok Azure Cosmos DB girdisi kullanın veya bir Cosmos DB hesabında MongoDB API kullanıyorsanız bağlamaları çıktı. Veri bozulması mümkündür.

## <a name="input---examples"></a>Giriş - örnekleri

Tek bir belgenin bir kimlik değeri belirterek okuma dile özgü örneklere bakın:

* [C#](#input---c-examples)
* [C# betik (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Giriş örnekleri atla](#input---attributes)

### <a name="input---c-examples"></a>Giriş - C# örnekleri

Bu bölüm, aşağıdaki örneklerde içerir:

* [Sıra tetikleyici, JSON Kimliğinden Ara](#queue-trigger-look-up-id-from-json-c)
* [HTTP tetikleyicisi, Sorgu dizesinden kimliği arama](#http-trigger-look-up-id-from-query-string-c)
* [HTTP tetikleyicisi, rota verilerinden kimliği arama](#http-trigger-look-up-id-from-route-data-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden kimliği arama](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP tetiklemek, SqlQuery kullanarak birden çok belge Al](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP tetiklemek, DocumentClient kullanarak birden çok belge Al](#http-trigger-get-multiple-docs-using-documentclient-c)

Örnekler için basit bir başvuru `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Sıra tetikleyici, JSON (C#) Kimliğinden Ara

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir belgenin alır. İşlevi bir JSON nesnesi içeren bir kuyruk iletisi tarafından tetiklenir. Sıra tetikleyici JSON adlı bir nesnesine ayrıştırır `ToDoItemLookup`, aramak için Kimliğini içerir. Kimliği almak için kullanılan bir `ToDoItem` belgeden belirtilen veritabanı ve koleksiyonu.

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItemLookup
    {
        public string ToDoItemId { get; set; }
    }
}
```

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{ToDoItemId}")]ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed Id={toDoItemLookup?.ToDoItemId}");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP tetikleyicisi, kimliği Ara Sorgu dizesinden (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir belgenin alır. İşlev aramak için Kimliğini belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belgeden belirtilen veritabanı ve koleksiyonu.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{Query.id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP tetikleyicisi, kimliği Ara rota verilerinden (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir belgenin alır. İşlev kullandığı aramak için Kimliğini belirtmek için verileri yönlendiren bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belgeden belirtilen veritabanı ve koleksiyonu.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems/{id}")]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                Id = "{id}")] ToDoItem toDoItem,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            if (toDoItem == null)
            {
                log.Info($"ToDo item not found");
            }
            else
            {
                log.Info($"Found ToDo item, Description={toDoItem.Description}");
            }
            return new OkResult();
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP tetikleyicisi, SqlQuery (C#) kullanarak rota verilerinden kimliği arama

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) tek bir belgenin alır. İşlev kullandığı aramak için Kimliğini belirtmek için verileri yönlendiren bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belgeden belirtilen veritabanı ve koleksiyonu. 

Örnek bir bağlama ifadesinde kullanmayı gösterir `SqlQuery` parametresi. Rota veri geçirebilirsiniz `SqlQuery` , ancak şu anda gösterildiği gibi parametre [sorgu dizesi değerlerini geçiremezsiniz](https://github.com/Azure/azure-functions-host/issues/2554#issuecomment-392084583).


```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = "todoitems2/{id}")]HttpRequest req,
            [CosmosDB("ToDoItems", "Items", 
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "select * from ToDoItems r where r.id = {id}")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP tetiklemek, SqlQuery (C#) kullanarak birden çok belge Al

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) belgelerin listesini alır. İşlevi bir HTTP isteği tarafından tetiklenir. Sorguyu belirtilen `SqlQuery` öznitelik özelliği.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV2
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static IActionResult Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection", 
                SqlQuery = "SELECT top 2 * FROM c order by c._ts desc")]
                IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return new OkResult();
        }
    }
}

```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP tetiklemek, DocumentClient (C#) kullanarak birden çok belge Al

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) belgelerin listesini alır. İşlevi bir HTTP isteği tarafından tetiklenir. Kod kullanan bir `DocumentClient` belgelerin listesini okumak için Azure Cosmos DB bağlama tarafından sağlanan örneği. `DocumentClient` Örneği yazma işlemleri için de kullanılabilirdi.

```cs
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", 
                Route = null)]HttpRequest req,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            var searchterm = req.Query["searchterm"];
            if (string.IsNullOrWhiteSpace(searchterm))
            {
                return (ActionResult)new NotFoundResult();
            }

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");

            log.Info($"Searching for: {searchterm}");

            IDocumentQuery<ToDoItem> query = client.CreateDocumentQuery<ToDoItem>(collectionUri)
                .Where(p => p.Description.Contains(searchterm))
                .AsDocumentQuery();

            while (query.HasMoreResults)
            {
                foreach (ToDoItem result in await query.ExecuteNextAsync())
                {
                    log.Info(result.Description);
                }
            }
            return new OkResult();
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

### <a name="input---c-script-examples"></a>Giriş - C# kod örnekleri

Bu bölüm, çeşitli kaynaklardan bir kimlik değeri belirterek tek bir belgenin okuma aşağıdaki örneklerde içerir:

* Sıra Tetikleyici Kimliği Ara sıra iletisi
* Sıra Tetikleyici Kimliği Ara sıra iletisi, SqlQuery kullanma

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-c-script"></a>Sıra Tetikleyici Kimliği Ara sıra iletisi (C# kod)

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "partitionKey": "{partition key value}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
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

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-queue-message-using-sqlquery-c-script"></a>Sıra tetikleyici, SqlQuery (C# betiği) kullanılarak kimliği kuyruk iletisi Ara

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlamasında gösterir bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanır. Sorgu parametrelerini özelleştirmek için bir sıra tetikleyici kullanarak bir SQL sorgusu tarafından belirtilen birden çok belge işlevi alır.

Bir parametre sırası tetikleyici sağlar `departmentId`. Bir kuyruk iletisi, `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür. 

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
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

[Giriş örnekleri atla](#input---attributes)

### <a name="input---javascript-examples"></a>Giriş - JavaScript örnekleri

Bu bölüm, çeşitli kaynaklardan bir kimlik değeri belirterek tek bir belgenin okuma aşağıdaki örneklerde içerir:

* Sıra Tetikleyici Kimliği Ara sıra iletisi
* Sıra Tetikleyici Kimliği Ara sıra iletisi, SqlQuery kullanma

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-javascript"></a>Sıra Tetikleyici Kimliği Ara sıra iletisi (JavaScript)

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "inputDocumentIn",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger_payload_property}",
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
    "direction": "in"
},
{
    "name": "inputDocumentOut",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": false,
    "partitionKey": "{queueTrigger_payload_property}",
    "connectionStringSetting": "MyAccount_COSMOSDB",
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

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-queue-message-using-sqlquery-javascript"></a>Sıra Tetikleyici Kimliği Ara sıra iletisi, SqlQuery (JavaScript) kullanarak

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlamasında gösterir bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanır. Sorgu parametrelerini özelleştirmek için bir sıra tetikleyici kullanarak bir SQL sorgusu tarafından belirtilen birden çok belge işlevi alır.

Bir parametre sırası tetikleyici sağlar `departmentId`. Bir kuyruk iletisi, `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür. 

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "documents",
    "type": "cosmosDB",
    "direction": "in",
    "databaseName": "MyDb",
    "collectionName": "MyCollection",
    "sqlQuery": "SELECT * from c where c.departmentId = {departmentId}",
    "connectionStringSetting": "CosmosDBConnection"
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

[Giriş örnekleri atla](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Giriş - F # örnekleri

Cosmos DB giriş bağlamasında aşağıdaki örnekte bir *function.json* dosyası ve bir [F # işlevi](functions-reference-fsharp.md) bağlama kullanır. İşlev tek bir belgenin okur ve belgenin metin değeri güncelleştirir.

Veri bağlama işte *function.json* dosyası:

```json
{
    "name": "inputDocument",
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "id" : "{queueTrigger}",
    "connectionStringSetting": "MyAccount_COSMOSDB",     
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

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliği.

Özniteliğin Oluşturucusu koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında daha fazla bilgi için bkz: [yapılandırma bölümü aşağıdaki](#input---configuration). 

## <a name="input---configuration"></a>Girişi - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `CosmosDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     || ayarlanmalıdır `cosmosDB`.        |
|**direction**     || ayarlanmalıdır `in`.         |
|**Adı**     || İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**databaseName** |**databaseName** |Belge içeren veritabanı.        |
|**collectionName** |**collectionName** | Belge içeren koleksiyonu adı. |
|**Kimliği**    | **Kimlik** | Alınacak belge kimliği. Bu özelliği destekleyen [ifadeleri bağlama](functions-triggers-bindings.md#binding-expressions-and-patterns). Her ikisi de ayarlamazsanız **kimliği** ve **sqlQuery** özellikleri. Bunlardan herhangi birinin ayarlamazsanız, tüm koleksiyon alınır. |
|**sqlQuery**  |**SqlQuery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Bu örnekte olduğu gibi çalışma zamanı bağlamaları özelliğini destekler: `SELECT * FROM c where c.departmentId = {departmentId}`. Her ikisi de ayarlamazsanız **kimliği** ve **sqlQuery** özellikleri. Bunlardan herhangi birinin ayarlamazsanız, tüm koleksiyon alınır.|
|**ConnectionStringSetting**     |**ConnectionStringSetting**|Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |
|**PartitionKey**|**PartitionKey**|Arama için bölüm anahtarı değerini belirtir. Bağlama parametreler içerebilir.|

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

* [C#](#output---c-examples)
* [C# betik (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Ayrıca bkz. [giriş örnek](#input---c-examples) kullanan `DocumentClient`.

[Çıkışı örnekleri atla](#output---attributes)

### <a name="ouput---c-examples"></a>Çıkış - C# örnekleri

Bu bölüm, aşağıdaki örneklerde içerir:

* Sıra tetikleyici, bir belge yazma
* Sıra tetikleyici, IAsyncCollector kullanarak yazma belgeleri

Örnekler için basit bir başvuru `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV2
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Çıkışı örnekleri atla](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Sıra tetikleyici, bir belge yazma (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) kuyruk depolama biriminden iletisinde sağlanan verileri kullanarak bir veritabanı için bir belge ekleyen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV2
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]out dynamic document,
            TraceWriter log)
        {
            document = new { Description = queueMessage, id = Guid.NewGuid() };

            log.Info($"C# Queue trigger function inserted one row");
            log.Info($"Description={queueMessage}");
        }
    }
}
```

[Çıkışı örnekleri atla](#output---attributes)

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Sıra tetikleyici, IAsyncCollector (C#) kullanarak yazma belgeleri

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir kuyruk iletisi JSON sağlanan verileri kullanarak, bir veritabanına belgeleri topluluğu ekleyen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV2
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [CosmosDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")]
                IAsyncCollector<ToDoItem> toDoItemsOut,
            TraceWriter log)
        {
            log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

            foreach (ToDoItem toDoItem in toDoItemsIn)
            {
                log.Info($"Description={toDoItem.Description}");
                await toDoItemsOut.AddAsync(toDoItem);
            }
        }
    }
}
```

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---c-script-examples"></a>Çıktı - C# kod örnekleri

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
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

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---javascript-examples"></a>Çıktı - JavaScript örnekleri

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
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

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---f-examples"></a>Çıktı - F # örnekleri

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
    "type": "cosmosDB",
    "databaseName": "MyDatabase",
    "collectionName": "MyCollection",
    "createIfNotExists": true,
    "connectionStringSetting": "MyAccount_COSMOSDB",     
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

## <a name="output---attributes"></a>Çıktı - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [CosmosDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/master/WebJobs.Extensions.CosmosDB/CosmosDBAttribute.cs) özniteliği.

Özniteliğin Oluşturucusu koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikler hakkında daha fazla bilgi için bkz: [çıktı - yapılandırma](#output---configuration). Burada bir `CosmosDB` yöntemi imza özniteliği örnekte:

```csharp
    [FunctionName("QueueToDocDB")]        
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [CosmosDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Tam bir örnek için bkz: [çıktısı - C# örnek](#output---c-example).

## <a name="output---configuration"></a>Çıktı - yapılandırma

Aşağıdaki tabloda, kümesinde bağlama yapılandırma özellikleri açıklanmaktadır *function.json* dosya ve `CosmosDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     || ayarlanmalıdır `cosmosDB`.        |
|**direction**     || ayarlanmalıdır `out`.         |
|**Adı**     || İşlevinde belgeyi temsil bağlama parametresinin adı.  |
|**databaseName** | **databaseName**|Belgenin oluşturulduğu koleksiyonu içeren veritabanı.     |
|**collectionName** |**collectionName**  | Belgenin oluşturulduğu koleksiyon adı. |
|**CreateIfNotExists**  |**CreateIfNotExists**    | Yoksa koleksiyon oluşturulduğunda olup olmadığını gösteren bir Boole değeri. Varsayılan değer *false* yeni koleksiyonları etkileri maliyet ayrılmış işleme ile oluşturulduğundan. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/cosmos-db/).  |
|**PartitionKey**|**PartitionKey** |Zaman `CreateIfNotExists` true ise, oluşturulan koleksiyonu için bölüm anahtar yolu tanımlar.|
|**CollectionThroughput**|**CollectionThroughput**| Zaman `CreateIfNotExists` true ise, tanımlar [işleme](../cosmos-db/set-throughput.md) oluşturulan koleksiyonu.|
|**ConnectionStringSetting**    |**ConnectionStringSetting** |Azure Cosmos DB bağlantı dizesi içeren uygulama ayarı adı.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıktı - kullanım

Çıktı parametresi, işlevinde yazdığınızda, varsayılan olarak, bir belge veritabanınızda oluşturulur. Bu belge otomatik olarak oluşturulan GUID belge kimliği olarak sahiptir. Belirterek, belge kimliği çıktı belgesinin belirtebilirsiniz `id` özelliği JSON nesnesinde çıktı parametresi geçirildi. 

> [!Note]  
> Var olan bir belgeyi Kimliğini belirttiğinizde, yeni çıktı belgenin üzerine. 

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB hata kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Cosmos DB tetikleyicisi kullanan bir hızlı başlangıç gidin](functions-create-cosmos-db-triggered-function.md)

> [!div class="nextstepaction"]
> [Cosmos DB ile bilgisayar sunucusuz veritabanı hakkında daha fazla bilgi edinin](..\cosmos-db\serverless-computing-database.md)

> [!div class="nextstepaction"]
> [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)
