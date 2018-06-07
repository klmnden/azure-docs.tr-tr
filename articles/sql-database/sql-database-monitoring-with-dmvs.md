---
title: Azure SQL veritabanını dinamik yönetim görünümlerini kullanarak izleme | Microsoft Docs
description: Algılamak ve Microsoft Azure SQL veritabanı izlemek için dinamik yönetim görünümlerini kullanarak genel performans sorunlarını tanılamak öğrenin.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: carlrab
ms.openlocfilehash: a1333680225923a4e27f96e61a5b6530f32a9329
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34647893"
---
# <a name="monitoring-azure-sql-database-using-dynamic-management-views"></a>Dinamik yönetim görünümlerini kullanarak Azure SQL Database’i izleme
Microsoft Azure SQL veritabanı bir alt kümesini engellenen veya uzun süre çalışan sorgular, kaynak darboğazları, zayıf sorgu planları ve benzeri tarafından kaynaklanabilir performans sorunları tanılamak için dinamik yönetim görünümlerini sağlar. Bu konu, dinamik yönetim görünümlerini kullanarak genel performans sorunlarını algılamak hakkında bilgi sağlar.

SQL veritabanı, kısmen dinamik yönetim görünümlerini üç kategoride destekler:

* Veritabanı ile ilgili dinamik yönetim görünümlerini.
* Yürütme ilgili dinamik yönetim görünümlerini.
* İşlem ilgili dinamik yönetim görünümlerini.

Dinamik Yönetim görünümlerini hakkında ayrıntılı bilgi için bkz: [dinamik yönetim görünümlerini ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) SQL Server Books Online.

## <a name="permissions"></a>İzinler
SQL veritabanı'nda dinamik yönetim görünümünü sorgulanmasını gerektirir **görünüm veritabanı durumu** izinleri. **Görünüm veritabanı durumu** izin geçerli veritabanının içindeki tüm nesneler hakkında bilgi döndürür.
Vermek için **görünüm veritabanı durumu** belirli veritabanı kullanıcı izni aşağıdaki sorguyu çalıştırın:

```GRANT VIEW DATABASE STATE TO database_user; ```

Bir şirket içi SQL Server örneğinde dinamik yönetim görünümlerini sunucu durumu bilgilerini döndürür. SQL veritabanı'nda, bunlar yalnızca geçerli, mantıksal veritabanı ile ilgili bilgiler döndürür.

## <a name="calculating-database-size"></a>Veritabanı boyutu hesaplama
Aşağıdaki sorguda (megabayt cinsinden) veritabanının boyutunu döndürür:

```
-- Calculates the size of the database.
SELECT SUM(reserved_page_count)*8.0/1024
FROM sys.dm_db_partition_stats;
GO
```

Aşağıdaki sorguda (megabayt cinsinden) ayrı ayrı nesneleri boyutunu veritabanınızda döndürür:

```
-- Calculates the size of individual database objects.
SELECT sys.objects.name, SUM(reserved_page_count) * 8.0 / 1024
FROM sys.dm_db_partition_stats, sys.objects
WHERE sys.dm_db_partition_stats.object_id = sys.objects.object_id
GROUP BY sys.objects.name;
GO
```

## <a name="monitoring-connections"></a>İzleme bağlantıları
Kullanabileceğiniz [sys.dm_exec_connections](https://msdn.microsoft.com/library/ms181509.aspx) belirli bir Azure SQL veritabanı sunucusunu ve her bağlantı ayrıntıları kurulan bağlantılar hakkında bilgi almak için görünümü. Ayrıca, [sys.dm_exec_sessions](https://msdn.microsoft.com/library/ms176013.aspx) görünümü, tüm etkin kullanıcı bağlantıları ve iç görevler hakkında bilgi alırken yararlıdır.
Aşağıdaki sorgu, geçerli bağlantı üzerine bilgi alır:

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
> Yürütülürken **sys.dm_exec_requests** ve **sys.dm_exec_sessions görünümleri**, varsa **görünüm veritabanı durumu** izin veritabanında, tüm yürütme görürsünüz. Veritabanı oturumlarını; Aksi takdirde, yalnızca geçerli oturumu bakın.
> 
> 

## <a name="monitoring-query-performance"></a>Sorgu performansını izleme
Yavaş ya da uzun çalışan sorgu önemli sistem kaynaklarını tüketebilir. Bu bölüm, dinamik yönetim görünümlerini birkaç ortak sorgu performans sorunlarını algılamak için nasıl kullanılacağını gösterir. Sorun giderme, eski ancak hala yararlı bir başvuru [SQL Server 2008'de performans sorunlarını giderme](http://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Microsoft TechNet makalesi.

### <a name="finding-top-n-queries"></a>İlk N sorguları bulma
Aşağıdaki örnek, ortalama CPU süresi tarafından derece üst beş sorguları hakkında bilgi döndürür. Mantıksal olarak eşdeğer sorguları kendi toplu kaynak tüketimi gruplanması Bu örnek sorgu karma değerlerine göre sorgular toplar.

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
Yavaş veya uzun süre çalışan sorgular, aşırı kaynak tüketimi katkıda ve engellenen sorgu sonucu olabilir. Engelleme nedeni, zayıf uygulama tasarımı, hatalı sorgu planları, kullanışlı dizinler ve benzeri olmaması olabilir. Azure SQL veritabanınızda geçerli kilitleme etkinliği hakkında bilgi almak için sys.dm_tran_locks görünümü kullanabilirsiniz. Örnek kod, bkz: [sys.dm_tran_locks (Transact-SQL)](https://msdn.microsoft.com/library/ms190345.aspx) SQL Server Books Online.

### <a name="monitoring-query-plans"></a>Sorgu planları izleme
Verimsiz sorgu planı da CPU tüketimi artırabilir. Aşağıdaki örnek kullanır [sys.dm_exec_query_stats](https://msdn.microsoft.com/library/ms189741.aspx) hangi sorgusu en toplam CPU kullanan belirlemek için Görünüm.

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
[SQL veritabanı giriş](sql-database-technical-overview.md)

