---
title: "Öğretici: Azure SQL veri ambarı için Azure Data Lake Deposu'ndan veri yükleme | Microsoft Docs"
description: Azure Data Lake Deposu'ndan veri Azure SQL Data Warehouse'a veri yüklemek için PolyBase dış tabloları kullanın.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: c6030d1951c22dddfe6df01225c63cf503a370ac
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="load-data-from-azure-data-lake-store-to-sql-data-warehouse"></a>Azure Data Lake Deposu'ndan veri SQL Data Warehouse'a veri yükleme
Azure Data Lake Deposu'ndan veri Azure SQL Data Warehouse'a veri yüklemek için PolyBase dış tabloları kullanın. ADLS içinde depolanan veriler üzerinde geçici sorguları çalıştırabilirsiniz, ancak en iyi performans için SQL veri ambarına veri alma öneririz.

> [!div class="checklist"]
> * Azure Data Lake Deposu'ndan veri yüklemek için gerekli veritabanı nesnelerini oluşturun.
> * Bir Azure Data Lake Store dizinine bağlanır.
> * Azure SQL Data Warehouse'a veri yükleme.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="before-you-begin"></a>Başlamadan önce
Bu öğreticiye başlamadan önce, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Bu öğretici çalıştırmak için gerekir:

* Azure Active Directory Hizmeti için kimlik doğrulaması için kullanılacak uygulama. Oluşturmak için izlemeniz [Active directory kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)

>[!NOTE] 
> İstemci kimliği, anahtar ve OAuth2.0 belirteç uç noktası değeri, uygulamanızın Active Directory, Azure Data Lake SQL veri ambarından bağlanmak için gerekir. Bu değerleri alma ayrıntılarını yukarıdaki bağlantıyı ' dir. Azure Active Directory Uygulama kaydı için istemci kimliği olarak uygulama kimliği kullanın.
> 

* Bir Azure SQL veri ambarı. Bkz: [oluşturma ve sorgu ve Azure SQL Data Warehouse](create-data-warehouse-portal.md).

* Azure Data Lake deposu. Bkz: [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md). 

##  <a name="create-a-credential"></a>Bir kimlik bilgisi oluşturma
Azure Data Lake Store erişmek için sonraki adımda kullanılan kimlik bilgileri gizli anahtarı şifrelemek için bir veritabanı ana anahtarı oluşturmanız gerekir. Bir veritabanı kapsamlı AAD'de ayarlanmış hizmet asıl kimlik bilgilerini depolayan kimlik bilgileri, oluşturursunuz. Bu Windows Azure depolama BLOB'larını bağlamak için PolyBase kullanmış olduğunuz CREDENTIAL sözdizimi farklı olduğuna dikkat edin.

Azure Data Lake Store'a bağlanmak için yapmanız gerekir **ilk** Azure Active Directory uygulama oluşturmak, bir erişim anahtarı oluşturun ve Azure Data Lake kaynak uygulama erişimi verin. Yönergeler için bkz: [Azure Data Lake deposu kullanarak Active Directory kimlik doğrulama](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/library/mt270260.aspx

CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '<client_id>@<OAuth_2.0_Token_EndPoint>',
    SECRET = '<key>'
;

-- It should look something like this:
CREATE DATABASE SCOPED CREDENTIAL ADLCredential
WITH
    IDENTITY = '536540b4-4239-45fe-b9a3-629f97591c0c@https://login.microsoftonline.com/42f988bf-85f1-41af-91ab-2d2cd011da47/oauth2/token',
    SECRET = 'BjdIlmtKp4Fpyh9hIvr8HJlUida/seM5kQ3EpLAmeDI='
;
```

## <a name="create-the-external-data-source"></a>Dış veri kaynağı oluşturun
Bu [dış veri kaynağı oluştur](/sql/t-sql/statements/create-external-data-source-transact-sql) verilerin konumu depolamak için komutu. 

```sql
-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure Data Lake Store.
-- LOCATION: Provide Azure Data Lake accountname and URI
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
WITH (
    TYPE = HADOOP,
    LOCATION = 'adl://<AzureDataLake account_name>.azuredatalakestore.net',
    CREDENTIAL = ADLCredential
);
```

## <a name="configure-data-format"></a>Veri biçimini yapılandırın
ADLS veri almak için dış dosya biçimini belirtmeniz gerekir. Bu nesne, dosyaları ADLS içinde nasıl yazılır tanımlar.
Tam liste için bizim T-SQL belgelerine bakın [oluşturmak dış dosya biçimi](/sql/t-sql/statements/create-external-file-format-transact-sql)

```sql
-- D: Create an external file format
-- FIELD_TERMINATOR: Marks the end of each field (column) in a delimited text file
-- STRING_DELIMITER: Specifies the field terminator for data of type string in the text-delimited file.
-- DATE_FORMAT: Specifies a custom format for all date and time data that might appear in a delimited text file.
-- Use_Type_Default: Store missing values as default for datatype.

CREATE EXTERNAL FILE FORMAT TextFileFormat
WITH
(   FORMAT_TYPE = DELIMITEDTEXT
,    FORMAT_OPTIONS    (   FIELD_TERMINATOR = '|'
                    ,    STRING_DELIMITER = ''
                    ,    DATE_FORMAT         = 'yyyy-MM-dd HH:mm:ss.fff'
                    ,    USE_TYPE_DEFAULT = FALSE
                    )
);
```

## <a name="create-the-external-tables"></a>Dış tabloları oluşturma
Veri kaynağı ve dosya biçimi belirttiğiniz, dış tablo oluşturmak hazır olursunuz. Dış tablolar dış veri ile nasıl etkileşim değildir. Konum parametresi bir dosya veya dizin belirtebilirsiniz. Bir dizin belirtiyorsa, dizin içindeki tüm dosyalar yüklenir.

```sql
-- D: Create an External Table
-- LOCATION: Folder under the ADLS root folder.
-- DATA_SOURCE: Specifies which Data Source Object to use.
-- FILE_FORMAT: Specifies which File Format Object to use
-- REJECT_TYPE: Specifies how you want to deal with rejected rows. Either Value or percentage of the total
-- REJECT_VALUE: Sets the Reject value based on the reject type.

-- DimProduct
CREATE EXTERNAL TABLE [dbo].[DimProduct_external] (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL
)
WITH
(
    LOCATION='/DimProduct/'
,   DATA_SOURCE = AzureDataLakeStore
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

```

## <a name="external-table-considerations"></a>Dış tablo konuları
Bir dış tablo oluşturmak kolaydır, ancak ele alınması gereken bazı küçük farklar vardır.

Dış tablolara kesin türü belirtilmiş. Başka bir deyişle, her alınan veri satırının tablo şeması tanımı karşılaması gerekir.
Bir satır şema tanımı eşleşmiyorsa, satır yüklerinin reddedilir.

REJECT_TYPE ve REJECT_VALUE seçenekleri, satır sayısını veya veri yüzdesini son tablosunda bulunmalıdır tanımlamanıza olanak sağlar. Reddetme değerine ulaşılana yüklenmesi sırasında yükleme başarısız olur. Reddedilen satır en yaygın nedeni bir şema tanımı eşleşmemesidir. Örneğin, veri dosyasındaki bir dize olduğunda bir sütunu int şeması yanlış verilirse, her satıra yüklemek başarısız olur.

 Azure Data Lake deposu, verilere erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanır. Başka bir deyişle, hizmet sorumlusu konumu parametresinde tanımlanan dizinlere ve son dizin ve dosyaların çocuklar için okuma iznine sahip olmalıdır. Bu kimlik doğrulaması ve bu verileri yüklemek PolyBase sağlar. 

## <a name="load-the-data"></a>Verileri yükleme
Azure Data Lake Store kullanımdan veri yüklemek için [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) deyimi. 

CTAS yeni bir tablo oluşturur ve bir select deyimi sonuçları ile doldurur. CTAS select deyimi sonuçları olarak aynı sütunları ve veri türleri için yeni tabloyu tanımlar. Dış bir tablodaki tüm sütunları seçin, yeni bir tablo sütunları ve dış tablosunda veri türlerini çoğaltmasını olur.

Bu örnekte, dış tablo DimProduct_external DimProduct adlı karma bir dağıtılmış tablo oluşturuyoruz.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Columnstore sıkıştırma en iyi duruma getirme
Varsayılan olarak, SQL Data Warehouse kümelenmiş columnstore dizini tablo depolar. Yükleme tamamlandıktan sonra bazı veriler satır columnstore sıkıştırılır değil.  Çeşitli nedenlerle oluşabilir neden yoktur. Daha fazla bilgi için bkz: [columnstore dizinleri yönetmek](sql-data-warehouse-tables-index.md).

Sorgu performansı ve yük sonra columnstore sıkıştırma iyileştirmek için tüm satırların sıkıştırılacak columnstore dizinini zorlamak için tabloyu yeniden oluşturun.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>İstatistikleri en iyi duruma getirme
Bir yük hemen sonra tek sütunlu istatistikler oluşturmak en iyisidir. İstatistikleri için bazı seçeneğiniz vardır. Örneğin, her sütunda tek sütunlu İstatistikler oluşturursanız, tüm istatistikleri yeniden oluşturmak için uzun zaman alabilir. Belirli sütunları sorgu koşullarında yapmayacağınız biliyorsanız, bu sütunlarda oluşturma istatistikleri atlayabilirsiniz.

Tek sütunlu istatistikler her tablonun her sütunu üzerinde oluşturmaya karar verirseniz, saklı yordam kod örneği kullanabilirsiniz `prc_sqldw_create_stats` içinde [istatistikleri](sql-data-warehouse-tables-statistics.md) makalesi.

Aşağıdaki istatistikler oluşturmak için iyi bir başlangıç noktası örnektir. Her sütunun Boyut tablosuna ve olgu tabloları katılan her sütunda tek sütunlu İstatistikler oluşturur. Her zaman tek veya birden çok sütun istatistikleri diğer olgu tablo sütunları daha sonra ekleyebilirsiniz.

## <a name="achievement-unlocked"></a>Kilidi başarı!
Azure SQL Data Warehouse'a veri başarıyla yüklemiş olduğunuz. Harika iş!

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide Azure Data Lake Store'da depolanan verilerin yapısını tanımlamak için dış tabloları oluşturulur ve ardından veri ambarına veri yüklemek için PolyBase CREATE TABLE AS SELECT deyimi kullanılır. 

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Azure Data Lake Deposu'ndan veri yüklemek için gereken oluşturulan veritabanı nesneleri.
> * Bir Azure Data Lake Store dizinine bağlı.
> * Yüklenen verilerin Azure SQL veri ambarında.
> 

Veri yükleme SQL Data Warehouse kullanarak bir veri ambarı çözüm geliştirmek için ilk adımdır. Bizim geliştirme kaynaklara gözatın.

> [!div class="nextstepaction"]
>[SQL veri ambarı tablolarda geliştirmeyi öğrenin](sql-data-warehouse-tables-overview.md)




