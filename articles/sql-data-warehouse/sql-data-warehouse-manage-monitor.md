---
title: Dmv'leri kullanarak iş yükünüzü izleme | Microsoft Docs
description: Dmv'leri kullanarak iş yükünüzü izleme hakkında bilgi edinin.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/12/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: ff1f613dfdfb5c43b727bcc9c7f7a1f0afca0975
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546905"
---
# <a name="monitor-your-workload-using-dmvs"></a>DMV’leri kullanarak iş yükünüzü izleme
Bu makalede, dinamik yönetim görünümlerini (Dmv'ler) iş yükünüzü izleme için kullanmayı açıklar. Bu, Azure SQL veri ambarı, sorgu yürütme araştırma içerir.

## <a name="permissions"></a>İzinler
Bu makalede Dmv'leri sorgulamak için VIEW DATABASE STATE ya da Denetim izni gerekir. Genellikle ekibi tarafından verilmesinin VIEW DATABASE STATE daha kısıtlayıcı olarak tercih edilen izindir.

```sql
GRANT VIEW DATABASE STATE TO myuser;
```

## <a name="monitor-connections"></a>Bağlantılarını izleme
Tüm oturum açma bilgileri SQL veri ambarı'nda oturum açtığınız [sys.dm_pdw_exec_sessions][sys.dm_pdw_exec_sessions].  Bu DMV son 10.000 giriş bilgilerini içerir.  Session_ıd, birincil anahtar ve sıralı olarak her yeni oturum açma için atanır.

```sql
-- Other Active Connections
SELECT * FROM sys.dm_pdw_exec_sessions where status <> 'Closed' and session_id <> session_id();
```

## <a name="monitor-query-execution"></a>İzleyici sorgu yürütme
SQL veri ambarı üzerinde yürütülen tüm sorguları oturum [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests].  Bu DMV yürütülen son 10.000 sorgular içerir.  Request_id benzersiz biçimde her bir sorgu tanımlar ve bu DMV için birincil anahtarı.  Request_id her yeni bir sorgu için sırayla atanır ve sorgu kimliği için temsil eden QID önüne  Belirli bir session_ıd için bu DMV sorgulama tüm sorgular için belirli bir oturum açma gösterir.

> [!NOTE]
> Saklı yordamları birden çok istek kimliklerini kullanın.  İstek kimliklerinin sırayla atanır. 
> 
> 

Sorgu yürütme planları ve belirli bir sorgu saatleri araştırmak için izlemeniz gereken adımlar aşağıda verilmiştir.

### <a name="step-1-identify-the-query-you-wish-to-investigate"></a>ADIM 1: Araştırmak istediğiniz sorgu tanımlama
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

Önceki sorgu sonuçlarından **istek kimliği Not** araştırmak istediğiniz sorgu.

Sorgulara **askıya alındı** durumu nedeniyle etkin çalışan sorgu çok fazla sayıda kuyruğa. Bu sorguları da görünür [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql) bekler sorgu UserConcurrencyResourceType türüne sahip. Eşzamanlılık sınırları hakkında daha fazla bilgi için bkz: [performans katmanları](performance-tiers.md) veya [iş yükü yönetimi için kaynak sınıfları](resource-classes-for-workload-management.md). Sorgular gibi diğer nedenler nesne kilitleri için de bekleyebilirsiniz.  Sorgunuz, bir kaynağı bekleyen olup [kaynaklar için bekleyen sorguların araştırma] [ Investigating queries waiting for resources] bu makalede daha ilerisine.

Bir sorgunun arama basitleştirmek için [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) tablo, kullanın [etiket] [ LABEL] bir yorum içinde sys.dm_pdw_exec_ aranabilir sorgunuzu atamak için isteklerini görüntüleyin.

```sql
-- Query with Label
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query')
;
```

### <a name="step-2-investigate-the-query-plan"></a>2. ADIM: Sorgu planı araştırın
İstek Kimliği sorgu dağıtılmış SQL (DSQL) plandan almanızı [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps].

```sql
-- Find the distributed query plan steps for a specific query.
-- Replace request_id with value from Step 1.

SELECT * FROM sys.dm_pdw_request_steps
WHERE request_id = 'QID####'
ORDER BY step_index;
```

DSQL planı beklenenden uzun sürüyor, karmaşık bir plan veya birçok DSQL adımları uzun sürüyor. yalnızca bir adım ile neden olabilir.  Plan çeşitli taşıma işlemlerini çok adımlı ise, veri taşıma azaltmak için tablo dağıtımlarını en iyi duruma getirme göz önünde bulundurun. [Tablo dağıtımı] [ Table distribution] makalede açıklanmaktadır neden veri sorgu çözmek için taşınmalıdır ve veri taşıma en aza indirmek için bazı dağıtım stratejilerini açıklar.

Tek bir adım hakkında ayrıntılar daha ayrıntılı araştırmaya *operation_type* uzun süre çalışan bir sorgu adımına ve Not sütun **adımı indisine**:

* Adım 3a için devam **SQL işlemleri**: OnOperation, RemoteOperation, ReturnOperation.
* Adım 3b için devam **veri taşıma işlemleri**: ShuffleMoveOperation, BroadcastMoveOperation, TrimMoveOperation, PartitionMoveOperation, MoveOperation, CopyOperation.

### <a name="step-3a-investigate-sql-on-the-distributed-databases"></a>Adım 3a: Dağıtılmış veritabanlarının SQL araştırın
İstek Kimliği ve adım dizini ayrıntılarını almak için kullanmak [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], bir sorgu adımına yürütme bilgilerinin tüm veritabanlarını içerir.

```sql
-- Find the distribution run times for a SQL step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_sql_requests
WHERE request_id = 'QID####' AND step_index = 2;
```

Sorgu adımı çalıştırırken [DBCC pdw_showexecutıonplan] [ DBCC PDW_SHOWEXECUTIONPLAN] çalıştıran belirli bir dağıtım adımı için SQL Server plan önbelleğinden SQL Server tahmini planı almak için kullanılabilir.

```sql
-- Find the SQL Server execution plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(1, 78);
```

### <a name="step-3b-investigate-data-movement-on-the-distributed-databases"></a>Adım 3b: Dağıtılmış veritabanları üzerinde veri taşıma araştırın
Her dağıtım noktasından çalışan bir veri taşıma adım hakkında bilgi almak için istek kimliği ve adım dizini kullanın [sys.dm_pdw_dms_workers][sys.dm_pdw_dms_workers].

```sql
-- Find the information about all the workers completing a Data Movement Step.
-- Replace request_id and step_index with values from Step 1 and 3.

SELECT * FROM sys.dm_pdw_dms_workers
WHERE request_id = 'QID####' AND step_index = 2;
```

* Denetleme *total_elapsed_time* belirli bir dağıtım önemli ölçüde uzun veri taşıma için diğerlerinden almadığını görmek için sütun.
* Uzun süre çalışan dağıtım için denetleme *rows_processed* bu dağıtım noktasından taşınan satır sayısını diğerlerinden daha önemli ölçüde daha büyük olup olmadığını görmek için sütun. Bu durumda, bu bulma, temel alınan veri dengesizliği gösteriyor olabilir.

Sorgu çalışıyorsa [DBCC pdw_showexecutıonplan] [ DBCC PDW_SHOWEXECUTIONPLAN] SQL Server tahmini planı şu anda çalışan SQL adım içinde belirli bir SQL Server plan önbelleğinden almak için kullanılabilir Dağıtım.

```sql
-- Find the SQL Server estimated plan for a query running on a specific SQL Data Warehouse Compute or Control node.
-- Replace distribution_id and spid with values from previous query.

DBCC PDW_SHOWEXECUTIONPLAN(55, 238);
```

<a name="waiting"></a>

## <a name="monitor-waiting-queries"></a>Bekleyen sorguları izleme
Bir kaynak için beklediği sorgunuzu ilerleme yaptığını değil bulursanız, bir sorgu için bekleyen tüm kaynakları gösteren bir sorgu aşağıda verilmiştir.

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

Sorgu etkin olarak başka bir sorgu kaynaklardan Bekleniyor durumunda durumları **AcquireResources**.  Sorgu tüm gerekli kaynaklara sahip sonra durumları **izin verildi**.

## <a name="monitor-tempdb"></a>İzleyici tempdb
Tempdb, sorgu yürütme işlemi sırasında Ara sonuçlarını tutmak için kullanılır. Tempdb veritabanının yüksek miktarda sorgu performansı yavaş neden olabilir. Azure SQL veri ambarı'nda her düğümü, yaklaşık 1 TB ham tempdb alanı vardır. Aşağıda, tempdb kullanımını izlemek için ve sorgularınızı tempdb kullanımını azaltmak için ipuçları verilmiştir. 

### <a name="monitoring-tempdb-with-views"></a>Tempdb görünümleri ile izleme
Tempdb kullanımını izlemek için ilk yükleme [microsoft.vw_sql_requests](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/solutions/monitoring/scripts/views/microsoft.vw_sql_requests.sql) görünümüne [SQL veri ambarı için Microsoft Toolkit](https://github.com/Microsoft/sql-data-warehouse-samples/tree/master/solutions/monitoring). Ardından, tüm yürütülen sorgular için düğüm başına tempdb kullanımını görmek için aşağıdaki sorguyu yürütebilirsiniz:

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
    INNER JOIN microsoft.vw_sql_requests AS sr ON ssu.session_id = sr.spid AND ssu.pdw_node_id = sr.pdw_node_id
WHERE DB_NAME(ssu.database_id) = 'tempdb'
    AND es.session_id <> @@SPID
    AND es.login_name <> 'sa' 
ORDER BY sr.request_id;
```

Büyük miktarda bellek tüketiyor veya tempdb ayırma için ilgili bir hata iletisini aldığınız bir sorgu varsa, genellikle nedeniyle çok büyük olduğundan [CREATE TABLE AS SELECT (CTAS)](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) veya [INSERT SELECT](https://docs.microsoft.com/sql/t-sql/statements/insert-transact-sql) çalıştıran deyimi içinde son veri taşıma işlemi başarısız oluyor. Bu genellikle ShuffleMove işlem olarak son INSERT SELECT hemen önce dağıtılmış sorgu planını tanımlanabilir.

En yaygın azaltma, veri hacmi 1 TB başına düğüm tempdb sınırını aşmayacak şekilde birden çok yük açıklamaalarını break, CTAS veya INSERT SELECT deyimi sağlamaktır. Kümenize tempdb boyutunu azaltma tempdb her bir düğümün üzerinde daha fazla düğüm arasında yayılır, daha büyük bir boyutu da ölçekleyebilirsiniz. 

## <a name="monitor-memory"></a>Bellek izleme

Bellek, kök nedeni yavaş performans ve bellek sorunlarını dışında olabilir. SQL Server bellek kullanımı sınırlarını ulaşma sorgu yürütme işlemi sırasında fark ederseniz veri Ambarınızı genişletmeyi düşünün.

Aşağıdaki sorgu, düğüm başına SQL Server bellek kullanımı ve bellek baskısı döndürür:   
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
## <a name="monitor-transaction-log-size"></a>İşlem günlüğü boyutu izleyin
Aşağıdaki sorgu, her dağıtım noktasında işlem günlüğü boyutu döndürür. Bir günlük dosyalarının 160 GB ulaşmasını varsa, örneği oluşturan ölçeklendirme veya işlem boyutunuz sınırlama düşünmelisiniz. 
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
## <a name="monitor-transaction-log-rollback"></a>İşlem günlüğü geri izleme
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
Dmv'leri hakkında daha fazla bilgi için bkz: [sistem görünümleri][System views].


<!--Image references-->

<!--Article references-->
[SQL Data Warehouse best practices]: ./sql-data-warehouse-best-practices.md
[System views]: ./sql-data-warehouse-reference-tsql-system-views.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Investigating queries waiting for resources]: ./sql-data-warehouse-manage-monitor.md#waiting

<!--MSDN references-->
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_exec_sessions]: https://msdn.microsoft.com/library/mt203883.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[DBCC PDW_SHOWSPACEUSED]: https://msdn.microsoft.com/library/mt204028.aspx
[LABEL]: https://msdn.microsoft.com/library/ms190322.aspx
