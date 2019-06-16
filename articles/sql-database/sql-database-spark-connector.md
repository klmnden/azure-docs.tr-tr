---
title: Spark Bağlayıcısı ile Azure SQL veritabanı ve SQL Server | Microsoft Docs
description: Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı'nı kullanmayı öğrenin
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: carlrab
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 8e531de34302ef8aee571c960955d33a4832aa11
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60331523"
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı ile gerçek zamanlı büyük veri analizi hızlandırın

Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı, SQL veritabanları, Azure SQL veritabanı ve SQL Server, Spark işleri için giriş veri kaynağı veya çıkış veri havuzu olarak çalışacak şekilde de dahil olmak üzere sağlar. Büyük veri analizini gerçek zamanlı işlem verilerini kullanan ve geçici sorguları veya raporlama için sonuçları kalıcı olanak tanır. Bu bağlayıcı, yerleşik JDBC bağlayıcıya karşılaştırıldığında, SQL veritabanlarına Ekle verileri toplu olanağı sağlar. Bu satır satır ekleme 20 x daha hızlı performans için 10 x ile daha iyi performans gösterir. Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı, AAD kimlik doğrulamasını da destekler. Güvenli bir AAD hesabınızı kullanarak Azure Databricks'ten Azure SQL veritabanınıza bağlanma sağlar. Bu, yerleşik JDBC Bağlayıcısı ile benzer arabirimleri sağlar. Bu yeni bağlayıcıyı kullanmak için var olan Spark işleri geçirmek kolay bir işlemdir.

## <a name="download"></a>İndirme
Kullanmaya başlamak için bir bağlayıcı SQL DB için Spark indirin [azure sqldb spark depo](https://github.com/Azure/azure-sqldb-spark) GitHub üzerinde.

## <a name="official-supported-versions"></a>Desteklenen sürümler resmi

| Bileşen                            |Sürüm                  |
| :----------------------------------- | :---------------------- |
| Apache Spark                         |2.0.2 veya üzeri           |
| Scala                                |2.10 veya üzeri            |
| SQL Server için Microsoft JDBC Sürücüsü |6.2 veya üstü             |
| Microsoft SQL Server                 |SQL Server 2008 veya üstü |
| Azure SQL Veritabanı                   |Desteklenen                |

Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı, Spark çalışan düğümlerine ve SQL veritabanları arasında veri taşımak SQL Server için Microsoft JDBC sürücüsü kullanır:
 
Veri akışı aşağıdaki gibidir:
1. Spark ana düğüm SQL Server veya Azure SQL veritabanına bağlanır ve belirli bir tabloyu veya belirli bir SQL sorgusunu kullanarak verileri yükler.
2. Spark ana düğüm veri dönüştürme için çalışan düğümlerine dağıtır. 
3. Çalışan düğümü, SQL Server veya Azure SQL veritabanına bağlanır ve veritabanına veri yazar. Satır ekleme kullanın veya ekleme toplu kullanıcı seçebilirsiniz.

Veri akışı aşağıdaki diyagramda gösterilmiştir.

   ![architecture](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>Spark SQL DB bağlayıcısı oluşturun
Şu anda, bağlayıcı proje maven kullanır. Bağımlılıkları olmadan bir bağlayıcı oluşturmak için çalıştırabilirsiniz:

- mvn temiz paket
- Yayın klasörünü JAR en son sürümlerini indirin
- SQL DB Spark JAR Ekle

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Spark Bağlayıcısı'nı kullanarak SQL Veritabanına bağlanma
Azure SQL veritabanı veya SQL Server Spark işlerini bağlanmak, okuma veya veri yazma. Bir Azure SQL veritabanı veya SQL Server veritabanı DML veya DDL sorgu da çalıştırabilirsiniz.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Azure SQL veritabanı veya SQL Server verilerini okuma

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "dbTable"        -> "dbo.Clients"
  "user"           -> "username",
  "password"       -> "*********",
  "connectTimeout" -> "5", //seconds
  "queryTimeout"   -> "5"  //seconds
))

val collection = sqlContext.read.sqlDB(config)
collection.show()
```
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Belirtilen SQL sorgusu ile Azure SQL veritabanı veya SQL Server verileri okuma
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "queryCustom"  -> "SELECT TOP 100 * FROM dbo.Clients WHERE PostalCode = 98074" //Sql query
  "user"         -> "username",
  "password"     -> "*********",
))

//Read all data in table dbo.Clients
val collection = sqlContext.read.sqlDb(config)
collection.show()
```

### <a name="write-data-to-azure-sql-database-or-sql-server"></a>Azure SQL veritabanı veya SQL Server veri yazma
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._
 
// Aquire a DataFrame collection (val collection)

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "dbTable"      -> "dbo.Clients"
  "user"         -> "username",
  "password"     -> "*********"
))

import org.apache.spark.sql.SaveMode
collection.write.mode(SaveMode.Append).sqlDB(config)
```

### <a name="run-dml-or-ddl-query-in-azure-sql-database-or-sql-server"></a>DML veya DDL sorguyu Azure SQL veritabanı veya SQL Server çalıştırın
```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.query._
val query = """
              |UPDATE Customers
              |SET ContactName = 'Alfred Schmidt', City= 'Frankfurt'
              |WHERE CustomerID = 1;
            """.stripMargin

val config = Config(Map(
  "url"          -> "mysqlserver.database.windows.net",
  "databaseName" -> "MyDatabase",
  "user"         -> "username",
  "password"     -> "*********",
  "queryCustom"  -> query
))

sqlContext.SqlDBQuery(config)
```

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>AAD kimlik doğrulamasını kullanarak Azure SQL veritabanı için Spark'ı bağlama
Azure Active Directory (AAD) kimlik doğrulamasını kullanarak Azure SQL veritabanına bağlanabilirsiniz. Veritabanı Kullanıcıları ve SQL Server kimlik doğrulamasının bir alternatifi olarak kimlikleri merkezi olarak yönetmek için AAD kimlik doğrulaması kullanın.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>ActiveDirectoryPassword kimlik doğrulama modunu kullanarak bağlanma
#### <a name="setup-requirement"></a>Kurulum gereksinimi
İndirmeniz gerekmez ActiveDirectoryPassword kimlik doğrulama modunu kullanıyorsanız, [azure-activedirectory-kitaplığı-için-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve onun bağımlılıklarını ve bunları Java derleme yolu içerir.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"            -> "mysqlserver.database.windows.net",
  "databaseName"   -> "MyDatabase",
  "user"           -> "username ",
  "password"       -> "*********",
  "authentication" -> "ActiveDirectoryPassword",
  "encrypt"        -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

### <a name="connecting-using-access-token"></a>Erişim belirteci kullanarak bağlanma
#### <a name="setup-requirement"></a>Kurulum gereksinimi
Erişim belirteci tabanlı kimlik doğrulaması modunu kullanıyorsanız, indirmeniz gerekmez [azure-activedirectory-kitaplığı-için-java](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve onun bağımlılıklarını ve bunları Java derleme yolu içerir.

Bkz: [kullanımı Azure Active Directory kimlik doğrulamasını SQL veritabanı ile](sql-database-aad-authentication.md) erişim Azure SQL veritabanınıza belirteci alma hakkında bilgi edinmek için.

```scala
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

val config = Config(Map(
  "url"                   -> "mysqlserver.database.windows.net",
  "databaseName"          -> "MyDatabase",
  "accessToken"           -> "access_token ",
  "hostNameInCertificate" -> "*.database.windows.net",
  "encrypt"               -> "true"
))

val collection = sqlContext.read.SqlDB(config)
collection.show()
```

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Azure SQL veritabanı veya Bulk INSERT kullanılarak SQL Server veri yazma
Geleneksel jdbc Bağlayıcısı, Azure SQL veritabanı veya SQL Server-satır ekleme kullanarak verileri yazar. Toplu ekleme kullanarak SQL veritabanına veri yazmak için Spark SQL DB Bağlayıcısı için kullanabilirsiniz. Önemli ölçüde yazma performansını büyük veri kümeleri veya veri yükleme columnstore dizininin kullanıldığı tablolarına yüklenirken artırır.

```scala
import com.microsoft.azure.sqldb.spark.bulkcopy.BulkCopyMetadata
import com.microsoft.azure.sqldb.spark.config.Config
import com.microsoft.azure.sqldb.spark.connect._

/** 
  Add column Metadata.
  If not specified, metadata is automatically added
  from the destination table, which may suffer performance.
*/
var bulkCopyMetadata = new BulkCopyMetadata
bulkCopyMetadata.addColumnMetadata(1, "Title", java.sql.Types.NVARCHAR, 128, 0)
bulkCopyMetadata.addColumnMetadata(2, "FirstName", java.sql.Types.NVARCHAR, 50, 0)
bulkCopyMetadata.addColumnMetadata(3, "LastName", java.sql.Types.NVARCHAR, 50, 0)

val bulkCopyConfig = Config(Map(
  "url"               -> "mysqlserver.database.windows.net",
  "databaseName"      -> "MyDatabase",
  "user"              -> "username",
  "password"          -> "*********",
  "databaseName"      -> "zeqisql",
  "dbTable"           -> "dbo.Clients",
  "bulkCopyBatchSize" -> "2500",
  "bulkCopyTableLock" -> "true",
  "bulkCopyTimeout"   -> "600"
))

df.bulkCopyToSqlDB(bulkCopyConfig, bulkCopyMetadata)
//df.bulkCopyToSqlDB(bulkCopyConfig) if no metadata is specified.
```

## <a name="next-steps"></a>Sonraki adımlar
Henüz yapmadıysanız, Azure SQL veritabanı ve SQL Server için Spark Bağlayıcısı'nı indirme [azure sqldb spark GitHub deposu](https://github.com/Azure/azure-sqldb-spark) ve depodaki ek kaynakları keşfedin:

-   [Örnek Azure Databricks Not Defterleri](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Örnek betikler (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

İncelemek isteyebilirsiniz [Apache Spark SQL ve DataFrames veri kümeleri Kılavuzu](https://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure Databricks belgeleri](https://docs.microsoft.com/azure/azure-databricks/).

