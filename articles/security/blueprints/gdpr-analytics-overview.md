---
title: Azure güvenlik ve uyumluluk planı - Analytics GDPR için
description: Azure güvenlik ve uyumluluk planı - Analytics GDPR için
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 3d15e747c129d2591f4cc70030d1cf858bcee49e
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51237663"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Azure güvenlik ve uyumluluk planı: GDPR için analiz

## <a name="overview"></a>Genel Bakış
Genel veri koruma yönetmeliği (GDPR) birçok gereksinimleri hakkında toplamak, depolamak ve nasıl kuruluşların tanımlamak ve kişisel verilerin güvenliğini sağlama, saydamlık gereksinimlerini karşılamak, algılamak ve rapor dahil olmak üzere, kişisel bilgileri kullanarak içerir kişisel veri ihlallerine ve eğitme gizlilik personeli ve diğer çalışanlarla. GDPR, kişilere kişisel verileri üzerinde daha fazla denetim verir ve birçok yeni yükümlülükler ile toplamak, tanıtıcı veya kişisel verileri analiz kuruluşlar uygular. GDPR mal teklif kuruluşlara yeni kurallarını uygular ve Hizmetleri kişilere Avrupa Birliği (AB) veya AB vatandaşlar için bağlı verileri toplayıp analiz. GDPR, kuruluşun bulunduğu yeri bağımsız olarak geçerlidir.

Microsoft Azure ile sektör lideri güvenlik önlemleri ve gizlilik ilkeleri, kişisel verilerin GDPR ile tanımlanan kategoriler dahil olmak üzere buluttaki verileri korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](https://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için sözleşme temelli taahhüt.

Azure güvenlik ve uyumluluk planı GDPR gereksinimlerine yardımcı olur. Azure veri analizi mimaride dağıtma konusunda rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi veri analizi çözümleri yapılandırmak bir temel olarak görev yapar. Müşteriler, bu başvuru mimarisinde yazılımınız ve Microsoft'un izleyin [dört adım işlemi](https://aka.ms/gdprebook) YOLCULUĞUNA GDPR uyumluluğuna içinde:
1. Keşfetme: kişisel verilerin var ve yer aldığı belirleyin.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığını ve erişilebilir.
3. Koruma: önlemenize, algılamanıza ve bu güvenlik açıklarına ve veri ihlallerini yanıt için güvenlik denetimleri oluştur.
4. Rapor: gereken belgeleri tutmak ve veri isteklerini yönetme ve güvenlik ihlali bildirimleri.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a GDPR ile uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, müşteriler, kendi analiz araçları oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, burada müşteriler giriş verileri toplu veri içeri aktarmaları SQL/veri Yöneticisi tarafından veya bir işletimsel kullanıcı işletimsel veri güncelleştirmelerine aracılığıyla genel kullanım örneği açıklanmaktadır. Her iki iş akışları, Azure SQL veritabanı'na veri almak için Azure işlevleri ekleyebilirsiniz. Azure işlevleri, her müşteri için kendi benzersiz alma görevlerini işlemek için Azure Portalı aracılığıyla müşteri tarafından yapılandırılmalıdır analizi gereksinimleri.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak, bu çözüm, Azure Machine Learning services verilerine hızla göz atın ve Müşteri verilerinin daha akıllı modelleme aracılığıyla daha hızlı sonuçlar teslim etmek için Azure SQL veritabanı ile birlikte içerir. Azure Machine Learning, machine Learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızı artırmak için hedeflenen bir biçimidir. Veriler çeşitli istatistik işlevleri aracılığıyla eğitim almış sonra en fazla 7 ek sorgu havuzları (müşteri sunucu dahil olmak üzere toplam 8) sorgu iş yükü dağıtabilir ve yanıt sürelerini kısaltmak için aynı tablosal modelleri ile eşitlenebilir.

Azure SQL veritabanları, Gelişmiş analiz ve raporlama için columnstore dizinleri ile yapılandırılabilir. Hem Azure Machine Learning ve Azure SQL veritabanları, yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Verileri Azure SQL veritabanı'na yüklenen ve Azure Machine Learning tarafından geliştirilen sonra hem işletimsel kullanıcı hem de Power BI ile SQL/veri yönetici tarafından Sindirilmiş. Power BI verilerini sezgisel görüntüler ve bilgi ilişkin daha kapsamlı içgörüler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler, iş gereksinimlerini gerektirdiği gibi senaryoları çeşit işlemek için yapılandırabilir, yüksek düzeyde uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Tüm çözüm, müşterilerin Azure portalından yapılandırdığınız Azure depolama bağlı oluşturulmuştur. Azure depolama, bekleyen verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler. Coğrafi olarak yedekli depolama (GRS), ikinci bir kopyasını ayrı bir yerde mil uzaklıkta, yüzlerce depolanacağı gibi olumsuz olaya müşterinin birincil veri merkezinde veri kaybına oluşturmayacaktır sağlar.

Gelişmiş güvenlik için bu mimari Azure Active Directory ve Azure anahtar kasası ile kaynaklarını yönetir. Sistem durumu, Log Analytics ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı ile SQL Server Management Studio (güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış yerel makineden çalışan SSMS), yaygın olarak yönetilir. **Yönetim ve veriler için bir VPN veya ExpressRoute bağlantısı yapılandırma azure önerir başvuru mimarisi kaynak grubuna içe**.

![Analytics GDPR için başvuru mimarisi diyagramı](images/gdpr-analytics-architecture.png?raw=true "Analytics GDPR için başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure İşlevleri
- Azure SQL Database
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Log Analytics
- Azure İzleyici
- Azure Storage
- Power BI Panosu
- Azure Veri Kataloğu
- Azure Güvenlik Merkezi
- Application Insights
- Azure Event Grid
- ağ güvenlik grubu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Event Grid**
[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) müşterilere, olay tabanlı mimariler ile uygulamaları kolayca oluşturmanıza olanak tanır. Kullanıcılar için abone olur ve olay işleyicisi veya Web kancası olayı göndermek için bir uç nokta vermek istediğiniz Azure kaynağını seçin. Müşteriler, bir olay aboneliği oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek Web kancası uç noktaları güvenliğini sağlayabilirsiniz. Azure Event Grid, yalnızca HTTPS Web kancası uç noktaları destekliyor. Azure Event Grid olay abonelikleri listesi gibi çeşitli yönetim işlemlerini yapmak için yenilerini oluşturun ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetleme olanağı sunar. Event Grid, Azure rol tabanlı Access Control (RBAC) kullanır.

**Azure işlevleri**
[Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı kullanıcıların sağlayan bir sunucusuz işlem hizmetidir. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning**
[Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) mevcut verileri gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir.

**Azure veri Kataloğu**: [veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynakları verileri yöneten kullanıcılar tarafından kolayca bulunabilmesini ve anlaşılır hale getirir. Genel veri kaynakları, etiketlenmiş ve kişisel verileri için arama kayıtlı. Var olan konumunda, ancak kendi meta verilerinin bir kopyasını veriler kalırken, veri kaynağı konumuna yönelik bir başvuru veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden bir sanal ağ içinde trafiği listeleri (ACL) içeriyor. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Active Directory için bir NSG
  - İş yükü için bir NSG

Her nsg sahip belirli bağlantı noktaları ve protokoller çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Log Analytics'e bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama** şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini GDPR tarafından tanımlanan desteklemek üzere kişisel verileri koruyarak yardımcı olur.

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
- [SQL dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılara veya uygulamalar için veri maskeleme tarafından kişisel hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. DDM otomatik olarak olası hassas verileri keşfedin ve uygulanması için uygun maskeleri önerin. Bu, kişisel verilerin GDPR koruma ve yetkisiz erişim veritabanı yok, erişim azaltmak için uygun kimlik ile yardımcı olur. **Not: Müşterilerin DDM ayarlarını kendi veritabanı şemasına bağlı kalması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında kişisel verilere erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar, Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere, AAD içinde oluşturulur.
-   Uygulama kimlik doğrulaması, AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için AAD veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Yöneticiler, Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine kişisel verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kişisel veriler gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
-   [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve şüpheli araştırır olaylar, bunları gidermek için uygun eylemi gerçekleştirebilir.

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

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

[Log Analytics](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Günlük arşivleme**: tüm tanılama günlükleri, bir merkezi ve şifrelenmiş Azure depolama hesabına arşivleme 2 gün tanımlanmış tutma süresine sahip yazma. Bu günlükler, işleme, depolama ve Panosu raporlama için Azure Log Analytics'e bağlayın.

Ayrıca, aşağıdaki izleme çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): depolar, çalıştırır ve runbook'ları yöneten Azure Otomasyon çözümü.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetimi çözümü, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerini müşteri yönetimi sağlar.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking): müşterilerin ortamındaki değişiklikler kolayca belirlemek değişiklik izleme çözümü sağlar.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) müşterilerin performans izleme, güvenliği sağlamak ve kuruluşların denetleme, uyarı oluşturma ve API izleme de dahil olmak üzere, verileri arşivlemek etkinleştirerek eğilimleri belirlemenize yardımcı olur. müşterilerin Azure kaynaklarında çağırır.

**Application Insights**
[Application Insights](https://docs.microsoft.com/azure/application-insights/) birden çok platformlardaki web geliştiricilerine yönelik genişletilebilir bir uygulama performans yönetimi (APM) hizmetidir. Bunu, canlı web uygulamasını izlemek için kullanabilirsiniz. Performans anomalileri algılar ve sorunlarının tanılanmasına yardımcı olmak ve kullanıcıların gerçekten uygulamayla neler anlamak için güçlü analiz araçları içerir. Performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/gdprAnalyticsdfd) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![Analytics GDPR için tehdit modeli](images/gdpr-analytics-threat-model.png?raw=true "Analytics GDPR için tehdit modeli")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: GDPR müşteri sorumluluk matris](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumlulukları listeler. Azure Hizmetleri için bir genellikle denetleyicisi ve Microsoft işlemcisi olarak davranır olduğunu lütfen unutmayın.

[Azure güvenlik ve uyumluluk planı - GDPR veri analizi uygulaması matris](https://aka.ms/gdprAnalytics) hangi GDPR hakkında makaleler ayrıntılı açıklamaları nasıl dahil olmak üzere veri analizi mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama her kapsanan bir makaleyi gereksinimlerini karşılar.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu verileri bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılacak analytics mimarisi başvurusu. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Çıkartma-dönüştürme-yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) yük verileri Azure SQL veritabanı'na ayrı bir ETL gerek kalmadan veya aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, AAD içinde ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD AAD ormandaki bir alt etki alanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
