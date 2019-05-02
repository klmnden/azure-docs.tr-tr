---
title: Azure SQL veritabanı yönetilen örnek T-SQL farklılıkları | Microsoft Docs
description: Bu makalede SQL Server arasındaki bir Azure SQL veritabanı yönetilen örneğinde T-SQL farklılıkları açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, bonova
manager: craigg
ms.date: 03/13/2019
ms.custom: seoapril2019
ms.openlocfilehash: 08920a25fc7213a773ef0d76a5daddbab3f765c2
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64866858"
---
# <a name="azure-sql-database-managed-instance-t-sql-differences-from-sql-server"></a>SQL Server'dan Azure SQL veritabanı yönetilen örnek T-SQL farklılıkları

Bu makalede özetler ve söz dizimi ve davranış Azure SQL veritabanı yönetilen örneği ve şirket içi SQL Server veritabanı altyapısı arasındaki farklar açıklanmaktadır. Aşağıdaki konular ele alınmıştır: <a name="Differences"></a>

- [Kullanılabilirlik](#availability) farklılıkları içerir [her zaman açık](#always-on-availability) ve [yedeklemeleri](#backup).
- [Güvenlik](#security) farklılıkları içerir [denetim](#auditing), [sertifikaları](#certificates), [kimlik bilgilerini](#credential), [şifreleme sağlayıcıları](#cryptographic-providers), [oturumlar ve kullanıcılar](#logins-and-users)ve [hizmet anahtarı ve hizmet ana anahtarını](#service-key-and-service-master-key).
- [Yapılandırma](#configuration) farklılıkları içerir [arabellek havuzu uzantısı](#buffer-pool-extension), [harmanlama](#collation), [Uyumluluk Düzeyleri](#compatibility-levels), [veritabanı yansıtma ](#database-mirroring), [veritabanı seçenekleri](#database-options), [SQL Server Agent](#sql-server-agent), ve [Tablo Seçenekleri](#tables).
- [İşlevler](#functionalities) içerir [toplu ekleme/OPENROWSET](#bulk-insert--openrowset), [CLR](#clr), [DBCC](#dbcc), [dağıtılmış işlemler](#distributed-transactions), [genişletilmiş olaylar](#extended-events), [dış kitaplıkları](#external-libraries), [filestream ve FileTable](#filestream-and-filetable), [anlam tam metin araması](#full-text-semantic-search), [bağlı sunucuları](#linked-servers), [PolyBase](#polybase), [çoğaltma](#replication), [geri](#restore-statement), [hizmet Aracısı](#service-broker), [saklı yordamlar, İşlevler ve tetikleyiciler](#stored-procedures-functions-and-triggers).
- [Farklı bir davranış olmayan özellikler yönetilen örnekleri](#Changes).
- [Geçici sınırlamalar ve bilinen sorunlar](#Issues).

Yönetilen örnek dağıtım seçeneği, şirket içi SQL Server veritabanı altyapısı ile yüksek uyumluluk sağlar. SQL Server veritabanı altyapısı özellikleri çoğunu bir yönetilen örneğinde desteklenmiyor.

![Geçiş](./media/sql-database-managed-instance/migration.png)

## <a name="availability"></a>Kullanılabilirlik

### <a name="always-on-availability"></a>Her zaman açık

[Yüksek kullanılabilirlik](sql-database-high-availability.md) yönetilen örneğe oluşturulmuştur ve kullanıcılar tarafından kontrol edilemez. Aşağıdaki ifadeleri desteklenmez:

- [UÇ NOKTA OLUŞTUR... DATABASE_MIRRORING İÇİN](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql)
- [KULLANILABİLİRLİK GRUBU OLUŞTURUN](https://docs.microsoft.com/sql/t-sql/statements/create-availability-group-transact-sql)
- [ALTER AVAILABILITY GROUP](https://docs.microsoft.com/sql/t-sql/statements/alter-availability-group-transact-sql)
- [BIRAKMA KULLANILABİLİRLİK GRUBU](https://docs.microsoft.com/sql/t-sql/statements/drop-availability-group-transact-sql)
- [SET HADR](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-set-hadr) yan tümcesi [ALTER DATABASE](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql) deyimi

### <a name="backup"></a>Backup

Yönetilen örneğiniz kullanıcılara tam bir veritabanı oluşturabilmesi için otomatik yedeklemeler, `COPY_ONLY` yedekler. Fark, günlük ve dosya anlık görüntüsü yedekleri desteklenmez.

- Yönetilen örnek sayesinde, bir Azure Blob Depolama hesabına yalnızca bir örnek veritabanını yedekleyebilirsiniz:
  - Yalnızca `BACKUP TO URL` desteklenir.
  - `FILE`, `TAPE`, ve yedekleme cihazlar desteklenmez.
- Çoğu genel `WITH` seçenek desteklenmez.
  - `COPY_ONLY` zorunludur.
  - `FILE_SNAPSHOT` desteklenmiyor.
  - Bant Seçenekleri: `REWIND`, `NOREWIND`, `UNLOAD`, ve `NOUNLOAD` desteklenmez.
  - Özel günlük seçenekleri: `NORECOVERY`, `STANDBY`, ve `NO_TRUNCATE` desteklenmez.

Sınırlamalar: 

- Yönetilen örnek sayesinde, veritabanları için yeterli olan bir yedekleme 32 adede kadar diziler için bir örnek veritabanını yedekleyebilirsiniz yedekleme sıkıştırma kullanılırsa, en fazla 4 TB.
- En fazla yedekleme stripe boyutu kullanarak `BACKUP` yönetilen örneğinde bir komuttur en yüksek blob boyutu 195 GB. Tek tek stripe boyutunu küçültmek ve bu sınırın içinde kalmanızı için yedekleme komutta şeritler sayısını artırın.

    > [!TIP]
    > Ya da SQL Server şirket içi ortam veya bir sanal makine bir veritabanını yedeklediğinizde, bu sınırlara yakın çalışmak için şunları yapabilirsiniz:
    >
    > - Yedekleme `DISK` yedekleme yerine `URL`.
    > - Yedekleme dosyaları Blob depolama alanına yükleyin.
    > - Yönetilen örneğine geri yükleyin.
    >
    > `Restore` Yönetilen örnek komutta farklı blob türü karşıya yüklenen yedekleme dosyalarının depolanması için kullanıldığından bu büyük blob boyutları yedekleme dosyaları destekler.

T-SQL kullanarak yedeklemeler hakkında daha fazla bilgi için bkz: [yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql).

## <a name="security"></a>Güvenlik

### <a name="auditing"></a>Denetim

Azure SQL veritabanı ve SQL Server'da veritabanlarını veritabanlarında denetimi arasındaki temel farklılıklar şunlardır:

- Azure SQL veritabanı yönetilen örneği'dağıtım seçeneği ile sunucu düzeyinde çalışır denetleme. `.xel` Günlük dosyaları, Azure Blob Depolama alanında depolanır.
- Tek veritabanı ve elastik havuz dağıtım seçeneklerinde Azure SQL veritabanı'nda çalışır ve veritabanı düzeyinde denetim.
- Sunucu düzeyinde çalışır, şirket içi SQL Server ya da sanal makineleri denetleme. Olaylar, dosya sisteminde veya Windows olay günlükleri üzerinde depolanır.
 
XEvent denetim yönetilen örneği'nde, Azure Blob Depolama hedeflerini destekler. Dosya ve Windows Günlükleri desteklenmez.

Anahtarının farklar içinde `CREATE AUDIT` Azure Blob depolama alanına denetim söz dizimi şunlardır:

- Yeni bir söz dizimi `TO URL` şartıyla, Azure Blob Depolama kapsayıcısının URL'sini belirtmek için kullanabileceğiniz olduğu yere `.xel` dosyalar yerleştirilir.
- Söz dizimi `TO FILE` yönetilen örnek, Windows dosya paylaşımları erişemediği için desteklenmiyor.

Daha fazla bilgi için bkz. 

- [SUNUCU DENETİMİ OLUŞTURMA](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql) 
- [ALTER SERVER DENETİM](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)
- [Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine)

### <a name="certificates"></a>Sertifikalar

Aşağıdaki kısıtlamalar uygulamak için bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- `CREATE FROM` / `BACKUP TO` Dosya için sertifikalar desteklenmiyor.
- `CREATE` / `BACKUP` Gelen sertifika `FILE` / `ASSEMBLY` desteklenmiyor. Özel anahtar dosyaları kullanılamaz. 

Bkz: [sertifika oluştur](https://docs.microsoft.com/sql/t-sql/statements/create-certificate-transact-sql) ve [yedekleme sertifika](https://docs.microsoft.com/sql/t-sql/statements/backup-certificate-transact-sql). 
 
**Geçici çözüm**: Özel anahtar ve sertifika için betik .sql dosyası olarak depolamak ve ikiliden oluşturun:

```sql
CREATE CERTIFICATE  
   FROM BINARY = asn_encoded_certificate
WITH PRIVATE KEY (<private_key_options>)
```

### <a name="credential"></a>Kimlik Bilgisi

Azure Key Vault ve `SHARED ACCESS SIGNATURE` kimlikleri desteklenir. Windows kullanıcıları desteklenmez.

Bkz: [oluşturma kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) ve [ALTER kimlik bilgisi](https://docs.microsoft.com/sql/t-sql/statements/alter-credential-transact-sql).

### <a name="cryptographic-providers"></a>Şifreleme sağlayıcıları

Şifreleme sağlayıcıları oluşturulamıyor, yönetilen örnek dosyaları erişilemiyor:

- `CREATE CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [Oluştur şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/create-cryptographic-provider-transact-sql).
- `ALTER CRYPTOGRAPHIC PROVIDER` desteklenmiyor. Bkz: [ALTER şifreleme SAĞLAYICISI](https://docs.microsoft.com/sql/t-sql/statements/alter-cryptographic-provider-transact-sql).

### <a name="logins-and-users"></a>Oturum açma bilgileri ve kullanıcılar

- Kullanılarak oluşturulan SQL oturum açmaları `FROM CERTIFICATE`, `FROM ASYMMETRIC KEY`, ve `FROM SID` desteklenir. Bkz: [Oluştur oturum açma](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql).
- İle oluşturulan azure Active Directory (Azure AD) sunucusu sorumluları (oturum açma bilgileri) [CREATE LOGIN](https://docs.microsoft.com/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) sözdizimi veya [oluşturma kullanıcı gelen oturum açma [Azure AD oturum açma]](https://docs.microsoft.com/sql/t-sql/statements/create-user-transact-sql?view=azuresqldb-mi-current) söz dizimi desteklenir (genel Önizleme). Bu oturum açma bilgileri, sunucu düzeyinde oluşturulur.

    Yönetilen örnek söz dizimi ile Azure AD veritabanı sorumlusu destekler `CREATE USER [AADUser/AAD group] FROM EXTERNAL PROVIDER`. Bu özellik yer alan Azure AD veritabanı kullanıcıları de denir.

- Windows oturum açma bilgileri ile oluşturulan `CREATE LOGIN ... FROM WINDOWS` sözdizimi desteklenmez. Azure Active Directory oturum açma bilgileri ve kullanıcılar bu seçeneği kullanın.
- Örneği oluşturan Azure AD kullanıcı [Kısıtlanmamış yönetici ayrıcalıkları](sql-database-manage-logins.md#unrestricted-administrative-accounts).
- Yönetici Azure AD olmayan veritabanı düzeyinde kullanıcılar kullanarak oluşturulabilir `CREATE USER ... FROM EXTERNAL PROVIDER` söz dizimi. Bkz: [kullanıcı oluştur... DIŞ SAĞLAYICISINDAN](sql-database-manage-logins.md#non-administrator-users).
- Azure AD sunucusu ilkeleri (oturum açma bilgileri) içinde yalnızca bir yönetilen örnek SQL özelliklerini destekler. Azure AD kullanıcıları için mi bunlar olduğunuz içinde aynı Azure AD kiracısı ya da farklı kiracıların ne olursa olsun arası örnek etkileşimi gerektiren özellikler desteklenmez. Bu özellikler örnekleri şunlardır:

  - SQL işlem çoğaltması.
  - Sunucuya Bağla.

- Veritabanı sahibi desteklenmiyor gibi bir Azure AD grubuna eşlenmiş bir Azure AD oturum açma ayarlama.
- Diğer Azure AD sorumlusu kullanarak Azure AD sunucu düzeyi asıl hesaplar, kimliğe bürünme desteği gibi [EXECUTE AS](/sql/t-sql/statements/execute-as-transact-sql) yan tümcesi. EXECUTE AS sınırlamalar vardır:

  - Adı oturum açma adından farklı olduğu durumlarda, EXECUTE AS USER Azure AD kullanıcıları için desteklenmez. Kullanıcı oluşturma kullanıcı [myAadUser] gelen oturum açma söz dizimi aracılığıyla oluşturulduğunda örneğidir [john@contoso.com] ve kimliğe bürünme EXEC AS USER denenir = _myAadUser_. Oluştururken bir **kullanıcı** bir Azure AD sunucusu sorumlusundan (oturum açma), user_name aynı login_name olarak belirtin. **oturum açma**.
  - Parçası olan yalnızca SQL sunucu düzeyi asıl hesaplar (oturum açma bilgileri) `sysadmin` rol, Azure AD sorumlusu hedefleyen aşağıdaki işlemler yürütebilirsiniz:

    - EXECUTE AS USER
    - EXECUTE AS LOGIN

- Azure AD sunucu sorumlusu (oturum açma bilgileri) genel Önizleme sınırlamaları:

  - Yönetilen örnek Active Directory yönetim sınırlamaları:

    - Yönetilen örneği için kullanılan Azure AD Yöneticisi, bir Azure AD sorumlusu (oturum açma) içinde sunucusu yönetilen örnek oluşturmak için kullanılamaz. İlk Azure SQL Server hesabı kullanarak asıl sunucu AD (oturum açma) oluşturmanız gerekir bu bir `sysadmin` rol. Azure AD sunucu sorumlusu (oturum açma bilgileri) genel olarak kullanılabilir hale geldikten sonra bu geçici bir sınırlama kaldırılır. Oturumu oluşturmak için bir Azure AD yönetici hesabı kullanmaya çalıştığınızda şu hatayı görürsünüz: `Msg 15247, Level 16, State 1, Line 1 User does not have permission to perform this action.`
      - Ana veritabanında oluşturulan ilk Azure AD oturum açma standart SQL Server hesabı (Azure dışı AD) tarafından oluşturulmalıdır şu anda, bu bir `sysadmin` kullanarak rol [CREATE LOGIN](/sql/t-sql/statements/create-login-transact-sql?view=azuresqldb-mi-current) gelen dış sağlayıcı. Genel kullanılabilirlik sonrasında, bu sınırlama kaldırılır. Bir ilk oluşturup yönetilen örnek için Active Directory Yöneticisi kullanarak, Azure AD oturum açma.
    - SQL Server Management Studio veya SqlPackage kullanılan DacFx (içeri/dışarı aktarma), Azure AD oturum açma için desteklenmiyor. Azure AD sunucu sorumlusu (oturum açma bilgileri) genel olarak kullanılabilir hale geldikten sonra bu sınırlama kaldırılır.
    - Azure AD sunucu sorumlusu (oturum açma bilgileri) ile SQL Server Management Studio kullanarak:

      - Kimliği doğrulanmış tüm oturum açma bilgilerini kullanacak Azure AD oturum açma komut dosyası desteklenmiyor.
      - IntelliSense, oturum açma dış sağlayıcı gelen oluşturma deyimi tanımıyor ve kırmızı alt çizgi gösterir.

- Sağlama işlemi, sunucu rollerinin üyeleri gibi yönetilen örneği'tarafından oluşturulan yalnızca sunucu düzeyinde asıl oturum açma, `securityadmin` veya `sysadmin`, veya sunucu düzeyinde ALTER ANY LOGIN iznine sahip başka oturum açma bilgisi, Azure AD'de oluşturabilirsiniz Yönetilen örnek için ana veritabanında sunucu ilkeleri (oturum açma bilgileri).
- Bir SQL sorumlusu oturum açma ise yalnızca oturum açma bilgileri parçası olan `sysadmin` rol için bir Azure AD hesabı oluştur oturum açma için create komutu kullanabilirsiniz.
- Azure AD oturum açma, Azure AD'yi Azure SQL veritabanı yönetilen örneği için kullanılan dizini içinde bir üyesi olmanız gerekir.
- Azure AD sunucusu ilkeleri (oturum açma bilgileri), SQL Server Management Studio 18.0 preview 5'ile başlayan nesne Gezgini'nde görünür.
- Bir Azure AD yönetici hesabıyla Azure AD sunucu sorumlusu (oturum açma bilgileri) çakışan izin verilir. Asıl çözmek ve izinler yönetilen örneğe uygulamak azure AD sunucusu ilkeleri (oturum açma bilgileri) Azure AD Yöneticisi önceliklidir.
- Kimlik doğrulaması sırasında kimlik doğrulama sorumlusu çözmek için aşağıdaki sırayla uygulanır:

    1. Azure AD hesabı olarak doğrudan varsa, "E" türü olarak sys.server_principals içinde mevcut olan Azure AD sunucu sorumlusuna (oturum açma), eşlenen erişim ve izinleri Azure AD sunucu sorumlusunun (oturum açma) uygulayın.
    2. Azure AD hesabı "X" yazarken sys.server_principals içinde mevcut olan Azure AD sunucu sorumlusuna (oturum açma) eşlenmiş bir Azure AD grubunun bir üyesi ise, erişim ve izinleri Azure AD grubu oturum açma uygulayın.
    3. Azure AD hesabını özel bir portal yapılandırılmış ise yönetilen örneği sistem görünümlerinde yok, yönetilen örnek için Azure AD Yöneticisi özel sabit Azure AD Yöneticisi izinleri yönetilen örneği (eski modu) için geçerlidir.
    4. Azure AD hesabı olarak bir Azure AD Kullanıcı türü "E" olarak sys.database_principals içinde mevcut olan bir veritabanında doğrudan eşlenen varsa Azure AD veritabanı kullanıcısının izinleri uygulamak ve erişim izni.
    5. Azure AD hesabı türü olarak "X" sys.database_principals içinde mevcut olan bir veritabanında bir Azure AD kullanıcı eşlenmiş bir Azure AD grubunun bir üyesi ise, erişim ve izinleri Azure AD grubu oturum açma uygulayın.
    6. Bir Azure AD kullanıcı hesabı veya kimlik doğrulaması kullanıcıya çözümler, bir Azure AD grubu hesabıyla eşlenmiş bir Azure AD oturum açma işlemi varsa, tüm bu Azure AD oturum açma izinlerinden uygulanır.

### <a name="service-key-and-service-master-key"></a>Hizmet anahtarı ve hizmet ana anahtarı

- [Ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor.
- [Ana anahtarı geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor.
- [Hizmet ana anahtarını yedekleme](https://docs.microsoft.com/sql/t-sql/statements/backup-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor.
- [Hizmet ana anahtarını geri yükleme](https://docs.microsoft.com/sql/t-sql/statements/restore-service-master-key-transact-sql) (SQL veritabanı hizmeti tarafından yönetilen) desteklenmiyor.

## <a name="configuration"></a>Yapılandırma

### <a name="buffer-pool-extension"></a>Arabellek havuzu genişletme

- [Arabellek havuzu uzantısı](https://docs.microsoft.com/sql/database-engine/configure-windows/buffer-pool-extension) desteklenmiyor.
- `ALTER SERVER CONFIGURATION SET BUFFER POOL EXTENSION` desteklenmiyor. Bkz: [ALTER SERVER Yapılandırması](https://docs.microsoft.com/sql/t-sql/statements/alter-server-configuration-transact-sql).

### <a name="collation"></a>Harmanlama

Varsayılan örneği harmanlaması `SQL_Latin1_General_CP1_CI_AS` ve oluşturma parametre olarak belirtilebilir. Bkz: [harmanlamaları](https://docs.microsoft.com/sql/t-sql/statements/collations).

### <a name="compatibility-levels"></a>Uyumluluk düzeyleri

- Desteklenen Uyumluluk Düzeyleri 100, 110, 120, 130 ve 140 ' dir.
- 100'den düşük bir uyumluluk düzeylerinde desteklenmez.
- Yeni veritabanları için varsayılan uyumluluk düzeyinin 140 değeri. Geri yüklenen veritabanları için salt okunur ise 100 ve üzeri uyumluluk düzeyini değişmeden kalır.

Bkz: [ALTER veritabanı uyumluluk düzeyi](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-compatibility-level).

### <a name="database-mirroring"></a>Veritabanı yansıtma

Veritabanı yansıtma desteklenmiyor.

- `ALTER DATABASE SET PARTNER` ve `SET WITNESS` seçenekleri desteklenmez.
- `CREATE ENDPOINT … FOR DATABASE_MIRRORING` desteklenmiyor.

Daha fazla bilgi için [ALTER DATABASE SET PARTNER ve SET WITNESS](https://docs.microsoft.com/sql/t-sql/statements/alter-database-transact-sql-database-mirroring) ve [uç nokta oluştur... DATABASE_MIRRORING için](https://docs.microsoft.com/sql/t-sql/statements/create-endpoint-transact-sql).

### <a name="database-options"></a>Veritabanı seçenekleri

- Birden çok günlük dosyası desteklenmez.
- Bellek içi nesneler genel amaçlı hizmet katmanında desteklenmiyor. 
- Veritabanı başına 280 dosyaları en fazla gelir genel amaçlı örneği başına 280 dosyaların bir sınırlama yoktur. Genel amaçlı katmanında, hem verileri hem de günlük dosyaları, bu sınırında sayılır. [İş açısından kritik katmanı ve veritabanı başına 32.767 dosyalarını destekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).
- Veritabanı dosya akışı verisi içeren dosya grupları içeremez. .Bak içeriyorsa başarısız geri `FILESTREAM` veri. 
- Her dosya, Azure Blob depolamada yer alır. G/ç ve dosya başına aktarım hızı, her bir dosya boyutuna bağlıdır.

#### <a name="create-database-statement"></a>CREATE DATABASE deyimi

İçin aşağıdaki sınırlamalar geçerlidir `CREATE DATABASE`:

- Dosyalar ve dosya grupları tanımlanamaz. 
- `CONTAINMENT` Seçeneği desteklenmez. 
- `WITH` seçenekleri desteklenmez. 
   > [!TIP]
   > Geçici çözüm olarak, `ALTER DATABASE` sonra `CREATE DATABASE` dosyaları ekleme ya da kapsama ayarlamak için veritabanı seçeneklerini ayarlamak için. 

- `FOR ATTACH` Seçeneği desteklenmez.
- `AS SNAPSHOT OF` Seçeneği desteklenmez.

Daha fazla bilgi için [CREATE DATABASE](https://docs.microsoft.com/sql/t-sql/statements/create-database-sql-server-transact-sql).

#### <a name="alter-database-statement"></a>ALTER DATABASE deyimi

Bazı dosya özelliklerini ayarlamak veya değiştirilemez:

- Bir dosya yolu belirtilemez `ALTER DATABASE ADD FILE (FILENAME='path')` T-SQL deyimi. Kaldırma `FILENAME` komut dosyası için bir yönetilen örnek dosyalarını otomatik olarak yerleştirir. 
- Kullanarak bir dosya adı değiştirilemez `ALTER DATABASE` deyimi.

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

- SQL Server Aracısı ayarları salt okunur. Yordamı `sp_set_agent_properties` yönetilen örneği'nde desteklenmiyor. 
- İşler
  - T-SQL iş adımları desteklenir.
  - Şu çoğaltma işleri desteklenir:
    - İşlem günlüğü okuyucusu
    - Anlık Görüntü
    - Dağıtıcı
  - SSIS iş adımları desteklenir.
  - İş adımları diğer türleri şu anda desteklenmemektedir:
    - Birleştirme çoğaltması iş adımı desteklenmez. 
    - Sıra okuyucusu desteklenmez. 
    - Komut kabuğu henüz desteklenmiyor.
  - Yönetilen örnek, dış kaynaklara erişemez, örneğin, ağ robocopy paylaşır. 
  - SQL Server Analysis Services desteklenmez.
- Bildirimleri kısmen desteklenir.
- Veritabanı posta profili yapılandırma gerektirse de, e-posta bildirimi desteklenmektedir. SQL Server Agent, yalnızca bir veritabanı posta profili kullanabilir ve çağrılması gerekir `AzureManagedInstance_dbmail_profile`. 
  - Çağrı desteklenmiyor. 
  - NetSend desteklenmez.
  - Uyarıları henüz desteklenmiyor.
  - Proxy'leri desteklenmez. 
- EventLog desteklenmez.

Aşağıdaki özellikler şu anda desteklenmez ancak gelecekte etkinleştirilecek:

- Proxy'ler
- Boş bir CPU üzerinde işlerini zamanlama
- Bir aracıyı devre dışı bırakma veya etkinleştirme
- Uyarılar

SQL Server Aracısı hakkında daha fazla bilgi için bkz. [SQL Server Agent](https://docs.microsoft.com/sql/ssms/agent/sql-server-agent).

### <a name="tables"></a>Tablolar

Aşağıdaki tablolarda desteklenmez:

- `FILESTREAM`
- `FILETABLE`
- `EXTERNAL TABLE`
- `MEMORY_OPTIMIZED` 

Oluşturma ve tabloları değiştirme hakkında daha fazla bilgi için bkz. [CREATE TABLE](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql) ve [ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql).

## <a name="functionalities"></a>İşlevler

### <a name="bulk-insert--openrowset"></a>Toplu ekleme / openrowset

Dosyaları Azure Blob depolama alanından içeri aktarılmalıdır şekilde yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- `DATASOURCE` gereklidir `BULK INSERT` dosyaları Azure Blob depolama alanından alırken komutu. Bkz: [toplu ekleme](https://docs.microsoft.com/sql/t-sql/statements/bulk-insert-transact-sql).
- `DATASOURCE` gereklidir `OPENROWSET` bir dosyanın içeriğini Azure Blob depolama alanından okurken işlev. Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).

### <a name="clr"></a>CLR

Aşağıdaki kısıtlamalar uygulamak için bir yönetilen örnek, dosya paylaşımları ve Windows klasörleri erişilemiyor:

- Yalnızca `CREATE ASSEMBLY FROM BINARY` desteklenir. Bkz: [ikili oluşturma DERLEMESİNDEN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql). 
- `CREATE ASSEMBLY FROM FILE` desteklenmiyor. Bkz: [Oluştur derleme DOSYASINDAN](https://docs.microsoft.com/sql/t-sql/statements/create-assembly-transact-sql).
- `ALTER ASSEMBLY` dosyaları başvuruda bulunamaz. Bkz: [ALTER derleme](https://docs.microsoft.com/sql/t-sql/statements/alter-assembly-transact-sql).

### <a name="dbcc"></a>DBCC

SQL Server'da etkin belgelenmemiş DBCC deyimleri içinde yönetilen örnekler desteklenmez.

- `Trace flags` desteklenmez. Bkz: [izleme bayrakları](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql).
- `DBCC TRACEOFF` desteklenmiyor. Bkz: [DBCC TRACEOFF](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceoff-transact-sql).
- `DBCC TRACEON` desteklenmiyor. Bkz: [DBCC TRACEON](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-traceon-transact-sql).

### <a name="distributed-transactions"></a>Dağıtılmış işlemler

MSDTC ve [elastik işlemler](sql-database-elastic-transactions-overview.md) yönetilen örnekleri şu anda desteklenmemektedir.

### <a name="extended-events"></a>Genişletilmiş Olaylar

Bazı Windows özgü hedefler için genişletilmiş olaylar (Xevent'ler) desteklenmez:

- `etw_classic_sync` Hedef desteklenmez. Store `.xel` Azure Blob depolamadaki dosyaları. Bkz: [etw_classic_sync hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#etw_classic_sync_target-target).
- `event_file` Hedef desteklenmez. Store `.xel` Azure Blob depolamadaki dosyaları. Bkz: [event_file hedef](https://docs.microsoft.com/sql/relational-databases/extended-events/targets-for-extended-events-in-sql-server#event_file-target).

### <a name="external-libraries"></a>Dış kitaplıkları

Veritabanında R ve Python, dış kitaplıkları olmayan henüz desteklenmiyor. Bkz: [SQL Server Machine Learning Hizmetleri](https://docs.microsoft.com/sql/advanced-analytics/r/sql-server-r-services).

### <a name="filestream-and-filetable"></a>FileStream ve FileTable

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

- Desteklenen hedef SQL Server ve SQL veritabanı ' dir.
- Desteklenmeyen dosyaları, Analysis Services ve diğer RDBMS hedeflerdir.

İşlemler

- Çapraz örnek yazma işlemleri desteklenmez.
- `sp_dropserver` bağlantılı bir sunucu bırakma için desteklenir. Bkz: [sp_dropserver](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-dropserver-transact-sql).
- `OPENROWSET` İşlevi, yalnızca SQL Server örneklerinde sorgularını yürütmek için kullanılabilir. Bunlar yönetilen, şirket içinde olabilir veya sanal makinelerde. Bkz: [OPENROWSET](https://docs.microsoft.com/sql/t-sql/functions/openrowset-transact-sql).
- `OPENDATASOURCE` İşlevi, yalnızca SQL Server örneklerinde sorgularını yürütmek için kullanılabilir. Bunlar yönetilen, şirket içinde olabilir veya sanal makinelerde. Yalnızca `SQLNCLI`, `SQLNCLI11`, ve `SQLOLEDB` değerler sağlayıcısı olarak desteklenir. `SELECT * FROM OPENDATASOURCE('SQLNCLI', '...').AdventureWorks2012.HumanResources.Employee` bunun bir örneğidir. Bkz: [OPENDATASOURCE](https://docs.microsoft.com/sql/t-sql/functions/opendatasource-transact-sql).

### <a name="polybase"></a>PolyBase

HDFS veya Azure Blob depolama alanındaki dosyalar, desteklenmeyen bu başvuruyu dış tablolar. PolyBase hakkında daha fazla bilgi için bkz. [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide).

### <a name="replication"></a>Çoğaltma

Çoğaltma, yönetilen örneği için genel önizlemesi için kullanılabilir. Çoğaltma hakkında daha fazla bilgi için bkz. [SQL Server çoğaltma](https://docs.microsoft.com/sql/relational-databases/replication/replication-with-sql-database-managed-instance).

### <a name="restore-statement"></a>GERİ bildirimi 

- Desteklenen sözdizimi:
  - `RESTORE DATABASE`
  - `RESTORE FILELISTONLY ONLY`
  - `RESTORE HEADER ONLY`
  - `RESTORE LABELONLY ONLY`
  - `RESTORE VERIFYONLY ONLY`
- Desteklenmeyen söz dizimleri:
  - `RESTORE LOG ONLY`
  - `RESTORE REWINDONLY ONLY`
- Kaynak: 
  - `FROM URL` (Azure Blob Depolama) yalnızca desteklenen seçenektir.
  - `FROM DISK`/`TAPE`/ yedekleme aygıtı desteklenmiyor.
  - Yedekleme kümeleri desteklenmez.
- `WITH` seçenekleri desteklenmez, Hayır gibi `DIFFERENTIAL` veya `STATS`.
- `ASYNC RESTORE`: İstemci bağlantısını keser bile geri yükleme devam eder. Bağlantınız kesilirse denetleyebilirsiniz `sys.dm_operation_status` görünümü oluşturma ve bırakma veritabanı ve geri yükleme işlemi durumunu için. Bkz: [sys.dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database). 

Aşağıdaki veritabanı seçeneklerini ayarlayın veya geçersiz kılındı ve daha sonra değiştirilemez: 

- `NEW_BROKER` Aracı .bak dosyasında etkinleştirilmediyse. 
- `ENABLE_BROKER` Aracı .bak dosyasında etkinleştirilmediyse. 
- `AUTO_CLOSE=OFF` bir veritabanında .bak dosyası varsa `AUTO_CLOSE=ON`. 
- `RECOVERY FULL` bir veritabanında .bak dosyası varsa `SIMPLE` veya `BULK_LOGGED` kurtarma modunda.
- Bellek için iyileştirilmiş bir dosya grubuna eklenir ve kaynak .bak dosyada gerçekleştirmediyseniz XTP çağrılır. 
- Var olan bir bellek için iyileştirilmiş dosya grubu için XTP adlandırılır. 
- `SINGLE_USER` ve `RESTRICTED_USER` seçenekleri dönüştürülür `MULTI_USER`.

Sınırlamalar: 

- `.BAK` birden fazla yedekleme kümesi içeren dosyalar geri yüklenemiyor. 
- `.BAK` birden çok günlük dosyalarını içeren dosyalar geri yüklenemiyor.
- .Bak içeriyorsa başarısız geri `FILESTREAM` veri.
- Yedeklemeler, etkin bir bellek içi nesneler veritabanlarını içeren bir genel amaçlı örneğinde geri yüklenemez. Restore deyimleri hakkında daha fazla bilgi için bkz. [RESTORE deyimleri](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql).

### <a name="service-broker"></a>Hizmet Aracısı

Çapraz örnek hizmet Aracısı desteklenmez:

- `sys.routes`: Bir önkoşul olarak adresi sys.routes seçmeniz gerekir. Adres yerel her bir yol olmalıdır. Bkz: [sys.routes](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-routes-transact-sql).
- `CREATE ROUTE`: Kullanamazsınız `CREATE ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [Oluştur ROTA](https://docs.microsoft.com/sql/t-sql/statements/create-route-transact-sql).
- `ALTER ROUTE`: Kullanamazsınız `ALTER ROUTE` ile `ADDRESS` dışında `LOCAL`. Bkz: [ALTER ROTA](https://docs.microsoft.com/sql/t-sql/statements/alter-route-transact-sql). 

### <a name="stored-procedures-functions-and-triggers"></a>Saklı yordamları, işlevleri ve Tetikleyicileri

- `NATIVE_COMPILATION` Genel amaçlı katmanında desteklenmiyor.
- Aşağıdaki [sp_configure](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-configure-transact-sql) seçenekleri desteklenmez: 
  - `allow polybase export`
  - `allow updates`
  - `filestream_access_level`
  - `remote data archive`
  - `remote proc trans`
- `sp_execute_external_scripts` desteklenmiyor. Bkz: [sp_execute_external_scripts](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql#examples).
- `xp_cmdshell` desteklenmiyor. Bkz: [xp_cmdshell](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/xp-cmdshell-transact-sql).
- `Extended stored procedures` Desteklenmeyen, içeren `sp_addextendedproc`  ve `sp_dropextendedproc`. Bkz: [genişletilmiş saklı yordamlar](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/general-extended-stored-procedures-transact-sql).
- `sp_attach_db`, `sp_attach_single_file_db`, ve `sp_detach_db` desteklenmez. Bkz: [sp_attach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-db-transact-sql), [sp_attach_single_file_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-attach-single-file-db-transact-sql), ve [sp_detach_db](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-detach-db-transact-sql).

## <a name="Changes"></a> Davranış değişiklikleri

Aşağıdaki değişkenler, İşlevler ve görünümleri farklı sonuçlar döndürebilir:

- `SERVERPROPERTY('EngineEdition')` 8 değeri döndürür. Bu özellik, bir yönetilen örnek benzersiz olarak tanımlar. Bkz: [SERVERPROPERTY](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `SERVERPROPERTY('InstanceName')` için SQL Server yönetilen örnek için geçerli değildir, örnek olarak bu kavramı var olduğundan, NULL döndürür. Bkz: [SERVERPROPERTY('InstanceName')](https://docs.microsoft.com/sql/t-sql/functions/serverproperty-transact-sql).
- `@@SERVERNAME` tam DNS "bağlanılabilirlik" adı, örneğin, yönetilen my instance.wcus17662feb9ce98.database.windows.net döndürür. Bkz: [@@SERVERNAME](https://docs.microsoft.com/sql/t-sql/functions/servername-transact-sql). 
- `SYS.SERVERS` bir tam DNS "bağlanılabilirlik" adı gibi döndürür `myinstance.domain.database.windows.net` özellikleri "name" ve "data_source." Bkz: [SYS. SUNUCULARI](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-servers-transact-sql).
- `@@SERVICENAME` için SQL Server yönetilen örnek için geçerli değildir, hizmet kavramını var olduğundan, NULL döndürür. Bkz: [@@SERVICENAME](https://docs.microsoft.com/sql/t-sql/functions/servicename-transact-sql).
- `SUSER_ID` desteklenir. Azure AD oturum açma sys.syslogins içinde değilse NULL döndürür. Bkz: [SUSER_ID](https://docs.microsoft.com/sql/t-sql/functions/suser-id-transact-sql). 
- `SUSER_SID` desteklenmiyor. Bilinen sorun geçici olduğu yanlış veriler döndürülür. Bkz: [SUSER_SID](https://docs.microsoft.com/sql/t-sql/functions/suser-sid-transact-sql). 

## <a name="Issues"></a> Bilinen sorunlar ve sınırlamalar

### <a name="tempdb-size"></a>TEMPDB boyutu

En büyük dosya boyutu `tempdb` genel amaçlı katmanında çekirdek başına 24 GB değerinden fazla olamaz. En fazla `tempdb` boyutu iş açısından kritik katmanında Örnek Depolama boyutuyla sınırlıdır. `tempdb` Veritabanı her zaman 12 veri dosyalarıyla bölünür. Bu maksimum boyutu dosya başına değiştirilemez ve yeni dosyaları eklenebilir `tempdb`. Bazı sorgular, çekirdek başına birden fazla 24 GB'a ihtiyacınız varsa bir hata döndürebilir `tempdb`.

### <a name="cant-restore-contained-database"></a>Kapsanan veritabanı geri yüklenemiyor

Yönetilen örnek geri yükleyemiyor [içerdiği veritabanları](https://docs.microsoft.com/sql/relational-databases/databases/contained-databases). Kapsanan veritabanlarında, zaman içinde nokta geri yönetilen örneği'nde çalışmaz. Bu sorun yakında çözülecektir. Bu sırada, yönetilen örneği'nde yerleştirilir, veritabanlarından içerme seçeneği kaldırmanız önerilir. İçerme seçeneği üretim veritabanı için kullanmayın. 

### <a name="exceeding-storage-space-with-small-database-files"></a>Küçük veritabanı dosyalarıyla aşan depolama alanı

`CREATE DATABASE`, `ALTER DATABASE ADD FILE`, ve `RESTORE DATABASE` deyimleri örneği Azure depolama sınırına ulaşıp ulaşamadığını nedeniyle başarısız olabilir.

Her bir genel amaçlı yönetilen örneği, en fazla 35 TB depolama alanı Azure Premium Disk alanı için ayrılmış sahiptir. Her veritabanını ayrı bir fiziksel diskte yer alır. Disk boyutları 128 GB, 256 GB, 512 GB, 1 TB veya 4 TB olabilir. Diskte kullanılmayan alan dolu değil, ancak Azure Premium Disk boyutları toplam 35 TB aşamaz. Bazı durumlarda, toplam 8 TB gerekli olmayan bir yönetilen örnek 35 TB Azure sınırlama nedeniyle iç parçalanma depolama boyutu aşabilir.

Örneğin, yönetilen örnek genel amaçlı boyutu 4 TB diskte 1,2 TB olan bir dosya olabilir. Ayrıca, ayrı 128 GB diskler üzerinde yerleştirilen her 1 GB boyutunda 248 dosyaları da olabilir. Bu örnekte:

- Toplam ayrılmış disk depolama boyutudur 4 x 1 TB + 248 x 128 GB = 35 TB.
- örneğindeki veritabanları için ayrılan toplam alan olan 1.2 x 1 TB + 248 x 1 GB = 1,4 TB.

Dosyaları, belirli bir dağıtım nedeniyle belirli koşullar altında bir yönetilen örnek için beklediğiniz değil, bağlı bir Azure Premium Disk için ayrılmış 35 TB sınırına ulaşmadığınız, bu örnekte gösterilmektedir.

Bu örnekte, var olan veritabanlarını çalışmaya devam eder ve yeni dosyalar eklenirken olmadığı sürece herhangi bir sorun büyüyebilir. Yeni veritabanları oluşturulamaz veya tüm veritabanlarının toplam boyutu örneği boyut sınırını ulaşmaz olsa bile yeni disk sürücüsü için yeterli alan olmadığından geri yüklendi. Bu durumda döndürülen hata, NET değildir.

Yapabilecekleriniz [kalan dosyaların sayısını belirleme](https://medium.com/azure-sqldb-managed-instance/how-many-files-you-can-create-in-general-purpose-azure-sql-managed-instance-e1c7c32886c1) sistem görünümleri kullanarak. Bu sınıra ulaştıysanız deneyin [boş ve DBCC SHRINKFILE deyimini kullanarak daha küçük dosyalar bazılarını silin](https://docs.microsoft.com/sql/t-sql/database-console-commands/dbcc-shrinkfile-transact-sql#d-emptying-a-file) veya geçiş [bu sınırı yok iş açısından kritik katmanında](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-resource-limits#service-tier-characteristics).

### <a name="incorrect-configuration-of-the-sas-key-during-database-restore"></a>SAS anahtarını yanlış yapılandırması sırasında veritabanı geri yükleme

`RESTORE DATABASE` .bak dosyası sürekli yeniden deneme .bak okumak için okuma dosyası ve bir hata uzun bir süre sonra döndürür, paylaşılan erişim imzası `CREDENTIAL` yanlış. SAS anahtarının doğru olduğundan emin olmak için bir veritabanını geri yüklemeden önce geri HEADERONLY yürütün.
Önde gelen kaldırdığınızdan emin olun `?` SAS anahtarından Azure portalı kullanılarak oluşturulur.

### <a name="tooling"></a>Araç kullanımı

Yönetilen örnek'na erişirken, SQL Server Management Studio ve SQL Server veri araçları, bazı sorunlara sahip olabilir.

- SQL Server veri araçları ile Azure AD sunucu sorumlusu (oturum açma bilgileri) ve kullanıcıları (genel Önizleme) şu anda kullanımı desteklenmiyor.
- Azure AD sunucu sorumlusu (oturum açma bilgileri) ve kullanıcılar (genel Önizleme) için komut dosyası, SQL Server Management Studio'da desteklenmez.

### <a name="incorrect-database-names-in-some-views-logs-and-messages"></a>Bazı görünümler, günlükleri ve iletileri yanlış veritabanı adları

Çeşitli sistem görünümleri, performans sayaçları, hata iletileri, Xevent'ler ve hata günlüğü girişleri gerçek veritabanı adları yerine GUID veritabanı tanımlayıcıları görüntüler. Bunlar gerçek veritabanı adları ile gelecekte değiştirilmiş olduğundan bu GUID tanımlayıcılarını güvenmeyin.

### <a name="database-mail"></a>Veritabanı posta

`@query` Parametresinde [sp_send_db_mail](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-send-dbmail-transact-sql) yordamı çalışmaz.

### <a name="database-mail-profile"></a>Veritabanı posta profili

SQL Server aracısı tarafından kullanılan veritabanı posta profili çağrılmalıdır `AzureManagedInstance_dbmail_profile`. Diğer veritabanı posta profili adları için sınırlama yoktur.

### <a name="error-logs-arent-persisted"></a>Hata günlüklerini kalıcı değil

Yönetilen örneği'nde kullanılabilir olan hata günlüklerini kalıcı değildir ve bunların boyutu en fazla depolama sınırı, dahil değildir. Yük devretme gerçekleşirse Hata günlüklerini otomatik olarak silinmiş.

### <a name="error-logs-are-verbose"></a>Ayrıntılı hata günlükleri

Yönetilen örnek hata günlükleri ayrıntılı bilgiler yerleştirir ve çoğu geçerli değildir. Gelecekte, hata günlüklerinde bilgi miktarını azaltır.

**Geçici çözüm:** Bazı ilgisiz girişleri filtreler Hata günlüklerini okumak için özel bir yordamı kullanın. Daha fazla bilgi için [yönetilen örneği – sp_readmierrorlog](https://blogs.msdn.microsoft.com/sqlcat/2018/05/04/azure-sql-db-managed-instance-sp_readmierrorlog/).

### <a name="transaction-scope-on-two-databases-within-the-same-instance-isnt-supported"></a>İşlem kapsamı aynı örneği içinde iki veritabanlarında desteklenmiyor

`TransactionScope` .NET sınıfında işe yaramazsa aynı örneğinde aynı işlem kapsamı altında iki veritabanı için iki sorgu gönderilir:

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

**Geçici çözüm:** Kullanma [SqlConnection.ChangeDatabase(String)](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlconnection.changedatabase) iki bağlantı kullanmak yerine bir bağlantı bağlamı başka bir veritabanı kullanmak için.

### <a name="clr-modules-and-linked-servers-sometimes-cant-reference-a-local-ip-address"></a>CLR modülleri ve bazen bağlı sunucular, yerel bir IP adresi başvuramaz

Yönetilen örnek ve bağlı sunucular ya da geçerli bir örneği bazen başvuru Dağıtılmış sorgularda yerleştirilen CLR modülleri, yerel bir örneğinin IP çözümlenemiyor. Bu hata geçici bir sorundur.

**Geçici çözüm:** Bağlam bağlantılarını mümkünse bir CLR modülde kullanın.

### <a name="tde-encrypted-databases-with-a-service-managed-key-dont-support-user-initiated-backups"></a>Bir hizmetle yönetilen anahtarı ile TDE şifreli veritabanlarına kullanıcı tarafından başlatılan yedeklemeleri desteklemez.

Kod yürütemez `BACKUP DATABASE ... WITH COPY_ONLY` hizmetle yönetilen şeffaf veri şifrelemesi (TDE ile) şifrelenmiş bir veritabanı. Hizmetle yönetilen TDE yedeklemelerinin bir iç TDE anahtarla şifrelenmesini zorlar. Yedeklemeyi geri yüklemek için anahtar dışarı aktarılamıyor.

**Geçici çözüm:** Otomatik yedeklemeler ve zaman içinde nokta geri yükleme veya kullanın [müşteri tarafından yönetilen (BYOK) TDE](https://docs.microsoft.com/azure/sql-database/transparent-data-encryption-azure-sql#customer-managed-transparent-data-encryption---bring-your-own-key) yerine. Veritabanı şifreleme devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen örnekleri hakkında daha fazla bilgi için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Bir özellik için ve karşılaştırma listesini görmek [Azure SQL veritabanı özellik karşılaştırması](sql-database-features.md).
- Yeni bir yönetilen örneğin nasıl oluşturulacağını gösteren Hızlı Başlangıç için bkz: [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
