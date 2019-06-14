---
title: Azure güvenlik ve uyumluluk planı - FedRAMP Otomasyon için veri ambarı
description: Azure güvenlik ve uyumluluk planı - FedRAMP Otomasyon için veri ambarı
services: security
author: jomolesk
ms.assetid: 834d1ff6-8369-455f-b052-1ef301e3d7e6
ms.service: security
ms.topic: article
ms.date: 05/02/2018
ms.author: jomolesk
ms.openlocfilehash: 3c78aed2f30ea85f5bc16a8c0fb270bb1c761be8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586041"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-fedramp-automation"></a>Azure güvenlik ve uyumluluk planı: FedRAMP Otomasyon için veri ambarı

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov/) bulut için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sunan bir Amerika Birleşik Devletleri devlet çapında program Ürün ve Hizmetleri. Azure güvenlik ve uyumluluk planı nasıl sağlayabileceğinizin FedRAMP yüksek denetimlerin bir alt kümesi uygulayan yardımcı olan bir Microsoft Azure veri ambarı mimarisi için yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolu gösteren bir ortak başvuru mimarisi için dağıtım ve yapılandırma, Azure kaynaklarınızın rehberlik sağlar ve müşterilere için temel olarak görev yapar derleme ve Azure'da kendi veri ambarı çözümleri yapılandırın.

Bu başvuru mimarisi, uygulama kılavuzları ilişkili denetim ve tehdit modelleri, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Bir uygulamaya bir değişiklik yapmadan bu ortama dağıtımı tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a FedRAMP uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir veri ambarı başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: bir yere veri aktarılır, depolanır ve bir kümelenmiş SQL ortamı ve başka bir Azure SQL veri ambarı için hazırlanmış bir ETL aracını kullanarak veri yüklüyse (örneğin [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL sorguları) işlemek için. Analytics, verileri Azure SQL veri ambarı'nda depolandıktan sonra çok büyük ölçekte çalıştırabilirsiniz.

Microsoft Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, Azure SQL veri ambarı raporları hızlı oluşturulması için SQL Server Raporlama Hizmetleri (SSRS) içerir. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Azure SQL veri ambarındaki ilişkisel tabloları sütunlu depolama, sorgu performansını artırırken veri depolama maliyetlerini önemli ölçüde azaltan bir biçim depolanır.  Kullanım gereksinimlerine bağlı olarak, Azure SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya işlem kaynakları gerektiren herhangi bir etkin işlem yoksa tamamen kapatabilmek.

SQL bir yük dengeleyici SQL trafik, yüksek bir performans sağlamaya yönetir. Bu başvuru mimarisindeki tüm sanal makinelerin yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri için bir AlwaysOn Kullanılabilirlik grubuna yapılandırılmış SQL Server örnekleri ile kullanılabilirlik kümesi dağıtın.

Bu veri ambarı başvuru mimarisi, yapı içinde kaynaklarının yönetimi için bir Active Directory (AD) katmanı da içerir. Active Directory alt büyük ormanına erişimi kullanılabilir olsa bile ortamı sürekli işlem için izin verme daha büyük bir AD ormanı yapısı altında kolay benimsenmesini sağlar. Tüm sanal makineler, Active Directory katmanı için etki alanı ile birleşik ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları zorlamak için Active Directory grup ilkeleri kullanın.

Bir sanal makine, yöneticilerin erişim dağıtılan kaynaklara güvenli bir bağlantı sağlayan bir yönetim savunma ana görevi görür. Veri hazırlama alanı aracılığıyla bu yönetim Burcu ana bilgisayarı yükler. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırma önerir.**

![Veri ambarı FedRAMP başvurusu mimari şeması için](images/fedramp-datawarehouse-architecture.png?raw=true "veri ambarı için FedRAMP başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

Azure sanal makineleri
-   (1) Burcu ana bilgisayarı
-   (2) active Directory etki alanı denetleyicisi
-   (2) SQL Server küme düğümü
-   (1) SQL Server tanığı

Kullanılabilirlik Kümeleri
-   (1) active Directory etki alanı denetleyicileri
-   (1) SQL küme düğümleri ve Tanık

Sanal Ağ
-   (4) alt ağlar
-   (4) ağ güvenlik grupları

SQL Veri Ambarı

SQL Server Reporting Services

Azure SQL yük dengeleyici

Azure Active Directory

Kurtarma Hizmetleri kasası

Azure Key Vault

Azure İzleyici günlükleri

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde, geliştirme ve uygulama öğeleri ayrıntıları.

**SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) olan yüksek düzeyde paralel işleme (hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için MPP) kullanan bir kurumsal veri ambarı (EDW). Büyük veri, basit PolyBase T-SQL sorguları ile SQL veri ambarı'na içeri aktarın ve yüksek performanslı bir analiz çalıştırılacak MPP gücünden yararlanın.

**SQL Server Raporlama Hizmetleri**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) raporları tabloları, grafikler, haritalar, ölçerler, matrisler ve Azure SQL veri ambarı için daha hızlı oluşturulmasını sağlar.

**Kale ana bilgisayarı**: Kale ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu (NSG) tanımlanması gerekir.

Bir sanal makine etki alanına katılmış Burcu ana bilgisayarı aşağıdaki yapılandırmaları olan olarak oluşturulmuştur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure İzleyici uzantı günlükleri](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) kullanarak Azure anahtar Kasası'nın (Azure kamu, PCI DSS, HIPAA ve diğer gereksinimler Uyar)
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden bir sanal ağ içinde trafiği listeleri (ACL) içeriyor. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL yük dengeleyici) için bir NSG
  - Yönetim Burcu ana bilgisayarı için bir NSG
  - Active Directory için bir NSG
  - SQL Server Raporlama Hizmetleri için bir NSG

Her nsg sahip belirli bağlantı noktaları ve protokoller çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Azure İzleyici günlüklerine bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: Her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama** şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).

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

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'daki sanal makinelerin yüksek kullanılabilirlik sağlamak için. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri etkinleştirme tüm VM'yi geri yüklemeden geri yükleyebilirsiniz.

### <a name="logging-and-audit"></a>Günlüğe kaydetme ve Denetim
[Azure İzleyici günlüklerine](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Güvenlik duvarı günlükleri**: Application Gateway tam tanılama sağlar ve günlükleri erişim. Güvenlik duvarı günlükleri, Application Gateway WAF özellikli kaynakları için kullanılabilir.
- **Günlük arşivleme**: Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme 2 gün tanımlanmış tutma süresine sahip yazın. Azure İzleyici günlüklerine işlenmesi, depolanması ve Panosu raporlama için bu günlükleri bağlanın.

Ayrıca, aşağıdaki izleme çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Güncelleştirme yönetimi çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere müşteri yönetilmesine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Değişiklik izleme çözümü, müşterilerin ortamında değişikliklerini kolayca belirlemenize olanak tanır.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojileri kimlik yönetimi özellikleri Azure ortamında sağlar:
-   [Active Directory (AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve Kimlik Yönetimi Hizmeti olabilir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.
-   Azure AD kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme uygulama Azure SQL veritabanı kimlik doğrulaması için Azure AD kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve araştırır bunları gidermek için uygun eylemde için şüpheli olayları.
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) etkinleştirir, erişim yönetimi için Azure odaklanır. Abonelik yöneticisine abonelik erişimi sınırlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic tanıtım uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği için sanal makineler sağlar, kötü amaçlı veya istenmeyen yazılım bilinen yapılandırılabilir uyarı ile çalışır yükleme veya korumalı sanal makineler üzerinde çalıştırın.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="expressroute-and-vpn"></a>ExpressRoute ve VPN
[ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya güvenli bir VPN tüneli kullanarak güvenli bir şekilde bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Müşteriler, uygun şekilde ExpressRoute veya VPN ayarlama ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

### <a name="extract-transform-load-etl-process"></a>Çıkartma-dönüştürme-yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) yük verileri Azure SQL veri ambarı'na ayrı bir ETL gerek kalmadan veya aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, AAD içinde ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD AAD ormandaki bir alt etki alanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

### <a name="additional-services"></a>Ek hizmetler
Bu veri ambarı mimarisi dağıtımı için tasarlanmamıştır rağmen [Azure ticari](https://azure.microsoft.com/overview/what-is-azure/) ortamı aynı hedeflerine, bu başvuru mimarisinde, açıklanan Hizmetleri aracılığıyla gerçekleştirilebilir yanı Ek Hizmetleri yalnızca Azure ticari ortamında kullanılabilir. Azure ticari bir FedRAMP JAB P-ATO devlet kurumlarının ve iş ortakları oldukça hassas bilgileri Azure ticari ortam yararlanarak buluta dağıtmak için izin verme Orta etki düzeyinde tutar olduğunu lütfen unutmayın.

Azure ticari teklifler de dahil olmak üzere çok çeşitli Bu tanıtıcı biçimlendirilmiş ve biçimlendirilmemiş veri depolama ve veri ambarı'nda, kullanılacak Hazırlama Hizmetleri:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) karmaşık karma Ayıkla-Dönüştür-yükle (ETL), yerleşik bir yönetilen bir bulut hizmeti Ayıkla-yükle-Dönüştür (ELT) ve veri tümleştirme projeleri. Müşteriler, Azure Data factory'yi kullanarak, oluşturma ve zamanlama veri odaklı iş akışları farklı veri depolarından veri alabilen işlem hatları olarak adlandırılır. Müşteriler sonra işleyebilir ve çıktı verilerini Azure SQL veri ambarı gibi veri depolarında dönüştürün.
-   [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) herhangi bir boyuta, türe ve alma hızına tek bir yerde işletimsel ve keşifsel analiz için veri yakalamayı etkinleştirir. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve Azure SQL veri ambarı gibi diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı (VAD) kullanılabilir [indirme](https://aka.ms/blueprintdwthreatmodel) veya aşağıda bulabilirsiniz:

![FedRAMP tehdit modeli için veri ambarı](images/fedramp-datawarehouse-threat-model.png?raw=true "FedRAMP tehdit modeli için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: FedRAMP yüksek müşteri sorumluluk matris](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Benzer şekilde, [Azure güvenlik ve uyumluluk planı: FedRAMP Orta müşteri sorumluluk matris](https://aka.ms/blueprintcrmmod) FedRAMP Orta temeli tarafından gerekli tüm güvenlik denetimleri listeler. Hem belgelerin her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntı veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - FedRAMP yüksek denetim uygulaması matris](https://aka.ms/blueprintdwcimhigh) ve [Azure güvenlik ve uyumluluk planı - FedRAMP Orta denetimi uygulama matris](https://aka.ms/blueprintdwcimmod) sağlayın bilgiler, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları da dahil olmak üzere her FedRAMP temel veri ambarı mimarisi üzerinde denetimleri ele alınmaktadır.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
