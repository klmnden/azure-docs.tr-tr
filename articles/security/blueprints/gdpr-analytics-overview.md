---
title: Azure güvenlik ve uyumluluk şeması - Analytics GDPR için
description: Azure güvenlik ve uyumluluk şeması - Analytics GDPR için
services: security
author: jomolesk
ms.assetid: fe97b5e5-72d8-4930-bbe9-ead2b6ef3542
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: 9114dea1f95198b5ff8dc1bf759be5d1179885fd
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
ms.locfileid: "34162186"
---
# <a name="azure-security-and-compliance-blueprint-analytics-for-gdpr"></a>Azure güvenlik ve uyumluluk şeması: GDPR için analiz

## <a name="overview"></a>Genel Bakış
Birçok gereksinimlerini toplamak, depolamak ve nasıl kuruluşlar tanımlamak ve kişisel verilerinizi güvenli, saydamlık gereksinimlerini karşılayacak, algılamak ve rapor gibi kişisel bilgileri kullanma hakkında genel bir veri koruma düzenleme (GDPR) içerir kişisel veriler ihlallerini ve tren gizlilik personeli ve diğer çalışanlarla. GDPR kişiler kişisel verilerini üzerinde daha fazla denetim sağlar ve birçok yeni yükümlülüklerin toplamak, işleme veya kişisel verileri çözümlemek kuruluşlar üzerinde uygular. GDPR mal ve hizmet teklifi kuruluşlar yeni kurallarını uygular ve Avrupa Birliği (AB) ya da, kişilere Hizmetleri toplamak ve AB Satışlar bağlı verileri analiz etmek. Bir kuruluş bulunduğu nerede olursa olsun GDPR geçerlidir.

Microsoft Azure endüstri lideri güvenlik önlemleri ve gizlilik ilkeleri GDPR tarafından tanımlanan kişisel veri kategorileri dahil olmak üzere bulutta verilerinizi korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](http://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için Yürüt.

Bu Azure güvenliği ve uyumluluk şeması GDPR gereksinimleriyle yönetmenize yardımcı olan Azure veri analizi mimarisinde dağıtmak için yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimleri karşılayabilecek yollarını gösterir ve müşterilerin oluşturmak ve Azure'da kendi veri analizi çözümleri yapılandırmak bir temel görevi görür. Müşteriler bu başvuru mimarisinin kullanan ve Microsoft'un izleyin [dört adımlı işlem](https://aka.ms/gdprebook) GDPR uyumluluk için kendi gezisine içinde:
1. Keşfetme: kişisel veri var ve bu tanımlayın.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığı ve erişilebilir.
3. Koruma: engellemek, algılamak ve güvenlik açıkları ve veri ihlallerinden yanıt için güvenlik denetimleri kurun.
4. Rapor: gerekli belgeleri korumak ve veri isteklerini yönetme ve bildirimler ihlal.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli belirli gereksinimlerine uyarlamak müşteriler için bir temel görevi gören yöneliktir ve olarak kullanılmamalıdır-bir üretim ortamında. Lütfen şunlara dikkat edin:
- Mimarisi müşteri iş yükleri için Azure GDPR uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri
Bu çözüm bağlı müşteri, kendi analiz araçları oluşturabilir analiz platformu sunar. Başvuru mimarisine genel kullanım örneği burada müşteriler toplu veri içeri aktarmaları tarafından SQL/veri Yöneticisi veya bir işlemsel kullanıcı işletimsel veri güncelleştirmelerine aracılığıyla giriş özetlenmektedir. Her iki iş akışları, Azure SQL veritabanına veri almak için Azure işlevleri dahil. Azure işlevleri yapılandırılmış, her müşteri için kendi benzersiz alma görevlerini işlemek için Azure Portalı aracılığıyla müşteri tarafından analytics gereksinimleri.

Azure birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Ancak, bu çözümü hızlı bir şekilde verilerine göz atın ve Müşteri verilerinin daha akıllı modelleme üzerinden daha hızlı sonuçlar teslim etmek için Azure SQL veritabanı ile birlikte Azure Machine Learning hizmetleri içerir. Azure Machine Learning veri kümeleri arasında yeni ilişkiler bularak sorgu hızları artırmak için hedeflenen machine learning biçimidir. Verileri çeşitli istatistik işlevleri eğitilmiş sonra en çok 7 ek sorgu havuzları (müşteri server dahil olmak üzere toplam 8) sorgu iş yükü yaymak ve yanıt sürelerini azaltmak için aynı tablolu modeller ile eşitlenebilir.

Gelişmiş analiz ve raporlama için Azure SQL veritabanlarını columnstore dizinleri ile yapılandırılabilir. Azure SQL veritabanlarının ve Azure Machine Learning yukarı veya aşağı ölçeklendirilmiş veya müşteri kullanım yanıtta tamamen kapatırlar. Tüm SQL trafiği SSL ile otomatik olarak imzalanan sertifikalar ekleme şifrelenir. En iyi uygulama, Azure Gelişmiş Güvenlik güvenilen bir sertifika yetkilisi kullanılmasını önerir.

Verileri Azure SQL veritabanına karşıya ve Azure Machine Learning tarafından eğitilmiş sonra işletimsel kullanıcı ve Power BI ile SQL/Data Yönetici tarafından Sindirilmiş. Power BI veri sezgisel görüntüler ve bilgileri hakkında daha ayrıntılı bilgiler çizmek için birden fazla veri kümesi arasında birlikte çeker. Müşteriler çok çeşitli senaryoları iş gereksinimlerine gerektirdiği şekilde işlemek üzere yapılandırabilirsiniz, yüksek derecede uyumluluk ve Azure SQL veritabanı ile kolay tümleştirme sağlar.

Çözümün tamamında müşterilerin Azure portalından yapılandırdığınız Azure depolama bağlı yerleşik olarak bulunur. Azure Storage rest verileri gizliliğini korumak için depolama hizmeti şifrelemesi ile tüm verileri şifreler. Coğrafi olarak yedekli depolama (GRS), ikinci bir kopyası mil hemen bir ayrı konumda yüzlerce depolanacağı olarak Müşteri'nin birincil veri merkezindeki olumsuz bir olay veri kaybına neden olacağını değil sağlar.

Gelişmiş güvenlik için bu mimarinin Azure Active Directory ve Azure anahtar kasası ile kaynakları yönetir. Sistem durumu Operations Management Suite (OMS) ve Azure İzleyicisi izlenir. Müşteriler günlükleri yakalamak ve kolayca gezinebilir, tek bir Panoda sistem durumu görüntülemek için her iki izleme hizmetleri yapılandırın.

Azure SQL veritabanı ile SQL Server Management Studio (güvenli bir VPN ya da ExpressRoute bağlantısı aracılığıyla Azure SQL veritabanına erişmek için yapılandırılmış bir yerel makineden çalışan SSMS), yaygın olarak yönetilir. **Bir VPN ya da ExpressRoute bağlantısı yönetimi ve veri için yapılandırma azure önerir alma başvuru mimarisi kaynak grubu**.

![Analytics GDPR için başvuru mimarisi diyagramı](images/gdpr-analytics-architecture.png?raw=true "Analytics GDPR için başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını olan [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure İşlevleri
- Azure SQL Database
- Azure Machine Learning
- Azure Active Directory
- Azure Key Vault
- Operations Management Suite (OMS)
- Azure İzleyici
- Azure Storage
- Power BI Panosu
- Azure Veri Kataloğu
- Azure Güvenlik Merkezi
- Application Insights
- Azure Event Grid
- ağ güvenlik grubu

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılarını verir.

**Azure olay kılavuz**
[Azure olay kılavuz](https://docs.microsoft.com/en-us/azure/event-grid/overview) müşterilerin olay tabanlı mimari ile uygulamaları kolayca oluşturmanıza olanak tanır. Kullanıcıların abone olmak ve olay işleyicisi veya Web kancası için olay göndermek için bir uç nokta vermek istediğiniz Azure kaynağı seçin. Müşteriler, bir olay abonelik oluştururken, Web kancası URL'si sorgu parametreleri ekleyerek Web kancası uç noktaları güvenliğini sağlayabilirsiniz. Azure olay kılavuz yalnızca HTTPS Web kancası uç noktalarını destekler. Azure olay kılavuz listesi olay abonelikleri gibi çeşitli yönetim işlemlerini yapmak için yeni kampanya oluşturmak ve anahtarlar oluşturmak için farklı kullanıcılara verilen erişim düzeyini denetlemenize olanak tanır. Olay kılavuz Azure rol tabanlı erişim denetimi (RBAC) kullanır.

**Azure işlevleri**
[Azure işlevleri](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) açıkça sağlamak veya altyapısını yönetmek zorunda kalmadan talep kodu çalıştırmak kullanıcıların sağlayan bir sunucu daha az işlem hizmetidir. Çok çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmak için Azure İşlevleri’ni kullanın.

**Azure Machine Learning**
[Azure Machine Learning](https://docs.microsoft.com/en-us/azure/machine-learning/preview/) gelecekteki davranışları, sonuçları ve eğilimleri öngörmek için var olan verileri kullanmayı verdiğinden bir veri bilimi tekniğidir.

**Azure veri Kataloğu**: [veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynakları verileri yönetmek kullanıcılar tarafından kolayca bulunabilir ve anlaşılabilir hale getirir. Genel veri kaynakları kayıtlı, etiketlenmiş ve kişisel verileri aranır. Var olan konumunda, kendi meta verilerinin bir kopyasını ancak veri kalır ve veri kaynağı konumuna yönelik bir başvuru veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisinin 10.0.0.0/16 bir adres alanına sahip özel bir VNet tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden sanal ağ içinde trafik listeleri (ACL'ler) içerir. Nsg'ler alt ağ veya tek tek VM düzeyinde trafiğinin güvenliğini sağlamak için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Active Directory için bir NSG
  - İş yükü için bir NSG

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü güvenli bir şekilde ve doğru şekilde çalışabilmeniz için açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log) etkin ve bir depolama hesabında depolanır
  - OMS günlük analizi bağlı [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-in-transit"></a>Aktarımdaki verileri
Azure varsayılan olarak tüm iletişimler için ve Azure veri merkezleri şifreler. Azure Portalı aracılığıyla Azure depolama birimine tüm işlemleri HTTPS gerçekleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme, Denetim veritabanı ve diğer ölçülere kullanılmadıkları sırada verilerini korur.

**Azure depolama** şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure Storage](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu korumak ve kurumsal güvenlik taahhütleri ve GDPR tarafından tanımlanan uyumluluk gereksinimlerini desteklemek kişisel verilerinizi korumaya yardımcı olur.

**Azure Disk şifrelemesi**
[Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneğinde aşağıdaki veritabanı güvenlik önlemleri kullanır:
-   [AD kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve bir Azure depolama hesabında bunları Denetim günlüğüne yazar.
-   Azure SQL veritabanını kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyalarını bekletin. TDE, kişisel veriler güvence yetkisiz erişim ayarlanmadı sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) uygun izinlerin verildiğinden kadar veritabanı sunucularına tüm erişimi engelleme. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi için güvenlik uyarıları sağlayarak göründüklerinde algılamayı ve olası tehditler yanıta etkinleştirir desenler.
-   [Her zaman şifreli sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) önemli kişisel veriler hiç veritabanı sistem içinde düz metin olarak göründüğünden emin olun. Veri şifreleme etkinleştirdikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlara erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş Özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özel özellikler veritabanı nesnelerini eklemek ve "işlenmesini önlemek için uygulama mantığını destekleyecek şekilde Üretimi Durdurulmuş" veri etiketi kullanıcıların verir veri konuları işlenmesini durdurmaya kullanılabilir ilişkili kişisel veriler.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) kullanıcıların verileri işlemeyi durdurmaya erişimi kısıtlamak için ilkeler tanımlama olanak tanır.
- [SQL dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılar veya uygulamalar için veri maskeleme tarafından hassas kişisel veri görünürlüğünü sınırlar. DDM otomatik olarak olası hassas verileri bulmak ve uygulanması için uygun maskeleri önerir. Bu, kişisel verileri GDPR koruma ve veritabanı yoluyla yetkisiz erişimden çıkmaz şekilde erişim azaltmak için uygun tanımlaması ile yardımcı olur. **Not: Müşteriler DDM ayarlarını kendi veritabanı şemaya bağlı olması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojileri Azure ortamında kişisel verilere erişimi yönetmek için özellikleri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure SQL veritabanına erişen kullanıcılar dahil olmak üzere AAD'de oluşturulur.
-   Uygulama kimlik doğrulama AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure SQL veritabanı uygulamaya kimlik doğrulaması için AAD kullanır. Daha fazla bilgi için bkz: nasıl yapılır [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) kullanıcıların işlerini yapmak için gereksinim duyduğu erişim miktarını yalnızca vermek için ayrıntılı erişim izinlerini tanımlamak yöneticilerin sağlar. Her Azure kaynaklarını Kısıtlanmamış kullanıcı izinlerini vermek yerine, yöneticiler kişisel verilere erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik erişim Abonelik Yöneticisi sınırlıdır.
- [AAD ayrıcalıklı Kimlik Yönetimi (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kişisel verileri gibi belirli bilgilere erişimi olan kullanıcıların sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management bulmak, kısıtlama ve ayrıcalıklı kimliklerinizi ve bunların kaynaklara erişimi izlemek için kullanabilirsiniz. Bu işlevsellik, gerektiğinde isteğe bağlı, yalnızca zaman yönetimsel erişim zorlamak için de kullanılabilir.
-   [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) kuruluşun kimlikleri etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır ve şüpheli araştırır bunları gidermek için uygun eylemi gerçekleştirin olaylar.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi** çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure anahtar kasası özellikleri kişisel veri ve bu tür veri erişimi koruma müşterilere yardımcı olun:
- Gelişmiş erişim ilkeleri, bir gereksinim temelinde yapılandırılır.
- Anahtar kasası erişim ilkeleri, anahtarları ve gizli anahtarları için gerekli minimum izinleri ile tanımlanır.
- Tüm anahtarları ve gizli anahtar kasasında sona erme tarihi vardır.
- Anahtar kasası tüm anahtarlarında özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Tanılama günlüklerini anahtar kasası için en az 365 gün bekletme süresi ile etkinleştirilir.
- İzin verilen şifreleme işlemleri anahtarları için gerekli olanlar kısıtlanır.

**Güvenlik Uyarıları**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/en-us/azure/security-center/security-center-intro) müşterilerin trafiği izlemek, günlükleri toplamak ve veri kaynaklarını tehditleri analiz olanak verir. Ayrıca, Azure Güvenlik Merkezi, var olan yapılandırma ve güvenlik tutumunu geliştirmek ve kişisel verilerinizi korumaya yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin yapılandırmasına ilişkin erişir. Azure Güvenlik Merkezi içeren bir [tehdit Intelligence rapor](https://docs.microsoft.com/en-us/azure/security-center/security-center-threat-report) her yardımcı olmak üzere tehdit algılanan için olay yanıtlama takımlar araştırın ve tehditleri düzeltin.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [günlük analizi](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.
- **Etkinlik günlükleri**: [etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemenize yardımcı olabilir geçişi ve durum süresi.
- **Tanılama günlüklerini**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından gösterilen tüm günlükler içerir. Bu günlükler Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Günlük arşivleme**: tüm tanılama günlüklerini merkezi ve şifrelenmiş Azure depolama hesabı için tanımlanan bekletme süresi 2 gün ile arşivleme yazma. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

Ayrıca, aşağıdaki OMS çözümleri Bu mimarinin bir parçası olarak eklenir:
-   [AD değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümünü kötü amaçlı yazılım, tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunları, algılama, tehdit bilgileri ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumu üst düzey bir fikir sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli listesi, müşteri sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetim çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmeleri durumunu ve gerekli güncelleştirmeleri yükleme işlemi de dahil olmak üzere müşteri yönetimine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümüne raporlarını kaç aracıları dağıtılan ve kullanıcıların coğrafi dağılımı yanı sıra, yanıt vermeyen kaç aracıları ve işletimsel veri gönderiliyor Aracı sayısına.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): etkinlik günlüğü analiz çözümü yardımcı olduğunu Azure etkinlik günlüğü Analizi ile bir müşteri için tüm Azure abonelikleri arasında.
-   [Değişiklik izleme](https://docs.microsoft.com/en-us/azure/automation/automation-change-tracking): müşterileri kolayca ortamında değişiklikleri belirlemek değişiklik izleme çözümü sağlar.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) müşterilerin performans izleme, güvenlik korumak ve kuruluşların denetim, uyarılar oluşturabilir ve API izleme de dahil olmak üzere, verileri arşivlemek etkinleştirerek eğilimleri belirlemenize yardımcı olur müşterilerin Azure kaynaklarında çağırır.

**Application Insights**
[Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/) birden çok platformdaki web geliştiricileri için genişletilebilir bir uygulama performansı Yönetimi (APM) hizmetidir. Canlı web uygulamasını izlemek için kullanın. Performans anormalliklerini algılar ve sorunları tanılamak ve kullanıcıların gerçekte uygulamayla ne anlamak için güçlü analytics araçlar içerir. Kullanıcıların sürekli performansını ve kullanımını geliştirmemize yardımcı olmak için tasarlanmıştır.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisi için veri akış diyagramı kullanılabilir [karşıdan](https://aka.ms/gdprAnalyticsdfd) veya altında bulunabilir. Bu model, müşteriler değişiklikler yaparken sistem altyapısında potansiyel risk puanları anlamanıza yardımcı olabilir.

![Analytics GDPR için iş parçacığı modeli](images/gdpr-analytics-threat-model.png?raw=true "Analytics GDPR için iş parçacığı modeli")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenliği ve uyumluluk şeması – GDPR müşteri sorumluluk matrisi](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumluluklarını listeler. Lütfen Azure Hizmetleri için bir müşteri genellikle denetleyicisidir ve Microsoft işlemci olarak davranan unutmayın.

[Azure güvenliği ve uyumluluk şeması - GDPR veri analizi uygulama matris](https://aka.ms/gdprAnalytics) üzerinde hangi GDPR makaleler ele ayrıntılı açıklamaları nasıl dahil olmak üzere veri analizi mimarisi tarafından bilgiler sağlar Uygulama kapsanan her makalenin gereksinimleri karşılıyor.


## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu verileri bir parçası olarak dağıtılan kaynak bağlantı kurmak için yapılandırılacak analytics mimarisi başvuru. Müşteriler, uygun bir VPN ya da ExpressRoute ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve bir Azure sanal ağ arasında özel bir sanal bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden gerçekleşir ve güvenli bir şekilde Müşteri'nin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde "tünel" bilgisini müşterilerin sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bu seçeneği bir şifreleme mekanizması olarak kullanılır.

VPN tüneli içindeki trafiği Internet bir siteden siteye VPN ile geçiş olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute olduğu adanmış WAN Azure ve bir şirket içi konum veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Ayrıca, bu doğrudan bağlantı, müşterinin telekomünikasyon sağlayıcısının olduğundan, veri Internet üzerinden yolculuk değil ve kendisine bu nedenle olarak gösterilmez.

Azure için bir şirket içi ağ genişleten güvenli karma ağ uygulamak için en iyi uygulamalar sağlanmıştır [kullanılabilir](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Extract dönüştürme yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/en-us/sql/relational-databases/polybase/polybase-guide) veri yükleme Azure SQL veritabanına ayrı ETL gerek kalmadan veya aracı içeri aktarın. PolyBase T-SQL sorgularını verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, hem de üçüncü taraf araçları SQL Server ile uyumlu PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-whatis) dağıtımını yönetme ve ortam ile etkileşim personel erişimi sağlama için gereklidir. AAD'de ile varolan bir Windows Server Active Directory tümleşik [dört tıklama](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD bir AAD ormanı bir alt etki alanı dağıtılan Active Directory altyapısını yaparak bağlayabilirsiniz.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.
 - Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
 - Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
