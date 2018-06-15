---
title: Azure güvenlik ve uyumluluk şeması - FedRAMP PaaS Web uygulaması
description: Azure güvenlik ve uyumluluk şeması - FedRAMP PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: 20aa842fb8168bc28a388c817f4e4eedbdd63ebd
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34728100"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Azure güvenlik ve uyumluluk şeması: FedRAMP PaaS Web uygulaması

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov/) bulut için standartlaştırılmış bir yaklaşım güvenlik değerlendirmesi, yetkilendirme ve sürekli izleme sağlayan bir Amerika Birleşik Devletleri kamu genelinde program Ürünler ve hizmetler. Bu Azure güvenliği ve uyumluluk şeması FedRAMP yüksek denetimleri kümesini uygulamak yardımcı olan bir hizmet (PaaS) mimari bir Microsoft Azure platform teslim etme için yönergeler sağlar. Bu çözüm Kılavuzu müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilen yolları gösteren bir ortak başvuru mimarisi için dağıtım ve yapılandırma, Azure kaynaklarının sağlar ve müşteriler için bir temel olarak görev yapar derleme ve Azure üzerinde kendi çözümleri yapılandırın.

Bu başvuru mimarisi, ilişkili denetim uygulama kılavuzları ve tehdit modelleri müşterilerin belirli gereksinimlerine ayarlamak bir temel görevi gören yöneliktir ve olarak kullanılmamalıdır-bir üretim ortamında. Değişiklik yapmadan bu ortama bir uygulama dağıtımının tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Mimarisi müşteri iş yükleri için Azure FedRAMP uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri
Bu çözüm, bir Azure SQL veritabanı arka ucu ile bir PaaS web uygulaması için referans mimarisi sağlar. Web uygulaması, yalıtılmış bir ortamda Azure App Service, Azure veri merkezinde özel, ayrılmış bir ortamda, barındırılır. Ortam yük Azure tarafından yönetilen sanal makineleri üzerinde web uygulaması için trafiği dengeler. Bu mimari, ağ güvenlik grupları, bir uygulama ağ geçidi, Azure DNS ve yük dengeleyici de içerir. Ayrıca, Operations Management Suite, güvenlik ve sistem durumu gerçek zamanlı analiz sağlar. **Azure VPN veya ExpressRoute bağlantısı başvuru mimarisi alt ağ yönetimi ve veri içe için yapılandırma önerir.**

![PaaS Web uygulaması için FedRAMP başvuru mimarisi diyagramı](images/fedramp-paaswa-architecture.png?raw=true) "FedRAMP başvuru mimarisi diyagramı için Web uygulaması PaaS"

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Active Directory
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) web uygulaması güvenlik duvarı
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici: bağlantı noktası 443
- Azure sanal ağı
- Ağ güvenlik grupları
- Azure DNS
- Azure Storage
- Operations Management Suite
- Azure İzleyici
- Uygulama hizmeti ortamı v2
- Azure Load Balancer
- Azure Web Uygulaması
- Azure Resource Manager

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılarını verir.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) müşterilerin kaynaklarla çözüm bir grup olarak çalışmanıza olanak sağlar. Müşteriler dağıtmak, güncelleştirmek veya tek ve eşgüdümlü bir işlemle çözümünüz için tüm kaynakları silin. Müşteriler, dağıtım için bir şablon kullanın ve bu şablon test, hazırlama ve üretim gibi farklı ortamları için çalışabilirsiniz. Resource Manager Güvenlik, denetleme ve etiketleme müşteriler dağıtımdan sonra kaynaklarını yönetmenize yardımcı olmak için özellikleri sunar.

**Uygulama hizmeti ortamı v2**: [Azure uygulama hizmeti ortamı (ana)](https://docs.microsoft.com/azure/app-service/environment/intro) App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir uygulama hizmeti özelliğidir.

ASEs yalnızca tek bir müşterinin uygulamaları çalıştırmak için yalıtılmış ve her zaman bir sanal ağ içinde dağıtılır. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim müşterilerin sahip ve uygulamaları şirket içi kurumsal kaynaklara sanal ağları üzerinden yüksek hızlı güvenli bağlantı kurabilirsiniz.

Bu mimari ASEs kullanılmak için aşağıdaki denetimleri/yapılandırmalarını izin verilir:

- Güvenli bir Azure sanal ağı içinde ana bilgisayar ve ağ güvenlik kuralları
- HTTPS iletişimi için otomatik olarak imzalanan ILB sertifikası ile yapılandırılan ana
- [İç Yük Dengeleme modu](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Devre dışı [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Değişiklik [TLS şifreleme](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Web uygulaması güvenlik duvarı – veri kısıtla](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [Azure SQL veritabanı trafiği](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

[Yönerge ve önerileri](#guidance-and-recommendations) bölüm ASEs hakkında ek bilgiler içerir.

**Azure Web uygulaması**: [Azure Web Apps](https://docs.microsoft.com/azure/app-service/) yapı ve altyapı yönetmek zorunda kalmadan kendi seçtikleri programlama dilinde web uygulamalarını barındırmak müşterilerin sağlar. Otomatik ölçeklendirme ve yüksek kullanılabilirlik sunar, hem Windows hem de Linux’ı destekler ve GitHub, Visual Studio Team Services veya herhangi bir Git deposundan otomatik dağıtım olanağı sağlar.

### <a name="virtual-network"></a>Sanal Ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: [ağ güvenlik grupları (Nsg'ler)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içindeki trafiği reddeden erişim denetim listeleri içerir. Nsg'ler alt ağ veya tek tek VM düzeyinde trafiğinin güvenliğini sağlamak için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
- Uygulama ağ geçidi için 1 NSG
- Uygulama hizmeti ortamı için 1 NSG
- Azure SQL veritabanı için 1 NSG

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü güvenli bir şekilde ve doğru şekilde çalışabilmeniz için açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkin ve bir depolama hesabında depolanır
  - OMS günlük analizi bağlı [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

**Azure DNS**: etki alanı adı sistemini veya DNS, çevirmek için sorumlu (veya çözme) bir Web sitesi ya da hizmet adı IP adresine. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) Azure altyapısı kullanılarak ad çözümlemesi sağlayan barındırma için bir DNS etki alanı hizmetidir. Kullanıcılar, Azure etki alanlarında barındırarak aynı kimlik bilgileri, API ' larını araçları kullanarak ve diğer Azure hizmetleriyle faturalama DNS kayıtlarını yönetebilir. Azure DNS, özel DNS etki alanı da destekler.

**Azure yük dengeleyici**: [Azure yük dengeleyici](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) müşterilerin kendi uygulamalarında ölçeklendirmenizi ve yüksek kullanılabilirlik Hizmetleri için oluşturmak olanak tanır. Yük Dengeleyici gelen hem de giden senaryolarını destekler ve düşük gecikme, yüksek verimlilik sağlar ve akışlar tüm TCP ve UDP uygulamalar için en çok bir milyonlarca ölçeklendirir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure varsayılan olarak tüm iletişimler için ve Azure veri merkezleri şifreler. Azure Portalı aracılığıyla Azure depolama birimine tüm işlemleri HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme, Denetim veritabanı ve diğer ölçülere kullanılmadıkları sırada verilerini korur.

**Azure depolama**: rest gereksinimleri, şifrelenmiş veri sağlamak için tüm [Azure Storage](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk şifrelemesi**
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneğinde aşağıdaki veritabanı güvenlik önlemleri kullanır:
-   [AD kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve bir Azure depolama hesabında bunları Denetim günlüğüne yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyalarını bekletin.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) uygun izinlerin verildiğinden kadar veritabanı sunucularına tüm erişimi engelleme. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi için güvenlik uyarıları sağlayarak göründüklerinde algılamayı ve olası tehditler yanıta etkinleştirir desenler.
-   [Her zaman şifreli sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman veritabanı sistem içinde düz metin olarak göründüğünden emin olun. Veri şifreleme etkinleştirdikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlara erişimi ile düz metin verilere erişebilir.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kullanıcıların verileri işlemeyi durdurmaya erişimi kısıtlamak için ilkeler tanımlama olanak tanır.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) referans mimarisi dağıtıldıktan sonra yapılabilir. Müşteriler dinamik veri maskeleme ayarlarına kendi veritabanı şeması uyması için ayarlamanız gerekir.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojileri kimlik Azure ortamında yönetim yeteneklerini sağlar:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere AAD'de oluşturulur.
-   Uygulama kimlik doğrulama AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure SQL veritabanı uygulama kimliğini doğrulamak için Azure Active Directory kullanır. Daha fazla bilgi için bkz: nasıl yapılır [Azure SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve kullanıcı rolüne bağlı kaynaklara erişimi sınırlı olabilir.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) belirli bilgilere erişimi olan kullanıcıların sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management bulmak, kısıtlama ve ayrıcalıklı kimliklerinizi ve bunların kaynaklara erişimi izlemek için kullanabilirsiniz. Bu işlevsellik, gerektiğinde isteğe bağlı, yalnızca zaman yönetimsel erişim zorlamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşun kimlikleri etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır ve araştırır bunları çözmek için gerekli işlemleri yapmanıza şüpheli olaylar.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi** çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure anahtar kasası özellikleri verileri ve bu tür veri erişimi koruma müşterilere yardımcı olun:
- Gelişmiş erişim ilkeleri, bir gereksinim temelinde yapılandırılır.
- Anahtar kasası erişim ilkeleri, anahtarları ve gizli anahtarları için gerekli minimum izinleri ile tanımlanır.
- Tüm anahtarları ve gizli anahtar kasasında sona erme tarihi vardır.
- Anahtar kasası tüm anahtarlarında özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimini kullanarak minimum gerekli izinleri verilir.
- Tanılama günlüklerini anahtar kasası için en az 365 gün bekletme süresi ile etkinleştirilir.
- İzin verilen şifreleme işlemleri anahtarları için gerekli olanlar kısıtlanır.

**Uygulama ağ geçidi** mimarisi Web uygulaması güvenlik duvarı ve etkin OWASP ruleset ile bir uygulama ağ geçidi kullanarak güvenlik açıkları riskini azaltır. Ek özellikler şunları içerir:
- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS v1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
- [Engelleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [tanılama günlükleri](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Özel sistem durumu araştırmalarının](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme
Operations Management suite, sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. Operations Management suite [günlük analizi](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.
- **Etkinlik günlükleri**: [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemenize yardımcı olabilir geçişi ve durum süresi.
- **Tanılama günlüklerini**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından gösterilen tüm günlükler içerir. Bu günlükler Windows olayı sistem günlükleri, Azure Storage günlükleri, anahtar kasası denetim günlüklerini ve uygulama ağ geçidi erişimi ve güvenlik duvarı günlüklerini içerir.
- **Günlük arşivleme**: tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabı arşivleme yazma. Bekletme kuruluşa özgü bekletme gereksinimlerini karşılamak için kullanıcı tarafından 730 gün olarak yapılandırılabilir, ayarlama. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

Ayrıca, aşağıdaki Operations Management suite çözümleri Bu mimarinin bir parçası olarak eklenir:
-   [Active directory değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümünü kötü amaçlı yazılım, tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir. Bu çözümde, Application Insights ve Azure SQL veritabanı günlükleri toplamak runbook'lar yardımcı olur.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunları, algılama, tehdit bilgileri ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumu üst düzey bir fikir sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli listesi, müşteri sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetim çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmeleri durumunu ve gerekli güncelleştirmeleri yükleme işlemi de dahil olmak üzere müşteri yönetimine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümüne raporlarını kaç aracıları dağıtılan ve kullanıcıların coğrafi dağılımı yanı sıra, yanıt vermeyen kaç aracıları ve işletimsel veri gönderiliyor Aracı sayısına.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): etkinlik günlüğü analiz çözümü yardımcı olduğunu Azure etkinlik günlüğü Analizi ile bir müşteri için tüm Azure abonelikleri arasında.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking): müşterileri kolayca ortamında değişiklikleri belirlemek değişiklik izleme çözümü sağlar.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların izleyen performans, güvenlik korumak ve kuruluşların denetim, uyarılar oluşturabilir ve API çağrıları izleme de dahil olmak üzere, verileri arşivlemek etkinleştirerek eğilimleri belirlemenize yardımcı olur müşterilerin Azure kaynaklarında.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akış diyagramı kullanılabilir [karşıdan](https://aka.ms/fedrampPaaSWebAppDFD) veya altında bulunabilir. Bu model, müşteriler değişiklikler yaparken sistem altyapısında potansiyel risk puanları anlamanıza yardımcı olabilir.

![FedRAMP tehdit modeli için Web uygulaması PaaS](images/fedramp-paaswa-threat-model.png?raw=true "FedRAMP tehdit modeli için Web uygulaması PaaS")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenliği ve uyumluluk şeması - FedRAMP yüksek müşteri sorumluluk matrisi](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris her denetimi uyarlamasını Microsoft, müşteri sorumluluk veya ikisi arasında paylaşılan olup olmadığını gösterir.

[Azure güvenliği ve uyumluluk şeması - FedRAMP PaaS WebApp yüksek denetim uygulama matris](https://aka.ms/fedrampPaaSWebAppCIM) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris denetimleri ayrıntılı açıklamalar için nasıl uygulama kapsanan her denetim gereksinimlerini karşılayan biri de dahil olmak üzere PaaS web uygulama mimarisi tarafından ele alınan bilgiler sağlar.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu PaaS web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN ya da ExpressRoute ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve bir Azure sanal ağı arasında bir VPN bağlantısı oluşturulabilir. Bu bağlantının Internet üzerinden gerçekleşir ve güvenli bir şekilde Müşteri'nin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde "tünel" bilgisini müşterilerin sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bu seçeneği bir şifreleme mekanizması olarak kullanılır.

VPN tüneli içindeki trafiği Internet bir siteden siteye VPN ile geçiş olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute olduğu adanmış WAN Azure ve bir şirket içi konum veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Ayrıca, bu doğrudan bağlantı, müşterinin telekomünikasyon sağlayıcısının olduğundan, veri Internet üzerinden yolculuk değil ve kendisine bu nedenle olarak gösterilmez.

Azure için bir şirket içi ağ genişleten güvenli karma ağ uygulamak için en iyi uygulamalar sağlanmıştır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
