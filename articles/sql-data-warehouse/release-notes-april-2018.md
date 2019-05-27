---
title: Nisan 2018'den Azure SQL veri ambarı sürüm notları | Microsoft Docs
description: Azure SQL veri ambarı için sürüm notları.
services: sql-data-warehouse
author: anumjs
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/23/2018
ms.author: anjangsh
ms.reviewer: jrasnick
ms.openlocfilehash: 5fd2d5e2022cc1cf552ee7b525dcc484cc718f1f
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65912249"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>Azure SQL veri ambarı'nda yenilikler nelerdir? Nisan 2018
Azure SQL veri ambarı, sürekli olarak iyileştirmeler alır. Bu makalede, Nisan 2018'de sunulan değişiklikler ve yeni özellikleri açıklar.

## <a name="ability-to-truncate-a-partition-before-a-switch"></a>Önce bir anahtar bir bölüm Truncate olanağı
Müşterilerin kullandığı bölüm düzeni olarak değiştirme bir tablonun meta verilerini değiştirerek verileri bir tablodan diğerine yüklemek için `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X` söz dizimi. SQL veri ambarı, hedef bölüm verilerini içerdiğinde bölüm geçişi desteklemez. Hedef bölüm veri içeriyorsa, müşteri hedef bölüm Kes ve sonra geçişi gerçekleştirmek gerekir.

SQL veri ambarı, bu işlem tek bir T-SQL deyiminde artık desteklemektedir.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Daha fazla bilgi için [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) makalesi.

## <a name="improved-query-compilation-performance"></a>Gelişmiş sorgu derleme performansı
SQL veri ambarı bir dizi sorgu derleme adımı dağıtılmış sorgular, iyileştirmeye yönelik değişiklikler getirmiştir. Bu değişiklikler sorgu derleme sürelerini en fazla geliştirme **10 x** azaltarak genel sorgu yürütme çalışma zamanları. Bu değişiklikler veri ambarları ile çok sayıda nesnelerini (tablolar, işlevleri, görünümleri, yordamları) üzerinde daha belirgin olur.

## <a name="dbcc-commands-do-not-consume-concurrency-slots-behavior-change"></a>DBCC komutları eşzamanlılık yuvaları (davranış değişikliği) kullanmayan
SQL veri ambarı, T-SQL kümesini destekler [DBCC komutları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) gibi [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Daha önce bu komutların kullanılmasına neden olur bir [eşzamanlılık yuvası](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) yürütülebilir kullanıcı yükleri/sorguların sayısını azaltır. `DBCC` Komutları artık genel sorgu yürütme performansı iyileştirme kaynak yuva kullanmayan yerel bir kuyrukta çalıştırın.

## <a name="updated-error-message-for-excessive-literals-behavior-change"></a>Güncelleştirilmiş hata iletisi aşırı sabit değerleri (davranış değişikliği)
Daha önce SQL veri ambarı verilebilir bir *yaklaşık* sayısı ne zaman bir sorgu çok fazla değişmez değerler içeriyor.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

Hata iletisi, yalnızca sabit değer sınırı isabet belirtmek için güncelleştirildi.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Daha fazla bilgi için [sorguları](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) bölümünü [kapasite sınırları](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) makalenin üst sınırlar hakkında daha fazla ayrıntı için.

## <a name="removed-the-syspdwdatabasemappings-view-behavior-change"></a>SYS kaldırıldı. PDW_DATABASE_MAPPINGS görünümü (davranış değişikliği)
Bu `sys.pdw_database_mappings` görünümdür kullanılmayan SQL veri ambarı'nda. Daha önce bu görünümün SELECT sonuç döndürür. Görünümü kaldırıldı. 

## <a name="next-steps"></a>Sonraki adımlar
SQL veri ambarı hakkında biraz bilmek, bilgi nasıl hızlı bir şekilde [SQL veri ambarı oluşturma][create a SQL Data Warehouse]. Azure'da yeniyseniz yeni terimlerle karşılaşabileceğinizi için [Azure sözlüğünü][Azure glossary] yararlı bulabilirsiniz. Alternatif olarak, aşağıdaki diğer SQL Veri Ambarı Kaynakları’na göz atın.  

* [Müşteri başarı hikayeleri]
* [Bloglar]
* [Özellik istekleri]
* [Videolar]
* [Müşteri Danışma Ekibi blogları]
* [Stack Overflow forumu]
* [Twitter]


[Bloglar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Müşteri Danışma Ekibi blogları]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Müşteri başarı hikayeleri]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Özellik istekleri]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow forumu]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videolar]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
