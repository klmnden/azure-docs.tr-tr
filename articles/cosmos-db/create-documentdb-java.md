---
title: "Java ile Azure Cosmos DB belge veritabanı oluşturma | Microsoft Docs | Microsoft Docs'"
description: "Azure Cosmos DB DocumentDB API'sine bağlanmak ve sorgu göndermek için kullanabileceğiniz bir Java kodu örneği sunar"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 89ea62bb-c620-46d5-baa0-eefd9888557c
ms.service: cosmos-db
ms.custom: quick start connect, mvc, devcenter
ms.workload: 
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: quickstart
ms.date: 11/20/2017
ms.author: mimig
ms.openlocfilehash: b36de6bce597569b4e1eaa615860acdf28dfa798
ms.sourcegitcommit: 4ea06f52af0a8799561125497f2c2d28db7818e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2017
---
# <a name="azure-cosmos-db-create-a-document-database-using-java-and-the-azure-portal"></a>Azure Cosmos DB: Java ve Azure portalını kullanarak bir belge veritabanı oluşturma

Azure Cosmos DB, Microsoft'un genel olarak dağıtılmış çok modelli veritabanı hizmetidir. Azure Cosmos DB kullanarak hızlı bir şekilde oluşturmak ve yönetilen belgesi, tablo ve grafik veritabanları sorgu.

Bu hızlı başlangıç Azure Cosmos DB için Azure portal araçlarını kullanarak bir belge veritabanı oluşturur. Bu hızlı başlangıçta ayrıca bir Java konsol uygulamasını [DocumentDB Java API](documentdb-sdk-java.md) kullanarak nasıl hızlı bir şekilde oluşturabileceğiniz gösterilmektedir. Bu hızlı başlangıçtaki yönergeler Java çalıştırabilen tüm işletim sistemlerinde izlenebilir. Bu hızlı başlangıç tamamlayarak oluşturma ve belge veritabanı kaynaklarında kullanıcı Arabirimi veya programlı olarak tercihinizi hangisi değiştirme hakkında bilgi sahibi olması.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Buna ek olarak: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Bir [Maven](http://maven.apache.org/) ikili arşivi [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir SQL (DocumentDB) veritabanı hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

Şimdi Veri Gezgini'ni kullanarak yeni koleksiyonunuza veri ekleyebilirsiniz.

1. Genişletme **öğeleri** koleksiyonu tıklatın **belgeleri** > **yeni belge**.

   ![Azure portalındaki Veri Gezgini'nde yeni belge oluşturma](./media/create-documentdb-java/azure-cosmosdb-data-explorer-new-document.png)
  
2. Şimdi şu yapıda koleksiyonuna bir belge ekleyin ve tıklatın **kaydetmek**.

     ```json
     {
         "id": "1",
         "category": "personal",
         "name": "groceries",
         "description": "Pick up apples and strawberries.",
         "isComplete": false
     }
     ```

    ![Azure portalında JSON verilerini kopyalayın ve Veri Gezgini'ne kaydedin](./media/create-documentdb-java/azure-cosmosdb-data-explorer-save-document.png)

3.  Oluşturma ve buradan bir daha fazla belgeyi kaydetmek `id` 2 ve gördüğünüz gibi diğer özellikleri uygun Değiştir. Azure Cosmos DB, verilerinizin bir şemaya uygun olmasını şart koşmadığı için yeni belgelerinizin yapısını istediğiniz şekilde oluşturabilirsiniz.

## <a name="query-your-data"></a>Verilerinizi sorgulama

Şimdi almak ve verilerinize filtre uygulamak için veri Explorer'da sorguları kullanabilirsiniz.

1. Varsayılan olarak, sorguyu kümesine bkz `SELECT * FROM c`. Bu varsayılan sorguyu alır ve koleksiyondaki tüm belgelerini görüntüler. 

    ![Varsayılan sorgu veri Explorer'da ' seçin * c '](./media/create-documentdb-java/azure-cosmosdb-data-explorer-query.png)

2. Tıklayarak sorguyu değiştirin **filtresi Düzenle** düğmesi, ekleme `ORDER BY c._ts DESC` sorgu koşul kutusunu tıklatıp, ardından için **Filtre Uygula**.

    ![ORDER BY c._ts DESC ekleme ve Filtre Uygula'ı tıklatarak varsayılan sorguyu değiştirin](./media/create-documentdb-java/azure-cosmosdb-data-explorer-edit-query.png)

Bu sorgu listeleri artık ikinci belgeyi kendi zaman damgası göre azalan sırayla düzenleyin belgeleri ilk listelenen değiştirdi. SQL sözdizimi hakkında bilginiz varsa, herhangi bir desteklenen yazabilirsiniz [SQL sorguları](documentdb-sql-query.md) bu kutuya. 

Veri Gezgini bizim işlerinde tamamlanan. Şu kod ile birlikte çalışmaya devam önce ayrıca Veri Gezgini saklı yordamlar, UDF'ler ve Tetikleyicileri yanı sıra sunucu tarafı iş mantığı gerçekleştirmek için işleme ölçeğini oluşturmak için kullanabileceğiniz unutmayın. Veri Gezgini, API'lerdeki tüm yerleşik programlı veri erişimini açığa çıkarır ancak Azure portalındaki verilerinize kolayca erişmenizi sağlar.

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub'dan bir DocumentDB API uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Bir komut istemi açın, git-samples adlı yeni bir klasör oluşturun ve sonra komut istemini kapatın.

    ```bash
    md "C:\git-samples"
    ```

2. Git bash gibi bir git terminal penceresi açın ve kullanmak `cd` örnek uygulamayı yüklemek için yeni klasör olarak değiştirmek için komutu. 

    ```bash
    cd "C:\git-samples"
    ```

3. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulaması bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynakları kodda nasıl oluşturulduğunu öğrenmek isterseniz, aşağıdaki kod parçacıkları gözden geçirebilirsiniz. Kod parçacıkları tüm gelen alınır `Program.java` C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted klasöründe yüklü dosya. Aksi takdirde, atlayabilirsiniz [bağlantı dizenizi güncelleştirme](#update-your-connection-string). 

* `DocumentClient`başlatma. [DocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client) Azure Cosmos DB veritabanı hizmeti için istemci tarafı mantıksal temsili sağlar. Bu istemci yapılandırma ve hizmet isteklerini yürütmek için kullanılır.

    ```java
    this.client = new DocumentClient("https://FILLME.documents.azure.com",
            "FILLME", 
            new ConnectionPolicy(),
            ConsistencyLevel.Session);
    ```

* [Veritabanı](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._database) oluşturma.

    ```java
    Database database = new Database();
    database.setId(databaseName);
    
    this.client.createDatabase(database, null);
    ```

* [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_collection) oluşturma.

    ```java
    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId(collectionName);

    ...

    this.client.createCollection(databaseLink, collectionInfo, requestOptions);
    ```

* Belge oluşturma kullanarak [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.createdocument) yöntemi.

    ```java
    // Any Java object within your code can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    this.client.createDocument(collectionLink, family, new RequestOptions(), true);
    ```

* JSON SQL sorguları kullanarak gerçekleştirilir [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.documentdb._document_client.querydocuments) yöntemi.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    FeedResponse<Document> queryResults = this.client.queryDocuments(
        collectionLink,
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }
    ```    

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, barındırılan veritabanıyla iletişim kurmak uygulamanızı sağlar.

1. İçinde [Azure portal](http://portal.azure.com/), tıklatın **anahtarları**. 

    URI üst değer kopyalamak için ekranın sağ tarafta kopyalama düğmelerini kullanın.

    ![Görüntüleme ve Azure portal, anahtarları sayfasında erişim tuşu kopyalama](./media/create-documentdb-java/keys.png)

2. Açık `Program.java` C:\git-samples\azure-cosmos-db-documentdb-java-getting-started\src\GetStarted klasöründen dosya. 

3. Üzerinden portalından URI değeri yapıştırın `https://FILLME.documents.azure.com` 45 satırındaki.

4. Portalına geri dönün ve aşağıdaki ekran görüntüsünde gösterildiği gibi birincil anahtar değerini kopyalayın. Üzerinden Portalı'ndan birincil anahtar değer Yapıştır `FILLME` 46 satırındaki.

    GetStartedDemo yöntemi şuna benzer görünmelidir: 
    
    ```java
    private void getStartedDemo() throws DocumentClientException, IOException {
        this.client = new DocumentClient("https://youraccountname.documents.azure.com:443/",
                "your-primary-key...RJhQrqQ5QQ==", 
                new ConnectionPolicy(),
                ConsistencyLevel.Session);
    ```

5. Program.java dosyasını kaydedin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinde `cd` komutuyla azure-cosmos-db-documentdb-java-getting-started klasörüne ulaşın.

    ```git
    cd "C:\git-samples\azure-cosmos-db-documentdb-java-getting-started"
    ```

2. Git terminal penceresi gerekli Java paketlerini yüklemek için aşağıdaki komutu kullanın.

    ```
    mvn package
    ```

3. Git terminal penceresinde Java uygulaması başlatmak için aşağıdaki komutu kullanın.

    ```
    mvn exec:java -D exec.mainClass=GetStarted.Program
    ```

    Terminal penceresi FamilyDB veritabanı oluşturulmuş bir bildirim görüntüler. 
    
4. Koleksiyonu oluşturmak için bir tuşa basın. 

5. Geçiş Veri Gezgini ve şimdi FamilyDB veritabanı içerdiği görürsünüz.
    
6. Konsol penceresinde belgeleri oluşturabilir ve bir sorguyu gerçekleştirmek için aşağıdaki kod tuşlarına devam edin.
    
    Böylece herhangi bir ücrete tabi olmayan program sonunda hesabınızdan bu uygulama tüm kaynaklar silinir. 

    ![Konsol çıktısı](./media/create-documentdb-java/console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabını ve belge veritabanını oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bu işlemleri programlı bir şekilde yapacak bir uygulamayı çalıştırmayı öğrendiniz. Şimdi, Azure Cosmos DB koleksiyona ek verileri içeri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)


