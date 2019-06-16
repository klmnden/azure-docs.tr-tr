---
title: Azure SQL veritabanı Dmv'leri kullanarak performans izleme | Microsoft Docs
description: Algılama ve dinamik yönetim görünümlerini kullanarak Microsoft Azure SQL veritabanı izlemek için genel performans sorunlarını tanılayın öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: juliemsft
ms.author: jrasnick
ms.reviewer: carlrab
manager: craigg
ms.date: 12/19/2018
ms.openlocfilehash: 371632a28d22583f8b206e4d8b9d2b6b4e510ab0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62103772"
---
# <a name="monitoring-performance-azure-sql-database-using-dynamic-management-views"></a>Azure SQL veritabanı performansı izleme dinamik yönetim görünümlerini kullanarak

Microsoft Azure SQL veritabanı, engellenen veya uzun süre çalışan sorguları, kaynak darboğazları, zayıf sorgu planlarına ve benzeri tarafından kaynaklanabilir performans sorunları tanılamak için dinamik yönetim görünümlerini kümesini sağlar. Bu konuda, dinamik yönetim görünümlerini kullanarak sık karşılaşılan performans sorunlarını algılamak nasıl hakkında bilgiler sağlar.

SQL veritabanı, kısmen dinamik yönetim görünümlerini üç kategoriye destekler:

- Veritabanı ile ilgili dinamik yönetimi görünümleri.
- Yürütme ile ilgili dinamik yönetimi görünümleri.
- İşlemle ilgili dinamik yönetimi görünümleri.

Dinamik Yönetim görünümlerini hakkında ayrıntılı bilgi için bkz. [dinamik yönetim görünümleri ve işlevleri (Transact-SQL)](https://msdn.microsoft.com/library/ms188754.aspx) SQL Server Books Online.

## <a name="permissions"></a>İzinler

SQL veritabanı'nda dinamik yönetim görünümünü sorgulama gerektiren **VIEW DATABASE STATE** izinleri. **VIEW DATABASE STATE** izin geçerli veritabanı içindeki tüm nesneler hakkında bilgi döndürür.
Vermek **VIEW DATABASE STATE** izin, belirli bir veritabanı kullanıcısı da aşağıdaki sorguyu çalıştırın:

```sql
GRANT VIEW DATABASE STATE TO database_user;
```

Şirket içi SQL Server örneğinde, dinamik yönetim görünümlerini sunucu durumu bilgilerini döndürür. SQL veritabanı'nda, bunlar, yalnızca geçerli mantıksal veritabanı ile ilgili bilgi döndürür.

## <a name="identify-cpu-performance-issues"></a>CPU performans sorunlarını belirleme

CPU kullanımı % 80'in uzun süre için aşağıdaki sorun giderme adımları göz önünde bulundurun:

### <a name="the-cpu-issue-is-occurring-now"></a>CPU sorunu şimdi gerçekleşiyor

Sorun şu anda meydana geliyorsa, iki olası senaryo vardır:

#### <a name="many-individual-queries-that-cumulatively-consume-high-cpu"></a>Üst üste yüksek CPU kullanan çok sayıda tek sorgu

Top sorgusu karmaları tanımlamak için aşağıdaki sorguyu kullanın:

```sql
PRINT '-- top 10 Active CPU Consuming Queries (aggregated)--';
SELECT TOP 10 GETDATE() runtime, *
FROM(SELECT query_stats.query_hash, SUM(query_stats.cpu_time) 'Total_Request_Cpu_Time_Ms', SUM(logical_reads) 'Total_Request_Logical_Reads', MIN(start_time) 'Earliest_Request_start_Time', COUNT(*) 'Number_Of_Requests', SUBSTRING(REPLACE(REPLACE(MIN(query_stats.statement_text), CHAR(10), ' '), CHAR(13), ' '), 1, 256) AS "Statement_Text"
     FROM(SELECT req.*, SUBSTRING(ST.text, (req.statement_start_offset / 2)+1, ((CASE statement_end_offset WHEN -1 THEN DATALENGTH(ST.text)ELSE req.statement_end_offset END-req.statement_start_offset)/ 2)+1) AS statement_text
          FROM sys.dm_exec_requests AS req
               CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) AS ST ) AS query_stats
     GROUP BY query_hash) AS t
ORDER BY Total_Request_Cpu_Time_Ms DESC;
```

#### <a name="long-running-queries-that-consume-cpu-are-still-running"></a>CPU kullanan uzun süren sorgular hala çalışıyor

Bu sorguları tanımlamak için aşağıdaki sorguyu kullanın:

```sql
PRINT '--top 10 Active CPU Consuming Queries by sessions--';
SELECT TOP 10 req.session_id, req.start_time, cpu_time 'cpu_time_ms', OBJECT_NAME(ST.objectid, ST.dbid) 'ObjectName', SUBSTRING(REPLACE(REPLACE(SUBSTRING(ST.text, (req.statement_start_offset / 2)+1, ((CASE statement_end_offset WHEN -1 THEN DATALENGTH(ST.text)ELSE req.statement_end_offset END-req.statement_start_offset)/ 2)+1), CHAR(10), ' '), CHAR(13), ' '), 1, 512) AS statement_text
FROM sys.dm_exec_requests AS req
     CROSS APPLY sys.dm_exec_sql_text(req.sql_handle) AS ST
ORDER BY cpu_time DESC;
GO
```

### <a name="the-cpu-issue-occurred-in-the-past"></a>Geçmişte CPU sorun oluştu

Geçmişte sorun oluştu ve kök neden çözümlemesi, kullanın [Query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store). Veritabanı erişimi olan kullanıcılar, Query Store verileri sorgulamak için T-SQL kullanabilirsiniz.  Query Store varsayılan yapılandırmaları 1 saatlik bir ayrıntı düzeyi kullanın.  Bir etkinlik için yüksek CPU kullanan sorguları aramak için aşağıdaki sorguyu kullanın. Bu sorgu, en çok 15 CPU kullanan sorguları döndürür.  Değiştirmeyi unutmayın `rsi.start_time >= DATEADD(hour, -2, GETUTCDATE()`:

```sql
-- Top 15 CPU consuming queries by query hash
-- note that a query  hash can have many query id if not parameterized or not parameterized properly
-- it grabs a sample query text by min
WITH AggregatedCPU AS (SELECT q.query_hash, SUM(count_executions * avg_cpu_time / 1000.0) AS total_cpu_millisec, SUM(count_executions * avg_cpu_time / 1000.0)/ SUM(count_executions) AS avg_cpu_millisec, MAX(rs.max_cpu_time / 1000.00) AS max_cpu_millisec, MAX(max_logical_io_reads) max_logical_reads, COUNT(DISTINCT p.plan_id) AS number_of_distinct_plans, COUNT(DISTINCT p.query_id) AS number_of_distinct_query_ids, SUM(CASE WHEN rs.execution_type_desc='Aborted' THEN count_executions ELSE 0 END) AS Aborted_Execution_Count, SUM(CASE WHEN rs.execution_type_desc='Regular' THEN count_executions ELSE 0 END) AS Regular_Execution_Count, SUM(CASE WHEN rs.execution_type_desc='Exception' THEN count_executions ELSE 0 END) AS Exception_Execution_Count, SUM(count_executions) AS total_executions, MIN(qt.query_sql_text) AS sampled_query_text
                       FROM sys.query_store_query_text AS qt
                            JOIN sys.query_store_query AS q ON qt.query_text_id=q.query_text_id
                            JOIN sys.query_store_plan AS p ON q.query_id=p.query_id
                            JOIN sys.query_store_runtime_stats AS rs ON rs.plan_id=p.plan_id
                            JOIN sys.query_store_runtime_stats_interval AS rsi ON rsi.runtime_stats_interval_id=rs.runtime_stats_interval_id
                       WHERE rs.execution_type_desc IN ('Regular', 'Aborted', 'Exception')AND rsi.start_time>=DATEADD(HOUR, -2, GETUTCDATE())
                       GROUP BY q.query_hash), OrderedCPU AS (SELECT query_hash, total_cpu_millisec, avg_cpu_millisec, max_cpu_millisec, max_logical_reads, number_of_distinct_plans, number_of_distinct_query_ids, total_executions, Aborted_Execution_Count, Regular_Execution_Count, Exception_Execution_Count, sampled_query_text, ROW_NUMBER() OVER (ORDER BY total_cpu_millisec DESC, query_hash ASC) AS RN
                                                              FROM AggregatedCPU)
SELECT OD.query_hash, OD.total_cpu_millisec, OD.avg_cpu_millisec, OD.max_cpu_millisec, OD.max_logical_reads, OD.number_of_distinct_plans, OD.number_of_distinct_query_ids, OD.total_executions, OD.Aborted_Execution_Count, OD.Regular_Execution_Count, OD.Exception_Execution_Count, OD.sampled_query_text, OD.RN
FROM OrderedCPU AS OD
WHERE OD.RN<=15
ORDER BY total_cpu_millisec DESC;
```

Sorunlu sorguları tanımladıktan sonra bunu CPU kullanımını azaltmak için bu sorgularınızı ayarlamak zamanı geldi.  Sorgularınızı ayarlamak için zamanınız yoksa, sorunu çözmek için veritabanının SLO'ı yükseltmeyi seçebilirsiniz.

## <a name="identify-io-performance-issues"></a>G/ç performans sorunlarını belirleme

G/ç performans sorunlarını tanımlamak için kullanıldığında, g/ç sorunlarını ile ilişkili üst bekleme türleri şunlardır:

- `PAGEIOLATCH_*`

  Veri dosyası g/ç sorunları için (dahil olmak üzere `PAGEIOLATCH_SH`, `PAGEIOLATCH_EX`, `PAGEIOLATCH_UP`).  Bekleme tür adı varsa **GÇ** içinde GÇ soruna işaret. Yoksa hiçbir **GÇ** sayfa Mandal bekleme adı, onu farklı bir sorun (örneğin, tempdb Çekişme) türüne işaret.

- `WRITE_LOG`

  İşlem günlüğü g/ç sorunları için.

### <a name="if-the-io-issue-is-occurring-right-now"></a>Şu anda GÇ sorunu meydana geliyorsa

Kullanım [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) veya [sys.dm_os_waiting_tasks](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-waiting-tasks-transact-sql) görmek için `wait_type` ve `wait_time`.

#### <a name="identify-data-and-log-io-usage"></a>Verileri tanımlamak ve günlük GÇ kullanımını

Veri GÇ kullanımını oturum üzere aşağıdaki sorguyu kullanın. Veri veya günlük GÇ % 80'in ise, kullanıcılar, SQL veritabanı hizmet katmanı için kullanılabilir GÇ kullanmış anlamına gelir.

```sql
SELECT end_time, avg_data_io_percent, avg_log_write_percent
FROM sys.dm_db_resource_stats
ORDER BY end_time DESC;
```

GÇ sınırına ulaşıldı, iki seçeneğiniz vardır:

- 1\. seçenek: İşlem boyutunu yükseltebilir veya hizmet katmanını
- 2\. seçenek: Belirleyin ve çoğu g/ç kullanan sorguları ayarlayabilirsiniz.

#### <a name="view-buffer-related-io-using-the-query-store"></a>Query Store kullanarak görünüm arabellek ilgili GÇ

Seçenek 2 için aşağıdaki sorguyu arabellek ilgili g/ç için Query Store karşı son iki saat izlenen etkinliği görüntülemek için kullanabilirsiniz:

```sql
-- top queries that waited on buffer
-- note these are finished queries
WITH Aggregated AS (SELECT q.query_hash, SUM(total_query_wait_time_ms) total_wait_time_ms, SUM(total_query_wait_time_ms / avg_query_wait_time_ms) AS total_executions, MIN(qt.query_sql_text) AS sampled_query_text, MIN(wait_category_desc) AS wait_category_desc
                    FROM sys.query_store_query_text AS qt
                         JOIN sys.query_store_query AS q ON qt.query_text_id=q.query_text_id
                         JOIN sys.query_store_plan AS p ON q.query_id=p.query_id
                         JOIN sys.query_store_wait_stats AS waits ON waits.plan_id=p.plan_id
                         JOIN sys.query_store_runtime_stats_interval AS rsi ON rsi.runtime_stats_interval_id=waits.runtime_stats_interval_id
                    WHERE wait_category_desc='Buffer IO' AND rsi.start_time>=DATEADD(HOUR, -2, GETUTCDATE())
                    GROUP BY q.query_hash), Ordered AS (SELECT query_hash, total_executions, total_wait_time_ms, sampled_query_text, wait_category_desc, ROW_NUMBER() OVER (ORDER BY total_wait_time_ms DESC, query_hash ASC) AS RN
                                                        FROM Aggregated)
SELECT OD.query_hash, OD.total_executions, OD.total_wait_time_ms, OD.sampled_query_text, OD.wait_category_desc, OD.RN
FROM Ordered AS OD
WHERE OD.RN<=15
ORDER BY total_wait_time_ms DESC;
GO
```

#### <a name="view-total-log-io-for-writelog-waits"></a>Görünüm toplam günlük g/ç için WRITELOG bekler

Bekleme türü ise `WRITELOG`, toplam günlük GÇ deyimi tarafından görüntülemek için aşağıdaki sorguyu kullanın:

```sql
-- Top transaction log consumers
-- Adjust the time window by changing
-- rsi.start_time >= DATEADD(hour, -2, GETUTCDATE())
WITH AggregatedLogUsed
AS (SELECT q.query_hash,
           SUM(count_executions * avg_cpu_time / 1000.0) AS total_cpu_millisec,
           SUM(count_executions * avg_cpu_time / 1000.0) / SUM(count_executions) AS avg_cpu_millisec,
           SUM(count_executions * avg_log_bytes_used) AS total_log_bytes_used,
           MAX(rs.max_cpu_time / 1000.00) AS max_cpu_millisec,
           MAX(max_logical_io_reads) max_logical_reads,
           COUNT(DISTINCT p.plan_id) AS number_of_distinct_plans,
           COUNT(DISTINCT p.query_id) AS number_of_distinct_query_ids,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Aborted' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Aborted_Execution_Count,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Regular' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Regular_Execution_Count,
           SUM(   CASE
                      WHEN rs.execution_type_desc = 'Exception' THEN
                          count_executions
                      ELSE
                          0
                  END
              ) AS Exception_Execution_Count,
           SUM(count_executions) AS total_executions,
           MIN(qt.query_sql_text) AS sampled_query_text
    FROM sys.query_store_query_text AS qt
        JOIN sys.query_store_query AS q
            ON qt.query_text_id = q.query_text_id
        JOIN sys.query_store_plan AS p
            ON q.query_id = p.query_id
        JOIN sys.query_store_runtime_stats AS rs
            ON rs.plan_id = p.plan_id
        JOIN sys.query_store_runtime_stats_interval AS rsi
            ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
    WHERE rs.execution_type_desc IN ( 'Regular', 'Aborted', 'Exception' )
          AND rsi.start_time >= DATEADD(HOUR, -2, GETUTCDATE())
    GROUP BY q.query_hash),
     OrderedLogUsed
AS (SELECT query_hash,
           total_log_bytes_used,
           number_of_distinct_plans,
           number_of_distinct_query_ids,
           total_executions,
           Aborted_Execution_Count,
           Regular_Execution_Count,
           Exception_Execution_Count,
           sampled_query_text,
           ROW_NUMBER() OVER (ORDER BY total_log_bytes_used DESC, query_hash ASC) AS RN
    FROM AggregatedLogUsed)
SELECT OD.total_log_bytes_used,
       OD.number_of_distinct_plans,
       OD.number_of_distinct_query_ids,
       OD.total_executions,
       OD.Aborted_Execution_Count,
       OD.Regular_Execution_Count,
       OD.Exception_Execution_Count,
       OD.sampled_query_text,
       OD.RN
FROM OrderedLogUsed AS OD
WHERE OD.RN <= 15
ORDER BY total_log_bytes_used DESC;
GO
```

## <a name="identify-tempdb-performance-issues"></a>Tanımlamak `tempdb` performans sorunları

G/ç performans sorunlarını tanımlamak için kullanıldığında, üst bekleyin ilişkilendirilmiş türleri `tempdb` konular `PAGELATCH_*` (değil `PAGEIOLATCH_*`). Ancak, `PAGELATCH_*` beklediği değil her zaman anlama sahip olduğunuz `tempdb` Çekişme.  Bu bekleyin, ayrıca, aynı veri sayfasını hedefleyen eş zamanlı istek nedeniyle kullanıcı nesnesi veri sayfası çakışması olduğunu anlamına gelebilir. Daha fazla onaylamak için `tempdb` çekişmesini kullanım [sys.dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) wait_resource değeri ile başlayan onaylamak için `2:x:y` 2 olduğu `tempdb` veritabanı kimliği `x` dosya kimliği ve `y` sayfa kimliği.  

Tempdb çakışma için dayanan uygulama kodu yeniden yazın ya da azaltmak için ortak bir yöntemi olan `tempdb`.  Ortak `tempdb` kullanım alanları şunlardır:

- Geçici tablolar
- Tablo değişkenleri
- Tablo değerli Parametreler
- Sürüm deposu kullanım (özellikle ilişkili olan işlemler uzun süre çalışan)
- Sıralar, karma birleştirmeler ve biriktiricileri sorgu planlarına sahip sorgular

### <a name="top-queries-that-use-table-variables-and-temporary-tables"></a>Tablo değişkenleri ve geçici tablolar kullanan en sık kullanılan sorgular

Tablo değişkenleri ve geçici tablolar kullanan üst sorguları tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT plan_handle, execution_count, query_plan
INTO #tmpPlan
FROM sys.dm_exec_query_stats
     CROSS APPLY sys.dm_exec_query_plan(plan_handle);
GO

WITH XMLNAMESPACES('http://schemas.microsoft.com/sqlserver/2004/07/showplan' AS sp)
SELECT plan_handle, stmt.stmt_details.value('@Database', 'varchar(max)') 'Database', stmt.stmt_details.value('@Schema', 'varchar(max)') 'Schema', stmt.stmt_details.value('@Table', 'varchar(max)') 'table'
INTO #tmp2
FROM(SELECT CAST(query_plan AS XML) sqlplan, plan_handle FROM #tmpPlan) AS p
    CROSS APPLY sqlplan.nodes('//sp:Object') AS stmt(stmt_details);
GO

SELECT t.plan_handle, [Database], [Schema], [table], execution_count
FROM(SELECT DISTINCT plan_handle, [Database], [Schema], [table]
     FROM #tmp2
     WHERE [table] LIKE '%@%' OR [table] LIKE '%#%') AS t
    JOIN #tmpPlan AS t2 ON t.plan_handle=t2.plan_handle;
```

### <a name="identify-long-running-transactions"></a>Uzun süre çalışan işlemleri tanımlayın

Uzun tanımlamak için aşağıdaki sorguyu kullanın, işlem çalışıyor. Uzun süre çalışan işlem sürüm deposuna temizleme engelleyin.

```sql
SELECT DB_NAME(dtr.database_id) 'database_name',
       sess.session_id,
       atr.name AS 'tran_name',
       atr.transaction_id,
       transaction_type,
       transaction_begin_time,
       database_transaction_begin_time transaction_state,
       is_user_transaction,
       sess.open_transaction_count,
       LTRIM(RTRIM(REPLACE(
                              REPLACE(
                                         SUBSTRING(
                                                      SUBSTRING(
                                                                   txt.text,
                                                                   (req.statement_start_offset / 2) + 1,
                                                                   ((CASE req.statement_end_offset
                                                                         WHEN -1 THEN
                                                                             DATALENGTH(txt.text)
                                                                         ELSE
                                                                             req.statement_end_offset
                                                                     END - req.statement_start_offset
                                                                    ) / 2
                                                                   ) + 1
                                                               ),
                                                      1,
                                                      1000
                                                  ),
                                         CHAR(10),
                                         ' '
                                     ),
                              CHAR(13),
                              ' '
                          )
                  )
            ) Running_stmt_text,
       recenttxt.text 'MostRecentSQLText'
FROM sys.dm_tran_active_transactions AS atr
    INNER JOIN sys.dm_tran_database_transactions AS dtr
        ON dtr.transaction_id = atr.transaction_id
    LEFT JOIN sys.dm_tran_session_transactions AS sess
        ON sess.transaction_id = atr.transaction_id
    LEFT JOIN sys.dm_exec_requests AS req
        ON req.session_id = sess.session_id
           AND req.transaction_id = sess.transaction_id
    LEFT JOIN sys.dm_exec_connections AS conn
        ON sess.session_id = conn.session_id
    OUTER APPLY sys.dm_exec_sql_text(req.sql_handle) AS txt
    OUTER APPLY sys.dm_exec_sql_text(conn.most_recent_sql_handle) AS recenttxt
WHERE atr.transaction_type != 2
      AND sess.session_id != @@spid
ORDER BY start_time ASC;
```

## <a name="identify-memory-grant-wait-performance-issues"></a>GRANT bekleyin performans sorunlarını belleği tanımlayın

En üst türü bekleyin ise `RESOURCE_SEMAHPORE` ve bir yüksek CPU kullanımı sorunu yoksa, bekleme sorunu vermek bellek olabilir.

### <a name="determine-if-a-resourcesemahpore-wait-is-a-top-wait"></a>Olup bir `RESOURCE_SEMAHPORE` üst bekleme bekleyin

Belirlemek için aşağıdaki sorguyu kullanın. bir `RESOURCE_SEMAHPORE` üst bekleme bekleyin

```sql
SELECT wait_type,
       SUM(wait_time) AS total_wait_time_ms
FROM sys.dm_exec_requests AS req
    JOIN sys.dm_exec_sessions AS sess
        ON req.session_id = sess.session_id
WHERE is_user_process = 1
GROUP BY wait_type
ORDER BY SUM(wait_time) DESC;
```

### <a name="identity-high-memory-consuming-statements"></a>Kimlik yüksek bellek tüketen deyimleri

Yüksek bellek tüketen deyimleri tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT IDENTITY(INT, 1, 1) rowId,
    CAST(query_plan AS XML) query_plan,
    p.query_id
INTO #tmp
FROM sys.query_store_plan AS p
    JOIN sys.query_store_runtime_stats AS r
        ON p.plan_id = r.plan_id
    JOIN sys.query_store_runtime_stats_interval AS i
        ON r.runtime_stats_interval_id = i.runtime_stats_interval_id
WHERE start_time > '2018-10-11 14:00:00.0000000'
      AND end_time < '2018-10-17 20:00:00.0000000';
GO
;WITH cte
AS (SELECT query_id,
        query_plan,
        m.c.value('@SerialDesiredMemory', 'INT') AS SerialDesiredMemory
    FROM #tmp AS t
        CROSS APPLY t.query_plan.nodes('//*:MemoryGrantInfo[@SerialDesiredMemory[. > 0]]') AS m(c) )
SELECT TOP 50
    cte.query_id,
    t.query_sql_text,
    cte.query_plan,
    CAST(SerialDesiredMemory / 1024. AS DECIMAL(10, 2)) SerialDesiredMemory_MB
FROM cte
    JOIN sys.query_store_query AS q
        ON cte.query_id = q.query_id
    JOIN sys.query_store_query_text AS t
        ON q.query_text_id = t.query_text_id
ORDER BY SerialDesiredMemory DESC;
```

### <a name="identify-the-top-10-active-memory-grants"></a>İlk 10 etkin bellek izinler tanımlayın

İlk 10 etkin bellek verir tanımlamak için aşağıdaki sorguyu kullanın:

```sql
SELECT TOP 10
    CONVERT(VARCHAR(30), GETDATE(), 121) AS runtime,
       r.session_id,
       r.blocking_session_id,
       r.cpu_time,
       r.total_elapsed_time,
       r.reads,
       r.writes,
       r.logical_reads,
       r.row_count,
       wait_time,
       wait_type,
       r.command,
       OBJECT_NAME(txt.objectid, txt.dbid) 'Object_Name',
       LTRIM(RTRIM(REPLACE(
                              REPLACE(
                                         SUBSTRING(
                                                      SUBSTRING(
                                                                   text,
                                                                   (r.statement_start_offset / 2) + 1,
                                                                   ((CASE r.statement_end_offset
                                                                         WHEN -1 THEN
                                                                             DATALENGTH(text)
                                                                         ELSE
                                                                             r.statement_end_offset
                                                                     END - r.statement_start_offset
                                                                    ) / 2
                                                                   ) + 1
                                                               ),
                                                      1,
                                                      1000
                                                  ),
                                         CHAR(10),
                                         ' '
                                     ),
                              CHAR(13),
                              ' '
                          )
                  )
            ) stmt_text,
       mg.dop,                                               --Degree of parallelism
       mg.request_time,                                      --Date and time when this query requested the memory grant.
       mg.grant_time,                                        --NULL means memory has not been granted
       mg.requested_memory_kb / 1024.0 requested_memory_mb,  --Total requested amount of memory in megabytes
       mg.granted_memory_kb / 1024.0 AS granted_memory_mb,   --Total amount of memory actually granted in megabytes. NULL if not granted
       mg.required_memory_kb / 1024.0 AS required_memory_mb, --Minimum memory required to run this query in megabytes.
       max_used_memory_kb / 1024.0 AS max_used_memory_mb,
       mg.query_cost,                                        --Estimated query cost.
       mg.timeout_sec,                                       --Time-out in seconds before this query gives up the memory grant request.
       mg.resource_semaphore_id,                             --Non-unique ID of the resource semaphore on which this query is waiting.
       mg.wait_time_ms,                                      --Wait time in milliseconds. NULL if the memory is already granted.
       CASE mg.is_next_candidate --Is this process the next candidate for a memory grant
           WHEN 1 THEN
               'Yes'
           WHEN 0 THEN
               'No'
           ELSE
               'Memory has been granted'
       END AS 'Next Candidate for Memory Grant',
       qp.query_plan
FROM sys.dm_exec_requests AS r
    JOIN sys.dm_exec_query_memory_grants AS mg
        ON r.session_id = mg.session_id
           AND r.request_id = mg.request_id
    CROSS APPLY sys.dm_exec_sql_text(mg.sql_handle) AS txt
    CROSS APPLY sys.dm_exec_query_plan(r.plan_handle) AS qp
ORDER BY mg.granted_memory_kb DESC;
```

## <a name="calculating-database-and-objects-sizes"></a>Veritabanı ve nesneleri boyutları hesaplanıyor

Aşağıdaki sorgu veritabanınızın (megabayt cinsinden) boyutunu döndürür:

```sql
-- Calculates the size of the database.
SELECT SUM(CAST(FILEPROPERTY(name, 'SpaceUsed') AS bigint) * 8192.) / 1024 / 1024 AS DatabaseSizeInMB
FROM sys.database_files
WHERE type_desc = 'ROWS';
GO
```

Aşağıdaki sorgu, veritabanınızda (megabayt cinsinden) tek tek nesnelerin boyutunu döndürür:

```sql
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

```sql
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

## <a name="monitor-resource-use"></a>Kaynak kullanımını izleme

Kaynak kullanımı kullanarak izleyebileceğiniz [SQL veritabanı sorgu performansı İçgörüleri](sql-database-query-performance.md) ve [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

Bu iki görünüm kullanarak kullanımını da izleyebilirsiniz:

- [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
- [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats

Kullanabileceğiniz [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) her SQL veritabanı'nda görüntüle. **Sys.dm_db_resource_stats** son kaynak kullanım verilerini hizmet katmanına göre görüntüler. Ortalama CPU, veri GÇ, günlük yazma ve bellek yüzdelerini her 15 saniyede kaydedilir ve 1 saat boyunca korunur.

Bu görünüm, kaynak kullanımını daha ayrıntılı göz sağladığından, kullanın **sys.dm_db_resource_stats** herhangi bir geçerli durumu çözümlemesi için ilk ya da sorun giderme. Örneğin, bu sorgu, geçerli veritabanı için ortalama ve en yüksek kaynak kullanımı son bir saat içinde gösterir:

```sql
SELECT  
    AVG(avg_cpu_percent) AS 'Average CPU use in percent',
    MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
    AVG(avg_data_io_percent) AS 'Average data IO in percent',
    MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
    AVG(avg_log_write_percent) AS 'Average log write use in percent',
    MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
    AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
    MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
FROM sys.dm_db_resource_stats;  
```

Diğer sorgular için örneklere bakın [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

### <a name="sysresourcestats"></a>sys.resource_stats

[Sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görünümünde **ana** veritabanı, belirli bir hizmet katmanında SQL veritabanınızın performansını izlemek ve boyutu işlem yardımcı olabilecek ek bilgiler vardır. Veriler her 5 dakikada bir toplanan ve yaklaşık 14 gün boyunca tutulur. Bu görünüm, SQL veritabanınızı kaynakları nasıl kullandığını daha uzun vadeli bir geçmiş analize yararlı olur.

Aşağıdaki grafikte CPU kaynak kullanımı için bir Premium veritabanının P2 işlem boyutu ile her saat için bir hafta içinde gösterilmektedir. Bu grafik Pazartesi günü başlar, 5 iş günü gösterir ve ardından hafta gösterir, uygulama üzerinde çok az olur.

![SQL veritabanı kaynak kullanımı](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Verileri, bu veritabanı şu anda en yüksek CPU yükü yok. yalnızca yüzde 50'işlem CPU kullanım P2 göreli boyutu (Salı günü öğle saati). CPU uygulamanın kaynak profilinde baskın faktörü ise, P2 iş yükü her zaman en uygun olduğunu garanti etmek için doğru işlem boyutu olduğunu karar verebilirsiniz. Bir uygulama, zaman içinde artmasına bekliyorsanız, böylece uygulama hiç olmadığı kadar performans düzeyi sınırına ulaşmadığınız olmayan bir ek kaynak arabelleği sağlamak için iyi bir fikirdir. İşlem boyutu artırmak istiyorsanız, bir veritabanı istekleri özellikle gecikmeye duyarlı ortamlarda etkili bir şekilde işlemek için yeterli güce sahip olmadığında ortaya çıkabilir müşteri görünür hatalarının önüne geçmek yardımcı olabilir. Örnek Web sayfalarını veritabanı çağrıları sonuçlarına göre boyar uygulamanın desteklediği bir veritabanıdır.

Diğer uygulama türleri aynı grafikte farklı yorumlayabilir. Örneğin, uygulamanın her gün Bordro veri işlemeye çalışır ve aynı grafiğe varsa, bu tür bir "toplu" modeli P1 bilgi işlem boyutta ince ayar yapabilirsiniz. İşlem boyutu 100 Dtu'yu kıyasla 200 dtu'ya varan P2, P1 işlem boyutu vardır. P1 işlem boyutu, boyut P2 yarım performansını işlem sağlar. Bu nedenle, P2 CPU kullanımı yüzde 50, P1, CPU kullanımı yüzde 100'e eşittir. Uygulama zaman aşımları yoksa bugün Bitti, bir işin 2 saat veya 2.5 tamamlamak için saat sürüyor olup olmaması önemli değil. Bu kategorideki bir uygulama, büyük olasılıkla P1 işlem boyutu kullanabilirsiniz. Böylece tüm "büyük yoğun" troughs birine gün içinde sığdırmaya kaynak kullanımının düşük olduğunda günde süreler olması, bir avantajlarından yararlanabilirsiniz. İşleri her gün zamanında bitirebilir sürece P1 işlem boyutu için bu türden uygulamayı (ve tasarruf), iyi olabilir.

Tüketilen kaynak bilgilerini active her veritabanı için Azure SQL veritabanı kullanıma sunan **sys.resource_stats** görünümünü **ana** her bir sunucudaki veritabanı. Tablodaki verileri için 5 dakikalık aralıklarla toplanır. Temel, standart ve Premium hizmet katmanlarıyla veri tabloda, bu verileri neredeyse gerçek zamanlı analiz yerine geçmiş çözümleme için daha yararlı olacak şekilde görünmesini fazla 5 dakika sürebilir. Sorgu **sys.resource_stats** bir veritabanının en son geçmişini görmek için görüntüleyin ve doğrulamak için mi ayırma, seçtiğiniz gerektiğinde istediğiniz performans teslim.

> [!NOTE]
> İçin bağlanmalıdır **ana** SQL veritabanı sunucunuzun sorgu için veritabanı **sys.resource_stats** ilişkin aşağıdaki örneklerde.

Bu örnekte, bu görünümdeki veriler nasıl sunulur gösterir:

```sql
SELECT TOP 10 *
FROM sys.resource_stats
WHERE database_name = 'resource1'
ORDER BY start_time DESC
```

![Sys.resource_stats Katalog görünümü](./media/sql-database-performance-guidance/sys_resource_stats.png)

Sonraki örnek, kullanabileceğiniz farklı yolları gösterir **sys.resource_stats** Katalog görünümü, SQL veritabanı kaynak kullanımı hakkında bilgi almak için:

1. Geçen hafta kaynakta aramak için veritabanı userdb1 için kullanın, bu sorguyu çalıştırabilirsiniz:

    ```sql
    SELECT *
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND
        start_time > DATEADD(day, -7, GETDATE())
    ORDER BY start_time DESC;
    ```

2. İş yükünüz işlem boyutu ne kadar iyi uyduğunu değerlendirmek için kaynak ölçümleri her yönüyle detaya gerekir: CPU, okuma, yazma, çalışan sayısı ve oturum sayısı. İşte bir düzeltilmiş kullanarak sorgu **sys.resource_stats** bu kaynak ölçümleri ortalama ve maksimum değerler bildirmek için:

    ```sql
    SELECT
        avg(avg_cpu_percent) AS 'Average CPU use in percent',
        max(avg_cpu_percent) AS 'Maximum CPU use in percent',
        avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
        max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
        avg(avg_log_write_percent) AS 'Average log write use in percent',
        max(avg_log_write_percent) AS 'Maximum log write use in percent',
        avg(max_session_percent) AS 'Average % of sessions',
        max(max_session_percent) AS 'Maximum % of sessions',
        avg(max_worker_percent) AS 'Average % of workers',
        max(max_worker_percent) AS 'Maximum % of workers'
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

3. Bu her kaynak ölçümü ile ortalama ve maksimum değerleri hakkında bilgi iş yükünüz, seçtiğiniz işlem boyuta ne kadar iyi uyduğunu değerlendirebilirsiniz. Genellikle, değerleri ortalama **sys.resource_stats** karşı hedef boyutu kullanmak iyi bir temel sağlar. Bu, birincil ölçüm Sopası olmalıdır. Örneğin, standart hizmet katmanı S2 işlem boyutu ile kullanıyor olabilir. Ortalama CPU ve g/ç okuma için yüzde kullanın ve yüzde 40'a yazma olan, çalışan ortalama sayısı 50 altına olduğu ve ortalama oturum sayısı 200. İş yükünüz S1 işlem boyutuna sığdırmaya. Veritabanınızı çalışan ve oturumu sınırları uygun olup olmadığını görmek kolay bir işlemdir. Bir veritabanı içine bir alt işlem boyutu CPU, okuma ve yazma işlemleri bölme bakımından en uygun olup olmadığını görmek için daha düşük DTU sayısını işlem boyutu, geçerli işlem boyutu DTU sayısı ve ardından sonucu 100 ile çarpın:

    ```S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40```

    İkisi arasındaki göreli performans farkı boyutları yüzde cinsinden işlem sonucudur. İş yükünüz, kaynak kullanımınızı bu miktarı aşmıyorsa, daha düşük işlem boyutuna sığdırmaya. Ancak, kaynak kullanımı değerlerinin tüm aralıklarına bakın ve belirlemek, yüzde olarak, ne sıklıkta, veritabanı iş yükünüzü daha düşük işlem boyutuna sığdırmaya gerekir. Aşağıdaki sorgu uygun yüzdesi eşiği Biz bu örnekte hesaplanan yüzde 40'ın temel kaynak boyut başına verir:

   ```sql
    SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent',
        (COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent',
        (COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
    FROM sys.resource_stats
    WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

    Veritabanı Hizmet katmanına bağlı olarak, iş yükünüzü daha düşük işlem boyuta uygun olup olmadığını karar verebilirsiniz. Veritabanı iş yükü hedefiniz yüzde 99,9 ise ve önceki sorgunun tüm üç kaynak boyutlar yüzde 99, 9'dan büyük değerleri döndürür, yükünüz büyük olasılıkla daha düşük işlem boyutuna sığar.

    Ayrıca uygun yüzdesiyle bakarak hedefiniz karşılamak için sonraki daha yüksek işlem boyutu için mi taşımalısınız Öngörüler sağlar. Örneğin, geçen hafta için aşağıdaki CPU kullanımı userdb1 gösterir:

   | Ortalama CPU yüzdesi | En fazla CPU yüzdesi |
   | --- | --- |
   | 24.5 |100.00 |

    Ortalama CPU iyi veritabanının işlem boyutuna sığdırmaya işlem boyutu sınırını çeyreği hakkındadır. Ancak, veritabanı işlem boyutu sınırına ulaştığında en büyük değeri gösterir. Sonraki daha yüksek işlem boyutu için taşımanız gerekiyor mu? Birden çok kez, iş yükü ulaştığında yüzde 100 konuları ele ve ardından veritabanı iş yükü hedefiniz karşılaştırır.

    ```sql
    SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
    ```

    Bu sorgu, bir değer döndürürse yüzde 99, 9'den küçük üç kaynak boyutların hiçbiri için sonraki daha yüksek işlem boyutu taşımayı deneyin veya SQL veritabanı üzerindeki yükü azaltmak için uygulama ayarlamayı tekniklerini kullanın.

4. Bu alıştırmada, tahmin edilen iş yükü artışı da gelecekte değerlendirir.

Elastik havuzlar için bu bölümde açıklanan olan tekniklerle havuzda bulunan tek veritabanlarını izleyebilirsiniz. Ancak havuzu bir bütün olarak da izleyebilirsiniz. Bilgi için bkz. [Elastik havuz izleme ve yönetme](sql-database-elastic-pool-manage-portal.md).

### <a name="maximum-concurrent-requests"></a>Maksimum eşzamanlı istekler

Eş zamanlı istek sayısını görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    ```sql
    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R;
    ```

Şirket içi SQL Server veritabanı iş yükü analiz etmek için analiz etmek istediğiniz belirli veritabanında filtrelemek için bu sorguyu değiştirin. Örneğin, bu Transact-SQL sorgusu Veritabanım adlı bir şirket içi veritabanı varsa, bu veritabanında eş zamanlı istek sayısını döndürür:

    ```sql
    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase';
    ```

Zaman içinde bir anlık görüntü yalnızca tek bir noktada budur. İş yükü ve eş zamanlı istek gereksinimlerini daha iyi anlamak için zaman içinde çok sayıda toplamak gerekir.

### <a name="maximum-concurrent-logins"></a>En fazla eşzamanlı oturum açma

Oturumlarının sıklığı hakkında bir fikir edinmek için kullanıcı ve uygulama desenleri analiz edebilirsiniz. Ayrıca, bu ya da bu makalede ele diğer sınırlamaları aldığınızı değil emin emin olmak için bir test ortamında gerçek dünyadaki yüklerin da çalıştırabilirsiniz. Tek bir sorgu veya eşzamanlı oturum açma sayıları göstermek, dinamik yönetim görünümünü (DMV) veya geçmiş yok.

Hizmet, birden çok istemci aynı bağlantı dizesi kullanıyorsanız, her oturum açma kimliğini doğrular. Aynı anda 10 kullanıcı aynı kullanıcı adı ve parola kullanarak bir veritabanına bağlanmak, 10 eşzamanlı oturum açma olacaktır. Bu sınır, oturum açma ve kimlik doğrulama süresi geçerlidir. Eşzamanlı oturum açma sayısı hiçbir zaman 10 kullanıcıları sırayla veritabanına bağlanmak, 1'den büyük olacaktır.

> [!NOTE]
> Şu anda, bu sınır elastik havuzlardaki veritabanları için geçerli değildir.

### <a name="maximum-sessions"></a>En fazla oturum sayısı

Geçerli etkin oturumları sayısını görmek için bu Transact-SQL sorgusu SQL veritabanınızda çalıştırın:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Bir şirket içi SQL Server iş yükü çözümlediğiniz, belirli bir veritabanı üzerinde odaklanmak için sorguyu değiştirin. Bu sorguyu Azure SQL veritabanı'na taşıma düşünüyorsanız veritabanı için olası oturum gereksinimlerini belirlemenize yardımcı olur.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Yeniden, bu sorguları zaman içinde nokta sayısını döndürür. Zaman içinde birden fazla örnek toplarsanız kullanmak en iyi anlama oturumunuz sahip olacaksınız.

SQL veritabanı analiz için geçmişe dönük İstatistikler oturumları sorgulayarak alabileceğiniz [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) görüntüleme ve İnceleme **active_session_count** sütun.

## <a name="monitoring-query-performance"></a>Sorgu performansını izleme

Yavaş veya uzun süre çalışan sorguların önemli sistem kaynakları kullanabilir. Bu bölümde, birkaç ortak sorgu performansı sorunlarını algılamak için dinamik yönetim görünümlerini nasıl yapılacağı açıklanır. Sorun giderme, eski ancak yine de yararlı bir başvuru [SQL Server 2008'de performans sorunlarını giderme](https://download.microsoft.com/download/D/B/D/DBDE7972-1EB9-470A-BA18-58849DB3EB3B/TShootPerfProbs2008.docx) Microsoft TechNet makalesi.

### <a name="finding-top-n-queries"></a>İlk N sorgularını bulma

Aşağıdaki örnek, ortalama CPU süresine göre sıralanmış ilk beş sorgu hakkındaki bilgileri döndürür. Mantıksal eşdeğer sorgular tarafından toplam kaynak tüketimi gruplanması Bu örnekte sorgu, sorgu karmasına göre toplar.

    ```sql
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

    ```sql
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
