---
title: "Öğretici: Bir Java uygulaması kullanarak Azure Cosmos DB'de Cassandra API'si tabloya örnek veri yükleme"
description: Bu öğreticide, bir java uygulaması kullanarak Azure Cosmos DB'de Cassandra API'si tabloya örnek kullanıcı verileri yüklemek gösterilir.
author: kanshiG
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: govindk
ms.reviewer: sngun
Customer intent: As a developer, I want to build a Java application to load data to a Cassandra API table in Azure Cosmos DB so that customers can store and manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: e9fc96b9f26344045aa7e45fe7bdbe389e329377
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66472687"
---
# <a name="tutorial-load-sample-data-into-a-cassandra-api-table-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'de Cassandra API'si tabloya örnek veri yükleme

Bir geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalar olabilir. Azure Cosmos DB'de Cassandra API hesabı, anahtar/değer veri depolamak ve yönetmek için kullanabilirsiniz. Bu öğreticide, bir Java uygulaması kullanarak örnek kullanıcı verileri bir Azure Cosmos DB Cassandra API'SİNİN hesabında bir tabloya yüklemek gösterilir. Java uygulaması kullanan [Java sürücüsü](https://github.com/datastax/java-driver) ve kullanıcı kimliği, kullanıcı adı ve kullanıcı şehir gibi kullanıcı verileri yükler. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Bir Cassandra tabloya veri yükleme
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Bu belgeye başlamadan önce [Cassandra API hesabını, anahtar alanını ve tabloyu](create-cassandra-api-account-java.md) oluşturduğunuzdan emin olun.   

## <a name="load-data-into-the-table"></a>Tabloya veri yükleme

Cassandra API tablonuza veri yüklemek için aşağıdaki adımları kullanın:

1. "Src\main\java\com\azure\cosmosdb\cassandra" klasörünün altındaki "UserRepository.java" dosyasını açın ve tabloya USER_ID, user_name ve user_bcity alanları eklemek için kod ekleyin:

   ```java
   /**
   * Insert a row into user table
   *
   * @param id   user_id
   * @param name user_name
   * @param city user_bcity
   */
   public void insertUser(PreparedStatement statement, int id, String name, String city) {
        BoundStatement boundStatement = new BoundStatement(statement);
        session.execute(boundStatement.bind(id, name, city));
   }

   /**
   * Create a PrepareStatement to insert a row to user table
   *
   * @return PreparedStatement
   */
   public PreparedStatement prepareInsertStatement() {
      final String insertStatement = "INSERT INTO  uprofile.user (user_id, user_name , user_bcity) VALUES (?,?,?)";
   return session.prepare(insertStatement);
   }
   ```
 
2. "Src\main\java\com\azure\cosmosdb\cassandra" klasörünün altındaki "UserProfile.java" dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir. Şimdi Cassandra API tablosuna bazı örnek veriler eklemek için aşağıdaki kodu ekleyin.

   ```java
   //Insert rows into user table
   PreparedStatement preparedStatement = repository.prepareInsertStatement();
     repository.insertUser(preparedStatement, 1, "JohnH", "Seattle");
     repository.insertUser(preparedStatement, 2, "EricK", "Spokane");
     repository.insertUser(preparedStatement, 3, "MatthewP", "Tacoma");
     repository.insertUser(preparedStatement, 4, "DavidA", "Renton");
     repository.insertUser(preparedStatement, 5, "PeterS", "Everett");
   ```

## <a name="run-the-app"></a>Uygulamayı çalıştırma

Bir komut istemi veya terminal penceresi açın ve proje oluşturduğunuz için klasör yolu değiştirin. Hedef klasördeki cosmosdb cassandra-examples.jar dosyasını oluşturun ve uygulamayı çalıştırmak için "mvn temiz yükleme" komutunu çalıştırın. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Artık kullanıcı bilgilerinin tabloya eklendiğini doğrulamak için Azure portalda Veri Gezgini'ni açabilirsiniz.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB Cassandra API'SİNİN hesabına örnek verileri yüklemeyi öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabından veri sorgulama](cassandra-api-query-data.md)
