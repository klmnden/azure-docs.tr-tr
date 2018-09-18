---
title: Ölçek tek veritabanı kaynakları - Azure SQL veritabanı | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı'nda işlem ve depolama kaynaklarını tek bir veritabanının ölçeğini açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: carlrab
ms.openlocfilehash: 61b9a6f3c629992e7cb2a8a64b66f63b11045eb8
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733806"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Azure SQL veritabanı'nda ölçek tek veritabanı kaynakları

Bu makalede, Azure SQL veritabanı'nda işlem ve depolama kaynaklarını tek bir veritabanının ölçeğini açıklar. 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>Sanal çekirdek tabanlı satın alma modeli: depolama boyutunu değiştirme

- 1 GB artışlarla kullanarak en büyük boyut sınırı en fazla depolama alanı sağlanabilir. Minimum yapılandırılabilir veri depolama 5 GB'tır 
- Artan veya azalan en büyük boyutu kullanarak tek bir veritabanı için depolama alanı sağlanabilir [Azure portalında](https://portal.azure.com), [Transact-SQL](/sql/t-sql/statements/alter-database-transact-sql?r#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- SQL veritabanı otomatik olarak ek depolama alanı % 30'luk 32 GB ve günlük dosyaları için sanal çekirdek TempDB için ancak 384 GB aşmayacak şekilde ayırır. TempDB ekli SSD'LERİN tüm hizmet katmanlarında bulunur.
- Tek bir veritabanı için depolama alanının fiyatı hizmeti katmanının depolama birimi fiyatı ile çarpılan veri depolama ve günlük depolama tutarlarının toplamıdır. Tempdb maliyeti, sanal çekirdek fiyatına dahildir. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>Sanal çekirdek tabanlı satın alma modeli: değişiklik bilgi işlem kaynakları

Başlangıçta çekirdek sayısını seçtikten sonra, tek bir veritabanının ölçeğini artırıp dinamik olarak gerçek deneyime kullanımına dayalı ölçeklendirebilirsiniz [Azure portalında](sql-database-single-database-scale.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Hizmet değiştiriliyor katmanının ve/veya işlem bir veritabanının boyut yeni işlem boyutu özgün veritabanının bir kopyasını oluşturur ve ardından bağlantıları çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak genel olarak 4 saniyenin altında 30 saniyeden daha kısa zaman %99 ise. Varsa büyük işlem şu bağlantıları uçuşta devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, için ya da bir genel amaçlı hizmet katmanında değiştirme 250 GB veritabanını altı saat içinde tamamlanır. Bir veritabanı için iş açısından kritik hizmet katmanında işlem boyutları ile değişiyor aynı boyutta, ölçek büyütme üç saat içinde tamamlanır.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [işlemlerini SQL REST API kullanarak yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanına yükseltme veya boyutu işlem, veritabanı boyutu üst sınırını (maxsıze) daha büyük bir boyut, açıkça belirtmediğiniz sürece artırmaz.
* Bir veritabanını indirgemek için kullanılan veritabanı boş alanı hedef hizmet katmanı boyutu ve işlem boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Bir veritabanını yükseltmek [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ikincil veritabanlarını istenilen hizmet katmanına yükseltme ve boyutunu (en iyi performans için genel kılavuz) birincil veritabanını yükseltmeden önce işlem. Farklı bir yükseltme sırasında ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanı ile alt sürüme düşürürken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ve birincil veritabanlarını istenen hizmet katmanı için eski sürümü yükleme boyutu (en iyi performans için genel kılavuz) ikincil veritabanı indirgemeden önce işlem. Farklı bir sürüme alt sürüme düşürürken birincil veritabanı eski sürüme düşürme ilk gereklidir.
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>DTU tabanlı satın alma modeli: depolama boyutunu değiştirme

- Belirli miktarda bir ek maliyet olmadan depolama tek veritabanı DTU ücretini içerir. Dahil edilen miktarın üzerinde ek depolama alanı 1 TB'kurmak 250 GB'lık artışlarla ve 1 TB ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet sağlanabilir. Dahil edilen depolama alanı miktarları ve en büyük boyutu sınırlar için bkz: [tek veritabanı: depolama boyutlarına ve bilgi işlem boyutlarına](#single-database-storage-sizes-and-performance-levels).
- Azure portalını kullanarak en büyük boyutunu artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [ REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının hizmet katmanı ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarı fiyatıdır. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>DTU tabanlı satın alma modeli: değişiklik bilgi işlem kaynakları (Dtu)

Başlangıçta bir hizmet katmanı, işlem boyutu ve depolama alanı miktarı seçtikten sonra tek bir veritabanının ölçeğini artırıp Azure portalını kullanarak gerçek deneyime göre dinamik olarak bağlı ölçekleme yapabilirsiniz [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Aşağıdaki video gösterildiği hizmet dinamik olarak değiştirme katmanı ve tek bir veritabanı için kullanılabilir Dtu'lar artırmak için boyutu işlem.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Hizmet değiştiriliyor katmanının ve/veya işlem bir veritabanının boyut yeni işlem boyutu özgün veritabanının bir kopyasını oluşturur ve ardından bağlantıları çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak 30 saniyeden daha kısa zaman %99 değildir. Varsa büyük işlem şu bağlantıları uçuşta devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, bir 250 GB veritabanı için ya da bir standart hizmet katmanında değiştirme altı saat içinde tamamlamanız gerekir. Bir veritabanı için Premium hizmet katmanında işlem boyutları ile değişiyor aynı boyutta, ölçek büyütme üç saat içinde tamamlanır.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [işlemlerini SQL REST API kullanarak yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanına yükseltme veya boyutu işlem, veritabanı boyutu üst sınırını (maxsıze) daha büyük bir boyut, açıkça belirtmediğiniz sürece artırmaz.
* Bir veritabanını indirgemek için kullanılan veritabanı boş alanı hedef hizmet katmanı boyutu ve işlem boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Öğesinden önceki sürüme indirirken **Premium** için **standart** katmanı, bir ek depolama alanı miktarları ücrete tabidir hem hedef işlem boyutu (1) veritabanının maksimum boyutunu desteklenir ve dahil edilen (2) en büyük boyutu aşıyor işlem boyutu hedef depolama miktarı. En fazla 500 GB boyutlu bir P1 veritabanı, S3 downsized, örneğin, ardından bir ek depolama alanı S3 500 GB'lık bir en büyük boyutu destekler ve kendi dahil edilen depolama miktarını yalnızca 250 GB olduğundan alanı miktarları ücrete tabidir. Bu nedenle, ek depolama alanı miktarı 500 GB – 250 GB = 250 GB ' dir. Ek depolama fiyatlandırması için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Gerçek kullanılan alanı miktarı dahil edilen depolama alanı miktarı azsa, daha sonra bu ek maliyet veritabanı boyutu için dahil edilen miktarın azaltarak önlenebilir. 
* Bir veritabanını yükseltmek [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ikincil veritabanlarını istenilen hizmet katmanına yükseltme ve boyutunu (en iyi performans için genel kılavuz) birincil veritabanını yükseltmeden önce işlem. Farklı bir yükseltme sırasında ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanı ile alt sürüme düşürürken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ve birincil veritabanlarını istenen hizmet katmanı için eski sürümü yükleme boyutu (en iyi performans için genel kılavuz) ikincil veritabanı indirgemeden önce işlem. Farklı bir sürüme alt sürüme düşürürken birincil veritabanı eski sürüme düşürme ilk gereklidir.
* Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. İçin indiriyorsanız **temel** katman, daha düşük bir yedekleme bekletme süresi - bkz [Azure SQL veritabanı yedeklemelerini](sql-database-automated-backups.md).
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>DTU tabanlı satın alma modeli: sınırlamalar, P11 ve P15 1 TB'den büyük olduğunda en büyük boyut

Boyut sınırı için P11 ve P15 veritabanı aşağıdaki bölgelerde desteklenir 1 TB'den büyük: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta ABD, Fransa Orta, Almanya Orta, Japonya Doğu, Japonya Batı, Kore Orta Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, UK Güney, UK Batı, ABD doğu2, Batı ABD, ABD Devleti Virginia ve Batı Avrupa. P11 ve P15 veritabanları en büyük boyutu 1 TB'den büyük ile aşağıdaki önemli noktalar ve sınırlamalar geçerlidir:

- Desteklenmeyen bir bölgede veritabanı sağlandığında create komutuyla en büyük boyutu 1 TB'den büyük (4 TB veya 4096 GB değeri kullanılarak) bir veritabanı oluşturulurken seçerseniz, bir hata ile başarısız olur.
- Desteklenen bölgelerden birinde bulunan mevcut P11 ve P15 veritabanları için en fazla depolama için 1 TB ötesinde 256 GB'lık artışlarla artırabilirsiniz 4 TB'a kadar. Daha büyük boyutta bölgenizde desteklenip desteklenmediğini görmek için [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) işlevi veya Azure portalında veritabanı boyutu inceleyin. Bir mevcut P11 veya P15 yükseltme veritabanı yalnızca bir sunucu düzeyi asıl oturum açma veya dbmanager veritabanı rolünün üyeleri tarafından gerçekleştirilebilir. 
- Bir yükseltme işlemi desteklenen bir bölgede yürütülürse yapılandırma hemen güncelleştirilir. Veritabanı yükseltme işlemi sırasında çevrimiçi kalır. Ancak, gerçek veritabanı dosyalarını yeni maksimum boyuta yükseltilene dek tam 1 TB'a kadar depolama alanı dışında depolama miktarını faydalanamaz. Gereken süre uzunluğunu yükseltilmekte olan veritabanının boyutuna bağlıdır. 
- Oluşturma veya güncelleştirme P11 veya P15 veritabanı, yalnızca 256 GB'lık artışlarla en büyük boyutu 1 TB ile 4 TB arasında seçim yapabilirsiniz. P11/P15 oluştururken, varsayılan depolama alanı 1 TB'lık önceden seçilmiş seçenektir. Desteklenen bölgelerden birinde bulunan veritabanları için en fazla 4 TB'ın yeni veya mevcut bir tek veritabanı için depolama en artırabilirsiniz. Diğer tüm bölgeler için 1 TB üst sınırı yükseltilemez. Dahil edilen depolama 4 TB'ı seçtiğinizde fiyat değiştirmez.
- Bir veritabanının en yüksek boyutu 1 TB'den büyük ayarlanırsa, 1 TB kullanılan gerçek depolama olsa bile, ardından 1 TB ile değiştirilemez. Bu nedenle, en fazla bir 1 TB P11 ya da 1 TB P15 1 TB'den büyük P11 veya P15 düşürme veya alt boyutu, P1-P6 gibi işlem). Belirli bir noktaya, dahil olmak üzere kopyalama senaryoları ve geri yükleme için de bu kısıtlamanın uygulandığı coğrafi geri yükleme, uzun-vadeli-yedekleme-elde tutma ve veritabanı kopyası. En fazla boyutu 1 TB'den büyük olan bir veritabanı yapılandırıldıktan sonra bu veritabanının tüm geri yükleme işlemleri en büyük boyutu 1 TB'den büyük P11/P15 çalıştırmanız gerekir.
- Etkin coğrafi çoğaltma senaryoları için:
   - Bir coğrafi çoğaltma ilişkisi ayarlama: birincil veritabanının P11 veya P15 ise secondary(ies) da P11 veya P15; olması gerekir alt işlem boyutu 1 TB'den fazla destekleme kapasitesine sahip olmadığından ikincil veritabanı reddedilir.
   - Coğrafi çoğaltma ilişkisinde birincil veritabanını yükseltmeden: 1 TB'den fazla birincil veritabanında en büyük boyutunu değiştirmek, ikincil veritabanında aynı değişikliği tetikler. Her iki yükseltmeleri değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil 1 TB'den fazla desteklemeyen bir bölgede, birincil yükseltilmez.
- 1 TB'den fazla P11/P15 veritabanlarını yüklemek için içeri/dışarı aktarma hizmetini kullanarak desteklenmiyor. Kullanmak için SqlPackage.exe [alma](sql-database-import.md) ve [dışarı](sql-database-export.md) veri.


## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak limitleri için bkz. [SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-single-databases.md).
