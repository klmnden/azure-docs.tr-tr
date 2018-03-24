---
title: Azure SQL veritabanı yönetilen örnek T-SQL farkları | Microsoft Docs
description: Bu makalede, Azure SQL veritabanı yönetilen örneğini ve SQL Server T-SQL farkları anlatılmaktadır.
services: sql-database
author: jovanpop-msft
ms.reviewer: carlrab, bonova
ms.service: sql-database
ms.custom: managed instance
ms.topic: article
ms.date: 03/19/2018
ms.author: jovanpop
manager: craigg
ms.openlocfilehash: b633c3c4a4f476cb8e89afde8adeb94558643d4b
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>SQL Server'dan Azure SQL Database yönetilen örnek T-SQL farkları 

Azure SQL veritabanı örneği (Önizleme) yönetilen şirket içi SQL Server veritabanı altyapısı yüksek uyum sağlar. SQL Server veritabanı altyapısı özelliklerin çoğunu yönetilen örneğinde desteklenir. Hala sözdizimi ve davranış bazı farklılıklar olduğundan, bu makalede özetler ve bu farkları açıklamaktadır.
 - [Desteklenmeyen özellikler ve T-SQL farkları](#Differences)
 - [Yönetilen örneğinde farklı davranışa sahip özellikleri](#Changes)
 - [Geçici sınırlamalar ve bilinen sorunlar](#Issues)

## <a name="Differences"></a> SQL Server T-SQL farkları 

Bu bölümde T-SQL söz dizimi ve davranış arasındaki farklar yönetilen örneği ve şirket içi SQL Server veritabanı altyapısı yanı sıra, desteklenmeyen özellikler özetlenmektedir.

### <a name="always-on-availability"></a>Always On kullanılabilirlik

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örneğine yerleşik olarak bulunur ve kullanıcılar tarafından denetlenemez. Aşağıdaki ifadeleri desteklenmez:
 - [UÇ NOKTA OLUŞTUR... DATABASE_MIRRORING İÇİN](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
 - [KULLANILABİLİRLİK GRUBU OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
 - [ALTER KULLANILABİLİRLİK GRUBU](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
 - [AÇILAN KULLANILABİLİRLİK GRUBU](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
 - [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) yan tümcesi [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) deyimi

### <a name="auditing"></a>Denetim 
 
Şirket içi yönetilen örneği, Azure SQL Database ve SQL Server SQL denetleme arasındaki temel farklılıklar şunlardır:
- Sunucu düzeyinde ve depoları yönetilen örneğinde SQL denetim çalışır `.xel` dosyaları Azure blob storage hesabı.  
- Azure SQL veritabanı'nda SQL denetim veritabanı düzeyinde çalışır.
- İçinde şirket içi SQL Server / sanal makine, SQL denetim sunucu düzeyinde çalışır, ancak dosyalar sistem/windows olay günlüklerini olayları depolar.  
  
Yönetilen örneğinde XEvent denetim Azure blob depolama hedefleri destekler. Dosya ve windows günlükleri desteklenmez.    
 
Anahtar farkları `CREATE AUDIT` sözdizimi denetimi için Azure blob depolama için:
- Yeni bir sözdizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısını URL'sini belirtmenize olanak tanıyan nerede `.xel` dosyaları yerleştirilecek 
- Sözdizimi `TO FILE` yönetilen örneği Windows dosya paylaşımlarında erişemediği için desteklenmiyor. 
 
Daha fazla bilgi için bkz.  
- [SUNUCU DENETİM OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql) 
- [Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)     

### <a name="backup"></a>Backup 

Yönetilen örneği otomatik yedeklemeler sahiptir ve kullanıcıların tam veritabanı oluşturmalarını sağlar `COPY_ONLY` yedekler. Fark, günlük ve dosya anlık görüntüsü yedekleri desteklenmez.  
- Yönetilen örneği yalnızca Azure Blob Storage hesabı için bir veritabanını yedekleyebilirsiniz: 
 - Yalnızca `BACKUP TO URL` desteklenir 
 - `FILE`, `TAPE`, ve yedekleme cihazları desteklenmez  
- Genel çoğu `WITH` seçenekleri desteklenir 
 - `COPY_ONLY` zorunludur
 - `FILE_SNAPSHOT` Desteklenmiyor  
 - Bant Seçenekleri: `REWIND`, `NOREWIND`, `UNLOAD`, ve `NOUNLOAD` desteklenmez 
 - Özel günlük seçenekleri: `NORECOVERY`, `STANDBY`, ve `NO_TRUNCATE` desteklenmez 
 
Sınırlamaları:  
- Yönetilen örneği başa bir veritabanını veritabanları için yeterli olan bir yedekleme en fazla 32 şeritler ile yedekleme sıkıştırma kullanılırsa, en fazla 4 TB.
- En fazla yedekleme stripe boyut 195 GB (en fazla blob boyutu)'dır. Tek tek stripe boyutunu küçültmek ve bu sınırı içinde kalmak için yedekleme komutta şeritler sayısını artırın. 

> [!TIP]
> Bu sınırlama şirket içi, yedekleme için geçici olarak çözmek için `DISK` yedekleme yerine `URL`, blob sonra geri yüklemek için yedekleme dosyasını karşıya yükleyin. Farklı blob türüne kullanıldığından geri yükleme büyük dosyaları destekler.  

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz: [yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

### <a name="buffer-pool-extension"></a>Arabellek havuzu genişletme 
 
- [Arabellek havuzu genişletme](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmiyor.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` desteklenmiyor. Bkz: [ALTER sunucu yapılandırması](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql). 
 
### <a name="bulk-insert--openrowset"></a>Toplu ekleme / openrowset

Dosyaların Azure blob depolama alanından içeri aktarılmalıdır şekilde yönetilen örnek dosya paylaşımları ve Windows klasörlerindeki erişemiyor.
- `DATASOURCE` gereklidir `BULK INSERT` Azure blob depolama alanından dosyaları alınırken komutu. Bkz: [toplu ekleme](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` gereklidir `OPENROWSET` Azure blob depolama alanından bir dosyanın içeriğini okurken işlev. Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
 
### <a name="certificates"></a>Sertifikalar 

Yönetilen örnek dosya paylaşımları ve Windows klasörlerindeki erişilemiyor. aşağıdaki kısıtlamalar uygulamak için: 
- `CREATE FROM`/`BACKUP TO` dosya için sertifikaları desteklenmiyor
- `CREATE`/`BACKUP` gelen sertifika `FILE` / `ASSEMBLY` desteklenmiyor. Özel anahtar dosyaları kullanılamaz.  
 
Bkz: [sertifika oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) ve [yedekleme sertifika](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
> [!TIP]
> Geçici çözüm: sertifika/özel anahtar komut dosyası, .sql dosyası olarak depolamak ve ikiliden oluşturun: 
> 
> ``` 
CREATE CERTIFICATE  
 FROM BINARY = asn_encoded_certificate    
WITH PRIVATE KEY ( <private_key_options> ) 
>```   
 
### <a name="clr"></a>CLR 

Yönetilen örnek dosya paylaşımları ve Windows klasörlerindeki erişilemiyor. aşağıdaki kısıtlamalar uygulamak için: 
- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz: [ikili oluşturma DERLEMESİNDEN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` desteklenmiyor. Bkz: [oluşturma derleme DOSYASINDAN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` dosyaları başvuramaz. Bkz: [ALTER ASSEMBLY](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).
 
### <a name="compatibility-levels"></a>Uyumluluk düzeyleri 
 
- Desteklenen uyumluluk düzeyleri şunlardır: 100, 110, 120, 130, 140  
- Uyumluluk Düzeyleri 100 aşağıda desteklenmez. 
- Yeni veritabanları için varsayılan uyumluluk düzeyi 140 ' dir. Geri yüklenen veritabanları için uyumluluk düzeyi 100 idiyse ve yukarıdaki değişmeden kalır.

Bkz: [ALTER veritabanı uyumluluk düzeyi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).
 
### <a name="credential"></a>Kimlik Bilgisi 
 
Yalnızca Azure anahtar kasası ve `SHARED ACCESS SIGNATURE` kimlikleri desteklenir. Windows kullanıcıları desteklenmez.
 
Bkz: [oluşturma kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) ve [ALTER kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql). 
 
### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Şifreleme sağlayıcıları oluşmasını yönetilen örnek dosyalara erişemiyor:
- `CREATE CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [oluşturma şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [ALTER şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql). 

### <a name="collation"></a>Harmanlama 
 
Sunucu harmanlama `SQL_Latin1_General_CP1_CI_AS` ve değiştirilemez. Bkz: [harmanlamaları](https://docs.microsoft.com/sql/t-sql/statements/collations).
 
### <a name="database-options"></a>Veritabanı seçenekleri 
 
- Birden çok günlük dosyalarını desteklenmez. 
- Bellek içi nesneler genel amaçlı hizmet katmanında desteklenmiyor.  
- Veritabanı başına en fazla 280 dosyaları olduğunu belirtmek örneği başına 280 dosyaların bir sınırı yoktur. Veri ve günlük dosyaları, bu sınırında sayılır.  
- Veritabanı filestream verileri içeren dosya grupları içeremez.  Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.  
- Her dosyayı Azure Premium depolama alanına yerleştirilir. Azure Premium Storage diskler için yaptığınız gibi g/ç ve dosya başına aynı şekilde her bir dosyayı boyutuna bağlıdır. Bkz: [Azure Premium disk performansı](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  
 
#### <a name="create-database-statement"></a>CREATE DATABASE deyimi

Aşağıdakiler `CREATE DATABASE` sınırlamaları: 
- Dosyaları ve dosya gruplarını tanımlanamaz.  
- `CONTAINMENT` Seçeneği desteklenmez.  
- `WITH`seçenekleri desteklenmez.  
   > [!TIP]
   > Geçici çözüm olarak kullanmak `ALTER DATABASE` sonra `CREATE DATABASE` veritabanı seçenekleri dosyaları ekleme ya da kapsama ayarlamak için ayarlanacak.  

- `FOR ATTACH` seçeneği desteklenmez 
- `AS SNAPSHOT OF` seçeneği desteklenmez 

Daha fazla bilgi için bkz: [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE deyimi

Bazı dosya özelliklerini ayarlamak veya değiştirilemez:
- Dosya yolu belirtilemez `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL ifadesi. Kaldırma `FILENAME` yönetilen örnek için otomatik olarak komut dosyaları yerleştirir.  
- Dosya adı kullanılarak değiştirilemez `ALTER DATABASE` deyimi.

Aşağıdaki seçenekler varsayılan olarak ayarlanır ve değiştirilemez: 
- `MULTI_USER` 
- `ENABLE_BROKER ON` 
- `AUTO_CLOSE OFF` 

Aşağıdaki seçenekler değiştirilemez: 
- `AUTO_CLOSE` 
- `AUTOMATIC_TUNING(CREATE_INDEX=ON|OFF)` 
- `AUTOMATIC_TUNING(DROP_INDEX=ON|OFF)` 
- `DISABLE_BROKER` 
- `EMERGENCY` 
- `ENABLE_BROKER` 
- `FILESTREAM` 
- `HADR`   
- `NEW_BROKER` 
- `OFFLINE` 
- `PAGE_VERIFY` 
- `PARTNER` 
- `READ_ONLY` 
- `RECOVERY BULK_LOGGED` 
- `RECOVERY_SIMPLE` 
- `REMOTE_DATA_ARCHIVE`  
- `RESTRICTED_USER` 
- `SINGLE_USER` 
- `WITNESS`

Değiştirme adı desteklenmiyor.

Daha fazla bilgi için bkz: [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma desteklenmiyor.
 - `ALTER DATABASE SET PARTNER` ve `SET WITNESS` seçenekleri desteklenmez.
 - `CREATE ENDPOINT … FOR DATABASE_MIRRORING` desteklenmiyor.

Daha fazla bilgi için bkz: [ALTER DATABASE SET PARTNER ve SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve [CREATE ENDPOINT... DATABASE_MIRRORING için](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="dbcc"></a>DBCC 
 
SQL Server'da etkin belgelenmemiş DBCC deyimleri yönetilen örneğinde desteklenmez.
- `Trace Flags` desteklenmez. Bkz: [izleme bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` desteklenmiyor. Bkz: [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` desteklenmiyor. Bkz: [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="extended-events"></a>Genişletilmiş Olaylar 

Bazı Windows özgü hedefler işlemleri, Xevent'ler için desteklenmez:
- `etw_classic_sync target` desteklenmiyor. Mağaza `.xel` dosyaları Azure blob depolama. Bkz: [etw_classic_sync hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target). 
- `event_file target`desteklenmiyor. Mağaza `.xel` dosyaları Azure blob depolama. Bkz: [event_file hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#eventfile-target).

### <a name="external-libraries"></a>Dış kitaplıkları

R ve Python dış kitaplıkları henüz desteklenmiyor veritabanı. Bkz: [Learning Services SQL Server makinesinde](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FILESTREAM ve Filetable

- FILESTREAM verileri desteklenmiyor. 
- Veritabanı ile dosya grupları içeremez `FILESTREAM` veri
- `FILETABLE` desteklenmiyor
- Tabloları olamaz `FILESTREAM` türleri
- Aşağıdaki işlevleri desteklenmez:
 - `GetPathLocator()` 
 - `GET_FILESTREAM_TRANSACTION_CONTEXT()` 
 - `PathName()` 
 - `GetFileNamespacePath()` 
 - `FileTableRootPath()` 

Daha fazla bilgi için bkz: [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) ve [FileTables](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Anlam tam metin araması

[Anlam arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) desteklenmiyor.

### <a name="linked-servers"></a>Bağlı sunucular
 
Bağlantılı sunucular yönetilen örneğinde hedefleri sınırlı sayıda destek: 
- Hedefleri desteklenen: SQL Server, SQL veritabanı, yönetilen örneği ve bir sanal makinede SQL Server.
- Hedefleri desteklenmez: dosya, Analysis Services ve diğer RDBMS.

İşlemler

- Çapraz örnek yazma işlemleri desteklenmez.
- `sp_dropserver` bağlantılı bir sunucu bırakma için desteklenir. Bkz: [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` işlevi, yalnızca SQL Server örnekleri üzerinde sorgularını yürütmek için kullanılabilir (ya da şirket içi, yönetilen ya da sanal makinelerde). Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` işlevi, yalnızca SQL Server örnekleri üzerinde sorgularını yürütmek için kullanılabilir (ya da şirket içi, yönetilen ya da sanal makinelerde). Yalnızca `SQLNCLI`, `SQLNCLI11`, ve `SQLOLEDB` değerler sağlayıcısı olarak desteklenir. Örneğin: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Bkz: [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).
 
### <a name="logins--users"></a>Oturum açma / kullanıcılar 

- Oluşturulan SQL oturum açma `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, ve `FROM SID` desteklenir. Bkz: [oluşturma oturum açma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Windows oturumu açma ile oluşturulan `CREATE LOGIN ... FROM WINDOWS` sözdizimi desteklenmiyor.
- Örneği oluşturan azure Active Directory (Azure AD) kullanıcı [Kısıtlanmamış yönetici ayrıcalıkları](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#unrestricted-administrative-accounts).
- Yönetici olmayan Azure Active Directory (Azure AD) veritabanı düzeyi kullanıcıların kullanılarak oluşturulabilir `CREATE USER ... FROM EXTERNAL PROVIDER` sözdizimi. Bkz: [kullanıcı oluşturma... DIŞ SAĞLAYICIDAN](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins#non-administrator-users)
 
### <a name="polybase"></a>Polybase

Dış tablolara başvuran HDFS veya Azure blob storage'da dosyaları desteklenmez. Polybase hakkında daha fazla bilgi için bkz: [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Çoğaltma 
 
Çoğaltmayı henüz desteklenmiyor. Çoğaltma hakkında daha fazla bilgi için bkz: [SQL Server çoğaltması](https://docs.microsoft.com/sql/relational-databases/replication/sql-server-replication).
 
### <a name="restore-statement"></a>Deyimi geri yükleme 
 
- Desteklenen sözdizimi  
   - `RESTORE DATABASE` 
   - `RESTORE FILELISTONLY ONLY` 
   - `RESTORE HEADER ONLY` 
   - `RESTORE LABELONLY ONLY` 
   - `RESTORE VERIFYONLY ONLY` 
- Desteklenmeyen söz dizimi 
   - `RESTORE LOG ONLY` 
   - `RESTORE REWINDONLY ONLY`
- Kaynak  
 - `FROM URL` (Azure blob depolama) yalnızca desteklenen bir seçenektir.
 - `FROM DISK`/`TAPE`/ yedekleme aygıtı desteklenmiyor.
 - Yedekleme kümesi desteklenmiyor. 
- `WITH` seçenekleri desteklenmez (No `DIFFERENTIAL`, `STATS`vb..)     
- `ASYNC RESTORE` -İstemci bağlantısını keser olsa bile geri yükleme devam eder. Bağlantınızı kesilirse kontrol edebilirsiniz `sys.dm_operation_status` görünüm için bir geri yükleme işlemi durumunu (yanı sıra oluştur ve açılan veritabanı için). See [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  
 
Aşağıdaki veritabanı seçenekleri kümesi ve geçersiz ve daha sonra değiştirilemez:  
- `NEW_BROKER` (Aracısı .bak dosyasında etkin değilse)  
- `ENABLE_BROKER` (Aracısı .bak dosyasında etkin değilse)  
- `AUTO_CLOSE=OFF` (.bak dosyası veritabanında varsa `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (.bak dosyası veritabanında varsa `SIMPLE` veya `BULK_LOGGED` kurtarma moduna)
- Bellek için iyileştirilmiş dosya eklenebilir ve kaynak .bak dosyasına olmadıysa XTP çağrılır  
- Tüm mevcut bellek için iyileştirilmiş dosya grubu için XTP yeniden adlandırıldı  
- `SINGLE_USER` ve `RESTRICTED_USER` seçenekleri dönüştürülür `MULTI_USER`   
Sınırlamaları:  
- `.BAK` birden çok yedekleme kümesi içeren dosyalar geri yüklenemiyor. 
- `.BAK` birden çok günlük dosyalarını içeren dosyalar geri yüklenemiyor. 
- Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.
- Bellekteki etkin nesneler sahip veritabanları içeren yedeklemeler şu anda geri yüklenemiyor.  
- Burada bellek içi nesneleri belirli bir noktada varolan veritabanlarını içeren yedeklemeler şu anda geri yüklenemiyor.   
- Salt okunur modda veritabanlarında içeren yedeklemeler şu anda geri yüklenemiyor. Bu sınırlama yakında kaldırılır.   
 
Geri yükleme deyimler hakkında daha fazla bilgi için bkz: [geri deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Hizmet Aracısı 
 
- Çapraz örnek hizmet Aracısı desteklenmiyor 
 - `sys.routes` -Önkoşul: Adres sys.routes seçin. Adresi her rotaya yerel olmalıdır. Bkz: [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
 - `CREATE ROUTE` -yapamazsınız `CREATE ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [oluşturma ROTA](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
 - `ALTER ROUTE` olamaz `ALTER ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [ALTER ROTA](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  
 
### <a name="service-key-and-service-master-key"></a>Hizmet anahtarı ve hizmet ana anahtarı 
 
- [Ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor 
- [Ana anahtarını geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor 
- [Hizmet ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor 
- [Hizmet ana anahtarını geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor 
 
### <a name="stored-procedures-functions-triggers"></a>Saklı yordamlar, İşlevler, Tetikleyicileri 
 
- `NATIVE_COMPILATION` şu anda desteklenmiyor. 
- Aşağıdaki [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez: 
 - `allow polybase export` 
 - `allow updates` 
 - `filestream_access_level` 
 - `max text repl size` 
 - `remote data archive` 
 - `remote proc trans` 
- `sp_execute_external_scripts` desteklenmiyor. Bkz: [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` desteklenmiyor. Bkz: [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` dahil olmak üzere desteklenmediği `sp_addextendedproc` ve `sp_dropextendedproc`. Bkz: [genişletilmiş saklı yordamları](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, ve `sp_detach_db` desteklenmez. Bkz: [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), ve [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` desteklenmiyor. Bkz: [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

### <a name="sql-server-agent"></a>SQL Server Agent 
 
- SQL Aracısı ayarları salt okunurdur. Yordam `sp_set_agent_properties` yönetilen örneğinde desteklenmez.  
- İşlerini - yalnızca T-SQL işi adımlardır şu anda desteklenen (daha fazla adım genel Önizleme sırasında eklenir).
 - SSIS henüz desteklenmiyor. 
 - Çoğaltmayı henüz desteklenmiyor  
  - İşlem günlüğü okuyucusu henüz desteklenmiyor.  
  - Anlık görüntü henüz desteklenmiyor.  
  - Dağıtıcı henüz desteklenmiyor.  
  - Birleştirme desteklenmiyor.  
  - Sıra okuyucusu desteklenmiyor.  
 - Komut kabuğu henüz desteklenmiyor. 
  - Yönetilen örneği (örneğin, ağ paylaşımları robocopy aracılığıyla) dış kaynaklara erişemez.  
 - PowerShell henüz desteklenmiyor.
 - Analysis Services desteklenmez.  
- Bildirimleri kısmen desteklenir.
 - E-posta bildirimi desteklenmektedir, bir veritabanı posta profili yapılandırma gerektirir. Yalnızca bir veritabanı posta profili olabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile` genel Önizleme (geçici sınırlama).  
 - Çağrı cihazı desteklenmiyor.  
 - Netsend gereksiz desteklenmiyor. 
 - Uyarıları değil henüz desteklenmiyor.
 - Ara sunucular desteklenmez.  
- Olay günlüğüne desteklenmiyor. 
 
Aşağıdaki özellikleri şu anda desteklenmiyor ancak gelecekte etkinleştirilir:  
- Proxy'ler
- Boşta CPU üzerinde işlerini zamanlama 
- Aracı etkinleştirme/devre dışı bırakma
- Uyarılar

SQL Server Agent hakkında daha fazla bilgi için bkz: [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).
 
### <a name="tables"></a>Tablolar 

Aşağıdakiler desteklenmez: 
- `FILESTREAM` 
- `FILETABLE` 
- `EXTERNAL TABLE` 
- `MEMORY_OPTIMIZED`  

Tablo oluşturma veya değiştirme hakkında daha fazla bilgi için bkz: [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) ve [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).
 
## <a name="Changes"></a> Davranış değişiklikleri 
 
Aşağıdaki değişkenler, İşlevler ve görünümler farklı sonuçlar döndürür:  
- `SERVERPROPERTY('EngineEdition')` döndürür 8 değeri. Bu özellik, yönetilen örneğini benzersiz şekilde tanımlar. Bkz: [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` Örneğin, 'myserver' kısa örnek adını döndürür. Bkz: [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` döndürür tam DNS 'bağlanılabilirlik' adı, örneğin, yönetilen my instance.wcus17662feb9ce98.database.windows.net. Bkz: [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -döndürür tam DNS 'bağlanılabilirlik' adı gibi `myinstance.domain.database.windows.net` Özellikleri 'name' ve 'data_source'. Bkz: [SYS. SUNUCULARI](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVERNAME` tam DNS 'bağlanılabilirlik' adı, gibi döndürür `my-managed-instance.wcus17662feb9ce98.database.windows.net`. Bkz: [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -döndürür tam DNS 'bağlanılabilirlik' adı gibi `myinstance.domain.database.windows.net` Özellikleri 'name' ve 'data_source'. Bkz: [SYS. SUNUCULARI](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql). 
- `@@SERVICENAME` hiçbir örnek yönetilen ortamında mantıklıdır NULL döndürür. Bkz: [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).   
- `SUSER_ID` desteklenir. AAD oturum açma sys.syslogins içinde değilse NULL döndürür. Bkz: [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` desteklenmiyor. (Geçici bilinen sorun) veri yanlış değerini döndürür. Bkz: [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 
- `GETDATE()` ve diğer yerleşik tarih/saat işlevleri her zaman saati UTC saat diliminde döndürür. Bkz: [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bilinen sorunlar ve sınırlamalar

### <a name="tempdb-size"></a>TEMPDB boyutu

`tempdb` 12 bölme dosyalarıyla her en büyük boyut 14 GB başına dosya. Bu maksimum boyutu dosya başına değiştirilemez ve yeni dosyalar eklenemez `tempdb`. Bu sınırlama yakında kaldırılır. Bazı sorgular 168 GB'den fazla gerekiyorsa bir hata döndürebilir `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Depolama alanı olan küçük veritabanı dosyalarını aşan

Her yönetilen örneğini 35 TB depolama için Azure Premium Disk alanı ayrılmış sahip ve her veritabanı dosyasını ayrı bir fiziksel diskte yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskte kullanılmayan alan ücret, ancak Azure Premium Disk boyutları toplamı 35 TB aşamaz. Bazı durumlarda, bir yönetilen 8 TB toplam gerekmeyen örneği TB Azure sınırlamak iç parçalanması nedeniyle depolama boyutu 35 aşabilir. 

Örneğin, yönetilen bir örnek 4 TB diski kullanan 1.2 TB boyuta sahip bir dosya ve her 128 GB boyutunu 248 disklerle yerleştirilir 248 dosyalarla 1 GB olabilir. Bu örnekte, toplam disk depolama boyutu 1 x 4 olan TB + 248 x 128 GB = 35 TB. Ancak, veritabanları için toplam ayrılmış örnek boyutu 1.2 x 1. TB + 248 x 1 GB = 1.4 TB. Dosyaları, belirli bir dağıtımını nedeniyle belirli koşullar altında yönetilen bir örneği burada beklediğiniz değil Azure Premium Disk Depolama sınırına ulaştığında, bu gösterilmektedir. 

Var olan veritabanlarını hata olacaktır ve yeni dosya eklenmez, ancak yeni veritabanları oluşturulan veya yeni bir disk sürücüsü için yeterli alan olmadığından tüm veritabanlarının toplam boyutu t ulaşmaz olsa bile geri herhangi bir sorun büyüyebilir He örneği boyut sınırını aştı. Döndürülen hata bu durumda açık değil.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>SAS anahtarı yanlış yapılandırma veritabanı sırasında geri yükleme

`RESTORE DATABASE` okuyan .bak dosyasının sürekli olarak yeniden deneniyor .bak dosyası ve döndürülen hata, uzun süre sonunda okumak için paylaşılan erişim imzasını `CREDENTIAL` yanlış. SAS anahtarı doğru olduğundan emin olmak için bir veritabanını geri yüklemeden önce geri yükleme HEADERONLY yürütün.
Önde gelen kaldırdığınızdan emin olun `?` Azure portal kullanılarak oluşturulan SAS anahtarı.

### <a name="tooling"></a>Araçları

SQL Server Management Studio'yu ve SQL Server veri araçları yönetilen örneği erişirken bazı sorunlar olabilir. Genel kullanılabilirlik önce tüm araçları sorunları ele alınacaktır.

### <a name="incorrect-database-names"></a>Yanlış veritabanı adları

Yönetilen örnek veritabanı adı yerine GUID değeri geri yükleme sırasında veya bazı hata iletileri gösterebilir. Bu sorunları önce genel kullanılabilirlik düzeltilecektir.

### <a name="database-mail-profile"></a>Veritabanı posta profili
Yalnızca bir veritabanı posta profili olabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile`. En kısa sürede kaldırılacak geçici bir kısıtlamadır.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örneği hakkında daha fazla ayrıntı için bkz: [yönetilen örneği nedir?](sql-database-managed-instance.md)
- Bir özellik için ve karşılaştırma listesi, bkz: [SQL ortak özellikleri](sql-database-features.md).
- Bir öğretici için bkz: [bir yönetilen örneği oluşturmayı](sql-database-managed-instance-tutorial-portal.md).
