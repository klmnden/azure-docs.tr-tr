---
title: Azure SQL veritabanı vCore tabanlı kaynak sınırları - tek veritabanı | Microsoft Docs
description: Bu sayfa, Azure SQL Database tek bir veritabanı için bazı ortak vCore tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 1a14e1a7c50f458067491a8605a0518056ac0aa8
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309900"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database-preview"></a>Azure SQL veritabanı vCore tabanlı modeli sınırları (Önizleme) tek bir veritabanı için satın alma

Bu makalede, Azure SQL Database tek veritabanları vCore tabanlı satın alma modeli kullanarak için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli sınırları için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Tek veritabanı: depolama boyutlarına ve performans düzeyleri

Tek veritabanları için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde tek bir veritabanı için kullanılabilir kaynakları gösterir. Hizmet katmanı, performans düzeyi ve kullanarak tek veritabanı için bir depolama miktarını ayarlayabilirsiniz [Azure portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-servers-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), veya [REST API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

#### <a name="generation-4-compute-platform"></a>Nesil 4 işlem platformu
|Performans düzeyi|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En büyük veri boyutu (GB)|1024|1024|1536|3072|4096|4096|
|En büyük günlük boyutu|307|307|461|922|1229|1229|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|000
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu
|Performans düzeyi|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En büyük veri boyutu (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|En büyük günlük boyutu|307|307|461|614|1229|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1|1|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı

#### <a name="generation-4-compute-platform"></a>Nesil 4 işlem platformu
|Performans düzeyi|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1|2|4|8|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|En büyük günlük boyutu|307|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|3|3|3|3|3|3|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>Nesil 5 işlem platformu
|Performans düzeyi|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|H/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|En büyük günlük boyutu|307|307|307|307|614|1229|1229|1229|
|TempDB size(DB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|3|3|3|3|3|3|3|3|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
