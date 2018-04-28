---
title: Azure SQL veritabanı yönetilen örneği'ne genel bakış | Microsoft Docs
description: Bu konu, yönetilen bir Azure SQL veritabanı örneği açıklar ve nasıl çalıştığı ve nasıl Azure SQL Database tek bir veritabanında farklı olduğu açıklanmaktadır.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: article
ms.date: 04/10/2018
ms.author: bonova
ms.openlocfilehash: eeb6b74fb7dfbf25e27963dd7a2f7f431feebcc8
ms.sourcegitcommit: 1362e3d6961bdeaebed7fb342c7b0b34f6f6417a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2018
---
# <a name="what-is-a-managed-instance-preview"></a>Bir yönetilen örneği (Önizleme) nedir?

Azure SQL veritabanı örneği (Önizleme) yönetilen yerel sağlama % 100 uyumluluk SQL Server şirket içi (Enterprise Edition) sağlayan Azure SQL veritabanı'nın yeni bir özellik olan [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) uygulamasında, genel güvenlik sorunları giderir ve [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) şirket içi SQL Server müşterileri için olumlu. Yönetilen örneği var olan SQL Server müşterilerin kaldırın ve şirket uygulamalarını küçük uygulama ve veritabanı değişiklikler ile bulut shift olanak tanır. Aynı anda yönetilen örnek yönetim yükünüzü ve toplam sahip olma Maliyetini önemli ölçüde azaltan PaaS olanaklarına (otomatik düzeltme eki uygulama ve sürüm güncelleştirmeleri, yedekleme, yüksek kullanılabilirlik) korur.

> [!IMPORTANT]
> Yönetilen Örneğin şu anda kullanılabilir olduğu bölgelerin listesi için bkz. [Azure SQL Veritabanı Yönetilen Örneği ile tam yönetilen hizmete veritabanlarınızı geçirme](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
Aşağıdaki diyagramda yönetilen örneğinin temel özellikleri özetlenmektedir:

![Anahtar Özellikler](./media/sql-database-managed-instance/key-features.png)

Aşağıdaki senaryolar için tercih edilen platformu olarak yönetilen örneği envisioned: 

- Şirket içi SQL Server / Iaas müşteriler için en az ile tam olarak yönetilen bir hizmet uygulamalarını geçirmeye bakıyorsanız değişiklikleri tasarlayın.
- Buluta geçiş ve önemli rekabet avantajı dolayısıyla elde müşterilerine isteyen SQL veritabanları güvenmek ISV'ler etkinleştirin veya küresel pazarda ulaşmak. 

Genel kullanılabilirlik tarafından yönetilen örneği en son sürümle şirket içi SQL Server hazırlanmış yayın planı aracılığıyla % 100 yüzey alanını uyumluluk yakın sunmak için amaçlar. 

Aşağıdaki tablo anahatları farklar anahtar ve SQL Iaas, Azure SQL Database ve SQL veritabanı yönetilen örneği arasında kullanım senaryoları envisioned:

| | Kullanım senaryosu | 
| --- | --- | 
|SQL Veritabanı Yönetilen Örneği |Şirket içi veya otomatik olarak oluşturulan, Iaas veya sağlanan, ISV çok sayıda uygulamaları geçirmek isteyen müşteriler için ile mümkün olduğunca düşük geçiş çaba olarak yönetilen örneği önerin. Tam otomatik kullanarak [veri taşıma hizmeti (DMS)](/sql/dma/dma-overview) Azure'da, müşterilerinizin kaldırın ve kendi şirket içi SQL Server örneğine bir yönetilen şirket içi SQL Server ve tam yalıtımını uyumluluğu sunan kaydırma Yerel VNET desteğiyle müşteri örnekleri.  Yazılım Güvencesi ile kullanarak bir SQL veritabanı yönetilen örneği üzerinde indirimli fiyatlar için var olan, lisansları değiştirebilir [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  SQL veritabanı yönetilen bulut yüksek güvenlik ve zengin programlamasına yüzeyini gerektiren SQL Server örnekleri için en iyi geçiş hedef örneğidir. |
|Azure SQL veritabanı (tek veya havuz) |**Esnek havuzlar**: yeni SaaS çok kiracılı uygulamaları geliştirme veya kasıtlı olarak var olan dönüştürme müşteriler uygulamaları SaaS çok müşterili bir uygulamaya şirket için esnek havuzlar önerin. Bu model avantajları şunlardır: <br><ul><li>Servis abonelikleri (ISV'ler için) satış lisansları satış gelen iş modeli dönüştürme</li></ul><ul><li>Kolay ve madde işareti kanıt Kiracı yalıtımı</li></ul><ul><li>Basitleştirilmiş bir veritabanı odaklı programlama modeli</li></ul><ul><li>Olası sabit bir tavan basarsa olmadan ölçeği genişletme</li></ul>**Tek veritabanlarını**: iş yükü kararlı ve öngörülebilir, SaaS çok kiracılı dışında yeni uygulama geliştirme müşterilerin tek veritabanlarını önermek için. Bu model avantajları şunlardır:<ul><li>Basitleştirilmiş bir veritabanı odaklı programlama modeli</li></ul>  <ul><li>Her veritabanı için tahmin edilebilir performans</li></ul>|
|SQL Iaas sanal makine|İşletim sistemini veya veritabanı sunucusu yanı sıra (aynı VM'de), SQL Server ile yan yana çalışan üçüncü taraf uygulamalar açısından belirli gereksinimlerine sahip müşteriler özelleştirmek ihtiyaç duyan müşteriler SQL VM'ler önermek için / Iaas en iyi çözümü olarak|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Program aracılığıyla yönetilen örneği tanımlama

Aşağıdaki tabloda çeşitli özellikleri, Transact SQL erişilebilir uygulamanızı yönetilen örneğiyle çalıştığını algılamak üzere kullanmak ve önemli özelliklerini almak gösterir.

|Özellik|Değer|Açıklama|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Telif Hakkı (C) 2018 Microsoft Corporation.|Bu değer, SQL veritabanında olduğu gibi aynı olur.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Bu değer, SQL veritabanında olduğu gibi aynı olur.|
|`SERVERPROPERTY('EngineEdition')`|8|Bu değer yönetilen örneğini benzersiz şekilde tanımlar.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Tam örnek DNS adı şu biçimde:<instanceName>.<dnsPrefix>. Database.Windows.NET, burada <instanceName> müşteri tarafından sağlanan ad sırada <dnsPrefix> Genel DNS adı benzersizliği güvence altına almak adı otomatik olarak oluşturulan parçası ("wcus17662feb9ce98", örneğin)|Örnek: my-yönetilen-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Anahtar özellik ve yetenekler yönetilen bir örneği 

> [!IMPORTANT]
> Yönetilen bir örnek özelliklerin tümü, çevrimiçi işlemler, otomatik planı düzeltmeleri ve diğer kurumsal performans iyileştirmeleri de dahil olmak üzere SQL Server ' ın en son sürümü ile çalışır. 

| **PaaS avantajları** | **İş sürekliliği** |
| --- | --- |
|Donanım satın alma ve Yönetimi <br>Temel alınan altyapısını yönetmeye yönelik ek yükü yönetimi yok <br>Hızlı sağlama ve hizmet ölçeklendirme <br>Otomatik düzeltme eki uygulama ve sürüm yükseltme <br>Diğer PaaS Veri Hizmetleri ile tümleştirme |% 99,99 çalışma süresi SLA'sı  <br>Yerleşik yüksek kullanılabilirlik <br>Otomatik yedeklemeler sayesinde korunan verileri <br>Müşteri yapılandırılabilir yedekleme Bekletme dönemi (7 gün içinde genel Önizleme için sabit) <br>Kullanıcı tarafından başlatılan yedekleme <br>Geri yükleme yeteneği zaman veritabanındaki noktası |
|**Güvenlik ve uyumluluk** | **Yönetim**|
|Yalıtılmış ortam (VNet tümleştirme, Kiracı tek hizmet, adanmış bir işlem ve depolama <br>Aktarımdaki verileri şifreleme <br>Azure AD kimlik doğrulaması, tek oturum açma desteği <br>Uyumluluk standartlarına aynı Azure SQL veritabanı olarak uyar <br>SQL denetimi <br>Tehdit algılama |Hizmet sağlama ve ölçeklendirme otomatikleştirmek için Azure Resource Manager API <br>El ile hizmet sağlama ve ölçeklendirme için Azure portal işlevi <br>Veri Taşıma hizmeti 

![Çoklu oturum açma](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model"></a>vCore tabanlı satın alma modeli

Ve çevirmek için basit bir yol içi buluta iş yükü gereksinimlerini esneklik, Denetim, saydamlık vCore tabanlı satın alma modeli sağlar. Bu model, bilgi işlem, bellek ve kendi iş yükü ihtiyaçlarına depolama olanak tanır. VCore modeli de yüzde 30 tasarrufları ile için uygun yukarı [SQL Server için Azure karma kullanımı avantajı](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Sanal bir çekirdek donanım nesli arasında seçmek için bir seçenek ile birlikte sunulan mantıksal CPU temsil eder.
- 4. Nesil Mantıksal CPU’lar Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri temel alır.
- 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı gen (Broadwell) 2.3 GHz işlemci.

Aşağıdaki tabloda, en uygun yapılandırma hesaplama, bellek, depolama ve g/ç kaynakları seçmek nasıl anlamanıza yardımcı olur.

||4. Nesil|5. Nesil|
|----|------|-----|
|Donanım|Intel E5-2673 v3 (Haswell) 2.4 GHz işlemci, bağlı SSD vCore = 1 GÖ (fiziksel çekirdek)|Intel E5-2673 v4 (Broadwell) 2.3 GHz işlemci, hızlı eNVM SSD, vCore = 1 LP (iş parçacığı hyper)|
|Performans seviyeleri|8, 16, 24 vCores|8, 16, 24, 32, 40 vCores|
|Bellek|VCore başına 7GB|VCore başına 5.5GB|
||||

## <a name="managed-instance-service-tier"></a>Örnek hizmet katmanı yönetilen

Yönetilen örneği tipik kullanılabilirlik ve ortak g/ç gecikme gereksinimlerine uygulamalar için tasarlanmış bir tek hizmet katmanına - genel amaçlı - başlangıçta kullanılabilir.

Aşağıdaki listede, genel amaçlı hizmet katmanının anahtar özelliği açıklanmaktadır: 

- Genel performans ve HA gereksinimleri iş uygulamalarının çoğu için tasarlama 
- Yüksek performanslı Azure Premium storage (8 TB) 
- 100 veritabanları / örnek 

Bu katmanında bağımsız olarak depolamayı seçin ve hesaplama kapasitesi. 

Aşağıdaki diyagram, etkin işlem ve bu hizmet katmanında yedekli düğümleri gösterir.
 
![Genel amaçlı hizmet katmanı](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

Genel amaçlı hizmet Katmanı'nın temel özellikleri özetlenmektedir:

|Özellik | Açıklama|
|---|---|
| VCores * sayısı | 8, 16, 24 (gen 4)<br>8, 16, 24, 32, 40 (Gen5)|
| SQL Server sürümü / build | SQL Server Son (kullanılabilir) |
| En küçük depolama boyutu | 32 GB |
| En fazla depolama boyutu | 8 TB |
| Veritabanı başına en fazla depolama | 8 TB |
| Beklenen depolama IOPS | 500-7500 IOPS her veri dosyası (veri dosyası üzerinde bağlıdır). Bkz: [Premium depolama](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Veritabanı başına veri dosyası (satırlar) sayısı | Birden çok | 
| Veritabanı başına günlük dosyası (günlük) sayısı | 1 | 
| Otomatik yedeklemeler yönetilen | Evet |
| HA | Uzak depolama birimine dayalı ve [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Yerleşik örneği ve veritabanı izleme ve ölçümleri | Evet |
| Otomatik yazılım düzeltme eki uygulama | Evet |
| VNet - Azure Resource Manager dağıtımı | Evet |
| VNet - Klasik dağıtım modeli | Hayır |
| Portal destek | Evet|
|||

\* Sanal bir çekirdek donanım nesli arasında seçmek için bir seçenek ile birlikte sunulan mantıksal CPU temsil eder. 4 mantıksal CPU üzerinde Intel E5-2673 v3 dayalı gen (Haswell) 2.4 GHz işlemci ve Gen 5 mantıksal CPU üzerinde Intel E5-2673 v4 dayalı (Broadwell) 2.3 GHz işlemci. 

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk 

### <a name="managed-instance-security-isolation"></a>Yönetilen örneği güvenlik yalıtımı 

Yönetilen örneği Azure bulutta diğer kiracıdan ek güvenlik yalıtım sağlar. Güvenlik yalıtımı içerir: 

- [Yerel sanal ağ uygulaması](sql-database-managed-instance-vnet-configuration.md) ve Azure Express rota ya da VPN ağ geçidi kullanarak, şirket içi ortamınız için bağlantı 
- SQL uç noktası güvenli bağlantı özel Azure veya karma ağları izin vererek yalnızca özel bir IP adresi, aracılığıyla sunulur
- Tek-Kiracı altyapısıyla ayrılmış temel alınan (işlem, depolama)

Aşağıdaki diyagramda yalıtım tasarım özetlenmektedir: 

![Yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

### <a name="auditing-for-compliance-and-security"></a>Uyumluluk ve güvenlik denetimi 

[Örnek denetim yönetilen](sql-database-managed-instance-auditing.md) parçaları veritabanı olayları ve Azure depolama hesabınızdaki bunları Denetim günlüğüne yazar. Denetim yönetmeliklere uygunluğu korumanıza, veritabanı etkinliklerini anlamanıza ve ifade eden tutarsızlıklar ve iş endişeleri veya güvenlik ihlalleri hakkında daha fazla bilgi kavramanıza yardımcı olabilir. 

### <a name="data-encryption-in-motion"></a>Hareket halinde veri şifrelemesi 

Yönetilen örneği, Aktarım Katmanı Güvenliği kullanarak Hareket halindeki verileriniz için şifreleme sağlayarak, verilerinizin güvenliğini sağlar.

Aktarım Katmanı Güvenliği yanı sıra, SQL veritabanı yönetilen örneği yürütülen, REST ve sorgu ile işleme sırasında önemli verilerin korunması sunar [her zaman şifreli](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted, kritik öneme sahip verilerin çalınması gibi güvenlik ihlallerine karşı sektörde benzersiz koruma sağlayan bir özelliktir. Örneğin, her zaman şifreli ile kredi kartı numaraları veritabanında her zaman bile işlenmesi, şifre çözme kullanım noktasında yetkili personel veya bu verileri işlemek için gereken uygulamalar tarafından izin sorgu sırasında şifrelenmiş olarak depolanır. 

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme 

SQL veritabanı [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking) ayrıcalıksız kullanıcılara maskeleyerek gizli verilerin açığa sınırlar. Dinamik veri maskeleme uygulama katmanı üzerinde en az etkiyle ortaya çıkarmak için gizli verilerin ne kadar tanımlamanızı etkinleştirerek hassas verilere yetkisiz erişimi önlemeye yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir. 

### <a name="row-level-security"></a>Satır düzeyi güvenlik 

[Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) göre bir sorgu yürütülürken kullanıcı özelliklerini denetleme erişmek için bir veritabanı tablosundaki satırları sağlar (gibi grubu üyeliği veya yürütme bağlamı olarak). Satır düzeyi güvenlik (RLS), uygulamanızın güvenlik tasarımını ve kodlama aşamasını kolaylaştırır. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin, Çalışanlar yalnızca kendi departmanı veya bir veri yalnızca ilgili verilere erişimi kısıtlayarak ilgili veri satırları eriştiğinden emin olun. 

### <a name="threat-detection"></a>Tehdit algılama 

[Yönetilen örneği tehdit algılama](sql-database-managed-instance-threat-detection.md) tamamlar [yönetilen örneği denetim](sql-database-managed-instance-auditing.md) olağan dışı ve zararlı girişimleri algılar hizmeti yerleşik güvenlik Intelligence ek katmanı sağlayarak erişim veya yararlanma veritabanları. Kuşkulu etkinlikler, olası güvenlik açıkları hakkında bir uyarı ve anormal veritabanı erişimi desenleri yanı sıra SQL ekleme saldırıları. Tehdit algılama uyarıları, gelen görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) ve şüpheli etkinlik ayrıntılarını sağlayın ve eylem araştırmak ve tehdidi azaltmak nasıl öneririz.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması 

SQL Veritabanı, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md) ile veritabanı kullanıcısı ve diğer Microsoft hizmetleri kimliklerini bir merkezden yönetmenizi sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md) (MFA) ve çoklu oturum açma işlemini destekler. 

### <a name="authentication"></a>Kimlik Doğrulaması 
SQL veritabanı kimlik doğrulaması nasıl kullanıcıların kimliğini veritabanına bağlanırken kanıtlamak için ifade eder. SQL Veritabanı iki kimlik doğrulaması türünü destekler:  

- SQL kullanıcı adı ve parola kullanan kimlik doğrulaması.
- Azure Active Directory, Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenen kimlik doğrulaması. 

### <a name="authorization"></a>Yetkilendirme

Yetkilendirme hangi kullanıcı bir Azure SQL veritabanı içinde yapabilirsiniz ve kullanıcı hesabınızın veritabanı rol üyeliklerini ve nesne düzeyi izinleri tarafından denetlenen ifade eder. Yönetilen örneği SQL Server 2017 aynı yetkilendirme özelliklerine sahiptir. 

## <a name="database-migration"></a>Veritabanı geçişi 

Örnek hedefleri kullanıcı senaryoları yığın veritabanı geçiş ile şirket içi veya Iaas veritabanı uygulamaları yönetilen. Örnek destekler birkaç veritabanı geçiş seçenekleri yönetilen: 

### <a name="data-migration-service"></a>Veri Taşıma hizmeti

Azure veritabanı geçiş hizmeti, Azure veri platformları en az kapalı kalma süresi ile birden çok veritabanı kaynakları sorunsuz kümesinden sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmet, var olan üçüncü taraf ve SQL Server veritabanlarınızı Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri genel Önizleme sırasında Azure VM'de Azure SQL veritabanını, örneği yönetilen ve SQL Server'ı içerir. Bkz: [DMS kullanarak örneğini yönetilen şirket içi veritabanınızı geçirmek nasıl](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Yedekleme ve geri yükleme  

Geçiş yaklaşım SQL yedeklemeleri Azure blob depolamaya yararlanır. Azure depolama blobunu depolanan yedeklerini doğrudan yönetilen örneğine geri yüklenebilir. Varolan bir SQL veritabanının bir yönetilen örneğine geri yüklemek için şunları yapabilirsiniz:

- Kullanım [veri taşıma hizmeti (DMS)](/sql/dma/dma-overview). Bir öğretici için bkz: [yönetilen Azure veritabanı geçiş hizmeti (DMS) kullanarak bir örneğini geçiş](../dms/tutorial-sql-server-to-managed-instance.md) bir veritabanı yedekleme dosyasından geri yüklemek için
- Kullanım [T-SQL Geri Yükle komutunu](https://docs.microsoft.com/en-us/sql/t-sql/statements/restore-statements-transact-sql). 
  - Wide World Importers - standart veritabanı yedek dosyasını geri yükleme gösteren bir öğretici için bkz: [bir yedekleme dosyası yönetilen bir örneğine geri](sql-database-managed-instance-restore-from-backup-tutorial.md). Bu öğretici, Azure blogu depolama ve güvenli bir paylaşılan erişim imzası (SAS) anahtarı kullanarak bir yedekleme dosyası karşıya yüklemeniz gösterir.
  - URL'den geri yükleme hakkında daha fazla bilgi için bkz: [yerel geri URL'den](sql-database-managed-instance-migrate.md#native-restore-from-url).
- [Bir BACPAC Dosyadan İçeri Aktar](sql-database-import.md)

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri 

Şirket içi SQL Server hizmetinin genel kullanılabilirlik kadar aşamada gelen ile % 100 yüzey alanını uyumluluk yakın sunmak için örnek amaçlar yönetilen. Bir özellik için ve karşılaştırma listesi, bkz: [SQL ortak özellikleri](sql-database-features.md).
 
SQL 2008 veritabanları için örneği destekleyen geriye dönük uyumluluk yönetilen. SQL 2005 veritabanı sunucularından doğrudan geçiş desteklenir, uyumluluk düzeyi geçirilen SQL 2005 veritabanları için SQL 2008 güncelleştirilir. 
 
Aşağıdaki diyagramda yüzey alanını uyumluluk yönetilen örneğinde özetlenmektedir:  

![Geçiş](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Şirket içi SQL Server ve örnek yönetilen arasındaki farklar 

Örnek avantajları yönetilen engeller her zaman-yukarı-to-date şirket içi SQL Server'ın bazı özellikleri ya da olabilir anlamına gelir kullanılmayan, bulutta devre dışı bırakılan veya seçeneğiniz vardır. Araçlar, belirli bir özellik biraz farklı bir şekilde çalıştığını veya hizmet değil tam denetim bir ortamda çalışıp çalışmadığını düzenlemeniz gerekirse, özel durumlar vardır: 

- Yüksek kullanılabilirlik yerleşik olarak bulunur ve önceden yapılandırılmış. Yüksek kullanılabilirlik özellikleri her zaman açık SQL Iaas uygulamalarında olduğu gibi aynı şekilde sunulmaz 
- Otomatik yedekleme ve geri yükleme noktası. Müşteri başlatabilir `copy-only` olan otomatik yedekleme zinciri karışmaması yedekler. 
- Yönetilen örneği izin vermiyor farklı desteklenmesi tüm ilgili senaryoları alacak şekilde tam fiziksel yolunu belirtme: geri DB WITH MOVE desteklemez, BULK INSERT çalışır ile Azure BLOB'ları yalnızca, vb. oluşturmak DB fiziksel yollar izin vermez. 
- Örnek destekleyen yönetilen [Azure AD kimlik doğrulaması](sql-database-aad-authentication.md) bulut alternatif olarak Windows kimlik doğrulaması. 
- Yönetilen örneği XTP dosya ve bellek içi OLTP nesnelerini içeren veritabanları için dosyaları otomatik olarak yönetir
 
### <a name="managed-instance-administration-features"></a>Yönetilen örnek yönetim özellikleri  

İş için en önemli noktalara odaklanmasına örneği etkinleştir Sistem Yöneticisi yönetilen. Birçok sistem yöneticisi/DBA etkinlik gerekli değildir veya basit. Örneğin, işletim sistemi / RDBMS yükleme ve düzeltme eki, dinamik örnek yeniden boyutlandırma ve yapılandırma, yedeklemeler, veritabanı çoğaltması (sistem veritabanları dahil), yüksek kullanılabilirlik yapılandırması ve sağlık ve performans verilerini izleme yapılandırması Akışlar. 

> [!IMPORTANT]
> Desteklenen, kısmen desteklenen ve desteklenmeyen özelliklerin listesi için bkz: [SQL veritabanı özellikleri](sql-database-features.md). Yönetilen örnekleri SQL Server ile T-SQL farkları listesi için bkz: [SQL Server örneği T-SQL farkları yönetilen](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- Bir özellik için ve karşılaştırma listesi, bkz: [SQL ortak özellikleri](sql-database-features.md).
- Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Yönetilen Örnek Sanal Ağ Yapılandırması](sql-database-managed-instance-vnet-configuration.md).
- Yönetilen bir örnek oluşturur ve bir yedek dosyasından bir veritabanı geri yükleyen bir öğretici için bkz: [bir yönetilen örneği oluşturmayı](sql-database-managed-instance-create-tutorial-portal.md).
- Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md).
