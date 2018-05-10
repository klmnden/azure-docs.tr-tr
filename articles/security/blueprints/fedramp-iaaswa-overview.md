---
title: Azure güvenlik ve uyumluluk şeması - FedRAMP için Web uygulaması
description: Azure güvenlik ve uyumluluk şeması - FedRAMP için Web uygulaması
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
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: b7a81db6a1caf11ac4a85a5202c5ed943225e849
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="azure-security-and-compliance-blueprint-web-application-for-fedramp"></a>Azure güvenlik ve uyumluluk şeması: Web uygulaması FedRAMP için

## <a name="overview"></a>Genel Bakış

[Federal Risk ve yetkilendirme yönetimi programı (FedRAMP)](https://www.fedramp.gov) bulut ürünleri için güvenlik değerlendirmesi, yetkilendirme ve sürekli izleme için standartlaştırılmış bir yaklaşım sağlayan bir ABD devlet genelinde program ve Hizmetler. Bu Azure güvenliği ve uyumluluk şeması Otomasyon basit bir Internet'e yönelik web uygulaması için uygun bir hizmet (Iaas) ortamı olarak FedRAMP uyumlu bir altyapı dağıtımı için yönergeler sağlar. Bu çözüm dağıtımını ve yapılandırmasını, müşteriler karşılamak belirli güvenlik ve uyumluluk gereksinimleri ve görevi görür müşterilerin için bir temel olarak yolları gösteren bir ortak başvuru mimarisi için Azure kaynaklarını otomatikleştirir ve kendi çözümleri Azure üzerinde yapılandırın. Çözüm NIST SP 800 53 tabanlı FedRAMP yüksek taban çizgisi denetimleri kümesini uygular. FedRAMP gereksinimleri ve bu çözüm hakkında daha fazla bilgi için bkz: [uyumluluk belgelerine](#compliance-documentation).
> [!NOTE]
> Bu çözüm için Azure kamu dağıtır.

Bu Azure güvenliği ve uyumluluk şeması Otomasyon Iaas web uygulama başvuru mimarisinin FedRAMP gereksinimleri ile uyumluluk elde müşterilere yardımcı olmak için önceden yapılandırılmış güvenlik denetimleri ile otomatik olarak dağıtır. Çözüm, Azure Resource Manager şablonları ve kaynak dağıtım ve Yapılandırma Kılavuzu PowerShell betikleri oluşur.

Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında. Bir uygulama değişiklik yapmadan bu ortamına dağıtma tamamen FedRAMP yüksek temel gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Bu mimari, müşterilerin Azure FedRAMP uyumlu bir biçimde kullanmalarını sağlamak için bir temel sağlar.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk değerlendirmesi gereksinimleri farklılık gösterebilir gibi bu mimarinin kullanılarak oluşturulan herhangi bir çözüm, her müşterinin uygulama ayrıntılarını dayanır.

Bu çözümün nasıl çalıştığı ilişkin hızlı genel bakış için bu izleme [video](https://aka.ms/fedrampblueprintvideo) açıklayan ve onun dağıtım gösterilmesi.

Tıklatın [burada](https://aka.ms/fedrampblueprintrepo) dağıtım yönergeleri için.

## <a name="architecture-diagram-and-components"></a>Mimarisi diyagramı ve bileşenleri
Bu çözüm, bir SQL Server arka uç bir Iaas web uygulaması için bir başvuru mimarisi dağıtır. Bir web katmanı, veri katmanı, Active Directory altyapısını, uygulama ağ geçidi ve yük dengeleyici mimarisi içerir. İçin web ve veri katmanlarını dağıtılan sanal makinelerin bir kullanılabilirlik kümesi içinde yapılandırılmış olan ve SQL Server örneklerinin bir AlwaysOn Kullanılabilirlik grubu yüksek kullanılabilirlik için yapılandırılır. Sanal makinelerin etki alanına katılan ve Active Directory grup ilkeleri, işletim sistemi düzeyinde güvenlik ve uyumluluk yapılandırmaları uygulamak için kullanılır. Savunma ana bilgisayar kaynaklarına dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlar. **Azure VPN veya Azure ExpressRoute bağlantısı başvuru mimarisi alt ağ yönetimi ve veri içe için yapılandırma önerir.**

![Iaas Web uygulaması FedRAMP başvuru mimarisi diyagramı için](images/fedramp-iaaswa-architecture.png?raw=true "Iaas Web uygulaması için FedRAMP başvuru mimarisi diyagramı")

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

- Azure Sanal Makineler
    - (1) güvenlikli ana bilgisayar (Windows Server 2016 Datacenter)
    - (2) active Directory etki alanı denetleyicisi (Windows Server 2016 Datacenter)
    - (2) SQL Server küme düğümü (Windows Server 2016 SQL Server 2017)
    - (2) web/IIS (Windows Server 2016 Datacenter)
- Kullanılabilirlik Kümeleri
    - (1) active Directory etki alanı denetleyicileri
    - (1) SQL küme düğümleri
    - (1) web/IIS
- Azure Sanal Ağ
    - (1) /16 sanal ağlar
    - (5) /24 alt ağlar
    - DNS ayarları her iki etki alanı denetleyicilerine ayarlanır
- Azure Load Balancer
- Azure Application Gateway
    - (1) WAF uygulama etkin ağ geçidi
        - Güvenlik Duvarı modu: önleme
        - Kural kümesi: OWASP 3.0
        - Dinleyici: bağlantı noktası 443
- Azure Storage
    - (7) coğrafi olarak yedekli depolama hesapları
- Azure bulut tanığı
- Kurtarma Hizmetleri kasası
- Azure Key Vault
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Operations Management Suite (OMS)
- Azure İzleyici

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde geliştirme ve uygulama öğeleri ayrıntılarını verir.

**Savunma konak**: savunma konak tek kaynaklarına dağıtılan erişmek için Yöneticiler için güvenli bir bağlantı sağlar giriş noktasıdır. Savunma ana bilgisayarın NSG bağlantılarında yalnızca TCP bağlantı noktası 3389 RDP sağlar. Müşteriler daha fazla kuruluş sistem sağlamlaştırma gereksinimlerine karşılamak için savunma konak yapılandırabilirsiniz.

### <a name="virtual-network"></a>Sanal ağ
10.200.0.0/16 bir adres alanı ile özel bir sanal ağ mimarisini tanımlar.

**Ağ güvenlik grupları**: Bu çözüm bir mimari ayrı web alt ağ, veritabanı alt ağ, Active Directory alt ve yönetimi alt sanal ağ içinde ağ kaynaklarında dağıtır. Alt ağları için yalnızca bu gerekli sistem ve yönetim işlevselliği için alt ağlar arasında trafiği kısıtlamak için ayrı alt ağlara uygulanan ağ güvenlik grubu kural tarafından mantıksal olarak ayrılır.

Lütfen yapılandırma için bkz: [ağ güvenlik grubu](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) bu çözümü ile dağıtılmış. Müşteriler, ağ güvenlik grupları kullanarak yukarıdaki dosyasını düzenleyerek yapılandırabilirsiniz [bu belge](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) bir kılavuz olarak.

Alt ağlar, ayrılmış bir ağ güvenlik grubu (NSG) sahiptir:
- Uygulama ağ geçidi (LBNSG) için 1 NSG
- Savunma ana bilgisayar (MGTNSG) için 1 NSG
- Birincil ve yedek etki alanı denetleyicileri (ADNSG) için 1 NSG
- SQL Server (SQLNSG) için 1 NSG
- Web Katmanı (WEBNSG) için 1 NSG

**Alt ağlar**: her alt ağ, karşılık gelen NSG ile ilişkilidir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, çeşitli şifreleme ölçüleri kullanarak rest verileri korur.

**Azure depolama**: rest sırasında veri şifreleme gereksinimlerini karşılamak üzere tüm depolama hesapları kullanmak [depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**: SQL Server kullanacak şekilde yapılandırılmış [saydam veri şifreleme (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), gerçek zamanlı şifreleme ve şifre çözme REST bilgileri korumak için veri ve günlük dosyalarının gerçekleştirir. TDE, depolanan verileri güvence yetkisiz erişim ayarlanmadı sağlar.

Müşteriler, aşağıdaki SQL Server güvenlik önlemleri de yapılandırabilirsiniz:
-   [AD kimlik doğrulama ve yetkilendirme](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) veritabanı kullanıcıları ve diğer Microsoft Hizmetleri tek bir merkezi konumda kimlik yönetimini sağlar.
-   [SQL veritabanı denetimi](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-auditing-get-started) parçaları veritabanı olayları ve bir Azure depolama hesabında bunları Denetim günlüğüne yazar.
-   [Güvenlik duvarı kuralları](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-firewall-configure) uygun izinlerin verildiğinden kadar veritabanı sunucularına tüm erişimi engelleme. Güvenlik duvarı, her bir isteğin kaynak IP adresine göre veritabanlarına erişim verir.
-   [SQL tehdit algılama](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-threat-detection-get-started) şüpheli veritabanı etkinliklerini, olası güvenlik açıkları, SQL ekleme saldırıları ve anormal veritabanı erişimi için güvenlik uyarıları sağlayarak göründüklerinde algılamayı ve olası tehditler yanıta etkinleştirir desenler.
-   [Sütunları'her zaman şifreli](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault) hassas verileri hiçbir zaman veritabanı sistem içinde düz metin olarak göründüğünden emin olun. Veri şifreleme etkinleştirdikten sonra yalnızca istemci uygulamaları veya uygulama sunucuları anahtarlara erişimi ile düz metin verilere erişebilir.
-   [SQL veritabanı dinamik veri maskeleme](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-dynamic-data-masking-get-started) referans mimarisi dağıtıldıktan sonra yapılabilir. Müşteriler dinamik veri maskeleme ayarlarına kendi veritabanı şeması uyması için ayarlamanız gerekir.

**Azure Disk şifrelemesi**: Azure Disk şifrelemesi, Windows Iaas sanal makine disklerini şifrelenmiş için kullanılır. [Azure Disk şifrelemesi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) işletim sistemi ve veri diskleri için birim şifrelemesi sağlamak için Windows BitLocker özelliğinden yararlanır. Çözüm denetlemeye yardımcı olmak ve disk şifreleme anahtarlarını yönetmek için Azure anahtar kasası ile tümleşiktir.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojileri kimlik Azure ortamında yönetim yeteneklerini sağlar:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir.
- Müşteri tarafından dağıtılan web uygulamasının kimlik doğrulama, Azure AD kullanılarak gerçekleştirilebilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Azure rol tabanlı erişim denetimi (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve kullanıcı rolüne bağlı kaynaklara erişimi sınırlı olabilir.
- Dağıtılan bir Iaas Active Directory örneğine dağıtılan Iaas sanal makineleri için işletim sistemi düzeyinde kimlik yönetimi sağlar.

### <a name="security"></a>Güvenlik
**Gizlilik Yönetimi**: Çözüm kullanır [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Yönetimi anahtarları ve gizli anahtarları için. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure anahtar kasası Iaas sanal makine disk şifreleme anahtarları ve gizli anahtarları için bu başvuru mimarisinin yönetmenize yardımcı olur.

**Düzeltme Eki Yönetimi**: Bu Azure güvenliği ve uyumluluk şeması Otomasyon tarafından dağıtılan Windows sanal makineleri, Windows Update hizmetinden otomatik güncelleştirmeleri almak için varsayılan olarak yapılandırılır. Bu çözüm Ayrıca güncelleştirme dağıtımları düzeltme ekleri gerektiğinde Windows sunucularına dağıtmak için oluşturulabileceği Azure Otomasyon çözümünü dağıtır.

**Kötü amaçlı yazılımdan koruma**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) sanal makineler için yardımcı tanımlamak ve virüsler, casus yazılım ve yapılandırılabilir uyarılarla diğer kötü amaçlı yazılım kaldırma gerçek zamanlı koruma özelliği sağlar kötü amaçlı veya istenmeyen bilinen başlatıldığında yazılım yüklemek veya korumalı sanal makineleri çalıştırmak dener.

**Uygulama ağ geçidi**: mimarisi web uygulaması güvenlik duvarı ile (WAF) bir uygulama ağ geçidi kullanarak güvenlik açıkları riskini azaltır ve OWASP ruleset etkin. Ek özellikler şunları içerir:

- [SSL uç bitiş](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL boşaltma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS v1.0 ve v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (WAF mod)
- [Engelleme modu](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile

### <a name="business-continuity"></a>İş sürekliliği

**Yüksek kullanılabilirlik**: en az bir sanal makine %99,95 toplantı planlı veya plansız bir bakım olayı sırasında kullanılabilir Azure SLA. Tüm web katmanı çözümü dağıtır ve veri katmanı sanal makinelerin bir [kullanılabilirlik kümesi](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Kullanılabilirlik kümeleri, sanal makinelerin kullanılabilirliğini artırmak için birden çok yalıtılmış donanım kümeler arasında dağıtılır emin olun. Ayrıca, bu çözüm bir kullanılabilirlik kümesi SQL Server sanal makine dağıtan bir [AlwaysOn Kullanılabilirlik grubu](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). Always On kullanılabilirlik grubu özelliğini için yüksek kullanılabilirlik ve olağanüstü durum kurtarma yetenekleri sağlar.

**Kurtarma Hizmetleri kasası**: [kurtarma Hizmetleri kasası](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) yedekleme verileri barındırır ve tüm yapılandırmaları bu mimarisinde Azure sanal makineleri korur. Bir kurtarma Hizmetleri kasası ile müşteriler dosya ve klasörlerin bir Iaas sanal makineden daha hızlı geri yükleme süreleri etkinleştirme tüm VM geri yüklemeden geri yükleyebilirsiniz.

**Bulut Tanık**: [bulut Tanık](https://docs.microsoft.com/en-us/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) yük devretme kümesi çekirdek tanığı yönetim noktası olarak Azure yararlanır Windows Server 2016 türüdür. Tüm diğer çekirdek tanığı gibi bulut Tanık bir oy alır ve çekirdek hesaplamalara katılabilir, ancak standart genel kullanıma açık Azure Blob Depolama kullanır. Bu, genel bulutta barındırılan sanal makineleri, ek bakım yükünü ortadan kaldırır.

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

OMS, sistem durumu yanı sıra sistem ve kullanıcı etkinliğini, ayrıntılı günlük kaydını sağlar. OMS [günlük analizi](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) çözüm toplar ve Azure kaynaklarında tarafından oluşturulan verileri çözümler ve şirket içi ortamları.

- **Etkinlik günlükleri:**[etkinlik günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bir aboneliğindeki kaynaklar üzerinde gerçekleştirilen işlemler hakkında bilgi sağlar.   Etkinlik günlükleri bir işlemin Başlatıcı belirlemenize yardımcı olabilir geçişi ve durum süresi.
- **Tanılama günlüklerini:**[tanılama günlükleri](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan her kaynak tarafından gösterilen tüm günlükleri.   Bu günlükler Windows olayı sistem günlükleri, Azure depolama günlükleri, anahtar kasası denetim günlüklerini ve uygulama ağ geçidi erişimi ve güvenlik duvarı günlüklerini içerir.
- **Günlük arşivleme:** tüm tanılama günlükleri için merkezi ve şifrelenmiş Azure depolama hesabı arşivleme yazma. Bekletme kuruluşa özgü bekletme gereksinimlerini karşılamak için kullanıcı tarafından 730 gün olarak yapılandırılabilir, ayarlama. Bu günlükler, işleme, depolama ve Pano raporlama için Azure günlük Analizi'ne bağlayın.

Ayrıca, aşağıdaki OMS çözümleri Bu mimarinin bir parçası olarak yüklenir. Bu çözümlerin FedRAMP güvenlik denetimleri ile hizalamak için yapılandırmak için Müşteri'nin sorumluluk olduğuna dikkat edin:
-   [AD değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli bir listesini sağlar.
-   [Kötü amaçlı yazılımdan koruma değerlendirme](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): kötü amaçlı yazılımdan koruma çözümünü kötü amaçlı yazılım, tehditleri ve koruma durumunu raporlar.
-   [Azure Otomasyonu](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): Azure Otomasyon çözümünü depolar, çalışır ve runbook'ları yönetir.
-   [Güvenlik ve Denetim](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): güvenlik ve Denetim Panosu güvenlik etki alanları, önemli sorunları, algılama, tehdit bilgileri ve ortak güvenlik sorguları ölçümleri sağlayarak kaynakların güvenlik durumu üst düzey bir fikir sağlar.
-   [SQL değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): SQL sistem durumu denetimi çözüm risk ve sunucu ortamları durumunu düzenli aralıklarla değerlendirir ve öneriler için dağıtılan sunucu altyapısı belirli öncelikli listesi, müşteri sağlar.
-   [Güncelleştirme yönetimi](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): güncelleştirme yönetim çözümü, işletim sistemi güvenlik güncelleştirmeleri, kullanılabilir güncelleştirmeleri durumunu ve gerekli güncelleştirmeleri yükleme işlemi de dahil olmak üzere müşteri yönetimine izin verir.
-   [Aracı sistem durumu](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): aracı durumu çözümüne raporlarını kaç aracıları dağıtılan ve kullanıcıların coğrafi dağılımı yanı sıra, yanıt vermeyen kaç aracıları ve işletimsel veri gönderiliyor Aracı sayısına.
-   [Azure etkinlik günlükleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): etkinlik günlüğü analiz çözümü yardımcı olduğunu Azure etkinlik günlüğü Analizi ile bir müşteri için tüm Azure abonelikleri arasında.
-   [Değişiklik izleme](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): müşterileri kolayca ortamında değişiklikleri belirlemek değişiklik izleme çözümü sağlar.

**Azure İzleyici**
[Azure İzleyici](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/) kullanıcıların izleyen performans, güvenlik korumak ve kuruluşların denetim, uyarılar oluşturabilir ve API çağrıları izleme de dahil olmak üzere, verileri arşivlemek etkinleştirerek eğilimleri belirlemenize yardımcı olur müşterilerin Azure kaynaklarında.

## <a name="threat-model"></a>Tehdit modeli
Bu başvuru mimarisi için veri akış diyagramı kullanılabilir [karşıdan](https://aka.ms/fedrampWAdfd) veya altında bulunabilir. Bu model, müşteriler değişiklikler yaparken sistem altyapısında potansiyel risk puanları anlamanıza yardımcı olabilir.

![Iaas Web uygulaması FedRAMP tehdit modeli için](images/fedramp-iaaswa-threat-model.png?raw=true "FedRAMP tehdit modeli için Iaas Web uygulaması")

## <a name="compliance-documentation"></a>Uyumluluk belgeleri

[Azure güvenliği ve uyumluluk şeması - FedRAMP yüksek müşteri sorumluluk matrisi](https://aka.ms/blueprinthighcrm) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris her denetimi uyarlamasını Microsoft, müşteri sorumluluk veya ikisi arasında paylaşılan olup olmadığını gösterir.

[Azure güvenliği ve uyumluluk şeması - FedRAMP Iaas Web uygulama yüksek denetim uygulama matris](https://aka.ms/blueprintwacim) FedRAMP yüksek temeli tarafından gerekli tüm güvenlik denetimleri listeler. Matris denetimleri ayrıntılı açıklamalar için nasıl uygulama kapsanan her denetim gereksinimlerini karşılayan biri de dahil olmak üzere Iaas web uygulama mimarisi tarafından ele alınan bilgiler sağlar.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu Azure güvenliği ve uyumluluk şeması Otomasyon den oluşur JSON yapılandırma dosyaları ve kaynakları Azure içinde dağıtmak için Azure Resource Manager'ın API hizmeti tarafından işlenen PowerShell komut dosyaları. Ayrıntılı dağıtım yönergeleri kullanılabilir [burada](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Bu çözüm için Azure kamu dağıtır.

#### <a name="quickstart"></a>Hızlı Başlangıç
1. Kopyalama veya indirme [bu](https://aka.ms/fedrampblueprintrepo) yerel iş istasyonunuzu GitHub deposuna.

2. Dağıtım öncesi PowerShell betiğini çalıştırın: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Aşağıdaki düğmeyi tıklatın, Azure portalında oturum açın, gerekli ARM şablonu parametreleri girin ve tıklatın **satın alma**.

    [![Azure’a dağıtma](http://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Yönerge ve öneriler
### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
Güvenli VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bir şekilde bu Iaas Web uygulaması başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bağlantı kurmak için yapılandırılması gerekir. Müşteriler, uygun bir VPN ya da ExpressRoute ayarlayarak, aktarım sırasında bir veri koruma katmanı ekleyebilirsiniz.

Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ve bir Azure sanal ağ arasında özel bir sanal bağlantı oluşturulabilir. Bu bağlantının Internet üzerinden gerçekleşir ve güvenli bir şekilde Müşteri'nin ağınız ve Azure arasında şifrelenmiş bir bağlantı içinde "tünel" bilgisini müşterilerin sağlar. Siteden siteye VPN ölçekteki kurumların on yılları için dağıtılan güvenli, olgun bir teknolojidir. [IPSec tünel modu](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) bu seçeneği bir şifreleme mekanizması olarak kullanılır.

VPN tüneli içindeki trafiği Internet bir siteden siteye VPN ile geçiş olduğundan, Microsoft başka bir ve daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute olduğu adanmış WAN Azure ve bir şirket içi konum veya bir Exchange barındırma sağlayıcısı arasındaki bağlantı. ExpressRoute bağlantıları Internet üzerinden geçmemektedir gibi bu bağlantıları Internet üzerinden daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik sunar. Ayrıca, bu doğrudan bağlantı, müşterinin telekomünikasyon sağlayıcısının olduğundan, veri Internet üzerinden yolculuk değil ve kendisine bu nedenle olarak gösterilmez.

Azure için bir şirket içi ağ genişleten güvenli karma ağ uygulamak için en iyi uygulamalar sağlanmıştır [kullanılabilir](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid).

## <a name="disclaimer"></a>Bildirim

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT HİÇBİR AÇIK, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER GARANTİDE BULUNMAZ. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.  
- Bu belge müşterilerle herhangi bir Microsoft ürünü veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.  
- Bu belgedeki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  
- Bu mimari müşterilerin belirli gereksinimlerine ayarlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında.
- Bu belge, bir başvuru olarak geliştirilen ve tüm anlamına gelir, bir müşteri belirli uyumluluk gereksinimleri ve düzenlemelere karşılayabilecek tanımlamak için kullanılmamalıdır. Müşteriler kendi organizasyon onaylı müşteri uygulamaları üzerinde yasal Destek'ten arama.
