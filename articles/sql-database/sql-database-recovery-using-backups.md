---
title: Azure SQL veritabanını bir yedekten geri yükleyin. | Microsoft Docs
description: Azure SQL veritabanı (en fazla 35 gün) zaman içinde önceki bir noktaya geri olanak tanıyan, zaman içinde nokta geri yükleme hakkında bilgi edinin.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/30/2019
ms.openlocfilehash: 47bf59adb33f3685b31430c652b31880d383833e
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232652"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Otomatik veritabanı yedeklerini kullanarak bir Azure SQL veritabanını kurtarma

Varsayılan olarak, SQL veritabanı yedeklemelerini coğrafi olarak çoğaltılmış bir blob depolama (RA-GRS) depolanır. Veritabanı kurtarma kullanmak için aşağıdaki seçenekler kullanılabilir [otomatik veritabanı yedeklemelerini](sql-database-automated-backups.md):

- Saklama dönemi içinde zaman içinde belirli bir noktaya kurtarılan aynı SQL veritabanı sunucusunda yeni bir veritabanı oluşturun.
- Silinen bir veritabanını silme süresine, kurtarılır aynı SQL veritabanı sunucusunda bir veritabanı oluşturun.
- Herhangi bir SQL veritabanı sunucusu son yedeklemelerin noktasına kurtarılır aynı bölgede yeni bir veritabanı oluşturun.
- Herhangi bir SQL veritabanı sunucusu en son çoğaltılan yedeklemeler noktasına kurtarılır bölgede yeni bir veritabanı oluşturun.

Yapılandırdıysanız [yedekleme uzun süreli saklama](sql-database-long-term-retention.md), herhangi bir SQL veritabanı sunucusunda LTR yedekten yeni bir veritabanı oluşturabilirsiniz.

> [!IMPORTANT]
> Varolan bir veritabanını geri yükleme sırasında üzerine yazılamıyor.

Standart veya Premium Hizmet katmanını kullanırken, geri yüklenen veritabanı aşağıdaki koşullarda bir ek depolama alanı maliyeti doğurur:

- Veritabanı boyutu 500 GB'den daha büyük ise, P11 – P15 S4-S12 veya P1 – P6 geri yükleyin.
- Veritabanı boyutu 250 GB'tan büyük ise, P1 – P6 S4-S12 için geri yükleme.

Ekstra maliyeti, icurred geri yüklenen veritabanının maksimum boyutunu hedef veritabanının Hizmet katmanını ve performans düzeyi ile dahil edilen depolama alanı miktarından daha büyük olduğunda. Dahil edilen miktarın üzerinde sağlanan ek depolama alanı, ek ücret alınır. Fiyatlandırma ayrıntıları ek depolama alanının bkz [SQL veritabanı fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/sql-database/). Kullanılan alan gerçek miktarı dahil edilen depolama alanı miktarı azsa, bu en büyük veritabanı boyutu için dahil edilen miktarı ayarlayarak ek maliyet önleyebilirsiniz.

> [!NOTE]
> [Veritabanı Yedeklemeleri otomatik](sql-database-automated-backups.md) , oluştururken kullanılan bir [veritabanı kopyalama](sql-database-copy.md).

## <a name="recovery-time"></a>Kurtarma zamanı

Otomatik veritabanı yedeklerini kullanarak bir veritabanını geri yüklemek için kurtarma zamanı çeşitli faktörler tarafından etkilenir:

- Veritabanı boyutu
- Veritabanı işlem boyutu
- İşlem günlükleri dahil sayısı
- Geri yükleme noktasına kurtarmak için yeniden yürütülmesi gereken etkinlik miktarı
- Geri yükleme farklı bir bölgeye ise ağ bant genişliği
- Hedef bölgede işlenmekte olan isteği eşzamanlı geri yükleme sayısı

Büyük ve/veya çok etkin veritabanı için geri yükleme işlemi birkaç saat sürebilir. Bir bölgede uzun süre boyunca kesinti oluşursa, çok sayıda diğer bölgelere tarafından işlenmekte olan coğrafi geri yükleme isteği olduğunu mümkündür. Çok sayıda istek olduğunda, bu bölgede veritabanları için kurtarma süresini artırabilir. Çoğu veritabanı tam 12 saatten kısa bir süre içinde geri yükler.

Tek bir abonelik için restore eş zamanlı istek sayısı sınırlamaları vardır.  Bu sınırlamalar zaman geri yükler, coğrafi geri yükler ve geri yükleme noktası herhangi bir birleşimini uzun süreli saklama yedeklemesinden geçerlidir):

| | **En fazla işlenmekte olan eş zamanlı istek sayısı** | **En fazla gönderilen eş zamanlı istek sayısı** |
| :--- | --: | --: |
|(Abonelik) başına tek veritabanı|10|60|
|Elastik havuz (başına havuzu)|4|200|
||||

Şu anda tüm sunucuyu geri yüklemek için yerleşik bir yöntem yoktur. [Azure SQL veritabanı: Tam sunucu kurtarma](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) betik, bu görevi gerçekleştirmek nasıl bir örnek.

> [!IMPORTANT]
> Otomatik yedekleri kullanarak kurtarma için bir Abonelikteki SQL Server Katılımcısı rolü üyesi olmanız veya - abonelik sahibi olmanız göz atın [RBAC: Yerleşik roller](../role-based-access-control/built-in-roles.md). Verileri Azure portalı, PowerShell veya REST API kullanarak kurtarabilirsiniz. Transact-SQL kullanamazsınız.

## <a name="point-in-time-restore"></a>Belirli bir noktaya geri yükleme

Havuza alınmış, tek başına geri yükleme ya da daha önceki bir noktaya veritabanını Azure portalını kullanarak örnek [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), veya [REST API](https://docs.microsoft.com/rest/api/sql/databases). İstek, herhangi bir hizmet katmanı belirtin veya geri yüklenen veritabanı için boyutu işlem. Veritabanını geri yüklediğiniz sunucuda yeterli kaynaklara sahip olun. İşlem tamamlandıktan sonra özgün veritabanıyla aynı sunucuda yeni bir veritabanı oluşturulur. Geri yüklenen veritabanı, hizmet katmanı ve işlem boyutuna bağlı olarak normal tarifesi üzerinden ücret alınacaktır. Veritabanı geri yükleme işlemi tamamlanana kadar bir ücrete tabi değildir.

Bir veritabanını daha önceki bir noktaya kurtarma amacıyla genellikle geri. Yerine özgün veritabanını geri yüklenen veritabanı işle veya özgün veritabanını güncellemek için bir kaynak veri kullanabilirsiniz.

- **Veritabanını değiştirme**

  Geri yüklenen veritabanının, özgün veritabanının bir ardılı olarak amaçlanıyorsa orinal veritabanının işlem boyutunu ve Hizmet katmanını belirtmeniz gerekir. Özgün veritabanını yeniden adlandırın ve özgün adı kullanarak geri yüklenen veritabanı vermek [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) T-SQL komutu.

- **Veri Kurtarma**

  Bir kullanıcı veya uygulama hatasından kurtarmak için geri yüklenen veritabanından veri almak planlama, yazma ve geri yüklenen veritabanından verileri ayıklayan ve özgün veritabanı için geçerli bir veri kurtarma betiği yürütmek gerekir. Geri yükleme işleminin tamamlanması uzun zaman alabilir ancak geri yüklenen veritabanının geri yükleme işlemi boyunca veritabanı listesinde görünür. Veritabanını geri yükleme sırasında silmeniz halinde, geri yükleme işlemi iptal edilir ve geri yükleme işlemi tamamlanamadı veritabanı için ücretlendirilmez.

Tek bir, kurtarılır havuza, veya Azure portalını kullanarak örnek bir noktasının veritabanına, veritabanı sayfasını açın ve tıklayın **geri** araç.

![noktası içinde belirli bir geri yükleme](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Program aracılığıyla bir veritabanı bir yedeklemeden geri yüklemek için bkz: [otomatik yedekleme kurtarma gerçekleştirme program aracılığıyla kullanarak](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Silinen veritabanını geri yükleme

Silinen bir veritabanını silme işlemi süresi veya daha önceki bir noktaya zamanlı Azure portalını kullanarak SQL veritabanını aynı sunucuya geri yükleyebilirsiniz [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), veya [REST (createMode = geri yükle)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). Yapabilecekleriniz [silinmiş öğeleri geri yükleme veritabanı yönetilen örneği PowerShell kullanarak](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Silinen bir veritabanını geri yükleme işlemini gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Bir Azure SQL veritabanı sunucusu örneğine silerseniz, tüm veritabanlarını da silinir ve kurtarılamaz. Şu anda silinen sunucunun geri yüklenmesi desteklenmez.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Azure portalını kullanarak silinen veritabanını geri yükleme

Azure portalını kullanarak silinen bir veritabanını kurtarmak için sunucunuzun ve işlem alanında sayfasını açın, **veritabanlarını sildi**.

![deleted-database-restore-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![deleted-database-restore-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Programlı olarak silinen bir veritabanını geri yüklemek için bkz: [otomatik yedekleme kurtarma gerçekleştirme program aracılığıyla kullanarak](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Coğrafi Geri Yükleme

Bir SQL veritabanı herhangi bir sunucuda herhangi bir Azure bölgesinde en son coğrafi çoğaltmalı yedeklerden geri yükleyebilirsiniz. Coğrafi geri yükleme, coğrafi olarak çoğaltılmış yedekleme kaynağı kullanır. Veritabanını veya veri merkezinde bir kesinti nedeniyle erişilemez durumda olsa bile istenebilir.

Veritabanınızı, barındırma bölgedeki bir olay nedeniyle kullanılamaz duruma geldiğinde coğrafi geri yükleme varsayılan kurtarma seçenektir. Veritabanı başka bir bölgede bir sunucuya geri yükleyebilirsiniz. Yedekleme zaman alınır ve Azure için coğrafi olarak çoğaltılmış olduğunda arasında bir gecikme blob farklı bir bölgede. Sonuç olarak, geri yüklenen veritabanı orignal veritabanı arkasındaki bir saate kadar olabilir. Aşağıdaki çizimde, başka bir bölgede kullanılabilir son yedekleme veritabanından veritabanı geri yükleme gösterilmektedir.

![Coğrafi geri yükleme](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Bir coğrafi geri yükleme gerçekleştirme gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).

Belirli bir noktaya geri yükleme bir coğrafi-ikincil üzerinde şu anda desteklenmiyor. Belirli bir noktaya geri yükleme, yalnızca birincil veritabanında gerçekleştirilebilir. Kesintiden kurtarma için coğrafi geri yükleme kullanma hakkında ayrıntılı bilgi için bkz: [kesintiden kurtarma](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> Coğrafi geri yükleme, en temel olağanüstü durum kurtarma çözümü SQL veritabanı'nda kullanılabilir olan. Coğrafi olarak çoğaltılmış yedekleme RPO ile otomatik olarak oluşturulan kullanır = 1 saat ve tahmini kurtarma süresi 12 saate kadar. Hedef bölge veritabanlarınız talebin keskin bir artış olasılığı nedeniyle bir bölgesel ourage sonra geri yüklemek için kapasite sahip olacağını garanti etmez. Görece küçük veritabanları olmayan iş kritik bir uygulama için coğrafi geri yükleme bir uygun olağanüstü durum kurtarma çözümüdür. Büyük veritabanları kullanın ve iş sürekliliğinin sağlanması gerekir busniess Kritik uygulamalar için kullanmanız gereken [otomatik yük devretme grupları](sql-database-auto-failover-group.md). Bir çok daha düşük RTO ve RPO sunar ve kapasite her zaman sağlanır. İş sürekliliği seçenekleri hakkında daha fazla bilgi için bkz. [iş sürekliliğine genel bakış](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Azure portalını kullanarak coğrafi-geri yükleme

Coğrafi geri yükleme sırasında veritabanını bir için kendi [DTU tabanlı model saklama süresi](sql-database-service-tiers-dtu.md) veya [sanal çekirdek tabanlı model saklama süresi](sql-database-service-tiers-vcore.md) Azure portalını kullanarak, SQL veritabanları sayfasını açın ve ardından **Ekle** . İçinde **Kaynak Seç** seçin, metin kutusuna **yedekleme**. Bölgede ve sunucunun seçtiğiniz kurtarma gerçekleştirmek yedekleme belirtin.

> [!Note]
> Azure portalını kullanarak coğrafi geri yönetilen örneği'nde kullanılamaz. Bunun yerine PowerShell kullanın.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Program aracılığıyla otomatik yedekleri kullanarak kurtarma gerçekleştirme

Daha önce bahsedildiği gibi Azure portalına ek olarak, veritabanı kurtarma Azure PowerShell veya REST API'sini kullanarak program aracılığıyla gerçekleştirilebilir. Aşağıdaki tabloda kullanılabilir komut kümesi açıklanmaktadır.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

- Tek başına veya havuza alınmış veritabanını geri yüklemek için bkz: [geri yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Bir veya daha fazla veritabanını alır. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Geri yükleyebileceğiniz, silinmiş bir veritabanını alır. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Bir veritabanının coğrafi olarak yedekli bir yedeklemesini alır. |
  | [Geri yükleme-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |SQL veritabanını geri yükler. |

  > [!TIP]
  > Bir veritabanının bir zaman içinde nokta geri yüklemeyi gerçekleştirmek nasıl gösteren bir örnek PowerShell Betiği için bkz: [PowerShell kullanarak bir SQL veritabanını geri](scripts/sql-database-restore-database-powershell.md).

- Yönetilen örnek veritabanını geri yüklemek için bkz: [geri yükleme-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | Açıklama |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Bir veya daha fazla yönetilen örneğini alır. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Veritabanlarını bir örneğini alır. |
  | [Geri yükleme-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Bir örnek veritabanını geri yükler. |

### <a name="rest-api"></a>REST API

REST API kullanarak bir tek veya havuza alınmış veritabanını geri yüklemek için:

| API | Açıklama |
| --- | --- |
| [REST (createMode kurtarma =)](https://docs.microsoft.com/rest/api/sql/databases) |Bir veritabanını geri yükler |
| [Alma oluşturma veya güncelleştirme veritabanı durumu](https://docs.microsoft.com/rest/api/sql/operations) |Durumu geri yükleme işlemi sırasında döndürür |

### <a name="azure-cli"></a>Azure CLI'si

- Azure CLI kullanarak tek veya havuza alınmış veritabanını geri yüklemek için bkz: [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).
- Azure CLI kullanarak bir yönetilen örneğine geri yüklemek için bkz: [az sql ORTAB geri yükleme](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Özet

Otomatik yedeklemeler, kullanıcı ve uygulama hataları, yanlışlıkla veritabanı silme ve verilerinizi uzun süren kesintileri veritabanlarınızı koruyun. Bu yerleşik özelliği, tüm bilgi işlem boyutlarına ve hizmet katmanları için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

- İş sürekliliğine genel bakış ve senaryolar için bkz: [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
- Veritabanı otomatik yedeklemeler Azure SQL hakkında bilgi edinmek için bkz: [SQL veritabanı otomatik yedeklerinde](sql-database-automated-backups.md).
- Uzun süreli saklama hakkında bilgi edinmek için bkz: [uzun süreli saklama](sql-database-long-term-retention.md).
- Daha hızlı kurtarma seçenekleri hakkında bilgi edinmek için [etkin coğrafi çoğaltma](sql-database-active-geo-replication.md) veya [otomatik yük devretme grupları](sql-database-auto-failover-group.md).
