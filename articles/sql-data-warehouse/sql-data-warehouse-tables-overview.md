---
title: "SQL veri ambarı tablolarda genel bakış | Microsoft Docs"
description: "Azure SQL veri ambarı tabloları ile çalışmaya başlama."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 2114d9ad-c113-43da-859f-419d72604bdf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/14/2017
ms.author: barbkess
ms.openlocfilehash: 46f7d2ea19a88e65b2d039fdf36d1619c4d74020
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/18/2017
---
# <a name="overview-of-tables-in-sql-data-warehouse"></a>SQL veri ambarı tablolarda genel bakış
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

SQL Data Warehouse'da tablo oluşturma ile çalışmaya başlama basit bir işlemdir.  Temel [CREATE TABLE] [ CREATE TABLE] sözdizimi aşağıdaki büyük olasılıkla genel sözdizimi zaten aşina diğer veritabanlarıyla çalışma.  Bir tablo oluşturmak için tablonuzun adının, sütunlar olarak adlandırın ve her sütun için veri türlerini tanımlamak yeterlidir.  Olduğunuz oluşturursanız tabloları diğer veritabanlarında, bu size tanıdık gelecektir.

```sql  
CREATE TABLE Customers (FirstName VARCHAR(25), LastName VARCHAR(25))
 ``` 

Yukarıdaki örnek, müşterilere iki sütunlarla FirstName ve LastName adlı bir tablo oluşturur.  Her sütun 25 karakter verileri sınırlar VARCHAR(25) veri türüne sahip tanımlanır.  Bu temel tabloya yanı sıra diğer özniteliklerini çoğunlukla diğer veritabanları ile aynıdır.  Veri türleri her sütun için tanımlanır ve verilerinizin bütünlüğünü sağlamak.  Dizinleri g/ç azaltarak performansı artırmak için eklenebilir.  Bölümleme veri değiştirmeniz gerektiğinde performansı artırmak için eklenebilir.

[Yeniden adlandırma] [ RENAME] bir SQL Data Warehouse tablo şöyle görünür:

```sql  
RENAME OBJECT Customer TO CustomerOrig; 
 ```

## <a name="distributed-tables"></a>Dağıtılmış tablolar
SQL veri ambarı gibi dağıtılmış sistemleri tarafından sunulan yeni bir temel öznitelik **dağıtım sütun**.  Dağıtım çok ne, gibi göründüğü bir sütundur.  Bunu nasıl dağıtmak veya bölmek, verilerinizi arka planda belirleyen sütundur.  Dağıtım sütun belirtmeden bir tablo oluşturduğunuzda, tablonun otomatik olarak kullanılarak dağıtılır **hepsini**.  Hepsini bir kez tabloları bazı senaryolarda yeterli olabilirler, ancak dağıtım sütunları tanımlama büyük ölçüde veri taşıma böylece performansı en iyi duruma getirme sorguları sırasında azaltabilir.  Durumlarda tabloyla oluşturmak çok küçük miktarda veri bir tabloda olduğu seçme **çoğaltmak** dağıtım türü her işlem düğümü için veri kopyalar ve veri taşıma sorgu yürütme zaman kaydeder. Bkz: [tablo dağıtma] [ Distribute] dağıtım sütun seçme hakkında daha fazla bilgi için.

## <a name="indexing-and-partitioning-tables"></a>Dizin oluşturma ve tabloları bölümlendirme
SQL veri ambarı kullanarak daha gelişmiş haline gelir ve performansı en iyi duruma getirmek istediğiniz gibi tablo tasarımı hakkında daha fazla bilgi istersiniz.  Daha fazla bilgi edinmek için üzerinde makalelerine bakın [tablo veri türleri][Data Types], [tablo dağıtma][Distribute], [bir tablodizin] [ Index] ve [bir tablo bölümleme][Partition].

## <a name="table-statistics"></a>Tablo istatistikleri
İstatistikleri bir son derece dışında SQL veri ambarı en iyi performans almak için önemlidir.  SQL veri ambarı değil henüz otomatik oluşturma ve güncelleştirme istatistikleri sizin için Azure SQL veritabanı'nda beklenir ortaya çıkabilir gibi beri bizim makale üzerinde okuma [istatistikleri] [ Statistics] en biri olabilir önemli makaleleri sorgularınızdan en iyi performansı elde emin olmak için okuyun.

## <a name="temporary-tables"></a>Geçici tablolar
Geçici tablolar yalnızca oturum boyunca var ve diğer kullanıcılar tarafından görülemeyen tablolardır.  Geçici tablolara başkalarının geçici sonuçları görmemesi ve ayrıca temizleme gereksinimini azaltmak için en iyi yolu olabilir.  Geçici tablolara ayrıca yerel depolama alanını olduğundan, bunlar bazı işlemler için daha hızlı performans sunabilir.  Bkz: [geçici tablo] [ Temporary] makaleleri geçici tabloları hakkında daha fazla ayrıntı için.

## <a name="external-tables"></a>Dış tablolar
Dış tablolar, Polybase tablolar olarak da bilinen SQL veri ambarından dış verileri SQL Data Warehouse, ancak noktası sorgulanabilir tabloların değildir.  Örneğin, bir dış tablo dosyaları Azure Blob Storage veya Azure Data Lake Store için hangi noktası oluşturabilirsiniz.  Oluşturma ve bir dış tablo sorgulama hakkında daha fazla ayrıntı için bkz: [Polybase ile veri yükleme][Load data with Polybase].  

## <a name="unsupported-table-features"></a>Desteklenmeyen tablo özellikleri
SQL veri ambarı diğer veritabanı tarafından sunulan aynı tablo özelliklerinin çoğunu içerirken, henüz desteklenmeyen bazı özellikler vardır.  Tablo özelliklerden bazıları henüz desteklenmemektedir listesi aşağıdadır.

| Desteklenmeyen özellikler |
| --- |
| Birincil anahtar, yabancı anahtarları, benzersiz ve onay [Tablo sınırlamaları][Table Constraints] |
| [Benzersiz dizinler][Unique Indexes] |
| [Hesaplanan sütunlar][Computed Columns] |
| [Seyrek sütun][Sparse Columns] |
| [Kullanıcı tanımlı türler][User-Defined Types] |
| [Sırası][Sequence] |
| [Tetikleyicileri][Triggers] |
| [Dizin oluşturulmuş görünümler][Indexed Views] |
| [Eş anlamlıları][Synonyms] |

## <a name="table-size-queries"></a>Tablo boyutu sorguları
Alan ve her 60 dağıtımları tabloda tarafından tüketilen satırları belirlemek için basit bir yolu kullanmaktır [DBCC PDW_SHOWSPACEUSED][DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ancak, DBCC komutlarını kullanarak oldukça sınırlama.  Dinamik Yönetim görünümlerini (Dmv'leri) yanı sıra daha fazla ayrıntı görmek, çok sorgu sonuçları üzerinde daha fazla denetime olanak sağlar.  Bu görünüm için birçok örneklerde bu ve diğer makaleler ifade edilecek oluşturarak başlayın.

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>Özet Tablo alanı
Bu sorgu, tablo tarafından alanı ve satır döndürür.  En büyük tabloları tablolardır ve hepsini, çoğaltılmış veya dağıtılmış karma olup olmadıkları görmek için harika bir sorgudur.  Dağıtılmış karma tablolar için dağıtım sütun da gösterir.  Çoğu durumda, en büyük tabloları karma bir kümelenmiş columnstore dizini ile dağıtılmış olmalıdır.

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>Dağıtım türüne göre tablo alanı
```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>Dizin türü tablo boşluk
```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>Dağıtım alanı özeti
```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek için üzerinde makalelerine bakın [tablo veri türleri][Data Types], [tablo dağıtma][Distribute], [bir tablodizin] [ Index], [Bir tablo bölümleme][Partition], [tablo istatistikleri koruma] [ Statistics] ve [Geçici tablolar][Temporary].  En iyi uygulamalar hakkında daha fazla bilgi için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md
[Load data with Polybase]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md

<!--MSDN references-->
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[Table Constraints]: https://msdn.microsoft.com/library/ms188066.aspx
[Computed Columns]: https://msdn.microsoft.com/library/ms186241.aspx
[Sparse Columns]: https://msdn.microsoft.com/library/cc280604.aspx
[User-Defined Types]: https://msdn.microsoft.com/library/ms131694.aspx
[Sequence]: https://msdn.microsoft.com/library/ff878091.aspx
[Triggers]: https://msdn.microsoft.com/library/ms189799.aspx
[Indexed Views]: https://msdn.microsoft.com/library/ms191432.aspx
[Synonyms]: https://msdn.microsoft.com/library/ms177544.aspx
[Unique Indexes]: https://msdn.microsoft.com/library/ms188783.aspx

<!--Other Web references-->
