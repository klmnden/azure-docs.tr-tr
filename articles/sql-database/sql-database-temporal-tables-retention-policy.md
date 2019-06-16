---
title: Zamana bağlı tablolarda geçmiş verilerin bekletme ilkesi ile yönetme | Microsoft Docs
description: Geçmiş verileri denetiminiz altında tutmak için zamana bağlı bekletme ilkesini kullanmayı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 09/25/2018
ms.openlocfilehash: 62e88d912c55015f87cc00f21527010ad01ee00c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60615708"
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Zamana bağlı tablolarda geçmiş verilerin bekletme ilkesi ile yönetme

Zamana bağlı tablolarda, özellikle daha uzun bir süre için geçmiş verileri tutuyorsanız normal tablolar, birden çok veritabanı boyutu artırabilir. Bu nedenle, geçmiş verileri için bekletme ilkesini planlama ve her zamana bağlı tablo yaşam döngüsü yönetimi önemli bir yönüdür ' dir. Zamana bağlı tablolarda Azure SQL veritabanı'nda, bu görevi gerçekleştirmenize yardımcı olan kullanımı kolay bekletme mekanizması ile gelir.

Zamana bağlı geçmiş saklama olabilir tek tek tablo düzeyinde yapılandırılır, kullanıcıların esnek eskime oluşturmasına olanak tanıyan ilkeler. Zamana bağlı bekletme uygulama basittir: Tablo oluşturma veya şema değişikliği sırasında ayarlamak için yalnızca bir parametresi gerektirir.

Bekletme İlkesi tanımladıktan sonra Azure SQL veritabanı otomatik veri temizleme için uygun geçmiş satırlar olup olmadığını düzenli olarak denetimini başlatır. Eşleşen satır tanımlayıcısı ve geçmiş tablosu, kaldırma, zamanlanan ve sistem tarafından çalıştırın arka plan görevinin saydam bir şekilde oluşur. Geçerlilik süresi geçmiş tablo satırları ULUN system_tıme süre sonunu temsil eden bir sütuna göre denetlenir. Saklama dönemi, örneğin, altı ay ayarlarsanız, temizleme için uygun olan tablo satırları aşağıdaki koşul karşılar:

```
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```

Önceki örnekte, biz, kabul **ValidTo** sütun system_tıme süre sonuna karşılık gelir.

## <a name="how-to-configure-retention-policy"></a>Bekletme İlkesi yapılandırma

Zamana bağlı tablo için bekletme ilkesini yapılandırmadan önce öncelikle zamana bağlı geçmiş saklama etkin olup olmadığını denetleyin *veritabanı düzeyinde*.

```
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Veritabanı bayrağı **is_temporal_history_retention_enabled** varsayılan olarak açık olarak ayarlandı, ancak kullanıcılar, ALTER DATABASE deyimi ile bunu değiştirebilirsiniz. OFF sonra da otomatik olarak ayarlanmış [zaman noktasına](sql-database-recovery-using-backups.md) işlemi. Veritabanınız için zamana bağlı geçmiş saklama temizleme etkinleştirmek için aşağıdaki deyimi yürütün:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

> [!IMPORTANT]
> Bekletme durumunda bile zamana bağlı tablolar için yapılandırabileceğiniz **is_temporal_history_retention_enabled** Kapalı'dır, ancak eski satırlar için otomatik temizleme değil tetiklenir durumda.

Bekletme İlkesi öğesini INFINITE olarak ayarlamayı parametresi için değer belirleyerek tablo oluşturma sırasında yapılandırılır:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
```

Azure SQL veritabanı Bekletme dönemi farklı zaman birimlerini kullanarak belirtmenize olanak sağlar: GÜN, hafta, ay ve yıl. Öğesini INFINITE olarak ayarlamayı atlanırsa, SINIRSIZ bekletme varsayılır. SONSUZ anahtar sözcüğü açıkça de kullanabilirsiniz.

Bazı senaryolarda tablo oluşturulduktan sonra saklamayı yapılandırmak isteyebilirsiniz veya değer daha önce değiştirmek için yapılandırılmış. Bu durumda, ALTER TABLE deyimini kullanın:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

> [!IMPORTANT]
> System_versıonıng öğesini OFF olarak ayarlanıyor *korumak değil* Bekletme dönemi değeri. Öğesini INFINITE olarak ayarlamayı, SINIRSIZ bekletme süresi sonuçları açıkça belirtilen olmadan system_versıonıng ON olarak ayarlanamadı.

Bekletme İlkesi geçerli durumunu gözden geçirmek için tek tek tablolar için bekletme süreleri olan veritabanı düzeyinde zamana bağlı bekletme etkinleştirme bayrağını birleştiren aşağıdaki sorguyu kullanın:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```


## <a name="how-sql-database-deletes-aged-rows"></a>SQL veritabanı eski satırları nasıl siler

Temizleme işlemi geçmiş tablosu üzerinde dizin düzeni bağlıdır. Dikkat edin önemlidir *yalnızca geçmiş tablosu kümelenmiş bir dizin (B-ağacı veya columnstore) ile yapılandırılmış sınırlı saklama ilkesi olabilir*. Bir arka plan görevi, sınırlı saklama süresi ile tüm zamana bağlı tablolar için eski verileri temizleme işlemini gerçekleştirmek için oluşturulur.
Temizleme mantığı rowstore (B-Ağacı) kümelenmiş dizini için (en fazla 10 K) daha küçük öbekler halinde eski satır siler baskı veritabanının günlük ve g/ç alt en aza indirir. Temizleme mantığı kullanır ancak B-Ağacı dizini, saklama dönemi sıkıca garanti edilemez daha eski satırlar için silme sırasını gereklidir. Bu nedenle, *bağımlılığın uygulamalarınızı temizleme sırayla almayan*.

Kümelenmiş columnstore için temizleme görevini tüm kaldırır [satır grupları](https://msdn.microsoft.com/library/gg492088.aspx) tek seferde (genellikle 1 milyon satır her, özellikle geçmiş verileri yüksek bir hızda oluşturulduğunda çok verimli olan içerir).

![Kümelenmiş columnstore bekletme](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

İş yükünüz yüksek miktarda geçmiş verileri hızlı bir şekilde oluşturduğunda columnstore dizini senaryolar için mükemmel bir seçim kümelenmiş, mükemmel veri sıkıştırma ve verimli saklama temizleme yapar. Bu yoğun için tipik bir desendir [zamana bağlı tablolar kullanan bir işlem gerçekleştirme iş yüklerinde](https://msdn.microsoft.com/library/mt631669.aspx) değişiklik izleme ve denetleme, eğilim analizi ve IOT veri alımı.

## <a name="index-considerations"></a>Dizin konuları

Dizin, kümelenmiş rowstore dizini ile tablolar için temizleme görevini system_tıme süre sonuna karşılık gelen sütun ile başlamasını gerektirir. Böyle bir dizin yoksa, sınırlı saklama süresi yapılandıramazsınız:

*Msg 13765, Level 16, State 1 <br> </br> sınırlı saklama süresi ayarlanamadı 'temporalstagetestdb.dbo.WebsiteUserInfo' sistem sürümü tutulan zamana bağlı tablo üzerinde olduğundan geçmiş tablosu ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' gerekli kümelenmiş dizini içermiyor. Kümelenmiş columnstore veya B-Ağacı dizini system_tıme süresinin sonuyla eşleşen sütunla başlayan oluşturmayı göz önünde bulundurun geçmiş tablosunda, dönem.*

Azure SQL veritabanı tarafından önceden oluşturulmuş varsayılan geçmiş tablosu için bekletme ilkesi uyumlu dizin, kümelenmiş olduğuna dikkat edin önemlidir. Bu dizin tablosunda sınırlı saklama süresi ile kaldırmaya işlemi şu hatayla başarısız olur:

*Msg 13766, Level 16, State 1 <br> </br> eski verileri otomatik olarak temizleme için kullanıldığından 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' kümelenmiş dizini bırakılamıyor. Bu dizini bırakmanız gerekiyorsa sistem sürümü tutulan ilgili zamana bağlı tablo üzerinde INFINITE öğesini INFINITE olarak ayarlamayı ayarlamayı göz önünde bulundurun.*

Geçmiş tablosu SYSTEM_VERSIONIOING mekanizması tarafından özel olarak doldurulduğunda her zaman söz konusu olduğu (dönem sütunu sonunda sıralı) artan düzende, geçmiş satırlar eklenir temizleme üzerinde kümelenmiş columnstore dizini en iyi şekilde çalışır. Geçmiş tablosu satır sonuna kadar (var olan geçmiş verileri geçirdiyseniz, durum olabilecek) dönem sütunu sıralı değil, doğru en iyi elde etmek için sıralanır B-Ağacı rowstore dizini üzerinde kümelenmiş columnstore dizini yeniden oluşturmanız gerekir performans.

Geçmiş tablosunda sınırlı saklama süresi ile kümelenmiş columnstore dizinini yeniden oluşturmayı kaçının, çünkü doğal olarak sistem sürümü oluşturma işlemi tarafından uygulanan satır gruplarında sırası değişebilir. Geçmiş tablosunda kümelenmiş columnstore dizini yeniden oluşturmanız gerekiyorsa, bu normal veri temizleme için gereken satır grupları olarak sıralama koruma uyumlu B-Ağacı dizini üzerinde yeniden oluşturmanız gerekir. Sütun dizini garantili veri sırası olmadan kümelenmiş mevcut geçmiş tablosunda zamana bağlı tablo oluşturursanız, aynı yaklaşımı gerçekleştirilmelidir:

```sql
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

Sınırlı saklama süresi, kümelenmiş columnstore dizini olan geçmiş tablosu için yapılandırıldığında, o tablosunda kümelenmemiş ek B-Ağacı dizinleri oluşturulamıyor:

```sql
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
```

Deyimi yürütme girişimi şu hatayla başarısız olur:

*Msg 13772, Level 16, State 1 <br> </br> sınırlı saklama süresi ve kümelenmiş columnstore dizini tanımlı olduğundan zamana bağlı geçmiş tablosunda bulunan 'WebsiteUserInfoHistory' kümelenmiş dizini oluşturulamıyor.*

## <a name="querying-tables-with-retention-policy"></a>Bekletme İlkesi olan tabloları sorgulama

Tüm sorguların zamana bağlı tablosunda sınırlı saklama İlkesi, eski satırları temizleme görevi tarafından silinebilir olduğundan tahmin edilemez ve tutarsız sonuçlar önlemek için eşleşen geçmiş satırlar otomatik olarak filtrelemek *herhangi bir noktada süresi ve rasgele Sipariş*.

Aşağıdaki resimde, basit bir sorgu için sorgu planı gösterilmiştir:

```sql
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
```

Sorgu planı dönem sütunu (ValidTo) sonuna kadar uygulanan ek filtresi (vurgulu) geçmiş tablosunda kümelenmiş dizin tarama işleci içerir. Bu örnek, bir aylık bekletme süresi WebsiteUserInfo tablosunda ayarlandı varsayar.

![Bekletme sorgu filtresi](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Ancak, geçmiş tablosu doğrudan sorgularsanız, dönem ancak herhangi bir garanti olmadan tekrarlanabilir sorgu sonuçları için belirtilen bekletme değerinden daha eski olan satırları görebilirsiniz. Aşağıdaki resimde, sorgu için sorgu yürütme planı uygulanan ek filtreler geçmiş tablosunda gösterilmektedir:

![Geçmiş saklama filtresi olmadan sorgulama](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

İş mantığınızı, tutarsız veya beklenmeyen sonuçlar alabilirsiniz gibi geçmiş tablosu saklama süresi ötesinde okuma güvenmeyin. Zamana bağlı sorgular zamana bağlı tablolardaki verileri çözümlemek için FOR system_tıme yan tümcesiyle birlikte kullanmanızı öneririz.

## <a name="point-in-time-restore-considerations"></a>İşaret zaman geri yükleme konuları

Yeni bir veritabanı oluşturduğunuzda, [var olan veritabanı zaman içinde belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md), veritabanı düzeyinde devre dışı zamana bağlı bekletme sahiptir. (**is_temporal_history_retention_enabled** bayrağı OFF olarak ayarlayın). Bu işlev, tüm geçmiş satırları geri yükleme sırasında bunları sorgulamak ulaşmadan önce eski satır kaldırılır endişelenmeden incelemenize olanak tanır. Bunu kullanabilirsiniz *ötesinde bir yapılandırılmış elde tutma süresi geçmiş verileri İnceleme*.

Zamana bağlı tablo, bir aylık Bekletme dönemi belirtilen olduğunu varsayalım. Premium hizmet katmanında veritabanınız oluşturulmuş olsa bile, 35 günde geri geçmişte yedekleme veritabanı durumu ile veritabanı kopyası oluşturmak mümkün olacaktır. Etkili bir şekilde geçmiş tablosu doğrudan sorgulayarak 65 güne kadar eski olan geçmiş satırlar analiz çalıştırmasına olanak tanır.

Zamana bağlı bekletme temizleme etkinleştirmek istiyorsanız, zaman geri yükleme noktası sonra aşağıdaki Transact-SQL deyimi çalıştırın:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

## <a name="next-steps"></a>Sonraki adımlar

Zamana bağlı tablolarda, uygulamalarınızda kullanılacak öğrenmek için kullanıma [zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama](sql-database-temporal-tables.md).

Dinlemek için Channel 9 ziyaret bir [gerçek müşteri zamana bağlı uygulama başarı hikayesi](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ve izleme bir [zamana bağlı tanıtım canlı](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Zamana bağlı tablolar hakkında ayrıntılı bilgi için gözden [MSDN belgelerine](https://msdn.microsoft.com/library/dn935015.aspx).
