---
title: Azure güvenlik ve uyumluluk planı - GDPR için Iaas Web uygulaması
description: Azure güvenlik ve uyumluluk planı - GDPR için Iaas Web uygulaması
services: security
author: jomolesk
ms.assetid: 04d5239c-fff0-4c2d-9379-1fa79ddbec78
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: eefbf4686e5bdc9e53f063221104a6af9817fb50
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2018
ms.locfileid: "45576332"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-gdpr"></a>Azure güvenlik ve uyumluluk planı - GDPR için Iaas Web uygulaması

## <a name="overview"></a>Genel Bakış
Genel veri koruma yönetmeliği (GDPR) birçok gereksinimleri hakkında toplamak, depolamak ve nasıl kuruluşların tanımlamak ve kişisel verilerin güvenliğini sağlama, saydamlık gereksinimlerini karşılamak, algılamak ve rapor dahil olmak üzere, kişisel bilgileri kullanarak içerir kişisel veri ihlallerine ve eğitme gizlilik personeli ve diğer çalışanlarla. GDPR, kişilere kişisel verileri üzerinde daha fazla denetim verir ve birçok yeni yükümlülükler ile toplamak, tanıtıcı veya kişisel verileri analiz kuruluşlar uygular. GDPR mal teklif kuruluşlara yeni kurallarını uygular ve Hizmetleri kişilere Avrupa Birliği (AB) veya AB vatandaşlar için bağlı verileri toplayıp analiz. GDPR, kuruluşun bulunduğu yeri bağımsız olarak geçerlidir.

Microsoft Azure ile sektör lideri güvenlik önlemleri ve gizlilik ilkeleri, kişisel verilerin GDPR ile tanımlanan kategoriler dahil olmak üzere buluttaki verileri korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](http://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için sözleşme temelli taahhüt.

Azure güvenlik ve uyumluluk planı, bir altyapı basit bir Internet'e yönelik web uygulaması için uygun bir hizmet (Iaas) ortamı olarak dağıtma konusunda rehberlik sağlar. Bu çözüm, müşterilerin gdpr belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi Iaas web uygulaması çözümleri yapılandırmak bir temel olarak görev yapar. Müşteriler, bu başvuru mimarisinde yazılımınız ve Microsoft'un izleyin [dört adım işlemi](https://aka.ms/gdprebook) YOLCULUĞUNA GDPR uyumluluğuna içinde:
1. Keşfetme: kişisel verilerin var ve yer aldığı belirleyin.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığını ve erişilebilir.
3. Koruma: önlemenize, algılamanıza ve bu güvenlik açıklarına ve veri ihlallerini yanıt için güvenlik denetimleri oluştur.
4. Rapor: gereken belgeleri tutmak ve veri isteklerini yönetme ve güvenlik ihlali bildirimleri.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a GDPR ile uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, bir başvuru mimarisi için SQL Server arka ucuna sahip bir Iaas web uygulaması dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapı, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. Web ve veri katmanları için dağıtılan sanal makinelerin bir kullanılabilirlik kümesine yapılandırılır ve SQL Server örnekleri bir AlwaysOn Kullanılabilirlik grubuna yüksek kullanılabilirlik için yapılandırılır. Etki alanına katılmış sanal makineleri ve Active Directory grup ilkeleri işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Yönetim Burcu ana bilgisayarı, yöneticilerin dağıtılan kaynaklara güvenli bir bağlantı sağlar. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya ExpressRoute bağlantısı yapılandırma önerir.**

![Iaas Web uygulaması başvuru mimarisi diyagramı GDPR için](images/gdpr-iaaswa-architecture.png?raw=true "Iaas Web uygulaması için GDPR başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını yerleştirilir [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
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
- Bulut tanığı
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Azure Active Directory (AAD)
- Azure Resource Manager
- Operations Management Suite'e (OMS)
- Azure Güvenlik Merkezi

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Kale ana bilgisayarı**: Burcu ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu (NSG) tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [OMS uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm kaynakları ayrı web alt ağı, veritabanı alt ağı, Active Directory alt ve bir sanal ağ içinde yönetim alt ağı ile bir mimari dağıtır. Alt ağlar için yalnızca bu gerekli system ve yönetim işlevselliği için alt ağlar arasındaki trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kuralları tarafından mantıksal olarak ayrılır.

Yapılandırma için bkz: [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) ile bu çözümü dağıtıldı. Kuruluşlar, ağ güvenlik grupları kullanarak yukarıda dosyasını düzenleyerek yapılandırabilirsiniz [bu belgeleri](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, özel bir ağ güvenlik grubu (NSG) vardır:
- Uygulama ağ geçidi (LBNSG) için 1 NSG
- Kale ana bilgisayarı (MGTNSG) için 1 NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için 1 NSG
- SQL sunucuları ve bulut tanığı (SQLNSG) için 1 NSG
- Web Katmanı (WEBNSG) için 1 NSG

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Ayrıca, Azure depolama için tüm işlemleri Azure portalı üzerinden, HTTPS oluşur.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme ve veritabanı denetimi gibi birden çok ölçü kullanılmadıkları verilerini korur.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini GDPR tarafından tanımlanan desteklemek üzere kişisel verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: Azure Disk şifrelemesi Windows Iaas sanal makine disklerini şifrelenmiş için kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

**SQL Server**: SQL Server örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. TDE, depolanmış kişisel veriler güvencesi yetkisiz erişim ayarlanmamış sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas kişisel verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özelliği veri sahipleri işlenmesini kesmek için kullanılabilir veritabanı nesneleri için özel özellikler ekleme ve veri "önlemek için uygulama mantığını destekleyecek şekilde artık Üretilmiyor" etiketi kullanıcılara verdiğinden ilişkili kişisel verilerin işlenmesini.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) işleme devam etmek için veri erişimini kısıtlamak için ilkeler tanımlamak üzere kullanıcıların sağlar.
- [SQL veritabanı dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılara veya uygulamalar için veri maskeleme tarafından kişisel hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. DDM otomatik olarak olası hassas verileri keşfedin ve uygulanması için uygun maskeleri önerin. Bu, kişisel verilerin GDPR koruma ve yetkisiz erişim veritabanı yok, erişim azaltmak için uygun kimlik ile yardımcı olur. **Not: Müşterilerin DDM ayarlarını kendi veritabanı şemasına bağlı kalması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında kişisel verilere erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar, SQL Server erişen kullanıcılar dahil olmak üzere, AAD içinde oluşturulur.
-   Uygulama kimlik doğrulaması, AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, uygulama SQL Server kimlik doğrulaması için AAD veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Yöneticiler, Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine kişisel verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [AAD Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli kaynaklara erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve şüpheli araştırır olaylar, bunları gidermek için uygun eylemi gerçekleştirebilir.
- Dağıtılan bir Iaas Active Directory örneğine dağıtılan Iaas sanal makineler için işletim sistemi düzeyinde kimlik yönetimi sağlar.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin kişisel veriler ve böyle verilere erişimin korunmasına yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı anahtarlar 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.
- Çözüm, Iaas sanal makine disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilmiştir.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm, ayrıca OMS içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım, yapılandırılabilir uyarı Kaldır gerçek zamanlı koruma özelliği sağlar. bilinen kötü amaçlı veya istenmeyen yazılım yükleme veya korumalı sanal makineler üzerinde çalışmayı denediğinde olduğunda.

**Güvenlik Uyarıları**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) müşterilerin trafiği izlemek için günlükleri toplayıp veri kaynakları için tehdit analiz sağlar. Ayrıca, Azure Güvenlik Merkezi, mevcut yapılandırma ve güvenlik duruşunu ve kişisel verilerini korumaya yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin yapılandırmasına ilişkin erişir. Azure Güvenlik Merkezi içeren bir [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her algılanan tehdit yardımcı olmak için olay yanıt ekiplerinin tehdit araştırma ve düzeltme.

**Uygulama ağ geçidi**: Mimari bir Application Gateway web uygulaması Güvenlik Duvarı (WAF) kullanarak güvenlik açıklarını riskini azaltır ve OWASP ruleset etkin. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: tüm sanal makinelerin çözümü dağıtan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden fazla yalıtılmış donanım kümesi arasında dağıtılmasını sağlar. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri etkinleştirme tüm VM'yi geri yüklemeden geri yükleyebilirsiniz.

**Bulut tanığı**: [bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yararlanan Azure eklenen ve yönetim noktası olarak Windows Server 2016 yük devretme kümesi çekirdek tanığı türüdür. Diğer tüm çekirdek tanıkları gibi bulut tanığı, bir oy alır ve çekirdek hesaplamalarına katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makinelerin ek bakım ek yükü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

OMS, sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [Log Analytics](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir.
- **Günlük arşivleme**: tüm tanılama günlükleri, bir merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazma. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,. Bu günlükler, işleme, depolama ve Panosu raporlama için Azure Log Analytics'e bağlayın.

Ayrıca, aşağıdaki OMS çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): depolar, çalıştırır ve runbook'ları yöneten Azure Otomasyon çözümü.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetimi çözümü, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerini müşteri yönetimi sağlar.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): müşterilerin ortamındaki değişiklikler kolayca belirlemek değişiklik izleme çözümü sağlar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı (VAD) kullanılabilir [indirme](https://aka.ms/gdprIaaSdfd) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![Iaas Web uygulaması için GDPR tehdit modeli](images/gdpr-iaaswa-threat-model.png?raw=true "GDPR tehdit modeli için Iaas Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı - GDPR müşteri sorumluluk matris](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumlulukları listeler. Azure Hizmetleri için bir genellikle denetleyicisi ve Microsoft işlemcisi olarak davranır olduğunu lütfen unutmayın.

[Azure güvenlik ve uyumluluk planı - GDPR, Iaas, Web, uygulama, uygulama, matris](https://aka.ms/gdprIaaSweb) hangi GDPR hakkında makaleler ayrıntılı açıklamaları da dahil olmak üzere Iaas Web uygulaması mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama her kapsanan makalenin gereksinimleri nasıl karşıladığını biri.

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
