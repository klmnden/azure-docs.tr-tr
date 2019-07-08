---
title: Contoso perakende Azure SQL veri ambarı'na veri yükleme | Microsoft Docs
description: İki tablo Contoso perakende verileri Azure SQL Data Warehouse'a yüklemek için PolyBase ve T-SQL komutlarını kullanın.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: load data
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: eb52169fc522ba323f82c42d9505571b18f49f1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66244474"
---
# <a name="load-contoso-retail-data-to-azure-sql-data-warehouse"></a>Azure SQL veri ambarı contoso perakende veri yükleme

Bu öğreticide, iki tablo Contoso perakende verileri Azure SQL Data Warehouse'a yüklemek için PolyBase ve T-SQL komutlarını kullanmayı öğrenin. 

Bu öğreticide şunları yapacaksınız:

1. Azure blob Depolama'yı yüklemek için PolyBase yapılandırın
2. Veritabanınıza genel veri yükleme
3. Yükleme tamamlandıktan sonra iyileştirmeler gerçekleştirin.

## <a name="before-you-begin"></a>Başlamadan önce
Bu öğreticide çalıştırmak için SQL veri ambarı zaten bir Azure hesabı gerekir. Sağlanan bir veri ambarına sahip değilseniz, bkz. [SQL veri ambarı oluşturma ve sunucu düzeyinde güvenlik duvarı kuralı ayarlayın][Create a SQL Data Warehouse].

## <a name="1-configure-the-data-source"></a>1. Veri kaynağını yapılandırma
PolyBase, dış veri özniteliklerini ve konumunu tanımlamak için T-SQL dış nesneleri kullanır. Dış nesne tanımları SQL veri ambarı'nda depolanır. Verilerin harici olarak depolanır.

### <a name="11-create-a-credential"></a>1.1. Bir kimlik bilgisi oluşturma
**Bu adımı atlayın** Contoso genel verileri yüklüyorsanız. Herkesin erişilebildiğinden, ortak verilere güvenli erişim gerekmez.

**Bu adım atlamazsanız** kendi verileri yüklemek için bir şablon olarak Bu öğretici kullanıyorsanız. Bir kimlik bilgisi verilere erişmek için bir veritabanı kapsamlı kimlik bilgisi oluşturmak için aşağıdaki betiği kullanın ve veri kaynağının konumunu tanımlarken kullanın.

```sql
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- IDENTITY: Provide any string, it is not used for authentication to Azure storage.
-- SECRET: Provide your Azure storage account key.


CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential
WITH
    IDENTITY = 'user',
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- TYPE: HADOOP - PolyBase uses Hadoop APIs to access data in Azure blob storage.
-- LOCATION: Provide Azure storage account name and blob container name.
-- CREDENTIAL: Provide the credential created in the previous step.

CREATE EXTERNAL DATA SOURCE AzureStorage
WITH (
    TYPE = HADOOP,
    LOCATION = 'wasbs://<blob_container_name>@<azure_storage_account_name>.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
);
```

### <a name="12-create-the-external-data-source"></a>1.2. Dış veri kaynağı oluşturma
Bunu kullanın [CREATE EXTERNAL DATA SOURCE][CREATE EXTERNAL DATA SOURCE] veri veri türü ve konumu depolamak için komutu. 

```sql
CREATE EXTERNAL DATA SOURCE AzureStorage_west_public
WITH 
(  
    TYPE = Hadoop 
,   LOCATION = 'wasbs://contosoretaildw-tables@contosoretaildw.blob.core.windows.net/'
); 
```

> [!IMPORTANT]
> Azure blob depolama kapsayıcıları genel hale getirmek isterseniz, verileri veri merkezi dışına çıktığında veri sahibi olarak, veriler için çıkış ücretlerini ücretlendirileceğinizi unutmayın. 
> 
> 

## <a name="2-configure-data-format"></a>2. Veri biçimi yapılandırma
Veriler Azure blob depolama alanındaki metin dosyalarında depolanır ve her bir alan sınırlayıcı ile ayrılır. SSMS'de, aşağıdakini çalıştırarak [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT] metin dosyalarında verilerin biçimini belirtmek için komutu. Contoso sıkıştırılmamış verilerdir ve kanal ayrılmış.

```sql
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

## <a name="3-create-the-external-tables"></a>3. Dış tablolar oluşturma
Belirttiğiniz veri kaynağı ve dosya biçimini, dış tablolar oluşturmak hazırsınız. 

### <a name="31-create-a-schema-for-the-data"></a>3.1. Veriler için bir şema oluşturun.
Veritabanınızda Contoso verileri depolamak için bir yer oluşturmak için bir şema oluşturun.

```sql
CREATE SCHEMA [asb]
GO
```

### <a name="32-create-the-external-tables"></a>3.2. Harici tabloları oluşturun.
DimProduct ve FactOnlineSales dış tablolar oluşturmak için aşağıdaki betiği çalıştırın. Burada yaptığınız tüm sütun adlarını ve veri türlerini tanımlama ve Azure blob depolama dosyalarının biçimi ve konumu bağlama. Tanımı SQL veri ambarı'nda depolanır ve yine de Azure depolama Blobu'ndaki verilerdir.

**Konumu** parametre Azure depolama blobu kök klasöründe bir klasördür. Farklı bir klasöre her tablodur.

```sql
--DimProduct
CREATE EXTERNAL TABLE [asb].DimProduct (
    [ProductKey] [int] NOT NULL,
    [ProductLabel] [nvarchar](255) NULL,
    [ProductName] [nvarchar](500) NULL,
    [ProductDescription] [nvarchar](400) NULL,
    [ProductSubcategoryKey] [int] NULL,
    [Manufacturer] [nvarchar](50) NULL,
    [BrandName] [nvarchar](50) NULL,
    [ClassID] [nvarchar](10) NULL,
    [ClassName] [nvarchar](20) NULL,
    [StyleID] [nvarchar](10) NULL,
    [StyleName] [nvarchar](20) NULL,
    [ColorID] [nvarchar](10) NULL,
    [ColorName] [nvarchar](20) NOT NULL,
    [Size] [nvarchar](50) NULL,
    [SizeRange] [nvarchar](50) NULL,
    [SizeUnitMeasureID] [nvarchar](20) NULL,
    [Weight] [float] NULL,
    [WeightUnitMeasureID] [nvarchar](20) NULL,
    [UnitOfMeasureID] [nvarchar](10) NULL,
    [UnitOfMeasureName] [nvarchar](40) NULL,
    [StockTypeID] [nvarchar](10) NULL,
    [StockTypeName] [nvarchar](40) NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [AvailableForSaleDate] [datetime] NULL,
    [StopSaleDate] [datetime] NULL,
    [Status] [nvarchar](7) NULL,
    [ImageURL] [nvarchar](150) NULL,
    [ProductURL] [nvarchar](150) NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/DimProduct/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;

--FactOnlineSales
CREATE EXTERNAL TABLE [asb].FactOnlineSales 
(
    [OnlineSalesKey] [int]  NOT NULL,
    [DateKey] [datetime] NOT NULL,
    [StoreKey] [int] NOT NULL,
    [ProductKey] [int] NOT NULL,
    [PromotionKey] [int] NOT NULL,
    [CurrencyKey] [int] NOT NULL,
    [CustomerKey] [int] NOT NULL,
    [SalesOrderNumber] [nvarchar](20) NOT NULL,
    [SalesOrderLineNumber] [int] NULL,
    [SalesQuantity] [int] NOT NULL,
    [SalesAmount] [money] NOT NULL,
    [ReturnQuantity] [int] NOT NULL,
    [ReturnAmount] [money] NULL,
    [DiscountQuantity] [int] NULL,
    [DiscountAmount] [money] NULL,
    [TotalCost] [money] NOT NULL,
    [UnitCost] [money] NULL,
    [UnitPrice] [money] NULL,
    [ETLLoadID] [int] NULL,
    [LoadDate] [datetime] NULL,
    [UpdateDate] [datetime] NULL
)
WITH
(
    LOCATION='/FactOnlineSales/' 
,   DATA_SOURCE = AzureStorage_west_public
,   FILE_FORMAT = TextFileFormat
,   REJECT_TYPE = VALUE
,   REJECT_VALUE = 0
)
;
```

## <a name="4-load-the-data"></a>4. Verileri yükleme
Dış verilere erişmek için farklı yolu vardır.  Dış tablolara doğrudan buradan veri sorgulamak, verileri veri ambarına yeni tablolara yüklemek veya var olan veri ambarı tabloları dış veri ekleyin.  

### <a name="41-create-a-new-schema"></a>4.1. Yeni bir şema oluşturun
CTAS, veri içeren yeni bir tablo oluşturur.  İlk olarak, contoso veriler için bir şema oluşturun.

```sql
CREATE SCHEMA [cso]
GO
```

### <a name="42-load-the-data-into-new-tables"></a>4.2. Yeni tablolarına veri yükleme
Verileri Azure blob depolamadan veri ambarı tablosuna yüklemek için kullanmak [CREATE TABLE AS SELECT (Transact-SQL)][CREATE TABLE AS SELECT (Transact-SQL)] deyimi. CTAS yükleyerek oluşturduğunuz kesin türü belirtilmiş dış tabloları kullanır. Verileri yeni tablolara yüklemek için kullanın [CTAS][CTAS] her tablo ifadesi. 
 
CTAS, yeni bir tablo oluşturur ve bir select deyiminin sonuçları ile doldurur. CTAS bir select deyiminin sonuçları aynı sütunlara ve veri türleri için yeni tablo tanımlar. Bir dış tablodan tüm sütunları seçerseniz, yeni bir tablo veri türleri, dış tablo ve sütunları çoğaltmasını olacaktır.

Bu örnekte, boyut hem de karma dağıtılmış tablo olarak Olgu Tablosu oluşturun. 

```sql
SELECT GETDATE();
GO

CREATE TABLE [cso].[DimProduct]            WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[DimProduct]             OPTION (LABEL = 'CTAS : Load [cso].[DimProduct]             ');
CREATE TABLE [cso].[FactOnlineSales]       WITH (DISTRIBUTION = HASH([ProductKey]  ) ) AS SELECT * FROM [asb].[FactOnlineSales]        OPTION (LABEL = 'CTAS : Load [cso].[FactOnlineSales]        ');
```

### <a name="43-track-the-load-progress"></a>4.3 yük ilerlemesini izleyebilirsiniz.
Dinamik Yönetim görünümlerini (Dmv'ler) kullanarak yükleme ilerlemesini izleyebilirsiniz. 

```sql
-- To see all requests
SELECT * FROM sys.dm_pdw_exec_requests;

-- To see a particular request identified by its label
SELECT * FROM sys.dm_pdw_exec_requests as r
WHERE r.[label] = 'CTAS : Load [cso].[DimProduct]             '
      OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
;

-- To track bytes and files
SELECT
    r.command,
    s.request_id,
    r.status,
    count(distinct input_name) as nbr_files, 
    sum(s.bytes_processed)/1024/1024/1024 as gb_processed
FROM
    sys.dm_pdw_exec_requests r
    inner join sys.dm_pdw_dms_external_work s
        on r.request_id = s.request_id
WHERE 
    r.[label] = 'CTAS : Load [cso].[DimProduct]             '
    OR r.[label] = 'CTAS : Load [cso].[FactOnlineSales]        '
GROUP BY
    r.command,
    s.request_id,
    r.status
ORDER BY
    nbr_files desc,
    gb_processed desc;
```

## <a name="5-optimize-columnstore-compression"></a>5. Columnstore sıkıştırması en iyi duruma getirme
Varsayılan olarak, SQL veri ambarı tablosu kümelenmiş columnstore dizini depolar. Yükleme tamamlandıktan sonra bazı veri satırları columnstore sıkıştırılmasının değil.  Neden bu durum ortaya çıkabilir farklı neden vardır. Daha fazla bilgi için bkz. [columnstore dizinlerini Yönet][manage columnstore indexes].

Sorgu performansı ve yük sonra columnstore sıkıştırması iyileştirmek için tüm satırları sıkıştırma columnstore dizinini zorlamak için tabloyu yeniden oluşturun. 

```sql
SELECT GETDATE();
GO

ALTER INDEX ALL ON [cso].[DimProduct]               REBUILD;
ALTER INDEX ALL ON [cso].[FactOnlineSales]          REBUILD;
```

Columnstore dizinleri koruma ile ilgili daha fazla bilgi için bkz: [columnstore dizinlerini Yönet][manage columnstore indexes] makalesi.

## <a name="6-optimize-statistics"></a>6. İstatistikleri en iyi duruma getirme
Hemen sonra bir yükleme tek sütunlu istatistikler oluşturmak idealdir. Bazı sütunlar sorgu koşullarda olacak olmayan biliyorsanız, bu sütunlar üzerinde oluşturmayı istatistikleri atlayabilirsiniz. Her sütunda tek sütunlu İstatistikler oluşturursanız, tüm İstatistikler yeniden uzun sürebilir. 

Tek sütunlu istatistikler her tablonun her sütunu oluşturmaya karar verirseniz, saklı yordam kod örneği kullanabilirsiniz `prc_sqldw_create_stats` içinde [istatistikleri][statistics] makalesi.

Aşağıdaki örnek istatistik oluşturmak için iyi bir başlangıç noktası var. Her sütunda bir boyut tablosuna ve olgu tabloları katılan her sütunda tek sütunlu İstatistikler oluşturur. Her zaman tek veya birden çok sütun istatistikleri diğer olgu tablo sütunları daha sonra ekleyebilirsiniz.

```sql
CREATE STATISTICS [stat_cso_DimProduct_AvailableForSaleDate] ON [cso].[DimProduct]([AvailableForSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_BrandName] ON [cso].[DimProduct]([BrandName]);
CREATE STATISTICS [stat_cso_DimProduct_ClassID] ON [cso].[DimProduct]([ClassID]);
CREATE STATISTICS [stat_cso_DimProduct_ClassName] ON [cso].[DimProduct]([ClassName]);
CREATE STATISTICS [stat_cso_DimProduct_ColorID] ON [cso].[DimProduct]([ColorID]);
CREATE STATISTICS [stat_cso_DimProduct_ColorName] ON [cso].[DimProduct]([ColorName]);
CREATE STATISTICS [stat_cso_DimProduct_ETLLoadID] ON [cso].[DimProduct]([ETLLoadID]);
CREATE STATISTICS [stat_cso_DimProduct_ImageURL] ON [cso].[DimProduct]([ImageURL]);
CREATE STATISTICS [stat_cso_DimProduct_LoadDate] ON [cso].[DimProduct]([LoadDate]);
CREATE STATISTICS [stat_cso_DimProduct_Manufacturer] ON [cso].[DimProduct]([Manufacturer]);
CREATE STATISTICS [stat_cso_DimProduct_ProductDescription] ON [cso].[DimProduct]([ProductDescription]);
CREATE STATISTICS [stat_cso_DimProduct_ProductKey] ON [cso].[DimProduct]([ProductKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductLabel] ON [cso].[DimProduct]([ProductLabel]);
CREATE STATISTICS [stat_cso_DimProduct_ProductName] ON [cso].[DimProduct]([ProductName]);
CREATE STATISTICS [stat_cso_DimProduct_ProductSubcategoryKey] ON [cso].[DimProduct]([ProductSubcategoryKey]);
CREATE STATISTICS [stat_cso_DimProduct_ProductURL] ON [cso].[DimProduct]([ProductURL]);
CREATE STATISTICS [stat_cso_DimProduct_Size] ON [cso].[DimProduct]([Size]);
CREATE STATISTICS [stat_cso_DimProduct_SizeRange] ON [cso].[DimProduct]([SizeRange]);
CREATE STATISTICS [stat_cso_DimProduct_SizeUnitMeasureID] ON [cso].[DimProduct]([SizeUnitMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_Status] ON [cso].[DimProduct]([Status]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeID] ON [cso].[DimProduct]([StockTypeID]);
CREATE STATISTICS [stat_cso_DimProduct_StockTypeName] ON [cso].[DimProduct]([StockTypeName]);
CREATE STATISTICS [stat_cso_DimProduct_StopSaleDate] ON [cso].[DimProduct]([StopSaleDate]);
CREATE STATISTICS [stat_cso_DimProduct_StyleID] ON [cso].[DimProduct]([StyleID]);
CREATE STATISTICS [stat_cso_DimProduct_StyleName] ON [cso].[DimProduct]([StyleName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitCost] ON [cso].[DimProduct]([UnitCost]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureID] ON [cso].[DimProduct]([UnitOfMeasureID]);
CREATE STATISTICS [stat_cso_DimProduct_UnitOfMeasureName] ON [cso].[DimProduct]([UnitOfMeasureName]);
CREATE STATISTICS [stat_cso_DimProduct_UnitPrice] ON [cso].[DimProduct]([UnitPrice]);
CREATE STATISTICS [stat_cso_DimProduct_UpdateDate] ON [cso].[DimProduct]([UpdateDate]);
CREATE STATISTICS [stat_cso_DimProduct_Weight] ON [cso].[DimProduct]([Weight]);
CREATE STATISTICS [stat_cso_DimProduct_WeightUnitMeasureID] ON [cso].[DimProduct]([WeightUnitMeasureID]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CurrencyKey] ON [cso].[FactOnlineSales]([CurrencyKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_CustomerKey] ON [cso].[FactOnlineSales]([CustomerKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_DateKey] ON [cso].[FactOnlineSales]([DateKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_OnlineSalesKey] ON [cso].[FactOnlineSales]([OnlineSalesKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_ProductKey] ON [cso].[FactOnlineSales]([ProductKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_PromotionKey] ON [cso].[FactOnlineSales]([PromotionKey]);
CREATE STATISTICS [stat_cso_FactOnlineSales_StoreKey] ON [cso].[FactOnlineSales]([StoreKey]);
```

## <a name="achievement-unlocked"></a>Kilidi başarı!
Azure SQL veri ambarı'na genel verileri başarıyla yüklediniz. Harika bir iş çıkardınız!

Artık verilerinizi araştırmak için tabloları sorgulama başlayabilirsiniz. Marka başına toplam satış öğrenmek için aşağıdaki sorguyu çalıştırın:

```sql
SELECT  SUM(f.[SalesAmount]) AS [sales_by_brand_amount]
,       p.[BrandName]
FROM    [cso].[FactOnlineSales] AS f
JOIN    [cso].[DimProduct]      AS p ON f.[ProductKey] = p.[ProductKey]
GROUP BY p.[BrandName]
```

## <a name="next-steps"></a>Sonraki adımlar
Veri kümesini yüklemek için örneği çalıştırmak [tam Contoso perakende veri ambarı yükleme](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md) depodan Microsoft SQL Server örnekleri.

Geliştirme ile ilgili daha fazla ipucu için bkz. [SQL Veri Ambarı’nda geliştirmeye genel bakış][SQL Data Warehouse development overview].

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
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433
[Load the full Contoso Retail Data Warehouse]: https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/contoso-data-warehouse/readme.md
