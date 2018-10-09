---
title: Bir Azure Cosmos DB Cassandra API hesabından veri sorgulama
description: Bu makalede bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabından kullanıcı verileri sorgulama gösterilmektedir.
services: cosmos-db
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.component: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
ms.openlocfilehash: c1fb4c27f897e3c0952ed6419e167613ac8204f7
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47223500"
---
# <a name="query-data-from-an-azure-cosmos-db-cassandra-api-account"></a>Bir Azure Cosmos DB Cassandra API hesabından veri sorgulama

Bu öğreticide bir Java uygulaması kullanarak Azure Cosmos DB Cassandra API hesabından kullanıcı verileri sorgulama gösterilmektedir. Java uygulaması [Java sürücüsünü](https://github.com/datastax/java-driver) kullanır ve ID, user name, user city gibi kullanıcı verilerini sorgular. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosu verilerini sorgulama
> * Uygulamayı çalıştırma

## <a name="prerequisites"></a>Ön koşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Başlamadan önce [Cassandra API hesabını, anahtar alanını ve tablosunu oluşturmak](create-cassandra-api-account-java.md) ve [tabloya örnek veriler yüklemek](cassandra-api-load-data.md) için önceki adımları tamamladığınızdan emin olun. 

## <a name="query-data"></a>Verileri sorgulama

`src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserRepository.java` dosyasını açın. Aşağıdaki kod bloğunu ekleyin. Bu kod üç işlev sağlar: veritabanındaki tüm kullanıcıları sorgulama, user ID ile filtrelenen belirli bir kullanıcıyı sorgulama ve bir tabloyu silme. 

```java
/**
* Select all rows from user table
*/
public void selectAllUsers() {

    final String query = "SELECT * FROM uprofile.user";
    List<Row> rows = session.execute(query).all();

    for (Row row : rows) {
       LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
    }
}

/**
* Select a row from user table
*
* @param id user_id
*/
public void selectUser(int id) {
    final String query = "SELECT * FROM uprofile.user where user_id = 3";
    Row row = session.execute(query).one();

    LOGGER.info("Obtained row: {} | {} | {} ", row.getInt("user_id"), row.getString("user_name"), row.getString("user_bcity"));
}

/**
* Delete user table.
*/
public void deleteTable() {
   final String query = "DROP TABLE IF EXISTS uprofile.user";
   session.execute(query);
}
```

`src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserProfile.java` dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable veri ekleme yöntemlerini çağıran main yöntemini içerir. Şimdi tüm kullanıcıları veya belirli bir kullanıcıyı sorgulayan aşağıdaki kodu ekleyin:

```java
LOGGER.info("Select all users");
repository.selectAllUsers();

LOGGER.info("Select a user by id (3)");
repository.selectUser(3);

LOGGER.info("Delete the users profile table");
repository.deleteTable();
```

## <a name="run-the-java-app"></a>Java uygulamasını çalıştırma
1. Bir komut istemi veya terminal penceresi açın. Aşağıdaki kod bloğunu yapıştırın. 

   Bu kod, dizini projeyi oluşturduğunuz klasör yolu ile değiştirir (cd). Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Şimdi Azure portalda **Veri Gezgini**'ni açın ve kullanıcı tablosunun silindiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

* Bu öğreticide Azure Cosmos DB Cassandra API hesabından veri sorgulamayı öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabına veri geçirme](cassandra-import-data.md)


