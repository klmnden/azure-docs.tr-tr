---
title: Tablolar - Azure SQL veri ambarı tasarlama | Microsoft Docs
description: Azure SQL veri ambarı tabloları tasarlama giriş.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 03/15/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 06bdd21363aee8202ce7178f157f01a5c26e3a52
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65851584"
---
# <a name="designing-tables-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı tabloları tasarlama

Azure SQL veri ambarı tabloları tasarlama için temel kavramları öğrenin. 

## <a name="determine-table-category"></a>Tablo kategori belirleme 

A [yıldız şeması](https://en.wikipedia.org/wiki/Star_schema) olgu ve boyut tablolara verileri düzenler. Bir olgu veya Boyut tablosuna taşınmadan önce bazı tablolar tümleştirme ya da hazırlık veriler için kullanılır. Bir tablo tasarlarken, tablo verilerini bir olgu, boyut veya tümleştirme tablo ait olup olmadığını belirleyin. Bu karara uygun tablo yapısı ve dağıtım bildirir. 

- **Olgu tabloları** genellikle işlem tabanlı bir sistemde oluşturulan ve ardından veri ambarı'na yüklenen nicel veriler içerir. Örneğin, bir perakendeci her gün satış işlem oluşturur ve ardından çözümleme için bir veri ambarı olgu tablosuna veri yükler.

- **Boyut tabloları** değişebilir ancak genellikle seyrek değişen öznitelik verileri içerir. Örneğin, bir müşterinin adı ve adresi Boyut tablosunda depolanır ve yalnızca müşterinin profil değiştiğinde güncelleştirildi. Büyük bir olgu tablonuz boyutunu en aza indirmek için müşterinin ad ve adres olgu tablosunun her satırında olması gerekmez. Bunun yerine, Olgu Tablosu ve Boyut tablosuna bir müşteri kimliği paylaşabilir Bir sorgu, bir müşteri profili ve işlemleri ilişkilendirmek için iki tablo katılabilirsiniz. 

- **Tümleştirme tabloları** tümleştirme ve veri hazırlık için bir yer sağlar. Bir tümleştirme tablo olağan bir tablo, bir dış tablo ya da geçici bir tablo olarak oluşturabilirsiniz. Örneğin, verileri bir hazırlama tablosuna yükleme, verileri hazırlama dönüşümleri gerçekleştirmenize ve ardından üretim tablosuna veri ekleme.

## <a name="schema-and-table-names"></a>Şema ve tablo adları
Şemaları grubunun tablolara benzer şekilde, birlikte kullanılan iyi bir yoludur.  Birden çok veritabanını SQL veri ambarı'na bir şirket içi çözümden geçiriyorsanız, tüm olgu, boyut ve tümleştirme tabloları SQL veri ambarı'nda bir şema geçirmek için en iyi çalışır. Örneğin, tüm tablolarda saklayabilirsiniz [Wideworldımportersdw](/sql/sample/world-wide-importers/database-catalog-wwi-olap) wwi adlı bir şema içinde veri ambarı örneği. Aşağıdaki kod oluşturur bir [kullanıcı tanımlı şema](/sql/t-sql/statements/create-schema-transact-sql) wwi çağrılır.

```sql
CREATE SCHEMA wwi;
```

SQL veri ambarı'nda tablolar organizasyonu göstermek için tablo adları için ön ekleri olarak olgu, boyutu ve int kullanabilirsiniz. Aşağıdaki tablo bazı Wideworldımportersdw için şema ve tablo adları gösterir.  

| Wideworldımportersdw tablo  | Tablo türü | SQL Veri Ambarı |
|:-----|:-----|:------|:-----|
| Şehir | Boyut | wwi. DimCity |
| Sipariş verme | Olgu | wwi.FactOrder |


## <a name="table-persistence"></a>Tablo kalıcılığı 

Tabloları, Azure Depolama'da kalıcı olarak, geçici olarak Azure depolama veya veri ambarı'na dış bir veri deposundaki verileri depolar.

### <a name="regular-table"></a>Normal bir tablo

Olağan bir tablo verilerini Azure Depolama'da veri ambarı'nın bir parçası olarak depolar. Tablo ve verilerin bir oturumu açık olup bağımsız olarak kalıcı hale getirin.  Bu örnekte, iki sütun olağan bir tablo oluşturur. 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>Geçici tablo
Oturum süresi boyunca yalnızca bir geçici tablo var. Geçici bir tablo, diğer kullanıcıların geçici sonuçları görmemesi ve temizleme gereksinimini azaltmak için kullanabilirsiniz.  Geçici tablolar, hızlı performans sunmak üzere yerel depolama kullanır.  Daha fazla bilgi için [geçici tablolar](sql-data-warehouse-tables-temporary.md).

### <a name="external-table"></a>Dış tablo
Azure depolama blobu veya Azure Data Lake Store içinde yer alan veriler için dış tablo işaret eder. CREATE TABLE AS SELECT deyimiyle birlikte kullanıldığında, bir dış tablodaki varlıkları seçerek verileri SQL Data Warehouse'a veri alır. Dış tablolar, bu nedenle verileri yüklemek için kullanışlıdır. Yükleme öğreticisi için bkz. [Azure blob depolama alanından verileri yüklemek için PolyBase kullanma](load-data-from-azure-blob-storage-using-polybase.md).

## <a name="data-types"></a>Veri türleri
SQL veri ambarı desteklediği veri türleri en yaygın olarak kullanılır. Desteklenen veri türleri listesi için bkz. [veri türleri, CREATE TABLE başvurusu](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes) CREATE TABLE deyiminde. Veri türlerini kullanma ile ilgili yönergeler için bkz: [veri türleri](sql-data-warehouse-tables-data-types.md).

## <a name="distributed-tables"></a>Dağıtılmış tablolar
SQL veri ambarı'nın temel bir özelliği, depolamak ve tablolar arasında işlem yoludur [dağıtımları](massively-parallel-processing-mpp-architecture.md#distributions).  SQL veri ambarı veri, hepsini bir kez deneme (varsayılan), karma dağıtılmasında üç yöntem destekler ve çoğaltılır.

### <a name="hash-distributed-tables"></a>Karma dağıtılmış tablolar
Bir karma dağıtılmış tablo satırları dağıtım sütununda değere göre dağıtır. Bir karma dağıtılmış tablo sorguları büyük tablolar için yüksek performans elde etmek için tasarlanmıştır. Dağıtım sütunu seçerken dikkat etmeniz gereken birkaç faktör vardır. 

Daha fazla bilgi için [tasarım kılavuzunu dağıtılmış tablolar için](sql-data-warehouse-tables-distribute.md).

### <a name="replicated-tables"></a>Çoğaltılmış tablolar
Çoğaltılmış bir tabloda, her işlem düğümü üzerinde kullanılabilir tablo tam bir kopyasına sahip olur. Çoğaltılmış tablolarda birleştirmeler veri taşıma gerektirmeyen bu yana çoğaltılmış tablolar sorguların hızlı üzerinde çalışır. Çoğaltma ek depolama alanı gerektirir ve büyük tablolar için pratik değildir. 

Daha fazla bilgi için [tasarım kılavuzunu çoğaltılmış tablolar için](design-guidance-for-replicated-tables.md).

### <a name="round-robin-tables"></a>Hepsini bir kez deneme tabloları
Hepsini bir kez deneme tablo tablo satırları tüm dağıtımlar arasında eşit olarak dağıtır. Satırlar rastgele dağıtılır. Hepsini bir kez deneme tabloya veri yüklenirken hızlıdır.  Ancak, sorgular, diğer dağıtım yöntemleri değerinden daha fazla veri taşıma gerektirebilir. 

Daha fazla bilgi için [tasarım kılavuzunu dağıtılmış tablolar için](sql-data-warehouse-tables-distribute.md).

### <a name="common-distribution-methods-for-tables"></a>Tablolar için yaygın dağıtım yöntemleri
Tablo kategori tablo dağıtılmasında seçmek için hangi seçeneği genellikle belirler. 

| Tablo kategorisi | Önerilen dağıtım seçeneği |
|:---------------|:--------------------|
| Olgu           | Kümelenmiş columnstore dizini ile karma dağıtım kullanın. İki karma tabloları aynı dağıtım sütunu katıldığında performansını artırır. |
| Boyut      | Çoğaltılmış daha küçük tablolar için kullanın. Karma dağıtılmış tablo her işlem düğümünde depolamak için çok büyük ise kullanın. |
| Staging        | Hepsini bir kez deneme hazırlama tablosu için kullanın. CTAS yüküyle hızlıdır. Veri hazırlama tablosunda eklendiğinde, INSERT kullanın... Üretim tablolarına veri taşımak için bu seçeneği seçin. |

## <a name="table-partitions"></a>Tablo bölümleri
Bölümlenmiş bir tablodaki depoları ve veri aralıkları göre tablo satırları işlemleri gerçekleştirir. Örneğin, bir tablo gün, ay veya yıl bölümlenmiş. Bölüm içindeki verileri için bir sorgu tarama sınırlayan bölüm eleme ile sorgu performansını iyileştirebilir. Ayrıca, bölüm değiştirme ile verileri koruyabilirsiniz. Verileri SQL veri ambarı'nda zaten dağıtılmış olduğundan, çok fazla sorgu performansı yavaşlatabilir. Daha fazla bilgi için [bölümleme Kılavuzu](sql-data-warehouse-tables-partition.md).  Ne zaman bölüm tablosuna değiştirme, bölümleri boş olmadığından, TRUNCATE_TARGET seçeneğini kullanmayı düşünün, [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) mevcut verileri kesilecek ise deyimi. Aşağıdaki kod anahtarları mevcut verilerin üzerine yazılacak SalesFact dönüştürülmüş günlük verileri. 

```sql
ALTER TABLE SalesFact_DailyFinalLoad SWITCH PARTITION 256 TO SalesFact PARTITION 256 WITH (TRUNCATE_TARGET = ON);  
```

## <a name="columnstore-indexes"></a>Columnstore dizinleri
Varsayılan olarak, SQL veri ambarı, bir tablo kümelenmiş bir columnstore dizini depolar. Bu form, veri depolama, yüksek veri sıkıştırma ve sorgu performansı büyük tablolar üzerindeki ulaşır.  Kümelenmiş columnstore dizinini genellikle en iyi seçenektir, ancak bazı durumlarda bir kümelenmiş dizin veya bir yığın uygun depolama yapısıdır.  Yığın tablo, son bir tabloya dönüştürülür bir hazırlama tablosuna gibi geçici verileri yüklemek için özellikle kullanışlı olabilir.

Columnstore özelliklerinin listesi için bkz. [columnstore dizinlerinde yenilikler](/sql/relational-databases/indexes/columnstore-indexes-what-s-new). Columnstore dizini performansını artırmak için bkz: [satır grubu kaliteli columnstore dizinleri için en üst düzeye](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md).

## <a name="statistics"></a>İstatistikler
Bir sorgu yürütme planı oluşturduğunda, sütun düzeyindeki istatistikleri sorgu iyileştiricisi kullanır. Sorgu performansını artırmak için tek tek sütunlara, özellikle sorgu birleşimlerde kullanılan sütun istatistikleri olması önemlidir. [İstatistik oluşturma](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistics) otomatik olarak gerçekleşir.  Ancak, istatistikleri güncelleştirmeyi otomatik olarak gerçekleştirilmez. Çok sayıda satır eklenen veya değiştirilen sonra istatistikleri güncelleştirin. Örneğin, bir yükleme sonrası istatistikleri güncelleştirin. Daha fazla bilgi için [istatistikleri Kılavuzu](sql-data-warehouse-tables-statistics.md).

## <a name="commands-for-creating-tables"></a>Tablo oluşturma için komutları
Yeni boş tablo olarak bir tablo oluşturabilirsiniz. Ayrıca, oluşturabilir ve bir select deyiminin sonuçları ile bir tabloyu doldurmak. Tablo oluşturma için T-SQL komutlarını verilmiştir.

| T-SQL deyimi | Açıklama |
|:----------------|:------------|
| [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | Tüm seçenekleri ve tablo sütunları tanımlayarak boş bir tablo oluşturur. |
| [DIŞ TABLO OLUŞTURMA](/sql/t-sql/statements/create-external-table-transact-sql) | Bir dış tablo oluşturur. Tablo tanımı, SQL veri ambarı'nda depolanır. Tablo verilerini Azure Blob Depolama veya Azure Data Lake Store içinde depolanır. |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | Yeni bir tablo select deyiminin sonuçları ile doldurur. Tablo sütunları ve veri türleri, select deyiminin sonuçlarına temel alır. Verileri içeri aktarmak için bu deyimi bir dış tablodan seçim seçebilirsiniz. |
| [DIŞ TABLO AS SELECT OLUŞTURMA](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | Bir select deyiminin sonuçları bir dış konuma dışarı aktararak yeni bir dış tablo oluşturur.  Azure Blob Depolama veya Azure Data Lake Store konumdur. |

## <a name="aligning-source-data-with-the-data-warehouse"></a>Veri ambarı ile kaynak verileri hizalama

Veri ambarı tabloları, başka bir veri kaynağından veri yüklenirken tarafından doldurulur. Başarılı bir yükleme gerçekleştirmek için kaynak verilerde sütunların sayısı ve veri türlerini veri ambarı tablosu tanımında ile hizalamanız gerekir. Hizalamak için veriler alınırken tablolarınızı tasarlamanın en zor bölümü olabilir. 

Birden çok veri depolarından veri geliyor varsa, verileri veri ambarı'na getirin ve bir tümleştirme tabloya kaydedin. Veri tümleştirme tabloya eklendiğinde, dönüştürme işlemlerini gerçekleştirmek için SQL veri ambarı'nın gücünü kullanabilirsiniz. Veriler hazır sonra üretim tablolarına ekleyin.

## <a name="unsupported-table-features"></a>Desteklenmeyen tablo özellikleri
SQL veri ambarı birçok destekler, ancak bazıları, tablo özellikleri diğer veritabanı tarafından sunulan.  Aşağıdaki liste, bazı SQL veri ambarı'nda desteklenmeyen tablo özelliklerini gösterir.

- Birincil anahtar, yabancı anahtarlar, benzersiz, denetimi [tablo kısıtlamaları](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)

- [Hesaplanan sütunlar](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [Dizin oluşturulmuş görünümler](/sql/relational-databases/views/create-indexed-views)
- [Dizisi](/sql/t-sql/statements/create-sequence-transact-sql)
- [Seyrek sütun](/sql/relational-databases/tables/use-sparse-columns)
- Vekil anahtar. İle uygulama [kimlik](sql-data-warehouse-tables-identity.md).
- [Eş anlamlıları](/sql/t-sql/statements/create-synonym-transact-sql)
- [Tetikleyiciler](/sql/t-sql/statements/create-trigger-transact-sql)
- [Benzersiz dizinler](/sql/t-sql/statements/create-index-transact-sql)
- [Kullanıcı tanımlı türler](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>Tablo boyutu sorguları
Boşluk ve satır tabloya her 60 dağıtım tarafından kullanılan tanımlamak için bir basit bir yolu [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql).

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

Ancak, DBCC komutları kullanarak oldukça kısıtlayıcı olabilecek.  Dinamik Yönetim görünümlerini (Dmv'ler) DBCC komutları değerinden daha fazla ayrıntı göstermek. Bu görünüm oluşturarak başlayın.

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

Bu sorgu, boşluk ve satır tablo döndürür.  En büyük tablolarınızı tablolarıdır ve bunların hepsini, çoğaltılmış, veya karma - dağıtılmış görmenize olanak sağlar.  Karma dağıtılmış tablo için sorgu dağıtım sütunu gösterir.  

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

### <a name="table-space-by-index-type"></a>Dizin türü tarafından tablo alanı

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
Tablolar için veri Ambarınızı oluşturduktan sonra verileri bir tabloya yüklemek için sonraki adım olacaktır.  Yükleme öğreticisi için bkz. [SQL Data warehouse'a veri yükleme](load-data-wideworldimportersdw.md).
