---
title: Bağlı Azure dağıtım kararları için Azure Stack tümleşik sistemleri | Microsoft Docs
description: Dağıtım kararları Azure Stack Azure bağlı çok düğümlü dağıtımlar için planlama saptayın.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/05/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.lastreviewed: 11/05/2018
ms.openlocfilehash: 491bdf121729d690784324051ff701f3ed2d2b7a
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243190"
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Azure bağlı dağıtım planlama kararları için Azure Stack tümleşik sistemleri
Karar verdim sonra [Azure Stack, hibrit bulut ortamına nasıl tümleştirilecek](azure-stack-connection-models.md), Azure Stack dağıtım kararlarınızı sonra sonlandır.

Azure Stack ile Azure'a bağlı dağıtma, Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) kimlik deponuz için olamayacağı anlamına gelir. Her iki faturalandırma modeli de seçebilirsiniz:-,-kullandıkça veya kapasite tabanlı. Bağlı bir dağıtım olduğundan varsayılan seçenek olan müşterilerin en çok değer dışında Azure Stack, özellikle hem Azure hem de Azure Stack içeren hibrit bulut senaryoları için alma olanak tanır. 

## <a name="choose-an-identity-store"></a>Bir kimlik deposu seçin
Bağlı bir dağıtım ile Azure AD veya kimlik deponuz için AD FS arasından seçim yapabilirsiniz. Bağlantısı kesilmiş bir dağıtım ile internet bağlantısı olmayan, yalnızca AD FS kullanabilirsiniz.

Kimlik deposu seçtiğiniz Kiracı sanal makinelerinde (VM'ler) bir ilgisi yoktur. Kiracı VM'ler, bunların nasıl yapılandırılır bağlı olarak bağlanma istedikleri hangi kimlik deposu tercih edebilirsiniz: Azure AD, Windows Server Active Directory etki alanına katılmış, çalışma grubu, vs. Azure Stack kimlik sağlayıcısı kararı ilgisiz olmasıdır. 

Iaas Kiracı Azure Stack üzerinde sanal makineleri dağıtmak ve bir kurumsal Active Directory etki alanına ve buradan hesaplarını kullanmak istiyorsanız, örneğin, yine de bunu yapabilirsiniz. Bu hesaplar için burada seçtiğiniz Azure AD kimlik deposu kullanmak için gerekli değildir.

### <a name="azure-ad-identity-store"></a>Azure AD kimlik deposu
Kullandığınızda, Azure AD kimlik deponuza iki Azure AD hesap gerektirir: bir genel yönetici hesabı ve Faturalama hesabı. Bu hesaplar aynı hesapları veya farklı hesaplar olabilir. Aynı kullanıcı hesabını kullanarak daha basit ve sınırlı sayıda Azure hesapları varsa yararlı olabilir ancak iki hesap kullanarak işletmenizin ihtiyaçlarını önerebilir:

1. **Genel yönetici hesabını** (yalnızca bağlı dağıtımları için gerekli). Azure Active Directory'de uygulamalar ve Azure Stack altyapı hizmetleri için hizmet sorumluları oluşturmak için kullanılan bir Azure hesabı budur. Bu hesap altında Azure Stack sistemi dağıtılacak dizin için dizin yönetici izinlerine sahip olmalıdır. "Bulut operatörü" olacak genel yönetici Azure AD için Kiracı ve kullanılacak: 
    - Sağlama ve temsilci uygulamalar ve Azure Active Directory Graph API ile etkileşim kurmak için gereken tüm Azure Stack Hizmetleri için hizmet sorumluları için. 
    - Hizmet Yöneticisi hesabını. (Daha sonra değiştirebilirsiniz) varsayılan sağlayıcı aboneliği sahibidir. Bu Hesapla Azure Stack Yönetici portalında oturum açabilir ve teklif ve plan oluşturabilir, kota ayarlamak ve Azure Stack'te diğer yönetim işlevleri gerçekleştirmek için kullanabilirsiniz.
2. **Fatura hesabı** (her ikisi de bağlı ve bağlantısı kesilmiş dağıtımları için gerekli). Bu Azure hesabı, Azure Stack tümleşik sistemi Azure ticaret arka uç arasında faturalama ilişki kurmak için kullanılır. Azure Stack ücretlerinin faturalandırılır hesabıdır. Bu hesap, öğeler Market ve diğer karma senaryolar teklifi için de kullanılır. 

### <a name="ad-fs-identity-store"></a>AD FS kimlik deposu
Hizmet Yöneticisi hesaplarınız için Kurumsal Active Directory gibi kendi kimlik deposu kullanmak istiyorsanız bu seçeneği belirleyin.  

## <a name="choose-a-billing-model"></a>Faturalama modelini seçin
Ya da tercih edebilirsiniz **-,-kullandıkça** veya **kapasite** faturalandırma modeli. ,-Kullandıkça Ödeme modeli dağıtımlarını bir bağlantı üzerinden Azure 30 günde bir en az bir kez rapor kullanım erişebilmelidir. Bu nedenle,-,-kullandıkça faturalandırma modeli yalnızca bağlı dağıtımları için kullanılabilir.  

### <a name="pay-as-you-use"></a>,-Kullandıkça
,-Kullandıkça faturalandırma modeli ile bir Azure aboneliğine kullanım ücretlendirilir. Yalnızca Azure Stack Hizmetleri kullandığınız zaman ücret ödersiniz. Bu, karar modeli ise, bir Azure aboneliği ve bu abonelikle ilişkili hesap kimliği gerekir (örneğin, serviceadmin@contoso.onmicrosoft.com). EA, CSP ve CSL abonelikleri desteklenir. Kullanım Raporlama sırasında yapılandırılır [Azure Stack kayıt](azure-stack-registration.md).

> [!NOTE]
> Çoğu durumda, Kurumsal müşterilerin EA aboneliklerini kullanır ve hizmet sağlayıcıları CSP veya CSL abonelik kullanır.

Bir CSP aboneliği kullanmak için kullanacaksanız, doğru yaklaşım tam CSP senaryoya bağlıdır kullanmak için hangi CSP aboneliği tanımlamak için aşağıdaki tabloyu gözden geçirin:

|Senaryo|Etki alanı ve abonelik seçenekleri|
|-----|-----|
|Siz bir **doğrudan bir CSP iş ortağı** veya **dolaylı CSP sağlayıcısı**, ve Azure Stack'te çalışır.|CSL (ortak hizmet katmanı) aboneliği kullanın.<br>     or<br>İş ortağı Merkezi'nde açıklayıcı bir ad ile Azure AD kiracısı oluşturun. Örneğin &lt;kuruluşunuz > CSPAdmin kendisiyle ilişkili bir Azure CSP aboneliği ile.|
|Siz bir **dolaylı CSP satıcısı**, ve Azure Stack'te çalışır.|Dolaylı CSP iş ortağı Merkezi'ni kullanarak kendisiyle ilişkili bir Azure CSP aboneliği ile kuruluşunuz için Azure AD kiracısı oluşturmak için sağlayıcınıza başvurun.|

### <a name="capacity-based-billing"></a>Kapasite tabanlı faturalandırma
Kapasite faturalandırma modeli kullanmaya karar verirseniz, bir Azure Stack kapasite planlama sisteminizin kapasiteye bağlı SKU satın almanız gerekir. Doğru miktarı satın almak için Azure Stack fiziksel çekirdek sayısı bilmeniz gerekir. 

Kapasite faturalandırma gerektiren bir Kurumsal Anlaşma (EA) kaydı için Azure aboneliği. Kayıt bir Azure aboneliği gerektirir Market'te öğeleri kullanılabilirliğinin dökümünü ayarlar nedenidir. Abonelik, Azure Stack kullanım için kullanılmaz.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz. [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemler, Azure Stack için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi teknik incelemesine bakın: [Azure Stack: Bir Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Microsoft Azure Stack paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [.pdf indirme](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri Merkezi ağ tümleştirmesi](azure-stack-network.md)