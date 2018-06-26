---
title: Azure SQL veritabanı DTU tabanlı kaynak sınırlar tek veritabanlarını | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanında tek veritabanları için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 5a7abb7d67de59ea326b5180cf94e3594cd06576
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753396"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli kullanarak tek veritabanları için kaynak sınırları 

Bu makale, Azure SQL Database esnek havuz DTU tabanlı satın alma modeli kullanarak için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli kaynak sınırları için esnek havuzlar için bkz: [DTU tabanlı kaynak sınırları - esnek havuzlar](sql-database-vcore-resource-limits-elastic-pools.md). VCore tabanlı kaynak sınırları için bkz: [vCore tabanlı kaynak sınırları - tek veritabanlarını](sql-database-vcore-resource-limits-single-databases.md) ve [vCore tabanlı kaynak sınırları - esnek havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Tek veritabanı: depolama boyutlarına ve performans düzeyleri

Tek veritabanları için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde tek bir veritabanı için kullanılabilir kaynakları gösterir. Hizmet katmanı, performans düzeyi ve kullanarak tek veritabanı için bir depolama miktarını ayarlayabilirsiniz [Azure portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-servers-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-servers-databases-manage.md#azure-cli-manage-logical-servers-and-databases), veya [REST API](sql-database-servers-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Temel hizmet katmanı
| **Performans düzeyi** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Eklenen depolama (GB) | 2 |
| En fazla depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| En fazla eşzamanlı çalışan (istek sayısı) | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

### <a name="standard-service-tier"></a>Standart hizmet katmanı
| **Performans düzeyi** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|---:|
| Maks. DTU | 10 | 20 | 50 | 100 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanında (devam)
| **Performans düzeyi** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|---:|
| Maks. DTU | 200 | 400 | 800 | 1600 | 3000 |
| Eklenen depolama (GB) | 250 | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| En fazla eşzamanlı çalışan (istek sayısı)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı 
| **Performans düzeyi** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Eklenen depolama (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| En fazla depolama seçenekleri (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| En fazla eşzamanlı çalışan (istek sayısı)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> Birden fazla 1 TB depolama alanı Premium katmanındaki şu anda kullanılabilir aşağıdakiler dışında tüm bölgelerde: Birleşik Krallık Kuzey Batı Orta ABD, İngiltere South2, Çin, Doğu, USDoDCentral, Almanya Orta, USDoDEast, ABD kamu Güneybatı, BİZE kamu Güney Merkezi, Almanya Kuzeydoğu, Çin Kuzey, ABD kamu Doğu. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Tek veritabanı: depolama boyutunu değiştirme

- Tek bir veritabanı için DTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [tek veritabanı: depolama boyutlarına ve performans düzeyleri](#single-database-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Azure portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının Hizmet katmanını ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Tek veritabanı: değişiklik Dtu'lar

Başlangıçta bir hizmet katmanı, performans düzeyi ve depolama miktarına seçtikten sonra tek bir veritabanı yukarı veya aşağı gerçek deneyimi kullanarak dinamik olarak bağlı ölçeklendirmek [Azure portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Aşağıdaki videoda, tek bir veritabanı için kullanılabilir Dtu'lar artırmak için performans katmanı dinamik olarak değiştirme gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Bir veritabanının hizmet katmanının ve/veya performansının değiştirilmesi, özgün veritabanının yeni performans düzeyinde bir çoğaltmasını oluşturur ve ardından bağlantıları bu çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak 30 saniyeden zamanın % 99 olur. Varsa büyük sayılar şu anda bağlantıları, yürütülen işlemler devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, değiştirilmesi için gelen veya bir standart hizmet katmanı içinde 250 GB veritabanı altı saat içinde tamamlamanız gerekir. Premium Hizmet katmanını içinde performans düzeylerini değiştirme boyutu aynı veritabanı için bir ölçek büyütme üç saat içinde tamamlamanız gerekir.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [SQL REST API kullanarak işlemlerini yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanı veya performans düzeyini yükseltme yapıyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı en büyük boyutunu artırmaz.
* Bir veritabanı düşürmek için kullanılan veritabanı alanı hedef hizmeti katmanını ve performans düzeyini boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Öğesinden önceki sürüme indirme zaman **Premium** için **standart** katmanı, bir ek depolama alanı maliyet geçerlidir hem max (1 veritabanının boyutu hedef performans düzeyinde desteklenir ve (2 en fazla boyutu aşıyor Hedef performans düzeyi dahil depolama miktarı. En büyük boyut 500 GB P1 veritabanıyla S3 için downsized, örneğin, daha sonra ek depolama alanı maliyeti S3 500 GB en büyük boyutunu destekler ve yalnızca 250 GB birlikte depolama miktarını olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarını 500 GB – 250 GB = 250 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 
* Bir veritabanıyla yükseltirken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkinse, onun ikincil veritabanları için istediğiniz performans katmanı birincil veritabanı (en iyi performans için genel yönergeler) yükseltmeden önce yükseltin. Farklı bir yükseltme yaparken, ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanını önceki sürüme indirme zaman [coğrafi çoğaltma](sql-database-geo-replication-portal.md) istediğiniz performans katmanına birincil veritabanlarını ikincil veritabanı (en iyi performans için genel yönergeler) eski sürüme düşürmeyi önce düşürmek etkin. Farklı bir sürüme eski sürüme düşürmeyi, birincil veritabanı eski sürüme düşürmeyi ilk gereklidir.
* Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. İçin olana varsa **temel** katmanı, daha düşük bir yedekleme Bekletme dönemi - bkz [Azure SQL veritabanı yedeklemeleri](sql-database-automated-backups.md).
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Tek veritabanı: P11 ve en büyük boyut, 1 TB'den büyükse P15 sınırlamaları

P11 ve P15 veritabanı aşağıdaki bölgelerde desteklenen 1 TB'den büyük bir maksimum boyut: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Orta Kanada, Doğu Kanada, Orta ABD, Fransa Merkezi, Almanya Merkezi, Japonya Doğu, Japonya Batı, Kore Merkezi Kuzey Orta ABD, Kuzey Avrupa, Orta Güney ABD, Güneydoğu Asya, Birleşik Krallık Güney, Birleşik Krallık Batı, ABD East2, Batı ABD, ABD kamu Virginia ve Batı Avrupa. En büyük boyutu 1 TB'den büyük olan P11 ve P15 veritabanları için aşağıdaki konuları ve sınırlamalar uygulanır:

- En büyük boyutu 1 TB'den büyük (4 TB veya 4096 GB değeri kullanarak) bir veritabanı oluşturulurken seçerseniz, veritabanı desteklenmeyen bir bölgede sağlandığında Oluştur komutu bir hata ile başarısız olur.
- Desteklenen bölgeleri birinde bulunan mevcut P11 ve P15 veritabanları için en fazla depolama için 1 TB ötesinde 256 GB artışlarla artırabilirsiniz en fazla 4 TB. Daha büyük bir boyutu bölgenizde destekleyip desteklemediğini görmek için [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) işlev veya Azure portalında veritabanı boyutu inceleyin. Varolan P11 veya P15 yükseltme veritabanı yalnızca sunucu düzeyinde asıl oturum açma veya dbmanager veritabanı rolünün üyeleri tarafından gerçekleştirilebilir. 
- Bir yükseltme işlemi desteklenen bir bölgede yürütülürse yapılandırma anında güncelleştirilir. Veritabanı yükseltme işlemi sırasında çevrimiçi kalır. Ancak, gerçek veritabanı dosyalarını yeni maksimum boyuta yükseltilene kadar tam tutar 1 TB depolama alanı dışında depolama alanını olamaz. Gereken süreyi yükseltilen veritabanı boyutuna bağlıdır. 
- Oluşturma veya güncelleştirme P11 veya P15 bir veritabanı, yalnızca 1 TB ve 4 TB en büyük boyutu 256 GB artışlarla arasında seçim yapabilirsiniz. P11/P15 oluştururken, varsayılan depolama 1 TB'lık önceden seçilmiş seçeneğidir. Desteklenen bölgeleri birinde bulunan veritabanları için en çok 4 TB yeni veya varolan bir tek veritabanı için depolama en artırabilir. Diğer tüm bölgeler için en büyük boyutu 1 TB yükseltilemez. 4 TB dahil depolama seçtiğinizde fiyat değiştirmez.
- Bir veritabanı en büyük boyutu 1 TB'den büyük ayarlanmışsa, 1 TB kullanılan depolamanın olsa bile, ardından 1 TB'ye kadar değiştirilemez. Bu nedenle, size bir P11 veya P15 TB P11 veya 1 TB P15 ya da daha düşük performans katmanı, P1 P6 gibi 1 1 TB'den büyük bir maksimum boyutu ile düşürülemiyor). Bu kısıtlama noktası zamanında dahil olmak üzere kopyalama senaryoları ve geri yükleme için de geçerlidir coğrafi geri yükleme, uzun-süreli yedekleme-saklama ve veritabanı kopyası. En büyük boyutu 1 TB'den büyük olan bir veritabanı yapılandırıldıktan sonra bu veritabanının tüm geri yükleme işlemleri en büyük boyutu 1 TB'den büyük olan P11/P15 çalıştırmanız gerekir.
- Aktif coğrafi çoğaltma senaryoları için:
   - Bu ilişki bir coğrafi çoğaltma ilişkisi ayarlanırken: birincil veritabanı P11 veya P15 ise, secondary(ies) de P11 veya P15; olması gerekir birden fazla 1 TB destekleme kapasitesine sahip olmadığından, düşük performans katmanı ikincil kopya reddedilir.
   - Coğrafi çoğaltma ilişkisinde birincil veritabanı yükseltme: en büyük boyutu 1 TB'den birincil veritabanında değiştirme ikincil veritabanını aynı değişiminde tetikler. Her iki yükseltmeyi değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil birden fazla 1 TB desteklemeyen bir bölgede ise, birincil yükseltilmez.
- Birden fazla 1 TB olan P11/P15 veritabanları yüklenmesi için içeri/dışarı aktarma hizmeti kullanma desteklenmiyor. SqlPackage.exe için kullanmak [alma](sql-database-import.md) ve [verme](sql-database-export.md) veri.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
- Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
