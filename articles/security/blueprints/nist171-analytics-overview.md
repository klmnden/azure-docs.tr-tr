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
ms.openlocfilehash: b2c96509b2b8fd61c1814d5b4a015048eeb1807d
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391791"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri analizi

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı, müşterilerin veri analizi mimarisi bir alt kümesini SP NIST 800-171 denetimleri uygulayan azure'da dağıtma yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi veri analizi çözümleri yapılandırmak bir temel olarak görev yapar.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmaması-üretim ortamıdır. Değişiklik yapmadan bu mimarisini dağıtma tamamen NIST SP 800-171 gereksinimlerini karşılamak için yeterli değil. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, müşteriler, kendi analiz araçları oluşturabileceğiniz bir analiz platformu sağlar. Başvuru mimarisi, burada müşteriler giriş verileri toplu veri içeri aktarmaları SQL/veri Yöneticisi tarafından veya bir işletimsel kullanıcı işletimsel veri güncelleştirmelerine aracılığıyla genel kullanım örneği açıklanmaktadır. Her iki iş akışları, Azure SQL veritabanı'na veri almak için Azure işlevleri ekleyebilirsiniz. Azure işlevleri, müşterinin analiz gereksinimleri için benzersiz alma görevlerini işlemek için Azure Portalı aracılığıyla müşteri tarafından yapılandırılması gerekir.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak, bu çözüm, Azure Machine Learning services verilerine hızla göz atın ve verilerin daha akıllı modelleme aracılığıyla daha hızlı sonuç sunmak için Azure SQL veritabanı ile birlikte içerir. Azure Machine Learning, machine Learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızı artırmak için hedeflenen bir biçimidir. Veriler çeşitli istatistik işlevleri aracılığıyla eğitim almış sonra en fazla 7 ek sorgu havuzları (müşteri sunucu dahil olmak üzere toplam 8) sorgu iş yükü dağıtabilir ve yanıt sürelerini kısaltmak için aynı tablosal modelleri ile eşitlenebilir.

Azure SQL veritabanı, Gelişmiş analiz ve raporlama için sütun deposu dizinleri ile yapılandırılabilir. Azure Machine Learning ve Azure SQL veritabanı hem yukarı veya aşağı ölçeklendirilebilir veya yanıt olarak müşteri kullanımını tamamen kapatabilmek. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Verileri Azure SQL veritabanı'na yüklenen ve Azure Machine Learning tarafından geliştirilen bir kez, her iki işlem kullanıcı tarafından Sindirilmiş ve Power BI ile SQL/veri Yöneticisi. Power BI verilerini sezgisel görüntüler ve bilgi ilişkin daha kapsamlı içgörüler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler, iş gereksinimlerini gerektirdiği gibi senaryoları çeşit işlemek için yapılandırabilir, yüksek düzeyde uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Çözümün tamamını müşteriler Azure portalından yapılandırdığınız Azure depolama üzerinde oluşturulmuştur. Azure depolama, bekleyen verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler. İkinci bir kopyasını ayrı bir yerde mil uzaklıkta, yüzlerce depolanacağı gibi olumsuz olaya müşterinin birincil veri merkezinde veri kaybına neden olmayan coğrafi olarak yedekli depolama sağlar.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Rol tabanlı erişim denetimi erişimi denetlemek için kullanılan Azure Active Directory, Azure anahtar Kasası'nda kendi anahtarları gibi kaynakları dağıtıldı. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı, yaygın olarak güvenli bir VPN veya ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış yerel makineden çalışan SQL Server Management Studio aracılığıyla yönetilir. **Yönetim ve veriler için bir VPN veya ExpressRoute bağlantısı yapılandırma Microsoft önerir içeri aktarma, kaynak grubuna**.

![Veri analizi için NIST SP 800-171 başvurusu mimari şeması](images/nist171-analytics-architecture.png "veri analizi için NIST SP 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Diğer ayrıntılar için bkz [dağıtım mimarisi](#deployment-architecture) bölümü.

- Application Insights
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Disk Şifrelemesi
- Azure Event Grid
- Azure İşlevleri
- Azure Key Vault
- Azure Machine Learning
- Azure İzleyici
- Azure Güvenlik Merkezi
- Azure SQL Database
- Azure Storage
- Azure Log Analytics
- Azure Sanal Ağ
    - (1) /16 ağ
    - (2) /24 ağlar
    - (2) ağ güvenlik grupları
- Power BI Panosu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure Event Grid**: [Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) müşterilere, olay tabanlı mimariler ile uygulamaları kolayca oluşturmanıza olanak tanır. Kullanıcıların abone ve olay işleyicisi veya Web kancası olayı göndermek için bir uç nokta vermek istediğiniz Azure kaynağını seçin. Müşteriler, bir olay aboneliği oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek Web kancası uç noktaları güvenliğini sağlayabilirsiniz. Azure Event Grid, yalnızca HTTPS Web kancası uç noktaları destekliyor. Azure Event Grid olay abonelikleri listesi gibi çeşitli yönetim işlemlerini yapmak için yenilerini oluşturun ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetleme olanağı sunar. Azure rol tabanlı erişim denetimi Event grid'i kullanır.

**Azure işlevleri**: [Azure işlevleri](https://docs.microsoft.com/azure/azure-functions/functions-overview) , açıkça sağlamak veya altyapıyı yönetmek zorunda kalmadan kodu isteğe bağlı kullanıcıların sağlayan bir sunucusuz işlem hizmetidir. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning**: [Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/) mevcut verileri gelecekteki davranışları, sonuçları ve eğilimleri öngörmelerini sağlayan bir veri bilimi tekniğidir.

**Azure veri Kataloğu**: [veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynakları verileri yöneten kullanıcılar tarafından kolayca bulunabilmesini ve anlaşılır hale getirir. Genel veri kaynakları, etiketlenmiş ve veriler için Aranan kayıtlı. Var olan konumunda, ancak kendi meta verilerinin bir kopyasını veriler kalırken, veri kaynağı konumuna yönelik bir başvuru veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:
  - Active Directory için ağ güvenlik grubu
  - İş yükü için ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Log Analytics'e bağlı olduğu [ağ güvenlik grubunun tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen ağ güvenlik grubu ile ilişkilidir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure, Azure veri merkezlerinden tüm iletişimi varsayılan olarak şifreler. Tüm Azure Depolama'ya Azure portalı üzerinden HTTPS gerçekleşir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimarisi, bekleyen veri şifrelemesi, Denetim veritabanı ve diğer ölçüler verilerinizi korumanızı sağlar.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption).  Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini 800-171 NIST SP tarafından tanımlanan desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. Veriler güvencesi yetkisiz erişim ayarlanmamış saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Dinamik veri maskeleme, otomatik olarak olası hassas verileri bulun ve uygulanması için uygun maskeleri önerin. Bu, hassas verilerin yetkisiz erişim veritabanı yok, erişim azaltmaya yardımcı olur. Müşteriler, dinamik veri maskeleme, veritabanı şeması uyması için ayarları ayarlamak için sorumludur.

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure Active kullanıcılar Azure SQL veritabanına erişim de dahil olmak üzere dizininde, oluşturulur.
-   Azure Active Directory kullanan uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için Azure Active Directory veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine, yöneticiler verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) verileri gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, Azure Active Directory Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
-   [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve araştırır bunları gidermek için uygun eylemde için şüpheli olayları.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşteri verilerini korumaya yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Anahtar türü, bir donanım güvenlik modülleri korumalı 2048 bit RSA anahtarı değil.
- Tüm kullanıcılar ve kimlikler rol tabanlı erişim denetimi kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Azure Güvenlik Merkezi**: ile [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

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

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) birden çok platformda web geliştiricileri için genişletilebilir bir Application Performance Management hizmetidir. Performans anomalileri algılar ve sorunlarının tanılanmasına yardımcı olmak ve kullanıcıların uygulamayla ne yaptığını anlamak için güçlü analiz araçları içerir. Performans ve kullanılabilirliği sürekli geliştirmenize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-analytics-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![NIST SP 800-171 tehdit modeli için veri analizi](images/nist171-analytics-threat-model.png "NIST SP 800-171 tehdit modeli için veri analizi")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP veri analizi denetimi uygulama matris](https://aka.ms/nist171-analytics-cim) hangi NIST SP üzerinde 800-171 denetimleri data analytics mimarisi tarafından açıklanmıştır bilgi sağlar dahil olmak üzere Uygulama her kapsanan denetim gereksinimlerini nasıl karşıladığı, ayrıntılı açıklamaları.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu verileri bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılacak analytics mimarisi başvurusu. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Çıkartma-dönüştürme-yükleme işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) Azure SQL veritabanı'na veri yükleme ayıklamak için ayrı bir gereksinimi olmadan, dönüştürme, yükleyebilir ve aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, Azure Active Directory ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) var olan bir Azure Active Directory alt etki alanı bir Azure Active Directory ormanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
