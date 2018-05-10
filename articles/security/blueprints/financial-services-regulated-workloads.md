---
title: Azure güvenlik ve uyumluluk şeması - FFIEC finansal hizmetler düzenlenen iş yükleri
description: Azure güvenlik ve uyumluluk şeması - FFIEC finansal hizmetler düzenlenen iş yükleri
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 17794288-9074-44b5-acc8-1dacceb3f56c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2018
ms.author: jomolesk
ms.openlocfilehash: f1339af22132d19f14ea8ebb72fe0e6bd45b7fad
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="azure-security-and-compliance-blueprint---ffiec-financial-services-regulated-workloads"></a>Azure güvenlik ve uyumluluk şeması - FFIEC finansal hizmetler düzenlenen iş yükleri

## <a name="overview"></a>Genel Bakış

Azure güvenlik ve uyumluluk şeması - FFIEC finansal hizmetler Regulated iş yükleri yardımcı güvenli ve uyumlu bir platform bulutta hassas verileri işlemek üzere tasarlanmış bir hizmet (PaaS) web uygulaması olarak dağıtın. Şeması otomatik betikler ve basit başvuru mimarisi ve Microsoft Azure çözümleri benimsenmesi basitleştirmeye yardımcı olan bir tasarım sergiler Kılavuzu oluşur. Bu şeması yükünü ve buluta dağıtma maliyetini azaltma yolları arayan kuruluşlar ihtiyaçlarını karşılamak üzere bir uçtan uca çözüm gösterilmektedir.

Bu şeması tarafından American Institute, Onaylı Ortak muhasebe gibi - SOC 1, SOC 2, ödeme kartı sektör veri güvenliği standartları council'ın DSS 3.2 ve FFIEC için ayarlanmış sıkı uyumlu standartları gereksinimlerini karşılamak üzere tasarlanmıştır Toplama, depolama ve hassas finansal verileri alma. Bu tür veriler uygun işlenmesini güvenli, uyumlu, çok katmanlı ortamında finansal verileri yöneten bir çözümü gösterir. Çözümü bir uçtan uca Azure tabanlı PaaS çözüm dağıtılır. 

Şeması, müşterilerin oluşturmanıza ve bulutta güvenli veri yönetme gereksinimleri anlamak bir temel olarak hizmet için tasarlanmıştır. Çözüm bir üretim dağıtımında kullanılmamalıdır-olduğunu anlamak, tasarım ve Azure Hizmetleri; dağıtmak ancak temel olarak, müşterilerin Microsoft Azure güvenli ve uyumlu bir şekilde kullanmalarını sağlamak için tasarlanmıştır.

Onaylanmış bir denetçi bu şeması dayalı herhangi bir üretim müşteri çözümü onaylaması gerekir. Çözümleri her müşterinin uygulama ve Coğrafya ayrıntılarını göre farklılık gösterebilir.

Bu çözümün nasıl çalıştığı ilişkin hızlı genel bakış için bu izleme [video](https://aka.ms/fsiblueprintvideo) açıklayan ve onun dağıtım gösterir.

## <a name="solution-components"></a>Çözüm bileşenleri

Mimari aşağıdaki bileşenlerden oluşur ve Azure PCI DSS uyumluluk çözüm dağıtım özelliklerini kullanır.

- **Mimari diyagramı**. Diyagramda Contoso Webstore çözüm için kullanılan Referans Mimarisi gösterilmektedir.
- **Dağıtım şablonları**. Bu dağıtımda [Azure Resource Manager şablonları](/azure/azure-resource-manager/resource-group-overview#template-deployment) Kurulum sırasında yapılandırma parametrelerini belirterek mimarisinin bileşenlerine Microsoft Azure otomatik olarak dağıtmak için kullanılan.
- **Dağıtım betikleri otomatik**. Bu komut dosyaları, uçtan uca çözüm dağıtımına yardımcı olur. Komut dosyaları oluşur:
    - Bir modül yükleme ve [genel yönetici](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) Kurulum komut dosyası yükleyin ve gerekli PowerShell modülleri ve genel yönetici rolleri doğru şekilde yapılandırıldığını doğrulamak için kullanılır. 
    - PowerShell komut dosyası yüklemesi bir .zip dosyası ve önceden derlenmiş demo web uygulamasıyla içeren bir .bacpac dosyasına aracılığıyla sağlanan uçtan uca çözümü dağıtmak için kullanılan [SQL veritabanı örnek](https://github.com/Microsoft/azure-sql-security-sample). İçerik. Bu çözüm için kaynak kodunu [ödeme işleme şeması kod deposu] Gözden Geçirilmeye hazır olduğunu [kodu-repo]. 

## <a name="architectural-diagram"></a>Mimari diyagramı

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Kullanıcı senaryosu

Aşağıdaki şeması adresleri aşağıdaki kullanım örneği.

> Bu senaryo, bir PaaS içine hassas verileri Azure tabanlı çözüm bulut kurgusal webstore nasıl taşınır gösterilmektedir. Örnek bir çözüm işleme ve temel kullanıcı bilgileri ve seçili hassas verileri topluluğunu gösterir. Bu iş Azure güvenlik ve uyumluluk şeması - PCI DSS uyumlu ödeme işlenirken ortamları taşır. Bu iş üzerinde genişletme hakkında daha fazla bilgi için ["Gözden geçirin ve yönergeler için uygulama"](https://aka.ms/pciblueprintprocessingoverview) kağıt PCI DSS uyumlu ortamları gözden sağlar.

### <a name="use-case"></a>Kullanım örneği
Küçük webstore adlı *Contoso Webstore* Müşteri ödeme bilgileri buluta içeren finansal verileri taşımak hazırdır. 

Contoso Webstore yönetici için kendi hedeflerinize ulaşmak için hızlı bir şekilde dağıtılabilir bir çözüm arıyor. Toplamak, depolamak ve katı uyumluluk gereksinimlere uygun finansal verileri almak için Azure'nın nasıl kullanılabileceğini kendi Paydaşlar ile tartışmak için kendisine bu--kavram kanıtı (POC) kullanır.

> Uygun güvenlik yürütmek için sorumlu olacaktır ve uyumluluk gereksinimleri farklılık gösterebilir gibi bu POC tarafından kullanılan mimarisi ile oluşturulan herhangi bir çözüm incelenmesi uygulama ve Coğrafya ayrıntılarını temel. 

### <a name="elements-of-the-foundational-architecture"></a>Temel mimari öğeleri

Temel mimari aşağıdaki kurgusal öğeleriyle tasarlanmıştır:

Etki alanı site `contosowebstore.com`

Kullanım örneği göstermek ve kullanıcı arabirimi bir anlayış sağlamak için kullanıcı rolleri çalışan.

#### <a name="role-site-and-subscription-admin"></a>Rol: Site ve abonelik yönetimi

|Öğe      |Örnek|
|----------|------|
|Kullanıcı adı: |`adminXX@contosowebstore.com`|
| Ad: |`Global Admin Azure PCI Samples`|
|Kullanıcı türü:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- Yönetici hesabı maskelenmeyen finansal bilgileri okunamıyor. Tüm Eylemler günlüğe kaydedilir.
- Yönetici hesabını yönetin veya SQL veritabanına oturum açın.
- Yönetici hesabı, Active Directory ve abonelikleri yönetebilirsiniz.

#### <a name="role-sql-administrator"></a>Rol: SQL Yöneticisi

|Öğe      |Örnek|
|----------|------|
|Kullanıcı adı: |`sqlAdmin@contosowebstore.com`|
| Ad: |`SQLADAdministrator PCI Samples`|
| Ad: |`SQL AD Administrator`|
|Soyadı: |`PCI Samples`|
|Kullanıcı türü:| `Administrator`|

- Sqladmin hesabı filtrelenmemiş finansal bilgi görüntüleyemezsiniz. Tüm Eylemler günlüğe kaydedilir.
- SQL veritabanı sqladmin hesabını yönetebilir.

#### <a name="role-clerk"></a>Rol: yazıcısı

|Öğe      |Örnek|
|----------|------|
|Kullanıcı adı:| `receptionist_EdnaB@contosowebstore.com`|
| Ad: |`Edna Benson`|
| Ad:| `Edna`|
|Soyadı:| `Benson`|
| Kullanıcı türü: |`Member`|

Edna Benson resepsiyonist ve iş yöneticisidir. Aynen, müşteri bilgileri doğru olduğunu ve faturalama tamamlandı sağlamak için sorumludur. Edna Contoso Webstore demo Web sitesi tüm etkileşim için oturum açmış kullanıcının ' dir. Edna aşağıdaki haklara sahiptir: 

- Edna oluşturmak ve müşteri bilgileri okuyun.
- Edna müşteri bilgilerini değiştirebilirsiniz.
- Edna finansal bilgi üzerine yazabilirsiniz.
- Edna hesabı filtrelenmemiş finansal bilgi görüntüleyemezsiniz.



### <a name="contoso-webstore---estimated-pricing"></a>Contoso tahmini Webstore fiyatlandırma-

Bu temel mimarisini ve örnek web uygulaması aylık bir ücret yapısı ve kullanım maliyeti çözümü boyutlandırma göz önünde bulundurulması gereken saatte sahiptir. Kullanarak bu maliyetleri tahmin edilebilir [Azure maliyetlendirme hesaplayıcı](https://azure.microsoft.com/pricing/calculator/). Bu çözüm için aylık tahmini maliyet Eylül 2017 itibariyle olduğu ~ $2500 bu 1000 ABD Doları/iletilerin kullanım ücret ana v2 içerir. Bu maliyetleri kullanım miktarına göre değişir ve değiştirilebilir. Dağıtım zaman daha doğru bir tahmin için tahmini aylık maliyetlerini hesaplamak için müşteri incumbent. 

Bu çözüm, aşağıdaki Azure hizmetlerini kullanılır. Dağıtım mimarisi ayrıntılarını içinde bulunur [dağıtım mimarisi](#deployment-architecture) bölümü.

>- Application Gateway
>- Azure Active Directory
>- Uygulama hizmeti ortamı v2
>- Log Analytics
>- Azure Key Vault
>- Ağ Güvenlik Grupları
>- Azure SQL DB
>- Azure Load Balancer
>- Application Insights
>- Azure Güvenlik Merkezi
>- Azure Web Uygulaması
>- Azure Otomasyonu
>- Azure Otomasyonu runbook'ları
>- Azure DNS
>- Azure Sanal Ağ
>- Azure Sanal Makinesi
>- Azure kaynak grubu ve ilkeleri
>- Azure Blob Depolama
>- Azure Active Directory rol tabanlı erişim denetimi (RBAC)

## <a name="deployment-architecture"></a>Dağıtım mimarisi

Aşağıdaki bölümde geliştirme ve uygulama öğeleri ayrıntılarını verir.

### <a name="network-segmentation-and-security"></a>Ağ kesimleme ve güvenlik

![](images/pci-tiers-diagram.png)

#### <a name="application-gateway"></a>Application Gateway

Temel mimari bir uygulama ağ geçidi kullanarak bir web uygulaması Güvenlik Duvarı (WAF) ve etkin OWASP ruleset güvenlik açıkları riskini azaltır. Ek özellikler şunları içerir:

- [SSL uç bitiş](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [SSL boşaltma](/azure/application-gateway/application-gateway-ssl-portal) etkin
- [TLS v1.0 ve v1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell) devre dışı
- [Web uygulaması güvenlik duvarı](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (WAF mod)
- [Engelleme modu](/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- [Tanılama günlükleri](/azure/application-gateway/application-gateway-diagnostics) etkin
- [Özel sistem durumu araştırmalarının](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations), ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

#### <a name="virtual-network"></a>Sanal ağ

Temel mimari 10.0.0.0/16 bir adres alanı ile özel bir sanal ağ tanımlar.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

Her ağ katmanı ayrılmış bir ağ güvenlik grubu (NSG) vardır:

- Güvenlik Duvarı ve uygulama ağ geçidi WAF için bir çevre ağ güvenlik grubu
- Yönetim jumpbox (savunma ana bilgisayarı) için bir NSG
- Uygulama hizmeti ortamı için bir NSG

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü güvenli ve doğru çalışması için açıldı. 

Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:

- Etkin [tanılama günlüklerini ve olayları](/azure/virtual-network/virtual-network-nsg-manage-log) depolama hesabında depolanır 
- Günlük analizi bağlı [NSG'ın tanılama](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

 
#### <a name="subnets"></a>Alt ağlar
 Her alt ağ, karşılık gelen NSG ile ilişkili olduğundan emin olun.

#### <a name="custom-domain-ssl-certificates"></a>Özel etki alanı SSL sertifikaları
 Özel etki alanı SSL sertifikası kullanarak HTTPS trafiği etkinleştirilir.

### <a name="data-at-rest"></a>Bekleyen veriler

Mimari, şifreleme, veritabanı denetimi ve diğer ölçülere kullanarak rest verileri korur.

#### <a name="azure-storage"></a>Azure Storage

Şifrelenmiş veri adresindeki rest gereksinimlerini karşılamak için tüm [Azure Storage](https://azure.microsoft.com/services/storage/) kullanan [depolama hizmeti şifrelemesi](/azure/storage/storage-service-encryption).

#### <a name="azure-sql-database"></a>Azure SQL Database

Azure SQL veritabanı örneğinde aşağıdaki veritabanı güvenlik önlemleri kullanır:

- [AD kimlik doğrulama ve yetkilendirme](/azure/sql-database/sql-database-aad-authentication)
- [SQL veritabanı denetimi](/azure/sql-database/sql-database-auditing-get-started)
- [Saydam veri şifreleme](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)
- [Güvenlik duvarı kuralları](/azure/sql-database/sql-database-firewall-configure), ana çalışan havuzları ve istemci IP yönetimi için izin verme
- [SQL tehdit algılama](/azure/sql-database/sql-database-threat-detection-get-started)
- [Her zaman şifrelenmiş sütunlar](/azure/sql-database/sql-database-always-encrypted-azure-key-vault)
- [SQL veritabanı dinamik veri maskeleme](/azure/sql-database/sql-database-dynamic-data-masking-get-started), dağıtım sonrası PowerShell komut dosyası kullanma

### <a name="logging-and-auditing"></a>Günlüğe kaydetme ve denetleme

[Günlük analizi](https://azure.microsoft.com/services/log-analytics) Contoso Webstore tüm sistemi ve kullanıcı etkinliğini kapsamlı günlük kaydıyla sağlamak, finansal veri günlük kaydı içerir. Değişiklikleri gözden ve doğruluk doğrulandı. 

- **Etkinlik günlükleri.**  [Etkinlik günlükleri](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar.
- **Tanılama günlükleri.**  [Tanılama günlüklerini](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan her kaynak tarafından gösterilen tüm günlükleri. Bu günlükler Windows olayı sistem günlükleri, Azure Blob Depolama günlükleri, tablolar ve sıra günlükleri içerir.
- **Güvenlik duvarı günlükleri.**  Uygulama ağ geçidi günlüklerine erişmek ve tam tanılama sağlar. Güvenlik Duvarı günlüklerini etkin WAF sahip uygulama ağ geçidi kaynakları için kullanılabilir.
- **Arşivleme oturum açın.**  Tüm tanılama günlükleri için merkezi ve şifreli bir Azure depolama hesabı için tanımlanan bekletme süresi (2 gün) ile arşivleme yazmak için yapılandırılır. Günlükleri işleme, depolama ve dashboarding için Azure günlük Analizi'ne bağlanmıştır. [Günlük analizi](https://azure.microsoft.com/services/log-analytics) ve şirket içi ortamları toplamak ve bulut kaynakları tarafından oluşturulan verileri çözümlemek yardımcı olan bir hizmettir.

### <a name="encryption-and-secrets-management"></a>Şifreleme ve gizli anahtarları Yönetimi

Contoso Webstore tüm hassas verileri şifreler ve anahtarlarını yönetmek ve CHD alınmasını önlemek için Azure anahtar kasası kullanır.

- [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) şifreleme anahtarları ve gizli anahtarları bulut uygulamalar ve hizmetler tarafından kullanılan korunmasına yardımcı. 
- [SQL TDE'nin](/sql/relational-databases/security/encryption/transparent-data-encryption) tüm müşteri finansal verileri şifrelemek için kullanılır.
- Veri diski kullanarak depolanan [Azure Disk şifrelemesi](/azure/security/azure-security-disk-encryption) ve BitLocker'ı.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojileri kimlik Azure ortamı yönetim yetenekleri sağlar.

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.
- Uygulama kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure AD uygulama Azure SQL veritabanı kimlik doğrulaması için de kullanır. Daha fazla bilgi için bkz: [her zaman şifreli: SQL veritabanındaki hassas verileri korumaya](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory kimlik koruması](/azure/active-directory/active-directory-identityprotection) kuruluşunuzdaki kimlikleri etkileyen, kuruluşunuzun kimlikleri, ilgili algılanan kuşkulu eylemlerin otomatik yanıtlar yapılandırır olası güvenlik açıklarını algılar ve Şüpheli olaylar araştırır ve bunları gidermek için uygun tedbiri alır.
- [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve Azure anahtar kasası erişim tüm kullanıcılara kısıtlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic Demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.
   
### <a name="web-and-compute-resources"></a>Web ve işlem kaynakları

#### <a name="app-service-environment"></a>App Service Ortamı

[Azure uygulama hizmeti](/azure/app-service/) web uygulamaları dağıtmak için yönetilen bir hizmettir. Contoso Webstore uygulama olarak dağıtılan bir [App Service Web uygulaması](/azure/app-service-web/app-service-web-overview).

[Azure uygulama hizmeti ortamı (ana v2)](/azure/app-service/app-service-environment/intro) App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir uygulama hizmeti özelliğidir. PCI DSS uyumluluk etkinleştirmek için bu temel mimarisi tarafından kullanılan bir Premium hizmet planı değil.

ASEs yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış, her zaman bir sanal ağ içinde dağıtılır. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim müşterilerin sahip ve uygulamaları şirket içi kurumsal kaynaklara sanal ağları üzerinden yüksek hızlı güvenli bağlantı kurabilirsiniz.

ASEs kullanımı için aşağıdaki denetimleri/yapılandırmalarını izin bu mimarisi için:

- Güvenli bir sanal ağ içinde ana bilgisayar ve ağ güvenlik kuralları
- HTTPS iletişimi için otomatik olarak imzalanan ILB sertifikası ile yapılandırılan ana
- [İç Yük Dengeleme modu](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) devre dışı
- [TLS şifreleme](/azure/app-service-web/app-service-app-service-environment-custom-settings) değiştirildi
- Denetim [gelen trafiği N/W bağlantı noktaları](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [WAF - verileri kısıtlamak](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- [SQL veritabanı trafiğinin](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview) izin


#### <a name="jumpbox-bastion-host"></a>Jumpbox (savunma ana bilgisayarı)

Uygulama hizmeti ortamı güvenli ve kilitli olduğundan var. herhangi bir DevOps sürümleri veya Kudu kullanarak web uygulaması izleme yeteneği gibi gerekli olabilecek değişiklikleri izin vermek için bir mekanizma olması gerekir. Bir sanal makine, VM TCP 3389 dışında bir bağlantı noktası üzerinde bağlanma özelliği sağlar NAT yük dengeleyicinin arkasındaki korunmaktadır. 

Bir sanal makine aşağıdaki yapılandırmaları olan bir jumpbox (savunma ana bilgisayarı) olarak oluşturuldu:

-   [Kötü amaçlı yazılımdan koruma uzantısı](/azure/security/azure-security-antimalware)
-   [Log Analytics uzantısı](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısını](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](/azure/security/azure-security-disk-encryption) Azure anahtar kasası kullanma 
-   Bir [otomatik kapatma ilkesi](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanılmadığında sanal makine kaynaklarının kullanımını azaltmak için

### <a name="security-and-malware-protection"></a>Güvenlik ve kötü amaçlı yazılımdan koruma

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz.  

[Azure Danışmanı](/azure/advisor/advisor-overview) olan yardımcı olan kişiselleştirilmiş bulut Danışman Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyin. Kaynak yapılandırması ve kullanım telemetri analiz eder ve maliyet verimliliği, performans, yüksek kullanılabilirlik ve Azure kaynaklarınızın güvenliğini artırmaya yardımcı olabilir çözümleri önerir.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) Azure bulut Hizmetleri ve sanal makineler için belirlemek ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım, bilinen yapılandırılabilir uyarır kaldırmak yardımcı olan gerçek zamanlı koruma kötü amaçlı veya istenmeyen bir özelliktir yazılım kendini yüklemeye veya Azure sistemleri üzerinde çalışan dener.

### <a name="operations-management"></a>Operasyon yönetimi

#### <a name="application-insights"></a>Application Insights

Kullanım [Application Insights](https://azure.microsoft.com/services/application-insights/) uygulama performans yönetimi ve anlık analytics üzerinden eyleme dönüştürülebilir Öngörüler elde etmek için.

#### <a name="log-analytics"></a>Log Analytics

[Günlük analizi](https://azure.microsoft.com/services/log-analytics/) ve şirket içi ortamları toplamak ve bulut kaynakları tarafından oluşturulan verileri çözümlemek yardımcı olan bir hizmettir.

#### <a name="managment-solutions"></a>Yönetimi çözümleri

Bu ek yönetim çözümleri kabul ve yapılandırılmış: 
- [Activity Log Analytics](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
- [Azure Ağ Analizi](/azure/log-analytics/log-analytics-azure-networking-analytics?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Azure SQL Analizi](/azure/log-analytics/log-analytics-azure-sql)
- [Değişiklik İzleme](/azure/log-analytics/log-analytics-change-tracking?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Anahtar Kasası Analizi](/azure/log-analytics/log-analytics-azure-key-vault?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Hizmet Eşlemesi](/azure/operations-management-suite/operations-management-suite-service-map)
- [Güvenlik ve Denetim](https://www.microsoft.com/cloud-platform/security-and-compliance)
- [Kötü Amaçlı Yazılımdan Koruma](/azure/log-analytics/log-analytics-malware?toc=%2fazure%2foperations-management-suite%2ftoc.json)
- [Güncelleştirme yönetimi](/azure/operations-management-suite/oms-solution-update-management)

### <a name="security-center-integration"></a>Güvenlik Merkezi tümleştirme

Varsayılan dağıtım güvenli ve sağlam yapılandırma durumunu belirten Güvenlik Merkezi önerileri için bir temel sağlamaya yöneliktir. Azure Güvenlik Merkezi'nde veri koleksiyonunu etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi - Başlarken](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu çözümü dağıtmak için [şeması kod depo] içinde kullanılabilir bileşenleridir [kodu-repo]. Temel mimari dağıtımını Microsoft PowerShell v5 yürütülen birkaç adımı gerektirir. Web sitesine bağlanmak için bir özel etki alanı adı (örneğin, contoso.com) sağlamanız gerekir. Bu kullanılarak belirtilir `-customHostName` 2. adımda geçiş yapın. Daha fazla bilgi için bkz: [Azure Web uygulamaları için özel etki alanı adı satın](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Özel etki alanı adı başarıyla dağıtmak ve çözümü çalıştırmak için gerekli değildir, ancak tanıtım amacıyla Web sitesine bağlanamadı olacaktır.

Komut dosyalarını belirttiğiniz Azure AD Kiracı etki alanı kullanıcıları ekleyin. Bir test olarak kullanmak için yeni bir Azure AD oluşturma Microsoft önerir Kiracı.

Dağıtım sırasında herhangi bir sorunla karşılaşırsanız, bkz: [sık sorulan sorular ve sorun giderme](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

Microsoft, yüksek oranda PowerShell temiz bir yüklemesini çözümü dağıtmak için kullanılmasını önerir. Alternatif olarak, uygun yükleme betiklerinin yürütülmesi için gereken en son modülleri kullandığınızdan emin olun. Bu örnek jumpbox (savunma ana bilgisayarı) bağlanır ve aşağıdaki komutları yürütür. Bu özel etki alanı komutunu etkinleştirir unutmayın.


1. Gerekli modüllerini yükleyin ve yönetici rolleri doğru olarak ayarlanmış.
 
    ```powershell
     .\0-Setup-AdministrativeAccountAndPermission.ps1 
        -azureADDomainName contosowebstore.com
        -tenantId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -subscriptionId XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX
        -configureGlobalAdmin 
        -installModules
    ```
    Ayrıntılı kullanım yönergeleri için bkz: [betik yönergeler - Kurulum yönetici hesabı ve izni](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/0-Setup-AdministrativeAccountAndPermission.md).
    
2. Çözüm güncelleştirme yönetimi yükleyin. 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
        -resourceGroupName contosowebstore
        -globalAdminUserName adminXX@contosowebstore.com 
        -globalAdminPassword **************
        -azureADDomainName contosowebstore.com 
        -subscriptionID XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX 
        -suffix PCIcontosowebstore
        -customHostName contosowebstore.com
        -sqlTDAlertEmailAddress edna@contosowebstore.com 
        -enableSSL
        -enableADDomainPasswordPolicy 
    ```
    
    Ayrıntılı kullanım yönergeleri için bkz: [betik yönergeler - dağıtmak ve Azure kaynaklarını Yapılandır](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md).
    
3. Günlük analizi günlüğe kaydetme ve izleme. Çözüm dağıtıldığında, bir günlük analizi çalışma alanı açılabilir ve çözüm deposunda sağlanan örnek şablonları nasıl izleme Panosu yapılandırılabilir göstermek için kullanılabilir. Örnek şablonları için bkz [omsDashboards klasörü](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Verileri doğru şekilde dağıtmak için şablonları için günlük analizi'içinde toplanması gereken unutmayın. Bu bir saat veya site etkinliğe bağlı olarak daha fazla sürebilir.
 
    Günlük analizi günlüğünü ayarlama, bu kaynakları da dahil olmak üzere göz önünde bulundurun:
 
    - Microsoft.Network/applicationGateways
    - Microsoft.Network/NetworkSecurityGroups
    - Microsoft.Web/serverFarms
    - Microsoft.Sql/servers/databases
    - Microsoft.Compute/virtualMachines
    - Microsoft.Web/sites
    - Microsoft.KeyVault/Vaults
    - Microsoft.Automation/automationAccounts
 

    
## <a name="threat-model"></a>Tehdit modeli

Bir veri akış diyagramı (DFD) ve örnek tehdit modeli Contoso Webstore için [şeması tehdit modeli](https://aka.ms/pciblueprintthreatmodel).

![](images/pci-threat-model.png)



## <a name="customer-responsibility-matrix"></a>Müşteri sorumluluk Matrisi

Müşterilerin bir kopyasını koruyarak için sorumlu [sorumluluk özeti matris](https://aka.ms/fsiblueprintcrm), müşteri ve Microsoft Azure sorumluluğunda olan sorumluluğundadır FFIEC gereksinimleri özetlenmektedir.



## <a name="disclaimer-and-acknowledgments"></a>Vazgeçme ve ilgili kaynaklar

*Eylül 2017*

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT VE AVYAN SARİH, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER HİÇBİR GARANTİ VERMEZ HALE GETİRİR. Bu belgede sağlanan "olarak-değil." URL ve diğer internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.  
- Bu belge müşterilerle herhangi bir Microsoft veya Avyan ürün veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.  

  > [!NOTE]
  > Bu Yazıdaki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  

- Bu belgede çözümü temel mimari tasarlanmıştır ve olarak kullanılmamalıdır-üretim amaçlıdır. Uyumluluk sağlamasını müşteriler son müşteri çözüm doğrulamak için kendi denetçi danışmanız gerekir.  
- Tüm müşteri adları, işlem kayıtlarını ve ilgili tüm verileri bu sayfada bu temel mimari amacıyla oluşturulur ve yalnızca gösterim amacıyla sağlanan hayali. Gerçek bir ilişki veya bağlantı yöneliktir ve hiçbiri çıkarılmamalıdır.  
- Bu çözüm, Microsoft ve Avyan danışmanlık tarafından ortaklaşa geliştirilmiştir ve altında kullanılabilir [MIT lisansı](https://opensource.org/licenses/MIT).

