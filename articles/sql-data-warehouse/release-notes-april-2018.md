---
title: Azure SQL veri ambarı sürüm notları Nisan 2018 | Microsoft Docs
description: Azure SQL Data Warehouse için sürüm notları.
services: sql-data-warehouse
author: twounder
manager: craigg-msft
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 05/28/2018
ms.author: twounder
ms.reviewer: twounder
ms.openlocfilehash: 49ffc3ddfd586964ae8a9688aeb48fffdd327b45
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738940"
---
# <a name="whats-new-in-azure-sql-data-warehouse-april-2018"></a>Azure SQL Data Warehouse (Nisan 2018) yenilikler nelerdir?
Azure SQL Data Warehouse sürekli olarak iyileştirmeler alır. Bu makalede, yeni özellikler ve Nisan 2018 sunulan değişiklikler açıklanmaktadır.

## <a name="features"></a>Özellikler

### <a name="ability-to-truncate-a-partition-before-a-switch"></a>Bir anahtar önce bir bölüm kesmek yeteneği
Müşteriler sık kullandığınız bölüm düzeni değiştirme tablo meta verileri değiştirerek verileri bir tablodan yüklemek için `ALTER TABLE SourceTable SWITCH PARTITION X TO TargetTable PARTITION X` sözdizimi. SQL veri ambarı hedef bölüm veri içerdiğinde bölüm değiştirme desteklemiyor. Hedef bölüm veri içeriyorsa, müşteri hedef bölüm kesmek ve geçişi gerçekleştirmek gerekir.

SQL veri ambarı artık tek bir T-SQL deyiminde bu işlemi destekler.

```sql
ALTER TABLE SourceTable 
    SWITCH PARTITION X TO TargetTable PARTITION X
    WITH (TRUNCATE_TARGET_PARTITION = ON)
```
Daha fazla bilgi için bkz: [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql) makalesi.

### <a name="improved-query-compilation-performance"></a>İyileştirilmiş sorgu derleme performansı
SQL veri ambarı dağıtılmış sorgular sorgu derleme adımında iyileştirmeye yönelik değişiklikler kümesi sunmuştur. Bu değişiklikler kadar sorgusu derleme sürelerini artırmaya **10 x** azaltma genel sorgu yürütme çalışma zamanları. Bu değişiklikleri veri ambarları büyük sayıda nesne (tablo, İşlevler, görünümler, yordamlar) ile hakkında daha fazla fark edilir.

## <a name="behavior-changes"></a>Davranış değişiklikleri

### <a name="dbcc-commands-do-not-consume-concurrency-slots"></a>DBCC komutu eşzamanlılık yuvaları tüketen değil
SQL veri ambarı T-SQL kümesini destekler [DBCC komutu](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-transact-sql) gibi [DBCC DROPCLEANBUFFERS](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-dropcleanbuffers-transact-sql). Daha önce bu komutları kullanılmasına neden olur bir [eşzamanlılık yuvası](https://docs.microsoft.com/azure/sql-data-warehouse/resource-classes-for-workload-management#concurrency-slots) yürütülebilir kullanıcı yükleri/sorgu sayısını azaltır. `DBCC` Komutları artık genel sorgu yürütme performansı iyileştirme bir kaynak yuva tüketen olmayan yerel bir sırada çalıştırın.

### <a name="updated-error-message-for-excessive-literals"></a>Aşırı değişmez değerleri için güncelleştirilmiş hata iletisi
Daha önce SQL veri ambarı içerir bir *yaklaşık* ne zaman bir sorgu çok fazla değişmez değerler içeriyor sayısı.
```
Msg 100086
Cannot have more than 20,000 literals in the query. The query contains [n] literals.
```

Hata iletisi, yalnızca değişmez değer sınırına ulaşıp belirtmek için güncelleştirilmiştir.
```
Msg 100086
The number of literals in the query is beyond the limit. Please rewrite your query.
```

Daha fazla bilgi için bkz: [sorguları](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#queries) bölümünü [kapasite limitlerini](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits) maksimum sınırları hakkında daha fazla ayrıntı için makalenin.

### <a name="removed-the-syspdwdatabasemappings-view"></a>SYS kaldırıldı. PDW_DATABASE_MAPPINGS görünümü
Bu `sys.pdw_database_mappings` görünümdür kullanılmayan SQL veri ambarı'nda. Daha önce bu görünümün bir SELECT sonuç döndürür. Görünüm kaldırılmıştır. 