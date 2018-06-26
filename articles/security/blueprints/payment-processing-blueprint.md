---
title: Azure güvenlik ve uyumluluk şeması - PCI DSS uyumlu ödeme işlenirken ortamları
description: Azure güvenlik ve uyumluluk şeması - PCI DSS uyumlu ödeme işlenirken ortamları
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 2f1e00a8-0dd6-477f-9453-75424d06a1df
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/09/2018
ms.author: jomolesk
ms.openlocfilehash: 223829df11bb1c9add811b40b55e47ee1fbb1fe4
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36751847"
---
# <a name="azure-security-and-compliance-blueprint---pci-dss-compliant-payment-processing-environments"></a>Azure güvenlik ve uyumluluk şeması - PCI DSS uyumlu ödeme işlenirken ortamları

## <a name="overview"></a>Genel Bakış

Ödeme işlenirken PCI DSS uyumlu ortamlar için hassas ödeme kartı verileri işlemek için uygun bir PCI DSS uyumlu Platform olarak-hizmet (PaaS) ortamı dağıtımı için yönergeler sağlanmaktadır. Ortak bir başvuru mimarisi paylaşan ve Microsoft Azure benimsenmesi kolaylaştırmak için tasarlanmıştır. Bu şeması yükünü ve dağıtım maliyetini azaltmak için bulut tabanlı bir yaklaşım arayan kuruluşlar ihtiyaçlarını karşılamak üzere bir uçtan uca çözüm gösterilmektedir.

Bu şeması sıkı ödeme kartı sektör veri güvenliği standartları (PCI DSS 3.2) gereksinimlerini karşılamak amacıyla toplama, depolama ve ödeme kartı verilerinin alınması için tasarlanmıştır. Bir uçtan uca Azure tabanlı PaaS çözüm dağıtılan bir ortamda güvenli, uyumlu çok katmanlı (kartı numarası, geçerlilik süresi ve doğrulama verileri dahil olmak üzere) kredi kartı verilerin düzgün işleme gösterir. PCI DSS 3.2 gereksinimleri ve bu çözüm hakkında daha fazla bilgi için bkz: [PCI DSS gereksinimleri - yüksek düzey genel bakış](pci-dss-requirements-overview.md).

Bu şeması müşterilerin belirli gereksinimlerini daha iyi anlamak bir temel olarak hizmet için tasarlanmıştır ve olarak kullanılmamalıdır-bir üretim ortamında. Bir uygulama değişiklik yapmadan bu ortamına dağıtma tamamen özel bir çözüm için PCI DSS uyumlu bir çözüm gereksinimlerini karşılamak için yeterli değil. Lütfen şunlara dikkat edin:
- Bu şeması, müşterilerin Microsoft Azure PCI DSS uyumlu bir biçimde kullanmalarını sağlamak için bir temel sağlar.
- PCI DSS uyumluluk sağlamasını bir onaylanmış tam güvenlik Assessor'nı (QSA) bir üretim müşteri çözüm onaylaması gerekir.
- Müşteriler, uygun güvenlik yürütmek için sorumludur ve uyumluluk gereksinimleri farklılık gösterebilir gibi bu temel mimari kullanılarak oluşturulan herhangi bir çözüm incelenmesi her müşterinin uygulama ve Coğrafya ayrıntılarını dayanır.  

Bu çözümün nasıl çalıştığı ilişkin hızlı genel bakış için bu izleme [video](https://aka.ms/pciblueprintvideo) açıklayan ve onun dağıtım gösterilmesi.

## <a name="solution-components"></a>Çözüm bileşenleri

Temel mimari aşağıdaki bileşenlerden oluşur:

- **Mimari diyagramı**. Diyagramda Contoso Webstore çözüm için kullanılan Referans Mimarisi gösterilmektedir.
- **Dağıtım şablonları**. Bu dağıtımda [Azure Resource Manager şablonları](/azure/azure-resource-manager/resource-group-overview#template-deployment) Kurulum sırasında yapılandırma parametrelerini belirterek mimarisinin bileşenlerine Microsoft Azure otomatik olarak dağıtmak için kullanılan.
- **Dağıtım betikleri otomatik**. Bu komut dosyaları, uçtan uca çözüm dağıtımına yardımcı olur. Komut dosyaları oluşur:
    - Bir modül yükleme ve [genel yönetici](/azure/active-directory/active-directory-assign-admin-roles-azure-portal) Kurulum komut dosyası yükleyin ve gerekli PowerShell modülleri ve genel yönetici rolleri doğru şekilde yapılandırıldığını doğrulamak için kullanılır.
    - PowerShell komut dosyası yüklemesi bir .zip dosyası ve önceden derlenmiş demo web uygulamasıyla içeren bir .bacpac dosyasına aracılığıyla sağlanan uçtan uca çözümü dağıtmak için kullanılan [SQL veritabanı örnek](https://github.com/Microsoft/azure-sql-security-sample) içeriği. Bu çözüm için kaynak kodu gözden geçirme için kullanılabilir [GitHub](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms). 

## <a name="architectural-diagram"></a>Mimari diyagramı

![](images/pci-architectural-diagram.png)

## <a name="user-scenario"></a>Kullanıcı senaryosu

Şeması aşağıdaki kullanım örneği giderir.

> Bu senaryo, nasıl Azure tabanlı PaaS çözümünü işleme, ödeme kartı kurgusal webstore taşınmış gösterilmektedir. Çözüm ödeme verileri dahil olmak üzere temel kullanıcı bilgilerin toplanması işler. Çözüm Bu kart sahibi verilerle ödemeler işlemez; Veriler toplandıktan sonra müşterileri başlatılması ve ödeme işlemciye sahip işlemler Tamamlanıyor sorumludur. Daha fazla bilgi için bkz: ["Gözden geçirme ve yönergeler için uygulama"](https://aka.ms/pciblueprintprocessingoverview).

### <a name="use-case"></a>Kullanım örneği
Küçük webstore adlı *Contoso Webstore* ödeme sistemlerine buluta taşımak hazır. Bunlar, Microsoft Azure kredi kartı ödemeler kendi müşterilerden toplamak bir yazıcısı izin veren ve satın alma işlemi barındırmak için seçtiniz.

Yönetici, cloud doğacak çözüm içinde kendi hedeflerinize ulaşmak için hızlı bir şekilde dağıtılabilir bir çözüm arıyor. Kendi Paydaşlar ile toplamak, depolamak ve ödeme kartı sektör veri güvenliği standardı (PCI DSS) sıkı gereksinimleriyle getirirken ödeme kartı verileri almak için Azure'nın nasıl kullanılabileceğini tartışmak için kendisine bu--kavram kanıtı (POC) kullanır.

> Uygun güvenlik yürütmek için sorumlu olacaktır ve uyumluluk gereksinimleri farklılık gösterebilir gibi bu POC tarafından kullanılan mimarisi ile oluşturulan herhangi bir çözüm incelenmesi uygulama ve Coğrafya ayrıntılarını temel. PCI DSS doğrudan bir onaylanmış tam güvenlik üretime hazır çözümünüzü onaylamak için Assessor ile iş gerektirir.

### <a name="elements-of-the-foundational-architecture"></a>Temel mimari öğeleri

Temel mimari aşağıdaki kurgusal öğeleriyle tasarlanmıştır:

Etki alanı site `contosowebstore.com`

Kullanım örneği göstermek ve kullanıcı arabirimi bir anlayış sağlamak için kullanılan kullanıcı rolleri.

#### <a name="role-site-and-subscription-admin"></a>Rol: Site ve abonelik yönetimi

|Öğe      |Örnek|
|----------|------|
|Kullanıcı Adı: |`adminXX@contosowebstore.com`|
| Ad: |`Global Admin Azure PCI Samples`|
|Kullanıcı türü:| `Subscription Administrator and Azure Active Directory Global Administrator`|

- Yönetici hesabı maskelenmeyen kredi kartı bilgileri okunamıyor. Tüm Eylemler günlüğe kaydedilir.
- Yönetici hesabını yönetin veya SQL veritabanına oturum açın.
- Yönetici hesabı, Active Directory ve abonelik yönetebilirsiniz.

#### <a name="role-sql-administrator"></a>Rol: SQL Yöneticisi

|Öğe      |Örnek|
|----------|------|
|Kullanıcı Adı: |`sqlAdmin@contosowebstore.com`|
| Ad: |`SQLADAdministrator PCI Samples`|
| Ad: |`SQL AD Administrator`|
|Soyadı: |`PCI Samples`|
|Kullanıcı türü:| `Administrator`|

- Sqladmin hesabı filtrelenmemiş kredi kartı bilgileri görüntüleyemez. Tüm Eylemler günlüğe kaydedilir.
- SQL veritabanı sqladmin hesabını yönetebilir.

#### <a name="role-clerk"></a>Rol: yazıcısı

|Öğe      |Örnek|
|----------|------|
|Kullanıcı Adı:| `receptionist_EdnaB@contosowebstore.com`|
| Ad: |`Edna Benson`|
| Ad:| `Edna`|
|Soyadı:| `Benson`|
| Kullanıcı türü: |`Member`|

Edna Benson resepsiyonist ve iş yöneticisidir. Aynen, müşteri bilgileri doğru olduğunu ve faturalama tamamlandı sağlamak için sorumludur. Edna Contoso Webstore demo Web sitesi tüm etkileşim için oturum açmış kullanıcının ' dir. Edna aşağıdaki haklara sahiptir: 

- Edna oluşturabilir ve müşteri bilgileri okuyun
- Edna müşteri bilgilerini değiştirebilirsiniz.
- Edna üzerine veya kredi kartı numarası, sona erme ve kart doğrulama bilgileri ile değiştirin.

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

Temel mimari web uygulaması Güvenlik Duvarı (WAF) sahip bir uygulama ağ geçidi ve etkin OWASP ruleset kullanarak güvenlik açıkları riskini azaltır. Ek özellikler şunları içerir:

- [SSL uç bitiş](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Etkinleştirme [SSL boşaltma](/azure/application-gateway/application-gateway-ssl-portal)
- Devre dışı [TLS v1.0 ve v1.1](/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Web uygulaması güvenlik duvarı](/azure/application-gateway/application-gateway-webapplicationfirewall-overview) (WAF mod)
- [Engelleme modu](/azure/application-gateway/application-gateway-web-application-firewall-portal) OWASP 3.0 ruleset ile
- Etkinleştirme [tanılama günlükleri](/azure/application-gateway/application-gateway-diagnostics)
- [Özel sistem durumu araştırmalarının](/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center) ve [Azure Danışmanı](/azure/advisor/advisor-security-recommendations) ek koruma ve bildirimleri sağlar. Azure Güvenlik Merkezi, ayrıca bir saygınlığı sistemi sağlar.

#### <a name="virtual-network"></a>Sanal ağ

Temel mimari 10.0.0.0/16 bir adres alanı ile özel bir sanal ağ tanımlar.

#### <a name="network-security-groups"></a>Ağ güvenlik grupları

Her ağ katmanı ayrılmış bir ağ güvenlik grubu (NSG) vardır:
- Güvenlik Duvarı ve uygulama ağ geçidi WAF için bir çevre ağ güvenlik grubu
- Yönetim jumpbox (savunma ana bilgisayarı) için bir NSG
- Uygulama hizmeti ortamı için bir NSG

Her Nsg'ler sahip belirli bağlantı noktalarını ve protokolleri çözümü için güvenli ve doğru çalışma açılır. Ayrıca, aşağıdaki yapılandırmalar her NSG için etkinleştirilir:
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

[Günlük analizi](https://azure.microsoft.com/services/log-analytics) Contoso Webstore tüm sistemi ve kullanıcı etkinliğini kapsamlı günlük kaydıyla sağlamak, kart sahibi veri günlük kaydı içerir. Değişiklikleri gözden ve doğruluk doğrulandı. 

- **Etkinlik günlükleri:**[etkinlik günlükleri](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) aboneliğinizde kaynaklara gerçekleştirilen işlemler hakkında bilgi sağlar.  
- **Tanılama günlüklerini:**[tanılama günlükleri](/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) olan her kaynak tarafından gösterilen tüm günlükleri.   Bu günlükler Windows olayı sistem günlükleri, Azure Blob Depolama, tablo ve kuyruk günlükleri içerir.
- **Güvenlik Duvarı günlüklerini:** uygulama ağ geçidi tanılama tam ve günlükleri erişim sağlar. Güvenlik Duvarı günlüklerini etkin WAF sahip uygulama ağ geçidi kaynakları için kullanılabilir.
- **Günlük arşivleme:** tüm tanılama günlüklerini merkezi ve şifrelenmiş Azure depolama hesabı için tanımlanan bekletme süresi (2 gün) ile arşivleme yazmak için yapılandırılır. Günlükleri işleme, depolama ve dashboarding için Azure günlük Analizi'ne bağlanmıştır. [Günlük analizi](https://azure.microsoft.com/services/log-analytics) ve şirket içi ortamları toplamak ve bulut kaynakları tarafından oluşturulan verileri çözümlemek yardımcı olan bir hizmettir.

### <a name="encryption-and-secrets-management"></a>Şifreleme ve gizli anahtarları Yönetimi

Contoso Webstore tüm kredi kartı verileri şifreler ve CHD alınmasını engelleyen anahtarlarını yönetmek için Azure anahtar kasası kullanır.

- [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) şifreleme anahtarları ve gizli anahtarları bulut uygulamalar ve hizmetler tarafından kullanılan korunmasına yardımcı. 
- [SQL TDE'nin](/sql/relational-databases/security/encryption/transparent-data-encryption) tüm müşteri kart sahibi verileri, sona erme tarihi ve kart doğrulama şifrelemek için kullanılır.
- Veri diski kullanarak depolanan [Azure Disk şifrelemesi](/azure/security/azure-security-disk-encryption) ve BitLocker'ı.

### <a name="identity-management"></a>Kimlik yönetimi

Aşağıdaki teknolojileri kimlik Azure ortamı yönetim yetenekleri sağlar.
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Çözüm için tüm kullanıcılar Azure Active SQL veritabanına erişen kullanıcılar dahil olmak üzere Directory'de oluşturuldu.
- Uygulama kimlik doğrulaması, Azure AD kullanılarak gerçekleştirilir. Daha fazla bilgi için bkz: [uygulamaları Azure Active Directory ile tümleştirme](/azure/active-directory/develop/active-directory-integrating-applications). Ayrıca, veritabanı sütun şifreleme Azure AD uygulama Azure SQL veritabanı kimlik doğrulaması için de kullanır. Daha fazla bilgi için bkz: [her zaman şifreli: SQL veritabanındaki hassas verileri korumaya](/azure/sql-database/sql-database-always-encrypted-azure-key-vault). 
- [Azure Active Directory kimlik koruması](/azure/active-directory/active-directory-identityprotection) kuruluşunuzdaki kimlikleri etkileyen olası güvenlik açıklarını algılarsa, otomatik yanıtları, kuruluşunuzun kimlikleri, ilgili algılanan kuşkulu eylemler için yapılandırır ve Şüpheli olaylar araştırır ve bunları gidermek için uygun tedbiri alır.
- [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/role-assignments-portal) tam olarak Azure için odaklı erişim yönetimi sağlar. Abonelik erişim Abonelik Yöneticisi sınırlıdır ve Azure anahtar kasası erişim tüm kullanıcılara kısıtlıdır.

Azure SQL veritabanı güvenlik özelliklerini kullanma hakkında daha fazla bilgi edinmek için [Contoso Clinic Demo uygulaması](https://github.com/Microsoft/azure-sql-security-sample) örnek.
   
### <a name="web-and-compute-resources"></a>Web ve işlem kaynakları

#### <a name="app-service-environment"></a>App Service Ortamı

[Azure uygulama hizmeti](/azure/app-service/) web uygulamaları dağıtmak için yönetilen bir hizmettir. Contoso Webstore uygulama olarak dağıtılan bir [App Service Web uygulaması](/azure/app-service-web/app-service-web-overview).

[Azure uygulama hizmeti ortamı (ana v2)](/azure/app-service/app-service-environment/intro) App Service uygulamalarını yüksek ölçekte güvenli bir şekilde çalıştırmak için tam yalıtılmış ve ayrılmış bir ortam sağlayan bir uygulama hizmeti özelliğidir. PCI DSS uyumluluk etkinleştirmek için bu temel mimarisi tarafından kullanılan bir Premium hizmet planı değil.

ASEs yalnızca tek bir müşterinin uygulamalarını çalıştırmak için yalıtılmış, her zaman bir sanal ağ içinde dağıtılır. Her iki uygulama gelen ve giden ağ trafiği üzerinde ayrıntılı denetim müşterilerin sahip ve uygulamaları şirket içi kurumsal kaynaklara sanal ağları üzerinden yüksek hızlı güvenli bağlantı kurabilirsiniz.

ASEs kullanımı için aşağıdaki denetimleri/yapılandırmalarını izin bu mimarisi için:

- Ana bilgisayar güvenlik kuralları bir güvenli sanal ağ ve ağ içinde
- HTTPS iletişimi için otomatik olarak imzalanan ILB sertifikası ile yapılandırılan ana
- [İç Yük Dengeleme modu](/azure/app-service-web/app-service-environment-with-internal-load-balancer) (mod 3)
- Devre dışı [TLS 1.0](/azure/app-service-web/app-service-app-service-environment-custom-settings) -PCI DSS açısından kullanım dışı bir TLS protokolü
- Değişiklik [TLS şifreleme](/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Denetim [gelen trafiği N/W bağlantı noktaları](/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic) 
- [WAF – veri kısıtla](/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- İzin [SQL veritabanı trafiği](/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)


#### <a name="jumpbox-bastion-host"></a>Jumpbox (savunma ana bilgisayarı)

Uygulama hizmeti ortamı güvenli ve kilitli olarak vardır herhangi bir DevOps sürümleri veya Kudu kullanarak web uygulaması izleme yeteneği gibi gerekli olabilecek değişiklikleri izin vermek için bir mekanizma olması gerekir. Sanal makine, VM TCP 3389 dışında bir bağlantı noktası üzerinde bağlanmanıza olanak sağlayan NAT yük dengeleyicinin arkasındaki korunmaktadır. 

Bir sanal makine aşağıdaki yapılandırmaları olan bir jumpbox (savunma ana bilgisayarı) olarak oluşturuldu:

-   [Kötü amaçlı yazılımdan koruma uzantısı](/azure/security/azure-security-antimalware)
-   [OMS uzantısı](/azure/virtual-machines/virtual-machines-windows-extensions-oms)
-   [Azure tanılama uzantısını](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk şifrelemesi](/azure/security/azure-security-disk-encryption) Azure anahtar Kasası'nın (Azure kamu, PCI DSS, HIPAA ve diğer gereksinimler Uyar) kullanarak.
-   Bir [otomatik kapatma ilkesi](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) kullanılmadığında sanal makine kaynaklarının kullanımını azaltmak için.

### <a name="security-and-malware-protection"></a>Güvenlik ve kötü amaçlı yazılımdan koruma

[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz.  

[Azure Danışmanı](/azure/advisor/advisor-overview) olan yardımcı olan kişiselleştirilmiş bulut Danışman Azure dağıtımlarınızı iyileştirmek için en iyi uygulamaları izleyin. Danışman, kaynak yapılandırmanızı ve kullanım telemetrinizi analiz ederek Azure kaynaklarınızın maliyet verimliliğini, performansını, yüksek kullanılabilirliğini ve güvenliğini geliştirmenize yardımcı olabilecek çözümler önerir.

[Microsoft Antimalware](/azure/security/azure-security-antimalware) Azure Cloud Services ve sanal makineler için belirlemek ve virüsler, casus yazılım ve diğer kötü amaçlı yazılım, bilinen yapılandırılabilir uyarır kaldırmak yardımcı olan gerçek zamanlı koruma kötü amaçlı veya istenmeyen bir özelliktir yazılım kendini yüklemeye veya Azure sistemleriniz üzerinde çalışmaya dener.

### <a name="operations-management"></a>Operasyon yönetimi

#### <a name="application-insights"></a>Application Insights

Kullanım [Application Insights](https://azure.microsoft.com/services/application-insights/) uygulama performans yönetimi ve anlık analytics üzerinden eyleme dönüştürülebilir Öngörüler elde etmek için.

#### <a name="log-analytics"></a>Log Analytics

[Günlük analizi](https://azure.microsoft.com/services/log-analytics/) ve şirket içi ortamları toplamak ve bulut kaynakları tarafından oluşturulan verileri çözümlemek yardımcı olan Azure hizmetidir.

#### <a name="management-solutions"></a>Yönetim çözümleri

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

Varsayılan dağıtım Güvenlik Merkezi önerilerini, sağlıklı ve güvenli yapılandırma durumunu gösteren bir temel sağlamaya yöneliktir. Azure Güvenlik Merkezi'nde veri koleksiyonunu etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [Azure Güvenlik Merkezi - Başlarken](/azure/security-center/security-center-get-started).

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Bu çözümü dağıtmak için kullanılabilir bileşenleridir [PCI şeması kod depo](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms). Temel mimari dağıtımını Microsoft PowerShell v5 yürütülen birkaç adımı gerektirir. Web sitesine bağlanmak için bir özel etki alanı adı (örneğin, contoso.com) sağlamanız gerekir. Bu, 2. adımda birincil dağıtım betikteki destekli Kullanıcı istemi üzerinden belirtilir. Daha fazla bilgi için bkz: [Azure Web uygulamaları için özel etki alanı adı satın](/azure/app-service-web/custom-dns-web-site-buydomains-web-app). Özel etki alanı adı başarıyla dağıtmak ve çözümü çalıştırmak için gerekli değildir, ancak tanıtım amacıyla Web sitesine bağlanamadı olacaktır.

Komut dosyalarını belirttiğiniz Azure AD Kiracı etki alanı kullanıcıları ekleyin. Yeni bir oluşturmanızı öneririz bir test olarak kullanmak için Azure AD kiracısı.

Dağıtım sırasında herhangi bir sorunla karşılaşırsanız, bkz: [sık sorulan sorular ve sorun giderme](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/pci-faq.md).

PowerShell temiz bir yüklemesini çözümü dağıtmak için kullanılması önerilir. Alternatif olarak, uygun yükleme betiklerinin yürütülmesi için gereken en son modülleri kullandığınızdan emin olun. Bu örnekte, (savunma ana bilgisayarı) jumpbox oturum açın ve aşağıdaki komutları yürütün. Bu özel etki alanı komutunu etkinleştirir unutmayın.


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
    
2. Çözüm güncelleştirme yönetimini yükleme 
 
    ```powershell
    .\1-DeployAndConfigureAzureResources.ps1 
    ```
    
    Ayrıntılı kullanım yönergeleri için bkz: [betik yönergeler - dağıtmak ve Azure kaynaklarını Yapılandır](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Bu kod, Contoso Web deposu demo destekleme veya PCI uyumluluğunu desteklemek için bir ortamı dağıtma başlangıç adımları Pilot uygulaması için kullanılabilir. 
    
    ```PowerShell
    .\1A-ContosoWebStoreDemoAzureResources.ps1
    ```
    
    Contoso Web deposu demo dağıtımını desteklemek için ayrıntılı kullanım yönergeleri için bkz: [betik yönergeler - Contoso Web deposu Demo Azure kaynakları](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1A-ContosoWebStoreDemoAzureResources.md). Bu kod, Contoso Web deposu demo altyapısını dağıtmak için kullanılabilir. 
    
    Bu komut dosyaları, birbirinden bağımsız olarak kullanılmak üzere tasarlanmıştır. En iyi çözüm anlamak için çözümü desteklemek için gereken gerekli Azure kaynakları tanımlamak için tanıtım dağıtımını tamamlamak için önerilir. 
    
3. Günlüğe kaydetme ve izleme. Çözüm dağıtıldığında, bir günlük analizi çalışma alanı açılabilir ve çözüm deposunda sağlanan örnek şablonları nasıl izleme Panosu yapılandırılabilir göstermek için kullanılabilir. Örnek şablonlar başvurmak için [omsDashboards klasörü](https://github.com/Azure/pci-paas-webapp-ase-sqldb-appgateway-keyvault-oms/blob/master/1-DeployAndConfigureAzureResources.md). Verileri doğru şekilde dağıtmak için şablonları için günlük analizi'içinde toplanması gereken unutmayın. Bu bir saat veya site etkinliğe bağlı olarak daha fazla sürebilir.
 
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

Müşterilerin bir kopyasını koruyarak için sorumlu [sorumluluk özeti matris](https://aka.ms/pciblueprintcrm32), müşteri ve Microsoft Azure sorumluluğunda olan olanlar sorumluluğundadır PCI DSS gereksinimleri özetlenmektedir.

## <a name="pci-compliance-review"></a>PCI uyumluluk gözden geçirme 

Çözüm Coalfire systems, Inc. (PCI-DSS tam güvenlik Assessors) tarafından gözden geçirildi. [PCI uyumluluk gözden geçirin ve uygulanması için yönergeler](https://aka.ms/pciblueprintprocessingoverview) bir denetçi'nın incelenmesi çözüm ve ayrıntılı bir üretime hazır dağıtımına dönüştürme dikkat edilmesi gereken noktalar sağlar.

## <a name="disclaimer-and-acknowledgements"></a>Vazgeçme ve bildirimleri

*Eylül 2017*

- Bu belgede yalnızca bilgilendirme amaçlıdır. MICROSOFT VE AVYAN SARİH, ZIMNİ VEYA NİZAMİ BU BELGEDEKİ BİLGİLER HİÇBİR GARANTİ VERMEZ HALE GETİRİR. Bu belgede sağlanan "olarak-değil." URL ve diğer Internet Web sitesi başvuruları dahil olmak üzere bu belgede belirtilen bilgiler ve görüntüler bildirim yapılmadan değiştirilebilir. Bu belgeyi okuma müşterilerin kullanım riski size aittir.  
- Bu belge müşterilerle herhangi bir Microsoft veya Avyan ürün veya çözümleri üzerinde hiçbir fikri mülkiyet hakkı sağlamaz.  
- Müşteriler kopyalayabilir ve bu belgeyi iç başvuru amacıyla kullanın.  

  > [!NOTE]
  > Bu Yazıdaki bazı öneriler artan veri, ağ veya azure'da işlem kaynağı kullanımına neden olabilir ve bir müşterinin Azure lisans ya da abonelik maliyetlerinizi artırabilir.  

- Bu belgede çözümü temel mimari tasarlanmıştır ve olarak kullanılmamalıdır-üretim amaçlıdır. PCI uyumluluk sağlamasını müşteriler kendi tam güvenlik Assessor ile danışmanız gerekir.  
- Tüm müşteri adları, işlem kayıtlarını ve ilgili tüm verileri bu sayfada bu temel mimari amacıyla oluşturulur ve yalnızca gösterim amacıyla sağlanan hayali. Gerçek bir ilişki veya bağlantı yöneliktir ve hiçbiri çıkarılmamalıdır.  
- Bu çözüm, Microsoft ve Avyan danışmanlık tarafından ortaklaşa geliştirilmiştir ve altında kullanılabilir [MIT lisansı](https://opensource.org/licenses/MIT).
- Bu çözüm Coalfire, Microsoft'un PCI-DSS denetçi gözden geçirildi. [PCI uyumluluk gözden](https://aka.ms/pciblueprintcrm32) bağımsız, üçüncü taraf bir gözden geçirme ve çözüm ele alınması gereken bileşenleri sağlar. 
