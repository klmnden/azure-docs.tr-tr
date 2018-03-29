---
title: Azure veri ambarında geliştirmek için kaynaklar | Microsoft Docs
description: Geliştirme kavramları, tasarım kararları, öneriler ve SQL Data Warehouse için kodlama teknikleri.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: ''
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 03/22/2018
ms.author: jrj;barbkess
ms.openlocfilehash: c81a6f9af556a86f10d2482333fdc98f9cfb1454
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>SQL Data Warehouse için tasarım kararları ve kodlama teknikleri
SQL Data Warehouse için temel tasarım kararları, öneriler ve kodlama teknikleri daha iyi anlamak için bu geliştirme makalelere göz atın.

## <a name="key-design-decisions"></a>Temel tasarım kararları
Aşağıdaki makaleler, kavramlar ve SQL Data Warehouse kullanarak dağıtılmış veri ambarına geliştirmek için tasarım kararları vurgula:

* [Bağlantıları][connections]
* [Eşzamanlılık][concurrency]
* [İşlemler][transactions]
* [Kullanıcı tanımlı şemaları][user-defined schemas]
* [Tablo dağıtım][table distribution]
* [Tablo dizinlerini][table indexes]
* [Tablo bölümleri][table partitions]
* [CTAS][CTAS]
* [İstatistikleri][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Bu makaleler, belirli kodlama teknikleri, ipuçları ve öneriler SQL veri ambarı geliştirmek için vurgula:

* [Saklı yordamlar][stored procedures]
* [Etiketleri][labels]
* [Görünümler][views]
* [geçici tablolar][temporary tables]
* [Dinamik SQL][dynamic SQL]
* [döngü][looping]
* [Grup tarafından seçenekleri][group by options]
* [Değişken ataması][variable assignment]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgileri için bkz: [SQL veri ambarı T-SQL deyimlerini](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
