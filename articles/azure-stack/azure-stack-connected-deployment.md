---
title: "Azure bağlı dağıtım kararları Azure yığınının tümleşik sistemleri | Microsoft Docs"
description: "Dağıtım Planlama Azure yığın Azure bağlı çok düğümlü dağıtımlar için kararları belirleyin."
services: azure-stack
documentationcenter: 
author: jeffgilb
manager: femila
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2018
ms.author: jeffgilb
ms.reviewer: wfayed
ms.openlocfilehash: def9d5381144026b5ad0e8a076edd3c0692a08f4
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2018
---
# <a name="azure-connected-deployment-planning-decisions-for-azure-stack-integrated-systems"></a>Planlama kararları Azure yığınının azure bağlı dağıtım sistemleri tümleşik
Karar verdim sonra [nasıl karma bulut ortamınıza Azure yığın tümleştirecek](azure-stack-connection-models.md), ardından Azure yığın dağıtım kararlarınızı sonlandır.

Azure'a bağlı dağıtma Azure yığın kimliği deponuz için Azure Active Directory (Azure AD) veya Active Directory Federasyon Hizmetleri (AD FS) olabilir anlamına gelir. Fatura ya da modelden de seçebilirsiniz: ödeme olarak-size-kullanım ve kapasite tabanlı. Bağlı bir dağıtım varsayılan seçeneği çünkü müşterilerin hem Azure hem de Azure yığın ile ilgili özellikle karma bulut senaryosu için en çok değer Azure yığın dışında get olanak tanır. 

## <a name="choose-an-identity-store"></a>Bir kimlik deposu seçin
Bağlı bir dağıtım ile Azure AD veya kimlik deposu için AD FS arasından seçim yapabilirsiniz. Bağlantısı kesilmiş bir dağıtım internet bağlantısı olmayan, yalnızca AD FS kullanabilirsiniz.

Kimlik deposu seçiminizi şifrelemeyle Kiracı sanal makineleri (VM'ler) sahiptir. Kiracı VM'ler istedikleri Bağlan bunların nasıl yapılandırılır bağlı olarak hangi kimlik deposu seçebilir: Azure AD, Windows Server Active Directory etki alanına katılmış, çalışma grubu, vs. Azure yığın kimlik sağlayıcısı kararını ilgisi yoktur. 

Iaas Kiracı VM'ler Azure yığın üstünde dağıtmak ve kurumsal Active Directory etki alanına katılmak ve buradan hesapları kullanmak istiyorsanız, örneğin, hala bunu yapabilirsiniz. Bu hesaplar için burada seçtiğiniz Azure AD kimlik deposu kullanmak için gerekli değildir.

### <a name="azure-ad-identity-store"></a>Azure AD kimlik deposu
Kullandığınızda, Azure AD kimlik deposu iki Azure AD hesapları gerektirir: bir genel yönetici hesabı ve bir faturalama hesabı. Bu hesapları aynı hesapları veya farklı hesapları olabilir. Aynı kullanıcı hesabı kullanarak daha basit ve sınırlı sayıda Azure hesabı varsa yararlı olabilir ancak iş gereksinimlerinizi iki hesaplarını kullanarak önerebilir:

1. **Genel yönetici hesabı** (yalnızca bağlı dağıtımları için gerekli). Bu uygulamalar ve hizmet asıl adı Azure yığın altyapı hizmetleri için Azure Active Directory'de oluşturmak için kullanılan Azure bir hesaptır. Bu hesap altında Azure yığın sisteminizi dağıtılacak dizinine directory yönetici izinleri olmalıdır. "Bulut operatörü" olacağı için Azure AD genel yönetici Kiracı ve kullanılır: 
    - Sağlama ve temsilci uygulamaları ve Azure Active Directory grafik API'si ile etkileşim kurmak için gerekli tüm Azure yığın hizmetler için hizmet asıl adı. 
    - Hizmet yöneticisi hesabı olarak. (Daha sonra değiştirebilirsiniz) varsayılan sağlayıcı aboneliği sahibidir. Bu hesap ile Azure yığın Yönetim Portalı uygulamasına oturum açabilir ve sunar ve planları oluşturun, kotaları ayarlayabilir ve Azure yığındaki diğer yönetim işlevleri gerçekleştirmek için kullanabilirsiniz.
2. **Faturalama hesabı** (her ikisi de bağlı ve bağlantısı kesilmiş dağıtımları için gerekli). Bu Azure hesap Azure tümleşik yığını sisteminizi ve Azure ticaret arka uç arasında fatura ilişkisi oluşturmak için kullanılır. Azure yığın ücretlerinin Fatura edilecek hesap budur. Bu hesap, Market dağıtım ve diğer karma senaryolar için kullanılır. 

### <a name="ad-fs-identity-store"></a>AD FS kimlik deposu
Şirket, Active Directory gibi kendi kimlik deposu hizmet yönetici hesapları için kullanmak istiyorsanız bu seçeneği belirleyin.  

## <a name="choose-a-billing-model"></a>Faturalama modelini seçin
Ya da seçebilirsiniz **ödeme olarak,-kullanımlı** veya **kapasite** faturalama modeli. Ödeme olarak,-kullanımlı faturalama modeli dağıtımları için bir bağlantı üzerinden Azure 30 günde en az bir kez rapor kullanımına sahip olması gerekir. Bu nedenle, ödeme olarak,-kullanımlı fatura modelini yalnızca bağlı dağıtımlar için kullanılabilir.  

### <a name="pay-as-you-use"></a>Ödeme olarak-size-kullanımı
Ödeme olarak,-kullanımlı fatura modeliyle bir Azure aboneliğine kullanım ücretlendirilir. Yalnızca Azure yığın Hizmetleri kullandığınızda ücret ödersiniz. Bu, karar modeli ise, bir Azure aboneliği ve bu abonelikle ilişkili hesap kimliği gerekir (örneğin, serviceadmin@contoso.onmicrosoft.com). EA, CSP ve CSL abonelikleri desteklenir. Kullanım raporlaması sırasında yapılandırılır [Azure yığın kayıt](azure-stack-registration.md).

> [!NOTE]
> Çoğu durumda, Kurumsal müşterilerin EA abonelikleri kullanır ve hizmet sağlayıcıları CSP veya CSL abonelik kullanır.

Bir CSP abonelik kullanacaksanız, doğru yaklaşım tam CSP senaryoya bağlıdır olarak kullanmak için hangi CSP aboneliği tanımlamak için aşağıdaki tabloyu gözden geçirin:

|Senaryo|Etki alanı ve abonelik seçenekleri|
|-----|-----|
|Siz bir **doğrudan CSP iş ortağı** veya bir **dolaylı CSP sağlayıcı**, ve Azure yığın çalışır|CSL (ortak hizmet katmanı) aboneliği kullanın.<br>     or<br>İş ortağı Merkezi'nde açıklayıcı bir ad ile Azure AD kiracısı oluşturun. Örneğin &lt;kuruluşunuz > CSPAdmin bir Azure CSP aboneliğiyle ilişkili.|
|Siz bir **dolaylı CSP satıcı**, ve Azure yığın çalışır|Bir Azure CSP aboneliğiyle ilişkili iş ortağı Merkezi'ni kullanarak, kuruluşunuz için Azure AD kiracısı oluşturmak için dolaylı, CSP sağlayıcınıza başvurun.|

### <a name="capacity-based-billing"></a>Kapasite tabanlı faturalama
Kapasite faturalama modeli kullanmaya karar verirseniz, bir Azure yığın kapasite planlama sisteminizi kapasitesini temel SKU satın almanız gerekir. Doğru miktarı satın almak için Azure yığınında fiziksel çekirdek sayısı bilmeniz gerekir. 

Kapasite faturalama gerektiren bir Kurumsal Anlaşma (Kurumsal Sözleşme) kaydı için Azure aboneliği. Kayıt bir Azure aboneliği gerektirir dağıtım ayarlar nedenidir. Abonelik Azure yığın kullanımı için kullanılmaz.

## <a name="learn-more"></a>Daha fazla bilgi edinin
- Kullanım örnekleri, satın alma, iş ortakları ve OEM donanım satıcıları hakkında daha fazla bilgi için bkz: [Azure yığın](https://azure.microsoft.com/overview/azure-stack/) ürün sayfası.
- Tümleşik sistemleri Azure yığını için yol haritası ve coğrafi kullanılabilirlik hakkında bilgi için teknik incelemesine bakın: [Azure yığın: Azure uzantısı](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
- Microsoft Azure yığın paketleme ve fiyatlandırma hakkında daha fazla bilgi edinmek için [.pdf karşıdan](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf). 

## <a name="next-steps"></a>Sonraki adımlar
[Veri Merkezi ağ tümleştirme](azure-stack-network.md)