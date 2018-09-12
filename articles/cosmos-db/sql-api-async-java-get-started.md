---
title: Azure Cosmos DB Async Java SDK'sını kullanarak bir Java uygulaması derleme | Microsoft Docs
description: Bu öğreticide Azure Cosmos DB SQL API'sinin Async Java uygulaması kullanarak veri depolama ve verilere erişme amacıyla nasıl kullanılacağı gösterilmektedir.
keywords: nosql öğreticisi, çevrimiçi veritabanı, java konsol uygulaması
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 06/29/2018
ms.author: sngun
ms.openlocfilehash: faa213caf415f98c230af741822e17a511b6fe43
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43696304"
---
# <a name="build-a-java-application-by-using-azure-cosmos-db-async-java-sdk"></a>Azure Cosmos DB Async Java SDK'sını kullanarak bir Java uygulaması derleme 

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> * [Node.js- v2](sql-api-nodejs-get-started-preview.md) 
> 

Azure Cosmos DB global olarak dağıtılmış, çok modelli bir veritabanıdır. Bu öğreticide Azure Cosmos DB SQL API'sinin Async Java uygulaması kullanarak veri depolama ve verilere erişme amacıyla nasıl kullanılacağı gösterilmektedir. 

Kapsanan konular:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Çözümünüzü yapılandırma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama

Şimdi başlayalım!

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Development Kit (JDK) 8+](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
* [Maven](http://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [GitHub projesini kopyalama](#GitClone) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [GitHub projesini kopyalama](#GitClone) adımına atlayın.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>2. Adım: GitHub projesini kopyalama
[Azure Cosmos DB ve Java’yı kullanmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) GitHub deposunu kopyalayarak başlayabilirsiniz. Örneğin, yerel bir dizinden örnek projesini yerele almak için aşağıdaki komutu çalıştırın.

```bash
git clone https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started.git

cd azure-cosmos-db-sql-api-async-java-getting-started
cd azure-cosmosdb-get-started

```
Dizinde proje için bir `pom.xml` nesnesinin yanı sıra Java kaynak kodunu içeren `src/main/java/com/microsoft/azure/cosmosdb/sample` klasörü vardır. Bu klasör içindeki `Main.java`, Azure Cosmos DB ile belge oluşturma ve bir koleksiyondaki verileri sorgulama gibi basit işlemlerin nasıl yapılacağını gösterir. `pom.xml`, [Maven üzerindeki Azure Cosmos DB Java SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb) bağımlılığını içerir.

```xml
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-documentdb</artifactId>
  <version>2.0.0</version>
</dependency>
```

## <a id="Connect"></a>3. Adım: Azure Cosmos DB hesabına bağlanma
Ardından, uç noktanızı ve birincil ana anahtarınızı almak için tekrar [Azure Portal](https://portal.azure.com)’a gidin. Azure Cosmos DB uç noktası ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir. `AccountSettings.java` dosyasında birincil anahtar ve URI değerleri bulunur. 

Azure Portal'da Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın. Portaldan URI ve BİRİNCİL ANAHTAR değerlerini kopyalayıp `AccountSettings.java` dosyasına yapıştırın. 

```java
public class AccountSettings 
{
  // Replace MASTER_KEY and HOST with values from your Azure Cosmos DB account.
    
  // <!--[SuppressMessage("Microsoft.Security", "CS002:SecretInNextLine")]-->
  public static String MASTER_KEY = System.getProperty("ACCOUNT_KEY", 
          StringUtils.defaultString(StringUtils.trimToNull(
          System.getenv().get("ACCOUNT_KEY")), "<Fill your Azure Cosmos DB account key>"));

  public static String HOST = System.getProperty("ACCOUNT_HOST",
           StringUtils.defaultString(StringUtils.trimToNull(
           System.getenv().get("ACCOUNT_HOST")),"<Fill your Azure Cosmos DB URI>"));
}
```

![Bir Java konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan Azure Portal’ın ekran görüntüsü. Azure Cosmos DB hesabı dikey penceresinde ANAHTARLAR düğmesi vurgulanmış, ETKİN hub'ı vurgulanmış ve Anahtarlar dikey penceresinde URI, BİRİNCİL ANAHTAR ve İKİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösterir][keys]

## <a name="step-4-initialize-the-client-object"></a>4. Adım: İstemci nesnesini başlatma
“AccountSettings.java” dosyasında tanımlı ana bilgisayar URI'si ve birincil anahtar değerlerini kullanarak istemci nesnesini başlatın

```java
client = new AsyncDocumentClient.Builder()
         .withServiceEndpoint(AccountSettings.HOST)
         .withMasterKey(AccountSettings.MASTER_KEY)
         .withConnectionPolicy(ConnectionPolicy.GetDefault())
         .withConsistencyLevel(ConsistencyLevel.Session)
         .build();
```

## <a id="CreateDatabase"></a>5. Adım: Veritabanı oluşturma

Azure Cosmos DB [veritabanınız](sql-api-resources.md#databases), DocumentClient sınıfının createDatabaseIfNotExists() yöntemi kullanılarak oluşturulabilir. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

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

## <a id="CreateColl"></a>6. Adım: Koleksiyon oluşturma

> [!WARNING]
> **createCollection**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
> 
> 
Bir koleksiyon, DocumentClient sınıfının createDocumentCollectionIfNotExists() yöntemi kullanılarak oluşturulabilir. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.

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

## <a id="CreateDoc"></a>7. Adım: JSON belgeleri oluşturma

Bir [belge](sql-api-resources.md#documents), DocumentClient sınıfının createDocument metodu kullanılarak oluşturulabilir. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. “src/main/java/com/microsoft/azure/cosmosdb/sample/Families.java” JSON belgeleri ailesini tanımlar 

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

## <a id="Query"></a>8. Adım: Azure Cosmos DB kaynaklarını sorgulama

Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](sql-api-sql-query.md) gerçekleştirilmesini destekler. Aşağıdaki örnek kodda Azure Cosmos DB içindeki belgelerin SQL söz dizimi ve [queryDocuments](/java/api/com.microsoft.azure.cosmosdb.rx._async_document_client.querydocuments) yöntemi kullanılarak nasıl sorgulanacağı gösterilmektedir.

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

## <a id="Run"></a>9. Adım: Java konsol uygulamanızı hep birlikte çalıştırın!
Konsoldan uygulamayı çalıştırmak için Maven kullanarak proje klasörüne gidin ve derleyin:

```bash
mvn package
```

`mvn package` komutunu çalıştırdığınızda Maven’dan en yeni Azure Cosmos DB kitaplığı indirilir ve `GetStarted-0.0.1-SNAPSHOT.jar` üretilir. Ardından şu komutla uygulamayı çalıştırın:

```bash
mvn exec:java -DACCOUNT_HOST=<YOUR_COSMOS_DB_HOSTNAME> -DACCOUNT_KEY= <YOUR_COSMOS_DB_MASTER_KEY>
```
Tebrikler! Bu NoSQL öğreticisini tamamladınız ve çalışan bir Java konsol uygulamasına sahipsiniz!

## <a name="next-steps"></a>Sonraki adımlar
* Java web uygulaması öğreticisi ister misiniz? Bkz. [Azure Cosmos DB kullanarak Java ile bir web uygulaması oluşturma](sql-api-java-application.md).
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
