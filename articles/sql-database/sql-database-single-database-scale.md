---
title: Ölçek tek veritabanı kaynakları - Azure SQL veritabanı | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı'nda işlem ve depolama kaynaklarını tek bir veritabanının ölçeğini açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: carlrab
manager: craigg
ms.date: 04/26/2019
ms.openlocfilehash: 1048b4e2ac3a8523d5539ddc1a1bdaca3ec2d912
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074254"
---
# <a name="scale-single-database-resources-in-azure-sql-database"></a>Azure SQL veritabanı'nda ölçek tek veritabanı kaynakları

Bu makalede sağlanan işlem katmanında bir tek veritabanı için kullanılabilen işlem ve depolama kaynaklarını ölçeklendirmek nasıl açıklar. Alternatif olarak, [(Önizleme) sunucusuz bilgi işlem katmanı](sql-database-serverless.md) işlem otomatik ölçeklendirme ve faturalar için kullanılan işlem saniyede sağlar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

## <a name="change-compute-size-vcores-or-dtus"></a>(Sanal çekirdekler veya Dtu) işlem boyutunu değiştir

Başlangıçta çekirdek veya Dtu sayısını seçtikten sonra tek bir veritabanı yukarı veya aşağı dinamik olarak gerçek deneyime kullanımına dayalı ölçeklendirebilirsiniz [Azure portalında](sql-database-single-databases-manage.md#manage-an-existing-sql-database-server), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [ PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), veya [REST API](https://docs.microsoft.com/rest/api/sql/databases/update).

Aşağıdaki video gösterildiği hizmet dinamik olarak değiştirme katmanı ve tek bir veritabanı için kullanılabilir Dtu'lar artırmak için boyutu işlem.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

### <a name="impact-of-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanı veya ölçeklendirme işlem boyutu değiştirme etkisi

Değiştirme Hizmet katmanı veya işlem tek bir veritabanının boyutunu çoğunlukla hizmeti için aşağıdaki adımları içerir:

1. Veritabanı için yeni bir işlem örneği oluşturun  

    İstenen hizmet katmanı ve işlem boyutu ile veritabanı için yeni bir işlem örneği oluşturulur. Bazı birleşimleri hizmet katmanı ve işlem boyut değişiklikleri için veritabanı çoğaltmasını hangi veri kopyalamayı yeni işlem örneğinde oluşturulmalı ve toplam gecikme süresi kesin etkileyebilir. Ne olursa olsun, bu adım sırasında veritabanı çevrimiçi kalır ve bağlantıları özgün işlem örneği veritabanına yönlendirilebilir devam eder.

2. Bağlantılar yeni bir işlem örneğine yönlendirilmesini geçiş

    Özgün işlem örneği veritabanına mevcut bağlantılar kesilir. Herhangi bir yeni bağlantı veritabanına yeni bilgi işlem örneği oluşturulur. Hizmet katmanı ve işlem boyut değişikliklerine bazı birleşimleri için veritabanı dosyalarını kullanımdan çıkarıldı ve geçiş sırasında eklenemeyeceği.  Ne olursa olsun, veritabanı kullanılabilir olduğunda genellikle 30 saniyeden kısa ve genellikle yalnızca birkaç saniye anahtar kısa kesintisine neden olabilir. Uzun süre çalışan varsa bu adımı süresi bağlantıları bırakıldığında çalışan işlemler, Durdurulan işlem sayısı kurtarmak için uzun sürebilir. [Veritabanı kurtarma hızlandırılmış](sql-database-accelerated-database-recovery.md) işlemleri uzun süre çalışan iptal etmesini etkisini azaltır.

> [!IMPORTANT]
> Herhangi bir iş akışı adımı sırasında veri kaybedilmez.

### <a name="latency-of-changing-service-tier-or-rescaling-compute-size"></a>Hizmet katmanı veya ölçeklendirme işlem boyutu değiştirme gecikme süresi

Hizmet katmanını değiştirebilir veya işlem boyutu tek bir veritabanı veya elastik Havuzu'nu yeniden ölçeklendirmek için gecikme süresi gibi parametreli:

|Hizmet katmanı|Temel tek veritabanı</br>Standart (S1 S0)|Temel esnek havuz</br>Standart (S2-S12) </br>Hiper ölçekli, </br>Genel amaçlı tek veritabanı'nı veya elastik Havuzu'nu|Premium veya iş açısından kritik tek veritabanı'nı veya elastik Havuzu'nu|
|:---|:---|:---|:---|
|**Temel tek veritabanı</br> standart (S1 S0)**|&bull; &nbsp;Kullanılan alan bağımsız sabit zaman gecikmesi</br>&bull; &nbsp;Genellikle, küçüktür 5 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|
|**Temel esnek havuz </br>Standard (S2-S12) </br>hiper ölçekli, </br>genel amaçlı tek veritabanı'nı veya elastik Havuzu'nu**|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Kullanılan alan bağımsız sabit zaman gecikmesi</br>&bull; &nbsp;Genellikle, küçüktür 5 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|
|**Premium veya iş açısından kritik tek veritabanı'nı veya elastik Havuzu'nu**|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|&bull; &nbsp;Veri kopyalama nedeniyle kullanılan veritabanı alanı orantılı bir gecikme</br>&bull; &nbsp;Genellikle, küçüktür alanı kullanılan GB başına 1 dakika|

> [!TIP]
> Devam eden işlemleri izlemek için bkz: [İşlemleri SQL REST API kullanarak yönetmek](https://docs.microsoft.com/rest/api/sql/operations/list), [CLI kullanarak işlemlerini yönetmek](/cli/azure/sql/db/op), [T-SQL kullanarak işlemlerini izleyin](/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) ve bu iki PowerShell komutları: [Get-AzSqlDatabaseActivity](/powershell/module/az.sql/get-azsqldatabaseactivity) ve [Stop-AzSqlDatabaseActivity](/powershell/module/az.sql/stop-azsqldatabaseactivity).

### <a name="cancelling-service-tier-changes-or-compute-rescaling-operations"></a>Hizmet katmanı değişikliklerini veya ölçeklendirme işlemleri işlem iptal ediliyor

Bir hizmet katmanı değiştirebilir veya işlem ölçeklendirme işlemi iptal edilebilir.

#### <a name="azure-portal"></a>Azure portal

Veritabanına genel bakış dikey penceresinde, gitmek **bildirimleri** ve devam eden bir işlemi belirten kutucuğuna tıklayın:

![Devam eden işlemi](media/sql-database-single-database-scale/ongoing-operations.png)

Ardından, etiketli düğmeye **bu işlemi iptal**.

![Devam eden işlemi iptal et](media/sql-database-single-database-scale/cancel-ongoing-operation.png)

#### <a name="powershell"></a>PowerShell

Bir PowerShell komut isteminde ayarlama `$ResourceGroupName`, `$ServerName`, ve `$DatabaseName`, ve ardından aşağıdaki komutu çalıştırın:

```PowerShell
$OperationName = (az sql db op list --resource-group $ResourceGroupName --server $ServerName --database $DatabaseName --query "[?state=='InProgress'].name" --out tsv)
if(-not [string]::IsNullOrEmpty($OperationName))
    {
        (az sql db op cancel --resource-group $ResourceGroupName --server $ServerName --database $DatabaseName --name $OperationName)
        "Operation " + $OperationName + " has been canceled"
    }
    else
    {
        "No service tier change or compute rescaling operation found"
    }
```

### <a name="additional-considerations-when-changing-service-tier-or-rescaling-compute-size"></a>Değiştirilirken ek hususlar hizmet katmanı veya ölçeklendirme işlem boyutu

- Daha yüksek bir hizmet katmanına yükseltme veya boyutu işlem, veritabanı boyutu üst sınırını (maxsıze) daha büyük bir boyut, açıkça belirtmediğiniz sürece artırmaz.
- Bir veritabanını indirgemek için kullanılan veritabanı boş alanı hedef hizmet katmanı boyutu ve işlem boyutu izin verilen üst sınırdan küçük olması gerekir.
- Öğesinden önceki sürüme indirirken **Premium** için **standart** katmanı, bir ek depolama alanı miktarları ücrete tabidir hem hedef işlem boyutu (1) veritabanının maksimum boyutunu desteklenir ve dahil edilen (2) en büyük boyutu aşıyor işlem boyutu hedef depolama miktarı. En fazla 500 GB boyutlu bir P1 veritabanı, S3 downsized, örneğin, ardından bir ek depolama alanı S3 500 GB'lık bir en büyük boyutu destekler ve kendi dahil edilen depolama miktarını yalnızca 250 GB olduğundan alanı miktarları ücrete tabidir. Bu nedenle, ek depolama alanı miktarı 500 GB – 250 GB = 250 GB ' dir. Ek depolama fiyatlandırması için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/). Gerçek kullanılan alanı miktarı dahil edilen depolama alanı miktarı azsa, daha sonra bu ek maliyet veritabanı boyutu için dahil edilen miktarın azaltarak önlenebilir.
- Bir veritabanını yükseltmek [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ikincil veritabanlarını istenilen hizmet katmanına yükseltme ve boyutunu (en iyi performans için genel kılavuz) birincil veritabanını yükseltmeden önce işlem. Farklı bir yükseltme sırasında ikincil veritabanını yükseltmeden önce gereklidir.
- Bir veritabanı ile alt sürüme düşürürken [coğrafi çoğaltma](sql-database-geo-replication-portal.md) etkin ve birincil veritabanlarını istenen hizmet katmanı için eski sürümü yükleme boyutu (en iyi performans için genel kılavuz) ikincil veritabanı indirgemeden önce işlem. Farklı bir sürüme alt sürüme düşürürken birincil veritabanı eski sürüme düşürme ilk gereklidir.
- Geri yükleme hizmeti teklifleri, çeşitli hizmet katmanları için farklılık gösterir. İçin indiriyorsanız **temel** katmanı, daha düşük bir yedekleme bekletme süresi vardır. Bkz: [Azure SQL veritabanı yedeklemelerini](sql-database-automated-backups.md).
- Veritabanının yeni özellikleri, değişiklikler tamamlanana kadar uygulanmaz.

### <a name="billing-during-compute-rescaling"></a>İşlem ölçeklendirme sırasında faturalandırma

Size en yüksek hizmet katmanı kullanarak bir veritabanının mevcut olduğu her saat için faturalandırılırsınız + işlem kullanıma veya veritabanının bir saatten az için etkin olup bağımsız olarak bu saat sırasında uygulanan boyutu. Örneğin, tek bir veritabanı oluşturup beş dakika sonra silerseniz faturanıza bir veritabanı saati ücreti yansıtır.

## <a name="change-storage-size"></a>Depolama boyutunu değiştir

### <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

- 1 GB artışlarla kullanarak en büyük boyut sınırı en fazla depolama alanı sağlanabilir. Minimum yapılandırılabilir veri depolama 5 GB'tır
- Artan veya azalan en büyük boyutu kullanarak tek bir veritabanı için depolama alanı sağlanabilir [Azure portalında](https://portal.azure.com), [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), veya [REST API](https://docs.microsoft.com/rest/api/sql/databases/update).
- SQL veritabanı otomatik olarak ek depolama alanı % 30'luk 32 GB ve günlük dosyaları için sanal çekirdek TempDB için ancak 384 GB aşmayacak şekilde ayırır. TempDB ekli SSD'LERİN tüm hizmet katmanlarında bulunur.
- Tek bir veritabanı için depolama alanının fiyatı hizmeti katmanının depolama birimi fiyatı ile çarpılan veri depolama ve günlük depolama tutarlarının toplamıdır. Tempdb maliyeti, sanal çekirdek fiyatına dahildir. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

### <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

- Belirli miktarda bir ek maliyet olmadan depolama tek veritabanı DTU ücretini içerir. Dahil edilen miktarın üzerinde ek depolama alanı 1 TB'kurmak 250 GB'lık artışlarla ve 1 TB ötesinde 256 GB'lık artışlarla maksimum boyut sınırına kadar ek bir maliyet sağlanabilir. Dahil edilen depolama alanı miktarları ve en büyük boyutu sınırlar için bkz: [tek veritabanı: Depolama boyutlarına ve bilgi işlem boyutlarına](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes).
- Azure portalını kullanarak en büyük boyutunu artırarak tek bir veritabanı için ek depolama alanı sağlanabilir [Transact-SQL](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current#examples-1), [PowerShell](/powershell/module/az.sql/set-azsqldatabase), [Azure CLI](/cli/azure/sql/db#az-sql-db-update), veya [ REST API](https://docs.microsoft.com/rest/api/sql/databases/update).
- Ek depolama alanı için tek bir veritabanının hizmet katmanı ek depolama alanı birim fiyatı ile çarpılan ek depolama alanı miktarı fiyatıdır. Ek depolama alanının fiyatı hakkında daha fazla bilgi için bkz: [SQL veritabanı fiyatlandırması](https://azure.microsoft.com/pricing/details/sql-database/).

> [!IMPORTANT]
> Bazı durumlarda, kullanılmayan alanı geri kazanmak için bir veritabanı daraltma gerekebilir. Daha fazla bilgi için [Azure SQL veritabanı'nda dosya alanı yönetmek](sql-database-file-space-management.md).

## <a name="p11-and-p15-constraints-when-max-size-greater-than-1-tb"></a>P11 ve P15 kısıtlamaları, en fazla 1 TB'den büyük boyut

1 TB'den fazla depolama Premium katmanında şu anda tüm bölgelerde kullanılabilir: Çin Doğu, Kuzey Çin, Almanya Orta, Almanya Kuzeydoğu, Batı Orta ABD, US DoD bölgeler ve ABD kamu orta. Bu bölgelerde Premium katmanda depolama için 1 TB üst sınırı uygulanır. P11 ve P15 veritabanları en büyük boyutu 1 TB'den büyük ile aşağıdaki önemli noktalar ve sınırlamalar geçerlidir:

- En büyük boyutu P11 veya P15 veritabanı için hiç olmadığı kadar bir değere 1 TB'den büyük ayarlandıysa, ardından bunu yalnızca geri veya P11 veya P15 veritabanına kopyalar.  Ardından, ölçeklendirme işlemi sırasındaki ayrılan alan miktarını yeni işlem boyutu en büyük boyut sınırlarını aşmayacak sağlanan veritabanı farklı işlem boyutuna boyutlandırılan.
- Etkin coğrafi çoğaltma senaryoları için:
  - Coğrafi çoğaltma ilişki kurma: Birincil veritabanı P11 veya P15 ise secondary(ies) ayrıca P11 veya P15 olmalıdır; alt işlem boyutu 1 TB'den fazla destekleme kapasitesine sahip olmadığından ikincil veritabanı reddedilir.
  - Coğrafi çoğaltma ilişkisinde birincil veritabanı yükseltiliyor: En büyük boyutu 1 TB'den fazla birincil veritabanında değiştirmek, ikincil veritabanında aynı değişikliği tetikler. Her iki yükseltmeleri değişikliğin etkili olması için birincil başarılı olması gerekir. Birden fazla 1 TB seçeneği için bölge sınırlamalar uygulanır. İkincil 1 TB'den fazla desteklemeyen bir bölgede, birincil yükseltilmez.
- 1 TB'den fazla P11/P15 veritabanlarını yüklemek için içeri/dışarı aktarma hizmetini kullanarak desteklenmiyor. Kullanmak için SqlPackage.exe [alma](sql-database-import.md) ve [dışarı](sql-database-export.md) veri.

## <a name="next-steps"></a>Sonraki adımlar

Genel kaynak limitleri için bkz. [SQL veritabanı sanal çekirdek tabanlı kaynak sınırları - tek veritabanları](sql-database-vcore-resource-limits-single-databases.md) ve [SQL veritabanı DTU tabanlı kaynak sınırları - elastik havuzlar](sql-database-dtu-resource-limits-single-databases.md).
