---
title: Esnek havuz kaynakları - Azure SQL Database ölçeği | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanında esnek havuzlar için ölçeklendirme kaynaklar açıklanır.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 0d15382f413485ccc287bac945b3c88eb748a9f6
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313321"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Esnek havuz kaynakları Azure SQL veritabanı'nda ölçeklendirme

Bu makalede, Azure SQL veritabanı'nda esnek havuzlar ve havuza alınmış veritabanları için kullanılabilen işlem ve depolama kaynaklarını ölçeklendirme açıklar. 

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>satın alma modeli vCore tabanlı: esnek havuz depolama boyutunu değiştirme

- En büyük boyut sınırı en fazla depolama alanı sağlanabilir: 
 - Standart depolama artırın veya 10 GB artışlarla boyutunu azaltın
 - Premium depolama artırın veya boyutu 250 GB artışlarla azaltın
- Esnek havuz için depolama artırarak veya azaltarak en büyük boyutuna sağlanabilir.
- Esnek havuz için depolama hizmet katmanının depolama birimi fiyatı ile çarpılan depolama miktarına fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>satın alma modeli vCore tabanlı: değişiklik esnek havuz işlem kaynaklarını (vCores)

Yükseltebilir veya ihtiyaçlarını kullanarak kaynağını temel bir esnek havuz performans düzeyine düşürebilirsiniz [Azure portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [REST API](/rest/api/sql/elasticpools/update).

- Havuz vCores rescaling, veritabanı bağlantılarını kısaca bırakılır. As (olmayan bir havuzdaki) tek bir veritabanı için Dtu'lar rescaling oluşur aynı davranış budur. Süresini ve rescaling işlemleri sırasında bırakılan bağlantıları için bir veritabanı etkisi hakkında daha fazla bilgi için bkz: [tek bir veritabanı için Dtu'lar Rescaling](#single-database-change-storage-size). 
- Havuz vCores yeniden Ölçeklendir süresi havuzdaki tüm veritabanları tarafından kullanılan depolama alanının toplam miktarını bağlı olabilir. Genel olarak, rescaling gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu rescaling için beklenen gecikme süresi 3 saat sonra veya daha az. Bazı durumlarda standart veya temel katmanı içinde altında kullanılan alanı miktarının bağımsız olarak beş dakika rescaling gecikme olabilir.
- Genel olarak, veritabanı veya en çok vCores veritabanı başına başına min vCores değiştirmek için süre beş dakikadır veya daha az.
- Havuz vCores downsizing, kullanılan havuzu alanı hedef hizmet katmanı ve havuzu vCores boyutu izin verilen üst sınırdan küçük olması gerekir.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>DTU tabanlı satın alma modeli: esnek havuz depolama boyutunu değiştirme

- Bir esnek havuz eDTU fiyat belirli bir miktarda depolama hiçbir ek ücret ödemeden içerir. Dahil edilen miktar ötesinde ek depolama alanı için en fazla 1 TB 250 GB artışlarla ve 1 TB ötesinde 256 GB artışlarla en büyük boyut sınırına kadar ek bir maliyet sağlanabilir. Eklenen depolama tutarlar ve en büyük boyut sınırları için bkz [esnek havuz: depolama boyutlarına ve performans düzeyleri](#elastic-pool-storage-sizes-and-performance-levels).
- En büyük boyut kullanılarak artırarak esnek havuz için ek depolama alanı sağlanabilir [Azure portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [REST API'si ](/rest/api/sql/elasticpools/update).
- Esnek havuz için ek depolama alanı hizmet katmanına ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarını fiyatıdır. Ek depolama alanı fiyatı hakkında daha fazla bilgi için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>DTU tabanlı satın alma modeli: değişiklik esnek havuz işlem kaynaklarını (Edtu'lar)

Artırmak veya ihtiyaçlarını kullanarak kaynağını temel bir esnek havuz için kullanılabilir kaynakları azaltmak [Azure portal](sql-database-elastic-pool-scale.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqlelasticpool), [Azure CLI](/cli/azure/sql/elastic-pool#az_sql_elastic_pool_update), veya [ REST API](/rest/api/sql/elasticpools/update).

- Havuz Edtu rescaling, veritabanı bağlantılarını kısaca bırakılır. As (olmayan bir havuzdaki) tek bir veritabanı için Dtu'lar rescaling oluşur aynı davranış budur. Süresini ve rescaling işlemleri sırasında bırakılan bağlantıları için bir veritabanı etkisi hakkında daha fazla bilgi için bkz: [tek bir veritabanı için Dtu'lar Rescaling](#single-database-change-storage-size). 
- Havuz Edtu yeniden Ölçeklendir süresi havuzdaki tüm veritabanları tarafından kullanılan depolama alanının toplam miktarını bağlı olabilir. Genel olarak, rescaling gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu rescaling için beklenen gecikme süresi 3 saat sonra veya daha az. Bazı durumlarda standart veya temel katmanı içinde altında kullanılan alanı miktarının bağımsız olarak beş dakika rescaling gecikme olabilir.
- Genel olarak, veritabanı veya maksimum Edtu başına veritabanı başına minimum edtu'larını değiştirmek için süre beş dakikadır veya daha az.
- Havuz Edtu downsizing, kullanılan havuzu alanı hedef hizmet katman ve havuz edtu'larını boyutu izin verilen üst sınırdan küçük olması gerekir.
- Havuz Edtu rescaling, ek depolama alanı maliyeti (1 depolama en büyük havuz boyutu hedef havuzu tarafından desteklenir ve (2 depolama en büyük boyut hedef havuzu dahil depolama miktarını aşarsa uygulanır. 100 eDTU standart havuzu en büyük boyutu 100 GB ile 50 standart havuz eDTU downsized, örneğin, daha sonra ek depolama alanı maliyeti en büyük boyutu 100 GB hedef havuzu destekler ve dahil edilen depolama miktarını yalnızca 50 GB olduğundan geçerlidir. Bu nedenle, ek depolama alanı miktarı 100 GB – 50 GB = 50 GB'tır. Ek depolama fiyatlandırma için bkz: [SQL Database fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan gerçek miktarını dahil depolama miktarına küçükse, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. 

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak sınırları için bkz: [SQL veritabanı vCore tabanlı kaynak sınırları - esnek havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - esnek havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).
