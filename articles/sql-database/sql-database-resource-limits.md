---
title: "Azure SQL veritabanı kaynak sınırları | Microsoft Docs"
description: "Bu sayfa, Azure SQL veritabanı için bazı ortak kaynak sınırları açıklar."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 884e519f-23bb-4b73-a718-00658629646a
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Active
ms.date: 01/12/2018
ms.author: carlrab
ms.openlocfilehash: 13641b190c3c157f5b302314f88a42a160a1f2e0
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/13/2018
---
# <a name="azure-sql-database-resource-limits"></a>Azure SQL veritabanı kaynak sınırları

## <a name="single-database-storage-sizes-and-performance-levels"></a>Tek veritabanı: depolama boyutlarına ve performans düzeyleri

Tek veritabanları için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde tek bir veritabanı için kullanılabilir kaynakları gösterir. Hizmet katmanı, performans düzeyi ve kullanarak tek veritabanı için bir depolama miktarını ayarlayabilirsiniz [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), veya [REST API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="single-database-change-storage-size"></a>Tek veritabanı: depolama boyutunu değiştirme

- Tek bir veritabanı için DTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [tek veritabanı: depolama boyutlarına ve performans düzeyleri](#single-database-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının Hizmet katmanını ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Tek veritabanı: Dtu'lar değiştirme

Başlangıçta bir hizmet katmanı, performans düzeyi ve depolama miktarına seçtikten sonra tek bir veritabanı yukarı veya aşağı gerçek deneyimi kullanarak dinamik olarak bağlı ölçeklendirmek [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Aşağıdaki videoda, tek bir veritabanı için kullanılabilir Dtu'lar artırmak için performans katmanı dinamik olarak değiştirme gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Bir veritabanının hizmet katmanının ve/veya performansının değiştirilmesi, özgün veritabanının yeni performans düzeyinde bir çoğaltmasını oluşturur ve ardından bağlantıları bu çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak genellikle altında 4 saniyeden 30 saniyeden zamanın % 99 olmasıdır. Varsa büyük sayılar şu anda bağlantıları, yürütülen işlemler devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, değiştirilmesi için gelen veya bir standart hizmet katmanı içinde 250 GB veritabanı altı saat içinde tamamlamanız gerekir. Premium Hizmet katmanını içinde performans düzeylerini değiştirme boyutu aynı veritabanı için bir ölçek büyütme üç saat içinde tamamlamanız gerekir.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [SQL REST API kullanarak işlemlerini yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanı veya performans düzeyini yükseltme yapıyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı en büyük boyutunu artırmaz.
* Bir veritabanı düşürmek için kullanılan veritabanı alanı hedef hizmeti katmanını ve performans düzeyini boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Öğesinden önceki sürüme indirme zaman **Premium** veya **Premium RS** için **standart** katmanı, bir ek depolama alanı maliyet geçerlidir her ikisi de (1) veritabanının en büyük boyutu bulunan destekleniyorsa hedef performans düzeyi ve (2) en büyük boyut hedef performans düzeyi dahil depolama miktarını aşıyor. En büyük boyut 500 GB P1 veritabanıyla S3 için downsized, örneğin, daha sonra ek depolama alanı maliyeti S3 500 GB en büyük boyutunu destekler ve yalnızca 250 GB birlikte depolama miktarını olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarını 500 GB – 250 GB = 250 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 
* Bir veritabanıyla yükseltirken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkinse, onun ikincil veritabanları için istediğiniz performans katmanı birincil veritabanı (genel rehberlik) yükseltmeden önce yükseltin. Farklı bir yükseltme yaparken, ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanını önceki sürüme indirme zaman [coğrafi çoğaltma](sql-database-geo-replication-portal.md) istediğiniz performans katmanına birincil veritabanlarını ikincil veritabanı (genel rehberlik) eski sürüme düşürmeyi önce düşürmek etkin. Farklı bir sürüme eski sürüme düşürmeyi, birincil veritabanı eski sürüme düşürmeyi ilk gereklidir.
* Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. İçin olana varsa **temel** katmanı, daha düşük bir yedekleme Bekletme dönemi - bkz [Azure SQL veritabanı yedeklemeleri](sql-database-automated-backups.md).
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Tek veritabanı: P11 ve en büyük boyut, 1 TB'den büyükse P15 sınırlamaları

P11 ve P15 veritabanı aşağıdaki bölgelerde desteklenen 1 TB'den büyük bir maksimum boyut: BİZE East2, Batı ABD, BİZE kamu Virginia, Batı Avrupa, Orta Almanya, Güney Doğu Asya, Japonya Doğu, Avustralya Doğu, Kanada merkezi ve Doğu Kanada. En büyük boyutu 1 TB'den büyük olan P11 ve P15 veritabanları için aşağıdaki konuları ve sınırlamalar uygulanır:

- En büyük boyutu 1 TB'den büyük (4 TB veya 4096 GB değeri kullanarak) bir veritabanı oluşturulurken seçerseniz, veritabanı desteklenmeyen bir bölgede sağlandığında Oluştur komutu bir hata ile başarısız olur.
- Desteklenen bölgeleri birinde bulunan mevcut P11 ve P15 veritabanları için en fazla depolama için 1 TB ötesinde 256 GB artışlarla artırabilirsiniz en fazla 4 TB. Daha büyük bir boyutu bölgenizde destekleyip desteklemediğini görmek için [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) işlev veya Azure portalında veritabanı boyutu inceleyin. Varolan P11 veya P15 yükseltme veritabanı yalnızca sunucu düzeyinde asıl oturum açma veya dbmanager veritabanı rolünün üyeleri tarafından gerçekleştirilebilir. 
- Bir yükseltme işlemi desteklenen bir bölgede yürütülürse yapılandırma anında güncelleştirilir. Veritabanı yükseltme işlemi sırasında çevrimiçi kalır. Ancak, gerçek veritabanı dosyalarını yeni maksimum boyuta yükseltilene kadar tam tutar 1 TB depolama alanı dışında depolama alanını olamaz. Gereken süreyi yükseltilen veritabanı boyutuna bağlıdır. 
- Oluşturma veya güncelleştirme P11 veya P15 bir veritabanı, yalnızca 1 TB ve 4 TB en büyük boyutu 256 GB artışlarla arasında seçim yapabilirsiniz. P11/P15 oluştururken, varsayılan depolama 1 TB'lık önceden seçilmiş seçeneğidir. Desteklenen bölgeleri birinde bulunan veritabanları için en çok 4 TB yeni veya varolan bir tek veritabanı için depolama en artırabilir. Diğer tüm bölgeler için en büyük boyutu 1 TB yükseltilemez. 4 TB dahil depolama seçtiğinizde fiyat değiştirmez.
- Bir veritabanı en büyük boyutu 1 TB'den büyük ayarlanmışsa, 1 TB kullanılan depolamanın olsa bile, ardından 1 TB'ye kadar değiştirilemez. Bu nedenle, size bir P11 veya P15 TB P11 veya 1 TB P15 ya da daha düşük performans katmanı, P1 P6 gibi 1 1 TB'den büyük bir maksimum boyutu ile düşürülemiyor). Bu kısıtlama noktası zamanında dahil olmak üzere kopyalama senaryoları ve geri yükleme için de geçerlidir coğrafi geri yükleme, uzun-süreli yedekleme-saklama ve veritabanı kopyası. En büyük boyutu 1 TB'den büyük olan bir veritabanı yapılandırıldıktan sonra bu veritabanının tüm geri yükleme işlemleri en büyük boyutu 1 TB'den büyük olan P11/P15 çalıştırmanız gerekir.
- Aktif coğrafi çoğaltma senaryoları için:
   - Bu ilişki bir coğrafi çoğaltma ilişkisi ayarlanırken: birincil veritabanı P11 veya P15 ise, secondary(ies) de P11 veya P15; olması gerekir birden fazla 1 TB destekleme kapasitesine sahip olmadığından, düşük performans katmanı ikincil kopya reddedilir.
   - Coğrafi çoğaltma ilişkisinde birincil veritabanı yükseltme: en büyük boyutu 1 TB'den birincil veritabanında değiştirme ikincil veritabanını aynı değişiminde tetikler. Her iki yükseltmeyi değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil birden fazla 1 TB desteklemeyen bir bölgede ise, birincil yükseltilmez.
- Birden fazla 1 TB olan P11/P15 veritabanları yüklenmesi için içeri/dışarı aktarma hizmeti kullanma desteklenmiyor. SqlPackage.exe için kullanmak [alma](sql-database-import.md) ve [verme](sql-database-export.md) veri.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Esnek havuz: depolama boyutlarına ve performans düzeyleri

SQL Database esnek havuzlar için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde kaynakları gösterir. Hizmet katmanı, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), veya [REST API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> Esnek havuzlar bulunan tek veritabanlarını kaynak sınırları genellikle Dtu'lar ve Hizmet katmanını temel alan havuzları dışında tek veritabanları ile aynıdır. Örneğin, S2 veritabanı için en fazla eşzamanlı çalışan 120 çalışanları olur. Bu nedenle, standart havuzdaki bir veritabanı için en fazla eşzamanlı çalışan olduğunu da 120 çalışanları havuzunda veritabanı başına maksimum DTU (hangi S2 eşdeğerdir) 50 Dtu'lar ise.
>

[!INCLUDE [SQL DB service tiers table for elastic pools](../../includes/sql-database-service-tiers-table-elastic-pools.md)]

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı. Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım. Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz. Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına en fazla depolama |Bir havuzdaki veritabanı için en fazla depolama alanı. Havuza alınmış veritabanları, havuz depolama alanını paylaşır. Bu nedenle veritabanı depolama alanı, kalan havuz depolama alanı ve veritabanı başına maksimum depolama alanı değerlerinin hangisi daha küçükse bu değerle sınırlıdır. Veritabanı başına düşen maksimum depolama alanı, veri dosyalarının maksimum boyutunu ifade eder ve günlük dosyalarının kullandığı alanı içermez. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>Esnek havuz: depolama boyutunu değiştirme

- Bir esnek havuz eDTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [esnek havuz: depolama boyutlarına ve performans düzeyleri](#elastic-pool-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak esnek havuz için ek depolama alanı sağlanabilir [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [REST API'si ](/rest/api/sql/elasticpools/update).
- Esnek havuz için ek depolama alanı hizmet katmanına ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-edtus"></a>Esnek havuz: Edtu'lar değiştirme

Artırmak veya ihtiyaçlarını kullanarak kaynağını temel bir esnek havuz için kullanılabilir kaynakları azaltmak [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [ REST API](/rest/api/sql/elasticpools/update).

- Havuz Edtu rescaling, veritabanı bağlantılarını kısaca bırakılır. As (olmayan bir havuzdaki) tek bir veritabanı için Dtu'lar rescaling oluşur aynı davranış budur. Süresini ve rescaling işlemleri sırasında bırakılan bağlantıları için bir veritabanı etkisi hakkında daha fazla bilgi için bkz: [tek bir veritabanı için Dtu'lar Rescaling](#single-database-change-storage-size). 
- Havuz Edtu yeniden Ölçeklendir süresi havuzdaki tüm veritabanları tarafından kullanılan depolama alanının toplam miktarını bağlı olabilir. Genel olarak, rescaling gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu rescaling için beklenen gecikme süresi 3 saat sonra veya daha az. Bazı durumlarda standart veya temel katmanı içinde altında kullanılan alanı miktarının bağımsız olarak beş dakika rescaling gecikme olabilir.
- Genel olarak, veritabanı veya maksimum Edtu başına veritabanı başına minimum edtu'larını değiştirmek için süre beş dakikadır veya daha az.
- Havuz Edtu downsizing, kullanılan havuzu alanı hedef hizmet katman ve havuz edtu'larını boyutu izin verilen üst sınırdan küçük olması gerekir.
- Havuz Edtu rescaling, ek depolama alanı maliyeti (1 depolama en büyük havuz boyutu hedef havuzu tarafından desteklenir ve (2 depolama en büyük boyut hedef havuzu dahil depolama miktarını aşarsa uygulanır. 100 eDTU standart havuzu en büyük boyutu 100 GB ile 50 standart havuz eDTU downsized, örneğin, daha sonra ek depolama alanı maliyeti en büyük boyutu 100 GB hedef havuzu destekler ve dahil edilen depolama miktarını yalnızca 50 GB olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarı 100 GB – 50 GB = 50 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Veritabanı ve esnek havuz kaynak sınırlarını ulaşıldığında ne olur?

### <a name="compute-dtus-and-edtus"></a>İşlem (Dtu ve Edtu)

(Dtu ve Edtu ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgu gecikmesi artırır ve hatta zaman aşımı olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynak olarak yürütme için kaynakları serbest hale sağlanır.
Yüksek işlem kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya daha fazla Dtu'lar veya Edtu'lar veritabanı sağlamak için esnek havuz performans düzeyini artırma. Bkz: [tek veritabanı: Dtu'lar değiştirme](#single-database-change-dtus) ve [esnek havuz: Edtu'lar değiştirmek](#elastic-pool-change-edtus).
- Her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı ulaştığında en büyük boyut sınırı, veritabanı ekler ve veri boyutunu artırın güncelleştirmeleri ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya esnek havuz ya da daha fazla dahil edilen depolama alanı almak için performans düzeyini değiştirme en büyük boyutunu artırma. Bkz: [SQL veritabanı kaynak sınırları](sql-database-resource-limits.md).
- Veritabanını bir esnek havuz ise, böylece kendi depolama alanını başka veritabanlarıyla paylaşılmayan sonra alternatif olarak veritabanı dışında havuzu taşınabilir.

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanları (istek sayısı) 

Hizmet katmanını ve performans düzeyine göre (Dtu ve Edtu) sayısını eşzamanlı oturumlar ve çalışanları belirlenir.  Yeni istekler oturumu veya çalışan sınırları ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir olsa da, eşzamanlı çalışan sayısı genellikle tahmin etmek ve denetlemek daha zordur. Bu zaman veritabanı kaynak sınırları ulaştı ve uzun çalışan sorguları nedeniyle çalışanları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur. 

Yüksek oturum ya da çalışan kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:
- Veritabanı veya esnek havuz hizmet katmanı veya performans düzeyini artırma. Bkz [tek veritabanı: depolama boyutunu değiştirme](#single-database-change-storage-size), [tek veritabanı: Dtu'lar değiştirmek](#single-database-change-dtus), [esnek havuz: depolama boyutunu değiştirme](#elastic-pool-change-storage-size), ve [esnek havuz: Edtu'lar değiştirme ](#elastic-pool-change-edtus).
- İşlem kaynaklarını çakışması nedeniyle artan çalışan kullanımı neden olması durumunda her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Sonraki adımlar

- Hizmet katmanları hakkında daha fazla bilgi için bkz: [hizmet katmanları](sql-database-service-tiers.md).
- Tek veritabanları hakkında daha fazla bilgi için bkz: [tek veritabanı kaynakları](sql-database-resource-limits.md).
- Esnek havuzları hakkında daha fazla bilgi için bkz: [esnek havuzlar](sql-database-elastic-pool.md).
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
- Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu](sql-database-what-is-a-dtu.md).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database bakın.
