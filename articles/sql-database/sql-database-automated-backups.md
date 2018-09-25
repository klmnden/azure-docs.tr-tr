---
title: Otomatik, coğrafi olarak yedekli Azure SQL veritabanı yedeklemelerini | Microsoft Docs
description: SQL veritabanı otomatik olarak birkaç dakikada bir yerel veritabanı yedeğini oluşturur ve Azure okuma erişimli coğrafi olarak yedekli depolama için coğrafi yedeklilik kullanır.
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
ms.openlocfilehash: 8fd8bf6128d09d6431a8542206430b9bb6df095d
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2018
ms.locfileid: "47063736"
---
# <a name="learn-about-automatic-sql-database-backups"></a>Otomatik SQL veritabanını yedekleme hakkında bilgi edinin

SQL veritabanı otomatik veritabanı yedeklerini oluşturur ve Azure okuma erişimli coğrafi olarak yedekli depolama (RA-GRS), coğrafi olarak yedeklilik sağlamak için kullanır. Bu yedeklemeler, otomatik olarak ve ek ücret olmadan oluşturulur. Bunları gerçekleştirmek için herhangi bir şey yapmanız gerekmez. Bunlar yanlışlıkla Bozulması veya silinmesi, verilerinizi korumak için veritabanı yedeklerini tüm iş sürekliliği ve olağanüstü durum kurtarma stratejinize önemli bir parçasıdır. Güvenlik kuralları, yedeklemelerinizi uzun bir süre için kullanılabilir olmasını gerektiren bir uzun süreli yedek saklama ilkesini yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>SQL veritabanı yedeklemesini nedir?

SQL veritabanı oluşturmak için SQL Server teknolojisini kullanan [tam](https://msdn.microsoft.com/library/ms186289.aspx), [fark](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server), ve [işlem günlüğü](https://msdn.microsoft.com/library/ms191429.aspx) yedeklerini amacı doğrultusunda, zaman içinde nokta geri (PITR). İşlem günlüğü yedeklemeleri genellikle 5-10 dakikada bir gerçekleşir ve değişiklik yedekleri İşlem boyutu ve veritabanı etkinliği miktarı göre sıklığı ile her 12 saatte bir, genellikle oluşur. İşlem günlüğü yedeklemeleri, tam ve farklı yedeklemelerini bir veritabanı, bir özel-belirli bir noktaya veritabanını barındıran aynı sunucuya geri yüklemenize olanak sağlar. Yedeklemeleri çoğaltılır RA-GRS depolama bloblarında depolanan bir [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md) bir veri merkezi arızasına karşı koruma için. Bir veritabanını geri yüklediğinizde, hizmetin hangi tam, değişiklik yedeklemelerinin ve işlem günlüğü yedekleri geri yüklenmelidir çözüyordu.


Bu yedeklemeler için kullanabilirsiniz:

* Bir veritabanı saklama süresi içinde bir-belirli bir noktaya için geri yükleyin. Bu işlem, özgün veritabanı ile aynı sunucuda yeni bir veritabanı oluşturur.
* Silinen bir veritabanını geri yükleme, silinmiş olduğu zaman veya Bekletme dönemi içinde istediğiniz zaman. Silinen veritabanını yalnızca özgün veritabanının oluşturulduğu aynı sunucuya geri yüklenebilir.
* Bir veritabanını başka bir coğrafi bölgeye geri yükleyin. Bu, sunucunuz ve veritabanınıza erişemediğinde coğrafi olağanüstü durumdan kurtarmanıza olanak tanır. Dünyanın her yerinden herhangi bir mevcut sunucusu içinde yeni bir veritabanı oluşturur. 
* Veritabanı uzun süreli saklama ilkesini (LTR) ile yapılandırılmış olması halinde bir veritabanını belirli bir uzun dönem yedeklemeden geri yükleyin. Bu veritabanının uyumluluk isteği karşılamak için veya uygulamanın eski bir sürümü eski bir sürümüne geri yüklemenize olanak sağlar. Bkz: [uzun süreli saklama](sql-database-long-term-retention.md).
* Bir geri yükleme gerçekleştirmek için bkz: [veritabanını yedeklerden geri yükleme](sql-database-recovery-using-backups.md).

> [!NOTE]
> Azure depolama alanında terimi *çoğaltma* dosyaları bir konumdan diğerine kopyalama işlemini ifade eder. SQL'in *veritabanı çoğaltması* birden çok ikincil veritabanlarını birincil veritabanı ile eşitlenmiş tutmak için ifade eder. 
> 

## <a name="how-long-are-backups-kept"></a>Yedeklemeleri ne kadar saklanır?
Her SQL veritabanı yedeği, veritabanının Hizmet katmanını temel alır ve arasında farklı bir varsayılan saklama süresi olan [DTU tabanlı satın alma modeli](sql-database-service-tiers-dtu.md) ve [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md). Bir veritabanı için yedekleme bekletme süresi güncelleştirebilirsiniz. Bkz: [değişiklik yedekleme Bekletme dönemi](#how-to-change-backup-retention-period) daha fazla ayrıntı için.

Bir veritabanı silerseniz, SQL veritabanı yedeklemeleri için çevrimiçi bir veritabanı olduğu aynı şekilde tutar. Örneğin, bir yedi günlük tutma süresine sahip bir temel veritabanı silerseniz, dört gün eski bir yedek üç gün boyunca kaydedilir.

Yedeklemeler en fazla PITR saklama süresinden daha uzun süre tutmanız gerekiyorsa, bir veya daha fazla uzun vadeli bekletme süreleri veritabanınıza eklemek için yedekleme özelliklerini değiştirebilirsiniz. Bkz: [uzun süreli yedek saklama](sql-database-long-term-retention.md) daha fazla ayrıntı için.

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL server silerseniz, tüm elastik havuzlara ve sunucuya ait veritabanlarına da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz. Ancak, uzun süreli saklama yapılandırdıysanız, LTR ile veritabanları için yedekleri silinmez ve bu veritabanlarını geri yüklenebilir.

### <a name="pitr-retention-period"></a>PITR saklama süresi
DTU tabanlı satın alma modeli kullanılarak oluşturulmuş bir veritabanı için varsayılan saklama süresi hizmet katmanına bağlıdır:

* Temel hizmet katmanı 1 hafta içindir.
* Standart hizmet katmanı, 5 hafta içindir.
* Premium Hizmet katmanını 5 hafta ' dir.

Kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), yedeklemelerin saklanacağı 35 gün öncesine yapılandırılabilir. 

Geçerli PITR bekletme süresini kısaltmaya, yeni saklama süresinden daha eski tüm mevcut yedeklemeler artık kullanılabilir. 

Geçerli PITR saklama süresini artırmak istiyorsanız, SQL veritabanı uzun bekletme süresine ulaşılana kadar mevcut yedeklemeler tutar.

## <a name="how-often-do-backups-happen"></a>Yedeklemeleri ne sıklıkta gerçekleşir?
### <a name="backups-for-point-in-time-restore"></a>Yedeklemeler için zaman içinde nokta geri yükleme
SQL veritabanı, tam yedekleme, değişiklik yedekleri ve işlem günlüğü yedeklemeleri otomatik olarak oluşturarak, zaman içinde nokta geri yükleme (PITR) Self Servis destekler. Haftalık tam veritabanı yedeklemeleri oluşturulur, Türevsel veritabanı yedekleri, genellikle her 12 saatte bir oluşturulur ve işlem günlüğü yedeklemeleri genellikle her 5-10 dakika işlem boyutu ve veritabanı etkinliği miktarı göre sıklığı oluşturulur. Hemen bir veritabanı oluşturulduktan sonra ilk tam yedeklemede zamanlanır. Genellikle 30 dakika içinde tamamlanır, ancak veritabanı önemli bir boyutta olduğunda daha uzun sürebilir. Örneğin, ilk yedekleme, geri yüklenen veritabanı veya veritabanı kopyası üzerinde daha uzun sürebilir. İlk tam yedeklemeden sonra tüm ek yedeklemeler otomatik olarak zamanlanan ve arka planda sessizce yönetilen. Genel sistem iş yükü dengeleyen gibi tüm veritabanı yedeklerinin tam zamanlama SQL veritabanı hizmeti tarafından belirlenir.

Coğrafi olarak yedekli ve korunan PITR yedeklemeleri [Azure depolama bölgeler arası çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Daha fazla bilgi için [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="backups-for-long-term-retention"></a>Uzun süreli saklama yedeklerini
SQL veritabanı, Azure blob depolama alanındaki 10 yıla kadar tam yedekleri uzun süreli saklama (LTR) yapılandırma seçeneğini sunar. LTR ilkesi etkinleştirilirse, haftalık tam yedeklemeler için farklı bir RA-GRS depolama kapsayıcısını otomatik olarak kopyalanır. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Depolama alanı tüketimi, yedekleme ve bekletme period(s) seçili sıklığına bağlıdır. Kullanabileceğiniz [LTR fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=sql-database) LTR depolama maliyetini tahmin etmek için. 

Coğrafi olarak yedekli ve korunan PITR gibi LTR yedekleme [Azure depolama bölgeler arası çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Daha fazla bilgi için [uzun süreli yedek saklama](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>Yedeklemeleri şifrelenir?

Veritabanınız ile TDE şifrelenmişse, yedeklemeleri otomatik olarak LTR yedekleme de dahil olmak üzere, bekleme sırasında şifrelenir. Bir Azure SQL veritabanı için TDE etkin olduğunda, yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları, varsayılan olarak etkin TDE ile yapılandırılır. TDE hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-do-automated-backups-impact-my-compliance"></a>Otomatik yedekleri my uyumluluk nasıl etkilediği?

Veritabanı DTU tabanlı hizmet katmanı varsayılan PITR bekletme 35 gün ile bir sanal çekirdek tabanlı hizmet katmanına geçiş yaparken, PITR bekletme uygulamanızın veri kurtarma ilkesi gizliliğinin tehlikeye girmemesini sağlamak için korunur. Varsayılan saklama süresi uyumluluk gereksinimlerinizi karşılamıyorsa, PowerShell veya REST API'sini kullanarak PITR saklama süresini değiştirebilirsiniz. Bkz: [değişiklik yedekleme Bekletme dönemi](#how-to-change-backup-retention-period) daha fazla ayrıntı için.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-backup-retention-period"></a>Yedekleme bekletme süresi değiştirme
REST API'si veya PowerShell kullanarak varsayılan saklama süresi değiştirebilirsiniz. Desteklenen değerler şunlardır: 7, 14, 21, 28 veya 35 gün. Aşağıdaki örnekler için 28 gün PITR bekletmeyi değiştirme işlemini göstermektedir. 

> [!NOTE]
> Bu API'leri yalnızca PITR saklama süresini etkiler. Veritabanınız için LTR yapılandırdıysanız, etkilenmeyecektir. Bkz: [uzun süreli yedek saklama](sql-database-long-term-retention.md) LTR bekletme period(s) değiştirme hakkında ayrıntılar için.

### <a name="change-pitr-backup-retention-period-using-powershell"></a>PowerShell kullanarak değişiklik PITR yedekleme bekletme süresi
```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```
> [!IMPORTANT]
> Bu API sürümünden başlayarak Azurerm.SQL'e PowerShell modülü dahil [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview). 

### <a name="change-pitr-retention-period-using-rest-api"></a>REST API kullanarak değişiklik PITR saklama süresi
**Örnek istek**
```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```
**İstek gövdesi**
```json
{
  "properties":{  
      "retentionDays":28
   }
}
```
**Örnek yanıt**

Durum kodu: 200
```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```
Bkz: [yedekleme bekletme REST API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies) daha fazla ayrıntı için.

## <a name="next-steps"></a>Sonraki adımlar

- Bunlar yanlışlıkla Bozulması veya silinmesi, verilerinizi korumak için veritabanı yedeklerini tüm iş sürekliliği ve olağanüstü durum kurtarma stratejinize önemli bir parçasıdır. Diğer Azure SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
- Azure portalını kullanarak bir noktaya geri yüklemek için bkz: [veritabanını Azure portalını kullanarak bir noktaya geri yükleme](sql-database-recovery-using-backups.md).
- PowerShell kullanarak bir noktaya geri yüklemek için bkz: [veritabanını PowerShell kullanarak bir noktaya geri yükleme](scripts/sql-database-restore-database-powershell.md).
- Otomatik yedeklemeler Azure portalını kullanarak Azure blob depolama alanındaki yapılandırmak, yönetmek ve uzun süreli saklamak üzere geri yüklemek için bkz: [Azure portalını kullanarak uzun vadeli yedekleme bekletmeyi yönetme](sql-database-long-term-backup-retention-configure.md).
- Otomatik yedeklemeler, PowerShell kullanarak Azure Web günlüğü depolama yapılandırmak, yönetmek ve uzun süreli saklamak üzere geri yüklemek için bkz: [PowerShell kullanarak uzun vadeli yedekleme bekletmeyi yönetme](sql-database-long-term-backup-retention-configure.md).
