---
title: Yerel olarak yönetilen ortağı hesapları erişim Azure AD B2B kullanıcılar bulut kaynaklarına | Microsoft Docs
description: Yerel olarak yönetilen dış iş ortakları ve Azure AD B2B işbirliği ile aynı kimlik bilgilerini kullanarak bulut kaynaklarına hem yerel erişim verin.
services: active-directory
ms.service: active-directory
ms.component: B2B
ms.topic: article
ms.date: 04/24/2018
ms.author: twooley
author: twooley
manager: mtillman
ms.reviewer: sasubram
ms.openlocfilehash: dc4c8b3f5cb20eaf76fc0ee47d195aca26d06f90
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/10/2018
---
# <a name="grant-locally-managed-partner-accounts-access-to-cloud-resources-using-azure-ad-b2b-collaboration"></a>Azure AD B2B işbirliği kullanarak bulut kaynaklarına iş ortağı yerel olarak yönetilen hesapları erişim izni ver

Azure Active Directory (Azure AD), kuruluşların şirket içi kimlik sistemlerinin ile kendi şirket içi dizin geleneksel yönetilen ortağı hesapları vardır. Bu tür bir kuruluşta, Azure AD ile uygulamaları taşımak başlattığınızda ortaklarınızdan kaynaklara erişim sağlayabildiğinizden emin olun istiyor. Şirket içi kaynakları olup olmadığına veya bulutta önemli döndürmemelidir. Ayrıca, iş ortağı kullanıcılarınızın aynı oturum açma kimlik bilgilerini hem şirket içi hem de Azure AD kaynaklarını kullanabilmek istiyorsunuz. 

Şirket içi dizininizdeki dış, iş ortakları için hesapları oluşturursanız (örneğin, bir hesap oturum açma adıyla bir dış kullanıcı "wmoran partners.contoso.com etki alanınızdaki Gülşen Moran adlı" oluşturduğunuz), bu hesaplar için Şimdi Eşitle bulut. Özellikle, bulut iş ortağı hesapları Azure AD B2B kullanıcılar olarak eşitleme için Azure AD Connect kullanabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =). Bu gereksinim duydukları daha fazla erişimi vermeden yerel hesaplarını aynı kimlik bilgilerini kullanarak bulut kaynaklarına erişmek iş ortağı kullanıcılarınızın sağlar. 

## <a name="identify-unique-attributes-for-usertype"></a>Benzersiz öznitelikleri için UserType tanımlayın

Temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce şirket içi Active Directory'den UserType özniteliği çıkarmaya nasıl karar vermeniz gerekir. Diğer bir deyişle, şirket içi ortamınızda hangi parametreler, dış ortak çalışanlarla benzersiz misiniz? Bu dış ortak veya kendi kuruluşunuzdaki üyelerinden ayıran bir parametre belirler.

Bu iki genel yaklaşım vardır:

- Kaynak özniteliği olarak kullanmak için kullanılmayan şirket içi Active Directory öznitelik (örneğin, extensionAttribute1) belirleyin. 
- Alternatif olarak, temel alınan UserType özniteliği değeri diğer özelliklerinden türetilir. Örneğin, kendi şirket içi Active Directory UserPrincipalName özniteliğinin etki alanı ile bitiyorsa tüm kullanıcılar konuk olarak eşitlemek için istediğiniz *@partners.contoso.com*.
 
Ayrıntılı öznitelik gereksinimleri için bkz: [UserType eşitlemeyi etkinleştirme](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect eşitleme kullanıcıları buluta için yapılandırma

Benzersiz özniteliği tanımladıktan sonra bu kullanıcılara bulut Azure AD B2B kullanıcılar olarak eşitlemek için Azure AD Connect yapılandırabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =). Bir yetkilendirme açısından bakıldığında, bu kullanıcılar Azure AD B2B işbirliği davet işlemi boyunca oluşturulan B2B kullanıcılardan ayırt edilemez.

Uygulama yönergeleri için bkz: [UserType eşitlemeyi etkinleştirme](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Sonraki adımlar

- [Karma kuruluşlar için Azure Active Directory B2B işbirliği](active-directory-b2b-hybrid-organizations.md)
- [Şirket içi uygulamalarınıza Azure AD erişim ver B2B kullanıcılar](active-directory-b2b-hybrid-cloud-to-on-premises.md)
- Azure AD Connect genel bakış için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](connect/active-directory-aadconnect.md).

