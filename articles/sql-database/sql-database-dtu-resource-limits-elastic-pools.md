---
title: Azure SQL veritabanı DTU tabanlı kaynak sınırlar esnek havuzlar | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanında esnek havuzlar için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
author: CarlRabeler
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 06/20/2018
ms.author: carlrab
ms.openlocfilehash: 08dabf1ad66f69c5e0f55aedbc2a4d0bb265a0bd
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752240"
---
# <a name="resources-limits-for-elastic-pools-using-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli kullanarak esnek havuzlar için kaynakları sınırları 

Bu makale, Azure SQL Database esnek havuzlar ve DTU tabanlı satın alma modeli kullanarak havuza alınmış veritabanları için ayrıntılı kaynak sınırları sağlar. 

DTU tabanlı satın alma modeli kaynak sınırları için tek veritabanları için bkz: [DTU tabanlı kaynak sınırları - tek veritabanlarını](sql-database-vcore-resource-limits-elastic-pools.md). VCore tabanlı kaynak sınırları için bkz: [vCore tabanlı kaynak sınırları - tek veritabanlarını](sql-database-vcore-resource-limits-single-databases.md) ve [vCore tabanlı kaynak sınırları - esnek havuzlar](sql-database-vcore-resource-limits-elastic-pools.md).

## <a name="elastic-pool-storage-sizes-and-performance-levels"></a>Esnek havuz: depolama boyutlarına ve performans düzeyleri

SQL Database esnek havuzlar için aşağıdaki tablolarda her hizmeti katmanını ve performans düzeyinde kaynakları gösterir. Hizmet katmanı, performans düzeyi ve depolama miktarını kullanarak ayarlayabilirsiniz [Azure portal](sql-database-elastic-pool-manage.md#azure-portal-manage-elastic-pools-and-pooled-databases), [PowerShell](sql-database-elastic-pool-manage.md#powershell-manage-elastic-pools-and-pooled-databases), [Azure CLI](sql-database-elastic-pool-manage.md#azure-cli-manage-elastic-pools-and-pooled-databases), veya [REST API](sql-database-elastic-pool-manage.md#rest-api-manage-elastic-pools-and-pooled-databases).

> [!NOTE]
> Esnek havuzlar bulunan tek veritabanlarını kaynak sınırları genellikle Dtu'lar ve Hizmet katmanını temel alan havuzları dışında tek veritabanları ile aynıdır. Örneğin, S2 veritabanı için en fazla eşzamanlı çalışan 120 çalışanları olur. Bu nedenle, standart havuzdaki bir veritabanı için en fazla eşzamanlı çalışan olduğunu da 120 çalışanları havuzunda veritabanı başına maksimum DTU (hangi S2 eşdeğerdir) 50 Dtu'lar ise.

### <a name="basic-elastic-pool-limits"></a>Temel esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800** | **1200** | **1600** |
|:---|---:|---:|---:| ---: | ---: | ---: | ---: | ---: |
| Eklenen depolama havuzu (GB) | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| Havuz (GB) başına en fazla depolama seçenekleri | 5 | 10 | 20 | 29 | 39 | 78 | 117 | 156 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | Yok | Yok |
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 500 | 500 |
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 | 2400 | 3200 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 |30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 | 0, 5 |
| Veritabanı başına maksimum Edtu seçenekleri | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Veritabanı başına maks. depolama alanı (GB) | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 2 | 
||||||||

### <a name="standard-elastic-pool-limits"></a>Standart esnek havuz sınırları

| Havuz başına eDTU | **50** | **100** | **200** | **300** | **400** | **800**| 
|:---|---:|---:|---:| ---: | ---: | ---: | 
| Eklenen depolama havuzu (GB) | 50 | 100 | 200 | 300 | 400 | 800 | 
| Havuz (GB) başına en fazla depolama seçenekleri | 50, 250, 500 | 100, 250, 500, 750 | 200, 250, 500, 750, 1024 | 300, 500, 750, 1024, 1280 | 400, 500, 750, 1024, 1280, 1536 | 800, 1024, 1280, 1536, 1792, 2048 | 
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 100 | 200 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 100 | 200 | 400 | 600 | 800 | 1600 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
| Veritabanı başına minimum Edtu seçenekleri | 0, 10, 20, 50 | 0, 10, 20, 50, 100 | 0, 10, 20, 50, 100, 200 | 0, 10, 20, 50, 100, 200, 300 | 0, 10, 20, 50, 100, 200, 300, 400 | 0, 10, 20, 50, 100, 200, 300, 400, 800 |
| Veritabanı başına maksimum Edtu seçenekleri | 10, 20, 50 | 10, 20, 50, 100 | 10, 20, 50, 100, 200 | 10, 20, 50, 100, 200, 300 | 10, 20, 50, 100, 200, 300, 400 | 10, 20, 50, 100, 200, 300, 400, 800 | 
| Veritabanı başına maks. depolama alanı (GB) | 500 | 750 | 1024 | 1024 | 1024 | 1024 |
||||||||

### <a name="standard-elastic-pool-limits-continued"></a>Standart esnek havuz limitleri (devam) 

| Havuz başına eDTU | **1200** | **1600** | **2000** | **2500** | **3000** |
|:---|---:|---:|---:| ---: | ---: |
| Eklenen depolama havuzu (GB) | 1200 | 1600 | 2000 | 2500 | 3000 | 
| Havuz (GB) başına en fazla depolama seçenekleri | 1200, 1280, 1536, 1792, 2048, 2304, 2560 | 1600, 1792, 2048, 2304, 2560, 2816, 3072 | 2000, 2048, 2304, 2560, 2816, 3072, 3328, 3584 | 2500, 2560, 2816, 3072, 3328, 3584, 3840, 4096 | 3000, 3072, 3328, 3584, 3840, 4096 |
| En fazla bellek içi OLTP depolama havuzu (GB) | Yok | Yok | Yok | Yok | Yok | 
| Havuz başına en fazla veritabanı | 500 | 500 | 500 | 500 | 500 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 2400 | 3200 | 4000 | 5000 | 6000 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına minimum Edtu seçenekleri | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 0, 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 |
| Veritabanı başına maksimum Edtu seçenekleri | 10, 20, 50, 100, 200, 300, 400, 800, 1200 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500 | 10, 20, 50, 100, 200, 300, 400, 800, 1200, 1600, 2000, 2500, 3000 | 
| Veritabanı (GB) başına en fazla depolama seçenekleri | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits"></a>Premium esnek havuz sınırları

| Havuz başına eDTU | **125** | **250** | **500** | **1000** | **1500**| 
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 250 | 500 | 750 | 1024 | 1536 | 
| Havuz (GB) başına en fazla depolama seçenekleri | 250, 500, 750, 1024 | 500, 750, 1024 | 750, 1024 | 1024 | 1536 |
| En fazla bellek içi OLTP depolama havuzu (GB) | 1 | 2 | 4 | 10 | 12 | 
| Havuz başına en fazla veritabanı | 50 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 200 | 400 | 800 | 1600 | 2400 | 
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına Min. eDTU | 0, 25, 50, 75, 125 | 0, 25, 50, 75, 125, 250 | 0, 25, 50, 75, 125, 250, 500 | 0, 25, 50, 75, 125, 250, 500, 1000 | 0, 25, 50, 75, 125, 250, 500, 1000, 1500 | 
| Veritabanı başına Maks. eDTU | 25, 50, 75, 125 | 25, 50, 75, 125, 250 | 25, 50, 75, 125, 250, 500 | 25, 50, 75, 125, 250, 500, 1000 | 25, 50, 75, 125, 250, 500, 1000, 1500 |
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

### <a name="premium-elastic-pool-limits-continued"></a>Premium esnek havuz limitleri (devam) 

| Havuz başına eDTU | **2000** | **2500** | **3000** | **3500** | **4000**|
|:---|---:|---:|---:| ---: | ---: | 
| Eklenen depolama havuzu (GB) | 2048 | 2560 | 3072 | 3548 | 4096 |
| Havuz (GB) başına en fazla depolama seçenekleri | 2048 | 2560 | 3072 | 3548 | 4096|
| En fazla bellek içi OLTP depolama havuzu (GB) | 16 | 20 | 24 | 28 | 32 |
| Havuz başına en fazla veritabanı | 100 | 100 | 100 | 100 | 100 | 
| Havuz başına maks. eş zamanlı çalışan (istek) | 3200 | 4000 | 4800 | 5600 | 6400 |
| Havuz başına maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 
| Veritabanı başına minimum Edtu seçenekleri | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750 | 0, 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maksimum Edtu seçenekleri | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750 | 25, 50, 75, 125, 250, 500, 1000, 1750, 4000 | 
| Veritabanı başına maks. depolama alanı (GB) | 1024 | 1024 | 1024 | 1024 | 1024 | 
||||||||

> [!IMPORTANT]
> Birden fazla 1 TB depolama alanı Premium katmanındaki şu anda kullanılabilir aşağıdakiler dışında tüm bölgelerde: Birleşik Krallık Kuzey Batı Orta ABD, İngiltere South2, Çin, Doğu, USDoDCentral, Almanya Orta, USDoDEast, ABD kamu Güneybatı, BİZE kamu Güney Merkezi, Almanya Kuzeydoğu, Çin Kuzey, ABD kamu Doğu. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

Bir elastik havuzun tüm DTU’ları kullanılırsa, sorguları işlemek üzere havuzdaki her bir veritabanı eşit miktarda kaynak alır. SQL Veritabanı hizmeti, eşit dilimlerde işlem süresi sunarak veritabanları arasında kaynak paylaşım eşitliğini sağlar. Elastik havuz kaynak paylaşımı eşitliği, veritabanı başına DTU dakikası sıfır olmayan bir değere ayarlandığında her bir veritabanı için garanti edilen herhangi bir kaynak miktarına ek niteliktedir.

### <a name="database-properties-for-pooled-databases"></a>Havuza alınmış veritabanları için veritabanı özellikleri

Aşağıdaki tabloda, havuza alınmış veritabanları için özellikleri açıklar.

| Özellik | Açıklama |
|:--- |:--- |
| Veritabanı başına Maks. eDTU |Havuzdaki diğer veritabanlarının kullanımına göre mevcutsa, havuzdaki herhangi bir veritabanının kullanabileceği en fazla eDTU sayısı. Veritabanı başına en fazla eDTU, veritabanı için kaynak garantisi anlamına gelmez. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı kullanımının en üst seviyeye çıktığı durumlarla baş edebilmek için veritabanı başına en fazla eDTU sayısını yeterince yüksek bir değere ayarlayın. Havuz genellikle tüm veritabanlarının eşzamanlı olarak en üst kullanım seviyesine çıkmadığı sıcak ve soğuk kullanım modellerini varsaydığından, bir miktar aşırı ayırma beklenir. Örneğin, veritabanı başına en yüksek kullanımın 20 eDTU olduğunu ve havuzdaki 100 veritabanının yalnızca %20’sinin aynı anda en yüksek seviyeye çıktığını varsayalım. Veritabanı başına en fazla eDTU değeri 20 eDTU’ya ayarlanırsa, havuzun 5 kat fazla kullanılması ve havuz başına eDTU sayısının 400’e ayarlanması makuldür. |
| Veritabanı başına Min. eDTU |Havuzdaki herhangi bir veritabanının kullanabileceği en az eDTU sayısı garanti edilir. Bu ayar havuzdaki tüm veritabanları için geçerli olan genel bir ayardır. Veritabanı başına en az eDTU sayısı 0’a ayarlanabilir; bu değer aynı zamanda varsayılan değerdir. Bu özellik, 0 ile veritabanı başına ortalama eDTU kullanımı arasında bir değere ayarlanır. Havuzdaki veritabanı sayısının veritabanı başına en az eDTU sayısıyla çarpımı, havuz başına eDTU sayısını aşamaz. Örneğin, bir havuzda 20 veritabanı varsa ve veritabanı başına en az eDTU sayısı 10 eDTU’ya ayarlanırsa, havuzdaki eDTU sayısı en az 200 eDTU olmalıdır. |
| Veritabanı başına en fazla depolama |Bir veritabanı için kullanıcı tarafından bir havuzda ayarlanan en fazla veritabanı boyutu. Ancak, havuza alınmış veritabanları ayrılmış havuz depolama paylaşır. Olsa bile toplam en fazla depolama *veritabanı başına* toplam kullanılabilir depolama alanına büyük olacak şekilde ayarlanmıştır *havuzunun alanı*, aslında tüm veritabanları tarafından kullanılan toplam alanı aşan mümkün olmayacaktır kullanılabilir havuz sınırı. En büyük veritabanı boyutu, veri dosyalarının en büyük boyuta başvuruyor ve günlük dosyaları tarafından kullanılan alanı içermez. |
|||
 


## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz: [Azure aboneliği ve hizmet sınırları, kotaları ve kısıtlamaları](../azure-subscription-service-limits.md).
- Dtu ve Edtu hakkında daha fazla bilgi için bkz: [Dtu ve Edtu](sql-database-service-tiers.md#what-are-database-transaction-units-dtus).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
