---
title: Dinamik SQL kullanarak Azure SQL veri ambarı'nda | Microsoft Docs
description: Dinamik SQL Azure SQL veri ambarı'nda çözümleri geliştirmek için kullanma hakkında ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 6793fba1476595918ac20c0484a661e3af7897d7
ms.sourcegitcommit: 2b2129fa6413230cf35ac18ff386d40d1e8d0677
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2018
ms.locfileid: "43247309"
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL veri ambarı'nda dinamik SQL
Dinamik SQL Azure SQL veri ambarı'nda çözümleri geliştirmek için kullanma hakkında ipuçları.

## <a name="dynamic-sql-example"></a>Dinamik SQL örneği

SQL Data Warehouse için uygulama kodu geliştirirken, dinamik sql esnek, genel ve modüler çözümler sunmak amacıyla kullanmak gerekebilir. SQL veri ambarı şu anda blob veri türlerini desteklemez. Blob veri türleri desteklemediğinden, varchar(max) hem de nvarchar(max) türlerini BLOB veri türleri dahil olduğundan dizelerinizi boyutunu sınırlayabilir. Büyük dizeleri oluşturmak için uygulama kodunuzda bu tür kullandıysanız, kod öbeklere ayırma ve EXEC deyimi kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Kısa bir dize ise, kullanabileceğiniz [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) normal olarak.

> [!NOTE]
> Dinamik SQL yürütülen deyimleri, tüm TSQL doğrulama kuralları tabi olmaya devam edecektir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

