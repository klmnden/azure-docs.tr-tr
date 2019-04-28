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
ms.openlocfilehash: f9773c3b372ab22cbcd99828e147d23c185c4eb6
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62127336"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-nist-special-publication-800-171"></a>Azure güvenlik ve uyumluluk planı - NIST özel yayını 800-171 için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı 800-171 denetimleri bir platform olarak NIST SP bir alt kümesi uygulayan bir Azure hizmet (PaaS) web uygulamasında dağıtmak müşteriler yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yol gösterir. Ayrıca müşterilerin oluşturmak ve Azure'da kendi web uygulaması yapılandırma temeli olarak kullanılır.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak görev yapacak yöneliktir. Olarak kullanılmaması-üretim ortamıdır. Değişiklik yapmadan bu mimarisini dağıtma tamamen NIST SP 800-171 gereksinimlerini karşılamak için yeterli değil. Müşteriler, uygun güvenlik ve uyumluluk değerlendirmesi bu mimari kullanılarak oluşturulan herhangi bir çözümün yürütmek için sorumludur. Gereksinimleri her bir müşterinin uygulama ayrıntılarına göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Azure güvenlik ve uyumluluk planı için bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması başvuru mimarisi sağlar. Azure veri merkezindeki özel, adanmış bir ortam olan yalıtılmış App Service ortamı, web uygulaması barındırır. Ortam yük, Azure tarafından yönetilen sanal makineler (VM'ler) arasında web uygulaması için trafiği dengeler. Bu mimari, ağ güvenlik grupları (Nsg'ler), bir Azure uygulama ağ geçidi, Azure DNS ve Azure Load Balancer de içerir.

Azure SQL veritabanları, Gelişmiş analiz ve raporlama için columnstore dizinleri ile yapılandırılabilir. Azure SQL veritabanları, yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili veri merkezinde verilerin üç kopyasını depolar. Coğrafi olarak yedekli depolama, verilerin bir ikincil veri merkezine mil uzaklıkta yüz çoğaltılmış ve tekrar bu veri merkezinde üç kopya olarak depolanan sağlar. Bu düzenleme, veri kaybına oluşan olumsuz olaya müşterinin birincil veri merkezinde engeller.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Dağıtılan kaynakları (Azure AD) rol tabanlı erişim denetimi (RBAC) erişimi denetlemek için kullanılan Azure Active Directory. Bu kaynaklar, Azure anahtar Kasası'nda müşteri anahtarlarını içerir. Sistem durumu Azure İzleyici izlenir. Müşteriler, günlükleri tutmak için bu izleme hizmetini yapılandırın. Sistem durumu, kullanımı kolay tek bir Panoda görüntülenir.

SQL veritabanı, yaygın olarak SQL Server Management Studio yönetilir. SQL veritabanına güvenli bir VPN veya Azure ExpressRoute bağlantısı üzerinden erişmek için yapılandırılmış bir yerel makineden çalıştırır.

Application Insights sağlayan gerçek zamanlı uygulama performansı yönetimi ve Azure İzleyici günlükleri aracılığıyla analiz *Microsoft'un önerdiği Başvurusu'na yönetimi ve veri aktarma için bir VPN veya ExpressRoute bağlantısı yapılandırma Mimari alt ağı.*

![PaaS Web uygulaması başvuru mimarisi diyagramı NIST SP 800-171 için](images/nist171-paaswa-architecture.png "PaaS Web uygulaması için NIST SP 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
- Azure Sanal Ağ
    - ((1) /16 ağ
    - (4) /24 ağlar
    - (4) ağ güvenlik grupları
- Azure Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Application Insights
- Azure Active Directory
- App Service ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici (günlük)
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Veritabanı
- Azure Storage
- Azure Otomasyonu
- Azure Web Apps

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Resource Manager**: [Kaynak Yöneticisi'ni](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) kaynakları bir grup çözümünde çalışmak için müşteriler tarafından kullanılabilir. Müşteriler dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle çözüm için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanabilirsiniz. Şablon test, hazırlık ve üretim gibi farklı ortamlarda çalışabilir. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler kaynaklarını dağıttıktan sonra yönetmenize yardımcı olacak özellikler sunar.

**Kale ana bilgisayarı**: Kale ana bilgisayarı, kullanıcıların bu ortama dağıtılan kaynaklara erişmek için kullanabileceği girişinin tek noktasıdır. Kale ana bilgisayarı, güvenli bir listede yalnızca genel IP adreslerinden gelen uzak trafiğe izin vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için NSG'de kaynak trafiği tanımlanmalıdır.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısını](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure tanılama uzantısını](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Key Vault'u kullanarak.
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında VM kaynaklarının kullanımını azaltmak için.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkinleştirilir.

**Web uygulamaları**: [Web uygulamaları](https://docs.microsoft.com/azure/app-service/) bir Azure App Service özelliğidir. Müşteriler, derleme ve altyapı yönetimine gerek kalmadan kendi seçtiğiniz programlama dilinde web uygulamaları barındırmak için kullanabilirsiniz. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar. Bu, Windows ve Linux'ı destekler ve GitHub, Azure DevOps ya da herhangi bir Git deposundan otomatik dağıtımlar sağlar.

**App Service ortamı**: [App Service ortamı](https://docs.microsoft.com/azure/app-service/environment/intro) bir App Service özelliğidir. Bu App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlar.

Yalnızca tek bir uygulamayı çalıştırmak için App Service ortamı yalıtılır. Bu, her zaman bir sanal ağa dağıtılır. Yalıtım özelliğini nedeniyle tam Kiracı yalıtımı başvuru mimarisini sahiptir ve Azure'nın çok kiracılı Ortamı'ndan kaldırılır. Müşterilerin her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim var. Uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz. Müşteriler "otomatik ölçeklendirmeyi" ile App Service ortamı yükleme ölçümleri, kullanılabilir bütçe veya tanımlı bir zamanlamaya göre yapabilirsiniz.

Bu mimari için App Service ortamı kullanımı, aşağıdaki denetimleri ve yapılandırmaları sağlar:

- Güvenli bir Azure sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları.
- HTTPS iletişimi için otomatik olarak imzalanan bir iç yük dengeleyici sertifikası. En iyi uygulama, Microsoft, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3).
- Devre dışı [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Denetim [trafiği N/W bağlantı noktalarına gelen](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic).
- [Web uygulaması güvenlik duvarı – veri kısıtlama](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall).
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview).

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
- Application Gateway için bir NSG
- App Service ortamı için bir NSG
- SQL veritabanı için bir NSG
- Kale ana bilgisayarı için bir NSG

Her nsg belirli bağlantı noktalarına sahiptir ve çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek protokollerini açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanır.
  - Azure İzleyici günlüklerine bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Alt ağlar**: Her alt ağ, karşılık gelen NSG ile ilişkilidir.

**Azure DNS**: Etki alanı adı sistemi (DNS) çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma hizmeti DNS etki alanları için. Etki alanlarını azure'da barındırarak kullanıcılar, aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama tarafından DNS kayıtlarınızı yönetebilirsiniz. Azure DNS özel DNS etki alanları da destekler.

**Azure yük dengeleyici**: [Yük Dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) uygulamalarını ölçeklendirme ve yüksek kullanılabilirlik hizmetleri oluşturmak için müşteriler tarafından kullanılabilir. Yük Dengeleyici, gelen ve giden senaryolarını destekler. Düşük gecikme süresi ve yüksek aktarım hızı sağlar ve en fazla akışlar tüm TCP ve UDP uygulamaları için milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezleri gelen ve giden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: Bekleyen şifreli verileri gereksinimlerini karşılamak için tüm [depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu özellik, Kurumsal güvenlik ve uyumluluk gereksinimlerini 800-171 NIST SP tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğini kullanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Key Vault ile tümleştirilir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Bu, gerçek zamanlı şifreleme ve şifre çözme veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları bekleyen bilgileri korumak için gerçekleştirir. Veriler güvencesi yetkisiz erişim olmamıştır saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) algılama ve yanıt olarak olası tehditleri ortaya çıktıkları sağlar. Şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim modellerinin güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Bu otomatik olarak olası hassas verileri bulabilir ve uygulanması için uygun maskeleri önerin. Dinamik veri maskeleme, hassas verilerin yetkisiz erişim veritabanı açıktan erişim azaltmaya yardımcı olur. *Müşteriler, kendi veritabanı şeması uyması ayarlarını sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure AD'de oluşturulur ve SQL veritabanına erişen kullanıcıları içerir.
-   Azure AD'yi kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için bkz. nasıl [Azure AD ile uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Veritabanı sütun şifreleme Azure AD uygulamasını SQL veritabanına kimlik doğrulaması için de kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ayrıntılı erişim izinlerini tanımlamak için yöneticiler tarafından kullanılabilir. Kullanıcıların işlerini yapması için gereken Bununla, bunlar sadece erişim miktarını verebilirsiniz. Azure kaynakları için her kullanıcının sınırsız erişim vermek yerine, yöneticiler kaynaklarına ve verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişimi olan kullanıcıların sayısını en aza indirmek için müşteriler tarafından kullanılabilir. Yöneticiler, Azure AD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
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

**Azure Güvenlik Merkezi**: İle [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları da erişir.

Güvenlik Merkezi algılama özellikleri çeşitli ortamlarını hedefleyen potansiyel saldırılar müşteriler uyarmak için kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) bir tehdit veya şüpheli etkinlik başladığında tetiklenir. Müşteriler [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kendi ortamından toplanmış veriler üzerinde yeni güvenlik uyarıları tanımlamak için.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi'ni keşfedin ve olası güvenlik sorunlarını çözmek, müşteriler için daha basit hale getirir. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her tehdit algılanan için oluşturulur. Bunlar tehdit araştırma ve düzeltme olay yanıt ekiplerinin raporları kullanabilirsiniz.

**Azure uygulama ağ geçidi**: Mimari, yapılandırılmış bir web uygulaması Güvenlik Duvarı etkin OWASP kural kümesi ile bir uygulama ağ geçidi kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

- [SSL uç son](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Etkinleştirme [SSL yük boşaltmasını](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Devre dışı [TLS sürüm 1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (önleme modu).
- [Önleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ile kural kümesi.
- Etkinleştirme [tanılama günlüğünü](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Özel durum araştırmaları](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Kullanıcılar saklama süresi 730 gün belirli gereksinimleri karşılamak için en fazla yapılandırabilirsiniz.

**Azure İzleyici günlüklerine**: Günlükleri birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Veriler toplandıktan sonra Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, tüm veri ve böylece özgün kaynağına bakılmaksızın birlikte çözümlenebilir. Güvenlik Merkezi, Azure İzleyici günlükleri ile tümleştirilir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanabilirsiniz.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Bu, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Müşteriler, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı bildirir. Ayrıca, kaç aracının yanıt vermeyen ve işletimsel verileri gönderme aracı sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, SQL veritabanı'ndan günlüklerini toplama runbook'ları yardımcı olur. Müşteriler, Otomasyon kullanabileceğiniz [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek için çözüm.

**Azure İzleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve eğilimleri belirlemenize yardımcı olur. Kuruluşlar, denetleme, uyarı oluşturma ve verileri arşivlemek için kullanabilirsiniz. Ayrıca, Azure kaynaklarını API çağrılarında takip edebilirsiniz.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) bir birden çok platformda web geliştiricileri için Genişletilebilir uygulama performansı yönetim hizmeti. Application Insights performans anomalileri algılar. Müşteriler, canlı web uygulaması izlemek için kullanabilirsiniz. Application Insights, sorunları tanılamanıza ve kullanıcıların kendi uygulamayla ne yaptığını anlamanıza yardımcı olacak güçlü analiz araçlarına içerir. Bunu&#39;s müşterilerin performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-paaswa-tm) veya buradan ulaşabilirsiniz. Bu model system altyapısında potansiyel risk puanları, değişiklikler yaptığınızda anlamasına yardımcı olabilir.

![PaaS Web uygulaması için NIST SP 800-171 tehdit modeli](images/nist171-paaswa-threat-model.png "NIST SP 800-171 tehdit modeli için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP PaaS Web uygulaması denetimi uygulama matris](https://aka.ms/nist171-paaswa-cim) hangi NIST SP üzerinde 800-171 denetimleri tarafından PaaS web uygulaması mimarisi açıklanmıştır bilgi sağlar. Bu, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları içerir.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu PaaS web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile geçtiğinden, Microsoft başka bir daha da güvenli bağlantı seçeneği sunar. ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları, bir müşterinin telekomünikasyon sağlayıcısına doğrudan bağlanın. Sonuç olarak, veriler Internet üzerinden yolculuk değil ve kendisine sunulan değil. Bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
