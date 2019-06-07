---
title: Azure güvenlik ve uyumluluk planı - UK resmi iş yükleri için barındırma PaaS Web uygulaması
description: Azure güvenlik ve uyumluluk planı - UK resmi iş yükleri için barındırma PaaS Web uygulaması
services: security
author: jomolesk
ms.assetid: 446105ad-a863-44f5-a964-6ead1dac4787
ms.service: security
ms.topic: article
ms.date: 07/13/2018
ms.author: jomolesk
ms.openlocfilehash: e3ee5a0aa22d1231dca7d02a77d39e0a2b569314
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66753824"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-hosting-for-uk-official-workloads"></a>Azure güvenlik ve uyumluluk planı: UK resmi iş yükleri için barındırma PaaS Web uygulaması

## <a name="azure-security-and-compliance-blueprints"></a>Azure Güvenlik ve Uyumluluk Şemaları

Azure bir Blueprint'i Kılavuzu belgeleri ve akreditasyon veya uyumluluk gereksinimlerine sahip senaryolar için çözümler sunmak için bulut tabanlı mimariler dağıtın Otomasyon şablonları oluşur. Azure şemaları, Microsoft Azure müşterilerine iş hedeflerine aracılığıyla herhangi bir gereksinimi karşılamak için genişletilmiş bir foundation mimarisi sağlama sürecini hızlandırmaya izni Kılavuzu ve Otomasyon şablonu koleksiyonlarıdır.

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı sağlayan bir Microsoft Azure sunmak için rehberlik ve Otomasyon betikleri [(PaaS) hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) barındırılan sınıflandırılmış iş yüklerini işlemek için uygun web uygulaması mimarisi olarak [UK-OFFICIAL](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/715778/May-2018_Government-Security-Classifications-2.pdf). Bu güvenlik sınıflandırmasının oluşturulan veya Kamu sektörü tarafından işlenebilir bilgiler çoğunu kapsar. Bu yordamı işle ilgili işlemler ve hangi if kaybolur veya çalınırsa medya bazıları zararlı sonuçları olabilir, yayımlanan hizmetleri içerir. Tipik iş parçacığı profil resmi sınıflandırma için çok değerli bilgiler ve hizmetleri sağlayan özel bir iş aynıdır. UK resmi İngiltere veri ya da hizmetleri tehdit veya ile saldırganlar tarafından tehlikeye karşı korumak için gereken özellikleri ve kaynakları gibi sınırlanmış düşünmektedir (ancak bunlarla sınırlı değil) hactivists, single-issue baskısı grupları araştırma gazetecilerin, yetkin bireysel saldırganların ve çoğu cezai bireyler ve gruplar.

Bu plan, Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC tarafından) Gözden geçirildi ve NCSC 14 bulut güvenliği prensipleri hizalar.

Mimaride, Azure [bir hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) gider ve satın yazılım lisanslarını, uygulama altyapının yönetme karmaşasından kaçınmak müşterilerin izin veren bir ortam sunmak için bileşenleri ve ara yazılım veya geliştirme araçları ve diğer kaynaklar. Müşteriyi yönetiyorsanız geliştirme, hizmetleri ve uygulamaları Microsoft Azure sanal makineler, depolama gibi diğer Azure kaynaklarını yöneten artırabileceksiniz Kurumsal değer sunmaya ve ağ, daha fazla yerleştirme odaklanarak [bölümü Sorumluluk](https://docs.microsoft.com/azure/security/security-paas-deployments#division-of-responsibility) için altyapı yönetimi, Azure platformu açın. [Azure uygulama hizmetleri](https://azure.microsoft.com/services/app-service/) otomatik ölçeklendirme, yüksek kullanılabilirlik sunar, Windows ve Linux'ı destekler ve GitHub, Azure DevOps ya da herhangi bir Git deposu varsayılan Hizmetleri olarak otomatik dağıtımlar sağlar. Uygulama hizmetleri kullanarak aracılığıyla geliştiricilerin altyapı yönetme yükü olmadan Kurumsal değer sunmaya odaklanabilirsiniz. Sıfırdan yeni Java, PHP, Node.js, Python, HTML veya C# web uygulamaları oluşturun veya var olan bir buluta geçirmek için olası veya şirket içi web uygulamaları için Azure uygulama Hizmetleri (kapsamlı rağmen dikkatli olmanızı ve test nedeniyle performans gerekli olup olmadığını onaylamak için).

Bu plan güvenli bir temel sağlama üzerinde odaklanır [bir hizmet olarak platform](https://azure.microsoft.com/overview/what-is-paas/) genel ve de arka ofis kullanıcıları için web tabanlı arabirim. Bu şema tasarım senaryosu burada genel bir kullanıcı güvenli bir şekilde gönderme, görüntüleyebilir ve hassas verileri yönetmek, Hizmetleri web tabanlı Azure kullanımını barındırılan olarak kabul eder; Ayrıca arka ofis veya kamu işleci ortak kullanıcı gönderdiği hassas verileri güvenli bir şekilde işleyebilir. Bu senaryo için kullanım örnekleri içerebilir:

- Bir kullanıcı bir vergi iadesi gönderim işleme kamu işleciyle gönderiliyor;
- Web tabanlı bir uygulama üzerinden hizmeti doğrulanıyor ve hizmet sunmaya arka ofis kullanıcıyla isteyen bir kullanıcı; veya
- Bir kullanıcı arama ve genel etki alanı görüntüleme kamu hizmeti ile ilgili bilgiler yardımcı olur.

Kullanarak [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) şema şablonları ve Azure komut satırı arabirimi betikleri, Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC için) 14 uygun bir ortam dağıtır [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) ve Internet güvenliği (CIS) için merkezi [kritik güvenlik denetimleri](https://www.cisecurity.org/critical-controls.cfm). Hizmeti güvenlik özelliklerini değerlendirmek ve Sorumluluk müşteri ve sağlayıcı arasında bölme anlamanıza yardımcı olmak için kendi bulut güvenliği prensipleri müşteriler tarafından kullanılabilir NCSC önerir. Microsoft sorumlulukları bölme daha iyi anlamanıza yardımcı olması için bu ilkelerin her biri karşı bilgiler sağlamıştır. Bu mimari, karşılık gelen Azure Resource Manager şablonları ve Microsoft teknik incelemeyi tarafından desteklenen [UK için 14 bulut güvenlik denetimleri kullanarak Microsoft Azure bulut](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1). Bu mimari, NCSC tarafından gözden geçirilir ve UK NCSC 14 bulut güvenlik Hızlı Gönderim yeteneklerini genel ve Birleşik Krallık'ta bir üzerinde bulut tabanlı hizmetler kullanarak uyumluluk yükümlülükler karşılamak Kamu sektörü kuruluşları bu nedenle Etkinleştirme ilkeleri ile uyumludur Microsoft Azure bulut. Bu şablon, iş yükü için altyapıyı dağıtır. Uygulama kodu ve iş katmanı ve veri katmanı yazılım destekleyen yüklenmeli ve müşteriler tarafından yapılandırılır. Ayrıntılı dağıtım yönergeleri [burada](https://aka.ms/ukofficial-paaswa-repo/).

Bu şema foundation mimaridir. Müşterilerimiz, bu şema resmi sınıflandırma web tabanlı iş yüklerini için temel olarak kullanın ve şablonları ve kaynakları kendi gereksinimler ile genişletin. Bu şema yapıları ilkeleri üzerinde [UK OFFICAL üç katmanlı Iaas Web uygulamaları blueprint](https://aka.ms/ukofficial-iaaswa) müşterilerimize hem sunmaya [altyapı (ıaas) olarak](https://azure.microsoft.com/overview/what-is-iaas/) ve PaaS uygulama seçenekleri Web tabanlı iş yüklerini barındırmak için.

Bu şema dağıtmak için bir Azure aboneliği gereklidir. Azure aboneliğiniz yoksa, hızlı ve kolay bir şekilde ücretsiz olarak kaydolabilirsiniz: Azure'u kullanmaya başlayın. Tıklayın [burada](https://aka.ms/ukofficial-paaswa-repo/) dağıtım yönergeleri.

## <a name="architecture-and-components"></a>Mimarisi ve bileşenleri

Bu şema barındırma çözümünüzü UK resmi iş yüklerini destekleyen bir Azure bulut ortamında bir web uygulaması sağlar. Mimari, bir hizmet özellikleri Azure platformundan yararlanır güvenli bir ortam sunar. Ortam içinde iki App Service web apps dağıtılan (genel kullanıcılar için bir) ve bir web ön ucu için iş hizmetleri sağlamak arka ofis kullanıcıları için bir API uygulaması katmanla var. Azure SQL veritabanı, uygulama için yönetilen ilişkisel bir veri deposu olarak dağıtılır. Bu bileşenlerden platformu dışında olan ve bu bileşenler arasındaki bağlantı, Azure Active Directory tarafından kimliği doğrulanmış erişim ile veri taşıma gizlilik emin olmak için TLS 1.2 ile şifrelenir.

![PaaS Web uygulama barındırma başvuru mimarisi diyagramı UK resmi iş yükleri için](images/ukofficial-paaswa-architecture.png?raw=true "PaaS Web uygulama barındırma başvuru mimarisi diyagramı UK resmi iş yükleri için")

Dağıtım mimarisi, güvenli depolama sağlama, izleme ve günlüğe kaydetme, Birleşik güvenlik yönetimi & Gelişmiş tehdit koruması ve Yönetimi kapsamında özellikleri de müşteriler için gereken tüm araçları olmasını sağlamak için dağıtılır güvenli ve bu çözümü kendi ortamlarından izleyin.

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Active Directory
- App Service
- Web Uygulaması
- API App
- Azure DNS
- Key Vault
- Azure İzleyici (günlük)
- Application Insights
- Azure Resource Manager
- Azure Güvenlik Merkezi
- Azure SQL Veritabanı
- Azure Storage

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

### <a name="security"></a>Güvenlik

#### <a name="identity-and-authentication"></a>Kimlik ve kimlik doğrulaması

Bu plan, kaynaklara erişimi dizin ve Kimlik Yönetimi Hizmetleri aracılığıyla korunmasını sağlar. Bu mimari tam olarak kullanmayı sağlar [kimlik güvenlik çevresi olarak](https://docs.microsoft.com/azure/security/security-paas-deployments). 

Aşağıdaki teknolojileri kimlik yönetimi özellikleri Azure ortamında sağlar:

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.
- Azure AD kullanarak web uygulaması ve erişim yönetimini Azure kaynakları için yan yana işleci kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- Veritabanı sütun şifreleme, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure AD kullanır. Daha fazla bilgi için [Always Encrypted: SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- Web uygulaması karşılıklı Vatandaşlık genel erişim için yapılandırılır. Hesap oluşturma ve kimlik doğrulaması active directory aracılığıyla veya sosyal kimlik sağlayıcıları ağ için izin veren [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) gerekirse tümleştirilebilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) algılar olası güvenlik açıklarını ve riskli hesapları kuruluşunuzun kimliklerini güvenlik duruşunu geliştirmek için öneriler, otomatik yanıtları algılanan yapılandırır kuşkulu eylemleri için kuruluşunuzun kimliklerini, ilgili ve şüpheli olaylar araştırır ve bunları gidermek için uygun tedbiri alır.
- [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklanmış erişim yönetimi sağlar. Abonelik erişimi için abonelik yöneticisine sınırlıdır ve anahtar yönetimi erişmesi gereken kullanıcılar için Azure anahtar kasası erişimi sınırlıdır.
- Yararlanarak aracılığıyla [Azure Active Directory koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) müşteriler, ek güvenlik denetimleri uygulamalara veya kullanıcılara ortamlarında konum, cihaz, durum ve oturum gibi belirli koşullara göre erişim zorunlu risk.
- [Azure DDoS koruması](https://docs.microsoft.com/azure/security/security-paas-deployments#security-advantages-of-a-paas-cloud-service-model) uygulama tasarım en iyi yöntemleri ile birlikte, her zaman açık trafik izleme, DDoS saldırılarına ve gerçek zamanlı azaltma ortak ağ düzeyinde saldırı karşı koruma sağlar. PaaS mimarisi ile platform düzeyi DDoS koruması müşteriye saydam ve platform içine eklenen ancak uygulama güvenlik tasarım sorumluluk müşteriyle kaynaklandığını dikkat edin önemlidir.

#### <a name="data-in-transit"></a>Aktarım durumundaki veriler

Veriler aktarım sırasında dışında ve Azure bileşenleri arasında kullanılarak korunan [Aktarım Katmanı Güvenliği/Güvenli Yuva Katmanı (TLS/SSL)](https://www.microsoft.com/TrustCenter/Security/Encryption), olarak iletişimleri şifrelemek için simetrik şifreleme paylaşılan bir parolaya dayanan kullanır ağ üzerinden yolculuk ediyor. Varsayılan olarak, TLS 1.2 kullanarak ağ trafiği güvenlik altına alınır.

#### <a name="security-and-malware-protection"></a>Güvenlik ve kötü amaçlı yazılımdan koruma

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimlerinin uygulandığını ve doğru bir şekilde yapılandırıldığını ve ilgilenmenizi gerektiren tüm kaynakları hızla tanımlayın doğrulayabilirsiniz.

[Azure Danışmanı](https://docs.microsoft.com/azure/advisor/advisor-overview) olan yardımcı olan kişiselleştirilmiş bulut Danışmanı, Azure dağıtımlarınızın iyileştirilmesine yönelik en iyi uygulamaları izleyin. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan bir gerçek zamanlı koruma özelliğidir. Bu varsayılan olarak, temel alınan PaaS sanal makine altyapısı üzerinde yüklü ve şeffaf bir şekilde müşteriye Azure fabric tarafından yönetilir.

### <a name="paas-services-in-this-blueprint"></a>Bu şema PaaS Hizmetleri

#### <a name="azure-app-service"></a>Azure uygulama hizmeti

Azure App Service barındırma ortamı geliştirilen Java, PHP, Node.js, Python, HTML web uygulaması için tam olarak yönetilen bir web sağlar ve C# altyapıyı yönetmek zorunda kalmadan. Otomatik ölçeklendirme sunar ve yüksek kullanılabilirlik, hem Windows hem de Linux destekler ve otomatik dağıtımlar sağlar [Azure DevOps](https://azure.microsoft.com/services/visual-studio-team-services/) veya herhangi bir Git tabanlı deposundan.

App Service, [ISO, SOC ve PCI uyumlu](https://www.microsoft.com/TrustCenter/) ve ile kullanıcıların kimliklerini doğrulayabilirsiniz [Azure Active Directory](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-aad) veya sosyal oturum açma ([Google](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-google), [Facebook](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-facebook), [Twitter](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-twitter), ve [Microsoft kimlik doğrulama](https://docs.microsoft.com/azure/app-service/configure-authentication-provider-microsoft).

Temel, standart ve Premium planlar, üretim iş yüklerine yöneliktir ve adanmış sanal makine örneklerinde çalıştırılır. Her örnek, birden çok uygulama ve etki alanlarını destekleyebilir. Uygulama Hizmetleri ayrıca Destek [IP adresi sınırlamaları](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions) gerekirse güvenilen IP adreslerine trafiği güvenli hale getirmek için ve ayrıca [kimliklerini Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/app-service/overview-managed-identity) güvenli bağlantı için diğer PaaS Hizmetleri gibi [anahtar kasası](https://azure.microsoft.com/services/key-vault/) ve [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/). Ek güvenlik gerekli olduğu yalıtılmış planımız, uygulamaları özel, adanmış bir Azure ortamında barındırır ve şirket içi ağa veya ek performans ve ölçek ile güvenli bağlantılar gerektiren uygulamalar için idealdir.

Bu şablon, aşağıdaki App Service özellikleri dağıtır:

- [Standart](https://docs.microsoft.com/azure/app-service/overview-hosting-plans) App Service planı katmanı
- Birden fazla App Service [dağıtım yuvalarını](https://docs.microsoft.com/azure/app-service/deploy-staging-slots): Geliştirme, Önizleme, QA, UAT ve Elbette üretim (varsayılan yuva).
- [Kimlikler Azure kaynakları için yönetilen](https://docs.microsoft.com/azure/app-service/overview-managed-identity) bağlanmak için [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) (Bu erişimi sağlamak için de kullanılabilir [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) 
- İle tümleştirme [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps) performansını izlemek için
- [Tanılama Günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) 
- Ölçüm [uyarıları](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) 
- [Azure API Apps](https://azure.microsoft.com/services/app-service/api/) 

#### <a name="azure-sql-database"></a>Azure SQL Veritabanı

SQL Veritabanı, Microsoft Azure'da yer alan ve ilişkisel veri, JSON, uzamsal ve XML gibi yapıları destekleyen çok amaçlı ilişkisel veritabanı yönetilen hizmetidir. SQL veritabanı teklifler yönetilen tek SQL veritabanları, içinde yönetilen SQL veritabanları bir [elastik havuz](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool)ve SQL [yönetilen örnekler](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) (genel önizlemede). [Dinamik olarak ölçeklenebilir performans](https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) sunan bu hizmet çok büyük ölçekli analitik analiz ve raporlama için [columnstore dizinleri](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) gibi seçenekler, raporlama ve çok büyük ölçekli işlemler için [bellek içi OLTP](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory) özelliklerine sahiptir. Microsoft, SQL kod tabanıyla ilgili tüm düzeltme ve güncelleştirme işlerini sorunsuz olarak yaparak altyapı yönetimini tamamen soyutlar.

Azure SQL veritabanı'nda bu şema

Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:

- [Sunucu düzeyinde ve veritabanı düzeyinde güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure), aracılığıyla veya [sanal ağ hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) kullanarak [sanal ağ kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview).
- [Saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) yardımcı olur, kötü amaçlı etkinlik tehdide karşı Azure SQL veritabanı ve Azure veri ambarı koruyun. Bu gerçek zamanlı şifreleme ve şifre çözme veritabanını, ilişkili yedeklemeler ve işlem günlük dosyaları bekleme sırasında uygulamada değişiklik gerektirmeden gerçekleştirir.
- [Azure AD kimlik doğrulaması](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication), veritabanı kullanıcıları ve tek bir merkezi konumda diğer Microsoft Hizmetleri kimliklerini merkezi olarak yönetebilir. Merkezi kimlik yönetimi, veritabanı kullanıcıları yönetmek için tek bir yerde sağlar ve izin yönetimini kolaylaştırır.
- Veritabanı yönetimi için Azure Active Directory kullanın
- [Denetim günlükleri](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) depolama hesaplarına
- Ölçüm [uyarılar](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) için DB bağlantıları başarısız oldu
- [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection)
- [Her zaman şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault)

### <a name="azure-storage"></a>Azure Storage

Microsoft [Azure depolama](https://azure.microsoft.com/services/storage/) yüksek oranda kullanılabilir, güvenli, dayanıklı, ölçeklenebilir ve yedekli depolama sağlayan bir Microsoft tarafından yönetilen bir bulut hizmetidir. Azure Depolama; Blob depolama, Dosya Depolama ve Kuyruk depolama hizmetlerinden oluşur.

#### <a name="azure-storage-in-this-blueprint"></a>Azure depolama alanında bu şema

Bu şablon aşağıdaki Azure depolama bileşenleri kullanır:

- [Depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) 
- Yalnızca HTTPS bağlantılarına izin ver

#### <a name="data-at-rest"></a>Bekleyen veriler

Aracılığıyla [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) 256 bit AES şifreleme en güçlü blok şifreleme özelliklerinden biri Azure Depolama'ya yazılan tüm veriler şifrelenir. SSE ile kullanabileceğiniz Microsoft tarafından yönetilen bir şifreleme anahtarları veya kullanabileceğiniz [kendi şifreleme anahtarlarınızı](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Depolama hesapları güvenli aracılığıyla [sanal ağ hizmet uç noktaları](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) kullanarak [sanal ağ kuralları](https://docs.microsoft.com/azure/storage/common/storage-network-security).

Azure depolama güvenliğini sağlama hakkında ayrıntılı bilgi bulunabilir [Güvenlik Kılavuzu](https://docs.microsoft.com/azure/storage/common/storage-security-guide).


### <a name="secrets-management"></a>Gizlilik Yönetimi

#### <a name="azure-key-vault"></a>Azure Key Vault

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) üçüncü tarafların uygulama anahtarları ve gizli anahtarları erişilebilir olduklarını değil emin olmak için güvenli hale getirmek için kullanılır. Key Vault, kullanıcı parolaları için depo olarak kullanılmaya yönelik değildir. Kasa adlı birden fazla güvenli kapsayıcı oluşturmanıza imkan tanır. Bu kasalar, donanım güvenlik modülleri (HSM) tarafından desteklenir. Kasalar, uygulama gizli dizilerinin depolanmasını merkezi hale getirerek güvenlik bilgilerini kazayla kaybetme olasılığını azaltmaya yardımcı olur. Anahtar Kasaları ayrıca içlerinde depolanmış her şeye erişimi denetler ve günlüğe kaydeder. Azure Key Vault, sağlam bir sertifika yaşam döngüsü yönetim çözümü için gereken özellikleri sağlayarak Aktarım Katmanı Güvenliği (TLS) sertifikalarını isteme ve yenileme işlemlerini gerçekleştirebilir.

#### <a name="azure-key-vault-in-this-blueprint"></a>Azure anahtar Kasası'nda bu şema

- Verilen okuma erişimli depolama erişim anahtarı tutan [yönetilen kimliği](https://docs.microsoft.com/azure/app-service/overview-managed-identity) müşteriye dönük web uygulaması
- SQL Server DBA parolası (içinde ayrı bir kasa) tutar.
- Tanılama günlükleri

### <a name="monitoring-logging-and-audit"></a>İzleme, günlüğe kaydetme ve Denetim

#### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri

[Azure İzleyici günlüklerine](https://azure.microsoft.com/services/log-analytics/) ve şirket içi Ortamlarınızdaki bulutunuzdaki kaynaklar tarafından oluşturulan verileri toplayıp analiz yardımcı olan bir Azure hizmetidir.

#### <a name="azure-monitor-logs-in-this-blueprint"></a>Azure İzleyici, bu blueprint'te günlüğe kaydeder

- SQL Değerlendirmesi
- Anahtar kasası tanılama
- Application Insights bağlantı
- Azure etkinlik günlüğü

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Bunu otomatik olarak performans anomalilerini algılamayı, performansı çözümleyebilir, tanılama canlı web uygulamaları izlemek için sorunları ve anlamak için kullanıcıların uygulamayla etkileşimini kullanılır. .NET, Node.js gibi platformlarda Application Insights dağıtılabilir ve Java EE barındırılan şirket içinde veya bulutta. DevOps işleminizle tümleştirilir ve çeşitli geliştirme araçlarıyla bağlantı noktaları vardır.

#### <a name="application-insights-in-this-blueprint"></a>Bu plan, Application Insights

Bu şablon, aşağıdaki Application Insights bileşenlerini kullanır:

- Application Insights Panosu (işleç, müşteri ve API) site başına

#### <a name="azure-activity-logs"></a>Azure etkinlik günlükleri

[Azure etkinlik günlüğü](https://docs.microsoft.com/azure/azure-monitor/platform/activity-logs-overview) Abonelikleriniz için Denetim düzlemi olaylarını denetler. Etkinlik günlüğü'nü kullanarak belirleyebilirsiniz ' ne, kim ve ne zaman ' işlemlerini (PUT, POST, DELETE), aboneliğinizdeki kaynaklar üzerinde gerçekleştirilen herhangi bir yazma için. Ayrıca, işlemi ve ilgili diğer özellikleri durumunu anlayabilirsiniz.

#### <a name="azure-monitor"></a>Azure İzleyici

[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) ölçümler, etkinlik günlükleri ve tanılama günlükleri koleksiyonunu sağlayarak Azure Hizmetleri için çekirdek izleme sağlar. Azure İzleyici, çoğu Microsoft Azure’daki çoğu hizmet için temel düzey altyapı ölçümleri ve günlükleri sağlar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/ukofficial-paaswa-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![PaaS Web uygulamasını barındıran UK resmi iş yükleri tehdit modeli için](images/ukofficial-paaswa-threat-model.png?raw=true "PaaS Web uygulamasını barındıran tehdit modeli UK resmi iş yükleri için")

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>Bulut güvenliği prensipleri NCSC uyumluluk belgeleri

Dama yapma ticari hizmeti (ticari ve tedarik etkinliği tarafından kamu artırmak için çalışan Aracısı) resmi düzeyinde tüm teklifleri kapsayan Microsoft kapsamındaki Kurumsal bulut hizmetlerine G-Cloud v6 sınıflandırmasını yenilendi. Azure G-Cloud ve ayrıntıları bulunabilir [Azure UK G-Cloud güvenlik değerlendirme özeti](https://www.microsoft.com/trustcenter/compliance/uk-g-cloud).

Bu şema içinde NCSC belgelenen 14 bulut güvenliği prensipleri hizalar [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) UK resmi sınıflandırılan iş yüklerini destekleyen bir ortam sağlamak için.

[Azure güvenlik ve uyumluluk planı - UK resmi müşteri sorumluluk matris](https://aka.ms/ukofficial-crm) (Excel çalışma kitabı) tüm 14 bulut güvenliği prensipleri listeler ve her ilke (veya ilke Altbölüm) gösterir, ilke Uygulama, Microsoft, müşterinin sorumluluğundadır veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - UK resmi ilke uygulaması matris PaaS Web uygulaması](https://aka.ms/ukofficial-paaswa-pim) (Excel çalışma kitabı) tüm 14 bulut güvenliği prensipleri listeler ve her ilke (veya ilke Altbölüm) gösterir tasarlanmış bir müşteri sorumluluk müşteri sorumlulukları Matristeki, 1) ilke ve uygulama ile İlkesi gereksinimlerin nasıl hizalandığını 2) bir açıklama blueprint uygular.  

Ayrıca, bulut güvenliği İttifakı (CSA) bulut denetim matrisi bulut sağlayıcılarının veriyi değerlendirmede müşterileri desteklemek ve bulut Hizmetleri taşımadan önce yanıtlanması soru tanımlamak için yayımladı. Yanıt olarak, Microsoft Azure CSA fikir birliğine varılmış değerlendirme girişimi anketi yanıtlanmış ([CSA CAIQ](https://www.microsoft.com/TrustCenter/Compliance/CSA)), Microsoft önerilen ilkeler nasıl ele açıklar.

## <a name="third-party-assessment"></a>Üçüncü taraf değerlendirmesi

Bu plan, Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC tarafından) Gözden ve NCSC 14 bulut güvenliği prensipleri hizalar

Otomasyon şablonları bizim Microsoft iş ortağı ve UK müşteri başarı birim Azure bulut çözümü Mimarı takım tarafından test edilmiştir [Ampliphae](https://www.ampliphae.com/).


## <a name="deploy-the-solution"></a>Çözümü dağıtma

Azure güvenlik ve uyumluluk şema Otomasyon JSON yapılandırma dosyaları ve kaynakları azure'da dağıtmak için Azure Resource Manager'ın API hizmeti tarafından işlenen PowerShell betikleri oluşur. Ayrıntılı dağıtım yönergeleri [burada](https://aka.ms/ukofficial-paaswa-repo).

Dağıtım için üç yaklaşımları sağlanmadı; Basit bir "express" [Azure CLI 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) hızla bir test oluşturmak için uygun ortam; bir parametreli [Azure CLI 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) yaklaşım ve bir Azure portalında iş yükü ortamlarında için; büyük yapılandırması sağlama Azure portal aracılığıyla dağıtım parametreleri işleci belirleyebileceğiniz dağıtım temel. 

1.  Kopyala veya indir [bu](https://aka.ms/ukofficial-paaswa-repo) yerel iş istasyonunuzu GitHub deposuna.
2.  Gözden geçirme [1. yöntem: Azure CLI 2 (Express sürüm)](https://aka.ms/ukofficial-paaswa-repo/#method-1-azure-cli-2-express-version) ve sağlanan komutları yürütün.
3.  Gözden geçirme [yöntemi 1a: Azure CLI (komut bağımsız değişkenleri yoluyla dağıtımı yapılandırma) 2](https://aka.ms/ukofficial-paaswa-repo/#method-1a-azure-cli-2-configuring-the-deployment-via-script-arguments) ve sağlanan komutları yürütme
4.  Gözden geçirme [yöntem 2: Azure portalında dağıtım işlemi](https://aka.ms/ukofficial-paaswa-repo/#method-2-azure-portal-deployment-process) ve listelenen komutları yürütme

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="api-management"></a>API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) önünde API App Service, ek güvenlik katmanları, azaltma ve kullanıma sunmak, proxy ve API'ların korunması için denetimleri sağlamak için kullanılabilir.

### <a name="azure-b2c"></a>Azure B2C

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) kaydetmek kullanıcılara izin vermek için bir denetim bir kimlik oluşturmak ve etkinleştirme yetkilendirme ve erişim denetimi için genel bir web uygulaması olarak uygulanabilir.

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
- Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
- Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
- Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
- Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
