---
title: "SQL veri ambarı geçici tablolarda | Microsoft Docs"
description: "Geçici tablolarda Azure SQL Data Warehouse ile çalışmaya başlama."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jenniehubbard
editor: 
ms.assetid: 9b1119eb-7f54-46d0-ad74-19c85a2a555a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 12/06/2017
ms.author: barbkess
ms.openlocfilehash: e3b2f9017ecea7d9f78c07476f96c3dd8d031863
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="temporary-tables-in-sql-data-warehouse"></a>SQL veri ambarı geçici tabloları
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

Geçici tablolara verileri - özellikle Ara sonuçların geçici nerede dönüştürme sırasında işlerken faydalıdır. SQL veri ambarı'nda geçici tablolara oturum düzeyindedir.  Bunlar yalnızca, bunlar oluşturuldu ve bu oturumu kapattığında otomatik olarak bırakılan oturum görünür olur.  Uzaktaki Depolama birimi yerine yerel sonuçları yazıldığından geçici tablolara performans avantajı sunar.  Bunlar her yerden ve saklı yordam haricindeki dahil olmak üzere oturumu içinde erişilebilir olarak geçici tabloları Azure SQL veritabanından Azure SQL Data Warehouse'da biraz farklı.

Bu makalede geçici tabloları kullanmak için temel kılavuz içerir ve oturum düzeyi geçici tablolara ilkeleri vurgular. Bu makaledeki bilgileri kullanarak yeniden kullanılırlığı ve kodunuzun bakım kolaylığı artırma kodunuzu modülarize etmek yardımcı olabilir.

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
> `CTAS`güçlü bir komut ve işlem günlüğü alanının kullanımını etkili olma ek avantajına sahiptir. 
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
Daha fazla bilgi edinmek için üzerinde makalelerine bakın [tablo genel bakışı][Overview], [tablo veri türleri][Data Types], [bir tablodağıtma] [ Distribute], [Tablo dizin][Index], [bir tablo bölümleme] [ Partition] ve [Tablo istatistikleri koruma][Statistics].  En iyi uygulamalar hakkında daha fazla bilgi için bkz: [SQL veri ambarı en iyi uygulamalar][SQL Data Warehouse Best Practices].

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

<!--MSDN references-->

<!--Other Web references-->
