---
title: Bir veri ambarı'nda Azure geliştirme kaynakları | Microsoft Docs
description: Geliştirme kavramları, tasarım kararlarını, öneriler ve SQL veri ambarı için kodlama tekniklerini.
services: sql-data-warehouse
author: XiaoyuL-Preview
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: 613bcb05dab993989a2ae00b71fef95794953ab8
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65850728"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Tasarım kararlarını ve SQL veri ambarı için kodlama tekniklerini
SQL veri ambarı için temel tasarım kararlarına, öneriler ve kodlama tekniklerini daha iyi anlamak için bu geliştirme makalelere göz atın.

## <a name="key-design-decisions"></a>Temel tasarım kararları
Aşağıdaki makaleler kavramları ve SQL veri ambarı'nı kullanarak bir Dağıtılmış veri ambarı geliştirmeye yönelik tasarım kararları vurgular:

* [Bağlantıları][connections]
* [Eşzamanlılık][concurrency]
* [İşlemler][transactions]
* [Kullanıcı tanımlı şemalar][user-defined schemas]
* [Tablo dağıtımı][table distribution]
* [Tablo dizinleri][table indexes]
* [Tablo bölümleri][table partitions]
* [CTAS][CTAS]
* [İstatistikleri][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama tekniklerini
Bu makaleler, belirli bir kodlama tekniklerini, ipuçları ve SQL veri Ambarınızı geliştirmeye yönelik öneriler vurgular:

* [Saklı yordamlar][stored procedures]
* [etiketleri][labels]
* [Görünümler][views]
* [Geçici tablolar][temporary tables]
* [Dinamik SQL][dynamic SQL]
* [döngü][looping]
* [Gruplandırma seçenekleri][group by options]
* [değişken ataması][variable assignment]

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla başvuru bilgileri için bkz: [SQL veri ambarı T-SQL deyimleri](sql-data-warehouse-reference-tsql-statements.md).

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
