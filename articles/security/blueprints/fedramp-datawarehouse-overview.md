---
title: Azure güvenlik ve uyumluluk şeması - FedRAMP Otomasyon için veri ambarı
description: Azure güvenlik ve uyumluluk şeması - FedRAMP Otomasyon için veri ambarı
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 110ce131286f437e051dd859eb4d2baa29f106f6
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207108"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Azure güvenlik ve uyumluluk şeması: FedRAMP Otomasyon için veri ambarı

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov/) bulut için standartlaştırılmış bir yaklaşım güvenlik değerlendirmesi, yetkilendirme ve sürekli izleme sağlayan bir Amerika Birleşik Devletleri kamu genelinde program Ürünler ve hizmetler. Bu Azure güvenliği ve uyumluluk şeması FedRAMP yüksek denetimleri kümesini uygulamak yardımcı olan bir Microsoft Azure data warehouse mimarisi teslim etme için yönergeler sağlar. Bu çözüm Kılavuzu müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilen yolları gösteren bir ortak başvuru mimarisi için dağıtım ve yapılandırma, Azure kaynaklarının sağlar ve müşteriler için bir temel olarak görev yapar derleme ve kendi veri ambarı çözümleri Azure içinde yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzları ve tehdit modelleri müşterilerin belirli gereksinimlerine ayarlamak bir temel görevi gören yöneliktir ve olarak kullanılmamalıdır-bir üretim ortamında. Değişiklik yapmadan bu ortama bir uygulama dağıtımının tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Mimarisi müşteri iş yükleri için Azure FedRAMP uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri

Bu çözüm, yüksek performanslı ve güvenli bulut veri ambarıdır uygulayan bir veri ambarı başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: biri burada veriler alınırken, depolanan ve kümelenmiş bir SQL ortamı ve başka içinde Azure SQL Data Warehouse için hazırlanan veri ETL aracı kullanılarak yüklenen burada (örneğin [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL sorgularını) işleme. Verileri Azure SQL Data Warehouse'da depolanır sonra analytics yoğun ölçekte çalıştırabilirsiniz.

Microsoft Azure birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, Azure SQL veri ambarı raporları hızlı oluşturulması için SQL Server Reporting Services (SSRS) içerir. Tüm SQL trafiği SSL ile otomatik olarak imzalanan sertifikalar ekleme şifrelenir. En iyi uygulama, Azure Gelişmiş Güvenlik güvenilen bir sertifika yetkilisi kullanılmasını önerir.

Azure SQL veri ambarındaki ilişkisel tablolarla sütunlu depolama, sorgu performansını artırırken veri depolama maliyetini önemli ölçüde azaltan bir biçimde depolanır.  Kullanım gereksinimlerine bağlı olarak, Azure SQL Data Warehouse işlem kaynakları yukarı veya aşağı ölçeklendirilmiş veya işlem kaynakları gerektiren hiçbir etkin işlemler varsa tamamen kapatırlar.

Bir SQL yük dengeleyici SQL trafik, yüksek performans sağlamaya yönetir. Bu başvuru mimarisinin tüm sanal makinelerin bir kullanılabilirlik kümesinde bir AlwaysOn Kullanılabilirlik grubu yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri için yapılandırılan SQL Server örnekleri ile dağıtın.

Bu veri ambarı başvuru mimarisi mimarisi içindeki kaynaklara yönetimi için bir Active Directory (AD) katmanı da içerir. Active Directory alt bile büyük ormanına erişimi kullanılamadığında ortamının sürekli işlem için izin verme daha büyük bir AD orman yapısı altında kolay benimseme sağlar. Tüm sanal makineler için Active Directory katmanı etki alanına katılan ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için Active Directory grup ilkeleri kullanın.

Bir sanal makine kaynaklarına dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlayan bir yönetim savunma ana olarak görev yapar. Bu yönetim savunma ana bilgisayar üzerinden hazırlama alanına veri yükler. **Azure VPN veya Azure ExpressRoute bağlantısı başvuru mimarisi alt ağ yönetimi ve veri içe için yapılandırma önerir.**

![Veri ambarı FedRAMP başvuru mimarisi diyagramı için](images/fedramp-datawarehouse-architecture.png?raw=true "veri ambarı için FedRAMP başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını olan [dağıtım mimarisi](#deployment-architecture) bölümü.

Azure Sanal Makineler
-   (1) güvenlikli ana bilgisayar
-   (2) active Directory etki alanı denetleyicisi
-   (2) SQL Server küme düğümü
-   (1) SQL Server tanığı

Kullanılabilirlik Kümeleri
-   (1) active Directory etki alanı denetleyicileri
-   (1) SQL küme düğümlerini ve Tanık

Sanal Ağ
-   (4) alt ağlar
-   (4) ağ güvenlik grupları

SQL Veri Ambarı

SQL Server Raporlama Hizmetleri

Azure SQL yük dengeleyici

Azure Active Directory

Kurtarma Hizmetleri kasası

Azure Key Vault

Operations Management Suite (OMS)

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde geliştirme ve uygulama öğeleri ayrıntılarını verir.

**SQL veri ambarı**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) yüksek düzeyde paralel işleme (hızlıca petabaytlarca verileri arasında karmaşık sorgular çalıştırmak için MPP) yararlanır bir kurumsal veri ambarı (EDW) değil. Büyük veri basit PolyBase T-SQL sorguları ile SQL Data Warehouse'a almak ve yüksek performans analizi çalıştırmak için MPP gücünü kullanın.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) raporları tablolar, grafikler, haritalar, ölçerler, matrisleri ile ve Azure SQL Data Warehouse için daha hızlı oluşturulmasını sağlar.

**Savunma konak**: savunma konak tek kullanıcıların bu ortamda dağıtılan kaynaklarına erişmesine izin veren giriş noktasıdır. Savunma ana bilgisayar, uzak trafik ortak IP adreslerinden yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için kaynak trafiği ağ güvenlik grubu (NSG) tanımlanması gerekiyor.

Bir sanal makine aşağıdaki yapılandırmaları olan bir etki alanına katılmış savunma konağı olarak oluşturuldu:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısını](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure anahtar Kasası'nın (Azure kamu, PCI DSS, HIPAA ve diğer gereksinimler Uyar) kullanma
-   Bir [otomatik kapatma ilkesi](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanılmadığında sanal makine kaynaklarının kullanımını azaltmak için
-   [Windows Defender'ın kimlik bilgisi koruma](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) çalışan işletim sisteminden yalıtılır korumalı bir ortamda kimlik bilgileri ve diğer parolaları çalışabilmesi etkin

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisinin 10.0.0.0/16 bir adres alanı ile özel bir sanal ağ tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden sanal ağ içinde trafik listeleri (ACL'ler) içerir. Nsg'ler alt ağ veya tek tek VM düzeyinde trafiğinin güvenliğini sağlamak için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Bir NSG'yi (SQL Server kümelerini, SQL Server Tanık ve SQL yük dengeleyici) veri katmanı
  - Yönetim savunma ana bilgisayar için bir NSG
  - Active Directory için bir NSG
  - SQL Server Raporlama Hizmetleri için bir NSG

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü güvenli bir şekilde ve doğru şekilde çalışabilmeniz için açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkin ve bir depolama hesabında depolanır
  - OMS günlük analizi bağlı [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme, Denetim veritabanı ve diğer ölçülere kullanılmadıkları sırada verilerini korur.

**Azure depolama** şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure Storage](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).

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

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: sunucu iş yükleri gruplandırılır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'da sanal makinelerin yüksek kullanılabilirliğini sağlamaya yardımcı olmak için. En az bir sanal makine %99,95 toplantı planlı veya plansız bir bakım olayı sırasında kullanılabilir Azure SLA.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimarisinde Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşteriler dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri etkinleştirme tüm VM geri yüklemeden geri yükleyebilirsiniz.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim
[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [günlük analizi](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.
- **Etkinlik günlükleri**: [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.
- **Tanılama günlüklerini**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından gösterilen tüm günlükler içerir. Bu günlükler Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Güvenlik Duvarı günlüklerini**: uygulama ağ geçidi tanılama tam ve günlükleri erişim sağlar. Güvenlik Duvarı günlüklerini WAF etkin uygulama ağ geçidi kaynakları için kullanılabilir.
- **Arşivleme oturum**: tüm tanılama günlüklerini merkezi ve şifrelenmiş Azure depolama hesabı için tanımlanan bekletme süresi 2 gün ile arşivleme yazma. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

Ayrıca, aşağıdaki OMS çözümleri Bu mimarinin bir parçası olarak eklenir:
-   [AD değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümünü kötü amaçlı yazılım, tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunları, algılama, tehdit bilgileri ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumu üst düzey bir fikir sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli listesi, müşteri sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetim çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmeleri durumunu ve gerekli güncelleştirmeleri yükleme işlemi de dahil olmak üzere müşteri yönetimine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümüne raporlarını kaç aracıları dağıtılan ve kullanıcıların coğrafi dağılımı yanı sıra, yanıt vermeyen kaç aracıları ve işletimsel veri gönderiliyor Aracı sayısına.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): etkinlik günlüğü analiz çözümü yardımcı olduğunu Azure etkinlik günlüğü Analizi ile bir müşteri için tüm Azure abonelikleri arasında.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): müşterileri kolayca ortamında değişiklikleri belirlemek değişiklik izleme çözümü sağlar.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojileri kimlik Azure ortamında yönetim yeteneklerini sağlar:
-   [Active Directory (AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti olabilir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.
-   Uygulama kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure SQL veritabanı uygulama kimliğini doğrulamak için Azure AD kullanır. Daha fazla bilgi için bkz: nasıl yapılır [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşun kimlikleri etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır ve araştırır bunları çözmek için gerekli işlemleri yapmanıza şüpheli olaylar.
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) etkinleştirir erişim yönetimi için Azure odaklanır. Abonelik erişim Abonelik Yöneticisi sınırlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic Demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi**: Çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüsler, casus yazılım ve yapılandırılabilir uyarılarla diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar kötü amaçlı veya istenmeyen bilinen başlatıldığında yazılım yüklemek veya korumalı sanal makineleri çalıştırmak dener.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca OMS içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde makineleri için hizmet.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya güvenli bir VPN tüneli güvenli bir şekilde bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bağlantı kurmak için yapılandırılması gerekir. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Müşteriler, uygun şekilde ExpressRoute veya bir VPN ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

### <a name="extract-transform-load-etl-process"></a>Extract dönüştürme yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) verileri Azure SQL Data warehouse'a ayrı ETL gerek kalmadan veya aracı içeri aktarın. PolyBase T-SQL sorgularını verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, hem de üçüncü taraf araçları SQL Server ile uyumlu PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yönetme ve ortam ile etkileşim personel erişimi sağlama için gereklidir. AAD'de ile varolan bir Windows Server Active Directory tümleşik [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD bir AAD ormanı bir alt etki alanı dağıtılan Active Directory altyapısını yaparak bağlayabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
Bu veri ambarı mimarisi, dağıtım için tasarlanmamıştır rağmen [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamı, benzer hedefleri elde edilebilir bu başvuru mimarisinin açıklanan Hizmetleri aracılığıyla yanı ek hizmetler yalnızca Azure ticari ortamında kullanılabilir. Lütfen Azure ticari bir FedRAMP JAB P-ATO devlet dairesi ve orta düzeyde hassas bilgileri Azure ticari ortamı yararlanarak buluta dağıtmak üzere iş ortakları izin vererek Orta etkisi düzeyinde tutar olduğunu unutmayın.

Azure ticari teklifler de dahil olmak üzere çok çeşitli bu biçimlendirilmiş tanıtıcısı ve biçimlendirilmemiş veri depolama ve veri ambarı içinde kullanılacak Hazırlama Hizmetleri:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) karmaşık karma extract-dönüştürme-yükleme için (ETL), yerleşik bir yönetilen bir bulut hizmetidir extract yük dönüştürme (ELT) ve veri tümleştirme projelerinin. Müşteriler, Azure Data Factory kullanarak oluşturun ve veri temelli iş akışlarını farklı veri depolarına verilerden alma ardışık düzen adlı zamanlama. Müşteriler daha sonra işlemek ve Azure SQL Data Warehouse gibi veri depolarına çıktı verilerini dönüştürür.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) herhangi boyutu, türü ve alım hızına işletimsel ve keşifsel analiz için tek bir yerde veri yakalama sağlar. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve Azure SQL Data Warehouse gibi diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisinin veri akış diyagramı (DFD) kullanılabilir [karşıdan](https://aka.ms/blueprintdwthreatmodel) veya altında bulunabilir:

![Veri ambarı FedRAMP tehdit modeli için](images/fedramp-datawarehouse-threat-model.png?raw=true "FedRAMP tehdit modeli için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenliği ve uyumluluk şeması – FedRAMP yüksek müşteri sorumluluk matrisi](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Benzer şekilde, [Azure güvenliği ve uyumluluk şeması – FedRAMP Orta müşteri sorumluluk matrisi](https://aka.ms/blueprintcrmmod) FedRAMP Orta temeli tarafından gerekli tüm güvenlik denetimleri listeler. Hem belgeleri her denetimi uyarlamasını Microsoft, müşteri sorumluluk olsun ayrıntı veya ikisi arasında paylaşılan.

[Azure güvenliği ve uyumluluk şeması - FedRAMP yüksek denetim uygulama matris](https://aka.ms/blueprintdwcimhigh) ve [Azure güvenliği ve uyumluluk şeması - FedRAMP Orta denetim uygulama matris](https://aka.ms/blueprintdwcimmod) sağlayın ayrıntılı açıklamalar için nasıl uygulama kapsanan her denetim gereksinimlerini karşılayan biri de dahil olmak üzere her FedRAMP temel veri ambarı mimarisi ele alınmıştır denetimleri bilgileri.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
