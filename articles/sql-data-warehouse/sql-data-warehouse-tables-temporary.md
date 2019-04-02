---
title: SQL Data warehouse'daki geçici tabloları | Microsoft Docs
description: Geçici tablolar ve oturum düzeyi geçici tablolar sürecin prensiplerini vurgular kullanarak temel Kılavuzu.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/01/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 23a62e28700ad5fd733040c43ea0eec225fd286f
ms.sourcegitcommit: ad3e63af10cd2b24bf4ebb9cc630b998290af467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58793110"
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>SQL veri ambarı'nda geçici tablolar
Bu makalede, geçici tabloları kullanmaya yönelik temel rehberlik içerir ve oturum düzeyi geçici tablolar sürecin prensiplerini vurgular. Bu makalede verilen bilgileri kullanarak, hem çalışmalarında kodunuzun bakım kolaylığı geliştirip kodunuzu modülerleştirmek yardımcı olabilir.

## <a name="what-are-temporary-tables"></a>Geçici tablolar nelerdir?
Geçici tablolar, özellikle Ara sonuçlar geçici olduğu dönüştürme sırasında veri - işleme sırasında yararlıdır. SQL veri ambarı'nda geçici tablolar oturum düzeyinde mevcuttur.  Yalnızca, bunlar oluşturulmuş ve bu oturumu kapattığında otomatik olarak bırakılır oturum görünür.  Uzak Depolama yerine yerel sonuçları yazıldığından geçici tablolar bir performans kazancı sağlar.

## <a name="create-a-temporary-table"></a>Geçici bir tablo oluşturma
Geçici tablolar tablo adınızla önek tarafından oluşturulan bir `#`.  Örneğin:

```sql
CREATE TABLE #stats_ddl
(
    [schema_name]        NVARCHAR(128) NOT NULL
,    [table_name]            NVARCHAR(128) NOT NULL
,    [stats_name]            NVARCHAR(128) NOT NULL
,    [stats_is_filtered]     BIT           NOT NULL
,    [seq_nmbr]              BIGINT        NOT NULL
,    [two_part_name]         NVARCHAR(260) NOT NULL
,    [three_part_name]       NVARCHAR(400) NOT NULL
)
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
```

Geçici tablolar da oluşturulabilir bir `CTAS` tam olarak aynı yaklaşımı kullanarak:

```sql
CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
,    HEAP
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
``` 

> [!NOTE]
> `CTAS` güçlü bir komutu ve işlem günlüğü alanının kullanımını etkili olma fayda vardır. 
> 
> 

## <a name="dropping-temporary-tables"></a>Geçici tablolar bırakılıyor
Yeni bir oturum oluşturulduğunda, geçici tablo var olmalıdır.  Ancak, emin olmak için aynı ada sahip bir geçici oluşturur aynı saklı yordam çağırma varsa, `CREATE TABLE` deyimleri başarılı bir basit öncesi varlığı denetimi ile bir `DROP` aşağıdaki örnekte olduğu gibi kullanılabilir:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Tutarlılık kodlama için tablo ve geçici tablolar için bu düzeni kullanmak için bir alışkanlıktır.  Ayrıca kullanmak iyi bir fikir olduğunu `DROP TABLE` bunları kodunuzda tamamladıktan sonra geçici tabloları kaldırabiliriz.  Saklı yordam geliştirme drop komutu bu nesneler temizlendiğinden emin olmak için bir yordam sonunda birlikte paketlenebilir görmek için yaygındır.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Kod modularizing
Geçici tablolar kullanıcı oturumunda herhangi bir yansıdığını görebiliriz olduğundan, bu uygulama kodunuz modülarize etmek amacıyla yararlanılabilir.  Örneğin, aşağıdaki depolanan yordamı istatistik adı veritabanındaki tüm istatistiklerin güncelleştirilmesi DDL oluşturur.

```sql
CREATE PROCEDURE    [dbo].[prc_sqldw_update_stats]
(   @update_type    tinyint -- 1 default 2 fullscan 3 sample 4 resample
    ,@sample_pct     tinyint
)
AS

IF @update_type NOT IN (1,2,3,4)
BEGIN;
    THROW 151000,'Invalid value for @update_type parameter. Valid range 1 (default), 2 (fullscan), 3 (sample) or 4 (resample).',1;
END;

IF @sample_pct IS NULL
BEGIN;
    SET @sample_pct = 20;
END;

IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END

CREATE TABLE #stats_ddl
WITH
(
    DISTRIBUTION = HASH([seq_nmbr])
)
AS
(
SELECT
        sm.[name]                                                                AS [schema_name]
,        tb.[name]                                                                AS [table_name]
,        st.[name]                                                                AS [stats_name]
,        st.[has_filter]                                                            AS [stats_is_filtered]
,       ROW_NUMBER()
        OVER(ORDER BY (SELECT NULL))                                            AS [seq_nmbr]
,                                 QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [two_part_name]
,        QUOTENAME(DB_NAME())+'.'+QUOTENAME(sm.[name])+'.'+QUOTENAME(tb.[name])  AS [three_part_name]
FROM    sys.objects            AS ob
JOIN    sys.stats            AS st    ON    ob.[object_id]        = st.[object_id]
JOIN    sys.stats_columns    AS sc    ON    st.[stats_id]        = sc.[stats_id]
                                    AND st.[object_id]        = sc.[object_id]
JOIN    sys.columns            AS co    ON    sc.[column_id]        = co.[column_id]
                                    AND    sc.[object_id]        = co.[object_id]
JOIN    sys.tables            AS tb    ON    co.[object_id]        = tb.[object_id]
JOIN    sys.schemas            AS sm    ON    tb.[schema_id]        = sm.[schema_id]
WHERE    1=1
AND        st.[user_created]   = 1
GROUP BY
        sm.[name]
,        tb.[name]
,        st.[name]
,        st.[filter_definition]
,        st.[has_filter]
)
SELECT
    CASE @update_type
    WHEN 1
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+');'
    WHEN 2
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH FULLSCAN;'
    WHEN 3
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH SAMPLE '+CAST(@sample_pct AS VARCHAR(20))+' PERCENT;'
    WHEN 4
    THEN 'UPDATE STATISTICS '+[two_part_name]+'('+[stats_name]+') WITH RESAMPLE;'
    END AS [update_stats_ddl]
,   [seq_nmbr]
FROM    t1
;
GO
```

Bu aşamada, gerçekleşen yalnızca geçici bir tablo DDL deyimleri ile #stats_ddl oluşturan bir saklı yordam oluşturma eylemdir.  Bir oturumda birden çok kez çalıştırırsanız başlayabildiğinden emin olmak için zaten varsa bu saklı yordamı #stats_ddl bırakır.  Ancak, olduğundan hiçbir `DROP TABLE` saklı yordam tamamlandığında, saklı yordam dışında okuyun saklı yordamın sonunda, oluşturulmuş bir tabloyu bırakır, böylece.  SQL veri ambarı'nda, diğer SQL Server veritabanlarının, oluşturulduğu yordamı dışında geçici tablo kullanmak mümkündür.  SQL veri ambarı geçici tablolar kullanılabilir **herhangi bir yere** oturumu içinde. Bu aşağıdaki örnekte olduğu gibi daha fazla modüler ve yönetilebilir kod neden olabilir:

```sql
EXEC [dbo].[prc_sqldw_update_stats] @update_type = 1, @sample_pct = NULL;

DECLARE @i INT              = 1
,       @t INT              = (SELECT COUNT(*) FROM #stats_ddl)
,       @s NVARCHAR(4000)   = N''

WHILE @i <= @t
BEGIN
    SET @s=(SELECT update_stats_ddl FROM #stats_ddl WHERE seq_nmbr = @i);

    PRINT @s
    EXEC sp_executesql @s
    SET @i+=1;
END

DROP TABLE #stats_ddl;
```

## <a name="temporary-table-limitations"></a>Geçici Tablo sınırlamaları
SQL veri ambarı, geçici tablolar uygularken birkaç sınırlama uygulamaktadır.  Şu anda, yalnızca oturum kapsamlı geçici tablolar desteklenir.  Genel geçici tablolar desteklenmez.  Ayrıca, geçici tablolarda görünümlere oluşturulamıyor.  Geçici tablolar ile karma veya hepsini bir kez deneme dağıtım yalnızca oluşturulabilir.  Yinelenen geçici tablo dağıtımı desteklenmiyor. 

## <a name="next-steps"></a>Sonraki adımlar
Tablo geliştirme hakkında daha fazla bilgi için bkz. [tabloya genel bakış](sql-data-warehouse-tables-overview.md).

