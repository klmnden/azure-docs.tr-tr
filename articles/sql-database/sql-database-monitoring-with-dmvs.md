---
title: Dinamik Yönetim görünümlerini kullanarak Azure SQL veritabanı izleme | Microsoft Docs
description: Algılama ve dinamik yönetim görünümlerini kullanarak Microsoft Azure SQL veritabanı izlemek için genel performans sorunlarını tanılayın öğrenin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 08/08/2018
ms.author: carlrab
ms.openlocfilehash: c4d1170bd2fe4acb135c88191b447f734e312723
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/08/2018
ms.locfileid: "39715966"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Dinamik yönetim görünümlerini kullanarak Azure SQL Database’i izleme
Microsoft Azure SQL veritabanı, engellenen veya uzun süre çalışan sorguları, kaynak darboğazları, zayıf sorgu planlarına ve benzeri tarafından kaynaklanabilir performans sorunları tanılamak için dinamik yönetim görünümlerini kümesini sağlar. Bu konuda, dinamik yönetim görünümlerini kullanarak sık karşılaşılan performans sorunlarını algılamak nasıl hakkında bilgiler sağlar.

SQL veritabanı, kısmen dinamik yönetim görünümlerini üç kategoriye destekler:

* Veritabanı ile ilgili dinamik yönetimi görünümleri.
* Yürütme ile ilgili dinamik yönetimi görünümleri.
* İşlemle ilgili dinamik yönetimi görünümleri.

Dinamik Yönetim görünümlerini hakkında ayrıntılı bilgi için bkz. [dinamik yönetim görünümleri ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) SQL Server Books Online.

## <a name="permissions"></a>İzinler
SQL veritabanı'nda dinamik yönetim görünümünü sorgulama gerektiren **VIEW DATABASE STATE** izinleri. **VIEW DATABASE STATE** izin geçerli veritabanı içindeki tüm nesneler hakkında bilgi döndürür.
Vermek **VIEW DATABASE STATE** izin, belirli bir veritabanı kullanıcısı da aşağıdaki sorguyu çalıştırın:

```GRANT VIEW DATABASE STATE TO database_user; ```

Şirket içi SQL Server örneğinde, dinamik yönetim görünümlerini sunucu durumu bilgilerini döndürür. SQL veritabanı'nda, bunlar, yalnızca geçerli mantıksal veritabanı ile ilgili bilgi döndürür.

## <a name="calculating-database-size"></a>Veritabanı boyutu hesaplanıyor
Aşağıdaki sorgu veritabanınızın (megabayt cinsinden) boyutunu döndürür:

```
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

Aşağıdaki sorgu, veritabanınızda (megabayt cinsinden) tek tek nesnelerin boyutunu döndürür:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>Bağlantı izleme
Kullanabileceğiniz [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) belirli bir Azure SQL veritabanı sunucusu ve her bağlantı ayrıntıları için kurulan bağlantılar hakkında bilgi almak için görünümü. Ayrıca, [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) görünümü, tüm etkin kullanıcı bağlantıları ve iç görevler hakkında bilgi alırken yararlıdır.
Aşağıdaki sorgu, geçerli bağlantı hakkında bilgi alır:

```
SELECT
    c.session_id, c.net_transport, c.encrypt_option,
    c.auth_scheme, s.host_name, s.program_name,
    s.client_interface_name, s.login_name, s.nt_domain,
    s.nt_user_name, s.original_login_name, c.connect_time,
    s.login_time
FROM sys.dm_exec_connections AS c
JOIN sys.dm_exec_sessions AS s
    ON c.session_id = s.session_id
WHERE c.session_id = @@SPID;
```

> [!NOTE]
> Yürütülürken **sys.dm_exec_requests** ve **sys.dm_exec_sessions görünümleri**, varsa **VIEW DATABASE STATE** izni veritabanında, tüm çalıştırma görürsünüz. oturumlarının veritabanında; Aksi takdirde, yalnızca mevcut oturum bakın.
> 
> 

## <a name="monitoring-query-performance"></a>Sorgu performansını izleme
Yavaş veya uzun süre çalışan sorguların önemli sistem kaynakları kullanabilir. Bu bölümde, birkaç ortak sorgu performansı sorunlarını algılamak için dinamik yönetim görünümlerini nasıl yapılacağı açıklanır. Sorun giderme, eski ancak yine de yararlı bir başvuru [SQL Server 2008'de performans sorunlarını giderme](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Microsoft TechNet makalesi.

### <a name="finding-top-n-queries"></a>İlk N sorgularını bulma
Aşağıdaki örnek, ortalama CPU süresine göre sıralanmış ilk beş sorgu hakkındaki bilgileri döndürür. Mantıksal eşdeğer sorgular tarafından toplam kaynak tüketimi gruplanması Bu örnekte sorgu, sorgu karmasına göre toplar.

```
SELECT TOP 5 query_stats.query_hash AS "Query Hash",
    SUM(query_stats.total_worker_time) / SUM(query_stats.execution_count) AS "Avg CPU Time",
    MIN(query_stats.statement_text) AS "Statement Text"
FROM
    (SELECT QS.*,
    SUBSTRING(ST.text, (QS.statement_start_offset/2) + 1,
    ((CASE statement_end_offset
        WHEN -1 THEN DATALENGTH(ST.text)
        ELSE QS.statement_end_offset END
            - QS.statement_start_offset)/2) + 1) AS statement_text
     FROM sys.dm_exec_query_stats AS QS
     CROSS APPLY sys.dm_exec_sql_text(QS.sql_handle) as ST) as query_stats
GROUP BY query_stats.query_hash
ORDER BY 2 DESC;
```

### <a name="monitoring-blocked-queries"></a>Engellenen sorguları izleme
Yavaş veya uzun süre çalışan sorgular, aşırı kaynak tüketimi için katkıda bulunan ve engellenen sorguların sonucu olabilir. Engelleme nedeni, zayıf uygulama tasarımı, hatalı sorgu planlarına, kullanışlı dizinleri ve benzeri olmaması olabilir. Azure SQL veritabanı'nda geçerli kilitleme etkinliği hakkında bilgi almak için sys.dm_tran_locks görünümü kullanabilirsiniz. Örnek kod için bkz: [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Sorgu planlarına izleme
Bir verimsiz bir sorgu planı, CPU tüketimi da artırabilir. Aşağıdaki örnekte [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) hangi sorgu toplu en çok CPU kullanan belirlemek için görüntüleme.

```
SELECT
    highest_cpu_queries.plan_handle,
    highest_cpu_queries.total_worker_time,
    q.dbid,
    q.objectid,
    q.number,
    q.encrypted,
    q.[text]
FROM
    (SELECT TOP 50
        qs.plan_handle,
        qs.total_worker_time
    FROM
        sys.dm_exec_query_stats qs
    ORDER BY qs.total_worker_time desc) AS highest_cpu_queries
    CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS q
ORDER BY highest_cpu_queries.total_worker_time DESC;
```

## <a name="see-also"></a>Ayrıca bkz.
[SQL Database'e giriş](sql-database-technical-overview.md)

