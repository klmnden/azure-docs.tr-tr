---
title: Azure Backup'ı kullanarak SQL Server veritabanı yedekleme sorunlarını giderme | Microsoft Docs
description: Azure Backup ile Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedeklemek için sorun giderme bilgileri sağlar.
services: backup
author: anuragm
manager: sivan
ms.service: backup
ms.topic: article
ms.date: 06/18/2019
ms.author: anuragm
ms.openlocfilehash: b2822a3c7dfa23065f2cbd35ef4e506efae026f2
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704842"
---
# <a name="troubleshoot-sql-server-database-backup-by-using-azure-backup"></a>Azure Backup'ı kullanarak SQL Server veritabanı yedekleme sorunlarını giderme

Bu makalede, Azure sanal makineler üzerinde çalışan SQL Server veritabanlarına yönelik sorun giderme bilgileri sağlar.

Yedekleme işlemi ve sınırlamalar hakkında daha fazla bilgi için bkz. [Azure vm'lerde SQL Server hakkında yedekleme](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>SQL Server izinleri

Bir sanal makinede SQL Server veritabanı için korumayı yapılandırmak için yüklemelisiniz **AzureBackupWindowsWorkload** uzantısı bu sanal makine üzerinde. Hata alırsanız **UserErrorSQLNoSysadminMembership**, SQL Server örneğinizi, gerekli yedekleme izinleri yok anlamına gelir. Bu hatayı düzeltmek için adımları izleyin. [kümesi VM izinleri](backup-azure-sql-database.md#set-vm-permissions).

## <a name="error-messages"></a>Hata iletileri

### <a name="backup-type-unsupported"></a>Yedekleme türü desteklenmiyor

| Severity | Açıklama | Olası nedenler | Önerilen eylem |
|---|---|---|---|
| Uyarı | Bu veritabanı için geçerli ayarları ilişkili ilkeyi mevcut belirli yedekleme türleri desteklemez. | <li>Yalnızca tam veritabanı yedekleme işlemi ana veritabanında gerçekleştirilemez. Değişiklik yedeği ya da işlem günlüğü yedeklemesi mümkündür. </li> <li>Basit kurtarma modelinde herhangi bir veritabanı işlem günlüklerinin yedeklemeye izin vermez.</li> | Veritabanı ayarlarını, ilke yedekleme türleri desteklenir gibi değiştirin. Ya da, geçerli ilkeyi yalnızca desteklenen yedekleme türleri içerecek şekilde değiştirin. Aksi takdirde, desteklenmeyen yedekleme türleri zamanlanan yedekleme sırasında atlanacak veya geçici yedekleme için yedekleme işi başarısız olur.


### <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu SQL veritabanı İstenen yedekleme türünü desteklemiyor. | Veritabanı kurtarma modeli İstenen yedekleme türünü izin vermeyen oluşur. Hata aşağıdaki durumlarda oluşabilir: <br/><ul><li>Basit kurtarma modelini kullanarak bir veritabanı günlük yedeği izin vermez.</li><li>Bir ana veritabanı için değişiklik ve günlük yedekleri izin verilmiyor.</li></ul>Daha fazla ayrıntı için [SQL Server kurtarma modelleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) belgeleri. | Veritabanı basit kurtarma modelinde için günlük yedekleme başarısız olursa, aşağıdaki seçeneklerden birini deneyin:<ul><li>Veritabanı basit kurtarma modunda ise günlük yedeklemeleri devre dışı bırakın.</li><li>Kullanım [SQL Server belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) veritabanı kurtarma modeli tam veya toplu günlüğe yazılan değiştirmek için. </li><li> Kurtarma modeli değiştirmek istemiyorsanız ve değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa hatayı yoksayın. Tam ve farklı yedeklemelerini zamanlamaya çalışır. Bu durumda beklenen günlük yedekleme atlanacak.</li></ul>Ana veritabanında ise ve fark yapılandırmış olmanız ya da günlük yedeği, aşağıdaki adımlardan birini kullanın:<ul><li>Ana veritabanı için yedekleme İlkesi zamanlamasını tam olarak değiştirmek için portal'ı kullanın.</li><li>Değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa, hatayı yoksayın. Tam yedekleme zamanlaması çalışır. Bu durumda beklenen fark ya da günlük yedeklemeler gerçekleşmez.</li></ul> |
| Aynı veritabanında çakışan bir işlem zaten çalışmakta olduğundan işlem iptal edildi. | Bkz: [yedekleme ve geri yükleme sınırlamaları hakkında blog girişine](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) , aynı anda çalışan.| [Yedekleme işleri izlemek için SQL Server Management Studio (SSMS) kullanma](manage-monitor-sql-database-backup.md). Çakışan bir işlem başarısız olduktan sonra işlemi yeniden başlatın.|

### <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL veritabanı yok. | Veritabanı ya da yeniden adlandırılmış veya silinmiş. | Veritabanı yanlışlıkla yeniden adlandırılmış veya silinip silinmediğini denetleyin.<br/><br/> Yedeklemeler devam etmek için veritabanı yanlışlıkla silinmişse, veritabanını özgün konuma geri.<br/><br/> Veritabanını ve ardından Kurtarma Hizmetleri Kasası'nda, gelecekteki yedeklemeler seçmeyin varsa **yedeklemeyi Durdur** ile **yedekleme verilerini koru** veya **yedekleme verilerini Sil**. Daha fazla bilgi için [yönetme ve izleme yedeklenen SQL Server veritabanlarını](manage-monitor-sql-database-backup.md).

### <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Günlük zinciri bozuk. | Veritabanı ya da VM günlük zinciri keser başka bir yedekleme çözümü desteklenir.|<ul><li>Onay başka bir yedekleme çözümü veya komut dosyası kullanılıyor. Bu durumda, başka bir yedekleme çözümü durdurun. </li><li>Yedekleme bir geçici günlük yedeği varsa, yeni bir günlük zinciri başlatmak için tam yedekleme tetikleyin. Azure Backup hizmeti, otomatik olarak bu sorunu gidermek için bir tam yedekleme gerçekleştirir, çünkü zamanlanmış günlük yedekleme için hiçbir eylem gerekmiyor.</li>|

### <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure yedekleme, SQL örneğine bağlanın mümkün değil. | Azure yedekleme SQL Server örneğine bağlanamıyor. | Ek ayrıntılar Azure portal hata menüsünde nedenlerini daraltmak için kullanın. Başvurmak [SQL yedekleme sorunlarını giderme](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) hatayı düzeltmek için.<br/><ul><li>Varsayılan SQL ayarları uzak bağlantılara izin ayarlarını değiştirin. Ayarları değiştirme hakkında bilgi için aşağıdaki makalelere bakın:<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Oturum açma sorunları varsa, bunları düzeltmek için aşağıdaki bağlantıları kullanın:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

### <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu veri kaynağı için ilk tam yedekleme eksik. | Veritabanı için tam yedekleme eksik. Günlük ve fark yedekleme tam yedekleme amcanız, bu nedenle tetiklemeden önce tam yedeklemeler değişiklik yapmaya ya da günlük yedeklemelerine emin olun. | Geçici bir tam yedekleme tetikleyin.   |

### <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veri kaynağı için işlem günlüğü dolu olduğundan yedek alınamıyor. | Veritabanı işlem günlüğü alanı dolu. | Bu sorunu gidermek için başvurmak [SQL Server belgeleri](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

### <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Aynı ada sahip bir veritabanı zaten hedef konumda var | Hedef geri yükleme, aynı ada sahip bir veritabanı zaten var.  | <ul><li>Hedef veritabanı adını değiştirin.</li><li>Veya geri yükleme sayfasında zorla üzerine yaz seçeneğini kullanın.</li> |

### <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veritabanı çevrimdışına alınamadığından geri yükleme başarısız oldu. | Bir geri yükleme yaparken, hedef veritabanını çevrimdışı duruma getirilmesi gerekir. Azure Backup, bu verileri çevrimdışı yapılamıyor. | Ek ayrıntılar Azure portal hata menüsünde nedenlerini daraltmak için kullanın. Daha fazla bilgi için [SQL Server belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

###  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedefte parmak izli sunucu sertifikası bulunamıyor. | Hedef örneğinde ana veritabanını geçerli şifreleme parmak izi yok. | Kaynak örneğinde hedef örneği için kullanılan geçerli sertifika parmak izini alın. |

### <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kurtarma için kullanılan günlük yedeklemesi toplu olarak günlüğe kaydedilen değişiklikler içeriyor. SQL yönergelerine göre zamanın rastgele bir noktasında durdurmak için kullanılamaz. | Bir veritabanı Toplu Kaydedilmiş kurtarma modunda olduğunda, bir toplu işlem ve sonraki günlük işlem arasındaki verileri kurtarılamaz. | Farklı bir noktaya kurtarma için seçin. [Daha fazla bilgi edinin](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105)).


### <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kullanılabilirlik Grubunun bazı düğümleri kaydedilmediğinden SQL AlwaysOn Kullanılabilirlik Grubu yedekleme tercihi uygulanamadı. | Yedekleme gerçekleştirmek için gereken düğümleri kayıtlı değil veya ulaşılamıyor. | <ul><li>Bu veritabanının yedekleme gerçekleştirmek için gereken tüm düğümlerin kaydedildiğinden ve sağlıklı durumda olduğundan ve sonra işlemi yeniden deneyin emin olun.</li><li>SQL Server Always On kullanılabilirlik grubu yedekleme tercihini değiştirin.</li></ul> |

### <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL server sanal makinesi kapatılmış olduğundan ve Azure yedekleme hizmetine erişilemiyor. | VM'yi kapatın. | SQL Server örneğinin çalıştığından emin olun. |

### <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Backup hizmeti yedekleme işlemi için Azure VM Konuk aracısını kullanır, ancak Konuk aracı hedef sunucuda kullanılabilir değil. | Konuk Aracısı etkin değil veya sağlam değil. | [VM Konuk Aracısı yükleme](../virtual-machines/extensions/agent-windows.md) el ile. |

### <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Otomatik koruma hedefini ya da kaldırıldı veya artık geçerli değil. | Bir SQL Server örneğinde otomatik korumayı etkinleştirdiğinizde **yedeklemeyi Yapılandır** işlerini çalıştırmak için bu örneği içindeki tüm veritabanlarına. İşleri çalıştırırken otomatik korumayı devre dışı bırakırsanız sonra **sürüyor** bu hata kodu ile işleri iptal edilir. | Bir kez daha kalan tüm veritabanlarını korumaya yardımcı olmak otomatik korumayı etkinleştirin. |

## <a name="re-registration-failures"></a>Yeniden kayıt hataları

Yeniden kayıt işlemini tetiklemeden önce bir veya daha fazla aşağıdaki belirtilerden için denetleyin:

* Tüm işlemler (Yedekleme gibi geri yükleyin ve yedeklemeyi yapılandırma) VM aşağıdaki hata kodlarından biriyle başarısız oluyor: **WorkloadExtensionNotReachable**, **UserErrorWorkloadExtensionNotInstalled**, **WorkloadExtensionNotPresent**, **WorkloadExtensionDidntDequeueMsg**.
* **Yedekleme durumu** yedekleme öğesi için alan olduğunu gösteren **ulaşılamıyor**. Aynı durum neden diğer tüm nedenler kullanıma kural:

  * VM yedekleme ile ilgili işlemler gerçekleştirme izni olmaması  
  * VM'yi kapatın, bunu yedeklemeler yer alamaz
  * Ağ sorunları  

  ![Bir VM'yi yeniden kaydetmeden "Ulaşılabilir değil" durumu](./media/backup-azure-sql-database/re-register-vm.png)

* Bir Always On kullanılabilirlik grubu söz konusu olduğunda, yedeklemelerin yedekleme tercihi değiştikten sonra veya bir yük devretme sonrasında başarısız başlatıldı.

Aşağıdaki belirtilerden birini ya da aşağıdaki nedenlerle ortaya çıkabilir:

* Bir uzantı silindi veya Portalı'ndan kaldırıldı. 
* Bir uzantı kaldırılıp kaldırılmadığını **Denetim Masası** altında VM'de **bir programı kaldırın veya değiştirin**.
* VM yeniden aracılığıyla yerinde disk geri yükleme süresini de geri yüklendi.
* VM uzantısı yapılandırma üzerindeki süresi uzun bir süre için kapatıldı.
* VM silindikten sonra ve başka bir sanal Makineye aynı ada sahip ve Silinen sanal Makinenin aynı kaynak grubunda oluşturuldu.
* Kullanılabilirlik grubu düğümlerinden biri tam yedekleme yapılandırma alamadık. Kullanılabilirlik grubu yeni bir düğüm eklendiğinde veya kasaya kaydettiğinizde bu durum oluşabilir.

Yukarıdaki senaryolarda, VM üzerinde yeniden kaydedin bir işlemi tetiklemek öneririz. Şu an için bu seçeneği yalnızca PowerShell üzerinden kullanılabilir.

## <a name="size-limit-for-files"></a>Dosyaları için boyut sınırı

Yalnızca dosyaların sayısı, aynı zamanda bunların adlarını ve yollarını dosyalarının toplam dize boyutuna bağlıdır. Her veritabanı dosyasının fiziksel yolunu ve mantıksal dosya adını alın. Bu SQL sorgusu kullanabilirsiniz:

```
SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
               INNER JOIN sys.databases db ON db.database_id = mf.database_id
               WHERE db.name = N'<Database Name>'"
```

Artık bunları şu biçimde düzenleyin:

```
[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
```

Bir örneği aşağıda verilmiştir:

```
[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
```

İçerik dize boyutunu 20.000 bayt aşarsa, veritabanı dosyalarını farklı şekilde depolanır. Kurtarma sırasında geri yükleme için hedef dosya yolunu ayarlamak mümkün olmayacaktır. SQL Server tarafından sağlanan varsayılan SQL yol dosyaları geri yüklenir.

### <a name="override-the-default-target-restore-file-path"></a>Varsayılan hedef geri yükleme dosya yolu geçersiz kıl

Hedef geri yükleme dosya yolunu eşleme veritabanı dosyası geri yükleme hedef yolu içeren bir JSON dosyası yerleştirerek geri yükleme işlemi sırasında geçersiz kılabilirsiniz. Oluşturma bir `database_name.json` konuma yerleştirin ve dosya *C:\Program Files\Azure iş yükü Backup\bin\plugins\SQL*.

Dosyanın içeriği şu biçimde olmalıdır:
```
[
  {
    "Path": "<Restore_Path>",
    "LogicalName": "<LogicalName>",
    "IsDir": "false"
  },
  {
    "Path": "<Restore_Path>",
    "LogicalName": "LogicalName",
    "IsDir": "false"
  },  
]
```

Bir örneği aşağıda verilmiştir:

```
[
  {
   "Path": "F:\\Data\\testdb2_1546408741449456.mdf",
   "LogicalName": "testdb7",
   "IsDir": "false"
  },
  {
    "Path": "F:\\Log\\testdb2_log_1546408741449456.ldf",
    "LogicalName": "testdb7_log",
    "IsDir": "false"
  },  
]
```

Önceki içeriğinde, aşağıdaki SQL sorgusunu kullanarak mantıksal veritabanı dosyası adını alabilirsiniz:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Bu dosya geri yükleme işlemini tetiklemeden önce yerleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

SQL Server Vm'leri (genel Önizleme) için Azure yedekleme hakkında daha fazla bilgi için bkz. [SQL VM'ler için Azure Backup](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
