---
title: Azure SQL veritabanı yönetilen örneği'ne genel bakış | Microsoft Docs
description: Bu konu Azure SQL veritabanı yönetilen örneği ve nasıl çalıştığını ve nasıl tek bir veritabanının Azure SQL veritabanı'nda farklı olduğunu açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: aaffdb2f0d21dbb7a5baf029a90e452658c5bd31
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353645"
---
# <a name="use-sql-database-managed-instance-with-virtual-networks-and-near-100-compatibility"></a>SQL veritabanı yönetilen örneği, sanal ağlarla ve neredeyse % 100 uyumluluk kullanın

Azure SQL veritabanı yönetilen örneği, Azure SQL veritabanı neredeyse % 100 uyumluluk en son SQL Server ile şirket içi (Enterprise Edition) veritabanı altyapısı sağlayarak, yerel sağlama, yeni bir dağıtım modeli olduğundan [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) ortak güvenlik endişelerini ortadan uygulama ve [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) şirket içi SQL Server müşterileri için yeterli. Yönetilen örnek lift- and -shift kendi şirket içi uygulamaları bulutta çok az değişiklikle uygulama ve veritabanı mevcut SQL Server müşterileri sağlar. Aynı anda yönetilen örneği, tüm PaaS özellikleri korur (otomatik düzeltme eki uygulama ve sürüm güncelleştirmeleri [otomatik yedeklemeler](sql-database-automated-backups.md), [yüksek kullanılabilirlik](sql-database-high-availability.md) ), yönetim yükünü önemli ölçüde azaltan ve toplam sahip olma maliyeti.

> [!IMPORTANT]
> Yönetilen örneği şu anda bölgelerin listesi için bkz. [desteklenen bölgeler](sql-database-managed-instance-resource-limits.md#supported-regions).

Aşağıdaki diyagramda, yönetilen örnek temel özellikleri özetlenmektedir:

![anahtar özellikleri](./media/sql-database-managed-instance/key-features.png)

ISV tam olarak yönetilen PaaS bulut ortamında, ortama mümkün olduğunca az geçiş çaba olarak sağlanan ya da Azure SQL veritabanı yönetilen örneği, şirket içi veya şirket içinde oluşturulmuş, Iaas, çok sayıda uygulamaları geçirmek isteyen müşteriler için tasarlanmıştır. Tam otomatik kullanarak [veri geçiş hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) Azure'da müşteriler kaldırma ve kendi şirket içi SQL Server şirket içi SQL Server ve tam bir yalıtım olanağı ile uyumluluk sağlayan yönetilen örneğe kaydırma yerel sanal ağ ile müşteri örneği destekler.  Yazılım Güvencesi içeren mevcut lisanslarını kullanarak bir SQL veritabanı yönetilen örneği üzerinde indirimli fiyatlar için exchange [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  SQL veritabanı yönetilen örneği, yüksek güvenlik ve zengin programlama yüzeyini gerektiren SQL Server örnekleri için bulutta en iyi geçiş hedef ' dir.

Genel kullanılabilirlik tarafından hazırlanmış yayın planı aracılığıyla en son şirket içi SQL Server sürümü ile % 100 yüzey alanını uyumluluk yakın sunmak için yönetilen örneği amaçlar.

Azure SQL veritabanı tek veritabanı ve Azure SQL veritabanı yönetilen örneği SQL Server Iaas sanal makine bakın barındırılan arasında karar [Azure bulutunda SQL Server'ın doğru sürümü seçmek nasıl](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Temel özellikleri

Azure SQL veritabanı yönetilen örneği, hem Azure SQL veritabanı ve SQL Server veritabanı altyapısı kullanılabilir olan en iyi özelliklerini bir araya getirir.

> [!IMPORTANT]
> Bir yönetilen örnek tüm özellikler çevrimiçi işlemleri, otomatik plan düzeltme ve diğer kurumsal performans geliştirmeleri dahil olmak üzere SQL Server'ın en son sürümü ile çalışır. Özelliklerin karşılaştırması açıklanan [özellik karşılaştırması: SQL Server yerine Azure SQL veritabanı](sql-database-features.md).

| **PaaS avantajları** | **İş sürekliliği** |
| --- | --- |
|Donanım satın alma ve Yönetimi <br>Temel altyapıyı yönetmek için ek yükü yönetimi yok <br>Hızlı sağlama ve hizmet ölçeklendirme <br>Otomatik düzeltme eki uygulama ve sürüm yükseltme <br>Diğer PaaS Veri Hizmetleri ile tümleştirme |% 99,99 çalışma süresi SLA'sı  <br>Yerleşik [yüksek kullanılabilirlik](sql-database-high-availability.md) <br>İle korunan verileri [otomatik yedeklemeler](sql-database-automated-backups.md) <br>Müşteri yapılandırılabilir yedekleme bekletme süresi (genel önizlemede 7 gün için sabit) <br>Kullanıcı tarafından başlatılan [yedekleri](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) <br>[Belirli bir veritabanı geri yükleme noktası](sql-database-recovery-using-backups.md#point-in-time-restore) özelliği |
|**Güvenlik ve uyumluluk** | **Yönetim**|
|Yalıtılmış ortamı ([VNet tümleştirmesi](sql-database-managed-instance-vnet-configuration.md)çoklu kiracı hizmeti, ayrılmış hesaplama ve depolama) <br>[Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Azure AD kimlik doğrulaması](sql-database-aad-authentication.md), çoklu oturum açma desteği <br>Azure SQL veritabanı olarak aynı uyumluluk standartlarına uyar <br>[SQL denetimi](sql-database-managed-instance-auditing.md) <br>[Tehdit algılama](sql-database-managed-instance-threat-detection.md) |Hizmet sağlama ve ölçeklendirme otomatikleştirmek için Azure Resource Manager API'si <br>Sağlama ve ölçeklendirme el ile hizmeti için Azure portal işlevi <br>Veri geçiş hizmeti

Yönetilen örnek temel özelliklerini, aşağıdaki tabloda gösterilmiştir:

|Özellik | Açıklama|
|---|---|
| SQL Server sürümü / build | SQL Server veritabanı altyapısı (en son kararlı) |
| Yönetilen otomatik yedekleri | Evet |
| Yerleşik bir örneği ve veritabanı izleme ve ölçümler | Evet |
| Otomatik yazılım düzeltme eki uygulama | Evet |
| En son veritabanı altyapısı özellikleri | Evet |
| Veritabanı başına veri dosyalarının (satırlar) | Birden çok |
| Günlük dosyası (günlük) veritabanı başına sayısı | 1 |
| VNet - Azure Resource Manager dağıtımı | Evet |
| VNet - Klasik dağıtım modeli | Hayır |
| Portalı desteği | Evet|
| Yerleşik tümleştirme hizmeti (SSIS) | Hayır - SSIS bir parçası olan [Azure Data Factory PaaS](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Yerleşik Analysis Services (SSAS) | Hayır - SSAS ayrıdır [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) |
| Yerleşik raporlama hizmeti (SSRS) | Hayır - Power BI veya SSRS Iaas kullanın |
|||

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

[Sanal çekirdek tabanlı satın alma modeli](sql-database-service-tiers-vcore.md) yönetilen örneği'nde size esneklik, Denetim, saydamlık ve şirket içi iş yükü gereksinimlerini buluta çevirmek için basit bir yol sağlar. Bu model, işlem, bellek ve depolama iş yükü gereksinimlerinize göre değiştirmenizi sağlar. VCore modeli de ile yüzde 30 tasarruf için uygun yedekleme [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Sanal çekirdek modeli içinde donanım Nesilleri arasında seçim yapabilirsiniz.

- **Gen 4** mantıksal CPU'lar Intel E5-2673 v3 dayalı (Haswell) 2,4 GHz işlemcileri, ekli SSD fiziksel çekirdek olarak çekirdek ve bilgi işlem boyutlarına arasındaki 8 ila 24 sanal çekirdek başına 7 GB RAM.
- **5 gen** mantıksal CPU'lar Intel E5-2673 v4 dayalı (Broadwell) 2.3 GHz işlemcileri, hızlı eNVM SSD, mantıksal çekirdek, hiper iş parçacıklıdır ve boyutları 8 ila 80 çekirdeğine işlem.

İçinde donanım Nesilleri arasındaki fark hakkında daha fazla bilgi [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

## <a name="managed-instance-service-tiers"></a>Yönetilen örnek hizmet katmanları

Yönetilen örnek, iki hizmet katmanlarda kullanılabilir:

- **Genel amaçlı**: tipik performans ve g/ç gecikme süresi gereksinimlerine sahip uygulamalar için tasarlanmıştır.
- **İş açısından kritik (Önizleme)**: düşük g/ç gecikme süresi gereksinimleri ve iş yükü temel bakım işlemlerinin en az etki ile uygulamalar için tasarlanmıştır.

Her iki hizmet katmanları, % 99,99 oranında kullanılabilirlik garantisi ve bağımsız olarak depolama boyutu seçin ve hesaplama kapasitesi sağlar. Azure SQL veritabanı yüksek kullanılabilirlik mimarisi hakkında daha fazla bilgi için bkz. [yüksek kullanılabilirlik ve Azure SQL veritabanı](sql-database-high-availability.md).

> [!IMPORTANT]
> Genel amaçlı iş açısından kritik veya tam tersi hizmet katmanınızın değiştirirken, genel Önizleme sürümünde desteklenmiyor. Farklı hizmet katmanında bir örnek için veritabanlarınızı geçirmek istiyorsanız, yeni bir örnek oluşturun, özgün örneğinden noktaya geri yükleme noktası ile veritabanlarını geri yükleme ve artık gerekli değilse özgün örneğe ardından bırakın. Ancak, sanal çekirdek ve depolama ölçeğini artırıp kesinti süresi olmaksızın bir hizmet katmanında sayısı ölçeklendirebilirsiniz.

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

Aşağıdaki listede, genel amaçlı hizmet katmanının önemli bir özelliği açıklanmaktadır:

- Çoğu tipik performans gereksinimlerine sahip iş kolu uygulamaları için Tasarım
- Yüksek performanslı Azure Premium depolama (8 TB)
- Yerleşik [yüksek kullanılabilirlik](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) güvenilir Azure Premium depolama tabanlı ve [Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Daha fazla bilgi için [depolama katmanı genel amaçlı katmanında](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) ve [en iyi performans uygulamaları, depolama ve Azure SQL DB yönetilen örneği (genel amaçlı) değerlendirmeleri](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/).

Hizmet katmanlarında arasındaki fark hakkında daha fazla bilgi [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier-preview"></a>İş kritik hizmet Katmanı (Önizleme)

İş kritik hizmet katmanı, yüksek GÇ gereksinimleri olan uygulamalar için tasarlanmıştır. Çeşitli yalıtılmış çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sunar.

Aşağıdaki listede, iş açısından kritik hizmet katmanının anahtar özellikleri özetlenmektedir:

- En yüksek performans ve yüksek kullanılabilirlik gereksinimleri olan iş uygulamaları için tasarlanmış
- Süper hızlı SSD depolama ile birlikte gelir (1 TB'a kadar 5. nesil Gen 4 ve 4 TB)
- Yerleşik [yüksek kullanılabilirlik](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) göre [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) ve [Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- Yerleşik ek [salt okunur veritabanı çoğaltmasını](sql-database-read-scale-out.md) kullanılabilecek raporlama ve diğer salt okunur iş yükleri için
- [Bellek içi OLTP](sql-database-in-memory.md) yüksek performans gereksinimlerine sahip iş yükü için kullanılabilir  

> [!IMPORTANT]
> **İş açısından kritik** hizmet katmanı önizlemededir.

Hizmet katmanlarında arasındaki fark hakkında daha fazla bilgi [yönetilen örnek kaynak sınırları](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk

Azure SQL veritabanı yönetilen örneği, Azure Bulut ve SQL Server veritabanı altyapısı tarafından sağlanan gelişmiş güvenlik özellikleri birleştirir.

### <a name="managed-instance-security-isolation-in-azure-cloud"></a>Örnek güvenlik yalıtımı Azure bulutunda yönetilen

Yönetilen örnek, diğer kiracıların Azure bulutunda ek güvenlik yalıtımı sağlar. Güvenlik yalıtımı içerir:

- [Yerel sanal ağ uygulaması](sql-database-managed-instance-vnet-configuration.md) ve Azure Express Route veya VPN ağ geçidi kullanarak şirket içi ortamınıza bağlantı
- Özel Azure ya da karma ağları güvenli bağlantı verme yalnızca özel bir IP adresi üzerinden sunulan SQL uç noktası
- Tek kiracılı ayrılmış temel alınan altyapı (bilgi işlem, depolama)

Aşağıdaki diyagramda, uygulamalarınız için çeşitli bağlantı seçenekleri özetlenmektedir:

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

VNet tümleştirmesi ve alt ağ düzeyinde ilke zorlaması ağ hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma](sql-database-managed-instance-vnet-configuration.md) ve [uygulamanızı Azure SQL veritabanına bağlanma Yönetilen örnek](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Güvenlik gereksinimlerinize göre izin, ek avantajlar getirecek şekilde her yerde aynı alt ağdaki birden çok yönetilen örnek yerleştirin. Aynı alt ağdaki collocating örnekleri, önemli ölçüde ağ altyapı Bakımı kolaylaştırabilir ve uzun süre sağlama bir alt ağdaki ilk dağıtımı yönetilen örnek maliyeti ile ilişkili olduğundan, zaman, sağlama örneği azaltın.

### <a name="azure-sql-database-security-features"></a>Azure SQL veritabanı güvenlik özellikleri

Azure SQL veritabanı, verilerinizi korumak için kullanılan gelişmiş güvenlik özellikleri sunmaktadır.

- [Yönetilen örnek denetim](sql-database-managed-instance-auditing.md) veritabanı olaylarını izler ve Azure depolama hesabınızdaki yerleştirilen bir denetim günlük dosyasına yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olabilir.
- Veri şifreleme Hareket halindeki - yönetilen örnek, Aktarım Katmanı Güvenliği kullanarak Hareket halindeki veriler için şifreleme sağlayarak verilerinizin güvenliğini sağlar. Aktarım Katmanı Güvenliği yanı sıra SQL veritabanı yönetilen örneği uçuş, bekleme sırasında ve sorgu ile işleme sırasında hassas verileri koruma sunar. [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted, kritik öneme sahip verilerin çalınması gibi güvenlik ihlallerine karşı sektörde benzersiz koruma sağlayan bir özelliktir. Örneğin, Always Encrypted ile kredi kartı numaraları veritabanında her zaman, hatta işleme, yetkili personel veya bu veriyi işlemek için gereken uygulamalar tarafından kullanımı dahil izin vererek sorgu sırasında şifrelenmiş olarak depolanır.
- [Tehdit algılama](sql-database-managed-instance-threat-detection.md) tamamlar [yönetilen örneği denetim](sql-database-managed-instance-auditing.md) ek bir güvenlik katmanı sağlayarak erişmek veya yararlanmak sıra dışı ve zararlı algılar hizmette yerleşik zeka çalışır veritabanları. Şüpheli etkinlikler, potansiyel açıklar, hakkında uyarı almanız ve anormal veritabanı erişim modellerinin yanı sıra SQL ekleme saldırıları. Tehdit algılama uyarıları görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) şüpheli etkinlik ayrıntılarını sağlayın ve eylemi araştırmak ve tehdidi azaltmak için önerilir.  
- [Dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking) hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıklı olmayan kullanıcılara gizleyerek sınırlar. Dinamik veri maskeleme, hassas verileri uygulama katmanı üzerinde en az etki ile ortaya çıkarmak için ne kadarının atamak sağlayarak hassas verilere yetkisiz erişimi önlemeye yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir.
- [Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) denetleme bir veritabanı tablosundaki satırlara erişimi etkinleştirir, sorguyu yürüten kullanıcının özelliklerine temel (gibi Grup üyeliği veya yürütme bağlamı olarak). Satır düzeyi güvenlik (RLS), uygulamanızın güvenlik tasarımını ve kodlama aşamasını kolaylaştırır. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin, çalışanlar departmanı veya ilgili veriler yalnızca bir veri erişimi kısıtlamak için uygun olan veri satırları eriştiğinden emin olun.
- [Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) bekleyen verileri şifreleme olarak bilinen Azure SQL veritabanı yönetilen örneği'nın veri dosyaları şifreler. TDE, gerçek zamanlı g/ç şifreleme ve şifre çözme veri ve günlük dosyalarının gerçekleştirir. Şifreleme, Kurtarma sırasında kullanılabilirlik veritabanı önyükleme kaydında depolanan bir veritabanı şifreleme anahtarı (DEK) kullanır. Tüm veritabanlarınızın yönetilen örneğinde saydam veri şifrelemesi ile koruyabilirsiniz. TDE, SQL'in depolama medyalarını hırsızlıktan koruma amacıyla birçok uyumluluk standardı tarafından talep edilen bekleme sırasında şifreleme teknolojisidir. Genel Önizleme sırasında otomatik anahtar yönetimi modeli (PaaS platform tarafından gerçekleştirilir) desteklenir.

Geçiş şifrelenmiş bir veritabanının SQL yönetilen örneği için Azure veritabanı geçiş hizmeti (DMS) veya yerel bir geri yükleme aracılığıyla desteklenir. Şifrelenmiş veritabanı yerel geri yükleme kullanarak geçirmeyi planlıyorsanız, SQL Server şirket içi veya SQL Server VM mevcut TDE sertifikanın yönetilen örneğe geçiş gerekli bir adımdır. Geçiş seçenekleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği SQL Server örneği geçiş](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Azure Active Directory Tümleştirmesi

Azure SQL veritabanı yönetilen örneği, geleneksel SQL server veritabanı altyapısı oturumları ve Azure Active Directory (AAD) ile tümleşik oturum açma bilgileri destekler. AAD, Azure bulut sürümü, şirket içi ortamınızda kullandığınız Windows veritabanı oturumu açmaları oturumlardır.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması

Yönetilen örnek veritabanı kullanıcısı ve diğer Microsoft hizmetleriyle kimliklerini merkezi olarak yönetmenize imkan sağlar [Azure Active Directory Tümleştirmesi](sql-database-aad-authentication.md). Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md) (MFA) ve çoklu oturum açma işlemini destekler.

### <a name="authentication"></a>Kimlik Doğrulaması

Yönetilen örnek nasıl kullanıcıların kimliklerini veritabanına bağlanırken kanıtlamak için kimlik doğrulaması belirtir. SQL Veritabanı iki kimlik doğrulaması türünü destekler:  

- **SQL kimlik doğrulaması**:

  Bu kimlik doğrulama yöntemi, bir kullanıcı adı ve parola kullanır.
- **Azure Active Directory kimlik doğrulaması**:

  Bu kimlik doğrulama yöntemi, Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenir. [Mümkün olduğunda](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) Active Directory kimlik doğrulamasını (tümleşik güvenlik) kullanın.

### <a name="authorization"></a>Yetkilendirme

Yetkilendirme hangi kullanıcının bir Azure SQL veritabanında yapabilir ve kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri ile denetlenir ifade eder. Yönetilen örnek, SQL Server 2017 ile aynı yetkilendirme özellikleri vardır.

## <a name="database-migration"></a>Veritabanı geçişi

Örnek hedefleri kullanıcı senaryoları yığın veritabanı geçişi ile şirket içi veya Iaas veritabanı uygulamaları yönetilen. Örnek destekler, çeşitli veritabanı geçiş seçenekleri yönetilen:

### <a name="backup-and-restore"></a>Yedekleme ve geri yükleme  

Azure blob depolama için yedeklemeleri SQL geçiş yaklaşımı yararlanır. Azure depolama blobu'nda depolanan yedeklemeler doğrudan geri yükleyebilirsiniz yönetilen örneğe kullanarak [T-SQL RESTORE komutunu](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current).

- Wide World Importers - standart veritabanı yedek dosyasını geri yükleme işlemini gösteren Hızlı Başlangıç için bkz. [yönetilen örneğe yedek dosyayı geri](sql-database-managed-instance-get-started-restore.md). Bu hızlı başlangıçta, Azure Web günlüğü depolama ve güvenli bir paylaşılan erişim imzası (SAS) anahtarı kullanarak bir yedekleme dosyası karşıya yüklemeniz gösterilmektedir.
- URL'den geri yükleme hakkında daha fazla bilgi için bkz. [yerel geri URL'den](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Bir yönetilen örnek yedeklerden yalnızca başka bir yönetilen örneğe geri yüklenebilir. Bunlar, bir şirket içi SQL Server'a veya tek veya havuza alınmış Azure SQL veritabanı mantıksal sunucusu veritabanına geri yüklenemez.

### <a name="data-migration-service"></a>Veri geçiş hizmeti

Azure veritabanı geçiş hizmeti, birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure Data platformlarına sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmeti mevcut üçüncü taraf ve SQL Server veritabanlarını Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri Azure VM'de genel Önizleme sırasında Azure SQL veritabanı yönetilen örneği ve SQL Server'ı içerir. Bkz: [DMS kullanarak yönetilen örneği'ne şirket içi veritabanınızı geçirme](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri

Hizmet genel kullanıma kadar aşamalı olarak kullanıma sunulacak olan şirket içi SQL Server ile % 100 yüzey alanını uyumluluk yakın sunmak için örnek aıms yönetilen. Bir özellik için ve karşılaştırma listesini görmek [SQL veritabanı özellik karşılaştırması](sql-database-features.md)ve yerine SQL Server yönetilen örneğinde T-SQL farklılıkları listesi için bkz. [SQLServer'danyönetilenörnekT-SQLfarklılıkları](sql-database-managed-instance-transact-sql-information.md).

Örnek destekleyen geriye dönük uyumluluk için SQL 2008 veritabanı yönetilir. SQL 2005 veritabanı sunucularından doğrudan geçiş desteklenir, uyumluluk düzeyi geçirilen SQL 2005 veritabanları için SQL 2008'e güncelleştirilmiştir.
  
Aşağıdaki diyagramda, yönetilen örnek'te yüzey alanını uyumluluğu özetlenmektedir:  

![Geçiş](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Şirket içi SQL Server yönetilen örneği arasındaki temel farklılıklar

Yönetilen örnek avantajları engeller her zaman yukarı-başından bu yana bulutta, şirket içi SQL Server'daki bazı özellikleri ya da olabilir anlamına gelir eski, devre dışı bırakılan veya seçeneğiniz vardır. Araçları tam olarak kontrol edebilirim bir ortamda hizmeti çalışmıyor veya belirli bir özellik biraz farklı bir şekilde çalıştığını düzenlemeniz gerekirse, özel durumlar vardır:

- Yüksek kullanılabilirlik yerleşiktir ve benzer şekilde teknolojisini kullanarak, önceden yapılandırılmış [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).
- Otomatik yedeklemeler ve noktaya geri yükleme noktası. Müşteri başlatabilir `copy-only` ile otomatik yedekleme zinciri müdahale etmez yedekler.
- Yönetilen örnek izin vermiyor karşılık gelen tüm senaryolar farklı desteklenen zorunda tam fiziksel yolu belirtme: geri DB WITH MOVE desteklemiyor, toplu ekleme çalışır ile Azure BLOB'ları yalnızca, vb. DB oluşturma fiziksel yollar izin vermez.
- Örnek destekleyen yönetilen [Azure AD kimlik doğrulaması](sql-database-aad-authentication.md) bulut alternatif Windows kimlik doğrulaması olarak.
- Yönetilen örnek XTP dosyası grubu ve bellek içi OLTP nesnelerini içeren veritabanları için dosyaları otomatik olarak yönetir
- Yönetilen örneği SQL Server Integration Services (SSIS) destekler ve SSIS kataloğunu (SSISDB) SSIS paketlerini depolar barındırabilirsiniz, ancak bunlar üzerinde bir yönetilen Azure-SSIS Integration Runtime (IR) Azure Data Factory (ADF) yürütülür, bkz: [oluştur Azure-SSIS IR ADF içinde](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). SQL veritabanı yönetilen örneği, SSIS özellikleri karşılaştırmak için bkz [karşılaştırma SQL veritabanı mantıksal sunucusu ve yönetilen örneği](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance).

### <a name="managed-instance-administration-features"></a>Yönetilen örnek yönetim özellikleri

Örnek etkinleştirme Sistem Yöneticisi, iş için en önemli şeylere odaklanmak için yönetilen. Birçok sistem yönetici/DBA etkinlikleri gerekli değildir veya bunların basittir. Örneğin, işletim sistemi / RDBMS yükleme ve düzeltme eki uygulama, dinamik boyutlandırma ve yapılandırması, yedekleme, örnek [veritabanı çoğaltması](replication-with-sql-database-managed-instance.md) (sistem veritabanları dahil olmak üzere), yüksek kullanılabilirlik yapılandırması ve yapılandırma sistem durumu ve performans veri akışları.

> [!IMPORTANT]
> Desteklenen, kısmen desteklenen ve desteklenmeyen özelliklerin bir listesi için bkz. [SQL veritabanı özellikleri](sql-database-features.md). SQL Server yerine yönetilen örneğinde T-SQL farklılıkları listesi için bkz [SQL Server'dan yönetilen örnek T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Program aracılığıyla yönetilen örneğe belirleme

Aşağıdaki tabloda, yönetilen örnek sayesinde, uygulamanızın çalıştığını algılamak için kullanmak ve önemli özelliklerini almak Transact SQL erişilebilir çeşitli özellikleri gösterir.

|Özellik|Değer|Açıklama|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 2018-03-07 12.0.2000.8 Telif Hakkı (C) 2018 Microsoft Corporation.|Bu değer, SQL veritabanı olduğu gibi aynı olur.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Bu değer, SQL veritabanı olduğu gibi aynı olur.|
|`SERVERPROPERTY('EngineEdition')`|8|Bu değer, yönetilen örneğe benzersiz olarak tanımlar.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Tam örnek DNS adı şu biçimde:`<instanceName>`.`<dnsPrefix>`.Database.Windows.NET, burada `<instanceName>` müşteri tarafından sağlanan ad sırada `<dnsPrefix>` Genel DNS adı benzersizliği garanti etme adı otomatik olarak oluşturulan parçası ("wcus17662feb9ce98", örneğin)|Örnek: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Sonraki adımlar

- İlk yönetilen örneğinizi oluşturma konusunda bilgi almak için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-managed-instance-get-started.md).
- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
- Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Yönetilen Örnek Sanal Ağ Yapılandırması](sql-database-managed-instance-vnet-configuration.md).
- Yönetilen bir örneğini oluşturur ve bir veritabanı yedekleme dosyasından geri yükleyen bir hızlı başlangıç için bkz: [bir yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md).
- Fiyatlandırma bilgileri için bkz: [SQL veritabanı yönetilen örneği fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/).
