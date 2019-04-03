---
title: Azure Cosmos DB Cassandra API'SİNİN tabloları Spark üzerinde toplama işlemleri
description: Bu makalede Azure Cosmos DB Cassandra API'SİNİN tabloları Spark karşı temel toplama işlemleri kapsar.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 4fbb86f4fbda9b8e521f7465bb8bb3d18602ca13
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877474"
---
# <a name="aggregate-operations-on-azure-cosmos-db-cassandra-api-tables-from-spark"></a>Azure Cosmos DB Cassandra API'SİNİN tabloları Spark üzerinde toplama işlemleri 

Bu makalede, Azure Cosmos DB Cassandra API'SİNİN tabloları Spark karşı temel toplama işlemleri açıklanır. 

> [!NOTE]
> Sunucu tarafı filtreleme ve sunucu tarafı toplama şu anda desteklenmiyor Azure Cosmos DB Cassandra API'si.

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
## <a name="sample-data-generator"></a>Örnek veri Oluşturucusu

```scala
// Generate a simple dataset containing five values
val booksDF = Seq(
   ("b00001", "Arthur Conan Doyle", "A study in scarlet", 1887,11.33),
   ("b00023", "Arthur Conan Doyle", "A sign of four", 1890,22.45),
   ("b01001", "Arthur Conan Doyle", "The adventures of Sherlock Holmes", 1892,19.83),
   ("b00501", "Arthur Conan Doyle", "The memoirs of Sherlock Holmes", 1893,14.22),
   ("b00300", "Arthur Conan Doyle", "The hounds of Baskerville", 1901,12.25)
).toDF("book_id", "book_author", "book_name", "book_pub_year","book_price")

booksDF.write
  .mode("append")
  .format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks", "output.consistency.level" -> "ALL", "ttl" -> "10000000"))
  .save()
```

## <a name="count-operation"></a>İşlem sayısı


### <a name="rdd-api"></a>RDD API

```scala
sc.cassandraTable("books_ks", "books").count
```

**Çıkış:**
```
res48: Long = 5
```

### <a name="dataframe-api"></a>API veri çerçevesi

Veri çerçevelerini karşı sayısı şu anda desteklenmiyor.  Aşağıdaki örnekte, bir dataframe sayısı belleğe geçici bir çözüm olarak dataframe başlamış sonra yürütme işlemi gösterilmektedir.

Seçin bir [depolama seçeneği]( https://spark.apache.org/docs/2.2.0/rdd-programming-guide.html#which-storage-level-to-choose) belleğe"dışında" çalışmasını önlemek için aşağıdaki seçeneklerden kullanılabilir, sorunları:

* MEMORY_ONLY: Varsayılan depolama seçenek budur. RDD JVM seri durumdan çıkarılmış Java nesneler olarak depolar. RDD bellekte uygun değilse, bazı bölümlerini önbelleğe alınmaz ve bunlar çalışma sırasında ihtiyaç duyulan her zaman yeniden.

* MEMORY_AND_DISK: RDD JVM seri durumdan çıkarılmış Java nesneler olarak depolar. RDD bellekte uygun değilse, disk üzerinde uygun kullanmayın ve gerekli olduğunda, bunları okuyun konum depolanırlar bölümler depolayın.

* MEMORY_ONLY_SER (Java/Scala): Bölüm başına serileştirilmiş Java nesnelerini bir bayt dizisi olarak RDD depolar. Bu seçenek alanı verimli seri durumdan çıkarılmış nesne için özellikle hızlı bir seri hale getirici kullanılırken karşılaştırıldığında, ancak daha fazla CPU-yoğun okumak için kullanılabilir.

* MEMORY_AND_DISK_SER (Java/Scala): Bu depolama seçeneği MEMORY_ONLY_SER gibi tek fark zaman yerine bunları yeniden hesaplanıyor disk belleği uygun olmayan bölümler sıvı sıçraması.

* DISK_ONLY: RDD bölümler yalnızca diskte depolar.

* MEMORY_ONLY_2, MEMORY_AND_DISK_2...: Yukarıdaki düzeyleri aynı ancak çoğaltır iki küme düğümlerinde her bölüm.

* OFF_HEAP (Deneysel): MEMORY_ONLY_SER, ancak benzer veri yığın bellekte depolar ve devre dışı yığın bellek önceden etkinleştirilmesini gerektirir. 

```scala
//Workaround
import org.apache.spark.storage.StorageLevel

//Read from source
val readBooksDF = spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()

//Explain plan
readBooksDF.explain

//Materialize the dataframe
readBooksDF.persist(StorageLevel.MEMORY_ONLY)

//Subsequent execution against this DF hits the cache 
readBooksDF.count

//Persist as temporary view
readBooksDF.createOrReplaceTempView("books_vw")
```

### <a name="sql"></a>SQL

```sql
select * from books_vw;
select count(*) from books_vw where book_pub_year > 1900;
select count(book_id) from books_vw;
select book_author, count(*) as count from books_vw group by book_author;
select count(*) from books_vw;
```

## <a name="average-operation"></a>Ortalama işlem

### <a name="rdd-api"></a>RDD API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Double) => c).mean
```

**Çıkış:**
```
res24: Double = 16.016000175476073
```

### <a name="dataframe-api"></a>API veri çerçevesi

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(avg("book_price"))
  .show
```

**Çıkış:**
```
+------------------+
|   avg(book_price)|
+------------------+
|16.016000175476073|
+------------------+
```

### <a name="sql"></a>SQL

```sql
select avg(book_price) from books_vw;
```
**Çıkış:**
```
16.016000175476073
```

## <a name="min-operation"></a>Minimum değeri alma işlemi

### <a name="rdd-api"></a>RDD API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).min
```

**Çıkış:**
```
res31: Float = 11.33
```

### <a name="dataframe-api"></a>API veri çerçevesi

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_id","book_price")
  .agg(min("book_price"))
  .show
```

**Çıkış:**
```
+---------------+
|min(book_price)|
+---------------+
|          11.33|
+---------------+
```

### <a name="sql"></a>SQL

```sql
select min(book_price) from books_vw;
```

**Çıkış:**
```
11.33
```

## <a name="max-operation"></a>En fazla işlem

### <a name="rdd-api"></a>RDD API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).max
```

### <a name="dataframe-api"></a>API veri çerçevesi

```scala 
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(max("book_price"))
  .show
```

**Çıkış:**
```
+---------------+
|max(book_price)|
+---------------+
|          22.45|
+---------------+
```

### <a name="sql"></a>SQL

```sql
select max(book_price) from books_vw;
```
**Çıkış:**
```
22.45
```

## <a name="sum-operation"></a>Toplam işlem

### <a name="rdd-api"></a>RDD API

```scala
sc.cassandraTable("books_ks", "books").select("book_price").as((c: Float) => c).sum
```

**Çıkış:**
```
res46: Double = 80.08000087738037
```

### <a name="dataframe-api"></a>API veri çerçevesi

```scala
spark
  .read
  .cassandraFormat("books", "books_ks", "")
  .load()
  .select("book_price")
  .agg(sum("book_price"))
  .show
```
**Çıkış:**
```
+-----------------+
|  sum(book_price)|
+-----------------+
|80.08000087738037|
+-----------------+
```

### <a name="sql"></a>SQL

```sql
select sum(book_price) from books_vw;
```

**Çıkış:**
```
80.08000087738037
```

## <a name="top-or-comparable-operation"></a>Üst veya karşılaştırılabilir işlemi

### <a name="rdd-api"></a>RDD API

```scala
val readCalcTopRDD = sc.cassandraTable("books_ks", "books").select("book_name","book_price").sortBy(_.getFloat(1), false)
readCalcTopRDD.zipWithIndex.filter(_._2 < 3).collect.foreach(println)
//delivers the first top n items without collecting the rdd to the driver.
```
**Çıkış:**
```
(CassandraRow{book_name: A sign of four, book_price: 22.45},0)
(CassandraRow{book_name: The adventures of Sherlock Holmes, book_price: 19.83},1)
(CassandraRow{book_name: The memoirs of Sherlock Holmes, book_price: 14.22},2)
readCalcTopRDD: org.apache.spark.rdd.RDD[com.datastax.spark.connector.CassandraRow] = MapPartitionsRDD[430] at sortBy at command-2371828989676374:1
```
### <a name="dataframe-api"></a>API veri çerçevesi

```scala
import org.apache.spark.sql.functions._

val readBooksDF = spark.read.format("org.apache.spark.sql.cassandra")
  .options(Map( "table" -> "books", "keyspace" -> "books_ks"))
  .load
  .select("book_name","book_price")
  .orderBy(desc("book_price"))
  .limit(3)

//Explain plan
readBooksDF.explain

//Top
readBooksDF.show
```

**Çıkış:**
```
== Physical Plan ==
TakeOrderedAndProject(limit=3, orderBy=[book_price#1840 DESC NULLS LAST], output=[book_name#1839,book_price#1840])
+- *(1) Scan org.apache.spark.sql.cassandra.CassandraSourceRelation@29cd5f58 [book_name#1839,book_price#1840] PushedFilters: [], ReadSchema: struct<book_name:string,book_price:float>
+--------------------+----------+
|           book_name|book_price|
+--------------------+----------+
|      A sign of four|     22.45|
|The adventures of...|     19.83|
|The memoirs of Sh...|     14.22|
+--------------------+----------+

import org.apache.spark.sql.functions._
readBooksDF: org.apache.spark.sql.Dataset[org.apache.spark.sql.Row] = [book_name: string, book_price: float]
```

### <a name="sql"></a>SQL

```sql
select book_name,book_price from books_vw order by book_price desc limit 3;
```

## <a name="next-steps"></a>Sonraki adımlar

Tablo kopyalama işlemleri gerçekleştirmek için bkz:

* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)
