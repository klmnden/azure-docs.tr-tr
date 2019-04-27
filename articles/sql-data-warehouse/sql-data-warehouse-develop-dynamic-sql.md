---
title: Dinamik SQL kullanarak Azure SQL veri ambarı'nda | Microsoft Docs
description: Dinamik SQL Azure SQL veri ambarı'nda çözümleri geliştirmek için kullanma hakkında ipuçları.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: implement
ms.date: 04/17/2018
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: e1d209828313068e61b4acec3c4f92b7d643e1b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60402532"
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

