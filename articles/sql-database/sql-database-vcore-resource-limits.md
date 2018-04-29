---
title: Azure SQL veritabanı vCore tabanlı kaynak sınırları | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı için bazı ortak vCore tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/04/2018
ms.author: carlrab
ms.openlocfilehash: 204702eee1cf502ac873e0c1f5e3fd257ecce33c
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="azure-sql-database-vcore-based-purchasing-model-limits-preview"></a>Azure SQL veritabanı vCore tabanlı satın alma modeli sınırları (Önizleme)

> [!IMPORTANT]
> DTU tabanlı satın alma modeli sınırları için bkz: [SQL veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md).

## <a name="single-database-storage-sizes-and-performance-levels"></a>Tek veritabanı: depolama boyutlarına ve performans düzeyleri

Tek veritabanları için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde tek bir veritabanı için kullanılabilir kaynakları gösterir. Hizmet katmanı, performans düzeyi ve kullanarak tek veritabanı için bir depolama miktarını ayarlayabilirsiniz [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](sql-database-single-database-resources.md#manage-single-database-resources-using-transact-sql), [PowerShell](sql-database-single-database-resources.md#manage-single-database-resources-using-powershell), [Azure CLI](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-cli), veya [REST API](sql-database-single-database-resources.md#manage-single-database-resources-using-the-rest-api).

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı
|Performans düzeyi|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Bellek (GB)|7|14|28|56|112|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En büyük veri boyutu (GB)|1024|1024|1536|3072|4096|
|En büyük günlük boyutu|307|307|461|922|1229|
|TempDB size(DB)|32|64|128|256|384|
|Hedef IOPS|320|640|1280|2560|5120|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|3200|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|
|Çoğaltmaların sayısı|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı
|Performans düzeyi|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Bellek (GB)|7|14|28|56|112|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1|2|4|8|20|
|Depolama türü|Ekli SSD|Ekli SSD|Ekli SSD|Ekli SSD|Ekli SSD|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|1024|
|En büyük günlük boyutu|307|307|307|307|307|
|TempDB size(DB)|32|64|128|256|384|
|Hedef IOPS|5000|10000|20000|40000|80000|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En fazla eşzamanlı çalışan (istek sayısı)|200|400|800|1600|3200|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|
|Çoğaltmaların sayısı|3|3|3|3|3|
|Birden çok AZ|Evet|Evet|Evet|Evet|Evet|
|Genişleme okuma|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

## <a name="single-database-change-storage-size"></a>Tek veritabanı: depolama boyutunu değiştirme

- 1 GB artışlarla kullanarak en büyük boyut sınırı en fazla depolama alanı sağlanabilir. Minimum yapılandırılabilir veri depolama 5 GB olacak 
- Tek bir veritabanı için depolama artırarak veya azaltarak, en büyük boyut kullanılarak sağlanabilir [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update).
- SQL veritabanı otomatik olarak ek depolama alanı % 30 32 GB ve günlük dosyaları için vCore TempDB, ancak 384 GB aşmayacak şekilde ayırır. TempDB tüm hizmet katmanlarında ekli bir SSD bulunur.
- Tek bir veritabanı için depolama hizmet katmanının depolama birimi fiyatı ile çarpılan veri depolama ve günlük depolama tutarlarının toplamı fiyatıdır. TempDB maliyetini vCore fiyatına dahil edilir. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-vcores"></a>Tek veritabanı: vCores değiştirme

Başlangıçta vCores sayısı seçtikten sonra tek bir veritabanı yukarı veya aşağı gerçek deneyimi kullanarak dinamik olarak bağlı ölçeklendirmek [Azure portal](sql-database-single-database-resources.md#manage-single-database-resources-using-the-azure-portal), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az_sql_db_update), veya [REST API](/rest/api/sql/databases/update). 

Bir veritabanının hizmet katmanının ve/veya performansının değiştirilmesi, özgün veritabanının yeni performans düzeyinde bir çoğaltmasını oluşturur ve ardından bağlantıları bu çoğaltmaya geçirir. Bu işlem sırasında veri kaybı olmaz, ancak çoğaltmaya geçişin gerçekleştiği kısa süre zarfında veritabanıyla bağlantılar devre dışı bırakılır, bu nedenle uçuştaki bazı işlemler geri alınabilir. Anahtar üzerinden süreyi değişir, ancak genellikle altında 4 saniyeden 30 saniyeden zamanın % 99 olmasıdır. Varsa büyük sayılar şu anda bağlantıları, yürütülen işlemler devre dışı bırakıldı, anahtar üzerinden süreyi daha uzun olabilir. 

Tüm ölçek artırma işleminin süresi hem veritabanı boyutuna hem de değişiklikten önceki ve sonraki hizmet katmanına bağlı olarak değişir. Örneğin, değiştirilmesi için gelen veya genel amaçlı hizmet katmanı içinde 250 GB veritabanı altı saat içinde tamamlamanız gerekir. İş kritik hizmet katmanı içinde performans düzeylerini değiştirme boyutu aynı veritabanı için bir ölçek büyütme üç saat içinde tamamlamanız gerekir.

> [!TIP]
> Edenler işlemleri izlemek için bkz: [SQL REST API kullanarak işlemlerini yönetmek](/rest/api/sql/Operations/List), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/get-azurermsqldatabaseactivity) ve [Stop-AzureRmSqlDatabaseActivity](/powershell/module/azurerm.sql/stop-azurermsqldatabaseactivity).

* Daha yüksek bir hizmet katmanı veya performans düzeyini yükseltme yapıyorsanız, daha büyük bir boyutu (maxsize) açıkça belirtmediğiniz sürece veritabanı en büyük boyutunu artırmaz.
* Bir veritabanı düşürmek için kullanılan veritabanı alanı hedef hizmeti katmanını ve performans düzeyini boyutu izin verilen üst sınırdan küçük olması gerekir. 
* Bir veritabanıyla yükseltirken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkinse, onun ikincil veritabanları için istediğiniz performans katmanı birincil veritabanı (en iyi performans için genel yönergeler) yükseltmeden önce yükseltin. Farklı bir yükseltme yaparken, ikincil veritabanını yükseltmeden önce gereklidir.
* Bir veritabanını önceki sürüme indirme zaman [coğrafi çoğaltma](sql-database-geo-replication-portal.md) istediğiniz performans katmanına birincil veritabanlarını ikincil veritabanı (en iyi performans için genel yönergeler) eski sürüme düşürmeyi önce düşürmek etkin. Farklı bir sürüme eski sürüme düşürmeyi, birincil veritabanı eski sürüme düşürmeyi ilk gereklidir.
* Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Esnek havuz: depolama boyutlarına ve performans düzeyleri

SQL Database esnek havuzlar için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde kaynakları gösterir. Hizmet katmanı, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-powershell), [Azure CLI](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-cli), veya [REST API](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-rest-api).

> [!NOTE]
> Esnek havuzlar bulunan tek veritabanlarını kaynak sınırları genellikle aynı performans düzeyine sahip havuzları dışında tek veritabanları ile aynıdır. Örneğin, bir GP_Gen4_1 veritabanı için en fazla eşzamanlı çalışan 200 çalışanları olur. Bu nedenle, GP_Gen4_1 havuzundaki bir veritabanı için en fazla eşzamanlı çalışan olduğu de 200 çalışanlar. Eşzamanlı çalışan GP_Gen4_1 havuzundaki toplam sayısı 210 olduğunu unutmayın.

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı
|Performans düzeyi|GP_Gen4_1|GP_Gen4_2|GP_Gen4_4|GP_Gen4_8|GP_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Bellek (GB)|7|14|28|56|112|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|Yok|Yok|Yok|Yok|Yok|
|Depolama türü|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|Premium (uzak) depolama|
|En büyük veri boyutu (GB)|512|756|1536|2048|3584|
|En büyük günlük boyutu|154|227|461|614|1075|
|TempDB size(DB)|32|64|128|256|384|
|Hedef IOPS|320|640|1280|2560|5120|
|G/ç gecikmesi (yaklaşık)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|5-7 ms (yazma)<br>5-10 ms (okuma)|
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|3360|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|100|200|500|500|500|
|Min/max esnek havuz tıklatın-durdurur|0, 0.25, 0,5, 1|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|
|Çoğaltmaların sayısı|1|1|1|1|1|
|Birden çok AZ|Yok|Yok|Yok|Yok|Yok|
|Genişleme okuma|Yok|Yok|Yok|Yok|Yok|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı
|Performans düzeyi|BC_Gen4_1|BC_Gen4_2|BC_Gen4_4|BC_Gen4_8|BC_Gen4_16|
|:--- | --: |--: |--: |--: |--: |
|H/W oluşturma|4|4|4|4|4|
|vCores|1|2|4|8|16|
|Bellek (GB)|7|14|28|56|112|
|Columnstore desteği|Evet|Evet|Evet|Evet|Evet|
|Bellek içi OLTP depolama (GB)|1|2|4|8|20|
|Depolama türü|Ekli SSD|Ekli SSD|Ekli SSD|Ekli SSD|Ekli SSD|
|En büyük veri boyutu (GB)|1024|1024|1024|1024|1024|
|En büyük günlük boyutu|307|307|307|461|614|
|TempDB size(DB)|32|64|128|256|384|
|Hedef IOPS|320|640|1280|2560|5120|
|G/ç gecikmesi (yaklaşık)|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|1-2 ms (yazma)<br>1-2 (okuma) ms|
|En fazla eşzamanlı çalışan (istek sayısı)|210|420|840|1680|3360|
|İzin verilen maks. oturumları|30000|30000|30000|30000|30000|
|En büyük havuz yoğunluğu|Yok|50|100|100|100|
|Min/max esnek havuz tıklatın-durdurur|0, 0.25, 0,5, 1|0, 0.25, 0,5, 1, 2|0, 0.25, 0,5, 1, 2, 4|0, 0.25, 0,5, 1, 2, 4, 8|0, 0.25, 0,5, 1, 2, 4, 8, 16|
|Birden çok AZ|Evet|Evet|Evet|Evet|Evet|
|Genişleme okuma|Evet|Evet|Evet|Evet|Evet|
|Yedekleme depolama dahil|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|1 X veritabanı boyutu|
|||
Bir esnek havuzun tüm vCores meşgul ise, havuzdaki her veritabanı sorguları işlemek için işlem kaynaklarını eşit miktarda alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Her bir veritabanı için veritabanı başına vCore minimum sıfır olmayan bir değere ayarlandığında, aksi takdirde garanti kaynak herhangi bir sayıdaki yanı sıra eşitliği paylaşımı esnek havuz kaynak değil.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına en fazla vCores |Havuzdaki diğer veritabanları tarafından kullanılabilir kullanımını temel alan varsa, havuzdaki herhangi bir veritabanı, kullanabilecek vCores maksimum sayısı. Veritabanı başına en fazla vCores bir veritabanı için kaynak garantisi değil. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımı genişliğindeki yükselmeleri işlemek için yüksek olan veritabanı başına en fazla vCores ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir.|
| Veritabanı başına Min vCores |Havuzdaki herhangi bir veritabanı garanti vCores minimum sayısı. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına min vCores 0 olarak ayarlayın ve ayrıca varsayılan değerdir. Bu özellik 0 ve veritabanı başına ortalama vCores kullanımını arasında herhangi bir yere ayarlanır. Havuz ve veritabanı başına min vCores bulunan veritabanlarının sayısına ürün havuzu başına vCores aşamaz.|
| Veritabanı başına en fazla depolama |Bir veritabanı için kullanıcı tarafından bir havuzda ayarlanan en fazla veritabanı boyutu. Havuza alınmış veritabanları bir veritabanı ulaşmak boyutu sınırlı olacak şekilde ayrılmış havuz depolama paylaşmak küçük kalan depolama havuzu ve veritabanı boyutu. En büyük veritabanı boyutu, veri dosyalarının en büyük boyuta başvuruyor ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||
 
## <a name="elastic-pool-change-storage-size"></a>Esnek havuz: depolama boyutunu değiştirme

- En büyük boyut sınırı en fazla depolama alanı sağlanabilir: 
 - Standart depolama artırın veya 10 GB artışlarla boyutunu azaltın
 - Premium depolama artırın veya boyutu 250 GB artışlarla azaltın
- Esnek havuz için depolama artırarak veya azaltarak, en büyük boyut kullanılarak sağlanabilir [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [ REST API](/rest/api/sql/elasticpools/update).
- Esnek havuz için depolama hizmet katmanının depolama birimi fiyatı ile çarpılan depolama miktarına fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="elastic-pool-change-vcores"></a>Esnek havuz: vCores değiştirme

Yükseltebilir veya ihtiyaçlarını kullanarak kaynağını temel bir esnek havuz performans düzeyine düşürebilirsiniz [Azure portal](sql-database-elastic-pool.md#manage-elastic-pools-and-databases-using-the-azure-portal), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [REST API](/rest/api/sql/elasticpools/update).

- Havuz vCores rescaling, veritabanı bağlantılarını kısaca bırakılır. As (olmayan bir havuzdaki) tek bir veritabanı için Dtu'lar rescaling oluşur aynı davranış budur. Süresini ve rescaling işlemleri sırasında bırakılan bağlantıları için bir veritabanı etkisi hakkında daha fazla bilgi için bkz: [tek bir veritabanı için Dtu'lar Rescaling](#single-database-change-storage-size). 
- Havuz vCores yeniden Ölçeklendir süresi havuzdaki tüm veritabanları tarafından kullanılan depolama alanının toplam miktarını bağlı olabilir. Genel olarak, rescaling gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu rescaling için beklenen gecikme süresi 3 saat sonra veya daha az. Bazı durumlarda standart veya temel katmanı içinde altında kullanılan alanı miktarının bağımsız olarak beş dakika rescaling gecikme olabilir.
- Genel olarak, veritabanı veya en çok vCores veritabanı başına başına min vCores değiştirmek için süre beş dakikadır veya daha az.
- Havuz vCores downsizing, kullanılan havuzu alanı hedef hizmet katmanı ve havuzu vCores boyutu izin verilen üst sınırdan küçük olması gerekir.

## <a name="what-is-the-maximum-number-of-servers-and-databases"></a>Sunucular ve veritabanları maksimum sayısı nedir?

| Maksimum | Değer |
| :--- | :--- |
| Sunucu başına veritabanları | 5000 |
| Her bölge abonelik başına sunucularının sayısı | 20 |
|||

> [!IMPORTANT]
> Sunucu başına izin verilen maksimum veritabanı sayısı yaklaşımı olarak aşağıdaki oluşabilir:
> <br> Sorguları ana veritabanına karşı çalışır • artırma gecikme süresi.  Bu, kaynak kullanımı istatistikleri sys.resource_stats gibi görünümlerini içerir.
> <br> • Yönetim işlemlerini gecikme artırma ve portal görüşlerini işleme, sunucunun veritabanlarında numaralandırma içerir.

## <a name="what-happens-when-database-and-elastic-pool-resource-limits-are-reached"></a>Veritabanı ve esnek havuz kaynak sınırlarını ulaşıldığında ne olur?

### <a name="compute-vcores"></a>İşlem (vCores)

(VCore kullanımı tarafından ölçülür) veritabanı işlem kullanımı yüksek olduğunda, sorgu gecikmesi artırır ve hatta zaman aşımı olabilir. Bu koşullar altında sorguları hizmeti tarafından sıraya alınmış ve kaynak olarak yürütme için kaynakları serbest hale sağlanır.
Yüksek işlem kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya veritabanı ile daha fazla vCores sağlamak için esnek havuz performans düzeyini artırma. Bkz: [tek veritabanı: cVcores değiştirme](#single-database-change-vcores) ve [esnek havuz: vCores değiştirmek](#elastic-pool-change-vcores).
- Her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

### <a name="storage"></a>Depolama

Kullanılan veritabanı alanı ulaştığında en büyük boyut sınırı, veritabanı ekler ve veri boyutunu artırın güncelleştirmeleri ve istemcileri alır bir [hata iletisi](sql-database-develop-error-messages.md). Veritabanı SEÇER ve SİLER devam başarılı olması.

Yüksek alan kullanımının karşılaşıldığında, azaltma seçenekleri şunlardır:

- Veritabanı veya esnek havuz ya da değişiklik maksimum depolama artırmak için performans düzeyinin en büyük boyutunu artırma. Bkz: [SQL veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits.md).
- Veritabanını bir esnek havuz ise, böylece kendi depolama alanını başka veritabanlarıyla paylaşılmayan sonra alternatif olarak veritabanı dışında havuzu taşınabilir.

### <a name="sessions-and-workers-requests"></a>Oturumlar ve çalışanları (istek sayısı) 

Maksimum sayısını oturumlar ve çalışan hizmet katmanını ve performans düzeyine göre belirlenir. Yeni istekler oturumu veya çalışan sınırları ulaştı ve istemciler bir hata iletisi alıyorsunuz reddedilir. Kullanılabilir bağlantı sayısı uygulama tarafından denetlenebilir olsa da, eşzamanlı çalışan sayısı genellikle tahmin etmek ve denetlemek daha zordur. Bu zaman veritabanı kaynak sınırları ulaştı ve uzun çalışan sorguları nedeniyle çalışanları üst üste yığmak yoğun yük dönemlerini sırasında özellikle doğrudur. 

Yüksek oturum ya da çalışan kullanımı karşılaşıldığında, azaltma seçenekleri şunlardır:
- Veritabanı veya esnek havuz hizmet katmanı veya performans düzeyini artırma. Bkz: [tek veritabanı: depolama boyutunu değiştirme](#single-database-change-storage-size), [tek veritabanı: vCores değiştirmek](#single-database-change-vcores), [esnek havuz: depolama boyutunu değiştirme](#elastic-pool-change-storage-size), ve [esnek havuz: vCores değiştirme ](#elastic-pool-change-vcores).
- İşlem kaynaklarını çakışması nedeniyle artan çalışan kullanımı neden olması durumunda her sorgu kaynak kullanımını azaltmak için en iyi duruma getirme sorgular. Daha fazla bilgi için bkz: [sorgu ayarlama/Hinting](sql-database-performance-guidance.md#query-tuning-and-hinting).

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
