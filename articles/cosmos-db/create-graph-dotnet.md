---
title: "Grafik API'sini kullanarak Azure Cosmos DB .NET uygulaması derleme | Microsoft Docs"
description: "Azure Cosmos DB’ye bağlanmak ve veritabanını sorgulamak için kullanabileceğiniz bir .NET kod örneği sunar"
services: cosmos-db
documentationcenter: 
author: dennyglee
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/14/2017
ms.author: denlee
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 1794341ed0d4519eef7f065d04ccf86a7e48a4a4
ms.contentlocale: tr-tr
ms.lasthandoff: 07/17/2017

---
# <a name="azure-cosmos-db-build-a-net-application-using-the-graph-api"></a>Azure Cosmos DB: Grafik API'sini kullanarak bir .NET uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak bir Azure Cosmos DB hesabını, veritabanını ve grafiği (kapsayıcı) nasıl oluşturacağınız anlatılmıştır. Bu adımların ardından [Grafik API'si](graph-sdk-dotnet.md) (önizleme) kullanarak bir konsol uygulaması derleyebilir ve çalıştırabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

Henüz Visual Studio 2017’yi yüklemediyseniz, **ücretsiz** [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/)’ı indirip kullanabilirsiniz. Visual Studio kurulumu sırasında **Azure dağıtımını** etkinleştirdiğinizden emin olun.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmos-db-create-dbaccount-graph](../../includes/cosmos-db-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Grafik ekleme

[!INCLUDE [cosmos-db-create-graph](../../includes/cosmos-db-create-graph.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Grafik API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-dotnet-getting-started.git
    ```

3. Ardından çözüm dosyasını Visual Studio'da açın. 

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

1. Azure portalında, Azure Cosmos DB hesabınızın sol gezinti menüsünden **Genel Bakış**'a tıklayın. Sonraki adımda, **Gremlin URI** değerini App.config dosyasına kopyalayacaksınız. 

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-graph-dotnet/gremlin-uri.png)

    **Gremlin URI** değeri boşsa, portaldaki **Anahtarlar** sayfasında bulunan **URI** değerini kullanıp https:// bölümünü çıkararak ve belgeleri grafiklere dönüştürerek bir değer oluşturabilirsiniz. 

2. Visual Studio 2017'de App.config dosyasını açın. 

3. Portaldaki **Gremlin URI** değerinizi kopyalayın ve bunu App.config dosyasındaki Endpoint anahtarının değeri yapın. 

    `<add key="Endpoint" value="FILLME.graphs.azure.com:443" />`

4. Azure portalına geri dönüp sol gezinti menüsünde **Anahtarlar**’a tıklayın, **BİRİNCİL ANAHTAR** değerinizi portaldan kopyalayın ve bunu App.config dosyasındaki AuthKey anahtarının değeri yapın. Sonra değişikliklerinizi kaydedin. 

    `<add key="AuthKey" value="FILLME" />`

    ![Azure portalının Anahtarlar sayfasında birincil anahtarı görüntüleme ve kopyalama](./media/create-graph-dotnet/keys.png)

Bu adımlarla uygulamanıza Azure Cosmos DB ile iletişim kurması için gereken tüm bilgileri eklemiş oldunuz. 

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Visual Studio'nun **Çözüm Gezgini** bölümünde **GraphGetStarted** projesine sağ tıklayın ve ardından **NuGet Paketlerini Yönet**'e tıklayın. 

2. NuGet **Gözat** kutusuna *Microsoft.Azure.Graphs* yazın ve **Ön Sürümü Dahil Et** kutusunu işaretleyin. 

3. Sonuçlardan **Microsoft.Azure.Graphs** kitaplığını yükleyin. Bu işlem Azure Cosmos DB grafik uzantısı kitaplık paketini ve tüm bağımlılıklarını yükler.

4. Uygulamayı çalıştırmak için CTRL+F5 tuşlarına basın.

   Konsol penceresinde grafiğe eklenmekte olan kenarlar ve köşeler gösterilir. Betik tamamlandığında, ENTER tuşuna iki kez basarak konsol penceresini kapatın. 

## <a name="browse-using-the-data-explorer"></a>Veri Gezgini'ni kullanarak göz atma

Şimdi Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinize göz atıp sorgu gönderebilirsiniz.

* Yeni veritabanı, Veri Gezgini'nin Koleksiyonlar bölmesinde görüntülenir. **graphdb**, **graphcoll** öğelerini genişletip **Grafik** öğesine tıklayın.

    Örnek uygulama tarafından oluşturulan veriler Grafikler bölmesinde görüntülenir.

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


