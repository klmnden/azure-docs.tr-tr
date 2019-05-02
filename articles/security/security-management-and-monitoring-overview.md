---
title: Yönetim ve güvenlik özellikleri - Microsoft Azure izleme | Microsoft Docs
description: Bu makalede, Azure yönetiminde yardımcı sağlayan güvenlik özellikleri ve Hizmetleri genel bakış ve Azure bulut Hizmetleri ve sanal makinelerin izlenmesini sağlar.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 5cf2827b-6cd3-434d-9100-d7411f7ed424
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: 9f741f578ea44e27814ddfcde2fadc44a0e90536
ms.sourcegitcommit: 8a681ba0aaba07965a2adba84a8407282b5762b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64872079"
---
# <a name="azure-security-management-and-monitoring-overview"></a>Azure güvenlik yönetimi ve izlemeye genel bakış
Bu makalede, Azure yönetiminde yardımcı sağlayan güvenlik özellikleri ve Hizmetleri genel bakış ve Azure bulut Hizmetleri ve sanal makinelerin izlenmesini sağlar.

## <a name="shared-responsibility"></a>Paylaşılan sorumluluk

Microsoft bulut hizmetlerinizi güvenliği, bir iş ortaklığı ve siz ve Microsoft arasında paylaşılan bir sorumluluğu vardır. Microsoft Azure platformundan ve onun veri merkezlerinin fiziksel güvenlik için sorumlu (kilitli rozet giriş kapılar, sınırlar ve koruyucuları gibi güvenlik korumaları kullanarak). Azure müşterilerine güvenlik, gizlilik ve uyumluluk gereksinimlerini karşılayan yazılım katmanında bulut güvenlik güçlü düzeyleri sağlar.

Verilerinizi ve kimlik, bunları, şirket içi kaynaklarınızın güvenliğini ve üzerinde denetime sahip bulut bileşenlerinin güvenliğini korumak için sorumluluk sahibi. Microsoft, güvenlik denetimleri ve verilerinizi ve uygulamalarınızı korumanıza yardımcı olmak için özellikleri sağlar. Güvenlik için sorumluluk, derece bulut hizmeti türüne bağlıdır.

Aşağıdaki grafikte, sorumluluk, müşteri ile Microsoft arasındaki dengeyi özetler.

![Paylaşılan sorumluluk][1]

Güvenlik yönetimi hakkında daha fazla bilgi için bkz. [azure'da güvenlik yönetimi](azure-security-management.md).

## <a name="role-based-access-control"></a>Rol Tabanlı Access Control

Rol tabanlı erişim denetimi (RBAC), Azure kaynakları için ayrıntılı erişim yönetimi sağlar. RBAC kullanarak, kişiler yalnızca kullanıcıların işlerini gerçekleştirmek için ihtiyaç duydukları erişim miktarını verebilirsiniz. RBAC da kişi kuruluştan ayrıldıktan sonra bunlar bulut kaynaklarına erişimlerini emin olmanıza yardımcı olabilir.

Daha fazla bilgi edinin:

* [RBAC üzerinde Active Directory ekibi blogu](https://cloudblogs.microsoft.com/enterprisemobility/?product=azure-active-directory)
* [Azure rol tabanlı erişim denetimi](../role-based-access-control/role-assignments-portal.md)

## <a name="antimalware"></a>Kötü Amaçlı Yazılımdan Koruma

Azure ile Microsoft, Symantec, Trend Micro, McAfee ve Kaspersky gibi önde gelen güvenlik satıcılardan kötü amaçlı yazılımdan koruma yazılımı kullanabilirsiniz. Bu yazılımı, sanal makinelerinizi kötü amaçlı dosyalardan, reklam yazılımlarından ve diğer tehditlerden korunmasına yardımcı olur.

Azure Cloud Services ve sanal makineler için Microsoft Antimalware hem PaaS rollerini hem de sanal makineler için bir kötü amaçlı yazılımdan koruma Aracısı yükleme olanağı sunar. System Center Endpoint Protection'ı bağlı olarak, bu özellik şirket kendini kanıtlamış bir güvenlik teknolojisidir buluta getirir.

Ayrıca, eğilim'ın için derin tümleştirme sunuyoruz [Deep Security](https://www.trendmicro.com/us/enterprise/cloud-solutions/deep-security/) ve [SecureCloud](https://www.trendmicro.com/us/enterprise/cloud-solutions/secure-cloud/) ürünleri, Azure platformunda. Bir şifreleme SecureCloud çözümüdür ve DEEP Security'yi bir virüsten koruma çözümüdür. DEEP Security'yi VM'ler içinde bir uzantı modeli aracılığıyla dağıtılır. Azure portalı kullanıcı Arabirimi ve PowerShell kullanarak çalışmaya başlar yeni VM'ler veya zaten dağıtılmış olan mevcut Vm'lere içinde Deep Security kullanmayı da tercih edebilirsiniz.

Symantec Endpoint Protection (SEP), Azure üzerinde de desteklenir. Portal tümleştirmesi, bir VM'de SEP kullanmayı düşündüğünüz belirtebilirsiniz. SEP yeni bir VM Azure portal aracılığıyla yüklenebilir veya mevcut bir VM'yi PowerShell aracılığıyla yüklenebilir.

Daha fazla bilgi edinin:

* [Azure Sanal Makinelerinde Kötü Amaçlı Yazılıma Karşı Koruma Çözümleri Dağıtma](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
* [Azure bulut Hizmetleri ve sanal makineler için Microsoft kötü amaçlı yazılımdan koruma](azure-security-antimalware.md)
* [Yükleme ve bir Windows VM'de hizmet olarak Trend Micro Deep Security yapılandırın](../virtual-machines/windows/classic/install-trend.md)
* [Yükleme ve bir Windows VM'de Symantec Endpoint Protection'ı yapılandırma](../virtual-machines/windows/classic/install-symantec.md)
* [Azure sanal makineleri korumak için yeni kötü amaçlı yazılımdan koruma seçenekleri](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)

## <a name="multi-factor-authentication"></a>Multi-Factor Authentication

Azure multi-Factor Authentication, birden fazla doğrulama yöntemi kullanılmasını gerektiren kimlik doğrulama yöntemidir. Kullanıcı oturum açmalarına ve işlemlerine önemli bir ikinci güvenlik katmanı ekler.

Çok faktörlü kimlik doğrulaması erişimi korumaya yardımcı olur ve uygulamalarınıza karşılarken basit bir oturum açma işlemi. Doğrulama seçenekleri (telefon araması, SMS mesajı veya mobil uygulama bildirimi ya da doğrulama kodu) ve üçüncü taraf OATH belirteçleri bir aralık aracılığıyla güçlü kimlik doğrulama sağlar.

Daha fazla bilgi edinin:

* [Multi-Factor Authentication](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)
* [Azure Multi-Factor Authentication nedir?](../active-directory/authentication/multi-factor-authentication.md)
* [Azure multi-Factor Authentication nasıl çalışır?](../active-directory/authentication/concept-mfa-howitworks.md)

## <a name="expressroute"></a>ExpressRoute

Azure ExpressRoute, bağlantı sağlayıcı tarafından kolaylaştırılan adanmış özel bağlantı üzerinden Microsoft Cloud ile şirket içi ağlarınızı genişletmek için kullanabilirsiniz. ExpressRoute ile Microsoft Azure, Office 365 ve CRM Online gibi bulut hizmetlerine bağlantı kurabilirsiniz. Bağlantı olabilir:

* Herhangi bir ağdan herhangi bir (IP VP) ağ.
* Noktadan noktaya Ethernet ağı.
* Bir sanal çapraz bağlantısından bir ortak konum tesisinde bağlantı sağlayıcısı üzerinden.

ExpressRoute bağlantıları ortak internet üzerinden kurulmaz. Bunlar daha fazla güvenilirlik, daha yüksek hız, daha düşük gecikme süreleri ve daha yüksek güvenlik tipik internet üzerinden sunabilir.

Daha fazla bilgi edinin:

* [Expressroute'a teknik genel bakış](../expressroute/expressroute-introduction.md)

## <a name="virtual-network-gateways"></a>Sanal ağ geçitleri

VPN ağ geçitleri, Azure sanal ağ geçitlerine olarak da bilinir, sanal ağlar ve şirket içi konumlara arasında ağ trafiği göndermek için kullanılır. Bunlar, ayrıca Azure'da (ağ için ağ) birden çok sanal ağ arasında trafik göndermek için kullanılır. VPN ağ geçitleri, Azure ile altyapınız arasında şirketler arası güvenli bağlantı sağlar.

Daha fazla bilgi edinin:

* [VPN ağ geçitleri hakkında](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [Azure ağ güvenliğine genel bakış](security-network-overview.md)

## <a name="privileged-identity-management"></a>Privileged Identity Management

Bazen kullanıcılar Azure kaynaklarına veya diğer SaaS uygulamalarına ayrıcalıklı işlemleri gerçekleştirmek gerekebilir. Bu, genellikle kuruluşların kalıcı ayrıcalıklı erişimi Azure Active Directory (Azure AD) vermediğiniz anlamına gelir.

Kuruluşlar, yeteri kadar ayrıcalıklı erişim ile bu kullanıcıların ne yaptıklarını izleyemez bulutta barındırılan kaynakları için artan bir güvenlik riski olmasıdır. Ayrıca, ayrıcalıklı erişime sahip bir hesap tehlikede olursa, bir İhlale yol açmak üzere bir kuruluşun genel bulut güvenliği etkileyebilir. Azure AD Privileged Identity Management ayrıcalıkların tehditlere maruz kalabileceği süreyi azaltarak ve artan kullanım görünürlük tarafından bu riski gidermeye yardımcı olur.  

Privileged Identity Management, geçici bir yönetici rolü veya yönetici erişimi "yalnızca zamanında" kavramını sunar. Bu tür bir yönetici rolü atanmış olan için etkinleştirme işlemini tamamlamak için gereken bir kullanıcıdır. Etkinleştirme işlemi etkin, belirli bir süredir etkin olmayan dönemden Azure AD'de rol atama kullanıcının değiştirir.

Daha fazla bilgi edinin:

* [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/pim-configure.md)
* [Azure AD Privileged Identity Management ile çalışmaya başlama](../active-directory/privileged-identity-management/pim-getting-started.md)

## <a name="identity-protection"></a>Kimlik Koruması

Azure AD kimlik koruması, şüpheli oturum açma etkinlikleri ve işletmenizin korunmasına yardımcı olmak için olası güvenlik açıklarını birleştirilmiş bir görünümünü sağlar. Kimlik koruması, kullanıcılar ve gibi işaretlere dayalı (Yönetici) ayrıcalıklı kimlikleri için şüpheli etkinlikleri algılar:

* Deneme yanılma saldırıları.
* Sızan kimlik bilgileri.
* Tanınmayan konumlardan ve virüslü cihazlardan oturum açma işlemleri.

Bildirimler ve önerilen düzeltmeyi sağlayarak gerçek zamanlı olarak riskleri azaltmak için kimlik koruması yardımcı olur. Bu kullanıcı risk önem derecesi hesaplar. Otomatik olarak erişim gelecekteki tehditlerden koruyun uygulama yardımcı olmak için risk tabanlı ilkeler yapılandırabilirsiniz.

Daha fazla bilgi edinin:

* [Azure Active Directory kimlik koruması](../active-directory/active-directory-identityprotection.md)
* [Kanal 9: Azure AD kimlik gösterin: Kimlik koruması önizlemesi](https://channel9.msdn.com/Series/Azure-AD-Identity/Azure-AD-and-Identity-Show-Identity-Protection-Preview)

## <a name="security-center"></a>Güvenlik Merkezi

Azure Güvenlik Merkezi, tehditleri önleyin, algılayın ve yardımcı olur. Güvenlik Merkezi verir artırılmış görünürlük ve üzerinde Azure kaynaklarınızın güvenliğini denetleyebilirsiniz. Bu, Azure aboneliklerinizde tümleşik güvenlik izleme ve ilke yönetimi sağlar. Aksi takdirde gözden kaçan geçebilir ve güvenlik çözümlerinin geniş ekosistemiyle çalışan tehditleri algılamanıza yardımcı olur.

Güvenlik Merkezi, Azure kaynaklarınızı izleyin ve en iyi duruma yardımcı olur:

* Azure abonelik kaynaklarınıza göre ilkeleri tanımlamak etkinleştirme:
  * Şirketinizin güvenlik gerekir.
  * Uygulamaları ya da her Abonelikteki verilerin duyarlılığına türü.
* Azure sanal makinelerinizin durumunu izleme, ağ ve uygulama.
* Tümleşik iş ortağı çözümlerinden gelen uyarılar dahil olmak üzere öncelikli güvenlik uyarıları listesi sağlama. Ayrıca, bir saldırı ve sorunu nasıl çözebileceğiyle ilgili öneriler hızla araştırmak ihtiyacınız olan bilgileri sağlar.

Daha fazla bilgi edinin:

* [Azure Güvenlik Merkezi'ne Giriş](../security-center/security-center-intro.md)
* [Azure Güvenlik Merkezi'nde güvenli puanınız geliştirin](../security-center/security-center-secure-score.md)

## <a name="intelligent-security-graph"></a>Intelligent Security Graph

Intelligent Security Graph, Microsoft ürünleri ve Hizmetleri gerçek zamanlı tehdit koruması sağlar. Kuruluş güvenliğini güçlendirebilirsiniz bilgiler sağlamak için tehdit zekası ve güvenlik verilerini büyük miktarda bağlantı Gelişmiş analiz kullanır. Microsoft, İleri düzey analizlerden — ayda 450 milyardan fazla kimlik doğrulamaları işlemeye, kötü amaçlı yazılım ve kimlik avı için 400 milyar e-posta tarama ve bir milyar cihazı güncelleştiren — daha zengin içgörüler sunmak için. Bu bilgiler, sizin kurumunuzun da saldırıları fark ederek hızlı bir şekilde karşılık vermesine yardımcı olabilir.

* [Akıllı güvenlik grafiği](https://www.microsoft.com/security/intelligence)

<!--Image references-->
[1]: ./media/security-management-and-monitoring-overview/shared-responsibility.png
