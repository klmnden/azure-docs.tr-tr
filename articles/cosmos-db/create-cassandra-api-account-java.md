---
title: 'Öğretici: Bir Java uygulaması - Azure Cosmos DB kullanarak bir Cassandra API hesabı oluşturma'
description: Bu öğretici, Cassandra API hesabı oluşturma, (bir anahtar alanı olarak da bilinir) bir veritabanı ekleyin ve bir Java uygulaması kullanarak bu hesaba tablo eklemek gösterilmektedir.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/06/2018
ms.custom: seodec18
Customer intent: As a developer, I want to build a Java application to access and manage Azure Cosmos DB resources so that customers can store key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: b6876bf8210d47729ad8e765ccffe709a0fccacc
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62120355"
---
# <a name="tutorial-create-a-cassandra-api-account-in-azure-cosmos-db-by-using-a-java-application-to-store-keyvalue-data"></a>Öğretici: Anahtar/değer veri depolamak için bir Java uygulaması kullanarak Azure Cosmos DB'de Cassandra API hesabı oluşturma

Bir geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalar olabilir. Azure Cosmos DB'de Cassandra API hesabı, anahtar/değer veri depolamak için kullanabilirsiniz. Bu öğreticide, Azure Cosmos DB'de Cassandra API hesabı oluşturma, (bir anahtar alanı olarak da bilinir) bir veritabanı ekleyin ve bir tablo eklemek için bir Java uygulaması kullanmayı açıklar. Java uygulaması kullanan [Java sürücüsü](https://github.com/datastax/java-driver) kullanıcı kimliği, kullanıcı adı ve kullanıcı şehir gibi ayrıntılarını içeren bir kullanıcı veritabanı oluşturmak için.  

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra veritabanı hesabı oluşturma
> * Hesabın bağlantı dizesini alma
> * Bir Maven projesi ve bağımlılıklarını oluşturma
> * Veritabanı ve tablo ekleme
> * Uygulamayı çalıştırma

## <a name="prerequisites"></a>Önkoşullar 

* Azure aboneliğiniz yoksa başlamadan önce  [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)  oluşturun. 

* En son sürümünü almak [Java Development Kit (JDK)](https://aka.ms/azure-jdks). 

* [İndirme](https://maven.apache.org/download.cgi) ve [yükleme](https://maven.apache.org/install.html) [Maven](https://maven.apache.org/) ikili arşivi. 
  - Ubuntu’da Maven’i yüklemek için  `apt-get install maven`  komutunu çalıştırabilirsiniz. 

## <a name="create-a-database-account"></a>Veritabanı hesabı oluşturma 

1.  [Azure portalda](https://portal.azure.com/) oturum açın.  

2. **Kaynak oluştur** > **Veritabanları** > **Azure Cosmos DB** seçeneğini belirleyin. 

3. İçinde **yeni hesabı** bölmesinde, yeni Azure Cosmos hesabının ayarlarını girin. 

   |Ayar   |Önerilen değer  |Açıklama  |
   |---------|---------|---------|
   |Kimlik   |   Benzersiz bir ad girin    | Bu Azure Cosmos hesabını tanımlamak için benzersiz bir ad girin. <br/><br/>Girdiğiniz kimliğe cassandra.cosmosdb.azure.com eklenerek temas noktanız oluşturulacağından benzersiz ancak tanımlanabilir bir kimlik kullanın.         |
   |API    |  Cassandra   |  API, oluşturulacak hesap türünü belirler. <br/> Seçin **Cassandra**, bu makalede, Cassandra sorgu dili (CQL) söz dizimini kullanarak sorgulanabilir bir geniş sütun veritabanı oluşturacaksınız.  |
   |Abonelik    |  Aboneliğiniz        |  Bu Azure Cosmos hesap için kullanmak istediğiniz Azure aboneliğini seçin.        |
   |Kaynak Grubu   | Ad girin    |  Seçin **Yeni Oluştur**ve ardından hesabınız için yeni bir kaynak grubu adı girin. Kolaylık olması için kimliğinizle aynı adı kullanabilirsiniz.    |
   |Konum    |  Kullanıcılarınıza en yakın bölgeyi seçin    |  Azure Cosmos hesabınızın barındırılacağı coğrafi konumu seçin. Bunları verilere en hızlı erişim sağlamak için kullanıcılarınıza en yakın konumu kullanın.    |

   ![Portalla hesap oluşturma](./media/create-cassandra-api-account-java/create-account.png)

4. **Oluştur**’u seçin. <br/>Hesabın oluşturulması birkaç dakika sürer. Kaynak oluşturulduktan sonra gördüğünüz **dağıtım başarılı** portal'ın işlecin sağ tarafındaki bildirim.

## <a name="get-the-connection-details-of-your-account"></a>Hesabınızın bağlantı ayrıntılarını alma  

Azure portalında bağlantı dizesi bilgilerini almak ve Java yapılandırma dosyasına kopyalayın. Bağlantı dizesi, uygulamanızın barındırılan veritabanıyla iletişim kurmasına olanak tanır. 

1. Gelen [Azure portalında](https://portal.azure.com/), Azure Cosmos hesabınıza gidin. 

2.  **Bağlantı Dizesi ** bölmesini açın.  

3. **TEMAS NOKTASI**, **BAĞLANTI NOKTASI**, **KULLANICI ADI** ve **BİRİNCİL PAROLA** değerlerini sonraki adımlarda kullanmak üzere kopyalayın.

## <a name="create-the-project-and-the-dependencies"></a>Projesi ve bağımlılıklarını oluşturma 

Bu makalede kullanan Java örnek proje, Github'da barındırılır. Bu belgedeki adımları çalıştırın veya içinden örneği karşıdan [azure-cosmos-db-cassandra-java-getting-started](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started) depo. 

Dosyaları indirdikten sonra içindeki bağlantı dizesi bilgilerini güncelleştirme `java-examples\src\main\resources\config.properties` dosya ve çalıştırın.  

```java
cassandra_host=<FILLME_with_CONTACT POINT> 
cassandra_port = 10350 
cassandra_username=<FILLME_with_USERNAME> 
cassandra_password=<FILLME_with_PRIMARY PASSWORD> 
```

Sıfırdan örneği oluşturmak için aşağıdaki adımları kullanın: 

1. Terminalden veya komut isteminden, Cassandra-demo adlı yeni bir Maven projesi oluşturun. 

   ```bash
   mvn archetype:generate -DgroupId=com.azure.cosmosdb.cassandra -DartifactId=cassandra-demo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false 
   ```
 
2. `cassandra-demo` klasörünü bulun. Metin düzenleyicisi kullanarak, oluşturulmuş olan `pom.xml` dosyasını açın. 

   Cassandra bağımlılıkları ekleyin ve projeniz tarafından gerekli eklentiler gösterildiği şekilde yapı [pom.xml](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/pom.xml) dosya.  

3. `cassandra-demo\src\main` klasörünün altında `resources` adlı yeni bir klasör oluşturun.  Resources klasörünün altına config.properties ve log4j.properties dosyalarını ekleyin:

   - [Config.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/config.properties) dosya Cassandra API hesabı bağlantı uç noktası ve anahtar değerlerini depolar. 
   
   - [Log4j.properties](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/resources/log4j.properties) dosyası, Cassandra API ile etkileşim kurmak için gereken günlüğe kaydetme düzeyini tanımlar.  

4. Gözat `src/main/java/com/azure/cosmosdb/cassandra/` klasör. Cassandra klasörünün içinde `utils` adlı başka bir klasör oluşturun. Yeni klasörde Cassandra API hesabına bağlanmak için gereken yardımcı program sınıfları depolanır. 

   Kümeyi oluşturmak ve Cassandra oturumlarını açıp kapatmak için [CassandraUtils](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/CassandraUtils.java) sınıfını ekleyin. Küme, Azure Cosmos DB'de Cassandra API hesabı bağlanır ve erişilecek bir oturum döndürür. Config.properties dosyasından bağlantı dizesi bilgisini okumak için [Configurations](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/util/Configurations.java) sınıfını kullanın. 

5. Java örnek, kullanıcı adı, kullanıcı kimliği ve kullanıcı şehir gibi kullanıcı bilgilerini içeren bir veritabanı oluşturur. Main işlevindeki kullanıcı ayrıntılarına erişmek için get ve set yöntemlerini tanımlamanız gerekir.
 
   Oluşturma bir [User.java](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-java-getting-started/blob/master/java-examples/src/main/java/com/azure/cosmosdb/cassandra/User.java) altında sınıfı `src/main/java/com/azure/cosmosdb/cassandra/` get klasörle ve ayarlamanıza yöntemleri. 

## <a name="add-a-database-and-a-table"></a>Veritabanı ve tablo ekleme  

Bu bölümde, bir veritabanı (anahtar) ve bir tablo CQL kullanarak eklemeyi açıklar.

1. `src\main\java\com\azure\cosmosdb\cassandra` klasörünün altında `repository` adlı yeni bir klasör oluşturun. 

2. Oluşturma `UserRepository` Java sınıfı ve aşağıdaki kodu ekleyin: 

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

4. Oluşturma `UserProfile` Java sınıfı. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir: 

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

   Bu kod, proje oluşturduğunuz klasör yoluna dizini (cd) değiştirir. Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd cassandra-demo

   mvn clean install 

   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile 
   ```

   Terminal penceresinde anahtar alanı ve tablonun oluşturulduğuna yönelik bildirimler gösterilir. 
   
2. Şimdi, keyspace'in ve tablonun oluşturulduğunu onaylamak için Azure portalda **Veri Gezgini**'ni açın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Java uygulaması kullanarak Azure Cosmos DB, bir veritabanı ve tablo bir Cassandra API hesabı oluşturma öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Örnek verileri Cassandra API'si tablosuna yükleme](cassandra-api-load-data.md).
