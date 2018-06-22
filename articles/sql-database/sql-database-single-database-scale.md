---
title: Tek veritabanı kaynaklar - Azure SQL Database ölçeği | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı'nda tek bir veritabanı için kullanılabilen işlem ve depolama kaynaklarını ölçeklendirme açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 525416506a22f386de574ca02b2e919ac47b8737
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309886"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Azure SQL veritabanı'nda tek veritabanı kaynaklarını ölçeklendirme

Bu makalede, Azure SQL veritabanı'nda tek bir veritabanı için kullanılabilen işlem ve depolama kaynaklarını ölçeklendirme açıklar. 

## <a name="vcore-based-purchasing-model-change-storage-size"></a>satın alma modeli vCore tabanlı: depolama boyutunu değiştirme

- 1 GB artışlarla kullanarak en büyük boyut sınırı en fazla depolama alanı sağlanabilir. Minimum yapılandırılabilir veri depolama 5 GB olacak 
- Tek bir veritabanı için depolama artırarak veya azaltarak, en büyük boyut kullanılarak sağlanabilir [Azure portal](https://portal.azure.com), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- SQL veritabanı otomatik olarak ek depolama alanı % 30 32 GB ve günlük dosyaları için vCore TempDB, ancak 384 GB aşmayacak şekilde ayırır. TempDB tüm hizmet katmanlarında ekli bir SSD bulunur.
- Tek bir veritabanı için depolama hizmet katmanının depolama birimi fiyatı ile çarpılan veri depolama ve günlük depolama tutarlarının toplamı fiyatıdır. TempDB maliyetini vCore fiyatına dahil edilir. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-compute-resources"></a>satın alma modeli vCore tabanlı: değişiklik işlem kaynakları

Başlangıçta vCores sayısı seçtikten sonra tek bir veritabanı yukarı veya aşağı gerçek deneyimi kullanarak dinamik olarak bağlı ölçeklendirmek [Azure portal](sql-database-single-database-scale.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Bir veritabanının hizmet katmanının ve/veya performansının değiştirilmesi, özgün veritabanının yeni performans düzeyinde bir çoğaltmasını oluşturur ve ardından bağlantıları bu çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak genellikle altında 4 saniyeden 30 saniyeden zamanın % 99 olmasıdır. Varsa büyük sayılar şu anda bağlantıları, yürütülen işlemler devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, değiştirilmesi için gelen veya genel amaçlı hizmet katmanı içinde 250 GB veritabanı altı saat içinde tamamlamanız gerekir. İş kritik hizmet katmanı içinde performans düzeylerini değiştirme boyutu aynı veritabanı için bir ölçek büyütme üç saat içinde tamamlamanız gerekir.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [SQL REST API kullanarak işlemlerini yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanı veya performans düzeyini yükseltme yapıyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı en büyük boyutunu artırmaz.
* Bir veritabanı düşürmek için kullanılan veritabanı alanı hedef hizmeti katmanını ve performans düzeyini boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Bir veritabanıyla yükseltirken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkinse, onun ikincil veritabanları için istediğiniz performans katmanı birincil veritabanı (en iyi performans için genel yönergeler) yükseltmeden önce yükseltin. Farklı bir yükseltme yaparken, ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanını önceki sürüme indirme zaman [coğrafi çoğaltma](sql-database-geo-replication-portal.md) istediğiniz performans katmanına birincil veritabanlarını ikincil veritabanı (en iyi performans için genel yönergeler) eski sürüme düşürmeyi önce düşürmek etkin. Farklı bir sürüme eski sürüme düşürmeyi, birincil veritabanı eski sürüme düşürmeyi ilk gereklidir.
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="dtu-based-purchasing-model-change-storage-size"></a>DTU tabanlı satın alma modeli: depolama boyutunu değiştirme

- Tek bir veritabanı için DTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [tek veritabanı: depolama boyutlarına ve performans düzeyleri](#single-database-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Azure portal](sql-database-servers-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının Hizmet katmanını ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-compute-resources-dtus"></a>DTU tabanlı satın alma modeli: değişiklik işlem kaynaklarını (Dtu'lar)

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

## <a name="dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>DTU tabanlı satın alma modeli: sınırlamalar, P11 ve en büyük boyut, 1 TB'den büyükse P15

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

Genel kaynak sınırları için bkz: [SQL veritabanı vCore tabanlı kaynak sınırları - tek veritabanlarını](sql-database-vcore-resource-limits-single-databases.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - esnek havuzlar](sql-database-dtu-resource-limits-single-databases.md).
