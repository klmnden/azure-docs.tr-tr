---
title: Azure güvenlik yönetimi ve izlemeye genel bakış | Microsoft Docs
description: Bu makale yönetimi yardımcı olmak için Azure sağlar güvenlik özellikleri ve Hizmetleri genel bakış ve Azure bulut Hizmetleri ve sanal makineleri izleme sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: StevenPo
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: terrylan
ms.openlocfilehash: 9e538ac39af5b6df44860a4a70b0fd1e058c060c
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752297"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure güvenlik yönetimi ve izlemeye genel bakış
Azure yönetimi ve izlenmesi Azure bulut Hizmetleri ve sanal makineleri (VM'ler) yardımcı olmak amacıyla güvenlik mekanizmaları sağlar. Bu makalede bu temel güvenlik özellikleri ve Hizmetleri genel bakış sağlar. Daha fazla bilgi için her ayrıntılarını veren makaleler için bağlantılar sağlanmaktadır.

Microsoft bulut Hizmetleri Güvenlik ortaklığı ve Microsoft arasında paylaşılan bir sorumluluğu ' dir. Microsoft (kilitli rozet giriş kapıları, dilimleri ve koruyucuları gibi güvenlik korumaları kullanarak) Azure platformu ve kendi veri merkezleri fiziksel güvenlik sorumludur. Azure bulut güvenlik, müşterilerinin, güvenlik, gizlilik ve uyumluluk gereksinimlerini karşılayan yazılım katmanında güçlü düzeyleri sağlar.

Sahip olduğunuz verilerinizi ve kimlikleri, bunları, şirket içi kaynaklarınızın güvenlik ve bulut bileşenleri üzerinde denetim sahibi güvenliğini koruma sorumluluğunu. Microsoft, güvenlik denetimleri ve verileriniz ve uygulamalarınız korumanıza yardımcı olmak için özellikleri sunar. Güvenlik sorumluluğunu, derecesini bulut hizmetinin türüne dayalıdır.

Aşağıdaki grafikte Microsoft ve müşterinin arasındaki sorumluluk bakiyesini özetler.

![Paylaşılan sorumluluk][1]

Güvenlik yönetimi hakkında daha fazla bilgi için bkz: [azure'da güvenlik yönetimi](azure-security-management.md).

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control
Rol tabanlı erişim denetimi (RBAC) Azure kaynakları için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kişiler yalnızca işlerini yapmak için gereksinim duydukları erişim miktarını verebilirsiniz. RBAC de kişiler kuruluş bıraktığınızda, bunlar kaynaklara erişimi bulutta kaybetmesine emin olmanıza yardımcı olabilir.

Daha fazla bilgi edinin:

* [RBAC üzerinde Active Directory ekip blogu](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure ile Microsoft, Symantec, eğilim mikro, McAfee ve Kaspersky gibi önemli güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımını kullanabilirsiniz. Bu yazılım, kötü amaçlı dosyalar, reklam ve diğer tehditlerden sanal makinelerinizi korunmasına yardımcı olur.

Azure Cloud Services ve sanal makineleri için Microsoft Antimalware PaaS rol ve sanal makineler için bir kötü amaçlı yazılımdan koruma Aracısı yükleme olanağı sunar. System Center Endpoint Protection'ı bağlı olarak, bu özellik kanıtlanmış şirket içi güvenlik teknolojisi buluta getirir.

Ayrıca eğilimi'nın için derin tümleştirme sunuyoruz [derin güvenlik](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) ve [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) Azure platformu ürünlerinde. SecureCloud bir şifreleme çözümüdür ve derin güvenlik virüsten koruma çözümüdür. Derin güvenlik VM'ler içinde bir uzantı modeli aracılığıyla dağıtılır. PowerShell ve Azure portalı kullanıcı arabirimini kullanarak çalışmaya başlar yeni VM'ler veya zaten dağıtılmış varolan VM'ler içinde derin güvenlik kullanmayı da tercih edebilirsiniz.

Symantec Endpoint Protection (Eylül) de Azure üzerinde desteklenir. Portal tümleştirme sayesinde, bir VM üzerinde Eylül kullanmayı düşündüğünüz belirtebilirsiniz. Eylül Azure Portalı aracılığıyla yeni bir VM yüklenebilir veya var olan bir VM'de PowerShell aracılığıyla yüklenebilir.

Daha fazla bilgi edinin:

* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Azure bulut Hizmetleri ve sanal makineleri için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Yükleme ve bir Windows VM bir hizmet olarak eğilimi mikro derin güvenliği yapılandırma](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve Symantec Endpoint Protection bir Windows VM yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure sanal makineleri koruma için yeni kötü amaçlı yazılımdan koruma seçenekleri](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure multi-Factor Authentication, birden fazla doğrulama yöntemi kullanılmasını gerektiren kimlik doğrulama yöntemidir. Kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekler. 

Çok faktörlü kimlik doğrulaması yardımcı erişimi korumaya veri ve uygulamalara basit bir oturum açma işlemi için kullanıcı talebine toplantı oluştu. Doğrulama seçeneklerini (telefon araması, SMS mesajı veya mobil uygulama bildirimi veya doğrulama kodu) ve üçüncü taraf OATH belirteçleri aralığı aracılığıyla güçlü kimlik doğrulaması sunar.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../active-directory/authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute
Azure ExpressRoute bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft Cloud genişletmek için kullanabilirsiniz. ExpressRoute ile Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilir. Bağlantı kaynaktan olabilir:

- Any herhangi bir (IP VP) ağı.
- Bir noktadan noktaya Ethernet ağı.
- Bir sanal çapraz bağlantısından bir ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden. 

ExpressRoute bağlantıları ortak internet üzerinden geçmez. Bunlar daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantıları daha yüksek güvenlik Internet üzerinden sunabilir.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Sanal ağ geçitleri
VPN ağ geçitleri, Azure sanal ağ geçitleri olarak da adlandırılan sanal ağlar ve şirket içi konumlara arasında ağ trafiğini göndermek için kullanılır. Bunlar Azure (ağ için ağ) içinde birden çok sanal ağlar arasında trafiği göndermek için kullanılır. VPN ağ geçitleri altyapınız ile Azure arasında güvenli şirket içi bağlantısı sağlar.

Daha fazla bilgi edinin:

* [VPN ağ geçitleri hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Azure ağ güvenliğine genel bakış](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Bazen kullanıcıların Azure kaynakları veya diğer SaaS uygulamalarında ayrıcalıklı işlemleri gerçekleştirmek gerekir. Bu, genellikle kuruluşların sürekli ayrıcalıklı erişim Azure Active Directory (Azure AD) vermediğiniz anlamına gelir. 

Kuruluşlar, yeteri kadar ayrıcalıklı erişim ile bu kullanıcıların ne yaptıklarını izleyemez bulutta barındırılan kaynaklar için büyüyen bir güvenlik riski olmasıdır. Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı aşılıp aşılmadığını o bir ihlali kuruluşun genel bulut güvenliği etkileyebilir. Azure AD Privileged Identity Management ayrıcalıkları Etkilenme süresini azaltarak ve kullanım görünürlük artırma tarafından bu riski gidermeye yardımcı olur.  

Privileged Identity Management geçici bir yönetici rolü veya yönetici erişimi "tam zamanında" kavramını sunmaktadır. Bu tür bir yönetici, için rolü atanmış bir etkinleştirme işlemini tamamlamak için gereken bir kullanıcıdır. Etkinleştirme işlemini etkin değil olarak belirli bir süre için etkin dönem başlangıcı Azure AD'de rol atama kullanıcının değiştirir.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Azure AD Privileged Identity Management ile çalışmaya başlama](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Kimlik Koruması
Azure AD kimlik koruması oturum açma kuşkulu etkinlikleri ve işinizin korunmasına yardımcı olmak için olası güvenlik açıklarını birleştirilmiş bir görünümünü sağlar. Kimlik koruması, kullanıcılar ve ayrıcalıklı (Yönetici) kimlikleri gibi sinyalleri göre kuşkulu etkinlikleri algılar:

- Kaba kuvvet saldırıları.
- Sızan kimlik bilgileri.
- Tanınmayan konumları ve virüs bulaşmış cihazlardan gerçekleştirilen oturum açma işlemleri.

Bildirimler ve önerilen düzeltme sağlayarak kimlik koruması gerçek zamanlı risklerin azaltılmasına yardımcı olur. Kullanıcı risk önem hesaplar. Otomatik olarak koruma uygulama gelecekteki tehditlerinden erişim yardımcı olmak için risk tabanlı ilkeleri yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD ve kimlik göster: kimlik koruması Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Güvenlik Merkezi
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur. Güvenlik Merkezi verir görünürlük artan ve Azure kaynaklarınızın güvenlik üzerinden, denetim. Aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi, Azure sağlar. Kaçabilecek ve güvenlik çözümlerinin geniş ekosistemiyle ile çalışır tehditleri algılayabilir yardımcı olur.

Güvenlik Merkezi en iyi duruma getirme ve Azure kaynaklarınızın güvenliğini izlemenize yardımcı olur:

* Azure aboneliği kaynaklarınızı göre ilkeleri tanımlamak etkinleştirme:
  * Şirketinizin güvenlik gerekir.
  * Uygulamaları ya da her Abonelikteki verilerin duyarlılığına türü.
* Azure sanal makineleri, ağ ve uygulamaların durumunu izleme.
* Tümleşik iş ortağı çözümlerinden gelen uyarılar dahil olmak üzere öncelikli güvenlik uyarıları listesi sağlama. Ayrıca, bir saldırı ve sorunun nasıl çözüleceğiyle hakkında öneriler hızlı araştırmanız gereken bilgileri sağlar.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
