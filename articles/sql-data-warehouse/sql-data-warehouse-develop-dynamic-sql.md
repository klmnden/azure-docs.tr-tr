---
title: Dinamik SQL Azure SQL Data Warehouse'da kullanarak | Microsoft Docs
description: Çözümleri geliştirme için Azure SQL Data Warehouse'da dinamik SQL kullanma ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: implement
ms.date: 04/12/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 260c8b69cbe783c2cf18e40669fe742867ab133d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL veri ambarı dinamik SQL
Çözümleri geliştirme için Azure SQL Data Warehouse'da dinamik SQL kullanma ipuçları.

## <a name="dynamic-sql-example"></a>Dinamik SQL örneği

Uygulama kodu SQL Data Warehouse için geliştirirken, esnek, genel ve modüler çözümler sunmak amacıyla dinamik sql kullanmanız gerekebilir. SQL veri ambarı blob veri türleri şu anda desteklemiyor. Varchar(max) ve nvarchar(max) türlerini BLOB veri türleri dahil olduğundan blob veri türleri desteklemediğinden dizelerinizi boyutunu sınırlayabilir. Büyük veri dizeleri oluşturmak için uygulama kodunuzda bu tür kullandıysanız, kodu parçalara Böl ve EXEC deyimi kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa, kullanabileceğiniz [sp_executesql](/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql) normal olarak.

> [!NOTE]
> Yürütülen dinamik SQL deyimleri tüm TSQL doğrulama kurallarını tabi olmaya devam eder.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış](sql-data-warehouse-overview-develop.md).

