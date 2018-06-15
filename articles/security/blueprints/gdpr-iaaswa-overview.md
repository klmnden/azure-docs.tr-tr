---
title: Azure güvenlik ve uyumluluk şeması - GDPR Iaas Web uygulaması
description: Azure güvenlik ve uyumluluk şeması - GDPR Iaas Web uygulaması
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c418664fe94ee2801a24df59b9ef3451f3985cdb
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162193"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Azure güvenlik ve uyumluluk şeması - GDPR Iaas Web uygulaması

## <a name="overview"></a>Genel Bakış
Birçok gereksinimlerini toplamak, depolamak ve nasıl kuruluşlar tanımlamak ve kişisel verilerinizi güvenli, saydamlık gereksinimlerini karşılayacak, algılamak ve rapor gibi kişisel bilgileri kullanma hakkında genel bir veri koruma düzenleme (GDPR) içerir kişisel veriler ihlallerini ve tren gizlilik personeli ve diğer çalışanlarla. GDPR kişiler kişisel verilerini üzerinde daha fazla denetim sağlar ve birçok yeni yükümlülüklerin toplamak, işleme veya kişisel verileri çözümlemek kuruluşlar üzerinde uygular. GDPR mal ve hizmet teklifi kuruluşlar yeni kurallarını uygular ve Avrupa Birliği (AB) ya da, kişilere Hizmetleri toplamak ve AB Satışlar bağlı verileri analiz etmek. Bir kuruluş bulunduğu nerede olursa olsun GDPR geçerlidir.

Microsoft Azure endüstri lideri güvenlik önlemleri ve gizlilik ilkeleri GDPR tarafından tanımlanan kişisel veri kategorileri dahil olmak üzere bulutta verilerinizi korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](http://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için Yürüt.

Bu Azure güvenliği ve uyumluluk şeması, bir altyapı (ıaas) ortamında basit bir Internet'e yönelik web uygulaması için uygun olarak dağıtmak için yönergeler sağlar. Bu çözüm, müşteriler GDPR belirli güvenlik ve uyumluluk gereksinimlerini karşılamak yollarını gösterir ve müşterilerin oluşturmak ve Azure'da kendi Iaas web uygulaması çözümleri yapılandırmak bir temel görevi görür. Müşteriler bu başvuru mimarisinin kullanan ve Microsoft'un izleyin [dört adımlı işlem](https://aka.ms/gdprebook) GDPR uyumluluk için kendi gezisine içinde:
1. Keşfetme: kişisel veri var ve bu tanımlayın.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığı ve erişilebilir.
3. Koruma: engellemek, algılamak ve güvenlik açıkları ve veri ihlallerinden yanıt için güvenlik denetimleri kurun.
4. Rapor: gerekli belgeleri korumak ve veri isteklerini yönetme ve bildirimler ihlal.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli belirli gereksinimlerine uyarlamak müşteriler için bir temel görevi gören yöneliktir ve olarak kullanılmamalıdır-bir üretim ortamında. Lütfen şunlara dikkat edin:
- Mimarisi müşteri iş yükleri için Azure GDPR uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri
Bu çözüm, bir SQL Server arka uç bir Iaas web uygulaması için bir başvuru mimarisi dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapısını, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. İçin web ve veri katmanlarını dağıtılan sanal makinelerin bir kullanılabilirlik kümesine yapılandırılır ve SQL Server örneklerinin bir AlwaysOn Kullanılabilirlik grubu yüksek kullanılabilirlik için yapılandırılır. Sanal makinelerin etki alanına katılan ve Active Directory grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Bir yönetim savunma ana bilgisayar kaynaklarına dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlar. **Azure VPN veya ExpressRoute bağlantısı başvuru mimarisi alt ağ yönetimi ve veri içe için yapılandırma önerir.**

![Iaas Web uygulaması GDPR başvuru mimarisi diyagramı için](images/gdpr-iaaswa-architecture.png?raw=true "Iaas Web uygulaması için GDPR başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server küme düğümü (Windows Server 2016 SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Kullanılabilirlik Kümeleri
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Sanal Ağ
    - (1) /16 sanal ağlar
    - (5) /24 alt ağlar
    - DNS ayarları her iki etki alanı denetleyicilerine ayarlanır
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF uygulama etkin ağ geçidi
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici: bağlantı noktası 443
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Bulut tanığı
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Operations Management Suite (OMS)
- Azure Güvenlik Merkezi

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılarını verir.

**Savunma konak**: savunma konak tek kullanıcıların bu ortamda dağıtılan kaynaklarına erişmesine izin veren giriş noktasıdır. Savunma ana bilgisayar, uzak trafik ortak IP adreslerinden yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için kaynak trafiği ağ güvenlik grubu (NSG) tanımlanması gerekiyor.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış savunma konağı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısını](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure anahtar kasası kullanma
-   Bir [otomatik kapatma ilkesi](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanılmadığında sanal makine kaynaklarının kullanımını azaltmak için
-   [Windows Defender'ın kimlik bilgisi koruma](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) çalışan işletim sisteminden yalıtılır korumalı bir ortamda kimlik bilgileri ve diğer parolaları çalışabilmesi etkin

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt sanal ağ içinde ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır.

Yapılandırma için bkz: [ağ güvenlik grubu](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) bu çözümü ile dağıtılmış. Kuruluşlar, ağ güvenlik grupları kullanarak yukarıdaki dosyasını düzenleyerek yapılandırabilirsiniz [bu belge](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, ayrılmış bir ağ güvenlik grubu (NSG) sahiptir:
- Uygulama ağ geçidi (LBNSG) için 1 NSG
- Savunma ana bilgisayar (MGTNSG) için 1 NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için 1 NSG
- SQL Server ve bulut Tanık (SQLNSG) için 1 NSG
- Web Katmanı (WEBNSG) için 1 NSG

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure varsayılan olarak tüm iletişimler için ve Azure veri merkezleri şifreler. Ayrıca, Azure Portalı aracılığıyla Azure depolama birimine tüm işlemleri HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme ve veritabanı denetimi dahil olmak üzere birden çok ölçü kullanılmadıkları sırada verilerini korur.

**Azure depolama**: rest gereksinimleri, şifrelenmiş veri sağlamak için tüm [Azure Storage](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu korumak ve kurumsal güvenlik taahhütleri ve GDPR tarafından tanımlanan uyumluluk gereksinimlerini desteklemek kişisel verilerinizi korumaya yardımcı olur.

**Azure Disk şifrelemesi**: Azure Disk şifrelemesi, Windows Iaas sanal makine disklerini şifrelenmiş için kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşiktir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemleri kullanır:
-   [AD kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve bir Azure depolama hesabında bunları Denetim günlüğüne yazar.
-   SQL veritabanını kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyalarını bekletin. TDE, kişisel veriler güvence yetkisiz erişim ayarlanmadı sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) uygun izinlerin verildiğinden kadar veritabanı sunucularına tüm erişimi engelleme. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi için güvenlik uyarıları sağlayarak göründüklerinde algılamayı ve olası tehditler yanıta etkinleştirir desenler.
-   [Her zaman şifreli sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) önemli kişisel veriler hiç veritabanı sistem içinde düz metin olarak göründüğünden emin olun. Veri şifreleme etkinleştirdikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlara erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özelliği veri konuları işlenmesini durdurmaya özel özellikler veritabanı nesnelerini eklemek ve "önlemek için uygulama mantığını destekleyecek şekilde Üretimi Durdurulmuş" veri etiketi kullanıcıların verir olarak kullanılabilir ilişkili kişisel verilerin işlenmesini.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kullanıcıların verileri işlemeyi durdurmaya erişimi kısıtlamak için ilkeler tanımlama olanak tanır.
- [SQL veritabanı dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılar veya uygulamalar için veri maskeleme tarafından hassas kişisel veri görünürlüğünü sınırlar. DDM otomatik olarak olası hassas verileri bulmak ve uygulanması için uygun maskeleri önerir. Bu, kişisel verileri GDPR koruma ve veritabanı yoluyla yetkisiz erişimden çıkmaz şekilde erişim azaltmak için uygun tanımlaması ile yardımcı olur. **Not: Müşteriler DDM ayarlarını kendi veritabanı şemaya bağlı olması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojileri Azure ortamında kişisel verilere erişimi yönetmek için özellikleri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar, SQL Server erişen kullanıcılar dahil olmak üzere AAD'de oluşturulur.
-   Uygulama kimlik doğrulama AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme uygulama SQL Server kimlik doğrulaması için AAD kullanır. Daha fazla bilgi için bkz: nasıl yapılır [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını yalnızca vermek için ayrıntılı erişim izinlerini tanımlamak yöneticilerin sağlar. Her Azure kaynaklarını Kısıtlanmamış kullanıcı izinlerini vermek yerine, yöneticiler kişisel verilere erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik erişim Abonelik Yöneticisi sınırlıdır.
- [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli kaynaklara erişimi olan kullanıcıların sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management bulmak, kısıtlama ve ayrıcalıklı kimliklerinizi ve bunların kaynaklara erişimi izlemek için kullanabilirsiniz. Bu işlevsellik, gerektiğinde isteğe bağlı, yalnızca zaman yönetimsel erişim zorlamak için de kullanılabilir.
- [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşun kimlikleri etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır ve şüpheli araştırır bunları gidermek için uygun eylemi gerçekleştirin olaylar.
- Dağıtılan bir Iaas Active Directory örneğine dağıtılan Iaas sanal makineleri için işletim sistemi düzeyinde kimlik yönetimi sağlar.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi**: Çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure anahtar kasası özellikleri kişisel veri ve bu tür veri erişimi koruma müşterilere yardımcı olun:
- Gelişmiş erişim ilkeleri, bir gereksinim temelinde yapılandırılır.
- Anahtar kasası erişim ilkeleri, anahtarları ve gizli anahtarları için gerekli minimum izinleri ile tanımlanır.
- Tüm anahtarları ve gizli anahtar kasasında sona erme tarihi vardır.
- Anahtar kasası tüm anahtarlarında özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Tanılama günlüklerini anahtar kasası için en az 365 gün bekletme süresi ile etkinleştirilir.
- İzin verilen şifreleme işlemleri anahtarları için gerekli olanlar kısıtlanır.
- Çözüm Iaas sanal makine disk şifreleme anahtarları ve parolaları yönetmek için Azure anahtar kasası ile tümleşiktir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca OMS içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde makineleri için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüsler, casus yazılım ve yapılandırılabilir uyarılarla diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar kötü amaçlı veya istenmeyen bilinen başlatıldığında yazılım yüklemek veya korumalı sanal makineleri çalıştırmak dener.

**Güvenlik Uyarıları**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) müşterilerin trafiği izlemek, günlükleri toplamak ve veri kaynaklarını tehditleri analiz olanak verir. Ayrıca, Azure Güvenlik Merkezi, var olan yapılandırma ve güvenlik tutumunu geliştirmek ve kişisel verilerinizi korumaya yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin yapılandırmasına ilişkin erişir. Azure Güvenlik Merkezi içeren bir [tehdit Intelligence rapor](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) her yardımcı olmak üzere tehdit algılanan için olay yanıtlama takımlar araştırın ve tehditleri düzeltin.

**Uygulama ağ geçidi**: mimarisi web uygulaması güvenlik duvarı ile (WAF) bir uygulama ağ geçidi kullanarak güvenlik açıkları riskini azaltır ve OWASP ruleset etkin. Ek özellikler şunları içerir:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS v1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Engelleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [tanılama günlükleri](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)
- [Özel sistem durumu araştırmalarının](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/en-us/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: çözüm tüm sanal makinelerin dağıtan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden çok yalıtılmış donanım kümeler arasında dağıtılır emin olun. En az bir sanal makine %99,95 toplantı planlı veya plansız bir bakım olayı sırasında kullanılabilir Azure SLA.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimarisinde Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşteriler dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri etkinleştirme tüm VM geri yüklemeden geri yükleyebilirsiniz.

**Bulut Tanık**: [bulut Tanık](https://docs.microsoft.com/en-us/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yük devretme kümesi çekirdek tanığı yönetim noktası olarak Azure yararlanır Windows Server 2016 türüdür. Tüm diğer çekirdek tanığı gibi bulut Tanık bir oy alır ve çekirdek hesaplamalara katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makineleri, ek bakım yükünü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

OMS, sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [günlük analizi](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.
- **Etkinlik günlükleri**: [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemenize yardımcı olabilir geçişi ve durum süresi.
- **Tanılama günlüklerini**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından gösterilen tüm günlükler içerir. Bu günlükler Windows olayı sistem günlükleri, Azure Storage günlükleri, anahtar kasası denetim günlüklerini ve uygulama ağ geçidi erişimi ve güvenlik duvarı günlüklerini içerir.
- **Günlük arşivleme**: tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabı arşivleme yazma. Bekletme kuruluşa özgü bekletme gereksinimlerini karşılamak için kullanıcı tarafından 730 gün olarak yapılandırılabilir, ayarlama. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

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

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisinin veri akış diyagramı (DFD) kullanılabilir [karşıdan](https://aka.ms/gdprIaaSdfd) veya altında bulunabilir. Bu model, müşteriler değişiklikler yaparken sistem altyapısında potansiyel risk puanları anlamanıza yardımcı olabilir.

![Iaas Web uygulaması GDPR tehdit modeli için](images/gdpr-iaaswa-threat-model.png?raw=true "GDPR tehdit modeli için Iaas Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenliği ve uyumluluk şeması - GDPR müşteri sorumluluk matrisi](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumluluklarını listeler. Lütfen Azure Hizmetleri için bir müşteri genellikle denetleyicisidir ve Microsoft işlemci olarak davranan unutmayın.

[Azure güvenliği ve uyumluluk şeması - GDPR Iaas Web uygulaması uygulama matris](https://aka.ms/gdprIaaSweb) üzerinde hangi GDPR makaleler ele ayrıntılı açıklamaları da dahil olmak üzere Iaas Web uygulama mimarisi tarafından bilgiler sağlar uygulama gereksinimleri kapsanan her makalenin nasıl karşıladığı biri.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu Iaas Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN ya da ExpressRoute ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve bir Azure sanal ağ arasında özel bir sanal bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden gerçekleşir ve güvenli bir şekilde Müşteri'nin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde "tünel" bilgisini müşterilerin sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bu seçeneği bir şifreleme mekanizması olarak kullanılır.

VPN tüneli içindeki trafiği Internet bir siteden siteye VPN ile geçiş olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute olduğu adanmış WAN Azure ve bir şirket içi konum veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Ayrıca, bu doğrudan bağlantı, müşterinin telekomünikasyon sağlayıcısının olduğundan, veri Internet üzerinden yolculuk değil ve kendisine bu nedenle olarak gösterilmez.

Azure için bir şirket içi ağ genişleten güvenli karma ağ uygulamak için en iyi uygulamalar sağlanmıştır [kullanılabilir](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.  
- Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.  
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  
- Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
- Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
