---
title: "SQL veri ambarı dinamik SQL'de | Microsoft Docs"
description: "Çözümleri geliştirme için Azure SQL Data Warehouse'da dinamik SQL kullanma ipuçları."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 29228676373aee8dbc7b1b2a7d92ffc978333804
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>SQL veri ambarı dinamik SQL
SQL Data Warehouse için uygulama kodu geliştirirken, esnek, genel ve modüler çözümler sunmak amacıyla dinamik sql kullanmanız gerekebilir. SQL veri ambarı blob veri türleri şu anda desteklemiyor. Varchar(max) ve nvarchar(max) türlerini BLOB türler gibi bu dizelerinizi boyutunu sınırlayabilir. Çok büyük dizeleri oluştururken, uygulama kodunuzda bu tür kullandıysanız, kodu parçalara Böl ve EXEC deyimi kullanmanız gerekir.

Basit bir örnek:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Dize kısaysa kullanabileceğiniz [sp_executesql] [ sp_executesql] normal olarak.

> [!NOTE]
> Yürütülen dinamik SQL deyimleri tüm TSQL doğrulama kurallarını tabi olmaya devam eder.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla geliştirme ipuçları için bkz: [geliştirmeye genel bakış][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
