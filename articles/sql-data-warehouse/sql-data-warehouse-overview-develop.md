---
title: "Azure veri ambarında geliştirmek için kaynaklar | Microsoft Docs"
description: "Geliştirme kavramları, tasarım kararları, öneriler ve SQL Data Warehouse için kodlama teknikleri."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: b85a4f09e561e429aa5bf46ec680014487fb40c7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>SQL Data Warehouse için tasarım kararları ve kodlama teknikleri
SQL Data Warehouse için temel tasarım kararları, öneriler ve kodlama teknikleri daha iyi anlamak için bu geliştirme makalelere göz atın.

## <a name="key-design-decisions"></a>Temel tasarım kararları
Aşağıdaki makaleler bazı temel kavramları ve SQL Data Warehouse kullanarak dağıtılmış veri ambarınız geliştirme için anlamak için gereken tasarım kararlarını vurgula:

* [bağlantıları][connections]
* [Eşzamanlılık][concurrency]
* [işlemler][transactions]
* [Kullanıcı tanımlı şemaları][user-defined schemas]
* [Tablo dağıtım][table distribution]
* [Tablo dizinlerini][table indexes]
* [Tablo bölümleri][table partitions]
* [CTAS][CTAS]
* [İstatistikleri][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Geliştirme önerileri ve kodlama teknikleri
Bu makaleler, belirli kodlama teknikleri, ipuçları ve öneriler SQL veri ambarı geliştirmek için vurgula:

* [saklı yordamlar][stored procedures]
* [etiketleri][labels]
* [Görünümler][views]
* [geçici tablolar][temporary tables]
* [dinamik SQL][dynamic SQL]
* [döngü][looping]
* [Gruplandırma seçenekleri][group by options]
* [değişken ataması][variable assignment]

## <a name="next-steps"></a>Sonraki adımlar
Geliştirme makalelerine işlendikten sonra aracılığıyla göz [Transact-SQL Başvurusu] [ Transact-SQL reference] SQL Data Warehouse için desteklenen sözdizimi hakkında daha fazla ayrıntı için sayfa.

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
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
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
