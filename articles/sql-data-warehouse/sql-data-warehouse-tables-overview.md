---
title: "Tablo Tasarımı giriş - Azure SQL Data Warehouse | Microsoft Docs"
description: "Azure SQL Data Warehouse tablolarda tasarlama giriş."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 01/05/2018
ms.author: barbkess
ms.openlocfilehash: 8e48d771ffcefe31c89a0d70f65ca867653a2163
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2018
---
# <a name="introduction-to-designing-tables-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse tablolarda tasarlama giriş

Azure SQL Data Warehouse tablolarda tasarlamak için temel kavramları öğrenin. 

## <a name="determining-table-category"></a>Tablo kategori belirleme 

A [yıldız şeması](https://en.wikipedia.org/wiki/Star_schema) verileri olgu ve boyut tabloları düzenler. Bir olgu veya Boyut tablosuna gitmeden önce bazı tablolar tümleştirme veya hazırlama veriler için kullanılır. Bir tablo tasarlarken, tablo verileri bir olgu, boyut veya tümleştirme tablo ait olup olmadığını belirleyin. Bu karara uygun tablo yapısı ve dağıtım bildirir. 

- **Olgu tabloları** genellikle bir işlem sistem içinde oluşturulan ve veri ambarına yüklenen nicel verileri içeren. Örneğin, bir perakende iş her gün satış hareketleri oluşturur ve analiz için veri ambarı olgu tablosu için veri yükler.

- **Boyut tabloları** değişebilir ancak genellikle değişmeyen özniteliği verileri içerir. Örneğin, bir müşterinin adını ve adresini Boyut tablosunda depolanır ve yalnızca Müşteri'nin profili değiştiğinde güncelleştirildi. Büyük veri tablosu boyutunu en aza indirmek için Müşteri'nin ad ve adres her bir Olgu Tablosu satırda olması gerekmez. Bunun yerine, Olgu Tablosu ve Boyut tablosuna bir müşteri kimliği paylaşabilirsiniz Bir sorgu Müşteri'nin profili ve işlemleri ilişkilendirmek için iki tabloları birleştirebilirsiniz. 

- **Tümleştirme tabloları** tümleştirme veya verileri hazırlamak için bir yer sağlar. Bu tablolar, normal tablolardaki, dış tablolara veya geçici tablolar oluşturulabilir. Örneğin, aşamalandırma tablosu veri yükleme, hazırlama veriler üzerinde dönüşümleri gerçekleştirir ve sonra üretim tabloya veri ekleyin.

## <a name="schema-and-table-names"></a>Şema ve tablo adları
SQL veri ambarı'nda bir veri ambarı veritabanı türüdür. Veri ambarındaki tabloların tümü aynı veritabanı içinde yer alır.  Tablolar arasında birden çok veri ambarlarında katılamaz. Bu davranış, SQL veritabanları arası birleştirmeler destekleyen sunucudan farklıdır. 

Bir SQL Server veritabanında olgu, boyutu, kullanın veya şema adlarını tümleştirmek. SQL veri ambarı için bir SQL Server veritabanı aktarıyorsanız, en iyi geçirmek için çalıştığını bir şemaya olgu, boyut ve tümleştirme tabloların tümü SQL veri ambarı'nda depolar. Örneğin, tüm tablolarda saklayabilirsiniz [WideWorldImportersDW](/sql/sample/world-wide-importers/database-catalog-wwi-olap) WWI adlı bir şema içinde örnek veri ambarı. Aşağıdaki kod oluşturur bir [kullanıcı tanımlı şema](/sql/t-sql/statements/create-schema-transact-sql) WWI çağrılır.

```sql
CREATE SCHEMA WWI;
```

SQL veri ambarı'nda tabloları organizasyonu göstermek için tablo adlarının ön eklerin olarak olgu, boyutu ve int'ı kullanın. Aşağıdaki tabloda bazı WideWorldImportersDW şema ve tablo adları gösterir. SQL Server ve SQL Data Warehouse adlarında karşılaştırır. 

| WWI boyut tabloları  | SQL Server | SQL Veri Ambarı |
|:-----|:-----|:------|
| Şehir | Dimension.City | WWI. DimCity |
| Müşteri | Dimension.Customer | WWI. DimCustomer |
| Tarih | Dimension.Date | WWI. DimDate'i |

| WWI olgu tabloları | SQL Server | SQL Veri Ambarı |
|:---|:---|:---|
| Sıra | Fact.Order | WWI. FactOrder |
| Satış  | Fact.Sale  | WWI. FactSale  |
| Satın Al | Olgusu | WWI. FactPurchase |


## <a name="table-definition"></a>Tablo tanımı 

Aşağıdaki kavramlar tabloları tanımlama anahtar yönleri açıklanmaktadır. 

### <a name="standard-table"></a>Standart tablo

Standart bir tablo, veri ambarı bir parçası olarak Azure storage'da depolanır. Tablo ve verileri bir oturumu açık olup bağımsız olarak kalıcı olmasını sağlar.  Bu örnek iki sütunların bulunduğu bir tablo oluşturur. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Geçici bir tablo
Geçici bir tablo yalnızca oturum boyunca bulunmaktadır. Geçici bir tabloya diğer kullanımlar geçici sonuçları görmemesi ve temizleme gereksinimini azaltmak için kullanabilirsiniz.  Geçici tablolara ayrıca yerel depolama alanını olduğundan, bunlar bazı işlemler için daha hızlı performans sunabilir.  Daha fazla bilgi için bkz: [geçici tablolar](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Dış tablo
Bir dış tablo verileri Azure blob depolama veya Azure Data Lake Store bulunan işaret eder. CREATE TABLE AS SELECT deyimi ile birlikte kullanıldığında, bir dış tablosundan seçerek verileri SQL Data Warehouse'a içeri aktarır. Dış tablolara, bu nedenle veri yüklemek için faydalıdır. Bir yükleme öğretici için bkz: [kullanım verileri Azure blob depolama alanından yüklemek için PolyBase](load-data-from-azure-blob-storage-using-polybase.md).

### <a name="data-types"></a>Veri türleri
SQL veri ambarı desteklediği veri türleri en yaygın olarak kullanılır. Desteklenen veri türleri listesi için bkz: [veri türleri](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) CREATE TABLE deyimi içinde. Veri türlerinin boyutu en aza sorgu performansını artırmaya yardımcı olur. Veri türleri hakkında yönergeler için bkz [veri türleri](sql-data-warehouse-tables-data-types.md).

### <a name="distributed-tables"></a>Dağıtılmış tablolar
SQL veri ambarı temel özelliği dağıtımları, Dağıtılmış sistemde adlı 60 dağıtılmış konumda tabloları depolayabilir yoludur.  SQL veri ambarı tablo şu üç yöntemden birini depolayabilirsiniz:

- **Hepsini** depoları tablo satırları rastgele ancak eşit dağıtımlar arasında. Hepsini Tablo Hızlı yükleme performans elde eder, ancak sütun katılma sorgular için diğer yöntemleri'den daha fazla veri taşıma gerektirir. 
- **Karma** dağıtım dağıtım sütununda değere göre satırları dağıtır. Karma dağıtılmış tablosu büyük tablolarda sorgu birleştirmelerde yüksek performans elde etmek için en olası sahiptir. Dağıtım sütun seçimi etkileyen çeşitli Etkenler vardır. Daha fazla bilgi için bkz: [dağıtılmış tabloları](sql-data-warehouse-tables-distribute.md).
- **Çoğaltılan** tabloları tablonun tam bir kopyasını kullanılabilir duruma her işlem düğümü üzerinde. Çoğaltılmış tablolarda birleştirmeler veri taşıma gerektirmeyen beri çoğaltılmış tablolar çalıştıracağınız hızlı sorgular. Çoğaltma ek depolama alanı gerektirir ancak ve büyük tablolar için kullanışlı değildir. Daha fazla bilgi için bkz: [Tasarım Kılavuzu çoğaltılmış tablolar için](design-guidance-for-replicated-tables.md).

Tablo kategori genellikle tablo dağıtılmasında seçmek için hangi seçeneği belirler.  

| Tablo kategorisi | Önerilen dağıtım seçeneği |
|:---------------|:--------------------|
| Olgusu           | Karma dağıtım kümelenmiş columnstore dizini ile kullanın. İki karma tablolar aynı dağıtım sütunda birleştirilir, performansı artırır. |
| Boyut      | Çoğaltılmış küçük tablolar için kullanın. Tabloların her işlem düğümü üzerinde depolamak için çok büyük olduğundan, karma dağıtılmış kullanın. |
| Hazırlanıyor        | Hepsini hazırlama tablosu için kullanın. Yük CTAS ile hızlıdır. Veri hazırlama tablosunda eklendiğinde, INSERT kullanın... Bir üretim tablolara verileri taşımak için bu seçeneği seçin. |

### <a name="table-partitions"></a>Tablo bölümleri
Bölümlenmiş bir tablodaki depolar ve veri aralıklarını göre tablo satırlarını işlemleri gerçekleştirir. Örneğin, bir tablo gün, ay veya yıl bölümlenmiş. Bir sorgu tarama bölüm içindeki verilere sınırlar sorgu performansı bölüm eleme artırabilir. Bölüm geçiş aracılığıyla veri koruyabilir. SQL veri ambarı verileri zaten dağıtılmış olduğundan, çok fazla bölümler sorgu performansı düşürebilir. Daha fazla bilgi için bkz: [Kılavuzu bölümleme](sql-data-warehouse-tables-partition.md).

### <a name="columnstore-indexes"></a>Columnstore dizinleri
Varsayılan olarak, SQL Data Warehouse kümelenmiş columnstore dizini bir tablo depolar. Bu form, veri depolama yüksek veri sıkıştırma ve sorgu performansını büyük tablolarda erişir.  Kümelenmiş columnstore dizini genellikle en iyi seçimdir, ancak bazı durumlarda kümelenmiş bir dizin veya yığın uygun depolama yapısıdır.

Columnstore özelliklerin listesi için bkz: [columnstore dizinlerinde yenilikler](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Columnstore dizini performansı artırmak için bkz: [columnstore dizinleri için satır grubu kimliğinde kalitesini en üst düzeye](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

### <a name="statistics"></a>İstatistikler
Bir sorgu yürütme için plan oluştururken sorgu iyileştiricisi sütun düzeyi istatistikleri kullanır. Sorgu performansını artırmak için tek tek sütunlarda özellikle sorgu birleşimlerde kullanılan sütun istatistik oluşturulması önemlidir. Oluşturma ve istatistikleri güncelleştirmeyi otomatik olarak gerçekleştirilmez. [İstatistik oluşturma](/sql/t-sql/statements/create-statistics-transact-sql) tablo oluşturduktan sonra. Çok sayıda satır eklenen veya değiştirilen sonra istatistikleri güncelleştirin. Örneğin, bir yükleme sonrası istatistikleri güncelleştirin. Daha fazla bilgi için bkz: [istatistikleri Kılavuzu](sql-data-warehouse-tables-statistics.md).

## <a name="ways-to-create-tables"></a>Tablolar oluşturma yolları
Yeni bir boş tablo olarak bir tablo oluşturabilirsiniz. Ayrıca, oluşturabilir ve bir select deyimi sonuçlarını içeren bir tablo doldurmak. Tablo oluşturma için T-SQL komutları şunlardır:

| T-SQL deyimi | Açıklama |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Tüm tablo sütunları ve seçenekleri tanımlayarak boş bir tablo oluşturur. |
| [DIŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql) | Bir dış tablo oluşturur. Tablo tanımı SQL veri ambarında depolanır. Tablo verileri Azure Blob storage veya Azure Data Lake Store depolanır. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Yeni bir tablo sonuçları bir select deyimi ile doldurur. Tablo sütunları ve veri türleri üzerinde select deyimi sonuçları temel alır. Veri almak için bu deyim bir dış tablosundan seçebilirsiniz. |
| [DIŞ TABLE AS SELECT OLUŞTURMA](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Sonuçları bir select deyimi, harici bir konuma dışa aktararak yeni bir dış tablo oluşturur.  Azure Blob storage veya Azure Data Lake Store konumdur. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Veri ambarı ile kaynak verileri hizalama

Veri ambarı tabloları, başka bir veri kaynağından veri yükleyerek doldurulur. Başarılı bir yükleme gerçekleştirmek için kaynak veri sütun numarası ve veri türlerini veri ambarındaki tablo tanımıyla hizalanması gerekir. Hizalamak için veri alma tablolarınızı tasarlama en zor bir parçası olabilir. 

Veri birden çok veri depolarına geliyorsa, verileri veri ambarına getirmek ve bir tümleştirme tablosunda saklayın. Veri integrtion tabloya eklendiğinde, SQL Data Warehouse gücünü dönüşüm işlemleri gerçekleştirmek için kullanabilirsiniz.

## <a name="unsupported-table-features"></a>Desteklenmeyen tablo özellikleri
SQL veri ambarı birçok destekler, ancak diğer veritabanı tarafından sunulan tüm, tablo özellikleri.  Aşağıdaki listede SQL veri ambarı'nda desteklenmeyen tablo özelliklerden bazıları gösterilir.

- Birincil anahtar, yabancı anahtarları, benzersiz, onay [Tablo sınırlamaları](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Hesaplanan sütunlar](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Dizin oluşturulmuş görünümler](/sql/relational-databases/views/create-indexed-views)
- [Sırası](/sql/t-sql/statements/create-sequence-transact-sql)
- [Seyrek sütun](/sql/relational-databases/tables/use-sparse-columns)
- [Vekil anahtarları](). İle uygulama [kimlik](sql-data-warehouse-tables-identity.md).
- [Eş anlamlıları](/sql/t-sql/statements/create-synonym-transact-sql)
- [Tetikleyiciler](/sql/t-sql/statements/create-trigger-transact-sql)
- [Benzersiz dizinler](/sql/t-sql/statements/create-index-transact-sql)
- [Kullanıcı tanımlı türler](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Tablo boyutu sorguları
Alan ve her 60 dağıtımları tabloda tarafından tüketilen satırları belirlemek için basit bir yolu kullanmaktır [DBCC PDW_SHOWSPACEUSED] [DBCC PDW_SHOWSPACEUSED].

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ancak, DBCC komutlarını kullanarak oldukça sınırlama.  Dinamik Yönetim görünümlerini (Dmv'leri) DBCC komutu'den daha fazla ayrıntı göster. Bu görünüm oluşturarak başlayın.

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

Bu sorgu, tablo tarafından alanı ve satır döndürür.  En büyük tabloları tablolardır ve bunların hepsini, çoğaltılan, olup karma - dağıtılmış görmenizi sağlar.  Karma dağıtılmış tablolar için sorgu dağıtım sütunda görüntülenir.  Çoğu durumda, kümelenmiş columnstore dizini ile karma dağıtılmış en büyük tabloları olmalıdır.

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
Veri ambarınız için tabloları oluşturduktan sonra sonraki tabloya veri yüklemek için bir adımdır.  Bir yükleme öğretici için bkz: [PolyBase ile Azure blob depolama biriminden verileri yüklenirken](load-data-from-azure-blob-storage-using-polybase.md).