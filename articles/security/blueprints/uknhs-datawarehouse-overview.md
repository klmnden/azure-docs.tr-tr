---
title: Azure güvenlik ve uyumluluk planı - UK NHS için veri ambarı
description: Azure güvenlik ve uyumluluk planı - UK NHS için veri ambarı
services: security
author: jomolesk
ms.assetid: f4e4b939-88db-4d78-8fa9-c2a12b2c025b
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: 12042b853682efcff2de285a97508b8a81b1d647
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610062"
---
# <a name="azure-security-and-compliance-blueprint-data-warehouse-for-uk-nhs"></a>Azure güvenlik ve uyumluluk planı: UK NHS için veri ambarı

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk planı, bir başvuru mimarisi ve güvenli bir şekilde almak, hazırlama, depolama ve sağlık hassas verilerle etkileşim için uygun bir veri ambarı çözümü için yönergeler sağlar. Bu çözüm, müşteriler uyumlu, sağlanan yönergeler ile yolları gösterir [bulut güvenlik iyi uygulama Kılavuzu](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) tarafından yayımlanan [NHS dijital](https://digital.nhs.uk/), Birleşik Krallık'ın (UK Bölgesinde) iş ortağı Departman, sağlık ve sosyal Bakım (DHSC). Bulut güvenliği iyi uygulama Kılavuzu 14 üzerinde alan [bulut güvenliği prensipleri](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) yayımlanan Birleşik Krallık Ulusal siber Güvenlik Merkezi (NCSC tarafından).

Bu başvuru mimarisi, Uygulama Kılavuzu ve tehdit modeli, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-ek yapılandırma olmadan bir üretim ortamında olduğundan. Müşteriler, uygun güvenlik yürütmek için sorumludur ve her bir müşterinin uygulama ayrıntılarına bağlı uyumluluk değerlendirme gereksinimleri değişebilir, bu mimariyi kullanarak oluşturulan bir çözümün temel.

## <a name="architecture-diagram-and-components"></a>Mimari diyagramı ve bileşenleri

Bu çözüm, yüksek performanslı ve güvenli bir bulut veri ambarı uygulayan bir başvuru mimarisi sağlar. Bu mimaride iki ayrı veri katmanı vardır: bir yere veri içe, depolanan ve kümelenmiş bir SQL ortamı içinde hazırlanmış ve başka bir Azure SQL veri ambarı veri yüklendiği kullanarak ayıklama, dönüştürme, yükleme aracı (örneğin [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase) T-SQL sorguları) işlemek için. Analytics, verileri Azure SQL veri ambarı'nda depolandıktan sonra çok büyük ölçekte çalıştırabilirsiniz.

Azure, birçok müşteri için raporlama ve Analiz Hizmetleri sunar. Bu çözüm, SQL Server Reporting Services için Azure SQL veri ambarı raporları hızlı oluşturulmasını içerir. Tüm SQL trafiği otomatik olarak imzalanan sertifikalar dahil edilmesi SSL ile şifrelenir. En iyi uygulama, Azure, Gelişmiş güvenlikten yararlanmaya başlamak için güvenilir bir sertifika yetkilisi kullanımını önerir.

Azure SQL veri ambarındaki ilişkisel tabloları sütunlu depolama, sorgu performansını artırırken veri depolama maliyetlerini önemli ölçüde azaltan bir biçim depolanır.  Kullanım gereksinimlerine bağlı olarak, Azure SQL veri ambarı işlem kaynakları yukarı veya aşağı ölçeklendirilebilir veya işlem kaynakları gerektiren herhangi bir etkin işlem yoksa tamamen kapatabilmek.

SQL bir yük dengeleyici SQL trafik, yüksek bir performans sağlamaya yönetir. Bu başvuru mimarisindeki tüm sanal makineleri bir Always On kullanılabilirlik grubu için yüksek kullanılabilirlik ve olağanüstü durum kurtarma özellikleri yapılandırılmış SQL Server örnekleri ile kullanılabilirlik kümesi dağıtın.

Gelişmiş güvenlik için bu çözümdeki tüm kaynaklar bir kaynak grubuyla Azure Resource Manager aracılığıyla yönetilir. Azure Active Directory rol tabanlı erişim denetimi erişimi denetlemek için kullanılan dağıtılan kaynakları ve Azure Key vault'taki anahtarları. Sistem durumu, Azure Güvenlik Merkezi ve Azure İzleyici izlenir. Müşteriler, günlükleri tutmak ve sistem durumu bir kolayca gezinebilir, tek bir Panoda görüntülemek için her iki izleme hizmetleri yapılandırın.


Bu veri ambarı başvuru mimarisi, yapı içinde kaynaklarının yönetimi için bir Active Directory katmanı da içerir. Active Directory alt büyük ormanına erişimi kullanılabilir olsa bile ortamı sürekli işlem için izin vererek daha büyük bir Active Directory orman yapısı altında kolay benimsenmesini sağlar. Tüm sanal makineler, Active Directory katmanı için etki alanı ile birleşik ve işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları zorlamak için Active Directory grup ilkeleri kullanın.

Bir sanal makine, yöneticilerin erişim dağıtılan kaynaklara güvenli bir bağlantı sağlayan bir yönetim savunma ana görevi görür. Veri hazırlama alanı aracılığıyla bu yönetim Burcu ana bilgisayarı yükler. **Microsoft, yönetim ve veri alma başvuru mimarisi alt ağa bir VPN veya Azure ExpressRoute bağlantısı yapılandırma önerir.**

![Veri ambarı UK NHS başvurusu mimari şeması için](images/uknhs-datawarehouse-architecture.png?raw=true "veri ambarı için UK NHS başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Ayrıntılar için bkz dağıtım mimarisi [dağıtım mimarisi](#deployment-architecture) bölümü.

- Kullanılabilirlik Kümeleri
    - Active Directory etki alanı denetleyicileri
    - SQL küme düğümleri ve Tanık
- Azure Active Directory
- Azure Veri Kataloğu
- Azure Key Vault
- Azure İzleyici
- Azure Güvenlik Merkezi
- Azure Load Balancer
- Azure Storage
- Azure Otomasyonu
- Azure Sanal Makineler
    - (1) Burcu ana bilgisayarı
    - (2) active Directory etki alanı denetleyicisi
    - (2) SQL Server küme düğümü
    - (1) SQL Server tanığı
- Azure Sanal Ağ
    - (1) /16 ağ
    - (4) /24 ağlar
    - (4) ağ güvenlik grupları
- Kurtarma Hizmetleri kasası
- SQL Veri Ambarı
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde dağıtım ve uygulama öğeleri ayrıntılı olarak açıklanmaktadır.

**SQL veri ambarı**: [SQL veri ambarı](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) bir kurumsal veri hızlıca petabaytlarca veri üzerinde karmaşık sorgular çalıştırmak için yüksek düzeyde paralel işleme yararlanan sağlık verileri verimli bir şekilde belirlemenize izin vererek ambarıdır. Kullanıcılar, yüksek performanslı bir analiz çalıştırmak için yüksek düzeyde paralel işleme gücünü yazılımınız olması ve SQL veri ambarı'na büyük veri almak için basit PolyBase T-SQL sorguları kullanabilir.

**SQL Server Raporlama Hizmetleri**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) tabloları, grafikler, haritalar, ölçerler, matrisler ve Azure SQL veri ambarı için daha hızlı oluşturulmasını raporlar sağlar.

**Veri Kataloğu**: [Veri Kataloğu](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) veri kaynakları verileri yöneten kullanıcılar tarafından kolayca bulunabilmesini ve anlaşılır hale getirir. Genel veri kaynakları, etiketlenmiş ve sistem durumu ile ilgili veriler için Aranan kayıtlı. Var olan konumunda, ancak kendi meta verilerinin bir kopyasını veriler kalırken, veri kaynağı konumuna yönelik bir başvuru veri Kataloğu'na eklenir. Bu meta veriler ayrıca her bir veri kaynağının arama ile kolayca bulunabilmesini ve bunları bulan kullanıcılar tarafından anlaşılabilmesini sağlamak üzere dizine alınır.

**Kale ana bilgisayarı**: Kale ana bilgisayarı tek kullanıcılara bu ortama dağıtılan kaynaklara erişmek giriş noktasıdır. Kale ana bilgisayarı, genel IP adreslerinden gelen uzak trafiğine yalnızca güvenli bir listede vererek dağıtılan kaynaklara güvenli bir bağlantı sağlar. Uzak Masaüstü (RDP) trafiğine izin vermek için trafik kaynağını ağ güvenlik grubu listesinde tanımlanması gerekir.

Bu çözüm aşağıdaki yapılandırmaları olan bir etki alanına katılmış Burcu ana bilgisayarı olarak bir sanal makine oluşturur:
-   [Kötü amaçlı yazılımdan koruma uzantısı](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Azure tanılama uzantısı](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) Azure Key Vault kullanma
-   Bir [otomatik kapatma ilke](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanımda olmadığında sanal makine kaynakların tüketimini azaltmak için
-   [Windows Defender Credential Guard](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) kimlik bilgilerini ve diğer gizli dizileri çalışan işletim sisteminden yalıtılmış korumalı bir ortamda çalıştırın böylece etkin

### <a name="virtual-network"></a>Sanal ağ

Bu başvuru mimarisi, özel bir sanal ağ ile bir 10.0.0.0/16 adres alanı tanımlar.

**Ağ güvenlik grupları**: [Ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) izin veren veya bir sanal ağ içinde trafiği reddeden erişim denetim listeleri içerir. Ağ güvenlik grupları, trafiğin bir alt ağ veya tek tek sanal makine düzeyinde güvenliğini sağlamak için kullanılabilir. Aşağıdaki ağ güvenlik grupları mevcut:

  - Veri katmanı (SQL Server kümeleri, SQL Server tanığı ve SQL yük dengeleyici) için ağ güvenlik grubu
  - Yönetim savunma ana bilgisayar için ağ güvenlik grubu
  - Active Directory için ağ güvenlik grubu
  - SQL Server Reporting Services için ağ güvenlik grubu

Sahip ağ güvenlik gruplarının her biri belirli bağlantı noktaları ve protokolleri çözüm güvenli bir şekilde ve doğru bir şekilde çalışabilmek açın. Ayrıca, aşağıdaki yapılandırmalar için her bir ağ güvenlik grubu etkinleştirilir:

- [Tanılama günlüklerini ve olayları](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) etkinleştirilir ve bir depolama hesabında depolanmış
- Azure İzleyici günlüklerine bağlı olduğu [ağ güvenlik grubu&#39;s tanılama günlükleri](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Alt ağlar**: Her alt ağa karşılık gelen ağ güvenlik grubu ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme ve veritabanı denetimi gibi birden çok ölçü kullanılmadıkları verilerini korur.

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

**Düzeltme Eki Yönetimi**: Bu başvuru mimarisinin bir parçası olarak dağıtılmış Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm ayrıca içerir [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-intro) üzerinden güncelleştirilmiş dağıtımları oluşturulabilir düzeltme eki gerektiğinde sanal makinelere için hizmet.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olan gerçek zamanlı koruma özelliği için sanal makineler sağlar, kötü amaçlı veya istenmeyen yazılım bilinen yapılandırılabilir uyarı ile çalışır yükleme veya korumalı sanal makineler üzerinde çalıştırın.

**Azure Güvenlik Merkezi**: İle [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-intro), müşterilerin merkezi olarak uygulama ve iş yüklerinizde güvenlik ilkelerini yönetme, sınırlama, tehditlere maruz kalma riskinizi ve algılayabilir ve saldırılara karşılık vermek. Ayrıca, Azure Güvenlik Merkezi, yapılandırma ve güvenlik duruşunu ve verilerin korunmasına yardımcı olmak için hizmet öneriler sağlamak üzere Azure hizmetlerinin mevcut yapılandırmaları erişir.

Azure Güvenlik Merkezi, ortamlarını hedefleyen potansiyel saldırılar müşterileri uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde bulunan bir dizi [güvenlik uyarıları önceden tanımlanmış](https://docs.microsoft.com/azure/security-center/security-center-alerts-type)bir tehdit tetiklenen veya şüpheli etkinlik gerçekleştiğinde. [Özel uyarı kuralları](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) Azure Güvenlik Merkezi'nde müşterilerin kendi ortamından toplanmış verileri temel alan yeni güvenlik uyarılarını tanımlamanıza izin verin.

Azure Güvenlik Merkezi, müşterilerin bulmasını ve olası güvenlik sorunlarını gidermek için daha basit hale öncelikli güvenlik uyarıları ve olayları sağlar. A [tehdit zekası raporu](https://docs.microsoft.com/azure/security-center/security-center-threat-report) her olay yanıt ekiplerinin tehditleri araştırmanıza ve düzeltme yardımcı olmak için tehdit algılanan için oluşturulur.

Ayrıca, bu başvuru mimarisi kullanan [güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) Azure Güvenlik Merkezi'nde. Yapılandırıldıktan sonra (örn. Qualys) iş ortağı Aracısı, güvenlik açığı verilerini iş ortağının yönetim platformuna bildirir. Buna karşılık, iş ortağının yönetim platformuna Güvenlik Açığı ve durum verileri Azure Güvenlik Merkezi, izleme güvenlik açığından etkilenen sanal makinelerin hızlı bir şekilde tanımlamak müşterilerin olanak sağlar.

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: Sunucu iş yükleri gruplanır bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) azure'daki sanal makinelerin yüksek kullanılabilirlik sağlamak için. En az bir sanal makine % 99,95 oranında toplantı planlı veya Plansız bakım olayı sırasında kullanılabilir Azure SLA'sı.

**Kurtarma Hizmetleri kasası**: [Kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimaride, Azure sanal makinelerini korur. Bir kurtarma Hizmetleri kasası ile müşterilerin dosya ve klasörleri bir Iaas sanal makineden sanal makinenin tamamını geri yüklemeden daha hızlı geri yükleme süreleri etkinleştirme geri yükleyebilirsiniz.

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

Bu başvuru mimarisine yönelik veri akış diyagramı kullanılabilir [indirme](https://aka.ms/uknhs-dw-tm) veya altında bulunabilir. Bu model, değişiklikler yaparken sistemi altyapısında potansiyel risk puanları anlamasına yardımcı olabilir.

![UK NHS tehdit modeli için veri ambarı](images/uknhs-datawarehouse-threat-model.png?raw=true "UK NHS tehdit modeli için veri ambarı")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenlik ve uyumluluk planı: UK NHS müşteri sorumluluk matris](https://aka.ms/uknhs-crm) bütün NHS UK gereksinimleri listelenir. Bu matris her ilkesi uygulaması, Microsoft, müşteri sorumluluğu olup ayrıntıları veya ikisi arasında paylaşılan.

[Azure güvenlik ve uyumluluk planı: UK NHS veri ambarı uygulaması matris](https://aka.ms/uknhs-dw-cim) üzerinde hangi UK NHS gereksinimleri nasıl ayrıntılı açıklamaları da dahil olmak üzere veri ambarı mimarisi tarafından açıklanmıştır bilgi sağlar Uygulama, her kapsanan ilkesi gereksinimlerini karşılar.

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute

Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu PaaS web uygulaması başvuru mimarisi bir parçası olarak dağıtılan kaynakların bir bağlantı kurmak için yapılandırılmış olması gerekir. Müşteriler, uygun bir VPN veya ExpressRoute ayarlayarak, Aktarımdaki veriler için koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile bir Azure sanal ağı arasında sanal bir özel bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden yer alır ve müşterilere güvenli bir şekilde sağlar &quot;tünel&quot; müşteri arasında şifrelenmiş bir bağlantı içinde bilgilerine&#39;s ağınız ve Azure. Siteden siteye VPN ölçeklerde tarafından dağıtılan yıllardır güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) şifreleme mekanizması olarak bu seçenek kullanılır.

İçindeki VPN tüneli trafik Internet bir siteden siteye VPN ile çapraz olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute adanmış WAN olan Azure ve şirket içi konum veya Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantılar, daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik Internet üzerinden sunar. Ayrıca, bu müşteri doğrudan bir bağlantı olduğundan&#39;s telekomünikasyon sağlayıcısı, verileri Internet üzerinden yolculuk ediyor mu değil ve ona bu nedenle olarak gösterilmez.

Bir şirket içi ağı Azure'a genişleten güvenli bir hibrit ağı uygulamak için en iyi uygulamalardan bazılarıdır [kullanılabilir](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

### <a name="extract-transform-load-process"></a>Çıkartma-dönüştürme-yükleme işlemi

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) Azure SQL veri ambarı'na veri yükleme ayıklamak için ayrı bir gereksinimi olmadan, dönüştürme, yükleyebilir ve aracı içeri aktarın. PolyBase, T-SQL sorguları verilerine erişmesini sağlar. Microsoft'un iş zekası ve analiz yığını, aynı zamanda SQL Server ile uyumlu üçüncü taraf araçları PolyBase ile birlikte kullanılabilir.

### <a name="azure-active-directory-setup"></a>Azure Active Directory Kurulumu

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) dağıtımını yöneten ve ortam ile etkileşim personel erişimi sağlama için gereklidir. Mevcut bir Windows Server Active Directory, Azure Active Directory ile tümleştirilebilir [dört tıklama](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Müşteriler ayrıca dağıtılan Active Directory altyapısı (etki alanı denetleyicileri) var olan bir Azure Active Directory alt etki alanı bir Azure Active Directory ormanı dağıtılan Active Directory altyapısı yaparak bağlayabilirsiniz.

### <a name="optional-services"></a>İsteğe bağlı hizmetler

Azure Hizmetleri depolama ve biçimlendirilmiş ve biçimlendirilmemiş veri hazırlama ile yardımcı olmak üzere çeşitli sunar. Aşağıdaki hizmetler bu başvuru mimarisinde müşteri gereksinimlerine bağlı olarak eklenebilir:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) karmaşık karma Ayıkla-Dönüştür-yükle ve veri tümleştirme projeleri için oluşturulan bir yönetilen bulut hizmeti olduğunu. Azure Data Factory, görselleştirme dahil olmak üzere ve izleme araçları veri geldiği zaman ve nereden geldiğini belirlemek için izleme yardımcı olmak ve sistem durumu ile ilgili verileri bulmak için özellikleri vardır. Müşteriler, Azure Data factory'yi kullanarak, oluşturma ve zamanlama veri odaklı iş akışları farklı veri depolarından veri alabilen işlem hatları olarak adlandırılır. Bu komut zincirleri, iç ve dış kaynaklardan gelen verileri alabilmek müşterilerin izin verin. Müşteriler sonra işleyebilir ve çıktı verilerini Azure SQL veri ambarı gibi veri depolarında dönüştürün.
- Müşteriler yapılandırılmamış verileri hazırlamak [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview), türü, her boyuttaki veriyi yakalanmasını sağlar ve alma hızı işletimsel ve keşfe dönük çözümleme için tek bir yerde.  Azure Data Lake, veri dönüştürme ve ayıklama sağlayan özellikleri vardır. Azure Data Lake Store, Hadoop ekosistemindeki çoğu açık kaynak bileşenle uyumludur ve Azure SQL veri ambarı gibi diğer Azure hizmetleriyle sorunsuz şekilde tümleşir.

## <a name="disclaimer"></a>Bildirim

 - Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR EXPRESS, ZIMNİ VEYA YASAL BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-olduğundan." Bilgi ve URL ve diğer Internet Web sitesi referansları da dahil olmak üzere bu belgede, bildirilmeksizin değiştirilebilir. Müşteriler bu belgeyi okuma KULLANIMLARDAN doğacak riskler size aittir.
 - Bu belge, müşterilerle herhangi bir Microsoft ürünü veya çözümler üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.
 - Müşteriler kopyalayabilir ve dahili başvuru amacıyla bu belgeyi kullanın.
 - Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.
 - Bu mimari, müşterilerin kendi belirli gereksinimlerine ayarlamak bir temel olarak hizmet vermek için tasarlanmıştır ve olarak kullanılmamalıdır-üretim ortamıdır.
 - Bu belge, bir başvuru olarak geliştirilir ve tüm anlamına gelir, bir müşteri özel uyumluluk gereksinimlerini ve düzenlemeleri karşılayabilecek tanımlamak için kullanılmamalıdır. Müşterilerin onaylı müşteri uygulamaları kuruluşları yasal Destek'ten arama.
