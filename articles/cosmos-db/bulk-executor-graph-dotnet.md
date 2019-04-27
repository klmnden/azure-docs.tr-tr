---
title: Azure Cosmos DB Gremlin API'de toplu işlem gerçekleştirmek için grafik BulkExecutor .NET kitaplığını kullanma
description: BulkExecutor kitaplığını kullanarak Azure Cosmos DB Gremlin API'si kapsayıcısına çok miktarda grafik verisini aktarmayı öğrenin.
author: luisbosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: lbosq
ms.reviewer: sngun
ms.openlocfilehash: 5e88602aa3b983e1533248253d53967f39e6b5eb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60894399"
---
# <a name="using-the-graph-bulkexecutor-net-library-to-perform-bulk-operations-in-azure-cosmos-db-gremlin-api"></a>Azure Cosmos DB Gremlin API'de toplu işlem gerçekleştirmek için grafik BulkExecutor .NET kitaplığını kullanma

Bu öğreticide Azure CosmosDB's BulkExecutor .NET kitaplığını kullanarak Azure Cosmos DB Gremlin API kapsayıcısında grafik nesnelerini aktarma ve güncelleştirme işlemleri hakkında bilgi verilmektedir. Bu işlem, [BulkExecutor kitaplığındaki](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-overview) Graph sınıfını kullanarak Vertex ve Edge nesnelerini program aracılığıyla oluşturur ve ağ isteğine göre birden fazlasını ekler. Bu davranış BulkExecutor kitaplığı aracılığıyla yapılandırılarak hem veritabanının hem de yerel bellek kaynaklarının en uygun şekilde kullanılması sağlanır.

Komutun değerlendirilip teker teker yürütüldüğü veritabanına Gremlin sorgusu gönderme durumundan farklı olarak BulkExecutor kitaplığı kullanıldığında nesnelerin yerel olarak oluşturulması ve doğrulanması gerekir. Kitaplık, nesneleri oluşturduktan sonra grafik nesnelerini veritabanı hizmetine sıralı bir şekilde göndermenizi sağlar. Bu yöntemle veri alımı hızları 100 kata kadar artırılabilir ve bu da ilk veri geçişi veya düzenli veri taşıma işlemleri için ideal bir yöntem olmasını sağlar. Daha fazla bilgi için [Azure Cosmos DB Graph BulkExecutor örnek uygulama](https://aka.ms/graph-bulkexecutor-sample) GitHub sayfasını ziyaret edin.

## <a name="bulk-operations-with-graph-data"></a>Grafik verileriyle toplu işlemler

[BulkExecutor kitaplığı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph?view=azure-dotnet), grafik nesnesi oluşturma ve içeri aktarma işlevi sunmak için bir `Microsoft.Azure.CosmosDB.BulkExecutor.Graph` ad alanı içerir. 

Aşağıdaki işlem, veri geçişinin bir Gremlin API kapsayıcısı için nasıl kullanılabileceğini göstermektedir:
1. Kayıtları veri kaynağından alın.
2. Alınan kayıtlardan `GremlinVertex` ve `GremlinEdge` nesnesini oluşturun ve bunları bir `IEnumerable` veri yapısına ekleyin. Uygulamanın bu bölümünde veri kaynağının grafik veritabanı olmaması halinde ilişki algılama ve ekleme mantığının uygulanması gerekir.
3. Grafik nesnelerini koleksiyona eklemek için [Grafik BulkImportAsync metodunu](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph.graphbulkexecutor.bulkimportasync?view=azure-dotnet) kullanın.

Bu mekanizma, Gremlin istemcisi kullanmaya kıyasla veri geçişinin verimliliğini artırır. Bu geliştirme, verilerin Gremlin ile eklenmesi için uygulamanın doğrulanması, değerlendirilmesi ve ardından veri oluşturmak için yürütülmesi gereken sorguları tek tek gönderilmesi gerektiğindendir. BulkExecutor kitaplığı, uygulamada doğrulama işlemini gerçekleştirir ve her ağ isteği için tek seferde birden fazla grafik nesnesi gönderir.

### <a name="creating-vertices-and-edges"></a>Köşe ve kenar oluşturma

`GraphBulkExecutor`, `Microsoft.Azure.CosmosDB.BulkExecutor.Graph.Element` ad alanında tanımlı `GremlinVertex` veya `GremlinEdge` nesnelerinin `IEnumerable` listesini gerektiren `BulkImportAsync` metodunu sunar. Örnekte kenarları ve köşeleri iki BulkExecutor içeri aktarma görevine ayırdık. Aşağıdaki örneğe bakın:

```csharp

IBulkExecutor graphbulkExecutor = new GraphBulkExecutor(documentClient, targetCollection);

BulkImportResponse vResponse = null;
BulkImportResponse eResponse = null;

try
{
    // Import a list of GremlinVertex objects
    vResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateVertices(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token);

    // Import a list of GremlinEdge objects
    eResponse = await graphbulkExecutor.BulkImportAsync(
            Utils.GenerateEdges(numberOfDocumentsToGenerate),
            enableUpsert: true,
            disableAutomaticIdGeneration: true,
            maxConcurrencyPerPartitionKeyRange: null,
            maxInMemorySortingBatchSize: null,
            cancellationToken: token);
}
catch (DocumentClientException de)
{
    Trace.TraceError("Document client exception: {0}", de);
}
catch (Exception e)
{
    Trace.TraceError("Exception: {0}", e);
}
```

BulkExecutor kitaplığının parametreleri hakkında daha fazla bilgi için [Azure Cosmos DB BulkImportData konusuna](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-dot-net#bulk-import-data-to-azure-cosmos-db) bakın.

Yükün `GremlinVertex` ve `GremlinEdge` nesnelerinde oluşturulması gerekir. Bu nesneler şu şekilde oluşturulabilir:

**Köşeler**:
```csharp
// Creating a vertex
GremlinVertex v = new GremlinVertex(
    "vertexId",
    "vertexLabel");

// Adding custom properties to the vertex
v.AddProperty("customProperty", "value");

// Partitioning keys must be specified for all vertices
v.AddProperty("partitioningKey", "value");
```

**Kenarlar**:
```csharp
// Creating an edge
GremlinEdge e = new GremlinEdge(
    "edgeId",
    "edgeLabel",
    "targetVertexId",
    "sourceVertexId",
    "targetVertexLabel",
    "sourceVertexLabel",
    "targetVertexPartitioningKey",
    "sourceVertexPartitioningKey");

// Adding custom properties to the edge
e.AddProperty("customProperty", "value");
```

> [!NOTE]
> BulkExecutor yardımcı programı, Kenarları eklemeden önce var olan Köşeleri otomatik olarak denetler. Bu durumun BulkImport görevlerini çalıştırmadan önce uygulamada doğrulanması gerekir.

## <a name="sample-application"></a>Örnek uygulama

### <a name="prerequisites"></a>Önkoşullar
* Azure geliştirme iş yüküyle Visual Studio 2017. [Visual Studio 2017 Community Edition](https://visualstudio.microsoft.com/downloads/)'ı ücretsiz kullanmaya başlayabilirsiniz.
* Azure aboneliği. [Buradan ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cosmos-db) oluşturabilirsiniz. Alternatif olarak Azure aboneliği kullanmadan [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) bağlantısından bir Cosmos DB veritabanı hesabı oluşturabilirsiniz.
* **Sınırsız koleksiyona** sahip Azure Cosmos DB Gremlin API veritabanı. Bu kılavuz, [.NET ile Azure Cosmos DB Gremlin API'yi](https://docs.microsoft.com/azure/cosmos-db/create-graph-dotnet) kullanmaya başlamayı göstermektedir.
* Git. Daha fazla bilgi için [Git indirme sayfasına](https://git-scm.com/downloads) bakın.

### <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama
Bu öğreticide GitHub üzerinde barındırılan [Azure Cosmos DB Graph BulkExecutor örneğini](https://aka.ms/graph-bulkexecutor-sample) kullanarak kullanmaya başlama adımlarını takip edeceğiz. Bu uygulama, rastgele köşe ve kenar nesneleri oluşturup belirtilen grafik veritabanı hesabına toplu ekleme yapan bir .NET çözümü içerir. Uygulamayı almak için aşağıdaki `git clone` komutunu çalıştırın:

```bash
git clone https://github.com/Azure-Samples/azure-cosmosdb-graph-bulkexecutor-dotnet-getting-started.git
```

Bu depo, aşağıdaki dosyalara sahip olan GraphBulkExecutor örneğini içerir:

Dosya|Açıklama
---|---
`App.config`|Uygulama ve veritabanına özgü parametreler burada belirtilir. Hedef veritabanına ve koleksiyonlara bağlanmak için bu dosyanın değiştirilmesi gerekir.
`Program.cs`| Temizleme işlemlerini gerçekleştiren ve BulkExecutor isteklerini gönderen bu dosya, `DocumentClient` koleksiyonu oluşturma mantığını içerir.
`Util.cs`| Bu dosya, test verileri oluşturmaya ek olarak veritabanı ve koleksiyonların var olup olmadığını kontrol eden mantığı içeren yardımcı sınıfı içerir.

`App.config` dosyasında aşağıdaki yapılandırma değerleri sağlanabilir:

Ayar|Açıklama
---|---
`EndPointUrl`|Bu, Azure Cosmos DB Gremlin API veritabanı hesabınızın Genel Bakış dikey penceresinde bulunan **.NET SDK uç noktanızdır**. `https://your-graph-database-account.documents.azure.com:443/` biçimine sahiptir
`AuthorizationKey`|Bu, Azure Cosmos DB hesabınızda listelenen Birincil veya İkincil anahtardır. [Azure Cosmos DB verilerine güvenli erişim sağlama](https://docs.microsoft.com/azure/cosmos-db/secure-access-to-data#master-keys) hakkında daha fazla bilgi edinin
`DatabaseName`, `CollectionName`|Bunlar **hedef veritabanı ve koleksiyon adlarıdır**. `ShouldCleanupOnStart`, `true` olarak ayarlandığında bu değerler `CollectionThroughput` ile birlikte bunları bırakmaya ek olarak yeni bir veritabanı ve koleksiyon oluşturmak için kullanılır. Benzer şekilde `ShouldCleanupOnFinish`, `true` olarak ayarlandığında alma işlemi sona erdiğinde veritabanını silmek için kullanılır. Hedef koleksiyonun **sınırsız koleksiyon** olması gerektiğini unutmayın.
`CollectionThroughput`|Bu, `ShouldCleanupOnStart` seçeneği `true` olarak ayarlandığında yeni koleksiyon oluşturmak için kullanılır.
`ShouldCleanupOnStart`|Bu işlem program çalıştırılmadan önce veritabanı hesabını ve koleksiyonlarını bırakır ve ardından `DatabaseName`, `CollectionName` ve `CollectionThroughput` değerleriyle yenilerini oluşturur.
`ShouldCleanupOnFinish`|Bu değer program çalıştırıldıktan sonra belirtilen `DatabaseName` ve `CollectionName` değerleriyle veritabanı hesabını ve koleksiyonlarını bırakır.
`NumberOfDocumentsToImport`|Bu değer örnekte oluşturulacak test köşesi ve kenarı sayısını belirler. Bu sayı hem köşeler hem de kenarlar için geçerlidir.
`NumberOfBatches`|Bu değer örnekte oluşturulacak test köşesi ve kenarı sayısını belirler. Bu sayı hem köşeler hem de kenarlar için geçerlidir.
`CollectionPartitionKey`|Bu değer bu özelliğin otomatik olarak atanacağı test köşelerini ve kenarlarını oluşturmak için kullanılır. Bu değer aynı zamanda `ShouldCleanupOnStart` seçeneği `true` olarak ayarlandığında veritabanını ve koleksiyonları yeniden oluşturmak için kullanılır.

### <a name="run-the-sample-application"></a>Örnek uygulamayı çalıştırın

1. Belirli veritabanı yapılandırma parametrelerinizi `App.config` içine ekleyin. Bu değer bir DocumentClient örneği oluşturmak için kullanılır. Veritabanı ve kapsayıcı henüz oluşturulmadıysa otomatik olarak oluşturulur.
2. Uygulamayı çalıştırın. Biri Köşeleri, biri Kenarları içeri aktarmak için olmak üzere `BulkImportAsync` iki kez çağrılır. Ekleme sırasında hata oluşturan nesneler `.\BadVertices.txt` veya `.\BadEdges.txt` içine eklenir.
3. Grafik veritabanını sorgulayarak sonuçları değerlendirin. `ShouldCleanupOnFinish` seçeneği true olarak ayarlanırsa veritabanı otomatik olarak silinir.

## <a name="next-steps"></a>Sonraki adımlar
* Nuget paket ayrıntıları hakkında bilgi edinin ve sürüm notları toplu Yürütücü .NET Kitaplığı'nın için bkz: [toplu Yürütücü SDK ayrıntıları](sql-api-sdk-bulk-executor-dot-net.md). 
* BulkExecutor kullanımını daha fazla iyileştirmek için [Performans İpuçları](https://docs.microsoft.com/azure/cosmos-db/bulk-executor-dot-net#performance-tips)'na bakın.
* Bu ad alanında tanımlanan sınıflar ve metotlar hakkında ayrıntılı bilgi için [BulkExecutor.Graph Başvurusu makalesini](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmosdb.bulkexecutor.graph?view=azure-dotnet) gözden geçirin.
