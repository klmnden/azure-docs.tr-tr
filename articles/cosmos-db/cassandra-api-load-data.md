---
title: Bir Java uygulaması kullanarak bir Azure Cosmos DB Cassandra API'SİNİN tabloya örnek veri yükleme
description: Bu makale, bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabındaki bir tabloya örnek kullanıcı verileri yüklemeyi göstermektedir.
services: cosmos-db
author: kanshiG
ms.service: cosmos-db
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: ac297a04936785c76b7651cb5b78080baf4f684c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52836262"
---
# <a name="tutorial-load-sample-data-into-an-azure-cosmos-db-cassandra-api-table"></a>Öğretici: Azure Cosmos DB Cassandra API tablosuna örnek veri yükleme

Bu öğretici, bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabındaki bir tabloya örnek kullanıcı verileri yüklemeyi göstermektedir. Java uygulaması [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve ID, user name, user city gibi kullanıcı verilerini yükler. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosuna veri yükleme
> * Uygulamayı çalıştırma

## <a name="prerequisites"></a>Önkoşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Bu belgeye başlamadan önce [Cassandra API hesabını, anahtar alanını ve tabloyu](create-cassandra-api-account-java.md) oluşturduğunuzdan emin olun.   

## <a name="load-data-into-the-table"></a>Tabloya veri yükleme

Cassandra API tablonuza veri yüklemek için aşağıdaki adımları kullanın:

1. “src\main\java\com\azure\cosmosdb\cassandra” klasöründeki “UserRepository.java” dosyasını açın ve user_id, user_name ve user_bcity alanlarını tabloya ekleme kodunu ekleyin:

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
 
2. “src\main\java\com\azure\cosmosdb\cassandra” klasöründeki “UserProfile.java” dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable yöntemlerini çağıran main yöntemini içerir. Şimdi Cassandra API tablosuna bazı örnek veriler eklemek için aşağıdaki kodu ekleyin.

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

Komut istemini veya terminal penceresini açın ve projeyi oluşturduğunuz klasörün yolunu değiştirin. Hedef klasörde cosmosdb-cassandra-examples.jar dosyasını oluşturmak için “mvn clean install” komutunu, ardından uygulamayı çalıştırın. 

```bash
cd "cassandra-demo"

mvn clean install

java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
```

Artık kullanıcı bilgilerinin tabloya eklendiğini doğrulamak için Azure portalda Veri Gezgini'ni açabilirsiniz.
    
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Cosmos DB Cassandra API hesabına örnek veri yüklemeyi öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabından veri sorgulama](cassandra-api-query-data.md)
