---
title: Karma kuruluşlara - Azure Active Directory B2B işbirliği | Microsoft Docs
description: İş ortakları, hem şirket içi için erişim vermek ve Azure AD B2B işbirliği kaynaklarla bulut.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/26/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: 943ccadbc87cd8d2345078405e2a27930634668e
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
ms.locfileid: "33928105"
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Karma kuruluşlar için Azure Active Directory B2B işbirliği

Azure Active Directory (Azure AD) B2B işbirliği, Dış ortaklarınız uygulamaları ve kuruluşunuzdaki kaynaklara erişmesini sağlamak kolaylaştırır. Bu şirket içi ve bulut tabanlı kaynaklar sahip olduğu bir karma yapılandırmasında bile geçerlidir. Şu anda dış ortağı hesapları yerel olarak, şirket içi kimlik sistemi yönetiyorsanız veya Azure AD B2B kullanıcılar olarak bulutta olan dış hesapların yönetiyorsanız, önemli değildir. Artık bu kullanıcılar, her iki ortamlar için aynı oturum açma kimlik bilgilerini kullanarak her iki konumda kaynaklarına erişimi verebilirsiniz.

## <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-apps"></a>Şirket içi uygulamalarınızı Azure AD erişim ver B2B kullanıcılar

Kuruluşunuz Azure AD iş ortağı kuruluşlardan konuk kullanıcılara davet etmek için Azure AD B2B işbirliği özelliklerinden kullanıyorsa, artık bu B2B kullanıcılara şirket içi uygulamalar için erişim sağlayabilir.

SAML tabanlı kimlik doğrulaması kullanan uygulamalar için bu uygulamaları Azure AD uygulama proxy'si için kimlik doğrulaması kullanarak B2B kullanıcılara Azure portalı üzerinden sunabilirsiniz.

Kerberos Kısıtlı temsilci (KCD) ile tümleşik Windows kimlik doğrulaması (IWA) kullanan uygulamalar için ayrıca Azure AD Proxy kimlik doğrulaması için kullanın. Ancak, çalışmak yetkilendirme için şirket içi Windows Server Active Directory içinde bir kullanıcı nesnesi için gereklidir. B2B Konuk kullanıcılar temsil eden yerel kullanıcı nesnelerini oluşturmak için kullanabileceğiniz iki yöntem vardır.

- Microsoft Identity Manager (MIM) 2016 SP1 ve MIM yönetim Aracısı Microsoft Graph için kullanabilirsiniz.
- Bir PowerShell betiğini kullanabilirsiniz. (Bu çözüm MIM gerektirmez.)

Bu çözümlerin nasıl hakkında daha fazla ayrıntı için bkz: [Grant B2B kullanıcılar Azure AD'de erişim, şirket içi uygulamalarınızı](active-directory-b2b-hybrid-cloud-to-on-premises.md).

## <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources"></a>Yerel olarak yönetilen ortağı hesapları bulut kaynaklarına erişim

Azure AD önce şirket içi kimlik sistemlerinin kuruluşlarla geleneksel olarak şirket içi directory'lerinde ortağı hesapları yüklediniz. Bu tür bir kuruluşun değilseniz, ortaklarınız, uygulamalara ve diğer kaynaklarına buluta taşımak gibi erişiminiz devam ettiğinden emin olmak istersiniz. İdeal olarak, Bulut ve şirket içi kaynaklara erişmek için aynı kimlik bilgileri kümesini kullanmak için bu kullanıcılara istiyor. 

Biz şimdi burada hesapları yalnızca davranır bulut için bu yerel hesaplar "konuk kullanıcılar" olarak eşitlemek için Azure AD Connect burada kullanabileceğiniz teklif yöntemleri Azure AD B2B kullanıcılar ister.

Şirket verilerinizi korumaya yardımcı olmak için yalnızca doğru kaynaklara erişimi denetlemek ve bu konuk kullanıcıların farklı çalışanlarınızın kabul yetkilendirme ilkelerini yapılandırın.

Uygulama ayrıntıları için bkz: [Grant ortağı yerel olarak yönetilen hesapları Azure AD B2B işbirliği kullanarak bulut kaynaklarına erişim](active-directory-b2b-hybrid-on-premises-to-cloud.md).
 
## <a name="next-steps"></a>Sonraki adımlar

- [Şirket içi uygulamalarınıza Azure AD erişim ver B2B kullanıcılar](active-directory-b2b-hybrid-cloud-to-on-premises.md)
- [Azure AD B2B işbirliği kullanarak bulut kaynaklarına iş ortağı yerel olarak yönetilen hesapları erişim izni ver](active-directory-b2b-hybrid-on-premises-to-cloud.md)


