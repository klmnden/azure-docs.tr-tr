---
title: Azure Active Directory B2B kullanıcıları - bulut yerel iş ortağı hesapları eşitleme | Microsoft Docs
description: Yerel olarak yönetilen dış iş ortakları, hem yerel erişim ve Azure AD B2B işbirliği ile aynı kimlik bilgilerini kullanarak bulut kaynaklarını sağlar.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 04/24/2018
ms.author: mimart
author: msmimart
manager: daveba
ms.reviewer: sasubram
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 66c5ec6a41b630ee20139575080d8874d819bb59
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60355242"
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliğini kullanarak bulut kaynaklarına erişime yerel olarak yönetilen bir iş ortağı hesapları

Azure Active Directory önce (Azure AD), kuruluşların şirket içi kimlik sistemlerinin ile kendi şirket içi dizininde geleneksel olarak yönetilen bir iş ortağı hesapları vardır. Böyle bir kuruluşta, uygulamaları Azure AD'ye geçmeye başlattığınızda, iş ortaklarınızı ihtiyaç duyduğu kaynakları erişebildiğinden emin olun istersiniz. Şirket içi kaynakları olup olmadığını veya buluttaki önemli olmamalıdır. Ayrıca, iş ortağı kullanıcılarınızın aynı oturum açma kimlik bilgilerini hem şirket içi hem de Azure AD kaynaklarını kullanabilmek istiyorsunuz. 

Şirket içi dizininizdeki dış, iş ortakları için hesapları oluşturmak istiyorsanız (örneğin, bir hesap için bir dış kullanıcının "wmoran Gülşen Moran partners.contoso.com etki alanınızda adlı" oturum açma adı ile oluşturduğunuz), bu hesaplar için Şimdi Eşitle bulut. Özellikle, bulut iş ortağı hesapları olarak Azure AD B2B kullanıcıları eşitleme için Azure AD Connect kullanabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =). Bu, iş ortağı kullanıcılarınızın ihtiyaç duydukları daha fazla erişimi vermeden yerel hesapları, aynı kimlik bilgilerini kullanarak bulut kaynaklarını sağlar. 

## <a name="identify-unique-attributes-for-usertype"></a>UserType için benzersiz bir öznitelik tanımlayın

Temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce şirket içi Active Directory'den temel alınan UserType özniteliği türetmek nasıl karar vermeniz gerekir. Diğer bir deyişle, şirket içi ortamınızda hangi parametrelerin, dış ortak çalışanlarla benzersiz misiniz? Bu dış Ortak Çalışanlar kendi kuruluş üyelerinden ayırt eden bir parametre belirler.

Bunun için iki ortak yaklaşım vardır:

- Kaynak özniteliği olarak kullanmak için kullanılmayan şirket içi Active Directory öznitelik (örneğin, extensionAttribute1) belirleyin. 
- Alternatif olarak, temel alınan UserType özniteliği için değer, diğer özelliklerden türetilir. Örneğin, kendi şirket içi Active Directory UserPrincipalName özniteliğinin etki alanı ile bitiyorsa, konuk olarak tüm kullanıcılar eşitlemek istediğiniz  *\@partners.contoso.com*.
 
Ayrıntılı öznitelik gereksinimleri için bkz [UserType, eşitlemeyi etkinleştirme](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect eşitleme kullanıcılara bulut yapılandırma

Benzersiz öznitelik tanımladıktan sonra bu kullanıcılara bulut Azure AD B2B kullanıcıları olarak eşitleme için Azure AD Connect'i yapılandırabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =). Bir yetkilendirme açısından bakıldığında, bu kullanıcılar B2B kullanıcıları Azure AD B2B işbirliği davet işlemi oluşturulan döndürsün.

Uygulama yönergeleri için bkz. [UserType, eşitlemeyi etkinleştirme](../hybrid/how-to-connect-sync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Sonraki adımlar

- [Hibrit kuruluşlar için Azure Active Directory B2B işbirliği](hybrid-organizations.md)
- [GRANT B2B kullanıcıları Azure AD'de, şirket içi uygulamalarınıza erişim](hybrid-cloud-to-on-premises.md)
- Azure AD Connect genel bakış için bkz. [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md).

