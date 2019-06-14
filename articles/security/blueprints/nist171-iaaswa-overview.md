---
title: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için Iaas Web uygulaması
description: Azure güvenlik ve uyumluluk planı - Iaas Web uygulaması NIST SP 800-171
services: security
author: jomolesk
ms.assetid: 1f1ad27f-32c3-4e76-abb9-ea768d01747f
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: b30094e264086f018acbf84144300df46c60ac4e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610260"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-nist-sp-800-171"></a>Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için Iaas Web uygulaması

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı, müşterilerin azure'da bir alt kümesini SP NIST 800-171 denetimleri uygulayan bir web uygulaması mimarisi dağıtma yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yol gösterir. Ayrıca müşterilerin oluşturmak ve Azure'da kendi web uygulamalarını yapılandırma temeli olarak kullanılır.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak görev yapacak yöneliktir. Olarak kullanılmaması-üretim ortamıdır. Değişiklik yapmadan bu mimarisini dağıtma tamamen NIST SP 800-171 gereksinimlerini karşılamak için yeterli değil. Müşteriler, uygun güvenlik ve uyumluluk değerlendirmesi bu mimari kullanılarak oluşturulan herhangi bir çözümün yürütmek için sorumludur. Gereksinimleri her bir müşterinin uygulama ayrıntılarına göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Azure güvenlik ve uyumluluk planı, bir başvuru mimarisi için bir SQL Server arka ucu ile bir Iaas web uygulaması dağıtır. Mimari bir web katmanı, veri katmanı, Active Directory altyapısı, Azure Application Gateway ve Azure Load Balancer'ı içerir. Web ve veri katmanları için dağıtılan sanal makinelerin (VM'ler), bir kullanılabilirlik kümesine yapılandırılır. SQL Server örnekleri, bir Always On kullanılabilirlik grubuna yüksek kullanılabilirlik için yapılandırılır. Etki alanına katılmış vm'leridir. Active Directory grup ilkeleri işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygular.

Çözümün tamamını müşteriler Azure portalından yapılandırdığınız Azure depolama üzerinde oluşturulmuştur. Depolama, bekleyen verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler. Müşterinin birincil veri merkezinde olumsuz bir olay veri kaybına neden olmayan coğrafi olarak yedekli depolama sağlar. İkinci bir kopyasını ayrı bir yerde mil uzaklıkta, yüzlerce depolanır.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Azure Active Directory (Azure AD) rol tabanlı erişim denetimi (RBAC), dağıtılan kaynakları ve Azure Key vault'taki anahtarları erişimi denetlemek için kullanılır. Sistem durumu Azure İzleyici izlenir. Müşteriler, günlükleri tutmak için her iki izleme hizmetleri yapılandırın. Sistem durumu, kullanımı kolay tek bir Panoda görüntülenir.

Yönetim Burcu ana bilgisayarı, yöneticilerin dağıtılan kaynaklara güvenli bir bağlantı sağlar. *Microsoft, bir VPN veya Azure ExpressRoute bağlantısı başvuru mimarisi alt ağa yönetimi ve veri alma için yapılandırdığınız önerir.*


![Iaas Web uygulaması başvuru mimarisi diyagramı NIST SP 800-171 için](images/nist171-iaaswa-architecture.png "Iaas Web uygulaması için NIST SP 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure sanal makineleri
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server kümesi düğümünün (Windows Server 2016 üzerinde SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Azure Sanal Ağ
    - ((1) /16 ağ
    - (5) /24 ağlar
    - (5) ağ güvenlik grupları
- Kullanılabilirlik kümeleri
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Application Gateway
    - (1) web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici bağlantı noktası: 443
- Azure Active Directory
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici (günlük)
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure Storage
- Azure Otomasyonu
- Bulut tanığı
- Kurtarma Hizmetleri kasası

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Kale ana bilgisayarı**: Kale ana bilgisayarı, kullanıcıların bu ortama dağıtılan kaynaklara erişmek için kullanabileceği girişinin tek noktasıdır. Kale ana bilgisayarı, güvenli bir listede yalnızca genel IP adreslerinden gelen uzak trafiğe izin vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için ağ güvenlik grubu (NSG) kaynak trafiği tanımlanmalıdır.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısını](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure tanılama uzantısını](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Key Vault'u kullanarak.
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında VM kaynaklarının kullanımını azaltmak için.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri yalıtılmış olan korumalı bir ortamda çalışan işletim sistemini çalıştırmak için etkin.

### <a name="virtual-network"></a>Sanal ağ
10\.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm, web, veritabanı, Active Directory ve Yönetim sanal ağ içinde ayrı alt ağlar ile bir mimari kaynaklarında dağıtır. Alt ağları, mantıksal olarak ayrı bir alt ağa uygulanan NSG kuralları tarafından ayrılır. Kurallar, yalnızca bu gerekli system ve yönetim işlevselliği için alt ağlar arasındaki trafiği kısıtlayın.

Yapılandırma için bkz: [Nsg'ler](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) ile bu çözümü dağıtıldı. Kuruluşlar, Nsg'ler kullanılarak önceki dosyasını düzenleyerek yapılandırabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, adanmış bir NSG sahiptir:
- Uygulama ağ geçidi (LBNSG) için bir NSG
- Kale ana bilgisayarı (MGTNSG) için bir NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için bir NSG
- SQL sunucuları ve bulut tanığı (SQLNSG) için bir NSG
- Web Katmanı (WEBNSG) için bir NSG

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezleri gelen ve giden tüm iletişimi varsayılan olarak şifreler. Ayrıca, depolama için tüm işlemleri Azure portalı üzerinden HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri birden çok ölçü kullanılmadıkları korur. Bu ölçüler, şifreleme ve veritabanı denetimi içerir.

**Azure depolama**: Bekleyen şifreli verileri gereksinimlerini karşılamak için tüm [depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu özellik, Kurumsal güvenlik ve uyumluluk gereksinimlerini NIST 800-171 SP tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: Disk şifrelemesi, şifrelenmiş Windows Iaas VM diskleri için kullanılır. [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğini kullanır. Çözüm, denetlemenize ve disk şifreleme anahtarlarını yönetmek için Key Vault ile tümleşiktir.

**SQL Server**: SQL Server örneğini aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [SQL Server Denetim](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) veritabanı olaylarını izler ve Denetim günlükleri için yazar.
-   [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) gerçek zamanlı şifreleme ve şifre çözme veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları bekleyen bilgileri korumak için gerçekleştirir. Veriler güvencesi yetkisiz erişim olmamıştır saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) hassas verileri hiçbir zaman içinde veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Bu otomatik olarak olası hassas verileri bulabilir ve uygulanması için uygun maskeleri önerin. Dinamik veri maskeleme, hassas verilerin yetkisiz erişim veritabanı açıktan erişim azaltmaya yardımcı olur. *Müşteriler, kendi veritabanı şeması uyması ayarlarını sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure AD'de oluşturulur ve SQL Server örneğine erişmesi kullanıcıları içerir.
-   Azure AD'yi kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için bkz. nasıl [Azure AD ile uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) erişim miktarını yalnızca kullanıcıların işlerini yapması için gereken vermek için ayrıntılı erişim izinlerini tanımlamak için yöneticiler tarafından kullanılabilir. Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli kaynaklara erişebilen kullanıcı sayısını en aza indirmek için müşteriler tarafından kullanılabilir. Yöneticiler, Azure AD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır. Aynı zamanda bunları gidermek için uygun eylemde için şüpheli olayları sorunu kısmen Araştırıyor.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Key Vault](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Key Vault, şifreleme anahtarlarını koruyun ve bulut uygulamaları ve Hizmetleri tarafından kullanılan gizli yardımcı olur. Aşağıdaki anahtar kasası özellikleri müşteri verilerini korumaya yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtar anahtar türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.
- Çözüm, Iaas VM disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Key Vault ile tümleşiktir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows Vm'leri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmeti üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki Vm'lere gerektiğinde.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) VM'ler için yardımcı tanımlamak ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar. Müşteriler, bilinen kötü amaçlı veya istenmeyen yazılım yükleme veya korumalı Vm'leri çalıştırma girişiminde oluşturmasını uyarıları yapılandırabilirsiniz.

**Azure Güvenlik Merkezi**: İle [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları da erişir.

Güvenlik Merkezi algılama özellikleri çeşitli ortamlarını hedefleyen potansiyel saldırılar müşteriler uyarmak için kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) , tetiklenen bir tehdit veya şüpheli etkinlik gerçekleştiğinde. Müşteriler [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kendi ortamından toplanmış veriler üzerinde yeni güvenlik uyarıları tanımlamak için.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi'ni keşfedin ve olası güvenlik sorunlarını çözmek, müşteriler için daha basit hale getirir. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her tehdit algılanan için oluşturulur. Bunlar tehdit araştırma ve düzeltme olay yanıt ekiplerinin raporları kullanabilirsiniz.

Bu başvuru mimarisi kullanır [güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) Güvenlik Merkezi'nde yeteneği. Yapılandırıldıktan sonra (örneğin, Qualys) iş ortağı Aracısı, güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu da güvenlik açığı ve durum izleme verilerini Güvenlik Merkezi’ne geri gönderir. Müşteriler, güvenlik açığından etkilenen sanal makineleri hızlı bir şekilde tanımlamak için bu bilgileri kullanabilirsiniz.

**Azure uygulama ağ geçidi**: Mimari, yapılandırılmış bir web uygulaması Güvenlik Duvarı etkin OWASP kural kümesi ile bir uygulama ağ geçidi kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç son](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Etkinleştirme [SSL yük boşaltmasını](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu).
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ile kural kümesi.
- Etkinleştirme [tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: Çözüm içindeki tüm sanal makineler dağıtan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlayın. En az bir sanal makine % 99,95 oranında karşılayan planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den tüm VM'yi geri yüklemeden geri yükleyebilirsiniz. Bu işlem geri yükleme sürelerini hızlandırır.

**Bulut tanığı**: [Bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) Azure yönetim noktası olarak kullanan bir Windows Server 2016 yük devretme kümesi çekirdek tanığı türüdür. Diğer tüm çekirdek tanıkları gibi bulut tanığı, bir oy alır ve çekirdek hesaplamalarına katılabilir. Standart genel kullanıma açık Azure Blob Depolama kullanır. Bu düzenleme, genel bulutta barındırılan sanal makinelerin ek bakım ek yükü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Kullanıcılar saklama süresi 730 gün belirli gereksinimleri karşılamak için en fazla yapılandırabilirsiniz.

**Azure İzleyici günlüklerine**: Bu günlükler, birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Veriler toplandıktan sonra Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, tüm veri ve böylece özgün kaynağına bakılmaksızın birlikte çözümlenebilir. Güvenlik Merkezi, Azure İzleyici günlükleri ile tümleştirilir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanabilirsiniz.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Bu, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Müşteriler, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı bildirir. Ayrıca, kaç aracının yanıt vermeyen ve işletimsel verileri gönderme aracı sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, SQL Server günlük toplama runbook'ları yardımcı olur. Müşteriler, Otomasyon kullanabileceğiniz [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek için çözüm.

**Azure İzleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve eğilimleri belirlemenize yardımcı olur. Kuruluşlar, denetleme, uyarı oluşturma ve verileri arşivlemek için kullanabilirsiniz. Ayrıca, Azure kaynaklarını API çağrılarında takip edebilirsiniz.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-iaaswa-tm) veya buradan ulaşabilirsiniz. Bu model system altyapısında potansiyel risk puanları, değişiklikler yaptığınızda anlamasına yardımcı olabilir.

![Iaas Web uygulaması için NIST SP 800-171 tehdit modeli](images/nist171-iaaswa-threat-model.png "NIST SP 800-171 tehdit modeli için Iaas Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP Iaas Web uygulaması denetimi uygulama matris](https://aka.ms/nist171-iaaswa-cim) hangi NIST SP üzerinde 800-171 denetimleri tarafından Iaas web uygulaması mimarisi açıklanmıştır bilgi sağlar. Bu, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları içerir.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu Iaas web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleşir. Müşteriler, bilgilere güvenli bir şekilde "tüneli" müşterinin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde bu bağlantıyı kullanabilirsiniz. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile geçtiğinden, Microsoft başka bir daha da güvenli bağlantı seçeneği sunar. ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları, müşterinin telekomünikasyon sağlayıcısına doğrudan bağlanın. Sonuç olarak, veriler Internet üzerinden yolculuk değil ve kendisine sunulan değil. Bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar. 

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir. 
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz. 
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın. 
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir. 
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
