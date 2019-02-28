---
title: Elastik havuz kaynak - Azure SQL veritabanı ölçeklendirme | Microsoft Docs
description: Bu sayfada Azure SQL veritabanındaki elastik havuzlar için ölçeklendirme kaynaklar açıklanır.
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
ms.date: 02/07/2019
ms.openlocfilehash: 2726c1fd08414f112035d5378fc7e395ca7ace4c
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2019
ms.locfileid: "56959094"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Azure SQL veritabanı elastik havuz kaynakları ölçeklendirin

Bu makalede, Azure SQL veritabanı'nda elastik havuzlara ve havuza alınmış veritabanları için kullanılabilen işlem ve depolama kaynaklarının ölçeğini açıklar.

## <a name="vcore-based-purchasing-model-change-elastic-pool-storage-size"></a>Sanal çekirdek tabanlı satın alma modeli: Esnek havuz depolama boyutunu değiştir

- En büyük boyut sınırı en fazla depolama alanı sağlanabilir:

  - Depolama, standart veya genel amaçlı hizmet katmanlarında sunulduğundan, artırmak veya azaltmak boyutu 10 GB'lık artışlarla
  - İş açısından kritik veya premium depolama için hizmet katmanları, artırmak veya azaltmak boyutu 250 GB'lık artışlarla
- Elastik havuz için depolama, artan ya da en büyük boyutunu azaltarak sağlanabilir.
- Elastik havuz için depolama hizmeti katmanının depolama birimi fiyatı ile çarpılan depolama alanı miktarı fiyatıdır. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="vcore-based-purchasing-model-change-elastic-pool-compute-resources-vcores"></a>Sanal çekirdek tabanlı satın alma modeli: Elastik havuzunu Değiştir bilgi işlem kaynakları (sanal çekirdek)

Artırmak veya azaltmak kaynağını ihtiyaçlarını kullanarak temel bir elastik havuz için işlem boyutu [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [ REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

- Bir elastik havuzdaki Vcore ölçeklendirme, veritabanı bağlantıları kısaca bırakılır. Bu davranış için tek veritabanı Dtu ölçeklendirme ortaya çıkan aynı davranıştır. Bir veritabanı için ölçeklendirme işlemleri sırasında kesilen bağlantıları etkisini ve süresi hakkında daha fazla bilgi için bkz. [değişiklik bilgi işlem kaynakları (Dtu)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- Havuzu sanal çekirdekler yeniden Ölçeklendir süresi, havuzdaki tüm veritabanları tarafından kullanılan depolama alanı toplam miktarına bağlı olabilir. Genel olarak, ölçeklendirme gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu ölçeklendirme için beklenen gecikme süresi 3 saat ise ya da daha az. Standart veya temel katmanında bazı durumlarda, beş dakikadan kısa kullanılan alanı miktarından bağımsız olarak ölçeklendirme gecikme olabilir.
- Genel olarak, veritabanı başına veritabanı veya en çok sanal çekirdek başına minimum sanal çekirdekler değiştirmek için süre beş dakikadır veya daha az.
- Havuzu sanal çekirdekler downsizing, kullanılan havuzu alanını hedef hizmet katmanı ve havuzu sanal çekirdekler, izin verilen boyut üst sınırı aşıyor küçük olması gerekir.

## <a name="dtu-based-purchasing-model-change-elastic-pool-storage-size"></a>DTU tabanlı satın alma modeli: Esnek havuz depolama boyutunu değiştir

- Belirli miktarda bir ek maliyet olmadan depolama için elastik havuz eDTU fiyatı içerir. Dahil edilen miktarın üzerinde ek depolama alanı 1 TB'kurmak 250 GB'lık artışlarla ve 1 TB ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet sağlanabilir. Dahil edilen depolama alanı miktarları ve en büyük boyutu sınırlar için bkz: [elastik havuz: depolama boyutlarına ve bilgi işlem boyutlarına](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- En büyük boyutu kullanarak artırarak bir elastik havuz için ek depolama alanı sağlanabilir [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [REST API ](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- Elastik havuz için ek depolama alanının fiyatı, hizmet katmanı ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarı ' dir. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="dtu-based-purchasing-model-change-elastic-pool-compute-resources-edtus"></a>DTU tabanlı satın alma modeli: İşlem kaynakları (Edtu) esnek havuzunu Değiştir

Artırmak veya azaltmak kaynağını ihtiyaçlarını kullanarak temel bir elastik havuz için kullanılabilir kaynakları [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [ REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

- Havuz edtu'ları ölçeklendirme, veritabanı bağlantıları kısaca bırakılır. Bu davranış için tek veritabanı Dtu ölçeklendirme ortaya çıkan aynı davranıştır. Bir veritabanı için ölçeklendirme işlemleri sırasında kesilen bağlantıları etkisini ve süresi hakkında daha fazla bilgi için bkz. [değişiklik bilgi işlem kaynakları (Dtu)](sql-database-single-database-scale.md#dtu-based-purchasing-model-change-compute-resources-dtus).
- Havuz edtu'ları yeniden Ölçeklendir süresi, havuzdaki tüm veritabanları tarafından kullanılan depolama alanı toplam miktarına bağlı olabilir. Genel olarak, ölçeklendirme gecikme süresi 90 dakika ortalama başına 100 GB veya daha az. Örneğin, toplam alanı tarafından kullanılan havuzdaki tüm veritabanları ise 200 GB havuzu ölçeklendirme için beklenen gecikme süresi 3 saat ise ya da daha az. Standart veya temel katmanında bazı durumlarda, beş dakikadan kısa kullanılan alanı miktarından bağımsız olarak ölçeklendirme gecikme olabilir.
- Genel olarak, veritabanı veya en fazla Edtu başına veritabanı başına Min. Edtu değiştirmek için süre beş dakikadır veya daha az.
- Elastik havuzlar için Edtu downsizing, kullanılan havuzu alanını hedef hizmet katmanı ve havuz edtu'ları, izin verilen boyut üst sınırı aşıyor küçük olması gerekir.
- Elastik havuzlar için edtu'ları ölçeklendirme, ek depolama alanı maliyeti havuz (1) depolama maksimum boyutunu hedef havuz tarafından desteklenir ve (2) depolama boyutu hedef havuzunun dahil edilen depolama miktarını aşıyor durumunda geçerlidir. 100 eDTU standart havuzda en fazla 100 GB boyutlu, 50 eDTU standart havuzda downsized, örneğin, ardından bir ek depolama alanı bir en büyük boyutu 100 GB'lık hedef havuzu destekler ve yalnızca 50 GB, dahil edilen depolama miktarını olduğundan alanı miktarları ücrete tabidir. Bu nedenle, ek depolama alanı miktarı 100 GB – 50 GB = 50 GB olur. Ek depolama fiyatlandırması için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Gerçek kullanılan alanı miktarı dahil edilen depolama alanı miktarı azsa, daha sonra bu ek maliyet veritabanı boyutu için dahil edilen miktarın azaltarak önlenebilir.

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak limitleri için bkz. [SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).
