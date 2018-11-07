---
title: Azure güvenlik ve uyumluluk planı - için GDPR veri ambarı
description: Azure güvenlik ve uyumluluk planı - için GDPR veri ambarı
services: security
author: jomolesk
ms.assetid: 7a33fb3b-cfb6-49dd-ab4d-c53cf0d5da41
ms.service: security
ms.topic: article
ms.date: 05/14/2018
ms.author: jomolesk
ms.openlocfilehash: d1099b813e84cd4885b011dec205a1e631fc6599
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51250722"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-gdpr"></a>Azure güvenlik ve uyumluluk planı: GDPR için veri ambarı

## <a name="overview"></a>Genel Bakış
Genel veri koruma yönetmeliği (GDPR) birçok gereksinimleri hakkında toplamak, depolamak ve nasıl kuruluşların tanımlamak ve kişisel verilerin güvenliğini sağlama, saydamlık gereksinimlerini karşılamak, algılamak ve rapor dahil olmak üzere, kişisel bilgileri kullanarak içerir kişisel veri ihlallerine ve eğitme gizlilik personeli ve diğer çalışanlarla. GDPR, kişilere kişisel verileri üzerinde daha fazla denetim verir ve birçok yeni yükümlülükler ile toplamak, tanıtıcı veya kişisel verileri analiz kuruluşlar uygular. GDPR mal teklif kuruluşlara yeni kurallarını uygular ve Hizmetleri kişilere Avrupa Birliği (AB) veya AB vatandaşlar için bağlı verileri toplayıp analiz. GDPR, kuruluşun bulunduğu yeri bağımsız olarak geçerlidir.

Microsoft Azure ile sektör lideri güvenlik önlemleri ve gizlilik ilkeleri, kişisel verilerin GDPR ile tanımlanan kategoriler dahil olmak üzere buluttaki verileri korumak için tasarlanmıştır. Microsoft'un [sözleşme koşullarını](https://aka.ms/Online-Services-Terms) Microsoft işlemci gereksinimleri için sözleşme temelli taahhüt.

Azure güvenlik ve uyumluluk planı, veri ambarı mimarisi GDPR gereksinimleriyle yönetmenize yardımcı olan azure'da dağıtma konusunda rehberlik sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yolunu gösterir ve müşterilerin oluşturmak ve Azure'da kendi veri ambarı çözümleri yapılandırmak bir temel olarak görev yapar. Müşteriler, bu başvuru mimarisinde yazılımınız ve Microsoft'un izleyin [dört adım işlemi](https://aka.ms/gdprebook) YOLCULUĞUNA GDPR uyumluluğuna içinde:
1. Keşfetme: kişisel verilerin var ve yer aldığı belirleyin.
2. Yönetme: nasıl kişisel verileri yöneten kullanıldığını ve erişilebilir.
3. Koruma: önlemenize, algılamanıza ve bu güvenlik açıklarına ve veri ihlallerini yanıt için güvenlik denetimleri oluştur.
4. Rapor: gereken belgeleri tutmak ve veri isteklerini yönetme ve güvenlik ihlali bildirimleri.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır. Lütfen şunlara dikkat edin:
- Mimari müşteriler iş yüklerini Azure'a GDPR ile uyumlu bir şekilde dağıtmak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: bir yere veri aktarılır, depolanır ve bir kümelenmiş SQL ortamı ve başka bir Azure SQL veri ambarı için hazırlanmış bir ETL aracını kullanarak veri yüklüyse (örneğin [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)T-SQL sorguları) işlemek için. Analytics, verileri Azure SQL veri ambarı'nda depolandıktan sonra çok büyük ölçekte çalıştırabilirsiniz.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, Azure SQL veri ambarı raporları hızlı oluşturulması için SQL Server Raporlama Hizmetleri (SSRS) içerir. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Azure SQL veri ambarındaki ilişkisel tabloları sütunlu depolama, sorgu performansını artırırken veri depolama maliyetlerini önemli ölçüde azaltan bir biçim depolanır.  Kullanım gereksinimlerine bağlı olarak, Azure SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya işlem kaynakları gerektiren herhangi bir etkin işlem yoksa tamamen kapatabilmek.

SQL bir yük dengeleyici SQL trafik, yüksek bir performans sağlamaya yönetir. Bu başvuru mimarisindeki tüm sanal makinelerin yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri için bir AlwaysOn Kullanılabilirlik grubuna yapılandırılmış SQL Server örnekleri ile kullanılabilirlik kümesi dağıtın.

Bu veri ambarı başvuru mimarisi, yapı içinde kaynaklarının yönetimi için bir Active Directory (AD) katmanı da içerir. Active Directory alt büyük ormanına erişimi kullanılabilir olsa bile ortamı sürekli işlem için izin verme daha büyük bir AD ormanı yapısı altında kolay benimsenmesini sağlar. Tüm sanal makineler, Active Directory katmanı için etki alanı ile birleşik ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları zorlamak için Active Directory grup ilkeleri kullanın.

Bir sanal makine, yöneticilerin erişim dağıtılan kaynaklara güvenli bir bağlantı sağlayan bir yönetim savunma ana görevi görür. Veri hazırlama alanı aracılığıyla bu yönetim Burcu ana bilgisayarı yükler. **Azure başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya Azure ExpressRoute bağlantısı yapılandırma önerir.**

![Veri ambarı için GDPR başvurusu mimari şeması](images/gdpr-datawarehouse-architecture.png?raw=true "veri ambarı için GDPR başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

-   Azure Sanal Makineler
    - (1) Burcu ana bilgisayarı
    -   (2) active Directory etki alanı denetleyicisi
    -   (2) SQL Server küme düğümü
    -   (1) SQL Server tanığı
-   Kullanılabilirlik Kümeleri
    - Active Directory etki alanı denetleyicileri
    - SQL küme düğümleri ve Tanık
-   Sanal Ağ
    - (4) alt ağlar
    -   (4) ağ güvenlik grupları
- SQL Veri Ambarı
- SQL Server Raporlama Hizmetleri
- Azure SQL yük dengeleyici
- Azure Active Directory
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Log Analytics
- Azure Veri Kataloğu
- Azure Güvenlik Merkezi

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) olan bir kurumsal veri ambarı (yüksek düzeyde paralel işleme (hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için MPP) kullanan kullanıcılara izin verme EDW) Kişisel verileri verimli bir şekilde belirleyin. Kullanıcılar, SQL veri ambarı'na büyük veri almak ve yüksek performanslı bir analiz çalıştırılacak MPP gücünden yararlanmak için basit PolyBase T-SQL sorguları kullanabilir.

**SQL Server Raporlama Hizmetleri (SSRS)**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) tabloları, grafikler, haritalar, ölçerler, matrisler ve Azure SQL veri ambarı için daha hızlı oluşturulmasını raporlar sağlar.

**Veri Kataloğu**: [veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynakları verileri yöneten kullanıcılar tarafından kolayca bulunabilmesini ve anlaşılır hale getirir. Genel veri kaynakları, etiketlenmiş ve kişisel verileri için arama kayıtlı. Var olan konumunda, ancak kendi meta verilerinin bir kopyasını veriler kalırken, veri kaynağı konumuna yönelik bir başvuru veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

**Kale ana bilgisayarı**: Burcu ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu (NSG) tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Log Analytics uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Nsg'ler](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) erişim denetimi izin veren veya reddeden bir sanal ağ içinde trafiği listeleri (ACL) içeriyor. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL yük dengeleyici) için bir NSG
  - Yönetim Burcu ana bilgisayarı için bir NSG
  - Active Directory için bir NSG
  - SQL Server Raporlama Hizmetleri için bir NSG

Her nsg sahip belirli bağlantı noktaları ve protokoller çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
  - Log Analytics'e bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, şifreleme ve veritabanı denetimi gibi birden çok ölçü kullanılmadıkları verilerini korur.

**Azure depolama**: şifrelenmiş verileri rest gereksinimleri karşılamak için tüm [Azure depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu, kuruluş güvenlik ve uyumluluk gereksinimlerini GDPR tarafından tanımlanan desteklemek üzere kişisel verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğidir yararlanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleştirilir.

**Azure SQL veritabanı**: Azure SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [AD kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), gerçek zamanlı şifreleme ve şifre çözme veritabanının, ilişkili yedeklemeler gerçekleştirir ve bilgileri korumak için işlem günlüğü dosyaları bekletin. TDE, depolanmış kişisel veriler güvencesi yetkisiz erişim ayarlanmamış sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim için güvenlik uyarıları sağlayarak oluşunca algılama ve olası tehditlere yanıt sağlar. desenler.
-   [Always Encrypted sütunları](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas kişisel verileri hiçbir zaman içinde bir veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) özelliği veri sahipleri işlenmesini kesmek için kullanılabilir veritabanı nesneleri için özel özellikler ekleme ve veri "önlemek için uygulama mantığını destekleyecek şekilde artık Üretilmiyor" etiketi kullanıcılara verdiğinden ilişkili kişisel verilerin işlenmesini.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) işleme devam etmek için veri erişimini kısıtlamak için ilkeler tanımlamak üzere kullanıcıların sağlar.
- [SQL veritabanı dinamik veri maskeleme (DDM)](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ayrıcalıklı olmayan kullanıcılara veya uygulamalar için veri maskeleme tarafından kişisel hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. DDM otomatik olarak olası hassas verileri keşfedin ve uygulanması için uygun maskeleri önerin. Bu, kişisel verilerin GDPR koruma ve yetkisiz erişim veritabanı yok, erişim azaltmak için uygun kimlik ile yardımcı olur. **Not: Müşterilerin DDM ayarlarını kendi veritabanı şemasına bağlı kalması gerekir.**

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında kişisel verilere erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar, SQL veritabanına erişen kullanıcılar dahil olmak üzere, AAD içinde oluşturulur.
-   Uygulama kimlik doğrulaması, AAD kullanılarak gerçekleştirilir. Daha fazla bilgi için [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, Azure SQL veritabanı uygulamaya kimlik doğrulaması için AAD veritabanı sütun şifreleme kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) yöneticilerin kullanıcıların işlerini yapması için gereken sadece erişim miktarını vermek için ayrıntılı erişim izinleri tanımlamanıza olanak tanır. Yöneticiler, Azure kaynakları için her sınırsız kullanıcı izinleri vermek yerine kişisel verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [AAD Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) kişisel veriler gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek müşterilerin sağlar.  Yöneticiler, AAD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [AAD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar, bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır ve şüpheli araştırır olaylar, bunları gidermek için uygun eylemi gerçekleştirebilir.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Key Vault özellikleri müşterilerin kişisel veriler ve böyle verilere erişimin korunmasına yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri (HSM'ler) tarafından korunur. Anahtar bir HSM korumalı anahtarlar 2048 bit RSA anahtarı türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım, yapılandırılabilir uyarı Kaldır gerçek zamanlı koruma özelliği sağlar. bilinen kötü amaçlı veya istenmeyen yazılım yükleme veya korumalı sanal makineler üzerinde çalışmayı denediğinde olduğunda.

**Güvenlik Uyarıları**: [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro) müşterilerin trafiği izlemek için günlükleri toplayıp veri kaynakları için tehdit analiz sağlar. Ayrıca, Azure Güvenlik Merkezi, mevcut yapılandırma ve güvenlik duruşunu ve kişisel verilerini korumaya yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin yapılandırmasına ilişkin erişir. Azure Güvenlik Merkezi içeren bir [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her algılanan tehdit yardımcı olmak için olay yanıt ekiplerinin tehdit araştırma ve düzeltme.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'daki sanal makinelerin yüksek kullanılabilirlik sağlamak için. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den daha hızlı geri yükleme süreleri etkinleştirme tüm VM'yi geri yüklemeden geri yükleyebilirsiniz.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim
[Log Analytics](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview) sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. [Log Analytics](https://azure.microsoft.com/services/log-analytics/) çözüm toplar ve Azure içinde kaynaklar tarafından oluşturulan verileri analiz eder ve şirket içi Ortamlarınızdaki.
- **Etkinlik günlükleri**: [etkinlik günlüklerini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri ve Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Arşivleme oturum**: tüm tanılama günlükleri, bir merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazma. Bekletme kuruluşa özgü saklama gereksinimlerini karşılamak için kullanıcı-730 gün için yapılandırılabilir,. Bu günlükler, işleme, depolama ve Panosu raporlama için Azure Log Analytics'e bağlayın.

Ayrıca, aşağıdaki Log Analytics çözümleri Bu mimarinin bir parçası olarak dahil edilir:
-   [AD değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümü, kötü amaçlı yazılım tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): depolar, çalıştırır ve runbook'ları yöneten Azure Otomasyon çözümü.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunlar, algılamalar, tehdit zekası ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumuyla ilgili bir yüksek düzeyde öngörü sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir ve müşterilerin Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetimi çözümü, kullanılabilir güncelleştirmelerin durumunu ve gerekli güncelleştirmeleri yükleme işlemi dahil olmak üzere işletim sistemi güvenlik güncelleştirmelerini müşteri yönetimi sağlar.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı yanı sıra yanıt vermeyen aracı sayısı ve işletimsel veriler gönderen aracıların sayısını raporlar.
-   [Azure etkinlik günlüklerini](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümünü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle destekler.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): müşterilerin ortamındaki değişiklikler kolayca belirlemek değişiklik izleme çözümü sağlar.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/gdprDWdfd) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![GDPR tehdit modeli için veri ambarı](images/gdpr-datawarehouse-threat-model.png?raw=true "GDPR tehdit modeli için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: GDPR müşteri sorumluluk matris](https://aka.ms/gdprCRM) tüm GDPR makaleler için denetleyici ve işlemci sorumlulukları listeler. Azure Hizmetleri için bir genellikle denetleyicisi ve Microsoft işlemcisi olarak davranır olduğunu lütfen unutmayın.

[Azure güvenlik ve uyumluluk planı - GDPR veri ambarı uygulaması matris](https://aka.ms/gdprDW) hangi GDPR hakkında makaleler ayrıntılı açıklamaları nasıl dahil olmak üzere veri ambarı mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama her kapsanan bir makaleyi gereksinimlerini karşılar.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleştirilir ve şifreli bir bağlantı müşterinin ağınız ve Azure arasında güvenli bir şekilde "tüneli" bilgilerin müşterilere sağlar. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcıları, doğrudan bir bağlantı olduğu için veriler Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-etl-process"></a>Çıkartma-dönüştürme-yükleme (ETL) işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) yük verileri Azure SQL veri ambarı'na ayrı bir ETL gerek kalmadan veya aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, AAD içinde ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) için mevcut bir AAD AAD ormandaki bir alt etki alanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

### <a name="optional-services"></a>İsteğe bağlı hizmetler
Azure Hizmetleri depolama ve biçimlendirilmiş ve biçimlendirilmemiş veri hazırlama ile yardımcı olmak üzere çeşitli sunar. Aşağıdaki hizmetler bu başvuru mimarisinde müşteri gereksinimlerine bağlı olarak eklenebilir:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) karmaşık karma Ayıkla-Dönüştür-yükle (ETL), yerleşik bir yönetilen bir bulut hizmeti Ayıkla-yükle-Dönüştür (ELT) ve veri tümleştirme projeleri. Azure Data Factory, görselleştirme dahil olmak üzere ve izleme araçları veri geldiği zaman ve nereden geldiğini belirlemek için izleme yardımcı olmak ve kişisel verileri bulmak için özellikleri vardır. Müşteriler, Azure Data factory'yi kullanarak, oluşturma ve zamanlama veri odaklı iş akışları farklı veri depolarından veri alabilen işlem hatları olarak adlandırılır. Bu komut zincirleri, iç ve dış kaynaklardan gelen verileri alabilmek müşterilerin izin verin. Müşteriler sonra işleyebilir ve çıktı verilerini Azure SQL veri ambarı gibi veri depolarında dönüştürün.
- Müşteriler yapılandırılmamış verileri hazırlamak [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), türü, her boyuttaki veriyi yakalanmasını sağlar ve alma hızı işletimsel ve keşfe dönük çözümleme için tek bir yerde.  Azure Data Lake, veri dönüştürme ve ayıklama sağlayan özellikleri vardır. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve Azure SQL veri ambarı gibi diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
