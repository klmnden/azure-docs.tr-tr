---
title: Duraklatma, sürdürme, ölçeklendirme ile Azure SQL Data Warehouse, KALAN | Microsoft Docs
description: İşlem gücü SQL Data warehouse'da REST API'leri aracılığıyla yönetin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 21423d69bf2cf06bcd208082ce492bf5dd038e29
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="rest-apis-for-azure-sql-data-warehouse"></a>Azure SQL veri ambarı için REST API'leri
REST API'ları yönetmek için Azure SQL Data Warehouse'da işlem.

## <a name="scale-compute"></a>Hesaplamayı ölçeklendirme
Data warehouse birimleri değiştirmek için kullanın [oluşturma veya güncelleştirme veritabanı](/rest/api/sql/databases/createorupdate) REST API. Aşağıdaki örnek data warehouse birimleri için DW1000 MyServer sunucusunda barındırılan MySQLDW veritabanı için ayarlar. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

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

Bir veritabanı duraklatmak için kullanmak [duraklatma veritabanı](/rest/api/sql/databases/pause) REST API. Aşağıdaki örnek Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanı duraklatır. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor.

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/pause?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="resume-compute"></a>Resume işlem

Bir veritabanı başlatmak için kullanmak [Sürdür veritabanı](/rest/api/sql/databases/resume) REST API. Aşağıdaki örnek Server01 adlı bir sunucuda barındırılan Database02 adlı bir veritabanı başlatır. Sunucu ResourceGroup1 adlı bir Azure kaynak grubunda yer alıyor. 

```
POST https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}/resume?api-version=2014-04-01-preview HTTP/1.1
```

## <a name="check-database-state"></a>Veritabanı durumunu kontrol edin

```
GET https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Sql/servers/{server-name}/databases/{database-name}?api-version=2014-04-01 HTTP/1.1
```


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için bkz: [işlem yönetmek](sql-data-warehouse-manage-compute-overview.md).

