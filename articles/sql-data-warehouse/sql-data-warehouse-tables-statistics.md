---
title: Oluşturma, istatistikleri - Azure SQL Data Warehouse güncelleştirme | Microsoft Docs
description: Öneriler ve örnekleri oluşturma ve Azure SQL Data Warehouse tablolarda sorgu iyileştirme istatistikleri güncelleştirme.
services: sql-data-warehouse
author: ckarst
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 05/09/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: bbc6a5083aebba40885700cab6c67128c9d9f916
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34643439"
---
# <a name="creating-updating-statistics-on-tables-in-azure-sql-data-warehouse"></a>Azure SQL Data Warehouse tablolarda istatistikleri güncelleştirmeyi oluşturma
Öneriler ve örnekleri oluşturma ve Azure SQL Data Warehouse tablolarda sorgu iyileştirme istatistikleri güncelleştirme.

## <a name="why-use-statistics"></a>İstatistikleri neden kullanılır?
Azure SQL Data Warehouse verilerinizi hakkında daha fazla bilir, o kadar hızlı sorgular çalıştırabilirsiniz. Verilerinizi istatistikleri toplama ve SQL Data Warehouse'a veri yükleme sorgularınızı iyileştirmek için yapabileceğiniz en önemli şeyler biridir. SQL veri ambarı sorgu iyileştiricisi maliyet tabanlı iyileştirici olmasıdır. Çeşitli sorgu planlarının maliyetlerini karşılaştırarak en düşük maliyetli planı seçer. Çoğu durumda bu, en hızlı yürütülen plan olur. Sorgunuzda filtreleme tarih bir satır döndürülecek iyileştirici tahminleri varsa, seçilen tarihten tahminleri 1 milyon satır döndürür daha Örneğin, onu farklı bir plan seçebilirsiniz.

## <a name="automatic-creation-of-statistics"></a>İstatistikleri otomatik olarak oluşturulması
Otomatik oluşturduğunuzda istatistikleri seçeneği, AUTO_CREATE_STATISTICS, SQL Data Warehouse için istatistikleri eksik olan sütunları tek sütun istatistikleri oluşturulduğu gelen kullanıcı sorgularının analiz eder. Sorgu iyileştiricisi sorgu planı için önem düzeyi tahminleri artırmak için sorgu koşulu veya birleşim koşulu tek tek sütunlarda İstatistikler oluşturur. İstatistikleri otomatik olarak oluşturulması şu anda varsayılan olarak açıktır.

Veri ambarı bu aşağıdaki komutu çalıştırarak yapılandırılmış olup olmadığını denetleyebilirsiniz:

```sql
SELECT name, is_auto_create_stats_on 
FROM sys.databases
```
Veri ambarınız yapılandırılmış AUTO_CREATE_STATISTICS yoksa, aşağıdaki komutu çalıştırarak bu özelliği etkinleştirmek öneririz:

```sql
ALTER DATABASE <yourdatawarehousename> 
SET AUTO_CREATE_STATISTICS ON
```
Aşağıdaki deyimleri İstatistikleri otomatik olarak oluşturulmasını tetikleyecek: Seç, Ekle seçin, CTAS, güncelleştirme, silme ve açıklama bir birleştirme içerdiğinde veya bir koşul varlığını algılandı. 

> [!NOTE]
> İstatistikleri otomatik olarak oluşturulmasını, geçici veya dış tablolarda oluşturulmaz.
> 

Sütunlar için oluşturuldukları istatistikleri zaten yoksa hafif düşürülmüş sorgu performansı maruz kalabilirsiniz şekilde İstatistikleri otomatik olarak oluşturulması zaman uyumlu olarak oluşturulur. İstatistik oluşturma tablo boyutuna bağlı olarak tek bir sütun üzerinde birkaç saniye sürebilir. Ölçme performansı değerlendirmesi, özellikle de performans düşüşünü önlemek için istatistiği sistem profil önce Kıyaslama iş yükü yürüterek ilk oluşturulmuş emin olmalısınız.

> [!NOTE]
> İstatistikleri oluşturulmasını de kaydedilir [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016) farklı bir kullanıcı bağlamında.
> 

Otomatik istatistik oluşturduğunuzda, formun sürer: _WA_Sys_< onaltılık 8 basamaklı sütun kimliği > _ < onaltılık 8 basamaklı tablo kimliği >. Çalıştırarak önceden oluşturulmuş istatistiklerini görüntüleyebilirsiniz [DBCC SHOW_STATISTICS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?view=sql-server-2017) komutu:

```sql
DBCC SHOW_STATISTICS (<tablename>, <targetname>)
```
İlk bağımsız değişken görüntülemek için istatistikleri içeren bir tablodur. Bu, bir dış tablo olamaz. İkinci bağımsız değişken hedef dizin, istatistik veya sütunu için istatistik bilgileri görüntülenecek addır.



## <a name="updating-statistics"></a>İstatistikleri güncelleştirme

Bir en iyi uygulama yeni tarihleri eklendikçe her gün güncelleştirmektir tarih sütunlarının ilgili istatistikler. Her zaman yeni satırlar veri ambarına yüklenir, yeni yükleme veya işlem tarihleri eklenir. Bu, veri dağıtımı değiştirmek ve istatistikleri güncel sağlamak. Değerlerin dağıtım genellikle değişmediğinden buna karşılık, bir müşteri tablosundaki Ülke sütunu istatistiklerle hiçbir zaman güncelleştirilmesi, gerekebilir. Dağıtım müşterileri arasında sabit olduğunu varsayarak, yeni satırlar için tablo değişim ekleme veri dağıtım değiştirmek için adımıdır değil. Ancak, veri Ambarınızı yalnızca bir ülkede varsa ve yeni bir ülkeden verilerde Getir depolanmakta, birden fazla ülkede verilerden sonuçta sonra Ülke sütunu istatistiklerle güncelleştirmeniz gerekir.

İstatistikleri güncelleştirmeyi öneriler şunlardır:

| **İstatistikleri güncelleştirmelerinin sıklığını** | Koruyucu: günlük <br></br> Yükleme veya veri dönüştürme sonra || **Örnekleme** |  1 milyondan az satırlara varsayılan örnekleme (yüzde 20) kullanın <br></br> 2-yüzde aralığı istatistiklerle 1 milyardan fazla satır ile iyi |

Sorgu zaman gidermeye çalışıyorsanız sorulacak ilk sorular biri, **"istatistikleri güncel misiniz?"**

Bu soru verilerin yaşını tarafından yanıtlanan biri değil. Güncel istatistikleri nesnenin temel alınan veri malzeme hiçbir değişiklik yapıldıysa eski olabilir. Satır sayısını önemli ölçüde değişti veya bir sütunun değerlerini dağıtımını maddi bir değişiklik olduğunda *sonra* istatistikleri güncelleştirme zamanı geldi.

Son zaman istatistikleri güncelleştirmesinden tablodaki verileri değişip değişmediğini belirlemek için hiç dinamik yönetim görünümünü olduğundan, istatistikleri yaşını bilerek, resmi bir parçası olan sağlayabilirsiniz.  Her bir tabloda, istatistikleri güncelleştirildiği son saat belirlemek için aşağıdaki sorguyu kullanın.  

> [!NOTE]
> Bir sütunun değerlerini dağıtımını maddi bir değişiklik varsa, bunlar güncelleştirildiği son zaman bağımsız olarak istatistiklerini güncelle unutmayın.  
> 
> 

```sql
SELECT
    sm.[name] AS [schema_name],
    tb.[name] AS [table_name],
    co.[name] AS [stats_column_name],
    st.[name] AS [stats_name],
    STATS_DATE(st.[object_id],st.[stats_id]) AS [stats_last_updated_date]
FROM
    sys.objects ob
    JOIN sys.stats st
        ON  ob.[object_id] = st.[object_id]
    JOIN sys.stats_columns sc    
        ON  st.[stats_id] = sc.[stats_id]
        AND st.[object_id] = sc.[object_id]
    JOIN sys.columns co    
        ON  sc.[column_id] = co.[column_id]
        AND sc.[object_id] = co.[object_id]
    JOIN sys.types  ty    
        ON  co.[user_type_id] = ty.[user_type_id]
    JOIN sys.tables tb    
        ON  co.[object_id] = tb.[object_id]
    JOIN sys.schemas sm    
        ON  tb.[schema_id] = sm.[schema_id]
WHERE
    st.[user_created] = 1;
```

**Tarih sütunları** veri ambarında, örneğin, genellikle istatistikleri güncelleştirmeleri sık. Her zaman yeni satırlar veri ambarına yüklenir, yeni yükleme veya işlem tarihleri eklenir. Bu, veri dağıtımı değiştirmek ve istatistikleri güncel sağlamak.  Buna karşılık, bir müşteri tablosu cinsiyetiniz sütununda istatistiklerle hiçbir zaman güncelleştirilmesi gerekebilir. Dağıtım müşterileri arasında sabit olduğunu varsayarak, yeni satırlar için tablo değişim ekleme veri dağıtım değiştirmek için adımıdır değil. Veri ambarınız tek cinsiyeti ve birden çok genders yeni gereksinimi sonuçlarında içeriyorsa, ancak daha sonra cinsiyetiniz sütun istatistiklerle güncelleştirmeniz gerekir.

Daha fazla bilgi için genel yönergeler için bkz: [istatistikleri](/sql/relational-databases/statistics/statistics).

## <a name="implementing-statistics-management"></a>İstatistikleri yönetimini uygulama
İstatistikleri yük sonunda güncelleştirildiğinden emin olmak için veri yükleme işlemi genişletmek için genellikle iyi bir fikirdir. Tabloları boyutlarına ve/veya bunların değerleri dağıtımını en sık değiştirdiğinizde veri yükü var. Bu nedenle, bu bazı yönetim işlemlerini uygulamak için bir mantıksal yerdir.

Yükleme işlemi sırasında İstatistikleri güncelleştirmek için aşağıdaki temel ilkeler sağlanır:

* Yüklenen her tablo güncelleştirilmiş en az bir istatistik nesnesi olduğundan emin olun. Bu tablo boyutu (satır sayısı ve sayfa sayısı) bilgileri istatistikleri güncelleştirmesinin bir parçası olarak güncelleştirir.
* BİRLEŞTİRME, GROUP BY, ORDER BY ve DISTINCT yan tümcelerinde katılan sütunları odaklanır.
* İşlem daha sık tarihleri gibi bu değerleri istatistikleri histogram dahil edilmeyen olduğundan "anahtar artan" sütunlarını güncelleştirme göz önünde bulundurun.
* Statik dağıtım sütunları daha az sıklıkla güncelleştirmeyi deneyin.
* Unutmayın, her istatistik nesne sırayla güncelleştirilir. Yalnızca uygulama `UPDATE STATISTICS <TABLE_NAME>` her zaman özellikle istatistikleri nesnelerin çok geniş tablolar için ideal değildir.

Daha fazla bilgi için bkz: [kardinalite tahmin](/sql/relational-databases/performance/cardinality-estimation-sql-server).

## <a name="examples-create-statistics"></a>Örnekler: istatistikler oluşturma
Bu örnekler istatistikleri oluşturmak için çeşitli seçenekler kullanmak nasıl gösterir. Her sütun için kullandığınız seçenekler verilerinizi ve sütunu sorguda nasıl kullanılacağını özelliklerine bağlıdır.

### <a name="create-single-column-statistics-with-default-options"></a>Varsayılan seçeneklerle tek sütunlu istatistikler oluşturma
Bir sütun üzerinde istatistik oluşturmak için istatistikleri nesnesi için bir ad ve sütun adını belirtmeniz yeterlidir.

Bu sözdiziminin tüm varsayılan seçenekleri kullanır. Varsayılan olarak, SQL Data Warehouse örnekleri **yüzde 20** istatistikleri oluşturduğunda, tablonun.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satır inceleyerek tek sütunlu istatistikler oluşturma
Yüzde 20 varsayılan örnekleme oranını çoğu durumlar için yeterli olur. Bununla birlikte, örnekleme hızını ayarlayabilirsiniz.

Tam tablo örneklemek için şu sözdizimini kullanın:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutunu belirterek tek sütunlu istatistikler oluşturma
Alternatif olarak, örnek boyutu bir yüzde olarak belirtebilirsiniz:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Tek sütunlu İstatistikler yalnızca bazı satırları oluşturma
Ayrıca, satırları bir kısmı tablonuzda istatistik oluşturabilirsiniz. Bu filtrelenmiş istatistiği çağrılır.

Örneğin, büyük bölümlenmiş bir tablodaki belirli bir bölüm sorgulamak planlama yaparken, filtrelenmiş istatistik kullanabilirsiniz. İstatistikler yalnızca bölüm değerlerine oluşturmayı tarafından istatistikleri doğruluğunu artırmak ve bu nedenle sorgu performansını artırmak.

Bu örnek değerleri çeşitli İstatistikler oluşturur. Değerleri bir bölümdeki değerleri aralığı eşleştirmek için kolayca tanımlanabilir.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Sorgu iyileştiricisi dağıtılmış sorgu planı seçtiğinde filtrelenmiş istatistik kullanmayı sorgu istatistiklerini nesnesinin tanımı sığması gerekir. Önceki örnekte, burada yan tümcesi 2000101 ve 20001231 arasındaki col1 değerleri belirtilmesi gerekiyor sorgunun kullanma.
> 
> 

### <a name="create-single-column-statistics-with-all-the-options"></a>Tüm seçeneklerle tek sütunlu istatistikler oluşturma
Ayrıca, seçenekleri birlikte birleştirebilirsiniz. Aşağıdaki örnek, bir özel örnek boyutu ile filtrelenmiş istatistik nesnesi oluşturur:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Tam başvuru için bkz: [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql).

### <a name="create-multi-column-statistics"></a>Çok sütunlu istatistikler oluşturma
Bir çok sütunlu İstatistikler nesnesi oluşturmak için yalnızca önceki örneklerde kullanır, ancak daha fazla sütun belirtin.

> [!NOTE]
> Sorgu sonucu satır sayısı tahmin etmek için kullanılan histogram yalnızca istatistikleri nesne tanımı'nda listelenen ilk sütun için kullanılabilir.
> 
> 

Bu örnekte, üzerinde çubuk grafik ise *ürün\_kategori*. Çapraz sütunlu İstatistikler hesaplanır *ürün\_kategori* ve *ürün\_sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Arasında bir ilişki olduğundan *ürün\_kategori* ve *ürün\_alt\_kategori*, çok sütunlu İstatistikler nesnesinde bu durumunda yararlı olabilir sütunlar aynı anda erişilir.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Bir tablodaki tüm sütunları istatistikler oluşturma
İstatistik oluşturmak için bir yol tablosu oluşturduktan sonra CREATE STATISTICS komutları vermektir:

```sql
CREATE TABLE dbo.table1
(
   col1 int
,  col2 int
,  col3 int
)
WITH
  (
    CLUSTERED COLUMNSTORE INDEX
  )
;

CREATE STATISTICS stats_col1 on dbo.table1 (col1);
CREATE STATISTICS stats_col2 on dbo.table2 (col2);
CREATE STATISTICS stats_col3 on dbo.table3 (col3);
```

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Veritabanındaki tüm sütunları istatistikleri oluşturmak için bir saklı yordam kullanın
SQL Data Warehouse, SQL Server'da sp_create_stats için eşdeğer bir sistem saklı yordamı yok. Bu saklı yordam istatistikleri zaten sahip değil veritabanının her sütunda bir tek sütun istatistikleri nesnesi oluşturur.

Aşağıdaki örnek, veritabanı tasarımınız ile çalışmaya başlamanıza yardımcı olur. Gereksinimlerinize uyarlamak çekinmeyin:

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_create_stats]
(   @create_type    tinyint -- 1 default 2 Fullscan 3 Sample
,   @sample_pct     tinyint
)
AS

IF @create_type IS NULL
BEGIN
    SET @create_type = 1;
END;

IF @create_type NOT IN (1,2,3)
BEGIN
    THROW 151000,'Invalid value for @stats_type parameter. Valid range 1 (default), 2 (fullscan) or 3 (sample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN;
    DROP TABLE #stats_ddl;
END;

CREATE TABLE #stats_ddl
WITH    (   DISTRIBUTION    = HASH([seq_nmbr])
        ,   LOCATION        = USER_DB
        )
AS
WITH T
AS
(
SELECT      t.[name]                        AS [table_name]
,           s.[name]                        AS [table_schema_name]
,           c.[name]                        AS [column_name]
,           c.[column_id]                   AS [column_id]
,           t.[object_id]                   AS [object_id]
,           ROW_NUMBER()
            OVER(ORDER BY (SELECT NULL))    AS [seq_nmbr]
FROM        sys.[tables] t
JOIN        sys.[schemas] s         ON  t.[schema_id]       = s.[schema_id]
JOIN        sys.[columns] c         ON  t.[object_id]       = c.[object_id]
LEFT JOIN   sys.[stats_columns] l   ON  l.[object_id]       = c.[object_id]
                                    AND l.[column_id]       = c.[column_id]
                                    AND l.[stats_column_id] = 1
LEFT JOIN    sys.[external_tables] e    ON    e.[object_id]        = t.[object_id]
WHERE       l.[object_id] IS NULL
AND            e.[object_id] IS NULL -- not an external table
)
SELECT  [table_schema_name]
,       [table_name]
,       [column_name]
,       [column_id]
,       [object_id]
,       [seq_nmbr]
,       CASE @create_type
        WHEN 1
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+')' AS VARCHAR(8000))
        WHEN 2
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH FULLSCAN' AS VARCHAR(8000))
        WHEN 3
        THEN    CAST('CREATE STATISTICS '+QUOTENAME('stat_'+table_schema_name+ '_' + table_name + '_'+column_name)+' ON '+QUOTENAME(table_schema_name)+'.'+QUOTENAME(table_name)+'('+QUOTENAME(column_name)+') WITH SAMPLE '+CONVERT(varchar(4),@sample_pct)+' PERCENT' AS VARCHAR(8000))
        END AS create_stat_ddl
FROM T
;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''
;

WHILE @i <= @t
BEGIN
    SET @s=(SELECT create_stat_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

İstatistikleri varsayılan değerleri kullanarak tablodaki tüm sütunların oluşturmak için basitçe yordam çağrısı.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 1, NULL;
```
Bir fullscan kullanarak tablodaki tüm sütunları istatistikleri oluşturmak için bu yordamı arayın:

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```
Tablodaki tüm sütunların örneklenen istatistikleri oluşturmak için 3 ve örnek yüzde girin.  Bu yordamlar, yüzde 20 örnek hızı kullanır.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```


Örneklenen tüm sütunları istatistik oluşturmak için 

## <a name="examples-update-statistics"></a>Örnekler: İstatistikleri güncelleştir
İstatistikleri güncelleştirmek için şunları yapabilirsiniz:

- Bir istatistik Nesne güncelleştirilemiyor. Güncelleştirmek istediğiniz istatistikleri nesnesinin adını belirtin.
- Bir tablodaki tüm istatistikleri nesneleri güncelleştirin. Bir özel istatistikleri nesne yerine tablo adını belirtin.

### <a name="update-one-specific-statistics-object"></a>Güncelleştirme belirli bir istatistik nesnesi
Belirli istatistikleri nesneyi güncelleştirmek için aşağıdaki sözdizimini kullanın:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Örneğin:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Belirli istatistikleri nesneleri güncelleştirerek zaman ve kaynak istatistikleri yönetmek için gereken en aza indirebilirsiniz. Bu, bazı güncelleştirmek için en iyi istatistiklerini nesneleri seçmek için zorlayıcı gerektirir.

### <a name="update-all-statistics-on-a-table"></a>Bir tablodaki tüm istatistikleri güncelleştir
Bu tabloda tüm istatistikleri nesnelerini güncelleştirme için basit bir yöntemi gösterir:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Örneğin:

```sql
UPDATE STATISTICS dbo.table1;
```

Bu kullanımı kolay açıklamadır. Bu güncelleştirmeler hemen unutmayın *tüm* tablo istatistikleri ve bu nedenle gerekli olandan daha fazla iş gerçekleştirebilir. Performans sorunu değilse, istatistikleri güncel olmasını garanti etmek için en kolay ve eksiksiz yolu budur.

> [!NOTE]
> Bir tablodaki tüm istatistikleri güncelleştirirken, SQL veri ambarı örneği tablonun her istatistikleri nesne için bir tarama yapar. Tablo büyük ve çok sayıda sütun ve birçok istatistikleri varsa, tek tek istatistikleri gereksinimleri temelinde güncelleştirmek için daha etkili olabilir.
> 
> 

Bir uygulama için bir `UPDATE STATISTICS` yordamı, bkz: [geçici tablolar](sql-data-warehouse-tables-temporary.md). Uygulama yöntemi önceki öğesinden biraz farklıdır `CREATE STATISTICS` yordamı, ancak sonuç aynıdır.

Tam sözdizimi için bkz: [Update STATISTICS](/sql/t-sql/statements/update-statistics-transact-sql).

## <a name="statistics-metadata"></a>İstatistikleri meta verileri
Çeşitli sistem görünümleri ve İstatistikler hakkında bilgi bulmak için kullanabileceğiniz işlevleri vardır. Örneğin, bir istatistik nesne istatistikleri en son oluşturulan veya güncelleştirilen zaman görmek için istatistikleri tarih işlevini kullanarak güncel olabilir, görebilirsiniz.

### <a name="catalog-views-for-statistics"></a>Katalog görünümleri istatistikleri
Bu sistem görünümleri İstatistikler hakkında bilgi sağlar:

| Katalog görünümü | Açıklama |
|:--- |:--- |
| [sys.Columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql) |Her sütun için bir satır. |
| [sys.objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Veritabanında her nesne için bir satır. |
| [sys.schemas](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Veritabanındaki her şema için bir satır. |
| [sys.stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql) |Her bir istatistik nesne için bir satır. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql) |İstatistikleri nesnesindeki her sütun için bir satır. Sys.columns geri bağlantılar. |
| [sys.Tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql) |(Dış tablolara içerir) her tablo için bir satır. |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql) |Her bir veri türü için bir satır. |

### <a name="system-functions-for-statistics"></a>Sistem işlevleri için istatistikleri
Bu sistem işlevler istatistikleri ile çalışmak için yararlıdır:

| Sistem işlevi | Açıklama |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql) |İstatistikleri nesne son güncelleştirildiği tarihi. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql) |Özet düzeyi ve ayrıntılı dağıtımı hakkında bilgi istatistikleri nesne tarafından anlaşılan gibi değerler. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Bir görünüme istatistikleri sütunları ve işlevlerini birleştirme
Bu görünüm istatistikleri ilişkili sütun getirir ve STATS_DATE() işlevinden birlikte sonuçlanır.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_defiinition]
,       st.[has_filter]                     AS [stats_is_filtered]
,       STATS_DATE(st.[object_id],st.[stats_id])
                                            AS [stats_last_updated_date]
,       co.[name]                           AS [stats_column_name]
,       ty.[name]                           AS [column_type]
,       co.[max_length]                     AS [column_max_length]
,       co.[precision]                      AS [column_precision]
,       co.[scale]                          AS [column_scale]
,       co.[is_nullable]                    AS [column_is_nullable]
,       co.[collation_name]                 AS [column_collation_name]
,       QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS two_part_name
,       QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])
                                            AS three_part_name
FROM    sys.objects                         AS ob
JOIN    sys.stats           AS st ON    ob.[object_id]      = st.[object_id]
JOIN    sys.stats_columns   AS sc ON    st.[stats_id]       = sc.[stats_id]
                            AND         st.[object_id]      = sc.[object_id]
JOIN    sys.columns         AS co ON    sc.[column_id]      = co.[column_id]
                            AND         sc.[object_id]      = co.[object_id]
JOIN    sys.types           AS ty ON    co.[user_type_id]   = ty.[user_type_id]
JOIN    sys.tables          AS tb ON  co.[object_id]        = tb.[object_id]
JOIN    sys.schemas         AS sm ON  tb.[schema_id]        = sm.[schema_id]
WHERE   1=1
AND     st.[user_created] = 1
;
```

## <a name="dbcc-showstatistics-examples"></a>DBCC SHOW_STATISTICS() örnekleri
DBCC SHOW_STATISTICS() istatistikleri nesnesi içinde tutulan verileri gösterir. Bu veriler üç bölümlerinde gelir:

- Üst bilgi
- Yoğunluğu vektör
- Çubuk grafik

İstatistikler hakkında üstbilgi meta veriler. Histogram değerleri dağıtım istatistikleri nesnesi ilk anahtar sütununda görüntüler. Yoğunluğu vektör arası sütun bağıntı ölçer. SQL veri ambarı nicelik tahminlerde istatistikleri nesnesindeki verilerin hesaplar.

### <a name="show-header-density-and-histogram"></a>Üstbilgi, yoğunluğu ve histogram Göster
Bu basit bir örnek istatistikleri nesnesinin tüm üç bölümden gösterir:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>DBCC SHOW_STATISTICS() bir veya daha fazla bölümlerini göster
Yalnızca belirli bölümlerini görüntüleme ilgilendiğiniz kullanırsanız `WITH` yan tümcesi ve görmek istediğiniz hangi bölümlerinin belirtin:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() farklar
DBCC SHOW_STATISTICS() SQL veri ambarı SQL Server'a karşılaştırıldığında daha kesinlikle uygulanır:

- Belgelenmemiş özellikler desteklenmez.
- Stats_stream kullanamazsınız.
- İstatistik verileri belirli alt kümeleri için sonuçlarını katılamaz. Örneğin, (birleştirme STAT_HEADER DENSITY_VECTOR).
- NO_INFOMSGS ileti gizleme için ayarlanamaz.
- Köşeli parantezler istatistikleri adları içinde kullanılamaz.
- Sütun adları istatistikleri nesneleri tanımlamak için kullanılamaz.
- Özel hata 2767 desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar
İçin daha fazla sorgu performansı artırır, bkz: [, iş yükü izleme](sql-data-warehouse-manage-monitor.md)

