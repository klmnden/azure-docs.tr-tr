---
title: Duraklatma, sürdürme, Azure SQL veri ambarı'nda REST ile ölçeklendirme | Microsoft Docs
description: REST API'leri üzerinden SQL Data warehouse'daki işlem gücü yönetin.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 8db4d5cb69b65e60cd77d85d743798168bc6d813
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43300842"
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


## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için [işlemleri yönetme](sql-data-warehouse-manage-compute-overview.md).

