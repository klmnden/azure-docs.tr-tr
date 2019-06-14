---
title: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri analizi
description: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri analizi
services: security
author: jomolesk
ms.assetid: ca75d2a8-4e20-489b-9a9f-3a61e74b032d
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: f79ba9ae60454d4e73c914fc1c8af675a6d07d5d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60608862"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri analizi

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı, müşterilerin veri analizi mimarisi bir alt kümesini SP NIST 800-171 denetimleri uygulayan azure'da dağıtma yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yol gösterir. Ayrıca müşterilerin oluşturmak ve Azure'da kendi veri analizi çözümleri yapılandırma temeli olarak kullanılır.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak görev yapacak yöneliktir. Olarak kullanılmaması-üretim ortamıdır. Değişiklik yapmadan bu mimarisini dağıtma tamamen NIST SP 800-171 gereksinimlerini karşılamak için yeterli değil. Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi yerleşik herhangi bir çözümün bu mimari kullanır. Gereksinimleri her bir müşterinin uygulama ayrıntılarına göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, müşteriler, kendi analiz araçları oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, genel kullanım örneği açıklanmaktadır. Müşteriler, toplu veri içeri aktarmaları SQL/veri Yöneticisi tarafından veri girişi için kullanabilirsiniz. Bunlar, işletimsel veri güncelleştirmelerine işletimsel bir kullanıcı verilerine giriş için de kullanabilirsiniz. Her iki iş akışları, Azure SQL veritabanı'na veri almak için Azure işlevleri ekleyebilirsiniz. Azure işlevleri, müşterinin analiz gereksinimleri için benzersiz alma görevlerini işlemek için Azure Portalı aracılığıyla müşteri tarafından yapılandırılması gerekir.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, Azure Machine Learning Hizmetleri ve SQL veritabanı hızlı bir şekilde verilerine göz atın ve verilerinizin daha akıllı modelleme aracılığıyla daha hızlı sonuç sunmak için kullanır. Machine Learning, veri kümeleri arasında yeni ilişkiler bularak sorgu hızı artırmak için tasarlanmıştır. Başlangıçta, verileri çeşitli istatistik işlevleri aracılığıyla çalıştırılır. Daha sonra en fazla yedi ek sorgu havuzları sorgu iş yükü dağıtabilir ve yanıt sürelerini kısaltmak için aynı tablosal modelleri ile eşitlenebilir. Müşteri sunucunun sorgu havuzu toplam sekiz için getirir.

Gelişmiş analiz ve raporlama için SQL veritabanı ile sütun deposu dizinleri yapılandırılabilir. Machine Learning ve SQL veritabanı yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını öneririz.

Verileri SQL veritabanına karşıya ve makine öğrenimi tarafından geliştirilen sonra işletimsel kullanıcı ve Power BI ile SQL/veri Yöneticisi tarafından Sindirilmiş. Power BI verilerini sezgisel görüntüler ve bilgi ilişkin daha kapsamlı içgörüler çizmek için birden fazla veri kümesi arasında birlikte çeker. Power BI, yüksek düzeyde uyumluluk sahiptir ve SQL veritabanı ile kolayca tümleşir. Müşterilerin iş gereksinimlerini tarafından gerekli olan senaryoları çeşit işlemek için yapılandırabilirsiniz.

Çözümün tamamını müşteriler Azure portalından yapılandırdığınız Azure depolama üzerinde oluşturulmuştur. Depolama, bekleyen verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler. Müşterinin birincil veri merkezinde olumsuz bir olay veri kaybına neden olmayan coğrafi olarak yedekli depolama sağlar. İkinci bir kopyasını ayrı bir yerde mil uzaklıkta, yüzlerce depolanır.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Dağıtılan kaynakları (Azure AD) rol tabanlı erişim denetimi (RBAC) erişimi denetlemek için kullanılan Azure Active Directory. Bu kaynaklar, Azure anahtar Kasası'nda müşteri anahtarlarını içerir. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak için her iki izleme hizmetleri yapılandırın. Sistem durumu, kullanımı kolay tek bir Panoda görüntülenir.

SQL veritabanı, yaygın olarak SQL Server Management Studio yönetilir. SQL veritabanına güvenli bir VPN veya Azure ExpressRoute bağlantısı üzerinden erişmek için yapılandırılmış yerel makineden çalıştırır. *Bir VPN veya ExpressRoute bağlantısı için kaynak grubu yönetimi ve veri içe yapılandırmanızı öneririz*.

![Veri analizi için NIST SP 800-171 başvurusu mimari şeması](images/nist171-analytics-architecture.png "veri analizi için NIST SP 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için [dağıtım mimarisi](#deployment-architecture) bölümü.

- Application Insights
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Disk Şifrelemesi
- Azure Event Grid
- Azure İşlevleri
- Azure Key Vault
- Azure Machine Learning
- Azure İzleyici (günlük)
- Azure Güvenlik Merkezi
- Azure SQL Veritabanı
- Azure Storage
- Azure Sanal Ağ
    - (1) /16 ağ
    - (2) /24 ağlar
    - (2) ağ güvenlik grupları
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Event grid'i**: İle [Event Grid](https://docs.microsoft.com/azure/event-grid/overview), müşterilerin kolayca olay tabanlı mimariler ile uygulama oluşturun. Kullanıcılar, abone olmak istediğiniz Azure kaynağını seçin. Ardından, olay işleyicisi veya Web kancası olayı göndermek için bir uç nokta sağlar. Müşteriler, bir olay aboneliği oluşturmak için Web kancası URL'si sorgu parametreleri ekleyerek Web kancası uç noktalarını güvenli hale getirebilirsiniz. Event Grid yalnızca HTTPS Web kancası uç noktaları destekliyor. Event Grid ile müşteriler, çeşitli yönetim işlemlerini yapmak için farklı kullanıcılara verilen erişim düzeyini denetleyebilirsiniz. Kullanıcılar, olay abonelikleri listesi, yenilerini oluşturabilir ve anahtarları oluşturun. Event Grid, Azure RBAC kullanır.

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) kod üzerine çalışan bir sunucusuz işlem hizmetidir. Açıkça sağlamak veya altyapıyı yönetmek zorunda değilsiniz. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning hizmeti**: [Makine öğrenimi](https://docs.microsoft.com/azure/machine-learning/service/) mevcut verileri gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir.

**Azure veri Kataloğu**: [Veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynaklarını bulma ve verileri yöneten kullanıcılar tarafından anlamak daha kolay hale getirir. Genel veri kaynakları, etiketlenmiş ve veriler için Aranan kayıtlı. Veri kalırken, meta veri kopyası mevcut konumunda, veri Kataloğu'na eklenir. Veri kaynağı konumuna yönelik bir başvuru bulunur. Meta veriler, her veri kaynağı arama yoluyla bulmasını kolaylaştırmak üzere dizine alınır. Dizin oluşturma anlaşılır bulan kullanıcılar için kolaylaştırır.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri (Nsg'ler) içerir. Nsg bir alt ağ veya tek tek sanal makine düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Active Directory için bir NSG
  - İş yükü için bir NSG

Her nsg belirli bağlantı noktalarına sahiptir ve çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek protokollerini açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Azure İzleyici günlüklerine bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: Her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-in-transit"></a>Aktarım durumundaki veriler
Azure, Azure veri merkezleri gelen ve giden tüm iletişimi varsayılan olarak şifreler. Tüm depolama için Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: Bekleyen şifreli verileri gereksinimlerini karşılamak için tüm [depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu özellik, Kurumsal güvenlik ve uyumluluk gereksinimlerini 800-171 NIST SP tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğini kullanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Bu, gerçek zamanlı şifreleme ve şifre çözme veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları bekleyen bilgileri korumak için gerçekleştirir. Veriler güvencesi yetkisiz erişim olmamıştır saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) algılama ve yanıt olarak olası tehditleri ortaya çıktıkları sağlar. Şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim modellerinin güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Bu otomatik olarak olası hassas verileri bulabilir ve uygulanması için uygun maskeleri önerin. Dinamik veri maskeleme, hassas verilerin yetkisiz erişim veritabanı açıktan erişim azaltmaya yardımcı olur. *Müşteriler, kendi veritabanı şeması uyması ayarlarını sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsoft çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure AD'de oluşturulur ve SQL veritabanına erişim kullanıcıları içerir.
-   Azure AD'yi kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için bkz. nasıl [Azure AD ile uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Veritabanı sütun şifreleme Azure AD uygulamasını SQL veritabanına kimlik doğrulaması için de kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ayrıntılı erişim izinlerini tanımlamak için yöneticiler tarafından kullanılabilir. Kullanıcıların işlerini yapması için gereken Bununla, bunlar sadece erişim miktarını verebilirsiniz. Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) verileri gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek için müşteriler tarafından kullanılabilir. Yöneticiler, Azure AD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır. Aynı zamanda bunları gidermek için uygun eylemde için şüpheli olayları sorunu kısmen Araştırıyor.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Key Vault, şifreleme anahtarlarını koruyun ve bulut uygulamaları ve Hizmetleri tarafından kullanılan gizli yardımcı olur. Aşağıdaki anahtar kasası özellikleri müşteri verilerini korumaya yardımcı olur:
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

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, depolama günlükleri, anahtar kasası denetim günlüklerini ve Azure Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Kullanıcılar saklama süresi 730 gün belirli gereksinimleri karşılamak için en fazla yapılandırabilirsiniz.

**Azure İzleyici günlüklerine**: Günlükleri birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Veriler toplandıktan sonra Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, tüm veri ve böylece özgün kaynağına bakılmaksızın birlikte çözümlenebilir. Güvenlik Merkezi, Azure İzleyici günlükleri ile tümleştirilir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanabilirsiniz.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Bu, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Müşteriler, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı bildirir. Ayrıca, kaç aracının yanıt vermeyen ve işletimsel verileri gönderme aracı sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, SQL veritabanı'ndan günlüklerini toplama runbook'ları yardımcı olur. Müşteriler, Otomasyon kullanabileceğiniz [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek için çözüm.

**Azure İzleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve eğilimleri belirlemenize yardımcı olur. Kuruluşlar, denetleme, uyarı oluşturma ve verileri arşivlemek için kullanabilirsiniz. Ayrıca, Azure kaynaklarını API çağrılarında takip edebilirsiniz.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) birden çok platformda web geliştiricileri için genişletilebilir bir Application Performance Management hizmetidir. Performans anomalileri algılar ve güçlü analiz araçları içerir. Araçlar, sorunları tanılamanıza ve kullanıcıların uygulamayla ne yaptığını anlamanıza yardımcı yardımcı olur. Performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-analytics-tm) veya buradan ulaşabilirsiniz. Bu model system altyapısında potansiyel risk puanları, değişiklikler yaptığınızda anlamasına yardımcı olabilir.

![NIST SP 800-171 tehdit modeli için veri analizi](images/nist171-analytics-threat-model.png "NIST SP 800-171 tehdit modeli için veri analizi")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP veri analizi denetimi uygulama matris](https://aka.ms/nist171-analytics-cim) hangi NIST SP üzerinde 800-171 denetimleri ile veri analizi mimarisi açıklanmıştır bilgi sağlar. Bu, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları içerir.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu verileri analiz başvuru mimarisinin bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleşir. Müşteriler, şifreli bir bağlantı, ağ ve Azure arasında güvenli bir şekilde bilgilerin "tüneli" bağlantısını kullanabilirsiniz. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile geçtiğinden, Microsoft başka bir daha da güvenli bağlantı seçeneği sunar. ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları, bir müşterinin telekomünikasyon sağlayıcısına doğrudan bağlanın. Sonuç olarak, veriler Internet üzerinden yolculuk değil ve kendisine sunulan değil. Bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar. 

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Çıkartma-dönüştürme-yükleme işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) veri yükleme SQL veritabanı'na bir ayrı çıkartma-dönüştürme-yükleme gerek kalmadan veya aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. SQL Server ile uyumlu üçüncü taraf araçları ve Microsoft iş zekası ve analiz yığını PolyBase ile birlikte kullanılabilir.

### <a name="azure-ad-setup"></a>Azure AD Kurulumu
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşime geçen personelin erişimi sağlama için gereklidir. Şirket içinde Active Directory, Azure AD ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca Azure AD'ye dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) bağlayabilirsiniz. Bunu yapmak için bir alt etki alanı bir Azure AD ormanı dağıtılan Active Directory altyapısı olun.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
