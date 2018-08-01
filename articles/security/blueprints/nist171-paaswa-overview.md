---
title: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için PaaS Web uygulaması
description: Azure güvenlik ve uyumluluk planı - PaaS Web uygulaması NIST SP 800-171
services: security
author: jomolesk
ms.assetid: eea21a0a-5930-43e8-937f-5419c20744c9
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: 8411e9d2d4118c4e1f656b5bc6e9dafb311aeb26
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391756"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-nist-special-publication-800-171"></a>Azure güvenlik ve uyumluluk planı - NIST özel yayını 800-171 için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı 800-171 denetimleri bir platform olarak NIST SP bir alt kümesi uygulayan bir Azure hizmet (PaaS) web uygulamasında dağıtmak müşteriler yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi web uygulamasını yapılandırmak bir temel olarak görev yapar.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmaması-üretim ortamıdır. Değişiklik yapmadan bu mimarisini dağıtma tamamen NIST SP 800-171 gereksinimlerini karşılamak için yeterli değil. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Azure güvenlik ve uyumluluk planı için bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması başvuru mimarisi sağlar. Web uygulaması, yalıtılmış bir ortamda Azure uygulama hizmeti olan Azure veri merkezindeki özel, adanmış bir ortamda, barındırılır. Ortam yük, Azure tarafından yönetilen sanal makinelerdeki web uygulaması için trafiği dengeler. Bu mimari, ağ güvenlik grupları, bir Application Gateway, Azure DNS ve yük dengeleyici de içerir.

Azure SQL veritabanları, Gelişmiş analiz ve raporlama için columnstore dizinleri ile yapılandırılabilir. Azure SQL veritabanları, yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili merkezinde verilerin üç kopyasını depolar. Veriler bir ikincil veri merkezine mil uzaklıkta, yüz çoğaltılmış ve müşterinin birincil veri merkezinde olumsuz bir olay kaybına oluşan önleme, veri merkezi içinde üç kopya olarak yeniden depolanan, coğrafi olarak yedekli depolama sağlar. veriler.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Rol tabanlı erişim denetimi erişimi denetlemek için kullanılan Azure Active Directory, Azure anahtar Kasası'nda kendi anahtarları gibi kaynakları dağıtıldı. Sistem durumu Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı, yaygın olarak güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış yerel makineden çalışan SQL Server Management Studio aracılığıyla yönetilir.

Ayrıca, Application Insights, gerçek zamanlı uygulama performansı yönetimi ve Log Analytics aracılığıyla analiz sağlar. **Microsoft, yönetim ve veri alma başvuru mimarisi alt ağa bir VPN veya ExpressRoute bağlantısı yapılandırma önerir.**

![PaaS Web uygulaması başvuru mimarisi diyagramı NIST SP 800-171 için](images/nist171-paaswa-architecture.png "PaaS Web uygulaması için NIST SP 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Diğer ayrıntılar için bkz [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
- Azure Sanal Ağ
    - ((1) /16 ağ
    - (4) /24 ağlar
    - (4) ağ güvenlik grupları
- Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Application Insights
- Azure Active Directory
- Azure uygulama hizmeti ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Log Analytics
- Azure Otomasyonu
- Azure Web Uygulaması

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) müşterilerin kaynaklarla çözümünde bir grup olarak çalışmanıza olanak tanır. Müşteriler dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle çözüm için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanabilirsiniz ve bu şablon test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler kaynaklarını dağıttıktan sonra yönetmenize yardımcı olacak özellikler sunar.

**Kale ana bilgisayarı**: Burcu ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu listesinde tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

**Azure Web uygulaması**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) müşterilerin oluşturmanıza ve altyapı yönetimine gerek kalmadan kendi seçtiğiniz programlama dilinde web uygulamaları barındırmanıza olanak sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Visual Studio Team Services veya herhangi bir Git deposundan otomatik dağıtım olanağı sağlar.

**Uygulama Service ortamı v2**: [Azure uygulama hizmeti ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) güvenli bir App Service uygulamaları çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir bir yüksek ölçek.

Uygulama hizmeti ortamı yalnızca tek bir uygulamayı çalıştırmak üzere yalıtılmıştır ve her zaman bir sanal ağa dağıtılır. Bu yalıtım özellik başvuru mimarisi, tüm Kiracı yalıtımı, Azure'nın çok kiracılı ortamından kaldırma olmasını sağlar. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim imajlarını ve uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz. Müşteriler "otomatik ölçeklendirmeyi" ile uygulama hizmeti ortamı yükleme ölçümleri, kullanılabilir bütçe veya tanımlı bir zamanlamaya göre yapabilirsiniz.

Uygulama hizmeti ortamı için bu mimari için aşağıdaki denetimleri/yapılandırmaları kullanımına izin verir:

- Güvenli bir Azure sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları
- HTTPS iletişimi için otomatik olarak imzalanan bir iç yük dengeleyici sertifikası. En iyi uygulama, Microsoft, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- Devre dışı [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web uygulaması güvenlik duvarı – veri kısıtlama](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

### <a name="virtual-network"></a>Sanal Ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:
- Application Gateway için 1 ağ güvenlik grubu
- Uygulama hizmeti ortamı için 1 ağ güvenlik grubu
- Azure SQL veritabanı için 1 ağ güvenlik grubu
- Kale ana bilgisayarı için 1 ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Log Analytics'e bağlı olduğu [ağ güvenlik grubunun tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen ağ güvenlik grubu ile ilişkilidir.

**Azure DNS**: etki alanı adı sistemini veya DNS çevirmek için sorumlu (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma hizmeti DNS etki alanları için. Kullanıcılar, etki alanlarını azure'da barındırarak aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama DNS kayıtlarını yönetebilirsiniz. Azure DNS özel DNS etki alanları da destekler.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) müşterilerin uygulamalarını ölçeklendirme ve yüksek kullanılabilirlik hizmetleri oluşturma olanak tanır. Yük Dengeleyici, gelen yanı sıra giden senaryoları destekler ve düşük gecikme süreli, yüksek aktarım hızı sağlar ve akışlar tüm TCP ve UDP uygulamaları için en fazla bir milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini 800-171 NIST SP tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, hassas verilerin yetkisiz erişim veritabanı yok, erişim azaltmaya yardımcı olur. **Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.**

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure Active Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere dizininde, oluşturulur.
-   Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her kullanıcının sınırsız erişim vermek yerine, yöneticiler kaynaklarına ve verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve araştırır bunları gidermek için uygun eylemde için şüpheli olayları.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşteri verilerini korumaya yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Azure Güvenlik Merkezi**: ile [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

**Azure Application Gateway**: Mimari OWASP ruleset etkinleştirilmiş ve yapılandırılmış bir web uygulaması güvenlik duvarı ile bir Azure uygulama ağ geçidi kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL yük boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu)
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [Tanılama Günlüğü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,.

**Log Analytics**: Bu günlükler, birleştirilmiş [Log Analytics](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Toplandığında, veriler birlikte analiz ve böylece özgün kaynağına bakılmaksızın tüm verilerin Operations Management Suite çalışma içindeki her veri türü için ayrı tablolar halinde düzenlenir. Ayrıca, Azure Güvenlik Merkezi, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Log Analytics sorguları kullanmak için sağlayarak müşterilerin Log Analytics ile entegre olur.

Aşağıdaki Log Analytics [yönetim çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL veritabanı'ndan günlüklerini toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve bunların Azure'da API çağrıları izleme dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur kaynaklar.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformda web geliştiricileri için genişletilebilir bir Application Performance Management hizmetidir. Application Insights performans anomalileri algılar ve müşterilerin canlı web uygulaması izlemek için kullanabilirsiniz. Bu, müşterilerin sorunları tanılamanıza yardımcı olmak için ve kullanıcıların kendi uygulamayla ne yaptığını anlamak için güçlü analiz araçları içerir. Bunu&#39;s müşterilerin performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-paaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![PaaS Web uygulaması için NIST SP 800-171 tehdit modeli](images/nist171-paaswa-threat-model.png "NIST SP 800-171 tehdit modeli için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP PaaS Web uygulaması denetimi uygulama matris](https://aka.ms/nist171-paaswa-cim) hangi NIST SP üzerinde 800-171 denetimleri PaaS web uygulaması mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama her kapsanan denetim gereksinimlerini nasıl karşıladığı, ayrıntılı açıklamaları dahil.

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
