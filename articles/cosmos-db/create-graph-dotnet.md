---
title: "Graph API'sini kullanarak Azure Cosmos DB NET Framework veya Core uygulaması derleme | Microsoft Docs"
description: "Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET Framework/Core kod örneği sunar"
services: cosmos-db
documentationcenter: 
author: luisbosquez
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: lbosq
ms.openlocfilehash: 29153180da576f144a3f21718c3044b7b843eafb
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="azure-cosmos-db-build-a-net-framework-or-core-application-using-the-graph-api"></a>Azure Cosmos DB: Graph API’si kullanarak bir .NET Framework/Core uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını, veritabanını ve grafiği (kapsayıcı) nasıl oluşturacağınız anlatılmıştır. Bu adımların ardından [Graph API](graph-sdk-dotnet.md) kullanarak bir konsol uygulaması derleyebilir ve çalıştırabilirsiniz.  

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

Örnek proje .NET Core proje biçimini kullanır ve aşağıdaki çerçeveleri hedeflemek için yapılandırılmıştır:
 - netcoreapp2.0
 - net461

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Ardından Visual Studio’yu ve çözüm dosyasını açın. 

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. Program.cs dosyasını açtığınızda Azure Cosmos DB kaynaklarını bu kod satırlarının oluşturduğunu göreceksiniz. 

* DocumentClient başlatılır. Önizleme sürümünde Azure Cosmos DB istemcisine bir grafik uzantısı API'si ekledik. Azure Cosmos DB istemci ve kaynaklarından ayrılmış bir tek başına grafik istemcisi üzerinde çalışıyoruz.

    ```csharp
    using (DocumentClient client = new DocumentClient(
        new Uri(endpoint),
        authKey,
        new ConnectionPolicy { ConnectionMode = ConnectionMode.Direct, ConnectionProtocol = Protocol.Tcp }))
    ```

* Yeni bir veritabanı oluşturulur.

    ```csharp
    Database database = await client.CreateDatabaseIfNotExistsAsync(new Database { Id = "graphdb" });
    ```

* Yeni bir grafik oluşturulur.

    ```csharp
    DocumentCollection graph = await client.CreateDocumentCollectionIfNotExistsAsync(
        UriFactory.CreateDatabaseUri("graphdb"),
        new DocumentCollection { Id = "graph" },
        new RequestOptions { OfferThroughput = 1000 });
    ```
* Bir dizi Gremlin adımı `CreateGremlinQuery` yöntemi kullanılarak çalıştırılır.

    ```csharp
    // The CreateGremlinQuery method extensions allow you to execute Gremlin queries and iterate
    // results asychronously
    IDocumentQuery<dynamic> query = client.CreateGremlinQuery<dynamic>(graph, "g.V().count()");
    while (query.HasMoreResults)
    {
        foreach (dynamic result in await query.ExecuteNextAsync())
        {
            Console.WriteLine($"\t {JsonConvert.SerializeObject(result)}");
        }
    }

    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), **Anahtarlar**’a tıklayın. 

    URI değerinin ilk parçasını kopyalayın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar sayfası](./media/create-graph-dotnet/keys.png)

2. Visual Studio 2017'de, appsettings.json dosyasını açın ve değeri `endpoint` içinde `FILLME` üzerine yapıştırın. 

    `"endpoint": "https://FILLME.documents.azure.com:443/",`

    Uç nokta değeri şimdi şöyle görünmelidir:

    `"endpoint": "https://testgraphacct.documents.azure.com:443/",`

3. Graf veritabanı hesabınızı 27 Kasım 2017'den önce oluşturduysanız, `endpoint` değerindeki `documents` öğesini `graphs` olarak değiştirin. Graf veritabanı hesabınızı 27 Kasım 2017'da veya bu tarihten sonra oluşturduysanız, `endpoint` değerindeki `documents` öğesini `gremlin.cosmosdb` olarak değiştirin.

    Uç nokta değeri şimdi şöyle görünmelidir:

    `"endpoint": "https://testgraphacct.graphs.azure.com:443/",` veya `"endpoint": "https://testgraphacct.gremlin.cosmosdb.azure.com:443/",`

4. Portaldaki **BİRİNCİL ANAHTAR** değerinizi kopyalayın ve App.config dosyasındaki AuthKey anahtarının değeri yaptıktan sonra değişikliklerinizi kaydedin. 

    `"authkey": "FILLME"`

5. Appsettings.json dosyasını kaydedin. 

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

Uygulamayı çalıştırmadan önce, *Microsoft.Azure.Graphs* paketini son sürüme güncelleştirmeniz önerilir.

1. Visual Studio'nun **Çözüm Gezgini** bölümünde **GraphGetStarted** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet Paket Yöneticisi **Güncelleştirmeler** kutusuna *Microsoft.Azure.Graphs* yazın ve **Ön Sürümü Dahil Et** kutusunu işaretleyin. 

3. Sonuçlardan, **Microsoft.Azure.Graphs** kitaplığını paketin son sürümüne güncelleştirin. Bu işlem Azure Cosmos DB grafik uzantısı kitaplık paketini ve tüm bağımlılıklarını yükler.

    Çözümdeki değişiklikleri gözden geçirme hakkında iletiler alırsanız **Tamam**'a tıklayın. Lisans kabulü hakkında bir ileti alırsanız **Kabul ediyorum**'a tıklayın.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın.

   Konsol penceresinde grafiğe eklenmekte olan kenarlar ve köşeler gösterilir. Betik tamamlandığında, ENTER tuşuna iki kez basarak konsol penceresini kapatın.

## <a name="browse-using-the-data-explorer"></a>Veri Gezgini'ni kullanarak göz atma

Şimdi Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinize göz atıp sorgu gönderebilirsiniz.

1. Yeni veritabanı, Veri Gezgini'nin Graflar bölmesinde görüntülenir. **graphdb**, **graphcollz** öğelerini genişletip **Graf** öğesine tıklayın.

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

