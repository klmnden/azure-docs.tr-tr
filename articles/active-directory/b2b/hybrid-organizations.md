---
title: Hibrit kuruluşlar - Azure Active Directory B2B işbirliği | Microsoft Docs
description: İş ortakları, hem şirket içi için erişim verin ve bulut kaynakları Azure AD B2B işbirliği ile.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: b30e9c14863f67d28203eac916606e8786101e3b
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65768277"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Hibrit kuruluşlar için Azure Active Directory B2B işbirliği

Azure Active Directory (Azure AD) B2B işbirliği, dış iş ortakları uygulama ve kuruluşunuzdaki kaynaklara erişmesini kolaylaştırır. Bu hem şirket içi hem de bulut tabanlı kaynaklara sahip olduğu bir karma yapılandırmasında bile geçerlidir. Şu anda dış iş ortağı hesapları yerel olarak şirket içi kimlik sisteminizde yönetiyorsanız veya Azure AD B2B kullanıcıları olarak bulutta olan dış hesapların yönetiyorsanız önemi yoktur. Artık bu kullanıcılar, her iki ortamları için aynı oturum açma kimlik bilgilerini kullanarak ya da konumda kaynaklarına erişimi verebilirsiniz.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>GRANT B2B kullanıcıları Azure AD'de şirket içi uygulamalarınıza erişim

Kuruluşunuz Azure ad iş ortağı kuruluşlardan Konuk kullanıcıları davet etmek için Azure AD B2B işbirliği özellikleri kullanıyorsa, artık şirket içi uygulamalara bu B2B kullanıcıları erişimi sağlayabilirsiniz.

SAML tabanlı kimlik doğrulaması kullanan uygulamalar için bu uygulamalar için kullanılabilir Azure portalı üzerinden kimlik doğrulaması için Azure AD uygulama proxy'si kullanarak B2B kullanıcıları hale getirebilirsiniz.

Kerberos Kısıtlı temsilci (KCD) ile tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için de Azure AD Proxy kimlik doğrulaması için kullanabilirsiniz. Ancak, çalışması yetkilendirme için şirket içi Windows Server Active Directory'de bir kullanıcı nesnesi gereklidir. B2B Konuk kullanıcılarınızın temsil eden yerel kullanıcı nesneleri oluşturmak için kullanabileceğiniz iki yöntem vardır.

- Microsoft Identity Manager (MIM) 2016 SP1 ve MIM yönetim aracısı için Microsoft Graph kullanabilirsiniz.
- Bir PowerShell betiğini kullanabilirsiniz. (Bu çözüm, MIM gerektirmez.)

Bu çözümleri uygulama hakkında daha fazla ayrıntı için bkz. [verme B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Yerel olarak yönetilen bir iş ortağı hesapları bulut kaynaklarına erişim

Azure AD önce şirket içi kimlik sistemlerinin bir kuruluşlarıyla geleneksel şirket içi directory'lerinde iş ortağı hesapları yüklediniz. Siz bu tür bir kuruluş gibiyseniz, iş ortaklarınızı uygulamalarınızı ve diğer kaynakları buluta taşıdıkça erişiminiz devam ettiğinden emin olmanız gerekir. İdeal olarak, bu kullanıcılar, hem buluttaki hem de şirket içi kaynaklara erişmek için aynı kimlik bilgileri kümesi kullanmak istersiniz. 

Şimdi, burada hesapları yalnızca davranır "konuk kullanıcılar," buluta bu yerel hesaplar eşitlemek için Azure AD Connect kullanabileceğiniz teklif yöntemleri Azure AD B2B kullanıcıları istiyoruz.

Şirket verilerinizi korumaya yardımcı olmak için yalnızca doğru kaynaklara erişimi denetlemek ve bu konuk kullanıcılara çalışanlarınızın öğesinden farklı davranma yetkilendirme ilkelerini yapılandırın.

Uygulama ayrıntıları için bkz. [verme yerel olarak yönetilen bir iş ortağı hesapları Azure AD B2B işbirliğini kullanarak bulut kaynaklarına erişim](hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Sonraki adımlar

- [GRANT B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](hybrid-cloud-to-on-premises.md)
- [Azure AD B2B işbirliğini kullanarak bulut kaynaklarına erişime yerel olarak yönetilen bir iş ortağı hesapları](hybrid-on-premises-to-cloud.md)


