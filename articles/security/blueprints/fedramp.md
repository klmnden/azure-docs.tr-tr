---
title: "Azure şeması Otomasyon - FedRAMP için Web uygulamaları"
description: "Azure şeması Otomasyon - FedRAMP için Web uygulamaları"
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: d0521d68bab8bd0b7db53a512da6d37033abd85e
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="azure-blueprint-automation---web-applications-for-fedramp"></a>Azure şeması Otomasyon - FedRAMP için Web uygulamaları

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov), bulut ürünleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izleme için standartlaştırılmış bir yaklaşım sağlayan bir ABD devlet genelinde program ve Hizmetler. Azure bu şeması Otomasyon - Web uygulamaları FedRAMP için basit bir Internet'e yönelik web uygulaması için uygun bir hizmet (Iaas) ortamı olarak FedRAMP uyumlu bir altyapı dağıtımı için yönergeler sağlar. Bu çözüm dağıtımını ve yapılandırmasını, müşteriler karşılamak belirli güvenlik ve uyumluluk gereksinimleri ve görevi görür müşterilerin için bir temel olarak yolları gösteren bir ortak başvuru mimarisi için Azure kaynaklarını otomatikleştirir ve kendi çözümleri Azure üzerinde yapılandırın. Çözüm NIST SP 800 53 tabanlı FedRAMP yüksek taban çizgisi denetimleri kümesini uygular. FedRAMP yüksek gereksinimleri ve bu çözüm hakkında daha fazla bilgi için bkz: [FedRAMP yüksek gereksinimleri - yüksek düzey genel bakış](fedramp-controls-overview.md). ***Not: Bu çözüm için Azure kamu dağıtır.***

Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında. Bir uygulama değişiklik yapmadan bu ortamına dağıtma tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Bu mimari, müşterilerin Azure FedRAMP uyumlu bir biçimde kullanmalarını sağlamak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır. 

Bu çözümün nasıl çalıştığı ilişkin hızlı genel bakış için bu izleme [video](https://aka.ms/fedrampblueprintvideo) açıklayan ve onun dağıtım gösterilmesi.

Tıklatın [burada](https://aka.ms/fedrampblueprintrepo) dağıtım yönergeleri için.

## <a name="solution-components"></a>Çözüm bileşenleri

Bu Azure şeması Otomasyonu Iaas web uygulama başvuru mimarisinin FedRAMP gereksinimleri ile uyumluluk elde müşterilere yardımcı olmak için önceden yapılandırılmış güvenlik denetimleri ile otomatik olarak dağıtır. Çözüm, Azure Resource Manager şablonları ve kaynak dağıtım ve Yapılandırma Kılavuzu PowerShell betikleri oluşur. Azure şeması eşlik [uyumluluk belgelerine](#compliance-documentation) , Azure ve dağıtılan kaynakları ve NIST SP ile 800 53 güvenlik denetimleri, böylece hizalama yapılandırmaları güvenlik denetiminin devralmadan belirten sağlanır kuruluşların Fast track uyumluluk yükümlülüklerin etkinleştiriliyor.

## <a name="architecture-diagram"></a>Mimari diyagramı

Bu çözüm, bir veritabanı arka ucu ile bir Iaas web uygulaması için bir başvuru mimarisi dağıtır. Web katmanı mimarisi içerir, veri katmanı, Active Directory altyapısını, uygulama ağ geçidi ve yük dengeleyici. İçin web ve veri katmanlarını dağıtılan sanal makinelerin bir kullanılabilirlik kümesine yapılandırılır ve SQL Server örneklerinin bir AlwaysOn Kullanılabilirlik grubu yüksek kullanılabilirlik için yapılandırılır. Sanal makinelerin etki alanına katılan ve Active Directory grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Yönetim jumpbox (savunma ana bilgisayarı) kaynaklara dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlar.

![Alternatif metin](images/fedramp-architectural-diagram.png?raw=true "Iaas web uygulama şeması Otomasyon FedRAMP uyumlu ortamlar için")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

* **Azure sanal makineler**
    - (1) yönetim/savunma (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server küme düğümü (Windows Server 2012 R2'de SQL Server 2016)
    - (1) SQL Server tanığı (Windows Server 2016 Datacenter)
    - (2) web/IIS (Windows Server 2016 Datacenter)
* **Kullanılabilirlik Kümeleri**
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümlerini ve Tanık
    - (1) web/IIS
* **Azure Sanal Ağ**
    - (1) /16 sanal ağlar
    - (5) /24 alt ağlar
    - DNS ayarları her iki etki alanı denetleyicilerine ayarlanır
* **Azure Load Balancer**
    - (1) SQL yük dengeleyici
* **Azure uygulama ağ geçidi**
    - (1) WAF uygulama etkin ağ geçidi
      - Güvenlik Duvarı modu: önleme
      - Kural kümesi: OWASP 3.0
      - Dinleyici: Bağlantı noktası 443
* **Azure Depolama**
    - (7) coğrafi olarak yedekli depolama hesapları
* **Azure Backup**
    - (1) kurtarma Hizmetleri kasası
* **Azure Anahtar Kasası.**
    - (1) anahtar kasası
* **Azure Active Directory**
* **Azure Resource Manager**
* **Azure Log Analytics**
* **Azure Otomasyonu**
    - (1) Otomasyon hesabı
* **Operations Management Suite**
    - (1) OMS çalışma

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde geliştirme ve uygulama öğeleri ayrıntılarını verir.

### <a name="network-segmentation-and-security"></a>Ağ kesimleme ve güvenlik

#### <a name="application-gateway"></a>Application Gateway

Mimari web uygulaması Güvenlik Duvarı (WAF) sahip bir uygulama ağ geçidi ve etkin OWASP ruleset kullanarak güvenlik açıkları riskini azaltır. Ek özellikler şunları içerir:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS v1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Engelleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile

#### <a name="virtual-network"></a>Sanal ağ

10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

Bu çözüm bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt sanal ağ içinde ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır.

Yapılandırma için bkz: [ağ güvenlik grupları](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) bu çözümü ile dağıtılmış. Kuruluşlar, ağ güvenlik grupları kullanarak yukarıdaki dosyasını düzenleyerek yapılandırabilirsiniz [bu belge](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, ayrılmış bir ağ güvenlik grubu (NSG) sahiptir:
- Uygulama ağ geçidi (LBNSG) için 1 NSG
- Jumpbox (MGTNSG) için 1 NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için 1 NSG
- SQL sunucuları ve dosya paylaşım tanığı (SQLNSG) için 1 NSG
- Web Katmanı (WEBNSG) için 1 NSG

#### <a name="subnets"></a>Alt ağlar

Her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Rest verileri

Mimari, çeşitli şifreleme ölçüleri kullanarak rest verileri korur.

#### <a name="azure-storage"></a>Azure Storage

Bekleyen sırasında veri şifreleme gereksinimlerini karşılamak üzere tüm depolama hesapları kullanmak [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

#### <a name="sql-database"></a>SQL Veritabanı

SQL veritabanını kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), gerçek zamanlı şifreleme ve şifre çözme REST bilgileri korumak için veri ve günlük dosyalarının gerçekleştirir. TDE, depolanan verileri güvence yetkisiz erişim ayarlanmadı sağlar. 

#### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi

Azure Disk şifrelemesi şifrelenmiş Windows Iaas sanal makine disklerini kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşiktir.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

[Operations Management Suite (OMS)](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) , sistem ve kullanıcı etkinliğini ve bunun yanı sıra sistem durumu ayrıntılı günlük kaydını sağlar. 

- **Etkinlik günlükleri:**[etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar.  
- **Tanılama günlüklerini:**[tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan her kaynak tarafından gösterilen tüm günlükleri.   Bu günlükler Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve uygulama ağ geçidi erişimi ve güvenlik duvarı günlüklerini içerir.
- **Günlük arşivleme:** Azure etkinlik ve tanılama günlükleri bağlanması için Azure günlük analizi işleme, depolama ve dashboarding için. Bekletme kuruluşa özgü bekletme gereksinimlerini karşılamak için kullanıcı tarafından 730 gününe yapılandırılabilen ayarlama.

### <a name="secrets-management"></a>Gizlilik Yönetimi

Çözüm, anahtarları ve gizli anahtarları yönetmek için Azure anahtar kasası kullanır.

- [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) şifreleme anahtarları ve gizli anahtarları bulut uygulamalar ve hizmetler tarafından kullanılan korunmasına yardımcı. 
- Çözüm Iaas sanal makine disk şifreleme anahtarları ve parolaları yönetmek için Azure anahtar kasası ile tümleşiktir.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojileri kimlik Azure ortamı yönetim yetenekleri sağlar.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir.
- Müşteri tarafından dağıtılan web uygulamasının kimlik doğrulama, Azure AD kullanılarak gerçekleştirilebilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve kullanıcı rolüne bağlı kaynaklara erişimi sınırlı olabilir.
- Dağıtılan bir Iaas Active Directory örneğine dağıtılan Iaas sanal makineleri için işletim sistemi düzeyinde kimlik yönetimi sağlar.
   
### <a name="compute-resources"></a>İşlem kaynaklarını

#### <a name="web-tier"></a>Web Katmanı

Web katmanı sanal makinelerinizde çözümü dağıtan bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden çok yalıtılmış donanım kümeler arasında dağıtılır emin olun.

#### <a name="database-tier"></a>Veritabanı katmanı

Veritabanı katmanı sanal makinelerin bir kullanılabilirlik kümesi çözüm dağıtan bir [AlwaysOn Kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). Always On kullanılabilirlik grubu özelliğini için yüksek kullanılabilirlik ve olağanüstü durum kurtarma yetenekleri sağlar. 

#### <a name="active-directory"></a>Active Directory

Etki alanına katılmış çözümü tarafından dağıtılmış olan tüm sanal makineleri ve Active Directory grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Active Directory sanal makineleri bir kullanılabilirlik kümesi içinde dağıtılır.


#### <a name="jumpbox-bastion-host"></a>Jumpbox (savunma ana bilgisayarı)

Yönetim jumpbox (savunma ana bilgisayarı) kaynaklara dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlar. Jumpbox sanal makinenin bulunduğu yönetim alt ağla ilişkili NSG bağlantılarında yalnızca TCP bağlantı noktası 3389 RDP sağlar. 

### <a name="malware-protection"></a>Kötü amaçlı yazılımdan koruma

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) tanımlamak ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım kaldırma yardımcı olan gerçek zamanlı koruma özelliği sanal makineler için bilinen kötü amaçlı veya istenmeyen yazılım zaman yapılandırılabilir uyarılarla dener yükleyin veya korumalı sanal makinelerin çalıştırın.

### <a name="patch-management"></a>Düzeltme Eki Yönetimi

Bu şeması Otomasyon tarafından dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca güncelleştirme dağıtımları düzeltme ekleri gerektiğinde Windows sunucularına dağıtmak için oluşturulabileceği OMS Azure Otomasyon çözümünü dağıtır.

### <a name="operations-management"></a>İşlem yönetimi

#### <a name="log-analytics"></a>Log Analytics

[Günlük analizi](https://azure.microsoft.com/services/log-analytics/) ve şirket içi ortamları Operations Management Suite (OMS) toplama ve Azure kaynaklarında tarafından oluşturulan veri analizini sağlar bir hizmettir.

#### <a name="oms-solutions"></a>OMS çözümleri

Aşağıdaki OMS çözümleri bu çözümün bir parçası önceden yüklenir:
- [AD Değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment)
- [Kötü amaçlı yazılımdan koruma değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware)
- [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker)
- [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started)
- [SQL Değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment)
- [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management)
- [Aracı Durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth)
- [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)
- [Değişiklik İzleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity)

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

### <a name="customer-responsibility-matrix"></a>Müşteri sorumluluk Matrisi

[Müşteri sorumlulukları matris](https://aka.ms/blueprinthighcrm) (Excel çalışma kitabı) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris uygulama denetimi için sorumlu bir şekilde Microsoft, müşteri sorumluluğunda olsun, her denetim (veya denetim Altbölüm), gösterir veya ikisi arasında paylaşılan. 

### <a name="control-implementation-matrix"></a>Denetim uygulama Matrisi

[Denetim uygulama matris](https://aka.ms/blueprintwacim) (Excel çalışma kitabı) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris gösterir, her denetim (veya denetim Altbölüm) için tasarlanmış bir Matristeki şeması Otomasyon denetimi ve uygulama ile nasıl hizalandığını 2) bir açıklama 1) uyguluyorsa müşteri-sorumlu bir şekilde müşteri sorumlulukları, requirement(s) denetler. Bu içerik da kullanılabilir [burada](fedramp-controls-overview.md).

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu Azure şeması çözüm JSON yapılandırma dosyaları ve kaynakları Azure içinde dağıtmak için Azure Resource Manager'ın API hizmeti tarafından işlenen PowerShell betikleri oluşur. Ayrıntılı dağıtım yönergeleri kullanılabilir [burada](https://aka.ms/fedrampblueprintrepo). ***Not: Bu çözüm için Azure kamu dağıtır.***

#### <a name="quickstart"></a>Hızlı Başlangıç
1. Kopyalama veya indirme [bu](https://aka.ms/fedrampblueprintrepo) yerel iş istasyonunuzu GitHub deposuna.

2. Dağıtım öncesi PowerShell betiğini çalıştırın: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Aşağıdaki düğmeyi tıklatın, Azure portalında oturum açın, gerekli ARM şablonu parametreleri girin ve tıklatın **satın alma**.

    [![Azure’a dağıtma](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.  
- Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.  
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  
- Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
- Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
