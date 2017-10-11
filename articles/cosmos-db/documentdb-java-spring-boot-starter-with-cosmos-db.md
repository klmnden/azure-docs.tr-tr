---
title: "Yay önyükleme Starter bir Azure Cosmos DB DocumentDB API'si ile kullanma"
description: "Yay Önyükleme Başlatıcısı Azure Cosmos DB DocumentDB API'si ile oluşturulan bir uygulama yapılandırma konusunda bilgi edinin."
services: cosmos-db
documentationcenter: java
author: rmcmurray
manager: cfowler
editor: 
keywords: "İlkbahar, yay önyükleme Starter Cosmos DB"
ms.assetid: 
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 08/08/2017
ms.author: robmcm;yungez;kevinzha
ms.openlocfilehash: 273cc750857c5e466882060a38ac0f3475811e98
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-use-the-spring-boot-starter-with-azure-cosmos-db-documentdb-api"></a>Yay önyükleme Starter Azure Cosmos DB DocumentDB API'si ile kullanma

## <a name="overview"></a>Genel Bakış

 **[Yay Framework]**  Java geliştiriciler kuruluş düzeyinde uygulamalar oluşturmanıza yardımcı olan bir açık kaynaklı bir çözümdür. Yerleşik daha popüler projelerden biri üzerinde üst o platformudur [yay önyükleme], tek başına Java uygulamaları oluşturmak için basitleştirilmiş bir yaklaşım sağlar. Yay önyükleme ile çalışmaya başlama geliştiricilerin yardımcı olmak için birkaç örnek yay önyükleme paketleri kullanılabilir <https://github.com/spring-guides/>. Temel yay önyükleme projeleri, listeden seçerek ek olarak  **[yay Initializr]**  özel yay önyükleme uygulamalar oluşturmaya başlamak geliştiricilere yardımcı olur.

Azure Cosmos DB DocumentDB, MongoDB, grafik ve tablo API'leri gibi standart API'leri çeşitli kullanarak verileri geliştiricilerin izin veren bir genel dağıtılmış veritabanı hizmetidir. Microsoft'un yay önyükleme Starter kolayca DocumentDB API'lerini kullanarak Azure Cosmos DB ile tümleştirmek yay önyükleme uygulamaları kullanmak geliştiricilere sağlar.

Bu makale bir Azure Cosmos Azure Portalı'nı kullanarak, daha sonra kullanarak DB oluşturmayı gösterir **yay Initializr** özel java uygulaması oluşturmak ve özel uygulamanızı yay önyükleme Starter işlevselliği eklemek için verileri depolamak ve DocumentDB API'sini kullanarak Azure Cosmos Veritabanından veri alın.

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları için aşağıdaki önkoşullar gereklidir:

* Bir Azure aboneliği; bir Azure aboneliği zaten sahip değilseniz, etkinleştirebilir, [MSDN abone Avantajlarınızı] veya kaydolun bir [ücretsiz Azure hesabı].

* A [Java Geliştirme Seti (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/), 1,7 veya sonraki bir sürümü.

* [Apache Maven](http://maven.apache.org/), sürüm 3.0 veya üstü.

## <a name="create-an-azure-cosmos-db-by-using-the-azure-portal"></a>Azure portalı kullanarak bir Azure Cosmos DB oluştur

1. Azure portalında göz <https://portal.azure.com/> tıklatıp **+ yeni**.

   ![Azure portalına][AZ01]

1. Tıklatın **veritabanları**ve ardından **Azure Cosmos DB**.

   ![Azure portalına][AZ02]

1. Üzerinde **Azure Cosmos DB** sayfasında, aşağıdaki bilgileri girin:

   * Benzersiz bir girin **kimliği**, veritabanınız için URI olarak kullanacağı. Örneğin: *wingtiptoysdata.documents.azure.com*.
   * Seçin **SQL (belge DB)** API'si.
   * Seçin **abonelik** veritabanınız için kullanmak istediğiniz.
   * Yeni bir oluşturulup oluşturulmayacağını belirtin **kaynak grubu** , veritabanı veya varolan bir kaynak grubu seçin.
   * Belirtin **konumu** veritabanınız için.
   
   Bu seçenek belirtildiğinde tıklatın **oluşturma** veritabanınızı oluşturmak için.

   ![Azure portalına][AZ03]

1. Veritabanınızı oluşturduğunuzda, Azure üzerinde listelenir **Pano**altında da olarak **tüm kaynakları** ve **Azure Cosmos DB** sayfaları. Veritabanınızı önbelleğiniz için Özellikler sayfasını açmak için bu konumların hiçbirinde tıklatabilirsiniz.

   ![Azure portalına][AZ04]

1. Özellikler sayfasında Veritabanınızı görüntülenen için tıklattığınızda **erişim anahtarları** ve veritabanınız için URI ve erişim anahtarlarınızı kopyalayın; yay önyükleme uygulamanızda bu değerleri kullanır.

   ![Azure portalına][AZ05]

## <a name="create-a-simple-spring-boot-application-with-the-spring-initializr"></a>Yay Initializr ile basit bir yay önyükleme uygulaması oluşturma

1. Gözat <https://start.spring.io/>.

1. Oluşturmak istediğiniz belirtin bir **Maven** ile proje **Java**, girin **grup** ve **yapı** adları, uygulamanız için ve düğmesini tıklatıp **proje oluştur**.

   ![Basic yay Initializr seçenekleri][SI01]

   > [!NOTE]
   >
   > Yay Initializr kullanır **grup** ve **yapı** paket adı; oluşturmak için adlarını, örneğin: *com.example.wintiptoys*.
   >

1. İstendiğinde, yerel bilgisayarınızda bir yola projenizi indirin.

   ![Özel yay önyükleme projenizi indirin][SI02]

1. Yerel sisteminizde dosyaları ayıkladıktan sonra basit yay önyükleme uygulamanızı düzenlemek için hazır olacaktır.

   ![Özel yay önyükleme proje dosyaları][SI03]

## <a name="configure-your-spring-boot-app-to-use-the-azure-spring-boot-starter"></a>Yay önyükleme uygulamanızı Azure yay önyükleme Starter kullanacak şekilde yapılandırma

1. Bulun *pom.xml* , uygulamanızın; dizindeki dosyayı örneğin:

   `C:\SpringBoot\wingtiptoys\pom.xml`

   -veya-

   `/users/example/home/wingtiptoys/pom.xml`

   ![Pom.xml dosyasını bulun][PM01]

1. Açık *pom.xml* dosyasını bir metin düzenleyicisinde açın ve aşağıdaki satırları listesine eklemek `<dependencies>`:

   ```xml
   <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-documentdb-spring-boot-starter</artifactId>
      <version>0.1.4</version>
   </dependency>
   ```

   ![Pom.xml dosyasını düzenleme][PM02]

1. Kaydet ve Kapat *pom.xml* dosya.

## <a name="configure-your-spring-boot-app-to-use-your-azure-cosmos-db"></a>Yay önyükleme uygulamanızı Azure Cosmos DB kullanacak şekilde yapılandırma

1. Bulun *application.properties* dosyasını *kaynakları* , uygulamanızın dizin; örneğin:

   `C:\SpringBoot\wingtiptoys\src\main\resources\application.properties`

   -veya-

   `/users/example/home/wingtiptoys/src/main/resources/application.properties`

   ![Application.properties dosyasını bulun][RE01]

1. Açık *application.properties* dosyasını bir metin düzenleyicisinde dosyasına aşağıdaki satırları ekleyin ve veritabanınız için uygun özelliklere sahip örnek değerleri değiştirin:

   ```yaml
   # Specify the DNS URI of your Azure Cosmos DB.
   azure.documentdb.uri=https://wingtiptoys.documents.azure.com:443/

   # Specify the access key for your database.
   azure.documentdb.key=57686f6120447564652c20426f6220526f636b73==

   # Specify the name of your database.
   azure.documentdb.database=wingtiptoysdata
   ```

   ![Application.properties dosya düzenleme][RE02]

1. Kaydet ve Kapat *application.properties* dosya.

## <a name="add-sample-code-to-implement-basic-database-functionality"></a>Temel veritabanı işlevselliği uygulamak için örnek kod ekleme

Bu bölümde kullanıcı verilerini depolamak için iki Java sınıf oluşturun ve ardından kullanıcı sınıfının bir örneğini oluşturup, veritabanına kaydetmek için ana uygulama sınıfı değiştirin.

### <a name="define-a-basic-class-for-storing-user-data"></a>Kullanıcı verilerini depolamak için bir temel sınıf tanımlama

1. Adlı yeni bir dosya oluşturun *User.java* ana uygulama Java dosyası ile aynı dizinde.

1. Açık *User.java* dosyasını bir metin düzenleyicisinde ve dosyasını depolayan ve veritabanınızdaki değerleri almak genel kullanıcı sınıfı tanımlamak için aşağıdaki satırları ekleyin:

   ```java
   package com.example.wingtiptoys;

   public class User {
      private String id;
      private String firstName;
      private String lastName;
 
      public User(String id, String firstName, String lastName) {
         this.id = id;
         this.firstName = firstName;
         this.lastName = lastName;
      }
   
      public String getId() {
         return this.id;
      }

      public void setId(String id) {
         this.id = id;
      }

      public String getFirstName() {
         return firstName;
      }

      public void setFirstName(String firstName) {
         this.firstName = firstName;
      }

      public String getLastName() {
         return lastName;
      }

      public void setLastName(String lastName) {
         this.lastName = lastName;
      }

      @Override
      public String toString() {
         return String.format("User: %s %s", firstName, lastName);
      }
   }
   ```

1. Kaydet ve Kapat *User.java* dosya.

### <a name="define-a-data-repository-interface"></a>Bir veri deposu arabirimi tanımlayın

1. Adlı yeni bir dosya oluşturun *UserRepository.java* ana uygulama Java dosyası ile aynı dizinde.

1. Açık *UserRepository.java* dosyasını bir metin düzenleyicisinde açın ve dosyanın varsayılan DocumentDB depo arabirimi genişleten bir kullanıcı deposu arabirimi tanımlamak için aşağıdaki satırları ekleyin:

   ```java
   package com.example.wingtiptoys;

   import com.microsoft.azure.spring.data.documentdb.repository.DocumentDbRepository;
   import org.springframework.stereotype.Repository;

   @Repository
   public interface UserRepository extends DocumentDbRepository<User, String> {}   
   ```

1. Kaydet ve Kapat *UserRepository.java* dosya.

### <a name="modify-the-main-application-class"></a>Ana uygulama sınıfını değiştirme

1. Ana uygulama Java dosyası, uygulamanızın paket dizinini bulun; Örneğin:

   `C:\SpringBoot\wingtiptoys\src\main\java\com\example\wingtiptoys\WingtiptoysApplication.java`

   -veya-

   `/users/example/home/wingtiptoys/src/main/java/com/example/wingtiptoys/WingtiptoysApplication.java`

   ![Uygulama Java dosyasını bulun][JV01]

1. Ana uygulama Java dosyasını bir metin düzenleyicisinde açın ve aşağıdaki satırları dosyaya ekleyin:

   ```java
   package com.example.wingtiptoys;

   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.beans.factory.annotation.Autowired;
   import org.springframework.boot.CommandLineRunner;

   @SpringBootApplication
   public class WingtiptoysApplication implements CommandLineRunner {

      @Autowired
      private UserRepository repository;
    
      public static void main(String[] args) {
         SpringApplication.run(WingtiptoysApplication.class, args);
      }

      public void run(String... var1) throws Exception {
         final User testUser = new User("testId", "testFirstName", "testLastName");

         repository.deleteAll();
         repository.save(testUser);

         final User result = repository.findOne(testUser.getId());

         System.out.printf("\n\n%s\n\n",result.toString());
      }
   }
   ```

1. Ana uygulama Java dosyasını kaydedip kapatın.

## <a name="build-and-test-your-app"></a>Derleme ve uygulamanızı test etme

1. Bir komut istemi açın ve dizin klasörüne geçin Burada, *pom.xml* dosyasının bulunduğu; örneğin:

   `cd C:\SpringBoot\wingtiptoys`

   -veya-

   `cd /users/example/home/wingtiptoys`

1. Yay önyükleme uygulamanızı Maven ile ve çalıştırın; Örneğin:

   ```shell
   mvn package
   java -jar target/wingtiptoys-0.0.1-SNAPSHOT.jar
   ```

1. Uygulamanız birden fazla çalışma zamanı iletileri görüntülenir ve iletiyi görmelisiniz. `User: testFirstName testLastName` değerleri başarıyla depolanan ve, veritabanından alınan olduğunu belirtmek için görüntülenir.

   ![Uygulama başarılı çıktısı][JV02]

1. İsteğe bağlı: Tıklayarak veritabanınız için Özellikler sayfasında, Azure Cosmos DB'den içeriğini görüntülemek için Azure portalını kullanabilirsiniz **belge Gezgini**ve ardından seçerek ve görüntülenen listeyi öğesinden içeriği görüntülemek için.

   ![Verilerinizi görüntülemek için belge Gezgini kullanma][JV03]

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB ve Java kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure Cosmos DB belgelerine].

* [Azure Cosmos DB: Java ve Azure portal ile bir DocumentDB API uygulaması oluşturma][Build a DocumentDB API app with Java]

Azure üzerinde yay önyükleme uygulamalarında kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Azure için yay önyükleme DocumenDB Başlatıcı](https://github.com/Microsoft/azure-spring-boot-starters/tree/master/azure-documentdb-spring-boot-starter-sample)

* [Yay önyükleme uygulamasını Azure App Service'e dağıtma](../app-service/app-service-deploy-spring-boot-web-app-on-azure.md)

* [Azure kapsayıcı hizmeti Kubernetes kümesinde bir yay önyükleme uygulama çalıştıran](../container-service/container-service-deploy-spring-boot-app-on-kubernetes.md)

Azure’u Java ile kullanma hakkında daha fazla bilgi edinmek için bkz. [Azure Java Geliştirici Merkezi] ve [Visual Studio Team Services için Java Araçları].

<!-- URL List -->

[Azure Cosmos DB belgelerine]: /azure/cosmos-db/
[Azure Java Geliştirici Merkezi]: https://azure.microsoft.com/develop/java/
[Build a DocumentDB API app with Java]: https://docs.microsoft.com/azure/cosmos-db/create-documentdb-java
[ücretsiz Azure hesabı]: https://azure.microsoft.com/pricing/free-trial/
[Visual Studio Team Services için Java Araçları]: https://java.visualstudio.com/
[MSDN abone Avantajlarınızı]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[yay önyükleme]: http://projects.spring.io/spring-boot/
[yay Initializr]: https://start.spring.io/
[Yay Framework]: https://spring.io/

<!-- IMG List -->

[AZ01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ01.png
[AZ02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ02.png
[AZ03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ03.png
[AZ04]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ04.png
[AZ05]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/AZ05.png

[SI01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI01.png
[SI02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI02.png
[SI03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/SI03.png

[RE01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE01.png
[RE02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/RE02.png

[PM01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM01.png
[PM02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/PM02.png

[JV01]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV01.png
[JV02]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV02.png
[JV03]: ./media/documentdb-java-spring-boot-starter-with-cosmos-db/JV03.png
