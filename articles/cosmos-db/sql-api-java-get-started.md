---
title: "NoSQL Öğreticisi: SQL API'si için Azure Cosmos DB Java SDK'sı"
description: Azure Cosmos DB için SQL API’sini kullanarak çevrimiçi bir veritabanı ve Java konsol uygulaması oluşturan bir NoSQL öğreticisi. Azure SQL, JSON için bir NoSQL veritabanıdır.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: sngun
ms.openlocfilehash: dbaafca8692b144b0032315c0b40eb02bec0e3ff
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553086"
---
# <a name="nosql-tutorial-build-a-sql-api-java-console-application"></a>NoSQL Öğreticisi: Bir SQL API Java konsol uygulaması oluşturma

> [!div class="op_single_selector"]
> * [.NET](sql-api-get-started.md)
> * [.NET (Önizleme)](sql-api-dotnet-get-started-preview.md)
> * [.NET Core](sql-api-dotnetcore-get-started.md)
> * [.NET core (Önizleme)](sql-api-dotnet-core-get-started-preview.md)
> * [Java](sql-api-java-get-started.md)
> * [Async Java](sql-api-async-java-get-started.md)
> * [Node.js](sql-api-nodejs-get-started.md)
> 

Azure Cosmos DB Java SDK’sı için SQL API’sine yönelik NoSQL öğreticisine hoş geldiniz! Bu öğreticiyi uyguladıktan sonra, Azure Cosmos DB kaynaklarını oluşturan ve sorgulayan bir konsol uygulamasına sahip olacaksınız.

Kapsanan konular:

* Azure Cosmos DB hesabı oluşturma ve hesaba bağlanma
* Visual Studio Çözümünüzü yapılandırma
* Çevrimiçi bir veritabanı oluşturma
* Koleksiyon oluşturma
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* JSON belgeleri oluşturma
* Koleksiyonu sorgulama
* Bir belgeyi değiştirme
* Bir belgeyi silme
* Veritabanını silme

Şimdi başlayalım!

## <a name="prerequisites"></a>Önkoşullar
Aşağıdakilere sahip olduğunuzdan emin olun:

* Etkin bir Azure hesabı. Bir aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) için kaydolabilirsiniz. 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Git](https://git-scm.com/downloads).
* [Java Geliştirme Seti (JDK) 7 +](https://aka.ms/azure-jdks).
* [Maven](https://maven.apache.org/download.cgi).

## <a name="step-1-create-an-azure-cosmos-db-account"></a>1. Adım: Azure Cosmos DB hesabı oluşturma
Bir Azure Cosmos DB hesabı oluşturalım. Kullanmak istediğiniz bir hesap zaten varsa [GitHub projesini kopyalama](#GitClone) adımına atlayabilirsiniz. Azure Cosmos DB Öykünücüsü’nü kullanıyorsanız öykünücünün kurulumunu gerçekleştirmek için [Azure Cosmos DB Öykünücüsü](local-emulator.md) konusundaki adımları izleyin ve [GitHub projesini kopyalama](#GitClone) adımına atlayın.

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

## <a id="GitClone"></a>2. adım: GitHub projesini kopyalama
[Azure Cosmos DB ve Java’yı kullanmaya başlama](https://github.com/Azure-Samples/documentdb-java-getting-started) GitHub deposunu kopyalayarak başlayabilirsiniz. Örneğin, yerel bir dizinden örnek projesini yerele almak için aşağıdaki komutu çalıştırın.

    git clone git@github.com:Azure-Samples/azure-cosmos-db-documentdb-java-getting-started.git

    cd azure-cosmos-db-documentdb-java-getting-started

Dizinde proje için bir `pom.xml` nesnesinin yanı sıra Java kaynak kodunu içeren `src` klasörü vardır. Bu klasör içindeki `Program.java`, Azure Cosmos DB ile belge oluşturma ve bir koleksiyondaki verileri sorgulama gibi basit işlemlerin nasıl yapılacağını gösterir. `pom.xml`, [Maven üzerindeki Azure Cosmos DB Java SDK’sı](https://mvnrepository.com/artifact/com.microsoft.azure/azure-documentdb) bağımlılığını içerir.

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-documentdb</artifactId>
        <version>LATEST</version>
    </dependency>

## <a id="Connect"></a>3. adım: Bir Azure Cosmos DB hesabına bağlanma
Ardından, uç noktanızı ve birincil ana anahtarınızı almak için tekrar [Azure Portal](https://portal.azure.com)’a gidin. Azure Cosmos DB uç noktası ve birincil anahtar, uygulamanızın nereye bağlanacağını anlaması ve Azure Cosmos DB’nin uygulamanızın bağlantısına güvenmesi için gereklidir.

Azure Portal'da Azure Cosmos DB hesabınıza gidin ve ardından **Anahtarlar**’a tıklayın. Portaldaki URI’yi kopyalayın ve Program.java dosyasındaki `https://FILLME.documents.azure.com` içine yapıştırın. Ardından portaldan BİRİNCİL ANAHTARI kopyalayın ve `FILLME` içine yapıştırın.

    this.client = new DocumentClient(
        "https://FILLME.documents.azure.com",
        "FILLME"
        , new ConnectionPolicy(),
        ConsistencyLevel.Session);

![Bir Java konsol uygulaması oluşturmak için NoSQL Öğreticisi tarafından kullanılan Azure portalının ekran görüntüsü. Azure Cosmos DB hesabı dikey penceresinde ANAHTARLAR düğmesi vurgulanmış, ETKİN hub'ı vurgulanmış ve Anahtarlar dikey penceresinde URI, BİRİNCİL ANAHTAR ve İKİNCİL ANAHTAR değerleri vurgulanmış bir Azure Cosmos DB hesabını gösterir][keys]

## <a name="step-4-create-a-database"></a>4. Adım: Veritabanı oluşturma
Azure Cosmos DB [veritabanınız](databases-containers-items.md#azure-cosmos-databases), **DocumentClient** sınıfının [createDatabase](/java/api/com.microsoft.azure.documentdb.documentclient.createdatabase) yöntemi kullanılarak oluşturulabilir. Veritabanı, koleksiyonlar genelinde bölümlenmiş JSON belgesi depolama alanının mantıksal bir kapsayıcısıdır.

    Database database = new Database();
    database.setId("familydb");
    this.client.createDatabase(database, null);

## <a id="CreateColl"></a>5. adım: Koleksiyon oluşturma
> [!WARNING]
> **createCollection**, ayrılmış işleme ile yeni bir koleksiyon oluşturur, bu da ücret ödenmesini gerektirebilir. Daha ayrıntılı bilgi için [fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cosmos-db/) ziyaret edin.
> 
> 

Bir koleksiyon kullanarak oluşturulabilir [createCollection](/java/api/com.microsoft.azure.documentdb.documentclient.createcollection) yöntemi **DocumentClient** sınıfı. Koleksiyon, JSON belgelerinin ve ilişkili JavaScript uygulama mantığının bir kapsayıcısıdır.


    DocumentCollection collectionInfo = new DocumentCollection();
    collectionInfo.setId("familycoll");

    // Azure Cosmos DB collections can be reserved with throughput specified in request units/second. 
    // Here we create a collection with 400 RU/s.
    RequestOptions requestOptions = new RequestOptions();
    requestOptions.setOfferThroughput(400);

    this.client.createCollection("/dbs/familydb", collectionInfo, requestOptions);

## <a id="CreateDoc"></a>6. adım: JSON belgeleri oluşturma
Bir belge kullanarak oluşturulabilir [createDocument](/java/api/com.microsoft.azure.documentdb.documentclient.createdocument) yöntemi **DocumentClient** sınıfı. Belgeler, kullanıcı tanımlı (rastgele) JSON içerikleridir. Şimdi bir veya daha fazla belge ekleyebiliriz. Veritabanınızda depolamak istediğiniz veriler zaten varsa, verileri bir veritabanına içeri aktarmak için Azure Cosmos DB’nin [Veri Geçişi aracını](import-data.md) kullanabilirsiniz.

    // Insert your Java objects as documents 
    Family andersenFamily = new Family();
    andersenFamily.setId("Andersen.1");
    andersenFamily.setLastName("Andersen")

    // More initialization skipped for brevity. You can have nested references
    andersenFamily.setParents(new Parent[] { parent1, parent2 });
    andersenFamily.setDistrict("WA5");
    Address address = new Address();
    address.setCity("Seattle");
    address.setCounty("King");
    address.setState("WA");

    andersenFamily.setAddress(address);
    andersenFamily.setRegistered(true);

    this.client.createDocument("/dbs/familydb/colls/familycoll", family, new RequestOptions(), true);

![Bir Java konsol uygulaması oluşturmak için NoSQL öğreticisi tarafından kullanılan belgeler, hesap, çevrimiçi veritabanı ve koleksiyon arasındaki hiyerarşik ilişkiyi gösteren diyagram](./media/sql-api-get-started/nosql-tutorial-account-database.png)

## <a id="Query"></a>7. adım: Azure Cosmos DB kaynaklarını sorgulama
Azure Cosmos DB, her bir koleksiyonda depolanan JSON belgeleri için [zengin sorguların](how-to-sql-query.md) gerçekleştirilmesini destekler.  Aşağıdaki örnek kodda Azure Cosmos DB içindeki belgelerin SQL söz dizimi ve [queryDocuments](/java/api/com.microsoft.azure.documentdb.documentclient.querydocuments) yöntemi kullanılarak nasıl sorgulanacağı gösterilmektedir.

    FeedResponse<Document> queryResults = this.client.queryDocuments(
        "/dbs/familydb/colls/familycoll",
        "SELECT * FROM Family WHERE Family.lastName = 'Andersen'", 
        null);

    System.out.println("Running SQL query...");
    for (Document family : queryResults.getQueryIterable()) {
        System.out.println(String.format("\tRead %s", family));
    }

## <a id="ReplaceDocument"></a>8. adım: JSON belgesini değiştirme
Azure Cosmos DB, JSON belgelerinin [replaceDocument](/java/api/com.microsoft.azure.documentdb.documentclient.replacedocument) yöntemiyle güncelleştirilmesini destekler.

    // Update a property
    andersenFamily.Children[0].Grade = 6;

    this.client.replaceDocument(
        "/dbs/familydb/colls/familycoll/docs/Andersen.1", 
        andersenFamily,
        null);

## <a id="DeleteDocument"></a>9. adım: JSON belgesini silme
Benzer şekilde, Azure Cosmos DB, JSON belgelerinin [deleteDocument](/java/api/com.microsoft.azure.documentdb.documentclient.deletedocument) yöntemiyle silinmesini destekler.  

    this.client.delete("/dbs/familydb/colls/familycoll/docs/Andersen.1", null);

## <a id="DeleteDatabase"></a>10. adım: Veritabanını silme
Oluşturulan veritabanı silindiğinde, veritabanı ve tüm alt kaynaklar (koleksiyonlar, belgeler vb.) kaldırılır.

    this.client.deleteDatabase("/dbs/familydb", null);

## <a id="Run"></a>11. adım: Çalıştırın, Java konsol uygulaması tümünü bir araya!
Konsoldan uygulamayı çalıştırmak için Maven kullanarak proje klasörüne gidin ve derleyin:
    
    mvn package

`mvn package` komutunu çalıştırdığınızda Maven’dan en yeni Azure Cosmos DB kitaplığı indirilir ve `GetStarted-0.0.1-SNAPSHOT.jar` üretilir. Ardından şu komutla uygulamayı çalıştırın:

    mvn exec:java -D exec.mainClass=GetStarted.Program

Tebrikler! Bu NoSQL öğreticisini tamamladınız ve çalışan bir Java konsol uygulamasına sahipsiniz!

## <a name="next-steps"></a>Sonraki adımlar
* Java web uygulaması öğreticisi ister misiniz? Bkz. [Azure Cosmos DB kullanarak Java ile bir web uygulaması oluşturma](sql-api-java-application.md).
* [Azure Cosmos DB hesabını nasıl izleyebileceğinizi](monitor-accounts.md) öğrenin.
* [Query Playground](https://www.documentdb.com/sql/demo)'daki örnek veri kümelerimizde sorgular çalıştırın.

[keys]: media/sql-api-get-started/nosql-tutorial-keys.png
