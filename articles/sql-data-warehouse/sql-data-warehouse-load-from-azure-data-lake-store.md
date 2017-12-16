---
title: "Yük - SQL veri ambarı için Azure Data Lake Store | Microsoft Docs"
description: "Azure Data Lake Deposu'ndan veri Azure SQL Data Warehouse'a veri yüklemek için PolyBase dış tablolara kullanmayı öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 12/14/2017
ms.author: cakarst;barbkess
ms.openlocfilehash: a2a7d15eb51374b828d1d641e0e6754115f7aaf6
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2017
---
# <a name="load-data-from-azure-data-lake-store-into-sql-data-warehouse"></a>Azure Data Lake Deposu'ndan veri SQL Data Warehouse'a veri yükleme
Bu belge Polybase'i kullanarak Azure Data Lake deposu (ADLS) SQL Data Warehouse'a veri yüklemek için gereken tüm adımları sağlar.
Geçici sorguları dış tablolara kullanarak ADLS içinde depolanan veriler üzerinde çalıştırmak mümkün olmakla birlikte, en iyi uygulama olarak SQL Data Warehouse'a veri aktarma öneririz.

Bu öğreticide şunları öğreneceksiniz nasıl yapılır:

1. Azure Data Lake Deposu'ndan veri yüklemek için dış veritabanı nesneleri oluşturma.
2. Bir Azure Data Lake Store dizinine bağlanır.
3. Azure SQL Data Warehouse'a veri yükleme.

## <a name="before-you-begin"></a>Başlamadan önce
Bu öğretici çalıştırmak için gerekir:

* Azure Active Directory Hizmeti için kimlik doğrulaması için kullanılacak uygulama. Oluşturmak için izlemeniz [Active directory kimlik doğrulaması](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory)

>[!NOTE] 
> İstemci kimliği, anahtar ve OAuth2.0 belirteç uç noktası değeri, uygulamanızın Active Directory, Azure Data Lake SQL veri ambarından bağlanmak için gerekir. Bu değerleri alma ayrıntılarını yukarıdaki bağlantıyı ' dir.
>Not Azure Active Directory Uygulama kaydı için istemci kimliği olarak 'Uygulama kimliği' kullanın

* SQL Server Management Studio veya SQL Server veri araçları, SSMS karşıdan yüklemek ve bağlamak için bkz: [sorgu SSMS](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-query-ssms)

* Bir Azure SQL veri oluşturmak için bir izleyin deposu: https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-provision _

* Bir Azure Data Lake oluşturmak için bir izleyin Store,: https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal


###  <a name="create-a-credential"></a>Bir kimlik bilgisi oluşturma
Azure Data Lake Store erişmek için sonraki adımda kullanılan kimlik bilgileri gizli anahtarı şifrelemek için bir veritabanı ana anahtarı oluşturmanız gerekir.
AAD'de ayarlanmış hizmet asıl kimlik bilgilerini depolayan bir veritabanı kapsamlı kimlik bilgisi oluşturursunuz. Bu Windows Azure depolama BLOB'larını bağlamak için PolyBase kullanmış olduğunuz CREDENTIAL sözdizimi farklı olduğuna dikkat edin.
Azure Data Lake Store'a bağlanmak için yapmanız gerekir **ilk** Azure Active Directory uygulama oluşturmak, bir erişim anahtarı oluşturun ve Azure Data Lake kaynak uygulama erişimi verin. Bu adımları gerçekleştirmek için yönergeler konumlandırıldığını [burada](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-authenticate-using-active-directory).

```sql
-- A: Create a Database Master Key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.
-- For more information on Master Key: https://msdn.microsoft.com/en-us/library/ms174382.aspx?f=255&MSPPError=-2147217396

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Pass the client id and OAuth 2.0 Token Endpoint taken from your Azure Active Directory Application
-- SECRET: Provide your AAD Application Service Principal key.
-- For more information on Create Database Scoped Credential: https://msdn.microsoft.com/en-us/library/mt270260.aspx

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


### <a name="create-the-external-data-source"></a>Dış veri kaynağı oluşturun
Bu [dış veri kaynağı oluştur] [ CREATE EXTERNAL DATA SOURCE] verilerin konumu depolamak için komutu. Azure portalında ADL URI bulmak için Azure Data Lake Store için gidin ve sonra Essentials masasında bakın.

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
ADLS veri almak için dış dosya biçimini belirtmeniz gerekir. Bu komut, verilerinizi tanımlamak için biçim özgü seçenek vardır.
Tam bir listesi için T-SQL belgelerimize bakın [oluşturmak dış dosya biçimi][CREATE EXTERNAL FILE FORMAT]

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

 Azure Data Lake deposu, verilere erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanır. Başka bir deyişle, hizmet sorumlusu konumu parametresinde tanımlanan dizinlere ve son dizin ve dosyaların çocuklar için okuma iznine sahip olmalıdır. Bu kimlik doğrulaması ve bu verileri okuma yüklemek PolyBase sağlar. 

## <a name="load-the-data"></a>Verileri yükleme
Azure Data Lake Store kullanımdan veri yüklemek için [CREATE TABLE AS SELECT (Transact-SQL)] [ CREATE TABLE AS SELECT (Transact-SQL)] deyimi. 

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
Varsayılan olarak, SQL Data Warehouse kümelenmiş columnstore dizini tablo depolar. Yükleme tamamlandıktan sonra bazı veriler satır columnstore sıkıştırılır değil.  Çeşitli nedenlerle oluşabilir neden yoktur. Daha fazla bilgi için bkz: [columnstore dizinleri yönetmek][manage columnstore indexes].

Sorgu performansı ve yük sonra columnstore sıkıştırma iyileştirmek için tüm satırların sıkıştırılacak columnstore dizinini zorlamak için tabloyu yeniden oluşturun.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

Columnstore dizinleri koruma ile ilgili daha fazla bilgi için bkz: [columnstore dizinleri yönetmek] [ manage columnstore indexes] makalesi.

## <a name="optimize-statistics"></a>İstatistikleri en iyi duruma getirme
Bir yük hemen sonra tek sütunlu istatistikler oluşturmak en iyisidir. İstatistikleri için bazı seçeneğiniz vardır. Örneğin, her sütunda tek sütunlu İstatistikler oluşturursanız, tüm istatistikleri yeniden oluşturmak için uzun zaman alabilir. Belirli sütunları sorgu koşullarında yapmayacağınız biliyorsanız, bu sütunlarda oluşturma istatistikleri atlayabilirsiniz.

Tek sütunlu istatistikler her tablonun her sütunu üzerinde oluşturmaya karar verirseniz, saklı yordam kod örneği kullanabilirsiniz `prc_sqldw_create_stats` içinde [istatistikleri] [ statistics] makalesi.

Aşağıdaki istatistikler oluşturmak için iyi bir başlangıç noktası örnektir. Her sütunun Boyut tablosuna ve olgu tabloları katılan her sütunda tek sütunlu İstatistikler oluşturur. Her zaman tek veya birden çok sütun istatistikleri diğer olgu tablo sütunları daha sonra ekleyebilirsiniz.


## <a name="achievement-unlocked"></a>Kilidi başarı!
Azure SQL Data Warehouse'a veri başarıyla yüklemiş olduğunuz. Harika iş!

## <a name="next-steps"></a>Sonraki Adımlar
Veri yükleme SQL Data Warehouse kullanarak bir veri ambarı çözüm geliştirmek için ilk adımdır. Geliştirme KAYNAKLARIMIZI kontrol [tabloları](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-overview) ve [T-SQL](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-develop-loops.md).


<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Load data into SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[manage columnstore indexes]: sql-data-warehouse-tables-index.md
[Statistics]: sql-data-warehouse-tables-statistics.md
[CTAS]: sql-data-warehouse-develop-ctas.md
[label]: sql-data-warehouse-develop-label.md

<!--MSDN references-->
[CREATE EXTERNAL DATA SOURCE]: https://msdn.microsoft.com/library/dn935022.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE TABLE AS SELECT (Transact-SQL)]: https://msdn.microsoft.com/library/mt204041.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[REBUILD]: https://msdn.microsoft.com/library/ms188388.aspx

<!--Other Web references-->
[Microsoft Download Center]: http://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
