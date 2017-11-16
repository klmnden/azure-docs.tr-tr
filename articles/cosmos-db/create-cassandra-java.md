---
title: "Hızlı Başlangıç: Cassandra API'si Java - Azure Cosmos DB ile | Microsoft Docs"
description: "Bu hızlı başlangıç Azure Cosmos DB Cassandra API Java ve Azure portal ile bir profil uygulaması oluşturmak için nasıl kullanılacağını gösterir"
services: cosmos-db
author: mimig1
manager: jhubbard
documentationcenter: 
ms.assetid: ef611081-0195-4ad8-9b54-b313588e5754
ms.service: cosmos-db
ms.custom: quick start connect, mvc
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: mimig
ms.openlocfilehash: 0aafdade2cbf293cf70f09721102ae8ceaef6303
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-build-a-cassandra-app-with-java-and-azure-cosmos-db"></a>Hızlı Başlangıç: Java ve Azure Cosmos DB ile Cassandra uygulaması oluşturma

Bu hızlı başlangıç Java ve Azure Cosmos DB nasıl kullanılacağını gösterir [Cassandra API](cassandra-introduction.md) örneği github'dan kopyalanarak profili uygulamanızı oluşturmak için. Bu hızlı başlangıç Ayrıca, bir Azure Cosmos DB hesap oluşturulmasını web tabanlı Azure portalını kullanarak açıklanmaktadır.

Azure Cosmos DB Microsoft'un Genel dağıtılmış birden çok model veritabanı hizmetidir. Hızlı bir şekilde oluşturmak ve belge, tablo, anahtar-değer ve grafik veritabanları, her biri genel dağıtım ve yatay ölçek yetenekleri Azure Cosmos DB en yararlı sorgulayabilirsiniz. 

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

    ![Görüntüleme ve bir kullanıcı adı Azure portal, bağlantı dizesi sayfasından kopyalama](./media/create-cassandra-java/keys.png)

2. Kullanın ![Kopyala düğmesini](./media/create-cassandra-java/copy.png) KİŞİ nokta değeri kopyalamak için ekranın sağ taraftaki düğmesi.

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

    Programın exection durdurun ve konsol penceresini kapatmak için CTRL + C tuşlarına basın. 
    
    Sorgu görmek, değiştirmek ve bu yeni verilerle çalışmak için Azure portalında Veri Gezgini artık açabilirsiniz. 

    ![Verileri veri Gezgini'nde görüntüleyin](./media/create-cassandra-java/data-explorer.png)

## <a name="review-slas-in-the-azure-portal"></a>Azure portalında SLA'ları gözden geçirme

[!INCLUDE [cosmosdb-tutorial-review-slas](../../includes/cosmos-db-tutorial-review-slas.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç bir Azure Cosmos DB hesap, Cassandra veritabanı ve Veri Gezgini'ni kullanarak koleksiyonu oluşturmak ve aynı şeyi programlı olarak yapmak için bir uygulamayı çalıştırmak nasıl öğrendiniz. Şimdi, Azure Cosmos DB koleksiyona ek verileri içeri aktarabilirsiniz. 

> [!div class="nextstepaction"]
> [Azure Cosmos Veritabanına Cassandra veri alma](cassandra-import-data.md)
