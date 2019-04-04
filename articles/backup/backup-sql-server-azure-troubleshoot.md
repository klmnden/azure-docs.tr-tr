---
title: SQL Server veritabanı yedekleme Azure yedekleme ile ilgili sorunları giderme | Microsoft Docs
description: Azure Backup ile Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedeklemek için sorun giderme bilgileri sağlar.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 03/13/2019
ms.author: anuragm
ms.openlocfilehash: db204c0e881200f667484daf4348c336f94a0ce7
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58916697"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Azure'da SQL Server Yedekleme sorunlarını giderme

Bu makalede, SQL Server Vm'leri (Önizleme) azure'da korunması için sorun giderme bilgileri sağlar.

## <a name="feature-consideration-and-limitations"></a>Özellik önemli noktalar ve sınırlamalar

Özellik göz önünde bulundurarak görüntülemek için bkz [Azure vm'lerde SQL Server hakkında yedekleme](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>SQL Server izinleri

Bir sanal makinede SQL Server veritabanı için koruma yapılandırılamadı **AzureBackupWindowsWorkload** uzantısı, bu sanal makinede yüklü olması gerekir. Bir hata alırsanız **UserErrorSQLNoSysadminMembership**, SQL örneğinizin, gerekli yedekleme izinleri yok anlamına gelir. Bu hatayı düzeltmek için adımları izleyin. [Market dışı SQL VM'ler için izinleri ayarla](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).

## <a name="troubleshooting-errors"></a>Sorun giderme hataları

Bilgileri aşağıdaki tablolarda, sorunlar ve Azure SQL Server'ı korurken karşılaşılan hataları giderme öğrenin.

## <a name="alerts"></a>Uyarılar

### <a name="backup-type-unsupported"></a>Yedekleme türü desteklenmiyor

| Severity | Açıklama | Olası nedenler | Önerilen eylem |
|---|---|---|---|
| Uyarı | Bu veritabanı için geçerli ayarları, belirli türde bir ilişkili ilkeyi mevcut yedekleme türleri desteklemez. | <li>**Ana DB**: Yalnızca tam veritabanı yedekleme işlemi, ana veritabanı üzerinde gerçekleştirilebilir; ne **fark** yedekleme ya da işlem **günlükleri** yedekleme mümkündür. </li> <li>Herhangi bir veritabanına **basit kurtarma modelini** işlem için izin vermiyor **günlükleri** gerçekleştirilecek yedekleme.</li> | Veritabanı ayarlarını, ilke yedekleme türleri desteklenir gibi değiştirin. Alternatif olarak, geçerli ilkeyi yalnızca desteklenen yedekleme türleri içerecek şekilde değiştirin. Aksi takdirde, desteklenmeyen yedekleme türleri zamanlanan yedekleme sırasında atlanacak veya geçici yedekleme için yedekleme işi başarısız olur.


## <a name="backup-failures"></a>Yedekleme hataları

Aşağıdaki tablolarda, hata kodu tarafından düzenlenir.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu SQL veritabanı İstenen yedekleme türünü desteklemiyor. | Veritabanı kurtarma modeli İstenen yedekleme türünü izin vermeyen oluşur. Hata aşağıdaki durumlarda oluşabilir: <br/><ul><li>Basit kurtarma modelini kullanarak veritabanı günlük yedeği izin vermez.</li><li>Değişiklik ve günlük yedekleri için asıl veritabanı izin verilmez.</li></ul>Daha fazla ayrıntı için [SQL kurtarma modelleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) belgeleri. | Basit kurtarma modelinde bir veritabanı için günlük yedekleme başarısız olursa, aşağıdaki seçeneklerden birini deneyin:<ul><li>Veritabanı basit kurtarma modunda ise günlük yedeklemeleri devre dışı bırakın.</li><li>Kullanım [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) veritabanı kurtarma modeli tam veya toplu günlüğe değiştirmek için. </li><li> Kurtarma modeli değiştirmek istemiyorsanız ve değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa hatayı yoksayın. Tam ve farklı yedeklemelerini zamanlamaya çalışır. Bu durumda beklenen günlük yedekleme atlanacak.</li></ul>Salt okunur ise asıl veritabanını yapılandırdığınızdan fark ya da günlük yedekleme, kullanım aşağıdaki adımları:<ul><li>Portal Yöneticisi Yedekleme İlkesi zamanlamasını değiştirmek için veritabanı, tam olarak kullanın.</li><li>Değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa, hatayı yoksayın. Tam yedekleme zamanlaması çalışır. Bu durumda beklenen fark ya da günlük yedeklemeler gerçekleşmez.</li></ul> |
| Aynı veritabanında çakışan bir işlem zaten çalışmakta olduğundan işlem iptal edildi. | Bkz: [hakkındaki blog girişine yedekleme ve geri yükleme sınırlamaları](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) , aynı anda çalışan.| [Yedekleme işleri izlemek için SQL Server Management Studio (SSMS) kullanın.](manage-monitor-sql-database-backup.md) Çakışan bir işlem başarısız olursa, sonra işlemi yeniden başlatın.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL veritabanı yok. | Veritabanı ya da yeniden adlandırılmış veya silinmiş. | Veritabanı yanlışlıkla yeniden adlandırılmış veya silinip silinmediğini denetleyin.<br/><br/> Yedeklemeler devam etmek için veritabanı yanlışlıkla silinmişse, veritabanını özgün konuma geri.<br/><br/> Veritabanını ve ardından Kurtarma Hizmetleri Kasası'nda, gelecekteki yedeklemeler tıklamayın varsa ["Verileri silme/tut" içeren yedeklemeyi durdurma](manage-monitor-sql-database-backup.md).

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Günlük zinciri bozuk. | Veritabanı ya da VM günlük zinciri keser başka bir yedekleme çözümü kullanarak yedeklenir.|<ul><li>Onay başka bir yedekleme çözümü veya komut dosyası kullanılıyor. Bu durumda, başka bir yedekleme çözümü durdurun. </li><li>Yedekleme bir geçici günlük yedeği varsa, yeni bir günlük zinciri başlatmak için tam yedekleme tetikleyin. Azure Backup hizmeti, bu sorunu gidermek için tam bir yedekleme otomatik olarak tetikleyecek şekilde zamanlanmış günlük yedekleme için hiçbir eylem gerekmiyor.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure yedekleme, SQL örneğine bağlanın mümkün değil. | Azure yedekleme, SQL örneğine bağlanamıyor. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Başvurmak [SQL yedekleme sorunlarını giderme](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) hatayı düzeltmek için.<br/><ul><li>Varsayılan SQL ayarları uzak bağlantılara izin vermiyorsa ayarlarını değiştirin. Ayarları değiştirme hakkında bilgi için aşağıdaki makalelere bakın.<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Oturum açma sorunları alıyorsa, aşağıdaki bağlantıları Düzelt için:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu veri kaynağı için ilk tam yedekleme eksik. | Veritabanı için tam yedekleme eksik. Tam yedeklemeler tetiklemeden önce fark atılmalıdır ya da günlük yedeklemelerine tam bir yedekleme günlüğü ve fark yedekleri üst. | Geçici bir tam yedekleme tetikleyin.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veri kaynağı için işlem günlüğü dolu olduğundan yedek alınamıyor. | Veritabanı işlem günlüğü alanı dolu. | Bu sorunu gidermek için başvurmak [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Bu SQL veritabanı İstenen yedekleme türünü desteklemiyor. | Her zaman ağ üzerinde ikincil çoğaltmaları, tam ve farklı yedeklemelerini desteklemez. | <ul><li>Geçici bir yedekleme tetikleyen, birincil düğümdeki yedeklemeler tetikler.</li><li>Yedekleme İlkesi tarafından zamanladıysanız birincil düğüm kayıtlı olduğundan emin olun. Düğümünü kaydetmek için [bir SQL Server veritabanı bulmak için adımları](backup-sql-server-database-azure-vms.md#discover-sql-server-databases).</li></ul> |

## <a name="restore-failures"></a>Geri yükleme hatalarının

Geri yükleme işleri başarısız olduğunda, aşağıdaki hata kodları gösterilir.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Aynı ada sahip bir veritabanı zaten hedef konumda var | Hedef geri yükleme, aynı ada sahip bir veritabanı zaten var.  | <ul><li>Hedef veritabanı adını değiştirin</li><li>Veya geri yükleme sayfasında zorla üzerine yaz seçeneğini kullanın</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veritabanını çevrimdışı duruma getirilemiyor geri yükleme başarısız oldu. | Bir geri yükleme yaparken, hedef veritabanının çevrimdışı duruma getirilmesi gerekir. Azure Backup, bu veriyi çevrimdışı duruma getirmek mümkün değil. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Daha fazla bilgi için [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedefte parmak izli sunucu sertifikası bulunamıyor. | Ana hedef örneğinde veritabanını, geçerli şifreleme parmak izi yok. | Kaynak örneğinde hedef örneği için kullanılan geçerli sertifika parmak izini alın. |

### <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kurtarma için kullanılan günlük yedeği toplu günlüğe kaydedilen değişiklikler içeriyor. SQL yönergelerine göre zaman içindeki rastgele bir noktada durdurmak için kullanılamaz. | Bir veritabanında toplu günlüğe kaydedilen kurtarma modunda olduğunda, bir toplu işlem ve sonraki günlük işlem arasındaki verileri kurtarılamaz. | Lütfen farklı bir noktaya kurtarma için seçin. [Daha fazla bilgi edinin](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105))


## <a name="registration-failures"></a>Kayıt hataları

Aşağıdaki hata kodları için kayıt hatalarıdır.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL Always On kullanılabilirlik grubu için yedekleme tercihi, kullanılabilirlik grubu'nın bazı düğümleri kaydedilmediğinden karşılanamıyor. | Yedekleme gerçekleştirmek için gereken düğümleri kayıtlı değil veya ulaşılamıyor. | <ul><li>Bu veritabanının yedekleme gerçekleştirmek için gereken tüm düğümlerin kaydedildiğinden ve sağlıklı durumda ve sonra işlemi yeniden deneyin emin olun.</li><li>Değişiklik SQL Always On kullanılabilirlik grubu yedekleme tercihi.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL server sanal makinesi kapatılmış olduğundan ve Azure yedekleme hizmetine erişilemiyor. | VM'yi kapatın | SQL server'ın çalıştığından emin olun. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Backup hizmeti yedekleme işlemi için Azure VM Konuk aracısını kullanır, ancak Konuk aracı hedef sunucuda kullanılabilir değil. | Konuk Aracısı etkin değil veya sağlıksız | [VM Konuk Aracısı yükleme](../virtual-machines/extensions/agent-windows.md) el ile. |

## <a name="configure-backup-failures"></a>Yedekleme hataları yapılandırın

Aşağıdaki hata kodları için yapılandırmasını yedekleme hataları.

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Otomatik koruma hedefini ya da kaldırıldı veya artık geçerli değil. | Bir SQL örneğinde otomatik korumayı etkinleştirdiğinizde **yedeklemeyi Yapılandır** işlerini çalıştırmak için bu örneği içindeki tüm veritabanlarına. İşleri çalıştırırken otomatik korumayı devre dışı bırakırsanız sonra **sürüyor** bu hata kodu ile işleri iptal edilir. | Kalan tüm veritabanlarını yeniden korumak otomatik korumayı etkinleştirin. |

## <a name="re-registration-failures"></a>Yeniden kayıt hataları

Bir veya daha fazlası için onay [belirtileri](#symptoms) yeniden kayıt işlemini tetiklemeden önce.

### <a name="symptoms"></a>Belirtiler

* Yedekleme gibi tüm işlemleri geri yükleyin ve yedeklemeyi yapılandırma VM aşağıdaki hata kodlarından biriyle başarısız oluyor: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**
* **Yedekleme durumu** yedekleme öğesi gösterildiğini **ulaşılamıyor**. Aynı durumunda da sonuçlanabilir ve diğer tüm sebepler kullanıma kuralı olsa da:

  * Sanal makinedeki yedekleme ilgili işlemleri gerçekleştirmek için izni olmaması  
  * VM yer alamaz yedeklemeleri nedeniyle kapatıldı
  * Ağ sorunları  

    ![VM yeniden kaydedin](./media/backup-azure-sql-database/re-register-vm.png)

* Yedekleme tercihi değiştikten sonra veya bir yük devretme değiştirildiği başarısız olması durumunda her zaman açık kullanılabilirlik grubu, yedeklemeleri başlatıldı

### <a name="causes"></a>Nedenler
Aşağıdaki belirtilerden birini veya daha fazlasını aşağıdaki nedenlerden dolayı ortaya çıkabilir:

  * Uzantı silinmiş veya Portalı'ndan kaldırıldı 
  * Uzantı kaldırıldığında **Denetim Masası** altında VM'nin **bir programı kaldırın veya değiştirin** kullanıcı Arabirimi
  * VM yerinde diskleri geri yükleme kullanarak geçmişe geri yüklendi
  * VM üzerindeki uzantı yapılandırması nedeniyle dolduğu uzun bir süre için kapatıldı
  * VM silindikten sonra ve başka bir sanal Makineye aynı ada sahip ve Silinen sanal Makinenin aynı kaynak grubunda oluşturuldu
  * AG düğümlerinden biri tam yedekleme yapılandırma almadınız, bu ya da kullanılabilirlik grubu kayıt kasaya zaman veya ne zaman yeni bir düğüm eklenir oluşabilir  <br>
   
Yukarıdaki senaryolarda, yeniden kayıt işlemi VM'de tetiklemek için önerilir. Bu seçenek yalnızca PowerShell üzerinden kullanılabilir ve yakında Azure portalında kullanıma sunulacak.


## <a name="next-steps"></a>Sonraki adımlar

SQL Server Vm'leri (genel Önizleme) için Azure yedekleme hakkında daha fazla bilgi için bkz. [SQL Vm'leri (genel Önizleme) için Azure Backup](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
