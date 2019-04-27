---
title: İzleme iş yükü - Azure portalı | Microsoft Docs
description: Azure portalını kullanarak Azure SQL veri ambarı'nı izleme
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/22/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 6c8ce090039e3d5cc85c86d920710294de2165f9
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60748927"
---
# <a name="monitor-workload---azure-portal"></a>İzleme iş yükü - Azure portalı

Bu makalede, iş yükünüzü izleme için Azure portalını kullanmayı açıklar. Bu ayar için log analytics kullanarak sorgu yürütme ve iş yükü eğilimleri araştırmak amacıyla Azure İzleyicisi günlükleri içerir [Azure SQL veri ambarı](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/).

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Azure SQL veri ambarı: Biz, SQL veri ambarı için günlüklerinin toplanması. Sağlanan SQL veri ambarı yoksa, yönergelere bakın [SQL veri ambarı oluşturma](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-tutorial).

## <a name="create-a-log-analytics-workspace"></a>Log Analytics çalışma alanı oluşturma

Log Analytics çalışma alanları için Gözat dikey penceresine gidin ve bir çalışma alanı oluşturma 

![Log Analytics çalışma alanları](media/sql-data-warehouse-monitor/log_analytics_workspaces.png)

![Analytics çalışma alanı Ekle](media/sql-data-warehouse-monitor/add_analytics_workspace.png)

![Analytics çalışma alanı Ekle](media/sql-data-warehouse-monitor/add_analytics_workspace_2.png)

Aşağıdaki çalışma alanları hakkında daha fazla bilgi için ziyaret [belgeleri](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#create-a-workspace).

## <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini açın 

SQL veri ambarınızın günlüklerinden yaymak için tanılama ayarlarını yapılandırın. Veri ambarınız SQL veri ambarı için Dmv'leri sorun giderme en yaygın olarak kullanılan performans eşdeğer telemetri görünümlerini günlükleri oluşur. Şu anda aşağıdaki görünümleri desteklenmektedir:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?view=aps-pdw-2016-au7)


![Tanılama günlüklerini etkinleştirme](media/sql-data-warehouse-monitor/enable_diagnostic_logs.png)

Günlükleri, Azure depolama, Stream Analytics ve Log Analytics için yayılabilir. Bu öğreticide, Log Analytics seçin.

![Günlükleri belirtin](media/sql-data-warehouse-monitor/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Log Analytics karşı sorguları çalıştırma

Burada aşağıdakileri yapabilirsiniz Log Analytics çalışma alanınıza gidin:

- Günlük sorgular kullanarak günlükleri analiz edin ve yeniden kullanmak üzere sorguları Kaydet
- Yeniden kullanmak üzere sorguları Kaydet
- Günlüğü uyarıları oluşturma
- Panoya Sabitle sorgu sonuçları

Aşağıdaki günlük sorguları özellikleri hakkında ayrıntılı bilgi için ziyaret [belgeleri](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).

![Log Analytics çalışma alanı Düzenleyicisi](media/sql-data-warehouse-monitor/log_analytics_workspace_editor.png)



![Günlük analizi çalışma alanı sorguları](media/sql-data-warehouse-monitor/log_analytics_workspace_queries.png)

## <a name="sample-log-queries"></a>Örnek günlük sorguları



```Kusto
//List all queries 
AzureDiagnostics
| where Category contains "ExecRequests"
| project TimeGenerated, StartTime_t, EndTime_t, Status_s, Command_s, ResourceClass_s, duration=datetime_diff('millisecond',EndTime_t, StartTime_t)
```
```Kusto
//Chart the most active resource classes
AzureDiagnostics
| where Category contains "ExecRequests"
| where Status_s == "Completed"
| summarize totalQueries = dcount(RequestId_s) by ResourceClass_s
| render barchart 
```
```Kusto
//Count of all queued queries
AzureDiagnostics
| where Category contains "waits" 
| where Type_s == "UserConcurrencyResourceType"
| summarize totalQueuedQueries = dcount(RequestId_s)
```
## <a name="next-steps"></a>Sonraki adımlar

Ayarlama ve Azure İzleyici günlüklerine yapılandırılmış [Azure panoları özelleştirin](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) ekibinizle paylaşma.
