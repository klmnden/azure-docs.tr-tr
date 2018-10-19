---
title: Azure güvenlik ve uyumluluk planı - GDPR için PaaS Web uygulaması
description: Azure güvenlik ve uyumluluk planı - GDPR için PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: 229759a1-f984-4985-a3fd-3719a7d1c8ff
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: c51ce44d1f6c2dcacaed09a490e46ad3af1ec9dc
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2018
ms.locfileid: "49406696"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-gdpr"></a>Azure güvenlik ve uyumluluk planı - GDPR için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış
Genel veri koruma yönetmeliği (GDPR) birçok gereksinimleri hakkında toplamak, depolamak ve nasıl kuruluşların tanımlamak ve kişisel verilerin güvenliğini sağlama, saydamlık gereksinimlerini karşılamak, algılamak ve rapor dahil olmak üzere, kişisel bilgileri kullanarak içerir kişisel veri ihlallerine ve eğitme gizlilik personeli ve diğer çalışanlarla. GDPR, kişilere kişisel verileri üzerinde daha fazla denetim verir ve birçok yeni yükümlülükler ile toplamak, tanıtıcı veya kişisel verileri analiz kuruluşlar uygular. GDPR mal teklif kuruluşlara yeni kurallarını uygular ve Hizmetleri kişilere Avrupa Birliği (AB) veya AB vatandaşlar için bağlı verileri toplayıp analiz. GDPR, kuruluşun bulunduğu yeri bağımsız olarak geçerlidir.

Microsoft Azure ile sektör lideri güvenlik önlemleri ve gizlilik ilkeleri, kişisel verilerin GDPR ile tanımlanan kategoriler dahil olmak üzere buluttaki verileri korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](http://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için sözleşme temelli taahhüt.

Azure güvenlik ve uyumluluk planı, bir platform, basit bir Internet'e yönelik web uygulaması için uygun bir hizmet (PaaS) ortamı olarak dağıtma konusunda rehberlik sağlar. Bu çözüm, müşterilerin GDPR belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi PaaS web uygulaması çözümleri yapılandırmak bir temel olarak görev yapar. Müşteriler, bu başvuru mimarisinde yazılımınız ve Microsoft'un izleyin [dört adım işlemi](https://aka.ms/gdprebook) YOLCULUĞUNA GDPR uyumluluğuna içinde:
1. Keşfetme: kişisel verilerin var ve yer aldığı belirleyin.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığını ve erişilebilir.
3. Koruma: önlemenize, algılamanıza ve bu güvenlik açıklarına ve veri ihlallerini yanıt için güvenlik denetimleri oluştur.
4. Rapor: gereken belgeleri tutmak ve veri isteklerini yönetme ve güvenlik ihlali bildirimleri.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a GDPR ile uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, yalıtılmış bir ortamda Azure App Service, Azure veri merkezindeki özel, adanmış bir ortamda, barındırılır. Ortam yük, Azure tarafından yönetilen sanal makineleri arasında web uygulaması için trafiği dengeler. Bu mimari, ağ güvenlik grupları, bir Application Gateway, Azure DNS ve yük dengeleyici de içerir. Ayrıca, Azure İzleyici sistem durumu, gerçek zamanlı analizler sağlar. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya ExpressRoute bağlantısı yapılandırma önerir.**

![PaaS Web uygulaması başvuru mimarisi diyagramı GDPR için](images/gdpr-paaswa-architecture.png?raw=true "PaaS Web uygulaması için GDPR başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını yerleştirilir [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Active Directory (AAD)
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) WAF Application Gateway etkin
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici: bağlantı noktası 443
- Azure sanal ağı
- ağ güvenlik grubu
- Azure DNS
- Azure Storage
- Azure İzleyici
- Application Insights
- Azure Güvenlik Merkezi
- App Service ortamı v2
- Azure Load Balancer
- Azure Web Uygulaması
- Azure Resource Manager

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) müşterilerin kaynaklarla çözümünde bir grup olarak çalışmanıza olanak tanır. Müşteriler dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle çözüm için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanabilirsiniz ve bu şablon test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler kaynaklarını dağıttıktan sonra yönetmenize yardımcı olacak özellikler sunar.

**App Service ortamı v2**: [Azure App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) , App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

Ase'ler yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış ve her zaman bir sanal ağa dağıtılır. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim imajlarını ve uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz.

Bu mimari için Ase'ler kullanımını aşağıdaki denetimleri/yapılandırmalar için izin verilir:

- Güvenli Azure sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları
- ASE HTTPS iletişimi için otomatik olarak imzalanan ILB sertifikası ile yapılandırılmış
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- Devre dışı [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [WAF – veri kısıtlama](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure Web uygulaması**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) müşterilerin oluşturmanıza ve altyapı yönetimine gerek kalmadan kendi seçtiğiniz programlama dilinde web uygulamaları barındırmanıza olanak sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden bir sanal ağ içinde trafiği listeleri (ACL) içeriyor. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
- Application Gateway için 1 NSG
- App Service ortamı için 1 NSG
- Azure SQL veritabanı için 1 NSG

Her nsg sahip belirli bağlantı noktaları ve protokoller çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Log Analytics'e bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

**Azure DNS**: etki alanı adı sistemini veya DNS çevirmek için sorumlu (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma hizmeti DNS etki alanları için. Kullanıcılar, etki alanlarını azure'da barındırarak aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama DNS kayıtlarını yönetebilirsiniz. Azure DNS özel DNS etki alanları da destekler.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) müşterilerin uygulamalarını ölçeklendirme ve yüksek kullanılabilirlik hizmetleri oluşturma olanak tanır. Yük Dengeleyici, gelen yanı sıra giden senaryoları destekler ve düşük gecikme süreli, yüksek aktarım hızı sağlar ve akışlar tüm TCP ve UDP uygulamaları için en fazla bir milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini GDPR tarafından tanımlanan desteklemek üzere kişisel verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. TDE, depolanmış kişisel veriler güvencesi yetkisiz erişim ayarlanmamış sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas kişisel verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş Özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) veri sahipleri işlenmesini kesmek için veritabanı nesneleri için özel özellikler ekleme ve veri "işlenmesini önlemek için uygulama mantığını destekleyecek şekilde artık Üretilmiyor" etiketi kullanıcılara verdiğinden kullanılabilir ilişkili kişisel veriler.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) işleme devam etmek için veri erişimini kısıtlamak için ilkeler tanımlamak üzere kullanıcıların sağlar.
- [SQL veritabanı dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılara veya uygulamalar için veri maskeleme tarafından kişisel hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. DDM otomatik olarak olası hassas verileri keşfedin ve uygulanması için uygun maskeleri önerin. Bu, kişisel verilerin GDPR koruma ve yetkisiz erişim veritabanı yok, erişim azaltmak için uygun kimlik ile yardımcı olur. **Not: Müşterilerin DDM ayarlarını kendi veritabanı şemasına bağlı kalması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında kişisel verilere erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar, Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere, AAD içinde oluşturulur.
-   Uygulama kimlik doğrulaması, AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için AAD veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Yöneticiler, Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine kişisel verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kişisel veriler gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve şüpheli araştırır olaylar, bunları gidermek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim** çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin kişisel veriler ve böyle verilere erişimin korunmasına yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı anahtarlar 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Güvenlik Uyarıları**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) müşterilerin trafiği izlemek için günlükleri toplayıp veri kaynakları için tehdit analiz sağlar. Ayrıca, Azure Güvenlik Merkezi, mevcut yapılandırma ve güvenlik duruşunu ve kişisel verilerini korumaya yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin yapılandırmasına ilişkin erişir. Azure Güvenlik Merkezi içeren bir [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her algılanan tehdit yardımcı olmak için olay yanıt ekiplerinin tehdit araştırma ve düzeltme.

**Uygulama ağ geçidi** mimari bir Application Gateway Web uygulaması Güvenlik Duvarı (WAF) ve etkin OWASP ruleset kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure İzleyici sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. Bunu toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir.
- **Günlük arşivleme**: tüm tanılama günlükleri, bir merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazma. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,. Bu günlükler, işleme, depolama ve Panosu raporlama için Azure Log Analytics'e bağlayın.

Ayrıca, aşağıdaki izleme çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): depolar, çalıştırır ve runbook'ları yöneten Azure Otomasyon çözümü. Bu çözümde, Application Insights ve Azure SQL veritabanı'ndaki günlüklerini toplama runbook'ları yardımcı olur.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetimi çözümü, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerini müşteri yönetimi sağlar.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking): müşterilerin ortamındaki değişiklikler kolayca belirlemek değişiklik izleme çözümü sağlar.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve kuruluşların denetleme, uyarı oluşturma ve dahil API çağrıları izleme verilerini arşivleme etkinleştirerek eğilimleri belirlemenize yardımcı olur müşterilerin Azure kaynaklarında.

**Application Insights**
[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Application Insights performans anomalileri algılar ve müşterilerin canlı web uygulaması izlemek için kullanabilirsiniz. Bu, müşterilerin sorunları tanılamanıza yardımcı olmak ve kullanıcıların aslında kendi uygulaması ile neler anlamak için güçlü analiz araçları içerir. Bu, müşterilerin performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak amacıyla tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/gdprPaaSdfd) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![PaaS Web uygulaması için GDPR tehdit modeli](images/gdpr-paaswa-threat-model.png?raw=true "PaaS Web uygulaması için GDPR tehdit modeli")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: GDPR müşteri sorumluluk matris](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumlulukları listeler. Azure Hizmetleri için bir genellikle denetleyicisi ve Microsoft işlemcisi olarak davranır olduğunu lütfen unutmayın.

[Azure güvenlik ve uyumluluk planı - GDPR, PaaS, Web, uygulama, uygulama, matris](https://aka.ms/gdprPaaSWeb) hangi GDPR hakkında makaleler ayrıntılı açıklamaları gibi PaaS web uygulaması mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama her kapsanan makalenin gereksinimleri nasıl karşıladığını biri.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu PaaS web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

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
