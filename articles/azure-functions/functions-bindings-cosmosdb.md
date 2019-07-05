---
title: İşlevleri için Azure Cosmos DB bağlamaları 1.x
description: Azure Cosmos DB Tetikleyicileri ve bağlamaları Azure işlevleri'nde nasıl kullanılacağını anlayın.
services: functions
author: craigshoemaker
ms.author: cshoe
manager: gwallace
keywords: Azure işlevleri, İşlevler, olay işleme dinamik işlem, sunucusuz mimari
ms.service: azure-functions
ms.topic: reference
ms.date: 11/21/2017
ms.custom: seodec18
ms.openlocfilehash: 0e6782c48543723438ee332313de268117dee3e9
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2019
ms.locfileid: "67480732"
---
# <a name="azure-cosmos-db-bindings-for-azure-functions-1x"></a>Azure işlevleri için Azure Cosmos DB bağlamaları 1.x

> [!div class="op_single_selector" title1="Kullanmakta olduğunuz Azure işlevleri çalışma zamanı sürümünü seçin: "]
> * [Sürüm 1](functions-bindings-cosmosdb.md)
> * [Sürüm 2](functions-bindings-cosmosdb-v2.md)

Bu makalede ile nasıl çalışılacağı açıklanmaktadır [Azure Cosmos DB](../cosmos-db/serverless-computing-database.md) Azure işlevleri'nde bağlar. Tetiklemek, giriş ve çıktı bağlaması Azure Cosmos DB için Azure işlevleri destekler.

> [!NOTE]
> Bu makalede, Azure işlevleri için olan 1.x. Bu bağlamaları işlevlerini kullanma hakkında daha fazla bilgi için 2.x bkz [Azure işlevleri için Azure Cosmos DB bağlamaları 2.x](functions-bindings-cosmosdb-v2.md).
>
>Bu bağlama başlangıçta DocumentDB olarak adlandırılıyordu. İşlevleri sürümünde 1.x, yalnızca tetikleyici olan Cosmos DB;'olarak yeniden adlandırıldı giriş bağlaması, çıkış bağlaması ve NuGet paketini DocumentDB adını korur.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

> [!NOTE]
> Azure Cosmos DB bağlamaları yalnızca SQL API ile kullanım için desteklenir. Tüm diğer Azure Cosmos DB API'leri için veritabanına işlevinizden API'niz için statik bir istemci kullanarak erişmeli dahil olmak üzere [Azure Cosmos DB'nin MongoDB API'si](../cosmos-db/mongodb-introduction.md), [Cassandra API](../cosmos-db/cassandra-introduction.md), [ Gremlin API](../cosmos-db/graph-introduction.md), ve [tablo API'si](../cosmos-db/table-introduction.md).

## <a name="packages---functions-1x"></a>Paketler - 1.x işlevleri

Azure Cosmos DB bağlamaları için İşlevler sürümünü 1.x altında sağlanmıştır [Microsoft.Azure.WebJobs.Extensions.DocumentDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DocumentDB) NuGet paketi sürüm 1.x. Bağlamaları için kaynak kodu konusu [azure webjobs sdk uzantıları](https://github.com/Azure/azure-webjobs-sdk-extensions/tree/v2.x/src/WebJobs.Extensions.DocumentDB) GitHub deposu.

[!INCLUDE [functions-package](../../includes/functions-package.md)]

## <a name="trigger"></a>Tetikleyici

Azure Cosmos DB tetikleyicisi kullanan [Azure Cosmos DB değişiklik akışı](../cosmos-db/change-feed.md) ekler dinlemek ve bölümler arasında güncelleştirmeler. Değişiklik akışı, ekler ve güncelleştirme, silme değil yayımlar.

## <a name="trigger---example"></a>Tetikleyici - örnek

Dile özgü örneğe bakın:

* [C#](#trigger---c-example)
* [C# betiği (.csx)](#trigger---c-script-example)
* [JavaScript](#trigger---javascript-example)

[Tetikleyici örnekler atla](#trigger---attributes)

### <a name="trigger---c-example"></a>Tetikleyici - C# örneği

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) ekler veya güncelleştirir belirtilen veritabanı ve koleksiyonu olduğunda çağrılır.

```cs
using Microsoft.Azure.Documents;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;

namespace CosmosDBSamplesV1
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

### <a name="trigger---c-script-example"></a>Tetikleyici - C# betiği örneği

Aşağıdaki örnek, bağlama bir Cosmos DB tetikleyicisi gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. Cosmos DB kayıt değiştirildiğinde işlevi günlük iletisi yazar.

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

C# betik kodunu şu şekildedir:

```cs
    #r "Microsoft.Azure.Documents.Client"
    
    using System;
    using Microsoft.Azure.Documents;
    using System.Collections.Generic;
    

    public static void Run(IReadOnlyList<Document> documents, TraceWriter log)
    {
        log.Info("Documents modified " + documents.Count);
        log.Info("First document Id " + documents[0].Id);
    }
```

[Tetikleyici örnekler atla](#trigger---attributes)

### <a name="trigger---javascript-example"></a>Tetikleyici - JavaScript örneği

Aşağıdaki örnek, bağlama bir Cosmos DB tetikleyicisi gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. Cosmos DB kayıt değiştirildiğinde işlevi günlük iletisi yazar.

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

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikleri hakkında daha fazla bilgi için bkz. [tetikleyici - yapılandırma](#trigger---configuration). İşte bir `CosmosDBTrigger` özniteliği örnek bir yöntem imzası:

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

Tam bir örnek için bkz. [tetikleyici - C# örneği](#trigger---c-example).

## <a name="trigger---configuration"></a>Tetikleyici - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `CosmosDBTrigger` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type** || Ayarlanmalıdır `cosmosDBTrigger`. |
|**direction** || Ayarlanmalıdır `in`. Azure portalında tetikleyicisi oluşturduğunuzda bu parametre otomatik olarak ayarlanır. |
|**name** || Değişken adı değişikliklerle belgelerin listesini temsil eden bir işlev kodunu kullanılır. |
|**connectionStringSetting**|**connectionStringSetting** | İzlenmekte olan Azure Cosmos DB hesabına bağlanmak için kullanılan bağlantı dizesi içeren bir uygulama ayarı adı. |
|**databaseName**|**databaseName**  | İzlenmekte olan toplama ile Azure Cosmos DB veritabanının adı. |
|**collectionName** |**collectionName** | İzlenmekte olan koleksiyonun adı. |
|**leaseConnectionStringSetting** | **leaseConnectionStringSetting** | (İsteğe bağlı) Kira koleksiyonu içeren hizmete yönelik bağlantı dizesini içeren bir uygulama ayarı adı. Ne zaman ayarlanmadı, `connectionStringSetting` değeri kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. Bağlantı dizesini kiralarını koleksiyonuna yazma izinlerine sahip olmalıdır.|
|**leaseDatabaseName** |**leaseDatabaseName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonu içeren veritabanının adı. Ne zaman ayarlı değil, değerini `databaseName` ayarı kullanılır. Bu parametre, portalda bağlama oluşturulduğunda otomatik olarak ayarlanır. |
|**leaseCollectionName** | **leaseCollectionName** | (İsteğe bağlı) Kiraları depolamak için kullanılan koleksiyonun adı. Ne zaman ayarlı değil, değer `leases` kullanılır. |
|**createLeaseCollectionIfNotExists** | **createLeaseCollectionIfNotExists** | (İsteğe bağlı) Ayarlandığında `true`, kiralarını koleksiyonuna zaten mevcut değilse otomatik olarak oluşturulur. Varsayılan değer `false` şeklindedir. |
|**leasesCollectionThroughput**| **leasesCollectionThroughput**| (İsteğe bağlı) Kiralarını koleksiyonuna oluşturulduğunda atamak için istek birimi miktarı tanımlar. Bu ayar yalnızca kullanılan yaparken önemlidir `createLeaseCollectionIfNotExists` ayarlanır `true`. Bu parametre, portalı kullanarak bağlama oluşturulduğunda otomatik olarak ayarlanır.
|**leaseCollectionPrefix**| **leaseCollectionPrefix**| (İsteğe bağlı) Ayarlandığında, bir önek etkili bir şekilde iki ayrı Azure aynı kira koleksiyonu farklı önekler kullanarak paylaşmak işlevlere izin verme, bu işlev için kira koleksiyonu oluşturulan kiraları ekler.
|**feedPollDelay**| **feedPollDelay**| (İsteğe bağlı) Kümesi, milisaniye cinsinden gecikme bir bölüm akışın yeni değişiklikleri için yoklama arasında tanımlar, tüm geçerli değişiklikleri boşaltılır. 5000 (5 saniye) varsayılandır.
|**leaseAcquireInterval**| **leaseAcquireInterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden aralığı bölümler bilinen barındırma örnekleri arasında eşit olarak dağıtılmış, işlem için bir görev başlatabilir tanımlar. 13000 (13 saniye) varsayılandır.
|**leaseExpirationInterval**| **leaseExpirationInterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira bir bölüm temsil eden bir kira alınmış aralığı tanımlar. Kira bu aralıkta yenilenmezse, süresi dolacak şekilde neden olur ve bölüm sahipliğini başka bir örneğine taşınır. 60000 (60 saniye) varsayılandır.
|**leaseRenewInterval**| **leaseRenewInterval**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden geçerli bir örnek tarafından tutulan bölümler için tüm kira yenileme aralığı tanımlar. 17000 (17 saniye) varsayılandır.
|**checkpointFrequency**| **checkpointFrequency**| (İsteğe bağlı) Ayarlandığında, bu, milisaniye cinsinden kira kontrol noktaları arasındaki süreyi tanımlar. Her zaman her işlev çağrısından sonra varsayılandır.
|**maxItemsPerInvocation**| **maxItemsPerInvocation**| (İsteğe bağlı) Ayarlandığında, işlev çağrısı alınan öğeleri en uzun süreyi özelleştirir.
|**startFromBeginning**| **startFromBeginning**| (İsteğe bağlı) Ayarlandığında, değişiklik geçmişini geçerli zamanı yerine koleksiyonunun başından itibaren okumaya başlamak için tetikleyici söyler. Bu, yalnızca tetikleyici başladığında, sonraki çalıştırmaları, kontrol noktaları gibi zaten depolanmış ilk kez çalışır. Bu ayar `true` olduğunda önceden oluşturulmuş kiraları etkisi yoktur.

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="trigger---usage"></a>Tetikleyici - kullanım

Tetikleyici depolamak için kullandığı ikinci bir koleksiyon gerektirir _kiraları_ bölümler üzerinden. İzlenmekte olan toplama hem kiraları içeren koleksiyonu tetikleyicinin çalışma için kullanılabilir olmalıdır.

>[!IMPORTANT]
> Birden çok işlevleri Cosmos DB tetikleyicisi aynı koleksiyon için kullanmak üzere yapılandırıldıysa, bu işlevlerin her biri adanmış kira koleksiyonu kullanın veya farklı bir belirtin `LeaseCollectionPrefix` bulunabilir. Aksi takdirde, yalnızca bir işlev tetiklenir. Önek hakkında daha fazla bilgi için bkz. [yapılandırma bölümü](#trigger---configuration).

Tetikleyici, bir belge güncelleştirildi veya eklenen belge yalnızca sağladığı göstermiyor. Güncelleştirmeler ve ekler farklı şekilde işlemek istiyorsanız, zaman damgası alanları ekleme veya güncelleştirme uygulayarak yapabilirsiniz.

## <a name="input"></a>Girdi

Azure Cosmos DB giriş bağlama, bir veya daha fazla Azure Cosmos DB belgesi alınacağını SQL API'sini kullanır ve bunları işlevin giriş parametresi geçirir. Belge kimliği veya sorgu parametreleri işlevi çağıran bir tetikleyiciye bağlı olarak belirlenebilir.

## <a name="input---examples"></a>Giriş - örnekler

Tek bir belgenin kimlik değerini belirterek okuma dile özgü örneklere bakın:

* [C#](#input---c-examples)
* [C# betiği (.csx)](#input---c-script-examples)
* [JavaScript](#input---javascript-examples)
* [F#](#input---f-examples)

[Giriş örnekleri atla](#input---attributes)

### <a name="input---c-examples"></a>Giriş - C# örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON Kimliğinden Ara](#queue-trigger-look-up-id-from-json-c)
* [HTTP tetikleyicisi, Sorgu dizesinden Kimliği Ara](#http-trigger-look-up-id-from-query-string-c)
* [HTTP tetikleyicisi, rota verilerinden Kimliği Ara](#http-trigger-look-up-id-from-route-data-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak rota verilerinden Kimliği Ara](#http-trigger-look-up-id-from-route-data-using-sqlquery-c)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belgeleri edinin](#http-trigger-get-multiple-docs-using-sqlquery-c)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belgeleri edinin](#http-trigger-get-multiple-docs-using-documentclient-c)

Örnekler için basit bir başvuru `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-c"></a>Kuyruk tetikleyicisi, JSON (C#) Kimliğinden Ara

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir belge alır. İşlev bir JSON nesnesi içeren bir kuyruk iletisi tarafından tetiklenir. Kuyruk tetikleyicisi JSON adlı bir nesne ayrıştırır `ToDoItemLookup`, aramak üzere bir kimlik içerir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

```cs
namespace CosmosDBSamplesV1
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

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromJSON
    {
        [FunctionName("DocByIdFromJSON")]
        public static void Run(
            [QueueTrigger("todoqueueforlookup")] ToDoItemLookup toDoItemLookup,
            [DocumentDB(
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

#### <a name="http-trigger-look-up-id-from-query-string-c"></a>HTTP tetikleyicisi, kimliği arama Sorgu dizesinden (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir belge alır. İşlevi, aranacak kimliği belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromQueryString
    {
        [FunctionName("DocByIdFromQueryString")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
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
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c"></a>HTTP tetikleyicisi, kimliği bir ara rota verilerinden (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir belge alır. İşlevi, kullandığı veri aramak için kimliği belirtmek için yol bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteData
    {
        [FunctionName("DocByIdFromRouteData")]
        public static HttpResponseMessage Run(
            [HttpTrigger(
                AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems/{id}")]HttpRequestMessage req,
            [DocumentDB(
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
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-using-sqlquery-c"></a>HTTP tetikleyicisi, SqlQuery (C#) kullanarak rota verilerinden Kimliği Ara

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , tek bir belge alır. İşlevi, kullandığı veri aramak için kimliği belirtmek için yol bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocByIdFromRouteDataUsingSqlQuery
    {
        [FunctionName("DocByIdFromRouteDataUsingSqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post",
                Route = "todoitems2/{id}")]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection",
                SqlQuery = "select * from ToDoItems r where r.id = {id}")] IEnumerable<ToDoItem> toDoItems,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");
            foreach (ToDoItem toDoItem in toDoItems)
            {
                log.Info(toDoItem.Description);
            }
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c"></a>HTTP tetikleyicisi, SqlQuery (C#) kullanarak birden çok belgeleri edinin

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , belgelerin listesini alır. İşlevi, bir HTTP isteği tarafından tetiklenir. Sorguyu belirtilen `SqlQuery` özelliği özniteliği.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System.Collections.Generic;
using System.Net;
using System.Net.Http;

namespace CosmosDBSamplesV1
{
    public static class DocsBySqlQuery
    {
        [FunctionName("DocsBySqlQuery")]
        public static HttpResponseMessage Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]
                HttpRequestMessage req,
            [DocumentDB(
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
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c"></a>HTTP tetikleyicisi, DocumentClient (C#) kullanarak birden çok belgeleri edinin

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) , belgelerin listesini alır. İşlevi, bir HTTP isteği tarafından tetiklenir. Kod bir `DocumentClient` belgelerin listesini okumak için Azure Cosmos DB'ye bağlama tarafından sağlanan örneği. `DocumentClient` Örneği yazma işlemleri için de kullanılabilir.

```cs
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Azure.WebJobs.Host;
using System;
using System.Linq;
using System.Net;
using System.Net.Http;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class DocsByUsingDocumentClient
    {
        [FunctionName("DocsByUsingDocumentClient")]
        public static async Task<HttpResponseMessage> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)]HttpRequestMessage req,
            [DocumentDB(
                databaseName: "ToDoItems",
                collectionName: "Items",
                ConnectionStringSetting = "CosmosDBConnection")] DocumentClient client,
            TraceWriter log)
        {
            log.Info("C# HTTP trigger function processed a request.");

            Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
            string searchterm = req.GetQueryNameValuePairs()
                .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
                .Value;

            if (searchterm == null)
            {
                return req.CreateResponse(HttpStatusCode.NotFound);
            }

            log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
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
            return req.CreateResponse(HttpStatusCode.OK);
        }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

### <a name="input---c-script-examples"></a>Giriş - C# betik örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, dizeden Kimliği Ara](#queue-trigger-look-up-id-from-string-c-script)
* [Tetikleyici kuyruk, SqlQuery kullanarak birden çok belgeleri edinin](#queue-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, Sorgu dizesinden Kimliği Ara](#http-trigger-look-up-id-from-query-string-c-script)
* [HTTP tetikleyicisi, rota verilerinden Kimliği Ara](#http-trigger-look-up-id-from-route-data-c-script)
* [HTTP tetikleyicisi, SqlQuery kullanarak birden çok belgeleri edinin](#http-trigger-get-multiple-docs-using-sqlquery-c-script)
* [HTTP tetikleyicisi, DocumentClient kullanarak birden çok belgeleri edinin](#http-trigger-get-multiple-docs-using-documentclient-c-script)

İçin basit bir HTTP tetikleyici örneklere bakın `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-string-c-script"></a>Kuyruk tetikleyicisi, kimliği arama dizesinden (C# betik)

Aşağıdaki örnek, bir Cosmos DB giriş bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

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

C# betik kodunu şu şekildedir:

```cs
    using System;

    // Change input document contents using Azure Cosmos DB input binding
    public static void Run(string myQueueItem, dynamic inputDocument)
    {
        inputDocument.text = "This has changed.";
    }
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-c-script"></a>Tetikleyici kuyruk, SqlQuery (C# betik) kullanarak birden çok belgeleri edinin

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, belirtilen sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından birden çok belge alır.

Kuyruk tetikleyicisi parametre sağlar `departmentId`. Bir kuyruk iletisinin `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür.

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

C# betik kodunu şu şekildedir:

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

#### <a name="http-trigger-look-up-id-from-query-string-c-script"></a>HTTP tetikleyicisi, arama (C# betik) Sorgu dizesinden kimliği

Aşağıdaki örnekte gösterildiği bir [C# betik işlevi](functions-reference-csharp.md) , tek bir belge alır. İşlevi, aranacak kimliği belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
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
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-c-script"></a>HTTP tetikleyicisi, arama (C# betik) rota verilerinden kimliği

Aşağıdaki örnekte gösterildiği bir [C# betik işlevi](functions-reference-csharp.md) , tek bir belge alır. İşlevi, kullandığı veri aramak için kimliği belirtmek için yol bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, ToDoItem toDoItem, TraceWriter log)
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
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-sqlquery-c-script"></a>HTTP tetikleyicisi, SqlQuery (C# betik) kullanarak birden çok belgeleri edinin

Aşağıdaki örnekte gösterildiği bir [C# betik işlevi](functions-reference-csharp.md) , belgelerin listesini alır. İşlevi, bir HTTP isteği tarafından tetiklenir. Sorguyu belirtilen `SqlQuery` özelliği özniteliği.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItems",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "sqlQuery": "SELECT top 2 * FROM c order by c._ts desc"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System.Net;

public static HttpResponseMessage Run(HttpRequestMessage req, IEnumerable<ToDoItem> toDoItems, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    foreach (ToDoItem toDoItem in toDoItems)
    {
        log.Info(toDoItem.Description);
    }
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-get-multiple-docs-using-documentclient-c-script"></a>HTTP tetikleyicisi, DocumentClient (C# betik) kullanarak birden çok belgeleri edinin

Aşağıdaki örnekte gösterildiği bir [C# betik işlevi](functions-reference-csharp.md) , belgelerin listesini alır. İşlevi, bir HTTP isteği tarafından tetiklenir. Kod bir `DocumentClient` belgelerin listesini okumak için Azure Cosmos DB'ye bağlama tarafından sağlanan örneği. `DocumentClient` Örneği yazma işlemleri için de kullanılabilir.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "client",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "inout"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
#r "Microsoft.Azure.Documents.Client"

using System.Net;
using Microsoft.Azure.Documents.Client;
using Microsoft.Azure.Documents.Linq;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, DocumentClient client, TraceWriter log)
{
    log.Info("C# HTTP trigger function processed a request.");

    Uri collectionUri = UriFactory.CreateDocumentCollectionUri("ToDoItems", "Items");
    string searchterm = req.GetQueryNameValuePairs()
        .FirstOrDefault(q => string.Compare(q.Key, "searchterm", true) == 0)
        .Value;

    if (searchterm == null)
    {
        return req.CreateResponse(HttpStatusCode.NotFound);
    }

    log.Info($"Searching for word: {searchterm} using Uri: {collectionUri.ToString()}");
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
    return req.CreateResponse(HttpStatusCode.OK);
}
```

[Giriş örnekleri atla](#input---attributes)

### <a name="input---javascript-examples"></a>Giriş - JavaScript örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* [Kuyruk tetikleyicisi, JSON Kimliğinden Ara](#queue-trigger-look-up-id-from-json-javascript)
* [HTTP tetikleyicisi, Sorgu dizesinden Kimliği Ara](#http-trigger-look-up-id-from-query-string-javascript)
* [HTTP tetikleyicisi, rota verilerinden Kimliği Ara](#http-trigger-look-up-id-from-route-data-javascript)
* [Tetikleyici kuyruk, SqlQuery kullanarak birden çok belgeleri edinin](#queue-trigger-get-multiple-docs-using-sqlquery-javascript)

[Giriş örnekleri atla](#input---attributes)

#### <a name="queue-trigger-look-up-id-from-json-javascript"></a>Kuyruk tetikleyicisi, JSON (JavaScript) Kimliğinden Ara

Aşağıdaki örnek, bir Cosmos DB giriş bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

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

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-query-string-javascript"></a>HTTP tetikleyicisi, kimliği arama Sorgu dizesinden (JavaScript)

Aşağıdaki örnekte gösterildiği bir [JavaScript işlevi](functions-reference-node.md) , tek bir belge alır. İşlevi, aranacak kimliği belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{Query.id}"
    }
  ],
  "disabled": true
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Giriş örnekleri atla](#input---attributes)

#### <a name="http-trigger-look-up-id-from-route-data-javascript"></a>HTTP tetikleyicisi, kimliği bir ara rota verilerinden (JavaScript)

Aşağıdaki örnekte gösterildiği bir [JavaScript işlevi](functions-reference-node.md) , tek bir belge alır. İşlevi, aranacak kimliği belirtmek için bir sorgu dizesi kullanan bir HTTP isteği tarafından tetiklenir. Kimliği almak için kullanılan bir `ToDoItem` belge belirtilen veritabanı ve koleksiyonu.

İşte *function.json* dosyası:

```json
{
  "bindings": [
    {
      "authLevel": "anonymous",
      "name": "req",
      "type": "httpTrigger",
      "direction": "in",
      "methods": [
        "get",
        "post"
      ],
      "route":"todoitems/{id}"
    },
    {
      "name": "$return",
      "type": "http",
      "direction": "out"
    },
    {
      "type": "documentDB",
      "name": "toDoItem",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "in",
      "Id": "{id}"
    }
  ],
  "disabled": false
}
```

JavaScript kod aşağıdaki gibidir:

```javascript
module.exports = function (context, req, toDoItem) {
    context.log('JavaScript queue trigger function processed work item');
    if (!toDoItem)
    {
        context.log("ToDo item not found");
    }
    else
    {
        context.log("Found ToDo item, Description=" + toDoItem.Description);
    }

    context.done();
};
```

[Giriş örnekleri atla](#input---attributes)



#### <a name="queue-trigger-get-multiple-docs-using-sqlquery-javascript"></a>Tetikleyici kuyruk, SqlQuery (JavaScript) kullanarak birden çok belgeleri edinin

Aşağıdaki örnek, bir Azure Cosmos DB giriş bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, belirtilen sorgu parametrelerini özelleştirmek için bir kuyruk tetikleyicisi kullanarak bir SQL sorgusu tarafından birden çok belge alır.

Kuyruk tetikleyicisi parametre sağlar `departmentId`. Bir kuyruk iletisinin `{ "departmentId" : "Finance" }` Finans departmanı için tüm kayıtları döndürür.

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

[Giriş örnekleri atla](#input---attributes)

<a name="infsharp"></a>

### <a name="input---f-examples"></a>Giriş - F# örnekleri

Aşağıdaki örnek, bir Cosmos DB giriş bağlama gösterir. bir *function.json* dosyası ve bir [ F# işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlevi, tek bir belge okur ve belgenin metin değerini güncelleştirir.

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

İşte F# kod:

```fsharp
    (* Change input document contents using Azure Cosmos DB input binding *)
    open FSharp.Interop.Dynamic
    let Run(myQueueItem: string, inputDocument: obj) =
    inputDocument?text <- "This has changed."
```

Bu örnekte gerektiren bir `project.json` belirten dosyası `FSharp.Interop.Dynamic` ve `Dynamitey` NuGet bağımlılıklarını:

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

Eklemek için bir `project.json` bkz [ F# paket Yönetimi](functions-reference-fsharp.md#package).

## <a name="input---attributes"></a>Giriş - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) özniteliği.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikleri hakkında daha fazla bilgi için bkz. [aşağıdaki yapılandırma bölümüne](#input---configuration).

## <a name="input---configuration"></a>Giriş - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `DocumentDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     || Ayarlanmalıdır `documentdb`.        |
|**direction**     || Ayarlanmalıdır `in`.         |
|**name**     || İşlevinde belgeyi temsil eden bağlama parametresinin adı.  |
|**databaseName** |**databaseName** |Belge içeren veritabanı.        |
|**collectionName** |**collectionName** | Belgeyi içeren koleksiyon adı. |
|**id**    | **Kimlik** | Alınacak belgenin kimliği. Bu özelliği destekleyen [ifadeleri bağlama](./functions-bindings-expressions-patterns.md). Her ikisi de ayarlamamanız **kimliği** ve **sqlQuery** özellikleri. Tek ayarlamazsanız, tüm koleksiyon alınır. |
|**sqlQuery**  |**SqlQuery**  | Birden çok belge almak için kullanılan bir Azure Cosmos DB SQL sorgusu. Bu örnekte olduğu gibi çalışma zamanı bağlamaları özelliği destekler: `SELECT * FROM c where c.departmentId = {departmentId}`. Her ikisi de ayarlamamanız **kimliği** ve **sqlQuery** özellikleri. Tek ayarlamazsanız, tüm koleksiyon alınır.|
|**bağlantı**     |**connectionStringSetting**|Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |
|**partitionKey**|**partitionKey**|Arama için bölüm anahtarı değeri belirtir. Bağlama parametrelerinde içerebilir.|

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="input---usage"></a>Giriş - kullanım

İçinde C# ve F# İşlevler, işlev giriş belgesi adlandırılmış giriş aracılığıyla yapılan tüm değişiklikler başarıyla çıktığında parametreleri otomatik olarak kalıcı olur.

JavaScript işlevleri'nde güncelleştirmeleri otomatik olarak işlevi çıkıştan sonra duruma getirilmez. Bunun yerine, `context.bindings.<documentName>In` ve `context.bindings.<documentName>Out` güncelleştirmeleri yapmak. Bkz: [JavaScript örnek](#input---javascript-examples).

## <a name="output"></a>Çıktı

Azure Cosmos DB çıkış sağlar bağlaması SQL API'sini kullanarak bir Azure Cosmos DB veritabanına yeni bir belge yazma.

## <a name="output---examples"></a>Çıkış - örnekler

Dile özgü örneklere bakın:

* [C#](#output---c-examples)
* [C# betiği (.csx)](#output---c-script-examples)
* [JavaScript](#output---javascript-examples)
* [F#](#output---f-examples)

Ayrıca bkz: [giriş örnek](#input---c-examples) kullanan `DocumentClient`.

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---c-examples"></a>Çıkış - C# örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* Kuyruk tetikleyicisi, bir belge yazma
* Kuyruk tetikleyicisi yazma docs IAsyncCollector kullanma

Örnekler için basit bir başvuru `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

[Çıkışı örnekleri atla](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c"></a>Kuyruk tetikleyicisi, bir belge yazma (C#)

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) kuyruk depolama iletisinden sağlanan verileri kullanarak bir veritabanına bir belge ekleyen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System;

namespace CosmosDBSamplesV1
{
    public static class WriteOneDoc
    {
        [FunctionName("WriteOneDoc")]
        public static void Run(
            [QueueTrigger("todoqueueforwrite")] string queueMessage,
            [DocumentDB(
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

#### <a name="queue-trigger-write-docs-using-iasynccollector-c"></a>Kuyruk tetikleyicisi yazma docs IAsyncCollector (C#) kullanma

Aşağıdaki örnekte gösterildiği bir [C# işlevi](functions-dotnet-class-library.md) bir kuyruk iletisi JSON sağlanan verileri kullanarak bir veritabanına bir belge koleksiyonu ekleyen.

```cs
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Host;
using System.Threading.Tasks;

namespace CosmosDBSamplesV1
{
    public static class WriteDocsIAsyncCollector
    {
        [FunctionName("WriteDocsIAsyncCollector")]
        public static async Task Run(
            [QueueTrigger("todoqueueforwritemulti")] ToDoItem[] toDoItemsIn,
            [DocumentDB(
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

### <a name="output---c-script-examples"></a>Çıkış - C# betik örnekleri

Bu bölüm aşağıdaki örnekleri içerir:

* Kuyruk tetikleyicisi, bir belge yazma
* Kuyruk tetikleyicisi yazma docs IAsyncCollector kullanma

[Çıkışı örnekleri atla](#output---attributes)

#### <a name="queue-trigger-write-one-doc-c-script"></a>Kuyruk tetikleyicisi, bir belge yazma (C# betik)

Aşağıdaki örnek, bir Azure Cosmos DB çıktı bağlama gösterir. bir *function.json* dosyası ve bir [C# betik işlevi](functions-reference-csharp.md) bağlama kullanan. İşlevi, JSON alan şu biçimde bir kuyruk için bir kuyruk giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi Azure Cosmos DB belgeleri her kayıt için aşağıdaki biçimde oluşturur:

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

C# betik kodunu şu şekildedir:

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

#### <a name="queue-trigger-write-docs-using-iasynccollector"></a>Kuyruk tetikleyicisi yazma docs IAsyncCollector kullanma

Birden çok belge oluşturmak için adlarınıza bağlayabileceğiniz `ICollector<T>` veya `IAsyncCollector<T>` burada `T` desteklenen türlerden biridir.

Bu örnek için basit bir ifade eder `ToDoItem` türü:

```cs
namespace CosmosDBSamplesV1
{
    public class ToDoItem
    {
        public string Id { get; set; }
        public string Description { get; set; }
    }
}
```

Function.json dosyası aşağıda verilmiştir:

```json
{
  "bindings": [
    {
      "name": "toDoItemsIn",
      "type": "queueTrigger",
      "direction": "in",
      "queueName": "todoqueueforwritemulti",
      "connection": "AzureWebJobsStorage"
    },
    {
      "type": "documentDB",
      "name": "toDoItemsOut",
      "databaseName": "ToDoItems",
      "collectionName": "Items",
      "connection": "CosmosDBConnection",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

C# betik kodunu şu şekildedir:

```cs
using System;

public static async Task Run(ToDoItem[] toDoItemsIn, IAsyncCollector<ToDoItem> toDoItemsOut, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed {toDoItemsIn?.Length} items");

    foreach (ToDoItem toDoItem in toDoItemsIn)
    {
        log.Info($"Description={toDoItem.Description}");
        await toDoItemsOut.AddAsync(toDoItem);
    }
}
```

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---javascript-examples"></a>Çıkış - JavaScript örnekleri

Aşağıdaki örnek, bir Azure Cosmos DB çıktı bağlama gösterir. bir *function.json* dosyası ve bir [JavaScript işlevi](functions-reference-node.md) bağlama kullanan. İşlevi, JSON alan şu biçimde bir kuyruk için bir kuyruk giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi Azure Cosmos DB belgeleri her kayıt için aşağıdaki biçimde oluşturur:

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

[Çıkışı örnekleri atla](#output---attributes)

### <a name="output---f-examples"></a>Çıkış - F# örnekleri

Aşağıdaki örnek, bir Azure Cosmos DB çıktı bağlama gösterir. bir *function.json* dosyası ve bir [ F# işlevi](functions-reference-fsharp.md) bağlama kullanan. İşlevi, JSON alan şu biçimde bir kuyruk için bir kuyruk giriş bağlama kullanır:

```json
{
    "name": "John Henry",
    "employeeId": "123456",
    "address": "A town nearby"
}
```

İşlevi Azure Cosmos DB belgeleri her kayıt için aşağıdaki biçimde oluşturur:

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

İşte F# kod:

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

Bu örnekte gerektiren bir `project.json` belirten dosyası `FSharp.Interop.Dynamic` ve `Dynamitey` NuGet bağımlılıklarını:

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

Eklemek için bir `project.json` bkz [ F# paket Yönetimi](functions-reference-fsharp.md#package).

## <a name="output---attributes"></a>Çıkış - öznitelikleri

İçinde [C# sınıfı kitaplıklar](functions-dotnet-class-library.md), kullanın [DocumentDB](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/v2.x/src/WebJobs.Extensions.DocumentDB/DocumentDBAttribute.cs) özniteliği.

Özniteliğin oluşturucusu, koleksiyon adı ve veritabanı adını alır. Bu ayarlar ve yapılandırabileceğiniz diğer özellikleri hakkında daha fazla bilgi için bkz. [çıkışı - yapılandırma](#output---configuration). İşte bir `DocumentDB` özniteliği örnek bir yöntem imzası:

```csharp
    [FunctionName("QueueToDocDB")]
    public static void Run(
        [QueueTrigger("myqueue-items", Connection = "AzureWebJobsStorage")] string myQueueItem,
        [DocumentDB("ToDoList", "Items", Id = "id", ConnectionStringSetting = "myCosmosDB")] out dynamic document)
    {
        ...
    }
```

Tam bir örnek için bkz. [çıkış - C# örneği](#output---c-examples).

## <a name="output---configuration"></a>Çıkış - yapılandırma

Aşağıdaki tabloda ayarladığınız bağlama yapılandırma özelliklerini açıklayan *function.json* dosya ve `DocumentDB` özniteliği.

|Function.JSON özelliği | Öznitelik özelliği |Açıklama|
|---------|---------|----------------------|
|**type**     || Ayarlanmalıdır `documentdb`.        |
|**direction**     || Ayarlanmalıdır `out`.         |
|**name**     || İşlevinde belgeyi temsil eden bağlama parametresinin adı.  |
|**databaseName** | **databaseName**|Belge oluşturulduğu koleksiyonu içeren veritabanı.     |
|**collectionName** |**collectionName**  | Belge oluşturulduğu koleksiyonun adı. |
|**createıfnotexists**  |**Createıfnotexists**    | Mevcut değilse, koleksiyonun oluşturulup oluşturulmayacağını belirten bir Boole değeri. Varsayılan değer *false* etkileri maliyet ayrılmış işleme ile yeni Koleksiyonlar oluşturulduğundan. Daha fazla bilgi edinmek için bkz. [fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/documentdb/).  |
|**partitionKey**|**partitionKey** |Zaman `CreateIfNotExists` true ise, oluşturulan koleksiyon için bölüm anahtarı yolunu tanımlar.|
|**collectionThroughput**|**collectionThroughput**| Zaman `CreateIfNotExists` true ise, tanımlar [aktarım hızı](../cosmos-db/set-throughput.md) oluşturulan koleksiyon.|
|**bağlantı**    |**connectionStringSetting** |Azure Cosmos DB bağlantı dizenizi içeren uygulama ayarının adı.        |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="output---usage"></a>Çıkış - kullanım

Çıkış parametresi işlevinizde yazdığınızda varsayılan olarak, veritabanınızdaki bir belge oluşturulur. Bu belge otomatik olarak oluşturulan bir GUID belge kimliğinin sahiptir. Çıkış belgesinin Belge Kimliğini belirterek belirtebilirsiniz `id` JSON nesnesi özelliği için çıkış parametresi geçirildi.

> [!Note]
> Var olan bir belgeyi kimliği belirttiğinizde, yeni çıkış belgesi tarafından üzerine.

## <a name="exceptions-and-return-codes"></a>Özel durumlar ve dönüş kodları

| Bağlama | Başvuru |
|---|---|
| CosmosDB | [CosmosDB hata kodları](https://docs.microsoft.com/rest/api/cosmos-db/http-status-codes-for-cosmosdb) |

## <a name="next-steps"></a>Sonraki adımlar

* [Bilgi işlem Cosmos DB ile sunucusuz veritabanı hakkında daha fazla bilgi edinin](../cosmos-db/serverless-computing-database.md)
* [Azure işlevleri Tetikleyicileri ve bağlamaları hakkında daha fazla bilgi edinin](functions-triggers-bindings.md)

<!---
> [!div class="nextstepaction"]
> [Go to a quickstart that uses a Cosmos DB trigger](functions-create-cosmos-db-triggered-function.md)
--->
