---
title: Azure SQL veri ambarı için işlemleri iyileştirme | Microsoft Docs
description: Uzun bir geri alma işlemleri için riski en aza Azure SQL veri ambarı, işlem kodunuzun performansını nasıl iyileştirebileceğinizi öğrenin.
services: sql-data-warehouse
author: ckarst
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/19/2018
ms.author: cakarst
ms.reviewer: igorstan
ms.openlocfilehash: f5e0b2b75ac111f3221108936f84e5883aebfc1a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478836"
---
# <a name="optimizing-transactions-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı işlemleri iyileştirme
Uzun bir geri alma işlemleri için riski en aza Azure SQL veri ambarı, işlem kodunuzun performansını nasıl iyileştirebileceğinizi öğrenin.

## <a name="transactions-and-logging"></a>İşlemler ve günlüğe kaydetme
İşlem, bir ilişkisel veritabanı altyapısı'nın önemli bir bileşenidir. SQL veri ambarı işlemleri sırasında veri değişikliği kullanır. Bu işlem, açık veya örtük olabilir. Tek bir INSERT, UPDATE ve DELETE deyimleri örtük işlemleri tüm örnekleridir. Açık işlemleri TRAN başlamak, TRAN İŞLEYİN veya geri alma TRAN'ı kullanın. Açık işlemleri, genellikle birden çok değişikliği deyimleri içinde tek bir atomik birim birlikte bağlanması gerektiğinde kullanılır. 

Azure SQL veri ambarı, işlem günlüklerini kullanarak veritabanına değişiklikleri kaydeder. Her dağıtım kendi işlem günlüğü sahiptir. İşlem günlüğü yazma işlemlerini otomatik olarak yapılır. Gerekli yapılandırma yoktur. Ancak, bu işlem yazma garanti artırabileceksiniz, bir ek sistemde tanıtır. İşlemsel olarak verimli kod yazarak bu etkiyi en aza indirebilirsiniz. İşlemsel olarak verimli kod, iki kategoride problem döner.

* Mümkün olduğunda kullanımı en az günlük kaydı oluşturur.
* Tekil uzun süre çalışan işlemleri önlemek için kapsamlı toplu kullanarak verilerini işleme
* Bir bölüm düzeni belirli bir bölüme büyük değişiklikler için değiştirme benimseyin

## <a name="minimal-vs-full-logging"></a>En az tam günlük kaydı karşılaştırması
İşlem günlüğü her satır değişikliği izlemek için kullanabileceğiniz, tam olarak kayıtlı işlemler en düşük düzeyde kayıtlı işlemler kapsam ayırma ve yalnızca meta veri değişiklikleri takip edin. Bu nedenle, en az günlük bir hatadan sonra ya da açık bir istek (geri alma TRAN) için işlemin geri alınması için gerekli olan bilgi günlük kaydı içerir. Bilgi işlem günlüğüne çok daha az izlenen gibi en düşük düzeyde günlüğe kaydedilmiş bir işlemin benzer şekilde ölçekli, tam olarak kaydedilmiş bir işlemin daha iyi gerçekleştirir. Ayrıca, daha az yazma işlem günlüğüne gidin, çünkü günlük verilerini çok daha az miktarda oluşturulur ve bu nedenle daha fazla g/ç etkilidir.

İşlem güvenliği sınırları, yalnızca tam olarak kayıtlı işlemler için geçerlidir.

> [!NOTE]
> En düşük düzeyde kayıtlı işlemler açık işlemlerde katılabilir. İzlenen tüm değişiklikleri ayırma yapılardaki gibi en düşük düzeyde kayıtlı işlemler geri almak mümkündür. 
> 
> 

## <a name="minimally-logged-operations"></a>En düşük düzeyde kayıtlı işlemler
Aşağıdaki işlemleri en düşük düzeyde günlüğe kaydedilmesini yeteneğine sahiptir:

* TABLO AS SELECT OLUŞTURMA ([CTAS](sql-data-warehouse-develop-ctas.md))
* EKLE... SEÇİN
* DİZİN OLUŞTURMA
* ALTER INDEX YENİDEN OLUŞTURMA
* DROP INDEX
* TRUNCATE TABLE
* TABLO BIRAKMA
* ALTER TABLO ANAHTAR BÖLÜM

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> İç veri taşıma işlemleri (örneğin, yayın ve KARIŞTIRMA) işlem güvenlik sınırını tarafından etkilenmez.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Toplu yükleme ile en az günlüğe kaydetme
CTAS ve Ekle... Seçim, hem toplu yük işlemlerdir. Ancak, hem de hedef tablo tanımı tarafından etkilenir ve yük senaryoya bağlıdır. Aşağıdaki tabloda, tam olarak veya en düşük düzeyde toplu işlemleri zaman günlüğe açıklanmaktadır:  

| Birincil dizin | Yükleme senaryosu | Oturum açma modu |
| --- | --- | --- |
| Yığın |Herhangi biri |**En az** |
| Kümelenmiş dizin |Boş hedef tablo |**En az** |
| Kümelenmiş dizin |Yüklenen satırları hedef var olan sayfaları ile çakışmaması |**En az** |
| Kümelenmiş dizin |Hedef mevcut sayfalarında yüklenen satırları çakışıyor |Tam |
| Kümelenmiş Columnstore dizini |Yığın boyutu > bölüm hizalı dağıtım başına 102,400 = |**En az** |
| Kümelenmiş Columnstore dizini |Toplu iş boyutu < 102,400 hizalı bölüm dağıtım başına |Tam |

Bu, ikincil veya kümelenmemiş dizinler güncelleştirmek için herhangi bir yazma tam olarak kayıtlı işlemler her zaman olacağını hatalarının ayıklanabileceğini belirtmekte yarar.

> [!IMPORTANT]
> SQL veri ambarı 60 dağıtım sahiptir. Bu nedenle, tüm satırları eşit olarak dağıtılır ve tek bir bölümde giriş, batch 6,144,000 satırları içermesi gerekir varsayarak veya en düşük düzeyde bir kümelenmiş Columnstore dizinine yazarken günlüğe kaydedilecek daha büyük. Ardından Tablo ve eklenen satırları span bölüm sınırları, bölüm sınırının bile veri dağıtım varsayılarak başına 6,144,000 satır gerekir. Her dağıtımdaki her bölüm bağımsız olarak en düşük düzeyde dağıtım günlüğe kaydedilecek INSERT 102. 400 satır eşiği aşması gerekir.
> 
> 

Boş olmayan tablo kümelenmiş bir dizin ile veri yükleme, genellikle tam olarak günlüğe kaydedilen ve en düşük düzeyde oturum satırları bir karışımını içerebilir. Kümelenmiş bir dizin bir Dengeli (b-ağacı) sayfaların ağacıdır. Sayfa için zaten var. başka bir işlem satırlardan yazılmakta olan ise bu yazma tam olarak kaydedilir. Sayfa boş olduğunda ancak ardından bu sayfaya yazma en düşük düzeyde günlüğe kaydedilir.

## <a name="optimizing-deletes"></a>Siler en iyi duruma getirme
DELETE tam olarak günlüğe kaydedilen bir işlemdir.  Büyük miktarda verileri bir tablo veya bir bölümü silmek gerekiyorsa, genellikle daha fazla için mantıklıdır `SELECT` korumak istediğiniz veri en düşük düzeyde kaydedilmiş bir işlemin çalıştırılabilir.  Verileri seçmek için yeni bir tablo oluşturma [CTAS](sql-data-warehouse-develop-ctas.md).  Oluşturulduktan sonra kullanın [Yeniden Adlandır](/sql/t-sql/statements/rename-transact-sql) eski tablonuzun yeni oluşturulan tabloyu ile takas etmek.

```sql
-- Delete all sales transactions for Promotions except PromotionKey 2.

--Step 01. Create a new table select only the records we want to kep (PromotionKey 2)
CREATE TABLE [dbo].[FactInternetSales_d]
WITH
(    CLUSTERED COLUMNSTORE INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
)
AS
SELECT     *
FROM     [dbo].[FactInternetSales]
WHERE    [PromotionKey] = 2
OPTION (LABEL = 'CTAS : Delete')
;

--Step 02. Rename the Tables to replace the 
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_d] TO [FactInternetSales];
```

## <a name="optimizing-updates"></a>Güncelleştirmeleri en iyi duruma getirme
Tam olarak kaydedilmiş bir işlemin güncelleştirmedir.  Çok sayıda satır bir tablo veya bir bölümünü güncelleştirmeniz gerekiyorsa, bu genellikle en düşük düzeyde günlüğe kaydedilmiş bir işlemin gibi kullanmak için çok daha verimli olabilir [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) Bunu yapmak için.

En az günlük mümkün olması eksiksiz bir tablo aşağıdaki örnekte güncelleştirme için CTAS dönüştürüldü.

Bu durumda, retrospectively indirim tutarı için sales tablosundaki ekliyoruz:

```sql
--Step 01. Create a new table containing the "Update". 
CREATE TABLE [dbo].[FactInternetSales_u]
WITH
(    CLUSTERED INDEX
,    DISTRIBUTION = HASH([ProductKey])
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20000101, 20010101, 20020101, 20030101, 20040101, 20050101
                                                ,    20060101, 20070101, 20080101, 20090101, 20100101, 20110101
                                                ,    20120101, 20130101, 20140101, 20150101, 20160101, 20170101
                                                ,    20180101, 20190101, 20200101, 20210101, 20220101, 20230101
                                                ,    20240101, 20250101, 20260101, 20270101, 20280101, 20290101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
OPTION (LABEL = 'CTAS : Update')
;

--Step 02. Rename the tables
RENAME OBJECT [dbo].[FactInternetSales]   TO [FactInternetSales_old];
RENAME OBJECT [dbo].[FactInternetSales_u] TO [FactInternetSales];

--Step 03. Drop the old table
DROP TABLE [dbo].[FactInternetSales_old]
```

> [!NOTE]
> Büyük tablo yeniden oluşturma, SQL veri ambarı iş yükü yönetimi özelliklerini kullanarak yararlı olabilir. Daha fazla bilgi için [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
> 
> 

## <a name="optimizing-with-partition-switching"></a>Bölüm değiştirme ile en iyi duruma getirme
Büyük ölçekli değişiklikler ile karşılaşılan, bir [tablo bölümleme](sql-data-warehouse-tables-partition.md), sonra da bir bölüm düzeni değiştirme mantıklıdır. Veri değişikliği önemlidir ve birden çok bölüme yayılan, ardından bölümler üzerinde yineleme aynı sonuç elde edilir.

Bir bölüm anahtarı gerçekleştirme adımları aşağıdaki gibidir:

1. Boş bir bölüm oluşturun
2. 'Güncelleştirme' CTAS gerçekleştirin
3. Var olan verilerini out tabloya geç
4. Yeni verileri geçiş
5. Verileri temizleme

Ancak, geçiş yapmak için bölümleri belirlemenize yardımcı olması için aşağıdaki yardımcı yordamı oluşturun.  

```sql
CREATE PROCEDURE dbo.partition_data_get
    @schema_name           NVARCHAR(128)
,    @table_name               NVARCHAR(128)
,    @boundary_value           INT
AS
IF OBJECT_ID('tempdb..#ptn_data') IS NOT NULL
BEGIN
    DROP TABLE #ptn_data
END
CREATE TABLE #ptn_data
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
WITH CTE
AS
(
SELECT     s.name                            AS [schema_name]
,        t.name                            AS [table_name]
,         p.partition_number                AS [ptn_nmbr]
,        p.[rows]                        AS [ptn_rows]
,        CAST(r.[value] AS INT)            AS [boundary_value]
FROM        sys.schemas                    AS s
JOIN        sys.tables                    AS t    ON  s.[schema_id]        = t.[schema_id]
JOIN        sys.indexes                    AS i    ON     t.[object_id]        = i.[object_id]
JOIN        sys.partitions                AS p    ON     i.[object_id]        = p.[object_id] 
                                                AND i.[index_id]        = p.[index_id] 
JOIN        sys.partition_schemes        AS h    ON     i.[data_space_id]    = h.[data_space_id]
JOIN        sys.partition_functions        AS f    ON     h.[function_id]        = f.[function_id]
LEFT JOIN    sys.partition_range_values    AS r     ON     f.[function_id]        = r.[function_id] 
                                                AND r.[boundary_id]        = p.[partition_number]
WHERE i.[index_id] <= 1
)
SELECT    *
FROM    CTE
WHERE    [schema_name]        = @schema_name
AND        [table_name]        = @table_name
AND        [boundary_value]    = @boundary_value
OPTION (LABEL = 'dbo.partition_data_get : CTAS : #ptn_data')
;
GO
```

Bu yordam, kodun yeniden kullanımını en üst düzeye çıkarır ve bölüm örnek daha kompakt değiştirme tutar.

Aşağıdaki kod, bir tam bölüm yordamı değiştirme elde etmek için daha önce anlatılan adımları gösterir.

```sql
--Create a partitioned aligned empty table to switch out the data 
IF OBJECT_ID('[dbo].[FactInternetSales_out]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_out]
END

CREATE TABLE [dbo].[FactInternetSales_out]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS
SELECT *
FROM    [dbo].[FactInternetSales]
WHERE 1=2
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Create a partitioned aligned table and update the data in the select portion of the CTAS
IF OBJECT_ID('[dbo].[FactInternetSales_in]') IS NOT NULL
BEGIN
    DROP TABLE [dbo].[FactInternetSales_in]
END

CREATE TABLE [dbo].[FactInternetSales_in]
WITH
(    DISTRIBUTION = HASH([ProductKey])
,    CLUSTERED COLUMNSTORE INDEX
,     PARTITION     (    [OrderDateKey] RANGE RIGHT 
                                    FOR VALUES    (    20020101, 20030101
                                                )
                )
)
AS 
SELECT
    [ProductKey]  
,    [OrderDateKey] 
,    [DueDateKey]  
,    [ShipDateKey] 
,    [CustomerKey] 
,    [PromotionKey] 
,    [CurrencyKey] 
,    [SalesTerritoryKey]
,    [SalesOrderNumber]
,    [SalesOrderLineNumber]
,    [RevisionNumber]
,    [OrderQuantity]
,    [UnitPrice]
,    [ExtendedAmount]
,    [UnitPriceDiscountPct]
,    ISNULL(CAST(5 as float),0) AS [DiscountAmount]
,    [ProductStandardCost]
,    [TotalProductCost]
,    ISNULL(CAST(CASE WHEN [SalesAmount] <=5 THEN 0
         ELSE [SalesAmount] - 5
         END AS MONEY),0) AS [SalesAmount]
,    [TaxAmt]
,    [Freight]
,    [CarrierTrackingNumber] 
,    [CustomerPONumber]
FROM    [dbo].[FactInternetSales]
WHERE    OrderDateKey BETWEEN 20020101 AND 20021231
OPTION (LABEL = 'CTAS : Partition Switch IN : UPDATE')
;

--Use the helper procedure to identify the partitions
--The source table
EXEC dbo.partition_data_get 'dbo','FactInternetSales',20030101
DECLARE @ptn_nmbr_src INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_src

--The "in" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_in',20030101
DECLARE @ptn_nmbr_in INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_in

--The "out" table
EXEC dbo.partition_data_get 'dbo','FactInternetSales_out',20030101
DECLARE @ptn_nmbr_out INT = (SELECT ptn_nmbr FROM #ptn_data)
SELECT @ptn_nmbr_out

--Switch the partitions over
DECLARE @SQL NVARCHAR(4000) = '
ALTER TABLE [dbo].[FactInternetSales]    SWITCH PARTITION '+CAST(@ptn_nmbr_src AS VARCHAR(20))    +' TO [dbo].[FactInternetSales_out] PARTITION '    +CAST(@ptn_nmbr_out AS VARCHAR(20))+';
ALTER TABLE [dbo].[FactInternetSales_in] SWITCH PARTITION '+CAST(@ptn_nmbr_in AS VARCHAR(20))    +' TO [dbo].[FactInternetSales] PARTITION '        +CAST(@ptn_nmbr_src AS VARCHAR(20))+';'
EXEC sp_executesql @SQL

--Perform the clean-up
TRUNCATE TABLE dbo.FactInternetSales_out;
TRUNCATE TABLE dbo.FactInternetSales_in;

DROP TABLE dbo.FactInternetSales_out
DROP TABLE dbo.FactInternetSales_in
DROP TABLE #ptn_data
```

## <a name="minimize-logging-with-small-batches"></a>Günlük kaydı ile küçük partiler halinde simge durumuna küçült
Büyük veri değiştirme işlemleri için öbekleri veya iş birimini kapsamını belirlemek için toplu işlem bölmek mantıklı olabilir.

Aşağıdaki kod, bir çalışma örnektir. Toplu iş boyutu Önemsiz birkaç teknik vurgulamak için ayarlandı. Gerçekte, toplu iş boyutunu önemli ölçüde daha büyük olacaktır. 

```sql
SET NO_COUNT ON;
IF OBJECT_ID('tempdb..#t') IS NOT NULL
BEGIN
    DROP TABLE #t;
    PRINT '#t dropped';
END

CREATE TABLE #t
WITH    (    DISTRIBUTION = ROUND_ROBIN
        ,    HEAP
        )
AS
SELECT    ROW_NUMBER() OVER(ORDER BY (SELECT NULL)) AS seq_nmbr
,        SalesOrderNumber
,        SalesOrderLineNumber
FROM    dbo.FactInternetSales
WHERE    [OrderDateKey] BETWEEN 20010101 and 20011231
;

DECLARE    @seq_start        INT = 1
,        @batch_iterator    INT = 1
,        @batch_size        INT = 50
,        @max_seq_nmbr    INT = (SELECT MAX(seq_nmbr) FROM dbo.#t)
;

DECLARE    @batch_count    INT = (SELECT CEILING((@max_seq_nmbr*1.0)/@batch_size))
,        @seq_end        INT = @batch_size
;

SELECT COUNT(*)
FROM    dbo.FactInternetSales f

PRINT 'MAX_seq_nmbr '+CAST(@max_seq_nmbr AS VARCHAR(20))
PRINT 'MAX_Batch_count '+CAST(@batch_count AS VARCHAR(20))

WHILE    @batch_iterator <= @batch_count
BEGIN
    DELETE
    FROM    dbo.FactInternetSales
    WHERE EXISTS
    (
            SELECT    1
            FROM    #t t
            WHERE    seq_nmbr BETWEEN  @seq_start AND @seq_end
            AND        FactInternetSales.SalesOrderNumber        = t.SalesOrderNumber
            AND        FactInternetSales.SalesOrderLineNumber    = t.SalesOrderLineNumber
    )
    ;

    SET @seq_start = @seq_end
    SET @seq_end = (@seq_start+@batch_size);
    SET @batch_iterator +=1;
END
```

## <a name="pause-and-scaling-guidance"></a>Duraklatma ve ölçeklendirme yönergeleri
Azure SQL veri ambarı sayesinde [duraklatma, sürdürme ve ölçeklendirme](sql-data-warehouse-manage-compute-overview.md) veri Ambarınızı isteğe bağlı. Duraklatmak ya da SQL veri Ambarınızı ölçeklendirebilirsiniz yürütülen işlemler hemen sonlandırılır anlaşılması önemlidir; Tüm açık işlemlerin geri alınması neden oluyor. İş yükünüz bir uzun süre çalışan ve eksik veri değişikliği duraklatma veya ölçeklendirme işleminin önce verilen, bu iş geri alınması gerekecektir. Bu geri alma, duraklatma veya ölçeklendirme, Azure SQL veri ambarı veritabanı için gereken zamanı etkileyebilir. 

> [!IMPORTANT]
> Her ikisi de `UPDATE` ve `DELETE` tam olarak günlüğe kaydedilen işlemleridir ve bunlar geri al/Yinele şekilde işlemleri eşdeğer en düşük düzeyde işlemleri günlüğe önemli ölçüde uzun sürebilir. 
> 
> 

Uçuş veri değiştirme işlemleri duraklatma veya ölçeklendirme SQL veri ambarı önce tam olarak izin vermek için en iyi senaryodur bakın. Ancak, bu senaryoda her zaman pratik olmayabilir. Uzun bir geri alma riskini azaltmak için aşağıdaki seçeneklerden birini göz önünde bulundurun:

* Uzun süren işlemlere kullanarak yeniden yazma [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)
* İşlemi öbeklere ayırma; Satır alt kümesi üzerinde çalıştırma

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [SQL veri ambarı işlemlerinde](sql-data-warehouse-develop-transactions.md) yalıtım düzeyleri ve işlem sınırları hakkında daha fazla bilgi için.  Diğer en iyi genel bakış için bkz. [SQL veri ambarı en iyi yöntemler](sql-data-warehouse-best-practices.md).

