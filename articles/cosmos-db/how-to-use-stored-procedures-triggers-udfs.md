---
title: Saklı yordamlar, tetikleyiciler ve Azure Cosmos DB SDK'larını kullanarak kullanıcı tanımlı işlevler çağırma
description: Kaydolun ve saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri kullanarak Azure Cosmos DB SDK'ları çağırma hakkında bilgi edinin
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 12/08/2018
ms.author: mjbrown
ms.openlocfilehash: ac70e5f4a5ff144107a0e5080a7563128a6825f5
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925968"
---
# <a name="how-to-register-and-use-stored-procedures-triggers-and-user-defined-functions-in-azure-cosmos-db"></a>Kaydetme ve kullanma hakkında yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri Azure Cosmos DB'de depolanan

Azure Cosmos DB SQL API kaydetme ve saklı yordamlar, tetikleyiciler ve JavaScript dilinde yazılmış kullanıcı tanımlı işlevler (UDF'ler) çağırma destekler. SQL API'si kullanabileceğiniz [.NET](sql-api-sdk-dotnet.md), [.NET Core](sql-api-sdk-dotnet-core.md), [Java](sql-api-sdk-java.md), [JavaScript](sql-api-sdk-node.md), [Node.js](sql-api-sdk-node.md), veya [Python](sql-api-sdk-python.md) kaydetmek ve saklı yordamları çağırmak için SDK'ları. Bir veya daha fazla saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri tanımladıktan sonra yükleme ve görüntülemeye [Azure portalında](https://portal.azure.com/) Veri Gezgini'ni kullanarak.

## <a id="stored-procedures"></a>Saklı yordamları çalıştırma

Saklı yordamlar, JavaScript kullanarak yazılır. Bunlar oluşturabilir, güncelleştirme, okuma, sorgu ve Azure Cosmos kapsayıcı içindeki öğeleri silin. Azure Cosmos DB'de depolanan yordamları yazma hakkında daha fazla bilgi için bkz. [saklı yordamlar, Azure Cosmos DB'de yazma](how-to-write-stored-procedures-triggers-udfs.md#stored-procedures) makalesi.

Aşağıdaki örnekler, kaydetme ve Azure Cosmos DB SDK'larını kullanarak bir saklı yordamı çağırma gösterilmektedir. Başvurmak [belge oluşturma](how-to-write-stored-procedures-triggers-udfs.md#create-an-item) Bu saklı yordam için kaynak olarak kaydedildiğinde `spCreateToDoItem.js`.

> [!NOTE]
> Bir saklı yordamı yürütülürken bölümlenmiş kapsayıcılar için bir bölüm anahtarı değeri istek seçenekleri sağlanmalıdır. Saklı yordamlar, her zaman bir bölüm anahtarı için kapsama eklenir. Farklı bir bölüm anahtarı değerine sahip öğeleri saklı yordam için görünür olmaz. Bu da Tetikleyiciler için de geçerli.

### <a name="stored-procedures---net-sdk"></a>Saklı yordamlar - .NET SDK'sı

Aşağıdaki örnek, .NET SDK'sını kullanarak bir saklı yordam kaydettirmek gösterilmektedir:

```csharp
string storedProcedureId = "spCreateToDoItem";
StoredProcedure newStoredProcedure = new StoredProcedure
   {
       Id = storedProcedureId,
       Body = File.ReadAllText($@"..\js\{storedProcedureId}.js")
   };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var response = await client.CreateStoredProcedureAsync(containerUri, newStoredProcedure);
StoredProcedure createdStoredProcedure = response.Resource;
```

Aşağıdaki kod, .NET SDK'sını kullanarak bir saklı yordamı çağırma gösterilmektedir:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

Uri uri = UriFactory.CreateStoredProcedureUri("myDatabase", "myContainer", "spCreateToDoItem");
RequestOptions options = new RequestOptions { PartitionKey = new PartitionKey("Personal") };
var result = await client.ExecuteStoredProcedureAsync<string>(uri, options, newItem);
var id = result.Response;
```

### <a name="stored-procedures---java-sdk"></a>Saklı yordamlar - Java SDK'sı

Aşağıdaki örnek, Java SDK'sını kullanarak bir saklı yordam kaydettirmek gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
StoredProcedure newStoredProcedure = new StoredProcedure(
    "{" +
        "  'id':'spCreateToDoItem'," +
        "  'body':" + new String(Files.readAllBytes(Paths.get("..\\js\\spCreateToDoItem.js"))) +
    "}");
//toBlocking() blocks the thread until the operation is complete and is used only for demo.  
StoredProcedure createdStoredProcedure = asyncClient.createStoredProcedure(containerLink, newStoredProcedure, null)
    .toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'sını kullanarak bir saklı yordamı çağırma gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String sprocLink = String.format("%s/sprocs/%s", containerLink, "spCreateToDoItem");
final CountDownLatch successfulCompletionLatch = new CountDownLatch(1);

class ToDoItem {
    public String category;
    public String name;
    public String description;
    public boolean isComplete;
}

ToDoItem newItem = new ToDoItem();
newItem.category = "Personal";
newItem.name = "Groceries";
newItem.description = "Pick up strawberries";
newItem.isComplete = false;

RequestOptions requestOptions = new RequestOptions();
requestOptions.setPartitionKey(new PartitionKey("Personal"));

Object[] storedProcedureArgs = new Object[] { newItem };
asyncClient.executeStoredProcedure(sprocLink, requestOptions, storedProcedureArgs)
    .subscribe(storedProcedureResponse -> {
        String storedProcResultAsString = storedProcedureResponse.getResponseAsString();
        successfulCompletionLatch.countDown();
        System.out.println(storedProcedureResponse.getActivityId());
    }, error -> {
        System.err.println("an error occurred while executing the stored procedure: actual cause: "
                + error.getMessage());
    });

successfulCompletionLatch.await();
```

### <a name="stored-procedures---javascript-sdk"></a>Saklı yordamlar - JavaScript SDK'sı

Aşağıdaki örnek, JavaScript SDK'sını kullanarak bir saklı yordam kaydettirmek gösterilmektedir

```javascript
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
await container.storedProcedures.create({
    id: sprocId,
    body: require(`../js/${sprocId}`)
});
```

Aşağıdaki kod, JavaScript SDK'sını kullanarak bir saklı yordamı çağırma gösterilmektedir:

```javascript
const newItem = [{
    category: "Personal",
    name: "Groceries",
    description: "Pick up strawberries",
    isComplete: false
}];
const container = client.database("myDatabase").container("myContainer");
const sprocId = "spCreateToDoItem";
const {body: result} = await container.storedProcedure(sprocId).execute(newItem, {partitionKey: newItem[0].category});
```

### <a name="stored-procedures---python-sdk"></a>Saklı yordamlar - Python SDK'sı

Aşağıdaki örnek Python SDK'sını kullanarak bir saklı yordam kaydettirmek gösterilmektedir

```python
with open('../js/spCreateToDoItem.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
sproc_definition = {
            'id': 'spCreateToDoItem',
            'serverScript': file_contents,
        }
sproc = client.CreateStoredProcedure(container_link, sproc_definition)
```

Aşağıdaki kod Python SDK'sını kullanarak bir saklı yordam çağırmak nasıl gösterir

```python
sproc_link = 'dbs/myDatabase/colls/myContainer/sprocs/spCreateToDoItem'
new_item = [{
    'category':'Personal',
    'name':'Groceries',
    'description':'Pick up strawberries',
    'isComplete': False
}]
client.ExecuteStoredProcedure(sproc_link, new_item, {'partitionKey': 'Personal'}
```

## <a id="pre-triggers"></a>Öncesi Tetikleyicileri çalıştırmayı öğrenin

Aşağıdaki örnekler, kaydetme ve Azure Cosmos DB SDK'ları kullanarak öncesi bir tetikleyici çağırmak nasıl gösterir. Başvurmak [öncesi tetikleyicisi örneğinde](how-to-write-stored-procedures-triggers-udfs.md#pre-triggers) bu ön tetikleyicisi için kaynak olarak kaydedildiğinde `trgPreValidateToDoItemTimestamp.js`.

Yürütülürken öncesi Tetikleyicileri RequestOptions nesneyi belirterek geçirilen `PreTriggerInclude` ve ardından bir liste nesnesinde tetikleyici adını geçirerek.

> [!NOTE]
> Tetikleyicinin adı bir liste olarak geçirilen olsa bile, işlem başına yalnızca bir tetikleyici yine de yürütebilir.

### <a name="pre-triggers---net-sdk"></a>Öncesi Tetikleyiciler-.NET SDK'sı

Aşağıdaki kod, .NET SDK kullanarak ön tetiği kaydetme gösterir:

```csharp
string triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger
{
    Id =  triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Pre
};
containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

Aşağıdaki kod, .NET SDK kullanarak öncesi bir tetikleyici çağrı gösterilmektedir:

```csharp
dynamic newItem = new
{
    category = "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
};

containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
RequestOptions requestOptions = new RequestOptions { PreTriggerInclude = new List<string> { "trgPreValidateToDoItemTimestamp" } };
await client.CreateDocumentAsync(containerUri, newItem, requestOptions);
```

### <a name="pre-triggers---java-sdk"></a>Öncesi Tetikleyiciler-Java SDK'sı

Aşağıdaki kod, Java SDK'sını kullanarak bir ön tetikleyici kaydettirmek gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPreValidateToDoItemTimestamp";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Pre);
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'sını kullanarak bir ön tetikleyici çağrılacak gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
    Document item = new Document("{ "
            + "\"category\": \"Personal\", "
            + "\"name\": \"Groceries\", "
            + "\"description\": \"Pick up strawberries\", "
            + "\"isComplete\": false, "
            + "}"
            );
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPreTriggerInclude(Arrays.asList("trgPreValidateToDoItemTimestamp"));
//toBlocking() blocks the thread until the operation is complete and is used only for demo. 
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="pre-triggers---javascript-sdk"></a>Öncesi Tetikleyiciler-JavaScript SDK'sı

Aşağıdaki kod, JavaScript SDK'sını kullanarak bir ön tetikleyici kaydettirmek gösterilmektedir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "pre"
});
```

Aşağıdaki kod, JavaScript SDK'sını kullanarak bir ön tetikleyici çağrılacak gösterilmektedir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPreValidateToDoItemTimestamp";
await container.items.create({
    category: "Personal",
    name = "Groceries",
    description = "Pick up strawberries",
    isComplete = false
}, {preTriggerInclude: [triggerId]});
```

### <a name="pre-triggers---python-sdk"></a>Öncesi Tetikleyiciler-Python SDK'sı

Aşağıdaki kod, Python SDK'sını kullanarak bir ön tetikleyici kaydettirmek gösterilmektedir:

```python
with open('../js/trgPreValidateToDoItemTimestamp.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
            'id': 'trgPreValidateToDoItemTimestamp',
            'serverScript': file_contents,
            'triggerType': documents.TriggerType.Pre,
            'triggerOperation': documents.TriggerOperation.Create
        }
trigger = client.CreateTrigger(container_link, trigger_definition)
```

Aşağıdaki kod nasıl çağrılacağını Python SDK'sını kullanarak bir ön tetikleyici gösterir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = { 'category': 'Personal', 'name': 'Groceries', 'description':'Pick up strawberries', 'isComplete': False}
client.CreateItem(container_link, item, { 'preTriggerInclude': 'trgPreValidateToDoItemTimestamp'})
```

## <a id="post-triggers"></a>Çalıştırma sonrası Tetikleyicileri

Aşağıdaki örnekler, Azure Cosmos DB SDK'ları kullanarak sonrası tetiği kaydetme işlemini göstermektedir. Başvurmak [sonrası tetikleyicisi örneğinde](how-to-write-stored-procedures-triggers-udfs.md#post-triggers) sonrası Bu tetikleyici için kaynak olarak kaydedildiğinde `trgPostUpdateMetadata.js`.

### <a name="post-triggers---net-sdk"></a>Sonrası Tetikleyicileri-.NET SDK'sı

Aşağıdaki kod, .NET SDK kullanarak sonrası tetiği kaydetme gösterir:

```csharp
string triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger
{
    Id = triggerId,
    Body = File.ReadAllText($@"..\js\{triggerId}.js"),
    TriggerOperation = TriggerOperation.Create,
    TriggerType = TriggerType.Post
};
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateTriggerAsync(containerUri, trigger);
```

Aşağıdaki kod, .NET SDK kullanarak sonrası bir tetikleyici çağrılacak gösterilmektedir:

```csharp
var newItem = { 
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};

var options = { postTriggerInclude: "trgPostUpdateMetadata" };
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.createDocumentAsync(containerUri, newItem, options);
```

### <a name="post-triggers---java-sdk"></a>Sonrası Tetikleyicileri-Java SDK'sı

Aşağıdaki kod, Java SDK'sını kullanarak sonrası tetiği kaydetme gösterir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String triggerId = "trgPostUpdateMetadata";
Trigger trigger = new Trigger();
trigger.setId(triggerId);
trigger.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", triggerId)))));
trigger.setTriggerOperation(TriggerOperation.Create);
trigger.setTriggerType(TriggerType.Post);
Trigger createdTrigger = asyncClient.createTrigger(containerLink, trigger, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'sını kullanarak sonrası bir tetikleyici çağrılacak gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Document item = new Document(String.format("{ "
    + "\"name\": \"artist_profile_1023\", "
    + "\"artist\": \"The Band\", "
    + "\"albums\": [\"Hellujah\", \"Rotators\", \"Spinning Top\"]"
    + "}"
));
RequestOptions requestOptions = new RequestOptions();
requestOptions.setPostTriggerInclude(Arrays.asList("trgPostUpdateMetadata"));
//toBlocking() blocks the thread until the operation is complete, and is used only for demo.
asyncClient.createDocument(containerLink, item, requestOptions, false).toBlocking();
```

### <a name="post-triggers---javascript-sdk"></a>Sonrası Tetikleyicileri-JavaScript SDK'sı

Aşağıdaki kod, JavaScript SDK'sını kullanarak sonrası tetiği kaydetme gösterir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.triggers.create({
    id: triggerId,
    body: require(`../js/${triggerId}`),
    triggerOperation: "create",
    triggerType: "post"
});
```

Aşağıdaki kod, JavaScript SDK'sını kullanarak sonrası bir tetikleyici çağrılacak gösterilmektedir:

```javascript
const item = {
    name: "artist_profile_1023",
    artist: "The Band",
    albums: ["Hellujah", "Rotators", "Spinning Top"]
};
const container = client.database("myDatabase").container("myContainer");
const triggerId = "trgPostUpdateMetadata";
await container.items.create(item, {postTriggerInclude: [triggerId]});
```

### <a name="post-triggers---python-sdk"></a>Sonrası Tetikleyicileri-Python SDK'sı

Aşağıdaki kod, Python SDK'sını kullanarak sonrası tetiği kaydetme gösterir:

```python
with open('../js/trgPostUpdateMetadata.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
trigger_definition = {
            'id': 'trgPostUpdateMetadata',
            'serverScript': file_contents,
            'triggerType': documents.TriggerType.Post,
            'triggerOperation': documents.TriggerOperation.Create
        }
trigger = client.CreateTrigger(container_link, trigger_definition)
```

Aşağıdaki kod nasıl çağrılacağını Python SDK'sını kullanarak sonrası bir tetikleyici gösterir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
item = { 'name': 'artist_profile_1023', 'artist': 'The Band', 'albums': ['Hellujah', 'Rotators', 'Spinning Top']}
client.CreateItem(container_link, item, { 'postTriggerInclude': 'trgPostUpdateMetadata'})
```

## <a id="udfs"></a>Kullanıcı tanımlı işlevlerle çalışma

Aşağıdaki örnekler, Azure Cosmos DB SDK'larını kullanarak bir kullanıcı tanımlı işlev kaydettirmek gösterilmektedir. Bu [kullanıcı tanımlı işlev örneği](how-to-write-stored-procedures-triggers-udfs.md#udfs) sonrası Bu tetikleyici için kaynak olarak kaydedildiğinde `udfTax.js`.

### <a name="user-defined-functions---net-sdk"></a>Kullanıcı tanımlı işlevler - .NET SDK'sı

Aşağıdaki kod, .NET SDK'sını kullanarak bir kullanıcı tanımlı işlev kaydettirmek gösterilmektedir:

```csharp
string udfId = "Tax";
var udfTax = new UserDefinedFunction
{
    Id = udfId,
    Body = File.ReadAllText($@"..\js\{udfId}.js"),
};

containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
await client.CreateUserDefinedFunctionAsync(containerUri, udfTax);

```

Aşağıdaki kod, .NET SDK kullanarak kullanıcı tanımlı bir işlevi çağırmak nasıl gösterir:

```csharp
Uri containerUri = UriFactory.CreateDocumentCollectionUri("myDatabase", "myContainer");
var results = client.CreateDocumentQuery<dynamic>(containerUri, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000"));

foreach (var result in results)
{
    //iterate over results
}
```

### <a name="user-defined-functions---java-sdk"></a>Kullanıcı tanımlı işlevler - Java SDK'sı

Aşağıdaki kod, Java SDK'sını kullanarak bir kullanıcı tanımlı işlev kaydettirmek gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
String udfId = "Tax";
UserDefinedFunction udf = new UserDefinedFunction();
udf.setId(udfId);
udf.setBody(new String(Files.readAllBytes(Paths.get(String.format("..\\js\\%s.js", udfId)))));
//toBlocking() blocks the thread until the operation is complete and is used only for demo.
UserDefinedFunction createdUDF = client.createUserDefinedFunction(containerLink, udf, new RequestOptions()).toBlocking().single().getResource();
```

Aşağıdaki kod, Java SDK'sını kullanarak bir kullanıcı tanımlı işlevi çağırma gösterilmektedir:

```java
String containerLink = String.format("/dbs/%s/colls/%s", "myDatabase", "myContainer");
Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(containerLink, "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000", new FeedOptions());
final CountDownLatch completionLatch = new CountDownLatch(1);
queryObservable.subscribe(
        queryResultPage -> {
            System.out.println("Got a page of query result with " +
                    queryResultPage.getResults().size());
        },
        // terminal error signal
        e -> {
            e.printStackTrace();
            completionLatch.countDown();
        },

        // terminal completion signal
        () -> {
            completionLatch.countDown();
        });
completionLatch.await();
```

### <a name="user-defined-functions---javascript-sdk"></a>Kullanıcı tanımlı işlevler - JavaScript SDK'sı

Aşağıdaki kod, JavaScript SDK'sını kullanarak bir kullanıcı tanımlı işlev kaydettirmek gösterilmektedir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const udfId = "Tax";
await container.userDefinedFunctions.create({
    id: udfId,
    body: require(`../js/${udfId}`)
```

Aşağıdaki kod, JavaScript SDK'sını kullanarak bir kullanıcı tanımlı işlevi çağırma gösterilmektedir:

```javascript
const container = client.database("myDatabase").container("myContainer");
const sql = "SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000";
const {result} = await container.items.query(sql).toArray();
```

### <a name="user-defined-functions---python-sdk"></a>Kullanıcı tanımlı işlevler - Python SDK'sı

Aşağıdaki kod, Python SDK'sını kullanarak bir kullanıcı tanımlı işlev kaydettirmek gösterilmektedir:

```python
with open('../js/udfTax.js') as file:
    file_contents = file.read()
container_link = 'dbs/myDatabase/colls/myContainer'
udf_definition = {
            'id': 'Tax',
            'serverScript': file_contents,
        }
udf = client.CreateUserDefinedFunction(container_link, udf_definition)
```

Aşağıdaki kod, Python SDK'sını kullanarak bir kullanıcı tanımlı işlevi çağırma gösterilmektedir:

```python
container_link = 'dbs/myDatabase/colls/myContainer'
results = list(client.QueryItems(container_link, 'SELECT * FROM Incomes t WHERE udf.Tax(t.income) > 20000'))
```

## <a name="next-steps"></a>Sonraki adımlar

Kavramları ve nasıl yapılır yazma daha fazla bilgi edinin veya saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler, Azure Cosmos DB'de kullanın:

- [Yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevleri Azure Cosmos DB'de depolanan Azure Cosmos DB ile çalışma](stored-procedures-triggers-udfs.md)
- [Azure Cosmos DB'de sorgu API'si çalışma JavaScript dil ile tümleşik](javascript-query-api.md)
- [Azure Cosmos DB'de saklı yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler yazma](how-to-write-stored-procedures-triggers-udfs.md)
- [Saklı yordamları ve Tetikleyicileri Javascript sorgu API'si kullanarak Azure Cosmos DB'de yazma](how-to-write-javascript-query-api.md)
