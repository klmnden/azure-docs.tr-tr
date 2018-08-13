---
title: Azure SQL veritabanı yönetilen örneği'ne genel bakış | Microsoft Docs
description: Bu konu Azure SQL veritabanı yönetilen örneği ve nasıl çalıştığını ve nasıl tek bir veritabanının Azure SQL veritabanı'nda farklı olduğunu açıklar.
services: sql-database
author: bonova
ms.reviewer: carlrab
manager: craigg
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 08/01/2018
ms.author: bonova
ms.openlocfilehash: edacb9fe1d09a4e775f8f7107dfa4d9810f53f07
ms.sourcegitcommit: d0ea925701e72755d0b62a903d4334a3980f2149
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2018
ms.locfileid: "40006053"
---
# <a name="what-is-a-managed-instance-preview"></a>Yönetilen örnek (Önizleme) nedir?

Azure SQL veritabanı yönetilen örneği (Önizleme) yerel sağlayan % 100 uyum SQL Server şirket içi (Enterprise Edition) sağlayarak Azure SQL veritabanı'nın yeni bir özellik olan [sanal ağ (VNet)](../virtual-network/virtual-networks-overview.md) Ortak Güvenlik endişelerini ortadan uygulama ve [iş modeli](https://azure.microsoft.com/pricing/details/sql-database/) şirket içi SQL Server müşterileri için yeterli. Yönetilen örnek lift- and -shift kendi şirket içi uygulamaları bulutta çok az değişiklikle uygulama ve veritabanı mevcut SQL Server müşterileri sağlar. Yönetilen örneği, aynı zamanda, yönetim yükünü ve toplam sahip olma Maliyetini önemli ölçüde azaltan tüm PaaS özellikleri (otomatik düzeltme eki uygulama ve sürüm güncelleştirmeleri, yedekleme, yüksek kullanılabilirlik) korur.

> [!IMPORTANT]
> Yönetilen Örneğin şu anda kullanılabilir olduğu bölgelerin listesi için bkz. [Azure SQL Veritabanı Yönetilen Örneği ile tam yönetilen hizmete veritabanlarınızı geçirme](https://azure.microsoft.com/blog/migrate-your-databases-to-a-fully-managed-service-with-azure-sql-database-managed-instance/).
 
Aşağıdaki diyagramda, yönetilen örnek temel özellikleri özetlenmektedir:

![anahtar özellikleri](./media/sql-database-managed-instance/key-features.png)

Yönetilen örnek, aşağıdaki senaryolar için tercih edilen platformu olarak envisioned: 

- Şirket içi SQL Server / değişiklikleri en düşük ile tam olarak yönetilen bir hizmet uygulamalarını taşımayı planlıyorsanız Iaas müşteriler tasarlayın.
- Buluta geçiş ve bu nedenle önemli bir rekabet avantajı elde etmek, müşterilerine isteyen SQL veritabanları, bağlı ISV'ler etkinleştirin veya küresel pazara ulaşın. 

Genel kullanılabilirlik tarafından hazırlanmış yayın planı aracılığıyla en son şirket içi SQL Server sürümü ile % 100 yüzey alanını uyumluluk yakın sunmak için yönetilen örneği amaçlar. 

Aşağıdaki tabloda ana hatlarını anahtarının farklar ve kullanım senaryoları arasında SQL Iaas, Azure SQL veritabanı ve SQL veritabanı yönetilen örneği envisioned:

| | Kullanım senaryosu | 
| --- | --- | 
|SQL Veritabanı Yönetilen Örneği |Şirket içi veya şirket içinde oluşturulmuş, Iaas veya sağlanan ISV çok sayıda uygulamaları geçirmek isteyen müşteriler için ile mümkün olduğunca az geçiş çaba olarak yönetilen örneği önerin. Tam otomatik kullanarak [veri geçiş hizmeti (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance) Azure'da müşteriler kaldırma ve kendi şirket içi SQL Server şirket içi SQL Server ve tam bir yalıtım olanağı ile uyumluluk sağlayan yönetilen örneğe kaydırma yerel sanal ağ desteği olan müşteri örnekleri.  Yazılım Güvencesi içeren mevcut lisanslarını kullanarak bir SQL veritabanı yönetilen örneği üzerinde indirimli fiyatlar için exchange [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  SQL veritabanı yönetilen örneği, yüksek güvenlik ve zengin programlama yüzeyini gerektiren SQL Server örnekleri için bulutta en iyi geçiş hedef ' dir. |
|Azure SQL veritabanı (tek veya havuz) |**Elastik havuzlar**: yeni SaaS çok kiracılı uygulamalar geliştiriyorum veya kasıtlı olarak var olan dönüştürme müşterilerin uygulamaları bir çok kiracılı SaaS uygulamasında oturum şirket için elastik havuzlar teklif. Bu modelin avantajlarından şunlardır: <br><ul><li>İş modeli hizmeti abonelikleri (ISV'ler için) satış için lisans satış öğesinden dönüştürme</li></ul><ul><li>Kolay ve madde işareti kavram Kiracı yalıtımı</li></ul><ul><li>Basitleştirilmiş bir veritabanı odaklı programlama modeli</li></ul><ul><li>Olası sabit bir tavan ulaşmaktan olmadan ölçeği genişletme</li></ul>**Tek veritabanları**: dışındaki iş yükü, kararlı ve öngörülebilir, çok kiracılı SaaS, yeni uygulama geliştirme müşteriler tek veritabanları önermek için. Bu modelin avantajlarından şunlardır:<ul><li>Basitleştirilmiş bir veritabanı odaklı programlama modeli</li></ul>  <ul><li>Her veritabanı için öngörülebilir performans</li></ul>|
|SQL Iaas sanal makine|SQL Vm'leri işletim sistemini veya veritabanı sunucusu, hem de (aynı VM'de), SQL Server ile yan yana çalışan üçüncü taraf uygulamalar açısından belirli gereksinimleri olan müşterilerin özelleştirmek ihtiyaç duyan müşteriler önermek için / Iaas en iyi çözüm olarak|
|||

## <a name="how-to-programmatically-identify-a-managed-instance"></a>Program aracılığıyla yönetilen örneğe belirleme

Aşağıdaki tabloda, yönetilen örnek sayesinde, uygulamanızın çalıştığını algılamak için kullanmak ve önemli özelliklerini almak Transact SQL erişilebilir çeşitli özellikleri gösterir.

|Özellik|Değer|Açıklama|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 2018-03-07 12.0.2000.8 Telif Hakkı (C) 2018 Microsoft Corporation.|Bu değer, SQL veritabanı olduğu gibi aynı olur.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Bu değer, SQL veritabanı olduğu gibi aynı olur.|
|`SERVERPROPERTY('EngineEdition')`|8|Bu değer, yönetilen örneğe benzersiz olarak tanımlar.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Tam örnek DNS adı şu biçimde:<instanceName>.<dnsPrefix>. Database.Windows.NET, burada <instanceName> müşteri tarafından sağlanan ad sırada <dnsPrefix> Genel DNS adı benzersizliği güvence altına almak adı otomatik olarak oluşturulan parçası ("wcus17662feb9ce98", örneğin)|Örnek: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="key-features-and-capabilities-of-a-managed-instance"></a>Anahtar özellikleri ve yetenekleri bir yönetilen örnek 

> [!IMPORTANT]
> Bir yönetilen örnek tüm özellikler çevrimiçi işlemleri, otomatik plan düzeltme ve diğer kurumsal performans geliştirmeleri dahil olmak üzere SQL Server'ın en son sürümü ile çalışır. 

| **PaaS avantajları** | **İş sürekliliği** |
| --- | --- |
|Donanım satın alma ve Yönetimi <br>Temel altyapıyı yönetmek için ek yükü yönetimi yok <br>Hızlı sağlama ve hizmet ölçeklendirme <br>Otomatik düzeltme eki uygulama ve sürüm yükseltme <br>Diğer PaaS Veri Hizmetleri ile tümleştirme |% 99,99 çalışma süresi SLA'sı  <br>Yerleşik yüksek kullanılabilirlik <br>Otomatik yedekleme işlemleriyle korunan verileri <br>Müşteri yapılandırılabilir yedekleme bekletme süresi (genel önizlemede 7 gün için sabit) <br>Kullanıcı tarafından başlatılan bir yedekleme <br>Veritabanı zaman içinde bir noktaya geri yükleme özelliği |
|**Güvenlik ve uyumluluk** | **Yönetim**|
|Yalıtılmış ortamı (sanal ağ tümleştirmesi, tek kiracılı bir hizmet, adanmış işlem ve depolama) <br>Saydam veri şifrelemesi<br>Azure AD kimlik doğrulaması, çoklu oturum açma desteği <br>Azure SQL veritabanı olarak aynı uyumluluk standartlarına uyar <br>SQL denetimi <br>Tehdit algılama |Hizmet sağlama ve ölçeklendirme otomatikleştirmek için Azure Resource Manager API'si <br>Sağlama ve ölçeklendirme el ile hizmeti için Azure portal işlevi <br>Veri geçiş hizmeti 

![Çoklu oturum açma](./media/sql-database-managed-instance/sso.png) 

## <a name="vcore-based-purchasing-model"></a>Sanal çekirdek tabanlı satın alma modeli

Sanal çekirdek tabanlı satın alma modeli, esneklik, denetimi, saydamlık sağlar ve basit bir yol çevirmek için şirket iş yükü gereksinimlerini buluta. Bu model, Ölçek işlem, bellek ve iş yükü gereksinimlerine göre depolama sağlar. VCore modeli de ile yüzde 30 tasarruf için uygun yedekleme [SQL Server için Azure hibrit kullanım teklifi](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

Sanal çekirdek, donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder.
- 4. Nesil Mantıksal CPU’lar Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri temel alır.
- 5 mantıksal CPU'lar Intel E5-2673 v4 nesil (Broadwell) 2,3 GHz işlemcileri.

Aşağıdaki tabloda, işlem, bellek, depolama ve g/ç kaynakları en iyi yapılandırmasının nasıl seçileceğini anlamanıza yardımcı olur.

||4. Nesil|5. Nesil|
|----|------|-----|
|Donanım|Intel E5-2673 v3 (Haswell) 2,4 GHz işlemcileri, bağlı SSD sanal çekirdek = 1 PP (fiziksel çekirdek)|Intel E5-2673 v4 (Broadwell) 2,3 GHz işlemcileri, hızlı eNVM SSD, sanal çekirdek = 1 LP (hiper iş parçacığı)|
|Performans seviyeleri|8, 16, 24 sanal çekirdek|8, 16, 24, 32, 40, 64, 80 sanal çekirdekler|
|Bellek|Sanal çekirdek başına 7 GB|Sanal çekirdek başına 5.5 GB|
||||

## <a name="managed-instance-service-tiers"></a>Yönetilen örnek hizmet katmanları

Yönetilen örnek, iki hizmet katmanlarda kullanılabilir:
- **Genel amaçlı**: tipik kullanılabilirlik ve ortak g/ç gecikme süresi gereksinimlerine uygulamalar için tasarlandı.
- **İş açısından kritik**: yüksek kullanılabilirlik ve düşük g/ç gecikme süresi gereksinimlerine uygulamalar için tasarlanmıştır.
 
> [!IMPORTANT]
> Genel amaçlı iş açısından kritik veya tam tersi hizmet katmanınızın değiştirirken, genel Önizleme sürümünde desteklenmiyor. Farklı hizmet katmanında bir örnek için veritabanlarınızı geçirmek istiyorsanız, yeni bir örnek oluşturun, özgün örneğinden noktaya geri yükleme noktası ile veritabanlarını geri yükleme ve artık gerekli değilse özgün örneğe ardından bırakın. 

### <a name="general-purpose-service-tier"></a>Genel amaçlı hizmet katmanı

Aşağıdaki listede, genel amaçlı hizmet katmanının önemli bir özelliği açıklanmaktadır: 

- Çoğu tipik performans ve yüksek kullanılabilirlik gereksinimleri olan iş kolu uygulamaları için Tasarım 
- Yüksek performanslı Azure Premium depolama (8 TB) 
- 100 veritabanının / örnek 

Bu katmanda bağımsız olarak depolama seçebilir ve hesaplama kapasitesi. 

Aşağıdaki diyagram, aktif işlem ve bu hizmet katmanında yedekli düğümleri gösterir.
 
![Genel amaçlı hizmet katmanı](./media/sql-database-managed-instance/general-purpose-service-tier.png) 

Aşağıdaki listede, genel amaçlı hizmet katmanının anahtar özellikleri özetlenmektedir:

|Özellik | Açıklama|
|---|---|
| Sanal çekirdekler * sayısı | 8, 16, 24 (gen 4)<br>8, 16, 24, 32, 40, 64, 80 (gen 5)|
| SQL Server sürümü / build | SQL Server Son (kullanılabilir) |
| En düşük depolama boyutu | 32 GB |
| En büyük depolama boyutu | 8 TB |
| Veritabanı başına maks. depolama | Örnek başına en fazla depolama boyutu tarafından belirlenir. |
| Beklenen depolama IOPS | Veri dosyası (veri dosyası üzerinde bağlıdır) başına 500-7500 IOPS. Bkz: [Premium depolama](../virtual-machines/windows/premium-storage-performance.md#premium-storage-disk-sizes) |
| Veritabanı başına veri dosyalarının (satırlar) | Birden çok | 
| Günlük dosyası (günlük) veritabanı başına sayısı | 1 | 
| Yönetilen otomatik yedekleri | Evet |
| YÜKSEK KULLANILABİLİRLİK | Uzak depolama alanına dayalı olarak ve [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Yerleşik bir örneği ve veritabanı izleme ve ölçümler | Evet |
| Otomatik yazılım düzeltme eki uygulama | Evet |
| VNet - Azure Resource Manager dağıtımı | Evet |
| VNet - Klasik dağıtım modeli | Hayır |
| Portalı desteği | Evet|
|||

\* Sanal çekirdek, donanım Nesilleri arasında seçim yapma olanağı ile sunulan mantıksal CPU'yu temsil eder. Gen 4 mantıksal CPU'lar Intel E5-2673 v3 dayalı (Haswell) 2,4 GHz işlemcileri ve genel 5 mantıksal CPU'lar Intel E5-2673 v4 dayalı (Broadwell) 2,3 GHz işlemcileri. 

### <a name="business-critical-service-tier"></a>İş kritik hizmet katmanı

İş kritik hizmet katmanı, yüksek GÇ gereksinimleri olan uygulamalar için tasarlanmıştır. Çeşitli yalıtılmış Always On çoğaltmaları kullanarak hatalara karşı en yüksek düzeyde dayanıklılık sunar. Aşağıdaki diyagramda, bu hizmet katmanında temel mimarisi gösterilmektedir:

![İş kritik hizmet katmanı](./media/sql-database-managed-instance/business-critical-service-tier.png)  

Aşağıdaki listede, iş açısından kritik hizmet katmanının anahtar özellikleri özetlenmektedir: 
-   En yüksek performans ve yüksek kullanılabilirlik gereksinimleri olan iş uygulamaları için tasarlanmış 
-   Süper hızlı SSD depolama ile birlikte gelir (1 TB'a kadar 5. nesil Gen 4 ve 4 TB)
-   Örnek başına en fazla 100 veritabanlarını destekler 

|Özellik | Açıklama|
|---|---|
| Sanal çekirdekler * sayısı | 8, 16, 24, 32 (gen 4)<br>8, 16, 24, 32, 40, 64, 80 (gen 5)|
| SQL Server sürümü / build | SQL Server Son (kullanılabilir) |
| Ek Özellikler | [Bellek içi OLTP](sql-database-in-memory.md)<br> 1 ek salt okunur çoğaltma ([okuma ölçeği genişletme](sql-database-read-scale-out.md))
| En düşük depolama boyutu | 32 GB |
| En büyük depolama boyutu | Gen 4: 1 TB (tüm sanal çekirdek boyutları<br> 5. nesil:<ul><li>1 TB 8, 16 sanal çekirdek</li><li>24 sanal çekirdekler için 2 TB</li><li>4 TB 32, 40, 64, 80 sanal çekirdekler</ul>|
| Veritabanı başına maks. depolama | Örnek başına en fazla depolama boyutu tarafından belirlenir. |
| Veritabanı başına veri dosyalarının (satırlar) | Birden çok | 
| Günlük dosyası (günlük) veritabanı başına sayısı | 1 | 
| Yönetilen otomatik yedekleri | Evet |
| YÜKSEK KULLANILABİLİRLİK | Temel [Always On kullanılabilirlik grupları](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) ve [Azure Service Fabric](../service-fabric/service-fabric-overview.md) |
| Yerleşik bir örneği ve veritabanı izleme ve ölçümler | Evet |
| Otomatik yazılım düzeltme eki uygulama | Evet |
| VNet - Azure Resource Manager dağıtımı | Evet |
| VNet - Klasik dağıtım modeli | Hayır |
| Portalı desteği | Evet|
|||

## <a name="advanced-security-and-compliance"></a>Gelişmiş koruma ve uyumluluk 

### <a name="managed-instance-security-isolation"></a>Yönetilen örnek güvenlik yalıtımı 

Yönetilen örnek, diğer kiracıların Azure bulutunda ek güvenlik yalıtımı sağlar. Güvenlik yalıtımı içerir: 

- [Yerel sanal ağ uygulaması](sql-database-managed-instance-vnet-configuration.md) ve Azure Express Route veya VPN ağ geçidi kullanarak şirket içi ortamınıza bağlantı 
- Özel Azure ya da karma ağları güvenli bağlantı verme yalnızca özel bir IP adresi üzerinden sunulan SQL uç noktası
- Tek kiracılı ayrılmış temel alınan altyapı (bilgi işlem, depolama)

Aşağıdaki diyagramda, uygulamalarınız için çeşitli bağlantı seçenekleri özetlenmektedir: 

![yüksek kullanılabilirlik](./media/sql-database-managed-instance/application-deployment-topologies.png)  

VNet tümleştirmesi ve alt ağ düzeyinde ilke enforcements ağ hakkında daha fazla bilgi edinmek için bkz. [Azure SQL veritabanı yönetilen örneği için bir sanal ağ yapılandırma](sql-database-managed-instance-vnet-configuration.md) ve [uygulamanızı Azure SQL veritabanına bağlanma Yönetilen örnek](sql-database-managed-instance-connect-app.md). 

> [!IMPORTANT]
> Güvenlik gereksinimlerinize göre izin, ek avantajlar getirecek şekilde her yerde aynı alt ağdaki birden çok yönetilen örnek yerleştirin. Aynı alt ağdaki collocating örnekleri, önemli ölçüde ağ altyapı Bakımı kolaylaştırabilir ve uzun süre sağlama bir alt ağdaki ilk dağıtımı yönetilen örnek maliyeti ile ilişkili olduğundan, zaman, sağlama örneği azaltın.


### <a name="auditing-for-compliance-and-security"></a>Uyumluluk ve güvenlik denetimi 

[Yönetilen örnek denetim](sql-database-managed-instance-auditing.md) veritabanı olaylarını izler ve bir denetim günlüğüne Azure depolama hesabınızdaki yazar. Denetim mevzuatla uyumluluk, veritabanı etkinliğini anlama ve tutarsızlıklar ve işletme sorunlarını veya şüpheli güvenlik ihlallerini anomalileri kavramanıza yardımcı olabilir. 

### <a name="data-encryption-in-motion"></a>Hareket halinde veri şifrelemesi 

Yönetilen örnek, Aktarım Katmanı Güvenliği kullanarak Hareket halindeki veriler için şifreleme sağlayarak verilerinizin güvenliğini sağlar.

Aktarım Katmanı Güvenliği yanı sıra SQL veritabanı yönetilen örneği uçuş, bekleme sırasında ve sorgu ile işleme sırasında hassas verileri koruma sunar. [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted, kritik öneme sahip verilerin çalınması gibi güvenlik ihlallerine karşı sektörde benzersiz koruma sağlayan bir özelliktir. Örneğin, Always Encrypted ile kredi kartı numaraları veritabanında her zaman, hatta işleme, yetkili personel veya bu veriyi işlemek için gereken uygulamalar tarafından kullanımı dahil izin vererek sorgu sırasında şifrelenmiş olarak depolanır. 

### <a name="data-encryption-at-rest"></a>Bekleme sırasında veri şifrelemesi 
[Saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) bekleyen verileri şifreleme olarak bilinen Azure SQL yönetilen örnek veri dosyaları şifreler. TDE, gerçek zamanlı g/ç şifreleme ve şifre çözme veri ve günlük dosyalarının gerçekleştirir. Şifreleme, Kurtarma sırasında kullanılabilirlik veritabanı önyükleme kaydında depolanan bir veritabanı şifreleme anahtarı (DEK) kullanır. Tüm veritabanlarınızın yönetilen örneğinde saydam veri şifrelemesi ile koruyabilirsiniz. TDE, SQL'in depolama medyalarını hırsızlıktan koruma amacıyla birçok uyumluluk standardı tarafından talep edilen bekleme sırasında şifreleme teknolojisidir. Genel Önizleme sırasında otomatik anahtar yönetimi modeli (PaaS platform tarafından gerçekleştirilir) desteklenir. 

Geçiş şifrelenmiş bir veritabanının SQL yönetilen örneği için Azure veritabanı geçiş hizmeti (DMS) veya yerel bir geri yükleme aracılığıyla desteklenir. Şifrelenmiş veritabanı yerel geri yükleme kullanarak geçirmeyi planlıyorsanız, SQL Server şirket içi veya SQL Server VM mevcut TDE sertifikanın yönetilen örneğe geçiş gerekli bir adımdır. Geçiş seçenekleri hakkında daha fazla bilgi için bkz. [Azure SQL veritabanı yönetilen örneği SQL Server örneği geçiş](sql-database-managed-instance-migrate.md).

### <a name="dynamic-data-masking"></a>Dinamik veri maskeleme 

SQL veritabanı [dinamik veri maskeleme](/sql/relational-databases/security/dynamic-data-masking) hassas verilerin görünürlüğünü ayrıcalık sahibi ayrıcalıksız kullanıcılar gizleyerek sınırlar. Dinamik veri maskeleme, hassas verileri uygulama katmanı üzerinde en az etki ile ortaya çıkarmak için ne kadarının atamak sağlayarak hassas verilere yetkisiz erişimi önlemeye yardımcı olur. Bu özellik, hassas verileri belirlenen veritabanı alanlarına yapılan sorgunun sonuç kümesinde gizleyen ancak veritabanındaki verileri değiştirmeyen ilke tabanlı bir güvenlik özelliğidir. 

### <a name="row-level-security"></a>Satır düzeyi güvenlik 

[Satır düzeyi güvenlik](/sql/relational-databases/security/row-level-security) denetleme bir veritabanı tablosundaki satırlara erişimi etkinleştirir, sorguyu yürüten kullanıcının özelliklerine temel (gibi Grup üyeliği veya yürütme bağlamı olarak). Satır düzeyi güvenlik (RLS), uygulamanızın güvenlik tasarımını ve kodlama aşamasını kolaylaştırır. RLS, veri satırı erişiminde kısıtlama uygulamanızı sağlar. Örneğin, çalışanlar departmanı veya ilgili veriler yalnızca bir veri erişimi kısıtlamak için uygun olan veri satırları eriştiğinden emin olun. 

### <a name="threat-detection"></a>Tehdit algılama 

[Yönetilen örnek tehdit algılama](sql-database-managed-instance-threat-detection.md) tamamlar [yönetilen örneği denetim](sql-database-managed-instance-auditing.md) ek bir olağan dışı ve zararlı olabilecek girişimleri algılayabilen yerleşik güvenlik zekası katmanı sağlayarak veya veritabanı açıklarından yararlanmaya erişin. Şüpheli etkinlikler, potansiyel açıklar, hakkında uyarı almanız ve anormal veritabanı erişim modellerinin yanı sıra SQL ekleme saldırıları. Tehdit algılama uyarıları görüntülenebilir [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) şüpheli etkinlik ayrıntılarını sağlayın ve eylemi araştırmak ve tehdidi azaltmak için önerilir.  

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Azure Active Directory tümleştirmesi ve çok faktörlü kimlik doğrulaması 

SQL Veritabanı, [Azure Active Directory tümleştirmesi](sql-database-aad-authentication.md) ile veritabanı kullanıcısı ve diğer Microsoft hizmetleri kimliklerini bir merkezden yönetmenizi sağlar. Bu özellik, izin yönetimini kolaylaştırırken güvenliği artırır. Azure Active Directory, veri ve uygulama güvenliğini artırmak için [çok faktörlü kimlik doğrulamasını](sql-database-ssms-mfa-authentication-configure.md) (MFA) ve çoklu oturum açma işlemini destekler. 

### <a name="authentication"></a>Kimlik Doğrulaması 
SQL veritabanı kimlik doğrulaması nasıl kullanıcıların kimliklerini veritabanına bağlanırken kanıtlamak için ifade eder. SQL Veritabanı iki kimlik doğrulaması türünü destekler:  

- SQL kullanıcı adı ve parola kullanan kimlik doğrulaması.
- Azure Active Directory, Azure Active Directory tarafından yönetilen kimlikleri kullanır ve yönetilen ve tümleşik etki alanları için desteklenen kimlik doğrulaması. 

### <a name="authorization"></a>Yetkilendirme

Yetkilendirme hangi kullanıcının bir Azure SQL veritabanında yapabilir ve kullanıcı hesabınızın veritabanı rolü üyelikleri ve nesne düzeyi izinleri ile denetlenir ifade eder. Yönetilen örnek, SQL Server 2017 ile aynı yetkilendirme özellikleri vardır. 

## <a name="database-migration"></a>Veritabanı geçişi 

Örnek hedefleri kullanıcı senaryoları yığın veritabanı geçişi ile şirket içi veya Iaas veritabanı uygulamaları yönetilen. Örnek destekler, çeşitli veritabanı geçiş seçenekleri yönetilen: 

### <a name="data-migration-service"></a>Veri geçiş hizmeti

Azure veritabanı geçiş hizmeti, birden çok veritabanı kaynağını sorunsuz geçiş için en düşük kapalı kalma süresi ile Azure Data platformlarına sağlamak için tasarlanmış tam olarak yönetilen bir hizmettir. Bu hizmeti mevcut üçüncü taraf ve SQL Server veritabanlarını Azure'a taşımak için gereken görevleri kolaylaştırır. Dağıtım seçenekleri Azure VM'de genel Önizleme sırasında Azure SQL veritabanı yönetilen örneği ve SQL Server'ı içerir. Bkz: [DMS kullanarak yönetilen örneği'ne şirket içi veritabanınızı geçirme](https://aka.ms/migratetoMIusingDMS). 

### <a name="backup-and-restore"></a>Yedekleme ve geri yükleme  

Azure blob depolama için yedeklemeleri SQL geçiş yaklaşımı yararlanır. Azure depolama blobu'nda depolanan yedeklemeler doğrudan yönetilen örneğe geri yüklenebilir. Mevcut bir SQL veritabanı yönetilen örneğine geri yüklemek için aşağıdakileri yapabilirsiniz:

- Kullanım [veri geçiş hizmeti (DMS)](../dms/dms-overview.md). Bir öğretici için bkz. [Azure veritabanı geçiş hizmeti (DMS) kullanarak yönetilen örneğe geçirme](../dms/tutorial-sql-server-to-managed-instance.md) bir veritabanı yedekleme dosyasından geri yüklemek için
- Kullanım [T-SQL RESTORE komutunu](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql). 
  - Wide World Importers - standart veritabanı yedek dosyasını geri yükleme işlemini gösteren bir öğretici için bkz. [yönetilen örneğe yedek dosyayı geri](sql-database-managed-instance-restore-from-backup-tutorial.md). Bu öğretici, Azure Web günlüğü depolama ve güvenli bir paylaşılan erişim imzası (SAS) anahtarı kullanarak bir yedekleme dosyası karşıya yüklemeniz gösterir.
  - URL'den geri yükleme hakkında daha fazla bilgi için bkz. [yerel geri URL'den](sql-database-managed-instance-migrate.md#native-restore-from-url).

## <a name="sql-features-supported"></a>Desteklenen SQL özellikleri 

Hizmet genel kullanıma kadar aşamalı olarak kullanıma sunulacak olan şirket içi SQL Server ile % 100 yüzey alanını uyumluluk yakın sunmak için örnek aıms yönetilen. Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
 
Örnek destekleyen geriye dönük uyumluluk için SQL 2008 veritabanı yönetilir. SQL 2005 veritabanı sunucularından doğrudan geçiş desteklenir, uyumluluk düzeyi geçirilen SQL 2005 veritabanları için SQL 2008'e güncelleştirilmiştir. 
 
Aşağıdaki diyagramda, yönetilen örnek'te yüzey alanını uyumluluğu özetlenmektedir:  

![Geçiş](./media/sql-database-managed-instance/migration.png) 

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Şirket içi SQL Server yönetilen örneği arasındaki temel farklılıklar 

Yönetilen örnek avantajları engeller her zaman yukarı-başından bu yana bulutta, şirket içi SQL Server'daki bazı özellikleri ya da olabilir anlamına gelir eski, devre dışı bırakılan veya seçeneğiniz vardır. Araçları tam olarak kontrol edebilirim bir ortamda hizmeti çalışmıyor veya belirli bir özellik biraz farklı bir şekilde çalıştığını düzenlemeniz gerekirse, özel durumlar vardır: 

- Yüksek kullanılabilirlik yerleşik olarak ve önceden yapılandırılmış. Always On yüksek kullanılabilirlik özelliklerinden SQL Iaas uygulamalarında olduğu gibi aynı şekilde gösterilmez 
- Otomatik yedeklemeler ve noktaya geri yükleme noktası. Müşteri başlatabilir `copy-only` ile otomatik yedekleme zinciri müdahale etmez yedekler. 
- Yönetilen örnek izin vermiyor karşılık gelen tüm senaryolar farklı desteklenen zorunda tam fiziksel yolu belirtme: geri DB WITH MOVE desteklemiyor, toplu ekleme çalışır ile Azure BLOB'ları yalnızca, vb. DB oluşturma fiziksel yollar izin vermez. 
- Örnek destekleyen yönetilen [Azure AD kimlik doğrulaması](sql-database-aad-authentication.md) bulut alternatif Windows kimlik doğrulaması olarak. 
- Yönetilen örnek XTP dosyası grubu ve bellek içi OLTP nesnelerini içeren veritabanları için dosyaları otomatik olarak yönetir
- Yönetilen örneği SQL Server Integration Services (SSIS) destekler ve SSIS kataloğunu (SSISDB) SSIS paketlerini depolar barındırabilirsiniz, ancak bunlar üzerinde bir yönetilen Azure-SSIS Integration Runtime (IR) Azure Data Factory (ADF) yürütülür, bkz: [oluştur Azure-SSIS IR ADF içinde](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime). SQL veritabanı yönetilen örneği, SSIS özellikleri karşılaştırmak için bkz [karşılaştırma SQL veritabanı ve yönetilen örneği (Önizleme)](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-and-managed-instance-preview).

### <a name="managed-instance-administration-features"></a>Yönetilen örnek yönetim özellikleri  

Örnek etkinleştirme Sistem Yöneticisi, iş için en önemli şeylere odaklanmak için yönetilen. Birçok sistem yönetici/DBA etkinlikleri gerekli değildir veya bunların basittir. Örneğin, işletim sistemi / RDBMS yükleme ve düzeltme eki uygulama, dinamik örnek yeniden boyutlandırma ve yapılandırma, yedekleme, veritabanı çoğaltması (sistem veritabanları dahil olmak üzere), yüksek kullanılabilirlik yapılandırması ve sistem durumu ve performans izleme verilerini yapılandırma akışları. 

> [!IMPORTANT]
> Desteklenen, kısmen desteklenen ve desteklenmeyen özelliklerin bir listesi için bkz. [SQL veritabanı özellikleri](sql-database-features.md). SQL Server yerine yönetilen örneğinde T-SQL farklılıkları listesi için bkz [SQL Server'dan yönetilen örnek T-SQL farklılıkları](sql-database-managed-instance-transact-sql-information.md)
 
## <a name="next-steps"></a>Sonraki adımlar

- Bir özellik için ve karşılaştırma listesini görmek [SQL ortak özellikleri](sql-database-features.md).
- Sanal ağ yapılandırması hakkında daha fazla bilgi için bkz. [Yönetilen Örnek Sanal Ağ Yapılandırması](sql-database-managed-instance-vnet-configuration.md).
- Yönetilen bir örneğini oluşturur ve bir veritabanı yedekleme dosyasından geri yükleyen bir öğretici için bkz. [bir yönetilen örnek oluşturma](sql-database-managed-instance-create-tutorial-portal.md).
- Azure Veritabanı Geçiş Hizmeti’ni (DMS) geçiş için kullanmaya ilişkin bir öğretici için bkz. [DMS kullanarak Yönetilen Örnek geçişi](../dms/tutorial-sql-server-to-managed-instance.md).
- Fiyatlandırma bilgileri için bkz: [SQL veritabanı yönetilen örneği fiyatlandırma](https://azure.microsoft.com/pricing/details/sql-database/managed/).
