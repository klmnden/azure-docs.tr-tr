---
title: "Karma kuruluşlar için Azure Active Directory B2B işbirliği | Microsoft Docs"
description: "İş ortakları ve Azure AD B2B işbirliği ile bulut kaynaklarına hem yerel erişim verin"
services: active-directory
documentationcenter: 
author: twooley
manager: mtillman
editor: 
tags: 
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.date: 12/15/2017
ms.author: twooley
ms.reviewer: sasubram
ms.openlocfilehash: 2e690eeea6a9f7e1cc10830a913774daa3c66689
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2017
---
# <a name="azure-active-directory-b2b-collaboration-for-hybrid-organizations"></a>Karma kuruluşlar için Azure Active Directory B2B işbirliği

Bir karma hem olduğu kuruluş, şirket içi ve kaynakları, hem yerel dış iş ortakları erişim verin ve bulut kaynaklarına isteyebilirsiniz bulut. Yerel kaynaklara erişim için iş ortağı hesapları yerel olarak şirket içi Active Directory ortamınızı yönetebilirsiniz. İş ortakları, kuruluşunuzun kaynaklarına erişmek için kendi iş ortağı hesabı kimlik bilgilerinizle oturum. İş ortakları bu kimlik bilgilerini kullanarak bulut kaynaklarına erişmesini sağlamak için artık Azure Active Directory (Azure AD) Bağlan bulut iş ortağı hesapları Azure AD B2B kullanıcılar olarak eşitlemek için kullanabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =).

## <a name="identify-unique-attributes-for-usertype"></a>Benzersiz öznitelikleri için UserType tanımlayın

Temel alınan UserType özniteliği eşitlenmesi etkinleştirmeden önce şirket içi Active Directory'den UserType özniteliği çıkarmaya nasıl karar vermeniz gerekir. Diğer bir deyişle, şirket içi ortamınızda hangi parametreler, dış ortak çalışanlarla benzersiz misiniz? Bu dış ortak veya kendi kuruluşunuzdaki üyelerinden ayıran bir parametre belirler.

Bu iki genel yaklaşım vardır:

- Kaynak özniteliği olarak kullanmak için kullanılmayan şirket içi Active Directory öznitelik (örneğin, extensionAttribute1) belirleyin. 
- Alternatif olarak, temel alınan UserType özniteliği değeri diğer özelliklerinden türetilir. Örneğin, kendi şirket içi Active Directory UserPrincipalName özniteliğinin etki alanı ile bitiyorsa tüm kullanıcılar konuk olarak eşitlemek için istediğiniz  *@partners.fabrikam123.org* .
 
Ayrıntılı öznitelik gereksinimleri için bkz: [UserType eşitlemeyi etkinleştirme](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype). 

## <a name="configure-azure-ad-connect-to-sync-users-to-the-cloud"></a>Azure AD Connect eşitleme kullanıcıları buluta için yapılandırma

Benzersiz özniteliği tanımladıktan sonra bu kullanıcılara bulut Azure AD B2B kullanıcılar olarak eşitlemek için Azure AD Connect yapılandırabilirsiniz (diğer bir deyişle, UserType kullanıcılarla konuk =). Bir yetkilendirme açısından bakıldığında, bu kullanıcılar Azure AD B2B işbirliği davet işlemi boyunca oluşturulan B2B kullanıcılardan ayırt edilemez.

Uygulama yönergeleri için bkz: [UserType eşitlemeyi etkinleştirme](connect/active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-usertype).

## <a name="next-steps"></a>Sonraki adımlar

- Azure AD B2B işbirliği genel bakış için bkz: [Azure AD B2B işbirliği nedir?](active-directory-b2b-what-is-azure-ad-b2b.md)
- Azure AD Connect genel bakış için bkz: [şirket içi dizinlerinizi Azure Active Directory ile tümleştirme](connect/active-directory-aadconnect.md).

