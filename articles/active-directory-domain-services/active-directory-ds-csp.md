---
title: Azure Active Directory etki alanı Hizmetleri için Azure bulut çözüm sağlayıcıları | Microsoft Docs
description: Azure bulut çözüm sağlayıcıları için Azure Active Directory etki alanı Hizmetleri.
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mahesh-unnikrishnan
editor: curtand
ms.assetid: 56ccb219-11b2-4e43-9f07-5a76e3cd8da8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/08/2017
ms.author: ergreenl
ms.openlocfilehash: 8beba4f66cf24a937eec77e4bfdee2057b417269
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58892815"
---
# <a name="azure-active-directory-ad-domain-services-for-azure-cloud-solution-providers-csp"></a>Azure bulut çözüm sağlayıcıları (CSP) için Azure Active Directory (AD) etki alanı Hizmetleri
Bu makalede, Azure AD Etki Alanı Hizmetleri'ni bir Azure CSP aboneliği içinde nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="overview-of-azure-csp"></a>Azure CSP genel bakış
Azure CSP Microsoft Partners programdır ve çeşitli Microsoft bulut Hizmetleri için bir lisans kanal sağlar. Azure CSP iş ortaklarına, satışı yönetme, faturalandırma ilişkisine, teknik ve faturalandırma desteği sağlayan ve müşterinin tek temas noktası olma sağlar. Ayrıca, Azure CSP araçları, tam bir dizi, bir Self Servis portalı da dahil olmak üzere ve bunlara eşlik eden API'ler sağlar. Bu araçlar, etkinleştirme kolayca sağlamak ve Azure kaynaklarınızı yönetmek CSP iş ortakları ve müşterileri ve abonelikleri için fatura sağlayın.

[İş ortağı merkezi portalında](https://docs.microsoft.com/azure/cloud-solution-provider/overview/partner-center-overview) tüm Azure CSP iş ortakları için giriş noktası olarak görev yapar. Bu, zengin müşteri yönetimi özellikleri, otomatik işleme ve daha fazlasını sağlar. Azure CSP iş ortakları, web tabanlı bir kullanıcı arabirimini kullanarak veya PowerShell ve çeşitli API çağrıları kullanarak iş ortağı merkezi özellikleri kullanabilirsiniz.

Aşağıdaki diyagram, CSP modeli yüksek bir düzeyde nasıl çalıştığını gösterir. Contoso, bir Azure AD Active Directory'e sahiptir. Dağıtır ve Azure CSP aboneliklerinde kaynaklarını yöneten bir CSP ile iş ortaklığı sahiptirler. Contoso, Contoso için doğrudan faturalandırılan normal (doğrudan) Azure Abonelikleri, da olabilir.

![CSP modeline genel bakış](./media/csp/csp_model_overview.png)

CSP iş ortağı Kiracı yönetici üç özel aracı grupları - olan aracıları, Yardım Masası aracıları ve satış aracıları. Yönetim aracıları grup, Contoso Azure AD dizini Kiracı Yönetici rolüne atanır. Sonuç olarak, CSP iş ortağının yönetim aracıları gruba ait olan bir kullanıcı Contoso Azure AD dizininde Kiracı yönetici ayrıcalıklarına sahip. Ne zaman CSP iş ortağı hükümlerine Contoso, yönetim aracıları grup için bir Azure CSP aboneliği Bu abonelik için sahip rolü atanır. Sonuç olarak, CSP iş ortağının yönetim aracıları, sanal makineler, sanal ağlar ve adına Contoso Azure AD Domain Services gibi Azure kaynaklarının sağlanması için gerekli ayrıcalıklara sahip.

Daha fazla bilgi için [Azure CSP genel bakış](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-overview)

## <a name="benefits-of-using-azure-ad-domain-services-in-an-azure-csp-subscription"></a>Bir Azure CSP aboneliği içinde Azure AD Domain Services'ı kullanmanın avantajları
Azure AD etki alanı Hizmetleri, LDAP, Kerberos/NTLM kimlik doğrulaması, etki alanına katılım, Grup İlkesi ve DNS gibi azure'daki Windows Server AD uyumlu hizmetleri sağlar. Konusundaki onlarca yıllık birçok uygulama karşı AD çalışmak için bu özellikleri kullanarak eklenmiştir. Çok sayıda bağımsız yazılım satıcılarına (ISV) oluşturulan ve dağıtılan uygulamalar, müşterilerinin şirket içi. Bu, genellikle bu uygulamaların dağıtıldığı farklı ortamlara erişim gerektirdiğinden desteklemek için onerous uygulamalardır. Azure CSP aboneliklerinde ile ölçek ve esneklik, Azure ile daha basit bir alternatif sahip.

Azure AD etki alanı Hizmetleri artık Azure CSP aboneliklerinde destekler. Artık müşterinizin Azure AD dizini için bağlı bir Azure CSP aboneliği içinde uygulamanızın dağıtabilirsiniz. Sonuç olarak, çalışanlarınızın (destek personeli) yönetme, yönetmek ve kuruluşunuzun Kurumsal kimlik bilgilerini kullanarak, uygulamanızın dağıtıldığı sanal makineler hizmet. Ayrıca, müşterinizin Azure AD dizini için bir Azure AD Domain Services yönetilen etki alanı sağlayabilirsiniz. Uygulamanızı müşterinizin yönetilen etki alanına bağlı. Bu nedenle, Kerberos/NTLM, LDAP, kullanan özellikleri uygulamanızda veya [System.DirectoryServices API](/dotnet/api/system.directoryservices) müşterinizin directory karşı sorunsuz bir şekilde çalışır. Uygulamanın dağıtıldığı altyapı bakımı hakkında endişelenmenize gerek kalmadan uygulamanızı bir hizmet olarak kullanan son müşterileriniz büyük ölçüde yarar.

Abonelik, Azure AD Domain Services de dahil olmak üzere size geri ücretlendirilir, tükettiğiniz Azure kaynakları için tüm faturalama'yı tıklatın. Satış, faturalandırma, teknik destek vb. geldiğinde müşteriyle ilişki üzerinde tam denetim korur. Azure CSP platform'ın esnekliği sayesinde, küçük bir takımda desteği aracıların örneğe sahip çok sayıda bu tür müşteriler hizmet verebilir, uygulamanızın dağıtılmış.


## <a name="csp-deployment-models-for-azure-ad-domain-services"></a>Azure AD Domain services için CSP dağıtım modelleri
Azure AD Domain Services ile bir Azure CSP aboneliği olarak kullanan iki yolu vardır. Güvenlik ve Basitlik konularına müşterilerinizin göre doğru olanı seçin.

### <a name="direct-deployment-model"></a>Doğrudan dağıtım modeli
Bu dağıtım modelinde, Azure AD Domain Services Azure CSP aboneliğe ait sanal ağ içinde etkinleştirilir. CSP iş ortağının yönetim aracıları, aşağıdaki ayrıcalıklara sahip:
* Müşterinin Azure AD dizininde genel yönetici ayrıcalıkları.
* Azure CSP aboneliği abonelik sahibi ayrıcalıkları.

![Doğrudan dağıtım modeli](./media/csp/csp_direct_deployment_model.png)

Bu dağıtım modelinde CSP sağlayıcının yönetim aracıları için müşteri kimliklerini yönetebilirsiniz. Bu yönetim aracıları yeni kullanıcıları, grupları sağlama, müşterinin Azure AD dizini vb. uygulamalarda ekleme seçeneğine sahipsiniz. Bu dağıtım modeli, bir özel kimlik yöneticiniz veya gerçekleştirilemeyeceğine ilişkin kimlikleri yönetmek CSP iş ortağı için tercih ettiğiniz daha küçük kuruluşlar için uygun olmayabilir.


### <a name="peered-deployment-model"></a>Eşlenen dağıtım modeli
Bu dağıtım modelinde, Azure AD Domain Services müşteriye - diğer bir deyişle, müşteri tarafından ödenen doğrudan bir Azure aboneliğine ait bir sanal ağ içinde etkinleştirilir. CSP iş ortağı, ardından müşterinin CSP aboneliğe ait sanal ağ içindeki uygulamalar dağıtabilirsiniz. Sanal ağlar, ardından Azure sanal ağ eşlemesi kullanarak bağlanabilir. Sonuç olarak, Azure CSP aboneliği CSP iş ortağı tarafından dağıtılan iş yüklerini/uygulamaları müşterinin doğrudan Azure aboneliğinde sağlanan müşterinin yönetilen etki alanına bağlanabilir.

![Eşlenen dağıtım modeli](./media/csp/csp_peered_deployment_model.png)

Bu dağıtım modeli ayrıcalıkları ayrımı sağlar ve Azure aboneliğini yönetmek ve dağıtmak ve içindeki kaynakları yönetmek CSP iş ortağı Yardım Masası agents sağlar. Bununla birlikte, CSP iş ortağı Yardım Masası aracıları müşterinin Azure AD dizininde genel yönetici ayrıcalıklarına sahip olmanız gerekmez. Müşteri'nin kimlik yöneticileri için kuruluş kimlik yönetmeye devam edebilirsiniz.

Bu dağıtım modeli burada bir ISV (bağımsız yazılım satıcısı) sağlayan bir barındırılan senaryoları için uygun olması da müşteriye bağlanmak için gereken kendi şirket içi uygulama sürümü kullanıcının AD.


## <a name="administering-azure-ad-domain-services-managed-domains-in-csp-subscriptions"></a>CSP aboneliklerini etki alanlarında yönetilen Azure AD Domain Services'ı yönetme
Yönetilen etki alanını Azure CSP aboneliği'ndeki yönetirken aşağıdaki önemli noktalar geçerlidir:

* **CSP yönetim aracıları, kimlik bilgilerini kullanarak yönetilen bir etki alanı sağlayabilirsiniz:** Azure CSP aboneliklerinde Azure AD etki alanı Hizmetleri destekler. Bu nedenle, bir CSP iş ortağının yönetim aracıları grubuna ait olan kullanıcılar, yeni bir Azure AD Domain Services yönetilen etki sağlayabilirsiniz.

* **CSP'ler yeni yönetilen etki alanları oluşturmak için PowerShell kullanarak müşterilerinin yazabilirsiniz:** Bkz: [PowerShell kullanarak Azure AD etki alanı hizmetleri nasıl](active-directory-ds-enable-using-powershell.md) Ayrıntılar için.

* **CSP yönetim aracıları, devam eden yönetim görevlerini, kimlik bilgilerini kullanarak yönetilen etki alanında gerçekleştiremezsiniz:** CSP yönetici kullanıcılar, kimlik bilgilerini kullanarak yönetilen etki alanı içinde olağan yönetim görevlerini gerçekleştirilemiyor. Müşterinin Azure AD dizini için bu kullanıcılar haricidir ve kimlik bilgilerini müşterinin Azure AD dizini içinde kullanılabilir değil. Bu nedenle, Azure AD Domain Services yok Kerberos ve NTLM parola karmalarını erişim bu kullanıcılar için. Sonuç olarak, bu kullanıcılar, Azure AD Domain Services yönetilen etki alanlarında doğrulanamıyor.

  > [!WARNING]
  > **Yönetilen etki alanı üzerinde devam eden yönetim görevlerini gerçekleştirmek için Müşteri'nin dizin içinde bir kullanıcı hesabı oluşturmanız gerekir.**
  > Bir CSP yönetici kullanıcı kimlik bilgilerini kullanarak yönetilen etki alanında oturum açamazsınız. Bunu yapmak için müşterinin Azure AD dizinine ait bir kullanıcı hesabının kimlik bilgilerini kullanın. Sanal makineleri yönetilen etki alanına katılma, DNS yönetme, Grup İlkesi vb. yönetme gibi görevler için bu kimlik bilgileri gerekir.
  >

* **Devam eden Yönetim için oluşturulan kullanıcı hesabının 'AAD DC Administrators' grubuna eklenmesi gerekir:** 'AAD DC Administrators' grubunun, yönetilen etki alanında belirli temsilci ile yönetim görevleri gerçekleştirmek için ayrıcalıklara sahip değil. Bu görevler DNS kuruluş birimleri, vb. Grup İlkesi Yönetimi oluşturma, yapılandırma içerir. Yönetilen bir etki alanında gibi görevleri gerçekleştirmek üzere bir CSP iş ortağı için bir kullanıcı hesabı müşterinin Azure AD dizini içinde oluşturulması gerekir. Bu hesabın kimlik bilgilerini, CSP iş ortağının yönetim aracıları ile paylaşılması gerekir. Ayrıca, bu kullanıcı hesabı, bu kullanıcı hesabını kullanarak gerçekleştirilmesi için yönetilen etki alanındaki yapılandırma görevlerini etkinleştirmek için 'AAD DC Administrators' grubuna eklenmelidir.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure CSP programı'na kayıt](https://docs.microsoft.com/partner-center/enrolling-in-the-csp-program) ve Azure CSP aracılığıyla işinizi oluşturmaya başlayın.
* Listesini gözden geçirin [Azure CSP'de kullanılabilen Azure Hizmetleri](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).
* [PowerShell kullanarak Azure AD Domain Services'ı etkinleştirme](active-directory-ds-enable-using-powershell.md)
* [Azure AD Domain Services ile çalışmaya başlama](active-directory-ds-getting-started.md)
