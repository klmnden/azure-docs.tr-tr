---
title: Azure güvenlik ve uyumluluk planı - FedRAMP için Iaas Web uygulaması
description: Azure güvenlik ve uyumluluk planı - FedRAMP için Iaas Web uygulaması
services: security
documentationcenter: na
author: jomolesk
manager: barbkess
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: 1ba5b813843ce2f5d31f337ab4d3d94e521b0e0c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60586143"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Azure güvenlik ve uyumluluk planı: FedRAMP için Iaas Web uygulaması

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov) bulut ürünleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izlemeye standartlaştırılmış bir yaklaşım sunan bir ABD'de devlet çapında programdır ve Hizmetler. Azure güvenlik ve uyumluluk şema Otomasyon, basit bir Internet'e yönelik web uygulaması için uygun bir hizmet (Iaas) ortamı olarak FedRAMP uyumlu bir altyapı dağıtımı için yönergeler sağlar. Bu çözüm dağıtımı ve yapılandırması, müşteriler karşılamak belirli güvenlik ve uyumluluk gereksinimlerini ve hizmet etmesi müşterilerin oluşturmak bir temel olarak yol gösteren bir ortak başvuru mimarisi için Azure kaynaklarınızın otomatik hale getirir ve kendi çözümlerini, Azure üzerinde yapılandırın. Çözüm, NIST SP 800-53'ün üzerinde temel FedRAMP yüksek çizgisinden denetimleri kümesini uygular. FedRAMP gereksinimleri ve bu çözüm hakkında daha fazla bilgi için bkz. [uyumluluk belgeleri](#compliance-documentation).
> [!NOTE]
> Bu çözüm, Azure Kamu'ya dağıtır.

Azure güvenlik ve uyumluluk şema Otomasyon bir Iaas web uygulaması başvuru mimarisi FedRAMP gereksinimleri ile uyumluluk ilkelerini yerine getirmesini müşterilere yardımcı olmak için önceden yapılandırılmış güvenlik denetimleri ile otomatik olarak dağıtır. Çözüm, Azure Resource Manager şablonları ve kaynak dağıtım ve Yapılandırma Kılavuzu PowerShell betikleri oluşur.

Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Bir uygulamaya bir değişiklik yapmadan bu ortama dağıtımı tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Bu mimari, müşterilerin Azure FedRAMP uyumlu bir şekilde kullanmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan herhangi bir çözüm uyumluluk değerlendirmesini temel.

Bu çözümü nasıl çalıştığına ilişkin hızlı bir genel bakış için bu izleme [video](https://aka.ms/fedrampblueprintvideo) açıklayan ve gösteren dağıtımı.

Tıklayın [burada](https://aka.ms/fedrampblueprintrepo) dağıtım yönergeleri.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, bir başvuru mimarisi için SQL Server arka ucuna sahip bir Iaas web uygulaması dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapı, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. Web ve veri katmanları için dağıtılan sanal makinelerin bir kullanılabilirlik kümesi'nde yapılandırılır ve SQL Server örnekleri bir AlwaysOn Kullanılabilirlik grubuna yüksek kullanılabilirlik için yapılandırılır. Etki alanına katılmış sanal makineleri ve Active Directory grup ilkeleri işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Burcu ana bilgisayarı, yöneticilerin erişim dağıtılan kaynaklara güvenli bir bağlantı sağlar. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırma önerir.**

![Iaas Web uygulaması başvuru mimarisi diyagramı FedRAMP için](images/fedramp-iaaswa-architecture.png?raw=true "Iaas Web uygulaması için FedRAMP başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını yerleştirilir [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) savunma ana bilgisayar (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server kümesi düğümünün (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Kullanılabilirlik Kümeleri
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Sanal Ağ
    - ((1) /16 sanal ağlar
    - (5) /24 alt ağlar
    - DNS ayarları, her iki etki alanı denetleyicilerine ayarlanır
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF Application Gateway etkin
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici: bağlantı noktası 443
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Azure bulut tanığı
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Azure İzleyici (günlük)

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde, geliştirme ve uygulama öğeleri ayrıntıları.

**Kale ana bilgisayarı**: Kale ana bilgisayarı tek yöneticiler tarafından dağıtılan kaynaklara güvenli bir bağlantı sağlayan giriş noktası ' dir. Savunma ana bilgisayarın NSG RDP için 3389 numaralı TCP bağlantı noktasında yalnızca bağlantılar sağlar. Müşteriler daha fazla kuruluş sistem sağlamlaştırma gereksinimlerine karşılamak için Burcu ana bilgisayarı yapılandırabilirsiniz.

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, kaynakları ayrı web alt ağı, veritabanı alt ağı, Active Directory alt ve bir sanal ağ içinde yönetim alt ağı ile bir mimari dağıtır. Alt ağlar için yalnızca bu gerekli system ve yönetim işlevselliği için alt ağlar arasındaki trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Lütfen yapılandırma için bkz. [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) ile bu çözümü dağıtıldı. Müşteriler, ağ güvenlik grupları kullanarak yukarıda dosyasını düzenleyerek yapılandırabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, özel bir ağ güvenlik grubu (NSG) vardır:
- Uygulama ağ geçidi (LBNSG) için 1 NSG
- Kale ana bilgisayarı (MGTNSG) için 1 NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için 1 NSG
- SQL sunucuları (SQLNSG) için 1 NSG
- Web Katmanı (WEBNSG) için 1 NSG

**Alt ağlar**: Her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, birçok şifreleme ölçüleri kullanarak bekleyen verileri korur.

**Azure depolama**: Bekleyen veri şifreleme gereksinimlerini karşılamak için tüm depolama hesaplarını kullanmak [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**: SQL Server kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), gerçek zamanlı şifreleme ve şifre çözme bekleyen bilgileri korumak için veri ve günlük dosyalarının gerçekleştirir. TDE, depolanan verileri güvencesi yetkisiz erişim ayarlanmamış sağlar.

Müşteriler, aşağıdaki SQL Server güvenlik önlemlerini de yapılandırabilirsiniz:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Sütunları'her zaman şifreli](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) başvuru mimarisini dağıttıktan sonra yapılabilir. Müşterilerin, dinamik veri maskeleme ayarları kendi veritabanı şeması uyması için ayarlamanız gerekir.

**Azure Disk şifrelemesi**: Azure Disk şifrelemesi, şifrelenmiş Windows Iaas sanal makine diskleri için kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojileri kimlik yönetimi özellikleri Azure ortamında sağlar:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir.
- Bir müşteri dağıtılan web uygulaması için kimlik doğrulaması, Azure AD kullanarak gerçekleştirilebilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklanmış erişim yönetimi sağlar. Abonelik erişimi için abonelik yöneticisine sınırlıdır ve kullanıcı rolü tabanlı kaynaklara erişimi sınırlı olabilir.
- Dağıtılan bir Iaas Active Directory örneğine dağıtılan Iaas sanal makineler için işletim sistemi düzeyinde kimlik yönetimi sağlar.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure Key Vault, Iaas sanal makine disk şifreleme anahtarlarını ve gizli anahtarları bu başvuru mimarisine yönelik yönetmenize yardımcı olur.

**Düzeltme Eki Yönetimi**: Azure güvenlik ve uyumluluk şema Otomasyon tarafından dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm, ayrıca Azure Otomasyon çözümünü düzeltme ekleri gerektiğinde Windows sunucuları dağıtmak için güncelleştirme dağıtımları üzerinden oluşturulabilir dağıtır.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği için sanal makineler sağlar, kötü amaçlı veya istenmeyen yazılım bilinen yapılandırılabilir uyarı ile çalışır yükleme veya korumalı sanal makineler üzerinde çalıştırın.

**Uygulama ağ geçidi**: Mimari etkin OWASP kural kümesi ile bir Application Gateway web uygulaması Güvenlik Duvarı (WAF) ile güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı. Çözümü dağıtan tüm web katmanı ve veri katmanı sanal makinelerinde bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. Ayrıca, bu çözüm SQL Server sanal makineleri bir kullanılabilirlik kümesinde dağıtılan bir [AlwaysOn Kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). Always On kullanılabilirlik grubu özelliğini için yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri sağlar.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri etkinleştirme tüm VM'yi geri yüklemeden geri yükleyebilirsiniz.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yararlanan Azure eklenen ve yönetim noktası olarak Windows Server 2016 yük devretme kümesi çekirdek tanığı türüdür. Diğer tüm çekirdek tanıkları gibi bulut tanığı, bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makinelerin ek bakım ek yükü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure İzleyici günlüklerine sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. [Azure İzleyici günlükleri](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) çözüm toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.

- **Etkinlik günlükleri:**  [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri:**  [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan her kaynak tarafından bir günlüklerdir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir.
- **Arşivleme günlük:**  Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,. Azure İzleyici günlüklerine işlenmesi, depolanması ve Panosu raporlama için bu günlükleri bağlanın.

Ayrıca, aşağıdaki izleme çözümleri Bu mimarinin bir parçası olarak yüklenir. FedRAMP güvenlik denetimleriyle hizalamak için bu çözümleri yapılandırmak için müşteri sorumluluk olduğuna dikkat edin:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): Kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): Güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): Güncelleştirme yönetimi çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere müşteri yönetilmesine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Değişiklik izleme çözümü, müşterilerin ortamında değişikliklerini kolayca belirlemenize olanak tanır.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve kuruluşların denetleme, uyarı oluşturma ve dahil API çağrıları izleme verilerini arşivleme etkinleştirerek eğilimleri belirlemenize yardımcı olur müşterilerin Azure kaynaklarında.

## <a name="threat-model"></a>Tehdit modeli
Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/fedrampWAdfd) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![Iaas Web uygulaması için FedRAMP tehdit modeli](images/fedramp-iaaswa-threat-model.png?raw=true "FedRAMP tehdit modeli için Iaas Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı - FedRAMP yüksek müşteri sorumluluk matris](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris, her denetimi uyarlamasını Microsoft, müşterinin sorumluluğundadır veya ikisi arasında paylaşılan gösterir.

[Azure güvenlik ve uyumluluk planı - FedRAMP Iaas Web uygulaması yüksek denetim uygulaması matris](https://aka.ms/blueprintwacim) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris denetimleri, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları da dahil olmak üzere Iaas web uygulaması mimarisi tarafından kapsanan bilgi sağlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Azure güvenlik ve uyumluluk şema Otomasyon JSON yapılandırma dosyaları ve kaynakları azure'da dağıtmak için Azure Resource Manager'ın API hizmeti tarafından işlenen PowerShell betikleri oluşur. Ayrıntılı dağıtım yönergeleri [burada](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Bu çözüm, Azure Kamu'ya dağıtır.

#### <a name="quickstart"></a>Hızlı Başlangıç
1. Kopyala veya indir [bu](https://aka.ms/fedrampblueprintrepo) yerel iş istasyonunuzu GitHub deposuna.

2. Dağıtım öncesi PowerShell betiğini çalıştırın: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Aşağıdaki düğmeye tıklayın, Azure portalında oturum açın, gerekli ARM şablonu parametreleri girin ve tıklatın **satın alma**.

    [![Azure’a dağıtma](https://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu Iaas Web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.  
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.  
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
