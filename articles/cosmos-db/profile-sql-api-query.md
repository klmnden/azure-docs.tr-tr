---
title: SQL sorgu performansı ve yürütme ölçümleri alma
description: SQL sorgusu yürütme ölçümleri ve Azure Cosmos DB isteklerinin SQL sorgu performansı profili alma hakkında bilgi edinin.
author: ginamr
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: girobins
ms.openlocfilehash: b4017666956d0e01ea19781fb4f1ce2dde15fff5
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481572"
---
# <a name="get-sql-query-execution-metrics-and-analyze-query-performance-using-net-sdk"></a>SQL sorgusu yürütme ölçümleri alın ve .NET SDK kullanarak sorgu performansını analiz edin

Bu makalede, Azure Cosmos DB'de SQL sorgu performansı profil nasıl gösterir. Bu profil oluşturma yapılabilir kullanarak `QueryMetrics` .NET SDK'sından alınan ve burada ayrıntıları. [QueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.querymetrics.aspx) arka uç sorgu yürütme hakkında bilgi içeren kesin türü belirtilmiş bir nesnedir. Bu ölçümleri daha ayrıntılı olarak belgelenmiştir [sorgu performansını ayarlama](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) makalesi.

## <a name="set-the-feedoptions-parameter"></a>FeedOptions parametre kümesi

İçin tüm aşırı yüklemeler [DocumentClient.CreateDocumentQuery](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentquery.aspx) bir isteğe bağlı olarak ele [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) parametresi. Bu seçenek, ne ayarlanmış ve parametreli sorguyu yürütme sağlayan olur. 

Sql sorgusu yürütme ölçümleri toplamak için parametre kümesi [PopulateQueryMetrics](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.populatequerymetrics.aspx#P:Microsoft.Azure.Documents.Client.FeedOptions.PopulateQueryMetrics) içinde [FeedOptions](https://msdn.microsoft.com/library/microsoft.azure.documents.client.feedoptions.aspx) için `true`. Ayarı `PopulateQueryMetrics` için true, bunu yapacak şekilde `FeedResponse` ilgili içerecek `QueryMetrics`. 

## <a name="get-query-metrics-with-asdocumentquery"></a>Get query metrics with AsDocumentQuery()
Aşağıdaki kod örneği, ölçümleri kullanırken alma işlemi gösterilmektedir [AsDocumentQuery()](https://msdn.microsoft.com/library/microsoft.azure.documents.linq.documentqueryable.asdocumentquery.aspx) yöntemi:

```csharp
// Initialize this DocumentClient and Collection
DocumentClient documentClient = null;
DocumentCollection collection = null;

// Setting PopulateQueryMetrics to true in the FeedOptions
FeedOptions feedOptions = new FeedOptions
{
    PopulateQueryMetrics = true
};

string query = "SELECT TOP 5 * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    
    // At this point you have QueryMetrics which you can serialize using .ToString()
    foreach (KeyValuePair<string, QueryMetrics> kvp in partitionIdToQueryMetrics)
    {
        string partitionId = kvp.Key;
        QueryMetrics queryMetrics = kvp.Value;
        
        // Do whatever logging you need
        DoSomeLoggingOfQueryMetrics(query, partitionId, queryMetrics);
    }
}
```
## <a name="aggregating-querymetrics"></a>QueryMetrics toplama

Önceki bölümde birden çok çağrı olduğunu fark [ExecuteNextAsync](https://msdn.microsoft.com/library/azure/dn850294.aspx) yöntemi. Döndürülen her çağrı bir `FeedResponse` sözlüğü nesnesi `QueryMetrics`; bir sorgunun her devamlılık için. Aşağıdaki örnek, bu toplama işlemi gösterilmektedir `QueryMetrics` LINQ kullanarak:

```csharp
List<QueryMetrics> queryMetricsList = new List<QueryMetrics>();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    queryMetricsList.AddRange(partitionIdToQueryMetrics.Values);
}

// Aggregate the QueryMetrics using the + operator overload of the QueryMetrics class.
QueryMetrics aggregatedQueryMetrics = queryMetricsList.Aggregate((curr, acc) => curr + acc);
Console.WriteLine(aggregatedQueryMetrics);
```

## <a name="grouping-query-metrics-by-partition-id"></a>Sorgu ölçümleri bölümü Kimliğine göre gruplandırma

Gruplandırma yapabilirsiniz `QueryMetrics` tarafından bölüm kimliği Gruplandırma yaparak, bölüm kimliği, belirli bir bölüme başkalarına karşılaştırıldığında performans sorunlarına neden olup olmadığını görmenize olanak sağlar. Aşağıdaki örnek nasıl gruplanacağını gösterir `QueryMetrics` LINQ ile:

```csharp
List<KeyValuePair<string, QueryMetrics>> partitionedQueryMetrics = new List<KeyValuePair<string, QueryMetrics>>();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    
    // This dictionary is maps the partitionId to the QueryMetrics of that query
    IReadOnlyDictionary<string, QueryMetrics> partitionIdToQueryMetrics = feedResponse.QueryMetrics;
    partitionedQueryMetrics.AddRange(partitionIdToQueryMetrics.ToList());
}

// Now we are able to group the query metrics by partitionId
IEnumerable<IGrouping<string, KeyValuePair<string, QueryMetrics>>> groupedByQueryMetrics = partitionedQueryMetrics
    .GroupBy(kvp => kvp.Key);

// If we wanted to we could even aggregate the groupedby QueryMetrics
foreach(IGrouping<string, KeyValuePair<string, QueryMetrics>> grouping in groupedByQueryMetrics)
{
    string partitionId = grouping.Key;
    QueryMetrics aggregatedQueryMetricsForPartition = grouping
        .Select(kvp => kvp.Value)
        .Aggregate((curr, acc) => curr + acc);
    DoSomeLoggingOfQueryMetrics(query, partitionId, aggregatedQueryMetricsForPartition);
}
```

## <a name="linq-on-documentquery"></a>LINQ on DocumentQuery

Ayrıca Al `FeedResponse` kullanarak LINQ sorgusu `AsDocumentQuery()` yöntemi:

```csharp
IDocumentQuery<Document> linqQuery = client.CreateDocumentQuery(collection.SelfLink, feedOptions)
    .Take(1)
    .Where(document => document.Id == "42")
    .OrderBy(document => document.Timestamp)
    .AsDocumentQuery();
FeedResponse<Document> feedResponse = await linqQuery.ExecuteNextAsync<Document>();
IReadOnlyDictionary<string, QueryMetrics> queryMetrics = feedResponse.QueryMetrics;
```

## <a name="expensive-queries"></a>Pahalı sorguları

Pahalı sorguları veya yüksek aktarım hızı tüketen sorguları araştırmak için her bir sorgu tarafından kullanılan istek birimleri yakalayabilirsiniz. İstek yükü kullanarak alabilirsiniz [RequestCharge](https://msdn.microsoft.com/library/azure/dn948712.aspx) özelliğinde `FeedResponse`. Farklı SDK'ları ve Azure portalı kullanarak istek ücret alma hakkında daha fazla bilgi için bkz. [istek birimi ücreti Bul](find-request-unit-charge.md) makalesi.

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();

while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    double requestCharge = feedResponse.RequestCharge
    
    // Log the RequestCharge how ever you want.
    DoSomeLogging(requestCharge);
}
```

## <a name="get-the-query-execution-time"></a>Sorgu yürütme süresini Al

Bir istemci-tarafı sorgu yürütmek için gereken süreyi hesaplanırken, yalnızca çağrı süresi eklediğinizden emin olun `ExecuteNextAsync` metodu ve kod tabanınız değil diğer bölümlerini. Yalnızca bu çağrıları ne sorgu yürütme aşağıdaki örnekte gösterildiği gibi sürdüğü hesaplamasında yardımcı olur:

```csharp
string query = "SELECT * FROM c";
IDocumentQuery<dynamic> documentQuery = documentClient.CreateDocumentQuery(Collection.SelfLink, query, feedOptions).AsDocumentQuery();
Stopwatch queryExecutionTimeEndToEndTotal = new Stopwatch();
while (documentQuery.HasMoreResults)
{
    // Execute one continuation of the query
    queryExecutionTimeEndToEndTotal.Start();
    FeedResponse<dynamic> feedResponse = await documentQuery.ExecuteNextAsync();
    queryExecutionTimeEndToEndTotal.Stop();
}

// Log the elapsed time
DoSomeLogging(queryExecutionTimeEndToEndTotal.Elapsed);
```

## <a name="scan-queries-commonly-slow-and-expensive"></a>Sorgular (genellikle yavaş ve pahalı) tarayın

Bir tarama sorgu nedeniyle hangi dizine göre birçok hizmet değildi bir sorguya başvurur belgeleri, sonuç kümesini döndürmeden önce yüklenir.

Bir tarama sorgu örneği aşağıdadır:

```sql
SELECT VALUE c.description 
FROM   c 
WHERE UPPER(c.description) = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Bu sorgunun filtre dizinden sunulan olmayan üst, sistem işlevi kullanır. Bu sorgu büyük koleksiyon karşı yürütülen ilk devamlılığın için aşağıdaki sorgu ölçümler üretilen:

```
QueryMetrics

Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
Output Document Count                    :               7
Output Document Size                     :             510 bytes
Index Utilization                        :            0.00 %
Total Query Execution Time               :        4,500.34 milliseconds
  Query Preparation Times
    Query Compilation Time               :            0.09 milliseconds
    Logical Plan Build Time              :            0.05 milliseconds
    Physical Plan Build Time             :            0.04 milliseconds
    Query Optimization Time              :            0.01 milliseconds
  Index Lookup Time                      :            0.01 milliseconds
  Document Load Time                     :        4,177.66 milliseconds
  Runtime Execution Times
    Query Engine Times                   :          322.16 milliseconds
    System Function Execution Time       :           85.74 milliseconds
    User-defined Function Execution Time :            0.00 milliseconds
  Document Write Time                    :            0.01 milliseconds
Client Side Metrics
  Retry Count                            :               0
  Request Charge                         :        4,059.95 RUs
```

Aşağıdaki değerler sorgu ölçümleri dikkat edin:

```
Retrieved Document Count                 :          60,951
Retrieved Document Size                  :     399,998,938 bytes
```

Bu sorgu 399,998,938 baytları toplamı 60,951 belgeleri yüklendi. Bu kadar bayt yükleniyor yüksek maliyet veya istek birimi ücreti sonuçlanır. Ayrıca, geçen toplam süre özellik boşsa sorguyu yürütmek için uzun zaman alır:

```
Total Query Execution Time               :        4,500.34 milliseconds
```

Yani sorgu yürütmek için 4.5 saniye sürdü (ve bu yalnızca bir devamlılık).

Bu örnek sorgu iyileştirmek için filtre üst kullanmaktan kaçının. Bunun yerine, ne zaman belgeleri oluşturulur veya güncelleştirilir `c.description` değerleri, tüm büyük harfli karakterler girilmelidir. Sorgu daha sonra olur: 

```sql
SELECT VALUE c.description 
FROM   c 
WHERE c.description = "BABYFOOD, DESSERT, FRUIT DESSERT, WITHOUT ASCORBIC ACID, JUNIOR"
```

Bu sorgu dizinden mümkün.

Sorgu performansını ayarlama hakkında daha fazla bilgi edinmek için [sorgu performansını ayarlama](https://docs.microsoft.com/azure/cosmos-db/documentdb-sql-query-metrics) makalesi.

## <a id="References"></a>Başvuruları

- [Azure Cosmos DB SQL belirtimi](https://go.microsoft.com/fwlink/p/?LinkID=510612)
- [ANSI SQL 2011](https://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=53681)
- [JSON](https://json.org/)
- [LINQ](/previous-versions/dotnet/articles/bb308959(v=msdn.10)) 

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu performansını ayarlama](sql-api-query-metrics.md)
- [Dizin oluşturma genel bakış](index-overview.md)
- [Azure Cosmos DB .NET örnekleri](https://github.com/Azure/azure-cosmosdb-dotnet)
