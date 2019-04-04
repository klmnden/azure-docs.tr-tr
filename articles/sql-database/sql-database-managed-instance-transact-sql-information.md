---
title: Azure SQL veritabanı yönetilen örnek T-SQL farklılıkları | Microsoft Docs
description: Bu makalede bir Azure SQL veritabanı yönetilen örneği SQL Server arasındaki T-SQL farklılıkları açıklar.
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
ms.date: 03/13/2019
ms.openlocfilehash: b633c6a8ccbf9f29b93314bb9391215031d523eb
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2019
ms.locfileid: "58893070"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>SQL Server'dan Azure SQL veritabanı yönetilen örnek T-SQL farklılıkları

Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server veritabanı altyapısı ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özelliklerin çoğu, yönetilen örneği'nde desteklenir.

![Geçiş](./media/sql-database-managed-instance/migration.png)

Yine de bazı farklılıkları söz dizimi ve davranışı olduğundan, bu makalede özetler ve bu farklar açıklanmaktadır. <a name="Differences"></a>

- [Kullanılabilirlik](#availability) farklılıkları dahil olmak üzere [her zaman açık](#always-on-availability) ve [yedeklemeleri](#backup),
- [Güvenlik](#security) farklılıkları dahil olmak üzere [denetim](#auditing), [sertifikaları](#certificates), [kimlik bilgilerini](#credential), [şifreleme sağlayıcıları](#cryptographic-providers), [Oturumları / kullanıcılar](#logins--users), [hizmet anahtarı ve hizmet ana anahtarını](#service-key-and-service-master-key),
- [Yapılandırma](#configuration) farklılıkları dahil olmak üzere [arabellek havuzu uzantısı](#buffer-pool-extension), [harmanlama](#collation), [Uyumluluk Düzeyleri](#compatibility-levels),[veritabanı Yansıtma](#database-mirroring), [veritabanı seçenekleri](#database-options), [SQL Server Agent](#sql-server-agent), [Tablo Seçenekleri](#tables),
- [İşlevler](#functionalities) dahil olmak üzere [toplu ekleme/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [dağıtılmış işlemler](#distributed-transactions), [ Genişletilmiş olaylar](#extended-events), [dış kitaplıkları](#external-libraries), [Filestream ve Filetable](#filestream-and-filetable), [anlam tam metin araması](#full-text-semantic-search), [bağlı sunucuları](#linked-servers), [Polybase](#polybase), [çoğaltma](#replication), [geri](#restore-statement), [hizmet Aracısı](#service-broker), [ Saklı yordamlar, İşlevler ve tetikleyiciler](#stored-procedures-functions-triggers),
- [Yönetilen örnekleri'nde farklı davranışa sahip özellikleri](#Changes)
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

Yönetilen örnek otomatik yedeklemelerini ve tam bir veritabanı oluşturmak için kullanıcıları etkinleştirmek `COPY_ONLY` yedekler. Fark, günlük ve dosya anlık görüntüsü yedekleri desteklenmez.

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
- En fazla yedekleme stripe boyutunda `BACKUP` komuttur yönetilen örneğinde 195 GB (en yüksek blob boyutu). Tek tek stripe boyutunu küçültmek ve bu sınırın içinde kalmanızı için yedekleme komutta şeritler sayısını artırın.

    > [!TIP]
    > Bir veritabanını bir şirket içi ortamda veya bir sanal makine ya da SQL Server'dan yedeklerken bu sınırlara yakın çalışmak için bunu yapabilirsiniz:
    >
    > - Yedekleme `DISK` yerine yedekleme `URL`
    > - Yedekleme dosyaları Blob depolama alanına yükleme
    > - Yönetilen örneğine geri yükleme
    >
    > `Restore` Komutu yönetilen örneğe farklı blob türü karşıya yüklenen yedekleme dosyalarının depolanması için kullanıldığından bu büyük blob boyutları yedekleme dosyaları destekler.

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz: [yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL veritabanı ve SQL Server'da veritabanlarını veritabanlarında denetimi arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı yönetilen örneği'dağıtım seçeneği ile works sunucu düzeyinde ve depoları denetim `.xel` günlük dosyalarını Azure Blob Depolama alanında.
- Tek veritabanı ve elastik havuz dağıtım seçeneklerinde Azure SQL veritabanı'nda çalışır ve veritabanı düzeyinde denetim.
- Şirket içi SQL Server / sanal makineler, sunucuda denetim works düzey, ancak dosyaları sistem/windows olay günlüklerini olayları depolar.
  
XEvent denetim yönetilen örneği'nde, Azure Blob Depolama hedeflerini destekler. Dosya ve windows günlükleri desteklenmez.

Anahtarının farklar içinde `CREATE AUDIT` denetleme için Azure Blob Depolama söz dizimi olan:

- Yeni bir söz dizimi `TO URL` sağlanır ve Azure blob depolama kapsayıcısının URL'sini belirtmenize olanak tanıyan burada `.xel` dosyaları yerleştirilecek
- Söz dizimi `TO FILE` bir yönetilen örnek, Windows dosya paylaşımları erişemediği için desteklenmiyor.

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

Şifreleme sağlayıcıları oluşturulamıyor, bir yönetilen örnek dosyalara erişemezsiniz:

- `CREATE CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [Oluştur şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [ALTER şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins--users"></a>Oturum açma bilgileri / kullanıcılar

- Oluşturulan SQL oturum açmaları `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, ve `FROM SID` desteklenir. Bkz: [Oluştur oturum açma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- İle oluşturulan azure Active Directory (Azure AD) sunucusu sorumluları (oturum açma bilgileri) [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sözdizimi veya [oluşturma kullanıcı gelen oturum açma [Azure AD oturum açma]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) söz dizimi desteklenir (**genel önizlemeye sunuldu** ). Bu sunucu düzeyinde oluşturulan oturumlardır.

    Yönetilen örnek söz dizimi ile Azure AD veritabanı sorumlusu destekler `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Olarak da bilinen yer alan Azure AD veritabanı kullanıcıları budur.

- Windows oturum açma bilgileri ile oluşturulan `CREATE LOGIN ... FROM WINDOWS` sözdizimi desteklenmez. Azure Active Directory oturum açma bilgileri ve kullanıcılar bu seçeneği kullanın.
- Örneği oluşturan azure AD kullanıcı [Kısıtlanmamış yönetici ayrıcalıkları](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Yönetici olmayan Azure Active Directory (Azure AD) veritabanı düzeyinde kullanıcılar kullanarak oluşturulabilir `CREATE USER ... FROM EXTERNAL PROVIDER` söz dizimi. Bkz: [kullanıcı oluştur... DIŞ SAĞLAYICISINDAN](sql-database-manage-logins.md#non-administrator-users).
- Azure AD sunucusu ilkeleri (oturum açma bilgileri), yalnızca bir mı örneğinde SQL özellikleri destekler. İçinde aynı Azure AD Kiracı veya farklı bir kiracı için Azure AD kullanıcılarının desteklenmiyorsa bakılmaksızın örneği arası etkileşimi gerektiren özellikleri. Bu özellikler örnekleri şunlardır:

  - SQL işlem çoğaltması ve
  - Sunucuya Bağla

- Veritabanı sahibi desteklenmiyor gibi bir Azure AD grubuna eşlenmiş bir Azure AD oturum açma ayarlama.
- Diğer Azure AD sorumlusu kullanarak Azure AD sunucu düzeyi asıl hesaplar, kimliğe bürünme desteği gibi [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql) yan tümcesi. Sınırlama olarak YÜRÜTÜN:

  - Ad oturum açma adından farklı olduğu durumlarda, EXECUTE AS USER Azure AD kullanıcıları için desteklenmez. Örneğin, bir kullanıcı oluşturulduğunda kullanıcı oluşturma [myAadUser] gelen oturum açma söz dizimi aracılığıyla [john@contoso.com], ve kimliğe bürünme EXEC AS USER denenir = _myAadUser_. Oluştururken bir **kullanıcı** bir Azure AD sunucusu sorumlusundan (oturum açma), user_name aynı login_name olarak belirtin. **oturum açma**.
  - Yalnızca parçası olan SQL sunucu düzeyi ilkeleri (oturum açma bilgileri) `sysadmin` rol, Azure AD sorumlusu hedefleme şu işlemler yürütebilirsiniz:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- **Genel Önizleme** Azure AD sunucu sorumlusu (oturum açma bilgileri) sınırlamaları:

  - Yönetilen örnek Active Directory Yöneticisi sınırlamaları:

    - Yönetilen örnek ' için kullanılan Azure AD Yöneticisi, bir Azure AD sorumlusu (oturum açma) içinde sunucusu yönetilen örneği oluşturmak için kullanılamaz. İlk Azure AD sunucu sorumlusu (oturum açma) kullanarak bir SQL Server hesabı oluşturmalısınız bir `sysadmin`. Azure AD sunucu sorumlusu (oturum açma bilgileri) büyüyecek haline sonra kaldırılacak geçici bir sınırlama budur. Oturumu oluşturmak için bir Azure AD yönetici hesabı kullanmaya çalışırsanız aşağıdaki hatayı görürsünüz: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - İlk Azure AD oturum açma master DB'de oluşturulan standart SQL Server hesabı tarafından (olmayan Azure AD) şu anda oluşturulmalıdır bir `sysadmin` kullanarak [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) gelen dış sağlayıcı. POST GA bu sınırlama kaldırılır ve başlangıç olacak Azure AD oturum açma oluşturulan yönetilen örnek için Active Directory Yöneticisi tarafından.
    - SQL Server Management Studio (SSMS) veya SqlPackage kullanılan DacFx (içeri/dışarı aktarma), Azure AD oturum açma için desteklenmiyor. Azure AD sunucu sorumlusu (oturum açma bilgileri) büyüyecek haline sonra bu sınırlama kaldırılır
    - SSMS ile Azure AD sunucu sorumlusu (oturum açma bilgileri) kullanma

      - (Tüm kimliği doğrulanmış oturum açma kullanarak) Azure AD oturum açma komut dosyası desteklenmiyor.
      - IntelliSense tanımıyor **oluşturma oturum açma gelen dış sağlayıcı** ifadesi ve bir kırmızı alt çizgiyle gösterilir.

- Yalnızca sunucu düzeyinde sorumlu oturumu (yönetilen sağlama işlemi örneği tarafından oluşturulan), sunucu rollerinin üyeleri (`securityadmin` veya `sysadmin`), veya sunucu düzeyinde ALTER ANY LOGIN iznine sahip başka oturum açma bilgisi, Azure AD sunucusu oluşturabilirsiniz Yönetilen örnek için ana veritabanında ilkeleri (oturum açma bilgileri).
- Oturum açma SQL sorumlunun parçası olan oturumları olup olmadığını `sysadmin` rol için bir Azure AD hesabı oluştur oturum açma için create komutu kullanabilirsiniz.
- Azure AD oturum açma, Azure AD'yi Azure SQL yönetilen örneği için kullanılan aynı dizin içinde bir üyesi olmanız gerekir.
- Azure AD sunucusu ilkeleri (oturum açma bilgileri), SSMS 18.0 preview 5 başlamanızı nesne Gezgini'nde görünür.
- Bir Azure AD yönetici hesabıyla Azure AD sunucu sorumlusu (oturum açma bilgileri) çakışan izin verilir. Azure AD sunucusu ilkeleri (oturum açma bilgileri) Azure AD Yöneticisi sorumlusu ve uygulama izinlerini yönetilen örneği'ne çözülürken önceliklidir.
- Kimlik doğrulaması sırasında kimlik doğrulama sorumlusu çözümlenecek dizisi aşağıdaki uygulanır:

    1. Azure AD hesabı olarak doğrudan eşlenen varsa Azure AD sunucu sorumlusuna (oturum açma) ('E' türü olarak sys.server_principals mevcut) erişim ve izinleri Azure AD sunucu sorumlusunun (oturum açma) uygulayın.
    2. Azure AD hesabını Azure AD asıl sunucu (oturum açma) ('X' yazarken sys.server_principals içinde mevcut) eşlenmiş bir Azure AD grubunun bir üyesi ise, erişim ve izinleri Azure AD grubu oturum açma uygulayın.
    3. Azure AD hesabı portalı yapılandırılan özel ise yönetilen (yönetilen örnek sistem görünümlerinde yok) örneği için Azure AD Yöneticisi özel sabit Azure AD Yöneticisi izinleri yönetilen örneği (eski modu) için geçerlidir.
    4. Azure AD hesabı olarak bir Azure AD kullanıcı veritabanındaki (sys.database_principals türü 'E' olarak) doğrudan eşlenen varsa, erişim ve izinleri Azure AD veritabanı kullanıcısının uygulayın.
    5. Azure AD hesabı bir Azure AD kullanıcı veritabanındaki (sys.database_principals türü 'X') eşlenmiş bir Azure AD grubu üyesi ise, erişim ve izinleri Azure AD grubu oturum açma uygulayın.
    6. Bir Azure AD kullanıcı hesabı veya bir Azure AD grubu hesabıyla eşlenmiş bir Azure AD oturum açma işlemi varsa, tüm bu Azure AD oturum açma izinlerinden kullanıcı kimlik doğrulaması için çözümleme uygulanır.

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
- Veritabanı başına en fazla 280 dosyaları olduğunu belirtmek genel amaçlı örneği başına 280 dosyaların bir sınırlama yoktur. Hem verileri hem de günlük dosyaları genel amaçlı katmanı, bu sınırında sayılır. [Kritik iş katmanı, veritabanı başına 32.767 dosyalarını destekler](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- Veritabanı dosya akışı verisi içeren dosya grupları içeremez.  Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.  
- Her dosya, Azure Blob depolamada yer alır. G/ç ve dosya başına aktarım hızı, her bir dosya boyutuna bağlıdır.  

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

- Dosya yolu belirtilemez `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL deyimi. Kaldırma `FILENAME` bir yönetilen örnek için otomatik olarak komut dosyaları yerleştirir.  
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

Daha fazla bilgi için [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-file-and-filegroup-options).

### <a name="sql-server-agent"></a>SQL Server Agent

- SQL Aracısı ayarları salt okunur. Yordam `sp_set_agent_properties` yönetilen örneği'nde desteklenmiyor.  
- İşler
  - T-SQL iş adımları desteklenir.
  - Şu çoğaltma işleri desteklenir:
    - İşlem günlüğü okuyucusu
    - Anlık Görüntü
    - Dağıtıcı
  - SSIS iş adımları desteklenir
  - İş adımları diğer türleri şu anda desteklenmez:
    - Birleştirme çoğaltması iş adımı desteklenmez.  
    - Sıra okuyucusu desteklenmez.  
    - Komut kabuğu henüz desteklenmiyor.
  - Yönetilen örnek (örneğin, ağ paylaşımları üzerinden robocopy) dış kaynaklara erişemez.  
  - PowerShell henüz desteklenmiyor.
  - Analysis Services desteklenmez.
- Bildirimleri kısmen desteklenir.
- E-posta bildirimi desteklenir, bir veritabanı posta profili yapılandırma gerektirir. Yalnızca bir veritabanı posta profili olabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile` genel önizlemeye sunuldu (geçici sınırlama).  
  - Çağrı desteklenmiyor.  
  - NetSend desteklenmez.
  - Uyarılar henüz desteklenmemektedir.
  - Ara sunucular desteklenmez.  
- Eventlog desteklenmez.

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

Dosyaları Azure Blob depolama alanından içeri aktarılmalıdır şekilde bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- `DATASOURCE` gereklidir `BULK INSERT` dosyaları Azure Blob depolama alanından alınırken komutu. Bkz: [toplu ekleme](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` gereklidir `OPENROWSET` bir dosyanın içeriğini Azure Blob depolama alanından okurken işlev. Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Aşağıdaki kısıtlamalar uygulamak için bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz: [ikili oluşturma DERLEMESİNDEN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).  
- `CREATE ASSEMBLY FROM FILE` desteklenmiyor. Bkz: [Oluştur derleme DOSYASINDAN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` dosyaları başvuruda bulunamaz. Bkz: [ALTER derleme](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

SQL Server'da etkin belgelenmemiş DBCC deyimleri yönetilen örnekleri'nde desteklenmez.

- `Trace Flags` desteklenmez. Bkz: [izleme bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` desteklenmiyor. Bkz: [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` desteklenmiyor. Bkz: [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

Hiçbiri MSDTC ya da [elastik işlemler](sql-database-elastic-transactions-overview.md) şu anda yönetilen örnekleri'nde desteklenir.

### <a name="extended-events"></a>Genişletilmiş Olaylar

Bazı Windows özgü hedefler Xevent'ler için desteklenmez:

- `etw_classic_sync target` desteklenmiyor. Store `.xel` dosyalarını Azure blob depolama. Bkz: [etw_classic_sync hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- `event_file target` desteklenmiyor. Store `.xel` dosyalarını Azure blob depolama. Bkz: [event_file hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Dış kitaplıkları

R ve Python dış kitaplıkları henüz desteklenmemektedir veritabanında. Bkz: [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream ve Filetable

- FILESTREAM verileri desteklenmez.
- Veritabanı ile dosya grupları içeremez `FILESTREAM` veri.
- `FILETABLE` desteklenmiyor.
- Tabloları olamaz `FILESTREAM` türleri.
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

Dış tablolar HDFS veya Azure Blob depolamadaki dosyalara başvurma desteklenmez. Polybase hakkında daha fazla bilgi için bkz. [Polybase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Çoğaltma

Çoğaltma, yönetilen örneği için genel önizlemesi için kullanılabilir. Çoğaltma hakkında daha fazla bilgi için bkz. [SQL Server çoğaltma](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

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
  - `FROM URL` (Azure Blob Depolama) yalnızca desteklenen seçenektir.
  - `FROM DISK`/`TAPE`/ yedekleme aygıtı desteklenmiyor.
  - Yedekleme kümeleri desteklenmez.
- `WITH` seçenekleri desteklenmez (Hayır `DIFFERENTIAL`, `STATS`vb..)
- `ASYNC RESTORE` -İstemci bağlantısını keser bile geri yükleme devam eder. Bağlantınız kesilirse denetleyebilirsiniz `sys.dm_operation_status` görünümü için bir geri yükleme işlemi durumunu (yanı sıra oluşturma ve bırakma veritabanı için). Bkz: [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database).  

Aşağıdaki veritabanı seçenekleri kümesi ve geçersiz ve daha sonra değiştirilemez:  

- `NEW_BROKER` (aracı .bak dosyasında etkinleştirilmemişse)  
- `ENABLE_BROKER` (aracı .bak dosyasında etkinleştirilmemişse)  
- `AUTO_CLOSE=OFF` (.bak dosyası içinde bir veritabanı varsa `AUTO_CLOSE=ON`)  
- `RECOVERY FULL` (.bak dosyası içinde bir veritabanı varsa `SIMPLE` veya `BULK_LOGGED` kurtarma modu)
- Bellek için iyileştirilmiş dosya grubu eklenir ve kaynak .bak dosyada gerçekleştirmediyseniz XTP çağırılır  
- Mevcut bellek için iyileştirilmiş dosya grubu için XTP yeniden adlandırıldı  
- `SINGLE_USER` ve `RESTRICTED_USER` seçenekleri dönüştürülür `MULTI_USER`

Sınırlamalar:  

- `.BAK` birden fazla yedekleme kümesi içeren dosyalar geri yüklenemiyor.
- `.BAK` birden çok günlük dosyalarını içeren dosyalar geri yüklenemiyor.
- Geri yükleme başarısız olur .bak içeriyorsa `FILESTREAM` veri.
- Şu anda etkin bellek içi nesneleri olan veritabanlarını içeren bir yedekleme geri yüklenemez.  
- Burada belirli bir noktada bellek içi nesneler şu anda var olan veritabanlarını içeren yedekleri geri yüklenemez.
- Şu anda salt okunur modda veritabanlarını içeren yedekleri geri yüklenemez. Bu sınırlama kısa süre içinde kaldırılacak.

Restore deyimleri hakkında daha fazla bilgi için bkz. [geri deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Hizmet Aracısı

Çapraz örnek hizmet Aracısı desteklenmez:

- `sys.routes` -Önkoşul: Adres sys.routes seçin. Adresi her rotaya yerel olmalıdır. Bkz: [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE` -kullanamazsınız `CREATE ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [Oluştur ROTA](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE` olamaz `ALTER ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [ALTER ROTA](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql).  

### <a name="stored-procedures-functions-triggers"></a>Saklı yordamlar, İşlevler, Tetikleyiciler

- `NATIVE_COMPILATION` Genel amaçlı katmanında desteklenmiyor.
- Aşağıdaki [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez:
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` desteklenmiyor. Bkz: [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` desteklenmiyor. Bkz: [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` desteklenmez `sp_addextendedproc` ve `sp_dropextendedproc`. Bkz: [genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql)
- `sp_attach_db`, `sp_attach_single_file_db`, ve `sp_detach_db` desteklenmez. Bkz: [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), ve [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Changes"></a> Davranış değişiklikleri

Aşağıdaki değişkenler, İşlevler ve görünümleri farklı sonuçlar döndürebilir:

- `SERVERPROPERTY('EngineEdition')` döndürür 8 değeri. Bu özellik, yönetilen örneğe benzersiz olarak tanımlar. Bkz: [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` SQL Server yönetilen örneği'ne uygulanmaz için örnek olarak bu kavramı var olduğundan, NULL döndürür. Bkz: [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` döndürür tam DNS 'bağlanılabilirlik' adı, örneğin, yönetilen my instance.wcus17662feb9ce98.database.windows.net. Bkz: [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql).  
- `SYS.SERVERS` -DNS 'bağlanılabilirlik' adı gibi tam döndürür `myinstance.domain.database.windows.net` Özellikleri 'name' ve 'data_source'. Bkz: [SYS. SUNUCULARI](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` SQL Server yönetilen örneği'ne uygulanmaz için hizmet olarak kavramı var olduğundan, NULL döndürür. Bkz: [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` desteklenir. Azure AD oturum açma sys.syslogins içinde değilse NULL döndürür. Bkz: [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql).  
- `SUSER_SID` desteklenmiyor. (Geçici bilinen sorun) veri yanlış döndürür. Bkz: [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql).
- `GETDATE()` ve diğer yerleşik tarih/saat işlevleri her zaman UTC saat diliminde saati döndürür. Bkz: [GETDATE](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql).

## <a name="Issues"></a> Bilinen sorunlar ve sınırlamalar

### <a name="tempdb-size"></a>TEMPDB boyutu

En büyük dosya boyutu `tempdb` 24 GB/core genel amaçlı katmanı, ondan olamaz. En fazla `tempdb` boyutu iş açısından kritik katmanında Örnek Depolama boyutuyla sınırlıdır. `tempdb` 12 veri dosyalarına veri her zaman ayrılır. Bu maksimum boyutu dosya başına değiştirilemez ve yeni dosyaları eklenebilir `tempdb`. Bazı sorgular, bunlar birden fazla 24 GB'a ihtiyacınız varsa bir hata döndürebilir / içinde çekirdek `tempdb`.

### <a name="cannot-restore-contained-database"></a>Kapsanan veritabanı geri yüklenemiyor

Yönetilen örnek geri yükleyemiyor [içerdiği veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). Kapsanan veritabanlarında, zaman içinde nokta geri yönetilen örneği'nde çalışmaz. Bu sorunu en kısa sürede kaldırılacak ve sırada, yönetilen örneği'nde yerleştirilir ve üretim veritabanları için içerme seçeneği kullanmayın veritabanlarınızı içerme seçeneği kaldırmak için önerilir.

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla aşan depolama alanı

Yönetilen her bir genel amaçlı örneği 35 TB depolama alanı Azure Premium Disk alanı için ayrılmış olan ve her veritabanını ayrı bir fiziksel diskte yerleştirilir. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskte kullanılmayan alan değil ücretlendirilir, ancak Azure Premium Disk boyutları toplam 35 TB aşamaz. Bazı durumlarda, yönetilen örneğe 8 TB toplam gerekmeyen 35 TB Azure sınırlama nedeniyle iç parçalanma depolama boyutu aşabilir.

Örneğin, yönetilen bir genel amaçlı örneği bir olabilir bir 4 TB diskine yerleştirilen boyutu ve ayrı 128 GB diskler üzerinde yerleştirilen 248 dosyaları (her 1 GB boyutunda) 1,2 TB dosya. Bu örnekte:

- Toplam ayrılmış disk depolama boyutudur 4 x 1 TB + 248 x 128 GB = 35 TB.
- örneğindeki veritabanları için ayrılan toplam alan olan 1.2 x 1 TB + 248 x 1 GB = 1,4 TB.

Bunu, belirli bir dağıtım dosyalarının nedeniyle belirli durumda altında göstermektedir, yönetilen örneğe beklediğiniz değil bağlı Azure Premium Disk için rezerve 35 TB ulaşmak.

Bu örnekte, var olan veritabanlarını çalışmaya devam eder ve yeni dosyaları eklenmedi sürece herhangi bir sorun büyüyebilir. Ancak yeni veritabanlarını değil oluşturulabilir veya tüm veritabanlarının toplam boyutu örneği boyutu sınırına ulaştığında değil olsa bile yeni disk sürücüsü için yeterli alan olmadığından geri. Döndürülen hata durumda açık değildir.

Yapabilecekleriniz [kalan dosyaların sayısını belirleme](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) sistem görünümleri kullanma. Erişmeye çalıştığınız deneyin bu sınırı [boş ve DBCC SHRINKFILE deyimini kullanarak daha küçük dosyalar bazılarını silin](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) veya için shitch [yoksa iş açısından kritik katmanında, bu sınırı bulunur](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-sas-key-during-database-restore"></a>SAS anahtarı hatalı yapılandırılması sırasında veritabanı geri yükleme

`RESTORE DATABASE` okuyan .bak dosyası sürekli yeniden deneme .bak dosyası ve döndürülen hata, uzun süre sonunda okumak için paylaşılan erişim imzasını `CREDENTIAL` yanlış. SAS anahtarının doğru olduğundan emin olmak için bir veritabanını geri yüklemeden önce geri HEADERONLY yürütün.
Önde gelen kaldırdığınızdan emin olun `?` SAS anahtarından Azure portalı kullanılarak oluşturulur.

### <a name="tooling"></a>Araç kullanımı

SQL Server Management Studio (SSMS) ve SQL Server veri Araçları (SSDT), yönetilen örneğe erişirken bazı sorunlar olabilir.

- Azure AD sunucu sorumlusu (oturum açma bilgileri) ve kullanıcıların kullanarak (**genel Önizleme**) SSDT ile şu anda desteklenmiyor.
- Azure AD sunucu sorumlusu (oturum açma bilgileri) ve kullanıcılar için komut dosyası (**genel Önizleme**) SSMS'de desteklenmez.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Bazı görünümler, günlükleri ve iletileri yanlış veritabanı adları

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, Xevent'ler ve hata günlüğü girişleri gerçek veritabanı adları yerine GUID veritabanı tanımlayıcıları görüntüler. Bunlar gerçek veritabanı adları ile gelecekte değiştirilmesi çünkü bu GUID tanımlayıcılarını güvenmeyin.

### <a name="database-mail-profile"></a>Veritabanı posta profili

SQL aracısı tarafından kullanılan veritabanı posta profili çağrılmalıdır `AzureManagedInstance_dbmail_profile`.

### <a name="error-logs-are-not-persisted"></a>Kalıcı olmayan hata günlükleri

Yönetilen örneği'nde kullanılabilir olan hata günlüklerini kalıcı değildir ve bunların boyutu en fazla depolama sınırı bulunmaz. Hata günlüklerini otomatik olarak yük devretme durumunda silinmiş.

### <a name="error-logs-are-verbose"></a>Ayrıntılı hata günlükleri

Bir yönetilen örnek hata günlükleri ayrıntılı bilgiler yerleştirir ve çoğu için uygun değildir. Hata günlüklerinde bilgi miktarını gelecekte de düşürülmesini.

**Geçici çözüm**: Özel yordam, filtre genişletme ilgili olmayan bazı girişler Hata günlüklerini okumak için kullanın. Ayrıntılar için bkz [yönetilen örneği – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>İşlem kapsamı aynı örneği içinde iki veritabanlarında desteklenmiyor

`TransactionScope` iki veritabanı aynı örneğinde aynı işlem kapsamı altında iki sorgu gönderilir,. NET'te sınıfı çalışmıyor:

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

### <a name="clr-modules-and-linked-servers-sometime-cant-reference-local-ip-address"></a>CLR modülleri ve süre bağlı sunucular, yerel IP adresi başvuramaz

Yönetilen bir örneği ve geçerli örnek süre başvuran bağlı sunucuları/dağıtılmış sorguları yerleştirilen CLR modülleri yerel örneğinin IP çözümlenemiyor. Bu hata geçici bir sorundur.

**Geçici çözüm**: Bağlam bağlantılarını CLR modülünde mümkün olduğunda kullanın.

### <a name="tde-encrypted-databases-dont-support-user-initiated-backups"></a>TDE şifreli veritabanlarına kullanıcı tarafından başlatılan yedeklemeleri desteklemez.

Kod yürütemez `BACKUP DATABASE ... WITH COPY_ONLY` bir veritabanında saydam veri şifrelemesi (TDE) ile şifrelenir. TDE yedeklemelerinin iç TDE anahtarlarla şifrelenmesini zorlar ve yedeklemeyi geri yükleme olanağınız olmayacaktır anahtarı verilemiyor.

**Geçici çözüm**: Otomatik yedeklemeler ve zaman içinde nokta geri yükleme kullanın veya veritabanı şifreleme devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekler hakkında daha fazla ayrıntı için bkz: [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren Hızlı Başlangıç için bkz: [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
