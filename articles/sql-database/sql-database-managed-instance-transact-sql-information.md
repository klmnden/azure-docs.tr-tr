---
title: Azure SQL veritabanı yönetilen örnek T-SQL farklılıkları | Microsoft Docs
description: Bu makalede SQL Server arasındaki bir Azure SQL veritabanı yönetilen örneğinde T-SQL farklılıkları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, bonova
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: f1adcca48882ca3a149046cbc0729612666363cc
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734615"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>Yönetilen örnek T-SQL farklılıkları, SQL Server'dan Azure SQL veritabanı

Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server veritabanı altyapısı ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özellikleri çoğunu bir yönetilen örneğinde desteklenmiyor.

![Geçiş](./media/sql-database-managed-instance/migration.png)

Yine de bazı farklılıkları söz dizimi ve davranışı olduğundan, bu makalede özetler ve bu farklar açıklanmaktadır. <a name="Differences"></a>
- [Kullanılabilirlik](#availability) farklılıkları dahil olmak üzere [her zaman açık](#always-on-availability) ve [yedeklemeleri](#backup),
- [Güvenlik](#security) farklılıkları dahil olmak üzere [denetim](#auditing), [sertifikaları](#certificates), [kimlik bilgilerini](#credentials), [şifreleme sağlayıcıları](#cryptographic-providers), [Oturumları / kullanıcılar](#logins--users), [hizmet anahtarı ve hizmet ana anahtarını](#service-key-and-service-master-key),
- [Yapılandırma](#configuration) farklılıkları dahil olmak üzere [arabellek havuzu uzantısı](#buffer-pool-extension), [harmanlama](#collation), [Uyumluluk Düzeyleri](#compatibility-levels),[veritabanı Yansıtma](#database-mirroring), [veritabanı seçenekleri](#database-options), [SQL Server Agent](#sql-server-agent), [Tablo Seçenekleri](#tables),
- [İşlevler](#functionalities) dahil olmak üzere [toplu ekleme/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [dağıtılmış işlemler](#distributed-transactions), [ Genişletilmiş olaylar](#extended-events), [dış kitaplıkları](#external-libraries), [Filestream ve Filetable](#filestream-and-filetable), [anlam tam metin araması](#full-text-semantic-search), [bağlı sunucuları](#linked-servers), [Polybase](#polybase), [çoğaltma](#replication), [geri](#restore-statement), [hizmet Aracısı](#service-broker), [ Saklı yordamlar, İşlevler ve tetikleyiciler](#stored-procedures-functions-triggers),
- [Farklı bir davranış, yönetilen örneğiniz özellikleri](#Changes)
- [Geçici sınırlamalar ve bilinen sorunlar](#Issues)

## <a name="availability"></a>Kullanılabilirlik

### <a name="always-on-availability"></a>Her zaman açık

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örneğe oluşturulmuştur ve kullanıcılar tarafından kontrol edilemez. Aşağıdaki ifadeleri desteklenmez:

- [UÇ NOKTA OLUŞTUR... DATABASE_MIRRORING İÇİN](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [KULLANILABİLİRLİK GRUBU OLUŞTURUN](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [BIRAKMA KULLANILABİLİRLİK GRUBU](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) yan tümcesi [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) deyimi

### <a name="backup"></a>Backup

Otomatik yedeklemeler ve kullanıcıların tam bir veritabanı oluşturmak için yönetilen örneğe sahip `COPY_ONLY` yedekler. Fark, günlük ve dosya anlık görüntüsü yedekleri desteklenmez.

- Yönetilen örnek sayesinde, bir Azure Blob Depolama hesabına yalnızca bir örnek veritabanını yedekleyebilirsiniz:
  - Yalnızca `BACKUP TO URL` desteklenir
  - `FILE`, `TAPE`, ve yedekleme cihazları desteklenmez  
- Çoğu genel `WITH` seçenek desteklenmez
  - `COPY_ONLY` zorunludur
  - `FILE_SNAPSHOT` Desteklenmiyor
  - Bant Seçenekleri: `REWIND`, `NOREWIND`, `UNLOAD`, ve `NOUNLOAD` desteklenmez
  - Özel günlük seçenekleri: `NORECOVERY`, `STANDBY`, ve `NO_TRUNCATE` desteklenmez

Sınırlamalar:  

- Yönetilen örnek sayesinde, veritabanları için yeterli olan bir yedekleme 32 adede kadar diziler için bir örnek veritabanını yedekleyebilirsiniz yedekleme sıkıştırma kullanılırsa, en fazla 4 TB.
- En yüksek yedek stripe boyutu 195 GB (en yüksek blob boyutu) ' dir. Tek tek stripe boyutunu küçültmek ve bu sınırın içinde kalmanızı için yedekleme komutta şeritler sayısını artırın.

> [!TIP]
> Geçici çözüm bu sınırlama şirket içi yedekleme `DISK` yedekleme yerine `URL`, blob ve geri yüklemek için yedekleme dosyasını karşıya yükleyin. Farklı bir blob türü kullanıldığından daha büyük dosyaları geri yükleme destekler.  

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz: [yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL veritabanı ve SQL Server'da veritabanlarını veritabanlarında denetimi arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı yönetilen örnek dağıtım seçeneği ile works sunucu düzeyinde ve depoları denetim `.xel` günlük dosyalarını Azure blob depolama hesabı.
- Tek veritabanı ve elastik havuz dağıtım seçeneklerinde Azure SQL veritabanı'nda çalışır ve veritabanı düzeyinde denetim.
- Şirket içi SQL Server / sanal makineler, sunucuda denetim works düzey, ancak dosyaları sistem/windows olay günlüklerini olayları depolar.
  
XEvent yönetilen örneğinde denetimi, Azure blob depolama hedeflerini destekler. Dosya ve windows günlükleri desteklenmez.

Anahtarının farklar içinde `CREATE AUDIT` denetleme için Azure blob depolama söz dizimi olan:

- Yeni bir söz dizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısının URL'sini belirtmenize olanak tanıyan burada `.xel` dosyaları yerleştirilecek
- Söz dizimi `TO FILE` yönetilen örnek, Windows dosya paylaşımları erişemediği için desteklenmiyor.

Daha fazla bilgi için bkz.  

- [SUNUCU DENETİMİ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)  
- [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Sertifikalar

Aşağıdaki kısıtlamalar uygulamak için bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- `CREATE FROM`/`BACKUP TO` dosya için sertifikalar desteklenmiyor
- `CREATE`/`BACKUP` gelen sertifika `FILE` / `ASSEMBLY` desteklenmiyor. Özel anahtar dosyaları kullanılamaz.  

Bkz: [sertifika oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) ve [yedekleme sertifika](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql).  
  
**Geçici çözüm**: betik sertifika/özel anahtar, .sql dosyası olarak depolamak ve ikiliden oluşturun:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Kimlik Bilgisi

Azure Key Vault ve `SHARED ACCESS SIGNATURE` kimlikleri desteklenir. Windows kullanıcıları desteklenmez.

Bkz: [oluşturma kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) ve [ALTER kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Şifreleme sağlayıcıları oluşturulamıyor, yönetilen örnek dosyalara erişemezsiniz:

- `CREATE CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [Oluştur şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [ALTER şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins--users"></a>Oturum açma bilgileri / kullanıcılar

- Oluşturulan SQL oturum açmaları `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, ve `FROM SID` desteklenir. Bkz: [Oluştur oturum açma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- Azure Active Directory (AAD) oturum açmalar oluşturulurken [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sözdizimi veya [CREATE USER](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) söz dizimi desteklenir (**genel Önizleme**).
- Windows oturum açma bilgileri ile oluşturulan `CREATE LOGIN ... FROM WINDOWS` sözdizimi desteklenmiyor. Azure Active Directory oturum açma bilgileri ve kullanıcılar bu seçeneği kullanın.
- Örneği oluşturan azure Active Directory (Azure AD) kullanıcı [Kısıtlanmamış yönetici ayrıcalıkları](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Yönetici olmayan Azure Active Directory (Azure AD) veritabanı düzeyinde kullanıcılar kullanarak oluşturulabilir `CREATE USER ... FROM EXTERNAL PROVIDER` söz dizimi. Bkz: [kullanıcı oluştur... DIŞ SAĞLAYICISINDAN](sql-database-manage-logins.md#non-administrator-users)

### <a name="service-key-and-service-master-key"></a>Hizmet anahtarı ve hizmet ana anahtarı

- [Ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor
- [Ana anahtarı geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor
- [Hizmet ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor
- [Hizmet ana anahtarını geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor

## <a name="configuration"></a>Yapılandırma

### <a name="buffer-pool-extension"></a>Arabellek havuzu genişletme

- [Arabellek havuzu uzantısı](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmiyor.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` desteklenmiyor. Bkz: [ALTER SERVER Yapılandırması](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Harmanlama

Varsayılan örneği harmanlaması `SQL_Latin1_General_CP1_CI_AS` ve oluşturma parametre olarak belirtilebilir. Bkz: [harmanlamaları](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Uyumluluk düzeyleri

- Desteklenen uyumluluk düzeyleri şunlardır: 100, 110, 120, 130, 140  
- 100'den düşük bir uyumluluk düzeylerinde desteklenmez.
- Yeni veritabanları için varsayılan uyumluluk düzeyinin 140 değeri. Geri yüklenen veritabanları için uyumluluk düzeyi 100 ise ve yukarıda değişmeden kalır.

Bkz: [ALTER veritabanı uyumluluk düzeyi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma desteklenmiyor.

- `ALTER DATABASE SET PARTNER` ve `SET WITNESS` seçenekleri desteklenmez.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` desteklenmiyor.

Daha fazla bilgi için [ALTER DATABASE SET PARTNER ve SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve [uç nokta oluştur... DATABASE_MIRRORING için](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Veritabanı seçenekleri

- Birden çok günlük dosyası desteklenmez.
- Bellek içi nesneler genel amaçlı hizmet katmanında desteklenmiyor.  
- Veritabanı başına en fazla 280 dosyaları olduğunu belirtmek örnek başına 280 dosyaların bir sınırlama yoktur. Hem verileri hem de günlük dosyaları, bu sınırında sayılır.  
- Veritabanı dosya akışı verisi içeren dosya grupları içeremez.  Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.  
- Her dosya Azure Premium depolama alanına yerleştirilir. Azure Premium depolama diskleri gibi her bir dosya boyutuna g/ç ve dosya başına aktarım hızı aynı şekilde değişir. Bkz: [Azure Premium disk performansı](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)  

#### <a name="create-database-statement"></a>CREATE DATABASE deyimi

Aşağıdaki `CREATE DATABASE` sınırlamaları:

- Dosyalar ve dosya grupları tanımlanamaz.  
- `CONTAINMENT` seçeneği desteklenmez.  
- `WITH`seçenekleri desteklenmez.  
   > [!TIP]
   > Geçici çözüm olarak, `ALTER DATABASE` sonra `CREATE DATABASE` dosyaları ekleme ya da kapsama ayarlamak için veritabanı seçeneklerini ayarlamak için.  

- `FOR ATTACH` seçeneği desteklenmiyor
- `AS SNAPSHOT OF` seçeneği desteklenmiyor

Daha fazla bilgi için [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE deyimi

Bazı dosya özelliklerini ayarlamak veya değiştirilemez:

- Dosya yolu belirtilemez `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL deyimi. Kaldırma `FILENAME` komut dosyası için bir yönetilen örnek dosyalarını otomatik olarak yerleştirir.  
- Dosya adı değiştirilemiyor kullanarak `ALTER DATABASE` deyimi.

Aşağıdaki seçenekler, varsayılan olarak ayarlanır ve değiştirilemez:

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

Daha fazla bilgi için [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server Agent

- SQL Aracısı ayarları salt okunur. Yordam `sp_set_agent_properties` yönetilen durumlarda desteklenmez.  
- İşler
  - T-SQL iş adımları desteklenir.
  - Şu çoğaltma işleri desteklenir:
    - İşlem günlüğü okuyucu.  
    - Anlık görüntü.
    - Dağıtıcı
  - SSIS iş adımları desteklenir
  - İş adımları diğer türleri şu anda, dahil olmak üzere desteklenmez:
    - Birleştirme çoğaltması iş adımı desteklenmiyor.  
    - Sıra okuyucusu desteklenmiyor.  
    - Komut kabuğu henüz desteklenmiyor
  - Yönetilen örnek (örneğin, ağ paylaşımları üzerinden robocopy) dış kaynaklara erişemez.  
  - PowerShell henüz desteklenmiyor.
  - Analysis Services desteklenmiyor
- Bildirimleri kısmen desteklenir
- E-posta bildirimi desteklenir, bir veritabanı posta profili yapılandırma gerektirir. Yalnızca bir veritabanı posta profili olabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile` genel önizlemeye sunuldu (geçici sınırlama).  
  - Çağrı desteklenmiyor.  
  - NetSend desteklenmiyor.
  - Uyarılar değil henüz desteklenmemektedir.
  - Ara sunucular desteklenmez.  
- Eventlog desteklenmiyor.

Aşağıdaki özellikleri halen desteklenmemektedir ancak gelecek etkinleştirilecek:

- Proxy'ler
- Boşta CPU üzerinde işlerini zamanlama
- Aracıyı etkinleştirme/devre dışı bırakma
- Uyarılar

SQL Server Aracısı hakkında daha fazla bilgi için bkz. [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablolar

Aşağıdakiler desteklenmez:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED`  

Tablo oluşturma veya değiştirme hakkında daha fazla bilgi için bkz: [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) ve [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>İşlevler

### <a name="bulk-insert--openrowset"></a>Toplu ekleme / openrowset

Dosyaları Azure blob depolama alanından içeri aktarılmalıdır şekilde yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- `DATASOURCE` gereklidir `BULK INSERT` dosyaları Azure blob depolama alanından alınırken komutu. Bkz: [toplu ekleme](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` gereklidir `OPENROWSET` bir dosyanın içeriğini Azure blob depolama alanından okurken işlev. Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Aşağıdaki kısıtlamalar uygulamak için bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz: [ikili oluşturma DERLEMESİNDEN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` desteklenmiyor. Bkz: [Oluştur derleme DOSYASINDAN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` dosyaları başvuruda bulunamaz. Bkz: [ALTER derleme](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).


### <a name="dbcc"></a>DBCC

SQL Server'da etkin belgelenmemiş DBCC deyimleri yönetilen durumlarda desteklenmez.

- `Trace Flags` desteklenmez. Bkz: [izleme bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` desteklenmiyor. Bkz: [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` desteklenmiyor. Bkz: [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

Hiçbiri MSDTC ya da [elastik işlemler](sql-database-elastic-transactions-overview.md) yönetilen örnekleri şu anda desteklenmiyor.

### <a name="extended-events"></a>Genişletilmiş Olaylar

Bazı Windows özgü hedefler Xevent'ler için desteklenmez:

- `etw_classic_sync target` desteklenmiyor. Store `.xel` dosyalarını Azure blob depolama. Bkz: [etw_classic_sync hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etwclassicsynctarget-target).
- `event_file target`desteklenmiyor. Store `.xel` dosyalarını Azure blob depolama. Bkz: [event_file hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Dış kitaplıkları

R ve Python dış kitaplıkları henüz desteklenmemektedir veritabanında. Bkz: [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream ve Filetable

- FILESTREAM verileri desteklenmiyor.
- Veritabanı ile dosya grupları içeremez `FILESTREAM` veri
- `FILETABLE` desteklenmiyor
- Tabloları olamaz `FILESTREAM` türleri
- Aşağıdaki işlevleri desteklenmez:
  - `GetPathLocator()`
  - `GET_FILESTREAM_TRANSACTION_CONTEXT()`
  - `PathName()`
  - `GetFileNamespacePat)`
  - `FileTableRootPath()`

Daha fazla bilgi için [FILESTREAM](https://docs.microsoft.com/sql/relational-databases/blob/filestream-sql-server) ve [filetable nesnelerine](https://docs.microsoft.com/sql/relational-databases/blob/filetables-sql-server).

### <a name="full-text-semantic-search"></a>Tam metin anlamsal arama

[Anlamsal arama](https://docs.microsoft.com/sql/relational-databases/search/semantic-search-sql-server) desteklenmiyor.

### <a name="linked-servers"></a>Bağlı sunucular

Yönetilen örneğe bağlı sunucular hedefleri sınırlı sayıda destekler:

- Desteklenen hedefleri: SQL Server ve SQL veritabanı
- Hedefleri desteklenmez: dosya, Analysis Services ve diğer RDBMS.

İşlemler

- Çapraz örnek yazma işlemleri desteklenmez.
- `sp_dropserver` bağlantılı bir sunucu bırakma için desteklenir. Bkz: [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` işlevi, yalnızca SQL Server örneklerinde sorgularını yürütmek için kullanılabilir (ya da şirket içi, yönetilen veya sanal makineler). Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` işlevi, yalnızca SQL Server örneklerinde sorgularını yürütmek için kullanılabilir (ya da şirket içi, yönetilen veya sanal makineler). Yalnızca `SQLNCLI`, `SQLNCLI11`, ve `SQLOLEDB` değerler sağlayıcısı olarak desteklenir. Örneğin: `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee`. Bkz: [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="polybase"></a>Polybase

Dış tablolar HDFS veya Azure blob depolamadaki dosyalara başvuru desteklenmez. Polybase hakkında daha fazla bilgi için bkz. [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Çoğaltma

Çoğaltma yönetilen örnekleri için genel Önizleme aşamasındadır. Çoğaltma hakkında daha fazla bilgi için bkz. [SQL Server çoğaltma](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>GERİ bildirimi

- Desteklenen bir söz dizimi
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Desteklenmeyen söz dizimleri
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Kaynak  
  - `FROM URL` (Azure blob depolama) yalnızca desteklenen seçenektir.
  - `FROM DISK`/`TAPE`/ yedekleme aygıtı desteklenmiyor.
  - Yedekleme kümeleri desteklenmez.
- `WITH` seçenekleri desteklenmez (Hayır `DIFFERENTIAL`, `STATS`vb..)
- `ASYNC RESTORE` -İstemci bağlantısını keser bile geri yükleme devam eder. Bağlantınız kesilirse denetleyebilirsiniz `sys.dm_operation_status` görünümü için bir geri yükleme işlemi durumunu (yanı sıra oluşturma ve bırakma veritabanı için). Bkz: [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

Aşağıdaki veritabanı seçenekleri kümesi ve geçersiz ve daha sonra değiştirilemez:  

- `NEW_BROKER` (aracı .bak dosyasında etkin değilse)  
- `ENABLE_BROKER` (aracı .bak dosyasında etkin değilse)  
- `AUTO_CLOSE=OFF` (.bak dosyası içinde bir veritabanı varsa `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (.bak dosyası içinde bir veritabanı varsa `SIMPLE` veya `BULK_LOGGED` kurtarma modu)
- Bellek için iyileştirilmiş dosya grubu eklenir ve kaynak .bak dosyada başarısız olduysa, XTP çağırılır  
- Mevcut bellek için iyileştirilmiş dosya grubu için XTP yeniden adlandırıldı  
- `SINGLE_USER` ve `RESTRICTED_USER` seçenekleri dönüştürülür `MULTI_USER`

Sınırlamalar:  

- `.BAK` birden fazla yedekleme kümesi içeren dosyalar geri yüklenemiyor.
- `.BAK` birden çok günlük dosyalarını içeren dosyalar geri yüklenemiyor.
- Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.
- Etkin bellek içi nesneler, veritabanlarını içeren bir yedekleme şu anda geri yüklenemiyor.  
- Belirli bir noktada bellek içi nesneler burada varolan veritabanlarını içeren bir yedekleme şu anda geri yüklenemiyor.
- Salt okunur modda veritabanlarını içeren bir yedekleme şu anda geri yüklenemiyor. Bu sınırlama kısa süre içinde kaldırılacak.

Restore deyimleri hakkında daha fazla bilgi için bkz. [geri deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Hizmet Aracısı

Çapraz örnek hizmet Aracısı desteklenmez:

- `sys.routes` -Önkoşul: Adres sys.routes seçin. Adresi her rotaya yerel olmalıdır. Bkz: [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE` -kullanamazsınız `CREATE ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [Oluştur ROTA](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE` olamaz `ALTER ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [ALTER ROTA](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="stored-procedures-functions-triggers"></a>Saklı yordamlar, İşlevler, Tetikleyiciler

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
- `Extended stored procedures` dahil olmak üzere desteklenmediği `sp_addextendedproc`  ve `sp_dropextendedproc`. Bkz: [genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, ve `sp_detach_db` desteklenmez. Bkz: [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), ve [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).
- `sp_renamedb` desteklenmiyor. Bkz: [sp_renamedb](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-renamedb-transact-sql).

## <a name="Changes"></a> Davranış değişiklikleri

Aşağıdaki değişkenler, İşlevler ve görünümleri farklı sonuçlar döndürebilir:

- `SERVERPROPERTY('EngineEdition')` döndürür 8 değeri. Bu özellik, bir yönetilen örnek benzersiz olarak tanımlar. Bkz: [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` SQL Server bir yönetilen örneğe uygulamak için örnek olarak bu kavramı var olduğundan, NULL döndürür. Bkz: [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` döndürür tam DNS 'bağlanılabilirlik' adı, örneğin, yönetilen my instance.wcus17662feb9ce98.database.windows.net. Bkz: [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -DNS 'bağlanılabilirlik' adı gibi tam döndürür `myinstance.domain.database.windows.net` Özellikleri 'name' ve 'data_source'. Bkz: [SYS. SUNUCULARI](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` SQL Server bir yönetilen örneğe uygulamak için hizmet olarak kavramı var olduğundan, NULL döndürür. Bkz: [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` desteklenir. AAD oturum açma sys.syslogins içinde değilse NULL döndürür. Bkz: [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` desteklenmiyor. (Geçici bilinen sorun) veri yanlış döndürür. Bkz: [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` ve diğer yerleşik tarih/saat işlevleri her zaman UTC saat diliminde saati döndürür. Bkz: [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bilinen sorunlar ve sınırlamalar

### <a name="tempdb-size"></a>TEMPDB boyutu

`tempdb` 12 bölme dosyalarıyla her en büyük boyutu 14 GB dosya. Bu maksimum boyutu dosya başına değiştirilemez ve yeni dosyalar için eklenemez `tempdb`. Bu sınırlama kısa süre içinde kaldırılacak. Bazı sorgular 168 GB'den fazla ihtiyaçları varsa bir hata döndürebilir `tempdb`.

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla aşan depolama alanı

Her yönetilen örnek 35 TB depolama alanı Azure Premium Disk alanı için ayrılmış olan ve her veritabanını ayrı bir fiziksel diskte yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskte kullanılmayan alan değil ücretlendirilir, ancak Azure Premium Disk boyutları toplam 35 TB aşamaz. Bazı durumlarda, toplam 8 TB gerekmeyen bir yönetilen örnek 35 TB Azure sınırlama nedeniyle iç parçalanma depolama boyutu aşabilir.

Örneğin, bir yönetilen örnek bir olabilir bir 4 TB diskine yerleştirilen boyutu ve ayrı 128 GB diskler üzerinde yerleştirilen 248 dosyaları (her 1 GB boyutunda) 1,2 TB dosya. Bu örnekte:

- Toplam ayrılmış disk depolama boyutudur 4 x 1 TB + 248 x 128 GB = 35 TB.
- örneğindeki veritabanları için ayrılan toplam alan olan 1.2 x 1 TB + 248 x 1 GB = 1,4 TB.

Bunu, belirli bir dağıtım dosyalarının nedeniyle belirli durumda altında göstermektedir, yönetilen örnek için beklediğiniz değil bağlı Azure Premium Disk için rezerve 35 TB ulaşmak.

Bu örnekte, var olan veritabanlarını çalışmaya devam eder ve yeni dosyaları eklenmedi sürece herhangi bir sorun büyüyebilir. Ancak yeni veritabanlarını değil oluşturulabilir veya tüm veritabanlarının toplam boyutu örneği boyutu sınırına ulaştığında değil olsa bile yeni disk sürücüsü için yeterli alan olmadığından geri. Döndürülen hata durumda açık değildir.

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>SAS anahtarı hatalı yapılandırılması sırasında veritabanı geri yükleme

`RESTORE DATABASE` okuyan .bak dosyası sürekli yeniden deneme .bak dosyası ve döndürülen hata, uzun süre sonunda okumak için paylaşılan erişim imzasını `CREDENTIAL` yanlış. SAS anahtarının doğru olduğundan emin olmak için bir veritabanını geri yüklemeden önce geri HEADERONLY yürütün.
Önde gelen kaldırdığınızdan emin olun `?` SAS anahtarından Azure portalı kullanılarak oluşturulur.

### <a name="tooling"></a>Araç kullanımı

SQL Server Management Studio (SSMS) ve SQL Server veri Araçları (SSDT) yönetilen örnek erişirken bazı sorunlar olabilir.

- Azure AD oturum açma bilgileri ve kullanıcılar kullanarak (**genel Önizleme**) SSDT ile şu anda desteklenmiyor.
- Azure AD oturum açma bilgileri ve kullanıcılar için komut dosyası (**genel Önizleme**) SSMS'de desteklenmez.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Bazı görünümler, günlükleri ve iletileri yanlış veritabanı adları

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, Xevent'ler ve hata günlüğü girişleri gerçek veritabanı adları yerine GUID veritabanı tanımlayıcıları görüntüler. Bunlar gerçek veritabanı adları ile gelecekte değiştirilmesi çünkü bu GUID tanımlayıcılarını güvenmeyin.

### <a name="database-mail-profile"></a>Veritabanı posta profili

Yalnızca bir veritabanı posta profili olabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile`.

### <a name="error-logs-are-not-persisted"></a>Kalıcı olmayan hata günlükleri

Yönetilen örnek'te mevcut hata günlüklerini kaybolacağından ve bunların boyutu en fazla depolama sınırı bulunmaz. Hata günlüklerini otomatik olarak yük devretme durumunda silinmiş.

### <a name="error-logs-are-verbose"></a>Ayrıntılı hata günlükleri

Yönetilen örnek hata günlükleri ayrıntılı bilgiler yerleştirir ve çoğu için uygun değildir. Hata günlüklerinde bilgi miktarını gelecekte de düşürülmesini.

**Geçici çözüm**: Özel yordam, filtre genişletme ilgili olmayan bazı girişler Hata günlüklerini okumak için kullanın. Ayrıntılar için bkz [yönetilen örnek – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-is-not-supported"></a>İşlem kapsamı aynı örneği içinde iki veritabanlarında desteklenmiyor

`TransactionScope` iki veritabanı aynı örneğinde aynı işlem kapsamı altında iki sorgu gönderilir,. NET'te sınıfı çalışmaz:

```C#
using (var scope = new TransactionScope())
{
    using (var conn1 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn1.Open();
        SqlCommand cmd1 = conn1.CreateCommand();
        cmd1.CommandText = string.Format("insert into T1 values(1)");
        cmd1.ExecuteNonQuery();
    }

    using (var conn2 = new SqlConnection("Server=quickstartbmi.neu15011648751ff.database.windows.net;Database=b;User ID=myuser;Password=mypassword;Encrypt=true"))
    {
        conn2.Open();
        var cmd2 = conn2.CreateCommand();
        cmd2.CommandText = string.Format("insert into b.dbo.T2 values(2)");        cmd2.ExecuteNonQuery();
    }

    scope.Complete();
}

```

Bu kod içinde aynı örnek verilerle çalışır, ancak MSDTC gereklidir.

**Geçici çözüm**: Kullanma [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) diğer veritabanı bağlantı bağlamı iki bağlantı kullanmak yerine kullanılacak.

### <a name="clr-modules-and-linked-servers-sometime-cannot-reference-local-ip-address"></a>CLR modülleri ve süre bağlı sunucular, yerel IP adresi başvuramaz

Yönetilen örnek ve geçerli örnek süre başvuran bağlı sunucuları/dağıtılmış sorguları yerleştirilen CLR modülleri yerel örneğinin IP çözümlenemiyor. Bu hata geçici bir sorundur.

**Geçici çözüm**: Bağlam bağlantılarını CLR modülünde mümkün olduğunda kullanın.

### <a name="tde-encrypted-databases-dont-support-user-initiated-backups"></a>TDE şifreli veritabanlarına kullanıcı tarafından başlatılan yedeklemeleri desteklemez.

Kod yürütemez `BACKUP DATABASE ... WITH COPY_ONLY` bir veritabanında saydam veri şifrelemesi (TDE) ile şifrelenir. TDE yedeklemelerinin iç TDE anahtarlarla şifrelenmesini zorlar ve yedeklemeyi geri yüklemeniz mümkün olmayacak şekilde anahtarı verilemiyor.

**Geçici çözüm**: Otomatik yedeklemeler ve zaman içinde nokta geri yükleme kullanın veya veritabanı şifreleme devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekleri hakkında daha fazla ayrıntı için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren Hızlı Başlangıç için bkz: [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
