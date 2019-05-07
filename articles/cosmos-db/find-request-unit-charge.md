---
title: İstek birimi (RU) ücretsiz olarak Azure Cosmos DB'de bulun.
description: İstek birimi (RU) ücretsiz bir Azure Cosmos kapsayıcı karşı yürütülen herhangi bir işlem bulma konusunda bilgi edinin.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: thweiss
ms.openlocfilehash: 73eaef1c9c8a9359ab931dbbe50496dc41a6f337
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65148969"
---
# <a name="find-the-request-unit-charge-in-azure-cosmos-db"></a>Azure Cosmos DB'de istek birimi ücretine ek olarak Bul

Bu makalede bulabilirsiniz farklı yollar sunar [istek birimi](request-units.md) (RU) tüketim için Azure Cosmos DB kapsayıcısında karşı yürütülen herhangi bir işlem. Şu anda yalnızca Azure portalında veya Azure Cosmos DB Sdk'lardan birini aracılığıyla geri gönderilen yanıt inceleyerek bu tüketim ölçebilirsiniz.

## <a name="sql-core-api"></a>SQL API (çekirdek)

SQL API kullanıyorsanız, bir Azure Cosmos kapsayıcısına yönelik bir işlem için RU kullanımını bulmak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda yalnızca bir SQL sorgusu için istek ücreti Azure portalında bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-sql-api-dotnet.md#create-account) ve verilerle akışı veya zaten veri içeriyor. mevcut bir Azure Cosmos hesabını seçin.

1. Git **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Seçin **yeni SQL sorgusu**.

1. Geçerli bir sorgu girin ve ardından **sorguyu Yürüt**.

1. Seçin **sorgu istatistikleri** gerçek istek yükü, yürütülen istek için görüntülenecek.

![Azure portalında bir SQL sorgu isteği ücret ekran görüntüsü](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk-v2"></a>.NET SDK'sı V2 kullanın

Öğesinden döndürülen nesneleri [.NET SDK'sı v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) kullanıma bir `RequestCharge` özelliği:

```csharp
ResourceResponse<Document> fetchDocumentResponse = await client.ReadDocumentAsync(
    UriFactory.CreateDocumentUri("database", "container", "itemId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
var requestCharge = fetchDocumentResponse.RequestCharge;

StoredProcedureResponse<string> storedProcedureCallResponse = await client.ExecuteStoredProcedureAsync<string>(
    UriFactory.CreateStoredProcedureUri("database", "container", "storedProcedureId"),
    new RequestOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    });
requestCharge = storedProcedureCallResponse.RequestCharge;

IDocumentQuery<dynamic> query = client.CreateDocumentQuery(
    UriFactory.CreateDocumentCollectionUri("database", "container"),
    "SELECT * FROM c",
    new FeedOptions
    {
        PartitionKey = new PartitionKey("partitionKey")
    }).AsDocumentQuery();
while (query.HasMoreResults)
{
    FeedResponse<dynamic> queryResponse = await query.ExecuteNextAsync<dynamic>();
    requestCharge = queryResponse.RequestCharge;
}
```

Daha fazla bilgi için [hızlı başlangıç: Azure Cosmos DB SQL API hesabı kullanarak bir .NET web uygulaması derleme](create-sql-api-dotnet.md).

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Öğesinden döndürülen nesneleri [Java SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) kullanıma bir `getRequestCharge()` yöntemi:

```java
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<ResourceResponse<Document>> readDocumentResponse = client.readDocument(String.format("/dbs/%s/colls/%s/docs/%s", "database", "container", "itemId"), requestOptions);
readDocumentResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

Observable<StoredProcedureResponse> storedProcedureResponse = client.executeStoredProcedure(String.format("/dbs/%s/colls/%s/sprocs/%s", "database", "container", "storedProcedureId"), requestOptions, null);
storedProcedureResponse.subscribe(result -> {
    double requestCharge = result.getRequestCharge();
});

FeedOptions feedOptions = new FeedOptions();
feedOptions.setPartitionKey(new PartitionKey("partitionKey"));

Observable<FeedResponse<Document>> feedResponse = client
    .queryDocuments(String.format("/dbs/%s/colls/%s", "database", "container"), "SELECT * FROM c", feedOptions);
feedResponse.forEach(result -> {
    double requestCharge = result.getRequestCharge();
});
```

Daha fazla bilgi için [hızlı başlangıç: Bir Azure Cosmos DB SQL API hesabı kullanarak bir Java uygulaması derlemek](create-sql-api-java.md).

### <a name="use-the-nodejs-sdk"></a>Node.js SDK'sını kullanma

Öğesinden döndürülen nesneleri [Node.js SDK'sı](https://www.npmjs.com/package/@azure/cosmos) kullanıma bir `headers` temel alınan HTTP API'si tarafından döndürülen tüm üstbilgileri eşlenen alt nesne. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı:

```javascript
const item = await client
    .database('database')
    .container('container')
    .item('itemId', 'partitionKey')
    .read();
var requestCharge = item.headers['x-ms-request-charge'];

const storedProcedureResult = await client
    .database('database')
    .container('container')
    .storedProcedure('storedProcedureId')
    .execute({
        partitionKey: 'partitionKey'
    });
requestCharge = storedProcedureResult.headers['x-ms-request-charge'];

const query = client.database('database')
    .container('container')
    .items
    .query('SELECT * FROM c', {
        partitionKey: 'partitionKey'
    });
while (query.hasMoreResults()) {
    var result = await query.executeNext();
    requestCharge = result.headers['x-ms-request-charge'];
}
```

Daha fazla bilgi için [hızlı başlangıç: Bir Azure Cosmos DB SQL API hesabı kullanarak bir Node.js uygulaması derleme](create-sql-api-nodejs.md). 

### <a name="use-the-python-sdk"></a>Python SDK'sını kullanma

`CosmosClient` Nesnesinden [Python SDK'sı](https://pypi.org/project/azure-cosmos/) sunan bir `last_response_headers` son yürütülen işlemi için temel alınan HTTP API'si tarafından döndürülen tüm üst bilgiler eşleyen sözlük. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı:

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

Daha fazla bilgi için [hızlı başlangıç: Bir Azure Cosmos DB SQL API hesabı kullanarak bir Python uygulaması derleme](create-sql-api-python.md). 

## <a name="azure-cosmos-db-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API

Özel değere göre RU ücretsiz olarak sunulur [veritabanı komut](https://docs.mongodb.com/manual/reference/command/) adlı `getLastRequestStatistics`. Komut yürütülen son işlem adı, kendi istek yükü ve süresinin içeren bir belgeyi döndürür. MongoDB için Azure Cosmos DB API kullanırsanız, RU ücret almak için birden çok seçeneğiniz vardır.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Şu anda yalnızca bir sorgu için istek ücreti Azure portalında bulabilirsiniz.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos hesabı oluşturma](create-mongodb-dotnet.md#create-a-database-account) ve verilerle akışı veya zaten veri içeriyor. mevcut bir hesabı seçin.

1. Git **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz koleksiyonu seçin.

1. **Yeni Sorgu**'yu seçin.

1. Geçerli bir sorgu girin ve ardından **sorguyu Yürüt**.

1. Seçin **sorgu istatistikleri** gerçek istek yükü, yürütülen istek için görüntülenecek.

![Azure portalında bir MongoDB sorgu isteği ücret ekran görüntüsü](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>MongoDB .NET sürücüsü kullanın

Kullanırken [resmi MongoDB .NET sürücüsü](https://docs.mongodb.com/ecosystem/drivers/csharp/), çağırarak komutları yürütebilir `RunCommand` metodunda bir `IMongoDatabase` nesne. Bu yöntem uygulaması gerektirir `Command<>` soyut sınıf:

```csharp
class GetLastRequestStatisticsCommand : Command<Dictionary<string, object>>
{
    public override RenderedCommand<Dictionary<string, object>> Render(IBsonSerializerRegistry serializerRegistry)
    {
        return new RenderedCommand<Dictionary<string, object>>(new BsonDocument("getLastRequestStatistics", 1), serializerRegistry.GetSerializer<Dictionary<string, object>>());
    }
}

Dictionary<string, object> stats = database.RunCommand(new GetLastRequestStatisticsCommand());
double requestCharge = (double)stats["RequestCharge"];
```

Daha fazla bilgi için [hızlı başlangıç: MongoDB için bir Azure Cosmos DB API kullanarak bir .NET web uygulaması derleme](create-mongodb-dotnet.md).

### <a name="use-the-mongodb-java-driver"></a>MongoDB Java sürücüsünü kullanın


Kullanırken [resmi MongoDB Java sürücüsünü](http://mongodb.github.io/mongo-java-driver/), çağırarak komutları yürütebilir `runCommand` metodunda bir `MongoDatabase` nesnesi:

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

Daha fazla bilgi için [hızlı başlangıç: MongoDB ve Java SDK'sı için Azure Cosmos DB API kullanarak bir web uygulaması derleme](create-mongodb-java.md).

### <a name="use-the-mongodb-nodejs-driver"></a>MongoDB Node.js sürücüsü kullanın

Kullanırken [resmi MongoDB Node.js sürücüsü](https://mongodb.github.io/node-mongodb-native/), çağırarak komutları yürütebilir `command` metodunda bir `db` nesnesi:

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

Daha fazla bilgi için [hızlı başlangıç: Mevcut bir MongoDB Node.js web uygulamasını Azure Cosmos DB'ye geçirilebilir](create-mongodb-nodejs.md).

## <a name="cassandra-api"></a>Cassandra API’si

Azure Cosmos DB Cassandra API'sine karşı işlemler gerçekleştirdiğinizde, RU ücret adlı bir alan gelen yük döndürülen `RequestCharge`. RU ücret almak için birçok seçeneğiniz vardır.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

Kullanırken [.NET SDK'sı](https://www.nuget.org/packages/CassandraCSharpDriver/), gelen yükü altında alabilirsiniz `Info` özelliği bir `RowSet` nesnesi:

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"], 0);
```

Daha fazla bilgi için [hızlı başlangıç: .NET SDK'sını ve Azure Cosmos DB kullanarak bir Cassandra uygulaması derleme](create-cassandra-dotnet.md).

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Kullanırken [Java SDK'sı](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core), gelen yükü çağırarak alabilirsiniz `getExecutionInfo()` metodunda bir `ResultSet` nesnesi:

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

Daha fazla bilgi için [hızlı başlangıç: Azure Cosmos DB ve Java SDK'sını kullanarak bir Cassandra uygulaması derleme](create-cassandra-java.md).

## <a name="gremlin-api"></a>Gremlin API

Gremlin API kullandığınızda, bir Azure Cosmos kapsayıcısına yönelik bir işlem için RU kullanımını bulmak için birçok seçeneğiniz vardır. 

### <a name="use-drivers-and-sdk"></a>Kullanım sürücüleri ve SDK'sı

Gremlin API'si tarafından döndürülen üst bilgiler, şu anda Gremlin .NET ve Java SDK'sı tarafından ortaya çıkmış özel durumu öznitelikleri eşlenir. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

Kullanırken [Gremlin.NET SDK](https://www.nuget.org/packages/Gremlin.Net/), durum öznitelikleri altında kullanılabilir `StatusAttributes` özelliği `ResultSet<>` nesnesi:

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

Daha fazla bilgi için [hızlı başlangıç: Azure Cosmos DB Gremlin API hesabı kullanarak bir .NET Framework veya Core uygulaması derleme](create-graph-dotnet.md).

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Kullanırken [Gremlin Java SDK'sı](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver), durum öznitelikleri çağırarak alabilirsiniz `statusAttributes()` metodunda `ResultSet` nesnesi:

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

Daha fazla bilgi için [hızlı başlangıç: Java SDK'sını kullanarak Azure Cosmos DB'de bir grafik veritabanı oluşturma](create-graph-java.md).

## <a name="table-api"></a>Tablo API’si

Tablo işlemleri RU ücreti döndüren yalnızca SDK'sı şu anda olduğu [.NET standart SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table). `TableResult` Nesne kullanıma sunan bir `RequestCharge` özelliği, Azure Cosmos DB tablo API'sine karşı kullandığınızda SDK tarafından doldurulur:

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

Daha fazla bilgi için [hızlı başlangıç: .NET SDK'sını ve Azure Cosmos DB kullanarak bir tablo API'si uygulaması derleme](create-table-dotnet.md).

## <a name="next-steps"></a>Sonraki adımlar

RU kullanımını en iyi duruma getirme hakkında bilgi edinmek için şu makalelere bakın:

* [Azure Cosmos DB'deki istek birimleri ve aktarım hızı](request-units.md)
* [Azure Cosmos DB'de sağlanan aktarım hızı maliyeti iyileştirin](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu gerçekleştirerek](optimize-cost-queries.md)
* [Genel olarak sağlanan aktarım hızını ölçeklendirme](scaling-throughput.md)
* [Kapsayıcılar ve veritabanları sağlama aktarım hızı](set-throughput.md)
* [Bir kapsayıcının sağlama aktarım hızı](how-to-provision-container-throughput.md)
