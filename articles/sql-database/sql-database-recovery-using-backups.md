---
title: Azure SQL veritabanını bir yedekten geri yükleyin. | Microsoft Docs
description: Azure SQL veritabanı (en fazla 35 gün) zaman içinde önceki bir noktaya geri olanak tanıyan, zaman içinde nokta geri yükleme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 09/14/2018
ms.openlocfilehash: 4c9edd60ffa1cd9ed5d95b37592fa49f44117818
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161344"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Otomatik veritabanı yedeklerini kullanarak bir Azure SQL veritabanını kurtarma
SQL veritabanı kullanarak veritabanı kurtarma için bu seçenekleri sağlar [otomatik veritabanı yedeklemelerini](sql-database-automated-backups.md) ve [uzun süreli saklama kapsamındaki yedekleri](sql-database-long-term-retention.md). Veritabanı yedeklemesini geri yükleyebilirsiniz:

* Saklama dönemi içinde zaman içinde belirli bir noktaya kurtarılan aynı mantıksal sunucu üzerinde yeni bir veritabanı. 
* Silinen bir veritabanını silme süresine, kurtarılır aynı mantıksal sunucu üzerinde bir veritabanı.
* Noktasına en son günlük yedeklemeler blob coğrafi olarak yedekli depolama (RA-GRS), kurtarılır herhangi bir bölgedeki herhangi bir mantıksal sunucuda yeni bir veritabanı.

> [!IMPORTANT]
> Varolan bir veritabanını geri yükleme sırasında üzerine yazılamıyor.
>

Geri yüklenen veritabanı aşağıdaki koşullarda bir ek depolama alanı maliyeti doğurur: 
- Veritabanı boyutu 500 GB'den daha büyük ise, P11 – P15 S4-S12 veya P1 – P6 geri yükleyin.
- Veritabanı boyutu 250 GB'tan büyük ise, P1 – P6 S4-S12 için geri yükleme.

Ekstra maliyeti, çünkü geri yüklenen veritabanının maksimum boyutunu işlem boyutu için dahil edilen depolama miktarını büyüktür ve dahil edilen miktarın üzerinde sağlanan ek depolama ek ücret alınır.  Fiyatlandırma ayrıntıları ek depolama alanının bkz [SQL veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/).  Gerçek kullanılan alanı miktarı dahil edilen depolama alanı miktarı azsa, daha sonra bu ek maliyet veritabanı boyutu için dahil edilen miktarın azaltarak önlenebilir.  

> [!NOTE]
> [Veritabanı Yedeklemeleri otomatik](sql-database-automated-backups.md) , oluştururken kullanılan bir [veritabanı kopyalama](sql-database-copy.md). 
>


## <a name="recovery-time"></a>Kurtarma zamanı
Otomatik veritabanı yedeklerini kullanarak bir veritabanını geri yüklemek için kurtarma zamanı çeşitli faktörler tarafından etkilenir: 

* Veritabanı boyutu
* Veritabanı işlem boyutu
* İşlem günlükleri dahil sayısı
* Geri yükleme noktasına kurtarmak için yeniden yürütülmesi gereken etkinlik miktarı
* Geri yükleme farklı bir bölgeye ise ağ bant genişliği 
* Hedef bölgede işlenmekte olan geri eş zamanlı istek sayısı. 
  
  İçin çok büyük ve/veya etkin bir veritabanı, geri yükleme işlemi birkaç saat sürebilir. Bir bölgede uzun süre boyunca kesinti oluşursa, çok sayıda diğer bölgelere tarafından işlenmekte olan coğrafi geri yükleme isteği olduğunu mümkündür. Çok sayıda istek olduğunda, bu bölgede veritabanları için kurtarma süresini artırabilir. Çoğu veritabanı 12 saat içinde tam geri yükler.

Tek bir abonelik için bazı sınırlamalar (belirli bir geri yükleme, coğrafi geri yükleme ve uzun süreli bekletme yedeklemesi geri yükleme noktası dahil) eş zamanlı geri yükleme isteği sayısı gönderildi ve bulunmaktadır proceeded:
|  | **En fazla işlenmekte olan eş zamanlı istek sayısı** | **En fazla gönderilen eş zamanlı istek sayısı** |
| :--- | --: | --: |
|(Abonelik) başına tek veritabanı|10|60|
|Elastik havuz (başına havuzu)|4|200|
||||

Geri yükleme toplu olarak yerleşik işlevi yoktur. [Azure SQL veritabanı: tam sunucu kurtarma](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) betik bu görevi yerine getirmeye bir yolu bir örnektir.

> [!IMPORTANT]
> Otomatik yedekleri kullanarak kurtarma için bir Abonelikteki SQL Server Katılımcısı rolü üyesi olmanız veya - abonelik sahibi olmanız göz atın [RBAC: yerleşik roller](../role-based-access-control/built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız. 
> 

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Azure portalını kullanarak aynı mantıksal sunucu üzerinde yeni bir veritabanı olarak zaman içinde önceki bir noktaya varolan bir veritabanını geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), veya [REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx). 

> [!TIP]
> Bir veritabanının bir zaman içinde nokta geri yüklemeyi gerçekleştirmek nasıl gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
>

Veritabanı herhangi bir hizmet katmanı veya işlem boyutu ve tek bir veritabanı veya elastik havuz içine geri yüklenebilir. Mantıksal sunucuda veya veritabanını geri yüklediğiniz esnek havuzda yeterli kaynaklara sahip olun. Tamamlandıktan sonra geri yüklenen veritabanı bir normal, tam olarak erişilebilir, çevrimiçi veritabanı hizmetidir. Geri yüklenen veritabanı, hizmet katmanı ve işlem boyutuna bağlı olarak normal fiyatlarıyla ücretlendirilir. Veritabanı geri yükleme işlemi tamamlanana kadar bir ücrete tabi değildir.

Bir veritabanını daha önceki bir noktaya kurtarma amacıyla genellikle geri. Bunun yapılması, yerine özgün veritabanını geri yüklenen veritabanı işle veya verileri almak ve sonra özgün veritabanını güncellemek için kullanın. 

* ***Veritabanı değiştirme:*** yerine özgün veritabanını geri yüklenen veritabanı hedeflenmişse, işlem boyutu doğrulamanız gerekir ve/veya hizmet katmanı uygundur ve gerekirse, veritabanı ölçeklendirin. Özgün veritabanını yeniden adlandırın ve ardından özgün adı kullanarak geri yüklenen veritabanı vermek [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL komutu. 
* ***Veri kurtarma:*** bir kullanıcı veya uygulama hatasından kurtarmak için geri yüklenen veritabanından veri almak planlama, yazma ve özgün veritabanına geri yüklenen veritabanından verileri ayıklamak için gerekli verileri kurtarma betiklerini yürütmek gerekir. Geri yükleme işleminin tamamlanması uzun zaman alabilir ancak geri yüklenen veritabanının geri yükleme işlemi boyunca veritabanı listesinde görünür. Veritabanını geri yükleme sırasında silmeniz halinde, geri yükleme işlemi iptal edildi ve geri yükleme işlemi tamamlanamadı veritabanı için ücretlendirilmez. 

### <a name="azure-portal"></a>Azure portal

Azure portalını kullanarak bir noktaya kurtarmak için veritabanı sayfasını açın ve **geri** araç.

![noktası içinde belirli bir geri yükleme](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

## <a name="deleted-database-restore"></a>Silinen veritabanını geri yükleme
Silinen bir veritabanını Azure portalını kullanarak aynı mantıksal sunucu üzerinde silme süresine, silinen veritabanını geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase), veya [REST (createMode = geri yükle)](https://msdn.microsoft.com/library/azure/mt163685.aspx). Bekletme kullanarak sırasında zaman içinde önceki bir noktaya silinen veritabanını geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/restore-azurermsqldatabase).

> [!TIP]
> Silinen bir veritabanını geri yükleme işlemini gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
>

> [!IMPORTANT]
> Bir Azure SQL veritabanı sunucusu örneğine silerseniz, tüm veritabanlarını da silinir ve kurtarılamaz. Şu anda silinen sunucunun geri yüklenmesi desteklenmez.
> 

### <a name="azure-portal"></a>Azure portal

Sırasında silinen bir veritabanını kurtarmak için kendi [DTU tabanlı model saklama süresi](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı model saklama süresi](sql-database-service-tiers-vcore.md) sunucunuzun ve işlem alanında sayfasını açın, Azure portalını kullanarak, **Veritabanlarını sildi**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)


![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

## <a name="geo-restore"></a>Coğrafi Geri Yükleme
Bir SQL veritabanı herhangi bir sunucuda herhangi bir Azure bölgesinde en son coğrafi olarak çoğaltılmış tam ve fark yedeklerden geri yükleyebilirsiniz. Coğrafi geri yükleme, coğrafi olarak yedekli bir yedeklemesini, kaynağı olarak kullanır ve veritabanı veya veri merkezinde bir kesinti nedeniyle erişilemez durumda olsa bile bir veritabanını kurtarmak için kullanılabilir. 

Coğrafi geri yükleme veritabanı barındırıldığı bölgedeki bir olay nedeniyle veritabanınız kullanılamıyor varsayılan kurtarma seçeneğini andır. Büyük ölçekli olay kullanılamazlık veritabanı uygulamanızın bir bölge sonucu, veritabanını coğrafi çoğaltmalı yedeklerden başka bir bölgede bir sunucuya geri yükleyebilirsiniz. Değişiklik yedeği zaman alınır ve Azure için coğrafi olarak çoğaltılmış olduğunda arasında bir gecikme blob farklı bir bölgede. Bu gecikme, bir saat, bu nedenle, bir olağanüstü durum oluşursa, olabilir yukarı bir saatlik veri kaybı için en fazla olabilir. Aşağıdaki çizimde, başka bir bölgede kullanılabilir son yedekleme veritabanından veritabanı geri yükleme gösterilmektedir.

![Coğrafi geri yükleme](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Bir coğrafi geri yükleme gerçekleştirme gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
> 

Belirli bir noktaya geri yükleme bir coğrafi-ikincil üzerinde şu anda desteklenmiyor. Belirli bir noktaya geri yükleme, yalnızca birincil veritabanında gerçekleştirilebilir. Kesintiden kurtarma için coğrafi geri yükleme kullanma hakkında ayrıntılı bilgi için bkz: [kesintiden kurtarma](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Kurtarılmasını, olağanüstü durum kurtarma çözümleri uzun kurtarma noktası hedefi (RPO) ve tahmini kurtarma süresi (ERT) ile SQL veritabanı'nda kullanılabilen en temel üyeliktir. Küçük boyut veritabanları (örneğin temel hizmet katmanı veya Kiracı veritabanları elastik havuzlardaki küçük boyut) kullanan çözümler için coğrafi geri yükleme sık makul bir DR çözümü ile 12 saatlik bir ERT var. Çözümleri kullanarak büyük veritabanları ve gerektiren daha kısa kurtarma süreleri ve kullanmayı düşünmeniz gerekir [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md). Etkin coğrafi çoğaltma, bir çok daha düşük ERT ve RPO yalnızca gerektirir gibi sunar sürekli olarak çoğaltılmış ikincil bir yük devretme başlatın. İş sürekliliği seçenekleri hakkında daha fazla bilgi için bkz. [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
> 

### <a name="azure-portal"></a>Azure portal

Coğrafi geri yükleme sırasında veritabanını bir için kendi [DTU tabanlı model saklama süresi](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı model saklama süresi](sql-database-service-tiers-vcore.md) Azure portalını kullanarak, SQL veritabanları sayfasını açın ve ardından **Ekle** . İçinde **Kaynak Seç** seçin, metin kutusuna **yedekleme**. Bölgede ve sunucunun seçtiğiniz kurtarma gerçekleştirmek yedekleme belirtin. 

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Program aracılığıyla otomatik yedekleri kullanarak kurtarma gerçekleştirme
Daha önce bahsedildiği gibi Azure portalına ek olarak, veritabanı kurtarma Azure PowerShell veya REST API'sini kullanarak program aracılığıyla gerçekleştirilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır.

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
| [Alma oluşturma veya güncelleştirme veritabanı durumu](https://msdn.microsoft.com/library/azure/mt643934.aspx) |Durumu geri yükleme işlemi sırasında döndürür |
|  | |

## <a name="summary"></a>Özet
Otomatik yedeklemeler, kullanıcı ve uygulama hataları, yanlışlıkla veritabanı silme ve verilerinizi uzun süren kesintileri veritabanlarınızı koruyun. Bu yerleşik özelliği, tüm bilgi işlem boyutlarına ve hizmet katmanları için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar
* İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
* Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
* Uzun süreli saklama hakkında bilgi edinmek için bkz: [uzun süreli saklama](sql-database-long-term-retention.md).
* Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [yük devretme grupları ve etkin coğrafi çoğaltma](sql-database-geo-replication-overview.md).  
