---
title: Azure ödeme işleme şeması - güvenlik duvarı gereksinimleri
description: PCI DSS gereksinim 1
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: b1935d88-acae-42f9-bc25-bb0766f876ab
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 4e04d6417f1468c1bafada1a93ab63a73e39653d
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="firewall-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için güvenlik duvarı gereksinimleri 
## <a name="pci-dss-requirement-1"></a>PCI DSS gereksinim 1

**Yüklemek ve kart sahibi verileri korumak için bir güvenlik duvarı yapılandırmasını yönetmek**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Güvenlik duvarları (iç) bir varlığın ağlar arasında izin verilen bilgisayar trafiği denetleyen aygıtlardır ve trafiği içine ve dışına iç daha hassas alanlara bir varlığın içinde yanı sıra, güvenilmeyen ağlara (harici) ağları güvenilir. Kart sahibi veri ortamı, bir varlığın güvenilir ağ içinde daha hassas alana örneğidir.
Bir güvenlik duvarı tüm ağ trafiğini inceler ve belirtilen güvenlik ölçütleri karşılamayan bu iletimleri engeller.
Tüm sistemler güvenilmeyen ağlardan yetkisiz erişimden e-ticaret, masaüstü tarayıcıları, çalışan e-posta erişimi, gibi adanmış bağlantılar üzerinden çalışan Internet erişimi olarak Internet üzerinden sistem girme olup olmadığını korunması gerekir işletmeler için bağlantıları, kablosuz ağlar üzerinden veya diğer kaynakları aracılığıyla. Genellikle, görünen Önemsiz yollar güvenilmeyen ağlara gelen ve giden korumasız yollarıyla anahtar sistemlere sağlayabilirsiniz. Güvenlik duvarları, herhangi bir bilgisayar ağ için bir anahtar koruması sistemdir.
Güvenlik duvarları için minimum gereksinimleri gereksinim 1'de tanımlanan karşıladıklarından sürece diğer sistem bileşenlerini güvenlik duvarı işlevselliği sağlayabilir. Diğer sistem bileşenlerini, güvenlik duvarı işlevselliği sağlamak için kart sahibi veri ortamında kullanıldığı, bu cihazların gereksinim 1 değerlendirmesi ve kapsam içinde dahil edilmelidir.

## <a name="pci-dss-requirement-11"></a>PCI DSS gereksinim 1.1

**1.1** (bkz. 1.1.1 1.1.7 aracılığıyla) aşağıdakiler dahil kurma ve uygulama güvenlik duvarı ve yönlendirici yapılandırması standartları.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore CDE saldırısından PaaS yalıtım kullanarak sağlar ve bir uygulama hizmeti ortamı uygulama CDE giriş ve çıkış veri korunur sağlar.<br /><br />Bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) olan uyumluluk nedenleriyle kullanılan bir Premium hizmet planı. Ana denetimleri ve yapılandırma hakkında daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-111"></a>PCI DSS gereksinim 1.1.1

**1.1.1** onaylama ve tüm ağ bağlantıları ve değişiklikleri test etmek için resmi bir işlem için güvenlik duvarı ve yönlendirici yapılandırmaları


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore örneği tüm değişiklikleri doğru yönetilen sağlamaya yönelik bir CI/CD DevOps modeli oluşturur. Günlük analizi değişiklikleri ayrıntılı günlük kaydını sağlar. Değişiklikleri gözden ve doğruluk doğrulandı. Daha ayrıntılı yönergeler için bkz [PCI Kılavuzu - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).<br /><br />[Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/) tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz.|



### <a name="pci-dss-requirement-112"></a>PCI DSS gereksinim 1.1.2

**1.1.2** kart sahibi veri ortamı ve hiçbir kablosuz ağ dahil olmak üzere diğer ağlar arasındaki tüm bağlantıların tanımlayan geçerli Ağ Diyagramı

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Yükleme düzeni çözümün bir parçası olarak sağlanan Contoso Webstore başvuru mimarisi ve tasarım belgelerine bakın.|



### <a name="pci-dss-requirement-113"></a>PCI DSS gereksinim 1.1.3

**1.1.3** tüm kart sahibi verileri gösterir geçerli diyagram sistemleri ve ağlar akar

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore DFD ve tehdit modeli bakın.|



### <a name="pci-dss-requirement-114"></a>PCI DSS gereksinim 1.1.4

**1.1.4** her bir Internet bağlantısı ve tüm sivil bölge (DMZ) ve iç ağ bölgesi arasında bir güvenlik duvarı gereksinimleri

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure sınır koruma cihazları ağ geçitleri, ağ ACL'leri ve platform düzeyinde iç ve dış sınırlarında denetim iletişimler için uygulama güvenlik duvarları gibi kullanır. Müşteri, sonra bunları kendi belirtimler ve gereklilikler için yapılandırır. Microsoft Azure platformu ile çıkarken iletişimi filtreler. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore PaaS yalıtım kullanarak DMZ sağlar ve bir uygulama hizmeti ortamı uygulama CDE giriş ve çıkış veri korunur sağlar.<br /><br />Bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) olan uyumluluk nedenleriyle kullanılan bir Premium hizmet planı. Ana denetimleri ve yapılandırma hakkında daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-115"></a>PCI DSS gereksinim 1.1.5

**1.1.5** gruplar, roller ve sorumluluklar ağ bileşenlerinin yönetimi için açıklama

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore kullanan [Azure rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/role-assignments-portal) kullanıcı rollerine ayırmak için. RBAC tam olarak Azure için odaklı erişim yönetimi sağlar. Belirli yapılandırmalar abonelik erişim ve Azure anahtar kasası erişim için mevcut.|



### <a name="pci-dss-requirement-116"></a>PCI DSS gereksinim 1.1.6

**1.1.6** iş gerekçesinin ve tüm hizmetleri, protokolleri ve izin verilen, bağlantı noktası protokollerin için güvenli olarak kabul uygulanan güvenlik özelliklerinin belgeleri de dahil olmak üzere kullanmak için onay belgeleri.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore, yalnızca gerekli bağlantı noktalarını ve protokolleri RA tasarım boyunca açar. Veri akışı ayrıntılarını DFD ve tehdit modeli görülebilir.|



### <a name="pci-dss-requirement-117"></a>PCI DSS gereksinim 1.1.7

**1.1.7** güvenlik duvarı veya yönlendirici kural gözden geçirmek için gereksinim en az altı ayda ayarlar

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore'un hiçbir gereksiz veya kullanılmayan kuralları dahil olduğundan emin olmak için güvenlik duvarı kural kümeleri incelenen. Tasarım gereği, en küçük yolu kaplama alanı bir en az ayrıcalık demo dağıtılır.|



## <a name="pci-dss-requirement-12"></a>PCI DSS gereksinim 1.2

**1.2** yapı güvenilmeyen ağlara ve kart sahibi veri ortamında tüm sistem bileşenleri arasındaki bağlantıları kısıtlamak güvenlik duvarı ve yönlendirici yapılandırmaları. 

> [!NOTE]
> "Güvenilmeyen bir ağ" dış gözden geçirme altında varlığa ait ağlara ve/veya denetim veya yönetmek için varlığın özelliği dışında olduğu herhangi bir ağdır.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore'nın CDE RA ve dağıtım belgelerinde tanımlanır. Güvenilmeyen ağlarda tasarım gereği reddedilir.|



### <a name="pci-dss-requirement-121"></a>PCI DSS gereksinim 1.2.1

**1.2.1** kart sahibi veri ortamı için gerekli olan, gelen ve giden trafiği kısıtlamak ve diğer tüm trafiğe özellikle reddedin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore'nın CDE RA ve dağıtım belgelerinde tanımlanır. Güvenilmeyen ağlarda tasarım gereği reddedilir. Contoso Webstore demo yalnızca belirtilen Microsoft Azure hizmetlerine erişmek için IP adresi aralıklarına izin vermek için Microsoft Azure uygulaması güvenlik duvarı yapılandırır. Contoso Webstore reddetme tüm güvenlik duvarı hiç CDE sınırları sağlar. Tüm yapılandırmaları dağıtımı ilk kurulum sırasında gerçekleştirilir.

> [!NOTE]
> Uygulama hizmeti ortamı (ana) kullanılan ana ana tarafından yapılması giden bağlantılara izin veren bir DMZ yalıtım uygulayan CDE ancak yalıtmak için bu çözümde, tam güvenlik Assessor'ne (QSA) Bu çözüm, hesaplar gerekli olur. PCI-DSS gerekli olmayan tüm gelen ve giden bağlantılara engellenmelidir gerektirir. Ana düzgün çalışması için ana giden bağlantılar olarak kuracak için tanımlandığı gibi gerekli ["Uygulama hizmeti ortamı için ağ konuları"](/azure/app-service/app-service-environment/network-info). Müşteriler gereksinimlerini karşılayacak emin olmak için çözüm bir üretim ortamına dağıtmadan önce QSA ile giden bağlantılar belirlemelidir. |



### <a name="pci-dss-requirement-122"></a>PCI DSS gereksinim 1.2.2

**1.2.2** güvenli ve yönlendirici yapılandırma dosyalarını eşitleyin.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore yapılandırmaları için Microsoft Azure yerel ağ denetimleri eşitlenmesini sağlar.|



### <a name="pci-dss-requirement-123"></a>PCI DSS gereksinim 1.2.3

**1.2.3** yükleme tüm kablosuz ağlar ve kart sahibi veri ortamı arasındaki çevre güvenlik duvarları ve reddetme veya trafiği ticari amaçlarla gerekliyse, kablosuz arasındaki trafiği yetkili yalnızca izin vermek için bu güvenlik duvarlarını yapılandırma ortam ve kart sahibi veri ortamı.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore herhangi bir kablosuz çözümleri veya etkin özelliklerine sahip değil.|



## <a name="pci-dss-requirement-13"></a>PCI DSS gereksinim 1.3

**1.3** Internet ve tüm sistem bileşenlerinin kart sahibi veri ortamında arasında doğrudan genel erişim engelle.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure güvenlik duvarları, yük Dengeleyiciler ve ACL'ler gibi ağ ve ana bilgisayar tabanlı sınır koruma cihazlar kullanır. Bu aygıtların VLAN yalıtımı, NAT ve paket ayrı müşteri Internet'ten ve yönetim trafiği için filtreleme gibi kullanın. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı, aynı anda yalnızca belirtilen Azure VM'ler savunma kendi CDE içeriyor, siteye erişmek için IP adreslerini aralıklarına izin vermek için Azure uygulaması güvenlik duvarı yapılandırmalarını sağlar.|



### <a name="pci-dss-requirement-131"></a>PCI DSS gereksinim 1.3.1

**1.3.1** yetkili genel olarak erişilebilir Hizmetleri, protokoller ve bağlantı noktaları sağlayan sistem bileşenleri için gelen trafiğini sınırlandırmak için DMZ uygulayın.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Yalnızca yetkili hizmetler ile CDE bağlanabilir kendi DMZ Contoso Webstore uygulanmasını sağlar.|



### <a name="pci-dss-requirement-132"></a>PCI DSS gereksinim 1.3.2

**1.3.2** gelen Internet trafiği DMZ içindeki IP adresleri de sınırı.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Yalnızca yetkili hizmetler ile CDE bağlanabilir kendi DMZ Contoso Webstore uygulanmasını sağlar.|



### <a name="pci-dss-requirement-133"></a>PCI DSS gereksinim 1.3.3

**1.3.3** algılamak ve engellemek için uygulama, ölçüleri yanıltma sahte ağ girmesini kaynak IP adresleri. (Örneğin, bir iç kaynak adresi ile Internet'ten gelen trafiği engeller.)

**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ağ sahte trafiğini önlemek ve güvenilir platform bileşenleri için gelen ve giden trafiği kısıtlamak için filtreyi uygular. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-134"></a>PCI DSS gereksinim 1.3.4

**1.3.4** Internet kart sahibi veri ortamından yetkisiz giden trafiğe izin verme.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore mimarisi kapsam içinde ortamından yetkisiz giden trafik Internet'e engeller. Bu, Microsoft Azure'da giden trafik ACL'ler onaylanan bağlantı noktalarını ve protokolleri için yapılandırarak gerçekleştirilir. Bu denetimler SQL Server veritabanında CDE erişimi bulunur. <br /><br />PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-135"></a>PCI DSS gereksinim 1.3.5

**1.3.5** izin yalnızca "kurulur" ağ bağlantıları.


**Sorumlulukları:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure ağ sahte trafiğini önlemek ve güvenilir platform bileşenleri için gelen ve giden trafiği kısıtlamak için filtreyi uygular. Microsoft Azure ağı müşteri trafiğinden gelen yönetim trafiğini ayırmak için ayrılır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Geçerli değil.|



### <a name="pci-dss-requirement-136"></a>PCI DSS gereksinim 1.3.6

**1.3.6** kart sahibi verileri (örneğin, bir veritabanı) bir iç ağ bölgesinde depolamak yerine sistem bileşenleri yinelenmeli DMZ ve diğer güvenilmeyen ağlara.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure müşteri trafiği yönetim trafiği ayrı ağ arasında ayrım yapma ve NAT'ı kullanır. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore mimarisi kapsam içinde ortamından yetkisiz giden trafik Internet'e engeller. Bu, Microsoft Azure'da giden trafik ACL'ler onaylanan bağlantı noktalarını ve protokolleri için yapılandırarak gerçekleştirilir. Bu denetimler SQL Server veritabanında CDE erişimi bulunur. <br /><br />PaaS SQL veritabanı örneğinde veritabanı güvenlik önlemleri göstermek için kullanılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Azure SQL veritabanı](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-137"></a>PCI DSS gereksinim 1.3.7

**1.3.7** özel IP adresleri ve yetkisiz taraflara yönlendirme bilgilerini ifşa değil. 

> [!NOTE]
> IP adresleme soyutlamaması yöntemleri içerebilir, ancak bunlarla sınırlı değildir:
> - Ağ adresi çevirisi (NAT).
> - Proxy sunucuları/güvenlik duvarı arkasında kart sahibi verileri içeren sunucuları yapılıyor.
> - Kaldırma veya yol tanıtımlarını kullanan özel ağlar için filtre adresleme kayıtlı.
> - RFC1918 adres alanı kayıtlı adresler yerine iç kullanımı.


**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure müşteri trafiği yönetim trafiği ayrı ağ adresi çevirisi (NAT) ve ağ arasında ayrım yapma kullanır. Azure cihazlar tarafından kendi UUID benzersiz şekilde tanımlanır ve Kerberos kullanarak kimlik doğrulaması. Azure ağ aygıtları ele RFC 1918 IP tarafından tanımlanan yönetilen. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore proxy sunucuları/güvenlik duvarı arkasında tüm kart sahibi veri yerleştirir ve RFC1918 adres alanı dahili olarak kullanır.|



## <a name="pci-dss-requirement-14"></a>PCI DSS gereksinim 1.4

**1.4** kişisel güvenlik duvarı yazılımı veya eşdeğer bir işlevi (şirket dahil olmak üzere ve/veya çalışana ait) tüm taşınabilir bilgisayar cihazlarda yüklemek olduğunda (çalışanlar tarafından kullanılan Örneğin, dizüstü), Ağ dışından Internet'e bağlanmak ve hangi CDE erişmek için kullanılır. Güvenlik Duvarı (veya eşdeğeri) yapılandırmaları şunları içerir:-özel yapılandırma ayarları tanımlanır.
- Kişisel Güvenlik Duvarı (veya eşdeğer bir işlevselliği) etkin bir şekilde çalışıyor.
- Kişisel Güvenlik Duvarı (veya eşdeğer bir işlevselliği) taşınabilir bilgisayar cihazlar kullanıcılar tarafından alterable değil.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore cihazları son kullanıcı koruma sağlamaz. [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) tanıdıkları kullanan şirket verilerine erişmek için mobil cihazları yönetmek için kullanılabilir.|



## <a name="pci-dss-requirement-15"></a>PCI DSS gereksinim 1.5

**1.5** güvenlik ilkeleri ve güvenlik duvarları yönetmek için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI&#8209;DSS&nbsp;Şeması)** | Contoso Webstore dağıtımı, aynı anda yalnızca belirtilen Azure VM'ler savunma kendi CDE içeriyor, siteye erişmek için IP adreslerini aralıklarına izin vermek için Azure uygulaması güvenlik duvarı yapılandırmalarını sağlar.|




