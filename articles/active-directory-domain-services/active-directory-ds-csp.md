---
title: Azure Active Directory etki alanı Hizmetleri için Azure bulut çözüm sağlayıcıları | Microsoft Docs
description: Azure bulut çözüm sağlayıcıları için Azure Active Directory etki alanı Hizmetleri.
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: maheshu
ms.openlocfilehash: 362d7226434733ffeaa5be6f988afa4016a7c827
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36213881"
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Azure bulut çözüm sağlayıcıları (CSP) için Azure Active Directory (AD) etki alanı Hizmetleri
Bu makalede, Azure AD etki alanı Hizmetleri Azure CSP abonelikte nasıl kullanabileceğiniz açıklanır.

## <a name="overview-of-azure-csp"></a>Azure CSP genel bakış
Azure CSP Microsoft Partners için bir program ve çeşitli Microsoft bulut Hizmetleri için bir lisans kanal sağlar. Azure CSP satış yönetmek, fatura ilişki sahibi, teknik destek ve faturalama desteği sağlamak ve Müşteri'nin tek iletişim noktası olması iş ortaklarının sağlar. Ayrıca, Azure CSP API'leri eşlik eden ve Self Servis portalı dahil olmak üzere Araçlar, tam kümesi sağlar. Bu araçları kolayca sağlamak ve Azure kaynaklarınızı yönetmek CSP ortakları etkinleştirin ve müşteriler ve bunların abonelikler için faturalama sağlayın.

[Ortağı Merkezi'nde portal](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) tüm Azure CSP iş ortakları için bir giriş noktası olarak davranır. Zengin müşteri yönetim özellikleri, otomatik işleme ve daha fazla bilgi sağlar. Azure CSP iş ortakları, web tabanlı bir kullanıcı arabirimini kullanarak veya PowerShell ve çeşitli API çağrılarını kullanarak iş ortağı merkezi işlevlerini kullanabilirsiniz.

Aşağıdaki diyagram, CSP modeli yüksek bir düzeyde nasıl çalıştığını gösterir. Contoso Azure AD Active Directory sahiptir. Dağıtır ve Azure CSP aboneliklerinin kaynaklarında yöneten bir CSP, yöneticileriyle sahiptirler. Contoso, doğrudan Contoso faturalandırılır normal (doğrudan) Azure abonelikleri da sahip olabilirsiniz.

![CSP modeline genel bakış](./media/csp/csp_model_overview.png)

CSP ortağın Kiracı yönetici üç özel aracı grupları - sahip aracıları, Yardım Masası aracıları ve satış aracıları. Yönetim aracıları Grup Contoso Azure AD dizini içinde Kiracı Yönetici rolü atanır. Sonuç olarak, CSP ortağın yönetim aracıları gruba ait olan bir kullanıcı Contoso Azure AD dizininde Kiracı yönetici ayrıcalıklarına sahip. Ne zaman CSP iş ortağı hükümleri Azure CSP aboneliği Contoso, kendi yönetim aracıları grup için bu abonelik için sahip rolünü atanır. Sonuç olarak, CSP ortağın yönetim aracıları sanal makineler, sanal ağlar ve adına Contoso Azure AD etki alanı Hizmetleri gibi Azure kaynaklarını hazırlama için gerekli ayrıcalıklara sahip.

Daha fazla bilgi için bkz: [Azure CSP genel bakış](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Bir Azure CSP abonelikte Azure AD etki alanı Hizmetleri kullanmanın yararları
Azure AD etki alanı Hizmetleri, Windows Server AD uyumlu LDAP, Kerberos/NTLM kimlik doğrulaması, etki alanına katılma, Grup İlkesi ve DNS gibi Azure hizmetlerinde sağlar. On yılları birçok uygulama AD karşı çalışması için bu özellikleri kullanılarak oluşturulmuş. Çok sayıda bağımsız yazılım satıcılarının (ISV'ler) yerleşik ve müşterilerin şirket içi uygulamalar dağıtılır. Bu, genellikle bu uygulamaları dağıtılmış olduğundan farklı ortamlar için erişim gerektiren bu yana desteklemek için onerous uygulamalardır. Azure CSP aboneliklerle daha basit bir alternatifi ölçek ve esneklik Azure ile sahip.

Azure AD etki alanı Hizmetleri artık Azure CSP abonelikleri destekler. Artık müşterinizin Azure AD dizini için bağlı bir Azure CSP abonelik uygulamanızda dağıtabilirsiniz. Sonuç olarak, çalışanlarınızın (destek personeli) yönetmek, yönetmek ve üzerinde uygulamanız, kuruluşunuzun Kurumsal kimlik bilgileri kullanılarak dağıtılan sanal makineler hizmet. Ayrıca, müşterinizin Azure AD dizini için bir Azure AD etki alanı Hizmetleri yönetilen etki alanı sağlayabilirsiniz. Uygulamanız, müşterinizin yönetilen etki alanına bağlanır. Bu nedenle, uygulamanızın içinde Kerberos/NTLM, LDAP, kullanan özellikleri veya [System.DirectoryServices API](https://msdn.microsoft.com/library/system.directoryservices) müşterinizin dizin karşı sorunsuz bir şekilde çalışır. Son müşterileriniz uygulamanızın bir hizmet olarak uygulamanın dağıtıldığı altyapısını sürdürme hakkında endişelenmeye gerek kalmadan tüketmesini büyük ölçüde yararlanır.

Abonelik, Azure AD etki alanı Hizmetleri dahil olmak üzere size geri doludur, tükettiğiniz Azure kaynakları için tüm faturalama'yı tıklatın. Satış, fatura, teknik destek vb. için geldiğinde müşteriyle ilişki üzerinde tam denetimi korumak. Azure CSP platform esnekliği örnekleri olan gibi birçok müşteri destek aracılarının küçük bir ekibe hizmet uygulamanızın dağıtılabilir.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>Azure AD etki alanı Hizmetleri için CSP dağıtım modelleri
Azure AD etki alanı Hizmetleri ile Azure CSP abonelik olarak kullanmak iki yolu vardır. Müşterilerinizin güvenlik ve Basitlik değerlendirmelerine göre doğru olanı seçin.

### <a name="direct-deployment-model"></a>Doğrudan dağıtım modeli
Bu dağıtım modelinde, Azure AD etki alanı Hizmetleri Azure CSP aboneliğe ait sanal ağ içinde etkindir. CSP ortağın yönetim aracıları aşağıdaki ayrıcalıklara sahip:
* Müşteri'nin Azure AD dizininde genel yönetici ayrıcalıkları.
* Abonelik sahibi ayrıcalıkları Azure CSP abonelikte.

![Doğrudan dağıtım modeli](./media/csp/csp_direct_deployment_model.png)

Bu dağıtım modelinde CSP sağlayıcının yönetim aracıları için müşteri kimlikleri yönetebilirsiniz. Bu yönetim aracıları yeni kullanıcılar, gruplar sağlamak, müşterinin Azure AD dizini vb. uygulamalarda Ekle seçeneğine sahipsiniz. Bu dağıtım modeli olmayan bir adanmış kimlik yöneticisi veya şirket adına kimlikleri yönetmek CSP ortak tercih daha küçük kuruluşlar için uygun.


### <a name="peered-deployment-model"></a>Eşlenen dağıtım modeli
Bu dağıtım modelinde, Azure AD etki alanı Hizmetleri müşteriye - diğer bir deyişle, müşteri tarafından ödendiği doğrudan bir Azure aboneliğine ait bir sanal ağ içinde etkindir. CSP iş ortağı sonra Müşteri'nin CSP aboneliğine ait bir sanal ağ içinde uygulamaları dağıtabilirsiniz. Sanal ağlar sonra Azure sanal ağ eşlemesi kullanarak bağlanabilir. Sonuç olarak, Azure CSP Abonelikteki CSP ortağı tarafından dağıtılan iş yükleri/uygulamaları Müşteri'nin doğrudan Azure aboneliğinizde sağlanan Müşteri'nin yönetilen etki alanı bağlanabilir.

![Eşlenen dağıtım modeli](./media/csp/csp_peered_deployment_model.png)

Bu dağıtım modeli ayrıcalıkları ayrımı sağlar ve Azure aboneliğini yönetmek ve dağıtmak ve içindeki kaynaklara yönetmek CSP ortağın Yardım Masası aracılarını etkinleştirir. Ancak, CSP ortağın Yardım Masası aracıları müşterinin Azure AD dizininde genel yönetici ayrıcalıklarına sahip olmanız gerekmez. Müşteri'nin kimlik Yöneticiler, kuruluşlarının için kimlikleri yönetme devam edebilirsiniz.

Bu dağıtım modeli burada (bağımsız yazılım satıcısı) ISV sağlar barındırılan senaryoları için uygun sürümü de müşteriye bağlanması için gereken kendi şirket içi uygulama, kullanıcının AD.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>CSP abonelikleri etki alanlarında yönetilen Azure AD etki alanı Hizmetleri yönetme
Yönetilen bir etki alanına bir Azure CSP Abonelikteki yönetirken aşağıdaki önemli noktalar geçerlidir:

* **CSP yönetim aracıları, yönetilen etki alanı kimlik bilgilerini kullanarak hazırlayabilirsiniz:** Azure AD etki alanı Hizmetleri Azure CSP abonelikleri destekler. Bu nedenle, bir CSP iş ortağının yönetim aracıları gruba ait kullanıcılar yeni bir Azure AD etki alanı Hizmetleri yönetilen etki alanı sağlayabilirsiniz.

* **CSP'ler yeni yönetilen etki alanları oluşturma PowerShell kullanarak müşterileri için komut dosyası:** bkz [Hizmetleri PowerShell kullanarak Azure AD etki alanını etkinleştirmek için nasıl](active-directory-ds-enable-using-powershell.md) Ayrıntılar için.

* **CSP yönetim aracıları kimlik bilgilerini kullanarak yönetilen etki alanı üzerinde devam eden yönetim görevlerini gerçekleştiremezsiniz:** CSP yönetici kullanıcıların kimlik bilgilerini kullanarak yönetilen etki alanı içinde olağan yönetim görevlerini gerçekleştiremez. Bu kullanıcılar müşterinin Azure AD dizini için dış ve kimlik bilgilerini müşterinin Azure AD dizini içinde kullanılabilir değil. Bu nedenle, Azure AD etki alanı Hizmetleri erişimi olmayan Kerberos ve NTLM parola karmaları için bu kullanıcılar için. Sonuç olarak, bu tür kullanıcıların Azure AD etki alanı Hizmetleri yönetilen etki alanlarında kimliği doğrulanamıyor.

  > [!WARNING]
  > **Yönetilen etki alanı üzerinde devam eden yönetim görevlerini gerçekleştirmek için Müşteri'nin dizindeki bir kullanıcı hesabı oluşturmanız gerekir.**
  > Bir CSP yönetici kullanıcının kimlik bilgilerini kullanarak yönetilen etki alanında oturum açamaz. Bunu yapmak için Müşteri'nin Azure AD dizinine ait bir kullanıcı hesabının kimlik bilgilerini kullanın. Sanal makineler için yönetilen etki alanına katılma, DNS yönetme, Grup İlkesi vb. yönetme gibi görevler için bu kimlik bilgileri gerekir.
  >

* **Devam eden Yönetim 'AAD DC Yöneticiler' grubuna eklenmesi için oluşturulan kullanıcı hesabı:** 'AAD DC Yöneticiler' grubu yönetilen etki alanında belirli temsilci ile yönetim görevlerini gerçekleştirmek için ayrıcalıklara sahiptir. Bu görevler DNS kuruluş birimleri, Grup İlkesi vb. yönetme oluşturma, yapılandırma içerir. Bir CSP ortağı yönetilen etki alanı gibi görevleri gerçekleştirmek bir kullanıcı hesabı müşterinin Azure AD dizini içinde oluşturulması gerekir. Bu hesabın kimlik bilgilerini CSP ortağının yönetici aracılarla paylaşılması gerekir. Ayrıca, bu kullanıcı hesabının yönetilen etki alanı bu kullanıcı hesabını kullanarak gerçekleştirilmesi gereken yapılandırma görevleri etkinleştirmek için 'AAD DC Yöneticiler' grubuna eklenmesi gerekir.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure CSP programa kaydolmak](https://partnercenter.microsoft.com/partner/programs) ve Azure CSP aracılığıyla iş oluşturmaya başlayın.
* Listesini gözden geçirin [Azure Hizmetleri Azure CSP kullanılabilir](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).
* [PowerShell kullanarak Azure AD Domain Services'ı etkinleştirme](active-directory-ds-enable-using-powershell.md)
* [Azure AD etki alanı Hizmetleri ile çalışmaya başlama](active-directory-ds-getting-started.md)
