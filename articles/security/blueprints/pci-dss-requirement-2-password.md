---
title: "Azure ödeme işleme şeması - parola gereksinimleri"
description: PCI DSS gereksinim 2
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 0df24870-6156-4415-a608-dd385b6ae807
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 4ae9fc7d5b53d33f9feb98c450970e0560afa2af
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/16/2017
---
# <a name="password-requirements-for-pci-dss-compliant-environments"></a>PCI DSS uyumlu ortamlar için parola gereksinimleri 
## <a name="pci-dss-requirement-2"></a>PCI DSS gereksinim 2

**Satıcı tarafından sağlanan varsayılan sistem parolalar ve diğer güvenlik parametreleri için kullanmayın**

> [!NOTE]
> Bu gereksinimleri tarafından tanımlanan [ödeme kartı endüstrisi (PCI) güvenlik standartları Council](https://www.pcisecuritystandards.org/pci_security/) parçası olarak [PCI veri güvenliği standardı (DSS) sürüm 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). PCI DSS her gereksinimi yönelik Rehber ve yordamları test etme hakkında bilgi için lütfen bakın.

Kötü niyetli kişilere (iç ve dış bir varlığa) genellikle sistemleri aşmaya Satıcı varsayılan parolaları ve diğer Satıcı varsayılan ayarları kullanın. Bu parolalar ve ayarları korsan topluluğu tarafından bilinen ve ortak bilgi kolayca belirlenir.

## <a name="pci-dss-requirement-21"></a>PCI DSS gereksinim 2.1
 
**2.1** her zaman satıcı tarafından sağlanan varsayılan değerlerini değiştirme ve kaldırma veya gereksiz varsayılan hesapları devre dışı **önce** bir sistem ağ üzerinde yükleme.
Bu, tüm varsayılan parolaları dahil, ancak bu işletim sistemleri tarafından kullanılan güvenlik hizmetleri, uygulama ve sistem hesapları, satış noktası (POS) Terminal, Basit Ağ Yönetim Protokolü (SNMP) topluluğu sağlayan yazılım bunlarla sınırlı olmamak için geçerlidir dizeleri, vb.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure Active Directory parola ilkesi gereksinimlerini AADUX portalındaki müşteriler tarafından sağlanan yeni parolalar için uygulanır. Müşteri tarafından başlatılan Self Servis parola değişiklikleri önceki parola doğrulanması gerekir. Yönetici parolaları sıfırlama sonraki oturum açma sırasında değiştirilmesi için gereklidir. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore kullanıcıların tüm kullanıcılar için kümesi güçlü parolalar kullanmasını gerektirir. Hiçbir örnek veya Konuk hesap gösteride etkinleştirilir.<br /><br />Kablosuz ve SNMP çözümde uygulanmadı.|



### <a name="pci-dss-requirement-211"></a>PCI DSS gereksinim 2.1.1

**2.1.1** dahil ancak bunlarla sınırlı olmamak üzere varsayılan kablosuz şifreleme anahtarları, parolalar, yükleme sırasında tüm kablosuz satıcı Varsayılanları kart sahibi veri ortamı veya aktaran kart sahibi veri bağlı kablosuz ortamları için değiştirmek ve SNMP Topluluğu dizeleri.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Kablosuz ve SNMP çözümde uygulanmadı.|



## <a name="pci-dss-requirement-22"></a>PCI DSS gereksinimi 2.2

**2.2** tüm sistem bileşenleri için yapılandırma standartları geliştirin. Bu standartlar bilinen tüm güvenlik açıklarını adres ve standartları sağlamlaştırma endüstri kabul sistemiyle tutarlı güvence altına alır.
Endüstri kabul sistem standartları sağlamlaştırma kaynakları içerebilir, ancak bunlarla sınırlı değildir:
- Internet güvenliği (CI) için merkezi
- Uluslararası Standartlar Organizasyonu (ISO)
- SysAdmin denetim ağ güvenlik (SAN) Institute
- Ulusal Standartlar Enstitüsü Technology (NIST)

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure için Microsoft Azure ortamında endüstri standartları sağlamlaştırma kabul edilen tutarlıdır sistemler için güvenlik yapılandırma standartları OSSC Teknik Güvenlik Hizmetleri ekibi geliştirir. Bu yapılandırmalar Sistem Temelleri belgelenen ve ilgili yapılandırma değişikliklerinin etkilenen takımlar (örn., IPAK ekibi) açıkça. Yordamlar, güvenlik yapılandırması standartlarına göre uyumluluk izlemek için uygulanır. Microsoft Azure ortamında sistemler için güvenlik yapılandırma standartları endüstri kabul sağlamlaştırma standartları ile tutarlı ve en az yıllık gözden. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore sağlamlaştırma kart sahibi veri ortamı (CDE) için kapsamdaki tüm hizmetleri sağlar. <br /><br />Contoso Webstore de dağıtır [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz.<br /><br />Contoso Webstore tüm sistem değişiklikleri oturum Operations Management Suite kullanır. [Operations Management Suite (OMS)](/azure/operations-management-suite/) değişiklikleri ayrıntılı günlük kaydını sağlar. Değişiklikleri gözden ve doğruluk doğrulandı. Daha ayrıntılı yönergeler için bkz [PCI Kılavuzu - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-221"></a>PCI DSS gereksinim 2.2.1

**2.2.1** , aynı sunucuda birlikte varolandan farklı güvenlik düzeylerine gerektiren işlevleri önlemek için sunucu başına yalnızca bir birincil işlevini uygulayın. (Örneğin, web sunucuları, veritabanı sunucuları ve DNS ayrı sunucularda uygulanması gerekir.) 

> [!NOTE]
> Sanallaştırma teknolojilerini kullanımda olduğu sanal sistem bileşeni başına yalnızca bir birincil işlevini uygulayın.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore Hizmetleri PaaS Hizmetleri olarak dağıtılır. Tüm hizmetler yalıtılır ve ağ kesimleri kullanarak bölümlenmiş.<br /><br />Contoso Webstore de kullanan bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) anahtar yöntemler zorlamak için. Daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-222"></a>PCI DSS gereksinim 2.2.2

**2.2.2** yalnızca gerekli hizmetleri, protokoller, arka plan programları, sistemin işlevi için gerekli olarak vb. etkinleştirin.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure yazılım ve donanım yapılandırmaları tanımlamak ve gereksiz işlevleri, bağlantı noktaları, protokolleri ve Hizmetleri ortadan kaldırmak için en az üç aylık incelenen. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore Hizmetleri PaaS Hizmetleri olarak dağıtılır. Tüm hizmetler yalıtılır ve ağ kesimleri kullanarak bölümlenmiş.<br /><br />Contoso Webstore de kullanan bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) anahtar yöntemler zorlamak için. Daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-223"></a>PCI DSS gereksinim 2.2.3

**2.2.3** uygulayan herhangi bir gerekli hizmetler, protokolleri veya güvenli olarak değerlendirilir Daemon için ek güvenlik özellikleri. 

> [!NOTE]
> SSL/erken TLS kullanıldığı ek A2 gereksinimleri tamamlanması gerekir.


**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore Hizmetleri PaaS Hizmetleri olarak dağıtılır. Tüm hizmetler yalıtılır ve ağ kesimleri kullanarak bölümlenmiş. Dağıtım sağlamlaştırma CDE kapsamındaki tüm hizmetleri de sağlar. <br /><br />Contoso Webstore de dağıtır [Azure Güvenlik Merkezi](https://azure.microsoft.com/services/security-center/), tüm Azure kaynaklarınızın güvenlik durumunu merkezi bir görünümünü sağlar. Bir bakışta, uygun güvenlik denetimleri yerinde olduğundan ve doğru şekilde yapılandırıldığını ve dikkat gerektiren herhangi bir kaynağa hızla tanımlayabilen doğrulayabilirsiniz.<br /><br />Contoso Webstore de kullanan bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) anahtar yöntemler zorlamak için. Daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-224"></a>PCI DSS gereksinim 2.2.4

**2.2.4** kötüye kullanımı önlemek için sistem güvenlik parametrelerini yapılandırın.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Azure çok faktörlü erişim denetimleri ve belgelenen iş gereksiniminin kullanarak yalnızca yetkili personel Azure platformu güvenlik denetimleri yapılandırmak için sağlar. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore AAD kullanır ve güvenlik parametreleri yönetmek için AD RBAC doğru şekilde dağıtılır. Daha fazla bilgi için bkz: [PCI Kılavuzu - Identity Management](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-225"></a>PCI DSS gereksinim 2.2.5

**2.2.5** kaldırma komut dosyalarını, sürücülerini, özellikleri, alt sistemleri, dosya sistemleri ve gereksiz web sunucuları gibi tüm gereksiz işlevi.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore sınırları nasıl kurulacağını belgeler sağlar. Contoso'nun tehdit modeli ve veri akış diyagramı tüm kullanılan hizmetler ve etkin denetimler gösterilmektedir.|



## <a name="pci-dss-requirement-23"></a>PCI DSS gereksinim 2.3

**2.3** güçlü şifreleme kullanmak, tüm konsol içi yönetimsel erişim şifreleyin. 

> [!NOTE]
> SSL/erken TLS kullanıldığı ek A2 gereksinimleri tamamlanması gerekir.

**Sorumlulukları:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Microsoft Azure, hiper yönetici altyapı erişirken güçlü şifreleme kullanımını zorunlu sağlar. Microsoft Azure, aynı zamanda Microsoft Azure Yönetim Portalı'nı kullanarak müşteriler kendi hizmet/Iaas konsolları güçlü şifreleme ile erişmelerini sağlar. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore nasıl güçlü parolalar bir çözümde uygulanması gerektiğini gösterir; Ayrıca, tüm testler, şifreleme çözüm uygulandıktan doğrulamak için gerçekleştirilebilir.<br /><br />Contoso Webstore de kullanan bir [uygulama hizmeti ortamı (ana)](/azure/app-service-web/app-service-app-service-environment-intro) anahtar yöntemler zorlamak için. Daha fazla bilgi için bkz: [PCI Kılavuzu - uygulama hizmeti ortamı](payment-processing-blueprint.md#app-service-environment).|



## <a name="pci-dss-requirement-24"></a>PCI DSS gereksinim 2.4

**2.4** PCI DSS kapsamında olan sistem bileşenlerinin bir stoğunu koruyun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore demo PaaS çözüm stok sağlanan belgelerde incelenebilir. Daha fazla bilgi için bkz: [PCI Kılavuzu - önceden OMS çözümleri](payment-processing-blueprint.md#oms-solutions).|



## <a name="pci-dss-requirement-25"></a>PCI DSS gereksinim 2,5

**2.5** güvenlik ilkeleri ve satıcı Varsayılanları ve diğer güvenlik parametreleri yönetmek için işlem yordamlarını, kullanımda, belgelenmiş ve tüm etkilenen taraflara bilinen emin olun.

**Sorumlulukları:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Contoso Webstore güvenlik parametreleri bir anlayış sağlar belgeler sağlar ve belgeleri öğeleri hizmet. |



## <a name="pci-dss-requirement-26"></a>PCI DSS gereksinim 2.6

**2.6** paylaşılan barındırma sağlayıcılarının her varlığın barındırılan ortam ve kart sahibi verilerini korur. Bu sağlayıcılar ayrıntılı biçimde açıklandığı gibi belirli gereksinimleri karşılaması gerekir *ek A: PCI DSS için ek gereksinimler paylaşılan barındırma sağlayıcıları.*

**Sorumlulukları:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Sağlayıcı<br />(Microsoft&nbsp;Azure)** | Geçerli değil. Microsoft Azure paylaşılan bir barındırma sağlayıcısı değil. |
| **Müşteri<br />(PCI &#8209; DSS&nbsp;Şeması)** | Geçerli değil. Microsoft Azure paylaşılan bir barındırma sağlayıcısı değil.|




