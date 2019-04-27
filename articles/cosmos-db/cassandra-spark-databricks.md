---
title: Azure Databricks gelen erişim Azure Cosmos DB Cassandra API'si
description: Bu makalede Azure Cosmos DB Cassandra API'SİNİN Azure Databricks ile nasıl çalışılacağı ele alınmaktadır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 37a06b19285c1196b5d87830ea176d4bd0d4eade
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60894020"
---
# <a name="access-azure-cosmos-db-cassandra-api-data-from-azure-databricks"></a>Azure Cosmos DB Cassandra API'SİNİN verileri Azure Databricks erişim

Bu makalede ayrıntıları veritabanlarıyla Azure Cosmos DB Cassandra API'SİNİN spark'tan üzerinde nasıl [Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks).

## <a name="prerequisites"></a>Önkoşullar

* [Bir Azure Cosmos DB Cassandra API hesabı sağlayın](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB Cassandra API'sine bağlanma hakkında temel bilgileri gözden geçirin](cassandra-spark-generic.md)

* [Azure Databricks kümesi sağlama](../azure-databricks/quickstart-create-databricks-workspace-portal.md)

* [Cassandra API ile çalışmak için kod örnekleri gözden geçirin](cassandra-spark-generic.md#next-steps)

* [Bu nedenle tercih ederseniz cqlsh doğrulama için kullanın.](cassandra-spark-generic.md#connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Cassandra API örnek yapılandırması Cassandra Bağlayıcısı için:**

  Spark bağlamı bir parçası olarak başlatılması için Cassandra bağlantı ayrıntıları Cassandra API'si için bağlayıcı gerektirir. Bir Databricks not defteri başlattığında, spark bağlamı zaten başlatıldı ve durdurup yeniden başlatın önerilir değil. Tek bir çözüm düzeyinde bir kümesi küme spark yapılandırmasında Cassandra API'si örnek yapılandırması eklemektir. Bu, Küme başına tek seferlik bir etkinliktir. Anahtar değer çifti boşlukla ayrılmış olarak Spark yapılandırması için aşağıdaki kodu ekleyin:
 
  ```scala
  spark.cassandra.connection.host YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com
  spark.cassandra.connection.port 10350
  spark.cassandra.connection.ssl.enabled true
  spark.cassandra.auth.username YOUR_COSMOSDB_ACCOUNT_NAME
  spark.cassandra.auth.password YOUR_COSMOSDB_KEY
  ```

## <a name="add-the-required-dependencies"></a>Gerekli bağımlılıkları ekleme

* **Cassandra Spark Bağlayıcısı:** - Azure Cosmos DB Cassandra API'SİNİN Spark Bağlayıcısı Azure Databricks kümesine bağlı Cassandra ile tümleştirmek için. Kümeye eklemek için:

  * Spark sürümü Databricks çalışma zamanı sürümünü gözden geçirin. Ardından bulun [maven koordinatları](https://mvnrepository.com/artifact/com.datastax.spark/spark-cassandra-connector) Cassandra Spark Bağlayıcısı ile uyumludur ve kümeye ekleyin. Bkz: ["Maven paketini veya Spark paketini karşıya yükle"](https://docs.databricks.com/user-guide/libraries.html) makale bağlayıcı kitaplık kümeye eklemek için. Örneğin, maven koordinatı "Databricks çalışma zamanı modülü sürümü 4.3", "2.3.1 Spark" ve "Scala 2.11" olan `spark-cassandra-connector_2.11-2.3.1`

* **Azure Cosmos DB Cassandra API özgü kitaplığı:** -özel bağlantı üreteci için Azure Cosmos DB Cassandra API'SİNİN Cassandra Spark Bağlayıcıdan yeniden deneme ilkesi yapılandırmak için gereklidir. Ekleme `com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0` [maven koordinatları](https://search.maven.org/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper/1.0.0/jar) kümeye kitaplık ekleme için.

## <a name="sample-notebooks"></a>Örnek not defterleri

Azure Databricks listesini [örnek not defterleri](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/notebooks/scala) GitHub deposunu, karşıdan yüklemek kullanılabilir. Bu örnekler Spark'tan Azure Cosmos DB Cassandra API'sine bağlanmak ve verileri farklı CRUD işlemleri gerçekleştirmek nasıl içerir. Ayrıca [tüm not defterlerinin alma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-databricks/tree/master/dbc) , Databricks çalışma alanı küme ve çalıştırın. 

## <a name="accessing-azure-cosmos-db-cassandra-api-from-spark-scala-programs"></a>Spark Scala programlarından Azure Cosmos DB Cassandra API'sine erişme

Spark programları otomatik işlemleri Azure databricks kullanarak kümeye gönderildiğinde çalıştırılacak [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html)) ve Azure Databricks işleri çalıştırmak için zamanlanmış.

Yardımcı olmak için bağlantılar, Azure Cosmos DB Cassandra API ile etkileşim kurmak için Scala Spark programları derleme başlama aşağıda verilmiştir.
* [Spark Scala programdan Azure Cosmos DB Cassandra API'sine bağlanma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)
* [Spark Scala program, Azure Databricks'te otomatik bir iş olarak çalıştırma](https://docs.azuredatabricks.net/user-guide/jobs.html)
* [Cassandra API ile çalışmak için kod örnekleri tam listesi](cassandra-spark-generic.md#next-steps)

## <a name="next-steps"></a>Sonraki adımlar

Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama.
