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
ms.date: 3/14/2019
ms.openlocfilehash: d8aaf51c836a8e88c4e9b92798067167cd044e72
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60848114"
---
# <a name="scale-elastic-pool-resources-in-azure-sql-database"></a>Azure SQL veritabanı elastik havuz kaynakları ölçeklendirin

Bu makalede, Azure SQL veritabanı'nda elastik havuzlara ve havuza alınmış veritabanları için kullanılabilen işlem ve depolama kaynaklarının ölçeğini açıklar.

## <a name="change-compute-resources-vcores-or-dtus"></a>Değişiklik işlem kaynakları (sanal çekirdekler veya Dtu)

Başlangıçta çekirdek veya Edtu sayısını seçtikten sonra elastik havuz ölçeğini artırıp dinamik olarak gerçek deneyime kullanımına dayalı ölçeklendirebileceğiniz [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI ](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [REST API](https://docs.microsoft.com/rest/api/sql/elasticpools/update).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanı veya ölçeklendirme işlem boyutu değiştirme etkisi

Değiştirme Hizmet katmanı veya işlem boyutu, bir elastik havuzun tek veritabanları için benzer bir desen izler ve çoğunlukla hizmeti için aşağıdaki adımları içerir:

1. Esnek havuz için yeni bir işlem örneği oluşturun  

    Esnek havuz için yeni bir işlem örneği işlem boyutu ve istenen hizmet katmanı ile oluşturulur. Hizmet katmanı ve işlem boyut değişikliklerine bazı birleşimleri, her veritabanı çoğaltmasını hangi veri kopyalamayı yeni bilgi işlem örneği oluşturulmalıdır ve toplam gecikme süresi kesin etkileyebilir. Ne olursa olsun, veritabanları, bu adım sırasında çevrimiçi kalan ve özgün işlem örneği içindeki veritabanlarına yönelik bağlantılar devam.

2. Bağlantılar yeni bir işlem örneğine yönlendirilmesini geçiş

    Özgün işlem örneği veritabanları için mevcut bağlantılar kesilir. Herhangi bir yeni bağlantı veritabanlarını yeni bilgi işlem örneği oluşturulur. Hizmet katmanı ve işlem boyut değişikliklerine bazı birleşimleri için veritabanı dosyalarını kullanımdan çıkarıldı ve geçiş sırasında eklenemeyeceği.  Ne olursa olsun, veritabanları genellikle 30 saniyeden kısa ve genellikle yalnızca birkaç saniye kullanılamadığında anahtar kısa kesintisine neden olabilir. Uzun süre çalışan varsa bu adımı süresi bağlantıları bırakıldığında çalışan işlemler, Durdurulan işlem sayısı kurtarmak için uzun sürebilir. [Veritabanı kurtarma hızlandırılmış](sql-database-accelerated-database-recovery.md) işlemleri uzun süre çalışan iptal etmesini etkisini azaltır.

> [!IMPORTANT]
> Herhangi bir iş akışı adımı sırasında veri kaybedilmez.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanı veya ölçeklendirme işlem boyutu değiştirme gecikme süresi

Hizmet katmanını değiştirebilir veya işlem boyutu tek bir veritabanı veya elastik Havuzu'nu yeniden ölçeklendirmek için gecikme süresi gibi parametreli:

|Hizmet katmanı|Temel tek veritabanı</br>Standart (S1 S0)|Temel esnek havuz</br>Standart (S2-S12) </br>Hiper ölçekli, </br>Genel amaçlı tek veritabanı'nı veya elastik Havuzu'nu|Premium veya iş açısından kritik tek veritabanı'nı veya elastik Havuzu'nu|
|:---|:---|:---|:---|
|**Temel tek veritabanı</br> standart (S1 S0)**|&bull; &nbsp;Kullanılan alan bağımsız sabit zaman gecikmesi</br>&bull; &nbsp;Genellikle, küçüktür 5 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|
|**Temel esnek havuz </br>Standard (S2-S12) </br>hiper ölçekli, </br>genel amaçlı tek veritabanı'nı veya elastik Havuzu'nu**|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Kullanılan alan bağımsız sabit zaman gecikmesi</br>&bull; &nbsp;Genellikle, küçüktür 5 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|
|**Premium veya iş açısından kritik tek veritabanı'nı veya elastik Havuzu'nu**|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|

> [!NOTE]
>
> - Hizmet katmanını değiştirmeniz veya işlem bir elastik havuz için ölçeklendirme, havuzdaki tüm veritabanları arasında kullanılan bir alanı toplamayı tahmin hesaplamak için kullanılması gerekir.
> - Bir veritabanını bir elastik havuzun içine/dışına taşıma söz konusu olduğunda, yalnızca veritabanı tarafından kullanılan alanı gecikme değil elastik havuzu tarafından kullanılan alanı etkiler.
>
> [!TIP]
> Devam eden işlemleri izlemek için bkz: [İşlemleri SQL REST API kullanarak yönetmek](https://docs.microsoft.com/rest/api/sql/operations/list), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) ve [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Değiştirilirken ek hususlar hizmet katmanı veya ölçeklendirme işlem boyutu

- Sanal çekirdekler ya da elastik havuzlar için Edtu downsizing, kullanılan havuzu alanını hedef hizmet katmanı ve havuz edtu'ları, izin verilen boyut üst sınırı aşıyor küçük olması gerekir.
- Sanal çekirdekler ya da elastik havuzlar için edtu'ları ölçeklendirme, ek depolama alanı maliyeti havuz (1) depolama maksimum boyutunu hedef havuz tarafından desteklenir ve (2) depolama boyutu hedef havuzunun dahil edilen depolama miktarını aşıyor durumunda geçerlidir. 100 eDTU standart havuzda en fazla 100 GB boyutlu, 50 eDTU standart havuzda downsized, örneğin, ardından bir ek depolama alanı bir en büyük boyutu 100 GB'lık hedef havuzu destekler ve yalnızca 50 GB, dahil edilen depolama miktarını olduğundan alanı miktarları ücrete tabidir. Bu nedenle, ek depolama alanı miktarı 100 GB – 50 GB = 50 GB olur. Ek depolama fiyatlandırması için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Gerçek kullanılan alanı miktarı dahil edilen depolama alanı miktarı azsa, daha sonra bu ek maliyet veritabanı boyutu için dahil edilen miktarın azaltarak önlenebilir.

### <a name="billing-during-rescaling"></a>Ölçeklendirme sırasında faturalandırma

Size en yüksek hizmet katmanı kullanarak bir veritabanının mevcut olduğu her saat için faturalandırılırsınız + işlem kullanıma veya veritabanının bir saatten az için etkin olup bağımsız olarak bu saat sırasında uygulanan boyutu. Örneğin, tek bir veritabanı oluşturup beş dakika sonra silerseniz faturanıza bir veritabanı saati ücreti yansıtır.

## <a name="change-elastic-pool-storage-size"></a>Esnek havuz depolama boyutunu değiştir

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

### <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

- En büyük boyut sınırı en fazla depolama alanı sağlanabilir:

  - Depolama, standart veya genel amaçlı hizmet katmanlarında sunulduğundan, artırmak veya azaltmak boyutu 10 GB'lık artışlarla
  - İş açısından kritik veya premium depolama için hizmet katmanları, artırmak veya azaltmak boyutu 250 GB'lık artışlarla
- Elastik havuz için depolama, artan ya da en büyük boyutunu azaltarak sağlanabilir.
- Elastik havuz için depolama hizmeti katmanının depolama birimi fiyatı ile çarpılan depolama alanı miktarı fiyatıdır. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

- Belirli miktarda bir ek maliyet olmadan depolama için elastik havuz eDTU fiyatı içerir. Dahil edilen miktarın üzerinde ek depolama alanı 1 TB'kurmak 250 GB'lık artışlarla ve 1 TB ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet sağlanabilir. Dahil edilen depolama alanı miktarları ve en büyük boyutu sınırlar için bkz: [elastik havuz: depolama boyutlarına ve bilgi işlem boyutlarına](sql-database-dtu-resource-limits-elastic-pools.md#elastic-pool-storage-sizes-and-compute-sizes).
- En büyük boyutu kullanarak artırarak bir elastik havuz için ek depolama alanı sağlanabilir [Azure portalında](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](/powershell/module/az.sql/Get-AzSqlElasticPool), [Azure CLI](/cli/azure/sql/elastic-pool#az-sql-elastic-pool-update), veya [REST API ](https://docs.microsoft.com/rest/api/sql/elasticpools/update).
- Elastik havuz için ek depolama alanının fiyatı, hizmet katmanı ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarı ' dir. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak limitleri için bkz. [SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-elastic-pools.md).
