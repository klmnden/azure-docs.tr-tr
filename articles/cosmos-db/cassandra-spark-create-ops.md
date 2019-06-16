---
title: Oluştur/Veri Azure Cosmos DB Cassandra API'SİNİN spark'tan içine Ekle
description: Bu makalede Azure Cosmos DB Cassandra API'SİNİN tablolara örnek veriler ekleme işlemi açıklanmaktadır
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: aea646e7a390d5b53f0d4b388cfecd0c80fb19da
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60894054"
---
# <a name="createinsert-data-into-azure-cosmos-db-cassandra-api-from-spark"></a>Oluştur/Veri Azure Cosmos DB Cassandra API'SİNİN spark'tan içine Ekle
 
Bu makalede, Azure Cosmos DB Cassandra API'SİNİN spark'tan bir tabloya örnek veri eklemek açıklar.

## <a name="cassandra-api-configuration"></a>Cassandra API configuration

```scala
import org.apache.spark.sql.cassandra._
//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")
//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```
## <a name="dataframe-api"></a>API veri çerçevesi

### <a name="create-a-dataframe-with-sample-data"></a>Örnek verilerle bir Dataframe oluşturun

```scala
// Generate a dataframe containing five records
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
).toDF("book_id", "book_author", "book_name", "book_pub_year")

//Review schema
booksDF.printSchema

//Print
booksDF.show
```

> [!NOTE]
> Satır düzeyinde "Yoksa oluştur" işlevler henüz desteklenmiyor.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'sine Sürdür

Veriler kaydedilirken de yaşam süresi ayarlayabilirsiniz ve aşağıdaki örnekte gösterildiği gibi tutarlılık ilke ayarları:

```scala
//Persist
booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

> [!NOTE]
> Sütun düzeyinde TTL henüz desteklenmiyor.

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

```sql
use books_ks;
select * from books;
```

## <a name="resilient-distributed-database-rdd-api"></a>Dayanıklı dağıtılmış veritabanı (RDD) API'si

### <a name="create-a-rdd-with-sample-data"></a>Örnek verilerle bir RDD oluşturma
```scala
//Delete records created in the previous section 
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("delete from books_ks.books where book_id in ('b00300','b00001','b00023','b00501','b09999','b01001','b00999','b03999','b02999');"))

//Create RDD
val booksRDD = sc.parallelize(Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901)
))

//Review
booksRDD.take(2).foreach(println)
```

> [!NOTE]
> Yoksa oluştur işlevi henüz desteklenmiyor.

### <a name="persist-to-azure-cosmos-db-cassandra-api"></a>Azure Cosmos DB Cassandra API'sine Sürdür

Veriler Cassandra API'sine kaydedilirken de yaşam süresi ayarlayabilirsiniz ve aşağıdaki örnekte gösterildiği gibi tutarlılık ilke ayarları:

```scala
import com.datastax.spark.connector.writer._

//Persist
booksRDD.saveToCassandra("books_ks", "books", SomeColumns("book_id", "book_author", "book_name", "book_pub_year"),writeConf = WriteConf(ttl = TTLOption.constant(900000),consistencyLevel = ConsistencyLevel.ALL))
```

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

```sql
use books_ks;
select * from books;
```

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB Cassandra API'si, depolanan veriler üzerinde başka işlemler gerçekleştirmek için aşağıdaki makaleleri, Azure Cosmos DB Cassandra API'SİNİN tablosuna veri ekleme sonra devam edin:
 
* [okuma işlemleri](cassandra-spark-read-ops.md)
* [Upsert işlem](cassandra-spark-upsert-ops.md)
* [Silme işlemleri](cassandra-spark-delete-ops.md)
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)

