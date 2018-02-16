---
title: "Dmv'leri kullanarak, iş yükünü izlemek | Microsoft Docs"
description: "Dmv'leri kullanarak, iş yükünü izlemek öğrenin."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 69ecd479-0941-48df-b3d0-cf54c79e6549
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 12/14/2017
ms.author: joeyong;barbkess;kevin
ms.openlocfilehash: 1895e9c6174dfb05212991040cc265b8cb6e0651
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-your-workload-using-dmvs"></a>DMV’leri kullanarak iş yükünüzü izleme
Bu makalede dinamik yönetim görünümlerini (Dmv'leri), iş yükünü izlemek ve sorgu yürütme Azure SQL Data Warehouse araştırmak için nasıl kullanılacağını açıklar.

## <a name="permissions"></a>İzinler
Bu makalede Dmv'leri sorgu görünümü veritabanı durumu veya Denetim izni gerekir. Genellikle çok daha kısıtlayıcı olduğu gibi izni veriliyor görünüm veritabanı durumu tercih edilen izni ' dir.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>İzleyici bağlantıları
SQL veri ambarı için tüm oturumları oturum [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Bu DMV 10. 000'son oturum açma bilgileri içerir.  Session_id birincil anahtar ve sıralı olarak her yeni oturum açma için atanır.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>İzleyici sorgu yürütme
SQL veri ambarı üzerinde yürütülen tüm sorguları oturum [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Bu DMV yürütülen son 10.000 sorgularını içerir.  Request_id benzersiz olarak her sorgu tanımlar ve bu DMV için birincil anahtar.  Request_id her yeni sorgu için ardışık olarak atanır ve sorgu kimliği için anlamına gelir QID ile önek  Bu DMV verilen session_id için sorgulama belirli bir oturum açma için tüm sorguları gösterir.

> [!NOTE]
> Birden çok istek kimliği saklı yordamları kullanın.  İstek kimlikleri sıralı bir düzende atanır. 
> 
> 

Sorgu yürütme planları ve belirli bir sorgu saatleri araştırmak için izlemeniz gereken adımlar şunlardır.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>1. adım: araştırmak istediğiniz sorgu tanımla
```sql
-- Monitor active queries
SELECT * 
FROM sys.dm_pdw_exec_requests 
WHERE status not in ('Completed','Failed','Cancelled')
  AND session_id <> session_id()
ORDER BY submit_time DESC;

-- Find top 10 queries longest running queries
SELECT TOP 10 * 
FROM sys.dm_pdw_exec_requests 
ORDER BY total_elapsed_time DESC;

-- Find a query with the Label 'My Query'
-- Use brackets when querying the label column, as it it a key word
SELECT  *
FROM    sys.dm_pdw_exec_requests
WHERE   [label] = 'My Query';
```

Önceki sorgu sonuçlarından **istek kimliği Not** araştırmak istediğiniz sorgunun.

İçindeki sorgular **askıya** durumu eşzamanlılık sınırlara sıraya. Bu sorguları da UserConcurrencyResourceType türüne sahip sys.dm_pdw_waits bekler sorgu görünür. Bkz: [eşzamanlılık ve iş yükü yönetimi] [ Concurrency and workload management] eşzamanlılık sınırları hakkında daha fazla ayrıntı için. Sorgular gibi başka bir nedenle nesne kilitleri için de bekleyebilirsiniz.  Sorgunuz bekliyor. bir kaynak için bkz: [kaynaklar bekleniyor sorguları araştırma] [ Investigating queries waiting for resources] bu makalede daha aşağı.

Sys.dm_pdw_exec_requests tablosundaki bir sorgunun arama basitleştirmek için kullanma [etiket] [ LABEL] sys.dm_pdw_exec_requests görünümünde aranabilir sorgunuzu yorum atamak için.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>2. adım: sorgu planı araştırın
İstek Kimliği sorgunun dağıtılmış SQL (DSQL) planından almanızı [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

DSQL planı beklenenden uzun sürdüğünde nedeni karmaşık planla birçok DSQL adımları ya da tek bir adım uzun sürüyor olabilir.  Plan birkaç taşıma işlemleri ile birçok adımı ise, veri taşıma azaltmak için tablo dağıtımları en iyi duruma getirme göz önünde bulundurun. [Tablo dağıtımı] [ Table distribution] makalede açıklanır neden verileri bir sorgu çözmek için taşınmalıdır ve veri taşıma en aza indirmek için bazı dağıtım stratejilerini açıklar.

Daha fazla tek bir adım ayrıntılarını araştırmak için *operation_type* uzun süre çalışan sorgu adım ve Not sütunu **adım dizin**:

* Adım 3a devam **SQL işlemleri**: OnOperation, RemoteOperation, ReturnOperation.
* Adım 3b devam **veri taşıma işlemleri**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Adım 3a: Dağıtılmış veritabanlarının SQL araştırın
İstek Kimliği ve adım dizin ayrıntılarını almak için kullanın [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], tüm dağıtılmış veritabanları üzerinde sorgu adımının yürütme bilgi içerir.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Sorgu adımı çalıştırıldığında, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] belirli bir dağıtım noktasında çalışan adım için SQL Server planı önbelleğinden SQL Server tahmini planı almak için kullanılır.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Adım 3b: Dağıtılmış veritabanlarının veri taşıma araştırın
Her dağıtım noktasından çalışan bir veri taşıma adım hakkında bilgi almak için istek kimliği ve adım dizini kullanın [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Denetleme *total_elapsed_time* belirli bir dağıtım diğerlerinden veri taşıma için önemli ölçüde uzun sürüyor durumunda görmek için sütun.
* Uzun süre çalışan dağıtım için denetleme *rows_processed* bu dağıtım noktasından taşınan satır sayısını diğerlerinden daha önemli ölçüde daha büyük olup olmadığını görmek için sütun. Öyleyse, bu, temel alınan verilerinizin eğme gösteriyor olabilir.

Sorgu çalışıyorsa, [DBCC PDW_SHOWEXECUTIONPLAN] [ DBCC PDW_SHOWEXECUTIONPLAN] SQL Server tahmini planı şu anda çalışan SQL adım içinde belirli bir dağıtım için SQL Server plan önbelleğinde almak için kullanılabilir.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>İzleyici bekleme sorguları
Bir kaynak için beklediğinden sorgunuzu ilerleme göstermiyor olduğunu fark ederseniz, tüm kaynaklar için bekliyor. bir sorgu gösteren bir sorgu aşağıda verilmiştir.

```sql
-- Find queries 
-- Replace request_id with value from Step 1.

SELECT waits.session_id,
      waits.request_id,  
      requests.command,
      requests.status,
      requests.start_time,  
      waits.type,
      waits.state,
      waits.object_type,
      waits.object_name
FROM   sys.dm_pdw_waits waits
   JOIN  sys.dm_pdw_exec_requests requests
   ON waits.request_id=requests.request_id
WHERE waits.request_id = 'QID####'
ORDER BY waits.object_name, waits.object_type, waits.state;
```

Sorgu etkin olarak başka bir sorgu kaynaklardan bekleyen sonra durumu olacaktır **AcquireResources**.  Sorgu gerekli tüm kaynaklara sahip sonra durumu olacaktır **izin verildi**.

## <a name="monitor-tempdb"></a>İzleyici tempdb
Yüksek tempdb kullanımı yavaş performans ve bellek sorunları dışında kök neden olabilir. Sorgu yürütme sırasında sınırlarına ulaşması tempdb bulursanız, veri ambarı ölçeklendirme göz önünde bulundurun. Aşağıdaki sorgu her düğümde başına tempdb kullanımı tanımlamak açıklar. 

Sys.dm_pdw_sql_requests için uygun düğüm kimliği ilişkilendirmek için aşağıdaki görünümü oluşturun. Bu, diğer geçiş Dmv'leri yararlanır ve bu tabloları sys.dm_pdw_sql_requests ile birleştirme olanak tanır.

```sql
-- sys.dm_pdw_sql_requests with the correct node id
CREATE VIEW sql_requests AS
(SELECT
       sr.request_id,
       sr.step_index,
       (CASE 
              WHEN (sr.distribution_id = -1 ) THEN 
              (SELECT pdw_node_id FROM sys.dm_pdw_nodes WHERE type = 'CONTROL') 
              ELSE d.pdw_node_id END) AS pdw_node_id,
       sr.distribution_id,
       sr.status,
       sr.error_id,
       sr.start_time,
       sr.end_time,
       sr.total_elapsed_time,
       sr.row_count,
       sr.spid,
       sr.command
FROM sys.pdw_distributions AS d
RIGHT JOIN sys.dm_pdw_sql_requests AS sr ON d.distribution_id = sr.distribution_id)
```
Tempdb izlemek için aşağıdaki sorguyu çalıştırın:

```sql
-- Monitor tempdb
SELECT
    sr.request_id,
    ssu.session_id,
    ssu.pdw_node_id,
    sr.command,
    sr.total_elapsed_time,
    es.login_name AS 'LoginName',
    DB_NAME(ssu.database_id) AS 'DatabaseName',
    (es.memory_usage * 8) AS 'MemoryUsage (in KB)',
    (ssu.user_objects_alloc_page_count * 8) AS 'Space Allocated For User Objects (in KB)',
    (ssu.user_objects_dealloc_page_count * 8) AS 'Space Deallocated For User Objects (in KB)',
    (ssu.internal_objects_alloc_page_count * 8) AS 'Space Allocated For Internal Objects (in KB)',
    (ssu.internal_objects_dealloc_page_count * 8) AS 'Space Deallocated For Internal Objects (in KB)',
    CASE es.is_user_process
    WHEN 1 THEN 'User Session'
    WHEN 0 THEN 'System Session'
    END AS 'SessionType',
    es.row_count AS 'RowCount'
FROM sys.dm_pdw_nodes_db_session_space_usage AS ssu
    INNER JOIN sys.dm_pdw_nodes_exec_sessions AS es ON ssu.session_id = es.session_id AND ssu.pdw_node_id = es.pdw_node_id
    INNER JOIN sys.dm_pdw_nodes_exec_connections AS er ON ssu.session_id = er.session_id AND ssu.pdw_node_id = er.pdw_node_id
    INNER JOIN sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```
## <a name="monitor-memory"></a>İzleyici bellek

Bellek yetersiz bellek sorunları ve yavaş performans için kök neden olabilir. SQL Server bellek kullanımı sorgu yürütme sırasında sınırlarına ulaşması bulursanız, veri ambarı ölçeklendirme göz önünde bulundurun.

Aşağıdaki sorgu düğümü başına SQL Server bellek kullanımı ve bellek baskısı döndürür:   
```sql
-- Memory consumption
SELECT
  pc1.cntr_value as Curr_Mem_KB, 
  pc1.cntr_value/1024.0 as Curr_Mem_MB,
  (pc1.cntr_value/1048576.0) as Curr_Mem_GB,
  pc2.cntr_value as Max_Mem_KB,
  pc2.cntr_value/1024.0 as Max_Mem_MB,
  (pc2.cntr_value/1048576.0) as Max_Mem_GB,
  pc1.cntr_value * 100.0/pc2.cntr_value AS Memory_Utilization_Percentage,
  pc1.pdw_node_id
FROM
-- pc1: current memory
sys.dm_pdw_nodes_os_performance_counters AS pc1
-- pc2: total memory allowed for this SQL instance
JOIN sys.dm_pdw_nodes_os_performance_counters AS pc2 
ON pc1.object_name = pc2.object_name AND pc1.pdw_node_id = pc2.pdw_node_id
WHERE
pc1.counter_name = 'Total Server Memory (KB)'
AND pc2.counter_name = 'Target Server Memory (KB)'
```
## <a name="monitor-transaction-log-size"></a>İzleyici işlem günlüğü boyutu
Aşağıdaki sorguda her dağıtım noktasında işlem günlüğü boyutu döndürür. Günlük dosyaları birini 160 GB ulaşıyor varsa, örnek, ölçeklendirme veya işlem boyutunuz sınırlama düşünmelisiniz. 
```sql
-- Transaction log size
SELECT
  instance_name as distribution_db,
  cntr_value*1.0/1048576 as log_file_size_used_GB,
  pdw_node_id 
FROM sys.dm_pdw_nodes_os_performance_counters 
WHERE 
instance_name like 'Distribution_%' 
AND counter_name = 'Log File(s) Used Size (KB)'
```
## <a name="monitor-transaction-log-rollback"></a>İzleme işlemi günlük geri alma
Sorgularınızın başarısız veya devam etmek için uzun sürüyor, denetleyin ve geri alma işlemler varsa izleyin.
```sql
-- Monitor rollback
SELECT 
    SUM(CASE WHEN t.database_transaction_next_undo_lsn IS NOT NULL THEN 1 ELSE 0 END),
    t.pdw_node_id,
    nod.[type]
FROM sys.dm_pdw_nodes_tran_database_transactions t
JOIN sys.dm_pdw_nodes nod ON t.pdw_node_id = nod.pdw_node_id
GROUP BY t.pdw_node_id, nod.[type]
```

## <a name="next-steps"></a>Sonraki adımlar
Bkz: [sistem görünümleri] [ System views] Dmv'leri hakkında daha fazla bilgi için.
Bkz: [SQL veri ambarı en iyi yöntemler] [ SQL Data Warehouse best practices] en iyi uygulamalar hakkında daha fazla bilgi için

<!--Image references-->

<!--Article references-->
[Manage overview]: ./sql-data-warehouse-overview-manage.md
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: http://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: http://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: http://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: http://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: http://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: http://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: http://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
