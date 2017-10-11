---
title: "Azure güvenlik yönetimi ve izlemeye genel bakış | Microsoft Docs"
description: " Azure yönetimi ve izlenmesi Azure bulut Hizmetleri ve sanal makineleri yardımcı olmak amacıyla güvenlik mekanizmaları sağlar.  Bu makalede bu temel güvenlik özellikleri ve Hizmetleri genel bakış sağlar. "
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
ms.openlocfilehash: 6787877deabafd0b7308e190cb45b4036049b05b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2017
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure güvenlik yönetimi ve izlemeye genel bakış
Azure yönetimi ve izlenmesi Azure bulut Hizmetleri ve sanal makineleri yardımcı olmak amacıyla güvenlik mekanizmaları sağlar. Bu makalede bu temel güvenlik özellikleri ve Hizmetleri genel bakış sağlar. Daha fazla bilgi için her ayrıntılarını veren makaleler için bağlantılar sağlanmaktadır.

Microsoft bulut Hizmetleri Güvenlik ortaklığı ve Microsoft arasında paylaşılan sorumluluk ' dir. Paylaşılan sorumluluk (kilitli rozet giriş kapıları, dilimleri ve koruyucuları gibi güvenlik korumaları kullanarak) Microsoft veri merkezleri, fiziksel güvenlik ve Microsoft Azure için sorumlu olduğu anlamına gelir. Ayrıca, Azure bulut güvenlik zorlu müşterilerinin, güvenlik, gizlilik ve uyumluluk gereksinimlerini karşılayan yazılım katmanında güçlü düzeyleri sağlar.

Sahip olduğunuz verilerinizi ve kimlikleri, bunları, şirket içi kaynaklarınızın güvenlik ve bulut bileşenleri üzerinde denetim sahibi güvenliğini koruma sorumluluğunu. Microsoft Güvenlik denetimleri ve verileriniz ve uygulamalarınız korumanıza yardımcı olmak için özellikleri sağlar. Güvenlik sorumluluğunu, derecesini bulut hizmetinin türüne dayalıdır.

Aşağıdaki grafikte hem Microsoft hem de müşteri sorumluluğu bakiyesini özetler.

![Paylaşılan sorumluluk][1]

Güvenlik yönetimi içine daha ayrıntılı bilgi edinmek için bkz: [azure'da güvenlik yönetimi](azure-security-management.md).

Bu makalede ele alınacak temel özellikleri aşağıda verilmiştir:

* Rol Tabanlı Access Control
* Kötü Amaçlı Yazılımdan Koruma
* Multi-Factor Authentication
* ExpressRoute
* Sanal ağ geçitleri
* Ayrıcalıklı Kimlik Yönetimi
* Kimlik koruması
* Güvenlik Merkezi

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control
Rol tabanlı erişim denetimi (RBAC) Azure kaynakları için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kişiler yalnızca işlerini yapmak için gereksinim duydukları erişim miktarını verebilirsiniz.  RBAC de kişiler kuruluş ayrıldığında kullanıcılar kaynaklara erişimi bulutta kaybetmesine emin olmanıza yardımcı olabilir.

Daha fazla bilgi edinin:

* [RBAC üzerinde Active Directory ekip blogu](http://i1.blogs.technet.com/b/ad/archive/2015/10/12/azure-rbac-is-ga.aspx)
* [Azure rol tabanlı erişim denetimi](../active-directory/role-based-access-control-configure.md)

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma
Azure ile Microsoft, Symantec, eğilim mikro, McAfee ve Kaspersky gibi önemli güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımı, sanal makinelerinizi kötü amaçlı dosyalar, reklam ve diğer tehditlere karşı korumaya yardımcı için kullanabilirsiniz.

Microsoft Antimalware PaaS rol ve sanal makineler için bir kötü amaçlı yazılımdan koruma Aracısı yükleme olanağı sunar. System Center Endpoint Protection'ı bağlı olarak, bu özellik kanıtlanmış şirket içi güvenlik teknolojisi buluta getirir.

Ayrıca eğilimi'nın için derin tümleştirme sunuyoruz [derin güvenlik](http://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/)™ ve [SecureCloud](http://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/)™ Azure platformu ürünlerinde. DeepSecurity virüsten koruma çözümüdür ve SecureCloud bir şifreleme çözümüdür. DeepSecurity bir uzantı modeli kullanarak VM'ler içinde dağıtılır. Portal UI ve PowerShell kullanarak çalışmaya başlar yeni VM'ler veya zaten dağıtılmış varolan VM'ler içinde DeepSecurity kullanmayı da tercih edebilirsiniz.

Symantec uç nokta koruma (Eylül) de Azure üzerinde desteklenir. Portal tümleştirme sayesinde, müşterilere Eylül bir VM içinde kullanmayı düşündüğünüz belirtebilirsiniz. Eylül yepyeni bir VM Azure portalı üzerinden yüklenebilir veya PowerShell kullanarak var olan bir VM üzerinde yüklenebilir.

Daha fazla bilgi edinin:

* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Azure bulut Hizmetleri ve sanal makineleri için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Yükleme ve bir Windows VM bir hizmet olarak eğilimi mikro derin güvenliği yapılandırma](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve Symantec Endpoint Protection bir Windows VM yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure sanal makineleri – McAfee Endpoint Protection korunması için yeni kötü amaçlı yazılımdan koruma seçenekleri](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication
Azure çok faktörlü kimlik doğrulaması (MFA), birden fazla doğrulama yöntemi kullanılmasını gerektiren ve kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekleyen kimlik doğrulama yöntemidir. MFA yardımcı olan veri ve basit bir oturum açma işlemi için kullanıcı talebine toplantı sırasında uygulamalara erişimi korumaya. Güçlü kimlik doğrulama seçeneklerini çeşitli aracılığıyla sunar — telefon araması, SMS mesajı veya mobil uygulama bildirimi veya doğrulama kodu ve üçüncü taraf OATH belirteçleri.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../multi-factor-authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır](../multi-factor-authentication/multi-factor-authentication-how-it-works.md)

## <a name="expressroute"></a>ExpressRoute
Microsoft Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden şirket içi ağlarınızı Microsoft bulutuna genişletmenizi sağlar. ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi Microsoft bulut hizmetlerine bağlantı kurabilirsiniz. Ortak yerleşim tesisinde bağlantı sağlayıcısı üzerinden herhangi bir ağdan herhangi bir ağa (IP VP), noktadan noktaya Ethernet ağı veya sanal çapraz bağlantısından bağlantı olabilir.  ExpressRoute bağlantıları ortak İnternet üzerinden geçmemektedir. Bu, ExpressRoute bağlantılarına İnternet üzerindeki sıradan bağlantılara göre daha fazla güvenilirlik, yüksek hız, düşük gecikme ve normal bağlantılardan daha yüksek güvenlik sağlar.

Daha fazla bilgi edinin:

* [ExpressRoute teknik genel bakış](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Sanal ağ geçitleri
Azure sanal ağ geçitleri olarak da adlandırılan, VPN ağ geçitleri, sanal ağlar ve şirket içi konumlara arasında ağ trafiğini göndermek için kullanılır. Azure'da (VNet-VNet) birden çok sanal ağlar arasında trafiği göndermek için de kullanılır.  VPN ağ geçitleri altyapınız ile Azure arasında güvenli şirket içi bağlantısı sağlar.

Daha fazla bilgi edinin:

* [VPN ağ geçitleri hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Azure ağ güvenliğine genel bakış](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management
Bazen kullanıcıların Azure kaynakları veya diğer SaaS uygulamalarında ayrıcalıklı işlemleri gerçekleştirmek gerekir. Bu, genellikle Azure Active Directory (Azure AD) kalıcı ayrıcalıklı erişim vermediğiniz kuruluşlar gerektiği anlamına gelir. Kuruluşlar, yeteri kadar ayrıcalıklı erişim ile bu kullanıcıların ne yaptıklarını izleyemez bulutta barındırılan kaynaklar için büyüyen bir güvenlik riski olmasıdır.
Ayrıca, ayrıcalıklı erişimi olan bir kullanıcı hesabı ihlal edilmesi durumunda, bir ihlali genel bulut güvenliğinizi etkileyebilir. Azure AD Privileged Identity Management ayrıcalıkları Etkilenme süresini azaltarak ve kullanım görünürlük artırma tarafından bu riski gidermeye yardımcı olur.  

Privileged Identity Management geçici bir yönetici rol ya da bunun için rolü atanmış bir etkinleştirme işlemini tamamlamak için gereken bir kullanıcı "tam zamanında" Yönetici erişimi için kavramını sunmaktadır. Etkinleştirme işlemi kullanıcının atama sekiz saat gibi belirli bir süredir etkin etkin olmayan dönemden Azure AD'de rol değiştirir.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../active-directory/active-directory-privileged-identity-management-configure.md)
* [Azure AD Privileged Identity Management ile çalışmaya başlama](../active-directory/active-directory-privileged-identity-management-getting-started.md)

## <a name="identity-protection"></a>Kimlik Koruması
Azure Active Directory (AD) kimlik koruması oturum açma kuşkulu etkinlikleri ve işinizin korunmasına yardımcı olmak için olası güvenlik açıklarını birleştirilmiş bir görünümünü sağlar. Kimlik koruması kullanıcılar için kuşkulu etkinlikleri algılar ve ayrıcalıklı (Yönetici) kimlikleri, kaba kuvvet saldırıları gibi sinyalleri dayalı kimlik bilgilerini ve oturum açma işlemleri tanınmayan konumlardan sızmasını ve aygıtları virüs bulaşmış.

Bildirimler ve önerilen düzeltme sağlayarak kimlik koruması gerçek zamanlı risklerin azaltılmasına yardımcı olur. Kullanıcı risk önem hesaplar ve otomatik olarak koruma uygulama gelecekteki tehditlerinden erişim yardımcı olmak için risk tabanlı ilkeleri yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD ve kimlik göster: kimlik koruması Önizleme](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Güvenlik Merkezi
Azure Güvenlik Merkezi, engellemenize, algılamanıza ve tehditlerine karşı yanıt yardımcı olur ve artan, görünürlük ve denetim üzerinden, Azure kaynaklarınızın güvenliğini sağlar. Aboneliklerinizde, tümleşik güvenlik izleme ve ilke yönetimi sağlar; normal koşullarda gözden kaçabilecek tehditleri algılamaya yardımcı olur ve güvenlik çözümlerinin geniş ekosistemiyle çalışır.

Güvenlik Merkezi en iyi duruma getirme ve Azure kaynaklarınızın güvenliğini izlemenize yardımcı olur:

* Her abonelik için Azure aboneliği kaynaklarınızı şirketinizin güvenlik gereksinimlerine ve uygulamaların türü veya verilerin duyarlılığına göre ilkeleri tanımlamak etkinleştiriliyor.
* Azure sanal makineleri, ağ ve uygulamaların durumunu izleme.
* Hızlı araştırmanız gereken bilgiler ve öneriler saldırıyı düzeltmek nasıl birlikte tümleşik iş ortağı çözümlerinden gelen uyarılar dahil olmak üzere öncelikli güvenlik uyarıları listesi sağlama.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne giriş](../security-center/security-center-intro.md)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
