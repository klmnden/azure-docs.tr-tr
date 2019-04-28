---
title: Duraklatma, sürdürme, Azure SQL veri ambarı'nda REST ile ölçeklendirme | Microsoft Docs
description: REST API'leri üzerinden SQL Data warehouse'daki işlem gücü yönetin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 03/29/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 658b35163e20d024118bc7a3142c86614540f00c
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62105335"
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için REST API'leri
Azure SQL veri ambarı'nda işlem yönetmek için REST API'ler.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme
Veri ambarı birimlerini değiştirmek için kullanın [oluşturma veya güncelleştirme veritabanı](/rest/api/sql/databases/createorupdate) REST API. Aşağıdaki örnek, veri ambarı birimleri MyServer sunucuda barındırılan MySQLDW, veritabanı için DW1000 için ayarlar. ResourceGroup1 adlı bir Azure kaynak grubunda sunucusudur.

```
PATCH https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01-preview HTTP/1.1
Content-Type: application/json; charset=UTF-8

{
    "properties": {
        "requestedServiceObjectiveName": DW1000
    }
}
```

## <a name="pause-compute"></a>Duraklatma işlem

Bir veritabanı duraklatmak için kullanmak [veritabanı duraklatma](/rest/api/sql/databases/pause) REST API. Aşağıdaki örnekte adlı Database02 Server01 adlı bir sunucuda barındırılan bir veritabanı duraklatır. ResourceGroup1 adlı bir Azure kaynak grubunda sunucusudur.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>İşlem devam et

Bir veritabanına başlamak için kullanmak [veritabanını sürdürme](/rest/api/sql/databases/resume) REST API. Aşağıdaki örnekte adlı Database02 Server01 adlı bir sunucuda barındırılan bir veritabanı başlatır. ResourceGroup1 adlı bir Azure kaynak grubunda sunucusudur. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Veritabanı durumunu kontrol edin

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```

## <a name="get-maintenance-schedule"></a>Bakım zamanlamasını Al
Veri ambarı için ayarlanan bakım zamanlaması denetleyin. 

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

```

## <a name="set-maintenance-schedule"></a>Bakım zamanlaması Ayarla
Ayarlayın ve var olan bir veri ambarı maintnenance zamanlamada güncelleştirmek için.

```
PUT https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/maintenanceWindows/current?maintenanceWindowName=current&api-version=2017-10-01-preview HTTP/1.1

{
    "properties": {
        "timeRanges": [
                {
                                "dayOfWeek": Saturday,
                                "startTime": 00:00,
                                "duration": 08:00,
                },
                {
                                "dayOfWeek": Wednesday
                                "startTime": 00:00,
                                "duration": 08:00,
                }
                ]
    }
}

```


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [işlemleri yönetme](sql-data-warehouse-manage-compute-overview.md).

