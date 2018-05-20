---
title: "Azure Cosmos DB: .NET'te Graph API ile geliştirme | Microsoft Docs"
description: .NET kullanarak Azure Cosmos DB SQL API'si ile geliştirmeyi öğrenin
services: cosmos-db
documentationcenter: ''
author: luisbosquez
manager: kfile
editor: ''
ms.assetid: cc8df0be-672b-493e-95a4-26dd52632261
ms.service: cosmos-db
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: tutorial
ms.date: 01/02/2018
ms.author: lbosq
ms.custom: mvc
ms.openlocfilehash: a442b6c3c8e2b8a781ee54f41a2e0db5b44b7395
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-cosmos-db-develop-with-the-graph-api-in-net"></a>Azure Cosmos DB: .NET’te Graph API ile geliştirme
Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğreticide Azure portalını kullanarak bir Azure Cosmos DB hesabını, bir grafik veritabanını ve kapsayıcıyı oluşturma işlemi gösterilmektedir. Uygulama daha sonra [Graph API](graph-sdk-dotnet.md) kullanarak dört kişilik basit bir sosyal ağ oluşturur, ardından Gremlin kullanarak grafik içinde dolaşıp sorgulama yapar.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Grafik veritabanı ve kapsayıcı oluşturma
> * .NET nesnelerinin köşe ve kenarlarını seri hale getirme
> * Köşe ve kenar ekleme
> * Gremlin kullanarak grafiği sorgulama

## <a name="graphs-in-azure-cosmos-db"></a>Azure Cosmos DB’de grafikler
Azure Cosmos DB’yi kullanarak [Microsoft.Azure.Graphs](graph-sdk-dotnet.md) kitaplığı ile grafikler oluşturabilir, güncelleştirebilir ve sorgulayabilirsiniz. Microsoft.Azure.Graph kitaplığı, Gremlin sorgularını yürütmek için `DocumentClient` sınıfının üstünde tek bir `CreateGremlinQuery<T>` genişletme yöntemi sağlar.

Gremlin, yazma işlemlerini (DML) ve sorgu ile çapraz geçişi destekleyen işlevsel bir programlama dilidir. Bu makalede Gremlin ile çalışmaya başlamanız için birkaç örneği ele aldık. Azure Cosmos DB’de kullanılabilen Gremlin özelliklerine ilişkin ayrıntılı bir kılavuz için bkz. [Gremlin sorguları](gremlin-support.md). 

## <a name="prerequisites"></a>Ön koşullar
Lütfen aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 
    * Alternatif olarak bu öğretici için [yerel öykünücü](local-emulator.md)’yü kullanabilirsiniz.
* [Visual Studio](http://www.visualstudio.com/).

## <a name="create-database-account"></a>Veritabanı hesabı oluşturma

İlk olarak Azure portalında bir Azure Cosmos DB hesabı oluşturalım.  

> [!TIP]
> * Zaten Azure Cosmos DB hesabınız var mı? Öyleyle, [Visual Studio çözümünüzü ayarlama](#SetupVS) adımına atlayın
> * Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için lütfen [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [Visual Studio Çözümünüzü ayarlama](#SetupVS) adımına atlayın. 
>
> 

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a id="SetupVS"></a>Visual Studio çözümünüzü kurma
1. Bilgisayarınızda **Visual Studio**'yu açın.
2. **Dosya** menüsünde **Yeni**'yi seçin ve ardından **Proje**'yi seçin.
3. **Yeni Proje** iletişim kutusunda, **Şablonlar** / **Visual C#** / **Konsol Uygulaması (.NET Framework)** öğesini seçin, projenizi adlandırın ve ardından **Tamam**'a tıklayın.
4. **Çözüm Gezgini**'nde Visual Studio çözümünüzün altındaki yeni konsol uygulamanıza sağ tıklayın ve **NuGet Paketlerini Yönet...** öğesine tıklayın.
5. **NuGet** sekmesinde **Göz At**’a tıklayın, arama kutusuna **Microsoft.Azure.Graphs** yazın ve **Ön sürümleri dahil et** seçeneğini işaretleyin.
6. Sonuçlarda **Microsoft.Azure.Graphs**'ı bulun ve **Yükle**'ye tıklayın.
   
   Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.
   
    `Microsoft.Azure.Graphs` kitaplığı, Gremlin işlemlerini yürütmek için tek bir `CreateGremlinQuery<T>` genişletme yöntemi sağlar. Gremlin, yazma işlemlerini (DML) ve sorgu ile çapraz geçişi destekleyen işlevsel bir programlama dilidir. Bu makalede Gremlin ile çalışmaya başlamanız için birkaç örneği ele aldık. [Gremlin sorguları](gremlin-support.md) bölümünde, Azure Cosmos DB’de kullanılabilen Gremlin özelliklerine ilişkin ayrıntılı bir kılavuz bulunur.

## <a id="add-references"></a>Uygulamanızı bağlama

Bu iki sabiti ve *client* değişkeninizi uygulamanıza ekleyin. 

```csharp
string endpoint = ConfigurationManager.AppSettings["Endpoint"]; 
string authKey = ConfigurationManager.AppSettings["AuthKey"]; 
``` 
Ardından, uç nokta URL’nizi ve birincil anahtarınızı almak için tekrar [Azure portalına](https://portal.azure.com) gidin. Uç nokta URL’si ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir. 

Azure portalında Azure Cosmos DB hesabınıza gidin **Anahtarlar**’a ve sonra da **Okuma-Yazma Anahtarları**'na tıklayın. 

Portaldaki URI’yi kopyalayın ve yukarıdaki uç nokta özelliğinde `Endpoint` üzerine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve yukarıdaki `AuthKey` özelliğine yapıştırın. 

![Bir C# uygulaması oluşturmak için öğretici tarafından kullanılan Azure portalının ekran görüntüsü. Azure Cosmos DB ezinti bölmesinde ANAHTARLAR düğmesi vurgulanmış ve Anahtarlar dikey penceresinde URI ve BİRİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösterir](./media/tutorial-develop-graph-dotnet/keys.png) 
 
## <a id="instantiate"></a>DocumentClient örneği oluşturma 
Sonra yeni bir **DocumentClient** örneği oluşturun.  

```csharp 
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey); 
``` 

## <a id="create-database"></a>Veritabanı oluşturma 

Şimdi, [SQL .NET SDK](sql-api-sdk-dotnet.md)'dan **DocumentClient** sınıfının [CreateDatabaseAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdatabaseasync.aspx) yöntemini veya [CreateDatabaseIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdatabaseifnotexistsasync.aspx) yöntemini kullanarak bir Azure Cosmos DB [veritabanı](sql-api-resources.md#databases) oluşturun.  

```csharp 
Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" }); 
``` 
 
## <a name="create-a-graph"></a>Grafik oluşturma 

Ardından, **DocumentClient** sınıfının [CreateDocumentCollectionAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.client.documentclient.createdocumentcollectionasync.aspx) yöntemini veya [CreateDocumentCollectionIfNotExistsAsync](https://msdn.microsoft.com/library/microsoft.azure.documents.client.documentclient.createdocumentcollectionifnotexistsasync.aspx) yöntemini kullanarak bir grafik kapsayıcısı oluşturun. Koleksiyon, bir grafik varlıkları kapsayıcısıdır. 

```csharp 
DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync( 
    UriFactory.CreateDatabaseUri("graphdb"), 
    new DocumentCollection { Id = "graphcollz" }, 
    new RequestOptions { OfferThroughput = 400 }); 
``` 

## <a id="serializing"></a>.NET nesnelerinin köşe ve kenarlarını seri hale getirme
Azure Cosmos DB; köşeler, kenarlar ve özellikler için bir JSON şeması tanımlayan [GraphSON kablo biçimini](gremlin-support.md) kullanır. Azure Cosmos DB .NET SDK’sı, bağımlılık olarak JSON.NET içerir ve bu durum, GraphSON biçimini kod içinde birlikte çalışabileceğimiz .NET nesneleri halinde serileştirmemize/seri durumdan çıkarmamıza olanak tanır.

Örnek olarak, dört kişilik basit bir sosyal ağ ile çalışalım. `Person` köşeleri oluşturma, aralarına `Knows` ilişkileri ekleme, ardından "arkadaşın arkadaşı" ilişkileri bulmak için grafiği sorgulama ve sonra grafik içinde dolaşma işlemlerine bakacağız. 

`Microsoft.Azure.Graphs.Elements` ad alanı, iyi tanımlanmış .NET nesnelerine GraphSON yanıtlarını seri durumdan çıkarmak için `Vertex`, `Edge`, `Property` ve `VertexProperty` sınıfları sağlar.

## <a name="run-gremlin-using-creategremlinquery"></a>CreateGremlinQuery kullanarak Gremlin çalıştırma
SQL gibi Gremlin de okuma, yazma ve sorgulama işlemlerini destekler. Örneğin, aşağıdaki kod parçacığında köşe ve kenarlar oluşturma, `CreateGremlinQuery<T>` kullanarak bazı örnek sorgular gerçekleştirme ve `ExecuteNextAsync` ile `HasMoreResults` kullanarak bunları zaman uyumsuz olarak yineleme işlemleri gösterilmektedir.

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

## <a name="add-vertices-and-edges"></a>Köşe ve kenar ekleme

Önceki bölümde gösterilen Gremlin deyimlerine daha ayrıntılı olarak bakalım. İlk olarak Gremlin'ın `addV` yöntemini kullanarak birkaç köşe oluşturalım. Örneğin, aşağıdaki kod parçacığı ad, soyadı ve yaş özellikleriyle "Person" türünde bir "Thomas Andersen" köşesi oluşturur.

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

Sonra Gremlin'in `addE` yöntemini kullanarak bu köşeler arasında bazı kenarlar oluşturacağız. 

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

Gremlin’de `properties` adımını kullanarak var olan bir köşeyi güncelleştirebiliriz. Örneklerin geri kalanında `HasMoreResults` ve `ExecuteNextAsync` aracılığıyla sorguyu yürütme çağrısını atlayacağız.

```cs
// Update a vertex
client.CreateGremlinQuery<Vertex>(
    graphCollection, 
    "g.V('thomas').property('age', 45)");
```

Gremlin'ın `drop` adımını kullanarak kenar ve köşeleri bırakabilirsiniz. Köşe ve kenarı silme işlemini gösteren bir kod parçacığı aşağıda verilmiştir. Bir köşe bırakıldığında ilişkili kenarların art arda silineceğini unutmayın.

```cs
// Drop an edge
client.CreateGremlinQuery(graphCollection, "g.E('thomasKnowsRobin').drop()");

// Drop a vertex
client.CreateGremlinQuery(graphCollection, "g.V('robin').drop()");
```

## <a name="query-the-graph"></a>Grafiği sorgulama

Sorguları ve çapraz geçişleri Gremlin kullanarak da gerçekleştirebilirsiniz. Örneğin, aşağıdaki kod parçacığı, graftaki köşe sayısının nasıl hesaplanacağını gösterir:

```cs
// Run a query to count vertices
IDocumentQuery<int> countQuery = client.CreateGremlinQuery<int>(graphCollection, "g.V().count()");
```
Gremlin’in `has` ve `hasLabel` etiketlerini kullanarak filtreler gerçekleştirebilir ve daha karmaşık filtreler derlemek için `and`, `or` ve `not` kullanarak bunları birleştirebilirsiniz:

```cs
// Run a query with filter
IDocumentQuery<Vertex> personsByAge = client.CreateGremlinQuery<Vertex>(
  graphCollection, 
  "g.V().hasLabel('person').has('age', gt(40))");
```

`values` adımını kullanarak sorgu sonuçlarında belirli özellikleri yansıtabilirsiniz:

```cs
// Run a query with projection
IDocumentQuery<string> firstNames = client.CreateGremlinQuery<string>(
  graphCollection, 
  $"g.V().hasLabel('person').values('firstName')");
```

Şu ana kadar yalnızca tüm veritabanlarında çalışan sorgu işleçlerini gördük. Graflar, ilgili kenarlara ve köşelere gitmeniz gerektiğinde çapraz geçiş işlemleri için hızlı ve etkilidir. Şimdi Thomas’ın tüm arkadaşlarını bulalım. Thomas’taki tüm dış kenarları bulmak için Gremlin’in `outE` adımını kullanıp sonra Gremlin’in `inV` adımını kullanarak bu kenarlardan iç köşelere çapraz geçiş yaparak bunu gerçekleştiririz:

```cs
// Run a traversal (find friends of Thomas)
IDocumentQuery<Vertex> friendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person')");
```

Sonraki sorgu, `outE` ve `inV` öğelerini iki kez çağırarak Thomas’ın tüm “arkadaşlarının arkadaşlarını” bulmak için iki atlama gerçekleştirir. 

```cs
// Run a traversal (find friends of friends of Thomas)
IDocumentQuery<Vertex> friendsOfFriendsOfThomas = client.CreateGremlinQuery<Vertex>(
  graphCollection,
  "g.V('thomas').outE('knows').inV().hasLabel('person').outE('knows').inV().hasLabel('person')");
```

Gremlin kullanarak, filtre ifadelerini karıştırma, `loop` adımını kullanarak döngü gerçekleştirme ve `choose` adımını kullanarak koşullu gezinti uygulama gibi güçlü grafik çapraz geçişi mantığı uygulayabilir ve daha karmaşık sorgular derleyebilirsiniz. [Gremlin desteği](gremlin-support.md) ile neler yapabileceğiniz hakkında daha fazla bilgi edinin!

İşte bu kadar; bu Azure Cosmos DB öğreticisini tamamladınız! 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu öğretici tarafından oluşturulan tüm kaynakları silin:  

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdakileri yaptınız:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma 
> * Grafik veritabanı ve kapsayıcı oluşturdunuz
> * .NET nesnelerinin köşe ve kenarlarını seri hale getirdiniz
> * Köşe ve kenar eklediniz
> * Gremlin kullanarak grafiği sorguladınız

Artık daha karmaşık sorgular oluşturabilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)
