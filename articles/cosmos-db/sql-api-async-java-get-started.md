---
title: "Öğretici: Bir Azure Cosmos DB SQL API hesabı yönetmek için zaman uyumsuz Java SDK'sı ile bir Java uygulaması derleme"
description: Bu öğreticide, nasıl depolanacağını ve verilere bir SQL API hesabı içinde Azure Cosmos DB'de Async Java uygulaması kullanarak gösterir.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 12/15/2018
ms.author: sngun
Customer intent: As a developer, I want to build a Java application with the Async Java SDK to access and manage Azure Cosmos DB resources so that customers can utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: b46914728c684fe4b28cb1325afb1e0b662522ad
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475820"
---
# <a name="tutorial-build-a-java-app-with-the-async-java-sdk-to-manage-data-stored-in-a-sql-api-account"></a>Öğretici: Bir SQL API hesabı depolanan verileri yönetmek için zaman uyumsuz Java SDK'sı ile bir Java uygulaması derleme

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Geliştirici olarak, NoSQL belge verileri kullanan uygulamalar olabilir. Azure Cosmos DB SQL API hesabı, depolamak ve bu belge verilere erişmek için kullanabilirsiniz. Bu öğreticide, belge veri depolamak ve yönetmek için zaman uyumsuz Java SDK'sı ile bir Java uygulaması oluşturma işlemini göstermektedir. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Oluşturma ve bir Azure Cosmos hesabına bağlanma
> * Çözümünüzü yapılandırma
> * Koleksiyon oluşturma
> * JSON belgeleri oluşturma
> * Koleksiyonu sorgulama

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki kaynaklara sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

* [Git](https://git-scm.com/downloads).

* [Java Development Kit (JDK) 8+](https://aka.ms/azure-jdks).

* [Maven](https://maven.apache.org/download.cgi).

## <a name="create-an-azure-cosmos-db-account"></a>Azure Cosmos DB hesabı oluşturma

Aşağıdaki adımları kullanarak bir Azure Cosmos hesabı oluşturun:

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>GitHub deposunu kopyalayın

GitHub deposunu kopyalayın [Azure Cosmos DB ve Java ile çalışmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started). Örneğin, yerel bir dizinden örnek projeyi yerel olarak almak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started
```

Dizin içeren bir `pom.xml` dosyası ve bir `src/main/java/com/microsoft/azure/cosmosdb/sample` Java kaynak kodunu içeren klasörü dahil olmak üzere `Main.java`. Proje belgeleri oluşturmak ve bir koleksiyondaki verileri sorgulama gibi Azure Cosmos DB ile işlemleri gerçekleştirmek için gereken kodu içerir. `pom.xml` Dosya çubuğunda bir bağımlılık içerir [Azure Cosmos DB Java SDK'sı maven'da](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb).

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>Bir Azure Cosmos hesabına bağlanma

Ardından, uç noktanızı ve birincil ana anahtarınızı almak için tekrar [Azure portala](https://portal.azure.com) gidin. Azure Cosmos DB uç noktası ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir. `AccountSettings.java` dosyasında birincil anahtar ve URI değerleri bulunur. 

Azure portalında Azure Cosmos hesabınıza gidin ve ardından **anahtarları**. Portaldan URI ve BİRİNCİL ANAHTAR değerlerini kopyalayıp `AccountSettings.java` dosyasına yapıştırın. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Portaldan anahtarları alma ekran görüntüsü][keys]

## <a name="initialize-the-client-object"></a>İstemci nesnesini başlatır

Konak URI ve birincil anahtar değerlerini "AccountSettings.java" dosyasında tanımlanan kullanarak istemci nesnesini başlatır.

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>Veritabanı oluşturma

Kullanarak Azure Cosmos DB veritabanınıza oluşturma `createDatabaseIfNotExists()` DocumentClient sınıfının yöntemi. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

```java
private void createDatabaseIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if database " + databaseName + " exists.");

    String databaseLink = String.format("/dbs/%s", databaseName);

    Observable<ResourceResponse<Database>> databaseReadObs = client.readDatabase(databaseLink, null);

    Observable<ResourceResponse<Database>> databaseExistenceObs = databaseReadObs
                .doOnNext(x -> {System.out.println("database " + databaseName + " already exists.");}).onErrorResumeNext(e -> {
   // if the database doesn't already exists
   // readDatabase() will result in 404 error
   if (e instanceof DocumentClientException) 
   {
       DocumentClientException de = (DocumentClientException) e;
       // if database 
       if (de.getStatusCode() == 404) {
       // if the database doesn't exist, create it.
       System.out.println("database " + databaseName + " doesn't existed," + " creating it...");
       Database dbDefinition = new Database();
       dbDefinition.setId(databaseName);
       return client.createDatabase(dbDefinition, null);
     }
     }

    // some unexpected failure in reading database happened.
    // pass the error up.
    System.err.println("Reading database " + databaseName + " failed.");
        return Observable.error(e);     
    });

   // wait for completion
   databaseExistenceObs.toCompletable().await();

   System.out.println("Checking database " + databaseName + " completed!\n");
}
```

## <a id="CreateColl"></a>Koleksiyon oluşturma

Kullanarak bir koleksiyon oluşturabilirsiniz `createDocumentCollectionIfNotExists()` DocumentClient sınıfının yöntemi. Koleksiyon, JSON belgeleri ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

> [!WARNING]
> **createCollection**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
> 
> 

```java
private void createDocumentCollectionIfNotExists() throws Exception 
{
    
    writeToConsoleAndPromptToContinue("Check if collection " + collectionName + " exists.");

    // query for a collection with a given id
    // if it exists nothing else to be done
    // if the collection doesn't exist, create it.

    String databaseLink = String.format("/dbs/%s", databaseName);

    // we know there is only single page of result (empty or with a match)
    client.queryCollections(databaseLink, new SqlQuerySpec("SELECT * FROM r where r.id = @id", 
            new SqlParameterCollection(new SqlParameter("@id", collectionName))), null).single() 
            .flatMap(page -> {
            if (page.getResults().isEmpty()) {
             // if there is no matching collection create the collection.
             DocumentCollection collection = new DocumentCollection();
             collection.setId(collectionName);
             System.out.println("Creating collection " + collectionName);

             return client.createCollection(databaseLink, collection, null);
            } 
            else {
              // collection already exists, nothing else to be done.
              System.out.println("Collection " + collectionName + "already exists");
              return Observable.empty();
            }
      }).toCompletable().await();

 System.out.println("Checking collection " + collectionName + " completed!\n");
    }
```

## <a id="CreateDoc"></a>JSON belgeleri oluşturma

DocumentClient sınıfının createDocument yöntemi kullanarak bir belge oluşturun. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. "Src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java" dosya ailesi JSON belgelerini tanımlar. 

```java
public static Family getJohnsonFamilyDocument() {
     Family andersenFamily = new Family();
     andersenFamily.setId("Johnson" + System.currentTimeMillis());
     andersenFamily.setLastName("Johnson");

      Parent parent1 = new Parent();
      parent1.setFirstName("John");

      Parent parent2 = new Parent();
      parent2.setFirstName("Lili");

      return andersenFamily;
    }
```

## <a id="Query"></a>Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için zengin sorguların gerçekleştirilmesini destekler. Aşağıdaki örnek kod ile SQL söz dizimini kullanarak Azure Cosmos DB belgeleri sorgulama gösterir `queryDocuments` yöntemi.

```java
private void executeSimpleQueryAsyncAndRegisterListenerForResult(CountDownLatch completionLatch) 
{
    // Set some common query options
    FeedOptions queryOptions = new FeedOptions();
    queryOptions.setMaxItemCount(100);
    queryOptions.setEnableCrossPartitionQuery(true);

    String collectionLink = String.format("/dbs/%s/colls/%s", databaseName, collectionName);
    Observable<FeedResponse<Document>> queryObservable = client.queryDocuments(collectionLink,
           "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", queryOptions);

    queryObservable.subscribe(queryResultPage -> {
            System.out.println("Got a page of query result with " +
            queryResultPage.getResults().size() + " document(s)"
            + " and request charge of " + queryResultPage.getRequestCharge());
    },
   // terminal error signal
        e -> {
          e.printStackTrace();
          completionLatch.countDown();
        },

   // terminal completion signal
         () -> {
            completionLatch.countDown();
         });
}
```

## <a id="Run"></a>Java Konsol uygulamanızı çalıştırın

Uygulamayı konsoldan çalıştırmak için proje klasörüne gidin ve Maven kullanarak derleyin:

```bash
mvn package
```

`mvn package` komutunu çalıştırdığınızda Maven’dan en yeni Azure Cosmos DB kitaplığı indirilir ve `GetStarted-0.0.1-SNAPSHOT.jar` üretilir. Ardından şu komutla uygulamayı çalıştırın:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```

Artık bu NoSQL öğreticisini tamamladınız ve çalışan bir Java konsol uygulaması vardır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulan, kaynak grubu, Azure Cosmos hesabı ve tüm ilgili kaynakları silin. Bunu yapmak için select sanal makinenin kaynak grubunu seçin. **Sil**ve ardından silmek için kaynak grubunun adını onaylayın.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB SQL API verileri yönetmek için zaman uyumsuz Java SDK'sı ile bir Java uygulamasının nasıl oluşturulacağını öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [JavaScript SDK’sı ve Azure Cosmos DB ile bir Node.js konsol uygulaması oluşturma](sql-api-nodejs-get-started.md)

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
