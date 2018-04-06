---
title: Zamana bağlı tablolarda geçmiş verilerin Bekletme İlkesi'yle yönetme | Microsoft Docs
description: Zamana bağlı bekletme ilkesi denetiminiz altında geçmiş verileri korumak için nasıl kullanılacağını öğrenin.
services: sql-database
author: bonova
manager: craigg
ms.service: sql-database
ms.custom: develop databases
ms.topic: article
ms.date: 10/12/2016
ms.author: bonova
ms.openlocfilehash: 1ebfab93c94c27de8e765ac3f8278372c8f0690f
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Zamana bağlı tablolarda geçmiş verilerin bekletme ilkesi ile yönetme
Zamana bağlı tablolarda özellikle uzun bir süre için geçmiş verileri tut normal tablolardaki'birden fazla veritabanı boyutunu artırabilir. Bu nedenle, geçmiş verileri için bekletme ilkesi planlama ve her zamana bağlı tablo yaşam döngüsü yönetiminden önemli bir yönü ' dir. Azure SQL veritabanı zamana bağlı tablolarda, bu görevi gerçekleştirmenize yardımcı olan kullanımı kolay bekletme mekanizmasıyla sunulur.

Zamana bağlı geçmiş saklama olabilir tek tek tablo düzeyinde yapılandırılan, kullanıcıların esnek eskime oluşturmasına olanak veren ilkeler. Zamana bağlı bekletme uygulama basittir: Tablo oluşturma veya şema değişikliği sırasında ayarlamak için yalnızca bir parametre gerektirir.

Bekletme İlkesi tanımladıktan sonra Azure SQL veritabanı düzenli aralıklarla otomatik veri temizleme için uygun olan geçmiş satır olup olmadığını denetleme başlatır. Saydam, eşleşen satırları tanımlaması ve bunların kaldırılması geçmiş tablosundan zamanlanmış ve sistem tarafından çalıştırılan arka plan görevi oluşur. Geçmiş tablo satır için yaş koşul system_tıme süre sonunu temsil eden sütun göre denetlenir. Saklama dönemi, örneğin, altı ay ayarlarsanız, aşağıdaki iki koşul Temizleme için uygun tablo satırları karşılar:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

Önceki örnekte, biz, kabul **ValidTo** sütun system_tıme dönemi sonuna karşılık gelir.

## <a name="how-to-configure-retention-policy"></a>Bekletme İlkesi yapılandırmak nasıl?
Bekletme İlkesi zamana bağlı tablo için yapılandırmadan önce ilk zamana bağlı geçmiş saklama etkinleştirilip etkinleştirilmediğini kontrol *veritabanı düzeyinde*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Veritabanı bayrağı **is_temporal_history_retention_enabled** varsayılan olarak açık olarak ayarlanmış, ancak kullanıcılar, ALTER DATABASE deyimi ile değiştirebilir. OFF sonra da otomatik olarak ayarlanmış [geri yükleme noktası](sql-database-recovery-using-backups.md) işlemi. Zamana bağlı geçmiş saklama temizleme veritabanınız için etkinleştirmek için şu deyimi yürütün:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> Bekletme olsa bile zamana bağlı tablolar için yapılandırabileceğiniz **is_temporal_history_retention_enabled** Kapalı'dır, ancak eski satırlar için otomatik temizleme değil tetiklenir bu durumda.
> 
> 

Bekletme İlkesi HISTORY_RETENTION_PERIOD parametresinin değeri belirterek tablo oluşturma sırasında yapılandırılır:

````
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
````

Azure SQL veritabanı Bekletme dönemi farklı zaman birimleri kullanarak belirtmenize olanak verir: gün, hafta, ay ve yıl. HISTORY_RETENTION_PERIOD atlanırsa, SONSUZ bekletme varsayılır. SONSUZ anahtar sözcüğü açıkça de kullanabilirsiniz.

Bazı senaryolarda bekletme tablo oluşturulduktan sonra yapılandırmak istediğiniz veya önceden değiştirmek için değer yapılandırılmamış. Bu durumda, ALTER TABLE deyimini kullanın:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> Kapalı system_versıonıng *değil korumak* Bekletme dönemi değeri. HISTORY_RETENTION_PERIOD sonuçları saklama süresi SONSUZ içinde açıkça belirtilen olmadan system_versıonıng ON olarak ayarlanamadı.
> 
> 

Bekletme İlkesi geçerli durumunu gözden geçirmek için tek tek tablolar için bekletme süreleri ile veritabanı düzeyinde zamana bağlı bekletme etkinleştirme bayrağını katıldığı aşağıdaki sorguyu kullanın:

````
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
````


## <a name="how-sql-database-deletes-aged-rows"></a>SQL veritabanı nasıl siler satırları eski?
Temizleme işlemi geçmiş tablosu üzerinde dizin düzeni bağlıdır. Dikkat önemlidir *yalnızca geçmiş tabloları kümelenmiş bir dizin (B-ağacı veya columnstore) ile sınırlı bekletme ilkesi yapılandırılmış olabilir*. Bir arka plan görevi sınırlı bekletme süresi ile tüm zamana bağlı tablolar için eski verileri temizlenmesini için oluşturulur.
Rowstore (B-Ağacı) kümelenmiş dizini için temizleme mantığı siler (en fazla 10 K) daha küçük parçalara eski satırda veritabanı günlüğü ve g/ç alt Basıncı en aza indirir. Temizleme mantığı kullanır ancak B-Ağacı dizini, silme saklama dönemi sıkıca garanti edilemez daha eski satırların sırasını gereklidir. Bu nedenle, *uygulamalarınızı temizleme sırayla üzerinde hiçbir bağımlılık almayan*.

Kümelenmiş columnstore temizleme görevi tüm kaldırır [satır grupları](https://msdn.microsoft.com/library/gg492088.aspx) aynı anda (genellikle 1 milyon satır her, özellikle geçmiş verileri yüksek hızı oluşturulduğunda çok verimli olduğu içerir).

![Kümelenmiş columnstore bekletme](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

İş yükünüzün yüksek miktarda geçmiş verileri hızlı bir şekilde oluşturduğunda mükemmel veri sıkıştırma ve verimli bekletme temizleme yapar columnstore dizini senaryoları için mükemmel bir seçim kümelenmiş. Bu desene yoğun için tipik [zamana bağlı tablolarda kullanmak işlemsel işleme iş yüklerinin](https://msdn.microsoft.com/library/mt631669.aspx) değişiklik izleme ve denetleme, eğilim analizi veya IOT veri alımı için.

## <a name="index-considerations"></a>Dizin konuları
Temizleme görevini rowstore kümelenmiş dizine sahip tablolar için dizin başlaması system_tıme dönemi sonuna karşılık gelen sütun gerektirir. Bu tür dizin yoksa, sonlu saklama dönemi yapılandıramazsınız:

*Msg 13765, Level 16, State 1 <br> </br> sonlu saklama dönemi ayarlama başarısız oldu: 'temporalstagetestdb.dbo.WebsiteUserInfo' sistem sürümü tutulan zamana bağlı tablo üzerinde olduğundan geçmiş tablosu ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' gerekli kümelenmiş dizin içermiyor. Kümelenmiş columnstore veya system_tıme sonuna eşleşen sütundan başlayarak B-Ağacı dizini oluşturmayı düşünün süresi geçmiş tablosu üzerinde.*

Azure SQL veritabanı tarafından önceden oluşturulmuş varsayılan geçmiş tablosu için bekletme ilkesi uyumlu dizin kümelenmiş fark önemlidir. Bu dizin sonlu saklama süresi olan bir tabloda kaldırmaya işlemi şu hata ile başarısız olur:

*Msg 13766, Level 16, State 1 <br> </br> eski verileri otomatik temizleme için kullanıldığından 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' kümelenmiş dizini bırakılamıyor. Bu dizini bırakın gerekiyorsa HISTORY_RETENTION_PERIOD karşılık gelen sistem sürümü tutulan zamana bağlı tabloda SONSUZ ayarlamayı göz önünde bulundurun.*

Geçmiş tablosu SYSTEM_VERSIONIOING mekanizması tarafından özel olarak doldurulduğunda her zaman söz konusu olduğu (dönem sütunu sonuna sıralı) artan düzende, geçmiş satırları eklediyseniz temizleme kümelenmiş columnstore dizini üzerinde en iyi şekilde çalışır. Geçmiş tablosundaki satırları (var olan geçmiş veri geçişi yaptıysanız, durum olabilir) dönem sütunu sonuna sıralanmaz, doğru en iyi elde etmek için sıralanır B-Ağacı rowstore dizini üstünde kümelenmiş columnstore dizini yeniden oluşturmanız gerekir performans.

Sınırlı bekletme süresi ile geçmiş tablosunda kümelenmiş columnstore dizinini yeniden oluşturmayı kaçının, çünkü sistem sürümü oluşturma işlemiyle doğal olarak uygulanan satır grupları sıralama değişebilir. Geçmiş tablosunda kümelenmiş columnstore dizini yeniden oluşturmanız gerekiyorsa, bu, normal veri temizleme için gerekli rowgroups sıralama koruma uyumlu B-Ağacı dizini üstünde yeniden oluşturarak yapabilirsiniz. Sütun dizini garantili veri sırası olmadan kümelenmiş varolan geçmiş tablosu ile zamana bağlı tablo oluşturursanız, aynı yaklaşımı yapılması gerekir:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Sınırlı saklama dönemi, kümelenmiş columnstore dizini olan geçmiş tablosu için yapılandırıldığında, o tablosunda kümelenmemiş ek B-Ağacı dizinleri oluşturulamıyor:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Deyimi yürütme girişimi şu hatayla başarısız olur:

*Msg 13772, Level 16, State 1 <br> </br> sonlu saklama dönemi ve tanımlanan kümelenmiş columnstore dizini bulunduğundan kümelenmemiş dizin 'WebsiteUserInfoHistory' zamana bağlı geçmiş tablosu üzerinde oluşturulamıyor.*

## <a name="querying-tables-with-retention-policy"></a>Bekletme İlkesi tablolarla sorgulama
Zamana bağlı tabloda tüm sorguları otomatik olarak eski satırlar temizleme görevi tarafından silinebilir beri öngörülemeyen ve tutarsız sonuçları oluşturmamak için sınırlı bekletme ilkesi, eşleşen geçmiş satırları filtreler *herhangi bir noktada süresi ve rasgele Sipariş*.

Aşağıdaki resimde, basit bir sorgu için sorgu planı gösterilmiştir:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

Sorgu planı dönem sütunu (ValidTo) sonuna kadar uygulanan ek filtre (vurgulanan) geçmiş tablosunda kümelenmiş dizin tarama işleci içerir. Bu örnek, bir ay saklama dönemi WebsiteUserInfo tablosunda ayarlamak varsayar.

![Bekletme sorgu filtresi](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Ancak, geçmiş tablosu doğrudan sorgu, dönem ancak hiçbir garanti olmaksızın yinelenebilir sorgu sonuçları için belirtilen bekletme eski olan satırları görebilirsiniz. Aşağıdaki resimde sorgu için sorgu yürütme planı uygulanan ek filtreler geçmiş tablosunda gösterilmiştir:

![Geçmiş saklama filtresi olmadan sorgulama](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

İş mantığınızın tutarsız veya beklenmeyen sonuçlar alabilirsiniz gibi geçmiş tablosu saklama dönemi ötesinde okuma kullanmayın. Zamana bağlı sorguları zamana bağlı tablolardaki verileri çözümlemek için FOR system_tıme yan tümcesiyle birlikte kullanmanızı öneririz.

## <a name="point-in-time-restore-considerations"></a>Zaman geri yükleme hakkında önemli noktalar noktası
Yeni veritabanı tarafından oluşturduğunuzda [varolan bir veritabanını zaman içinde belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md), veritabanı düzeyinde devre dışı zamana bağlı bekletme sahiptir. (**is_temporal_history_retention_enabled** flag set to OFF). Bu işlevsellik, bunları sorgulamak ulaşmadan eski satır kaldırılır endişelenmeden geri yükleme sırasında tüm geçmiş satırları incelemek sağlar. İçin kullanabileceğiniz *ötesinde yapılandırılan saklama süresi geçmiş verileri*.

Zamana bağlı tablo bir ay Bekletme dönemi belirtilen olduğunu varsayalım. Veritabanınızı Premium hizmet katmanında oluşturduysanız, veritabanı durumu ile veritabanı kopyasını geri geçmişte 35 güne Oluştur gerçekleştirebilir. Etkili bir şekilde geçmiş tablosu doğrudan sorgulayarak 65 güne kadar eski olan geçmiş satırları analiz etmenize olanak tanır.

Zamana bağlı bekletme temizleme etkinleştirmek istiyorsanız, geri yükleme noktası sonra aşağıdaki Transact-SQL deyimini çalıştırın:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Sonraki adımlar
Zamana bağlı tablolarda uygulamalarınızda kullanmayı öğrenmek için kullanıma [zamana bağlı tablolarda Azure SQL veritabanı ile çalışmaya başlama](sql-database-temporal-tables.md).

Dinlemek için kanal 9 ziyaret bir [gerçek müşteri zamana bağlı uygulama başarı Öykü](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) ve izleme bir [zamana bağlı tanıtım canlı](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Zamana bağlı tablolarda hakkında ayrıntılı bilgi için gözden [MSDN belgelerine](https://msdn.microsoft.com/library/dn935015.aspx).

