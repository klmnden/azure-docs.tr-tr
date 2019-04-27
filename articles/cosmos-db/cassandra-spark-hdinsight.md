---
title: Erişim Azure Cosmos DB Cassandra API'si YARN üzerinde spark'tan HDInsight ile
description: Bu makalede Azure Cosmos DB Cassandra API'SİNİN YARN ile HDInsight üzerinde Spark ile nasıl çalışılacağı ele alınmıştır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: f728baedf9e325f224ce52e64325064f553d2671
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60893711"
---
# <a name="access-azure-cosmos-db-cassandra-api-from-spark-on-yarn-with-hdinsight"></a>Erişim Azure Cosmos DB Cassandra API'si YARN üzerinde spark'tan HDInsight ile

Bu makalede Azure Cosmos DB Cassandra API'SİNİN Spark HDInsight Spark ile YARN üzerinde spark-shell erişmek nasıl ele alınmaktadır. HDInsight, Microsoft'un Hortonworks Hadoop PaaS azure'da nesne depolama için HDFS yararlanır ve dahil olmak üzere birkaç model olarak sağlanır gelen olan [Spark](../hdinsight/spark/apache-spark-overview.md).  Bu belgedeki içeriği HDInsight Spark başvuruyor, ancak tüm Hadoop dağıtımları için geçerlidir.  

## <a name="prerequisites"></a>Önkoşullar

* [Azure Cosmos DB Cassandra API'si sağlama](create-cassandra-dotnet.md#create-a-database-account)

* [Azure Cosmos DB Cassandra API'sine bağlanma hakkında temel bilgileri gözden geçirin](cassandra-spark-generic.md)

* [Bir HDInsight Spark kümesi sağlama](../hdinsight/spark/apache-spark-jupyter-spark-sql.md)

* [Cassandra API ile çalışmak için kod örnekleri gözden geçirin](cassandra-spark-generic.md#next-steps)

* [Bu nedenle tercih ederseniz cqlsh doğrulama için kullanın.](cassandra-spark-generic.md##connecting-to-azure-cosmos-db-cassandra-api-from-spark)

* **Cassandra API Spark2 yapılandırmasında** -Cassandra için Spark Bağlayıcısı, Cassandra bağlantı Spark bağlamı bir parçası olarak başlatılması için ayrıntıları gerektirir. Jupyter not defteri başlattığında, bağlam ve spark oturumunun zaten başlatılmış ve durdurmak ve tam HDInsight varsayılan Jupyter not defteri başlatma bir parçası olarak her bir yapılandırmaya sahip olmadığı sürece Spark bağlamı yeniden başlatmak için önerilir değil. Bir geçici çözüm, Ambari, doğrudan Spark2 hizmet yapılandırması Cassandra örnek ayrıntıları eklemektir. Bu, Spark2 hizmeti yeniden başlatma gerektiren Küme başına tek seferlik bir etkinliktir.
 
  1. Ambari, Spark2 hizmeti ve select yapılandırmaları için Git

  2. Ardından özel spark2-ayarlarına gidin ve aşağıdaki yeni bir özellik ekleyin ve Spark2 hizmetini yeniden başlatın:

  ```scala
  spark.cassandra.connection.host=YOUR_COSMOSDB_ACCOUNT_NAME.cassandra.cosmosdb.azure.com<br>
  spark.cassandra.connection.port=10350<br>
  spark.cassandra.connection.ssl.enabled=true<br>
  spark.cassandra.auth.username=YOUR_COSMOSDB_ACCOUNT_NAME<br>
  spark.cassandra.auth.password=YOUR_COSMOSDB_KEY<br>
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-spark-shell"></a>Spark kabuğundan erişim Azure Cosmos DB Cassandra API'si

Spark shell test keşif amacıyla kullanılır.

* Spark-shell kümenizin Spark sürümü ile uyumlu gerekli maven bağımlılıkları ile başlatın.

  ```scala
  spark-shell --packages "com.datastax.spark:spark-cassandra-connector_2.11:2.3.0,com.microsoft.azure.cosmosdb:azure-cosmos-cassandra-spark-helper:1.0.0"
  ```

* DDL ve DML bazı işlemler yürütün

  ```scala
  import org.apache.spark.rdd.RDD
  import org.apache.spark.{SparkConf, SparkContext}

  import spark.implicits._
  import org.apache.spark.sql.functions._
  import org.apache.spark.sql.Column
  import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType,LongType,FloatType,DoubleType, TimestampType}
  import org.apache.spark.sql.cassandra._

  //Spark connector
  import com.datastax.spark.connector._
  import com.datastax.spark.connector.cql.CassandraConnector

  //CosmosDB library for multiple retry
  import com.microsoft.azure.cosmosdb.cassandra

  // Specify connection factory for Cassandra
  spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

  // Parallelism and throughput configs
  spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
  spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
  spark.conf.set("spark.cassandra.output.concurrent.writes", "100")
  spark.conf.set("spark.cassandra.concurrent.reads", "512")
  spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
  spark.conf.set("spark.cassandra.connection.keep_alive_ms", "60000000") //Increase this number as needed
  ```

* CRUD işlemleri çalıştırma

  ```scala
  //1) Create table if it does not exist
  val cdbConnector = CassandraConnector(sc)
  cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000;"))

  //2) Delete data from potential prior runs
  cdbConnector.withSessionDo(session => session.execute("DELETE FROM books_ks.books WHERE book_id IN ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999','b000009');"))

  //3) Generate a few rows
  val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
  ).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

  //4) Persist
  booksDF.write.mode("append").format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000")).save()

  //5) Read the data in the table
  spark.read.format("org.apache.spark.sql.cassandra").options(Map( "table" -> "books", "keyspace" -> "books_ks")).load.show
  ```

## <a name="access-azure-cosmos-db-cassandra-api-from-jupyter-notebooks"></a>Jupyter not defterleri gelen Azure Cosmos DB Cassandra API'sine erişim

HDInsight Spark Zeppelin ve Jupyter not defteri Hizmetleri ile birlikte gelir. Scala ve Python desteği her iki web tabanlı bir not defteri ortamları değildirler. Not defterlerini etkileşimli keşif analizi ve işbirliği için kullanışlı olsa da işletimsel/productionized işlemleri için tasarlanmamıştır.

Aşağıdaki Jupyter Notebook belgeleri, HDInsight Spark kümesine yüklenebilir ve Azure Cosmos DB Cassandra API'si ile çalışmak için hazır örnekleri sağlar. İlk not defterini gözden geçirdiğinizden emin olun `1.0-ReadMe.ipynb` Azure Cosmos DB Cassandra API'sine bağlanmak için Spark hizmeti yapılandırmasını gözden geçirmek için.

Bu not defterlerini altında indirme [azure-cosmos-db-cassandra-api-spark-notebooks-jupyter](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-notebooks-jupyter/blob/master/scala/) makinenize.
  
### <a name="how-to-upload"></a>Karşıya yükleme:
Jupyter başlattığında, Scala için gidin. İlk olarak bir dizin oluşturun ve Not Defterleri dizine yükleyin. En üstte karşıya yükleme düğmesini olan sağ tarafı.  

### <a name="how-to-run"></a>Çalıştırmayı öğrenin:
Not defterlerini ve her bir not defteri hücre sırayla çalışır.  Tüm hücreleri yürütün veya shift + her hücre için girmek için her bir not defteri üst kısmındaki Çalıştır düğmesinin tıklayın.

## <a name="access-with-azure-cosmos-db-cassandra-api-from-your-spark-scala-program"></a>Spark Scala programınızdan ile Azure Cosmos DB Cassandra API'sine erişim

Üretimde otomatik işlemler için Spark programları kümesine gönderilen [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html).

## <a name="next-steps"></a>Sonraki adımlar

* [Spark Scala oluşturma IDE içinde program ve yürütme için Lıvy aracılığıyla HDInsight Spark kümesine gönderin](../hdinsight/spark/apache-spark-create-standalone-application.md)

* [Spark Scala programdan Azure Cosmos DB Cassandra API'sine bağlanma](https://github.com/Azure-Samples/azure-cosmos-db-cassandra-api-spark-connector-sample/blob/master/src/main/scala/com/microsoft/azure/cosmosdb/cassandra/SampleCosmosDBApp.scala)

* [Cassandra API ile çalışmak için kod örnekleri tam listesi](cassandra-spark-generic.md)
