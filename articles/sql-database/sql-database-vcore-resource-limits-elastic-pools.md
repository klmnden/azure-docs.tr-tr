---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanındaki elastik havuzlar için bazı ortak sanal çekirdek tabanlı kaynak sınırları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: carlrab
manager: craigg
ms.date: 04/22/2019
ms.openlocfilehash: 7f3afec0425033fba174e000195fa26b295aaef1
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65507963"
---
# <a name="resource-limits-for-elastic-pools-using-the-vcore-based-purchasing-model-limits"></a>Sanal çekirdek tabanlı satın alma modeli sınırlarını kullanarak elastik havuzlar için kaynak sınırları

Bu makalede, Azure SQL veritabanı elastik havuzları ve sanal çekirdek tabanlı satın alma modeli kullanarak havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli limitleri için bkz. [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

Bir hizmet katmanı, işlem boyutu ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), veya [REST API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!IMPORTANT]
> Bkz. yönergeler ve önemli noktalar ölçekleme için [elastik havuzu ölçekleme](sql-database-elastic-pool-scale.md)
> [!NOTE]
> Kaynak elastik havuzlardaki veritabanlarını tek tek genellikle tek veritabanları aynı havuzları dışında aynı boyutu işlem limitlerdir. Örneğin, maks. eş zamanlı çalışan GP_Gen4_1 veritabanı için 200 çalışanları olur. Bu nedenle, GP_Gen4_1 havuzdaki bir veritabanı için en fazla eş zamanlı çalışan var. Ayrıca 200 çalışanları 210 olduğuna dikkat edin. eş zamanlı çalışan GP_Gen4_1 havuzundaki toplam sayısı.

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Genel amaçlı hizmet katmanı: Depolama boyutlarına ve işlem boyutları

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: 4. nesil işlem platformu (1. bölüm)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|512|756|756|1536|1536|1536|
|Maksimum günlük boyutu|154|227|227|461|461|461|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|500|1000|1500|2000|2500|3000|
|Günlük oran sınırları (MBps)|4.6875|9.375|14.0625|18.75|23.4375|28.125|
|Maks. eş zamanlı çalışan (istek) havuz başına * |210|420|630|840|1050|1260|
|Havuz başına maks. eş zamanlı oturum * |210|420|630|840|1050|1260|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|100|200|300|500|500|500|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Çoğaltma sayısı|1|1.|1.|1.|1.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: 4. nesil işlem platformu (2. bölüm)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|1536|2048|2048|2048|3584|4096|
|Maksimum günlük boyutu (GB)|461|614|614|614|1075|1229|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|3500|4000|4500|5000|7000|7000|
|Günlük oran sınırları (MBps)|32.8125|37.5|37.5|37.5|37.5|37.5|
|Maks. eş zamanlı çalışan (istek) havuz başına *|1470|1680|1890|2100|3360|5040|
|En fazla eşzamanlı oturum açma havuzu (istek) *|1470|1680|1890|2100|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|200|500|500|500|500|500|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Çoğaltma sayısı|1|1.|1.|1.|1.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: 5. nesil işlem platformu (1. bölüm)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|512|756|756|1536|1536|1536|
|Maksimum günlük boyutu (GB)|154|227|227|461|461|461|461|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Günlük oran sınırları (MBps)|4.6875|9.375|14.0625|18.75|23.4375|28.125|32.8125|
|Maks. eş zamanlı çalışan (istek) havuz başına *|210|420|630|840|1050|1260|1470|
|(İstek) havuz başına maks. eş zamanlı oturum *|210|420|630|840|1050|1260|1470|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|200|500|500|500|500|500|500|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Çoğaltma sayısı|1|1.|1.|1.|1.|1.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: 5. nesil işlem platformu (2. bölüm)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|2048|2048|3072|3072|4096|4096|4096|
|Maksimum günlük boyutu (GB)|614|614|922|922|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Günlük oran sınırları (MBps)|37.5|37.5|37.5|37.5|37.5|37.5|37.5|
|Maks. eş zamanlı çalışan (istek) havuz başına *|1680|1890|2100|2520|33600|4200|8400|
|(İstek) havuz başına maks. eş zamanlı oturum *|1680|1890|2100|2520|33600|4200|8400|
|Havuz başına en fazla veritabanı|500|500|500|500|500|500|500|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...16, 24, 32, 40|0, 0.25, 0.5, 1...16, 24, 32, 40, 80|
|Çoğaltma sayısı|1|1.|1.|1.|1.|1.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

## <a name="business-critical-service-tier-storage-sizes-and-compute-sizes"></a>İş kritik hizmet katmanı: Depolama boyutlarına ve işlem boyutları

### <a name="business-critical-service-tier-generation-4-compute-platform-part-1"></a>İş kritik hizmet katmanı: 4. nesil işlem platformu (1. bölüm)

|İşlem boyutu|BC_Gen4_1|BC_Gen4_2|BC_Gen4_3|BC_Gen4_4|BC_Gen4_5|BC_Gen4_6|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|1|2|3|4|5|6|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (GB)|650|650|650|650|650|650|
|Maksimum günlük boyutu (GB)|195|195|195|195|195|195|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef IOPS (64 KB)|5000|10000|15000|20000|25000|30000|
|Günlük oran sınırları (MBps)|10|20|30|40|50|60|
|Maks. eş zamanlı çalışan (istek) havuz başına *|210|420|630|840|1050|1260|
|(İstek) havuz başına maks. eş zamanlı oturum *|210|420|630|840|1050|1260|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|Yalnızca tek veritabanları için bu işlem boyutu desteklenir|50|100|100|100|100|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|Yok|0, 0.25, 0.5, 1, 2|0, 0.25, 0.5, 1...3|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...5|0, 0.25, 0.5, 1...6|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş kritik hizmet katmanı: 4. nesil işlem platformu (2. bölüm)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|81.6|91.8|102|122.4|163.2|204|
|Columnstore desteği|Yok|Yok|Yok|Yok|Yok|Yok|
|Bellek içi OLTP depolama alanı (GB)|7|8|9.5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (GB)|650|650|650|650|1024|1024|
|Maksimum günlük boyutu (GB)|195|195|195|195|307|307|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef IOPS (64 KB)|35000|40000|45000|50000|80000|120000|
|Günlük oran sınırları (MBps)|70|80|80|80|80|80|
|Maks. eş zamanlı çalışan (istek) havuz başına *|1470|1680|1890|2100|3360|5040|
|(İstek) havuz başına maks. eş zamanlı oturum *|1470|1680|1890|2100|3360|5040|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|100|100|100|100|100|100|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1...7|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...9|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...10, 16|0, 0.25, 0.5, 1...10, 16, 24|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş kritik hizmet katmanı: 5. nesil işlem platformu (1. bölüm)

|İşlem boyutu|BC_Gen5_2|BC_Gen5_4|BC_Gen5_6|BC_Gen5_8|BC_Gen5_10|BC_Gen5_12|BC_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|1.571|3.142|4.713|6.284|8.655|11.026|13.397|
|En yüksek veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maksimum günlük boyutu (GB)|307|307|307|461|461|922|922|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef IOPS (64 KB)|5000|10000|15000|20000|25000|30000|35000|
|Günlük oran sınırları (MBps)|15|30|45|60|75|90|105|
|Maks. eş zamanlı çalışan (istek) havuz başına *|210|420|630|840|1050|1260|1470|
|(İstek) havuz başına maks. eş zamanlı oturum *|210|420|630|840|1050|1260|1470|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|Yalnızca tek veritabanları için bu işlem boyutu desteklenir|50|100|100|100|100|100|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|Yok|0, 0.25, 0.5, 1...4|0, 0.25, 0.5, 1...6|0, 0.25, 0.5, 1...8|0, 0.25, 0.5, 1...10|0, 0.25, 0.5, 1...12|0, 0.25, 0.5, 1...14|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

#### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş kritik hizmet katmanı: 5. nesil işlem platformu (2. bölüm)

|İşlem boyutu|BC_Gen5_16|BC_Gen5_18|BC_Gen5_20|BC_Gen5_24|BC_Gen5_32|BC_Gen5_40|BC_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|15.768|18.139|20.51|25.252|37.936|52.22|131.64|
|En yüksek veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef IOPS (64 KB)|40000|45000|50000|60000|80000|100000|200000|
|Günlük oran sınırları (MBps)|120|120|120|120|120|120|120|
|Maks. eş zamanlı çalışan (istek) havuz başına *|1680|1890|2100|2520|3360|4200|8400|
|(İstek) havuz başına maks. eş zamanlı oturum *|1680|1890|2100|2520|3360|4200|8400|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Havuz başına en fazla veritabanı|100|100|100|100|100|100|100|
|Veritabanı başına en düşük/en yüksek elastik havuz sanal çekirdek seçenekleri|0, 0.25, 0.5, 1...16|0, 0.25, 0.5, 1...18|0, 0.25, 0.5, 1...20|0, 0.25, 0.5, 1...20, 24|0, 0.25, 0.5, 1...20, 24, 32|0, 0.25, 0.5, 1...20, 24, 32, 40|0, 0.25, 0.5, 1...20, 24, 32, 40, 80|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

\* En fazla eş zamanlı çalışan (istek) herhangi bir tek veritabanı için bkz. [tek veritabanı kaynak limitleri](sql-database-vcore-resource-limits-single-databases.md). Örneğin, elastik havuzun 5. nesil ve veritabanı başına en fazla kendi sanal çekirdek kullanıyorsa, maks. eş zamanlı çalışan sonra 200 2 ' dir.  Veritabanı başına en çok sanal çekirdek 0,5 ardından maks. eş zamanlı çalışan ise, 50 üzerinde 5. nesil olduğundan sanal çekirdek başına 100 eşzamanlı çalışan en fazla.  Diğer en çok sanal çekirdek ayarlarını daha az 1 sanal çekirdek olan veritabanı başına ya da daha az, en fazla eş zamanlı çalışan sayısı için benzer şekilde içerik yeniden ölçeklenir.

Tüm sanal çekirdek, bir elastik havuzun meşgul ise, havuzdaki her bir veritabanı eşit miktarda sorguları işlemek için işlem kaynaklarını alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, herhangi bir kaynak yoksa veritabanı başına en düşük vCore değeri, sıfır olmayan bir değere ayarlandığında her veritabanı için garanti edilen miktarda'ek niteliğindedir.

## <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri tanımlar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en yüksek sanal çekirdekler |Havuzdaki diğer veritabanlarının tarafından kullanılabilir kullanım oranlarına dayalı, herhangi bir veritabanı havuzu kullanabiliriz sanal çekirdek sayısı. Veritabanı başına en yüksek Vcore bir veritabanı için kaynak garantisi değil. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımı yükündeki yüksek veritabanı başına en yüksek Vcore ayarlayın. Havuz genellikle sıcak ve soğuk kullanım modellerini varsaydığından aşırı işleme, belirli bir ölçüde burada tüm veritabanlarını değil aynı anda en üst seviyeye çıkan bekleniyor.|
| Veritabanı başına en düşük Vcore |Herhangi bir havuzda veritabanı sanal çekirdek sayısı alt sınırını garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına minimum sanal çekirdekler 0 olarak ayarlayın ve aynı zamanda varsayılan değerdir. Bu özellik 0 ile veritabanı başına ortalama çekirdek kullanımı arasında herhangi bir yere ayarlanır. Havuz başına sanal çekirdekler ürün veritabanları havuz ve veritabanı başına en az çekirdek sayısını aşamaz.|
| Veritabanı başına maks. depolama |Bir havuzda bir veritabanı için kullanıcı tarafından ayarlanmış maksimum veritabanı boyutu. Havuza alınmış veritabanları, bir veritabanı ulaşabilirsiniz boyutu sınırlıdır bu nedenle ayrılmış havuz depolama alanını paylaşır küçük kalan havuz depolama ve veritabanı boyutu. Maks. veritabanı boyutu, veri dosyaları için en büyük boyutunu ifade eder ve günlük dosyalarının kullandığı alanı içermez. |
|||

## <a name="next-steps"></a>Sonraki adımlar

- Tek bir veritabanı için sanal çekirdek kaynak limitleri için bkz. [sanal çekirdek tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](sql-database-vcore-resource-limits-single-databases.md)
- Tek veritabanının DTU kaynak limitleri için bkz. [DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md)
- Elastik havuzlar için DTU kaynak limitleri için bkz. [DTU tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md)
- Yönetilen örnek için kaynak limitleri için bkz [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Bir veritabanı sunucusunda kaynak sınırları hakkında daha fazla bilgi için bkz [kaynak sınırları üzerinde bir SQL veritabanı sunucusuna genel bakış](sql-database-resource-limits-database-server.md) sunucu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
