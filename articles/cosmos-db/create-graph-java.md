---
title: "Grafik API&quot;sini kullanarak Azure Cosmos DB Java uygulaması derleme | Microsoft Docs"
description: "Gremlin kullanarak Azure Cosmos DB&quot;ye bağlanmak ve içindeki grafik verilerini sorgulamak için kullanabileceğiniz bir Java kodu örneği sunar."
services: cosmosdb
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: daacbabf-1bb5-497f-92db-079910703046
ms.service: cosmosdb
ms.custom: quick start connect
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: arramac
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 8279ffc8dc69f0899ad7b5d3a528393fc2165b77
ms.contentlocale: tr-tr
ms.lasthandoff: 05/10/2017


---
# <a name="azure-cosmos-db-build-a-java-application-using-the-graph-api"></a>Azure Cosmos DB: Grafik API'sini kullanarak bir Java uygulaması derleme

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu hızlı başlangıç belgesinde Azure portalı kullanarak Grafik API'si (önizleme) için bir Azure Cosmos DB hesabını, veritabanını ve grafiğini nasıl oluşturacağınız anlatılmıştır. Bu adımların ardından OSS [Gremlin Java](https://mvnrepository.com/artifact/com.tinkerpop.gremlin/gremlin-java) sürücüsünü kullanarak bir konsol uygulaması derleyebilir ve çalıştırabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* Bu örneği çalıştırmadan önce aşağıdaki önkoşullara sahip olmanız gerekir:
   * JDK 1.7+ (JDK yoksa `apt-get install default-jdk` komutunu çalıştırın)
   * Maven (Maven yoksa `apt-get install maven` komutunu çalıştırın)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

[!INCLUDE [cosmosdb-create-dbaccount-graph](../../includes/cosmosdb-create-dbaccount-graph.md)]

## <a name="add-a-graph"></a>Grafik ekleme

[!INCLUDE [cosmosdb-create-graph](../../includes/cosmosdb-create-graph.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi GitHub'dan bir Grafik API'si (önizleme) uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu görüyorsunuz. 

1. Git bash gibi bir git terminal penceresi açın ve `cd` ile çalışma dizinine gidin.  

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. 

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-graph-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Uygulamada gerçekleşen işlemleri hızlıca gözden geçirelim. `Program.java` dosyasını açtığınızda bu kod satırlarıyla karşılaşacaksınız. 

* Gremlin `Client`, `src/remote-secure.yaml` içinde bulunan ve daha önce ayarladığınız yapılandırmadan başlatılır.

    ```java
    Cluster cluster = Cluster.build(new File("src/remote.yaml")).create();
    
    Client client = cluster.connect();
    ```

* Bir dizi Gremlin adımı `client.submit` yöntemi kullanılarak çalıştırılır.

    ```java
    ResultSet results = client.submit("g.V()");

    CompletableFuture<List<Result>> completableFutureResults = results.all();
    List<Result> resultList = completableFutureResults.get();

    for (Result result : resultList) {
        System.out.println(result.toString());
    }
    ```
## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin.

1. [Azure portalında](http://portal.azure.com/), Azure Cosmos DB hesabınızın sol taraftaki gezinti menüsünden **Anahtarlar**'a ve ardından **Okuma/Yazma Anahtarları**'na tıklayın. Ekranın sağ tarafındaki kopyalama düğmelerini kullanarak URI ve Birincil Anahtar değerlerini kopyalayarak sonraki adımda `Program.java` dosyasına yapıştırın.

    ![Azure portalında erişim anahtarı görüntüleme ve kopyalama, Anahtarlar dikey penceresi](./media/create-documentdb-dotnet/keys.png)

2. `src/remote-secure.yaml` dosyasını açın. 

3. `src/remote-secure.yaml` dosyasına *ana bilgisayar*, *bağlantı noktası*, *kullanıcı adı*, *parola*, *bağlantı havuzu*, ve *serileştirici* değerlerini girin:

    Ayar|Önerilen değer|Açıklama
    ---|---|---
    Ana bilgisayarlar|***.graphs.azure.com|Grafik hizmeti URI'si değeriniz, Azure portalından alabilirsiniz
    Bağlantı noktası|443|443 olarak ayarlayın
    Kullanıcı adı|*Kullanıcı adınız*|`/dbs/<db>/colls/<coll>` formunun kaynağı.
    Parola|*Birincil ana anahtarınız*|Azure Cosmos DB için birincil ana anahtarınız
    Bağlantı havuzu|{enableSsl: true}|SSL için bağlantı havuzu ayarınız
    Serileştirici|{ className:org.apache.tinkerpop.gremlin.<br>driver.ser.GraphSONMessageSerializerV1d0,<br> config: { serializeResultToString: true }}|Bu değere ayarlayın

## <a name="run-the-console-app"></a>Konsol uygulamasını çalıştırma

1. Gerekli npm modüllerini yüklemek için bir terminalde `mvn package` komutunu çalıştırın

2. Java uygulamanızı başlatmak için bir terminalde `mvn exec:java -D exec.mainClass=GetStarted.Program` komutunu çalıştırın.

Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="browse-using-the-data-explorer"></a>Veri Gezgini'ni kullanarak göz atma

Şimdi Azure portalındaki Veri Gezgini'ne dönerek yeni grafik verilerinize göz atıp sorgu gönderebilirsiniz.

* Yeni veritabanı, Veri Gezgini'nin Koleksiyonlar bölmesinde görüntülenir. **graphdb**, **graphcoll** öğelerini genişletip **Grafik** öğesine tıklayın.

    Örnek uygulama tarafından oluşturulan veriler Grafikler bölmesinde görüntülenir.

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmosdb-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz aşağıdaki adımları kullanarak Azure portalında bu hızlı başlangıç tarafından oluşturulan tüm kaynakları silin: 

1. Azure portalında sol taraftaki menüden, **Kaynak grupları**'na ve ardından oluşturduğunuz kaynağın adına tıklayın. 
2. Kaynak grubu sayfanızda, **Sil**'e tıklayın, metin kutusuna silinecek kaynağın adını yazın ve ardından **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabı oluşturmayı, Veri Gezgini'ni kullanarak grafik oluşturmayı ve bir uygulamayı çalıştırmayı öğrendiniz. Artık daha karmaşık sorgular derleyebilir ve Gremlin kullanarak güçlü grafik geçişi mantığını kullanabilirsiniz. 

> [!div class="nextstepaction"]
> [Gremlin kullanarak sorgulama](tutorial-query-graph.md)


