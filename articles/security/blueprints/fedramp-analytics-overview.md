---
title: Azure güvenlik ve uyumluluk şeması - Analytics FedRAMP için
description: Azure güvenlik ve uyumluluk şeması - Analytics FedRAMP için
services: security
author: jomolesk
ms.assetid: b4675715-668e-4557-9c1c-698aabf62ceb
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: db9e49cc4dc02b6864bee2dc4b73ff3c085f5380
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207119"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-fedramp"></a>Azure güvenlik ve uyumluluk şeması: FedRAMP için analiz

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov/) bulut için standartlaştırılmış bir yaklaşım güvenlik değerlendirmesi, yetkilendirme ve sürekli izleme sağlayan bir Amerika Birleşik Devletleri kamu genelinde program Ürünler ve hizmetler. Bu Azure güvenliği ve uyumluluk şeması FedRAMP yüksek denetimleri kümesini uygulamak yardımcı olan bir Microsoft Azure analytics mimarisi teslim etme için yönergeler sağlar. Bu çözüm Kılavuzu müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilen yolları gösteren bir ortak başvuru mimarisi için dağıtım ve yapılandırma, Azure kaynaklarının sağlar ve müşteriler için bir temel olarak görev yapar derleme ve Azure'da, kendi analiz çözümleri yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzları ve tehdit modelleri müşterilerin belirli gereksinimlerine ayarlamak bir temel görevi gören yöneliktir ve olarak kullanılmamalıdır-bir üretim ortamında. Değişiklik yapmadan bu ortama bir uygulama dağıtımının tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Mimarisi müşteri iş yükleri için Azure FedRAMP uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri

Bu çözüm bağlı müşteri, kendi analiz araçları oluşturabilir analiz platformu sunar. Başvuru mimarisine genel kullanım örneği burada müşteriler toplu veri içeri aktarmaları tarafından SQL/veri Yöneticisi veya bir işlemsel kullanıcı işletimsel veri güncelleştirmelerine aracılığıyla giriş özetlenmektedir. Hem de iş akışları birleştirme [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) SQL veritabanına verileri içeri aktarma. Azure işlevleri yapılandırılmış, her müşteri için kendi benzersiz alma görevlerini işlemek için Azure portalı üzerinden müşteri tarafından analytics gereksinimleri.

Microsoft Azure birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak, bu çözüm Azure Analysis Services verilerine hızlı bir şekilde göz atarak daha hızlı sonuçlar Müşteri verilerinin daha akıllı modelleme aracılığıyla teslim etmek için Azure SQL veritabanı ile birlikte bir araya getirir. Analiz Hizmetleri Azure machine learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızları artırmak için hedeflenen biçimidir. Verileri çeşitli istatistik işlevleri eğitilmiş sonra en çok 7 ek sorgu havuzları (müşteri server dahil olmak üzere toplam 8) sorgu iş yükü yaymak ve yanıt sürelerini azaltmak için aynı tablolu modeller ile eşitlenebilir.

Gelişmiş analiz ve raporlama için SQL veritabanları columnstore dizinleri ile yapılandırılabilir. SQL veritabanlarının ve Azure Analiz Hizmetleri yukarı veya aşağı ölçeklendirilmiş veya müşteri kullanım yanıtta tamamen kapatırlar. Tüm SQL trafiği SSL ile otomatik olarak imzalanan sertifikalar ekleme şifrelenir. En iyi uygulama, Azure Gelişmiş Güvenlik güvenilen bir sertifika yetkilisi kullanılmasını önerir.

Verileri Azure SQL veritabanına karşıya ve Azure Analysis Services tarafından eğitilmiş sonra işletimsel kullanıcı ve Power BI ile SQL/Data Yönetici tarafından Sindirilmiş. Power BI veri sezgisel görüntüler ve bilgileri hakkında daha ayrıntılı bilgiler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler çok çeşitli senaryoları iş gereksinimlerine gerektirdiği şekilde işlemek üzere yapılandırabilirsiniz, yüksek derecede uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Çözümün tamamında hesap müşteriler yapılandırdığınız bir Azure depolama Azure Portalı'ndan üretilmiştir. Azure Storage rest verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler.  Coğrafi olarak yedekli depolama (GRS), ikinci bir kopyası mil hemen bir ayrı konumda yüzlerce depolanacağı olarak Müşteri'nin birincil veri merkezindeki olumsuz bir olay veri kaybına neden olacağını değil sağlar.

Gelişmiş güvenlik için bu mimarinin Azure Active Directory ve Azure anahtar kasası ile kaynakları yönetir. Sistem durumu Operations Management Suite (OMS) ve Azure İzleyicisi izlenir. Müşteriler günlükleri yakalamak ve kolayca gezinebilir, tek bir Panoda sistem durumu görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı ile SQL Server Management Studio (güvenli bir VPN ya da ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış bir yerel makineden çalışan SSMS), yaygın olarak yönetilir. **Azure VPN veya Azure ExpressRoute bağlantısı başvuru mimarisi kaynak grubu yönetim ve veri içe için yapılandırma önerir.**

![Analytics FedRAMP başvuru mimarisi diyagramı için](images/fedramp-analytics-reference-architecture.png?raw=true "analizi için FedRAMP başvuru mimarisi diyagramı")

### <a name="roles"></a>Roller
Senaryo üç genel kullanıcı türleriyle analytics şeması özetlenmektedir: işletimsel kullanıcı, SQL/Data Yönetici ve Sistem Mühendisi. Azure rol tabanlı erişim denetimi (RBAC) yerleşik özel rolleri aracılığıyla kesin erişim yönetimi uygulaması sağlar. Yapılandırmak için kullanılabilir kaynak yok [rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) anahat oluşturma ve uygulama [önceden tanımlanmış roller](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles).

#### <a name="systems-engineer"></a>Sistem Mühendisi
Sistem Mühendisi Azure Müşteri aboneliğini sahibi ve Azure portalı üzerinden çözümün dağıtımı yapılandırır.

#### <a name="sqldata-administrator"></a>SQL/veri Yöneticisi
SQL/veri Yöneticisi toplu veri içeri aktarma işlevi ve Azure SQL veritabanına yüklemeyle işletimsel veri güncelleştirme işlevi oluşturur. SQL/veri Yöneticisi herhangi bir işlem verilerini güncelleştirme veritabanında sorumlu değildir ancak Power BI ile verileri görüntüleyebilir.

#### <a name="operational-user"></a>İşlem kullanıcı
İşlem kullanıcı verileri düzenli olarak güncelleştirir ve günlük verileri oluşturma sahip. İşlem kullanıcı da sonuçları Power BI arasında görürler.

### <a name="azure-services"></a>Azure hizmetleri

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını olan [dağıtım mimarisi](#deployment-architecture) bölümü.
- Azure İşlevleri
- Azure SQL Database
- Azure Analysis Service
- Azure Active Directory
- Azure Key Vault
- OMS
- Azure İzleyici
- Azure Storage
- ExpressRoute/VPN ağ geçidi
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde geliştirme ve uygulama öğeleri ayrıntılarını verir.

![Alternatif metin](images/fedramp-analytics-components.png?raw=true "FedRAMP bileşenleri diyagramı için analiz")

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) küçük kod parçalarını çoğu programlama dilleri bulutta çalıştırmak için çözümler. Bu çözümdeki işlevleri otomatik olarak müşteri verilerini buluta, diğer Azure hizmetleriyle tümleştirme kolaylaştırmanın çekmesini Azure Storage ile tümleştirin. İşlevleri kolayca ölçeklenebilir ve bunlar çalıştırırken yalnızca bir maliyet doğurur.

**Azure Analysis Service**: [Azure Analysis Service](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) kurumsal veri modellemesi ve Azure veri platformu hizmetleriyle tümleştirme sağlar. Azure Analysis Service büyük miktarlarda verinin bir tek veri modeline birden fazla kaynaktan veri birleştirerek gözatma yukarı hızlandırır.

**Power BI**: [Power BI](https://docs.microsoft.com/power-bi/service-azure-and-power-bi) veri işleme sarf dışında büyük Insight çekme çalışılırken müşteriler için raporlama işlevselliği ve analizi sağlar.

### <a name="networking"></a>Ağ
**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) dağıtılan kaynaklarını ve Hizmetleri yönlendirilmiş trafiği yönetmek üzere ayarlayın. Ağ güvenlik grupları için varsayılan olarak izin verme düzeni ayarlayın ve yalnızca önceden yapılandırılmış erişim denetimi listesi (ACL) içinde yer alan trafiğe izin.

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü güvenli bir şekilde ve doğru şekilde çalışabilmeniz için açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkin ve bir depolama hesabında depolanır
  - OMS [günlük analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics) NSG'ın tanılama günlüklerine bağlı.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme, Denetim veritabanı ve diğer ölçülere kullanılmadıkları sırada verilerini korur.

**Veri çoğaltma** Azure kamu için iki seçenek vardır [veri çoğaltma](https://docs.microsoft.com/azure/storage/common/storage-redundancy):
 - Veri çoğaltma için varsayılan ayar **coğrafi olarak yedekli depolama (GRS)**, zaman uyumsuz olarak depolayan müşteri verileri ayrı veri merkezinde birincil bölge dışında. Bu toplam kaybı olayından birincil veri merkezi için veri kurtarma sağlar.
 - **Yerel olarak yedekli depolama (LRS)** Azure depolama hesabı alternatif olarak yapılandırılabilir. LRS müşteri hesaplarını oluşturur aynı bölgede bulunan bir depolama ölçek birimi içinde verilerini çoğaltır. Tüm veri çoğaltıldığında eşzamanlı olarak, hiçbir yedekleme verilerini bir birincil depolama ölçek birimi hatası kaybolur sağlama.

**Azure depolama** tüm hizmetleri şifrelenmiş verileri rest gereksinimleri karşılamak için bu başvuru mimarisi Dengeleme dağıtılan [Azure Storage](https://azure.microsoft.com/services/storage/), verilerle depolar [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk şifrelemesi**
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşir.

**Azure SQL veritabanı** Azure SQL veritabanı örneğinde aşağıdaki veritabanı güvenlik önlemleri kullanır:
-   [AD kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve bir Azure depolama hesabında bunları Denetim günlüğüne yazar.
-   SQL veritabanını kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme REST bilgileri korumak için veri ve günlük dosyalarının gerçekleştirir. TDE, depolanan verileri güvence yetkisiz erişim ayarlanmadı sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) uygun izinlerin verildiğinden kadar veritabanı sunucularına tüm erişimi engelleme. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi için güvenlik uyarıları sağlayarak göründüklerinde algılamayı ve olası tehditler yanıta etkinleştirir desenler.
-   [Sütunları'her zaman şifreli](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman veritabanı sistem içinde düz metin olarak göründüğünden emin olun. Veri şifreleme etkinleştirdikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlara erişimi ile düz metin verilere erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) referans mimarisi dağıtıldıktan sonra yapılabilir. Müşteriler dinamik veri maskeleme ayarlarına kendi veritabanı şeması uyması için ayarlamanız gerekir.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started) izleme verilerinin etkinlik günlükleri, ölçümleri ve sistem durumu eksiksiz bir görünümünü oluşturmak Müşteriler etkinleştirme tanılama verilerini dahil olmak üzere tüm yukarı görüntü oluşturur.  
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [günlük analizi](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.
- **Etkinlik günlükleri**: [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.
- **Tanılama günlüklerini**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından gösterilen tüm günlükler içerir. Bu günlükler Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Güvenlik Duvarı günlüklerini**: uygulama ağ geçidi tanılama tam ve günlükleri erişim sağlar. Güvenlik Duvarı günlüklerini WAF etkin uygulama ağ geçidi kaynakları için kullanılabilir.
- **Arşivleme oturum**: tüm tanılama günlüklerini merkezi ve şifrelenmiş Azure depolama hesabı için tanımlanan bekletme süresi 2 gün ile arşivleme yazma. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

Ayrıca, aşağıdaki OMS çözümleri Bu mimarinin bir parçası olarak eklenir:
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunları, algılama, tehdit bilgileri ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumu üst düzey bir fikir sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli listesi, müşteri sağlar.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): etkinlik günlüğü analiz çözümü yardımcı olduğunu Azure etkinlik günlüğü Analizi ile bir müşteri için tüm Azure abonelikleri arasında.

### <a name="identity-management"></a>Kimlik yönetimi
-   Uygulama kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure SQL veritabanı uygulama kimliğini doğrulamak için Azure AD kullanır. Daha fazla bilgi için bkz: nasıl yapılır [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşun kimlikleri etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır ve araştırır bunları çözmek için gerekli işlemleri yapmanıza şüpheli olaylar.
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) etkinleştirir erişim yönetimi için Azure odaklanır. Abonelik erişim Abonelik Yöneticisi sınırlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic Demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi**: Çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya güvenli bir VPN tüneli güvenli bir şekilde bu veri analizi başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bağlantı kurmak için yapılandırılması gerekir. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Müşteriler, uygun şekilde ExpressRoute veya bir VPN ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yönetme ve ortam ile etkileşim personel erişimi sağlama için gereklidir. AAD'de ile varolan bir Windows Server Active Directory tümleşik [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD bir AAD ormanı bir alt etki alanı dağıtılan Active Directory altyapısını yaparak bağlayabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
#### <a name="iaas---vm-vonsiderations"></a>Iaas - VM vonsiderations
Bu PaaS çözüm tüm Azure Iaas VM'ler dahil değildir. Bir müşteri PaaS hizmetlerin çoğunu çalıştırmak için bir Azure VM oluşturabilirsiniz. Bu durumda, belirli özellikler ve hizmetler için iş devamlılığı ve OMS işlevden:

##### <a name="business-continuity"></a>İş sürekliliği
- **Yüksek kullanılabilirlik**: sunucu iş yükleri gruplandırılır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için. En az bir sanal makine %99,95 toplantı planlı veya plansız bir bakım olayı sırasında kullanılabilir Azure SLA.

- **Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimarisinde Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşteriler dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri etkinleştirme tüm VM geri yüklemeden geri yükleyebilirsiniz.

##### <a name="oms"></a>OMS
-   [AD değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümünü kötü amaçlı yazılım, tehditleri ve koruma durumunu raporlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetim çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmeleri durumunu ve gerekli güncelleştirmeleri yükleme işlemi de dahil olmak üzere müşteri yönetimine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümüne raporlarını kaç aracıları dağıtılan ve kullanıcıların coğrafi dağılımı yanı sıra, yanıt vermeyen kaç aracıları ve işletimsel veri gönderiliyor Aracı sayısına.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking): müşterileri kolayca ortamında değişiklikleri belirlemek değişiklik izleme çözümü sağlar.

##### <a name="security"></a>Güvenlik
- **Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüsler, casus yazılım ve yapılandırılabilir uyarılarla diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar kötü amaçlı veya istenmeyen bilinen başlatıldığında yazılım yüklemek veya korumalı sanal makineleri çalıştırmak dener.
- **Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca OMS içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde makineleri için hizmet.

#### <a name="azure-commercial"></a>Azure ticari
Bu veri analizi mimarisi, dağıtım için tasarlanmamıştır rağmen [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamı, benzer hedefleri elde edilebilir bu başvuru mimarisinin açıklanan Hizmetleri aracılığıyla yanı ek hizmetler yalnızca Azure ticari ortamında kullanılabilir. Lütfen Azure ticari bir FedRAMP JAB P-ATO devlet dairesi ve orta düzeyde hassas bilgileri Azure ticari ortamı yararlanarak buluta dağıtmak üzere iş ortakları izin vererek Orta etkisi düzeyinde tutar olduğunu unutmayın.

Azure ticari çok çeşitli büyük miktarlarda verinin dışında Öngörüler çekmek için Analiz Hizmetleri sunar:
-   [Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/what-is-ml-studio), bileşenini [Cortana Intelligence Suite](https://azure.microsoft.com/overview/ai-platform/), Tahmine dayalı bir modeli bir veya daha fazla veri kaynaklarından geliştirir. İstatistik işlevleri birkaç yinelemeler üzerinden Power BI gibi uygulamaları sonra tüketebileceği etkili bir model oluşturmak için kullanılır.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için tek bir yerde veri yakalama sağlar. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisinin veri akış diyagramı (DFD) kullanılabilir [karşıdan](https://aka.ms/blueprintanalyticsthreatmodel) veya altında bulunabilir:

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenliği ve uyumluluk şeması – FedRAMP yüksek müşteri sorumluluk matrisi](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Benzer şekilde, [Azure güvenliği ve uyumluluk şeması – FedRAMP Orta müşteri sorumluluk matrisi](https://aka.ms/blueprintanalyticscimmod) FedRAMP Orta temeli tarafından gerekli tüm güvenlik denetimleri listeler. Hem belgeleri her denetimi uyarlamasını Microsoft, müşteri sorumluluk olsun ayrıntı veya ikisi arasında paylaşılan.

[Azure güvenliği ve uyumluluk şeması - FedRAMP yüksek denetim uygulama matris](https://aka.ms/blueprintanalyticscimhigh) ve [Azure güvenliği ve uyumluluk şeması - FedRAMP Orta denetim uygulama matris](https://aka.ms/blueprintanalyticscimmod) sağlayın ayrıntılı açıklamalar için nasıl uygulama kapsanan her denetim gereksinimlerini karşılayan biri de dahil olmak üzere her FedRAMP taban çizgisi için analytics mimarisi ele alınmıştır denetimleri bilgileri.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
