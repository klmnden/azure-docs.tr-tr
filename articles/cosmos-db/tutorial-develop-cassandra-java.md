---
title: "Azure Cosmos DB: Java API Cassandra geliştirme | Microsoft Docs"
description: "Java kullanarak Azure Cosmos veritabanı Cassandra API'si ile geliştirmeyi öğrenin"
services: cosmos-db
documentationcenter: 
author: mimig1
manager: jhubbard
editor: 
tags: 
ms.assetid: 6732d883-835c-481f-98e1-287893530948
ms.service: cosmos-db
ms.devlang: dotnet
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/15/2017
ms.author: mimig
ms.custom: mvc
ms.openlocfilehash: 53987e5863d9fc11b4fa377295d198293819269c
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/14/2017
---
# <a name="azure-cosmosdb-develop-with-the-cassandra-api-in-java"></a>Azure CosmosDB: Java API Cassandra ile geliştirme

Azure Cosmos DB Microsoft'un Genel dağıtılmış birden çok model veritabanı hizmetidir. Bu hizmetle belge, anahtar/değer ve grafik veritabanlarını kolayca oluşturup sorgulayabilir ve tüm bunları yaparken Azure Cosmos DB'nin genel dağıtım ve yatay ölçeklendirme özelliklerinden faydalanabilirsiniz. 

Bu öğretici, Azure portalını kullanarak bir Azure Cosmos DB hesabı oluşturmak ve Cassandra Table(sql-api-partition-data.md#partition-keys) kullanarak oluşturmak gösterilmiştir [Cassandra API](cassandra-introduction.md). Bir tablo oluşturduğunuzda, bir birincil anahtar tanımlayarak, uygulamanızın verilerinizi büyüdükçe harcamadan ölçeklendirmek için hazırlanır. 

Bu öğretici, Cassandra API'sini kullanarak aşağıdaki görevleri içerir:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Keyspace ve tablo birincil bir anahtar ile oluşturma
> * Veri ekleme
> * Verileri sorgulama
> * SLA gözden geçirin

## <a name="prerequisites"></a>Ön koşullar

Azure Cosmos DB Cassandra API Önizleme programına erişim. Erişim için henüz yapmadıysanız uyguladıysanız [şimdi kaydolun](https://aka.ms/cosmosdb-cassandra-signup).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]Alternatif olarak, [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/) gider ve taahhüt bir Azure aboneliği boş.

Buna ek olarak: 

* [Java Development Kit (JDK) 1.7+](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Bir [Maven](http://maven.apache.org/) ikili arşivi [indirin](http://maven.apache.org/download.cgi) ve [yükleyin](http://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma

Bir belge veritabanı oluşturmadan önce Azure Cosmos DB ile Cassandra hesabı oluşturmanız gerekir.

[!INCLUDE [cosmos-db-create-dbaccount-cassandra](../../includes/cosmos-db-create-dbaccount-cassandra.md)]

## <a name="clone-the-sample-application"></a>Örnek uygulamayı kopyalama

Şimdi kod ile çalışmaya geçelim. Şimdi Cassandra uygulama github'dan bağlantı dizesini ayarlamak ve çalıştırın kopyalayın. Verilerle programlı bir şekilde çalışmanın ne kadar kolay olduğunu göreceksiniz. 

1. Git bash gibi bir git terminal penceresi açın ve kullanmak `cd` örnek uygulamayı yüklemek için bir klasör olarak değiştirmek için komutu. 

    ```bash
    cd "C:\git-samples"
    ```

2. Örnek depoyu kopyalamak için aşağıdaki komutu çalıştırın. Bu komut bilgisayarınızda örnek uygulaması bir kopyasını oluşturur.

    ```bash
    git clone https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started.git
    ```

## <a name="review-the-code"></a>Kodu gözden geçirin

Bu adım isteğe bağlıdır. Veritabanı kaynakları kodda nasıl oluşturulduğunu öğrenmek isterseniz, aşağıdaki kod parçacıkları gözden geçirebilirsiniz. Aksi takdirde, atlayabilirsiniz [bağlantı dizenizi güncelleştirme](#update-your-connection-string). Bu parçacıkları tüm src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java alınır.  

* Cassandra konak, bağlantı noktası, kullanıcı adı, parola ve SSL seçeneklerini ayarlayın. Azure Portalı'ndaki bağlantı dizesi sayfasından bağlantı dizesi bilgilerini gelir.

   ```java
   cluster = Cluster.builder().addContactPoint(cassandraHost).withPort(cassandraPort).withCredentials(cassandraUsername, cassandraPassword).withSSL(sslOptions).build();
   ```

* `cluster` Azure Cosmos DB Cassandra API'sine bağlanır ve erişmek için bir oturum döndürür.

    ```java
    return cluster.connect();
    ```

Src/main/java/com/azure/cosmosdb/cassandra/repository/UserRepository.java dosyasından parçacıklarıdır.

* Yeni bir keyspace oluşturun.

    ```java
    public void createKeyspace() {
        final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH replication = {'class': 'SimpleStrategy', 'replication_factor': '3' } ";
        session.execute(query);
        LOGGER.info("Created keyspace 'uprofile'");
    }
    ```

* Yeni bir tablo oluşturun.

   ```java
   public void createTable() {
        final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)";
        session.execute(query);
        LOGGER.info("Created table 'user'");
   }
   ```

* Hazırlanmış deyim nesnesini kullanarak kullanıcı varlıkları ekleyin.

    ```java
    public PreparedStatement prepareInsertStatement() {
        final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
        return session.prepare(insertStatement);
    }

    public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
    }
    ```

* Tüm kullanıcı bilgilerini almak için sorgu.

    ```java
   public void selectAllUsers() {
        final String query = "SELECT * FROM uprofile.user";
        List<Row> rows = session.execute(query).all();

        for (Row row : rows) {
            LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
        }
    }
    ```

* Tek bir kullanıcının bilgilerini almak için sorgu.

    ```java
    public void selectUser(int id) {
        final String query = "SELECT * FROM uprofile.user where user_id = 3";
        Row row = session.execute(query).one();

        LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
    ```

## <a name="update-your-connection-string"></a>Bağlantı dizenizi güncelleştirme

Bu adımda Azure portalına dönerek bağlantı dizesi bilgilerinizi kopyalayıp uygulamaya ekleyin. Bu, barındırılan veritabanıyla iletişim kurmak uygulamanızı sağlar.

1. İçinde [Azure portal](http://portal.azure.com/), tıklatın **bağlantı dizesi**. 

    ![Görüntüleme ve bir kullanıcı adı Azure portal, bağlantı dizesi sayfasından kopyalama](./media/tutorial-develop-cassandra-java/keys.png)

2. Kullanın ![Kopyala düğmesini](./media/tutorial-develop-cassandra-java/copy.png) KİŞİ nokta değeri kopyalamak için ekranın sağ taraftaki düğmesi.

3. Açık `config.properties` C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples\src\main\resources klasöründen dosya. 

3. Üzerinden portalı kişi noktası değerinden Yapıştır `<Cassandra endpoint host>` satırında 2.

    Config.properties 2 kolu benzer görünmelidir 

    `cassandra_host=cosmos-db-quickstarts.documents.azure.com`

3. Portalına geri dönün ve kullanıcı adı değerini kopyalayın. Portal USERNAME değerini aşan `<cassandra endpoint username>` satırında 4.

    Config.properties 4 satırlık benzer görünmelidir 

    `cassandra_username=cosmos-db-quickstart`

4. Portalına geri dönün ve parola değerini kopyalayın. Üzerinden Portalı'ndan parola değeri yapıştırın `<cassandra endpoint password>` satırında 5.

    Config.properties 5 kolu benzer görünmelidir 

    `cassandra_password=2Ggkr662ifxz2Mg...==`

5. Belirli bir SSL sertifikası kullanmak istiyorsanız, satır 6, ardından Değiştir `<SSL key store file location>` SSL sertifikasının konumu ile. Bir değer sağlanmazsa, < JAVA_HOME > konumundaki yüklü/jre/lib/güvenlik/cacerts JDK sertifika kullanılır. 

6. Satır belirli bir SSL sertifikası kullanmak için 6'yı değiştirdiyseniz, satır bu sertifika için parola kullanılacak 7 güncelleştirin. 

7. Config.properties dosyasını kaydedin.

## <a name="run-the-app"></a>Uygulamayı çalıştırma

1. Git terminal penceresinde `cd` azure-cosmosdb-cassandra-java-getting-started\java-examples klasörüne.

    ```git
    cd "C:\git-samples\azure-cosmosdb-cassandra-java-getting-started\java-examples"
    ```

2. Git terminal penceresi cosmosdb cassandra-examples.jar dosyası oluşturmak için aşağıdaki komutu kullanın.

    ```git
    mvn clean install
    ```

3. Git terminal penceresinde Java uygulamasını başlatmak için aşağıdaki komutu çalıştırın.

    ```git
    java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
    ```

    Terminal penceresi keyspace ve tablo oluşturulan bildirimleri görüntüler. Ardından seçer ve tablodaki tüm kullanıcılar döndürür ve çıktısını görüntüler ve sonra bir satır kimliğine göre seçer ve değeri görüntüler.  
    
    Şimdi Veri Gezgini'ne dönüp bu yeni verileri görebilir, sorgulayabilir, değiştirebilir ve onlarla çalışabilirsiniz. 

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç aşağıdakilerin nasıl yapılacağını öğrendiniz:

> [!div class="checklist"]
> * Azure Cosmos DB hesabı oluşturma
> * Keyspace ve tablo birincil bir anahtar ile oluşturma
> * Veri ekleme
> * Verileri sorgulama
> * Aws'de SLA'ları

Şimdi, Azure Cosmos DB koleksiyona ek verileri içeri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos Veritabanına Cassandra veri alma](cassandra-import-data.md)
