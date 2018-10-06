---
title: Azure SQL veritabanı DTU tabanlı kaynak sınırları tek veritabanları | Microsoft Docs
description: Bu sayfa, Azure SQL veritabanı'nda tek veritabanları için bazı ortak DTU tabanlı kaynak sınırları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 10/04/2018
ms.openlocfilehash: 3f9d595b0a8dff8fce52769aaa12043632107ff5
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/06/2018
ms.locfileid: "48830417"
---
# <a name="resource-limits-for-single-databases-using-the-dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli kullanarak tek veritabanı kaynak sınırları 

Bu makalede, Azure SQL veritabanı tek veritabanı DTU tabanlı satın alma modeli kullanarak için ayrıntılı kaynak sınırları sağlar.

DTU tabanlı satın alma modeli kaynak sınırları için elastik havuzlar için bkz: [DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md). Sanal çekirdek tabanlı kaynak sınırları için bkz: [sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [sanal çekirdek tabanlı kaynak sınırları - elastik havuzlar](sql-database-vcore-resource-limits-elastic-pools.md). Farklı satın alma modeli hakkında daha fazla bilgi için bkz. [modelleri ve hizmet katmanlarını satın](sql-database-service-tiers.md). 

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="single-database-storage-sizes-and-compute-sizes"></a>Tek veritabanı: depolama boyutlarına ve işlem boyutları

Tek veritabanları için aşağıdaki tablolarda her hizmet katmanında tek bir veritabanı için kullanılabilir kaynakları göster ve işlem boyutu. Hizmet katmanı, işlem boyutu ve depolama alanı miktarı kullanarak tek veritabanı için ayarlayabileceğiniz [Azure portalında](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [PowerShell](sql-database-single-databases-manage.md#powershell-manage-logical-servers-and-databases), [Azure CLI](sql-database-single-databases-manage.md#azure-cli-manage-logical-servers-and-databases), veya [ REST API](sql-database-single-databases-manage.md#rest-api-manage-logical-servers-and-databases).

### <a name="basic-service-tier"></a>Temel hizmet katmanı
| **İşlem boyutu** | **Temel** |
| :--- | --: |
| Maks. DTU | 5 |
| Dahil edilen depolama alanı (GB) | 2 |
| En fazla depolama seçenekleri (GB) | 2 |
| Maks. bellek içi OLTP depolama alanı (GB) |Yok |
| Maks. eş zamanlı çalışan (istek) | 30 |
| Maks. eş zamanlı oturum | 300 |
|||

### <a name="standard-service-tier"></a>Standart hizmet katmanı
| **İşlem boyutu** | **S0** | **S1** | **S2** | **S3** |
| :--- |---:| ---:|---:|---:|
| Maks. DTU | 10 | 20 | 50 | 100 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250 | 250 | 250 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |
| Maks. eş zamanlı çalışan (istek)| 60 | 90 | 120 | 200 |
| Maks. eş zamanlı oturum |600 | 900 | 1200 | 2400 |
||||||

### <a name="standard-service-tier-continued"></a>Standart hizmet katmanında (devam)
| **İşlem boyutu** | **S4** | **S6** | **S7** | **S9** | **S12** |
| :--- |---:| ---:|---:|---:|---:|
| Maks. DTU | 200 | 400 | 800 | 1600 | 3000 |
| Dahil edilen depolama alanı (GB) | 250 | 250 | 250 | 250 | 250 |
| En fazla depolama seçenekleri (GB) | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 | 250, 500, 750, 1024 |
| Maks. bellek içi OLTP depolama alanı (GB) | Yok | Yok | Yok | Yok |Yok |
| Maks. eş zamanlı çalışan (istek)| 400 | 800 | 1600 | 3200 |6000 |
| Maks. eş zamanlı oturum |4800 | 9600 | 19200 | 30000 |30000 |
|||||||

### <a name="premium-service-tier"></a>Premium hizmet katmanı 
| **İşlem boyutu** | **P1** | **P2** | **P4** | **P6** | **P11** | **P15** | 
| :--- |---:|---:|---:|---:|---:|---:|
| Maks. DTU | 125 | 250 | 500 | 1000 | 1750 | 4000 |
| Dahil edilen depolama alanı (GB) | 500 | 500 | 500 | 500 | 4096 | 4096 |
| En fazla depolama seçenekleri (GB) | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 500, 750, 1024 | 4096 | 4096 |
| Maks. bellek içi OLTP depolama alanı (GB) | 1 | 2 | 4 | 8 | 14 | 32 |
| Maks. eş zamanlı çalışan (istek)| 200 | 400 | 800 | 1600 | 2400 | 6400 |
| Maks. eş zamanlı oturum | 30000 | 30000 | 30000 | 30000 | 30000 | 30000 |
|||||||


> [!IMPORTANT]
> 1 TB'den fazla depolama Premium katmanında şu anda aşağıdakiler dışındaki tüm bölgelerde: Batı Orta ABD, Doğu Çin, USDoDCentral, Almanya Orta, USDoDEast, ABD Devleti Southwest, Almanya Kuzeydoğu, USGovIowa, Çin Kuzey. Diğer bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. Bkz. [P11 P15 Geçerli Sınırlamalar](#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

## <a name="single-database-change-storage-size"></a>Tek veritabanı: depolama boyutunu değiştirme

- Belirli miktarda bir ek maliyet olmadan depolama tek veritabanı DTU ücretini içerir. Dahil edilen miktarın üzerinde ek depolama alanı 1 TB'kurmak 250 GB'lık artışlarla ve 1 TB ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet sağlanabilir. Dahil edilen depolama alanı miktarları ve en büyük boyutu sınırlar için bkz: [tek veritabanı: depolama boyutlarına ve bilgi işlem boyutlarına](#single-database-storage-sizes-and-compute-sizes).
- En büyük boyutu kullanarak artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Azure portalında](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), veya [REST API](/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının hizmet katmanı ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarı fiyatıdır. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="single-database-change-dtus"></a>Tek veritabanı: Dtu Değiştir

Başlangıçta bir hizmet katmanı, işlem boyutu ve depolama alanı miktarı seçtikten sonra tek bir veritabanının ölçeğini artırıp dinamik olarak gerçek deneyime kullanımına dayalı ölçekleme yapabilirsiniz [Azure portalında](sql-database-single-databases-manage.md#azure-portal-manage-logical-servers-and-databases), [Transact-SQL](/sql/t-sql/statements/alter-database-azure-sql-database#examples), [ PowerShell](/powershell/module/azurerm.sql/set-azurermsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), veya [REST API](/rest/api/sql/databases/update). 

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

## <a name="single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb"></a>Tek veritabanı: P11 ve P15 1 TB'den büyük, maksimum boyutu sınırlamaları

Boyut sınırı için P11 ve P15 veritabanı aşağıdaki bölgelerde desteklenir 1 TB'den büyük: Avustralya Doğu, Avustralya Güneydoğu, Brezilya Güney, Kanada Orta, Kanada Doğu, Orta ABD, Fransa Orta, Almanya Orta, Japonya Doğu, Japonya Batı, Kore Orta Kuzey Orta ABD, Kuzey Avrupa, Güney Orta ABD, Güneydoğu Asya, UK Güney, UK Batı, ABD doğu2, Batı ABD, ABD Devleti Virginia ve Batı Avrupa. P11 ve P15 veritabanları en büyük boyutu 1 TB'den büyük ile aşağıdaki önemli noktalar ve sınırlamalar geçerlidir:

- Desteklenmeyen bir bölgede veritabanı sağlandığında create komutuyla en büyük boyutu 1 TB'den büyük (4 TB veya 4096 GB değeri kullanılarak) bir veritabanı oluşturulurken seçerseniz, bir hata ile başarısız olur.
- Desteklenen bölgelerden birinde bulunan mevcut P11 ve P15 veritabanları için en fazla depolama için 1 TB ötesinde 256 GB'lık artışlarla artırabilirsiniz 4 TB'a kadar. Daha büyük boyutta bölgenizde desteklenip desteklenmediğini görmek için [DATABASEPROPERTYEX](/sql/t-sql/functions/databasepropertyex-transact-sql) işlevi veya Azure portalında veritabanı boyutu inceleyin. Bir mevcut P11 veya P15 yükseltme veritabanı yalnızca bir sunucu düzeyi asıl oturum açma veya dbmanager veritabanı rolünün üyeleri tarafından gerçekleştirilebilir. 
- Bir yükseltme işlemi desteklenen bir bölgede yürütülürse yapılandırma hemen güncelleştirilir. Veritabanı yükseltme işlemi sırasında çevrimiçi kalır. Ancak, gerçek veritabanı dosyalarını yeni maksimum boyuta yükseltilene dek tam 1 TB'a kadar depolama alanı dışında depolama miktarını faydalanamaz. Gereken süre uzunluğunu yükseltilmekte olan veritabanının boyutuna bağlıdır. 
- Oluşturma veya güncelleştirme P11 veya P15 veritabanı, yalnızca 256 GB'lık artışlarla en büyük boyutu 1 TB ile 4 TB arasında seçim yapabilirsiniz. P11/P15 oluştururken, varsayılan depolama alanı 1 TB'lık önceden seçilmiş seçenektir. Desteklenen bölgelerden birinde bulunan veritabanları için en fazla 4 TB'ın yeni veya mevcut bir tek veritabanı için depolama en artırabilirsiniz. Diğer tüm bölgeler için 1 TB üst sınırı yükseltilemez. Dahil edilen depolama 4 TB'ı seçtiğinizde fiyat değiştirmez.
- Bir veritabanının en yüksek boyutu 1 TB'den büyük ayarlanırsa, 1 TB kullanılan gerçek depolama olsa bile, ardından 1 TB ile değiştirilemez. Bu nedenle, en fazla bir 1 TB P11 ya da 1 TB P15 1 TB'den büyük P11 veya P15 düşürme veya alt boyutu, P1-P6 gibi işlem). Belirli bir noktaya, dahil olmak üzere kopyalama senaryoları ve geri yükleme için de bu kısıtlamanın uygulandığı coğrafi geri yükleme, uzun-vadeli-yedekleme-elde tutma ve veritabanı kopyası. En fazla boyutu 1 TB'den büyük olan bir veritabanı yapılandırıldıktan sonra bu veritabanının tüm geri yükleme işlemleri en büyük boyutu 1 TB'den büyük P11/P15 çalıştırmanız gerekir.
- Etkin coğrafi çoğaltma senaryoları için:
   - Bir coğrafi çoğaltma ilişkisi ayarlama: birincil veritabanının P11 veya P15 ise secondary(ies) da P11 veya P15; olması gerekir 1 TB'den fazla destekleme kapasitesine sahip olmadığından, alt bilgi işlem boyutlarına ikincil veritabanı reddedilir.
   - Coğrafi çoğaltma ilişkisinde birincil veritabanını yükseltmeden: 1 TB'den fazla birincil veritabanında en büyük boyutunu değiştirmek, ikincil veritabanında aynı değişikliği tetikler. Her iki yükseltmeleri değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil 1 TB'den fazla desteklemeyen bir bölgede, birincil yükseltilmez.
- 1 TB'den fazla P11/P15 veritabanlarını yüklemek için içeri/dışarı aktarma hizmetini kullanarak desteklenmiyor. Kullanmak için SqlPackage.exe [alma](sql-database-import.md) ve [dışarı](sql-database-export.md) veri.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [SQL veritabanı SSS](sql-database-faq.md) sık sorulan soruların yanıtları için.
- Bkz: [kaynak bakış sınırlayan bir mantıksal sunucuda](sql-database-resource-limits-logical-server.md) sunucusu ve abonelik düzeyinde sınırları hakkında daha fazla bilgi için.
- Genel Azure sınırları hakkında daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](../azure-subscription-service-limits.md).
- Dtu'lar ve Edtu'lar hakkında daha fazla bilgi için bkz: [Dtu'lar ve Edtu'lar](sql-database-service-tiers.md#dtu-based-purchasing-model).
- Tempdb boyutu sınırları hakkında daha fazla bilgi için bkz: https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database.
