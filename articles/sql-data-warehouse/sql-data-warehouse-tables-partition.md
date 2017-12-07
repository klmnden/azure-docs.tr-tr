---
title: "SQL veri ambarı tablolarda bölümleme | Microsoft Docs"
description: "Azure SQL Data Warehouse'da tablo bölümleme ile çalışmaya başlama."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 6cef870c-114f-470c-af10-02300c58885d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: a28cb1f8a2e48332b344566620dc49b29d9d3c99
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2017
---
# <a name="partitioning-tables-in-sql-data-warehouse"></a>SQL veri ambarı tablolarda bölümlendirme
> [!div class="op_single_selector"]
> * [Genel bakış][Overview]
> * [Veri türleri][Data Types]
> * [Dağıt][Distribute]
> * [Dizin][Index]
> * [Bölüm][Partition]
> * [İstatistikleri][Statistics]
> * [Geçici][Temporary]
> 
> 

Bölümleme tüm SQL veri ambarı tablo türlerinde desteklenir; Kümelenmiş columnstore, kümelenmiş dizin ve yığın dahil.  Bölümlendirme, karma veya dağıtılmış hepsini dahil olmak üzere tüm dağıtım türlerinde de desteklenir.  Etkinleştirir bölümlendirme, bölümleme küçük gruplara veri ve çoğu durumda, verilerinizi ayırmak için bir tarih sütunu üzerinde gerçekleştirilir.

## <a name="benefits-of-partitioning"></a>Bölümleme avantajları
Bölümleme veri Bakım ve sorgu performansını yararlı olabilir.  Olup hem veya yalnızca bir avantaj verilerin nasıl yüklendiği ve bölümleme yalnızca bir sütun üzerinde yapılabilir bu yana aynı sütuna iki amaçlar için kullanılıp kullanılamayacağını bağımlıdır.

### <a name="benefits-to-loads"></a>Yükleri için avantajları
SQL veri ambarı'nda bölümlendirme, yararı, bölüm silme işlemini kullanarak verileri yüklenirken, değiştirme ve birleştirme performansı ve verimliliği artırmak için kullanılabilmesidir.  Çoğu durumda, yakından verileri veritabanına yüklendiği sırada bağlıdır bir tarih sütunu üzerinde verileri bölümlenen.  Verileri tutmak için bölümleri kullanmanın en büyük avantajlarından biri, işlem günlüğü kaçınma.  Yalnızca ekleme, güncelleştirme ya da verileri silme küçük bir düşünce ve çaba, en kolay yaklaşım olabileceği yükleme işlemi sırasında bölümleme kullanılarak önemli ölçüde performansı artırabilir.

Bölüm geçiş hızlı bir şekilde kaldırmak veya bir tablonun bölümünü değiştirmek için kullanılabilir.  Örneğin, satış Olgu Tablosu yalnızca veriler için geçmiş 36 ay içerebilir.  Her ayın sonunda, en eski aylık satış veri tablosundan silindi.  Bu veriler, en eski ay için verileri silmek için delete deyimi kullanarak silinemedi.  Ancak, çok miktarda veri-satır delete deyimi ile silme çok fazla zaman yanı bir sorun yaşanırsa, geri almak için uzun sürebilir büyük işlemleri riskini oluşturun.  Eski bölüm veri bırakma daha uygun bir yaklaşımdır.  Burada, tek tek satırları silme saat ele geçirebilir bölümünün tamamını silme saniye sürebilir.

### <a name="benefits-to-queries"></a>Sorgular için avantajları
Bölümleme de sorgu performansını artırmak için kullanılabilir.  Bölümlenmiş verilerine bir filtre uygular sorguda yalnızca uygun bölümleri için tarama sınırlayabilirsiniz. Filtreleme, bu yöntem, tam tablo taraması önlemek ve yalnızca küçük bir alt veri kümesini tarama. Kümelenmiş columnstore dizinleri giriş, koşul eleme performans avantajı daha az faydalı bağlıdır, ancak bazı durumlarda olabilir bir avantajı sorgulara.  Satış Olgu Tablosu 36 satış tarihi alanını kullanarak ay bölümlenmiş, sonra bu filtre satış tarihinde sorgular, örneğin, filtre eşleşmeyen bölümlerinde arama atlayabilirsiniz.

## <a name="partition-sizing-guidance"></a>Bölüm boyutlandırma kılavuzluğu
Bölümleme bazı senaryolar performansını artırmak için kullanılabilir, içeren bir tablo oluştururken **çok fazla** bölümleri bazı koşullarda performans ölçeklenme.  Bu sorunları için kümelenmiş columnstore tabloları özellikle doğrudur.  Yardımcı olması için bölümleme için bölümleme kullanmak ne zaman ve oluşturmak için bölüm sayısı anlamak önemlidir.  Kaç tane bölümleri çok fazla seçeceğine sabit hızlı kural yok, bağımlı verilerinizi ve kaç tane bölümler için aynı anda yüklüyorsunuz.  Başarılı bir bölümleme düzeni genellikle bölümler, değil binlerce yüzlerce onlarca sahiptir.

Bölümler oluşturulurken **kümelenmiş columnstore** , olduğu tablolar satır sayısını her birime ait göz önünde bulundurun.  Dağıtım ve bölüm başına 1 milyon satır en az, en iyi sıkıştırma ve kümelenmiş columnstore tabloları performansını için gereklidir.  Bölümler oluşturulmadan önce SQL veri ambarı her tablo 60 dağıtılmış veritabanlarına zaten böler.  Arka planda oluşturulan dağıtımları ek olarak, herhangi bir tabloya eklenen bölümleme olur.  Bu örnekte, satış Olgu Tablosu 36 aylık bölümleri yer alan ve o SQL veri ambarı 60 dağıtımları, tüm ay yerleştirildiğinde sonra satış Olgu Tablosu 60 milyon satır aylık veya 2.1 milyon satır içermelidir kullanıyor.  Bir tablo önemli ölçüde sayısından az önerilen en düşük bölüm başına satır içeriyorsa, bölüm başına satır sayısını artırmak için daha az bölümleri kullanmayı düşünün.  Ayrıca bkz. [dizin] [ Index] küme columnstore dizinleri kalitesini değerlendirmek için SQL veri ambarı üzerinde çalışan sorguları içerir makalesi.

## <a name="syntax-difference-from-sql-server"></a>SQL Server sözdizimi farkı
SQL veri ambarı SQL Server'dan biraz farklı olduğu bölümleri tanımlamak için basitleştirilmiş bir yol sunar.  SQL Server'da olduğu gibi SQL veri ambarı'nda bölümleme işlevleri ve şeması kullanılmaz.  Bunun yerine, yapmanız gereken tek şey bölümlenmiş sütun ve sınır noktaları tanımlamak.  Bölümleme sözdizimi SQL Server'dan biraz farklı olabilir, ancak temel kavramları aynıdır.  SQL Server ve SQL Data Warehouse aralıklı bölüm olabilir tablo başına bir bölüm sütunu destekler.  Bölümleme hakkında daha fazla bilgi için bkz: [bölümlenmiş tablolar ve dizinler][Partitioned Tables and Indexes].

Bölümlenmiş bir SQL Data Warehouse aşağıdaki örneği [CREATE TABLE] [ CREATE TABLE] deyimi, bölümler OrderDateKey sütununda Factınternetsales tablosunda:

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
SQL Server bölüm tanımları SQL veri ambarı'na basit geçirmek için:

* SQL Server ortadan [bölüm düzeni][partition scheme].
* Ekleme [bölümleme işlevi] [ partition function] CREATE TABLE tanımına.

Bölümlenmiş bir tablodaki bir SQL Server örneğinden geçiriyorsanız, aşağıdaki SQL satır sayısını anlamaya yardımcı olabilir, her bölüm.  SQL Data Warehouse aynı bölümleme ayrıntı kullandıysanız, bölüm başına satır sayısı 60 faktörüyle azaltır aklınızda bulundurun.  

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

## <a name="workload-management"></a>İş yükü yönetimi
Tablo bölüm kararı faktörü için bir son parçası husustur [iş yükü Yönetim][workload management].  SQL veri ambarı iş yükü yönetiminde öncelikle bellek ve eşzamanlılık yönetimidir.  SQL veri ambarı'nda sorgu yürütme sırasında her dağıtım için ayrılan en fazla bellek kaynağı sınıfları tarafından yönetilir.  İdeal olarak, bölümler, kümelenmiş columnstore dizinleri oluşturmanın bellek gereksinimlerini gibi diğer faktörlere söz konusu boyutlandırılır.  Daha fazla bellek ayırırken columnstore dizinleri avantajı büyük ölçüde kümelenmiş.  Bu nedenle, bir bölüm dizini yeniden oluşturma bellek gerek duyuldu değil emin olmak istiyorsanız. Sorgunuz için kullanılabilir bellek miktarını artırmayı varsayılan rolünden smallrc, largerc gibi diğer rolleri birini geçerek elde edilebilir.

Dağıtım başına bellek ayırma hakkında bilgi kaynak İdarecisi dinamik yönetim görünümlerini sorgulayarak kullanılabilir. Gerçekte, bellek ataması aşağıdaki rakamları küçüktür. Ancak, bu veri yönetimi işlemleri için iyi bir bölüm boyutlandırma olduğunda kullanabileceğiniz kılavuzu düzeyi sağlar.  Çok büyük kaynak sınıfı tarafından sağlanan bellek ataması ötesinde, bölümler boyutlandırma kaçınmaya çalışın. Bu şekil, bölümler büyümesine sırayla daha az en iyi sıkıştırma müşteri adayları bellek baskısı riskini çalıştırın.

```sql
SELECT  rp.[name]                                AS [pool_name]
,       rp.[max_memory_kb]                        AS [max_memory_kb]
,       rp.[max_memory_kb]/1024                    AS [max_memory_mb]
,       rp.[max_memory_kb]/1048576                AS [mex_memory_gb]
,       rp.[max_memory_percent]                    AS [max_memory_percent]
,       wg.[name]                                AS [group_name]
,       wg.[importance]                            AS [group_importance]
,       wg.[request_max_memory_grant_percent]    AS [request_max_memory_grant_percent]
FROM    sys.dm_pdw_nodes_resource_governor_workload_groups    wg
JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools    rp ON wg.[pool_id] = rp.[pool_id]
WHERE   wg.[name] like 'SloDWGroup%'
AND     rp.[name]    = 'SloDWPool'
;
```

## <a name="partition-switching"></a>Bölüm değiştirme
SQL veri ambarı geçiş bölme ve birleştirme bölüm destekler. Bu işlevlerin her biri excuted olan kullanarak [ALTER TABLE] [ ALTER TABLE] deyimi.

Bölüm iki tablo arasında geçiş yapmak için bölümleri ilgili sınırlarının hizalama ve tablo tanımları eşleştiğinden emin olmalısınız. Denetim kısıtlamalarında tablodaki değerleri aralığı zorlamak kullanılabilir olmadığından kaynak tablosu hedef tablo olarak aynı bölüm sınırları içermesi gerekir. Bu durumda değilse, bölüm meta verileri eşitlenmemiş bölüm anahtarı başarısız olur.

### <a name="how-to-split-a-partition-that-contains-data"></a>Veri içeren bir bölüme bölme
Veri içeren bir bölüm bölmek için en verimli yöntemi kullanmaktır bir `CTAS` deyimi. Bölümlenmiş tabloda kümelenmiş columnstore ise, bölünebilir önce sonra tablo bölüm boş olması gerekir.

Her bölümde bir satır içeren bir örnek bölümlenmiş columnstore tablo aşağıdadır:

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


CREATE STATISTICS Stat_dbo_FactInternetSales_OrderDateKey ON dbo.FactInternetSales(OrderDateKey);
```

> [!NOTE]
> İstatistik nesne oluşturarak, biz Bu tablo meta veri daha doğru olduğundan emin olun. Biz istatistikleri oluşturma atlarsanız, SQL veri ambarı varsayılan değerleri kullanır. Lütfen istatistikleri ayrıntıları gözden geçirme için [istatistikleri][statistics].
> 
> 

Biz sonra satır sayısı kullanarak için sorgu yürütebilir `sys.partitions` Katalog görünümü:

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

Biz bu tabloyu bölme denerseniz, şu hata iletisini alırsınız:

```sql
ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Bölüm boş olmadığından msg 35346, düzey 15, State 1, ALTER PARTITION deyiminin yan tümcesi 44 Satırı Böl başarısız oldu.  Tabloda bir columnstore dizini mevcut olduğunda, yalnızca boş bölümler bölünebilir. ALTER PARTITION deyimini yürütmeden, ardından ALTER PARTITION tamamlandıktan sonra columnstore dizinini yeniden oluşturmayı önce columnstore dizinini devre dışı bırakın.

Ancak, biz kullanabilirsiniz `CTAS` verilerimizi tutmak için yeni bir tablo oluşturmak için.

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

Bölüm sınırları hizalı gibi bir anahtar izin verilir. Bu kaynak tablosu biz sonradan bölebilirsiniz boş bir bölüm ile bırakır.

```sql
ALTER TABLE FactInternetSales SWITCH PARTITION 2 TO  FactInternetSales_20000101 PARTITION 2;

ALTER TABLE FactInternetSales SPLIT RANGE (20010101);
```

Tüm yapmak için sol bizim verileri kullanarak yeni bölüm sınırları hizalama etmektir `CTAS` ve verilerimizi yeniden ana tablo anahtarı

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

Veri hareketini tamamladıktan sonra bunlar doğru bir şekilde yeni dağıtım ilgili bölümlerinin verilerin yansıtmak emin olmak için hedef tablo istatistiklerle yenilemek için iyi bir fikirdir:

```sql
UPDATE STATISTICS [dbo].[FactInternetSales];
```

### <a name="table-partitioning-source-control"></a>Kaynak denetimi bölümleme tablosu
Tablo tanımından önlemek için **paslanma** kaynak denetimi sisteminizdeki aşağıdaki yaklaşımı düşünmek isteyebilirsiniz:

1. Bölümlenmiş bir tablodaki olarak ancak hiç bölüm değerlerle tablosu oluşturma

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
                    ()
                )
)
;
```

1. `SPLIT`Tablo dağıtım işleminin bir parçası olarak:

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

Bu yaklaşımda kaynak denetimi kodda statik kalır ve bölümleme sınır değerleri dinamik olarak izin verilir; Ambar zamanla gelişen.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek için üzerinde makalelerine bakın [tablo genel bakışı][Overview], [tablo veri türleri][Data Types], [bir tablodağıtma] [ Distribute], [Tablo dizin][Index], [tablo istatistikleri koruma] [ Statistics] ve [Geçici tablolar][Temporary].  En iyi uygulamalar hakkında daha fazla bilgi için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[workload management]: ./sql-data-warehouse-develop-concurrency.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!-- MSDN Articles -->
[Partitioned Tables and Indexes]: https://msdn.microsoft.com/library/ms190787.aspx
[ALTER TABLE]: https://msdn.microsoft.com/en-us/library/ms190273.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[partition function]: https://msdn.microsoft.com/library/ms187802.aspx
[partition scheme]: https://msdn.microsoft.com/library/ms179854.aspx


<!-- Other web references -->
