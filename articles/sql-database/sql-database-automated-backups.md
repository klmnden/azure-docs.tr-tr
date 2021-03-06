---
title: Otomatik, coğrafi olarak yedekli Azure SQL veritabanı yedeklemelerini | Microsoft Docs
description: SQL veritabanı otomatik olarak birkaç dakikada bir yerel veritabanı yedeğini oluşturur ve Azure okuma erişimli coğrafi olarak yedekli depolama için coğrafi yedeklilik kullanır.
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
ms.date: 06/27/2019
ms.openlocfilehash: ce16450f7f25e5703cf283c4babb2a935aad21de
ms.sourcegitcommit: af31deded9b5836057e29b688b994b6c2890aa79
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813061"
---
# <a name="automated-backups"></a>Otomatik yedeklemeler

SQL veritabanı otomatik olarak 7 ila 35 gün arasında tutulduğu veritabanı yedeklerini oluşturur ve Azure [okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) veri merkezi kullanılamaz durumda olsa bile, korunduğundan emin olmak için. Bu yedeklemeler otomatik olarak oluşturulur. Bunlar yanlışlıkla Bozulması veya silinmesi, verilerinizi korumak için veritabanı yedeklerini tüm iş sürekliliği ve olağanüstü durum kurtarma stratejinize önemli bir parçasıdır. Güvenlik kuralları, yedeklemelerinizi süre (en fazla 10 yıl) uzun bir süre için kullanılabilir olduğunu gerektiriyorsa, yapılandırabileceğiniz bir [uzun süreli saklama](sql-database-long-term-retention.md) tek veritabanları ve elastik havuzları.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>SQL veritabanı yedeklemesini nedir

SQL veritabanı oluşturmak için SQL Server teknolojisini kullanan [tam yedeklemeler](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) her hafta [değişiklik yedekleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) her 12 saatte bir, ve [işlem günlüğü yedeklemeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) her 5-10 dakika. Yedeklemeleri depolanan [RA-GRS depolama BLOB'ları](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage) için çoğaltılmış bir [eşleştirilmiş veri merkezine](../best-practices-availability-paired-regions.md) bir veri merkezi arızasına karşı koruma için. Bir veritabanını geri yüklediğinizde, hizmetin hangi tam, değişiklik yedeklemelerinin ve işlem günlüğü yedekleri geri yüklenmelidir çözüyordu.

Bu yedeklemeler için kullanabilirsiniz:

- **Varolan bir veritabanını bir-belirli bir noktaya için geçmişte geri** Azure portalı, Azure PowerShell, Azure CLI veya REST API'sini kullanarak Bekletme dönemi içinde. Bu işlem, tek veritabanı ve elastik havuzlar, özgün veritabanı ile aynı sunucuda yeni bir veritabanı oluşturur. Yönetilen örneği'nde, bu işlem veritabanı veya aynı veya farklı yönetilen örneği aynı abonelik altında bir kopyasını oluşturabilirsiniz.
  - **[Yedekleme Bekletme dönemi değiştirme](#how-to-change-the-pitr-backup-retention-period)**  yedekleme ilkenizi yapılandırmak için 7 ila 35 gün arasında.
  - **Değiştirme uzun süreli saklama ilkesini 10 yıla** tek veritabanı ve elastik havuzlar kullanarak [Azure portalında](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies) veya [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups).
- **Silinen bir veritabanını silinmiş durumuna geri** veya Bekletme dönemi içinde dilediğiniz zaman. Silinen veritabanını aynı mantıksal sunucu ya da özgün veritabanının oluşturulduğu yönetilen örneği yalnızca geri yüklenebilir.
- **Bir veritabanını başka bir coğrafi bölgeye geri**. Coğrafi geri yükleme, sunucunuz ve veritabanınıza erişemediğinde coğrafi olağanüstü durumdan kurtarmanıza olanak tanır. Dünyanın her yerinden herhangi bir mevcut sunucusu içinde yeni bir veritabanı oluşturur.
- **Bir veritabanını belirli bir uzun dönem yedeklemeden geri** tek veritabanı veya elastik veritabanı uzun süreli saklama ilkesini (LTR) ile yapılandırılmış olması halinde havuz. LTR veritabanı kullanarak eski bir sürümüne geri yüklemenize olanak verir [Azure portalında](sql-database-long-term-backup-retention-configure.md#view-backups-and-restore-from-a-backup-using-azure-portal) veya [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups) uyumluluk isteği karşılamak için veya uygulamanın eski bir sürümü. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).
- Bir geri yükleme gerçekleştirmek için bkz: [veritabanını yedeklerden geri yükleme](sql-database-recovery-using-backups.md).

> [!NOTE]
> Azure depolama alanında terimi *çoğaltma* dosyaları bir konumdan diğerine kopyalama işlemini ifade eder. SQL'in *veritabanı çoğaltması* birden çok ikincil veritabanlarını birincil veritabanı ile eşitlenmiş tutmak için ifade eder.

Aşağıdaki örnekler kullanarak bu işlemleri bazılarını deneyebilirsiniz:

| | Azure portal | Azure PowerShell |
|---|---|---|
| Değişiklik yedekleme bekletme | [Tek veritabanı](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-the-azure-portal) <br/> [Yönetilen örnek](sql-database-automated-backups.md#change-pitr-for-a-managed-instance) | [Tek veritabanı](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Değişiklik uzun süreli yedek saklama | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Yönetilen örnek - yok  | [Tek veritabanı](sql-database-long-term-backup-retention-configure.md#use-powershell-to-configure-long-term-retention-policies-and-restore-backups)<br/>Yönetilen örnek - yok  |
| Belirli bir noktaya veritabanı geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md#point-in-time-restore) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Silinen veritabanını geri yükleme | [Tek veritabanı](sql-database-recovery-using-backups.md#deleted-database-restore-using-the-azure-portal) | [Tek veritabanı](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Yönetilen örnek](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Azure Blob depolama alanından veritabanını geri yükleme | Tek veritabanı - yok <br/>Yönetilen örnek - yok  | Tek veritabanı - yok <br/>[Yönetilen örnek](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |

## <a name="how-long-are-backups-kept"></a>Yedeklemeleri ne kadar saklanır

Her bir SQL veritabanı, satın alma modeli ve hizmet katmanına bağlıdır 7 ila 35 gün arasında bir varsayılan yedekleme bekletme süresi vardır. SQL veritabanı sunucusunda bir veritabanı için yedekleme bekletme süresi güncelleştirebilirsiniz. Daha fazla bilgi için [değişiklik yedekleme Bekletme dönemi](#how-to-change-the-pitr-backup-retention-period).

Bir veritabanı silerseniz, SQL veritabanı yedeklemeleri için çevrimiçi bir veritabanı olduğu aynı şekilde tutar. Örneğin, bir yedi günlük tutma süresine sahip bir temel veritabanı silerseniz, dört gün eski bir yedek üç gün boyunca kaydedilir.

Yedeklemeler en uzun saklama süresinden daha uzun süre tutmanız gerekiyorsa, bir veya daha fazla uzun vadeli bekletme süreleri veritabanınıza eklemek için yedekleme özelliklerini değiştirebilirsiniz. Daha fazla bilgi için bkz. [Uzun süreli saklama](sql-database-long-term-retention.md).

> [!IMPORTANT]
> SQL veritabanlarını barındıran Azure SQL server silerseniz, tüm elastik havuzlara ve sunucuya ait veritabanlarına da silinir ve kurtarılamaz. Silinen bir sunucuya geri yükleyemezsiniz. Ancak, uzun süreli saklama yapılandırdıysanız, LTR ile veritabanları için yedekleri silinmez ve bu veritabanlarını geri yüklenebilir.

### <a name="default-backup-retention-period"></a>Varsayılan yedekleme bekletme süresi

#### <a name="dtu-based-purchasing-model"></a>DTU tabanlı satın alma modeli

DTU tabanlı satın alma modeli kullanılarak oluşturulmuş bir veritabanı için varsayılan saklama süresi hizmet katmanına bağlıdır:

- Temel hizmet katmanı **bir** hafta.
- Standart hizmet katmanı **beş** hafta.
- Premium hizmet katmanı **beş** hafta.

#### <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Kullanıyorsanız [sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md), varsayılan yedekleme Bekletme dönemi **yedi** (için tek bir havuzda ve örnek veritabanları) gün. Tüm Azure SQL veritabanları (havuza alınmış, tek ve örnek veritabanları yapabilecekleriniz [için 35 gün yedekleme bekletme süresi değiştirme](#how-to-change-the-pitr-backup-retention-period).

> [!WARNING]
> Geçerli saklama süresinin azaltılırsa, yeni saklama süresinden daha eski tüm mevcut yedeklemeler artık kullanılamaz. Geçerli saklama süresini artırmak istiyorsanız, SQL veritabanı uzun bekletme süresine ulaşılana kadar mevcut yedeklemeler tutar.

## <a name="how-often-do-backups-happen"></a>Yedeklemeleri ne sıklıkta gerçekleşir

### <a name="backups-for-point-in-time-restore"></a>Yedeklemeler için zaman içinde nokta geri yükleme

SQL veritabanı, tam yedekleme, değişiklik yedekleri ve işlem günlüğü yedeklemeleri otomatik olarak oluşturarak, zaman içinde nokta geri yükleme (PITR) Self Servis destekler. Haftalık tam veritabanı yedeklemeleri oluşturulur, Türevsel veritabanı yedekleri, genellikle her 12 saatte bir oluşturulur ve işlem günlüğü yedeklemeleri genellikle her 5-10 dakika işlem boyutu ve veritabanı etkinliği miktarı göre sıklığı oluşturulur. Hemen bir veritabanı oluşturulduktan sonra ilk tam yedeklemede zamanlanır. Genellikle 30 dakika içinde tamamlanır, ancak veritabanı önemli bir boyutta olduğunda daha uzun sürebilir. Örneğin, ilk yedekleme, geri yüklenen veritabanı veya veritabanı kopyası üzerinde daha uzun sürebilir. İlk tam yedeklemeden sonra tüm ek yedeklemeler otomatik olarak zamanlanan ve arka planda sessizce yönetilen. Genel sistem iş yükü dengeleyen gibi tüm veritabanı yedeklerinin tam zamanlama SQL veritabanı hizmeti tarafından belirlenir. Değiştiremez veya yedekleme işleri devre dışı bırakın. 

Coğrafi olarak yedekli ve korunan PITR yedeklemeleri [Azure depolama bölgeler arası çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Daha fazla bilgi için [-belirli bir noktaya geri yükleme](sql-database-recovery-using-backups.md#point-in-time-restore)

### <a name="backups-for-long-term-retention"></a>Uzun süreli saklama yedeklerini

Tek ve havuza alınmış veritabanları, Azure Blob Depolama alanında 10 yıla kadar tam yedekleri uzun süreli saklama (LTR) yapılandırma seçeneğini sunar. LTR ilkesi etkinleştirilirse, haftalık tam yedeklemeler için farklı bir RA-GRS depolama kapsayıcısını otomatik olarak kopyalanır. Farklı bir uyumluluk gereksinimini karşılamak için haftalık, aylık ve/veya yıllık yedeklemeler için farklı bekletme sürelerinin seçebilirsiniz. Depolama alanı tüketimi, yedekleme ve bekletme period(s) seçili sıklığına bağlıdır. Kullanabileceğiniz [LTR fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/?service=sql-database) LTR depolama maliyetini tahmin etmek için.

Coğrafi olarak yedekli ve korunan PITR gibi LTR yedekleme [Azure depolama bölgeler arası çoğaltma](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Daha fazla bilgi için [uzun süreli yedek saklama](sql-database-long-term-retention.md).

## <a name="storage-costs"></a>Depolama maliyetleri
Veritabanlarınızın yedi günlük otomatik yedeklemeleri, varsayılan olarak RA-GRS Standart blob depolamasına kopyalanır. Depolama alanı, haftalık tam yedeklemeler, günlük fark yedekleri ve 5 dakikada bir kopyalanan işlem günlüğü yedeklemeleri tarafından kullanılır. İşlem günlüğü boyutu veritabanı değişiklik oranına bağlıdır. Veritabanı boyutunun %100’üne eşit bir depolama alt sınırı ek ücret alınmadan sağlanır. Ek yedekleme alanı kullanımı, GB/ay üzerinden ücretlendirilir.

Depolama fiyatları hakkında daha fazla bilgi için bkz. [fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/single/) sayfası. 

## <a name="are-backups-encrypted"></a>Yedeklemeleri şifrelenir

Veritabanınız ile TDE şifrelenmişse, yedeklemeleri otomatik olarak LTR yedekleme de dahil olmak üzere, bekleme sırasında şifrelenir. Bir Azure SQL veritabanı için TDE etkin olduğunda, yedeklemeler de şifrelenir. Tüm yeni Azure SQL veritabanları, varsayılan olarak etkin TDE ile yapılandırılır. TDE hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı ile saydam veri şifrelemesi](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-does-microsoft-ensure-backup-integrity"></a>Microsoft, yedekleme tutarlılığı nasıl emin

Sürekli olarak, mühendislik ekibinin otomatik olarak Azure SQL veritabanı, testleri yerleştirilmiş mantıksal sunucuları ve elastik havuzlar (Bu yönetilen örneği'nde kullanılabilir değil) otomatik veritabanı yedeklerini geri yükleme veritabanları. Belirli bir noktaya geri yükleme sonrasında veritabanları DBCC CHECKDB kullanarak bütünlük denetimi de alır.

Yönetilen örnek alan ile ilk otomatik yedekleme `CHECKSUM` yerel kullanılarak geri veritabanlarının `RESTORE` komut veya geçiş tamamlandıktan sonra veri geçiş hizmeti.

Bütünlük denetimi sırasında bulunan tüm sorunları mühendislik ekibine bir uyarıya neden olur. Azure SQL veritabanında veri bütünlüğü hakkında daha fazla bilgi için bkz: [Azure SQL veritabanında veri bütünlüğü](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/).

## <a name="how-do-automated-backups-impact-compliance"></a>Otomatik yedekleri uyumluluk nasıl etkilediği

Veritabanı DTU tabanlı hizmet katmanı varsayılan PITR bekletme 35 gün ile bir sanal çekirdek tabanlı hizmet katmanına geçiş yaparken, PITR bekletme uygulamanızın veri kurtarma ilkesi gizliliğinin tehlikeye girmemesini sağlamak için korunur. Varsayılan saklama süresi uyumluluk gereksinimlerinizi karşılamıyorsa, PowerShell veya REST API'sini kullanarak PITR saklama süresini değiştirebilirsiniz. Daha fazla bilgi için [değişiklik yedekleme Bekletme dönemi](#how-to-change-the-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-the-pitr-backup-retention-period"></a>PITR yedekleme saklama süresini değiştirme

Azure portalı, PowerShell veya REST API'yi kullanarak varsayılan PITR yedek saklama süresini değiştirebilirsiniz. Desteklenen değerler şunlardır: 7, 14, 21, 28 veya 35 gün sayısı. Aşağıdaki örnekler için 28 gün PITR bekletmeyi değiştirme işlemini göstermektedir.

> [!NOTE]
> Bu API'ler, yalnızca PITR saklama süresini etkiler. Veritabanınız için LTR yapılandırdıysanız, etkilenmeyecektir. LTR bekletme period(s) değiştirme hakkında daha fazla bilgi için bkz. [uzun süreli saklama](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-the-azure-portal"></a>Azure portalını kullanarak PITR yedekleme bekletme süresi değiştirme

Azure portalını kullanarak PITR yedekleme bekletme süresini değiştirmek, saklama dönemi, portalın içinde değiştirin ve ardından uygun seçeneği belirleyin istediğiniz sunucu nesnesi gitmek hangi sunucu nesnesi üzerinde değişiklik yaptığınız temel.

#### <a name="change-pitr-for-a-sql-database-server"></a>SQL veritabanı sunucusu için PITR Değiştir

![Değiştirme PITR Azure portalı](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="change-pitr-for-a-managed-instance"></a>Yönetilen örnek için PITR Değiştir

![Değiştirme PITR Azure portalı](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

### <a name="change-pitr-backup-retention-period-using-powershell"></a>PowerShell kullanarak değişiklik PITR yedekleme bekletme süresi

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> Azure Resource Manager PowerShell modülü, Azure SQL veritabanı tarafından hala desteklenmektedir, ancak tüm gelecekteki geliştirme için Az.Sql modüldür. Bu cmdlet'ler için bkz. [Azurerm.SQL'e](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Az modül ve AzureRm modülleri komutları için bağımsız değişkenler büyük ölçüde aynıdır.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>REST API kullanarak değişiklik PITR saklama süresi

#### <a name="sample-request"></a>Örnek İstek

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>İstek Gövdesi

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Örnek Yanıtı

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

Daha fazla bilgi için [yedekleme bekletme REST API](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Sonraki adımlar

- Bunlar yanlışlıkla Bozulması veya silinmesi, verilerinizi korumak için veritabanı yedeklerini tüm iş sürekliliği ve olağanüstü durum kurtarma stratejinize önemli bir parçasıdır. Diğer Azure SQL veritabanı iş sürekliliği çözümleri hakkında bilgi edinmek için [iş sürekliliğine genel bakış](sql-database-business-continuity.md).
- Azure portalını kullanarak bir noktaya geri yüklemek için bkz: [veritabanını Azure portalını kullanarak bir noktaya geri yükleme](sql-database-recovery-using-backups.md).
- PowerShell kullanarak bir noktaya geri yüklemek için bkz: [veritabanını PowerShell kullanarak bir noktaya geri yükleme](scripts/sql-database-restore-database-powershell.md).
- Otomatik yedeklemeler Azure portalını kullanarak Azure Blob Depolama alanında yapılandırmak, yönetmek ve uzun süreli saklamak üzere geri yüklemek için bkz: [Azure portalını kullanarak uzun vadeli yedekleme bekletmeyi yönetme](sql-database-long-term-backup-retention-configure.md).
- Otomatik yedeklemeler PowerShell kullanarak Azure Blob Depolama alanında yapılandırmak, yönetmek ve uzun süreli saklamak üzere geri yüklemek için bkz: [PowerShell kullanarak uzun vadeli yedekleme bekletmeyi yönetme](sql-database-long-term-backup-retention-configure.md).
