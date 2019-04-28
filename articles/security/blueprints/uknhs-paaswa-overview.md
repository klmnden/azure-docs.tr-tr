---
title: Azure güvenlik ve uyumluluk planı - UK NHS için PaaS Web uygulaması
description: Azure güvenlik ve uyumluluk planı - UK NHS için PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: fe409add-6d09-4062-b3c8-d74574747739
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 8af6998c8e9fbfb230c00597600b3aa4f3991f03
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62112587"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-uk-nhs"></a>Azure güvenlik ve uyumluluk planı: UK NHS için PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı, bir başvuru mimarisi ve toplama, depolama ve sağlık verilerinin alınması için uygun bir hizmet (PaaS) çözümü olarak bir platform için yönergeler sağlar. Bu çözüm, müşteriler uyumlu, sağlanan yönergeler ile yolları gösterir [bulut güvenlik iyi uygulama Kılavuzu](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) tarafından yayımlanan [NHS dijital](https://digital.nhs.uk/), Birleşik Krallık'ın (UK Bölgesinde) iş ortağı Departman, sağlık ve sosyal Bakım (DHSC). Bulut güvenliği iyi uygulama Kılavuzu 14 üzerinde alan [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) yayımlanan Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC tarafından).

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-ek yapılandırma olmadan bir üretim ortamında olduğundan. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması için bir başvuru mimarisi sağlar. Web uygulaması, yalıtılmış bir ortamda Azure App Service, Azure veri merkezindeki özel, adanmış bir ortamda, barındırılır. Ortam yük, Azure tarafından yönetilen sanal makinelerdeki web uygulaması için trafiği dengeler. Tüm dış bağlantıları TLSv1.2 gerektirir. Bu mimari, ağ güvenlik grupları, bir Application Gateway, Azure DNS ve yük dengeleyici de içerir.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili merkezinde verilerin üç kopyasını depolar. Veriler bir ikincil veri merkezine mil uzaklıkta, yüz çoğaltılmış ve müşterinin birincil veri merkezinde olumsuz bir olay kaybına oluşan önleme, veri merkezi içinde üç kopya olarak yeniden depolanan, coğrafi olarak yedekli depolama sağlar. veriler.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Azure Active Directory rol tabanlı erişim denetimi erişimi denetlemek için kullanılan dağıtılan kaynakları ve Azure Key vault'taki anahtarları. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın. Azure Application Gateway önleme modunda bir güvenlik duvarı olarak yapılandırılır ve TLSv1.2 olmayan trafiği izin vermiyor. Çözüm, Azure uygulama hizmeti ortamı v2, çok müşterili bir ortamda web katmanı yalıtmak için kullanır.

**Microsoft, yönetim ve veri alma başvuru mimarisi alt ağa bir VPN veya ExpressRoute bağlantısı yapılandırma önerir.**

![PaaS Web uygulaması başvuru mimarisi diyagramı UK NHS için](images/uknhs-paaswa-architecture.png?raw=true "PaaS Web uygulaması için UK NHS başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

- Application Gateway
    - Web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP
        - Dinleyici bağlantı noktası: 443
- Azure Active Directory
- Azure uygulama hizmeti ortamı v2
- Azure Otomasyonu
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure İzleyici
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Veritabanı
- Azure Storage
- Azure Sanal Ağ
    - Ağ güvenlik grupları
- Azure Web Uygulaması

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) müşterilerin kaynaklarla çözümünde bir grup olarak çalışmanıza olanak tanır. Müşteriler dağıtma, güncelleştirme veya tek ve eşgüdümlü bir işlemle çözüm için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanabilirsiniz ve bu şablon test, hazırlama ve üretim gibi farklı ortamlarda çalışabilir. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler kaynaklarını dağıttıktan sonra yönetmenize yardımcı olacak özellikler sunar.

**App Service ortamı v2**: Azure App Service ortamı, App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir.

App Service ortamı yalnızca tek bir uygulamayı çalıştırmak üzere yalıtılmıştır ve her zaman bir sanal ağa dağıtılır. Bu yalıtım özellik başvuru mimarisi, tüm Kiracı yalıtımı, Azure'nın çok kiracılı ortamından kaldırma olmasını sağlar. Bu yalıtım özellik, Birleşik Krallık NHS İlkesi 3 gereksinimlerini karşılamak için gereklidir. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim imajlarını ve uygulamaları, şirket içi kurumsal kaynaklara sanal ağlar üzerinden yüksek hızda güvenli bağlantılar kurabilirsiniz. Müşteriler "otomatik ölçeklendirmeyi" ile App Service ortamı yükleme ölçümleri, kullanılabilir bütçe veya tanımlı bir zamanlamaya göre yapabilirsiniz.


App Service ortamı için bu mimari için aşağıdaki denetimleri/yapılandırmaları kullanımına izin verir:

- Güvenli bir Azure sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları
- HTTPS iletişimi için otomatik olarak imzalanan bir iç yük dengeleyici sertifikası. En iyi uygulama, Microsoft, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Devre dışı [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web uygulaması güvenlik duvarı – veri kısıtlama](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure Web uygulaması**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) müşterilerin oluşturmanıza ve altyapı yönetimine gerek kalmadan kendi seçtiğiniz programlama dilinde web uygulamaları barındırmanıza olanak sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Azure DevOps veya herhangi bir Git deposundan otomatik dağıtımlar sağlar.

### <a name="virtual-network"></a>Sanal Ağ

10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:

- Application Gateway için 1 ağ güvenlik grubu
- App Service ortamı için 1 ağ güvenlik grubu
- Azure SQL veritabanı için 1 ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:

- [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
- Azure İzleyici günlüklerine bağlı olduğu [ağ güvenlik grubu&#39;s tanılama günlükleri](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: Her alt ağa karşılık gelen ağ güvenlik grubu ile ilişkilidir.

**Azure DNS**: DNS veya etki alanı adı sistemi çevirmek için sorumludur (veya çözümleme) IP adresini bir Web sitesi veya hizmet adı. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan bir barındırma hizmeti DNS etki alanları için. Kullanıcılar, etki alanlarını azure'da barındırarak aynı kimlik bilgileri, API'ler, Araçlar ve diğer Azure hizmetlerinde faturalama DNS kayıtlarını yönetebilirsiniz. Azure DNS özel DNS etki alanları da destekler.

**Azure yük dengeleyici**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) müşterilerin uygulamalarını ölçeklendirme ve yüksek kullanılabilirlik hizmetleri oluşturma olanak tanır. Yük Dengeleyici, gelen yanı sıra giden senaryoları destekler ve düşük gecikme süreli, yüksek aktarım hızı sağlar ve akışlar tüm TCP ve UDP uygulamaları için en fazla bir milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarımdaki verileri

Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: Şifrelenmiş veri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini NHS dijital tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
- [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
- Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
- [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
- [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
- [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, tanımlamak ve yetkisiz erişim veritabanı yok, verilere erişim azaltmak için yardımcı olur. Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi

Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft&#39;s çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure Active Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere dizininde, oluşturulur.
- Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izin vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar. Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluş etkileyen olası güvenlik açıklarını algılar&#39;s kimlikleri, bir kuruluş için ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır&#39;s kimlikleri ve bunları gidermek için uygun eylemde için şüpheli olayları araştırır.

### <a name="security"></a>Güvenlik

**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin korumak ve böyle verilere Yardımı:

- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Azure Güvenlik Merkezi**: İle [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), bu çözüm merkezi olarak uygulamak ve iş yüklerinizde güvenlik ilkelerini yönetme, tehditlere maruz kalma riskinizi sınırlandırmak ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, bulmak ve olası güvenlik sorunlarını gidermek daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

**Azure uygulama ağ geçidi**: Mimari, Azure Application Gateway ile yapılandırılmış bir web uygulaması güvenlik duvarı ve etkin OWASP ruleset kullanarak güvenlik açıklarını riskini azaltır. Ek özellikler şunlardır:

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
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,.

**Azure İzleyici günlüklerine**: Bu günlükler, birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Toplandığında, veriler her bir veri türü için ayrı tablolar halinde düzenlenir ve böylece özgün kaynağına bakılmaksızın tüm verilerin birlikte analiz edilmesi sağlanır. Ayrıca, Azure Güvenlik Merkezi güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanmak müşterilerin sağlayan Azure İzleyici günlükleri ile tümleştirilir.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, runbook'ları, Azure SQL veritabanı'ndan günlüklerini toplama yardımcı olur. Otomasyon [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek müşterilerin çözüm sağlar.

**Azure İzleyici**: [Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve denetim, uyarılar oluşturabilir ve Azure kaynaklarını izleme API çağrıları dahil olmak üzere, verileri arşivlemek kuruluşların etkinleştirerek eğilimleri belirlemenize yardımcı olur.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/uknhs-paaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![PaaS Web uygulaması UK NHS tehdit modeli için](images/uknhs-paaswa-threat-model.png?raw=true "UK NHS tehdit modeli için PaaS Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı: UK NHS müşteri sorumluluk matris](https://aka.ms/uknhs-crm) bütün NHS UK gereksinimleri listelenir. Bu matris her ilkesi uygulaması, Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı: UK NHS PaaS Web uygulaması uygulama matris](https://aka.ms/uknhs-paaswa-cim) dahil olmak üzere ayrıntılı bilgiler üzerinde hangi UK NHS gereksinimleri PaaS web uygulaması mimarisi tarafından açıklanmıştır sağlar Uygulama her kapsanan İlkesi gereksinimleri nasıl karşıladığını, açıklamaları.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu PaaS web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden yer alır ve müşterilere güvenli bir şekilde sağlar &quot;tünel&quot; müşteri arasında şifrelenmiş bir bağlantı içinde bilgilerine&#39;s ağınız ve Azure. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşteri doğrudan bir bağlantı olduğundan&#39;s telekomünikasyon sağlayıcısı, verileri Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan &quot;olarak-olduğu.&quot; Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşteri artırabilir&#39;s Azure lisansı veya abonelik maliyetlerini.
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
