---
title: Azure yedekleme SQL Server VM'ler için sorun giderme kılavuzu | Microsoft Docs
description: SQL Server Vm'leri Azure'a yedekleme için sorun giderme bilgileri.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
keywords: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2018
ms.author: markgal;anuragm
ms.custom: ''
ms.openlocfilehash: 1c87382c2aae70b022fb391f80f7c75b0a4e5fe6
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296214"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Azure SQL Server'da Yedekleme sorun giderme

Bu makalede, SQL Server Vm'lerinin (Önizleme) azure'da korumak için sorun giderme bilgileri sağlar.

## <a name="public-preview-limitations"></a>Genel Önizleme sınırlamaları

Genel Önizleme sınırlamaları görüntülemek için bkz [Azure SQL Server veritabanında yedekleme](backup-azure-sql-database.md#public-preview-limitations).

## <a name="sql-server-permissions"></a>SQL Server izinleri

Bir sanal makinede bir SQL Server veritabanı için koruma yapılandırılamadı **AzureBackupWindowsWorkload** uzantı, sanal makinede yüklü olmalıdır. Hata iletisi alırsanız **UserErrorSQLNoSysadminMembership**, SQL örneğinizin gerekli yedekleme izinlere sahip değil anlamına gelir. Bu hatayı düzeltmek için adımları [Market dışı SQL VM'ler için izinleri ayarlama](backup-azure-sql-database.md#set-permissions-for-non-marketplace-sql-vms).

## <a name="troubleshooting-errors"></a>Sorun giderme

Sorunları ve Azure SQL Server'a korurken karşılaşılan hataları gidermek için aşağıdaki tablolarda bilgileri kullanın.

## <a name="backup-failures"></a>Yedekleme hataları

Aşağıdaki tablolarda, hata kodu tarafından düzenlenir.

### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu SQL veritabanını İstenen yedekleme türünü desteklemiyor. | Veritabanı kurtarma modelini İstenen yedekleme türüne izin vermediği oluşur. Hata, aşağıdaki durumlarda oluşabilir: <br/><ul><li>Basit kurtarma modeli kullanarak bir veritabanı günlük yedeği izin vermiyor.</li><li>Fark ve günlük yedekleri için asıl veritabanı izin verilmiyor.</li></ul>Daha fazla ayrıntı için [SQL kurtarma modelleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) belgeleri. | Basit kurtarma modeli DB'de için günlük yedekleme başarısız olursa, aşağıdaki seçeneklerden birini deneyin:<ul><li>Veritabanı basit kurtarma modunda ise, günlük yedeklemeleri devre dışı bırakın.</li><li>Kullanım [SQL belgelerine](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) veritabanının kurtarma modeli tam veya toplu günlüğe değiştirmek için. </li><li> Kurtarma modeli değiştirmek istemiyorsanız ve değiştirilemez birden çok veritabanlarını yedeklemek için standart bir ilke varsa, hatayı yoksayın. Tam ve fark Yedeklemelerinizin zamanlama çalışır. Bu durumda beklenen günlük yedeklerini atlanacak.</li></ul>Asıl ise, veritabanı ve yapılandırılmış fark ya da günlük yedekleme, kullanımı aşağıdaki adımları:<ul><li>Portalın ana yedekleme İlkesi zamanlamasını değiştirmek için veritabanı, tam olarak kullanın.</li><li>Değiştirilemez birden çok veritabanlarını yedeklemek için standart bir ilke varsa, hatayı yoksayın. Tam yedekleme zamanlamasını çalışır. Bu durumda beklenen fark ya da günlük yedekleri gerçekleşmez.</li></ul> |
| Çakışan bir işlem aynı veritabanında zaten çalışıyordu gibi işlem iptal edildi. | Bkz: [blog girdisi hakkında ve sınırlamaları geri](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) eşzamanlı olarak çalışan.| [Yedekleme işleri izlemek için SQL Server Management Studio (SSMS) kullanın.](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms) Çakışan işlem başarısız sonra işlemi yeniden deneyin.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL veritabanı yok. | Veritabanı kopya yeniden adlandırılmış veya silinmiş. | <ul><li>Veritabanı yanlışlıkla yeniden adlandırılmış veya silinip silinmediğini denetleyin.</li><li>Yedeklemeler, devam etmek için veritabanı yanlışlıkla silinmişse, veritabanını özgün konumuna geri.</li><li>Veritabanı silinir ve sonra kurtarma Hizmetleri kasası, gelecekteki yedeklemeler tıklamayın varsa [Dur yedekleme "Verileri silme/tut" ile](backup-azure-sql-database.md#manage-azure-backup-operations-for-sql-on-azure-vms).</li>|

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Günlük zinciri bozuk. | Veritabanı veya VM günlük zinciri kesen başka bir yedekleme çözümünü kullanarak yedeklenir.|<ul><li>Onay başka bir yedekleme çözümü veya komut dosyası kullanılıyor. Bu durumda, başka bir yedekleme çözümü durdurun. </li><li>Yedekleme geçici günlük yedeği olduysa, yeni bir günlük zinciri başlatmak için bir tam yedeklemeyi başlatın. Azure Backup hizmeti otomatik olarak bu sorunu gidermek için tam yedekleme tetikleyecek gibi zamanlanmış günlük yedeklemeler için Eylem gerekmiyor.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure yedekleme SQL örneğine bağlanın mümkün değil. | Azure yedekleme, bir SQL örneğine bağlanamıyor. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Başvurmak [SQL yedekleme sorun giderme](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) hatayı düzeltmek için.<br/><ul><li>Varsayılan SQL ayarlarını uzak bağlantılara izin vermiyorsa ayarlarını değiştirin. Başvurmak aşağıdaki ayarları değiştirmek için bağlantıları.<ul><li>[https://msdn.microsoft.com/library/bb326495.aspx](https://msdn.microsoft.com/library/bb326495.aspx)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Oturum açma sorunları varsa başvurmak aşağıdaki bağlantıları düzeltmek için:<ul><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| İlk tam yedekleme için bu veri kaynağı eksik. | Tam yedekleme için veritabanı eksik. Tam yedeklemeler tetiklemeden önce fark düşünülmesi gereken ya da günlük yedeklemelerine tam bir yedekleme günlüğü ve fark yedeklemeleri üst. | Bir geçici tam yedekleme tetikler.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veri kaynağı için işlem günlüğü dolu olarak yedekleme alamıyor. | Veritabanı işlem günlüğü alanı dolu. | Bu sorunu gidermek için bkz [SQL belgelerine](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |
| Bu SQL veritabanını İstenen yedekleme türünü desteklemiyor. | Her zaman AG üzerinde ikincil çoğaltmaları tam ve değişim yedeklemeleri desteklemez. | <ul><li>Bir geçici yedekleme tetiklenen, birincil düğümdeki yedeklemeler tetikler.</li><li>Yedekleme İlkesi tarafından zamanlanmış, birincil düğüm kayıtlı olduğundan emin olun. Düğüm kaydetmek için [bir SQL Server veritabanı bulmak için adımları](backup-azure-sql-database.md#discover-sql-server-databases).</li></ul> | 

## <a name="restore-failures"></a>Hataları geri yükleme

Geri yükleme işleri başarısız olduğunda, aşağıdaki hata kodları gösterilir.

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite 

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedef konumda aynı ada sahip veritabanı zaten var. | Geri yükleme hedef aynı ada sahip bir veritabanı zaten var.  | <ul><li>Hedef veritabanı adını değiştirin</li><li>Ya da geri yükleme sayfasında zorla üzerine yazma seçeneğini kullanın</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veritabanı çevrimdışı yapılamadı geri yükleme başarısız oldu. | Bir geri yükleme yaparken, hedef veritabanı çevrimdışı duruma getirilmesi gerekir. Azure Backup bu verileri çevrimdışı duruma getirin mümkün değil. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Daha fazla bilgi için bkz: [SQL belgelerine](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |


###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Sunucu sertifikası parmak izine sahip hedef bulunamıyor. | Ana hedef örneği üzerinde veritabanı, geçerli şifreleme parmak izi yok. | Kaynak örneğinde hedef örneği için kullanılan geçerli sertifika parmak izi içeri aktarın. |

## <a name="registration-failures"></a>Kayıt hataları

Aşağıdaki hata kodları için kayıt hatalarıdır.

### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError 

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bazı düğümler kullanılabilirlik grubunun kaydedilmemiş gibi SQL her zaman üzerindeki kullanılabilirlik grubu için yedekleme tercih karşılanamıyor. | Yedekleme gerçekleştirmek için gereken düğümleri kayıtlı değil veya ulaşılamıyor. | <ul><li>Bu veritabanının yedeklerini gerçekleştirmek için gereken tüm düğümleri kayıtlı ve sağlıklı ve işlemi yeniden deneyin emin olun.</li><li>Yedekleme tercih SQL her zaman üzerinde kullanılabilirlik grubu Değiştir.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL server VM, iki kapatma ve Azure yedekleme hizmetine erişilemiyor. | VM'yi kapatın | SQL server'ın çalıştığından emin olun. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Backup hizmeti yedekleme yapmak için Azure VM Konuk aracısının kullanır ancak Konuk aracı hedef sunucuda kullanılabilir değil. | Konuk Aracısı etkin değil veya sağlam değil | [VM Konuk Aracısı yükleme](../virtual-machines/extensions/agent-windows.md) el ile. |

## <a name="next-steps"></a>Sonraki adımlar

SQL Server Vm'lerinin (genel Önizleme) için Azure yedekleme hakkında daha fazla bilgi için bkz: [SQL VM'ler (genel Önizleme) için Azure Backup](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).