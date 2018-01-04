---
title: "Azure Cosmos DB: grafik API'si, .NET geliştirme | Microsoft Docs"
description: ".NET kullanarak Azure Cosmos veritabanı SQL API'si ile geliştirmeyi öğrenin"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: ddbfe11e4415e1c240914142f4daf54b3032f5d8
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: grafik API'si, .NET geliştirin
Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğretici, Azure portalını kullanarak bir Azure Cosmos DB hesabının nasıl oluşturulacağını ve grafik veritabanı ve kapsayıcı nasıl oluşturulacağını gösterir. Uygulama basit bir sosyal ağ kullanan dört kişilerle oluşturur [grafik API'si](graph-sdk-dotnet.md), ardından erişir ve Gremlin kullanarak grafik sorgular.

Bu öğretici, aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Bir grafik veritabanı ve kapsayıcı oluşturun
> * Köşeleri ve kenarları .NET nesneleri seri hale
> * Köşeleri ve kenarları ekleme
> * Grafiğin Gremlin kullanarak sorgulama

## <a name="graphs-in-azure-cosmos-db"></a>Azure Cosmos DB grafiklerde
Azure Cosmos DB oluşturmak, güncelleştirmek ve grafikler kullanarak sorgulamak için kullanabileceğiniz [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) kitaplığı. Microsoft.Azure.Graph kitaplığı tek genişletme yöntemi sağlar `CreateGremlinQuery<T>` üstünde `DocumentClient` Gremlin sorgularını yürütmek için sınıf.

Gremlin destekleyen işlevsel bir programlama dili yazma işlemleri (DML) ve sorgu ve çapraz geçişi işlemleri ' dir. Biz bu makalede, başlatılan Gremlin ile almak için birkaç örnek kapsar. Bkz: [Gremlin sorguları](gremlin-support.md) ayrıntılı kılavuz Gremlin özelliklerinden Azure Cosmos DB içinde kullanılabilir. 

## <a name="prerequisites"></a>Önkoşullar
Lütfen aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 
    * Alternatif olarak, kullanabileceğiniz [yerel öykünücüsü](local-emulator.md) Bu öğretici için.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Veritabanı hesabı oluşturma

Azure portalında bir Azure Cosmos DB hesabı oluşturarak başlayalım.  

> [!TIP]
> * Zaten Azure Cosmos DB hesabınız var mı? Bu durumda, İleri için atlayabilirsiniz [, Visual Studio çözümü ayarlama](#SetupVS)
> * Azure Cosmos DB öykünücüsü kullanıyorsanız, lütfen bölümündeki adımları izleyin [Azure Cosmos DB öykünücüsü](local-emulator.md) öykünücü kurulması ve için İleri atlayabilirsiniz [, Visual Studio çözümünü kurmak](#SetupVS). 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. İçinde **yeni proje** iletişim kutusunda **şablonları** / **Visual C#** / **konsol uygulaması (.NET Framework)** , projenizi adlandırın ve ardından **Tamam**.
4. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.
5. İçinde **NuGet** sekmesini tıklatın, **Gözat**ve türü **Microsoft.Azure.Graphs** arama kutusu ve onay **yayın öncesi sürümlerdahil**.
6. Sonuçları içinde bulma **Microsoft.Azure.Graphs** tıklatıp **yükleme**.
   
   Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.
   
    `Microsoft.Azure.Graphs` Kitaplığı, bir tek genişletme yöntemi sağlar `CreateGremlinQuery<T>` Gremlin işlemleri yürütmek. Gremlin destekleyen işlevsel bir programlama dili yazma işlemleri (DML) ve sorgu ve çapraz geçişi işlemleri ' dir. Biz bu makalede, başlatılan Gremlin ile almak için birkaç örnek kapsar. [Gremlin sorguları](gremlin-support.md) Azure Cosmos DB'de Gremlin özelliklerinin ayrıntılı bir kılavuz vardır.

## <a id="add-references"></a>Uygulamanızı bağlama

Bu iki sabitleri ekleyin ve *istemci* uygulamanızda değişken. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Head ardından, yeniden [Azure portal](https://portal.azure.com) uç noktasının URL'sini ve birincil anahtar alınamadı. Uç nokta URL’si ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir. 

Azure portalında Azure Cosmos DB hesabınıza gidin, tıklatın **anahtarları**ve ardından **okuma-yazma anahtarları**. 

URI Portal'dan kopyalayın ve üzerinden yapıştırın `Endpoint` yukarıdaki uç nokta özelliğinde. Ardından portaldan birincil anahtarı kopyalayın ve yapıştırın `AuthKey` yukarıdaki özelliği. 

![Bir C# uygulaması oluşturmak için Öğreticisi tarafından kullanılan Azure portal ekran görüntüsü. ANAHTARLAR düğmesi üzerinde Azure Cosmos DB Gezinti vurgulanmış ve anahtarlar dikey penceresinde URI ve birincil anahtar değerleri gösterir bir Azure Cosmos DB hesabı](./media/tutorial-develop-graph-dotnet/keys.png) 
 
## <a id="instantiate"></a>DocumentClient örneği 
Ardından, yeni bir örneğini oluşturun **DocumentClient**.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Bir veritabanı oluşturun 

Şimdi bir Azure Cosmos DB Oluştur [veritabanı](sql-api-resources.md#databases) kullanarak [Documentclient](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemi veya [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) yöntemi  **DocumentClient** sınıfıyla [SQL .NET SDK'sı](sql-api-sdk-dotnet.md).  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Bir grafik oluşturma 

Ardından, kullanarak kullanarak bir grafik kapsayıcı oluşturmak [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemi veya [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) yöntemi **DocumentClient** sınıfı. Bir koleksiyon, grafik varlıkların bir kapsayıcıdır. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 1000 }); 
``` 

## <a id="serializing"></a>Köşeleri ve kenarları .NET nesneleri seri hale
Azure Cosmos DB kullanır [GraphSON kablo biçiminde](gremlin-support.md), köşe, kenarları ve özellikleri için JSON şeması tanımlar. JSON.NET bağımlılık olarak Azure Cosmos DB .NET SDK'sı içerir ve bu bize seri/GraphSON biz kodda çalışabilirsiniz .NET nesneleri içine seri durumdan çıkarılacak sağlar.

Örnek olarak, basit bir sosyal ağ dört kişilerle şimdi çalışır. Oluşturmak nasıl tümleştirildiği incelenmektedir `Person` köşeleri eklemek `Knows` arasındaki ilişkileri ardından sorgu ve ilişkileri "arkadaş arkadaş" bulmak için grafiği çapraz geçiş. 

`Microsoft.Azure.Graphs.Elements` Ad alanı sağlar `Vertex`, `Edge`, `Property` ve `VertexProperty` GraphSON yanıtlarını iyi tanımlanmış .NET nesnelerini seri durumdan çıkarmak için sınıflar.

## <a name="run-gremlin-using-creategremlinquery"></a>Gremlin CreateGremlinQuery kullanarak çalıştırma
SQL gibi gremlin okuma, yazma ve sorgu işlemleri destekler. Örneğin, aşağıdaki kod parçacığında köşeleri kenarları oluşturmak, kullanan bazı örnek sorgular gerçekleştirmek gösterilmektedir `CreateGremlinQuery<T>`ve zaman uyumsuz olarak kullanarak bu sonuçlarını yinelemek `ExecuteNextAsync` ve ' HasMoreResults.

```cs
Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
{
    { "Cleanup",        "g.V().drop()" },
    { "AddVertex 1",    "g.addV('person').property('id', 'thomas').property('firstName', 'Thomas').property('age', 44)" },
    { "AddVertex 2",    "g.addV('person').property('id', 'mary').property('firstName', 'Mary').property('lastName', 'Andersen').property('age', 39)" },
    { "AddVertex 3",    "g.addV('person').property('id', 'ben').property('firstName', 'Ben').property('lastName', 'Miller')" },
    { "AddVertex 4",    "g.addV('person').property('id', 'robin').property('firstName', 'Robin').property('lastName', 'Wakefield')" },
    { "AddEdge 1",      "g.V('thomas').addE('knows').to(g.V('mary'))" },
    { "AddEdge 2",      "g.V('thomas').addE('knows').to(g.V('ben'))" },
    { "AddEdge 3",      "g.V('ben').addE('knows').to(g.V('robin'))" },
    { "UpdateVertex",   "g.V('thomas').property('age', 44)" },
    { "CountVertices",  "g.V().count()" },
    { "Filter Range",   "g.V().hasLabel('person').has('age', gt(40))" },
    { "Project",        "g.V().hasLabel('person').values('firstName')" },
    { "Sort",           "g.V().hasLabel('person').order().by('firstName', decr)" },
    { "Traverse",       "g.V('thomas').outE('knows').inV().hasLabel('person')" },
    { "Traverse 2x",    "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')" },
    { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
    { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
    { "CountEdges",     "g.E().count()" },
    { "DropVertex",     "g.V('thomas').drop()" },
};

foreach (KeyValuePair<string, string> gremlinQuery in gremlinQueries)
{
    Console.WriteLine($"Running {gremlinQuery.Key}: {gremlinQuery.Value}");

    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, gremlinQuery.Value);
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    Console.WriteLine();
}
```

## <a name="add-vertices-and-edges"></a>Köşeleri ve kenarları ekleme

Daha fazla ayrıntı önceki bölümde gösterilen Gremlin deyimlerini bakalım. İlk biz Gremlin'ın kullanarak bazı köşeleri `addV` yöntemi. Örneğin, aşağıdaki kod parçacığını ad, Soyadı ve yaş özellikleriyle "Kişi" türündeki "Thomas Andersen" köşe oluşturur.

```cs
// Create a vertex
IDocumentQuery<Vertex> createVertexQuery = client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.addV('person').property('firstName', 'Thomas')");

while (createVertexQuery.HasMoreResults)
{
    Vertex thomas = (await create.ExecuteNextAsync<Vertex>()).First();
}
```

Biz Gremlin'ın kullanarak bu köşeleri arasında bazı kenarları oluşturup `addE` yöntemi. 

```cs
// Add a "knows" edge
IDocumentQuery<Edge> createEdgeQuery = client.CreateGremlinQuery<Edge>(
    graphCollection, 
    "g.V('thomas').addE('knows').to(g.V('mary'))");

while (create.HasMoreResults)
{
    Edge thomasKnowsMaryEdge = (await create.ExecuteNextAsync<Edge>()).First();
}
```

Biz kullanarak var olan bir köşe güncelleştirebilirsiniz `properties` Gremlin içinde adım. Biz aracılığıyla sorguyu yürütmek için çağrı atla `HasMoreResults` ve `ExecuteNextAsync` örnekler geri kalanı için.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Kenarları ve Gremlin'ın kullanarak köşeleri düşürebilir `drop` adım. Bir köşe kenar silip gösteren bir parçacığı aşağıda verilmiştir. Bir köşe bırakarak ilişkili kenarlarının art arda delete gerçekleştirmediğini unutmayın.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Sorgu grafiği

Sorgular ve ayrıca Gremlin kullanarak çapraz geçişlerine gerçekleştirebilirsiniz. Örneğin, aşağıdaki kod parçacığında grafiği tepe sayısı gösterilmektedir:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Gremlin'ın kullanarak filtreler gerçekleştirebilirsiniz `has` ve `hasLabel` adımları ve bunları birleştirmek kullanarak `and`, `or`, ve `not` daha karmaşık filtreler oluşturmak için:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

Belirli özellikleri kullanarak sorgu sonuçlarındaki proje `values` . adım:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Şu ana kadar yalnızca tüm veritabanındaki iş sorgu işleçleri gördük. İlgili kenarları ve köşeleri gitmek gerektiğinde grafikleri hızlı ve verimli çapraz geçiş işlemleri için. Şimdi tüm arkadaşların Thomas bulun. Biz Gremlin'ın kullanarak bunu `outE` tüm bulmak için adım Thomas silip Gremlin'ın kullanarak bu kenarlarından içinde-köşeleri için çapraz geçiş yapan dışarı kenarları `inV` . adım:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Tüm Thomas' "arkadaş arkadaş", çağırarak bulmak için iki atlama sonraki sorgu gerçekleştirir `outE` ve `inV` iki kez. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Daha karmaşık sorgular derlemek ve döngü kullanarak gerçekleştirmeden, filtre ifadeleri karıştırma dahil olmak üzere Gremlin kullanan güçlü grafik geçişi mantığı uygulamanıza `loop` adım ve uygulama koşullu Gezinti kullanarak `choose` adım. İle yapabilecekleriniz hakkında daha fazla bilgi [Gremlin Destek](gremlin-support.md)!

İşte bu kadar bu Azure Cosmos DB öğretici tamamlandı! 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin:  

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdakileri yaptığınızdan:

> [!div class="checklist"]
> * Bir Azure Cosmos DB hesabı oluşturuldu 
> * Bir grafik veritabanı ve kapsayıcı oluşturuldu
> * Serileştirilmiş köşeleri ve kenarları .NET nesneleri
> * Eklenen köşeleri ve kenarları
> * Sorgulanan Gremlin kullanarak grafiği

Artık daha karmaşık sorgular oluşturabilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)
