---
title: "Öğretici: Azure Data Lake Store ' Azure SQL Data Warehouse'a veri yükleme | Microsoft Docs"
description: Verileri Azure Data Lake Store ' Azure SQL Data Warehouse'a yüklemek için PolyBase dış tabloları kullanın.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: 04676db3048cf747e9a20d91a404f29c6cfc6853
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306402"
---
# <a name="load-data-from-azure-data-lake-store-to-sql-data-warehouse"></a>Azure Data Lake Store yük verileri SQL veri ambarı
Verileri Azure Data Lake Store ' Azure SQL Data Warehouse'a yüklemek için PolyBase dış tabloları kullanın. Geçici sorguları, ADLS, depolanan veriler üzerinde çalıştırabilirsiniz, ancak en iyi performans için SQL veri ambarı'na verilerin içeri aktarılması önerilir.

> [!div class="checklist"]
> * Azure Data Lake Store ' yüklemek için gerekli veritabanı nesnelerini oluşturun.
> * Bir Azure Data Lake Store dizinine bağlanır.
> * Verileri Azure SQL veri ambarı'na yükleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="before-you-begin"></a>Başlamadan önce
Bu öğreticiye başlamadan önce, [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)’nun (SSMS) en yeni sürümünü indirin ve yükleyin.

Bu öğreticide çalıştırmak için ihtiyacınız vardır:

* Azure Active Directory hizmetten hizmete kimlik doğrulaması için kullanılacak uygulama. Oluşturmak için takip [Active directory kimlik doğrulaması](../data-lake-store/data-lake-store-authenticate-using-active-directory.md)

>[!NOTE] 
> İstemci Kimliğini, anahtar ve OAuth2.0 belirteç uç noktası değeri, Active Directory, Azure Data Lake için SQL veri ambarı'ndan bağlanmak için uygulamanızın ihtiyacınız var. Bu değerleri almak nasıl için Ayrıntılar yukarıdaki bağlantıya bulunur. Azure Active Directory Uygulama kaydı için istemci kimliği olarak uygulama Kimliğini kullanın.
> 

* Bir Azure SQL veri ambarı. Bkz: [oluşturma ve sorgu ve Azure SQL veri ambarı](create-data-warehouse-portal.md).

* Azure Data Lake Store. Bkz: [Azure Data Lake Store ile çalışmaya başlama](../data-lake-store/data-lake-store-get-started-portal.md). 

##  <a name="create-a-credential"></a>Bir kimlik bilgisi oluşturma
Azure Data Lake Store erişmek için kullanılan bir sonraki adımda, kimlik bilgisi gizli anahtarını şifrelemek için bir veritabanı ana anahtarı oluşturmanız gerekecektir. Daha sonra bir veritabanı kapsamlı AAD'de ayarlanmış hizmet sorumlusu kimlik bilgileri depolayan kimlik bilgisi, oluşturursunuz. Bu Windows Azure depolama BLOB'ları için bağlanmak için PolyBase kullanmış olduğunuz CREDENTIAL sözdizimi farklı olduğuna dikkat edin.

Azure Data Lake Store için bağlanmak için şunları yapmanız gerekir **ilk** Azure Data Lake kaynak için uygulama erişimi Azure Active Directory uygulaması oluşturma ve bir erişim anahtarı oluşturun. Yönergeler için [doğrulaması için Azure Data Lake Store kullanarak Active Directory](../data-lake-store/data-lake-store-authenticate-using-active-directory.md).

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

## <a name="create-the-external-data-source"></a>Dış veri kaynağı oluşturma
Bunu kullanın [CREATE EXTERNAL DATA SOURCE](/sql/t-sql/statements/create-external-data-source-transact-sql) konumu verileri depolamak için komutu. 

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

## <a name="configure-data-format"></a>Veri biçimi yapılandırma
ADLS verileri içeri aktarmak için dış dosya biçimini belirtmek gerekir. Bu nesne dosyaları ADLS, nasıl yazılır tanımlar.
Tam liste için T-SQL belgelerimize bakın [CREATE EXTERNAL FILE FORMAT](/sql/t-sql/statements/create-external-file-format-transact-sql)

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

## <a name="create-the-external-tables"></a>Dış tablolar oluşturma
Belirttiğiniz veri kaynağı ve dosya biçimini, dış tablolar oluşturmak hazır olursunuz. Dış tablolar, dış verilerle nasıl etkileşim değildir. Konum parametresi bir dosya veya dizin belirtebilirsiniz. Bir dizini belirtir, dizinin içindeki tüm dosyalar yüklenir.

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
Bir dış tablo oluşturmak kolaydır ancak ele alınması gereken bazı küçük farklar vardır.

Dış tablolar kesin türlü yapılır. Başka bir deyişle, her satır alınıyor verilerin tablo şeması tanımı karşılaması gerekir.
Bir satır şema tanımı eşleşmiyorsa yük satır reddedilir.

REJECT_TYPE ve REJECT_VALUE seçenekleri kaç satır veya verilerin yüzde son tablosunda bulunmalıdır tanımlamanıza olanak sağlar. Reddetme değere ulaşıncaya yükleme sırasında yükleme başarısız olur. Reddedilen satırların en yaygın nedeni bir şema tanımı uyumsuzluğu var. Örneğin, dosyadaki verileri bir dize olduğunda bir sütunu yanlış int şemasını belirtilmezse, her satır yüklemek başarısız olur.

 Azure Data Lake store, verilere erişimi denetlemek için rol tabanlı erişim denetimi (RBAC) kullanır. Bu hizmet sorumlusunu konumu parametresinde tanımlanan dizinleri ve dosyaları ve son dizin alt izni okuma izinlerinizin olduğunu anlamına gelir. Bu kimlik doğrulaması yapmak ve verileri yüklemek PolyBase sağlar. 

## <a name="load-the-data"></a>Verileri yükleme
Azure Data Lake Store kullanımdan verileri yüklemek için [CREATE TABLE AS SELECT (Transact-SQL)](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) deyimi. 

CTAS, yeni bir tablo oluşturur ve bir select deyiminin sonuçları ile doldurur. CTAS bir select deyiminin sonuçları aynı sütunlara ve veri türleri için yeni tablo tanımlar. Bir dış tablodan tüm sütunları seçerseniz, yeni bir tablo veri türleri, dış tablo ve sütunları çoğaltmasıdır.

Bu örnekte, DimProduct, dış tablo DimProduct_external adlı bir karma dağıtılmış tablo oluşturuyoruz.

```sql

CREATE TABLE [dbo].[DimProduct]
WITH (DISTRIBUTION = HASH([ProductKey]  ) )
AS
SELECT * FROM [dbo].[DimProduct_external]
OPTION (LABEL = 'CTAS : Load [dbo].[DimProduct]');
```


## <a name="optimize-columnstore-compression"></a>Columnstore sıkıştırması en iyi duruma getirme
Varsayılan olarak, SQL veri ambarı tablosu kümelenmiş columnstore dizini depolar. Yükleme tamamlandıktan sonra bazı veri satırları columnstore sıkıştırılmasının değil.  Çeşitli nedenlerle oluşabilir neden yoktur. Daha fazla bilgi için bkz. [columnstore dizinlerini Yönet](sql-data-warehouse-tables-index.md).

Sorgu performansı ve yük sonra columnstore sıkıştırması iyileştirmek için tüm satırları sıkıştırma columnstore dizinini zorlamak için tabloyu yeniden oluşturun.

```sql

ALTER INDEX ALL ON [dbo].[DimProduct] REBUILD;

```

## <a name="optimize-statistics"></a>İstatistikleri en iyi duruma getirme
Hemen sonra bir yükleme tek sütunlu istatistikler oluşturmak idealdir. İstatistikleri için bazı seçeneğiniz vardır. Örneğin, her sütunda tek sütunlu İstatistikler oluşturursanız, tüm İstatistikler yeniden uzun sürebilir. Bazı sütunlar sorgu koşullarda yapmayacağınız biliyorsanız, bu sütunlar üzerinde oluşturmayı istatistikleri atlayabilirsiniz.

Tek sütunlu istatistikler her tablonun her sütunu oluşturmaya karar verirseniz, saklı yordam kod örneği kullanabilirsiniz `prc_sqldw_create_stats` içinde [istatistikleri](sql-data-warehouse-tables-statistics.md) makalesi.

Aşağıdaki örnek istatistik oluşturmak için iyi bir başlangıç noktası var. Her sütunda bir boyut tablosuna ve olgu tabloları katılan her sütunda tek sütunlu İstatistikler oluşturur. Her zaman tek veya birden çok sütun istatistikleri diğer olgu tablo sütunları daha sonra ekleyebilirsiniz.

## <a name="achievement-unlocked"></a>Kilidi başarı!
Azure SQL Data Warehouse'a veri başarıyla yüklediniz. Harika bir iş çıkardınız!

## <a name="next-steps"></a>Sonraki adımlar 
Bu öğreticide, Azure Data Lake Store içinde depolanan verilerin yapısını tanımlamak için dış tablolar oluşturdunuz ve sonra verileri veri ambarınıza yüklemek için PolyBase CREATE TABLE AS SELECT deyimi kullanılır. 

Şu işlemleri yaptınız:
> [!div class="checklist"]
> * Azure Data Lake Store ' yüklemek için gereken oluşturulan veritabanı nesneleri.
> * Bir Azure Data Lake Store dizinine bağlı.
> * Azure SQL veri ambarı'na yüklenen verileri.
> 

Veri yükleme, SQL veri ambarı'nı kullanarak bir veri ambarı çözümü geliştirmek için ilk adımdır. Bizim geliştirme kaynaklara göz atın.

> [!div class="nextstepaction"]
>[SQL veri ambarı tabloları geliştirmeyi öğrenin](sql-data-warehouse-tables-overview.md)




