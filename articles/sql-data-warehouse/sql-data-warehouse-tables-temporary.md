---
title: SQL veri ambarı geçici tablolarda | Microsoft Docs
description: Geçici tablolara ve vurgular oturum düzeyi geçici tablolara ilkeleri kullanmaya yönelik temel kılavuz.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: a3e06a4074bc7b5cd8612162a624718107a50656
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>SQL veri ambarı geçici tabloları
Bu makalede geçici tabloları kullanmak için temel kılavuz içerir ve oturum düzeyi geçici tablolara ilkeleri vurgular. Bu makaledeki bilgileri kullanarak yeniden kullanılırlığı ve kodunuzun bakım kolaylığı artırma kodunuzu modülarize etmek yardımcı olabilir.

## <a name="what-are-temporary-tables"></a>Geçici tablolara nelerdir?
Geçici tablolara verileri - özellikle Ara sonuçların geçici nerede dönüştürme sırasında işlerken faydalıdır. SQL veri ambarı'nda geçici tablolara oturum düzeyindedir.  Bunlar yalnızca, bunlar oluşturuldu ve bu oturumu kapattığında otomatik olarak bırakılan oturum görünür olur.  Uzaktaki Depolama birimi yerine yerel sonuçları yazıldığından geçici tablolara performans avantajı sunar.  Bunlar her yerden ve saklı yordam haricindeki dahil olmak üzere oturumu içinde erişilebilir olarak geçici tabloları Azure SQL veritabanından Azure SQL Data Warehouse'da biraz farklı.

## <a name="create-a-temporary-table"></a>Geçici bir tablo oluştur
Geçici tablolara tablo adıyla önek tarafından oluşturulan bir `#`.  Örneğin:

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

Geçici tablolara de oluşturulabilir ile bir `CTAS` tam olarak aynı yaklaşımı kullanarak:

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
> `CTAS` güçlü bir komut ve işlem günlüğü alanının kullanımını etkili olma ek avantajına sahiptir. 
> 
> 

## <a name="dropping-temporary-tables"></a>Geçici tablolara bırakılıyor
Yeni bir oturum oluşturulduğunda, hiçbir geçici tablolar var olmalıdır.  Ancak, emin olmak için aynı ada sahip bir geçici oluşturur aynı saklı yordamı çağırma varsa, `CREATE TABLE` deyimleri başarılı bir basit öncesi varlığı denetimi ile bir `DROP` aşağıdaki örnekteki kullanılabilir:

```sql
IF OBJECT_ID('tempdb..#stats_ddl') IS NOT NULL
BEGIN
    DROP TABLE #stats_ddl
END
```

Tutarlılık kodlama için tablo ve geçici tablolar için bu deseni kullanmak için iyi bir uygulama olur.  Bu aynı zamanda kullanmak için iyi bir fikirdir `DROP TABLE` bunlarla kodunuzda tamamladığınızda geçici tabloları kaldırmak için.  Saklı yordam geliştirme drop komutu bu nesneler Temizlenen emin olmak için bir yordam sonunda birlikte gruplanır görmek için yaygın bir sorundur.

```sql
DROP TABLE #stats_ddl
```

## <a name="modularizing-code"></a>Modularizing kodu
Geçici tablolara herhangi bir kullanıcı oturum görülebilir olduğundan, bu uygulama kodunuz modülarize etmek amacıyla yararlanılabilir.  Örneğin, aşağıdaki depolanan yordamı istatistiği adıyla veritabanındaki tüm İstatistikleri güncelleştirmek için DDL oluşturur.

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

Bu aşamada, gerçekleşen tek bir saklı yordam oluşturulmasını eylemdir bu generatess geçici bir tablo DDL deyimleri ile #stats_ddl.  Oturum içindeki birden çok kez çalıştırırsanız başlayabildiğinden emin olmak için zaten varsa bu saklı yordam #stats_ddl bırakır.  Ancak, olduğundan hiçbir `DROP TABLE` saklı yordam tamamlandığında, saklı yordam dışında okunabilir saklı yordamın sonunda, oluşturulmuş bir tabloyu bırakır, böylece.  SQL veri ambarı'nda diğer SQL Server veritabanlarını geçici tablo oluşturulduğu yordamı dışında kullanmak mümkündür.  SQL veri ambarı geçici tablolar kullanılabilir **herhangi bir yere** oturumu içinde. Bu aşağıdaki örnekte olduğu gibi daha fazla modüler ve yönetilebilir kodu neden olabilir:

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
SQL veri ambarı birkaç sınırlama geçici tablolara uygularken zorunlu tuttukları.  Şu anda yalnızca oturum kapsamlı geçici tablolar desteklenir.  Genel geçici tablolar desteklenmez.  Ayrıca, geçici tablolarda görünümlere oluşturulamıyor.

## <a name="next-steps"></a>Sonraki adımlar
Tabloları geliştirme hakkında daha fazla bilgi için bkz: [tablo genel bakışı](sql-data-warehouse-tables-overview.md).

