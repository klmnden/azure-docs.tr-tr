---
title: Bir Azure SQL veritabanını bir yedekten geri | Microsoft Docs
description: Bir Azure SQL veritabanı (35 gün) zamandaki önceki bir noktaya geri olanak tanıyan, zaman içinde nokta geri yükleme hakkında bilgi edinin.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.date: 04/04/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: 027a10e687673bdeedf2858b4c23ff459df61b70
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34649117"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Otomatik veritabanı yedeklerini kullanarak bir Azure SQL veritabanını kurtarma
SQL veritabanı kullanarak veritabanı kurtarma için bu seçenekleri sağlar [veritabanı yedeklemeleri otomatik](sql-database-automated-backups.md) ve [uzun vadeli bekletme yedeklemeleri](sql-database-long-term-retention.md). Bir veritabanı yedeğinden geri yükleyebilirsiniz:

* Saklama dönemi içinde zaman içinde belirtilen bir nokta kurtarılan aynı mantıksal sunucuda yeni bir veritabanı. 
* Silinen bir veritabanını silme süresi kurtarılan aynı mantıksal sunucu bir veritabanı.
* Coğrafi olarak çoğaltılmış blob depolama (RA-GRS) en son günlük yedekleri noktasına kurtarılır bölgedeki herhangi bir mantıksal sunucuda yeni bir veritabanı.

> [!IMPORTANT]
> Varolan bir veritabanını geri yükleme sırasında üzerine yazılamıyor.
>

Geri yüklenen veritabanı aşağıdaki koşullarda bir ek depolama alanı maliyet oluşturur: 
- Veritabanı boyutu üst sınırını 500 GB'den büyükse, P11 – P15 S4 S12 veya P1 – P6 geri yükleme.
- Veritabanı boyutu 250 GB'den büyükse P1 – P6 S4 S12 için geri yükleme.

Ek maliyetidir geri yüklenen veritabanının en büyük boyut için performans düzeyi dahil depolama miktarını daha büyük olduğundan ve dahil edilen miktar sağlanan ek depolama alanı ekstra ücret kesilir.  Ek depolama alanı ayrıntılarını fiyatlandırma için bkz: [SQL veritabanı fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/sql-database/).  Kullanılan gerçek miktarını dahil depolama miktarından daha az ise, ardından bu ekstra maliyet veritabanı boyutu üst sınırını dahil edilen miktarını azaltarak önlenebilir. Veritabanı Depolama boyutları ve veritabanı en büyük boyutunu değiştirme hakkında daha fazla bilgi için bkz: [tek veritabanı DTU tabanlı kaynak sınırları](sql-database-dtu-resource-limits.md#single-database-storage-sizes-and-performance-levels) ve [tek veritabanı vCore tabanlı kaynak sınırları](sql-database-vcore-resource-limits.md#single-database-storage-sizes-and-performance-levels).  

> [!NOTE]
> [Veritabanı Yedeklemeleri otomatik](sql-database-automated-backups.md) , oluştururken kullanılan bir [veritabanı kopyalama](sql-database-copy.md). 
>


## <a name="recovery-time"></a>Kurtarma zamanı
Otomatik veritabanı yedeklerini kullanarak bir veritabanını geri yüklemek için kurtarma süresini çeşitli faktörler tarafından etkilenir: 

* Veritabanının boyutu
* Veritabanı performans düzeyi
* İşlem günlükleri söz konusu sayısı
* Geri yükleme noktası kurtarmak için yeniden yürütülmesi gereken etkinlik miktarı
* Geri yükleme farklı bir bölgeye ise ağ bant genişliği 
* Hedef bölgede işlenmekte olan eşzamanlı geri yükleme isteği sayısı. 
  
  İçin çok büyük ve/veya etkin bir veritabanı, geri yükleme birkaç saat sürebilir. Bir bölgede uzun süren kesinti ise, çok sayıda coğrafi geri yükleme isteği ülkeler tarafından işlenmekte olan mümkündür. Birçok istek olduğunda, bu bölgedeki veritabanları için kurtarma süresini artırabilir. Çoğu veritabanı 12 saat içinde tam geri yükler.

Tek bir abonelik için vardır (geri yükleme, coğrafi geri yükleme ve uzun vadeli bekletme yedekten geri yükleme noktası dahil) eşzamanlı geri yükleme isteklerinin sayısı bazı sınırlamalar gönderildi ve proceeded:
|  | **En fazla işlenmekte olan eşzamanlı istek sayısı** | **Max gönderilmesini eşzamanlı istek sayısı** |
| :--- | --: | --: |
|Tek veritabanı (her abonelik)|10|60|
|Esnek havuz (her havuzu)|4|200|
||||

Geri yükleme toplu olarak yerleşik bir işlevi yoktur. [Azure SQL Database: tam sunucu kurtarma](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) komut dosyası bu görevi gerçekleştirmeye bir yolu bir örnektir.

> [!IMPORTANT]
> Otomatik yedeklemeler kullanarak kurtarmak için SQL Server katılımcı rolü Abonelikteki bir üyesi olmanız veya abonelik sahibi olmanız gerekir. Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız. 
> 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Azure portalını kullanarak aynı mantıksal sunucuda yeni bir veritabanı olarak zaman içinde önceki bir noktaya varolan bir veritabanını geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), veya [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Bir veritabanının bir zaman içinde nokta geri yüklemeyi gerçekleştirmek nasıl gösteren bir örnek PowerShell komut dosyası için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
>

Veritabanı tüm hizmet katmanı veya performans düzeyini ve tek veritabanı olarak veya bir esnek havuz içine geri yüklenebilir. Mantıksal sunucu veya veritabanı geri yüklediğiniz esnek havuzda yeterli kaynaklara sahip olun. Tamamlandıktan sonra geri yüklenen veritabanının normal, tamamen erişilebilir, çevrimiçi bir veritabanı değil. Geri yüklenen veritabanı kendi hizmet katmanını ve performans düzeyi temelinde normal hızlarında doludur. Veritabanı geri yükleme işlemi tamamlanana kadar ücrete tabi değildir.

Genellikle daha önceki bir noktaya kurtarma amacıyla bir veritabanını geri. Bunun yapılması, geri yüklenen veritabanının özgün veritabanı için bir yedek olarak kabul eder ya da verilerin alınacağı ve özgün veritabanını güncelleştirmek için kullanın. 

* ***Veritabanı değiştirme:*** geri yüklenen veritabanının özgün veritabanı için bir yedek olarak amaçlanıyorsa, performans düzeyinin doğrulamanız gerekir ve/veya hizmet katmanı uygundur ve gerekirse, veritabanı ölçeklendirme. Özgün veritabanını yeniden adlandırın ve ardından özgün adı kullanarak geri yüklenen veritabanı verin [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL komutu. 
* ***Veri kurtarma:*** bir kullanıcı veya uygulama hatadan kurtarmak için geri yüklenen veritabanından veri almak planlıyorsanız, yazma ve özgün veritabanına geri yüklenen veritabanından veri ayıklamak için gerekli verileri kurtarma betikleri çalıştırmak gerekir. Geri yükleme işleminin tamamlanması uzun zaman alabilir ancak geri yükleme veritabanı geri yükleme işlemi boyunca veritabanı listesinde görünür olur. Veritabanını geri yükleme sırasında silerseniz, geri yükleme işlemi iptal edilir ve geri yükleme tamamlanmadı veritabanı için sizden ücret istenmese. 

### <a name="azure-portal"></a>Azure portalına

Azure portalını kullanarak zamanında bir noktaya kurtarmak için veritabanınız için sayfasını açın ve **geri** araç çubuğunda.

![noktası içinde zaman geri yükleme](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Silinen bir veritabanını geri yükleme
Azure portalını kullanarak aynı sunucuda mantıksal silinen bir veritabanını silme saate silinen bir veritabanını geri [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), veya [REST (createMode = geri yükle)](https://msdn.microsoft.com/library/azure/mt163685.aspx). Bekletme kullanarak sırasında zaman içinde önceki bir noktaya silinen bir veritabanını geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!TIP]
> Silinen bir veritabanını geri yükleme gösteren bir örnek PowerShell komut dosyası için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Bir Azure SQL veritabanı sunucusu örneği silerseniz, tüm veritabanlarını da silinir ve kurtarılamaz. Şu anda silinen bir sunucuya geri yüklemek için destek yok.
> 

### <a name="azure-portal"></a>Azure portalına

Sırasında silinen bir veritabanını kurtarmak için kendi [DTU tabanlı modeli saklama dönemi](sql-database-service-tiers-dtu.md) veya [vCore tabanlı modeli saklama dönemi](sql-database-service-tiers-vcore.md) Azure Portalı'nı kullanarak, sunucunuz ve işlemleri alanında sayfasını açın,'ı tıklatın **Veritabanlarını sildi**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Coğrafi Geri Yükleme
Bir SQL veritabanı herhangi bir Azure bölgesine herhangi bir sunucuda en son coğrafi olarak çoğaltılmış tam ve fark yedeklerden geri yükleyebilirsiniz. Coğrafi geri yükleme, coğrafi olarak yedekli yedekleme, kaynağı olarak kullanır ve veritabanı veya veri merkezi kesinti nedeniyle erişilemez durumda olsa bile bir veritabanını kurtarmak için kullanılabilir. 

Veritabanı barındırıldığı bölgede bir olay nedeniyle veritabanınızı kullanılamaz duruma geldiğinde coğrafi geri yükleme varsayılan kurtarma seçeneğidir. Büyük ölçekli olay durumunda olmadığından, veritabanı uygulamanızın bir bölge sonuçları, bir veritabanı coğrafi olarak çoğaltılmış yedeklemelerden başka bir bölgede bulunan bir sunucuya geri yükleyebilirsiniz. Ne zaman değişiklik yedeklemesi alınır ve bir Azure coğrafi olarak çoğaltılmış olduğu zaman arasında bir gecikme olur blob farklı bir bölgede. Bu gecikme en çok bir saat, bu nedenle, olağanüstü bir durum oluşursa, olabilir yukarı bir saat veri kaybı olabilir. Aşağıdaki çizimde başka bir bölgede son kullanılabilir yedekten geri yükleme veritabanı gösterir.

![coğrafi geri yükleme](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Bir coğrafi geri yükleme gerçekleştirmek nasıl gösteren bir örnek PowerShell komut dosyası için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
> 

Zaman içinde nokta geri yükleme coğrafi ikincil şu anda desteklenmiyor. Zaman içinde nokta geri yükleme, yalnızca birincil veritabanı üzerinde gerçekleştirilebilir. Bir kesintisinden kurtarma için coğrafi geri yükleme kullanma hakkında ayrıntılı bilgi için bkz: [bir kesintisinden kurtarma](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Kurtarma yedeklerden olağanüstü durum kurtarma çözümleri SQL veritabanındaki en uzun kurtarma noktası hedefi (RPO) ve tahmin kurtarma süresi (Ekle) ile kullanılabilir en temel ' dir. Küçük boyutu veritabanları (örneğin temel hizmet katmanını veya esnek havuzlar veritabanlarında Kiracı küçük boyut) kullanan çözümler için coğrafi geri yükleme sık bir makul DR 12 saatlik bir Ekle ile çözümüdür. Çözümleri kullanarak büyük veritabanları ve kısa kurtarma gerektiren kez kullanmayı düşünmelisiniz [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md). Aktif coğrafi çoğaltma, bir çok daha kısa RPO ve Ekle yalnızca gerektirir gibi sunar sürekli olarak çoğaltılmış ikincil bir yük devretme başlatın. İş sürekliliği seçenekleri hakkında daha fazla bilgi için bkz: [iş sürekliliği genel bakış](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure portalına

Coğrafi geri yükleme sırasında veritabanı bir için kendi [DTU tabanlı modeli saklama dönemi](sql-database-service-tiers-dtu.md) veya [vCore tabanlı modeli saklama dönemi](sql-database-service-tiers-vcore.md) Azure Portalı'nı kullanarak, SQL veritabanları sayfasını açın ve ardından **Ekle** . İçinde **kaynak seçme** metin kutusunda seçin **yedekleme**. Hangi bölgede ve tercih ettiğiniz sunucusundaki kurtarma gerçekleştirmek yedekleme belirtin. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Program aracılığıyla otomatik yedekleme kullanarak kurtarma gerçekleştirme
Daha önce ele alındığı gibi Azure portalına ek olarak, veritabanı kurtarma program aracılığıyla Azure PowerShell veya REST API'si kullanılarak gerçekleştirilebilir. Aşağıdaki tablolar kullanılabilir komutlar kümesi açıklamaktadır.

### <a name="powershell"></a>PowerShell
| Cmdlet | Açıklama |
| --- | --- |
| [Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase) |Bir veya daha fazla veritabanını alır. |
| [Get-AzureRMSqlDeletedDatabaseBackup](/powershell/module/azurerm.sql/get-azurermsqldeleteddatabasebackup) | Geri yükleyebileceğiniz, silinmiş bir veritabanını alır. |
| [Get-AzureRmSqlDatabaseGeoBackup](/powershell/module/azurerm.sql/get-azurermsqldatabasegeobackup) |Bir veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
| [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) |SQL veritabanını geri yükler. |
|  | |

### <a name="rest-api"></a>REST API
| API | Açıklama |
| --- | --- |
| [REST (createMode kurtarma =)](https://msdn.microsoft.com/library/azure/mt163685.aspx) |Bir veritabanını geri yükler |
| [Get oluştur veya veritabanı durumunu güncelleştir](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Durumu geri yükleme işlemi sırasında döndürür |
|  | |

## <a name="summary"></a>Özet
Otomatik yedekleme, kullanıcı ve uygulama hataları, yanlışlıkla veritabanı silme ve uzun süren kesintileri veritabanlarınızı koruyun. Bu yerleşik yetenek tüm hizmet katmanları ve performans düzeyleri için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Veritabanı Yedeklemeleri otomatik Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı yedeklemeleri otomatik](sql-database-automated-backups.md).
* Uzun vadeli bekletme hakkında bilgi edinmek için [uzun vadeli bekletme](sql-database-long-term-retention.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md).  
