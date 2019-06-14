---
title: Azure güvenlik ve uyumluluk planı - Analytics FedRAMP için
description: Azure güvenlik ve uyumluluk planı - Analytics FedRAMP için
services: security
author: jomolesk
ms.assetid: b4675715-668e-4557-9c1c-698aabf62ceb
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: fa10ff14bf893c268d6b6b1a0d181d11a3f27dc4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586296"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Azure güvenlik ve uyumluluk planı: FedRAMP için analiz

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov/) bulut için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sunan bir Amerika Birleşik Devletleri devlet çapında program Ürün ve Hizmetleri. Azure güvenlik ve uyumluluk planı nasıl sağlayabileceğinizin FedRAMP yüksek denetimlerin bir alt kümesi uygulayan yardımcı olan bir Microsoft Azure analytics mimarisi için yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolu gösteren bir ortak başvuru mimarisi için dağıtım ve yapılandırma, Azure kaynaklarınızın rehberlik sağlar ve müşterilere için temel olarak görev yapar derleme ve Azure'da kendi analiz çözümleri yapılandırın.

Bu başvuru mimarisi, uygulama kılavuzları ilişkili denetim ve tehdit modelleri, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Bir uygulamaya bir değişiklik yapmadan bu ortama dağıtımı tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a FedRAMP uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, müşteriler, kendi analiz araçları oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, burada müşteriler giriş verileri toplu veri içeri aktarmaları SQL/veri Yöneticisi tarafından veya bir işletimsel kullanıcı işletimsel veri güncelleştirmelerine aracılığıyla genel kullanım örneği açıklanmaktadır. Hem iş akışlarını birleştirme [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) SQL veritabanı'na veri alma. Azure işlevleri, her müşteri için kendi benzersiz alma görevlerini işlemek için Azure Portal aracılığıyla müşteri tarafından yapılandırılmalıdır analizi gereksinimleri.

Microsoft Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak bu çözüm, Azure Analysis Services verilerine hızla göz atın ve Müşteri verilerinin daha akıllı modelleme aracılığıyla daha hızlı sonuçlar teslim etmek için Azure SQL veritabanı ile birlikte içerir. Azure Analiz Hizmetleri, machine Learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızı artırmak için hedeflenen bir biçimidir. Veriler çeşitli istatistik işlevleri aracılığıyla eğitim almış sonra en fazla 7 ek sorgu havuzları (müşteri sunucu dahil olmak üzere toplam 8) sorgu iş yükü dağıtabilir ve yanıt sürelerini kısaltmak için aynı tablosal modelleri ile eşitlenebilir.

SQL veritabanları, Gelişmiş analiz ve raporlama için columnstore dizinleri ile yapılandırılabilir. Azure Analiz Hizmetleri ve SQL veritabanlarını hem yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Verileri Azure SQL veritabanı'na yüklenen ve Azure Analysis Services tarafından geliştirilen sonra hem işletimsel kullanıcı hem de Power BI ile SQL/veri yönetici tarafından Sindirilmiş. Power BI verilerini sezgisel görüntüler ve bilgi ilişkin daha kapsamlı içgörüler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler, iş gereksinimlerini gerektirdiği gibi senaryoları çeşit işlemek için yapılandırabilir, yüksek düzeyde uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Çözümün tamamını Azure Portalı'ndan yapılandırdığınız hesabı müşteriler bir Azure depolama üzerinde oluşturulmuştur. Azure depolama, bekleyen verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler.  Coğrafi olarak yedekli depolama (GRS), ikinci bir kopyasını ayrı bir yerde mil uzaklıkta, yüzlerce depolanacağı gibi olumsuz olaya müşterinin birincil veri merkezinde veri kaybına oluşturmayacaktır sağlar.

Gelişmiş güvenlik için bu mimari Azure Active Directory ve Azure anahtar kasası ile kaynaklarını yönetir. Sistem durumu Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı ile SQL Server Management Studio (güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış yerel makineden çalışan SSMS), yaygın olarak yönetilir. **Azure başvuru mimarisi kaynak grubuna yönetimi ve veri içeri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırma önerir.**

![FedRAMP başvurusu mimari şeması için Analytics](images/fedramp-analytics-reference-architecture.png?raw=true "analizi için FedRAMP başvuru mimarisi diyagramı")

### <a name="roles"></a>Roller
Analytics şema üç genel kullanıcı türleri içeren bir senaryo açıklanmaktadır: işletimsel kullanıcı ve SQL/veri yönetim sistemleri Mühendisi. Azure rol tabanlı erişim denetimi (RBAC) yerleşik özel roller aracılığıyla tam erişim yönetimi uygulamasını sağlar. Kaynaklar yapılandırmak için kullanılabilir [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) anahat oluşturma ve uygulama [önceden tanımlı roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

#### <a name="systems-engineer"></a>Sistem Mühendisi
Sistem Mühendisi, Azure müşteri aboneliği sahibi ve Azure Portalı aracılığıyla çözümün dağıtımı yapılandırır.

#### <a name="sqldata-administrator"></a>SQL/veri Yöneticisi
SQL/veri Yöneticisi toplu veri Al işlevini ve Azure SQL veritabanı için karşıya yükleme için işletimsel verileri güncelleştirme işlevi oluşturur. SQL/veri Yöneticisi veritabanında işletimsel verileri güncelleştirmelerden sorumlu değildir ancak Power BI ile verileri görüntüleme olanağınız olacaktır.

#### <a name="operational-user"></a>İşletimsel kullanıcı
İşletimsel kullanıcı verileri düzenli olarak güncelleştirir ve günlük veri oluşturma sahip. İşletimsel kullanıcı ayrıca sonuçlarını Power BI aracılığıyla görürler.

### <a name="azure-services"></a>Azure hizmetleri

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.
- Azure İşlevleri
- Azure SQL Veritabanı
- Azure Analysis Services
- Azure Active Directory
- Azure Key Vault
- Azure İzleyici (günlük)
- Azure Storage
- ExpressRoute/VPN Gateway
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde, geliştirme ve uygulama öğeleri ayrıntıları.

![Alternatif metin](images/fedramp-analytics-components.png?raw=true "analizi için FedRAMP bileşenleri diyagramı")

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) küçük kod parçaları çoğu programlama dilinden bulutta çalıştırmaya yönelik çözümler. Bu çözümdeki işlevleri diğer Azure hizmetleriyle tümleştirme etkinleştirme müşteri verilerini buluta otomatik olarak çekmek için Azure depolama ile tümleştirin. İşlevleri kolayca ölçeklenebilir ve çalışırken yalnızca bir ücret doğurur.

**Azure Analysis Services**: [Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) kurumsal veri modelleme ve Azure veri platformu hizmetleriyle tümleştirme sağlar. Azure Analysis Services, bir tek veri modeline birden çok kaynaktan gelen verileri birleştirerek çok büyük miktarda veriyi gözatma yukarı hızlandırır.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) analytics sağlar ve müşterilere ilişkin daha kapsamlı içgörüler veri işleme çalışmalarını dışında çekme çalışılırken raporlama işlevselliği.

### <a name="networking"></a>Ağ
**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) dağıtılan kaynakları ve Hizmetleri yönlendirilen trafiği yönetmek üzere ayarlayın. Ağ güvenlik grupları varsayılan olarak reddetme düzenine ayarlanır ve yalnızca önceden yapılandırılmış erişim denetimi listesi (ACL) içinde yer alan trafiğine izin verir.

Her nsg sahip belirli bağlantı noktaları ve protokoller çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - [Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics) için tanılama günlüklerini NSG'ın bağlı.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Veri çoğaltma** Azure kamu için iki seçenek vardır [veri çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy):
 - Veri çoğaltma için varsayılan ayar **coğrafi olarak yedekli depolama (GRS)** , zaman uyumsuz olarak depolayan müşteri verilerini bir birincil bölge dışında ayrı veri merkezindeki. Bu kurtarma kaybedilmesi olay birincil veri merkezi için veri sağlar.
 - **Yerel olarak yedekli depolama (LRS)** alternatif olarak Azure depolama hesabı yapılandırılabilir. LRS, müşteri, hesabı oluşturan aynı bölgede barındırılan bir depolama ölçek birimi içinde verileri çoğaltır. Tüm veri çoğaltıldığında eşzamanlı olarak, hiçbir yedekleme verilerini bir birincil depolama ölçek birimi hata kaybolur sağlama.

**Azure depolama** tüm hizmetleri şifrelenmiş verileri rest gereksinimlerini karşılamak için bu başvuru mimarisi yararlanarak dağıtılan [Azure depolama](https://azure.microsoft.com/services/storage/), verileri depolayan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk şifrelemesi**
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı** Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme bekleyen bilgileri korumak için veri ve günlük dosyalarının gerçekleştirir. TDE, depolanan verileri güvencesi yetkisiz erişim ayarlanmamış sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Sütunları'her zaman şifreli](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) başvuru mimarisini dağıttıktan sonra yapılabilir. Müşterilerin, dinamik veri maskeleme ayarları kendi veritabanı şeması uyması için ayarlamanız gerekir.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) izleme verilerinin etkinlik günlükleri, Ölçümler ve Tanılama verileri, sistem durumu için eksiksiz bir görünümünü oluşturmak müşterilerin dahil olmak üzere bir tüm görüntü oluşturur.  
[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. Bunu toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Güvenlik duvarı günlükleri**: Application Gateway tam tanılama sağlar ve günlükleri erişim. Güvenlik duvarı günlükleri, Application Gateway WAF özellikli kaynakları için kullanılabilir.
- **Günlük arşivleme**: Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme 2 gün tanımlanmış tutma süresine sahip yazın. Azure İzleyici günlüklerine işlenmesi, depolanması ve Panosu raporlama için bu günlükleri bağlanın.

Ayrıca, aşağıdaki izleme çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

### <a name="identity-management"></a>Kimlik yönetimi
-   Azure AD kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme uygulama Azure SQL veritabanı kimlik doğrulaması için Azure AD kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve araştırır bunları gidermek için uygun eylemde için şüpheli olayları.
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) etkinleştirir, erişim yönetimi için Azure odaklanır. Abonelik yöneticisine abonelik erişimi sınırlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic tanıtım uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya güvenli bir VPN tüneli kullanarak güvenli bir şekilde bu verileri analiz başvuru mimarisinin bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Müşteriler, uygun şekilde ExpressRoute veya VPN ayarlama ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, AAD içinde ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD AAD ormandaki bir alt etki alanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
#### <a name="iaas---vm-considerations"></a>Iaas - VM konuları
Bu PaaS çözümü herhangi bir Azure Iaas VM dahil değildir. Bir müşteri, çoğu bu PaaS hizmetlerini çalıştırmak için bir Azure VM oluşturabilirsiniz. Bu durumda, belirli özellikler ve hizmetler için iş sürekliliği ve Azure İzleyici günlüklerine yararlanılarak:

##### <a name="business-continuity"></a>İş sürekliliği
- **Yüksek kullanılabilirlik**: Sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'daki sanal makinelerin yüksek kullanılabilirlik sağlamak için. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

- **Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri etkinleştirme tüm VM'yi geri yüklemeden geri yükleyebilirsiniz.

##### <a name="monitoring-solutions"></a>İzleme çözümleri
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Güncelleştirme yönetimi çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere müşteri yönetilmesine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking): Değişiklik izleme çözümü, müşterilerin ortamında değişikliklerini kolayca belirlemenize olanak tanır.

##### <a name="security"></a>Güvenlik
- **Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği için sanal makineler sağlar, kötü amaçlı veya istenmeyen yazılım bilinen yapılandırılabilir uyarı ile çalışır yükleme veya korumalı sanal makineler üzerinde çalıştırın.
- **Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

#### <a name="azure-commercial"></a>Azure ticari
Data analytics mimarisi bu dağıtım için tasarlanmamış rağmen [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamı aynı hedeflerine, bu başvuru mimarisinde, açıklanan Hizmetleri aracılığıyla gerçekleştirilebilir yanı Ek Hizmetleri yalnızca Azure ticari ortamında kullanılabilir. Azure ticari bir FedRAMP JAB P-ATO devlet kurumlarının ve iş ortakları oldukça hassas bilgileri Azure ticari ortam yararlanarak buluta dağıtmak için izin verme Orta etki düzeyinde tutar olduğunu lütfen unutmayın.

Azure ticari çok çeşitli ınsights'tan büyük miktarlarda veri çekmek için Analiz Hizmetleri sunar:
-   [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), bir bileşeni olan [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/), bir veya daha fazla veri kaynağından bir Tahmine dayalı analiz modeli geliştirilir. İstatistiksel İşlevler, birkaç yineleme, Power BI gibi uygulamalar ardından tüketebileceği etkili bir modeldir oluşturmak için kullanılır.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) herhangi bir boyuta, türe ve alma hızına tek bir yerde işletimsel ve keşifsel analiz için veri yakalamayı etkinleştirir. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı (VAD) kullanılabilir [indirme](https://aka.ms/blueprintanalyticsthreatmodel) veya aşağıda bulabilirsiniz:

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: FedRAMP yüksek müşteri sorumluluk matris](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Benzer şekilde, [Azure güvenlik ve uyumluluk planı: FedRAMP Orta müşteri sorumluluk matris](https://aka.ms/blueprintanalyticscimmod) FedRAMP Orta temeli tarafından gerekli tüm güvenlik denetimleri listeler. Hem belgelerin her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntı veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - FedRAMP yüksek denetim uygulaması matris](https://aka.ms/blueprintanalyticscimhigh) ve [Azure güvenlik ve uyumluluk planı - FedRAMP Orta denetimi uygulama matris](https://aka.ms/blueprintanalyticscimmod) sağlayın bilgiler, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları da dahil olmak üzere her FedRAMP temeli için analytics mimarisi üzerinde denetimleri ele alınmaktadır.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
