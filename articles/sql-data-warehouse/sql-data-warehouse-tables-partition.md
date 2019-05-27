---
title: Azure SQL Data warehouse'da tablo bölümleme | Microsoft Docs
description: Öneriler ve Azure SQL Data Warehouse'da tablo bölümleri kullanımına ilişkin örnekler.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 03/18/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: af9fa49d274036888fd266f8983c523a3b077cbd
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851519"
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>SQL Data warehouse'da tablo bölümleme
Öneriler ve Azure SQL Data Warehouse'da tablo bölümleri kullanımına ilişkin örnekler.

## <a name="what-are-table-partitions"></a>Tablo bölümleri nelerdir?
Tablo bölümleri, verilerinizi veri daha küçük gruplar halinde ayırmak etkinleştirin. Çoğu durumda, bir tarih sütunu üzerinde tablo bölümleri oluşturulur. Bölümleme tüm SQL veri ambarı tablo türlerinde desteklenir. Kümelenmiş columnstore, kümelenmiş dizin ve yığın dahil. Bölümleme karma veya hepsini hem de dahil olmak üzere tüm dağıtım türlerinde desteklenir.  

Bölümleme veri Bakım ve sorgu performansını yararlı olabilir. Yalnızca bir tane veya her ikisi de yarar verilerin nasıl yüklendiği ve bölümleme yalnızca bir sütun üzerinde yapılabilir olduğundan aynı sütuna iki amaçları için kullanılıp kullanılamayacağını bağlıdır.

### <a name="benefits-to-loads"></a>Yükleri yararları
SQL veri ambarı'nda bölümleme yararı, bölüm silme işlemi kullanarak veri yükleme, değiştirme ve birleştirme performansı ve verimliliği artırmak için kullanılabilmesidir. Çoğu durumda veriler yakından verileri veritabanına yüklendiği sırayı bağlı bir tarih sütunu bölümlenir. Bölümleri korumak için kullanmanın yararlarından biri, işlem günlüğü mahal vermemek. Yükleme işleminiz sırasında bölümleme kullanarak yalnızca ekleme, güncelleştirme veya veri silme küçük bir düşünce ve çaba, en kolay yaklaşım olabilir, ancak önemli ölçüde performansını iyileştirebilir.

Bölüm değiştirme hızlı bir şekilde kaldırmak veya bir tablonun bölümünü değiştirmek için kullanılabilir.  Örneğin, bir satış olgu tablosunun son 36 ay için yalnızca veri içerebilir. Her ayın sonunda, satış verilerini eski ayın tablosundan silindi.  Bu veriler eski bir ay için verileri silmek için delete deyimini kullanarak silinemedi. Ancak, büyük miktarda veri-satır silme bildirimi siliniyor çok fazla zaman alabileceğini yanı bir sorun yaşanırsa geri almak için uzun süren büyük işlemi riskini oluşturun. En eski veri bölümünü bırak daha iyi bir yaklaşımdır. Burada, tek tek satırları silme saat ele geçirebilir bölüm silme saniye sürebilir.

### <a name="benefits-to-queries"></a>Sorgular için avantajları
Bölümleme de sorgu performansını artırmak için kullanılabilir. Bölümlenmiş verileri için bir filtre uygulayan bir sorgu yalnızca uygun bölüm tarama sınırlayabilirsiniz. Filtreleme, bu yöntem, tam tablo taraması önlemek ve yalnızca küçük bir alt kümesi veri tarama. Kümelenmiş columnstore dizinleri sunulmasıyla, koşul eleme performans avantajlarını daha az kullanışlıdır, ancak bazı durumlarda olabilir bir yararı, sorgular. Satış Olgu Tablosu satış tarih alanı kullanarak 36 ay bölümlenmiş, ardından bu filtre satış tarihinde sorgular, örneğin, filtre eşleşmeyen bölümler aranmasını atlayabilirsiniz.

## <a name="sizing-partitions"></a>Boyutlandırma bölümleri
Bölümleme bazı senaryolarda performansı artırmak için kullanılabilse de içeren bir tablo oluşturma **çok fazla** bölümleri bazı koşullarda performans zor.  Bu sorunlar, kümelenmiş columnstore tablolarının için özellikle doğrudur. Yardımcı olması için bölümleme için bölümleme kullanmak ne zaman ve oluşturulacak bölüm sayısı anlamak önemlidir. Kaç bölümlerin çok fazla dair hızlı sabit kural yoktur, aynı anda yükleniyor, verilerinizi ve kaç bölümler bağlıdır. Başarılı bir bölümleme düzeni onlarca, yüzlerce, binlerce bölümler genellikle sahiptir.

Bölüm oluştururken **kümelenmiş columnstore** tablolar, satır sayısı her bölüme ait göz önünde bulundurun. En iyi sıkıştırma ve kümelenmiş columnstore tablolarının performans için en az bir dağıtım ve bölüm başına 1 milyon satır gereklidir. Bölümler oluşturulmadan önce SQL veri ambarı her tablo zaten dağıtılmış 60 veritabanına böler. Arka planda oluşturulan dağıtımların yanı sıra, herhangi bir tabloya eklenen bölümleme olduğu. Bu örnekte, satış Olgu Tablosu 36 aylık bölümler yer alan ve SQL veri ambarı 60 dağıtım düşünüldüğünde, tüm ay yerleştirildiğinde ardından satış Olgu Tablosu ayda 60 milyon satır veya 2.1 milyar satıra içermelidir kullanıyor. Önerilen en düşük bölüm başına satır sayısı daha az bir tablo içeren bölüm başına satır sayısını artırmak için daha az bölümler kullanmayı düşünün. Daha fazla bilgi için [dizin](sql-data-warehouse-tables-index.md) makalesini cluster columnstore dizinlerine kalitesini değerlendirmek sorguları içerir.

## <a name="syntax-differences-from-sql-server"></a>SQL Server'dan söz dizimi farklılıkları
SQL veri ambarı, SQL Server basit olan bölümleri tanımlamak için bir yol sunar. SQL Server'da olduğu gibi SQL veri ambarı'nda bölümleme işlevleri ve düzenleri kullanılmaz. Bunun yerine, yapmak için ihtiyacınız olan bölümlenmiş sütun ile sınır noktaları belirleyin. Bölümleme sözdizimi SQL Server'dan biraz farklı olabilir, ancak temel kavramları aynıdır. SQL Server ve SQL veri ambarı aralıklı bölüm olabilecek tablo başına bir bölüm sütunu destekler. Bölümleme hakkında daha fazla bilgi için bkz. [bölümlenmiş tablolar ve dizinler](/sql/relational-databases/partitions/partitioned-tables-and-indexes).

Aşağıdaki örnekte [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) OrderDateKey sütun Factınternetsales tablosunda bölümlemek için deyimi:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
    [ProductKey]            int          NOT NULL
,   [OrderDateKey]          int          NOT NULL
,   [CustomerKey]           int          NOT NULL
,   [PromotionKey]          int          NOT NULL
,   [SalesOrderNumber]      nvarchar(20) NOT NULL
,   [OrderQuantity]         smallint     NOT NULL
,   [UnitPrice]             money        NOT NULL
,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

## <a name="migrating-partitioning-from-sql-server"></a>SQL Server'dan bölümleme geçirme
SQL Server bölüm tanımları yalnızca SQL veri ambarı'na geçirmek için:

- SQL Server ortadan [bölüm düzeni](/sql/t-sql/statements/create-partition-scheme-transact-sql).
- Ekleme [bölümleme işlevinin](/sql/t-sql/statements/create-partition-function-transact-sql) tanımına tablo oluştur.

Bölümlenmiş bir tablodaki bir SQL Server örneğinden geçiriyorsanız, aşağıdaki SQL, satır sayısının ölçeğini belirlemeye yardımcı olabilir, her bölümdeki. SQL veri ambarı ile aynı bölümleme ayrıntı düzeyine kullandıysanız, bölüm başına satır sayısı 60 faktörüyle azaltır aklınızda bulundurun.  

```sql
-- Partition information for a SQL Server Database
SELECT      s.[name]                        AS      [schema_name]
,           t.[name]                        AS      [table_name]
,           i.[name]                        AS      [index_name]
,           p.[partition_number]            AS      [partition_number]
,           SUM(a.[used_pages]*8.0)         AS      [partition_size_kb]
,           SUM(a.[used_pages]*8.0)/1024    AS      [partition_size_mb]
,           SUM(a.[used_pages]*8.0)/1048576 AS      [partition_size_gb]
,           p.[rows]                        AS      [partition_row_count]
,           rv.[value]                      AS      [partition_boundary_value]
,           p.[data_compression_desc]       AS      [partition_compression_desc]
FROM        sys.schemas s
JOIN        sys.tables t                    ON      t.[schema_id]         = s.[schema_id]
JOIN        sys.partitions p                ON      p.[object_id]         = t.[object_id]
JOIN        sys.allocation_units a          ON      a.[container_id]      = p.[partition_id]
JOIN        sys.indexes i                   ON      i.[object_id]         = p.[object_id]
                                            AND     i.[index_id]          = p.[index_id]
JOIN        sys.data_spaces ds              ON      ds.[data_space_id]    = i.[data_space_id]
LEFT JOIN   sys.partition_schemes ps        ON      ps.[data_space_id]    = ds.[data_space_id]
LEFT JOIN   sys.partition_functions pf      ON      pf.[function_id]      = ps.[function_id]
LEFT JOIN   sys.partition_range_values rv   ON      rv.[function_id]      = pf.[function_id]
                                            AND     rv.[boundary_id]      = p.[partition_number]
WHERE       p.[index_id] <=1
GROUP BY    s.[name]
,           t.[name]
,           i.[name]
,           p.[partition_number]
,           p.[rows]
,           rv.[value]
,           p.[data_compression_desc]
;
```

## <a name="partition-switching"></a>Bölüm değiştirme
SQL veri ambarı geçiş bölme ve birleştirme bölüm destekler. Bu işlevlerin her biri kullanılarak yürütülür [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql) deyimi.

Bölüm iki tablo arasında geçiş yapmak için bölümler ilgili sınırlarının Hizala ve tablo tanımları eşleştiğinden emin olmanız gerekir. Kontrol kısıtlamaları bir tablodaki değerleri aralığı zorlamak kullanılamaz olarak kaynak tablo olarak hedef tablosu aynı bölüm sınırları içermelidir. Bölüm sınırları ardından aynı değilse, bölüm meta verileri eşitlenmemiş bölüm anahtarı başarısız olur.

### <a name="how-to-split-a-partition-that-contains-data"></a>Veri içeren bir bölüm bölme
Veri içeren bir bölüm bölmek için en verimli bir yöntem bir `CTAS` deyimi. Bölümlenmiş bir tablodaki kümelenmiş columnstore ise, bu ayırmadan önce ardından tablo bölümü boş olması gerekir.

Aşağıdaki örnek, bir bölümlenmiş columnstore tablosu oluşturur. Bu, her bölüme bir satır ekler:

```sql
CREATE TABLE [dbo].[FactInternetSales]
(
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
)
WITH
(   CLUSTERED COLUMNSTORE INDEX
,   DISTRIBUTION = HASH([ProductKey])
,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                    (20000101
                    )
                )
)
;

INSERT INTO dbo.FactInternetSales
VALUES (1,19990101,1,1,1,1,1,1);
INSERT INTO dbo.FactInternetSales
VALUES (1,20000101,1,1,1,1,1,1);
```

Aşağıdaki sorguyu kullanarak satır sayısını bulur. `sys.partitions` Katalog görünümü:

```sql
SELECT  QUOTENAME(s.[name])+'.'+QUOTENAME(t.[name]) as Table_name
,       i.[name] as Index_name
,       p.partition_number as Partition_nmbr
,       p.[rows] as Row_count
,       p.[data_compression_desc] as Data_Compression_desc
FROM    sys.partitions p
JOIN    sys.tables     t    ON    p.[object_id]   = t.[object_id]
JOIN    sys.schemas    s    ON    t.[schema_id]   = s.[schema_id]
JOIN    sys.indexes    i    ON    p.[object_id]   = i.[object_Id]
                            AND   p.[index_Id]    = i.[index_Id]
WHERE t.[name] = 'FactInternetSales'
;
```

Aşağıdaki komut bölünmüş bir hata iletisi alır:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Bölüm boş olmadığından msg 35346, düzeyi 15, State 1, ALTER PARTITION deyiminin yan tümcesi satırı 44 Böl başarısız oldu. Tabloda bir columnstore dizini mevcut olduğunda, yalnızca boş bölümler bölünebilir. ALTER PARTITION deyimini yürütmeden sonra ALTER PARTITION tamamlandıktan sonra columnstore dizinini yeniden oluşturmayı önce columnstore dizinini devre dışı bırakmayı düşünün.

Ancak, kullanabileceğiniz `CTAS` verileri tutmak için yeni bir tablo oluşturun.

```sql
CREATE TABLE dbo.FactInternetSales_20000101
    WITH    (   DISTRIBUTION = HASH(ProductKey)
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101
                                )
                            )
            )
AS
SELECT *
FROM    FactInternetSales
WHERE   1=2
;
```

Bölüm sınırları hizalı olarak, bir anahtar izin verilir. Bu kaynak tablosu sonradan bölebilirsiniz bir boş bölümüyle bırakın.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Kalan tüm verileri kullanarak yeni bölüm sınırları hizalama etmektir `CTAS`, veri ana tabloya geri dönersiniz.

```sql
CREATE TABLE [dbo].[FactInternetSales_20000101_20010101]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales_20000101]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

ALTER TABLE dbo.FactInternetSales_20000101_20010101 SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2;
```

Veri taşıma işlemlerini tamamladıktan sonra hedef tablodaki istatistikleri yenilemek için iyi bir fikirdir. İstatistikleri güncelleştirmeyi, istatistikleri yeni dağıtım ilgili bölümlerinin verilerin doğru bir şekilde yansıtmak sağlar.

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="load-new-data-into-partitions-that-contain-data-in-one-step"></a>Yeni verileri tek bir adımda verileri içeren bölümleri yüklemek
Bölümler bölüm değiştirme ile veri yükleme olan kullanışlı bir yol aşama kullanıcılarına görünür değil bir tabloda yeni veriler yeni veri anahtarı.  Bölüm değiştirme ile ilişkili kilitleme Çekişme uğraşmanız meşgul sistemlerinde zor olabilir.  Mevcut verileri bir bölüme göz temizlemek için bir `ALTER TABLE` verileri geçiş yapmak için gerekli olarak kullanılır.  Başka bir `ALTER TABLE` yeni verileri geçiş yapmak için gereklidir.  SQL veri ambarı ' nda `TRUNCATE_TARGET` seçeneği desteklenir `ALTER TABLE` komutu.  İle `TRUNCATE_TARGET` `ALTER TABLE` komut bölümünde mevcut verileri yeni verilerle üzerine yazar.  Kullanan bir örnek aşağıdadır `CTAS` mevcut verileri yeni bir tablo oluşturmak için tüm veriler var olan verilerin üzerine yazılacak hedef tabloya yapar sonra yeni verileri ekler.

```sql
CREATE TABLE [dbo].[FactInternetSales_NewSales]
    WITH    (   DISTRIBUTION = HASH([ProductKey])
            ,   CLUSTERED COLUMNSTORE INDEX
            ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES
                                (20000101,20010101
                                )
                            )
            )
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
WHERE   [OrderDateKey] >= 20000101
AND     [OrderDateKey] <  20010101
;

INSERT INTO dbo.FactInternetSales_NewSales
VALUES (1,20000101,2,2,2,2,2,2);

ALTER TABLE dbo.FactInternetSales_NewSales SWITCH PARTITION 2 TO dbo.FactInternetSales PARTITION 2 WITH (TRUNCATE_TARGET = ON);  
```

### <a name="table-partitioning-source-control"></a>Tablo bölümleme kaynak denetimi
Tablo tanımınızdan önlemek için **paslanma** kaynak denetimi sisteminizdeki aşağıdaki yaklaşımı göz önünde bulundurun isteyebilirsiniz:

1. Bölümlenmiş bir tablodaki ancak hiçbir bölüm değerlerle tablosu oluşturma

    ```sql
    CREATE TABLE [dbo].[FactInternetSales]
    (
        [ProductKey]            int          NOT NULL
    ,   [OrderDateKey]          int          NOT NULL
    ,   [CustomerKey]           int          NOT NULL
    ,   [PromotionKey]          int          NOT NULL
    ,   [SalesOrderNumber]      nvarchar(20) NOT NULL
    ,   [OrderQuantity]         smallint     NOT NULL
    ,   [UnitPrice]             money        NOT NULL
    ,   [SalesAmount]           money        NOT NULL
    )
    WITH
    (   CLUSTERED COLUMNSTORE INDEX
    ,   DISTRIBUTION = HASH([ProductKey])
    ,   PARTITION   (   [OrderDateKey] RANGE RIGHT FOR VALUES () )
    )
    ;
    ```

1. `SPLIT` dağıtım işleminin bir parçası olarak tablo:

    ```sql
     -- Create a table containing the partition boundaries

    CREATE TABLE #partitions
    WITH
    (
        LOCATION = USER_DB
    ,   DISTRIBUTION = HASH(ptn_no)
    )
    AS
    SELECT  ptn_no
    ,       ROW_NUMBER() OVER (ORDER BY (ptn_no)) as seq_no
    FROM    (
        SELECT CAST(20000101 AS INT) ptn_no
        UNION ALL
        SELECT CAST(20010101 AS INT)
        UNION ALL
        SELECT CAST(20020101 AS INT)
        UNION ALL
        SELECT CAST(20030101 AS INT)
        UNION ALL
        SELECT CAST(20040101 AS INT)
    ) a
    ;

     -- Iterate over the partition boundaries and split the table

    DECLARE @c INT = (SELECT COUNT(*) FROM #partitions)
    ,       @i INT = 1                                 --iterator for while loop
    ,       @q NVARCHAR(4000)                          --query
    ,       @p NVARCHAR(20)     = N''                  --partition_number
    ,       @s NVARCHAR(128)    = N'dbo'               --schema
    ,       @t NVARCHAR(128)    = N'FactInternetSales' --table
    ;

    WHILE @i <= @c
    BEGIN
        SET @p = (SELECT ptn_no FROM #partitions WHERE seq_no = @i);
        SET @q = (SELECT N'ALTER TABLE '+@s+N'.'+@t+N' SPLIT RANGE ('+@p+N');');

        -- PRINT @q;
        EXECUTE sp_executesql @q;
        SET @i+=1;
    END

     -- Code clean-up

    DROP TABLE #partitions;
    ```

Bu yaklaşımda, kaynak denetiminde kod statik kalır ve bölümleme sınır değerleri dinamik olarak izin verilir; Ambar ile zaman içinde gelişen.

## <a name="next-steps"></a>Sonraki adımlar
Makalelerde tablolar geliştirme hakkında daha fazla bilgi için bakın [tabloya genel bakış](sql-data-warehouse-tables-overview.md).

