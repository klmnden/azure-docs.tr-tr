---
title: İş yükünüzü - Azure SQL veri ambarı çözümleme | Microsoft Docs
description: Azure SQL veri ambarı iş yükünüz için öncelik teknikleri analiz etmek için sorgu.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 9025eccabcbf7052131fee741a1e1f6a2139366b
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/31/2019
ms.locfileid: "55476766"
---
# <a name="analyze-your-workload-in-azure-sql-data-warehouse"></a>Azure SQL veri ambarı iş yükünüzü çözümleme
Azure SQL veri ambarı iş yükünüz için öncelik teknikleri analiz etmek için sorgu.

## <a name="workload-groups"></a>İş yükü grupları 
SQL veri ambarı iş yükü gruplarını kullanarak kaynak sınıfları uygular. Toplam sekiz iş yükü gruplarının çeşitli DWU boyutları arasında kaynak sınıfları davranışını denetleyen vardır. Her DWU için SQL veri ambarı yalnızca dört sekiz iş yükü gruplarını kullanır. Her iş yükü grubu dört kaynak sınıflardan birine atandığı için bu yaklaşım mantıklı: smallrc, mediumrc, largerc veya xlargerc. Bu iş yükü grupları bazıları için daha yüksek ayarlandığını iş yükü gruplarını anlama önemi olan *önem*. Önem derecesi için CPU kullanılan zamanlama. Üç kat daha fazla CPU çevrimleri Orta önem derecesiyle çalıştırılan sorguların daha yüksek önem derecesiyle çalıştırılan sorguların alın. Bu nedenle, eşzamanlılık yuvası eşlemeleri de CPU önceliği belirleyin. Sorgu en az 16 yuvaları kullanırken, yüksek öncelikli çalışır.

Aşağıdaki tabloda, her iş yükü grubu için önem eşlemeler gösterilmektedir.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>İş yükü grubu eşlemelere eşzamanlılık yuvaları ve önem derecesi

| İş yükü grupları | Eşzamanlılık yuvası eşleme | MB / dağıtım (esneklik) | MB / dağıtım (işlem) | Önem derecesi eşleme |
|:---------------:|:------------------------:|:------------------------------:|:---------------------------:|:------------------:|
| SloDWGroupC00   | 1                        |    100                         | 250                         | Orta             |
| SloDWGroupC01   | 2                        |    200                         | 500                         | Orta             |
| SloDWGroupC02   | 4                        |    400                         | 1000                        | Orta             |
| SloDWGroupC03   | 8                        |    800                         | 2000                        | Orta             |
| SloDWGroupC04   | 16                       |  1,600                         | 4000                        | Yüksek               |
| SloDWGroupC05   | 32                       |  3,200                         | 8000                        | Yüksek               |
| SloDWGroupC06   | 64                       |  6,400                         | 16,000                      | Yüksek               |
| SloDWGroupC07   | 128                      | 12,800                         | 32,000                      | Yüksek               |
| SloDWGroupC08   | 256                      | 25,600                         | 64,000                      | Yüksek               |

<!-- where are the allocation and consumption of concurrency slots charts? --> **Ayırma ve eşzamanlılık yuvaları tüketiminin** grafik gösterir bir DW500 kullanan 1, 4, 8 veya 16 eşzamanlılık yuvaları smallrc, mediumrc, largerc ve xlargerc, sırasıyla. Her kaynak sınıfı için önemini bulmak için bu değerleri önceki grafikte bakabilirsiniz.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>Önem derecesi için kaynak sınıfları DW500 eşleme
| Kaynak sınıfı | İş yükü grubu | Kullanılan eşzamanlılık yuvaları | MB / dağıtım | Önem derecesi |
|:-------------- |:-------------- |:----------------------:|:-----------------:|:---------- |
| smallrc        | SloDWGroupC00  | 1                      | 100               | Orta     |
| mediumrc       | SloDWGroupC02  | 4                      | 400               | Orta     |
| largerc        | SloDWGroupC03  | 8                      | 800               | Orta     |
| xlargerc       | SloDWGroupC04  | 16                     | 1,600             | Yüksek       |
| staticrc10     | SloDWGroupC00  | 1                      | 100               | Orta     |
| staticrc20     | SloDWGroupC01  | 2                      | 200               | Orta     |
| staticrc30     | SloDWGroupC02  | 4                      | 400               | Orta     |
| staticrc40     | SloDWGroupC03  | 8                      | 800               | Orta     |
| staticrc50     | SloDWGroupC03  | 16                     | 1,600             | Yüksek       |
| staticrc60     | SloDWGroupC03  | 16                     | 1,600             | Yüksek       |
| staticrc70     | SloDWGroupC03  | 16                     | 1,600             | Yüksek       |
| staticrc80     | SloDWGroupC03  | 16                     | 1,600             | Yüksek       |

## <a name="view-workload-groups"></a>İş yükü grupları görüntüleyin
Aşağıdaki sorguyu kaynak İdarecisi perspektifinden bellek kaynak ayırma ayrıntılarını gösterir. Bu, etkin ve geçmiş kullanımı iş yükü gruplarının sorunlarını giderirken çözümlemek için yararlıdır.

```sql
WITH rg
AS
(   SELECT  
     pn.name                                AS node_name
    ,pn.[type]                              AS node_type
    ,pn.pdw_node_id                         AS node_id
    ,rp.name                                AS pool_name
    ,rp.max_memory_kb*1.0/1024              AS pool_max_mem_MB
    ,wg.name                                AS group_name
    ,wg.importance                          AS group_importance
    ,wg.request_max_memory_grant_percent    AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                             AS group_max_dop
    ,wg.effective_max_dop                   AS group_effective_max_dop
    ,wg.total_request_count                 AS group_total_request_count
    ,wg.total_queued_request_count          AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
    AND     rp.name    = 'SloDWPool'
)
SELECT  pool_name
,       pool_max_mem_MB
,       group_name
,       group_importance
,       (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,       node_name
,       node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
        node_name
,       group_request_max_memory_grant_pcnt
,       group_importance
;
```

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
* **BackupConcurrencyResourceType**: Bu bekleme, bir veritabanı yedekleniyor olduğunu gösterir. Bu kaynak türü için maksimum değeri 1'dir. Aynı zamanda, diğer birden çok yedekleme istenen, kuyruk.

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

`sys.dm_pdw_resource_waits` DMV yalnızca belirli bir sorgu tarafından tüketilen kaynak bekler gösterir. Kaynak bekleme süresi yalnızca CPU üzerinde sorgu zamanlamak temel alınan SQL sunucuları için gereken süre anlamına gelmektedir sinyal bekleme süresi aksine sağlanması kaynaklar için bekleme süresini ölçer.

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


