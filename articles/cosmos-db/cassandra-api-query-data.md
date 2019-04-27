---
title: "Öğretici: Azure Cosmos DB'de Cassandra API'si hesabından veri sorgulama"
description: Bu öğreticide, bir Java uygulaması kullanarak bir Azure Cosmos DB Cassandra API'SİNİN hesaptan kullanıcı verilerini sorgulama işlemi gösterilmektedir.
ms.service: cosmos-db
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 09/24/2018
Customer intent: As a developer, I want to build a Java application to query data stored in a Cassandra API account of Azure Cosmos DB so that customers can manage the key/value data and utilize the global distribution, elastic scaling, multi-master, and other capabilities offered by Azure Cosmos DB.
ms.openlocfilehash: 69a9bc912f2cd52e52ca6403187f993413539ecd
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60899908"
---
# <a name="tutorial-query-data-from-a-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'de Cassandra API'si hesabından veri sorgulama

Bir geliştirici olarak, anahtar/değer çiftleri kullanan uygulamalar olabilir. Azure Cosmos DB'de Cassandra API hesabı, depolamak ve anahtar/değer veri sorgulamak için kullanabilirsiniz. Bu öğreticide, bir Java uygulaması kullanarak Azure Cosmos DB'de Cassandra API'si hesabından kullanıcı verilerini sorgulama işlemi gösterilmektedir. Java uygulaması kullanan [Java sürücüsü](https://github.com/datastax/java-driver) ve kullanıcı kimliği, kullanıcı adı ve kullanıcı şehir gibi kullanıcı verileri sorgular. 

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Cassandra tablosu verileri Sorgulama
> * Uygulamayı çalıştırma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

* Bu makale çok bölümlü bir öğreticiye aittir. Başlamadan önce Cassandra API hesabı, anahtar alanı tablosu oluşturmak için önceki adımları tamamladığınızdan emin olun ve [tabloya örnek veri yükleyebilir](cassandra-api-load-data.md). 

## <a name="query-data"></a>Verileri sorgulama

Cassandra API hesabınızdan veri sorgulaması yapmak için aşağıdaki adımları kullanın:

1. `src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserRepository.java` dosyasını açın. Aşağıdaki kod bloğunu ekleyin. Bu kod üç yöntem sunar: 

   * Veritabanındaki tüm kullanıcıları sorgulama
   * Kullanıcı kimliğine göre filtreleyerek belirli bir kullanıcıyı sorgulama
   * Bir tabloyu silmek için

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

2. `src\main\java\com\azure\cosmosdb\cassandra` klasöründeki `UserProfile.java` dosyasını açın. Bu sınıf, daha önce tanımladığınız createKeyspace ve createTable veri ekleme yöntemlerini çağıran main yöntemini içerir. Şimdi tüm kullanıcıları veya belirli bir kullanıcıyı sorgulayan aşağıdaki kodu ekleyin:

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

   Bu kod, proje oluşturduğunuz klasör yoluna dizini (cd) değiştirir. Ardından hedef klasörde `cosmosdb-cassandra-examples.jar` dosyasını oluşturmak için `mvn clean install` komutunu çalıştırır. Son olarak, Java uygulamasını çalıştırır.

   ```bash
   cd "cassandra-demo"
   
   mvn clean install
   
   java -cp target/cosmosdb-cassandra-examples.jar com.azure.cosmosdb.cassandra.examples.UserProfile
   ```

2. Şimdi Azure portalda **Veri Gezgini**'ni açın ve kullanıcı tablosunun silindiğini doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulan, kaynak grubu, Azure Cosmos hesabı ve tüm ilgili kaynakları silin. Bunu yapmak için select sanal makinenin kaynak grubunu seçin. **Sil**ve ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB Cassandra API'SİNİN hesabında buradan veri sorgulamak öğrendiniz. Şimdi bir sonraki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Cassandra API hesabına veri geçirme](cassandra-import-data.md)


