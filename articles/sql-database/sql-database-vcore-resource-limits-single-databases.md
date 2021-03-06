---
title: Azure SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - tek veritabanı | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı'nda tek bir veritabanı için bazı ortak sanal çekirdek tabanlı kaynak sınırları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/22/2019
ms.openlocfilehash: c89aa3b4ecf0c07cfbb579cdc18fac6e822bc047
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536229"
---
# <a name="resource-limits-for-single-databases-using-the-vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları

Bu makalede ayrıntılı kaynak sınırları sanal çekirdek tabanlı satın alma modelini kullanarak Azure SQL veritabanı tek veritabanı sağlar.

DTU tabanlı satın alma modeli sınırları için bir SQL veritabanı sunucusunda tek veritabanları için bkz: [kaynak bakış sınırlayan bir SQL veritabanı sunucusunda](sql-database-resource-limits-database-server.md).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

Hizmet katmanı, işlem boyutu ve depolama alanı miktarı kullanarak tek veritabanı için ayarlayabileceğiniz [Azure portalında](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](sql-database-single-databases-manage.md#transact-sql-manage-sql-database-servers-and-single-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-sql-database-servers-and-single-databases), [ Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-sql-database-servers-and-single-databases), veya [REST API](sql-database-single-databases-manage.md#rest-api-manage-sql-database-servers-and-single-databases).

> [!IMPORTANT]
> Bkz. yönergeler ve önemli noktalar ölçekleme için [tek bir veritabanının ölçeğini](sql-database-single-database-scale.md).

## <a name="general-purpose-service-tier-storage-sizes-and-compute-sizes"></a>Genel amaçlı hizmet katmanı: Depolama boyutlarına ve işlem boyutları

> [!IMPORTANT]
> Yeni 4. nesil veritabanları, AustraliaEast bölgesinde artık desteklenmemektedir.

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: 4. nesil işlem platformu (1. bölüm)

|İşlem boyutu|GP_Gen4_1|GP_Gen4_2|GP_Gen4_3|GP_Gen4_4|GP_Gen4_5|GP_Gen4_6
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|1|2|3|4|5|6|
|Bellek (GB)|7|14|21|28|35|42|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|1024|1024|1024|1536|1536|1536|
|Maksimum günlük boyutu (GB)|307|307|307|461|461|461|
|TempDB boyutu (GB)|32|64|96|128|160|192|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|500|1000|1500|2000|2500|3000|
|Günlük oran sınırları (MBps)|3.75|7.5|11.25|15|18.75|22.5|
|Maks. eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1\.|1\.|1\.|1\.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="general-purpose-service-tier-generation-4-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: 4. nesil işlem platformu (2. bölüm)

|İşlem boyutu|GP_Gen4_7|GP_Gen4_8|GP_Gen4_9|GP_Gen4_10|GP_Gen4_16|GP_Gen4_24
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|1536|3072|3072|3072|4096|4096|
|Maksimum günlük boyutu (GB)|461|922|922|922|1229|1229|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)
|Hedef IOPS (64 KB)|3500|4000|4500|5000|7000|7000|
|Günlük oran sınırları (MBps)|26.25|30|30|30|30|30|
|Maks. eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1\.|1\.|1\.|1\.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-1"></a>Genel amaçlı hizmet katmanı: 5. nesil işlem platformu (1. bölüm)

|İşlem boyutu|GP_Gen5_2|GP_Gen5_4|GP_Gen5_6|GP_Gen5_8|GP_Gen5_10|GP_Gen5_12|GP_Gen5_14|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|6|8|10|12|14|
|Bellek (GB)|10.2|20.4|30.6|40.8|51|61.2|71.4|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|1024|1024|1536|1536|1536|3072|3072|
|Maksimum günlük boyutu (GB)|307|307|307|461|461|461|461|
|TempDB boyutu (GB)|64|128|192|256|320|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|1000|2000|3000|4000|5000|6000|7000|
|Günlük oran sınırları (MBps)|3.75|7.5|11.25|15|18.75|22.5|26.25|
|Maks. eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1\.|1\.|1\.|1\.|1\.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="general-purpose-service-tier-generation-5-compute-platform-part-2"></a>Genel amaçlı hizmet katmanı: 5. nesil işlem platformu (2. bölüm)

|İşlem boyutu|GP_Gen5_16|GP_Gen5_18|GP_Gen5_20|GP_Gen5_24|GP_Gen5_32|GP_Gen5_40|GP_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|
|Sanal çekirdekler|16|18|20|24|32|40|80|
|Bellek (GB)|81.6|91.8|102|122.4|163.2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|3072|3072|3072|4096|4096|4096|4096|
|Maksimum günlük boyutu (GB)|922|922|922|1229|1229|1229|1229|
|TempDB boyutu (GB)|384|384|384|384|384|384|384|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|7000|7000|7000|7000|7000|7000|7000|
|Günlük oran sınırları (MBps)|30|30|30|30|30|30|30|
|Maks. eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|1|1\.|1\.|1\.|1\.|1\.|1|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="serverless-compute-tier"></a>Sunucusuz işlem katmanı

[Sunucusuz bilgi işlem katmanı](sql-database-serverless.md) Önizleme aşamasındadır ve yalnızca sanal çekirdek satın kullanarak tek veritabanı modeli aranır.

#### <a name="generation-5-compute-platform"></a>5\. nesil işlem platformu

|İşlem boyutu|GP_S_Gen5_1|GP_S_Gen5_2|GP_S_Gen5_4|
|:--- | --: |--: |--: |
|S/W oluşturma|5|5|5|
|Min-Maks sanal çekirdekler|0.5-1|0.5-2|0.5-4|
|Min-Maks bellek (GB)|2.02-3|2.05-6|2.10-12|
|Min otomatik duraklatma gecikme (saat)|6|6|6|
|Columnstore desteği|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|
|En yüksek veri boyutu (GB)|512|1024|1024|
|Maksimum günlük boyutu (GB)|12|24|48|
|TempDB boyutu (GB)|32|64|128|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|GÇ gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|Hedef IOPS (64 KB)|500|1000|2000|
|Günlük oran sınırları (MBps)|2,5|5.6|10|
|Maks. eş zamanlı çalışan (istek)|75|150|300|
|İzin verilen maks. oturumları|30000|30000|30000|
|Çoğaltma sayısı|1|1\.|1|
|Çok AZ|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Yok|Yok|Yok|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

## <a name="business-critical-service-tier-for-provisioned-compute-tier"></a>Sağlanan işlem katmanı için iş kritik hizmet katmanı

> [!IMPORTANT]
> Yeni 4. nesil veritabanları, AustraliaEast bölgesinde artık desteklenmemektedir.

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
|Günlük oran sınırları (MBps)|8|16|24|32|40|48|
|Maks. eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="business-critical-service-tier-generation-4-compute-platform-part-2"></a>İş kritik hizmet katmanı: 4. nesil işlem platformu (2. bölüm)

|İşlem boyutu|BC_Gen4_7|BC_Gen4_8|BC_Gen4_9|BC_Gen4_10|BC_Gen4_16|BC_Gen4_24|
|:--- | --: |--: |--: |--: |--: |--: |
|S/W oluşturma|4|4|4|4|4|4|
|Sanal çekirdekler|7|8|9|10|16|24|
|Bellek (GB)|49|56|63|70|112|168|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|7|8|9.5|11|20|36|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|En yüksek veri boyutu (GB)|650|650|650|650|1024|1024|
|Maksimum günlük boyutu (GB)|195|195|195|195|307|307|
|TempDB boyutu (GB)|224|256|288|320|384|384|
|GÇ gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|1-2 ms (yazma)<br>1-2 ms (okuma)|
|Hedef IOPS (64 KB)|35000|40000|45000|50000|80000|120000|
|Günlük oran sınırları (MBps)|56|64|64|64|64|64|
|Maks. eş zamanlı çalışan (istek)|1400|1600|1800|2000|3200|4800|
|En fazla eşzamanlı oturum açma (istek)|1400|1600|1800|2000|3200|4800|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-1"></a>İş kritik hizmet katmanı: 5. nesil işlem platformu (1. bölüm)

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
|Hedef IOPS (64 KB)|8000|16000|24000|32000|40000|48000|56000|
|Günlük oran sınırları (MBps)|12|24|36|48|60|72|84|
|Maks. eş zamanlı çalışan (istek)|200|400|600|800|1000|1200|1400|
|Maks. eş zamanlı oturum|200|400|600|800|1000|1200|1400|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

### <a name="business-critical-service-tier-generation-5-compute-platform-part-2"></a>İş kritik hizmet katmanı: 5. nesil işlem platformu (2. bölüm)

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
|Hedef IOPS (64 KB)|64000|72000|80000|96000|128000|160000|320000|
|Günlük oran sınırları (MBps)|96|96|96|96|96|96|96|
|Maks. eş zamanlı çalışan (istek)|1600|1800|2000|2400|3200|4000|8000|
|Maks. eş zamanlı oturum|1600|1800|2000|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|4|4|4|4|4|4|4|
|Çok AZ|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|

## <a name="hyperscale-service-tier"></a>Hiper ölçekli hizmet katmanı

### <a name="generation-5-compute-platform"></a>5\. nesil işlem platformu

|Performans düzeyi|HS_Gen5_2|HS_Gen5_4|HS_Gen5_8|HS_Gen5_16|HS_Gen5_24|HS_Gen5_32|HS_Gen5_40|HS_Gen5_80|
|:--- | --: |--: |--: |--: |---: | --: |--: |--: |
|S/W oluşturma|5|5|5|5|5|5|5|5|
|Sanal çekirdekler|2|4|8|16|24|32|40|80|
|Bellek (GB)|10.2|20.4|40.8|81.6|122.4|163.2|204|408|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama alanı (GB)|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|En yüksek veri boyutu (TB)|100 |100 |100 |100 |100 |100 |100 |100 |
|Maksimum günlük boyutu (TB)|1 |1\. |1\. |1\. |1\. |1\. |1\. |1 |
|TempDB boyutu (GB)|64|128|256|384|384|384|384|384|
|Depolama türü|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|Yerel SSD|
|Hedef IOPS (64 KB)| [Not 1](#note-1) |[Not 1](#note-1)|[Not 1](#note-1) |[Not 1](#note-1) |[Not 1](#note-1) |[Not 1](#note-1) |[Not 1](#note-1) | [Not 1](#note-1) |
|GÇ gecikmesi (yaklaşık)|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|Belirlenecek|
|Maks. eş zamanlı çalışan (istek)|200|400|800|1600|2400|3200|4000|8000|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|30000|30000|30000|
|Çoğaltma sayısı|2|2|2|2|2|2|2|2|
|Çok AZ|Yok|Yok|Yok|Yok|Yok|Yok|Yok|Yok|
|Ölçek genişletme okuyun|Evet|Evet|Evet|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama alanı dahil |7|7|7|7|7|7|7|7|
|||

### <a name="note-1"></a>Not 1

Hiper ölçekli birden çok düzeyde önbelleğe alma ile çok katmanlı bir mimari var. Etkili IOPS, iş yüküne bağlıdır.

### <a name="next-steps"></a>Sonraki adımlar

- Tek veritabanının DTU kaynak limitleri için bkz. [DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları](sql-database-dtu-resource-limits-single-databases.md)
- Elastik havuzlar için sanal çekirdek kaynak limitleri için bkz. [sanal çekirdek tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-vcore-resource-limits-elastic-pools.md)
- Elastik havuzlar için DTU kaynak limitleri için bkz. [DTU tabanlı satın alma modeli kullanarak elastik havuzlar için kaynak sınırları](sql-database-dtu-resource-limits-elastic-pools.md)
- Yönetilen örnek için kaynak limitleri için bkz [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md).
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Bir veritabanı sunucusunda kaynak sınırları hakkında daha fazla bilgi için bkz [kaynak sınırları üzerinde bir SQL veritabanı sunucusuna genel bakış](sql-database-resource-limits-database-server.md) sunucu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
