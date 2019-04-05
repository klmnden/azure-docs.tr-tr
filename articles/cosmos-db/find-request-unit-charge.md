---
title: İstek birimi (RU) ücretsiz olarak Azure Cosmos DB'de bulun.
description: Bir Azure Cosmos kapsayıcı karşı yürütülen herhangi bir işlemi için istek birimi ücreti bulmayı öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 03/21/2019
ms.author: thweiss
ms.openlocfilehash: e3175ee136057c695ceef3cd1976b447a529c803
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59053170"
---
# <a name="find-the-request-unit-ru-charge-in-azure-cosmos-db"></a>İstek birimi (RU) ücretsiz olarak Azure Cosmos DB'de bulun.

Bu makalede bulmak için farklı yollar sunar [istek birimi](request-units.md) tüketimi için bir Azure Cosmos kapsayıcı karşı yürütülen herhangi bir işlem. Bu, şu anda Azure portalını kullanarak ya da Azure Cosmos DB Sdk'lardan birini aracılığıyla geri gönderilen yanıt inceleyerek bu tüketim ölçmek mümkündür.

## <a name="core-api"></a>Core API'si

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalı şu anda yalnızca bir SQL sorgusu için istek yükü bulmanıza olanak tanır.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos DB hesabı oluşturmayı](create-sql-api-dotnet.md#create-account) ve verilerle akışı veya zaten veri içeriyor. mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz kapsayıcıyı seçin.

1. Tıklayarak **yeni SQL sorgusu**.

1. Geçerli bir sorgu girin ardından tıklayarak **sorguyu Yürüt**.

1. Tıklayarak **sorgu istatistikleri** yalnızca yürütülen istek asıl isteğinin ücreti görüntülenecek.

![Azure portalı ekran görüntüsü, SQL sorgu isteği ücreti](./media/find-request-unit-charge/portal-sql-query.png)

### <a name="use-the-net-sdk-v2"></a>.NET SDK'sı V2 kullanın

Öğesinden döndürülen nesneleri [.NET SDK'sı v2](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/) (bkz [Bu hızlı başlangıçta](create-sql-api-dotnet.md) kullanımı ile ilgili) kullanıma bir `RequestCharge` özelliği.

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

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Öğesinden döndürülen nesneleri [Java SDK'sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) (bkz [Bu hızlı başlangıçta](create-sql-api-java.md) kullanımı ile ilgili) kullanıma bir `getRequestCharge()` yöntemi.

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

### <a name="use-the-nodejs-sdk"></a>Node.js SDK'sını kullanma

Öğesinden döndürülen nesneleri [Node.js SDK'sı](https://www.npmjs.com/package/@azure/cosmos) (bkz [Bu hızlı başlangıçta](create-sql-api-nodejs.md) kullanımı ile ilgili) kullanıma bir `headers` temel alınan HTTP API'si tarafından döndürülen tüm üstbilgileri eşlenen alt nesne. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı.

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

### <a name="use-the-python-sdk"></a>Python SDK'sını kullanma

`CosmosClient` Nesnesinden [Python SDK'sı](https://pypi.org/project/azure-cosmos/) (bkz [Bu hızlı başlangıçta](create-sql-api-python.md) kullanımı ile ilgili) sunan bir `last_response_headers` temel alınan HTTP API'si tarafından döndürülen tüm üstbilgileri eşleyen sözlük yürütülen son işlem. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı.

```python
response = client.ReadItem('dbs/database/colls/container/docs/itemId', { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']

response = client.ExecuteStoredProcedure('dbs/database/colls/container/sprocs/storedProcedureId', None, { 'partitionKey': 'partitionKey' })
request_charge = client.last_response_headers['x-ms-request-charge']
```

## <a name="azure-cosmos-dbs-api-for-mongodb"></a>MongoDB için Azure Cosmos DB API'si

İstek birimi ücretine ek olarak özel kullanıma sunulan [veritabanı komut](https://docs.mongodb.com/manual/reference/command/) adlı `getLastRequestStatistics`. Bu komut, son yürütülen işlemi adını, kendi istek yükü ve süresinin içeren bir belgeyi döndürür.

### <a name="use-the-azure-portal"></a>Azure portalı kullanma

Azure portalında şu anda yalnızca bir sorgu için istek yükü bulmanıza olanak tanır.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. [Yeni bir Azure Cosmos DB hesabı oluşturmayı](create-mongodb-dotnet.md#create-a-database-account) ve verilerle akışı veya zaten veri içeriyor. mevcut bir hesabı seçin.

1. Açık **Veri Gezgini** bölmesi ve üzerinde çalışmak istediğiniz koleksiyonu seçin.

1. Tıklayarak **yeni sorgu**.

1. Geçerli bir sorgu girin ardından tıklayarak **sorguyu Yürüt**.

1. Tıklayarak **sorgu istatistikleri** yalnızca yürütülen istek asıl isteğinin ücreti görüntülenecek.

![Azure portalı ekran görüntüsü, MongoDB sorgu isteği ücreti](./media/find-request-unit-charge/portal-mongodb-query.png)

### <a name="use-the-mongodb-net-driver"></a>MongoDB .NET sürücüsü kullanın

Kullanırken [resmi MongoDB .NET sürücüsü](https://docs.mongodb.com/ecosystem/drivers/csharp/) (bkz [Bu hızlı başlangıçta](create-mongodb-dotnet.md) kullanımı ile ilgili), komutları yürütülebilir çağırarak `RunCommand` metodunda bir `IMongoDatabase` nesne. Bu yöntem uygulaması gerektirir `Command<>` soyut sınıf.

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

### <a name="use-the-mongodb-java-driver"></a>MongoDB Java sürücüsünü kullanın

Kullanırken [resmi MongoDB Java sürücüsünü](http://mongodb.github.io/mongo-java-driver/) (bkz [Bu hızlı başlangıçta](create-mongodb-java.md) kullanımı ile ilgili), komutları yürütülebilir çağırarak `runCommand` metodunda bir `MongoDatabase` nesne.

```java
Document stats = database.runCommand(new Document("getLastRequestStatistics", 1));
Double requestCharge = stats.getDouble("RequestCharge");
```

### <a name="use-the-mongodb-nodejs-driver"></a>MongoDB Node.js sürücüsü kullanın

Kullanırken [resmi MongoDB Node.js sürücüsü](https://mongodb.github.io/node-mongodb-native/) (bkz [Bu hızlı başlangıçta](create-mongodb-nodejs.md) kullanımı ile ilgili), komutları yürütülebilir çağırarak `command` metodunda bir `Db` nesne.

```javascript
db.command({ getLastRequestStatistics: 1 }, function(err, result) {
    assert.equal(err, null);
    const requestCharge = result['RequestCharge'];
});
```

## <a name="cassandra-api"></a>Cassandra API’si

Azure Cosmos DB'nin Cassandra API'si işlemleri gerçekleştirilirken istek birimi ücreti adlı bir alan gelen yük döndürülen `RequestCharge`.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

Kullanırken [.NET SDK'sı](https://www.nuget.org/packages/CassandraCSharpDriver/) (bkz [Bu hızlı başlangıçta](create-cassandra-dotnet.md) kullanımı ile ilgili), gelen yükü altında alınabilir `Info` özelliği bir `RowSet` nesne.

```csharp
RowSet rowSet = session.Execute("SELECT table_name FROM system_schema.tables;");
double requestCharge = BitConverter.ToDouble(rowSet.Info.IncomingPayload["RequestCharge"], 0);
```

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Kullanırken [Java SDK'sı](https://mvnrepository.com/artifact/com.datastax.cassandra/cassandra-driver-core) (bkz [Bu hızlı başlangıçta](create-cassandra-java.md) kullanımı ile ilgili), gelen yükü çağırarak alınabilir `getExecutionInfo()` metodunda bir `ResultSet` nesne.

```java
ResultSet resultSet = session.execute("SELECT table_name FROM system_schema.tables;");
Double requestCharge = resultSet.getExecutionInfo().getIncomingPayload().get("RequestCharge").getDouble();
```

## <a name="gremlin-api"></a>Gremlin API

### <a name="use-drivers-and-sdk"></a>Kullanım sürücüleri ve SDK'sı

Gremlin API'si tarafından döndürülen üst bilgiler ve Gremlin .NET ve Java SDK'sı tarafından şu anda öne çıkarılır özel durumu öznitelikler eşlenir. İstek yükü altında kullanılabilir `x-ms-request-charge` anahtarı.

### <a name="use-the-net-sdk"></a>.NET SDK’yı kullanma

Kullanırken [Gremlin.NET SDK](https://www.nuget.org/packages/Gremlin.Net/) (bkz [Bu hızlı başlangıçta](create-graph-dotnet.md) kullanımı ile ilgili), durum öznitelikleri altında kullanılabilir `StatusAttributes` özelliği `ResultSet<>` nesne.

```csharp
ResultSet<dynamic> results = client.SubmitAsync<dynamic>("g.V().count()").Result;
double requestCharge = (double)results.StatusAttributes["x-ms-request-charge"];
```

### <a name="use-the-java-sdk"></a>Java kullanma SDK'sı

Kullanırken [Gremlin Java SDK'sı](https://mvnrepository.com/artifact/org.apache.tinkerpop/gremlin-driver) (bkz [Bu hızlı başlangıçta](create-graph-java.md) kullanımı ile ilgili), durum öznitelikleri çağrılarak alınabilir `statusAttributes()` metodunda `ResultSet` nesne.

```java
ResultSet results = client.submit("g.V().count()");
Double requestCharge = (Double)results.statusAttributes().get().get("x-ms-request-charge");
```

## <a name="table-api"></a>Tablo API’si

Şu anda tablo işlemleri için istek birimi ücreti döndüren yalnızca SDK'sı [.NET standart SDK](https://www.nuget.org/packages/Microsoft.Azure.Cosmos.Table) (bkz [Bu hızlı başlangıçta](create-table-dotnet.md) kullanımı ile ilgili). `TableResult` Nesne kullanıma sunan bir `RequestCharge` Azure Cosmos DB'nin tablo API'si ile programlama kullanıldığında SDK'sı tarafından doldurulur özelliği.

```csharp
CloudTable tableReference = client.GetTableReference("table");
TableResult tableResult = tableReference.Execute(TableOperation.Insert(new DynamicTableEntity("partitionKey", "rowKey")));
if (tableResult.RequestCharge.HasValue) // would be false when using Azure Storage Tables
{
    double requestCharge = tableResult.RequestCharge.Value;
}
```

## <a name="next-steps"></a>Sonraki adımlar

İstek birimi tüketiminiz en iyi duruma getirme hakkında bilgi edinmek için aşağıdaki makalelere bakın:

* [Azure Cosmos DB'de sağlanan aktarım hızı maliyeti iyileştirin](optimize-cost-throughput.md)
* [Azure Cosmos DB'de sorgu gerçekleştirerek](optimize-cost-queries.md)