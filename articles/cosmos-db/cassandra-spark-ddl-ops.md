---
title: Azure Cosmos DB Cassandra API'SİNİN spark'tan DDL işlemleri
description: Bu makalede Azure Cosmos DB Cassandra API'SİNİN spark'tan karşı anahtar alanı ve tablo DDL işlemleri ayrıntılı olarak açıklanmaktadır.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 5c12787cd6e0df19fd842dd44da49aa5ea97aa05
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60898891"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>Azure Cosmos DB Cassandra API'SİNİN spark'tan DDL işlemleri

Bu makalede Azure Cosmos DB Cassandra API'SİNİN spark'tan karşı anahtar alanı ve tablo DDL işlemleri ayrıntılı olarak açıklanmaktadır.

## <a name="cassandra-api-related-configuration"></a>Cassandra API ile ilgili yapılandırma 

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

## <a name="keyspace-ddl-operations"></a>Anahtar alanı DDL işlemleri

### <a name="create-a-keyspace"></a>Bir anahtar alanı oluşturun

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

Cqlsh içinde aşağıdaki komutu çalıştırın ve daha önce oluşturduğunuz anahtar alanı görmeniz gerekir.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Bir anahtar alanı bırak

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Tablo DDL işlemleri

**Dikkate alınacak noktalar:**  

- Aktarım hızı, create table deyimini kullanarak tablo düzeyinde atanabilir.  
- Bir bölüm anahtarı 10 GB veri depolayabilirsiniz.  
- Tek bir kayıtta, en fazla 2 MB veri depolayabilirsiniz.  
- Bir bölüm anahtar aralığı birden çok bölüm anahtarları depolayabilir.

### <a name="create-a-table"></a>Bir tablo oluşturma

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

Cqlsh içinde aşağıdaki komutu çalıştırın ve adlı tabloda görmeniz gerekir "books: 

```bash
USE books_ks;
DESCRIBE books;
```

Sağlanan aktarım hızı ve varsayılan TTL değerleri önceki komutun çıktısındaki gösterilmez, bu değerleri portaldan alabilirsiniz.

### <a name="alter-table"></a>Alter Table

Alter table komutu kullanarak, aşağıdaki değerleri değiştirebilirsiniz:

* sağlanan aktarım hızı 
* yaşam süresi değeri
<br>Sütun değişiklikleri şu anda desteklenmemektedir.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Tablo bırakma

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>İçinde cqlsh doğrula

Cqlsh içinde aşağıdaki komutu çalıştırın ve "books" tablosu artık kullanılabilir olduğunu görmeniz gerekir:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Sonraki adımlar

Anahtar alanı ve tablonun oluşturduktan sonra CRUD işlemleri ve daha fazla bilgi için aşağıdaki makalelere geçin:
 
* [İşlem oluşturma/Ekle](cassandra-spark-create-ops.md)  
* [okuma işlemleri](cassandra-spark-read-ops.md)  
* [Upsert işlem](cassandra-spark-upsert-ops.md)  
* [Silme işlemleri](cassandra-spark-delete-ops.md)  
* [Toplama işlemleri](cassandra-spark-aggregation-ops.md)  
* [Tablo kopyalama işlemleri](cassandra-spark-table-copy-ops.md)  
