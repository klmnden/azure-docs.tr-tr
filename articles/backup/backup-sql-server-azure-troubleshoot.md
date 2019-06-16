---
title: SQL Server veritabanı yedekleme Azure yedekleme ile ilgili sorunları giderme | Microsoft Docs
description: Azure Backup ile Azure Vm'leri üzerinde çalışan SQL Server veritabanlarını yedeklemek için sorun giderme bilgileri sağlar.
services: backup
author: anuragm
manager: shivamg
ms.service: backup
ms.topic: article
ms.date: 05/27/2019
ms.author: anuragm
ms.openlocfilehash: 8459bb451c4ff462ee816b986cafdbf776603917
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306963"
---
# <a name="troubleshoot-back-up-sql-server-on-azure"></a>Azure'da SQL Server Yedekleme sorunlarını giderme

Bu makalede, azure'da SQL Server Vm'leri korumak için sorun giderme bilgileri sağlar.

## <a name="feature-consideration-and-limitations"></a>Özellik önemli noktalar ve sınırlamalar

Özellik göz önünde bulundurarak görüntülemek için bkz [Azure vm'lerde SQL Server hakkında yedekleme](backup-azure-sql-database.md#feature-consideration-and-limitations).

## <a name="sql-server-permissions"></a>SQL Server izinleri

Bir sanal makinede SQL Server veritabanı için koruma yapılandırılamadı **AzureBackupWindowsWorkload** uzantısı, bu sanal makinede yüklü olması gerekir. Bir hata alırsanız **UserErrorSQLNoSysadminMembership**, SQL örneğinizin, gerekli yedekleme izinleri yok anlamına gelir. Bu hatayı düzeltmek için adımları izleyin. [Market dışı SQL VM'ler için izinleri ayarla](backup-azure-sql-database.md#fix-sql-sysadmin-permissions).


## <a name="backup-type-unsupported"></a>Yedekleme türü desteklenmiyor

| Severity | Açıklama | Olası nedenler | Önerilen eylem |
|---|---|---|---|
| Uyarı | Bu veritabanı için geçerli ayarları, belirli türde bir ilişkili ilkeyi mevcut yedekleme türleri desteklemez. | <li>**Ana DB**: Yalnızca tam veritabanı yedekleme işlemi, ana veritabanı üzerinde gerçekleştirilebilir; ne **fark** yedekleme ya da işlem **günlükleri** yedekleme mümkündür. </li> <li>Herhangi bir veritabanına **basit kurtarma modelini** işlem için izin vermiyor **günlükleri** gerçekleştirilecek yedekleme.</li> | Veritabanı ayarlarını, ilke yedekleme türleri desteklenir gibi değiştirin. Alternatif olarak, geçerli ilkeyi yalnızca desteklenen yedekleme türleri içerecek şekilde değiştirin. Aksi takdirde, desteklenmeyen yedekleme türleri zamanlanan yedekleme sırasında atlanacak veya geçici yedekleme için yedekleme işi başarısız olur.


## <a name="usererrorsqlpodoesnotsupportbackuptype"></a>UserErrorSQLPODoesNotSupportBackupType

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu SQL veritabanı İstenen yedekleme türünü desteklemiyor. | Veritabanı kurtarma modeli İstenen yedekleme türünü izin vermeyen oluşur. Hata aşağıdaki durumlarda oluşabilir: <br/><ul><li>Basit kurtarma modelini kullanarak veritabanı günlük yedeği izin vermez.</li><li>Değişiklik ve günlük yedekleri için asıl veritabanı izin verilmez.</li></ul>Daha fazla ayrıntı için [SQL kurtarma modelleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/recovery-models-sql-server) belgeleri. | Basit kurtarma modelinde bir veritabanı için günlük yedekleme başarısız olursa, aşağıdaki seçeneklerden birini deneyin:<ul><li>Veritabanı basit kurtarma modunda ise günlük yedeklemeleri devre dışı bırakın.</li><li>Kullanım [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/view-or-change-the-recovery-model-of-a-database-sql-server) veritabanı kurtarma modeli tam veya toplu günlüğe değiştirmek için. </li><li> Kurtarma modeli değiştirmek istemiyorsanız ve değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa hatayı yoksayın. Tam ve farklı yedeklemelerini zamanlamaya çalışır. Bu durumda beklenen günlük yedekleme atlanacak.</li></ul>Salt okunur ise asıl veritabanını yapılandırdığınızdan fark ya da günlük yedekleme, kullanım aşağıdaki adımları:<ul><li>Portal Yöneticisi Yedekleme İlkesi zamanlamasını değiştirmek için veritabanı, tam olarak kullanın.</li><li>Değiştirilemeyen birden çok veritabanlarını yedeklemek için standart bir ilke varsa, hatayı yoksayın. Tam yedekleme zamanlaması çalışır. Bu durumda beklenen fark ya da günlük yedeklemeler gerçekleşmez.</li></ul> |
| Aynı veritabanında çakışan bir işlem zaten çalışmakta olduğundan işlem iptal edildi. | Bkz: [hakkındaki blog girişine yedekleme ve geri yükleme sınırlamaları](https://blogs.msdn.microsoft.com/arvindsh/2008/12/30/concurrency-of-full-differential-and-log-backups-on-the-same-database) , aynı anda çalışan.| [Yedekleme işleri izlemek için SQL Server Management Studio (SSMS) kullanın.](manage-monitor-sql-database-backup.md) Çakışan bir işlem başarısız olursa, sonra işlemi yeniden başlatın.|

## <a name="usererrorsqlpodoesnotexist"></a>UserErrorSQLPODoesNotExist

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL veritabanı yok. | Veritabanı ya da yeniden adlandırılmış veya silinmiş. | Veritabanı yanlışlıkla yeniden adlandırılmış veya silinip silinmediğini denetleyin.<br/><br/> Yedeklemeler devam etmek için veritabanı yanlışlıkla silinmişse, veritabanını özgün konuma geri.<br/><br/> Veritabanını ve ardından Kurtarma Hizmetleri Kasası'nda, gelecekteki yedeklemeler tıklamayın varsa ["Verileri silme/tut" içeren yedeklemeyi durdurma](manage-monitor-sql-database-backup.md).

## <a name="usererrorsqllsnvalidationfailure"></a>UserErrorSQLLSNValidationFailure

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Günlük zinciri bozuk. | Veritabanı ya da VM günlük zinciri keser başka bir yedekleme çözümü kullanarak yedeklenir.|<ul><li>Onay başka bir yedekleme çözümü veya komut dosyası kullanılıyor. Bu durumda, başka bir yedekleme çözümü durdurun. </li><li>Yedekleme bir geçici günlük yedeği varsa, yeni bir günlük zinciri başlatmak için tam yedekleme tetikleyin. Azure Backup hizmeti, bu sorunu gidermek için tam bir yedekleme otomatik olarak tetikleyecek şekilde zamanlanmış günlük yedekleme için hiçbir eylem gerekmiyor.</li>|

## <a name="usererroropeningsqlconnection"></a>UserErrorOpeningSQLConnection

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure yedekleme, SQL örneğine bağlanın mümkün değil. | Azure yedekleme, SQL örneğine bağlanamıyor. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Başvurmak [SQL yedekleme sorunlarını giderme](https://docs.microsoft.com/sql/database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine) hatayı düzeltmek için.<br/><ul><li>Varsayılan SQL ayarları uzak bağlantılara izin vermiyorsa ayarlarını değiştirin. Ayarları değiştirme hakkında bilgi için aşağıdaki makalelere bakın.<ul><li>[MSSQLSERVER_-1](/previous-versions/sql/sql-server-2016/bb326495(v=sql.130))</li><li>[MSSQLSERVER_2](/sql/relational-databases/errors-events/mssqlserver-2-database-engine-error)</li><li>[MSSQLSERVER_53](/sql/relational-databases/errors-events/mssqlserver-53-database-engine-error)</li></ul></li></ul><ul><li>Oturum açma sorunları alıyorsa, aşağıdaki bağlantıları Düzelt için:<ul><li>[MSSQLSERVER_18456](/sql/relational-databases/errors-events/mssqlserver-18456-database-engine-error)</li><li>[MSSQLSERVER_18452](/sql/relational-databases/errors-events/mssqlserver-18452-database-engine-error)</li></ul></li></ul> |

## <a name="usererrorparentfullbackupmissing"></a>UserErrorParentFullBackupMissing

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Bu veri kaynağı için ilk tam yedekleme eksik. | Veritabanı için tam yedekleme eksik. Tam yedeklemeler tetiklemeden önce fark atılmalıdır ya da günlük yedeklemelerine tam bir yedekleme günlüğü ve fark yedekleri üst. | Geçici bir tam yedekleme tetikleyin.   |

## <a name="usererrorbackupfailedastransactionlogisfull"></a>UserErrorBackupFailedAsTransactionLogIsFull

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veri kaynağı için işlem günlüğü dolu olduğundan yedek alınamıyor. | Veritabanı işlem günlüğü alanı dolu. | Bu sorunu gidermek için başvurmak [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/errors-events/mssqlserver-9002-database-engine-error). |

## <a name="usererrorcannotrestoreexistingdbwithoutforceoverwrite"></a>UserErrorCannotRestoreExistingDBWithoutForceOverwrite

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Aynı ada sahip bir veritabanı zaten hedef konumda var | Hedef geri yükleme, aynı ada sahip bir veritabanı zaten var.  | <ul><li>Hedef veritabanı adını değiştirin</li><li>Veya geri yükleme sayfasında zorla üzerine yaz seçeneğini kullanın</li> |

## <a name="usererrorrestorefaileddatabasecannotbeofflined"></a>UserErrorRestoreFailedDatabaseCannotBeOfflined

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Veritabanı çevrimdışına alınamadığından geri yükleme başarısız oldu. | Bir geri yükleme yaparken, hedef veritabanının çevrimdışı duruma getirilmesi gerekir. Azure Backup, bu veriyi çevrimdışı duruma getirmek mümkün değil. | Ek ayrıntılar Azure portal hata menüde nedenlerini daraltmak için kullanın. Daha fazla bilgi için [SQL belgeleri](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms). |

##  <a name="usererrorcannotfindservercertificatewiththumbprint"></a>UserErrorCannotFindServerCertificateWithThumbprint

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Hedefte parmak izli sunucu sertifikası bulunamıyor. | Ana hedef örneğinde veritabanını, geçerli şifreleme parmak izi yok. | Kaynak örneğinde hedef örneği için kullanılan geçerli sertifika parmak izini alın. |

## <a name="usererrorrestorenotpossiblebecauselogbackupcontainsbulkloggedchanges"></a>UserErrorRestoreNotPossibleBecauseLogBackupContainsBulkLoggedChanges

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kurtarma için kullanılan günlük yedeklemesi toplu olarak günlüğe kaydedilen değişiklikler içeriyor. SQL yönergelerine göre zamanın rastgele bir noktasında durdurmak için kullanılamaz. | Bir veritabanında toplu günlüğe kaydedilen kurtarma modunda olduğunda, bir toplu işlem ve sonraki günlük işlem arasındaki verileri kurtarılamaz. | Farklı bir noktaya kurtarma için seçin. [Daha fazla bilgi edinin](https://docs.microsoft.com/previous-versions/sql/sql-server-2008-r2/ms186229(v=sql.105))


## <a name="fabricsvcbackuppreferencecheckfailedusererror"></a>FabricSvcBackupPreferenceCheckFailedUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Kullanılabilirlik Grubunun bazı düğümleri kaydedilmediğinden SQL AlwaysOn Kullanılabilirlik Grubu yedekleme tercihi uygulanamadı. | Yedekleme gerçekleştirmek için gereken düğümleri kayıtlı değil veya ulaşılamıyor. | <ul><li>Bu veritabanının yedekleme gerçekleştirmek için gereken tüm düğümlerin kaydedildiğinden ve sağlıklı durumda ve sonra işlemi yeniden deneyin emin olun.</li><li>Değişiklik SQL Always On kullanılabilirlik grubu yedekleme tercihi.</li></ul> |

## <a name="vmnotinrunningstateusererror"></a>VMNotInRunningStateUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| SQL server sanal makinesi kapatılmış olduğundan ve Azure yedekleme hizmetine erişilemiyor. | VM'yi kapatın | SQL server'ın çalıştığından emin olun. |

## <a name="guestagentstatusunavailableusererror"></a>GuestAgentStatusUnavailableUserError

| Hata iletisi | Olası nedenler | Önerilen eylem |
|---|---|---|
| Azure Backup hizmeti yedekleme işlemi için Azure VM Konuk aracısını kullanır, ancak Konuk aracı hedef sunucuda kullanılabilir değil. | Konuk Aracısı etkin değil veya sağlıksız | [VM Konuk Aracısı yükleme](../virtual-machines/extensions/agent-windows.md) el ile. |

## <a name="autoprotectioncancelledornotvalid"></a>AutoProtectionCancelledOrNotValid

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

## <a name="files-size-limit-beyond-which-restore-happens-to-default-path"></a>Dosyanın boyut sınırını aşan hangi geri yükleme için varsayılan yolu olur

Dosyaların toplam dize boyutu yalnızca dosyaların sayısı, aynı zamanda bunların adlarını ve yollarını bağlıdır. Her veritabanı dosyalarının fiziksel yolunu ve mantıksal dosya adını alın.
Aşağıda verilen SQL sorgusu kullanabilirsiniz:

  ```
  SELECT mf.name AS LogicalName, Physical_Name AS Location FROM sys.master_files mf
                 INNER JOIN sys.databases db ON db.database_id = mf.database_id
                 WHERE db.name = N'<Database Name>'"
 ```

Artık bunları aşağıda belirtilen biçimde düzenleyin:

  ```[{"path":"<Location>","logicalName":"<LogicalName>","isDir":false},{"path":"<Location>","logicalName":"<LogicalName>","isDir":false}]}
  ```

Örnek:

  ```[{"path":"F:\\Data\\TestDB12.mdf","logicalName":"TestDB12","isDir":false},{"path":"F:\\Log\\TestDB12_log.ldf","logicalName":"TestDB12_log","isDir":false}]}
  ```

Yukarıda verilen içeriğin dize boyutunu 20.000 baytı aşarsa, ardından veritabanı dosyalarını farklı şekilde depolanır ve Kurtarma sırasında geri yükleme için hedef dosya yolunu ayarlamak mümkün olmayacaktır. SQL Server tarafından sağlanan varsayılan SQL yoluna dosyaları geri yüklenir.

### <a name="override-the-default-target-restore-file-path"></a>Varsayılan hedef geri yükleme dosya yolu geçersiz kıl

Hedef geri yükleme dosya yolunu eşleme veritabanı dosyası geri yükleme yolu hedef içeren bir JSON dosyası yerleştirerek geri yükleme işlemi sırasında geçersiz kılabilirsiniz. Bunun için bir dosya oluşturun `database_name.json` ve konuma yerleştirmek *C:\Program Files\Azure iş yükü Backup\bin\plugins\SQL*.

Dosyasının içeriği aşağıda belirtilen biçimde olmalıdır:
  ```[
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

Örnek:

  ```[
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

 
Yukarıdaki içeriği mantıksal aşağıda verilen SQL sorgusunu kullanarak veritabanı dosyası adını alabilirsiniz:

```
SELECT mf.name AS LogicalName FROM sys.master_files mf
                INNER JOIN sys.databases db ON db.database_id = mf.database_id
                WHERE db.name = N'<Database Name>'"
  ```


Bu dosya geri yükleme işlemini tetiklemeden önce yerleştirilmelidir.

## <a name="next-steps"></a>Sonraki adımlar

SQL Server Vm'leri (genel Önizleme) için Azure yedekleme hakkında daha fazla bilgi için bkz. [SQL Vm'leri (genel Önizleme) için Azure Backup](../virtual-machines/windows/sql/virtual-machines-windows-sql-backup-recovery.md#azbackup).
