---
title: İşlemleri Azure SQL Data Warehouse için en iyi duruma getirme | Microsoft Docs
description: Uzun düzeyine riskini en aza indirme sırasında işlem kodunuzu Azure SQL veri ambarı performansını en iyi duruma öğrenin.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 55fc317dc9e7a1401aef8c5431ba04d86822d333
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="optimizing-transactions-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse işlemlerinde en iyi duruma getirme
Uzun düzeyine riskini en aza indirme sırasında işlem kodunuzu Azure SQL veri ambarı performansını en iyi duruma öğrenin.

## <a name="transactions-and-logging"></a>İşlemler ve günlüğe kaydetme
İşlemler, ilişkisel veritabanı altyapısını önemli bileşenidir. SQL veri ambarı işlemleri sırasında veri değişikliği kullanır. Bu işlemler, açık veya örtülü olabilir. Tek bir INSERT, UPDATE ve DELETE deyimleri örtülü işlemler tüm örnekleridir. Açık işlemleri başlamak TRAN, TRAN yürütme veya geri alma TRAN kullanın. Birden çok değişikliği deyimleri tek bir atomik biriminde birlikte bağlanması gerektiğinde açık işlemler tipik olarak kullanılır. 

Azure SQL Data Warehouse, işlem günlükleri kullanarak veritabanı değişiklikleri kaydeder. Her dağıtım kendi işlem günlüğü vardır. İşlem günlüğü yazma otomatik olarak yapılır. Gerekli yapılandırma yoktur. Ancak, bu işlem yazma garanti adımında, bir ek yük sistemde tanıtır. İşlemsel olarak verimli kodlar yazarak bu etkiyi en aza indirebilirsiniz. İşlemsel olarak verimli kod kapsamlı iki kategoride yer alır.

* Mümkün olduğunda kullanımı en az günlük oluşturur
* Tekil uzun süre çalışan işlemleri önlemek için kapsamlı toplu işlemleri kullanarak verileri işlemek
* Bir bölüm düzeni büyük değişiklikler belli bir bölüm için değiştirme benimsemeyi

## <a name="minimal-vs-full-logging"></a>En az tam günlük kaydı karşılaştırması
Her satır değişiklik izlemek için işlem günlüğü kullanın, tam olarak günlüğe kaydedilmiş işlemlerin en düşük düzeyde günlüğe kaydedilmiş işlemlerin kapsam ayırma ve yalnızca meta veri değişiklikleri takip edin. Bu nedenle, en az günlük işlem bir hatadan sonra veya açık bir istek (ROLLBACK TRAN) için geri almak için gerekli olan bilgi günlük kaydı içerir. Bilgi işlem günlüğünde daha az izlenen gibi en düşük düzeyde günlüğe kaydedilmiş bir işlemin benzer şekilde boyutlandırılmış tam olarak günlüğe kaydedilmiş bir işlemin daha iyi gerçekleştirir. Ayrıca, daha az yazma işlem günlüğü gitmek için günlük verileri kadar daha az miktarda oluşturulur ve bu nedenle daha fazla g/ç etkilidir.

İşlem güvenliği sınırları yalnızca tam olarak günlüğe kaydedilen işlemleri için geçerlidir.

> [!NOTE]
> En düşük düzeyde günlüğe kaydedilmiş işlemlerin açık işlemlerde yer alabilir. Ayırma yapılarında tüm değişiklikleri izlenen gibi en düşük düzeyde günlüğe kaydedilmiş işlemlerin geri mümkündür. 
> 
> 

## <a name="minimally-logged-operations"></a>En düşük düzeyde günlüğe kaydedilmiş işlemlerin
Aşağıdaki işlemleri en düşük düzeyde günlüğe kaydedilmesini yeteneğine sahiptir:

* TABLE AS SELECT OLUŞTURUN ([CTAS](sql-data-warehouse-develop-ctas.md))
* EKLE... SEÇİN
* DİZİN OLUŞTURMA
* ALTER INDEX YENİDEN OLUŞTURMA
* DROP INDEX
* TRUNCATE TABLE
* TABLO BIRAKMA
* ALTER TABLE SWITCH PARTITION

<!--
- MERGE
- UPDATE on LOB Types .WRITE
- SELECT..INTO
-->

> [!NOTE]
> İç veri taşıma işlemleri (örneğin, yayın ve KARIŞIK) göre hareket güvenlik sınırı etkilenmez.
> 
> 

## <a name="minimal-logging-with-bulk-load"></a>Toplu yükleme ile en az günlüğe kaydetme
CTAS ve Ekle... SELECT hem toplu yük işlemleridir. Ancak, hem de hedef tablo tanımı tarafından etkilenir ve yük senaryoya bağlıdır. Aşağıdaki tabloda, tam olarak veya en düşük düzeyde toplu işlemler ne zaman oturum açıklanmaktadır:  

| Birincil dizin | Yükleme senaryosu | Oturum açma modu |
| --- | --- | --- |
| Yığın |Herhangi biri |**en az** |
| Kümelenmiş dizini |Boş hedef tablo |**en az** |
| Kümelenmiş dizini |Yüklenen satırları hedef var olan sayfaları ile çakışmaması |**en az** |
| Kümelenmiş dizini |Hedef varolan sayfalarında yüklenen satırları çakışıyor |Tam |
| Kümelenmiş Columnstore dizini |Yığın boyutu > hizalı bölüm dağıtım başına 102,400 = |**en az** |
| Kümelenmiş Columnstore dizini |Toplu iş boyutu < 102,400 hizalı bölüm dağıtım başına |Tam |

Bu, ikincil veya kümelenmemiş dizinler güncelleştirilecek tüm yazma tam olarak günlüğe kaydedilmiş işlemlerin her zaman olacağını dikkate değerdir.

> [!IMPORTANT]
> SQL veri ambarı 60 dağıtımları sahiptir. Bu nedenle, tüm satırları eşit olarak dağıtılır ve tek bir bölüm giriş, toplu 6,144,000 satır içermesi gerekir varsayılarak veya en düşük düzeyde bir kümelenmiş Columnstore dizini yazılırken günlüğe kaydedilecek daha büyük. Ardından tablonun bölümlenme şekli ve bölüm sınırları eklenen satır span bile veri dağıtım varsayılarak bölüm sınır başına 6,144,000 satır gerekir. Her bölümde her dağıtım bağımsız olarak dağıtım en düşük düzeyde tutulacak INSERT 102,400 satır eşiği aşması gerekir.
> 
> 

Kümelenmiş bir dizin ile bir boş olmayan tabloya veri yükleme genellikle tam olarak günlüğe kaydedilen ve en düşük düzeyde oturum satır bir karışımını içerebilir. Kümelenmiş bir dizin sayfalarını dengeli ağacının (b-ağacı) ' dir. Sayfa için zaten var. başka bir işlem satırları yazılmakta varsa, bu yazma tam olarak kaydedilir. Sayfa boş ise, ancak daha sonra bu sayfayı yazma en düşük düzeyde günlüğe kaydedilir.

## <a name="optimizing-deletes"></a>Siler en iyi duruma getirme
DELETE tam olarak günlüğe kaydedilen bir işlemdir.  Büyük miktarda veri bir tablo veya bir bölümü silmek gerekiyorsa, genellikle daha fazla için mantıklıdır `SELECT` korumak istediğiniz veri en düşük düzeyde günlüğe kaydedilen bir işlem olarak çalıştırabilirsiniz.  Verileri seçmek için ile yeni bir tablo oluştur [CTAS](sql-data-warehouse-develop-ctas.md).  Oluşturduktan sonra kullanmak [yeniden adlandırma](/sql/t-sql/statements/rename-transact-sql) eski tablonuz yeni oluşturulan tabloyla takas etmek.

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
Tam olarak günlüğe kaydedilmiş bir işlemin güncelleştirmesidir.  Çok sayıda satır bir tablo veya bölüm güncellemeniz gerekiyorsa, bu genellikle en düşük düzeyde günlüğe kaydedilmiş bir işlemin gibi kullanmak için daha etkili olabilir [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) Bunu yapmak için.

Böylece en az günlük mümkündür tam tablo aşağıdaki örnekte güncelleştirme bir CTAS dönüştürüldü.

Bu durumda, retrospectively bir indirim tutarı tablosundaki Satış ekliyoruz:

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
> Büyük tabloları yeniden oluşturma, SQL veri ambarı iş yükü yönetimi özelliklerini kullanarak yararlı olabilir. Daha fazla bilgi için bkz: [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md).
> 
> 

## <a name="optimizing-with-partition-switching"></a>Bölüm geçiş ile en iyi duruma getirme
Büyük ölçekli değişiklikleri içinde ile karşılaştığı varsa bir [tablo bölüm](sql-data-warehouse-tables-partition.md), bir bölüm düzeni değiştirme mantıklı sonra. Veri değişikliği önemlidir ve birden çok bölüm yayılan, bölümler yineleme aynı sonucu elde eder.

Bir bölüm anahtarı gerçekleştirme adımları aşağıdaki gibidir:

1. Boş bir bölüm çıkışı oluşturma
2. 'Güncelleştirme' CTAS gerçekleştirme
3. Var olan verilerini out tabloya geç
4. Yeni verileri geçiş
5. Verileri temizleme

Ancak, geçiş bölümlerini tanımlamaya yardımcı olmak için aşağıdaki yardımcı yordamı oluşturun.  

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

Bu yordam, kodu yeniden kullanma en üst düzeye çıkarır ve bölüm örnek daha küçük değiştirme tutar.

Aşağıdaki kod, tam bir bölüm yordamı değiştirme elde etmek için daha önce bahsedilen adımlarını gösterir.

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

## <a name="minimize-logging-with-small-batches"></a>Küçük toplu günlüğüyle simge durumuna küçült
Büyük veri değiştirme işlemleri için bu işlemi öbekleri veya toplu iş birimini kapsam içine bölmek için mantıklı olabilir.

Aşağıdaki kod, bir çalışma örnektir. Toplu iş boyutu Önemsiz birkaç teknik vurgulamak için ayarlandı. Gerçekte, toplu iş boyutu önemli ölçüde daha büyük olabilir. 

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

## <a name="pause-and-scaling-guidance"></a>Duraklatma ve ölçeklendirme Kılavuzu
Azure SQL Data Warehouse olanak tanır [duraklatma, sürdürme ve ölçeklendirme](sql-data-warehouse-manage-compute-overview.md) veri ambarınız isteğe bağlı. Duraklatmak veya SQL Data Warehouse ölçeklendirebilir, yürütülen işlemler hemen sonlandırılır anlamak önemlidir; geri alınması açık işlemler neden oluyor. İş yükünüzün uzun çalışan ve tamamlanmamış veri değişikliği duraklama veya ölçek işlemi öncesinde yayımlanan değilse, bu iş geri gerekecektir. Bu geri alma, duraklatmak veya Azure SQL Data Warehouse veritabanınızı ölçeklendirmek için geçen süreyi etkiler. 

> [!IMPORTANT]
> Her ikisi de `UPDATE` ve `DELETE` tam olarak günlüğe kaydedilen işlemleri ve bunlar geri alma/yineleme için işlemleri eşdeğer en düşük düzeyde işlemleri günlüğe daha önemli ölçüde uzun sürebilir. 
> 
> 

En iyi senaryo uçuş veri değişikliği işlemlerin duraklatma veya SQL Data Warehouse ölçeklendirme önce tam kurmanızı sağlamaktır. Ancak, bu senaryo her zaman pratik olmayabilir. Uzun bir geri alma riskini azaltmak için aşağıdaki seçeneklerden birini göz önünde bulundurun:

* Uzun süre çalışan işlemleri kullanarak yeniden yazma [CTAS](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)
* İşlemi parçalara Böl; Alt satır kümesi üzerinde çalışan

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [SQL Data Warehouse işlemlerinde](sql-data-warehouse-develop-transactions.md) yalıtım düzeylerinde ve işlem sınırları hakkında daha fazla bilgi edinmek için.  Diğer en iyi yöntemler genel bakış için bkz: [SQL veri ambarı en iyi yöntemler](sql-data-warehouse-best-practices.md).

