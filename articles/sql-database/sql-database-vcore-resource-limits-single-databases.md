---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - tek veritabanı | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı'nda tek bir veritabanı için bazı ortak sanal çekirdek tabanlı kaynak sınırları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: CarlRabeler
ms.author: carlrab
ms.reviewer: ''
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 5f0e5de7503d06d1aff319434d763d3b034053b3
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166387"
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-for-a-single-database"></a>Azure SQL veritabanı sanal çekirdek tabanlı model sınırları tek bir veritabanı için satın alma

Bu makalede ayrıntılı kaynak sınırları sanal çekirdek tabanlı satın alma modelini kullanarak Azure SQL veritabanı tek veritabanı sağlar.

DTU tabanlı satın alma modeli limitleri için bkz. [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).


## <a name="single-database-storage-sizes-and-compute-sizes"></a>Tek veritabanı: depolama boyutlarına ve işlem boyutları

Tek veritabanları için aşağıdaki tablolarda her hizmet katmanında tek bir veritabanı için kullanılabilir kaynakları göster ve işlem boyutu. Hizmet katmanı, işlem boyutu ve depolama alanı miktarı kullanarak tek veritabanı için ayarlayabileceğiniz [Azure portalında](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), veya [REST API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

#### <a name="generation-4-compute-platform"></a>4. nesil işlem platformu
|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En yüksek veri boyutu (GB)|1024|1024|1536|3072|4096|4096|
|Maksimum günlük boyutu (GB)|307|307|461|922|1229|1229|
|TempDB boyutu (GB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|7000|7000|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1|1|1|1|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|000
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>5. nesil işlem platformu
|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_8|GP_Gen5_16|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40| GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En yüksek veri boyutu (GB)|1024|1024|1536|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|307|307|461|614|1229|1229|1229|1229|
|TempDB boyutu (GB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|500|1000|2000|4000|6000|7000|7000|7000|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1|1|1|1|1|1|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı

#### <a name="generation-4-compute-platform"></a>4. nesil işlem platformu
|İşlem boyutu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Yok|Yok|Yok|Yok|Yok|Yok|
|Bellek içi OLTP depolama alanı (GB)|1|2|4|8|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (GB)|1024|1024|1024|1024|1024|1024|
|Maksimum günlük boyutu (GB)|307|307|307|307|307|307|
|TempDB boyutu (GB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|80000|120000|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|3|3|3|3|3|3|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

#### <a name="generation-5-compute-platform"></a>5. nesil işlem platformu
|İşlem boyutu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_8|BC_Gen5_16|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|1.571|3,142|6.284|15.768|25.252|37.936|52.22|131.64|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|En yüksek veri boyutu (GB)|1024|1024|1024|1024|2048|4096|4096|4096|
|Maksimum günlük boyutu (GB)|307|307|307|307|614|1229|1229|1229|
|TempDB boyutu (GB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|5000|10000|20000|40000|60000|80000|100000|200000
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|3|3|3|3|3|3|3|3|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="hyperscale-service-tier-preview"></a>Hiper ölçekli hizmet Katmanı (Önizleme)

#### <a name="generation-4-compute-platform"></a>4. nesil işlem platformu
|Performans düzeyi|HS_Gen4_1|HS_Gen4_2|HS_Gen4_4|HS_Gen4_8|HS_Gen4_16|HS_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|4|8|16|24|
|Bellek (GB)|7|14|28|56|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (TB)|100 |100 |100 |100 |100 |100 |
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |
|TempDB boyutu (GB)|32|64|128|256|384|384|
|Hedef IOPS (64 KB)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|GÇ gecikmesi (yaklaşık)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|2|2|2|2|2|2|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|7|7|7|7|7|7|
|||
### <a name="generation-5-compute-platform"></a>5. nesil işlem platformu
|Performans düzeyi|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |--: |--: |--: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|11|22|44|88|132|176|220|440|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maksimum günlük boyutu (TB)|1 |1 |1 |1 |1 |1 |1 |1 |
|TempDB boyutu (GB)|64|128|256|384|384|384|384|384|
|Hedef IOPS (64 KB)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|GÇ gecikmesi (yaklaşık)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|2|2|2|2|2|2|2|2|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Dahil edilen yedekleme depolama alanı (sınırlı Önizleme)|7|7|7|7|7|7|7|7|
|||

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
