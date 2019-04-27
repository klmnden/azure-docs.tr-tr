---
title: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri ambarı
description: Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri ambarı
services: security
author: jomolesk
ms.assetid: dbc9cafe-115d-4965-b0d4-fcf235a064c8
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: a1850ecfbb21eb9495bb0e6de362dc8dee3026a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609560"
---
# <a name="azure-security-and-compliance-blueprint---data-warehouse-for-nist-sp-800-171"></a>Azure güvenlik ve uyumluluk planı - 800-171 NIST SP için veri ambarı

## <a name="overview"></a>Genel Bakış
[NIST özel yayını 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) nonfederal bilgi sistemleri ve kuruluşlar içinde bulunduğu kontrollü Sınıflandırılmamış bilgiler (CUI) korumak için yönergeler sağlar. NIST SP 800-171 CUI gizliliğini korumaya yönelik güvenlik gereksinimleri 14 ailelerinde oluşturur.

Azure güvenlik ve uyumluluk planı 800-171 denetimleri bir veri ambarı mimarisi NIST SP bir alt kümesi uygulayan azure'da dağıtma müşteriler yardımcı olan yönergeler sağlar. Bu çözüm, müşterilerin belirli güvenlik ve uyumluluk gereksinimlerini karşılayabilecek bir yol gösterir. Ayrıca müşterilerin oluşturmak ve Azure'da kendi veri ambarı çözümleri yapılandırma temeli olarak kullanılır.

Bu başvuru mimarisi, ilişkili Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine uyum sağlamak bir temel olarak görev yapacak yöneliktir. Olarak kullanılmaması-üretim ortamıdır. Müşteriler, uygun güvenlik ve uyumluluk değerlendirmesi bu mimari kullanılarak oluşturulan herhangi bir çözümün yürütmek için sorumludur. Gereksinimleri her bir müşterinin uygulama ayrıntılarına göre farklılık gösterebilir.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri
Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır. Bir katman verileri burada alınır, depolanan ve kümelenmiş bir SQL ortamda hazırlandı. SQL veri ambarı için başka bir katmandır. Bu katman ile bir ayıklama-dönüştürme-yükleme (ETL) aracını kullanarak veri yüklenir (örneğin, [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase) T-SQL sorguları) işleme için. Analytics, verileri SQL veri ambarı'nda depolandıktan sonra çok büyük ölçekte çalıştırabilirsiniz.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, SQL Server Reporting Services raporları SQL veri ambarı hızlı oluşturulması için içerir. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını öneririz.

Azure SQL veri ambarı sütunlu depolama alanındaki ilişkisel tabloları veri depolar. Sorgu performansını artırır ancak bu biçim veri depolama maliyetlerini önemli ölçüde azaltır. Kullanım gereksinimlerine bağlı olarak, SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya hiç etkin işlem işlem kaynakları gerekiyorsa tamamen kapatabilmek.

Bir SQL Server Yük Dengeleyici, yüksek performans sağlamak için SQL trafiğini yönetir. Tüm sanal makineleri (VM'ler) Bu başvuru mimarisinde bir Always On kullanılabilirlik grubunda yapılandırılmış SQL Server örnekleri ile kullanılabilirlik kümesi dağıtın. Bu yapılandırma yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri sağlar.

Bu veri ambarı başvuru mimarisi, yapı içinde kaynaklarının yönetimi için bir Active Directory katmanı da içerir. Active Directory alt daha büyük bir Active Directory orman yapısı altında kolay benimsenmesini sağlar. Bu şekilde, daha büyük ormanına erişimi kullanılabilir olsa bile ortamı sürekli olarak çalışabilir. Active Directory katmanı için etki alanına katılmış tüm vm'leridir. Bunlar, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için Active Directory grup ilkeleri kullanır.

Çözüm, müşterilerin bekleyen verilerin gizliliğini depolama hizmeti Şifrelemesi'ni kullanmak için yapılandırabilirsiniz Azure depolama hesapları kullanır. Azure dayanıklılık için Müşteri'nin seçili veri merkezinde verilerin üç kopyasını depolar. Coğrafi olarak yedekli depolama, veri yüzlerce mil uzaktaki ve yeniden, veri merkezinde üç kopya olarak depolanan bir ikincil veri merkezine çoğaltılmasını sağlar. Bu düzenleme, veri kaybına oluşan olumsuz olaya müşterinin birincil veri merkezinde engeller.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Dağıtılan kaynakları (Azure AD) rol tabanlı erişim denetimi (RBAC) erişimi denetlemek için kullanılan Azure Active Directory. Bu kaynaklar, Azure anahtar Kasası'nda müşteri anahtarlarını içerir. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak için her iki izleme hizmetleri yapılandırın. Sistem durumu, kullanımı kolay tek bir Panoda görüntülenir.

VM management Burcu ana bilgisayarı görev yapar. Yöneticilerin dağıtılan kaynaklara güvenli bir bağlantı sağlar. Veri hazırlama alanı aracılığıyla bu yönetim Burcu ana bilgisayarı yükler. *Başvuru mimarisi alt ağa yönetimi ve veri içeri aktarma için bir VPN veya Azure ExpressRoute bağlantı yapılandırmanızı öneririz.*

![Veri ambarı için SP NIST 800-171 başvurusu mimari şeması](images/nist171-dw-architecture.png "veri ambarı için SP NIST 800-171 başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Daha fazla bilgi için [dağıtım mimarisi](#deployment-architecture) bölümü.

- Kullanılabilirlik kümeleri
    - Active Directory etki alanı denetleyicileri
    - SQL Server küme düğümleri ve Tanık
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Key Vault
- Azure İzleyici (günlük)
- Azure Güvenlik Merkezi
- Azure Load Balancer
- Azure Storage
- Azure Sanal Makineler
    - (1) Burcu ana bilgisayarı
    - (2) active Directory etki alanı denetleyicisi
    - (2) SQL Server küme düğümü
    - (1) SQL Server tanığı
- Azure Sanal Ağ
    - ((1) /16 ağ
    - (4) /24 ağlar
    - (4) ağ güvenlik grupları
- Kurtarma Hizmetleri kasası
- SQL Veri Ambarı
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Dağıtım mimarisi
Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**Azure SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için yüksek düzeyde paralel işleme yararlanır bir kurumsal veri ambarıdır. Kullanıcılar, büyük veri, SQL veri ambarı'na içeri aktarma ve yüksek performanslı bir analiz çalıştırmak için yüksek düzeyde paralel işleme gücünü kullanın & basit PolyBase T-SQL sorguları kullanabilir.

**SQL Server Raporlama Hizmetleri**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) tabloları, grafikler, haritalar, ölçerler, matrisler ve SQL veri ambarı için daha hızlı oluşturulmasını raporlar sağlar.

**Azure veri Kataloğu**: [Veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynaklarını bulma ve verileri yöneten kullanıcılar tarafından anlamak daha kolay hale getirir. Genel veri kaynakları, etiketlenmiş ve veriler için Aranan kayıtlı. Veri kalırken, meta veri kopyası mevcut konumunda, veri Kataloğu'na eklenir. Veri kaynağı konumuna yönelik bir başvuru bulunur. Meta veriler, her veri kaynağı arama yoluyla bulmasını kolaylaştırmak üzere dizine alınır. Dizin oluşturma anlaşılır bulan kullanıcılar için kolaylaştırır.

**Kale ana bilgisayarı**: Kale ana bilgisayarı, kullanıcıların bu ortama dağıtılan kaynaklara erişmek için kullanabileceği girişinin tek noktasıdır. Kale ana bilgisayarı, güvenli bir listede yalnızca genel IP adreslerinden gelen uzak trafiğe izin vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü trafiğine izin vermek için trafik kaynağı ağ güvenlik grubunda tanımlanmış olması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir VM oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısını](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Azure tanılama uzantısını](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Key Vault'u kullanarak.
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında VM kaynaklarının kullanımını azaltmak için.
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri yalıtılmış olan korumalı bir ortamda çalışan işletim sistemini çalıştırmak için etkin.

### <a name="virtual-network"></a>Sanal ağ
Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri (Nsg'ler) içerir. Nsg bir alt ağ veya tek tek VM düzeyinde trafiği güvenli hale getirmek için kullanılabilir. Aşağıdaki Nsg'ler mevcuttur:
  - Veri katmanı (SQL Server kümelerini, SQL Server tanığı ve SQL yük dengeleyici) için bir NSG
  - Yönetim Burcu ana bilgisayarı için bir NSG
  - Active Directory için bir NSG
  - SQL Server Raporlama Hizmetleri için bir NSG

Her nsg belirli bağlantı noktalarına sahiptir ve çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek protokollerini açın. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
  - [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanır.
  - Azure İzleyici günlüklerine bağlı olduğu [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Alt ağlar**: Her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler
Mimari, verileri birden çok ölçü kullanılmadıkları korur. Bu ölçüler, şifreleme ve veritabanı denetimi içerir.

**Azure depolama**: Bekleyen şifreli verileri gereksinimlerini karşılamak için tüm [depolama](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/storage-service-encryption). Bu özellik, Kurumsal güvenlik ve uyumluluk gereksinimlerini desteklemek üzere verileri koruyarak yardımcı olur.

**Azure Disk şifrelemesi**: [Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğini kullanır. Çözüm denetlemenize ve disk şifreleme anahtarlarını yönetmek için Key Vault ile tümleştirilir.

**Azure SQL veritabanı**: SQL veritabanı örneği aşağıdaki veritabanı güvenlik önlemlerini kullanır:
-   [Active Directory kimlik doğrulaması ve yetkilendirme](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) veritabanı olaylarını izler ve bir denetim günlüğüne bir Azure depolama hesabında yazar.
-   SQL veritabanını kullanacak şekilde yapılandırıldığını [saydam veri şifrelemesi](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Bu, gerçek zamanlı şifreleme ve şifre çözme veritabanı, ilişkili yedeklemeler ve işlem günlük dosyaları bekleyen bilgileri korumak için gerçekleştirir. Veriler güvencesi yetkisiz erişim olmamıştır saydam veri şifrelemesi sağlar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) doğru izinler verilene kadar veritabanı sunucularına tüm erişimi engeller. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) algılama ve yanıt olarak olası tehditleri ortaya çıktıkları sağlar. Şüpheli veritabanı etkinlikleri, potansiyel açıklar, SQL ekleme saldırıları ve anormal veritabanı erişim modellerinin güvenlik uyarıları sağlar.
-   [Şifrelenmiş sütunlar](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman içinde veritabanı sistemi düz metin olarak göründüğünden emin olun. Veri şifrelemesi etkinleştirildikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlarına erişimi ile düz metin verilere erişebilir.
- [Genişletilmiş Özellikler](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) veri sahipleri işlenmesini kesmek için kullanılabilir. Kullanıcı veritabanı nesneleri için özel özellikler ekleyebilirsiniz. Bunlar ayrıca "ilişkili finansal verileri işlenmesini önlemek için uygulama mantığını destekleyecek şekilde artık Üretilmiyor" verileri etiketleyebilirsiniz.
- [Satır düzeyi güvenlik](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) işleme devam etmek için veri erişimini kısıtlamak için ilkeler tanımlamak üzere kullanıcıların sağlar.
- [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) tarafından ayrıcalıksız kullanıcılar veya uygulamalar için veri maskeleme, hassas verilerin görünürlüğünü ayrıcalık sahibi sınırlar. Bu otomatik olarak olası hassas verileri bulabilir ve uygulanması için uygun maskeleri önerin. Dinamik veri maskeleme, hassas verilerin yetkisiz erişim veritabanı açıktan erişim azaltmaya yardımcı olur. *Müşteriler, kendi veritabanı şeması uyması ayarlarını sorumludur.*

### <a name="identity-management"></a>Kimlik yönetimi
Özellikleri, Azure ortamında veri erişimi yönetmek için aşağıdaki teknolojileri sağlar:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) Microsoft çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözüm için tüm kullanıcılar Azure AD'de oluşturulur ve SQL veritabanına erişim kullanıcıları içerir.
-   Azure AD'yi kullanarak uygulamaya kimlik doğrulaması gerçekleştirilir. Daha fazla bilgi için bkz. nasıl [Azure AD ile uygulamaları tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Veritabanı sütun şifreleme Azure AD uygulamasını SQL veritabanına kimlik doğrulaması için de kullanır. Daha fazla bilgi için bkz. nasıl [SQL veritabanındaki hassas verileri korumaya](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) ayrıntılı erişim izinlerini tanımlamak için yöneticiler tarafından kullanılabilir. Kullanıcıların işlerini yapması için gereken Bununla, bunlar sadece erişim miktarını verebilirsiniz. Azure kaynakları için her kullanıcının sınırsız erişim vermek yerine, yöneticiler kaynaklarına ve verilerine erişmek için yalnızca belirli eylemleri izin verebilir. Abonelik yöneticisine abonelik erişimi sınırlıdır.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) verileri gibi belirli bilgileri erişebilen kullanıcı sayısını en aza indirmek için müşteriler tarafından kullanılabilir. Yöneticiler, Azure AD Privileged Identity Management, bulmak, kısıtlamak ve ayrıcalıklı kimlikleri ve bu kimliklerin kaynaklara erişimini izlemek için kullanabilirsiniz. Bu işlev, gerektiğinde talep üzerine tam zamanında yönetimsel erişim uygulamak için de kullanılabilir.
- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) bir kuruluşunuzun kimliklerini etkileyen olası güvenlik açıklarını algılar. Bir kuruluşun kimlikleri ilgili algılanan kuşkulu eylemleri için otomatik yanıtlar yapılandırır. Aynı zamanda bunları gidermek için uygun eylemde için şüpheli olayları sorunu kısmen Araştırıyor.

### <a name="security"></a>Güvenlik
**Gizli dizileri Yönetim**: Çözüm [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) anahtar ve gizli dizi yönetimi. Key Vault, şifreleme anahtarlarını koruyun ve bulut uygulamaları ve Hizmetleri tarafından kullanılan gizli yardımcı olur. Aşağıdaki anahtar kasası özellikleri müşteri verilerini korumaya yardımcı olur:
- Gelişmiş erişim ilkeleri gereksinim olarak yapılandırılır.
- Key Vault erişim ilkeleri anahtarlara ve gizli anahtarları en düşük gerekli izinlerle tanımlanır.
- Tüm anahtarları ve gizli anahtarları Key vault'ta sona erme tarihi vardır.
- Tüm anahtarları Key vault'ta özel bir donanım güvenlik modülleri tarafından korunur. Bir donanım güvenlik modülü korumalı anahtarlar 2048 bit RSA anahtar anahtar türüdür.
- Tüm kullanıcılar ve kimlikler RBAC kullanarak minimum gerekli izinleri verilir.
- Key Vault için tanılama günlükleri ile 365 gün en az bir saklama süresi etkinleştirilir.
- Anahtarlar için izin verilen şifreleme işlemleri gerekli olanlarla sınırlıdır.

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılan Windows Vm'leri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) hizmeti üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki Vm'lere gerektiğinde.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) VM'ler için yardımcı tanımlamak ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar. Müşteriler, bilinen kötü amaçlı veya istenmeyen yazılım yükleme veya korumalı Vm'leri çalıştırma girişiminde oluşturmasını uyarıları yapılandırabilirsiniz.

**Azure Güvenlik Merkezi**: İle [Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları da erişir.

Güvenlik Merkezi algılama özellikleri çeşitli ortamlarını hedefleyen potansiyel saldırılar müşteriler uyarmak için kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) bir tehdit veya şüpheli etkinlik başladığında tetiklenir. Müşteriler [özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) kendi ortamından toplanmış veriler üzerinde yeni güvenlik uyarıları tanımlamak için.

Güvenlik Merkezi, öncelikli güvenlik uyarıları ve olayları sağlar. Güvenlik Merkezi'ni keşfedin ve olası güvenlik sorunlarını çözmek, müşteriler için daha basit hale getirir. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her tehdit algılanan için oluşturulur. Bunlar tehdit araştırma ve düzeltme olay yanıt ekiplerinin raporları kullanabilirsiniz.

Bu başvuru mimarisi de kullanır [güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) Güvenlik Merkezi'nde yeteneği. Yapılandırıldıktan sonra (örneğin, Qualys) iş ortağı Aracısı, güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformu da güvenlik açığı ve durum izleme verilerini Güvenlik Merkezi’ne geri gönderir. Müşteriler, güvenlik açığından etkilenen sanal makineleri hızlı bir şekilde tanımlamak için bu bilgileri kullanabilirsiniz.

### <a name="business-continuity"></a>İş sürekliliği
**Yüksek kullanılabilirlik**: Sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'da VM'ler yüksek kullanılabilirliğini sağlamak için. En az bir sanal makine % 99,95 oranında karşılayan planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve sanal makinelerin bu mimaride tüm yapılandırmalar korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas VM'den tüm VM'yi geri yüklemeden geri yükleyebilirsiniz. Bu işlem geri yükleme sürelerini hızlandırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve Denetim

Azure Hizmetleri, sistem ve kullanıcı etkinliğini yanı sıra, sistem durumu kapsamlı bir şekilde oturum:
- **Etkinlik günlükleri**: [Etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir Abonelikteki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar. Etkinlik günlükleri bir işlemin Başlatıcı belirlemek yardımcı olabilir, oluşumunu ve durum zaman.
- **Tanılama günlükleri**: [Tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından oluşturulan tüm günlükleri içerir. Bu günlükler, Windows olayı sistem günlükleri, depolama günlükleri, anahtar kasası denetim günlüklerini ve Azure Application Gateway erişim ve güvenlik duvarı günlükleri içerir. Tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabına arşivleme yazın. Kullanıcılar saklama süresi 730 gün belirli gereksinimleri karşılamak için en fazla yapılandırabilirsiniz.

**Azure İzleyici günlüklerine**: Bu günlükler, birleştirilmiş [Azure İzleyici günlükleri](https://azure.microsoft.com/services/log-analytics/) işleme, depolama ve Panosu raporlama. Veriler toplandıktan sonra Log Analytics çalışma alanları içindeki her bir veri türü için ayrı tablolar halinde düzenlenir. Bu şekilde, tüm veri ve böylece özgün kaynağına bakılmaksızın birlikte çözümlenebilir. Güvenlik Merkezi, Azure İzleyici günlükleri ile tümleştirilir. Müşteriler, güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorguları kullanabilirsiniz.

Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) Bu mimarinin bir parçası olarak dahil edilir:
-   [Active Directory değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Bu, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözümü, risk ve server ortamlarının sistem durumunu düzenli aralıklarla değerlendirir. Müşteriler, Önceliklendirilmiş öneriler için dağıtılan sunucu altyapısı belirli listesini sağlar.
- [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı durumu çözümü, kaç aracının dağıtılır ve kullanıcıların coğrafi dağılımı bildirir. Ayrıca, kaç aracının yanıt vermeyen ve işletimsel verileri gönderme aracı sayısını raporlar.
-   [Etkinlik günlüğü analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Etkinlik günlüğü analizi çözümü, bir müşteri için tüm Azure abonelikleri arasında Azure etkinlik günlüklerini analiziyle yardımcı olur.

**Azure Otomasyonu**: [Otomasyon](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) runbook'ları yöneten depolar ve çalıştırır. Bu çözümde, SQL veritabanı'ndan günlüklerini toplama runbook'ları yardımcı olur. Müşteriler, Otomasyon kullanabileceğiniz [değişiklik izleme](https://docs.microsoft.com/azure/automation/automation-change-tracking) ortamındaki değişiklikler kolayca belirlemek için çözüm.

**Azure İzleyici**: [İzleyici](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) kullanıcıların performans izleme, güvenliği koruma ve eğilimleri belirlemenize yardımcı olur. Kuruluşlar, denetleme, uyarı oluşturma ve verileri arşivlemek için kullanabilirsiniz. Ayrıca, Azure kaynaklarını API çağrılarında takip edebilirsiniz.

## <a name="threat-model"></a>Tehdit modeli

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/nist171-dw-tm) veya buradan ulaşabilirsiniz. Bu model system altyapısında potansiyel risk puanları, değişiklikler yaptığınızda anlamasına yardımcı olabilir.

![Veri ambarı için SP NIST 800-171 tehdit modeli](images/nist171-dw-threat-model.png "veri ambarı için SP NIST 800-171 tehdit modeli")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri
[Azure güvenlik ve uyumluluk planı: NIST SP 800-171 müşteri sorumluluk matris](https://aka.ms/nist171-crm) 800-171 NIST SP tarafından gerekli tüm güvenlik denetimleri listeler. Bu matris her denetimi uyarlamasını Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı - 800-171 NIST SP veri ambarı denetimi uygulama matris](https://aka.ms/nist171-dw-cim) hangi NIST SP üzerinde 800-171 denetimleri tarafından veri ambarı mimarisi alınmıştır bilgi sağlar. Bu, uygulama kapsanan her denetimin gereksinimleri nasıl karşıladığını ayrıntılı açıklamaları içerir.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu veri ambarı başvuru mimarisinin bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantı, Internet üzerinden gerçekleşir. Müşteriler, bilgilere güvenli bir şekilde "tüneli" müşterinin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde bu bağlantıyı kullanabilirsiniz. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile geçtiğinden, Microsoft başka bir daha da güvenli bağlantı seçeneği sunar. ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları, müşterinin telekomünikasyon sağlayıcısına doğrudan bağlanın. Sonuç olarak, veriler Internet üzerinden yolculuk değil ve kendisine sunulan değil. Bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, düşük gecikme ve tipik bağlantılardan daha yüksek güvenlik sunar.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Çıkartma-dönüştürme-yükleme işlemi
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) yük verileri SQL veri ambarı'na ayrı bir ETL gerek kalmadan veya aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. SQL Server ile uyumlu üçüncü taraf araçları ve Microsoft iş zekası ve analiz yığını PolyBase ile birlikte kullanılabilir.

### <a name="azure-ad-setup"></a>Azure AD Kurulumu
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Şirket içinde Active Directory, Azure AD ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca Azure AD'ye dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) bağlayabilirsiniz. Bunu yapmak için bir alt etki alanı bir Azure AD ormanı dağıtılan Active Directory altyapısı olun.

### <a name="optional-services"></a>İsteğe bağlı hizmetler
Azure Hizmetleri depolama ve biçimlendirilmiş ve biçimlendirilmemiş veri hazırlama ile yardımcı olmak üzere çeşitli sunar. Müşteri gereksinimlerine göre bu başvuru mimarisi aşağıdaki hizmetleri eklenebilir:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) , karmaşık karma Ayıkla-yükle-Dönüştür olan ETL ve veri tümleştirme projeleri için oluşturulmuş bir yönetilen bir bulut hizmetidir. Data factory'yi izleme yardımcı olmak ve verileri yerleştirmek için özellikleri vardır. Görselleştirme ve izleme araçları veri geldiği zaman ve nereden geldiğini belirleyin. Müşteriler, oluşturun ve farklı veri depolarından veri alabilen işlem hatları olarak adlandırılır veri odaklı iş akışları, zamanlayın. İç ve dış kaynaklardan veri almak için işlem hatları kullanabilirler. Müşteriler daha sonra işlem ve veri çıkışı için SQL veri ambarı gibi veri depolarında dönüştürün.
- Müşteriler yapılandırılmamış verileri hazırlamak [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) veri herhangi bir boyuta, türe ve alma hızına işletimsel ve keşfe dönük çözümleme için tek bir yerde yakalamak için. Azure Data Lake, veri dönüştürme ve ayıklama sağlayan özellikleri vardır. Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur. Bu da düzgün şekilde SQL veri ambarı gibi diğer Azure hizmetleriyle tümleşir.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
