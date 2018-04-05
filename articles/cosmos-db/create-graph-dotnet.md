---
title: Graph API'sini kullanarak Azure Cosmos DB NET Framework veya Core uygulaması derleme | Microsoft Docs
description: Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET Framework/Core kod örneği sunar
services: cosmos-db
documentationcenter: ''
author: luisbosquez
manager: jhubbard
editor: ''
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/08/2018
ms.author: lbosq
ms.openlocfilehash: 21d8b39032c290d45a3ff772a769b427bded50b1
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2018
---
# <a name="azure-cosmos-db-build-a-net-framework-or-core-application-using-the-graph-api"></a>Azure Cosmos DB: Graph API’si kullanarak bir .NET Framework/Core uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıçta Azure portalı kullanarak bir Azure Cosmos DB [Graph API](graph-introduction.md) hesabını, veritabanını ve grafiğini (kapsayıcı) nasıl oluşturacağınız anlatılmıştır. Daha sonra açık kaynaklı [Gremlin.Net](http://tinkerpop.apache.org/docs/3.2.7/reference/#gremlin-DotNet) sürücüsünü kullanarak bir konsol uygulaması oluşturabilir ve çalıştırabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

Visual Studio 2017 zaten yüklüyse, [Visual Studio 2017 Güncelleştirme 3](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-relnotes) sürümünün yüklü olduğundan emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Graf ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Graph API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizininize gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-gremlindotnet-getting-started.git
    ```

3. Ardından Visual Studio’yu ve çözüm dosyasını açın.

4. Projedeki NuGet paketlerini geri yükleyin. Bu Gremlin.Net sürücüsünü ve Newtonsoft.Json paketini içermelidir.

5. Ayrıca NuGet paket yöneticisi veya [NuGet komut satırı aracını](https://docs.microsoft.com/en-us/nuget/install-nuget-client-tools) kullanarak Gremlin.Net sürüm 3.2.7’yi el ile yükleyebilirsiniz: 

    ```bash
    nuget install Gremlin.Net -Version 3.2.7
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. Program.cs dosyasını açtığınızda Azure Cosmos DB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* Yukarıda oluşturulan hesaba göre bağlantı parametrelerinizi ayarlayın (Satır 19): 

    ```csharp
    private static string hostname = "your-endpoint.gremlin.cosmosdb.azure.com";
    private static int port = 443;
    private static string authKey = "your-authentication-key";
    private static string database = "your-database";
    private static string collection = "your-collection-or-graph";
    ```

* Yürütülecek Gremlin komutları bir Sözlük içinde listelenir (Satır 26):

    ```csharp
    private static Dictionary<string, string> gremlinQueries = new Dictionary<string, string>
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
        { "Traverse",       "g.V('thomas').out('knows').hasLabel('person')" },
        { "Traverse 2x",    "g.V('thomas').out('knows').hasLabel('person').out('knows').hasLabel('person')" },
        { "Loop",           "g.V('thomas').repeat(out()).until(has('id', 'robin')).path()" },
        { "DropEdge",       "g.V('thomas').outE('knows').where(inV().has('id', 'mary')).drop()" },
        { "CountEdges",     "g.E().count()" },
        { "DropVertex",     "g.V('thomas').drop()" },
    };
    ```


* Yukarıda sağlanan parametreleri kullanarak bir `GremlinServer` bağlantı nesnesi oluşturun (Satır 52):

    ```csharp
    var gremlinServer = new GremlinServer(hostname, port, enableSsl: true, 
                                                    username: "/dbs/" + database + "/colls/" + collection, 
                                                    password: authKey);
    ```

* Yeni bir `GremlinClient` nesnesi oluşturun (Satır 56):

    ```csharp
    var gremlinClient = new GremlinClient(gremlinServer);
    ```

* Zaman uyumsuz bir görev içinde `GremlinClient` nesnesini kullanarak her bir Gremlin sorgusunu yürütün (Satır 63). Bu yukarıda tanımlanan sözlükten Gremlin sorgularını okur (Satır 26):

    ```csharp
    var task = gremlinClient.SubmitAsync<dynamic>(query.Value);
    task.Wait();
    ```

* Newtonsoft.Json öğesinden `JsonSerializer` sınıfını kullanarak sonucu alın ve sözlük olarak biçimlendirilen değerleri okuyun:

    ```csharp
    foreach (var result in task.Result)
    {
        // The vertex results are formed as dictionaries with a nested dictionary for their properties
        string output = JsonConvert.SerializeObject(result);
        Console.WriteLine(String.Format("\tResult:\n\t{0}", output));
    }
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), **Anahtarlar**’a tıklayın. 

    URI değerinin ilk parçasını kopyalayın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar sayfası](./media/create-graph-dotnet/keys.png)

2. Program.cs içinde değeri satır 19’daki `hostname` değişkeninde `your-endpoint` üzerine yapıştırın. 

    `"private static string hostname = "your-endpoint.gremlin.cosmosdb.azure.com";`

    Uç nokta değeri şimdi şöyle görünmelidir:

    `"private static string hostname = "testgraphacct.gremlin.cosmosdb.azure.com";`

3. Portaldan **BİRİNCİL ANAHTAR** değerinizi kopyalayın ve satır 21’de `"your-authentication-key"` yer tutucusunu değiştirerek `authkey` değişkeni içinde yapıştırın. 

    `private static string authKey = "your-authentication-key";`

4. Yukarıda oluşturulan veritabanının bilgilerini kullanarak, veritabanı adını satır 22’de `database` değişkeni içinde yapıştırın. 

    `private static string database = "your-database";`

5. Benzer şekilde, yukarıda oluşturulan koleksiyonun bilgilerini kullanarak, koleksiyonu (aynı zamanda grafik adı) satır 23’teki `collection` değişkeninin içinde yapıştırın. 

    `private static string collection = "your-collection-or-graph";`

6. Program.cs dosyasını kaydedin. 

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın. Uygulama hem Gremlin sorgu komutlarını hem de konsol sonuçlarını yazdırır.

   Konsol penceresinde grafiğe eklenmekte olan kenarlar ve köşeler gösterilir. Betik tamamlandığında, ENTER tuşuna basarak konsol penceresini kapatın.

## <a name="browse-using-the-data-explorer"></a>Veri Gezgini'ni kullanarak göz atma

Şimdi Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinize göz atıp sorgu gönderebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Graflar bölmesinde görüntülenir. Veritabanını ve koleksiyon düğümlerini genişletip **Graf**’a tıklayın.

2. Graftaki tüm köşeleri görüntülemek üzere varsayılan sorguyu kullanmak için **Filtre Uygula** düğmesine tıklayın. Örnek uygulama tarafından oluşturulan veriler Grafikler bölmesinde görüntülenir.

    Grafiği yakınlaştırıp uzaklaştırabilir, grafik görüntüleme alanını genişletebilir, başka köşeler ekleyebilir ve görüntüleme alanında köşeleri taşıyabilirsiniz.

    ![Azure portalındaki Veri Gezgini'nde grafiği görüntüleme](./media/create-graph-dotnet/graph-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak grafik oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Artık daha karmaşık sorgular oluşturabilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)

