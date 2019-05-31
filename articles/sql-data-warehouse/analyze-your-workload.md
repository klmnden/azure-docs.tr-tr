---
title: Azure SQL veri ambarı iş yükünüzü çözümleme | Microsoft Docs
description: Azure SQL veri ambarı iş yükünüz için öncelik teknikleri analiz etmek için sorgu.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: f470670ae3d526f3b66badf219a01a471c24db0d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242234"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı iş yükünüzü çözümleme

Azure SQL veri ambarı iş yükünüzü Çözümleme yöntemleri.

## <a name="resource-classes"></a>Kaynak Sınıfları

SQL veri ambarı, sorgular için sistem kaynakları atamak için kaynak sınıfları sağlar.  Kaynak sınıfları hakkında daha fazla bilgi için bkz. [kaynak sınıfları ve iş yükü yönetimi](resource-classes-for-workload-management.md).  Sorguları bir sorgu için atanan kaynak sınıfı, şu anda mevcut olandan daha fazla kaynak gerekiyorsa bekler.

## <a name="queued-query-detection-and-other-dmvs"></a>Sıraya alınan sorgu algılama ve diğer Dmv'ler

Kullanabileceğiniz `sys.dm_pdw_exec_requests` DMV'sini eşzamanlılık kuyrukta bekleyen sorguları belirleyin. Sorgular için eşzamanlılık yuvası bekleniyor durumuna sahip **askıya**.

```sql
SELECT  r.[request_id]                           AS Request_ID
,       r.[status]                               AS Request_Status
,       r.[submit_time]                          AS Request_SubmitTime
,       r.[start_time]                           AS Request_StartTime
,       DATEDIFF(ms,[submit_time],[start_time])  AS Request_InitiateDuration_ms
,       r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r
;
```

İş yükünün yönetim rollerini görüntülenebilir `sys.database_principals`.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0
;
```

Aşağıdaki sorgu, her kullanıcıya atanan rolü gösterir.

```sql
SELECT  r.name AS role_principal_name
,       m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id      = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc')
;
```

SQL veri ambarı, aşağıdaki türleri bekleyin sahiptir:

* **LocalQueriesConcurrencyResourceType**: Eşzamanlılık yuvası çerçevenin dışında sit sorgular. DMV sorgular ve sistem işlevleri gibi `SELECT @@VERSION` yerel sorgular örnekleridir.
* **UserConcurrencyResourceType**: İçinde eşzamanlılık yuvası framework sit sorgular. Son kullanıcı tablolarda yürütülen sorgular, bu kaynak türü kullanacağınız örnekleri temsil eder.
* **DmsConcurrencyResourceType**: Veri taşıma işlemlerini kaynaklanan bekler.
* **BackupConcurrencyResourceType**: Bu bekleme, bir veritabanı yedekleniyor olduğunu gösterir. Bu kaynak türü için maksimum değeri 1'dir. Aynı zamanda, diğer birden çok yedekleme istenen, kuyruk. Genel olarak, 10 dakikalık ardışık anlık görüntü arasındaki en az bir süre öneririz. 

`sys.dm_pdw_waits` DMV, hangi kaynakların bir isteği bekliyor görmek için kullanılabilir.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]                                           AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,       SESSION_ID()                                       AS Current_session
,       s.[status]                                         AS Session_status
,       s.[login_name]
,       s.[query_count]
,       s.[client_id]
,       s.[sql_spid]
,       r.[command]                                        AS Request_command
,       r.[label]
,       r.[status]                                         AS Request_status
,       r.[submit_time]
,       r.[start_time]
,       r.[end_compile_time]
,       r.[end_time]
,       DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,       DATEDIFF(ms,r.[start_time],r.[end_compile_time])   AS Request_compile_time_ms
,       DATEDIFF(ms,r.[end_compile_time],r.[end_time])     AS Request_execution_time_ms
,       r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID()
;
```

`sys.dm_pdw_resource_waits` DMV, belirli bir sorgu için bekleme bilgileri gösterir. Kaynak sağlanması için kaynakları bekleme süresini saat ölçülerine bekleyin. Sinyal bekleme süresi CPU üzerinde sorgu zamanlamak temel alınan SQL sunucuları geçen süredir.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID()
;
```

Ayrıca `sys.dm_pdw_resource_waits` DMV hesaplamak kaç tane eşzamanlı kullanım hakkı verilir.

```sql
SELECT  SUM([concurrency_slots_used]) as total_granted_slots
FROM    sys.[dm_pdw_resource_waits]
WHERE   [state]           = 'Granted'
AND     [resource_class] is not null
AND     [session_id]     <> session_id()
;
```

`sys.dm_pdw_wait_stats` DMV bekler, geçmiş eğilim analizi için kullanılabilir.

```sql
SELECT   w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w
;
```

## <a name="next-steps"></a>Sonraki adımlar

Veritabanı kullanıcıları yönetme ve güvenlik hakkında daha fazla bilgi için bkz. [güvenli bir veritabanında SQL veri ambarı](sql-data-warehouse-overview-manage-security.md). Ne kadar büyük kaynak sınıfları hakkında daha fazla bilgi kümelenmiş columnstore dizini kalitesini artırmak, bkz [segment kalitesini artırmak için dizinlerini yeniden oluşturma](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality).
