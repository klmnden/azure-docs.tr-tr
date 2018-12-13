---
title: Kullanarak bir Java uygulaması - Azure Cosmos DB Cassandra API hesabı oluşturma
description: Bu makalede, Java uygulaması kullanarak Cassandra API'si hesabı, bu hesaba bir veritabanı (keyspace olarak da adlandırılır) ve tablo oluşturma işlemi gösterilir.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
services: cosmos-db
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 0bff57d91a777619b825dacef5988dda010c794b
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138849"
---
# <a name="tutorial-create-an-azure-cosmos-db-cassandra-api-account-by-using-a-java-application"></a>Öğretici: Bir Java uygulaması kullanarak bir Azure Cosmos DB Cassandra API hesabı oluşturma

Bu öğreticide Azure Cosmos DB'de Cassandra API'si hesabı oluşturmak, bir veritabanı (keyspace olarak da adlandırılır) ve tablo eklemek için Java uygulamasının nasıl kullanıldığı açıklanır. Java uygulaması kullanıcı kimliği, kullanıcı adı ve kullanıcı şehri gibi ayrıntıları içeren bir kullanıcı veritabanı oluşturmak için [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır.  

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra veritabanı hesabı oluşturma
> * Hesabın bağlantı dizesini alma
> * Maven projesi ve bağımlılıklarını oluşturma
> * Veritabanı ve tablo ekleme
> * Uygulamayı çalıştırma

## <a name="prerequisites"></a>Önkoşullar 

* Azure aboneliğiniz yoksa başlamadan önce  [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  oluşturun. Alternatif olarak, Azure aboneliği olmadan ve herhangi bir taahhütte bulunmadan  [Azure Cosmos DB’yi ücretsiz olarak deneyebilirsiniz](https://azure.microsoft.com/try/cosmosdb/) . 

* [Java Geliştirme Seti'nin (JDK)](https://aka.ms/azure-jdks) en son sürümünü alın 

* [Maven](https://maven.apache.org/) ikili arşivini [indirin](https://maven.apache.org/download.cgi) ve [yükleyin](https://maven.apache.org/install.html) 
  - Ubuntu’da Maven’i yüklemek için  `apt-get install maven`  komutunu çalıştırabilirsiniz. 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma 

1.  [Azure portalda](https://portal.azure.com/) oturum açın. 

2.   **Kaynak oluştur**  >  **Veritabanları**  >  **Azure Cosmos DB** 'yi seçin.  

3.   **Yeni hesap**  bölmesinde, yeni Azure Cosmos DB hesabının ayarlarını girin. 

   |Ayar   |Önerilen değer  |Açıklama  |
   |---------|---------|---------|
   |Kimlik   |   Benzersiz bir ad girin    | Bu Azure Cosmos DB hesabını tanımlayan benzersiz bir ad girin. <br/><br/>Girdiğiniz kimliğe cassandra.cosmosdb.azure.com eklenerek temas noktanız oluşturulacağından benzersiz ancak tanımlanabilir bir kimlik kullanın.         |
   |API    |  Cassandra   |  API, oluşturulacak hesap türünü belirler. <br/> **Cassandra** 'yı seçin çünkü bu makalede CQL söz dizimi kullanılarak sorgulanabilecek geniş sütunlu bir veritabanı oluşturacaksınız.  |
   |Abonelik    |  Aboneliğiniz        |  Bu Azure Cosmos DB hesabı için kullanmak istediğiniz Azure aboneliğini seçin.        |
   |Kaynak Grubu   | Ad girin    |   **Yeni Oluştur**’u seçin ve ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz.    |
   |Konum    |  Kullanıcılarınıza en yakın bölgeyi seçin    |  Azure Cosmos DB hesabınızın barındırılacağı coğrafi konumu seçin. Verilere en hızlı erişimi sağlamak için kullanıcılarınıza en yakın olan konumu kullanın.    |

   ![Portalla hesap oluşturma](./media/create-cassandra-api-account-java/create-account.png)

4. Ardından  **Oluştur**'u seçin. <br/>Hesabın oluşturulması birkaç dakika sürer. Kaynak oluşturulduktan sonra, portalın sağ köşesinde **Dağıtım başarılı** bildirimini görebilirsiniz.

## <a name="get-the-connection-details-of-your-account"></a>Hesabınızın bağlantı ayrıntılarını alma  

Azure portaldan bağlantı dizesi bilgisini alın ve bunu Java yapılandırma dosyasına kopyalayın. Bağlantı dizesi, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1.  [Azure portalda](https://portal.azure.com/) Cosmos DB hesabınıza gidin. 

2.  **Bağlantı Dizesi ** bölmesini açın.  

3. **TEMAS NOKTASI**, **BAĞLANTI NOKTASI**, **KULLANICI ADI** ve **BİRİNCİL PAROLA** değerlerini sonraki adımlarda kullanmak üzere kopyalayın.

## <a name="create-maven-project-dependencies-and-utility-classes"></a>Maven projesi, bağımlılıklar ve yardımcı program sınıfları oluşturma 

Bu makalede kullandığınız Java örnek projesi GitHub'da barındırılır. Bu projeyi [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started) deposundan indirebilirsiniz. 

Dosyaları indirdikten sonra, `java-examples\src\main\resources\config.properties` dosyasının içindeki bağlantı dizesi bilgilerini güncelleştirin ve çalıştırın.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_port = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Alternatif olarak örneği sıfırdan da oluşturabilirsiniz.  

1. Terminalden veya komut isteminden, Cassandra-demo adlı yeni bir Maven projesi oluşturun. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. `cassandra-demo` klasörünü bulun. Metin düzenleyicisi kullanarak, oluşturulmuş olan `pom.xml` dosyasını açın. 

   [Pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml) dosyasında gösterildiği gibi gerekli Cassandra bağımlılıklarını ekleyin ve eklentileri oluşturun.  

3. `cassandra-demo\src\main` klasörünün altında `resources` adlı yeni bir klasör oluşturun.  Resources klasörünün altına config.properties ve log4j.properties dosyalarını ekleyin:

   - [Config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) dosyasında Azure Cosmos DB Cassandra API'sinin bağlantı uç noktası ve anahtar değerleri depolanır. 
   
   - [Log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) dosyası Cassandra API'siyle etkileşimli çalışırken gereken günlük düzeyini tanımlar.  

4. Sonra, `src/main/java/com/azure/cosmosdb/cassandra/` klasörüne gidin. Cassandra klasörünün içinde `utils` adlı başka bir klasör oluşturun. Yeni klasörde Cassandra API hesabına bağlanmak için gereken yardımcı program sınıfları depolanır. 

   Kümeyi oluşturmak ve Cassandra oturumlarını açıp kapatmak için [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) sınıfını ekleyin. Küme, Azure Cosmos DB Cassandra API’sine bağlanır ve erişilecek bir oturum döndürür. Config.properties dosyasından bağlantı dizesi bilgisini okumak için [Configurations](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) sınıfını kullanın. 

5. Java örneği; kullanıcı adı, kullanıcı kimliği ve kullanıcı şehri gibi kullanıcı bilgilerini içeren bir veritabanı oluşturur. Main işlevindeki kullanıcı ayrıntılarına erişmek için get ve set yöntemlerini tanımlamanız gerekir.
 
   Get ve set yöntemleriyle `src/main/java/com/azure/cosmosdb/cassandra/` klasörünün altında [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) sınıfını oluşturun. 

## <a name="add-a-database-and-a-table"></a>Veritabanı ve tablo ekleme  

Bu bölümde Cassandra Sorgu Dili'ni (CQL) kullanarak veritabanı (keyspace) ve tablo ekleme işlemi açıklanır. Bu komutların CQL söz dizimini öğrenmek için [keyspace oluşturma](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateKeyspace.html) ve [tablo oluşturma](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCreateTable.html#cqlCreateTable) sorgusu söz dizimine bakın. 

1. `src\main\java\com\azure\cosmosdb\cassandra` klasörünün altında `repository` adlı yeni bir klasör oluşturun. 

2. Ardından, `UserRepository` Java sınıfını oluşturun ve bu sınıfa aşağıdaki kodu ekleyin: 

   ```java
   package com.azure.cosmosdb.cassandra.repository; 
   import java.util.List; 
   import com.datastax.driver.core.BoundStatement; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Row; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Create a Cassandra session 
    */ 
   public class UserRepository { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserRepository.class); 
       private Session session; 
       public UserRepository(Session session) { 
           this.session = session; 
       } 
   
       /** 
       * Create keyspace uprofile in cassandra DB 
        */ 
   
       public void createKeyspace() { 
            final String query = "CREATE KEYSPACE IF NOT EXISTS uprofile WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy', 'datacenter1' : 1 }"; 
           session.execute(query); 
           LOGGER.info("Created keyspace 'uprofile'"); 
       } 
   
       /** 
        * Create user table in cassandra DB 
        */ 
   
       public void createTable() { 
           final String query = "CREATE TABLE IF NOT EXISTS uprofile.user (user_id int PRIMARY KEY, user_name text, user_bcity text)"; 
           session.execute(query); 
           LOGGER.info("Created table 'user'"); 
       } 
   } 
   ```

3. `src\main\java\com\azure\cosmosdb\cassandra` klasörünü bulun ve `examples` adlı yeni bir alt klasör oluşturun.

4. Ardından `UserProfile` Java sınıfını oluşturun. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir: 

   ```java
   package com.azure.cosmosdb.cassandra.examples; 
   import java.io.IOException; 
   import com.azure.cosmosdb.cassandra.repository.UserRepository; 
   import com.azure.cosmosdb.cassandra.util.CassandraUtils; 
   import com.datastax.driver.core.PreparedStatement; 
   import com.datastax.driver.core.Session; 
   import org.slf4j.Logger; 
   import org.slf4j.LoggerFactory; 
   
   /** 
    * Example class which will demonstrate following operations on Cassandra Database on CosmosDB 
    * - Create Keyspace 
    * - Create Table 
    * - Insert Rows 
    * - Select all data from a table 
    * - Select a row from a table 
    */ 
   
   public class UserProfile { 
   
       private static final Logger LOGGER = LoggerFactory.getLogger(UserProfile.class); 
       public static void main(String[] s) throws Exception { 
           CassandraUtils utils = new CassandraUtils(); 
           Session cassandraSession = utils.getSession(); 
   
           try { 
               UserRepository repository = new UserRepository(cassandraSession); 
               //Create keyspace in cassandra database 
               repository.createKeyspace(); 
               //Create table in cassandra database 
               repository.createTable(); 
   
           } finally { 
               utils.close(); 
               LOGGER.info("Please delete your table after verifying the presence of the data in portal or from CQL"); 
           } 
       } 
   } 
   ```
 
## <a name="run-the-app"></a>Uygulamayı çalıştırma 

1. Bir komut istemi veya terminal penceresi açın. Aşağıdaki kod bloğunu yapıştırın. 

   Bu kod, dizini projeyi oluşturduğunuz klasör yolu ile değiştirir (cd). Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   Terminal penceresinde anahtar alanı ve tablonun oluşturulduğuna yönelik bildirimler gösterilir. 
   
2. Şimdi, keyspace'in ve tablonun oluşturulduğunu onaylamak için Azure portalda **Veri Gezgini**'ni açın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Java uygulaması kullanarak Azure Cosmos DB Cassandra API'si hesabı, veritabanı ve tablo oluşturmayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Örnek verileri Cassandra API'si tablosuna yükleme](cassandra-api-load-data.md).
