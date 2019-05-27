---
title: Oluşturma, güncelleştiriliyor istatistik - Azure SQL veri ambarı | Microsoft Docs
description: Öneriler ve iyileştirme sorgu istatistikleri tablolarda Azure SQL veri ambarı oluşturma ve güncelleştirme için örnekler.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 05/09/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seoapril2019
ms.openlocfilehash: c5043d99dd130bc7dc7b35eaa5ecadf11d7644db
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851536"
---
# <a name="table-statistics-in-azure-sql-data-warehouse"></a>Azure SQL Data warehouse'da tablo istatistikleri

Öneriler ve iyileştirme sorgu istatistikleri tablolarda Azure SQL veri ambarı oluşturma ve güncelleştirme için örnekler.

## <a name="why-use-statistics"></a>İstatistikleri neden kullanılır?

Azure SQL veri ambarı verilerinizi hakkında daha fazla bilir, daha hızlı sorgular çalıştırabilirsiniz. Veriler SQL veri ambarı'na yüklendikten sonra verilerinizi temel istatistikleri toplama sorgularınızı iyileştirmeniz için yapabileceğiniz en önemli şeylerden biridir. SQL Veri Ambarı sorgu iyileştiricisi maliyet tabanlı bir iyileştiricidir. Çeşitli sorgu planlarına maliyetini karşılaştırır ve daha sonra planla birlikte en düşük maliyetle seçer. Çoğu durumda en hızlı yürütme planı seçer. Örneğin, sorgunuz üzerinde filtreleme tarih bir satır döndürür iyileştirici tahminleri, bir plan seçersiniz. Seçilen tarih, 1 milyon satır döndürür tahminleri, başka bir plana döndürür.

## <a name="automatic-creation-of-statistic"></a>İstatistikleri otomatik olarak oluşturulması

SQL veri ambarı veritabanı AUTO_CREATE_STATISTICS seçeneği etkin olduğunda, gelen kullanıcı sorgularındaki eksik istatistikleri analiz eder. İstatistikleri eksikse, sorgu iyileştiricisi istatistikleri tek tek sütunlardaki kardinalite tahmin için sorgu planı geliştirmek için sorgu karşılaştırma veya birleştirme koşulu oluşturur. İstatistikleri otomatik olarak oluşturulması şu anda varsayılan olarak etkinleştirilir.

Veri ambarınız AUTO_CREATE_STATISTICS aşağıdaki komutu çalıştırarak yapılandırılmış olup olmadığını kontrol edebilirsiniz:

```sql
SELECT name, is_auto_create_stats_on
FROM sys.databases
```

Yapılandırılmış AUTO_CREATE_STATISTICS veri ambarınız yoksa, aşağıdaki komutu çalıştırarak bu özelliği etkinleştirmeniz önerilir:

```sql
ALTER DATABASE <yourdatawarehousename>
SET AUTO_CREATE_STATISTICS ON
```

Bu deyimler, İstatistikleri otomatik olarak oluşturulmasını tetikleyecek:

- SEÇ
- INSERT SEÇİN
- CTAS
- UPDATE
- DELETE
- Bir birleştirme veya bir koşul varlığı içeren algılandığında AÇIKLAYIN

> [!NOTE]
> İstatistikleri otomatik olarak oluşturulması, geçici veya dış tablolarda oluşturulmaz.

İstatistikleri sütunlarınızı eksikse biraz düşürülmüş sorgu performansı gerektirebilir. Bu nedenle İstatistikleri otomatik olarak oluşturulması zaman uyumlu olarak gerçekleştirilir. Tek bir sütunu için istatistik oluşturulması zaman tablo boyutuna bağlıdır. Performans değerlendirmesi, özellikle de ölçülebilir bir performans düşüşüne önlemek için istatistikleri Kıyaslama iş yükünün yürüterek sistem profil oluşturmadan önce ilk oluşturulmuş emin olmalısınız.

> [!NOTE]
> İstatistikleri oluşturulmasını kaydedilir [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) farklı kullanıcı bağlamı altında.

Otomatik İstatistikler oluşturduğunuzda, bunlar formun gerçekleştirir: _WA_Sys_< onaltılık basamak 8 sütun kimliği > _ < onaltılık basamak 8 tablo kimliği >. Çalıştırarak önceden oluşturulmuş istatistikleri görüntüleyebilirsiniz [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql?view=azure-sqldw-latest) komutu:

```sql
DBCC SHOW_STATISTICS (<table_name>, <target>)
```

Table_name görüntülemek için istatistikleri içeren bir tablo adıdır. Bu, bir dış tablo olamaz. Hedef dizin, istatistik veya istatistik bilgilerini görüntülenecek sütun adını hedefidir.

## <a name="updating-statistics"></a>İstatistikleri güncelleştirmeyi

Yeni tarihleri eklendikçe istatistikleri Tarih sütunlarını her gün güncelleştirmek için bir en iyi yöntemidir. Her zaman yeni satır, veri ambarı'na yüklenen, yeni yükleme veya işlem tarihleri eklenir. Bu veri dağıtım değiştirmek ve istatistikleri güncel olun. Değer dağılımı genellikle değişmediğinden buna karşılık, bir Müşteri tablosunda bir ülke/bölge sütun istatistikleri hiçbir zaman güncelleştirilmesi gerekebilir. Dağıtım müşteriler arasında sabit olduğu varsayıldığında, yeni satırlar için tablo değişim ekleme veri dağıtım değişmesini değil. Ülke/bölge sütununda istatistiklerin güncelleştirilmesi gerekir ancak, veri ambarınız bir ülke/bölge yalnızca içerir ve yeni bir ülke/bölgeden veri getirin, birden çok ülkeler/bölgeler depolanmakta olan verileri olmasıyla sonuçlanır.

İstatistikleri güncelleştirmeyi öneriler şunlardır:

|||
|-|-|
| **İstatistiklerini güncelleştirme sıklığı**  | Koruyucu: Günlük </br> Yükleme veya veri dönüştürme sonra |
| **Örnekleme** |  1 milyardan az satır varsayılan örnekleme (yüzde 20) kullanın. </br> 1 milyardan fazla satır ile iki oranında örnekleme kullanın. |

İlk hakkında soruların yanıtlarını ne zaman bir sorgu gidermeye çalışıyorsanız biridir, **"istatistikleri güncel misiniz?"**

Bu soruyu tarafından verilerin yaşını yanıtlanan biri değil. Güncel istatistikleri nesnenin, temel alınan verileri malzeme değişiklik yapıldıysa eski olabilir. Satır sayısını önemli ölçüde değişti veya bir sütunun değerlerini dağıtımını malzeme değişikliği olduğunda *ardından* istatistikleri güncelleştirme zamanı geldi.

Son zaman istatistikleri güncelleştirmesinden tablo içindeki veri değişip değişmediğini belirlemek için hiç dinamik yönetim görünümünü yoktur. İstatistiklerin yaşını bilerek, resmin bir bölümünü ile sağlayabilirsiniz. Aşağıdaki sorgu, istatistiklerinizi her tabloda güncelleştirildiği son zaman belirlemek için kullanabilirsiniz.

> [!NOTE]
> Bir sütunun değerlerini dağıtımını malzeme değişikliği varsa, bunlar güncelleştirildiği son zaman bakılmaksızın istatistikleri güncelleştirmeniz gerekir.

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

**Tarih sütunları** bir veri ambarı'nda, örneğin, genellikle istatistikleri güncelleştirmeleri sık. Her zaman yeni satır, veri ambarı'na yüklenen, yeni yükleme veya işlem tarihleri eklenir. Bu veri dağıtım değiştirmek ve istatistikleri güncel olun. Buna karşılık, bir Müşteri tablosunda bir cinsiyet sütun istatistikleri, hiçbir zaman güncelleştirilmesi gerekebilir. Dağıtım müşteriler arasında sabit olduğu varsayıldığında, yeni satırlar için tablo değişim ekleme veri dağıtım değişmesini değil. Veri ambarınız yalnızca bir cinsiyet ve birden çok cinsiyetleri yeni bir gereksinim sonuçlarında içeriyorsa, ancak daha sonra cinsiyet sütun istatistikleri güncelleştirmeniz gerekir.

Daha fazla bilgi için genel yönergeler için bkz. [istatistikleri](/sql/relational-databases/statistics/statistics).

## <a name="implementing-statistics-management"></a>İstatistikleri yönetimini uygulama

İstatistikleri yük sonunda güncelleştirildiğinden emin olmak için veri yükleme işlemi genişletmek için genellikle iyi bir fikirdir. Tablolar, boyutunun ve/veya bunların dağıtım değerlerinin en sık değiştirdiğinizde veri yüktür. Bu nedenle, bazı yönetim işlemleri uygulamak üzere mantıksal bir yerde budur.

Yükleme işlemi sırasında istatistiklerinizi güncelleştirmek için aşağıdaki yol gösterici ilkeler sağlanır:

* Yüklenen her tablonun en az bir istatistik nesne güncelleştirilmiş olduğundan emin olun. Bu tablo boyutu (satır sayısı ve sayfa sayısı) bilgileri istatistikleri güncelleştirmeyi bir parçası olarak güncelleştirir.
* BİRLEŞTİRME, GROUP BY, ORDER BY ve DISTINCT yan tümcelerinde katılan sütunlar üzerinde odaklanın.
* İşlem daha sık tarihleri gibi bu değerleri istatistikleri histogramda dahil edilmemesi için "anahtar artan" sütunları güncelleştirme göz önünde bulundurun.
* Statik dağıtım sütunları daha az sık güncelleştirme seçeneğini değerlendirin.
* Unutmayın, her istatistik nesne dizisi güncelleştirilir. Yalnızca uygulama `UPDATE STATISTICS <TABLE_NAME>` her zaman, özellikle çok sayıda istatistikleri nesnelerin içeren geniş tablolarda için ideal değildir.

Daha fazla bilgi için [kardinalite tahmini](/sql/relational-databases/performance/cardinality-estimation-sql-server).

## <a name="examples-create-statistics"></a>Örnekler: İstatistik oluşturma

Bu örnekler istatistikleri oluşturmak için çeşitli seçenekler kullanmak nasıl gösterir. Her sütun için kullandığınız seçenekleri, verilerinizi ve sütun sorgularda nasıl kullanılacağını özelliklerini bağlıdır.

### <a name="create-single-column-statistics-with-default-options"></a>Varsayılan seçeneklerle tek sütunlu istatistikler oluşturma

Sütun istatistikleri oluşturmak için yalnızca istatistikleri nesne için bir ad ve sütunun adını sağlayın.

Bu söz dizimi varsayılan seçeneklerin tümünü kullanır. Varsayılan olarak, SQL veri ambarı örnekleri **yüzde 20 oranında** istatistikleri oluşturduğunda, tablo.

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]);
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1);
```

### <a name="create-single-column-statistics-by-examining-every-row"></a>Her satır inceleyerek tek sütunlu istatistikler oluşturma

Yüzde 20 oranında varsayılan örnekleme hızını, çoğu durum için yeterlidir. Bununla birlikte, örnekleme hızını ayarlayabilirsiniz.

Tam tablo örneği için bu sözdizimini kullanın:

```sql
CREATE STATISTICS [statistics_name] ON [schema_name].[table_name]([column_name]) WITH FULLSCAN;
```

Örneğin:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH FULLSCAN;
```

### <a name="create-single-column-statistics-by-specifying-the-sample-size"></a>Örnek boyutu belirterek tek sütunlu istatistikler oluşturma

Alternatif olarak, örnek boyutu bir yüzde olarak belirtebilirsiniz:

```sql
CREATE STATISTICS col1_stats ON dbo.table1 (col1) WITH SAMPLE = 50 PERCENT;
```

### <a name="create-single-column-statistics-on-only-some-of-the-rows"></a>Yalnızca bazı satırları tek sütunlu istatistikler oluşturma

Ayrıca, satırları bir kısmı üzerinde tablonuzda istatistik oluşturabilirsiniz. Bu, filtrelenmiş istatistiği çağrılır.

Örneğin, büyük ve bölümlenmiş bir tablodaki belirli bir bölümünü sorgulamak planlama yaparken, filtrelenmiş istatistik kullanabilirsiniz. Yalnızca bölüm değerlerine istatistikleri oluşturarak istatistikleri doğruluğunu ve bu nedenle sorgu performansını geliştirmek.

Bu örnek, bir aralıktaki değerleri üzerinde İstatistikler oluşturur. Değerleri, bir bölümdeki değerleri aralığı eşleşecek şekilde kolayca tanımlanabilir.

```sql
CREATE STATISTICS stats_col1 ON table1(col1) WHERE col1 > '2000101' AND col1 < '20001231';
```

> [!NOTE]
> Sorgu iyileştiricisi dağıtılmış sorgu planını seçtiğinde filtrelenmiş istatistik kullanmayı göz önünde sorgu istatistikleri nesnesinin tanımı sığması gerekir. Önceki örnekte, yan tümcesi 2000101 20001231 arasındaki col1 değerleri belirtmek için gereken yere sorgunun kullanma.

### <a name="create-single-column-statistics-with-all-the-options"></a>Tüm seçeneklerle tek sütunlu istatistikler oluşturma

Ayrıca, seçenekleri birlikte birleştirebilirsiniz. Aşağıdaki örnek bir özel örnek boyutu ile bir filtrelenmiş istatistik oluşturur:

```sql
CREATE STATISTICS stats_col1 ON table1 (col1) WHERE col1 > '2000101' AND col1 < '20001231' WITH SAMPLE = 50 PERCENT;
```

Tam başvuru için bkz: [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql).

### <a name="create-multi-column-statistics"></a>Çok sütunlu istatistikler oluşturma

Çok sütunlu İstatistikler nesne oluşturmak için yalnızca önceki örneklerde, ancak daha fazla sütun belirleyin.

> [!NOTE]
> Sorgu sonuç satır sayısını tahmin etmek için kullanılır, çubuk grafik, yalnızca ilk sütun istatistikleri nesne tanımı içinde listelenen kullanılabilir.

Bu örnekte, çubuk grafik açıktır *ürün\_kategori*. Çapraz-sütun istatistikleri hesaplanır *ürün\_kategori* ve *ürün\_sub_category*:

```sql
CREATE STATISTICS stats_2cols ON table1 (product_category, product_sub_category) WHERE product_category > '2000101' AND product_category < '20001231' WITH SAMPLE = 50 PERCENT;
```

Arasında bir bağıntı olduğundan *ürün\_kategori* ve *ürün\_alt\_kategori*, çok sütunlu İstatistikler nesnesinde bu varsa yararlı olabilir sütunları aynı anda erişebilir.

### <a name="create-statistics-on-all-columns-in-a-table"></a>Bir tablodaki tüm sütunlarda istatistikler oluşturma

İstatistikleri oluşturmak için bir yol tablosu oluşturulduktan sonra CREATE STATISTICS komutları vermektir:

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

### <a name="use-a-stored-procedure-to-create-statistics-on-all-columns-in-a-database"></a>Bir veritabanındaki tüm sütunlarında istatistik oluşturmak için bir saklı yordamı kullanın.

SQL veri ambarı, SQL Server için sp_create_stats eşdeğer bir sistem saklı yordam yok. Bu saklı yordamı zaten istatistikleri yok veritabanının her sütunu bir tek sütun istatistikleri nesnesi oluşturur.

Aşağıdaki örnek, veritabanı tasarımı ile çalışmaya başlamanıza yardımcı olur. İhtiyaçlarınıza uyum çekinmeyin:

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

Varsayılan değerleri kullanarak tablodaki tüm sütunların istatistiklerini oluşturmak için saklı yordamını yürütün.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 1, NULL;
```

Bir fullscan kullanarak tablodaki tüm sütunların istatistiklerini oluşturmak için bu yordamı çağırın:

```sql
EXEC [dbo].[prc_sqldw_create_stats] 2, NULL;
```

Tablodaki tüm sütunların örneklenen istatistikleri oluşturmak için 3 ve örnek yüzde girin. Bu yordamlar, yüzde 20 örnek hızı kullanır.

```sql
EXEC [dbo].[prc_sqldw_create_stats] 3, 20;
```

Tüm sütunlarındaki istatistikleri oluşturmak için örneklenir

## <a name="examples-update-statistics"></a>Örnekler: Update STATISTICS

İstatistikleri güncelle için şunları yapabilirsiniz:

- Bir istatistik Nesne güncelleştirilemiyor. Güncelleştirmek istediğiniz istatistikleri nesnesinin adını belirtin.
- Bir tablodaki tüm istatistikleri nesneleri güncelleştirin. Bir özel istatistikleri nesnesi yerine tablonun adını belirtin.

### <a name="update-one-specific-statistics-object"></a>Güncelleştirme belirli bir istatistik nesnesi

Belirli istatistikleri nesneyi güncelleştirmek için aşağıdaki sözdizimini kullanın:

```sql
UPDATE STATISTICS [schema_name].[table_name]([stat_name]);
```

Örneğin:

```sql
UPDATE STATISTICS [dbo].[table1] ([stats_col1]);
```

Belirli istatistikleri nesneleri güncelleştirerek zaman ve kaynak istatistikleri yönetmek için gereken en aza indirebilirsiniz. Bu, bazı düşündüğünüz güncelleştirmek için en iyi istatistiklerini nesneleri seçmenizi gerektirir.

### <a name="update-all-statistics-on-a-table"></a>Bir tablodaki tüm istatistiklerini güncelle

Bir tablodaki tüm istatistikleri nesneleri güncelleştirmek için kolay bir yöntemdir:

```sql
UPDATE STATISTICS [schema_name].[table_name];
```

Örneğin:

```sql
UPDATE STATISTICS dbo.table1;
```

UPDATE STATISTICS kullanımı kolay bir ifadedir. Bu güncelleştirmeleri hemen unutmayın *tüm* tablo istatistikleri ve bu nedenle gerekli olandan daha fazla iş gerçekleştirebilir. Performans sorunu değilse, istatistikleri güncel olduğunu garanti etmek için en kolay ve eksiksiz bir yolu budur.

> [!NOTE]
> Bir tablodaki tüm istatistikleri güncelleştirmeyi, SQL veri ambarı örneği tablo istatistikleri her nesne için bir tarama yapar. Tablo büyük ve çok sayıda sütun ve birçok istatistikleri varsa, ihtiyaca göre tek tek istatistiklerin güncelleştirilmesi daha verimli olabilir.

Bir uygulama için bir `UPDATE STATISTICS` yordamı için bkz [geçici tablolar](sql-data-warehouse-tables-temporary.md). Uygulama yöntemi önceki öğesinden biraz daha farklı olmasına `CREATE STATISTICS` yordamı, ancak sonuç aynıdır.

Tam sözdizimi için bkz [Update STATISTICS](/sql/t-sql/statements/update-statistics-transact-sql).

## <a name="statistics-metadata"></a>İstatistikleri meta verileri

Çeşitli sistem görünümleri ve istatistikleri hakkında bilgi almak için kullanabileceğiniz işlevleri vardır. Örneğin, bir istatistikleri nesne istatistikleri en son oluşturulan veya güncelleştirilen zaman istatistikleri tarih işlevi kullanarak eski olabilir, görebilirsiniz.

### <a name="catalog-views-for-statistics"></a>Katalog görünümleri istatistikleri

Bu sistem görünümleri istatistikleri hakkında bilgi sağlar:

| Katalog görünümü | Açıklama |
|:--- |:--- |
| [sys.columns](/sql/relational-databases/system-catalog-views/sys-columns-transact-sql) |Her sütun için bir satır. |
| [sys.objects](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Veritabanındaki her nesne için bir satır. |
| [sys.schemas](/sql/relational-databases/system-catalog-views/sys-objects-transact-sql) |Veritabanındaki her bir şema için bir satır. |
| [sys.stats](/sql/relational-databases/system-catalog-views/sys-stats-transact-sql) |Her istatistikleri nesne için bir satır. |
| [sys.stats_columns](/sql/relational-databases/system-catalog-views/sys-stats-columns-transact-sql) |Her sütunda istatistikleri nesne için bir satır. Sys.columns geri bağlar. |
| [sys.Tables](/sql/relational-databases/system-catalog-views/sys-tables-transact-sql) |(Dış tablolar içerir) her tablo için bir satır. |
| [sys.table_types](/sql/relational-databases/system-catalog-views/sys-table-types-transact-sql) |Her veri türü için bir satır. |

### <a name="system-functions-for-statistics"></a>Sistem işlevleri için istatistikleri

Bu sistem işlevlerini istatistikleriniz ile çalışmak için kullanışlıdır:

| System işlevi | Açıklama |
|:--- |:--- |
| [STATS_DATE](/sql/t-sql/functions/stats-date-transact-sql) |İstatistikleri nesne en son güncelleştirildiği tarih. |
| [DBCC SHOW_STATISTICS](/sql/t-sql/database-console-commands/dbcc-show-statistics-transact-sql) |Özet düzeyine ve ayrıntılı bilgileri istatistikleri nesne tarafından anlaşılabilecek bir değerler dağıtımı hakkında. |

### <a name="combine-statistics-columns-and-functions-into-one-view"></a>Bir görünüme istatistikleri sütunları ve işlevlerini birleştirme

Bu görünüm, istatistiklerle ilgili sütunları getirir ve STATS_DATE() işlevden birlikte sonuçlanır.

```sql
CREATE VIEW dbo.vstats_columns
AS
SELECT
        sm.[name]                           AS [schema_name]
,       tb.[name]                           AS [table_name]
,       st.[name]                           AS [stats_name]
,       st.[filter_definition]              AS [stats_filter_definition]
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
- Vektör yoğunluğu
- Histogram

İstatistikler hakkında üstbilgi meta veriler. Histogram istatistikleri nesnenin ilk anahtar sütunu değerlerinin dağıtım görüntüler. Yoğunluk vektör arası sütunlu bağıntı ölçer. SQL veri ambarı kardinalite tahminlerde istatistikleri nesne verileri hesaplar.

### <a name="show-header-density-and-histogram"></a>Üst bilgi, yoğunluğu ve histogram Göster

Bu basit örnekte, tüm üç istatistikleri nesne parçaları gösterilmektedir:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>)
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1);
```

### <a name="show-one-or-more-parts-of-dbcc-showstatistics"></a>DBCC SHOW_STATISTICS() bölümlerini bir veya daha fazla Göster

Yalnızca belirli bölümleri görüntülemek istiyorsanız kullanırsanız `WITH` yan tümcesi ve görmek istediğiniz hangi parçalarının belirtin:

```sql
DBCC SHOW_STATISTICS([<schema_name>.<table_name>],<stats_name>) WITH stat_header, histogram, density_vector
```

Örneğin:

```sql
DBCC SHOW_STATISTICS (dbo.table1, stats_col1) WITH histogram, density_vector
```

## <a name="dbcc-showstatistics-differences"></a>DBCC SHOW_STATISTICS() farkları

DBCC SHOW_STATISTICS() SQL Data warehouse'da SQL Server'a kıyasla daha katı şekilde uygulanır:

- Belgelenmemiş özellikler desteklenmez.
- Stats_stream kullanamazsınız.
- İstatistik verileri için belirli alt kümelerine sonuçlarını katılamaz. Örneğin, STAT_HEADER DENSITY_VECTOR katılın.
- NO_INFOMSGS ileti gizleme için ayarlanamaz.
- Köşeli parantezler istatistikleri adları içinde kullanılamaz.
- Sütun adları istatistikleri nesneleri tanımlamak için kullanılamaz.
- Özel hata 2767 desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

İçin daha fazla sorgu performansı artırır, bkz: [iş yükünüzü izleme](sql-data-warehouse-manage-monitor.md)
