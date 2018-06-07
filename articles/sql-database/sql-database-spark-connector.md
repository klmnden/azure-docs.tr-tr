---
title: Spark Azure SQL Database ve SQL Server bağlayıcısıyla | Microsoft Docs
description: Azure SQL Database ve SQL Server için Spark Bağlayıcısı'nı kullanmayı öğrenin
services: sql-database
author: allenwux
manager: craigg
ms.service: sql-database
ms.custom: ''
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: xiwu
ms.openlocfilehash: a422f65097466e4bbe5740c449d3ccf88701802b
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34650171"
---
# <a name="accelerate-real-time-big-data-analytics-with-spark-connector-for-azure-sql-database-and-sql-server"></a>Azure SQL Database ve SQL Server için Spark Bağlayıcısı ile gerçek zamanlı büyük veri analizi hızlandırmak

Spark bağlayıcı Azure SQL Database ve SQL Server için SQL veritabanları, Azure SQL Database ve SQL Server, Spark işleri için giriş veri kaynağı veya çıkış veri havuzu olarak davranmak üzere de dahil olmak üzere sağlar. Büyük veri analizini gerçek zamanlı işlem verileri kullanan ve sonuçlarını geçici sorgular veya Raporlama kalıcı imkan tanır. Yerleşik JDBC Bağlayıcısı ile karşılaştırıldığında, bu bağlayıcı Ekle verilerini SQL veritabanlarına toplu olanağı sağlar. Bu satır temelinde ekleme 20 x daha hızlı performans 10 x ile daha iyi performans gösterir. Azure SQL Database ve SQL Server için Spark Bağlayıcısı AAD kimlik doğrulamasını da destekler. Azure AAD hesabınızı kullanarak Databricks güvenli bir şekilde, Azure SQL veritabanına bağlanma sağlar. Bu, yerleşik JDBC bağlayıcısıyla benzer arabirimleri sağlar. Bu yeni bağlayıcıyı kullanmak için var olan Spark işleri geçirmeyi kolaydır.

## <a name="download"></a>İndirme
Başlamak için Spark SQL DB bağlayıcısından yüklenecek [azure sqldb spark deposu](https://github.com/Azure/azure-sqldb-spark) github'da.

## <a name="official-supported-versions"></a>Resmi desteklenen sürümleri

| Bileşen                            |Sürüm                  |
| :----------------------------------- | :---------------------- |
| Apache Spark                         |2.0.2 veya üzeri           |
| Scala                                |2.10 veya üzeri            |
| SQL Server için Microsoft JDBC Sürücüsü |6.2 veya üstü             |
| Microsoft SQL Server                 |SQL Server 2008 veya üstü |
| Azure SQL Database                   |Desteklenen                |

Spark bağlayıcı Azure SQL Database ve SQL Server için SQL Server'ın SQL veritabanlarının ve Spark alt düğümler arasında verileri taşımak için Microsoft JDBC sürücüsü kullanır:
 
Veri akışı aşağıdaki gibidir:
1. Spark ana düğümünün SQL Server veya Azure SQL veritabanına bağlanır ve belirli bir tablo veya belirli bir SQL sorgusunu kullanarak verileri yükler
2. Spark ana düğümünün alt düğümleri dönüştürme için veri dağıtır. 
3. Çalışan düğümüne SQL Server veya Azure SQL veritabanına bağlanır ve veritabanına veri yazar. Kullanıcı satır temelinde ekleme kullanın veya ekleme toplu seçebilirsiniz.

Aşağıdaki diyagramda, veri akışı gösterilmektedir.

   ![architecture](./media/sql-database-spark-connector/architecture.png)

### <a name="build-the-spark-to-sql-db-connector"></a>Spark SQL DB bağlayıcısı oluşturun
Şu anda bağlayıcı proje maven kullanır. Bağımlılıklar olmadan bağlayıcısı oluşturmak için çalıştırabilirsiniz:

- mvn temiz paketi
- Yayın klasöründen JAR en son sürümlerini indirme
- SQL DB Spark JAR Ekle

## <a name="connect-spark-to-sql-db-using-the-connector"></a>Spark Bağlayıcısı'nı kullanarak SQL Veritabanına bağlanın
Azure SQL Database veya SQL Server Spark işlerden bağlanmak, okuma veya veri yazma. DML veya DDL sorgusu de, bir Azure SQL database veya SQL Server veritabanı da çalıştırabilirsiniz.

### <a name="read-data-from-azure-sql-database-or-sql-server"></a>Azure SQL Database veya SQL Server verilerini okuma

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
### <a name="reading-data-from-azure-sql-database-or-sql-server-with-specified-sql-query"></a>Belirtilen SQL sorgusu ile Azure SQL Database veya SQL Server verileri okuma
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

### <a name="write-data-to-azure-sql-database-or-sql-server"></a>Azure SQL Database veya SQL Server için veri yazma
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

### <a name="run-dml-or-ddl-query-in-azure-sql-database-or-sql-server"></a>Azure SQL Database veya SQL Server DML veya DDL sorgu çalıştırma
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

## <a name="connect-spark-to-azure-sql-database-using-aad-authentication"></a>Spark AAD kimlik doğrulaması kullanarak Azure SQL veritabanı için bağlantı
Azure SQL veritabanına Azure Active Directory (AAD) kimlik doğrulaması kullanarak bağlanabilir. Veritabanı Kullanıcıları ve SQL Server kimlik doğrulaması için bir alternatif olarak kimlikleri merkezi olarak yönetmek için AAD kimlik doğrulaması kullanın.
### <a name="connecting-using-activedirectorypassword-authentication-mode"></a>ActiveDirectoryPassword kimlik doğrulama modunu kullanarak bağlanma
#### <a name="setup-requirement"></a>Kurulum gereksinimi
Karşıdan yüklemeniz ActiveDirectoryPassword kimlik doğrulama modu kullanıyorsanız, [azure-Active Directory-library--java için](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve onun bağımlılıklarını ve Java derleme yolu içerir.

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
Karşıdan yüklemek gereken erişim belirteç tabanlı kimlik doğrulama modu kullanıyorsanız, [azure-Active Directory-library--java için](https://github.com/AzureAD/azure-activedirectory-library-for-java) ve onun bağımlılıklarını ve Java derleme yolu içerir.

Bkz: [kullanım Azure Active Directory kimlik doğrulaması kimlik doğrulaması için SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) erişim Azure SQL veritabanınıza belirteci alma hakkında bilgi için.

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

## <a name="write-data-to-azure-sql-database-or-sql-server-using-bulk-insert"></a>Azure SQL database veya Bulk INSERT kullanarak SQL Server için veri yazma
Geleneksel jdbc bağlayıcı Azure SQL veritabanı veya satır temelinde ekleme kullanarak SQL Server veri yazar. Toplu ekleme kullanarak SQL veritabanına veri yazmak için Spark SQL DB bağlayıcıya kullanabilirsiniz. Önemli ölçüde yazma performansı büyük veri kümeleri veya veri sütun deposu dizini kullanıldığı tablolara yüklenirken iyileştirir.

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
Henüz yapmadıysanız, Azure SQL Database ve SQL Server için Spark Bağlayıcısı'nı indirin [azure sqldb spark GitHub deposunu](https://github.com/Azure/azure-sqldb-spark) ve depodaki ek kaynakları araştırın:

-   [Örnek Azure Databricks dizüstü bilgisayarlar](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/notebooks)
- [Örnek komut dosyaları (Scala)](https://github.com/Azure/azure-sqldb-spark/tree/master/samples/scripts)

Gözden geçirmek isteyebilirsiniz [Apache Spark SQL, DataFrames ve veri kümeleri Kılavuzu](http://spark.apache.org/docs/latest/sql-programming-guide.html) ve [Azure Databricks belgelerine](https://docs.microsoft.com/azure/azure-databricks/).

