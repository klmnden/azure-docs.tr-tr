---
title: Java ile bir Azure Cosmos DB belge veritabanı oluşturma '
description: Azure Cosmos DB SQL API'sine bağlanmak ve sorgu göndermek için kullanabileceğiniz bir Java kodu örneği sunar
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.custom: quick start connect, mvc, devcenter
ms.devlang: java
ms.topic: quickstart
ms.date: 10/24/2018
ms.author: moderakh
ms.openlocfilehash: 65af8a27d864231e6302d813836e2f63803234d2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52844796"
---
# <a name="create-and-manage-resources-of-an-azure-cosmos-db-sql-api-account-using-a-java-application"></a>Java uygulaması kullanarak bir Azure Cosmos DB SQL API'si hesabının kaynaklarını oluşturma ve yönetme

> [!div class="op_single_selector"]
> * [.NET](create-sql-api-dotnet.md)
> * [.NET (Önizleme)](create-sql-api-dotnet-preview.md)
> * [Java](create-sql-api-java.md)
> * [Node.js](create-sql-api-nodejs.md)
> * [Python](create-sql-api-python.md)
> * [Xamarin](create-sql-api-xamarin-dotnet.md)

Bu hızlı başlangıçta Java uygulaması kullanarak bir Azure Cosmos DB [SQL API'si](sql-api-introduction.md) hesabının kaynaklarını oluşturma ve yönetme adımları gösterilmektedir. İlk olarak Azure portalı kullanarak bir Azure Cosmos DB SQL API'si hesabı oluşturacak, ardından [SQL Java SDK'sını](sql-api-sdk-async-java.md) kullanarak bir Java uygulaması oluşturacak ve son adımda Java uygulamasını kullanarak Cosmos DB hesabınıza kaynak ekleyeceksiniz. Bu hızlı başlangıçtaki yönergeler Java çalıştırabilen tüm işletim sistemlerinde izlenebilir. Bu hızlı başlangıcı tamamladıktan sonra, tercihinize bağlı olarak Kullanıcı Arabiriminde veya programlama arabiriminde Cosmos DB veritabanlarını ve koleksiyonlarını oluşturma ve değiştirme hakkında bilgi sahibi olacaksınız.

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)] 
[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Buna ek olarak: 

* [Java Development Kit (JDK) 1.8+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Bir [Maven](http://maven.apache.org/) ikili arşivi [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile bir SQL API hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a name="add-a-collection"></a>Koleksiyon ekleme

[!INCLUDE [cosmos-db-create-collection](../../includes/cosmos-db-create-collection.md)]

<a id="add-sample-data"></a>
## <a name="add-sample-data"></a>Örnek verileri ekleme

[!INCLUDE [cosmos-db-create-sql-api-add-sample-data](../../includes/cosmos-db-create-sql-api-add-sample-data.md)]

## <a name="query-your-data"></a>Verilerinizi sorgulayın

[!INCLUDE [cosmos-db-create-sql-api-query-data](../../includes/cosmos-db-create-sql-api-query-data.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. GitHub'dan bir SQL API'si uygulaması kopyalayalım, bağlantı dizesini ayarlayalım ve uygulamayı çalıştıralım. Verilerle program aracılığıyla çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulamanın bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynaklarının kodda nasıl oluşturulduğunu öğrenmekle ilgileniyorsanız aşağıdaki kod parçacıklarını gözden geçirebilirsiniz. Aksi takdirde, [Uygulamayı çalıştırma](#run-the-app) konusuna atlayabilirsiniz. 

* `AsyncDocumentClient` başlatma. [AsyncDocumentClient](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client), Azure Cosmos DB veritabanı hizmeti için istemci tarafı mantıksal gösterim sağlar. Bu istemci, istekleri hizmete göre yapılandırmak ve çalıştırmak için kullanılır.

    ```java
    client = new AsyncDocumentClient.Builder()
             .withServiceEndpoint(YOUR_COSMOS_DB_ENDPOINT)
             .withMasterKeyOrResourceToken(YOUR_COSMOS_DB_MASTER_KEY)
             .withConnectionPolicy(ConnectionPolicy.GetDefault())
             .withConsistencyLevel(ConsistencyLevel.Eventual)
             .build();
    ```

* [Veritabanı](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._database) oluşturma.

    ```java
    Database databaseDefinition = new Database();
    databaseDefinition.setId(databaseName);
    
    client.createDatabase(databaseDefinition, null)
            .toCompletable()
            .await();
    ```

* [DocumentCollection](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document_collection) oluşturma.

    ```java
    DocumentCollection collectionDefinition = new DocumentCollection();
    collectionDefinition.setId(collectionName);

    //...

    client.createCollection(databaseLink, collectionDefinition, requestOptions)
            .toCompletable()
            .await();
    ```

* [createDocument](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb._document) metodunu kullanarak belge oluşturma.

    ```java
    // Any Java object within your code
    // can be serialized into JSON and written to Azure Cosmos DB
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen");
    // More properties

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    client.createDocument(collectionLink, family, null, true)
            .toCompletable()
            .await();

    ```

* JSON üzerindeki SQL sorguları, [queryDocuments](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments?view=azure-java-stable) metoduyla yapılır.

    ```java
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setPageSize(-1);
    queryOptions.setEnableCrossPartitionQuery(true);
    queryOptions.setMaxDegreeOfParallelism(-1);

    String collectionLink = String.format("/dbs/%s/colls/%s",
            databaseName,
            collectionName);
    Iterator<FeedResponse<Document>> it = client.queryDocuments(
            collectionLink,
            "SELECT * FROM Family WHERE Family.lastName = 'Andersen'",
            queryOptions).toBlocking().getIterator();

    System.out.println("Running SQL query...");
    while (it.hasNext()) {
        FeedResponse<Document> page = it.next();
        System.out.println(
                String.format("\tRead a page of results with %d items",
                        page.getResults().size()));
        for (Document doc : page.getResults()) {
            System.out.println(String.format("\t doc %s", doc));
        }
    }
    ```    

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bu adımda Azure portala dönerek bağlantı dizesi bilgilerinizi alın ve uç nokta bilgileriyle uygulamayı çalıştırın. Bu, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır.


1. git terminal penceresinde `cd` komutuyla örnek kod klasörüne gidin.

    ```bash
    cd azure-cosmos-db-sql-api-async-java-getting-started/azure-cosmosdb-get-started
    ```

2. Git terminal penceresinde aşağıdaki komutu kullanarak gerekli Java paketlerini yükleyin.

    ```bash
    mvn package
    ```

3. git terminal penceresinde aşağıdaki komutu kullanarak Java uygulamasını başlatın (YOUR_COSMOS_DB_HOSTNAME yerine portaldan aldığınız URI değerini, YOUR_COSMOS_DB_MASTER_KEY yerine de portaldan aldığınız birincil anahtar değerini yazın)

    ```bash
    mvn exec:java -DACCOUNT_HOST=YOUR_COSMOS_DB_HOSTNAME -DACCOUNT_KEY=YOUR_COSMOS_DB_MASTER_KEY

    ```

    Terminal penceresinde, FamilyDB veritabanının oluşturulduğunu belirten bir bildirim görüntülenir. 
    
4. Veritabanını oluşturmak için bir tuşa basın ve sonra koleksiyonu oluşturmak için başka bir tuşa basın. 

    Tarayıcınızda Veri Gezgini’ne geri dönerek bir FamilyDB veritabanı ve FamilyCollection koleksiyonu içerdiğini doğrulayın.

5. Konsol penceresine geçin ve birinci belgeyi oluşturmak için bir tuşa ve ikinci belgeyi oluşturmak için başka bir tuşa basın. Daha sonra Veri Gezgini’ne geri dönerek bunları görüntüleyin. 

6. Bir sorgu çalıştırmak ve konsol penceresinde çıktıyı görmek için bir tuşa basın. 

7. Uygulama, oluşturulan kaynakları silmez. [Kaynakları temizlemek](#clean-up-resources) için portala dönün.  Ücret oluşmaması için kaynakları hesabınızdan silin.

    ![Konsol çıktısı](./media/create-sql-api-java/rxjava-console-output.png)


## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Azure Cosmos DB hesabını ve belge veritabanını oluşturmayı, Veri Gezgini'ni kullanarak koleksiyon oluşturmayı ve bu işlemleri programlı bir şekilde yapacak bir uygulamayı çalıştırmayı öğrendiniz. Şimdi Azure Cosmos DB koleksiyonunuza ek veri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos DB hesabınıza veri aktarma](import-data.md)
